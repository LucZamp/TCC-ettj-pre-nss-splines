Modelagem da Estrutura a Termo da Taxa de Juros Pré (ETTJ-Pré)

Este repositório contém o código, os dados tratados e os experimentos desenvolvidos no Trabalho de Conclusão de Curso (TCC), cujo objetivo é a modelagem e análise da Estrutura a Termo da Taxa de Juros Pré-fixada no Brasil (ETTJ-Pré) a partir de dados de mercado.

O trabalho investiga diferentes métodos de ajuste de curvas de juros, avaliando sua estabilidade, capacidade de interpolação e desempenho fora da amostra, com foco em aplicações práticas em precificação, gestão de risco e análise econômica.

Objetivos do trabalho

Os principais objetivos deste estudo são:

Construir a ETTJ-Pré a partir de dados observados de mercado

Comparar métodos paramétricos e não paramétricos de ajuste de curva

Avaliar a sensibilidade dos métodos ao número e à escolha dos pontos de ajuste

Analisar o comportamento médio e a variabilidade temporal da curva

Validar os métodos por meio de divisão treino/teste, métricas de erro e simulações de Monte Carlo

Base de dados

Fonte: Curva Pré disponibilizada diariamente pelo mercado brasileiro

Período analisado:

01/10/2025 a 30/12/2025

Unidade temporal: dias úteis (DU)

Taxas: anualizadas no critério de 252 dias

Para garantir comparabilidade ao longo do tempo, foi definido um conjunto fixo de 41 pontos de vencimento (DU), utilizado de forma consistente em todos os experimentos.

Quando um ponto de vencimento específico não estava disponível em uma curva diária, utilizou-se o ponto mais próximo, prática comum na literatura aplicada.

Metodologia
Métodos de ajuste de curva

Foram avaliados os seguintes métodos, todos implementados em Python:

Spline Cúbico Natural
Ajuste não paramétrico com suavidade garantida até a segunda derivada.

B-Spline Cúbica (Base + Ridge)
Variante robusta de B-splines com regularização, utilizada para evitar instabilidades numéricas.

Nelson–Siegel–Svensson (NSS)
Modelo paramétrico amplamente utilizado por bancos centrais, estimado via L-BFGS-B.

GA → NSS + L-BFGS-B
Estratégia híbrida que utiliza um Algoritmo Genético para busca global inicial, seguido de refinamento local com L-BFGS-B, reduzindo a probabilidade de convergência para mínimos locais.

Todos os métodos seguem a mesma filosofia de implementação, garantindo comparabilidade direta entre os resultados.

Experimentos realizados
1. Análise descritiva da curva

Construção das curvas diárias ao longo do período analisado

Cálculo, para cada ponto de vencimento, das estatísticas:

média

desvio padrão

mínimo

máximo

2. Visualização

Curvas diárias (uma curva por dia)

Curvas médias mensais (outubro, novembro e dezembro de 2025)

Curva média total do período analisado

3. Validação fora da amostra

Divisão treino/teste em proporção 70% / 30%, realizada por data

Em cada dia:

os 41 pontos são utilizados para ajustar a curva

a curva ajustada é comparada com todos os pontos observados naquele dia

Métricas de avaliação:

RMSE

MAE

MAPE

4. Simulações de Monte Carlo

Adição de ruído aos pontos de ajuste, em basis points

Repetição do processo por múltiplas simulações

Avaliação da robustez dos métodos frente a perturbações nos dados

Estrutura do repositório
ettj-pre/
├── data/
│   ├── raw/            # dados brutos (opcional ou amostral)
│   ├── processed/      # bases tratadas
│   └── du_points/      # pontos fixos de vencimento (DU)
│
├── notebooks/
│   ├── 01_extracao_dados.ipynb
│   ├── 02_analise_descritiva.ipynb
│   ├── 03_visualizacao_curvas.ipynb
│   └── 04_validacao_treino_teste.ipynb
│
├── src/
│   ├── splines.py      # métodos baseados em splines
│   ├── nss.py          # NSS e GA→NSS
│   └── metrics.py     # métricas de erro
│
├── figures/
│   ├── todas_curvas.png
│   ├── curvas_media_mensal.png
│   └── curva_media_total.png
│
├── results/
│   ├── tabelas_metricas.csv
│   └── simulacoes_monte_carlo.csv
│
├── README.md
└── requirements.txt
Tecnologias utilizadas

Python

pandas

numpy

scipy

matplotlib

jupyter

Como reproduzir os resultados

Clonar o repositório:

git clone https://github.com/seu-usuario/ettj-pre.git
cd ettj-pre

Instalar as dependências:

pip install -r requirements.txt

Executar os notebooks na ordem:

01 → 02 → 03 → 04
Observações finais

Este repositório possui caráter acadêmico e educacional.
Os resultados obtidos dependem do período analisado, da escolha dos pontos de ajuste e das hipóteses adotadas em cada método.

O trabalho busca equilibrar rigor matemático, interpretação econômica e aplicabilidade prática, aproximando teoria e uso real em mercado financeiro.

Autor

Lucas Domingues Zampol
Graduando em Matemática Aplicada
Universidade de São Paulo (USP)