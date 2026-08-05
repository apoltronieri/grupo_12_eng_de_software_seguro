# Proposta de definição do sistema

## 1. Status da proposta

A escolha de uma **plataforma de agendamento de consultas** já foi realizada pelo grupo.

Este documento apresenta uma proposta inicial para delimitar o funcionamento geral do sistema e permitir que as atividades individuais sejam desenvolvidas de forma coerente. As definições ainda poderão ser revisadas antes de serem incorporadas ao documento principal do trabalho.

---

## 2. Visão geral e escopo

A plataforma terá como objetivo facilitar o agendamento de consultas entre pacientes e profissionais de saúde. O sistema permitirá que pacientes consultem profissionais e horários disponíveis, realizem agendamentos e acompanhem suas próprias consultas.

Os profissionais poderão disponibilizar horários e consultar a própria agenda. A administração será responsável pela manutenção geral da plataforma, incluindo o gerenciamento de contas, profissionais e especialidades.

O escopo inicial incluirá:

* cadastro e autenticação de usuários;
* consulta de profissionais e especialidades;
* consulta de horários disponíveis;
* criação de agendamentos;
* visualização das próprias consultas;
* remarcação e cancelamento;
* gestão da disponibilidade dos profissionais;
* manutenção administrativa da plataforma.

Para limitar a complexidade da análise, ficarão inicialmente fora do escopo:

* prontuários médicos completos;
* diagnósticos e prescrições;
* consultas por vídeo;
* pagamentos;
* integração com planos de saúde;
* emissão de documentos médicos;
* comunicação clínica entre paciente e profissional.

Essas funcionalidades poderão ser consideradas em etapas futuras, mas não serão utilizadas como base para a análise atual.

---

## 3. Perfis de usuário

### Paciente

Usuário que busca atendimento e utiliza a plataforma para consultar profissionais e horários, criar agendamentos e gerenciar as próprias consultas.

### Profissional de saúde

Usuário que oferece atendimento, configura sua disponibilidade e consulta os agendamentos associados à própria agenda.

### Administrador

Usuário responsável pela manutenção administrativa da plataforma, como gerenciamento de contas, profissionais e especialidades.

Os limites detalhados de acesso, as operações restritas e a matriz de permissões serão definidos e aprofundados pela pessoa responsável pela análise de perfis e controle de acesso.

---

## 4. Funcionalidades e regras gerais

As funcionalidades principais do sistema serão:

| Funcionalidade            | Perfis envolvidos       | Descrição                                          |
| ------------------------- | ----------------------- | -------------------------------------------------- |
| Cadastro e autenticação   | Todos                   | Permitir a criação de contas e o acesso ao sistema |
| Consulta de profissionais | Paciente                | Localizar profissionais e especialidades           |
| Consulta de horários      | Paciente                | Visualizar horários disponíveis                    |
| Criação de agendamento    | Paciente                | Reservar um horário disponível                     |
| Consulta de agendamentos  | Paciente e profissional | Visualizar consultas relacionadas ao usuário       |
| Remarcação                | Paciente                | Alterar a data ou o horário de uma consulta        |
| Cancelamento              | Paciente                | Cancelar uma consulta sem excluir seu registro     |
| Gestão de disponibilidade | Profissional            | Configurar dias e horários de atendimento          |
| Gestão administrativa     | Administrador           | Manter contas, profissionais e especialidades      |

Como regras gerais:

1. Apenas horários disponíveis poderão ser agendados.
2. Um paciente ou profissional não poderá possuir consultas conflitantes no mesmo horário.
3. Cada usuário somente poderá executar operações compatíveis com seu perfil.
4. Remarcações deverão validar novamente a disponibilidade do horário.
5. O cancelamento deverá preservar o registro da consulta.
6. As regras de negócio deverão ser verificadas no servidor, independentemente das validações realizadas na interface do usuário.

Os mecanismos específicos de autenticação, autorização, concorrência e persistência serão definidos pelas pessoas responsáveis por essas áreas.

---

## 5. Dados e ativos principais

O sistema poderá armazenar ou transmitir as seguintes categorias de informações:

* dados de identificação e contato;
* credenciais de acesso;
* dados profissionais e especialidades;
* disponibilidade dos profissionais;
* informações dos agendamentos;
* vínculo entre paciente e profissional;
* estado das consultas;
* informações administrativas;
* dados de autenticação ou sessão, conforme o mecanismo posteriormente adotado.

Os principais ativos que precisam ser protegidos são:

* contas dos usuários;
* credenciais;
* dados pessoais;
* agenda dos profissionais;
* registros de consultas;
* horários disponíveis;
* API;
* banco de dados;
* configurações da aplicação;
* disponibilidade do serviço.

A arquitetura deverá preservar especialmente:

* **confidencialidade**, evitando o acesso indevido a informações;
* **integridade**, impedindo alterações não autorizadas;
* **disponibilidade**, mantendo o sistema acessível;
* **autenticidade**, permitindo identificar corretamente os usuários;
* **rastreabilidade**, possibilitando identificar ações relevantes.

A classificação detalhada dos dados, as decisões sobre armazenamento e as restrições de persistência serão aprofundadas pela pessoa responsável pela modelagem de dados.

---

## 6. Arquitetura conceitual

A arquitetura conceitual inicial será composta por:

* interface web utilizada pelos usuários;
* API responsável por receber as requisições;
* camada de regras de negócio;
* mecanismo de autenticação e autorização;
* camada de persistência;
* banco de dados;
* possíveis serviços externos, como notificações.

Fluxo geral:

```text
Usuário → Interface web → API → Regras de negócio → Persistência
```

A interface poderá realizar validações preliminares para melhorar a experiência do usuário. Entretanto, as validações de segurança, as permissões e as regras de negócio deverão ser verificadas no servidor.

Essa representação serve apenas como base comum. O diagrama, os limites de confiança, a comunicação detalhada entre componentes e os possíveis gargalos serão desenvolvidos pelas pessoas responsáveis pelas áreas de arquitetura e infraestrutura.

---

## 7. Decisões pendentes

As seguintes decisões ainda precisam ser confirmadas pelo grupo:

* nome definitivo do sistema;
* atendimento a uma única clínica ou a múltiplas organizações;
* necessidade de confirmação da consulta pelo profissional;
* utilização de notificações por e-mail ou outro canal;
* política de antecedência para remarcações e cancelamentos;
* necessidade de outros perfis de usuário;
* stack tecnológica utilizada pelo projeto;
* mecanismo de autenticação e manutenção da sessão;
* banco de dados;
* ferramentas de documentação e teste da API.

As tecnologias deverão ser padronizadas pelo grupo antes de serem utilizadas como decisões oficiais nas entregas individuais.

---
