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
| `GET` | `/profissionais/me/agenda` | Listar a própria agenda | Profissional | Nenhum / filtros opcionais | Identidade via contexto de autenticação. |
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
|  |  |  |  |  |

## 6. Casos de abuso

### CA01 — Título do caso de abuso

- **Ator:**
- **Objetivo:**
- **Condições necessárias:**
- **Fluxo de abuso:**
- **Impacto esperado:**
- **Categorias STRIDE relacionadas:**

## 7. Considerações finais

### 7.1 Ameaças mais preocupantes

### 7.2 Ativos mais importantes

### 7.3 Abusos de maior impacto

### 7.4 Dificuldades encontradas
