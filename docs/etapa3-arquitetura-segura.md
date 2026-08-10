# Etapa 3 — Projeto de uma Arquitetura Segura

## Decisões de arquitetura

| Decisão | Risco tratado | Justificativa | Componente afetado | Resultado esperado |
|---|---|---|---|---|
| Implementar *Rate Limiting* (Limitação de Taxa) por IP na borda da aplicação, alinhado à configuração de *Timeouts* estritos para consultas longas no banco de dados. | R05 — Indisponibilidade da API por exaustão de recursos | A ausência de restrições no tráfego de entrada permite que a API tente processar picos massivos de requisições anômalas ou maliciosas (como ataques DDoS ou varreduras automatizadas). Isso gera um gargalo rápido que esgota recursos vitais do sistema (CPU, memória RAM e, principalmente, o *pool* de conexões do banco de dados). Ao limitar a taxa de acesso, estipulamos um teto de processamento seguro que a infraestrutura suporta sem degradar. | API Gateway (Proxy Reverso) e Servidor de Aplicação | Rejeição automática de requisições excedentes (retornando imediatamente `429 Too Many Requests`) antes que elas impactem a aplicação. Isso preserva a estabilidade geral da plataforma, evita interrupções na comunicação com o banco de dados e mantém o serviço de agendamento 100% disponível e funcional para os usuários legítimos. |
