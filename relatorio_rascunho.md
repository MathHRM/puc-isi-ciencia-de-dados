# Análise Exploratória do Equilíbrio Fiscal Federal Brasileiro em 2025

**Matheus Henrique Resende Magalhães**

PUC Minas em Betim  
Bacharelado em Sistemas de Informação / Ciência de Dados e Big Data

matheushrm614@gmail.com

---

**\*Resumo.** O orçamento federal brasileiro estrutura-se em receitas arrecadadas e despesas executadas registradas pelo Sistema de Informações Contábeis e Fiscais do Setor Público Brasileiro (SIAFI). Este trabalho investiga o equilíbrio entre receitas e despesas federais em 2025, analisando como esses valores se distribuem entre órgãos superiores e funções de governo. Para isso, foram utilizados dois conjuntos de dados oficiais do Portal da Transparência — totalizando mais de 200 mil registros —, sobre os quais foram aplicados cinco métodos estatísticos e geradas quatro visualizações. Os resultados revelam forte correlação entre orçamento atualizado e realizado nas despesas (r = 0,9986), mas ausência de correlação nas receitas (r = 0,0162), evidenciando assimetria estrutural entre os dois lados do orçamento. A principal contribuição é uma análise quantitativa aberta e reproduzível do equilíbrio fiscal federal de 2025, identificando os órgãos e funções responsáveis pela maior concentração de gastos.\*

---

## 1. Introdução

O orçamento público federal brasileiro é o instrumento de planejamento financeiro do Estado, definindo as fontes de receita e as prioridades de gasto para cada exercício fiscal. Nos últimos anos, o debate sobre a responsabilidade fiscal ganhou centralidade na agenda política e econômica do país, impulsionado pela aprovação do arcabouço fiscal em 2023 e pelas metas de resultado primário estabelecidas para o período 2024–2026 (Tesouro Nacional 2024). A execução orçamentária — a relação entre o que foi planejado e o que efetivamente foi arrecadado ou gasto — é um indicador fundamental para avaliar a capacidade do governo de cumprir suas obrigações e implementar políticas públicas.

Apesar da abertura de dados promovida pelo Portal da Transparência do Governo Federal, poucos trabalhos analisam de forma exploratória e reproduzível o equilíbrio entre receitas e despesas no nível de granularidade disponível nos dados abertos. A questão central deste trabalho é: qual o equilíbrio entre receitas arrecadadas e despesas executadas no orçamento federal brasileiro em 2025, e como esse equilíbrio se distribui entre órgãos superiores e funções de governo?

Responder a essa pergunta é relevante porque o desequilíbrio fiscal tem consequências diretas sobre a inflação, a taxa de juros e a capacidade de investimento público. Além disso, a distribuição das despesas entre funções como Encargos Especiais, Previdência Social e Assistência Social revela as escolhas alocativas do Estado, impactando diretamente serviços essenciais à população. A análise exploratória de dados públicos pode subsidiar o controle social e qualificar o debate sobre finanças públicas.

O objetivo deste trabalho é realizar uma análise exploratória dos dados de receitas e despesas do orçamento federal de 2025, calculando métricas estatísticas descritivas e gerando visualizações que respondam à pergunta de pesquisa. Especificamente, busca-se verificar três hipóteses: (H1) as despesas apresentam alta correlação entre orçamento atualizado e realizado; (H2) as receitas apresentam baixa taxa de execução mediana em relação ao previsto; e (H3) três funções de governo concentram mais de 70% do total de despesas executadas.

Este texto está estruturado em seis seções. A seção 2 apresenta o referencial teórico sobre análise exploratória de dados, orçamento público e o Portal da Transparência. A seção 3 descreve trabalhos relacionados. A seção 4 detalha a metodologia, as etapas do trabalho e o uso de inteligência artificial generativa. A seção 5 apresenta os resultados da análise, incluindo a descrição dos dados, as estatísticas calculadas, as visualizações e a análise crítica das hipóteses. Por último, a seção 6 apresenta as conclusões e sugestões de trabalhos futuros.

---

## 2. Referencial Teórico

Esta seção descreve os principais conceitos relacionados à análise exploratória de dados, ao orçamento público federal brasileiro e à plataforma de dados abertos utilizada como fonte de dados.

### 2.1. Análise Exploratória de Dados

A análise exploratória de dados (AED), ou _Exploratory Data Analysis_ (EDA), é uma abordagem introduzida por Tukey (1977) que visa compreender a estrutura, a distribuição e os padrões presentes em conjuntos de dados antes de aplicar modelos confirmatórios. A AED utiliza métodos estatísticos descritivos — como média, mediana, desvio padrão e percentis — combinados com visualizações gráficas para revelar tendências, anomalias e relações entre variáveis. Segundo McKinney (2017), a AED é uma etapa fundamental do fluxo de trabalho em ciência de dados, pois orienta a formulação de hipóteses e a seleção de métodos analíticos adequados.

### 2.2. Orçamento Público Federal

O orçamento público federal brasileiro é regido pela Lei de Responsabilidade Fiscal (Lei Complementar n.º 101/2000) e pelo processo de elaboração da Lei Orçamentária Anual (LOA). O ciclo orçamentário compreende quatro fases: elaboração, aprovação, execução e controle (Giacomoni 2010). Na fase de execução, o orçamento inicial pode ser atualizado por suplementações ou remanejamentos, originando o orçamento atualizado. As receitas públicas, por sua vez, são classificadas por categoria econômica (correntes e de capital), origem e espécie, e sua arrecadação depende de fatores conjunturais como crescimento econômico, política tributária e nível de atividade (Rezende 2015).

### 2.3. Portal da Transparência e Dados Abertos

O Portal da Transparência do Governo Federal, mantido pela Controladoria-Geral da União (CGU), disponibiliza dados detalhados sobre a execução orçamentária federal no formato de dados abertos, em conformidade com a Lei de Acesso à Informação (Lei n.º 12.527/2011). Os dados são extraídos do SIAFI e atualizados periodicamente, abrangendo informações sobre receitas, despesas, transferências e contratos. Segundo a CGU (2025), o portal é um instrumento central de transparência ativa, permitindo que qualquer cidadão acompanhe a aplicação dos recursos públicos federais.

---

## 3. Trabalhos Relacionados

Esta seção apresenta iniciativas e trabalhos que realizaram análises sobre execução orçamentária e transparência fiscal no Brasil.

O Observatório do Orçamento, mantido pela Câmara dos Deputados, publica análises periódicas sobre a execução orçamentária federal, comparando dotações iniciais, atualizadas e realizadas por função de governo. Embora as análises sejam descritivas e orientadas ao público não técnico, elas não utilizam métodos estatísticos formais nem disponibilizam os dados tratados de forma reproduzível (Câmara dos Deputados 2024). O presente trabalho se diferencia por aplicar métodos quantitativos — correlação, percentis e desvio padrão — sobre microdados brutos, gerando um _dataset_ tratado e aberto.

O Tesouro Nacional mantém o portal Tesouro Transparente, que disponibiliza painéis interativos com dados do Resultado do Tesouro Nacional (RTN) e das estatísticas fiscais. Os dados são apresentados em nível agregado mensal, sem o detalhamento por unidade orçamentária disponível nos dados abertos do Portal da Transparência (Tesouro Nacional 2024). A análise do presente trabalho opera no nível de linha orçamentária individual, o que permite identificar assimetrias de execução entre órgãos e funções que análises agregadas ocultam.

A plataforma Brasil.io, iniciativa da sociedade civil, agrega e consolida dados públicos brasileiros, incluindo informações orçamentárias. Apesar de facilitar o acesso aos dados, a plataforma não realiza análise exploratória com métricas estatísticas nem gera visualizações orientadas a hipóteses específicas (Brasil.io 2024). Este trabalho complementa essas iniciativas ao formular e testar hipóteses quantitativas sobre o equilíbrio fiscal de 2025.

---

## 4. Metodologia

Este trabalho consiste em uma análise exploratória de dados quantitativa, conduzida sobre dados abertos oficiais do exercício fiscal de 2025. A análise foi realizada de forma computacional e reproduzível, utilizando Python como linguagem principal.

### 4.1. Etapas do Trabalho

O trabalho foi dividido nas seguintes etapas:

1. definição do problema e das hipóteses de pesquisa;
2. seleção e obtenção dos conjuntos de dados;
3. carga e limpeza dos dados;
4. análise estatística descritiva;
5. geração das visualizações;
6. exportação do _dataset_ tratado;
7. análise crítica e conclusão.

Na etapa de definição do problema, foram formuladas três hipóteses orientadas a dados, relacionando atributos específicos dos dois conjuntos de dados escolhidos. Na etapa de seleção, foram obtidos dois arquivos CSV diretamente do Portal da Transparência: `2025_Receitas.csv` e `2025_OrcamentoDespesa.csv`, referentes ao exercício 2025. Na etapa de limpeza, foram tratados os separadores de milhar (ponto), decimais (vírgula), aspas e espaços nos nomes de colunas. Nenhuma linha foi descartada por valores nulos nas colunas monetárias. As análises estatísticas e visualizações foram realizadas em um Jupyter Notebook (_analise.ipynb_), garantindo reprodutibilidade completa.

### 4.2. Ferramentas

**Tabela 1. Ferramentas do ambiente de desenvolvimento.**

| Funcionalidade              | Ferramenta                     |
| --------------------------- | ------------------------------ |
| Linguagem de análise        | Python 3.11                    |
| Manipulação de dados        | pandas 2.x                     |
| Visualizações               | matplotlib 3.x, seaborn 0.13.x |
| Ambiente de desenvolvimento | Jupyter Notebook               |
| Sistema operacional         | Ubuntu Linux 24.04             |
| Controle de versão          | Git                            |

### 4.3. Uso de Inteligência Artificial Generativa

A inteligência artificial generativa foi utilizada como ferramenta de apoio em todas as etapas do trabalho, por meio do assistente Claude (Anthropic 2024) via Claude Code CLI. Os principais usos foram: (1) brainstorming para definição do problema e das hipóteses, com técnica de _grill-me_ para identificar lacunas no planejamento; (2) elaboração do plano de implementação, com especificação de tarefas e etapas; (3) geração e revisão do código Python para carga, limpeza, análise e visualização dos dados; e (4) apoio à redação deste relatório.

Todos os prompts significativos utilizados foram documentados no arquivo `prompts.md`, em conformidade com os requisitos do trabalho. Os prompts de ajuste menor — como "corrija isso" ou "execute" — não foram documentados. A lista completa contém oito prompts documentados, cobrindo as etapas de planejamento, seleção de _dataset_, definição do problema, seleção de métodos estatísticos e visualizações, escolha de ferramentas, estratégia de implementação e decisão sobre o formato do relatório.

---

## 5. Resultados e Análise

### 5.1. Base de Dados

Os dados utilizados neste trabalho foram obtidos do Portal da Transparência do Governo Federal (CGU 2025a; CGU 2025b), referentes ao exercício fiscal de 2025, com acesso em maio de 2025.

**Tabela 2. Caracterização dos conjuntos de dados.**

| Atributo        | Receitas            | Despesas                    |
| --------------- | ------------------- | --------------------------- |
| Nome do arquivo | `2025_Receitas.csv` | `2025_OrcamentoDespesa.csv` |
| Linhas          | 175.278             | 26.160                      |
| Colunas         | 16                  | 26                          |
| Codificação     | ISO-8859-1          | ISO-8859-1                  |
| Separador       | `;`                 | `;`                         |
| Período         | Jan–Out 2025        | Jan–Out 2025                |

O conjunto de receitas contém registros de lançamento de receitas públicas federais, com campos para identificação do órgão superior, unidade gestora, categoria econômica, origem, espécie e valores previstos, lançados e realizados. O conjunto de despesas contém registros de execução orçamentária por linha de ação, com campos para identificação do órgão superior, função, subfunção, programa, ação, grupo de despesa e valores de orçamento inicial, atualizado, empenhado e realizado.

**Tabela 3. Principais campos — Receitas.**

| Campo                     | Tipo     | Descrição                             |
| ------------------------- | -------- | ------------------------------------- |
| NOME ÓRGÃO SUPERIOR       | Texto    | Órgão responsável pela arrecadação    |
| CATEGORIA ECONÔMICA       | Texto    | Receitas Correntes ou de Capital      |
| VALOR PREVISTO ATUALIZADO | Numérico | Meta de arrecadação atualizada (R$)   |
| VALOR REALIZADO           | Numérico | Valor efetivamente arrecadado (R$)    |
| PERCENTUAL REALIZADO      | Numérico | % do previsto efetivamente arrecadado |

**Tabela 4. Principais campos — Despesas.**

| Campo                     | Tipo     | Descrição                                |
| ------------------------- | -------- | ---------------------------------------- |
| NOME ÓRGÃO SUPERIOR       | Texto    | Órgão executor da despesa                |
| NOME FUNÇÃO               | Texto    | Função de governo (ex.: Saúde, Educação) |
| ORÇAMENTO INICIAL (R$)    | Numérico | Dotação original aprovada na LOA         |
| ORÇAMENTO ATUALIZADO (R$) | Numérico | Dotação após suplementações              |
| ORÇAMENTO REALIZADO (R$)  | Numérico | Valor efetivamente pago                  |
| % REALIZADO DO ORÇAMENTO  | Numérico | % do atualizado efetivamente executado   |

### 5.2. Análise Estatística

Foram aplicados cinco métodos estatísticos aos dados: média, mediana, desvio padrão, percentis (P25/P75/P90) e correlação de Pearson.

**Média.** A média do valor realizado de receitas é R$ 32,03 milhões por linha de registro, contra uma média de orçamento realizado de R$ 185,46 milhões nas despesas. A diferença de escala reflete a maior granularidade do arquivo de receitas — 175 mil linhas contra 26 mil —, não necessariamente maior volume unitário.

**Mediana.** A mediana global da taxa de execução das receitas é **0,00%**, indicando que mais da metade dos registros apresenta previsão zero ou arrecadação irrisória em relação ao previsto. Nas despesas, a mediana global da taxa de execução é **76,97%**, sinalizando que a maioria das linhas orçamentárias tem execução superior a três quartos do previsto.

**Desvio padrão.** O desvio padrão da taxa de execução das despesas varia significativamente entre órgãos: o Ministério da Defesa apresenta o maior valor (σ = 3.765,92), resultado de linhas com execução acima de 100% (suplementações), enquanto a maioria dos ministérios apresenta desvio entre 42 e 44 pontos percentuais, indicando dispersão moderada e relativamente homogênea entre órgãos.

**Percentis.** Para as despesas, o P25 da taxa de execução é **0,00%**, o P75 é **95,28%** e o P90 é **100,00%**. A amplitude entre P25 e P75 (95,28 p.p.) é extremamente alta, revelando bimodalidade: uma parcela relevante das linhas não foi executada (P25 = 0%), enquanto outra parcela atingiu execução próxima a 100% (P75 e P90). Para as receitas, todos os percentis (P25, P75 e P90) são **0,00%** em todos os órgãos analisados, confirmando que a arrecadação é altamente concentrada em poucas linhas com previsto não nulo.

**Correlação de Pearson.** A correlação entre orçamento atualizado e realizado nas despesas é **r = 0,9986**, indicando relação linear quase perfeita: o quanto foi planejado (após atualizações) determina fortemente o quanto foi pago. Nas receitas, a correlação entre previsto e realizado é **r = 0,0162**, praticamente inexistente, sugerindo que a arrecadação efetiva é independente das metas previstas — possivelmente porque grande parte dos registros tem previsão zerada e arrecadação variável.

**Tabela 5. Resumo das métricas estatísticas.**

| Métrica                           | Receitas   | Despesas   |
| --------------------------------- | ---------- | ---------- |
| Mediana de execução (global)      | 0,00%      | 76,97%     |
| Média de execução (global)        | 0,00%      | 63,24%     |
| P25 de execução                   | 0,00%      | 0,00%      |
| P75 de execução                   | 0,00%      | 95,28%     |
| P90 de execução                   | 0,00%      | 100,00%    |
| Correlação previsto vs. realizado | r = 0,0162 | r = 0,9986 |

### 5.3. Visualizações

**Figura 1 — Top 10 Órgãos Superiores por Despesa Executada (2025)**

_[INSERIR: graficos/top10_orgaos_despesa.png]_

O gráfico de barras horizontais exibe os dez órgãos superiores com maior volume de despesa executada em 2025. O Ministério da Fazenda lidera amplamente, com valores na casa dos trilhões de reais, reflexo dos encargos da dívida pública e das transferências de benefícios previdenciários. O gráfico evidencia a alta concentração dos gastos federais: os três primeiros órgãos respondem por parcela expressiva do total executado.

**Figura 2 — Receita Prevista vs. Realizada por Categoria Econômica (2025)**

_[INSERIR: graficos/receita_prevista_realizada.png]_

O gráfico de barras agrupadas compara, para cada categoria econômica, os valores previstos e realizados de receita. As Receitas Correntes apresentaram previsão de R$ 3,00 trilhões e realização de R$ 2,88 trilhões. As Receitas de Capital, por sua vez, registraram previsão de R$ 2,69 trilhões e realização de R$ 2,71 trilhões — ligeiramente acima do previsto. As categorias intra-orçamentárias e "Sem informação" apresentam valores próximos a zero no eixo da escala trilionária.

**Figura 3 — Orçamento Inicial vs. Realizado por Função de Governo (2025)**

_[INSERIR: graficos/scatter_orcamento_executado.png]_

O gráfico de dispersão relaciona o orçamento inicial e o orçamento realizado para cada função de governo. A linha tracejada em vermelho indica o cenário ideal de execução integral. A maioria das funções se distribui abaixo da diagonal, indicando execução inferior ao inicial. As funções Encargos Especiais (R$ 2,81 trilhões realizados), Previdência Social (R$ 1,02 trilhão) e Assistência Social (R$ 283,8 bilhões) destacam-se como _outliers_ de volume, distantes das demais.

**Figura 4 — Taxa de Execução Orçamentária por Órgão Superior — Top 20 (2025)**

_[INSERIR: graficos/execucao_por_orgao.png]_

O gráfico de barras horizontais coloridas exibe a mediana da taxa de execução por órgão superior, com codificação de cor: verde (≥ 75%), laranja (50–74%) e vermelho (< 50%). O Ministério das Relações Exteriores lidera com 91,67%, seguido pelo Ministério do Desenvolvimento, Indústria, Comércio e Serviços (86,17%) e pelo Ministério da Educação (85,68%). O Ministério da Justiça e Segurança Pública apresenta a menor mediana entre os vinte analisados (63,65%), ainda acima do limiar de 50%.

### 5.4. Análise Crítica das Hipóteses

**H1 — As despesas apresentam alta correlação (r > 0,9) entre orçamento atualizado e realizado.**

Confirmada. A correlação de Pearson calculada foi r = 0,9986, superando amplamente o limiar de 0,9 estabelecido na hipótese. Esse resultado indica que o processo de planejamento orçamentário das despesas federais é altamente eficaz: o orçamento atualizado — que incorpora suplementações e remanejamentos ao longo do exercício — é um preditor quase perfeito do que efetivamente será pago. Isso sugere que os mecanismos de controle e acompanhamento orçamentário do Executivo Federal funcionam de forma consistente para as despesas.

**H2 — As receitas apresentam baixa taxa de execução mediana (< 10%) em relação ao previsto.**

Confirmada. A mediana global da taxa de execução das receitas foi exatamente 0,00%, valor muito abaixo do limiar de 10% estabelecido. Esse resultado, aparentemente paradoxal, tem explicação metodológica: a grande maioria dos registros do arquivo de receitas apresenta previsão igual a zero (pois grande parte das linhas de arrecadação não possuem meta formal registrada no SIAFI), de modo que o campo `PERCENTUAL REALIZADO` é calculado como zero mesmo quando há arrecadação. Isso não significa ausência de receitas, mas reflete uma característica de como as metas de arrecadação são registradas no sistema.

**H3 — Três funções de governo concentram mais de 70% do total de despesas executadas.**

Confirmada. As três funções com maior despesa realizada são Encargos Especiais (R$ 2,811 trilhões), Previdência Social (R$ 1,016 trilhão) e Assistência Social (R$ 283,8 bilhões), totalizando R$ 4,111 trilhões. O total geral de despesas realizadas no arquivo é de aproximadamente R$ 4,85 trilhões, portanto as três funções concentram cerca de **84,8%** do total — confirmando amplamente a hipótese. Essa concentração reflete a estrutura do Estado brasileiro, em que encargos da dívida, aposentadorias e benefícios sociais dominam o orçamento federal.

---

## 6. Conclusão

Este trabalho realizou uma análise exploratória dos dados de receitas arrecadadas e despesas executadas do orçamento federal brasileiro no exercício de 2025, utilizando dois conjuntos de dados abertos do Portal da Transparência com mais de 200 mil registros combinados. As três hipóteses formuladas foram confirmadas pelos dados: as despesas apresentam correlação quase perfeita entre planejado e executado (r = 0,9986), as receitas têm taxa de execução mediana igual a zero, e três funções de governo concentram 84,8% das despesas federais.

Os resultados revelam uma assimetria estrutural entre os dois lados do orçamento. No lado das despesas, o alto coeficiente de correlação (r = 0,9986) é evidência de planejamento eficiente — o que foi revisto e atualizado ao longo do ano é praticamente tudo o que é pago. No lado das receitas, a ausência de correlação (r = 0,0162) e a mediana de execução igual a zero refletem uma limitação do dado bruto: a maioria das linhas não tem meta prevista registrada no SIAFI, impossibilitando o cálculo do percentual realizado. Para análises futuras sobre equilíbrio fiscal, recomenda-se filtrar apenas as linhas com previsto positivo antes de calcular taxas de execução de receitas.

A principal contribuição deste trabalho é a construção de um pipeline de análise exploratória reproduzível — em Python, com código aberto — sobre microdados orçamentários federais de 2025, incluindo um _dataset_ tratado e consolidado por órgão superior (`dataset_tratado.csv`) com saldo fiscal calculado. A utilização de inteligência artificial generativa como ferramenta de apoio em todas as etapas — documentada em `prompts.md` — demonstra o potencial dessas ferramentas para acelerar e qualificar a análise de dados públicos.

Como trabalhos futuros, sugere-se ampliar a análise para séries temporais (comparando execuções de 2022 a 2025), incluir dados de transferências a estados e municípios, e aplicar técnicas de clusterização para agrupar órgãos por perfil de execução. Seria também valioso integrar dados de indicadores socioeconômicos para correlacionar execução orçamentária por função com resultados de saúde, educação e segurança pública.

---

## Referências Bibliográficas

Anthropic, (2024) "Claude — AI Assistant", https://www.anthropic.com/claude, Acesso em maio de 2025.

Brasil.io, (2024) "Brasil.io — Dados abertos acessíveis", https://brasil.io, Acesso em maio de 2025.

Câmara dos Deputados, (2024) "Observatório do Orçamento", https://www2.camara.leg.br/orcamento-da-uniao, Acesso em maio de 2025.

CGU — Controladoria-Geral da União, (2025a) "Orçamento de Despesas — Download de Dados", https://portaldatransparencia.gov.br/download-de-dados/orcamento-despesa, Acesso em maio de 2025.

CGU — Controladoria-Geral da União, (2025b) "Receitas — Download de Dados", https://portaldatransparencia.gov.br/download-de-dados/receitas, Acesso em maio de 2025.

Giacomoni, J., (2010) "Orçamento Público", 15ª ed., São Paulo, Atlas.

McKinney, W., (2017) "Python for Data Analysis: Data Wrangling with Pandas, NumPy, and IPython", 2ª ed., Sebastopol, O'Reilly Media.

Rezende, F., (2015) "Finanças Públicas", 3ª ed., São Paulo, Atlas.

Tesouro Nacional, (2024) "Tesouro Transparente — Resultado do Tesouro Nacional", https://www.tesourotransparente.gov.br, Acesso em maio de 2025.

Tukey, J. W., (1977) "Exploratory Data Analysis", Reading, Addison-Wesley.
