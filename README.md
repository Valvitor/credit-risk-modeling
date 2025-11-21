# 📊 Credit Risk Ecosystem: Do Micro ao Macro

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge&logo=python)
![Status](https://img.shields.io/badge/Status-Portfólio_Completo-success?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Finanças_Quantitativas-red?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

> **Business Challenge:** O gerenciamento de risco de crédito exige uma abordagem holística. Este repositório consolida soluções para duas frentes críticas:
> 1.  **Micro:** Mitigação de Seleção Adversa na concessão individual (**Credit Scoring**).
> 2.  **Macro:** Calibragem de Risco Sistêmico e Provisões (**Forecasting de Inadimplência**).

---

## 📂 Estrutura do Portfólio

| Módulo | Foco | Técnica | Target (Alvo) |
| :--- | :--- | :--- | :--- |
| **1. Credit Scoring** | **Micro** (Cliente) | Random Forest | Probabilidade de Default Individual |
| **2. Macro Forecast** | **Macro** (Mercado) | SARIMA | Taxa de Inadimplência do Sistema (Séries Temporais) |

### 🌳 Organização de Arquivos
```text
credit-risk-modeling/
├── data/                # Dados brutos (Home Credit & BACEN via API)
├── images/              # Resultados gráficos e visuais
├── notebooks/           # Jupyter Notebooks
│   ├── 1.0-mvp-modelagem-credito.ipynb    # Projeto 1 (Scoring)
│   └── 2.0-forecast-inadimplencia.ipynb   # Projeto 2 (Séries Temporais)
├── requirements.txt     # Dependências do projeto
└── README.md            # Documentação Executiva
````

-----

# 🏢 Projeto 1: Credit Scoring (Micro)

**Objetivo:** Desenvolver um classificador capaz de ordenar proponentes por risco, maximizando o retorno ajustado ao risco (RAROC) e reduzindo a assimetria de informação.

### 🧠 Engenharia de Atributos (Economic Feature Engineering)

Diferencial do projeto: A seleção de features não foi puramente estatística, mas fundamentada em hipóteses econômicas de **Solvência** e **Liquidez**.

| Variável Derivada | Fórmula (Proxy) | Hipótese Econômica |
| :--- | :--- | :--- |
| **Alavancagem** | $$\text{DTI} \approx \frac{\text{Valor do Crédito}}{\text{Renda Anual}}$$ | Clientes alavancados muito acima de sua geração de caixa anual apresentam risco exponencial de insolvência. |
| **Esforço Mensal** | $$\text{Liquidez} = \frac{\text{Valor da Parcela}}{\text{Renda Anual}}$$ | Mede a pressão no fluxo de caixa. Parcelas que consomem grande fatia da renda aumentam a sensibilidade a choques exógenos. |
| **Ciclo de Vida** | $$\text{Estabilidade} = \frac{\text{Tempo Emprego}}{\text{Idade}}$$ | Baseado na *Life-cycle hypothesis*: estabilidade profissional relativa à idade indica menor volatilidade de renda futura. |

### 📈 Resultados (MVP)

O modelo (Random Forest com balanceamento) atingiu um **ROC AUC de 0.7151** na base de teste (Holdout 30%).

#### Capacidade de Discriminação e Drivers de Risco

\<p float="left"\>
\<img src="images/roc_curve.png" width="48%" /\>
\<img src="images/feature_importance.png" width="48%" /\>
\</p\>

**Insight de Negócio:** O gráfico de *Feature Importance* (direita) valida a hipótese econômica: a variável criada **`DAYS_EMPLOYED_PERCENT`** (Estabilidade) provou-se um dos maiores preditores de adimplência, superando variáveis brutas de renda.

#### Matriz de Confusão (Threshold 0.5)

\<img src="images/confusion_matrix.png" width="60%" /\>

> *Código Fonte:* [`notebooks/1.0-mvp-modelagem-credito.ipynb`](https://www.google.com/search?q=notebooks/1.0-mvp-modelagem-credito.ipynb)

-----

# 📈 Projeto 2: Forecast de Inadimplência (Macro)

**Objetivo:** Prever a tendência da taxa de inadimplência (Pessoa Física) para calibrar a **Provisão para Devedores Duvidosos (PDD)** e realizar cenários de estresse (Basel III).

### 📊 Dados e Modelagem

  * **Fonte:** Dados oficiais do **Banco Central do Brasil (SGS)** extraídos via API em tempo real (Série 21082).
  * **Período:** Ciclo de crédito completo (2011 - 2024).
  * **Modelo:** **SARIMA** (Seasonal AutoRegressive Integrated Moving Average).
  * **Racional:** O modelo captura explicitamente a tendência de longo prazo e os ciclos sazonais de endividamento das famílias brasileiras (ex: aumento de inadimplência pós-final de ano).

### 🚨 Resultados e Alerta de Risco

O modelo obteve um erro médio absoluto (**MAPE**) de apenas **6.09%**, excelente para dados macroeconômicos voláteis.

**Cenário Base (Projeção 2025-2026):**
O modelo aponta uma **tendência clara de alta** na inadimplência, projetando o rompimento do patamar de **4.15%** no início de 2026.

  * **Recomendação Estratégica:** A tesouraria deve considerar o fortalecimento do colchão de liquidez (PDD) e revisão de políticas de concessão para faixas de rating de maior risco (D-H) para 2026.

> *Código Fonte:* [`notebooks/2.0-forecast-inadimplencia.ipynb`](https://www.google.com/search?q=notebooks/2.0-forecast-inadimplencia.ipynb)

-----

## 🛠️ Tech Stack & Reproducibilidade

  * **Linguagem:** Python 3.12
  * **Bibliotecas:** Pandas, NumPy, Scikit-Learn, Statsmodels, Seaborn.

### Como rodar o projeto:

```bash
# 1. Clone o repositório
git clone [https://github.com/Valvitor/credit-risk-modeling.git](https://github.com/Valvitor/credit-risk-modeling.git)

# 2. Instale as dependências
pip install -r requirements.txt

# 3. Execute os Notebooks (A ordem não interfere)
# notebooks/1.0-mvp-modelagem-credito.ipynb
# notebooks/2.0-forecast-inadimplencia.ipynb
```

-----

## 🔮 Roadmap (Próximos Passos)

Para evoluir estes MVPs para modelos produtivos de nível bancário:

  * [ ] **Modelagem Avançada:** Testar Gradient Boosting (XGBoost/LightGBM) para o Score de Crédito.
  * [ ] **Explicabilidade (XAI):** Implementar **SHAP Values** para justificar decisões individuais (Compliance regulatório).
  * [ ] **Deploy:** Criar uma API com FastAPI para servir o modelo de Score em tempo real.

-----

## 📞 Contato

**Valvitor Santos** - Economista & Data Scientist

  * [LinkedIn](https://www.linkedin.com/in/valvitor-santos/)
  * [E-mail](mailto:valvitorscf@gmail.com)