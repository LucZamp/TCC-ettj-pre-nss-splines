# Modelagem da Estrutura a Termo da Taxa de Juros Pré (ETTJ-Pré)


Este repositório contém código, dados tratados e experimentos desenvolvidos no Trabalho de Conclusão de Curso (TCC), cujo objetivo é modelar e analisar a Estrutura a Termo da Taxa de Juros Pré-fixada no Brasil (ETTJ-Pré) a partir de dados de mercado.


O trabalho compara métodos paramétricos e não paramétricos de ajuste de curva, avaliando estabilidade, capacidade de interpolação e desempenho fora da amostra, com foco em aplicações em precificação, gestão de risco e análise econômica.


## Objetivos


- Construir a ETTJ-Pré a partir de curvas observadas
- Comparar métodos paramétricos e não paramétricos de ajuste
- Avaliar sensibilidade ao número/escolha de pontos de ajuste
- Analisar comportamento médio e variabilidade temporal
- Validar por treino/teste, métricas de erro e simulações de Monte Carlo


## Base de dados


- Fonte: Curva Pré disponibilizada diariamente pelo mercado brasileiro
- Período: 01/10/2025 a 30/12/2025
- Eixo de vencimentos: dias úteis (DU)
- Taxas: anualizadas no critério 252 dias


Para garantir comparabilidade ao longo do tempo, utiliza-se um conjunto fixo de 41 pontos de vencimento (DU). Quando um DU exato não está disponível em uma curva diária, utiliza-se o ponto mais próximo (aproximação prática comum em aplicações de mercado).


## Metodologia


### Métodos de ajuste de curva


1. Spline Cúbico Natural  
   Ajuste não paramétrico com suavidade até a segunda derivada.


2. B-Spline Cúbica (Base + Ridge)  
   Variante robusta de B-splines com regularização para estabilidade numérica.


3. Nelson–Siegel–Svensson (NSS)  
   Modelo paramétrico estimado via L-BFGS-B.


4. GA → NSS + L-BFGS-B  
   Estratégia híbrida: Algoritmo Genético para busca global inicial + refinamento local com L-BFGS-B, reduzindo risco de mínimos locais.


## Experimentos


### 1) Análise descritiva
- Construção das curvas diárias no período
- Estatísticas por ponto (DU): média, desvio padrão, mínimo e máximo


### 2) Visualização
- Curvas diárias (uma curva por dia)
- Curvas médias mensais (outubro, novembro e dezembro de 2025)
- Curva média total do período


### 3) Validação fora da amostra
- Divisão treino/teste 70% / 30% por data
- Em cada dia:
  - usar os 41 pontos para ajustar a curva
  - comparar a curva ajustada com todos os pontos observados naquele dia
- Métricas: RMSE, MAE, MAPE


### 4) Simulações de Monte Carlo
- Adição de ruído (em basis points) aos pontos de ajuste
- Repetição por múltiplas simulações
- Avaliação de robustez dos métodos frente a perturbações


## Reprodução dos resultados

Os experimentos apresentados no trabalho podem ser reproduzidos a partir dos notebooks e scripts disponibilizados neste repositório. A sequência geral adotada no estudo é descrita a seguir.

### 1. Preparação dos dados

Os arquivos diários da curva pré-fixada são organizados e consolidados em uma base única, contendo:
- data da curva;
- eixo de vencimentos em dias úteis (DU);
- taxas anualizadas no critério 252 dias.

Esse procedimento resulta em uma base balanceada, utilizada em todas as etapas posteriores da análise.

### 2. Seleção dos pontos de ajuste

Define-se um conjunto fixo de 41 pontos de vencimento (DU), comum a todas as datas da amostra.  
Quando um vencimento exato não está disponível em uma curva diária, utiliza-se o ponto mais próximo observado naquele dia.

### 3. Construção e análise das curvas

A partir da base consolidada:
- são geradas as curvas diárias;
- calculam-se estatísticas descritivas por ponto (média, desvio padrão, mínimo e máximo);
- constroem-se curvas médias mensais e a curva média total do período.

As figuras correspondentes encontram-se na pasta `03 - Imagens`.

### 4. Validação fora da amostra

Para cada data:
- utiliza-se o conjunto de 41 pontos para ajustar a curva pelos métodos estudados;
- a curva estimada é comparada com todos os pontos observados naquele dia.

A validação é conduzida por meio de uma divisão treino/teste (70% / 30%) e avaliada com métricas de erro, como RMSE, MAE e MAPE.

### 5. Simulações de Monte Carlo

Com o objetivo de avaliar a robustez dos métodos:
- adiciona-se ruído (em basis points) aos pontos de ajuste;
- repete-se o procedimento por múltiplas simulações;
- analisam-se a distribuição e a variabilidade dos resultados gerados.

---

## Organização do repositório

A estrutura do repositório está organizada da seguinte forma:

```text
01 - Data/
  ├─ Arquivos antes de virarem um CSV/
  └─ curva_pre_20251001_20251230.csv

02 - Notebooks/
  ├─ Transformação e consolidação dos dados
  ├─ Estatísticas descritivas
  ├─ Visualizações
  └─ Experimentos e validações

03 - Imagens/
  └─ Figuras utilizadas no trabalho
