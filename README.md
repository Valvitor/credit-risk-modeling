# 💰 Credit Scoring: Modelagem de Risco de Crédito

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge&logo=python)
![Scikit-Learn](https://img.shields.io/badge/Scikit_Learn-F7931E?style=for-the-badge&logo=scikit-learn)
![Status](https://img.shields.io/badge/Status-MVP_Concluído-success?style=for-the-badge)

> **Objetivo:** Desenvolver um algoritmo de Machine Learning capaz de prever a probabilidade de inadimplência (*default*), reduzindo a exposição ao risco e otimizando a concessão de crédito.

---

## 📋 Visão Geral do Negócio
Em instituições financeiras, o maior desafio não é apenas conceder crédito, mas concedê-lo para quem consegue pagar. O problema central é um **problema de classificação desbalanceada**: a maioria dos clientes paga em dia, mas o erro de conceder empréstimo a um mau pagador gera prejuízo direto de capital (Perda do Principal).

Neste projeto, utilizamos dados históricos do **Home Credit** para prever a classe `TARGET` (0: Bom Pagador, 1: Mau Pagador).

### 🎯 KPIs e Métricas de Sucesso
Dada a natureza desbalanceada do dataset, a **Acurácia** é uma métrica enganosa. O foco deste projeto foi maximizar a **ROC AUC (Area Under the Curve)**.
* **Por que AUC?** Ela mede a capacidade do modelo de *ordenar* os clientes. Um bom modelo de risco deve dar uma probabilidade de default mais alta para quem realmente vai atrasar, permitindo à mesa de crédito definir o ponto de corte (threshold) ideal baseada no apetite ao risco da instituição.

---

## 📊 Resultados do Modelo (MVP)

O modelo final, um **Random Forest Classifier** com balanceamento de classes, foi avaliado em 30% dos dados (conjunto de teste).

| Métrica | Resultado | Interpretação |
| :--- | :--- | :--- |
| **ROC AUC** | **0.7151** | Boa capacidade de discriminação entre bons e maus pagadores. |
| **Dataset** | Desbalanceado | Tratado via parâmetro `class_weight='balanced'`. |

### 1. Curva ROC
A curva demonstra que o modelo é superior a uma escolha aleatória (linha pontilhada).
![Curva ROC](./notebooks/outputs/roc_curve.png)

### 2. Importância das Variáveis (Feature Importance)
O que define um cliente de risco? Segundo o modelo, dados externos (Bureau de crédito) e a idade são cruciais.
![Feature Importance](./notebooks/outputs/feature_importance.png)

### 3. Matriz de Confusão
![Matriz de Confusão](./notebooks/outputs/confusion_matrix.png)

---

## 🧠 Metodologia e Engenharia de Atributos

Como economista, a abordagem não foi apenas "jogar dados no modelo". Houve um processo de construção de hipóteses econômicas transformadas em variáveis (Feature Engineering).

### Variáveis Criadas (Domain Knowledge)
Foram derivadas novas métricas para capturar a saúde financeira real do cliente:

1.  **Comprometimento de Renda (`CREDIT_INCOME_PERCENT`):**
    $$\frac{\text{Valor do Crédito}}{\text{Renda Anual}}$$
    *Hipotése:* Clientes pedindo empréstimos muitas vezes superiores à sua renda anual apresentam maior risco.

2.  **Peso da Parcela (`ANNUITY_INCOME_PERCENT`):**
    $$\frac{\text{Valor da Parcela (Anuidade)}}{\text{Renda Anual}}$$
    *Hipotése:* Quanto maior a parcela em relação ao salário, maior a probabilidade de default.

3.  **Estabilidade Profissional (`DAYS_EMPLOYED_PERCENT`):**
    $$\frac{\text{Dias Empregado}}{\text{Idade do Cliente}}$$
    *Hipotése:* Clientes com maior tempo de emprego relativo à idade tendem a ser mais estáveis.

---

## 🛠️ Tech Stack e Pipeline

O projeto segue um pipeline linear de Data Science:

1.  **Coleta de Dados:** Leitura de arquivos CSV (Pandas).
2.  **Limpeza (Preprocessing):**
    * Remoção de colunas irrelevantes (IDs).
    * Imputação de valores nulos utilizando a **Mediana** (para evitar distorção por outliers de renda).
3.  **Modelagem:**
    * Uso de `RandomForestClassifier`.
    * Configuração `class_weight='balanced'` para penalizar erros na classe minoritária (inadimplentes).
4.  **Avaliação:** Scikit-learn metrics (AUC, Confusion Matrix).

```python
# Exemplo do Core do Modelo
model = RandomForestClassifier(
    n_estimators=100,
    class_weight='balanced', # Tratamento essencial para risco de crédito
    random_state=42,
    n_jobs=-1
)
model.fit(X_train, y_train)
````

-----

## 🚀 Como Reproduzir este Projeto

1.  **Clone o repositório**

    ```bash
    git clone [https://github.com/Valvitor/credit-risk-modeling.git](https://github.com/Valvitor/credit-risk-modeling.git)
    cd credit-risk-modeling
    ```

2.  **Instale as dependências**

    ```bash
    pip install -r requirements.txt
    ```

3.  **Execute o Notebook**

      * Navegue até a pasta `notebooks/`.
      * Certifique-se de que o arquivo `application_train.csv` está na pasta `data/`.
      * Execute todas as células para gerar o treinamento e os gráficos na pasta `outputs/`.

-----

# 📊 Credit Risk Ecosystem: Do Micro ao Macro

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge&logo=python)
![Status](https://img.shields.io/badge/Status-Portfólio_Completo-success?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Finanças_Quantitativas-red?style=for-the-badge)

> **Objetivo:** Este repositório consolida duas abordagens complementares de risco de crédito: a análise individual do tomador (Scoring) e a projeção do cenário econômico (Forecasting), utilizando dados reais do Banco Central do Brasil.

---

## 📂 Estrutura do Portfólio

| Projeto | Foco | Técnica | Target |
| :--- | :--- | :--- | :--- |
| **1. Credit Scoring** | **Micro** (Cliente) | Random Forest | Probabilidade de Default Individual |
| **2. Macro Forecast** | **Macro** (Mercado) | SARIMA | Taxa de Inadimplência do Sistema (Séries Temporais) |

---

# 🏢 Projeto 1: Credit Scoring (Micro)
**Objetivo:** Classificar clientes propensos a inadimplência para otimizar a concessão de crédito.

### Resultados (MVP)
* **Métrica:** ROC AUC **0.7151**
* **Insight:** A estabilidade profissional relativa à idade (`DAYS_EMPLOYED_PERCENT`) provou-se um preditor mais forte do que a renda bruta.

### Visualizações Chave
<p float="left">
  <img src="./notebooks/outputs/roc_curve.png" width="45%" />
  <img src="./notebooks/outputs/feature_importance.png" width="45%" /> 
</p>

> *Para detalhes técnicos e código, acesse:* [`notebooks/1.0-mvp-modelagem-credito.ipynb`](notebooks/1.0-mvp-modelagem-credito.ipynb)

---

# 📈 Projeto 2: Forecast de Inadimplência (Macro)
**Objetivo:** Prever a tendência da taxa de inadimplência (Pessoa Física) para calibrar a **Provisão para Devedores Duvidosos (PDD)** e testes de estresse (Basel III).

### Fonte de Dados
Dados oficiais do **Banco Central do Brasil (SGS)** via API, cobrindo o ciclo de crédito de 2011 a 2024.

### Modelagem (SARIMA)
Utilizou-se um modelo **SARIMA (Seasonal ARIMA)** para capturar:
1.  **Tendência:** O crescimento recente da inadimplência pós-pandemia.
2.  **Sazonalidade:** Padrões cíclicos de endividamento ao longo do ano.

### 🚨 Resultados e Alerta de Negócio
O modelo performou com um erro baixíssimo (**MAPE: 6.09%**) e traz um alerta importante para a gestão de risco:

![Forecast Sarima](images/forecast_sarima.png)

**Projeção para 2026:**
O modelo aponta uma **tendência de alta**, rompendo a barreira de **4.15%** no início de 2026.
* **Ação Recomendada:** Revisar políticas de concessão e aumentar o colchão de liquidez (PDD) para absorver o provável aumento de perdas.

```text
PREVISÃO (Próximos 6 Meses):
2025-10: 3.97%
2025-11: 3.98%
2025-12: 3.91% (Sazonalidade de fim de ano)
2026-01: 4.05% 🔺
2026-02: 4.12% 🔺
2026-03: 4.15% 🔺
````

> *Para detalhes técnicos e código, acesse:* [`notebooks/2.0-forecast-inadimplencia.ipynb`](https://www.google.com/search?q=notebooks/2.0-forecast-inadimplencia.ipynb)

-----

## 🛠️ Tech Stack Geral

  * **Linguagem:** Python 3.12
  * **Bibliotecas:** Pandas, NumPy, Scikit-Learn, Statsmodels, Seaborn.
  * **Dados:** Kaggle (Home Credit) & API Banco Central (SGS).

## 📞 Contato

**Valvitor Santos** - Economista & Data Scientist

  * [LinkedIn](https://www.linkedin.com/in/valvitor-santos/)
  * [E-mail](mailto:valvitorscf@gmail.com)

<!-- end list -->

````

## 📞 Contato

**Valvitor Santos**

  * 💼 [LinkedIn](https://www.linkedin.com/in/valvitor-santos/)
  * 📧 [Email](valvitorscf@gmail.com)
  * 🐱 [GitHub](https://github.com/Valvitor)

-----

```
```