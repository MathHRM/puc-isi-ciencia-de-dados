# Dicionário de Dados — Execução da Receita

| Coluna                 | Descrição                                                                 |
| ---------------------- | ------------------------------------------------------------------------- |
| Código Órgão Superior  | Código do Órgão Superior responsável pela receita                         |
| Nome Órgão Superior    | Nome do Órgão Superior responsável pela receita                           |
| Código Órgão           | Código do Órgão Subordinado responsável pela receita                      |
| Nome Órgão             | Nome do Órgão Subordinado responsável pela receita                        |
| Código Unidade Gestora | Código da Unidade Gestora responsável pela receita                        |
| Nome Unidade Gestora   | Nome da Unidade Gestora responsável pela receita                          |
| Categoria Econômica    | Classificação da receita em “Receitas Correntes” ou “Receitas de Capital” |
| Origem                 | Detalhamento da categoria econômica da receita                            |
| Espécie                | Qualificação detalhada do fato gerador da receita                         |
| Detalhamento           | Detalhamento adicional da classificação da receita                        |
| Valor Previsto         | Valor previsto da receita atualizado                                      |
| Valor Lançado          | Valor da receita lançada                                                  |
| Valor Realizado        | Valor efetivamente arrecadado/recolhido                                   |
| Percentual Realizado   | `(Valor Realizado / Valor Previsto) * 100`                                |
| Data Lançamento        | Data de lançamento da receita                                             |
| Exercício              | Ano de referência dos valores                                             |

---

## Unidade Gestora (UG)

Unidade Orçamentária ou Administrativa que realiza atos de gestão orçamentária, financeira e/ou patrimonial, cujo responsável está sujeito à prestação de contas anual.

Base legal:

- Decreto-Lei nº 200, de 25 de fevereiro de 1967.

---

# Categoria Econômica

## Receitas Correntes

Receitas destinadas ao financiamento das atividades do Estado e manutenção dos serviços públicos.

### Origens das Receitas Correntes

| Código | Origem                                      |
| ------ | ------------------------------------------- |
| 1      | Impostos, Taxas e Contribuições de Melhoria |
| 2      | Contribuições                               |
| 3      | Receita Patrimonial                         |
| 4      | Receita Agropecuária                        |
| 5      | Receita Industrial                          |
| 6      | Receita de Serviços                         |
| 7      | Transferências Correntes                    |

---

## Receitas de Capital

Receitas relacionadas à obtenção de recursos para investimentos e financiamento de despesas de capital.

### Origens das Receitas de Capital

| Código | Origem                     |
| ------ | -------------------------- |
| 1      | Operações de Crédito       |
| 2      | Alienação de Bens          |
| 3      | Amortização de Empréstimos |
| 4      | Transferências de Capital  |
| 9      | Outras Receitas de Capital |

---

# Espécie

Nível de classificação vinculado à origem da receita, permitindo detalhar melhor o fato gerador.

Exemplo:
Dentro da origem **“Contribuições”**, existem espécies como:

- Contribuições Sociais
- Contribuições Econômicas
- Contribuições para Entidades Privadas de Serviço Social e Formação Profissional

---

# Valor Realizado

Corresponde ao valor arrecadado/recolhido da receita.

## Arrecadação

Fase em que os contribuintes entregam os valores devidos ao Governo por meio de agentes arrecadadores ou bancos autorizados.

## Recolhimento

Transferência dos valores arrecadados aos cofres públicos e à Conta Única do Tesouro Nacional.

---

# Percentual Realizado

```text
(Valor Realizado / Valor Previsto) * 100
```
