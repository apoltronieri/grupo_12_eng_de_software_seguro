# Etapa 3 — Projeto de uma Arquitetura Segura

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