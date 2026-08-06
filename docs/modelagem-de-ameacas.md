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
| T01 | Spoofing | Token JWT de sessão | Um atacante explora uma vulnerabilidade XSS para executar um script no navegador, captura um JWT armazenado indevidamente no `localStorage` e reutiliza o token para se passar por um paciente, profissional ou administrador. | Acesso indevido às contas de médicos e pacientes, exposição de dados pessoais e realização de agendamentos ou cancelamentos fraudulentos. |

### 5.1 Detalhamento da ameaça T01 — Falsificação ou roubo de token

No cenário analisado, o front-end descumpre a especificação segura e armazena o JWT no `localStorage`. Como esse armazenamento pode ser lido por JavaScript executado na mesma origem, uma falha de XSS em um campo de texto permite que um script malicioso capture o token e o envie ao atacante. Outros cenários de roubo incluem exposição por conexão sem TLS, logs, URLs, extensões maliciosas ou dispositivo comprometido. A falsificação também pode ocorrer se a chave de assinatura vazar, se um segredo simétrico for fraco ou se o backend aceitar algoritmos não previstos, tokens sem assinatura, emissor ou público incorretos ou campos de validade sem verificação.

Depois de obter ou forjar o token, o atacante o envia como `Bearer Token`. Se o backend considerar o token válido, as ações serão associadas à identidade da vítima. A assinatura protege a integridade do JWT, mas não impede o uso de um token válido que tenha sido roubado; por isso, validade curta, proteção no armazenamento, rotação e revogação da renovação são necessárias.

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

## 7. Considerações finais

### 7.1 Ameaças mais preocupantes

### 7.2 Ativos mais importantes

### 7.3 Abusos de maior impacto

### 7.4 Dificuldades encontradas
