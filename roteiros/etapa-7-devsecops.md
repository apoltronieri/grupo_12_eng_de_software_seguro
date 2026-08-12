# Etapa 7 — DevSecOps: Monitoramento e Resposta no Pipeline

## 1. Monitoramento de segurança no pipeline

### 1.1 Contexto

No modelo DevSecOps, a segurança deixa de ser uma fase separada ao final do desenvolvimento e passa a ser integrada em cada etapa do pipeline de entrega de software. Para o risco `R06 — Escalada indevida de privilégios`, isso significa que os controles de autorização não apenas existem em produção, mas são verificados automaticamente a cada alteração de código entregue.

### 1.2 Atividades de monitoramento no pipeline

| Etapa do pipeline | Atividade de segurança | Relação com R06 | Evidência produzida |
|---|---|---|---|
| **Commit / Pull Request** | Análise estática de código (SAST) para identificar rotas sem anotação de autorização, DTOs com campos de perfil expostos e *hardcoded credentials*. | Detecta, antes do merge, código que expõe rotas sem `@PreAuthorize` ou `hasRole()`, ou DTOs que mapeiam o campo `perfil`. | Relatório da ferramenta SAST (ex.: resultado do SonarQube ou semgrep) vinculado ao PR; a pipeline deve falhar se regras críticas forem violadas. |
| **Build** | Execução dos testes unitários de autorização (`TS06.1` a `TS06.3`) para verificar que perfis não autorizados recebem `403 Forbidden` nas rotas restritas e que *Mass Assignment* não altera o papel. | Valida que as regras de RBAC codificadas no backend funcionam corretamente antes de qualquer implantação. | Relatório de testes (ex.: JUnit/surefire) com cobertura dos testes de autorização; a pipeline deve falhar se qualquer teste de autorização não passar. |
| **Testes de integração** | Execução de testes de integração contra a API real em ambiente de homologação, verificando o comportamento fim a fim dos endpoints administrativos com tokens de diferentes perfis. | Garante que filtros, configurações do Spring Security e regras de negócio funcionam em conjunto, não apenas em isolamento. | Relatório de testes de integração com cenários de acesso negado e acesso autorizado; evidência de resposta `403` para perfis incorretos. |
| **Análise de dependências (SCA)** | Verificação de vulnerabilidades conhecidas em bibliotecas de terceiros (ex.: Spring Security, bibliotecas de JWT). | Bibliotecas com falhas conhecidas de bypass de autorização podem anular os controles implementados. | Relatório SCA (ex.: OWASP Dependency-Check ou Snyk) com lista de CVEs encontradas e status de remediação. |
| **Deploy / Pós-deploy** | Execução automatizada de testes de fumaça de segurança (*smoke tests*) contra o ambiente recém-implantado, verificando que as rotas administrativas estão inacessíveis a usuários comuns. | Confirma que a configuração de produção não diverge do que foi testado em homologação. | Log de execução dos *smoke tests* de segurança com resultado esperado `403` para cada rota administrativa testada. |
| **Produção contínua** | Monitoramento contínuo de logs de auditoria com as regras `RD06` e `RD06-B` definidas na etapa 6, incluindo alertas e dashboards de `403 Forbidden` por usuário e por rota. | Detecta tentativas de exploração em produção que não foram capturadas pelos controles preventivos. | Logs de auditoria com eventos `EV-03`, `EV-04` e `EV-05`; alertas disparados; dashboard com métricas de acesso negado. |

### 1.3 Testes de autorização definidos

Os testes abaixo deverão existir como testes automatizados e ser executados no pipeline a cada alteração.

| ID | Tipo | Entrada ou ação | Resultado seguro esperado |
|---|---|---|---|
| `TS06.1` | Controle de acesso por papel | Token JWT com papel `PACIENTE` envia `POST /profissionais` com payload de criação de profissional. | A API retorna `403 Forbidden`; nenhum profissional é criado; o evento é registrado como `EV-04`. |
| `TS06.2` | Controle de acesso por papel | Token JWT com papel `PROFISSIONAL` envia `DELETE /usuarios/{id}` para remover uma conta de paciente. | A API retorna `403 Forbidden`; a conta não é excluída; o evento é registrado como `EV-04`. |
| `TS06.3` | Prevenção de *Mass Assignment* | Token JWT com papel `PACIENTE` envia `POST /usuarios` com payload contendo `"perfil": "ADMIN"`. | A conta é criada com papel `PACIENTE`; o campo `perfil` do payload é ignorado; o evento é registrado como `EV-05`; nenhum papel elevado é persistido. |
| `TS06.4` | Acesso legítimo | Token JWT com papel `ADMIN` envia `POST /profissionais` com payload válido. | A API retorna `201 Created`; o profissional é criado; o evento é registrado como `EV-06`. |
| `TS06.5` | Acesso legítimo | Token JWT com papel `PACIENTE` envia `GET /agendamentos` (rota do próprio perfil). | A API retorna `200 OK` com os agendamentos do paciente autenticado; nenhum erro de autorização é gerado. |

## 2. Resposta a incidentes relacionados a R06 no pipeline

### 2.1 Gatilhos de resposta

| Gatilho | Origem | Ação imediata |
|---|---|---|
| Falha de teste de autorização na pipeline de build | Pipeline CI/CD | Bloquear o merge do PR; notificar o desenvolvedor responsável para análise e correção. |
| Alerta `RD06` disparado em produção | Monitoramento de logs | Notificar o canal de segurança; iniciar análise do usuário e IP envolvidos conforme procedimento da Etapa 6. |
| Alerta `RD06-B` disparado em produção | Monitoramento de logs | Auditar o banco para verificar se o papel foi alterado; acionar revisão emergencial de código se confirmada a persistência indevida. |
| Vulnerabilidade crítica em biblioteca de autorização detectada pelo SCA | Pipeline CI/CD | Bloquear o deploy; abrir tarefa de atualização urgente com SLA definido pela equipe de segurança. |

### 2.2 Papéis e responsabilidades

| Papel | Responsabilidade |
|---|---|
| **Equipe de desenvolvimento** | Implementar e manter os testes de autorização (`TS06.1` a `TS06.5`); corrigir falhas de SAST antes do merge; atualizar dependências vulneráveis sinalizadas pelo SCA. |
| **Equipe de infraestrutura/segurança** | Configurar e manter as ferramentas de SAST, SCA e monitoramento de logs; definir e revisar as regras `RD06` e `RD06-B`; gerenciar alertas e dashboards. |
| **Responsável pelo produto** | Aprovar o risco residual de `R06` após evidências dos controles; priorizar correções emergenciais quando alertas críticos forem disparados. |
| **Equipe de operação e monitoramento** | Monitorar os dashboards de `403 Forbidden` e responder aos alertas durante o horário de operação; escalar para a equipe de segurança quando necessário. |

### 2.3 Evidências esperadas ao fim do ciclo

Ao final de cada ciclo de entrega, as seguintes evidências deverão estar disponíveis e armazenadas de forma auditável:

| Evidência | Descrição | Produzida em |
|---|---|---|
| Relatório SAST | Resultado da análise estática com ausência de violações críticas de autorização. | Pipeline de PR/commit |
| Relatório de testes unitários e de integração | Cobertura e resultado dos testes `TS06.1` a `TS06.5` com todos passando. | Pipeline de build e integração |
| Relatório SCA | Lista de dependências verificadas sem CVEs críticas não remediadas. | Pipeline de build |
| Log de *smoke tests* pós-deploy | Confirmação de que rotas administrativas retornam `403` para perfis não autorizados no ambiente de produção. | Etapa de deploy |
| Logs de auditoria | Registros dos eventos `EV-03` a `EV-08` produzidos em produção durante o período. | Produção contínua |
| Histórico de alertas | Registro de alertas `RD06`/`RD06-B` disparados, classificações e ações tomadas. | Produção contínua |

## 3. Requisitos e decisões de arquitetura relacionados a R02

### 3.1 Posição no pipeline

A análise de requisitos e decisões de arquitetura deverá ocorrer depois da identificação e priorização dos riscos e antes da implementação. Toda alteração nos endpoints de remarcação ou cancelamento de agendamentos deverá manter a rastreabilidade entre `R02`, o controle `C02.2`, o requisito `RS02`, a prática segura `PS02` e os testes `TS02.1` e `TS02.2`.

| Momento | Atividade de segurança | Evidência produzida | Condição para continuar |
|---|---|---|---|
| **Requisitos e decisões de arquitetura** | Revisar mudanças que afetem as operações de agendamento em relação ao `RS02` e às decisões que o materializam: obter a identidade pelo contexto de autenticação, verificar no backend a autorização sobre o objeto e negar a operação antes de qualquer alteração persistente. Registrar no Pull Request quais endpoints e critérios de verificação são afetados. | Referência ao `RS02` e ao seu critério de verificação; vínculo com `R02` e `C02.2`; registro dos endpoints afetados e das decisões de autorização aplicáveis. | A alteração somente poderá avançar para a implementação quando o requisito, seu critério de verificação e o tratamento arquitetural da autorização estiverem documentados e coerentes entre si. |

### 3.2 Condição de bloqueio

**Gate `G-RS02` — Integridade de agendamentos:** o pipeline deverá aplicar dois pontos de verificação. Antes da implementação, deverá bloquear uma alteração que afete remarcação ou cancelamento quando não houver rastreabilidade com o `RS02`, critério de verificação ou tratamento arquitetural correspondente. Durante os testes automatizados, deverá bloquear a integração quando `TS02.1` ou `TS02.2`, executado para o endpoint afetado, apresentar resultado diferente do esperado. A continuidade dependerá da correção da documentação ou da implementação, conforme o ponto da falha, e de uma nova verificação bem-sucedida.

## 4. Planejamento e análise de ameaças relacionados a R01

### 4.1 Posição no pipeline

O planejamento e a análise de ameaças deverão ocorrer antes da implementação e ser revisados quando uma alteração afetar autenticação, emissão ou validação de JWT, armazenamento de tokens, sessões ou endpoints protegidos. A revisão deverá manter a rastreabilidade entre `T01 — Spoofing`, `CA01`, `R01 — Comprometimento de conta ou token`, o controle `C01.2`, o requisito `RS01` e a prática `PS01`.

| Momento | Atividade de segurança | Evidência produzida | Condição para continuar |
|---|---|---|---|
| **Planejamento e análise de ameaças** | Revisar se a mudança cria ou altera caminhos para roubo, adulteração ou aceitação indevida de JWT. Confirmar os ativos e componentes afetados, as condições do `CA01`, a probabilidade e o impacto do `R01` e a necessidade de atualizar o `RS01`, seus critérios de verificação e os testes `TS01.1`, `TS01.2` e `TS01.3`. Registrar na issue relacionada o resultado da revisão e os documentos afetados. | Registro de análise de ameaças na issue, contendo a decisão da revisão, a rastreabilidade `T01 → CA01 → R01 → C01.2 → RS01 → PS01`, os componentes afetados, a indicação de quais critérios e testes deverão ser mantidos ou atualizados e o link do commit correspondente. | A alteração somente poderá avançar para implementação quando a análise estiver registrada, a rastreabilidade estiver completa e eventuais mudanças no risco, requisito ou testes estiverem documentadas. |

### 4.2 Evidência da atividade

A evidência principal será o **registro de análise de ameaças na issue relacionada à atividade**. Esse registro deverá informar:

- qual mudança foi analisada;
- quais componentes de autenticação ou sessão foram afetados;
- se `T01`, `CA01` ou `R01` precisaram ser atualizados;
- se a avaliação de probabilidade, impacto ou risco residual mudou;
- quais critérios do `RS01` e testes de `PS01` se aplicam à mudança;
- quem realizou e quem revisou a análise;
- a decisão final: aprovada, aprovada com ajustes ou bloqueada.
- o link do commit que registra a alteração.

O vínculo entre a issue, o commit e os documentos versionados constituirá a evidência auditável de que o planejamento de segurança foi realizado e registrado no histórico do projeto.

### 4.3 Condição de bloqueio

**Gate `G-R01-AMEACAS` — Revisão de autenticação:** o pipeline deverá impedir o avanço para implementação quando uma alteração relacionada a autenticação ou sessão não apresentar o registro de análise de ameaças, não mantiver a rastreabilidade com `R01/RS01/PS01` ou identificar mudança relevante sem atualizar os documentos e testes correspondentes. A continuidade dependerá da correção da documentação e de uma nova revisão aprovada.
