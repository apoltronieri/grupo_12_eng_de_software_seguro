# Etapa 3 — Projeto de uma Arquitetura Segura

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
