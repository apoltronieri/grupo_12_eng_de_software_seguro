# Etapa 2 — Análise, Priorização e Tratamento de Riscos

## 8. Critérios de avaliação

### 8.1 Critérios de probabilidade

| Valor | Classificação | Critério |
|---:|---|---|
| 1 | Baixa | O evento depende de condições incomuns, acesso muito específico ou grande capacidade técnica. |
| 2 | Média-baixa | O evento é possível, mas depende de uma vulnerabilidade ou condição específica. |
| 3 | Média-alta | O evento é plausível e pode ocorrer em situações comuns de uso ou ataque. |
| 4 | Alta | O evento pode ocorrer com facilidade, frequência ou durante condições previsíveis do sistema. |

### 8.2 Critérios de impacto

| Valor | Classificação | Critério |
|---:|---|---|
| 1 | Baixo | Causa pequeno transtorno e pode ser corrigido rapidamente. |
| 2 | Moderado | Causa interrupção ou inconsistência limitada, com possibilidade de recuperação. |
| 3 | Alto | Causa prejuízo relevante aos usuários, ao negócio, à administração ou à privacidade. |
| 4 | Muito alto | Pode afetar muitos usuários, comprometer operações críticas ou causar prejuízo grave. |

### 8.3 Cálculo e classificação

A pontuação é calculada por `Probabilidade × Impacto` e classificada conforme a escala abaixo.

| Pontuação | Nível do risco |
|---:|---|
| 1 a 3 | Baixo |
| 4 a 7 | Médio |
| 8 a 11 | Alto |
| 12 a 16 | Crítico |

## 9. Registro de riscos

| ID | Origem STRIDE | Probabilidade | Impacto | Pontuação | Nível |
|---|---|---:|---:|---:|---|
| [R01](registro-de-riscos/R01.md) | T01 — Spoofing / CA01 | 3 | 4 | 12 | Crítico |
| [R02](registro-de-riscos/R02.md) | T02 — Tampering / CA02 | 3 | 4 | 12 | Crítico |
| [R03](registro-de-riscos/R03.md) | T03 — Repudiation / CA03 | 2 | 3 | 6 | Médio |