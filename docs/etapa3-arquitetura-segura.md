# Etapa 3 — Projeto de uma Arquitetura Segura

## Decisões de arquitetura

| Decisão | Risco tratado | Justificativa | Componente afetado | Resultado esperado |
|---|---|---|---|---|
| Implementar *Rate Limiting* (Limitação de Taxa) por IP na borda da aplicação, alinhado à configuração de *Timeouts* estritos para consultas longas no banco de dados. | R05 — Indisponibilidade da API por exaustão de recursos | A ausência de restrições no tráfego de entrada permite que a API tente processar picos massivos de requisições anômalas ou maliciosas (como ataques DDoS ou varreduras automatizadas). Isso gera um gargalo rápido que esgota recursos vitais do sistema (CPU, memória RAM e, principalmente, o *pool* de conexões do banco de dados). Ao limitar a taxa de acesso, estipulamos um teto de processamento seguro que a infraestrutura suporta sem degradar. | API Gateway (Proxy Reverso) e Servidor de Aplicação | Rejeição automática de requisições excedentes (retornando imediatamente `429 Too Many Requests`) antes que elas impactem a aplicação. Isso preserva a estabilidade geral da plataforma, evita interrupções na comunicação com o banco de dados e mantém o serviço de agendamento 100% disponível e funcional para os usuários legítimos. |
## RS01 — Validação de tokens de acesso

### Origem e objetivo

| Campo | Definição |
|---|---|
| ID | `RS01` |
| Risco de origem | `R01 — Comprometimento de conta ou token` (`Crítico`, pontuação `12`) |
| Ameaça e caso de abuso relacionados | `T01 — Spoofing` e `CA01` |
| Controle relacionado | `C01.2 — Proteger a sessão e validar os tokens de acesso` |
| Objetivo | Impedir que tokens de acesso expirados, alterados ou assinados com chave ou algoritmo não autorizado sejam aceitos pela API. |

### Requisito de segurança

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| `RS01` | `R01` | Antes de autorizar qualquer requisição protegida, a API deverá validar a assinatura, o algoritmo permitido, o emissor (`iss`), o público destinatário (`aud`) e a expiração (`exp`) do JWT. Se qualquer validação falhar, a API deverá recusar a autenticação e não executar a operação solicitada. | Testes unitários e de integração deverão demonstrar que um token válido permite o acesso autorizado e que tokens expirados, alterados, com assinatura inválida, algoritmo não permitido, emissor incorreto ou público incorreto recebem `401 Unauthorized`. Em todos os cenários recusados, a operação protegida deverá permanecer sem execução. |

O requisito deverá ser aplicado a todos os endpoints protegidos da API, independentemente do perfil apresentado no token. A existência de informações de identidade ou perfil no payload não deverá compensar uma autenticação ausente ou inválida.

### Vulnerabilidade catalogada

| Risco e requisito | Vulnerabilidade ou categoria | Referência utilizada | Relação com o sistema |
|---|---|---|---|
| `R01` / `RS01` | `OWASP API2:2023 — Broken Authentication` | [OWASP API Security Top 10 — API2:2023](https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/) | A API utiliza tokens JWT para identificar pacientes, profissionais e administradores. Se não validar a autenticidade, a assinatura ou a expiração do token, poderá aceitar uma credencial inválida e associar a requisição à identidade representada no JWT. |

A categoria `API2:2023 — Broken Authentication` inclui APIs que não validam a autenticidade dos tokens, aceitam JWTs sem assinatura ou com assinatura fraca ou deixam de verificar sua expiração. Essas falhas correspondem diretamente ao `R01`, pois permitem que um atacante use um token inválido ou comprometido para assumir a conta de outra pessoa. O `RS01` reduz esse risco ao exigir a validação do token antes de qualquer operação protegida e ao definir testes que comprovem sua rejeição.

## RS02 — Autorização por objeto em alterações de agendamentos

### Origem e objetivo

| Campo | Definição |
|---|---|
| ID | `RS02` |
| Risco de origem | `R02 — Adulteração de dados de agendamentos` (`Crítico`, pontuação `12`) |
| Ameaça e caso de abuso relacionados | `T02 — Tampering` e `CA02 — Adulteração de dados de um agendamento` |
| Controle relacionado | `C02.2 — Verificar a relação entre o usuário autenticado e o agendamento` |
| Objetivo | Impedir que um usuário autenticado altere um agendamento sobre o qual não possui autorização, mesmo que manipule o identificador enviado à API. |

### Requisito de segurança

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| `RS02` | `R02` | Em toda alteração que receba um `agendamentoId`, a API deverá obter a identidade exclusivamente do contexto de autenticação e, antes de executar a operação, verificar se o usuário está autorizado a agir sobre o agendamento solicitado. Quando essa relação ou permissão não existir, a requisição deverá ser recusada sem modificar o registro. | Para cada endpoint de alteração listado abaixo, testes com dois pacientes e seus respectivos agendamentos deverão demonstrar que o titular consegue executar uma operação permitida sobre o próprio registro e que a tentativa de usar o identificador do agendamento do outro paciente retorna `403 Forbidden` e mantém o registro inalterado. Um identificador de paciente enviado pelo cliente não deverá substituir a identidade autenticada. |

O requisito deverá ser aplicado, no mínimo, às operações conceituais que recebem o identificador de um agendamento:

- `PATCH /agendamentos/{agendamentoId}/remarcacao`;
- `PATCH /agendamentos/{agendamentoId}/cancelamento`.

A autorização deverá considerar simultaneamente o usuário autenticado, o agendamento solicitado e a ação pretendida. Conhecer ou adivinhar um identificador não concede permissão sobre o objeto.

Este requisito trata da autorização horizontal sobre cada agendamento. As permissões gerais de cada perfil e o acesso a funções administrativas pertencem ao escopo de controle de acesso definido para o sistema.

### Vulnerabilidade catalogada

| Risco e requisito | Vulnerabilidade ou categoria | Referência utilizada | Relação com o sistema |
|---|---|---|---|
| `R02` / `RS02` | `OWASP API1:2023 — Broken Object Level Authorization (BOLA)` | [OWASP API Security Top 10 — API1:2023](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/) | Os endpoints de remarcação e cancelamento recebem o identificador do agendamento na rota. Sem uma verificação de autorização sobre o objeto, um paciente autenticado poderia substituir esse valor pelo identificador de uma consulta de terceiro e modificar um registro que não lhe pertence. |

A categoria BOLA corresponde ao vetor do `CA02` baseado na manipulação de `agendamentoId`: o usuário possui acesso legítimo à função, mas tenta ultrapassar o limite de autorização alterando a referência do objeto. Por isso, validar apenas a autenticação, o formato do identificador ou sua imprevisibilidade não satisfaz o `RS02`; a permissão deverá ser conferida no backend para o agendamento e a ação solicitados.

## RS03 — Proteção contra exposição excessiva de dados na API

### Origem e objetivo

| Campo | Definição |
|---|---|
| ID | `RS03` |
| Risco de origem | `R04 — Acesso a dados pessoais do profissional` (`Crítico`, pontuação `12`) |
| Ameaça e caso de abuso relacionados | `T04 — Information Disclosure` |
| Controle relacionado | `C04.1 — Implementar retorno de dados mínimos no backend, expondo apenas os campos necessários para a operação de agendamento ou consulta.` |
| Objetivo | Garantir que as respostas da API enviadas ao cliente contenham estritamente os dados necessários e autorizados para a visualização, impedindo o vazamento de informações pessoais e sensíveis no tráfego de rede. |

### Requisito de segurança

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| `RS03` | `R04` | A API deverá utilizar Objetos de Transferência de Dados (DTOs) ou mecanismos de projeção estruturados para garantir que atributos sensíveis do modelo de domínio original (ex.: CPF, e-mail pessoal, endereço) não sejam serializados e enviados nas respostas HTTP. As regras de visibilidade dos campos deverão ser validadas no servidor com base no perfil de acesso do usuário solicitante. | Revisão de código e testes automatizados (unitários e de integração) deverão demonstrar: (a) o uso exclusivo de DTOs nas respostas dos endpoints de profissionais; (b) que o payload das respostas HTTP não contém campos além daqueles estritamente mapeados e necessários; (c) que a omissão dos campos ocorre no backend, não dependendo de ocultação no frontend. |

### Vulnerabilidade catalogada

| Risco e requisito | Vulnerabilidade ou categoria | Referência utilizada | Relação com o sistema |
|---|---|---|---|
| `R04` / `RS03` | `OWASP API3:2023 - CWE-212 — Improper Disclosure of Information` | https://cwe.mitre.org/data/definitions/212.html | A ausência de filtros no backend faz com que a API envie o objeto completo do banco para o cliente. Um atacante pode interceptar esse tráfego ou inspecionar a resposta da requisição (Network do navegador) para acessar dados que a interface gráfica do usuário simplesmente não desenhou. RS03 mitiga isso garantindo que o dado sensível nunca cruze o Trust Boundary (limite de confiança) da API para a Internet. |

## RS06 — Controle de acesso baseado em papéis e proteção contra atribuição em massa

### Origem e objetivo

| Campo | Definição |
|---|---|
| ID | `RS06` |
| Risco de origem | `R06 — Escalada indevida de privilégios` (`Crítico`, pontuação `12`) |
| Ameaça e caso de abuso relacionados | `T06 — Elevation of Privilege` e `CA06 — Escalada indevida de privilégios` |
| Controle relacionado | `C06.2 — Aplicar o princípio do menor privilégio, validar papéis nos endpoints restritos e prevenir Mass Assignment` |
| Objetivo | Impedir que um usuário autenticado com baixo privilégio acesse endpoints administrativos ou promova seu próprio perfil por meio de campos injetados no payload. |

### Decisão arquitetural

| Campo | Definição |
|---|---|
| Risco tratado | `R06 — Escalada indevida de privilégios` |
| Decisão | Toda rota que executa operação administrativa ou sensível deverá exigir explicitamente o papel `ADMIN` verificado no backend, independentemente do que estiver visível ou oculto no frontend. A vinculação de dados de entrada a objetos de domínio deverá ocorrer por meio de DTOs estritos que não exponham o campo `perfil` ou `role` nas operações de cadastro e atualização acessíveis a usuários comuns. |
| Justificativa | Controles presentes apenas na interface gráfica não constituem barreira de segurança: um usuário mal-intencionado pode contorná-la enviando requisições diretamente à API com ferramentas como Postman ou cURL. A verificação de papel no servidor é a única barreira confiável. Da mesma forma, permitir que o cliente envie o campo `perfil` em um payload de cadastro ou atualização abre a possibilidade de *Mass Assignment*, promovendo a conta a um papel privilegiado sem intervenção administrativa. |
| Componente afetado | Camada de autorização da API (filtros e anotações do Spring Security) e DTOs de criação e atualização de usuário. |
| Resultado esperado | Requisições de usuários com papel `PACIENTE` ou `PROFISSIONAL` direcionadas a endpoints administrativos deverão receber `403 Forbidden` antes de qualquer processamento de negócio. Tentativas de injetar o campo `perfil` num payload de cadastro ou atualização deverão ser silenciosamente ignoradas, pois o campo não estará mapeado no DTO correspondente. |

### Requisito de segurança

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| `RS06` | `R06` | A API deverá verificar o papel do usuário autenticado em cada endpoint que execute operação administrativa ou restrita. Além disso, os DTOs utilizados em operações de criação e atualização de usuários acessíveis a perfis comuns não poderão mapear o campo `perfil` ou equivalente, impedindo sua persistência mesmo que seja enviado pelo cliente. | Testes de autorização deverão demonstrar que: (a) um token de `PACIENTE` válido recebe `403 Forbidden` ao tentar acessar rotas administrativas; (b) um payload de cadastro contendo `"perfil": "ADMIN"` não altera o papel armazenado no banco; (c) rotas legítimas do perfil continuam acessíveis com o respectivo token. |

O requisito deverá ser aplicado, no mínimo, às rotas que realizam as operações administrativas descritas na seção 3.1 do documento de modelagem de ameaças, como manutenção de profissionais, especialidades e gestão global de agendamentos.

### Vulnerabilidade catalogada

| Risco e requisito | Vulnerabilidade ou categoria | Referência utilizada | Relação com o sistema |
|---|---|---|---|
| `R06` / `RS06` | `OWASP API5:2023 — Broken Function Level Authorization` e `OWASP API6:2023 — Unrestricted Access to Sensitive Business Flows` | [OWASP API Security Top 10 — API5:2023](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/) | O sistema expõe rotas administrativas que, sem a verificação de papel no servidor, podem ser acessadas por qualquer usuário autenticado que descubra ou deduza o caminho. Além disso, a ausência de DTOs estritos permite que campos como `perfil` sejam aceitos e persistidos indevidamente por *Mass Assignment*. O `RS06` mitiga ambos os vetores ao centralizar a verificação de autorização no backend e ao restringir o contrato de dados aceito pela API. |

## RS04 — Rastreabilidade e retenção de registros 

### Origem e objetivo

| Campo | Definição|
|---|---| 

| ID | `RS04` |
| Risco de origem | `R03 — Alteração de registro sem rastreabilidade` (`Médio`, pontuação `6`) |
| Ameaça e caso de abuso relacionados | `T03 — Repudiation` e `CA03 — Exclusão sem rastreabilidade` |
| Controle relacionado | `C03.1 — Implementar o padrão Soft Delete` e `C03.2 — Criação de Triggers de auditoria` |
| Objetivo | Garantir que nenhuma exclusão de agendamento seja física e que toda modificação de dados sensíveis gere uma trilha de auditoria irrefutável e externa à aplicação. |

### Decisão arquitetural

| Campo | Definição | 
|---|---| 
| Risco tratado | `R03 — Alteração de registro sem rastreabilidade` |
| Decisão | Adoção obrigatória do padrão *Soft Delete* na camada de aplicação via ORM, combinada com a implementação de *Triggers* de Auditoria nativas no banco de dados relacional para espelhar eventos críticos. |
| Justificativa | O histórico de agendamentos contém dados sensíveis regulados pela LGPD. Confiar exclusivamente no backend para evitar exclusões físicas é insuficiente caso a aplicação falhe ou um atacante interno obtenha acesso direto ao banco. A estratégia de Defesa em Profundidade assegura que o banco atue como uma segunda camada autônoma, registrando ações destrutivas de forma imutável. |
| Componente afetado | Camada de Persistência (Entidades no Backend/ORM e Banco de Dados PostgreSQL). |
| Resultado esperado | Qualquer solicitação de `DELETE` vinda da API será convertida em uma atualização de status. Além disso, alterações de estado no banco gerarão logs automáticos rastreáveis, inviabilizando o repúdio da ação. |

### Requisito de segurança

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| `RS04` | `R03` | O sistema não deve permitir exclusões físicas nas tabelas de agendamentos e pacientes. Toda remoção deve ser lógica. Simultaneamente, o banco de dados deve registrar em uma tabela de auditoria separada os detalhes (data, hora, estado anterior) de qualquer modificação. | Testes de integração deverão demonstrar que a execução de um comando de deleção pela API resulta na atualização da coluna `deleted_at` e que o registro continua existindo fisicamente na tabela. A validação técnica no banco deverá comprovar que a tabela de auditoria recebe uma nova linha de log automaticamente após qualquer operação DML. |

### Vulnerabilidade catalogada

| Risco e requisito | Vulnerabilidade ou categoria | Referência utilizada | Relação com o sistema |
|---|---|---|---|
| `R03` / `RS04` | `OWASP Top 10 2021 — A09:2021-Security Logging and Monitoring Failures` | [OWASP Top 10:2021 — A09 Security Logging and Monitoring Failures](https://owasp.org/Top10/A09_2021-Security_Logging_and_Monitoring_Failures/) | O sistema lida com dados críticos de saúde e agendas. A ausência de logs adequados e a permissão de exclusão física permanente impossibilitam a detecção de fraudes, a recuperação de dados perdidos e a identificação de atores maliciosos internos ou externos. O `RS04` soluciona essa falha impondo retenção lógica e geração de trilha de auditoria contínua na camada de persistência. |
