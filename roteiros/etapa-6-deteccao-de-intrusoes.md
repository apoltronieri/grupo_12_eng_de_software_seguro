# Etapa 6 — Detecção de Intrusões

## 1. Conceito de detecção de intrusões

Detecção de intrusões é o processo de identificar, em tempo real ou de forma retrospectiva, eventos que indicam tentativas não autorizadas de acesso, exploração de vulnerabilidades ou violações de políticas de segurança em um sistema. Ela complementa os controles preventivos ao reconhecer que nem toda tentativa de ataque será bloqueada antes de se manifestar — e que a capacidade de detectar e responder rapidamente reduz o impacto de eventuais falhas.

Um sistema de detecção de intrusões (IDS) analisa fontes de dados — como logs de aplicação, eventos de rede e métricas de sistema — e aplica regras ou modelos estatísticos para identificar padrões anômalos ou conhecidamente maliciosos. Quando um padrão é encontrado, um alerta é gerado para que a equipe de segurança possa investigar e responder.

### 1.1 Diferença entre prevenção e detecção

| Dimensão | Prevenção | Detecção |
|---|---|---|
| Objetivo | Impedir que o evento ocorra antes de causar dano. | Identificar que o evento ocorreu ou está em andamento. |
| Momento de ação | Antes ou durante a tentativa de ataque. | Durante ou após a tentativa de ataque. |
| Exemplos de mecanismo | Verificação de papel no backend (`403 Forbidden`), bloqueio de campos via DTO, autenticação multifator. | Alertas sobre múltiplos `403 Forbidden` consecutivos, correlação de logs de auditoria, análise de anomalias. |
| Resultado de falha | Ataque é executado porque o controle não funcionou. | Ataque não é detectado, dificultando a resposta e a contenção. |
| Complementaridade | A prevenção reduz a superfície de ataque. | A detecção reduz o tempo até a contenção quando a prevenção falha ou é contornada. |

Os dois mecanismos são complementares. Depender apenas de prevenção pressupõe que todos os controles são perfeitos e que nenhuma vulnerabilidade desconhecida será explorada. A detecção cobre o espaço onde a prevenção não chega, possibilitando resposta e aprendizado contínuo.

## 2. Eventos de segurança a serem registrados

Os eventos abaixo deverão ser registrados nos logs de auditoria e de aplicação para apoiar a detecção, a resposta a incidentes e a análise forense.

| ID | Evento | Fonte de dados | Informações mínimas a registrar |
|---|---|---|---|
| `EV-01` | Autenticação bem-sucedida | Log de aplicação (autenticação) | Identificador do usuário, perfil, endereço IP, *timestamp*. |
| `EV-02` | Falha de autenticação | Log de aplicação (autenticação) | Endereço IP, credencial tentada (sem senha), motivo da falha, *timestamp*. |
| `EV-03` | Acesso negado por autorização insuficiente (`403 Forbidden`) | Log de aplicação (autorização) | Identificador do usuário, perfil autenticado, rota tentada, método HTTP, endereço IP, *timestamp*. |
| `EV-04` | Tentativa de acesso a rota administrativa por perfil não autorizado | Log de aplicação (autorização) | Identificador do usuário, perfil, rota tentada, endereço IP, *timestamp*. |
| `EV-05` | Payload contendo campo `perfil` ou `role` recebido pela API | Log de aplicação (validação de entrada) | Identificador do usuário (se autenticado), campos inesperados detectados, rota, *timestamp*. |
| `EV-06` | Operação administrativa executada com sucesso | Log de auditoria | Identificador do administrador, ação executada, recurso afetado, *timestamp*. |
| `EV-07` | Alteração de papel ou nível de acesso de um usuário | Log de auditoria | Identificador do executor, identificador do usuário afetado, papel anterior, papel novo, *timestamp*. |
| `EV-08` | Token JWT com papel diferente do perfil registrado no banco | Log de aplicação (validação de token) | Identificador do usuário, papel no token, papel no banco, endereço IP, *timestamp*. |

## 3. Regra de detecção relacionada a R06 e CA06

### RD06 — Tentativas repetidas de acesso não autorizado a rotas restritas

#### Identificação da regra

| Campo | Definição |
|---|---|
| ID da regra | `RD06` |
| Risco observado | `R06 — Escalada indevida de privilégios` |
| Caso de abuso relacionado | `CA06 — Escalada indevida de privilégios` |
| Ameaça de origem | `T06 — Elevation of Privilege` |
| Objetivo | Detectar quando um usuário autenticado tenta acessar repetidamente rotas administrativas para as quais não possui autorização, indicando uma exploração ativa ou reconhecimento da API. |

#### Fonte de dados e condição de alerta

| Campo | Definição |
|---|---|
| Fonte de dados | Logs de autorização da API — eventos do tipo `EV-03` e `EV-04`. |
| Condição de alerta | **5 ou mais ocorrências de `403 Forbidden`** para rotas administrativas originadas do **mesmo identificador de usuário ou endereço IP** em uma **janela de 5 minutos**. |
| Severidade | Alta |
| Justificativa da condição | Um usuário legítimo raramente acessa rotas restritas inadvertidamente. Múltiplas tentativas em curto intervalo sugerem exploração sistemática da API — mapeamento de endpoints ou tentativa de *Broken Function Level Authorization*. O limiar de 5 eventos em 5 minutos reduz falsos positivos de acesso acidental ao mesmo tempo em que responde rapidamente a varreduras automatizadas. |

#### Campos esperados no alerta

| Campo | Descrição |
|---|---|
| `usuario_id` | Identificador do usuário autenticado responsável pelas tentativas. |
| `perfil` | Papel registrado no token JWT apresentado. |
| `ip_origem` | Endereço IP de origem das requisições. |
| `rotas_tentadas` | Lista das rotas administrativas acessadas (ex.: `POST /profissionais`, `DELETE /usuarios/{id}`). |
| `contagem` | Número de eventos `403` detectados na janela. |
| `janela_inicio` | *Timestamp* do primeiro evento da série. |
| `janela_fim` | *Timestamp* do último evento da série. |

#### Regra complementar — detecção de Mass Assignment

| Campo | Definição |
|---|---|
| ID da regra | `RD06-B` |
| Fonte de dados | Logs de validação de entrada da API — eventos do tipo `EV-05`. |
| Condição de alerta | Qualquer ocorrência de recebimento de campo `perfil`, `role` ou `authorities` num payload de cadastro ou atualização de usuário por parte de um perfil não administrador. |
| Severidade | Média |
| Justificativa | A tentativa de injetar campos de elevação de privilégio é incomum em uso legítimo e indica tentativa deliberada de *Mass Assignment*. A regra não exige limiar por ser um evento de baixo volume e alta especificidade. |

## 4. Tratamento inicial esperado após geração de alerta

### Para `RD06` — Acesso repetido negado

1. **Notificação imediata:** O alerta deverá ser enviado ao canal de segurança da equipe (ex.: e-mail, Slack, sistema de tickets) com os campos listados acima.
2. **Bloqueio temporário automático (opcional):** A plataforma poderá bloquear temporariamente o usuário ou o IP de origem por 15 minutos enquanto a análise ocorre, impedindo progressão do reconhecimento.
3. **Análise do identificador:** A equipe de segurança deverá verificar o histórico da conta, checando se houve autenticação legítima recente, padrão de uso normal e se outras contas do mesmo IP também dispararam alertas.
4. **Classificação:** O analista deverá classificar o evento como falso positivo (ex.: bug no frontend), uso indevido não malicioso ou tentativa de exploração ativa.
5. **Escalada:** Caso a tentativa seja classificada como exploração ativa, a equipe de segurança deverá iniciar o procedimento de resposta a incidentes de `R06`, notificando o responsável pelo produto.

### Para `RD06-B` — Injeção de campo de privilégio

1. **Notificação imediata:** O alerta deverá ser enviado ao canal de segurança com o identificador do usuário, o campo detectado e o contexto da requisição.
2. **Auditoria da conta:** Verificar se o papel do usuário no banco foi alterado após o evento — mesmo que o DTO tenha ignorado o campo, a auditoria garante que nenhuma falha de mapeamento persistiu o valor indevido.
3. **Revisão de código:** Identificar se o campo foi mapeado por algum DTO acessível e corrigir imediatamente caso sim.
4. **Registro do evento:** Manter o log como evidência para análise forense e eventual auditoria regulatória.
