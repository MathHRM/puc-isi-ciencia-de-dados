# EDA — Equilíbrio Fiscal Federal Brasileiro 2025

Análise Exploratória de Dados do orçamento federal brasileiro, exercício 2025.  
Trabalho acadêmico — PUC Minas Betim, Ciência de Dados e Big Data, Unidade II.

**Autor:** Matheus Henrique Resende Magalhães

---

## Problema

> Qual o equilíbrio entre receitas arrecadadas e despesas executadas no orçamento federal brasileiro em 2025, e como esse equilíbrio se distribui entre órgãos superiores e funções de governo?

## Hipóteses

| ID | Hipótese | Resultado |
|----|----------|-----------|
| H1 | Despesas: correlação r > 0,9 entre orçamento atualizado e realizado | Confirmada (r = 0,9986) |
| H2 | Receitas: mediana de execução < 10% do previsto | Confirmada (mediana = 0,00%) |
| H3 | Top 3 funções concentram > 70% das despesas executadas | Confirmada (84,8%) |

---

## Estrutura do Projeto

```
.
├── analise.ipynb                  # Notebook principal — toda a análise
├── prompts.md                     # Prompts de IA documentados (requisito do trabalho)
├── relatorio_rascunho.md          # Rascunho do relatório científico (formato PUC TCC)
├── base-de-dados/
│   ├── 2025_Receitas.csv          # Dataset receitas (175.278 linhas, ISO-8859-1, sep=;)
│   ├── 2025_OrcamentoDespesa.csv  # Dataset despesas (26.160 linhas, ISO-8859-1, sep=;)
│   ├── dataset_tratado.csv        # Dataset consolidado por órgão superior com saldo fiscal
│   ├── dicionario-de-dados-receita.md
│   └── dicionario-de-dados-orcamento-despesa.md
└── graficos/
    ├── top10_orgaos_despesa.png       # Top 10 órgãos por despesa executada
    ├── receita_prevista_realizada.png # Receita prevista vs realizada por categoria
    ├── scatter_orcamento_executado.png # Orçamento inicial vs executado por função
    └── execucao_por_orgao.png         # Taxa de execução por órgão superior (Top 20)
```

---

## Datasets

Fonte: Portal da Transparência do Governo Federal (CGU), exercício 2025.

| Dataset | URL de download |
|---------|----------------|
| Orçamento de Despesas | https://portaldatransparencia.gov.br/download-de-dados/orcamento-despesa |
| Receitas | https://portaldatransparencia.gov.br/download-de-dados/receitas |

---

## Métodos Estatísticos

1. **Média** — valor médio de receita/despesa por linha de registro
2. **Mediana** — taxa de execução mediana por órgão
3. **Desvio padrão** — variância nas taxas de execução entre órgãos
4. **Percentis** (P25 / P75 / P90) — concentração e distribuição dos gastos
5. **Correlação de Pearson** — previsto vs. realizado em ambos os datasets

---

## Principais Resultados

- Despesas: r = **0,9986** entre orçamento atualizado e realizado — planejamento altamente eficaz
- Receitas: mediana de execução = **0,00%** — maioria das linhas sem meta prevista registrada no SIAFI
- **Encargos Especiais + Previdência Social + Assistência Social** = 84,8% das despesas totais (~R$ 4,1 tri de R$ 4,85 tri)
- P25 das despesas = 0%, P75 = 95,28% — bimodalidade alta: linhas ou não executadas ou quase integralmente executadas

---

## Como Reproduzir

```bash
# Requisitos
pip install pandas matplotlib seaborn jupyter

# Executar análise completa
jupyter notebook analise.ipynb
# Executar todas as células em ordem
```

Os gráficos são salvos automaticamente em `graficos/` e o dataset tratado em `base-de-dados/dataset_tratado.csv`.

---

## Ferramentas

| Funcionalidade | Ferramenta |
|---|---|
| Linguagem | Python 3.11 |
| Manipulação de dados | pandas 2.x |
| Visualizações | matplotlib 3.x, seaborn 0.13.x |
| Ambiente | Jupyter Notebook |
| SO | Ubuntu Linux 24.04 |
| IA generativa | Claude (Anthropic) via Claude Code CLI |
