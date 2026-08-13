# Modelagem de Ameaças com STRIDE

## 1. Identificação do sistema

### 1.1 Nome do sistema

Sistema de Agendamento de Consultas.

### 1.2 Integrantes do grupo

Aira Arima, Ana Carolina Poltronieri, Graziela Bitencourt, Luan Martins, Lucie Grillo e Mariana Ferrao.

### 1.3 Endereço do repositório

- **Nome:** `grupo_12_eng_de_software_seguro`
- **Endereço:** https://github.com/apoltronieri/grupo_12_eng_de_software_seguro

### 1.4 Justificativa da escolha

O sistema de agendamento de consultas foi escolhido porque reúne diferentes perfis de usuário, dados pessoais, credenciais, agendas e operações relevantes, como criação, remarcação e cancelamento de consultas. Além disso, envolve concorrência pelo mesmo horário, funções administrativas e comunicação entre interface, API e banco de dados. Essas características permitem analisar ameaças concretas relacionadas à confidencialidade, integridade, disponibilidade, autenticidade, autorização e rastreabilidade.

## 2. Descrição do sistema

### 2.1 Problema que o sistema resolve

O sistema centraliza a oferta de horários por profissionais de saúde e a procura por consultas pelos pacientes. A plataforma permite localizar profissionais e horários disponíveis, registrar e acompanhar agendamentos e administrar as agendas, reduzindo a necessidade de coordenação manual e o risco de informações inconsistentes ou reservas conflitantes.

### 2.2 Usuários do sistema

O sistema é utilizado por visitantes ainda não autenticados, pacientes, profissionais de saúde e administradores. Visitantes podem consultar informações públicas, cadastrar-se e autenticar-se. Pacientes gerenciam as próprias consultas; profissionais configuram sua disponibilidade e consultam sua agenda; administradores realizam a manutenção geral da plataforma.

### 2.3 Principais funcionalidades

As principais funcionalidades consideradas na análise são:

- cadastro e autenticação de usuários;
- consulta de profissionais, especialidades e horários disponíveis;
- criação, visualização, remarcação e cancelamento de agendamentos;
- gestão da disponibilidade e da agenda dos profissionais;
- manutenção administrativa de usuários, profissionais e especialidades.

Prontuários médicos completos, diagnósticos, prescrições, consultas por vídeo, pagamentos, integração com planos de saúde, emissão de documentos médicos e comunicação clínica não fazem parte do escopo analisado.

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

O sistema armazena ou transmite dados de identificação e contato, credenciais de acesso, dados de autenticação e sessão, informações profissionais e especialidades, horários disponíveis, dados e estados dos agendamentos, vínculos entre pacientes e profissionais, informações administrativas e registros de auditoria. As comunicações entre cliente e API deverão ocorrer por HTTPS.

### 2.5 Recursos que precisam ser protegidos

Os principais recursos protegidos são as contas e credenciais dos usuários, os dados pessoais, os tokens e segredos da aplicação, as agendas e disponibilidades dos profissionais, os registros e históricos de consultas, os logs de auditoria, a API, o banco de dados, as configurações da aplicação e a disponibilidade do serviço.

## 3. Usuários, ativos e pontos de interação

### 3.1 Usuários e perfis de acesso

O sistema utiliza Controle de Acesso Baseado em Papéis (RBAC - *Role-Based Access Control*) para restringir as funcionalidades de acordo com o perfil do usuário. Foram identificados os seguintes perfis e suas respectivas permissões:

- **Visitante (Público):** Usuário não autenticado. Permissões limitadas a visualizar informações públicas (profissionais, especialidades), cadastrar-se como paciente e realizar autenticação no sistema.
- **Paciente:** Usuário autenticado. Possui permissões para consultar disponibilidade de horários, criar, visualizar, remarcar e cancelar apenas seus próprios agendamentos (autorização em nível de recurso baseada em sua identidade).
- **Profissional (Médico/Atendente de Saúde):** Usuário autenticado responsável por gerenciar seus horários. Possui permissões para cadastrar, atualizar suas disponibilidades e visualizar sua própria agenda de consultas.
- **Administrador:** Usuário autenticado com privilégios elevados. Possui permissões globais sobre os recursos do sistema, sendo o único capaz de executar **operações restritas e administrativas**, como:
  - **Manutenção de Usuários/Pacientes:** bloqueio de contas e intervenções de suporte.
  - **Manutenção de Profissionais:** cadastro de novos profissionais e vinculação a especialidades.
  - **Manutenção de Especialidades:** criação, edição e exclusão de áreas de atuação clínica.
  - **Gestão Global de Agendamentos:** cancelamento ou modificação de agendamentos de qualquer paciente ou profissional (para fins de contingência ou moderação).

### 3.2 Ativos importantes

A camada de persistência deste sistema, baseada em um banco de dados relacional, é responsável por armazenar os ativos mais críticos da aplicação. O controle estrutural do banco é versionado através de ferramentas de *Migrations*. Para garantir a segurança e a adequação à LGPD, destacam-se os seguintes ativos e diretrizes de proteção:

- **Credenciais de Acesso:** Senhas não são armazenadas em texto plano. Utiliza-se criptografia unidirecional com *hashing* de alto custo ou extensões nativas.
- **Dados Pessoais e Histórico de Consultas (LGPD):** Dados de identificação, informações de contato e o histórico de agendamentos dos pacientes exigem proteção reforçada em repouso, utilizando técnicas como *Transparent Data Encryption (TDE)* para o armazenamento físico.
- **Logs de Auditoria (Audit Trails):** Trilha de auditoria essencial para registrar operações críticas, garantindo a rastreabilidade das ações e prevenindo o repúdio.
- **Registros de Agendamento (Integridade Referencial):** Conforme regra de negócio, os registros de consulta não sofrem deleção física, devendo adotar a abordagem de *Soft Delete* para manter o histórico intacto em caso de cancelamentos.

### 3.3 Pontos de interação

Os principais pontos de interação e entrada de dados são:

- a interface web, utilizada por visitantes, pacientes, profissionais e administradores;
- a API, que recebe requisições HTTPS e aplica autenticação, autorização, validações e regras de negócio;
- o mecanismo de autenticação e gerenciamento de sessões, responsável pelas credenciais e tokens;
- a camada de persistência e o banco de dados, que armazenam usuários, disponibilidades, agendamentos e logs;
- as rotas públicas, autenticadas e administrativas apresentadas na interface conceitual da API.

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

![Diagrama de Contexto](/diagramas/diagrama_de_contexto.png)

O fluxo de processamento do sistema de agendamento de consultas ocorre através da comunicação entre três componentes principais:
1. **Cliente (Interface Web):** Onde os usuários (pacientes, profissionais e administradores) interagem com o sistema, enviando requisições HTTPS.
2. **API (Servidor de Aplicação):** Recebe as requisições, valida a autenticação, verifica as regras de negócio e processa a lógica do agendamento ou consulta.
3. **Banco de Dados:** Armazena as informações persistentes, como dados de usuários, agendas e disponibilidade.

**Fluxo básico de agendamento:**
O Cliente envia uma requisição para listar horários disponíveis -> A API consulta o Banco de Dados, aplica as regras de negócio e retorna os horários -> O Cliente escolhe um horário e envia a requisição de reserva -> A API valida a disponibilidade e registra a consulta no Banco de Dados -> O Cliente recebe a confirmação.

**Gargalos e recursos suscetíveis à exaustão:**
- **Conexões com o Banco de Dados:** O limite de conexões simultâneas pode ser atingido rapidamente se a API não realizar um bom gerenciamento (pool de conexões) ou se consultas complexas demorarem a concluir.
- **Processamento e Memória na API:** Requisições de busca muito amplas (ex: listar todos os profissionais sem paginação) ou falhas na validação de entrada podem forçar a API a alocar muita memória ou gastar ciclos de CPU desnecessariamente.
- **Banda de Rede e Conexões HTTP:** Muitos acessos simultâneos ou ataques volumétricos podem esgotar a capacidade do servidor web de aceitar novas requisições.


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
| `T04` | Information Disclosure | API de busca de profissionais | Ao realizar a busca por determinado profissional ou especiliadade, um usuário autenticado clica em `Inspecionar` e depois na aba `Network` do navegador, onde consegue visualizar dados pessoais e sensíveis do profissional no response da API.  | Acesso a dados pessoais como endereço, CPF, número de telefone, trazendo riscos como o uso indevido dos dados e tentativas de golpes ao profissional. |
| `T05` | Denial of Service | API e Banco de Dados | Um atacante envia um número massivo de requisições complexas de busca de horários, exaurindo as conexões com o banco e o processamento da API. | O sistema fica indisponível para os usuários legítimos, impedindo novos agendamentos e a consulta de agendas. |
| `T06` | Elevation of Privilege | Controle de Acesso (API e Backend) | Um usuário autenticado com baixo privilégio (ex: Paciente) envia requisições para endpoints administrativos ou explora falhas no modelo de autorização (*Broken Access Control* ou *Mass Assignment*) para realizar ações restritas (ex: gerenciar profissionais). | Acesso não autorizado a funções críticas, comprometimento total da integridade do sistema e capacidade de executar ações destrutivas (ex: exclusão em massa). |


### 5.1 Detalhamento da ameaça T01 — Falsificação ou roubo de token

No cenário analisado, o front-end descumpre a especificação segura e armazena o JWT no `localStorage`. Como esse armazenamento pode ser lido por JavaScript executado na mesma origem, uma falha de XSS em um campo de texto permite que um script malicioso capture o token e o envie ao atacante. Outros cenários de roubo incluem exposição por conexão sem TLS, logs, URLs, extensões maliciosas ou dispositivo comprometido. A falsificação também pode ocorrer se a chave de assinatura vazar, se um segredo simétrico for fraco ou se o backend aceitar algoritmos não previstos, tokens sem assinatura, emissor ou público incorretos ou campos de validade sem verificação.

Depois de obter ou forjar o token, o atacante o envia como `Bearer Token`. Se o backend considerar o token válido, as ações serão associadas à identidade da vítima. A assinatura protege a integridade do JWT, mas não impede o uso de um token válido que tenha sido roubado; por isso, validade curta, proteção no armazenamento, rotação e revogação da renovação são necessárias.

### 5.2 Detalhamento da ameaça T03 — Exclusão sem rastreabilidade (Repudiation)

No cenário analisado, as tabelas do banco de dados descumprem a regra de negócio e não utilizam *Soft Delete*, permitindo a exclusão física e permanente de registros de agendamento. Além disso, a aplicação não gera trilhas de auditoria para operações de escrita ou deleção. 

Dessa forma, um usuário interno mal-intencionado pode acessar a funcionalidade de deleção e excluir um agendamento. Quando o paciente reclamar do sumiço da consulta, a administração não conseguirá rastrear os logs de aplicação para vincular a exclusão a um usuário específico. O autor da ação pode negar que cancelou a consulta, gerando disputas legais e quebra de confiança no sistema.

### 5.3 Detalhamento da ameaça T04 — Acesso a dados pessoais do profissional (Information Disclosure)

No cenário analisado, a funcionalidade de busca de profissionais/especialidades pode retornar mais dados do que o usuário necessita para concluir a operação de agendamento. Um usuário mal intencionado pode facilmente acessar as ferramentas de desenvolvedor do navegador e visualizar o corpo de resposta retornado pela API de busca. Sem filtragem de campos e sem controle de autorização adequado, é possível visualizar informações sensíveis como o nome completo, CPF, endereço, telefone, e-mail e dados de vínculo profissional.

O impacto vai além da simples exposição de dados: informações pessoais podem ser usadas para golpes, contatos indevidos ou coleta de dados para fins maliciosos. Em cenários mais críticos, o atacante pode montar um perfil do profissional e usar essas informações para agir de forma fraudulenta.

### 5.4 Detalhamento da ameaça T06 — Escalada indevida de privilégios (Elevation of Privilege)

No cenário analisado, o sistema pode falhar em aplicar verificações rigorosas de autorização nos endpoints administrativos. Embora o backend exija um token JWT válido para identificar o usuário, ele pode omitir a checagem de regras de controle de acesso (RBAC), confiando equivocadamente que usuários comuns não descobrirão as rotas restritas apenas porque os botões correspondentes estão ocultos no front-end.

Outra falha estrutural comum associada à escalada de privilégios é o *Mass Assignment* (Atribuição em Massa), em que a API permite a injeção do campo `perfil` ou `role` no payload durante o cadastro ou atualização do usuário. Sem a devida sanitização de parâmetros no servidor, um usuário comum pode promover sua própria conta a "Administrador", adquirindo permissões elevadas para acessar todos os dados sensíveis, adulterar cadastros de médicos e manipular o fluxo de agendamentos globalmente.

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

### CA04 — Acesso a dados pessoais do profissional

- **Ator:** Usuário autenticado com intenção maliciosa.
- **Objetivo:** Visualizar informações pessoais de um profissional da saúde para realizar tentativas de golpes e contato indevido.
- **Condições necessárias:**
    - O ator possui uma conta válida.
    - O usuário realiza a busca por determinado profissional ou especialidade.
    - A API não filtra os dados do profissional e retorna todas as suas informações registradas no sistema no corpo de reposta da requisição.
- **Fluxo de abuso:**
  1. O usuário autentica-se normalmente na plataforma.
  2. Acessa a tela de agendamento de consultas e realiza a busca por um profissional ou especialidade.
  3. O atacante acessa as ferramentas de desenvolvedor do navegador.
  4. Acessa a aba `Network`, clica na chamada realizada para a API e, depois, em `Response`.
  5. Mesmo que o front-end não apresente dados sensíveis na interface, a API não filtrou quais informações devem ser retornadas para a requisição, portanto, o usuário passa a conseguir visualizar dados pessoais dos profissionais.
- **Impacto esperado:** Violação de privacidade, identidade comprometida e exposta de profissionais, tentativas de golpe e contatos indevidos com o profissional.
- **Categorias STRIDE relacionadas:** Information Disclosure.


### CA05 — Indisponibilidade por exaustão de recursos

- **Ator:** Usuário mal-intencionado ou rede de bots (botnet).
- **Objetivo:** Tornar a plataforma de agendamentos indisponível para usuários legítimos, causando prejuízos ao negócio e aos profissionais.
- **Condições necessárias:** A API do sistema não possui limitação de taxa (rate limiting) adequada e expõe endpoints de busca (ex: listagem de horários ou profissionais) que consomem muito processamento ou conexões com o banco de dados.
- **Fluxo de abuso:**
  1. O atacante identifica um endpoint custoso na API (por exemplo, a busca de disponibilidade sem filtros adequados de data).
  2. Utilizando ferramentas automatizadas, o atacante dispara milhares de requisições simultâneas para este endpoint.
  3. A API tenta processar todas as requisições, esgotando o pool de conexões com o banco de dados e/ou a capacidade de CPU/Memória do servidor.
  4. O sistema não consegue mais responder a novas requisições.
  5. Usuários legítimos recebem erros de tempo limite (timeout) ou falha no servidor (HTTP 500/503).
- **Impacto esperado:** Indisponibilidade total ou parcial do sistema de agendamento, causando insatisfação dos usuários, perda de consultas, danos à reputação e potenciais prejuízos financeiros.
- **Categorias STRIDE relacionadas:** Denial of Service.

### CA06 — Escalada indevida de privilégios

- **Ator:** Usuário autenticado com perfil de baixo privilégio (ex: Paciente).
- **Objetivo:** Obter acesso administrativo para realizar operações restritas, como cadastrar ou excluir profissionais, ou acessar os dados globais do sistema.
- **Condições necessárias:**
  - O ator possui uma conta válida (ex: Paciente).
  - A API possui endpoints administrativos (ex: `POST /profissionais`) e falha em validar se o papel (*role*) contido no JWT possui autorização para aquela rota.
  - Alternativamente, a API de atualização de cadastro aceita e persiste campos críticos (como `"perfil": "ADMIN"`) fornecidos pelo cliente (*Mass Assignment*).
- **Fluxo de abuso:**
  1. O usuário (Paciente) autentica-se na plataforma, recebe seu JWT e analisa as requisições normais feitas pelo front-end.
  2. O atacante descobre ou deduz a existência de rotas administrativas da API.
  3. Utilizando uma ferramenta como o Postman ou o cURL, o atacante constrói uma requisição para o endpoint restrito (ex: criação de um médico fantasma) e anexa seu JWT legítimo de Paciente no cabeçalho `Authorization`.
  4. O backend recebe a requisição, valida a assinatura e a expiração do JWT (confirmando o login), mas omite a validação de autorização de perfis para aquela rota específica.
  5. A requisição é processada com sucesso, executando a ação administrativa não autorizada.
- **Impacto esperado:** Comprometimento da segurança e integridade de todo o sistema. Acesso irrestrito a dados confidenciais e capacidade de realizar sabotagem (exclusão de registros críticos).
- **Categorias STRIDE relacionadas:** Elevation of Privilege, com consequências secundárias de Information Disclosure e Tampering.



## 7. Considerações finais

### 7.1 Ameaças mais preocupantes

### 7.2 Ativos mais importantes

### 7.3 Abusos de maior impacto

### 7.4 Dificuldades encontradas
