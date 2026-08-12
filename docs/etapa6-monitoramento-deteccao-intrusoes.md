# Etapa 6 - Monitoramento e Detecção de Intrusões

### Regra de Detecção: R04 / CA04
| Campo | Descrição |
|---|---|
| Risco observado | `R04 / CA04` — Tentativa de extração em massa (*scraping*) de dados pessoais e sensíveis dos profissionais. |
| Fonte de dados | Logs de requisição HTTP da API e logs de auditoria de acessos aos endpoints de busca/consulta de profissionais. |
| Condição de alerta | Um único usuário autenticado (mesmo ID de sessão ou IP) realiza um volume excessivo e atípico de buscas ou acessos a perfis de profissionais em um curto intervalo de tempo (ex: paginação rápida, mais de 30 requisições por minuto), indicando o uso de automação ou comportamento anômalo para coleta de dados da resposta da API. |
| Resposta inicial | Invalidar imediatamente a sessão ativa (logout forçado), bloquear temporariamente novas requisições da conta/IP e disparar um alerta de alta prioridade para a equipe de segurança investigar se houve vazamento efetivo. |