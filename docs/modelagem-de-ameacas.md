# Modelagem de Ameaças com STRIDE

> Documento em construção.

## 1. Identificação do sistema

### 1.1 Nome do sistema

### 1.2 Integrantes do grupo

### 1.3 Endereço do repositório

- **Nome:** `grupo_12_eng_de_software_seguro`
- **Endereço:** A definir

### 1.4 Justificativa da escolha

## 2. Descrição do sistema

### 2.1 Problema que o sistema resolve

### 2.2 Usuários do sistema

### 2.3 Principais funcionalidades

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

## 5. Modelagem de ameaças com STRIDE

| ID | Categoria STRIDE | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---|---|---|---|
| `T01` | Tampering | API de agendamentos, registros de consultas e agendas | Um paciente autenticado modifica identificadores ou campos enviados em uma operação de criação, remarcação ou cancelamento. Caso a API aceite campos indevidos ou não valide a titularidade, a disponibilidade e a integridade da operação, dados de um agendamento poderão ser alterados de forma não autorizada. | Alteração ou cancelamento indevido de consultas, conflitos nas agendas, perda de integridade dos registros e prejuízo ao atendimento. |

## 6. Casos de abuso

### CA01 — Título do caso de abuso

- **Ator:**
- **Objetivo:**
- **Condições necessárias:**
- **Fluxo de abuso:**
- **Impacto esperado:**
- **Categorias STRIDE relacionadas:**

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

## 8. Etapa 2 — Avaliação e tratamento de riscos

### R02 — Adulteração de dados de agendamentos

#### Identificação do risco

| Campo                     | Descrição                                                                                                                                                                                                                                                   |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ID do risco               | `R02`                                                                                                                                                                                                                                                       |
| Ameaça de origem          | `T01 — Tampering`                                                                                                                                                                                                                                           |
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
