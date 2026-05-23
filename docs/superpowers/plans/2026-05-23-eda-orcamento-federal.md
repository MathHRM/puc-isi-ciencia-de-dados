# EDA — Equilíbrio Fiscal Federal 2025 — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a Python EDA notebook analyzing federal fiscal balance in 2025 using revenue and spending datasets, producing 4 visualizations, 5 statistical methods, a treated dataset, and a prompts log.

**Architecture:** Single Jupyter notebook (`analise.ipynb`) loads both ISO-8859-1 semicolon-separated CSVs, cleans numeric columns (Brazilian decimal format), runs statistical analysis, generates 4 PNG charts, and exports a treated CSV. Report is written manually in Google Docs.

**Tech Stack:** Python 3, pandas, matplotlib, seaborn, jupyter

---

## File Structure

| File | Purpose |
|------|---------|
| `analise.ipynb` | Main EDA notebook — all code |
| `graficos/top10_orgaos_despesa.png` | Viz 1 output |
| `graficos/receita_prevista_realizada.png` | Viz 2 output |
| `graficos/scatter_orcamento_executado.png` | Viz 3 output |
| `graficos/execucao_por_orgao.png` | Viz 4 output |
| `base-de-dados/dataset_tratado.csv` | Cleaned/aggregated output |
| `prompts.md` | All LLM prompts used |

---

## Task 1: Environment Setup

**Files:**
- Create: `analise.ipynb`
- Create: `graficos/` directory

- [ ] **Step 1: Install dependencies**

```bash
pip install pandas matplotlib seaborn jupyter openpyxl
```

Expected: all packages install without error.

- [ ] **Step 2: Create graficos directory**

```bash
mkdir -p ./graficos
```

- [ ] **Step 3: Create notebook with imports cell**

Create `analise.ipynb`. First cell:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.ticker as mticker
import seaborn as sns
import warnings

warnings.filterwarnings('ignore')
plt.rcParams['figure.dpi'] = 150
plt.rcParams['font.family'] = 'DejaVu Sans'
sns.set_theme(style='whitegrid')

GRAFICOS = 'graficos'
BASE = 'base-de-dados'
```

- [ ] **Step 4: Verify imports run**

Run the cell. Expected: no errors, no output.

---

## Task 2: Load Receitas Dataset

**Files:**
- Modify: `analise.ipynb`

- [ ] **Step 1: Load raw CSV**

New cell in `analise.ipynb`:

```python
receitas_raw = pd.read_csv(
    f'{BASE}/2025_Receitas.csv',
    sep=';',
    encoding='iso-8859-1',
    dtype=str,
    quotechar='"'
)
receitas_raw.columns = receitas_raw.columns.str.strip().str.strip('"')
print(receitas_raw.shape)
print(receitas_raw.columns.tolist())
```

Expected output: `(175279, 16)` and 16 column names.

- [ ] **Step 2: Preview data**

New cell:

```python
receitas_raw.head(3)
```

Expected: 3 rows with string values, commas as decimal separators.

---

## Task 3: Clean Receitas Dataset

**Files:**
- Modify: `analise.ipynb`

- [ ] **Step 1: Convert money columns to float**

New cell:

```python
MONEY_COLS_REC = [
    'VALOR PREVISTO ATUALIZADO',
    'VALOR LANÇADO',
    'VALOR REALIZADO',
    'PERCENTUAL REALIZADO',
]

receitas = receitas_raw.copy()

for col in MONEY_COLS_REC:
    receitas[col] = (
        receitas[col]
        .str.strip('"')
        .str.replace('.', '', regex=False)
        .str.replace(',', '.', regex=False)
        .pipe(pd.to_numeric, errors='coerce')
    )

print(receitas[MONEY_COLS_REC].dtypes)
print(receitas[MONEY_COLS_REC].isnull().sum())
```

Expected: all 4 columns as `float64`, null counts shown.

- [ ] **Step 2: Strip quotes from string columns**

New cell:

```python
STR_COLS_REC = [
    'NOME ÓRGÃO SUPERIOR', 'NOME ÓRGÃO',
    'CATEGORIA ECONÔMICA', 'ORIGEM RECEITA', 'ESPÉCIE RECEITA',
    'DATA LANÇAMENTO', 'ANO EXERCÍCIO',
]

for col in STR_COLS_REC:
    receitas[col] = receitas[col].str.strip('"').str.strip()

print(receitas['CATEGORIA ECONÔMICA'].unique())
print(receitas['ANO EXERCÍCIO'].unique())
```

Expected: `['Receitas Correntes' 'Receitas de Capital']` and `['2025']`.

- [ ] **Step 3: Drop rows with null realized value**

New cell:

```python
before = len(receitas)
receitas = receitas.dropna(subset=['VALOR REALIZADO'])
print(f'Dropped {before - len(receitas)} rows with null VALOR REALIZADO')
print(f'Remaining: {len(receitas)} rows')
```

---

## Task 4: Load and Clean OrcamentoDespesa Dataset

**Files:**
- Modify: `analise.ipynb`

- [ ] **Step 1: Load raw CSV**

New cell:

```python
desp_raw = pd.read_csv(
    f'{BASE}/2025_OrcamentoDespesa.csv',
    sep=';',
    encoding='iso-8859-1',
    dtype=str,
    quotechar='"'
)
desp_raw.columns = desp_raw.columns.str.strip().str.strip('"')
print(desp_raw.shape)
print(desp_raw.columns.tolist())
```

Expected: `(26161, 26)` and 26 column names.

- [ ] **Step 2: Convert money columns to float**

New cell:

```python
MONEY_COLS_DESP = [
    'ORÇAMENTO INICIAL (R$)',
    'ORÇAMENTO ATUALIZADO (R$)',
    'ORÇAMENTO EMPENHADO (R$)',
    'ORÇAMENTO REALIZADO (R$)',
]

PCT_COL_DESP = '% REALIZADO DO ORÇAMENTO (COM RELAÇÃO AO ORÇAMENTO ATUALIZADO)'

desp = desp_raw.copy()

for col in MONEY_COLS_DESP:
    desp[col] = (
        desp[col]
        .str.strip('"')
        .str.replace('.', '', regex=False)
        .str.replace(',', '.', regex=False)
        .pipe(pd.to_numeric, errors='coerce')
    )

desp[PCT_COL_DESP] = (
    desp[PCT_COL_DESP]
    .str.strip('"')
    .str.replace('%', '', regex=False)
    .str.replace('.', '', regex=False)
    .str.replace(',', '.', regex=False)
    .pipe(pd.to_numeric, errors='coerce')
)

print(desp[MONEY_COLS_DESP + [PCT_COL_DESP]].dtypes)
print(desp[MONEY_COLS_DESP].isnull().sum())
```

Expected: all 5 columns as `float64`.

- [ ] **Step 3: Strip quotes from string columns**

New cell:

```python
STR_COLS_DESP = [
    'NOME ÓRGÃO SUPERIOR', 'NOME ÓRGÃO SUBORDINADO',
    'NOME FUNÇÃO', 'NOME SUBFUNÇÃO',
    'NOME CATEGORIA ECONÔMICA', 'NOME GRUPO DE DESPESA',
]

for col in STR_COLS_DESP:
    desp[col] = desp[col].str.strip('"').str.strip()

print(desp['NOME FUNÇÃO'].value_counts().head(10))
```

Expected: top 10 functions by number of rows (not by value yet).

---

## Task 5: Statistical Analysis — Receitas

**Files:**
- Modify: `analise.ipynb`

- [ ] **Step 1: Summary stats (média, mediana, desvio padrão, percentis)**

New cell:

```python
print("=== ESTATÍSTICAS — RECEITAS ===\n")

stats_rec = receitas[['VALOR PREVISTO ATUALIZADO', 'VALOR REALIZADO']].describe(
    percentiles=[0.25, 0.50, 0.75, 0.90]
)
print(stats_rec.to_string())
```

Expected: table with count, mean, std, min, 25%, 50%, 75%, 90%, max for both columns.

- [ ] **Step 2: Mean and median by category**

New cell:

```python
rec_por_categoria = receitas.groupby('CATEGORIA ECONÔMICA').agg(
    total_previsto=('VALOR PREVISTO ATUALIZADO', 'sum'),
    total_realizado=('VALOR REALIZADO', 'sum'),
    media_realizado=('VALOR REALIZADO', 'mean'),
    mediana_realizado=('VALOR REALIZADO', 'median'),
    desvio_realizado=('VALOR REALIZADO', 'std'),
).reset_index()

rec_por_categoria['taxa_execucao_%'] = (
    rec_por_categoria['total_realizado'] / rec_por_categoria['total_previsto'] * 100
).round(2)

print(rec_por_categoria.to_string())
```

- [ ] **Step 3: Correlation between previsto and realizado**

New cell:

```python
corr_rec = receitas[['VALOR PREVISTO ATUALIZADO', 'VALOR LANÇADO', 'VALOR REALIZADO']].corr()
print("=== CORRELAÇÃO — RECEITAS ===")
print(corr_rec.to_string())
```

Expected: correlation matrix, expect high correlation between PREVISTO and REALIZADO.

- [ ] **Step 4: Percentile analysis by órgão superior**

New cell:

```python
rec_por_orgao = receitas.groupby('NOME ÓRGÃO SUPERIOR')['VALOR REALIZADO'].sum().reset_index()
rec_por_orgao.columns = ['orgao', 'total_realizado']

p25 = rec_por_orgao['total_realizado'].quantile(0.25)
p75 = rec_por_orgao['total_realizado'].quantile(0.75)
p90 = rec_por_orgao['total_realizado'].quantile(0.90)

print(f"P25: R$ {p25:,.2f}")
print(f"P75: R$ {p75:,.2f}")
print(f"P90: R$ {p90:,.2f}")
print(f"\nTop 10 órgãos por receita realizada:")
print(rec_por_orgao.sort_values('total_realizado', ascending=False).head(10).to_string(index=False))
```

---

## Task 6: Statistical Analysis — Despesas

**Files:**
- Modify: `analise.ipynb`

- [ ] **Step 1: Summary stats**

New cell:

```python
print("=== ESTATÍSTICAS — ORÇAMENTO DESPESA ===\n")

stats_desp = desp[MONEY_COLS_DESP].describe(percentiles=[0.25, 0.50, 0.75, 0.90])
print(stats_desp.to_string())
```

- [ ] **Step 2: Mean, median, std by função**

New cell:

```python
desp_por_funcao = desp.groupby('NOME FUNÇÃO').agg(
    total_inicial=('ORÇAMENTO INICIAL (R$)', 'sum'),
    total_atualizado=('ORÇAMENTO ATUALIZADO (R$)', 'sum'),
    total_realizado=('ORÇAMENTO REALIZADO (R$)', 'sum'),
    media_realizado=('ORÇAMENTO REALIZADO (R$)', 'mean'),
    mediana_realizado=('ORÇAMENTO REALIZADO (R$)', 'median'),
    desvio_realizado=('ORÇAMENTO REALIZADO (R$)', 'std'),
).reset_index()

desp_por_funcao['taxa_execucao_%'] = (
    desp_por_funcao['total_realizado'] / desp_por_funcao['total_atualizado'] * 100
).round(2)

print(desp_por_funcao.sort_values('total_realizado', ascending=False).to_string(index=False))
```

- [ ] **Step 3: Correlation between budget columns**

New cell:

```python
corr_desp = desp[MONEY_COLS_DESP].corr()
print("=== CORRELAÇÃO — DESPESAS ===")
print(corr_desp.to_string())
```

- [ ] **Step 4: Percentile analysis by órgão superior**

New cell:

```python
desp_por_orgao = desp.groupby('NOME ÓRGÃO SUPERIOR')['ORÇAMENTO REALIZADO (R$)'].sum().reset_index()
desp_por_orgao.columns = ['orgao', 'total_realizado']

p25_d = desp_por_orgao['total_realizado'].quantile(0.25)
p75_d = desp_por_orgao['total_realizado'].quantile(0.75)
p90_d = desp_por_orgao['total_realizado'].quantile(0.90)

print(f"P25: R$ {p25_d:,.2f}")
print(f"P75: R$ {p75_d:,.2f}")
print(f"P90: R$ {p90_d:,.2f}")
print(f"\nTop 10 órgãos por despesa realizada:")
print(desp_por_orgao.sort_values('total_realizado', ascending=False).head(10).to_string(index=False))
```

---

## Task 7: Visualization 1 — Top 10 Órgãos por Despesa Executada

**Files:**
- Modify: `analise.ipynb`
- Create: `graficos/top10_orgaos_despesa.png`

- [ ] **Step 1: Prepare data**

New cell:

```python
top10_desp = (
    desp.groupby('NOME ÓRGÃO SUPERIOR')['ORÇAMENTO REALIZADO (R$)']
    .sum()
    .sort_values(ascending=False)
    .head(10)
    .reset_index()
)
top10_desp.columns = ['orgao', 'realizado']
top10_desp['realizado_bi'] = top10_desp['realizado'] / 1e9
top10_desp['orgao_curto'] = top10_desp['orgao'].str[:40]
```

- [ ] **Step 2: Plot and save**

New cell:

```python
fig, ax = plt.subplots(figsize=(12, 6))

bars = ax.bar(
    top10_desp['orgao_curto'],
    top10_desp['realizado_bi'],
    color=sns.color_palette('Blues_d', 10)
)

ax.set_title('Top 10 Órgãos por Despesa Executada — 2025', fontsize=14, fontweight='bold')
ax.set_xlabel('Órgão Superior', fontsize=11)
ax.set_ylabel('Valor Realizado (R$ bilhões)', fontsize=11)
ax.tick_params(axis='x', rotation=45)

for bar, val in zip(bars, top10_desp['realizado_bi']):
    ax.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.5,
            f'R${val:.1f}bi', ha='center', va='bottom', fontsize=8)

plt.tight_layout()
plt.savefig(f'{GRAFICOS}/top10_orgaos_despesa.png', dpi=150, bbox_inches='tight')
plt.show()
print('Saved: graficos/top10_orgaos_despesa.png')
```

Expected: bar chart saved, displayed inline.

---

## Task 8: Visualization 2 — Receita Prevista vs Realizada por Categoria

**Files:**
- Modify: `analise.ipynb`
- Create: `graficos/receita_prevista_realizada.png`

- [ ] **Step 1: Prepare data**

New cell:

```python
rec_cat = receitas.groupby('CATEGORIA ECONÔMICA').agg(
    previsto=('VALOR PREVISTO ATUALIZADO', 'sum'),
    realizado=('VALOR REALIZADO', 'sum'),
).reset_index()

rec_cat['previsto_bi'] = rec_cat['previsto'] / 1e9
rec_cat['realizado_bi'] = rec_cat['realizado'] / 1e9
```

- [ ] **Step 2: Plot grouped bar chart and save**

New cell:

```python
x = np.arange(len(rec_cat))
width = 0.35

fig, ax = plt.subplots(figsize=(10, 6))

b1 = ax.bar(x - width/2, rec_cat['previsto_bi'], width, label='Previsto', color='#4C72B0')
b2 = ax.bar(x + width/2, rec_cat['realizado_bi'], width, label='Realizado', color='#55A868')

ax.set_title('Receita: Previsto vs Realizado por Categoria Econômica — 2025',
             fontsize=13, fontweight='bold')
ax.set_xlabel('Categoria Econômica', fontsize=11)
ax.set_ylabel('Valor (R$ bilhões)', fontsize=11)
ax.set_xticks(x)
ax.set_xticklabels(rec_cat['CATEGORIA ECONÔMICA'], rotation=15)
ax.legend()

for bar in [*b1, *b2]:
    h = bar.get_height()
    ax.text(bar.get_x() + bar.get_width()/2, h + 0.5,
            f'R${h:.0f}bi', ha='center', va='bottom', fontsize=9)

plt.tight_layout()
plt.savefig(f'{GRAFICOS}/receita_prevista_realizada.png', dpi=150, bbox_inches='tight')
plt.show()
print('Saved: graficos/receita_prevista_realizada.png')
```

---

## Task 9: Visualization 3 — Scatter Orçamento Inicial vs Executado por Função

**Files:**
- Modify: `analise.ipynb`
- Create: `graficos/scatter_orcamento_executado.png`

- [ ] **Step 1: Prepare data**

New cell:

```python
scatter_data = desp.groupby('NOME FUNÇÃO').agg(
    inicial=('ORÇAMENTO INICIAL (R$)', 'sum'),
    realizado=('ORÇAMENTO REALIZADO (R$)', 'sum'),
).reset_index()

scatter_data['inicial_bi'] = scatter_data['inicial'] / 1e9
scatter_data['realizado_bi'] = scatter_data['realizado'] / 1e9
scatter_data['funcao_curta'] = scatter_data['NOME FUNÇÃO'].str[:20]
```

- [ ] **Step 2: Plot scatter and save**

New cell:

```python
fig, ax = plt.subplots(figsize=(11, 7))

ax.scatter(
    scatter_data['inicial_bi'],
    scatter_data['realizado_bi'],
    s=80, alpha=0.7, color='#4C72B0', edgecolors='white', linewidth=0.5
)

# Diagonal reference line (100% execution)
max_val = max(scatter_data['inicial_bi'].max(), scatter_data['realizado_bi'].max())
ax.plot([0, max_val], [0, max_val], 'r--', linewidth=1, alpha=0.5, label='100% execução')

# Label points
for _, row in scatter_data.iterrows():
    ax.annotate(
        row['funcao_curta'],
        (row['inicial_bi'], row['realizado_bi']),
        fontsize=7, alpha=0.8,
        xytext=(4, 4), textcoords='offset points'
    )

ax.set_title('Orçamento Inicial vs Realizado por Função — 2025', fontsize=13, fontweight='bold')
ax.set_xlabel('Orçamento Inicial (R$ bilhões)', fontsize=11)
ax.set_ylabel('Orçamento Realizado (R$ bilhões)', fontsize=11)
ax.legend()

plt.tight_layout()
plt.savefig(f'{GRAFICOS}/scatter_orcamento_executado.png', dpi=150, bbox_inches='tight')
plt.show()
print('Saved: graficos/scatter_orcamento_executado.png')
```

---

## Task 10: Visualization 4 — % Execução por Órgão Superior

**Files:**
- Modify: `analise.ipynb`
- Create: `graficos/execucao_por_orgao.png`

- [ ] **Step 1: Prepare data**

New cell:

```python
exec_orgao = desp.groupby('NOME ÓRGÃO SUPERIOR').agg(
    atualizado=('ORÇAMENTO ATUALIZADO (R$)', 'sum'),
    realizado=('ORÇAMENTO REALIZADO (R$)', 'sum'),
).reset_index()

exec_orgao['pct_execucao'] = (
    exec_orgao['realizado'] / exec_orgao['atualizado'] * 100
).round(1)

# Keep only órgãos with meaningful budget (above P25)
p25_budget = exec_orgao['atualizado'].quantile(0.25)
exec_orgao = exec_orgao[exec_orgao['atualizado'] > p25_budget]

exec_orgao = exec_orgao.sort_values('pct_execucao', ascending=True)
exec_orgao['orgao_curto'] = exec_orgao['NOME ÓRGÃO SUPERIOR'].str[:45]

print(f'Órgãos no gráfico: {len(exec_orgao)}')
```

- [ ] **Step 2: Plot horizontal bar and save**

New cell:

```python
fig, ax = plt.subplots(figsize=(10, max(6, len(exec_orgao) * 0.35)))

colors = ['#d73027' if x < 50 else '#4575b4' if x >= 80 else '#fdae61'
          for x in exec_orgao['pct_execucao']]

bars = ax.barh(exec_orgao['orgao_curto'], exec_orgao['pct_execucao'], color=colors)

ax.axvline(x=100, color='gray', linestyle='--', linewidth=1, alpha=0.7)
ax.set_title('% Execução Orçamentária por Órgão Superior — 2025', fontsize=13, fontweight='bold')
ax.set_xlabel('% Realizado / Orçamento Atualizado', fontsize=11)

for bar, val in zip(bars, exec_orgao['pct_execucao']):
    ax.text(val + 0.5, bar.get_y() + bar.get_height()/2,
            f'{val:.1f}%', va='center', fontsize=8)

plt.tight_layout()
plt.savefig(f'{GRAFICOS}/execucao_por_orgao.png', dpi=150, bbox_inches='tight')
plt.show()
print('Saved: graficos/execucao_por_orgao.png')
```

---

## Task 11: Export Treated Dataset

**Files:**
- Modify: `analise.ipynb`
- Create: `base-de-dados/dataset_tratado.csv`

- [ ] **Step 1: Build aggregated fiscal view**

New cell:

```python
# Aggregate receitas by órgão superior
rec_agg = receitas.groupby('NOME ÓRGÃO SUPERIOR').agg(
    receita_prevista=('VALOR PREVISTO ATUALIZADO', 'sum'),
    receita_realizada=('VALOR REALIZADO', 'sum'),
).reset_index().rename(columns={'NOME ÓRGÃO SUPERIOR': 'orgao_superior'})

# Aggregate despesas by órgão superior
desp_agg = desp.groupby('NOME ÓRGÃO SUPERIOR').agg(
    despesa_inicial=('ORÇAMENTO INICIAL (R$)', 'sum'),
    despesa_atualizada=('ORÇAMENTO ATUALIZADO (R$)', 'sum'),
    despesa_realizada=('ORÇAMENTO REALIZADO (R$)', 'sum'),
).reset_index().rename(columns={'NOME ÓRGÃO SUPERIOR': 'orgao_superior'})

# Merge
fiscal = pd.merge(rec_agg, desp_agg, on='orgao_superior', how='outer')

fiscal['saldo_fiscal'] = fiscal['receita_realizada'] - fiscal['despesa_realizada']
fiscal['pct_execucao_despesa'] = (
    fiscal['despesa_realizada'] / fiscal['despesa_atualizada'] * 100
).round(2)
fiscal['pct_execucao_receita'] = (
    fiscal['receita_realizada'] / fiscal['receita_prevista'] * 100
).round(2)

print(fiscal.shape)
print(fiscal.head())
```

- [ ] **Step 2: Export CSV**

New cell:

```python
fiscal.to_csv(f'{BASE}/dataset_tratado.csv', index=False, encoding='utf-8-sig')
print(f'Exported {len(fiscal)} rows to base-de-dados/dataset_tratado.csv')
```

Expected: file created, row count printed.

---

## Task 12: Document prompts.md

**Files:**
- Create: `prompts.md`

- [ ] **Step 1: Create prompts.md with all LLM prompts used**

Create `prompts.md` at project root. Add every significant prompt used with Claude during this project. Template:

```markdown
# Prompts LLM — EDA Orçamento Federal 2025

Registro de todos os prompts relevantes usados com LLMs neste projeto.

---

## Planejamento do projeto

**Prompt:**
> help me create a plan to resolve this work
> use brainstorm and grill me skills to make a complete overview of the work
> divide the plan in multiple steps, documenting all

---

## Escolha do dataset

**Prompt:**
> [cole os prompts de seleção de dataset aqui]

---

## Análise estatística

**Prompt:**
> [cole prompts sobre estatísticas aqui]

---

## Visualizações

**Prompt:**
> [cole prompts sobre visualizações aqui]

---

## Redação do relatório

**Prompt:**
> [cole prompts sobre o relatório aqui]
```

- [ ] **Step 2: Fill in all prompts from this conversation**

Go through the conversation history and add every significant prompt exchanged. Ignore adjustment prompts like "fix it", "yes", "no". Include: planning prompts, dataset selection, statistical method choices, visualization choices.

---

## Task 13: Verify All Outputs

**Files:**
- Modify: `analise.ipynb`

- [ ] **Step 1: Checklist cell**

Add final cell to notebook:

```python
import os

checks = {
    'graficos/top10_orgaos_despesa.png': os.path.exists('graficos/top10_orgaos_despesa.png'),
    'graficos/receita_prevista_realizada.png': os.path.exists('graficos/receita_prevista_realizada.png'),
    'graficos/scatter_orcamento_executado.png': os.path.exists('graficos/scatter_orcamento_executado.png'),
    'graficos/execucao_por_orgao.png': os.path.exists('graficos/execucao_por_orgao.png'),
    'base-de-dados/dataset_tratado.csv': os.path.exists('base-de-dados/dataset_tratado.csv'),
    'prompts.md': os.path.exists('prompts.md'),
}

for path, exists in checks.items():
    status = '✓' if exists else '✗ MISSING'
    print(f'{status}  {path}')

assert all(checks.values()), 'Some outputs missing!'
print('\nAll outputs verified.')
```

Expected: all 6 lines show `✓`.

- [ ] **Step 2: Confirm deliverables ready for report**

Manually verify:
1. Open each PNG in `graficos/` — charts readable, labeled, titled
2. Open `base-de-dados/dataset_tratado.csv` — has data, no all-NaN rows
3. Open `prompts.md` — all major prompts documented
4. All stats results (Tasks 5 & 6) visible in notebook output cells

---

---

## Task 14: Write PDF Report ⚠️ NOT YET IMPLEMENTED

> **Status:** Pending. Complete Tasks 1–13 first, then return here.

**Files:**
- Create: `relatorio.pdf` (via Google Docs → Export PDF)

**Scientific format structure (all sections required):**

- [ ] **1. Capa** — título, autor, instituição (PUC), disciplina, data

- [ ] **2. Resumo** (abstract, ~150 words) — problema, dados usados, métodos, principais achados

- [ ] **3. Introdução** (~300 words)
  - Contexto: orçamento federal brasileiro 2025
  - Justificativa: importância da análise fiscal pública
  - Problema de pesquisa (copy from spec): "Qual o equilíbrio entre receitas arrecadadas e despesas executadas no orçamento federal brasileiro em 2025, e como esse equilíbrio se distribui entre órgãos e funções?"
  - Hipóteses (copy from spec): 3 hypotheses listed

- [ ] **4. Descrição dos Dados** (~200 words)
  - Dataset 1: 2025_Receitas.csv — fonte, colunas principais, 175.279 registros
  - Dataset 2: 2025_OrcamentoDespesa.csv — fonte, colunas principais, 26.161 registros
  - Link/referência: https://portaldatransparencia.gov.br/download-de-dados
  - Período: Jan–2025 a data de download

- [ ] **5. Metodologia** (~200 words)
  - Ferramentas: Python 3, pandas, matplotlib, seaborn
  - Tratamento: encoding ISO-8859-1, separador `;`, conversão decimal brasileiro
  - Métodos estatísticos usados: média, mediana, desvio padrão, percentis (P25/P75/P90), correlação

- [ ] **6. Resultados** (~500 words)
  - Subseção 6.1: Estatísticas das Receitas — tabela com describe() output
  - Subseção 6.2: Estatísticas das Despesas — tabela com describe() output
  - Subseção 6.3: Visualização 1 — inserir `graficos/top10_orgaos_despesa.png` + 2–3 sentences
  - Subseção 6.4: Visualização 2 — inserir `graficos/receita_prevista_realizada.png` + 2–3 sentences
  - Subseção 6.5: Visualização 3 — inserir `graficos/scatter_orcamento_executado.png` + 2–3 sentences
  - Subseção 6.6: Visualização 4 — inserir `graficos/execucao_por_orgao.png` + 2–3 sentences

- [ ] **7. Análise Crítica e Discussão** (~400 words)
  - Revisit each hypothesis: confirmed or refuted? With evidence from results
  - Discuss outliers (órgãos with very high/low execution rates)
  - Discuss gap between receita prevista vs realizada
  - Limitations: data only covers 2025 partial year, no comparison baseline

- [ ] **8. Conclusão** (~150 words)
  - Summary of main findings
  - Answer the research question
  - Suggestions for future work

- [ ] **9. Referências**
  - Portal da Transparência: https://portaldatransparencia.gov.br/download-de-dados
  - Manual Técnico do Orçamento (cite year)
  - pandas, matplotlib, seaborn documentation

- [ ] **10. Export to PDF**
  - File → Download → PDF in Google Docs
  - Save as `relatorio.pdf` in project root

---

## Grading Checklist

| Criterion | Weight | Covered by |
|-----------|--------|------------|
| Statistical results | 20% | Tasks 5, 6 |
| Visualizations | 20% | Tasks 7, 8, 9, 10 |
| Critical analysis | 15% | Task 14 §7 |
| Problem + hypotheses | 10% | Task 14 §3 |
| LLM prompts | 10% | Task 12 |
| Dataset description | 5% | Task 14 §4 |
| Dataset quality | 5% | 2025 data, 200k rows |
| Formatting | 5% | Task 14 (scientific format) |
