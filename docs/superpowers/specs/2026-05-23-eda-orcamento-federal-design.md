# EDA — Equilíbrio Fiscal Federal 2025

**Date:** 2026-05-23
**Deadline:** 2026-05-24
**Author:** Solo project — PUC Ciência de Dados e Big Data, Unidade II

---

## Problem Statement

"Qual o equilíbrio entre receitas arrecadadas e despesas executadas no orçamento federal brasileiro em 2025, e como esse equilíbrio se distribui entre órgãos e funções?"

## Hypotheses

1. Despesas sociais (Saúde, Educação, Previdência) concentram maior parcela do orçamento
2. Taxa de execução orçamentária varia significativamente entre órgãos
3. Receitas realizadas divergem do previsto em categorias específicas

---

## Datasets

| File                        | Rows    | Size | Encoding   | Separator |
| --------------------------- | ------- | ---- | ---------- | --------- |
| `2025_Receitas.csv`         | 175,279 | 54MB | ISO-8859-1 | `;`       |
| `2025_OrcamentoDespesa.csv` | 26,161  | 13MB | ISO-8859-1 | `;`       |

### Receitas — Key Columns

- `NOME ÓRGÃO SUPERIOR`, `NOME ÓRGÃO`
- `CATEGORIA ECONÔMICA`, `ORIGEM RECEITA`, `ESPÉCIE RECEITA`
- `VALOR PREVISTO ATUALIZADO`, `VALOR LANÇADO`, `VALOR REALIZADO`, `PERCENTUAL REALIZADO`
- `DATA LANÇAMENTO`, `ANO EXERCÍCIO`

### OrcamentoDespesa — Key Columns

- `NOME ÓRGÃO SUPERIOR`, `NOME ÓRGÃO SUBORDINADO`
- `NOME FUNÇÃO`, `NOME SUBFUNÇÃO`, `NOME PROGRAMA ORÇAMENTÁRIO`, `NOME AÇÃO`
- `NOME CATEGORIA ECONÔMICA`, `NOME GRUPO DE DESPESA`, `NOME ELEMENTO DE DESPESA`
- `ORÇAMENTO INICIAL`, + valores executados

---

## Statistical Methods (≥4 required)

1. **Média** — avg spending/revenue per organ and function
2. **Mediana** — median execution rate across organs
3. **Desvio padrão** — variance in budget execution rates
4. **Percentis (P25/P75/P90)** — spending concentration distribution
5. **Correlação** — planned vs realized values (both datasets)

---

## Visualizations (≥3 required)

1. **Bar chart** — Top 10 órgãos por despesa executada
2. **Bar chart** — Receita prevista vs realizada por categoria econômica
3. **Scatter plot** — Orçamento inicial vs executado por função
4. **Horizontal bar** — % execução orçamentária por órgão superior

All exported as PNG to `graficos/`.

---

## File Structure

```
puc-isi-ciencia-de-dados/
├── base-de-dados/
│   ├── 2025_Receitas.csv
│   ├── 2025_OrcamentoDespesa.csv
│   ├── dicionario-de-dados-receita.md
│   ├── dicionario-de-dados-orcamento-despesa.md
│   └── dataset_tratado.csv        (output)
├── analise.ipynb                  (Python EDA)
├── graficos/
│   ├── top10_orgaos_despesa.png
│   ├── receita_prevista_realizada.png
│   ├── scatter_orcamento_executado.png
│   └── execucao_por_orgao.png
├── relatorio.pdf                  (Google Docs → PDF)
├── prompts.md
└── CLAUDE.md
```

---

## Tools

- **Python**: pandas, matplotlib, seaborn
- **Report**: Google Docs → PDF
- **Encoding**: ISO-8859-1, separator `;`

---

## Execution Order

| Step | Task                                                          | Est. Time |
| ---- | ------------------------------------------------------------- | --------- |
| 1    | Data loading & cleaning (encoding, nulls, numeric conversion) | 1h        |
| 2    | EDA & statistical analysis (5 methods)                        | 1h        |
| 3    | Visualizations (4 charts, export PNG)                         | 1h        |
| 4    | Export treated dataset CSV                                    | 15min     |
| 5    | Report writing (Google Docs, scientific format)               | 2h        |
| 6    | prompts.md documentation                                      | 15min     |

**Total: ~5.5h**

---

## Grading Coverage

| Criterion                            | Weight | Coverage                             |
| ------------------------------------ | ------ | ------------------------------------ |
| Statistical results presentation     | 20%    | 5 methods, tabulated                 |
| Data visualization                   | 20%    | 4 charts                             |
| Critical analysis & conclusion       | 15%    | fiscal balance narrative             |
| Problem definition & hypotheses      | 10%    | 3 hypotheses defined                 |
| Generative AI use & prompt docs      | 10%    | prompts.md                           |
| Dataset identification & description | 5%     | both datasets described              |
| Dataset quality & adequacy           | 5%     | 2025 data, 175k+ rows                |
| Formatting, organization & citations | 5%     | scientific format, dataset URL cited |
