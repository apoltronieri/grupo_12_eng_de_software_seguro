# Engenharia de Software Seguro — Grupo 12

Trabalho final da disciplina de Engenharia de Software Seguro.

## Informações do projeto

- **Sistema escolhido:** Sistema de Agendamento de Consultas
- **Integrantes:** Aira Arima, Ana Carolina Poltronieri, Graziela Bitencourt, Luan Martins, Lucie Grillo, Mariana Ferrao
- **Repositório:** [`grupo_12_eng_de_software_seguro`](https://github.com/apoltronieri/grupo_12_eng_de_software_seguro)

## Sobre o sistema

O **Sistema de Agendamento de Consultas** centraliza a oferta de horários por profissionais de saúde e a procura por consultas pelos pacientes. A plataforma permite localizar profissionais e horários disponíveis, registrar e acompanhar agendamentos e administrar agendas, reduzindo a necessidade de coordenação manual e o risco de informações inconsistentes ou reservas conflitantes.

Os perfis de usuário contemplados são: **Visitante**, **Paciente**, **Profissional** e **Administrador**, com controle de acesso baseado em papéis (RBAC) gerenciado pelo backend.

## Etapas do trabalho

A análise de segurança foi desenvolvida em etapas progressivas, cada uma documentada na pasta correspondente.

| Etapa | Título | Documento |
|---|---|---|
| 1 | Modelagem de Ameaças com STRIDE | [`docs/etapa1-modelagem-de-ameacas.md`](docs/etapa1-modelagem-de-ameacas.md) |
| 2 | Análise, Priorização e Tratamento de Riscos | [`docs/etapa2-tratamento-de-riscos.md`](docs/etapa2-tratamento-de-riscos.md) |
| 3 | Projeto de uma Arquitetura Segura | [`docs/etapa3-arquitetura-segura.md`](docs/etapa3-arquitetura-segura.md) |
| 4 | Código Seguro e Testes de Segurança | [`docs/etapa4-codigo-seguro.md`](docs/etapa4-codigo-seguro.md) |
| 5 | Verificação de Vulnerabilidades | [`docs/etapa5-verificacao-vulnerabilidades.md`](docs/etapa5-verificacao-vulnerabilidades.md) |
| 6 | Detecção de Intrusões | [`roteiros/etapa-6-deteccao-de-intrusoes.md`](roteiros/etapa-6-deteccao-de-intrusoes.md) |
| 7 | DevSecOps: Monitoramento e Resposta no Pipeline | [`roteiros/etapa-7-devsecops.md`](roteiros/etapa-7-devsecops.md) |

### Resumo por etapa

**Etapa 1 — Modelagem de Ameaças com STRIDE**
Identificação do sistema, descrição dos usuários e ativos, definição da interface conceitual da API e modelagem de seis ameaças (T01–T06) cobrindo todas as categorias STRIDE. Cada ameaça é acompanhada de um caso de abuso detalhado (CA01–CA06).

**Etapa 2 — Análise, Priorização e Tratamento de Riscos**
Definição dos critérios de probabilidade e impacto, cálculo da pontuação de risco e registro formal de cada risco identificado no registro de riscos.

**Etapa 3 — Projeto de uma Arquitetura Segura**
Decisões arquiteturais e requisitos de segurança derivados dos riscos priorizados, com mapeamento de vulnerabilidades catalogadas (OWASP API Security Top 10).

**Etapa 4 — Código Seguro e Testes de Segurança**
Práticas seguras de implementação descritas em pseudocódigo (PS01 e PS02), com testes de segurança definidos antes da solução (abordagem TDD de segurança).

**Etapa 5 — Verificação de Vulnerabilidades**
Execução de scan automatizado com OWASP ZAP contra o OWASP Juice Shop, análise de três achados priorizados (CSP ausente, HSTS ausente e CORS permissivo), mapeamento para CWE e riscos do projeto, e tratamento de limitações e falsos positivos. Evidências armazenadas em `evidencias/etapa5/`.

**Etapa 6 — Detecção de Intrusões**
Definição dos eventos de segurança a registrar (EV-01 a EV-08), regras de detecção baseadas nos riscos prioritários (RD06, RD06-B) e procedimento de tratamento inicial após geração de alerta.

**Etapa 7 — DevSecOps: Monitoramento e Resposta no Pipeline**
Integração de segurança em cada etapa do pipeline de entrega (SAST, SCA, testes de autorização, smoke tests, DAST), com gatilhos de resposta a incidentes e responsabilidades por papel.

## Registro de riscos

| ID | Origem STRIDE | Probabilidade | Impacto | Pontuação | Nível |
|---|---|:---:|:---:|:---:|---|
| [R01](docs/registro-de-riscos/R01.md) | T01 — Spoofing / CA01 | 3 | 4 | 12 | Crítico |
| [R02](docs/registro-de-riscos/R02.md) | T02 — Tampering / CA02 | 3 | 4 | 12 | Crítico |
| [R03](docs/registro-de-riscos/R03.md) | T03 — Repudiation / CA03 | 2 | 3 | 6 | Médio |
| [R04](docs/registro-de-riscos/R04.md) | T04 — Information Disclosure / CA04 | 3 | 4 | 12 | Crítico |
| [R05](docs/registro-de-riscos/R05.md) | T05 — Denial of Service / CA05 | 4 | 3 | 12 | Crítico |
| [R06](docs/registro-de-riscos/R06.md) | T06 — Elevation of Privilege / CA06 | 3 | 4 | 12 | Crítico |

## Organização do repositório

```text
.
├── README.md
├── docs/
│   ├── etapa1-modelagem-de-ameacas.md
│   ├── etapa2-tratamento-de-riscos.md
│   ├── etapa3-arquitetura-segura.md
│   ├── etapa4-codigo-seguro.md
│   ├── etapa5-verificacao-vulnerabilidades.md
│   └── registro-de-riscos/
│       ├── R01.md
│       ├── R02.md
│       ├── R03.md
│       ├── R04.md
│       ├── R05.md
│       └── R06.md
├── roteiros/
│   ├── etapa-6-deteccao-de-intrusoes.md
│   └── etapa-7-devsecops.md
├── evidencias/
│   └── etapa5/
│       ├── evidencia A01.png
│       ├── evidencia A02.png
│       ├── evidencia A03.png
│       ├── evidencia captura geral.png
│       └── relatorio-zap.md
├── diagramas/
│   ├── diagrama_de_contexto.png
│   └── diagrama_arquitetura_segura.png
└── imagens/
    └── .gitkeep
```

- `docs/`: documentos de cada etapa entregável e registro de riscos.
- `roteiros/`: documentos de etapas orientadas a roteiro (detecção de intrusões e DevSecOps).
- `evidencias/`: capturas de tela e relatórios gerados pelas ferramentas de verificação.
- `diagramas/`: diagramas e seus arquivos-fonte.
- `imagens/`: imagens complementares utilizadas na documentação.
