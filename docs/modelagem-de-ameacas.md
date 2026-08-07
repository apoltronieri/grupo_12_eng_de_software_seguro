# Modelagem de Ameaças com STRIDE

> Documento em construção.

## 1. Identificação do sistema

### 1.1 Nome do sistema

Sistema de Agendamento de Consultas.

### 1.2 Integrantes do grupo

Aira Arima, Ana Carolina Poltronieri, Graziela Bitencourt, Luan Martins, Lucie Grillo e Mariana Ferrao.

### 1.3 Endereço do repositório

- **Nome:** `grupo_12_eng_de_software_seguro`
- **Endereço:** https://github.com/apoltronieri/grupo_12_eng_de_software_seguro

### 1.4 Justificativa da escolha

## 2. Descrição do sistema

### 2.1 Problema que o sistema resolve

### 2.2 Usuários do sistema

### 2.3 Principais funcionalidades

#### Autenticação e gerenciamento de sessões

O backend utiliza Spring Security para autenticar usuários e autorizar o acesso de acordo com seus perfis. No login, as credenciais são recebidas exclusivamente por HTTPS. O Spring Security localiza a conta, compara a senha apresentada com o hash armazenado e, após a autenticação bem-sucedida, emite um token de acesso JWT. A senha em texto puro não é armazenada nem incluída no token.

O JWT contém apenas as informações necessárias para autorização, como identificador do usuário (`sub`), perfil de acesso, instante de emissão (`iat`), expiração (`exp`), emissor (`iss`), público destinatário (`aud`) e identificador único do token (`jti`). O token possui validade curta e é assinado pelo servidor com algoritmo previamente definido. A chave privada de assinatura deve permanecer em um cofre de segredos, enquanto a aplicação utiliza a chave pública correspondente para validar a assinatura.

A aplicação mantém sessões sem estado no backend: em cada requisição protegida, o cliente envia o token de acesso no cabeçalho `Authorization: Bearer`. Um filtro do Spring Security verifica a assinatura, o algoritmo permitido, o emissor, o público, a validade temporal e a existência da conta antes de preencher o contexto de segurança. Somente depois dessas verificações as regras de autorização por perfil são aplicadas.

Para evitar que um token de longa duração permaneça exposto, a renovação utiliza um token de atualização aleatório, de uso restrito e com rotação a cada renovação. Ele é armazenado no cliente em cookie `HttpOnly`, `Secure` e `SameSite`, e seu resumo criptográfico é mantido no servidor. Logout, troca de senha ou detecção de reutilização revogam a cadeia de renovação. Tokens de acesso não devem ser gravados em URLs, logs ou armazenamento local acessível por scripts.

#### Prevenção de conflitos de horário

A disponibilidade mostrada ao usuário é apenas uma consulta preliminar e não garante a reserva. No momento da confirmação, o backend inicia uma transação, valida novamente a disponibilidade e tenta persistir o agendamento. O banco de dados é a autoridade final para decidir se o horário ainda está livre.

O método de confirmação é executado em uma transação. A entidade que representa o horário pode utilizar lock otimista com `@Version` no Hibernate/JPA, permitindo detectar quando duas requisições leram a mesma versão e tentaram alterá-la. O nível de isolamento da transação deve impedir que a decisão seja baseada em uma atualização concorrente ainda não consolidada. Se a versão tiver mudado, a segunda operação falha e deve refazer a consulta de disponibilidade, sem sobrescrever silenciosamente a primeira reserva.

Além do controle transacional, para consultas com horários fixos, uma restrição de unicidade sobre profissional, data e horário impede duas reservas do mesmo intervalo. Se houver consultas com durações variáveis, deve ser utilizada uma restrição que rejeite a sobreposição entre os intervalos de início e fim do mesmo profissional. A restrição considera apenas agendamentos em estados que ocupam a agenda, como pendente ou confirmado.

Quando duas requisições concorrentes tentam reservar o mesmo período, apenas uma transação é confirmada. A outra viola a restrição do banco, sofre rollback e recebe a resposta HTTP `409 Conflict`, devendo escolher outro horário. Assim, a verificação feita pela aplicação melhora a experiência do usuário, mas a garantia contra condições de corrida permanece no banco de dados.

### 2.4 Informações armazenadas ou transmitidas

### 2.5 Recursos que precisam ser protegidos

## 3. Usuários, ativos e pontos de interação

### 3.1 Usuários e perfis de acesso

### 3.2 Ativos importantes

A camada de persistência deste sistema, baseada em um banco de dados relacional, é responsável por armazenar os ativos mais críticos da aplicação. O controle estrutural do banco é versionado através de ferramentas de *Migrations*. Para garantir a segurança e a adequação à LGPD, destacam-se os seguintes ativos e diretrizes de proteção:

- **Credenciais de Acesso:** Senhas não são armazenadas em texto plano. Utiliza-se criptografia unidirecional com *hashing* de alto custo ou extensões nativas.
- **Dados Pessoais e Histórico de Consultas (LGPD):** Dados de identificação, informações de contato e o histórico de agendamentos dos pacientes exigem proteção reforçada em repouso, utilizando técnicas como *Transparent Data Encryption (TDE)* para o armazenamento físico.
- **Logs de Auditoria (Audit Trails):** Trilha de auditoria essencial para registrar operações críticas, garantindo a rastreabilidade das ações e prevenindo o repúdio.
- **Registros de Agendamento (Integridade Referencial):** Conforme regra de negócio, os registros de consulta não sofrem deleção física, devendo adotar a abordagem de *Soft Delete* para manter o histórico intacto em caso de cancelamentos.

### 3.3 Pontos de interação

### 3.4 Interface conceitual da API

| Método | Rota conceitual | Finalidade | Perfil ou contexto esperado | Dados de entrada | Validações principais |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `POST` | `/usuarios` | Cadastro de paciente | Público | Dados de identificação e senha | Validar dados de entrada; verificar duplicidade. |
| `POST` | `/autenticacao` | Autenticação de usuário | Público | Credenciais (ex: e-mail e senha) | Validar as credenciais apresentadas. |
| `GET` | `/profissionais` | Listar profissionais | Paciente | Parâmetros de busca/filtros opcionais | Validar formato dos parâmetros de busca. |
| `GET` | `/profissionais/{profissionalId}` | Consultar um profissional | Paciente | ID na rota | Verificar se profissional existe e está ativo. |
| `GET` | `/especialidades` | Listar especialidades | Público ou paciente | Nenhum | Retornar apenas especialidades disponíveis. |
| `GET` | `/profissionais/{profissionalId}/horarios` | Consultar horários disponíveis | Paciente | ID na rota | Verificar se o profissional existe e retornar apenas horários disponíveis. |
| `POST` | `/agendamentos` | Criar agendamento | Paciente | `profissionalId`, `horarioId` (payload) | Validar disponibilidade do horário no servidor. |
| `GET` | `/agendamentos` | Listar os próprios agendamentos | Paciente | Filtros opcionais | Identidade e autorização via contexto de autenticação. |
| `GET` | `/agendamentos/{agendamentoId}` | Consultar um agendamento relacionado ao usuário | Paciente ou profissional | Identificador na rota | Verificar se o agendamento pertence ao paciente ou está associado à agenda do profissional autenticado. |
| `PATCH` | `/agendamentos/{agendamentoId}/remarcacao` | Remarcar | Paciente | `novoHorarioId` (payload) | Confirmar titularidade; validar disponibilidade do novo horário. |
| `PATCH` | `/agendamentos/{agendamentoId}/cancelamento` | Cancelar | Paciente | `motivo` opcional (payload) | Confirmar titularidade; alterar estado para `CANCELADA` sem exclusão. |
| `GET` | `/profissionais/me/agenda` | Listar a própria agenda | Profissional | Filtros opcionais | Identidade via contexto de autenticação. |
| `POST` | `/profissionais/me/disponibilidades` | Cadastrar disponibilidade | Profissional | `inicio`, `fim` (payload) | Identidade via autenticação; validação cronológica das datas e verificação de possíveis conflitos de horário. |
| `PATCH` | `/profissionais/me/disponibilidades/{disponibilidadeId}` | Atualizar disponibilidade | Profissional | `inicio`, `fim` (payload) | Identidade via autenticação; titularidade da disponibilidade. |

A API também deverá disponibilizar operações administrativas para manutenção de usuários, profissionais e especialidades.

#### Payloads conceituais

**Criação de agendamento:**
```json
{
  "profissionalId": "identificador-do-profissional",
  "horarioId": "identificador-do-horario-disponivel"
}
```
* O paciente não deve enviar livremente o próprio identificador. A identidade do paciente deverá vir exclusivamente do contexto de autenticação.
* O cliente não poderá definir diretamente o estado da consulta no momento da criação.
* O servidor deverá verificar se o horário continua disponível antes de confirmar a operação.

**Remarcação:**
```json
{
  "novoHorarioId": "identificador-do-novo-horario"
}
```
* O servidor deverá confirmar que o agendamento pertence ao usuário autorizado.
* Deverá validar novamente a disponibilidade, para impedir conflito na agenda do paciente e do profissional.
* Não deverá aceitar alteração arbitrária de paciente, profissional ou estado se o cliente tentar enviar essas informações no payload.

**Cancelamento:**
```json
{
  "motivo": "texto opcional"
}
```
* O identificador do agendamento estará na rota, não necessitando vir no corpo.
* O cancelamento deverá alterar o estado para `CANCELADA`. O registro não deverá ser excluído.
* O servidor deverá validar rigorosamente se o usuário pode cancelar aquele agendamento.

**Disponibilidade do profissional:**
```json
{
  "inicio": "data e horário inicial",
  "fim": "data e horário final"
}
```
* O profissional deverá ser identificado pelo contexto de autenticação, e não por um identificador enviado no payload.

## 4. Visão geral da arquitetura ou fluxo

### 4.1 Fluxo de autenticação

1. O usuário envia suas credenciais ao endpoint de login por HTTPS.
2. O Spring Security valida a conta e o hash da senha.
3. O serviço de tokens gera e assina o JWT de acesso.
4. O cliente envia o JWT no cabeçalho `Authorization` das requisições protegidas.
5. O filtro de autenticação valida assinatura, algoritmo, emissor, público e expiração.
6. O Spring Security autoriza ou nega a operação conforme o perfil autenticado.

### 4.2 Fluxo de criação de agendamento

1. O paciente consulta os horários disponíveis de um profissional.
2. O paciente solicita a reserva de um intervalo.
3. O backend autentica o paciente e valida os dados da solicitação.
4. Em uma transação, o backend verifica e tenta registrar o intervalo.
5. A restrição do banco permite apenas um agendamento ativo por horário ou impede intervalos sobrepostos.
6. O backend confirma a reserva ou responde `409 Conflict` se outra transação tiver ocupado o período.

## 5. Modelagem de ameaças com STRIDE

| ID | Categoria STRIDE | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---|---|---|---|
| `T01` | Spoofing | Token JWT de sessão | Um atacante explora uma vulnerabilidade XSS para executar um script no navegador, captura um JWT armazenado indevidamente no `localStorage` e reutiliza o token para se passar por um paciente, profissional ou administrador. | Acesso indevido às contas de médicos e pacientes, exposição de dados pessoais e realização de agendamentos ou cancelamentos fraudulentos. |
| `T02` | Tampering | API de agendamentos, registros de consultas e agendas | Um paciente autenticado modifica identificadores ou campos enviados em uma operação de criação, remarcação ou cancelamento. Caso a API aceite campos indevidos ou não valide a titularidade, a disponibilidade e a integridade da operação, dados de um agendamento poderão ser alterados de forma não autorizada. | Alteração ou cancelamento indevido de consultas, conflitos nas agendas, perda de integridade dos registros e prejuízo ao atendimento. |
| `T03` | Repudiation | Banco de Dados, Tabela de Consultas e Logs de Auditoria | Um atendente desonesto ou usuário interno exclui fisicamente um agendamento do banco e nega a ação, não havendo trilhas de auditoria para rastrear o evento. | Perda de integridade dos dados, disputas legais entre clínica e paciente, e impossibilidade de responsabilizar o autor da fraude. |

### 5.1 Detalhamento da ameaça T01 — Falsificação ou roubo de token

No cenário analisado, o front-end descumpre a especificação segura e armazena o JWT no `localStorage`. Como esse armazenamento pode ser lido por JavaScript executado na mesma origem, uma falha de XSS em um campo de texto permite que um script malicioso capture o token e o envie ao atacante. Outros cenários de roubo incluem exposição por conexão sem TLS, logs, URLs, extensões maliciosas ou dispositivo comprometido. A falsificação também pode ocorrer se a chave de assinatura vazar, se um segredo simétrico for fraco ou se o backend aceitar algoritmos não previstos, tokens sem assinatura, emissor ou público incorretos ou campos de validade sem verificação.

Depois de obter ou forjar o token, o atacante o envia como `Bearer Token`. Se o backend considerar o token válido, as ações serão associadas à identidade da vítima. A assinatura protege a integridade do JWT, mas não impede o uso de um token válido que tenha sido roubado; por isso, validade curta, proteção no armazenamento, rotação e revogação da renovação são necessárias.

### 5.2 Detalhamento da ameaça T03 — Exclusão sem rastreabilidade (Repudiation)

No cenário analisado, as tabelas do banco de dados descumprem a regra de negócio e não utilizam *Soft Delete*, permitindo a exclusão física e permanente de registros de agendamento. Além disso, a aplicação não gera trilhas de auditoria para operações de escrita ou deleção. 

Dessa forma, um usuário interno mal-intencionado pode acessar a funcionalidade de deleção e excluir um agendamento. Quando o paciente reclamar do sumiço da consulta, a administração não conseguirá rastrear os logs de aplicação para vincular a exclusão a um usuário específico. O autor da ação pode negar que cancelou a consulta, gerando disputas legais e quebra de confiança no sistema.

## 6. Casos de abuso

### CA01 — Falsificação de identidade por roubo de JWT via XSS

- **Ator:** usuário mal-intencionado, utilizando um script automatizado.
- **Objetivo:** assumir a identidade de um usuário legítimo para acessar informações ou executar operações no sistema de agendamento.
- **Condições necessárias:** o front-end armazena o JWT no `localStorage`; um campo de texto fornecido pelo usuário é exibido sem tratamento seguro, permitindo XSS; e o token capturado permanece válido e é aceito pelo backend.
- **Fluxo de abuso:**
  1. O atacante insere um script malicioso em um campo de texto do sistema que posteriormente será exibido para outros usuários.
  2. Um paciente, profissional ou administrador legítimo acessa a página que contém o conteúdo injetado.
  3. O navegador executa o script, que lê o JWT armazenado no `localStorage` e o envia ao atacante.
  4. O atacante usa o token capturado no cabeçalho `Authorization: Bearer` de requisições automatizadas.
  5. Como o JWT é legítimo e ainda está válido, o backend associa as requisições à conta e ao perfil da vítima.
  6. O atacante consulta informações e cria ou cancela agendamentos até que o token expire ou seja revogado.
- **Impacto esperado:** violação de privacidade, alteração indevida da agenda, cancelamento ou criação fraudulenta de consultas, possível acesso a dados sensíveis e perda de confiança no sistema. O impacto aumenta caso a identidade comprometida pertença a um profissional ou administrador.
- **Categorias STRIDE relacionadas:** Spoofing, com possíveis consequências de Information Disclosure e Tampering.

### CA02 — Adulteração de dados de um agendamento

- **Ator:** Paciente autenticado com intenção maliciosa.

- **Objetivo:** Alterar indevidamente os dados de um agendamento, afetando uma consulta que não poderia modificar ou enviando valores diferentes dos permitidos pela operação.

- **Condições necessárias:**
  - O ator possui uma conta válida e acesso a uma operação de agendamento.
  - O ator consegue modificar a requisição antes de enviá-la à API.
  - A API não valida adequadamente a titularidade do agendamento, os campos permitidos ou a disponibilidade informada.

- **Fluxo de abuso:**
  1. O paciente autentica-se normalmente na plataforma.
  2. O paciente inicia uma operação de remarcação de consulta.
  3. Antes de enviar a requisição, altera o identificador do agendamento presente na rota ou acrescenta campos que não deveriam ser controlados pelo cliente, como paciente, profissional ou estado da consulta.
  4. A requisição adulterada é enviada à API.
  5. A API processa os valores recebidos sem verificar adequadamente a titularidade do agendamento, os campos permitidos ou a disponibilidade do horário.
  6. O sistema altera indevidamente o registro da consulta e a agenda relacionada.

- **Impacto esperado:** Alteração não autorizada de consultas, conflitos de horários, cancelamentos ou remarcações indevidas, perda de integridade dos registros e prejuízo para pacientes e profissionais.

- **Categorias STRIDE relacionadas:** Tampering.

### CA03 — Exclusão sem rastreabilidade

- **Ator:** Usuário interno (ex: atendente) ou sistema comprometido.
- **Objetivo:** Excluir um agendamento no banco de dados para encobrir falhas operacionais ou fraudar a agenda, sem deixar provas de quem executou a ação.
- **Condições necessárias:** As tabelas do banco de dados não utilizam *Soft Delete* e a aplicação não gera trilhas de auditoria para registrar alterações e deleções.
- **Fluxo de abuso:**
  1. O usuário mal-intencionado acessa a funcionalidade de deleção no sistema utilizando suas credenciais.
  2. A linha correspondente ao agendamento na tabela de consultas é excluída permanentemente via comando `DELETE` direto no banco de dados.
  3. O paciente entra em contato reclamando do sumiço da consulta, ou o profissional nota um buraco na agenda.
  4. O administrador do sistema tenta investigar quem apagou o registro.
  5. Por não haver trilha de auditoria, não são encontrados rastros ou logs vinculando a exclusão ao usuário, permitindo que ele negue a autoria.
- **Impacto esperado:** Perda do histórico de consultas e informações gerenciais, impossibilidade de responsabilizar o autor da exclusão e potenciais processos judiciais e disputas com pacientes.
- **Categorias STRIDE relacionadas:** Repudiation.

## 7. Considerações finais

### 7.1 Ameaças mais preocupantes

### 7.2 Ativos mais importantes

### 7.3 Abusos de maior impacto

### 7.4 Dificuldades encontradas

---

# Etapa 2 — Análise, Priorização e Tratamento de Riscos

## 8. Critérios de avaliação

### 8.1 Critérios de probabilidade

| Valor | Classificação | Critério |
|---:|---|---|
| 1 | Baixa | O evento depende de condições incomuns, acesso muito específico ou grande capacidade técnica. |
| 2 | Média-baixa | O evento é possível, mas depende de uma vulnerabilidade ou condição específica. |
| 3 | Média-alta | O evento é plausível e pode ocorrer em situações comuns de uso ou ataque. |
| 4 | Alta | O evento pode ocorrer com facilidade, frequência ou durante condições previsíveis do sistema. |

### 8.2 Critérios de impacto

| Valor | Classificação | Critério |
|---:|---|---|
| 1 | Baixo | Causa pequeno transtorno e pode ser corrigido rapidamente. |
| 2 | Moderado | Causa interrupção ou inconsistência limitada, com possibilidade de recuperação. |
| 3 | Alto | Causa prejuízo relevante aos usuários, ao negócio, à administração ou à privacidade. |
| 4 | Muito alto | Pode afetar muitos usuários, comprometer operações críticas ou causar prejuízo grave. |

### 8.3 Cálculo e classificação

A pontuação é calculada por `Probabilidade × Impacto` e classificada conforme a escala abaixo.

| Pontuação | Nível do risco |
|---:|---|
| 1 a 3 | Baixo |
| 4 a 7 | Médio |
| 8 a 11 | Alto |
| 12 a 16 | Crítico |

## 9. Registro de riscos

| ID | Origem STRIDE | Evento de risco | Vulnerabilidade ou condição | Probabilidade | Impacto | Pontuação | Nível |
|---|---|---|---|---:|---:|---:|---|
| R01 | T01 — Spoofing / CA01 | Um atacante rouba um JWT e assume a conta de um paciente, profissional ou administrador para consultar dados e realizar operações em nome da vítima. | JWT armazenado no `localStorage`, campo vulnerável a XSS, token com validade excessiva ou ausência de controles adicionais para detectar e interromper a sessão comprometida. | 3 | 4 | 12 | Crítico |

### 9.1 Justificativa do R01 — Comprometimento de conta ou token

**Probabilidade — 3 (média-alta):** o ataque depende de uma vulnerabilidade XSS e de o JWT estar acessível ao JavaScript, mas essas condições são plausíveis em uma aplicação web que recebe e exibe campos de texto. Uma vez executado na mesma origem, o script consegue ler o `localStorage` sem exigir acesso ao servidor ou grande capacidade técnica. A reutilização de um token do tipo *bearer* também é simples enquanto ele estiver válido. A probabilidade não foi classificada como 4 porque ainda depende da presença de XSS, do armazenamento inadequado e de uma vítima acessar o conteúdo malicioso.

**Impacto — 4 (muito alto):** o token permite agir com as permissões da vítima. O comprometimento pode expor dados pessoais e informações sobre consultas, causar agendamentos e cancelamentos fraudulentos e indisponibilizar horários legítimos. Se a vítima for profissional ou administrador, o atacante poderá alcançar agendas de vários pacientes e operações de maior privilégio. Há ainda possíveis danos à privacidade, à reputação e à continuidade do atendimento.

**Nível calculado — crítico:** `3 × 4 = 12`. O nível é adequado porque combina um cenário plausível com consequências graves sobre identidade, privacidade e funcionamento do serviço. O R01 deve receber tratamento prioritário antes da disponibilização pública do sistema.

## 10. Priorização

O R01 tem prioridade inicial alta por atingir a identidade digital, que é a base das demais regras de autorização. Se o backend confiar em um token comprometido, validações de titularidade continuarão tratando o atacante como usuário legítimo. O risco também pode afetar diferentes perfis e permitir várias ações fraudulentas durante a validade do token. Por isso, os controles preventivos de armazenamento, XSS e duração da sessão devem preceder controles que dependem da identidade autenticada.

## 11. Tratamento do risco R01

### 11.1 Estratégia

A estratégia escolhida é **reduzir**. A autenticação é necessária ao sistema e, portanto, não é viável evitar a atividade que origina o risco. Os controles propostos buscam reduzir a probabilidade de furto ou falsificação, limitar o período de uso de um token comprometido e permitir detecção e resposta rápidas.

### 11.2 Mapeamento para as funções do NIST CSF 2.0

| Risco | Govern | Identify | Protect | Detect | Respond | Recover |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| R01 | X |  | X | X | X | X |

- **Govern:** definir política de validade e rotação de tokens e chaves, responsáveis e prazo de revisão.
- **Protect:** proteger a sessão com cookie seguro, MFA, validade curta, rotação de tokens e chaves e prevenção de XSS.
- **Detect:** monitorar reutilização de token de atualização, múltiplos acessos incompatíveis e operações anormais.
- **Respond:** revogar sessões, bloquear temporariamente a conta, rotacionar chaves comprometidas e notificar o usuário.
- **Recover:** recuperar a conta de forma verificada e restabelecer o acesso legítimo após o incidente.

A função **Identify** não foi marcada neste registro porque o risco e os ativos envolvidos já estão identificados em T01, CA01 e R01. Ela continua relevante ao processo geral de gestão de riscos, mas não representa o foco dos controles selecionados para este plano.

### 11.3 Plano de tratamento

| Risco | Estratégia | Controles propostos | Funções relacionadas | Responsáveis | Evidências e verificação |
|---|---|---|---|---|---|
| R01 | Reduzir | Não armazenar JWT no `localStorage`; usar cookie `HttpOnly`, `Secure` e `SameSite`; codificar a saída de conteúdo não confiável e aplicar Content Security Policy contra XSS; exigir MFA/TOTP, principalmente para profissionais e administradores; usar token de acesso de curta duração e token de atualização com rotação e detecção de reutilização; rotacionar as chaves de assinatura e rejeitar algoritmos ou chaves não autorizados; revogar sessões em caso de incidente. | Govern, Protect, Detect, Respond e Recover | Equipe de desenvolvimento; equipe de infraestrutura/segurança; responsável pelo produto; suporte no atendimento a contas comprometidas. | Testes JUnit/Mockito e de integração verificando que tokens expirados, alterados ou com assinatura inválida recebem `401 Unauthorized`; testes no navegador confirmando que scripts não leem o cookie; testes de MFA; registro da rotação de chaves; logs e alertas de reutilização e múltiplos acessos; simulação documentada de revogação e recuperação de conta. |

### 11.4 Ordem inicial de implementação

1. Remover o JWT do `localStorage`, configurar cookies seguros e corrigir a causa de XSS com codificação de saída e Content Security Policy.
2. Implementar tokens de acesso curtos, rotação do token de atualização, revogação e rejeição de reutilização.
3. Definir armazenamento protegido e rotação das chaves de assinatura, incluindo procedimento para comprometimento.
4. Implantar MFA/TOTP, começando por profissionais e administradores.
5. Criar monitoramento, alertas, procedimentos de resposta e recuperação de conta.
6. Automatizar testes unitários, integrados e de segurança que produzam evidências dos controles.

A ordem prioriza a remoção das condições diretamente exploradas pelo CA01. Em seguida, reduz a janela de uso de credenciais roubadas e protege a assinatura. MFA e monitoramento complementam a defesa e a resposta, enquanto os testes verificam continuamente o comportamento esperado.

### 11.5 Estimativa do risco residual

| Risco | Nível inicial | Nível residual esperado | Condição para aceitar o residual |
|---|---|---|---|
| R01 | Crítico — P3 × I4 = 12 | Médio — P1 × I4 = 4 | Aceitar somente após implementar e testar todos os controles prioritários, comprovar que scripts não acessam a credencial, validar respostas `401` para tokens inválidos ou expirados, testar revogação e recuperação e manter monitoramento e revisão periódica das chaves. A aceitação deverá ser aprovada pelo responsável pelo produto e pela segurança. |

O impacto residual permanece 4 porque um eventual comprometimento ainda pode causar consequências graves. A redução esperada ocorre na probabilidade, de 3 para 1, devido às camadas preventivas e à limitação da janela de exploração. Esse valor é apenas uma estimativa: o risco não poderá ser considerado reduzido até que os controles sejam implementados e as evidências sejam obtidas.

## 8. Etapa 2 — Avaliação e tratamento de riscos

### R02 — Adulteração de dados de agendamentos

#### Identificação do risco

| Campo                     | Descrição                                                                                                                                                                                                                                                   |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ID do risco               | `R02`                                                                                                                                                                                                                                                       |
| Ameaça de origem          | `T02 — Tampering`                                                                                                                                                                                                                                           |
| Caso de abuso relacionado | `CA02 — Adulteração de dados de um agendamento`                                                                                                                                                                                                             |
| Evento de risco           | Um usuário autenticado modifica identificadores ou campos de uma requisição relacionada a um agendamento, e a API processa os dados adulterados sem validar adequadamente a titularidade, os campos permitidos, a disponibilidade ou a transição de estado. |
| Causa ou vulnerabilidade  | Validação insuficiente no servidor, aceitação de campos não permitidos e ausência de verificação adequada da relação entre o usuário autenticado e o agendamento.                                                                                           |
| Ativos afetados           | Registros de consultas, agendas de pacientes e profissionais, API de agendamentos, integridade dos dados e disponibilidade operacional do atendimento.                                                                                                      |
| Consequências             | Remarcações ou cancelamentos indevidos, conflitos de horários, alteração de consultas de terceiros, perda de integridade dos registros e prejuízos aos pacientes e profissionais.                                                                           |

#### Avaliação do risco

| Critério      |    Valor | Justificativa                                                                                                                                                                                  |
| ------------- | -------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Probabilidade |      `3` | A adulteração pode ser tentada por usuários autenticados com acesso às operações de agendamento. A exploração não exige acesso administrativo, mas depende de falhas de validação no servidor. |
| Impacto       |      `4` | Uma exploração bem-sucedida pode alterar ou cancelar consultas, afetar agendas de terceiros, comprometer a integridade dos registros e prejudicar o atendimento.                               |
| Pontuação     |     `12` | Resultado de `3 × 4 = 12`.                                                                                                                                                                     |
| Classificação |  Crítico | A pontuação está entre 12 e 16.                                                                                                                                                                |

#### Estratégia de tratamento

| Campo         | Definição                                                                                                                                                                                                                                                                          |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Estratégia    | Reduzir                                                                                                                                                                                                                                                                            |
| Justificativa | As funcionalidades de criação, remarcação e cancelamento são essenciais ao sistema e não podem ser simplesmente eliminadas. O risco deve ser reduzido por meio de validações no servidor, controle dos campos aceitos, verificação de titularidade e monitoramento das alterações. |

#### Controles propostos e mapeamento NIST CSF 2.0

| ID | Função do NIST CSF 2.0 | Controle proposto | Responsável | Evidência esperada |
| -- | ---------------------- | ----------------- | ----------- | ------------------ |
| `C02.1` | Protect — Proteger | Validar no servidor todos os dados recebidos, aceitar somente os campos permitidos e rejeitar campos inesperados ou não modificáveis pelo cliente. | Equipe de desenvolvimento da API | Especificação dos campos permitidos e testes com campos inválidos, adicionais ou proibidos. |
| `C02.2` | Protect — Proteger | Obter a identidade pelo contexto de autenticação e verificar a relação entre o usuário e o agendamento antes de permitir alterações. | Equipe de desenvolvimento e equipe responsável por autenticação e autorização | Regras de autorização documentadas e testes de tentativa de alteração de agendamento de terceiro. |
| `C02.3` | Protect — Proteger | Validar novamente a disponibilidade do horário, os conflitos de agenda e as transições de estado permitidas. | Equipe de desenvolvimento das regras de negócio e equipe responsável pela persistência | Casos de teste de remarcação, conflitos e transições inválidas. |
| `C02.4` | Detect — Detectar | Registrar tentativas rejeitadas de alteração de campos proibidos, identificadores incompatíveis ou agendamentos pertencentes a outro usuário. | Equipe de desenvolvimento e equipe de operação e monitoramento | Registros de auditoria e exemplos de eventos detectados. |
| `C02.5` | Respond — Responder | Definir procedimento para investigar alterações suspeitas, conter o acesso abusivo e corrigir consultas afetadas. | Equipe de segurança e administração da plataforma | Procedimento de resposta a incidentes e registros das ações realizadas. |
| `C02.6` | Recover — Recuperar | Preservar histórico suficiente para restaurar consultas adulteradas e reconciliar as agendas afetadas. | Equipe responsável pela persistência e equipe de operação | Histórico de alterações, registros de auditoria e procedimento de restauração testado. |

#### Ordem de implementação

1. Definição dos campos permitidos e das transições válidas.
2. Implementação das validações obrigatórias no servidor.
3. Verificação de titularidade e autorização.
4. Validação de disponibilidade, conflitos e estados.
5. Implantação dos registros de auditoria e detecção.
6. Definição e teste dos procedimentos de resposta e recuperação.
7. Execução de testes com requisições adulteradas.

Os controles preventivos devem ser priorizados, mas a redução real do risco deverá ser comprovada por implementação e testes.

#### Risco residual

| Critério | Valor estimado | Justificativa |
|---|---:|---|
| Probabilidade residual estimada | `1` | Espera-se que a validação no servidor, o controle dos campos aceitos, a verificação de titularidade e os testes reduzam significativamente a possibilidade de uma requisição adulterada ser aceita. |
| Impacto residual estimado | `4` | Caso uma falha ainda permita a adulteração, o impacto potencial sobre consultas e agendas permanecerá elevado. |
| Pontuação residual estimada | `4` | Resultado esperado de `1 × 4 = 4`. |
| Classificação residual estimada | Médio | A pontuação estimada 4 está na faixa de 4 a 7. |
| Condição para aceitar o residual | Após implementação e testes | O residual somente poderá ser aceito após a implementação dos controles, a execução de testes de adulteração e a confirmação de que requisições indevidas são rejeitadas e registradas. |
| Responsável pela aprovação | Gestão da plataforma | A aprovação deverá ocorrer com validação da equipe de segurança. |
| Revisão | Após testes, mudanças ou incidentes | Reavaliar após os testes, mudanças nos endpoints ou incidentes relacionados à integridade dos agendamentos. |

##### Comparação entre risco inicial e residual esperado

| Risco | Nível inicial | Nível residual esperado | Condição |
|---|---|---|---|
| `R02` | Crítico — pontuação `12` | Médio — pontuação estimada `4` | Resultado condicionado à implementação e à validação dos controles por meio de testes. |

A redução apresentada é apenas uma estimativa. O risco residual deverá ser recalculado após a implementação e os testes dos controles propostos.

### R03 — Alteração de registro sem rastreabilidade

#### Identificação do risco

| Campo                     | Descrição                                                                                                                                                                                                                                |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ID do risco               | `R03`                                                                                                                                                                                                                                    |
| Ameaça de origem          | `T03 — Repudiation`                                                                                                                                                                                                                      |
| Caso de abuso relacionado | `CA03 — Exclusão sem rastreabilidade`                                                                                                                                                                                                    |
| Evento de risco           | Um usuário interno ou sistema comprometido exclui ou altera um agendamento diretamente no banco de dados e nega a ação, não havendo trilha de auditoria para investigar e comprovar a autoria.                                           |
| Causa ou vulnerabilidade  | Ausência de *Soft Delete* nas entidades, falta de *triggers* de auditoria no banco de dados e inexistência de armazenamento externo e imutável de logs de ações destrutivas.                                                             |
| Ativos afetados           | Tabela de Consultas, Dados Pessoais de Pacientes (LGPD) e Logs de Auditoria.                                                                                                                                                             |
| Consequências             | Perda de histórico de agendamentos, impossibilidade de auditar ações maliciosas, violação de exigências legais da LGPD e disputas legais entre clínica e paciente.                                                                       |

#### Avaliação do risco

| Critério      |    Valor | Justificativa                                                                                                                                                                                  |
| ------------- | -------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Probabilidade |      `2` | Média-baixa. O evento depende de um funcionário ter acesso às rotas de deleção ou acesso direto/privilegiado ao banco de dados para executar a ação.                                           |
| Impacto       |      `3` | Alto. A exclusão do histórico de um agendamento fere diretamente as exigências legais de retenção da LGPD, além de causar disputas operacionais e inviabilizar a rastreabilidade.              |
| Pontuação     |      `6` | Resultado de `2 × 3 = 6`.                                                                                                                                                                      |
| Classificação |    Médio | A pontuação está na faixa de 4 a 7.                                                                                                                                                            |

#### Estratégia de tratamento

| Campo         | Definição                                                                                                                                                                                                                                                          |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Estratégia    | Reduzir                                                                                                                                                                                                                                                            |
| Justificativa | A operação de cancelamento e manipulação de registros é necessária ao fluxo da clínica e não pode ser eliminada. O risco será reduzido garantindo que nenhuma deleção seja física e que toda alteração gere um rastro irrefutável e externo ao banco de dados.     |

#### Controles propostos e mapeamento NIST CSF 2.0

| ID | Função do NIST CSF 2.0 | Controle proposto | Responsável | Evidência esperada |
| -- | ---------------------- | ----------------- | ----------- | ------------------ |
| `C03.1` | Protect — Proteger | Implementar o padrão *Soft Delete* na camada de aplicação (uso das anotações `@SQLDelete` e `@Where` do Hibernate manipulando uma coluna `deleted_at`), impedindo o comando `DELETE` físico. | Equipe de desenvolvimento Backend | Código-fonte evidenciando as anotações nas entidades críticas. |
| `C03.2` | Detect — Detectar | Criação de *Triggers* de auditoria diretamente no PostgreSQL para espelhar qualquer evento de `INSERT`, `UPDATE` ou `DELETE` em uma tabela secundária de histórico irrefutável. | Equipe de Banco de Dados | *Screenshots* ou *queries* mostrando a persistência na tabela de log após uma ação destrutiva. |
| `C03.3` | Detect — Detectar | Configuração de envio de logs centralizados, imutáveis e apensos para ferramentas externas. | Equipe de Infraestrutura/DevOps | Painel da ferramenta externa demonstrando a ingestão dos logs de auditoria. |
| `C03.4` | Respond & Recover — Responder e Recuperar | Estabelecer procedimentos operacionais para análise de logs em caso de disputas e documentar queries para restauração rápida de registros que sofreram *Soft Delete* acidental ou malicioso. | Equipe de Operações e Segurança | Procedimento documentado de *undelete* testado com sucesso. |

#### Ordem de implementação

1. Alterar a modelagem e as entidades da aplicação para adoção massiva de *Soft Delete*.
2. Implementar e testar as *triggers* de auditoria nas tabelas de banco de dados (PostgreSQL).
3. Configurar a remessa dos logs gerados para o repositório centralizado externo (ELK/Datadog).
4. Homologar os procedimentos de investigação e restauração (*Recover*) com a equipe de operações.

#### Risco residual

| Critério | Valor estimado | Justificativa |
|---|---:|---|
| Probabilidade residual estimada | `1` | A probabilidade de um repúdio bem-sucedido cai drasticamente, pois o usuário não conseguirá mais apagar fisicamente o dado e a ação ficará espelhada externamente. |
| Impacto residual estimado | `3` | O impacto legal perante a LGPD em caso de incidentes se mantém alto, embora a capacidade de recuperação seja praticamente imediata. |
| Pontuação residual estimada | `3` | Resultado esperado de `1 × 3 = 3`. |
| Classificação residual estimada | Baixo | A pontuação estimada 3 está na faixa de 1 a 3. |
| Condição para aceitar o residual | Após implementação técnica | Aceitar apenas após a validação de que comandos `DELETE` físicos falham no banco ou são redirecionados, e os testes de logs externos estiverem operacionais. |
| Responsável pela aprovação | Arquitetura de Dados | Aprovação técnica mediante validação do fluxo do Hibernate e do PostgreSQL. |
| Revisão | Anual | Revisar sempre que houver refatoração nas entidades de agendamento ou mudança na stack de logs. |

##### Comparação entre risco inicial e residual esperado

| Risco | Nível inicial | Nível residual esperado | Condição |
|---|---|---|---|
| `R03` | Médio — pontuação `6` | Baixo — pontuação estimada `3` | Condicionado à implementação do *Soft Delete* e espelhamento externo imutável de logs. |