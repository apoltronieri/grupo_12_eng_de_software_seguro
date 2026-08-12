# Etapa 4 — Código Seguro e Testes de Segurança

## PS01 — Validação centralizada de JWT no backend

### Identificação da prática

| Campo | Definição |
|---|---|
| ID | `PS01` |
| Prática | Validar tokens JWT de forma centralizada e negar o acesso quando qualquer verificação falhar |
| Risco relacionado | `R01 — Comprometimento de conta ou token` |
| Requisito relacionado | `RS01 — Validação de tokens de acesso` |
| Controle principal | `C01.2 — Proteger a sessão e validar os tokens de acesso` |
| Forma de realização | Pseudocódigo compatível com um filtro de autenticação do Spring Security |

A prática consiste em realizar toda validação do JWT em um componente central executado antes dos controladores. O algoritmo aceito e a chave de verificação devem vir da configuração confiável do servidor, nunca do próprio token. A autenticação deve falhar de forma segura: se assinatura, algoritmo, emissor, público ou validade temporal forem inválidos, o contexto de segurança não será criado e o endpoint protegido não será executado.

### Testes de segurança definidos antes da solução

Para os testes, considere o endpoint protegido `GET /agendamentos` e uma conta ativa de paciente identificada como `pacienteA`.

| ID | Tipo | Entrada ou ação | Resultado seguro esperado |
|---|---|---|---|
| `TS01.1` | Caso de uso válido | O `pacienteA` envia um JWT com assinatura válida, algoritmo permitido, `iss` e `aud` esperados e prazo de expiração vigente. | A API autentica o `pacienteA`, permite a execução do endpoint e retorna somente os agendamentos autorizados para essa identidade. |
| `TS01.2` | Caso inválido | O `pacienteA` envia um JWT corretamente assinado, mas cujo valor de `exp` já está no passado. | A API retorna `401 Unauthorized`, não cria o contexto autenticado e não executa a consulta de agendamentos. |
| `TS01.3` | Caso malicioso | Um atacante altera o `sub` ou o perfil de um JWT válido para assumir outra identidade, mantendo a assinatura original incompatível com o conteúdo modificado. | A verificação da assinatura falha, a API retorna `401 Unauthorized`, não executa o endpoint e registra apenas o motivo técnico necessário para auditoria, sem gravar o token no log. |

Os testes deverão confirmar tanto a resposta HTTP quanto a ausência de execução da operação protegida. Verificar apenas o código `401` não é suficiente se o controlador, serviço ou repositório tiver sido acionado antes da recusa.

### Pseudocódigo da solução

```text
função filtrarRequisicaoProtegida(requisicao):
    cabecalho = requisicao.obterCabecalho("Authorization")

    se cabecalho não começa exatamente com "Bearer ":
        retornar resposta 401 sem executar o endpoint

    token = extrairToken(cabecalho)

    tentar:
        configuracaoConfiavel = carregarDoServidor(
            algoritmoPermitido,
            chaveDeVerificacao,
            emissorEsperado,
            publicoEsperado
        )

        resultado = bibliotecaJWT.verificar(
            token,
            algoritmo = configuracaoConfiavel.algoritmoPermitido,
            chave = configuracaoConfiavel.chaveDeVerificacao
        )

        se resultado.assinaturaValida é falso:
            retornar resposta 401 sem executar o endpoint

        claims = resultado.claimsVerificadas

        se claims.iss != configuracaoConfiavel.emissorEsperado:
            retornar resposta 401 sem executar o endpoint

        se configuracaoConfiavel.publicoEsperado não pertence a claims.aud:
            retornar resposta 401 sem executar o endpoint

        se claims.exp está ausente ou claims.exp <= instanteAtual:
            retornar resposta 401 sem executar o endpoint

        se claims.nbf existe e claims.nbf > instanteAtual:
            retornar resposta 401 sem executar o endpoint

        conta = repositorioDeUsuarios.buscarAtivaPorId(claims.sub)
        se conta não existe:
            retornar resposta 401 sem executar o endpoint

        contextoDeSeguranca.autenticar(
            identidade = conta.id,
            perfil = perfilPermitido(claims)
        )

        continuar para o endpoint protegido

    capturar erro de token:
        auditoria.registrarFalhaDeAutenticacao(
            motivoClassificado = classificarSemExporToken(erro)
        )
        limpar contexto de segurança
        retornar resposta 401 sem executar o endpoint
```

### Aplicação da prática

1. O `SecurityFilterChain` do Spring Security posiciona o filtro JWT antes dos controladores protegidos.
2. O filtro extrai o token exclusivamente do esquema `Bearer` e não aceita credenciais por parâmetros de URL.
3. Uma biblioteca mantida realiza a verificação criptográfica; o sistema não implementa algoritmos próprios.
4. O servidor utiliza uma lista explícita de algoritmos permitidos e uma chave obtida de configuração confiável.
5. As declarações `iss`, `aud`, `exp` e, quando presente, `nbf` são verificadas antes da criação do contexto autenticado.
6. Qualquer falha limpa o contexto, interrompe a cadeia e aciona um `AuthenticationEntryPoint` que retorna `401 Unauthorized`.
7. O token completo não é escrito em logs. O registro contém apenas informações necessárias para investigar a falha.
8. Somente depois de todas as verificações o Spring Security disponibiliza a identidade aos serviços e controladores.

### Resultado esperado

Com a prática aplicada, somente JWTs cuja integridade e declarações obrigatórias tenham sido validadas criam um contexto autenticado. Tokens expirados, alterados, sem assinatura válida, com algoritmo não permitido ou com emissor ou público incorretos recebem `401 Unauthorized`, e a operação protegida permanece sem execução.

Essa prática implementa o `RS01` e reduz o `R01` ao impedir que conteúdo não confiável do JWT seja aceito como identidade. Os testes deverão ser automatizados e executados na integração contínua quando houver uma implementação executável da API.

### Referências OWASP

- [OWASP REST Security Cheat Sheet — JWT](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#jwt): recomenda proteger a integridade do JWT, não aceitar `alg: none`, determinar o algoritmo pela configuração do servidor e verificar declarações como `iss`, `aud`, `exp` e `nbf`.
- [OWASP API Security Top 10 — API2:2023 Broken Authentication](https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/): considera vulneráveis APIs que não validam a autenticidade ou a expiração dos tokens ou aceitam JWTs sem assinatura ou com assinatura fraca.

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
- os cenários deverão ser aplicados à remarcação (`PATCH /agendamentos/{agendamentoId}/remarcacao`, com `novoHorarioId`) e ao cancelamento (`PATCH /agendamentos/{agendamentoId}/cancelamento`, com `motivo` opcional).

| ID | Tipo | Entrada ou ação | Resultado seguro esperado |
|---|---|---|---|
| `TS02.1` | Caso de uso válido | Para cada operação, o `pacienteA` solicita a alteração de `AG-100` usando somente o payload permitido: um `novoHorarioId` disponível na remarcação ou o `motivo` opcional no cancelamento. | A solicitação é permitida e produz somente o efeito correspondente: o horário de `AG-100` é atualizado na remarcação ou seu estado passa para `CANCELADA` no cancelamento. A titularidade permanece inalterada e a operação é associada ao `pacienteA` no registro de auditoria. |
| `TS02.2` | Caso não autorizado | Para cada operação, o `pacienteA` substitui o identificador da rota por `AG-200` e envia o payload permitido para tentar remarcar ou cancelar o agendamento pertencente ao `pacienteB`. | A API retorna `403 Forbidden`, não revela dados do `pacienteB`, mantém inalterados o horário, o estado e a titularidade de `AG-200` e registra a tentativa recusada sem incluir dados sensíveis no log. |

O teste `TS02.2` deverá verificar o estado persistido antes e depois de cada operação, pois uma resposta de erro isolada não comprova que a alteração foi impedida ou revertida.

### Pseudocódigo da solução

O cancelamento é utilizado abaixo como exemplo representativo da prática. A remarcação deverá repetir a mesma sequência de identificação, autorização por objeto e recusa anterior à persistência, alterando apenas as regras e os dados próprios da operação.

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
