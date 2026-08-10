# Etapa 4 — Código Seguro e Testes de Segurança

## PS02 — Autorização por objeto no backend

### Identificação da prática

| Campo | Definição |
|---|---|
| ID | `PS02` |
| Prática | Autorização por objeto no backend para operações de agendamento |
| Risco relacionado | `R02 — Adulteração de dados de agendamentos` |
| Requisito relacionado | `RS02 — Autorização por objeto em agendamentos` |
| Controle principal | `C02.2 — Verificar a relação entre o usuário autenticado e o agendamento` |
| Forma de realização | Pseudocódigo independente de linguagem ou framework |

A prática consiste em negar por padrão e verificar, no backend e em toda alteração protegida, se a identidade autenticada pode executar a ação solicitada sobre aquele agendamento específico. Validações presentes somente na interface não constituem controle de autorização.

### Testes de segurança definidos antes da solução

Para os dois testes, considere os seguintes dados iniciais:

- o `pacienteA` está autenticado e é titular do agendamento `AG-100`;
- o `pacienteB` é titular do agendamento `AG-200`;
- os dois agendamentos estão no estado `AGENDADA`;
- a operação avaliada é `PATCH /agendamentos/{agendamentoId}/cancelamento`.

| ID | Tipo | Entrada ou ação | Resultado seguro esperado |
|---|---|---|---|
| `TS02.1` | Caso de uso válido | O `pacienteA` solicita o cancelamento de `AG-100` com um payload contendo apenas o motivo opcional. | A solicitação é permitida, o estado de `AG-100` passa para `CANCELADA`, sua titularidade permanece inalterada e a operação é associada ao `pacienteA` no registro de auditoria. |
| `TS02.2` | Caso não autorizado | O `pacienteA` substitui o identificador da rota por `AG-200` e solicita o cancelamento do agendamento pertencente ao `pacienteB`. | A API retorna `403 Forbidden`, não revela dados do `pacienteB`, mantém `AG-200` no estado `AGENDADA` e registra a tentativa recusada sem incluir dados sensíveis no log. |

O teste `TS02.2` deverá verificar o estado persistido antes e depois da requisição, pois uma resposta de erro isolada não comprova que a alteração foi impedida ou revertida.

### Pseudocódigo da solução

```text
função cancelarAgendamento(agendamentoId, entrada, contextoAutenticado):
    ator = contextoAutenticado.exigirIdentidade()
    dados = validarContratoOuRecusar(entrada, camposPermitidos = ["motivo"])

    agendamento = repositorio.buscarPorId(agendamentoId)
    se agendamento não existe:
        retornar resposta 404

    autorizado = politicaDeAgendamentos.podeCancelar(ator, agendamento)
    se autorizado é falso:
        auditoria.registrarTentativaRecusada(
            atorId = ator.id,
            acao = "CANCELAR_AGENDAMENTO",
            recursoId = agendamentoId
        )
        retornar resposta 403 sem dados do agendamento

    se agendamento.estado não permite cancelamento:
        retornar resposta 409 sem alterar o registro

    iniciar transação
        agendamento.cancelar(dados.motivo, ator.id)
        repositorio.salvar(agendamento)
        auditoria.registrarAlteracao(ator.id, agendamento.id, "CANCELAMENTO")
    confirmar transação

    retornar representação permitida do agendamento cancelado
```

### Aplicação da prática

1. A identidade é obtida do contexto de autenticação; valores enviados no payload não podem substituir o ator autenticado.
2. O backend rejeita campos que não estejam previstos para a operação. No cancelamento, o cliente pode informar o motivo, mas não pode alterar paciente, profissional ou estado diretamente.
3. O agendamento é localizado e submetido à política de autorização correspondente à ação solicitada.
4. A política nega o acesso por padrão e somente permite a operação quando a relação exigida entre ator e objeto estiver comprovada.
5. As regras de estado são verificadas após a autorização e antes da persistência.
6. A alteração e seu registro de auditoria são persistidos de forma transacional.
7. Falhas de autorização encerram o fluxo antes de qualquer modificação e retornam uma resposta sem informações do agendamento de terceiro.

A mesma verificação de autorização por objeto deverá proteger a remarcação. Cada operação poderá possuir regras de negócio próprias, mas nenhuma delas poderá omitir esse controle.

### Resultado esperado

Com a prática aplicada, a simples troca de `agendamentoId` não permite remarcar ou cancelar consultas de terceiros. Operações legítimas continuam disponíveis aos usuários autorizados, enquanto tentativas indevidas são recusadas antes da persistência e produzem evidência suficiente para auditoria.

Essa prática implementa o `RS02` e o controle `C02.2`; o registro das tentativas recusadas também apoia o controle `C02.4`. A validação dos campos e do estado apenas contextualiza a posição da autorização no fluxo e não substitui os demais controles de `R02`. Os testes deverão ser automatizados quando existir uma implementação executável da API e deverão bloquear a integração de alterações que quebrem a autorização esperada.

### Referências OWASP

- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html): recomenda menor privilégio, negação por padrão, validação das permissões em toda requisição e testes da lógica de autorização.
- [OWASP API Security Top 10 — API1:2023 Broken Object Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/): orienta verificar se o usuário autenticado pode executar a ação sobre o registro sempre que a API utiliza um identificador fornecido pelo cliente.
