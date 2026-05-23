# Dicionário de Dados — Orçamento da Despesa

## Categoria Econômica

| Código | Categoria Econômica |
| ------ | ------------------- |
| 3      | Despesas Correntes  |
| 4      | Despesas de Capital |

### Descrição

- **3 — Despesas Correntes:** despesas que não contribuem diretamente para a formação ou aquisição de um bem de capital.
- **4 — Despesas de Capital:** despesas que contribuem diretamente para a formação ou aquisição de um bem de capital.

---

## Grupo de Natureza da Despesa (GND)

| Código | Grupo de Natureza da Despesa |
| ------ | ---------------------------- |
| 1      | Pessoal e Encargos Sociais   |
| 2      | Juros e Encargos da Dívida   |
| 3      | Outras Despesas Correntes    |
| 4      | Investimentos                |
| 5      | Inversões Financeiras        |
| 6      | Amortização da Dívida        |
| 9      | Reserva de Contingência      |

### Descrição dos grupos

#### 1 — Pessoal e Encargos Sociais

Despesas com pessoal ativo, inativo e pensionistas, incluindo salários, vantagens, subsídios, aposentadorias, pensões e encargos sociais.

#### 2 — Juros e Encargos da Dívida

Despesas relacionadas ao pagamento de juros, comissões e encargos de operações de crédito internas e externas.

#### 3 — Outras Despesas Correntes

Despesas com materiais de consumo, diárias, auxílio-alimentação, auxílio-transporte, subvenções e demais despesas correntes.

#### 4 — Investimentos

Despesas com obras, instalações, equipamentos, softwares e aquisição de imóveis necessários às atividades públicas.

#### 5 — Inversões Financeiras

Despesas com aquisição de imóveis já em uso, compra de participação societária e aumento de capital em empresas.

#### 6 — Amortização da Dívida

Despesas destinadas ao pagamento ou refinanciamento do principal da dívida pública.

#### 9 — Reserva de Contingência

Reserva utilizada para cobertura de riscos fiscais e eventos imprevistos.

---

## Estrutura Geral do Dataset

| Coluna                       | Descrição                                               |
| ---------------------------- | ------------------------------------------------------- |
| Exercício                    | Ano de referência dos valores                           |
| Código Órgão Superior        | Código do órgão responsável pela despesa                |
| Nome Órgão Superior          | Nome do órgão responsável pela despesa                  |
| Código Órgão Subordinado     | Código do órgão subordinado                             |
| Nome Órgão Subordinado       | Nome do órgão subordinado                               |
| Código Unidade Orçamentária  | Código da unidade orçamentária                          |
| Nome Unidade Orçamentária    | Nome da unidade orçamentária                            |
| Código Função                | Código da função da despesa                             |
| Nome Função                  | Nome da função da despesa                               |
| Código Subfunção             | Código da subfunção da despesa                          |
| Nome Subfunção               | Nome da subfunção da despesa                            |
| Código Programa Orçamentário | Código do programa orçamentário                         |
| Nome Programa Orçamentário   | Nome do programa                                        |
| Código Ação                  | Código da ação orçamentária                             |
| Nome Ação                    | Nome da ação orçamentária                               |
| Código Categoria Econômica   | Código da categoria econômica                           |
| Categoria Econômica          | Nome da categoria econômica                             |
| Código Grupo de Despesa      | Código do grupo de despesa                              |
| Nome Grupo de Despesa        | Nome do grupo de despesa                                |
| Código Elemento de Despesa   | Código do elemento de despesa                           |
| Nome Elemento de Despesa     | Nome do elemento de despesa                             |
| Orçamento Inicial (R$)       | Valor inicial previsto                                  |
| Orçamento Atualizado (R$)    | Valor atualizado do orçamento                           |
| Orçamento Empenhado (R$)     | Valor já empenhado                                      |
| Orçamento Realizado (R$)     | Valor efetivamente pago                                 |
| % Realizado do orçamento     | Percentual executado em relação ao orçamento atualizado |

---

## Conceitos importantes

### Função

Maior nível de agregação das áreas de atuação do setor público, como saúde, educação e defesa.

### Subfunção

Nível abaixo da função que detalha a atuação governamental.

### Programa Orçamentário

Estrutura que organiza as ações governamentais para atingir objetivos estratégicos do PPA.

### Ação Orçamentária

Operação governamental que gera bens, serviços ou transferências públicas.

### Elemento de Despesa

Identifica especificamente o objeto do gasto público, como diárias, juros, obras ou materiais.

---

Fonte: Manual Técnico do Orçamento
