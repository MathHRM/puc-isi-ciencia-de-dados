# Prompts LLM — EDA Orçamento Federal 2025

Documentação dos prompts significativos usados com IA generativa neste projeto.
Conforme CLAUDE.md: prompts de ajuste menores ("proceed", "fix it") não são documentados.

---

## 1. Planejamento inicial

**Prompt:**

> "help me create a plan to resolve this work / use brainstorm and grill me skills to make a complete overview"

**Resultado:** Sessão de brainstorming + grill-me para levantar todos os requisitos do projeto, restrições de prazo, tamanho da equipe (solo), ferramentas disponíveis e escolha dos datasets. Gerou o spec em `docs/superpowers/specs/` e o plano em `docs/superpowers/plans/`.

---

## 2. Seleção do dataset

**Prompt:**

> Datasets escolhidos: Portal da Transparência — `2025_Receitas.csv` (175k linhas) e `2025_OrcamentoDespesa.csv` (26k linhas), ano-exercício 2025, encoding ISO-8859-1, separador `;`.

**Resultado:** Dois datasets federais brasileiros oficiais de 2025, cobrindo receitas arrecadadas e despesas orçamentárias executadas.

---

## 3. Definição do problema

**Prompt:**

> "Qual o equilíbrio entre receitas arrecadadas e despesas executadas no orçamento federal brasileiro em 2025, e como esse equilíbrio se distribui entre órgãos e funções?"

**Resultado:** Problema orientado a dados, com atributos específicos (receita realizada vs. despesa executada, por órgão superior), definido como hipótese central do projeto.

---

## 4. Métodos estatísticos

**Prompt (implícito na sessão de brainstorming):**

> Selecionar ≥4 métodos estatísticos cobrindo todos os critérios de avaliação.

**Resultado:** Cinco métodos definidos:

1. Média — gasto/receita médio por órgão e função
2. Mediana — taxa de execução mediana por órgão
3. Desvio padrão — variância nas taxas de execução
4. Percentis (P25/P75/P90) — concentração de gastos
5. Correlação — previsto vs. realizado (ambos os datasets)

---

## 5. Seleção das visualizações

**Prompt:**

> "use 1, 2, 3 and 6" (referente à lista de opções de gráficos apresentadas)

**Resultado:** Quatro visualizações selecionadas:

1. Bar chart — Top 10 órgãos por despesa executada
2. Bar chart agrupado — Receita prevista vs. realizada por categoria econômica
3. Scatter plot — Orçamento inicial vs. executado por função
4. Horizontal bar — % execução orçamentária por órgão superior

---

## 6. Escolha das ferramentas

**Contexto da sessão de brainstorming:**

> Python (pandas, matplotlib, seaborn) para análise; relatório em Google Docs → PDF.

**Resultado:** Stack definida: Python no Jupyter Notebook para EDA e geração dos gráficos PNG; relatório científico no Google Docs exportado como PDF.

---

## 7. Estratégia de implementação

**Prompt:**

> "implement using subagents / create revisable amounts of code, going through tasks slowly / ask confirmation before implementing a full task"

**Resultado:** Adotado o padrão subagent-driven development — um subagente por tarefa, revisão em duas etapas (spec compliance + code quality), confirmação do usuário antes de cada tarefa.

---

## 8. Relatório PDF — decisão de postergar

**Prompt:**

> "no. Lets make the report first, but document it is missing. We need to make it later"

**Contexto:** Pergunta sobre gerar o PDF automaticamente vs. manualmente no Google Docs.

**Resultado:** PDF do relatório marcado como pendente no plano (Task 14). Será produzido manualmente no Google Docs após conclusão da análise.

---

## 9. Estrutura do relatório — seguir template PUC

**Prompt:**

> "siga o padrao do template @template_tcc_desenvolvimento_template.md"

**Resultado:** Relatório reestruturado conforme template TCC de desenvolvimento da PUC Minas: seções numeradas (1. Introdução, 2. Referencial Teórico, 3. Trabalhos Relacionados, 4. Metodologia, 5. Resultados e Análise, 6. Conclusão), formatação de tabelas, referências alfabéticas com recuo 0,5 cm, indentação 1,27 cm a partir do segundo parágrafo.

---

## 10. Localização da seção de uso de IA

**Prompt:**

> "acredito que uso da ia vai em metodologia, nao?"

**Resultado:** Seção de uso de inteligência artificial generativa movida para dentro de 4. Metodologia como subseção 4.3, em vez de seção independente. Decisão estrutural sobre organização do relatório.

---

## 11. Correção das referências do dataset

**Prompt:**

> "the references for the dataset are only:\n\nhttps://portaldatransparencia.gov.br/download-de-dados/orcamento-despesa\nhttps://portaldatransparencia.gov.br/download-de-dados/receitas"

**Resultado:** Referências bibliográficas corrigidas: referência genérica ao Portal da Transparência substituída por duas entradas específicas (CGU 2025a e CGU 2025b) apontando para as páginas exatas de download dos datasets. Citações inline nas seções 2.3 e 5.1 atualizadas correspondentemente.
