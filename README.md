#  Estimação da Estrutura a Termo da Taxa de Juros Pré no Brasil
### Modelos Paramétricos (NSS) e Não Paramétricos (Splines)


Este repositório contém todo o código, dados e resultados utilizados no Trabalho de Conclusão de Curso:


> **Análise Comparativa de Metodologias para a Estimação da Estrutura a Termo da Taxa de Juros Pré no Mercado Brasileiro**


O trabalho investiga e compara métodos **paramétricos** (Nelson-Siegel e Nelson-Siegel-Svensson) e **não paramétricos** (splines cúbicas e B-splines) aplicados à curva de juros brasileira, com foco em:
- qualidade de ajuste,
- estabilidade temporal,
- robustez sob ruído,
- comportamento em **interpolação e extrapolação**.


---


##  Objetivos do Projeto


- Implementar e comparar modelos NSS e Splines para estimação da ETTJ Pré.
- Avaliar desempenho **in-sample** e **fora da região de calibração**.
- Investigar estabilidade ao longo do tempo via métricas agregadas.
- Analisar o comportamento dos métodos em extrapolações de longo prazo.
- Garantir **reprodutibilidade completa** dos resultados apresentados no trabalho.


---


##  Metodologias Implementadas


- **Spline Cúbico Natural**
- **B-Spline com regularização (Ridge)**
- **Nelson-Siegel-Svensson (NSS) via L-BFGS-B**
- **Abordagem híbrida: Algoritmo Genético → NSS + L-BFGS-B**
- Simulações de **Monte Carlo** com ruído nos pontos de ajuste
- Avaliação em:
- interpolação,
- extrapolação (DU > 3465),
- métricas diárias e agregadas no tempo


---


##  Estrutura do Repositório


A organização do projeto segue uma lógica **sequencial por etapas**, numeradas para refletir o fluxo metodológico do trabalho.
```
.
├── 01_Data/
│ ├── data/
│
├── 02_Notebooks/
│ ├── 01_Confecção_da_Base/
│ ├── 02_Estatistica_Descritiva_da_Base/
│ ├── 03_Curvas_Base/
│ ├── 04_Experimento_Testando_os_Métodos/
│ ├── 05_interpolacao/
│ ├── 06_extrapolacao/
│
├── 03_Imagens/
│ ├── Imagens_Geradas_nos_Códigos/
│
├── 04_resultados/
│ ├── 01_Base_Principal/
│ ├── 02_Testando_os_Métodos/
│ ├── 03_analise_descritiva/
│ ├── 04_interpolacao/
│ ├── 05_extrapolacao/
│
├── README.md
└── requirements.txt
```

###  Convenção Importante

- **Pastas numeradas (01, 02, 03, …)** representam **etapas do experimento**
- **Notebooks também são numerados** e seguem a mesma lógica
- A relação entre códigos, dados e resultados é **flexível**, mas consistente:
  - um código pode usar **mais de uma base**,
  - uma base pode ser usada por **mais de um código**,
  - nem todo código consome dados,
  - nem todo código gera resultados finais.

---

##  Fluxo Geral do Projeto

A ideia central é:

> **Etapas iniciais alimentam etapas intermediárias, que por sua vez geram os resultados finais**

Exemplo prático:

- Um notebook `02_xxx.ipynb`:
  - consome dados localizados em  
    `02_construcao_curvas/data/`
- Seus resultados (quando aplicável) são salvos em:
  - `04_resultados/02_interpolacao/`  
  ou  
  - `04_resultados/03_extrapolacao/`

Essa separação facilita:
- rastreabilidade,
- organização dos experimentos,
- reprodução parcial ou total do trabalho.

---

## ▶️ Como Utilizar o Repositório

### 1️⃣ Clonar o repositório

```bash
git clone https://github.com/LucZamp/TCC-ettj-pre-nss-splines.git
cd TCC-ettj-pre-nss-splines
````

### 2️⃣ Criar ambiente Python

Recomendado Python ≥ 3.9.

```
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows
pip install -r requirements.txt
```

### 3️⃣ Executar os notebooks

Os notebooks estão organizados de forma **sequencial**, refletindo as etapas metodológicas do trabalho.

Para reproduzir **todo o pipeline**, recomenda-se executar os notebooks **na ordem numérica**, respeitando a estrutura de pastas.

Caso o objetivo seja reproduzir apenas uma etapa específica (por exemplo, extrapolação):

- acesse diretamente a pasta correspondente;
- verifique quais arquivos estão disponíveis na subpasta `data/`;
- execute apenas os notebooks necessários para aquela análise.

#### Observações importantes

- Nem todos os notebooks consomem dados de entrada;
- Nem todos os notebooks geram arquivos de saída;
- Um mesmo conjunto de dados pode ser utilizado por mais de um notebook;
- Os resultados finais consolidados encontram-se na pasta `04_resultados/`.

Essa organização permite tanto a reprodução completa do trabalho quanto a execução isolada de experimentos específicos.


---

## Resultados e Conclusões

### Resultados

Os experimentos realizados permitiram avaliar de forma sistemática o desempenho dos métodos de estimação da Estrutura a Termo da Taxa de Juros Pré ao longo de toda a base analisada. Os principais resultados obtidos incluem:

- Construção de **curvas médias globais** para cada metodologia;
- Avaliação quantitativa por meio de métricas consolidadas:
  - Root Mean Squared Error (RMSE);
  - Mean Absolute Error (MAE);
  - viés médio em vencimentos específicos da curva;
- Análise da **dispersão temporal dos erros**, permitindo avaliar estabilidade ao longo do tempo;
- Estudos específicos de:
  - **Interpolação**, considerando apenas os pontos utilizados na calibração;
  - **Extrapolação**, avaliando o comportamento dos métodos além do último vencimento de ajuste
    (DU = 3717, 3969 e 5000);
- Geração de **tabelas e gráficos consolidados**, utilizados diretamente na análise empírica do trabalho.

---

### Conclusões

Os resultados mostram que diferentes métodos apresentam vantagens distintas a depender da região da curva analisada e do objetivo da aplicação.

De forma geral, os métodos baseados em **splines** apresentaram melhor desempenho na interpolação, com elevada aderência aos dados observados e baixos erros médios. Por outro lado, os modelos **paramétricos**, em especial o Nelson-Siegel-Svensson, apresentaram comportamento mais estável em extrapolações de médio e longo prazo, produzindo curvas mais suaves além da região de calibração.

A abordagem híbrida baseada na combinação de **Algoritmo Genético e otimização local** contribuiu para reduzir a sensibilidade a condições iniciais e melhorar a robustez do ajuste paramétrico. Assim, os resultados indicam que a escolha do método deve considerar o horizonte de vencimentos de interesse e o contexto de uso, não existindo uma metodologia dominante em todos os cenários.

---

## Reprodutibilidade

Todo o pipeline do trabalho — desde o tratamento das bases de dados até a geração dos resultados finais — está documentado e versionado neste repositório. A organização por etapas numeradas e a separação entre dados, códigos e resultados permitem:

- a **reprodução integral** dos experimentos apresentados;
- a execução isolada de etapas específicas, como interpolação ou extrapolação;
- a extensão do projeto para novos métodos, bases de dados ou períodos de análise.

Essa estrutura facilita a rastreabilidade dos experimentos e o reaproveitamento do código em estudos futuros.

---

## Autor

**Lucas Domingues Zampol**  
Instituto de Matemática e Estatística – USP  
Curso de Matemática Aplicada e Computacional  

🔗 GitHub: https://github.com/LucZamp

---

## Licença

Este projeto é disponibilizado exclusivamente para fins acadêmicos e educacionais.
