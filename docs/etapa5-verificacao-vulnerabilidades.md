# Etapa 5 — Verificação de Vulnerabilidades

## 1. Ambiente e Ferramenta de Teste

Para a realização desta etapa, optou-se pela verificação de uma aplicação deliberadamente vulnerável executada para fins educacionais, conforme permitido pelas orientações do trabalho, visto que o sistema de agendamento de consultas projetado pelo grupo ainda não possui implementação completa em ambiente de produção.

- **Sistema testado:** OWASP Juice Shop.
- **Ferramenta utilizada:** OWASP ZAP.
- **Evidências:** As capturas de tela demonstrando a execução do scan e a interface dos relatórios gerados foram armazenadas no diretório `evidencias/etapa-5/`.

## 2. Configuração Básica do Teste

O teste foi realizado utilizando a funcionalidade **Automated Scan** do OWASP ZAP. A configuração envolveu:
1.  **Spidering tradicional:** para mapeamento inicial de rotas e descoberta da superfície de ataque (endpoints da API e páginas front-end).
2.  **Active Scan:** com política de ataque padrão, injetando *payloads* comuns nos campos de formulários e parâmetros de URL descobertos.
3.  O teste foi executado sem autenticação prévia para simular a visão de um atacante externo não autenticado interagindo com a plataforma.

## 3. Análise de Alertas 

A ferramenta identificou diversas vulnerabilidades relacionadas a cabeçalhos de segurança ausentes e configurações sistêmicas permissivas. Foram selecionados três achados relevantes com base no relatório gerado pelo OWASP ZAP.

| ID | Alerta ou achado | Evidência | Possível impacto | Relação com OWASP/CWE e Riscos do Projeto | Correção proposta |
|---|---|---|---|---|---|
| `A01` | **Content Security Policy (CSP) Header Not Set** | Detalhamento na imagem `evidencia A01.png` e listagem principal em `evidencia captura geral.png` evidenciando a ausência do cabeçalho. | A ausência do CSP permite que o navegador carregue e execute scripts maliciosos de origens não aprovadas, facilitando ataques de injeção e roubo de dados (*Cross-Site Scripting*). | **CWE-693** (Protection Mechanism Failure). Relaciona-se com o risco **R01**, pois facilita o roubo de tokens JWT via script malicioso. | Configurar o servidor web e a aplicação para enviar o cabeçalho `Content-Security-Policy`, declarando uma lista estrita de origens de conteúdo aprovadas pelo sistema. |
| `A02` | **Strict-Transport-Security Header Not Set** | Registro da vulnerabilidade HSTS evidenciado no painel do ZAP conforme a imagem `evidencia A02.png`. | Um atacante na rede pode forçar a vítima a estabelecer conexões HTTP inseguras (Downgrade Attack), permitindo interceptar o tráfego em texto claro e capturar credenciais. | **CWE-319** (Cleartext Transmission of Sensitive Information). Relaciona-se aos riscos **R01** e **R04**. | Garantir que o servidor web, *load balancer* ou API esteja configurado para incluir o cabeçalho `Strict-Transport-Security` forçando conexões exclusivas via HTTPS. |
| `A03` | **Configuração Incorreta Entre Domínios (Cross Domain Misconfiguration)** | Painel do ZAP exibindo o uso permissivo de `Access-Control-Allow-Origin: *` capturado na `evidencia A03.png` e na resposta HTTP da `evidencia captura geral.png`. | O uso de curinga (`*`) no CORS permite que solicitações de leitura sejam feitas a partir de domínios arbitrários de terceiros, expondo dados sensíveis caso a segurança baseie-se em IP ou acessos não autenticados. | **CWE-264** (Permissions, Privileges, and Access Controls). Relaciona-se com o risco **R04**. | Substituir o curinga `*` no cabeçalho `Access-Control-Allow-Origin` por um conjunto restritivo de domínios permitidos, ou remover o cabeçalho para aplicar o SOP (*Same Origin Policy*) estrito. |

## 4. Limitações, Falsos Positivos e Descartes

Durante a execução da ferramenta, a equipe identificou a necessidade de refinar os resultados obtidos, considerando as seguintes limitações e critérios de descarte:

1.  **Falsos Positivos de XSS:** O OWASP ZAP apontou alguns alertas de *Cross-Site Scripting* (XSS) em cabeçalhos de requisição que não são refletidos no corpo da página no frontend. Esses alertas foram descartados da prioridade, pois não representam um vetor explorável sem a interação de outros sistemas internos.
2.  **Alertas Informativos (Low/Informational):** Foram descartados dezenas de alertas referentes à ausência de cabeçalhos genéricos de segurança (como `X-Content-Type-Options`) em arquivos estáticos (imagens e CSS). O grupo priorizou focar a análise em achados que afetam diretamente a lógica de negócios e o vazamento de dados, que são o foco das ameaças mapeadas para a API de agendamentos.
3.  **Limitação de Autenticação:** Como o scan automatizado foi executado no formato caixa preta (sem script de login configurado no ZAP), a ferramenta não conseguiu varrer páginas protegidas por sessão. Para um teste real no futuro sistema de agendamento, será necessário configurar o contexto de autenticação no ZAP para garantir a cobertura dos endpoints restritos a pacientes e profissionais.