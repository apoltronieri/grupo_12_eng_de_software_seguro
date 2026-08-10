# Etapa 6 — Monitoramento e Detecção de Intrusões

## Regras de detecção

| Risco observado | Fonte de dados | Condição de alerta | Resposta inicial |
|---|---|---|---|
| R05 / CA05 — Indisponibilidade por exaustão de recursos (Denial of Service) | Logs de Acesso do Servidor Web, Logs do API Gateway e Métricas da Aplicação (APM, monitoramento de JVM e de Conexões) | Identificação de um volume anormal de requisições provenientes de um mesmo IP, ou de um bloco de rede específico, num intervalo menor que 60 segundos. O alerta é disparado quando os logs registram picos acentuados de retornos HTTP `429` (Rate Limit Exceeded) ou quando o tempo de resposta da API se degrada a ponto de gerar erros HTTP `503` (Service Unavailable) ou *timeouts* de banco de dados. | Disparar imediatamente um alerta de alta severidade (via Slack, Teams ou e-mail) para a equipe de DevOps e Segurança. Automatizar o isolamento do tráfego suspeito através do bloqueio temporário do IP ofensor diretamente no WAF (Web Application Firewall). Iniciar investigação manual sobre o padrão das chamadas para descartar falsos positivos e ajustar os limites do rate limit, se necessário. |
