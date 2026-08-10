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

## 4. Visão geral da arquitetura ou fluxo

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

## 5. Modelagem de ameaças com STRIDE

| ID | Categoria STRIDE | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---|---|---|---|
| T05 | Denial of Service | API e Banco de Dados | Um atacante envia um número massivo de requisições complexas de busca de horários, exaurindo as conexões com o banco e o processamento da API. | O sistema fica indisponível para os usuários legítimos, impedindo novos agendamentos e a consulta de agendas. |

## 6. Casos de abuso

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

## 7. Considerações finais

### 7.1 Ameaças mais preocupantes

### 7.2 Ativos mais importantes

### 7.3 Abusos de maior impacto

### 7.4 Dificuldades encontradas
