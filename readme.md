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


## Estrutura do repositório (sugestão)


Se você preferir manter a estrutura atual, ignore esta seção. Caso queira padronizar:


```text
data/
  processed/        # bases tratadas (ex.: CSV consolidado)
notebooks/          # notebooks do projeto
figures/            # figuras usadas no texto
src/                # funções reutilizáveis (splines, NSS, métricas)
results/            # tabelas finais / saídas leves
Como reproduzir

Clonar o repositório

git clone https://github.com/LucZamp/TCC-ettj-pre-nss-splines.git
cd TCC-ettj-pre-nss-splines

Instalar dependências

pip install -r requirements.txt

Executar notebooks na ordem (se aplicável)

01 → 02 → 03 → 04

Autor

Lucas Domingues Zampol
Graduando em Matemática Aplicada
Universidade de São Paulo (USP)
