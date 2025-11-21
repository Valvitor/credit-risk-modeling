# 💰 Credit Scoring: Modelagem de Risco de Crédito

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge\&logo=python)
![Scikit-Learn](https://img.shields.io/badge/Scikit_Learn-F7931E?style=for-the-badge\&logo=scikit-learn)
![Status](https://img.shields.io/badge/Status-MVP_Concluído-success?style=for-the-badge)

> **Objetivo:** Desenvolver um algoritmo de Machine Learning capaz de prever a probabilidade de inadimplência (*default*), reduzindo a exposição ao risco e otimizando a concessão de crédito.

---

## 📋 Visão Geral do Negócio

Em instituições financeiras, o maior desafio não é apenas conceder crédito, mas concedê-lo para quem consegue pagar. O problema central é um **problema de classificação desbalanceada**: a maioria dos clientes paga em dia, mas conceder crédito a um mau pagador gera prejuízo direto (Perda do Principal).

Neste projeto, utilizamos dados históricos do **Home Credit** para prever a classe `TARGET`
*(0: Bom Pagador, 1: Mau Pagador)*.

### 🎯 KPIs e Métricas de Sucesso

A **Acurácia** é inadequada em bases desbalanceadas. Portanto, a métrica principal é:

* **ROC AUC** – Mede a capacidade do modelo de *ordenar* os clientes por risco.

---

## 📊 Resultados do Modelo (MVP)

| Métrica     | Resultado     | Interpretação                                  |
| ----------- | ------------- | ---------------------------------------------- |
| **ROC AUC** | **0.7151**    | Boa discriminação entre bons e maus pagadores. |
| **Dataset** | Desbalanceado | Mitigado com `class_weight='balanced'`.        |

### 🔍 Curva ROC

![Curva ROC](./notebooks/outputs/roc_curve.png)

### 🔍 Importância das Variáveis

![Feature Importance](./notebooks/outputs/feature_importance.png)

### 🔍 Matriz de Confusão

![Matriz de Confusão](./notebooks/outputs/confusion_matrix.png)

---

## 🧠 Engenharia de Atributos (Feature Engineering)

O projeto utilizou hipóteses econômicas para criar variáveis com significado financeiro.

1. **Comprometimento de Renda (`CREDIT_INCOME_PERCENT`)**
   $$\frac{\text{Valor do Crédito}}{\text{Renda Anual}}$$

2. **Peso da Parcela (`ANNUITY_INCOME_PERCENT`)**
   $$\frac{\text{Valor da Parcela}}{\text{Renda Anual}}$$

3. **Estabilidade Profissional (`DAYS_EMPLOYED_PERCENT`)**
   $$\frac{\text{Dias Empregado}}{\text{Idade}}$$

---

## 🛠️ Pipeline Técnico

1. **Coleta** — CSV (Pandas)
2. **Limpeza** — remoção de colunas irrelevantes + mediana para nulos
3. **Modelagem** — `RandomForestClassifier` com `class_weight='balanced'`
4. **Avaliação** — ROC AUC + curva ROC + matriz de confusão

```python
model = RandomForestClassifier(
    n_estimators=100,
    class_weight='balanced',
    random_state=42,
    n_jobs=-1
)
model.fit(X_train, y_train)
```

---

## 🚀 Como Reproduzir

```bash
git clone https://github.com/Valvitor/credit-risk-modeling.git
cd credit-risk-modeling
pip install -r requirements.txt
```

Depois, abra o arquivo:

📓 `notebooks/1.0-mvp-modelagem-credito.ipynb`

---

# 📊 Credit Risk Ecosystem: Do Micro ao Macro

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge\&logo=python)
![Status](https://img.shields.io/badge/Status-Portfólio_Completo-success?style=for-the-badge)
![Domain](https://img.shields.io/badge/Domain-Finanças_Quantitativas-red?style=for-the-badge)

> Repositório com duas abordagens de risco de crédito: **Scoring (Micro)** e **Forecasting (Macro)**.

---

## 📂 Estrutura do Portfólio

| Projeto               | Foco             | Técnica       | Target                   |
| --------------------- | ---------------- | ------------- | ------------------------ |
| **1. Credit Scoring** | Micro (Cliente)  | Random Forest | Probabilidade de Default |
| **2. Macro Forecast** | Macro (Economia) | SARIMA        | Inadimplência Agregada   |

---

# 🏢 Projeto 1: Credit Scoring (Micro)

* **Métrica:** ROC AUC **0.7151**
* **Insight:** Estabilidade (`DAYS_EMPLOYED_PERCENT`) superou renda como preditor.

<p float="left">
  <img src="./notebooks/outputs/roc_curve.png" width="45%" />
  <img src="./notebooks/outputs/feature_importance.png" width="45%" /> 
</p>

🔗 Notebook:
`notebooks/1.0-mvp-modelagem-credito.ipynb`

---

# 📈 Projeto 2: Forecast de Inadimplência (Macro)

**Objetivo:** Projetar inadimplência de Pessoa Física usando dados do Banco Central (SGS).

### Modelo SARIMA

Captura:

* Tendência
* Sazonalidade
* Choques estruturais pós-pandemia

![Forecast Sarima](images/forecast_sarima.png)

### 📌 Previsão (2025–2026)

| Mês     | Taxa Prevista |
| ------- | ------------- |
| 2025-10 | 3.97%         |
| 2025-11 | 3.98%         |
| 2025-12 | 3.91%         |
| 2026-01 | 4.05% 🔺      |
| 2026-02 | 4.12% 🔺      |
| 2026-03 | 4.15% 🔺      |

> Tendência de alta → instituições devem reforçar provisão (PDD).

🔗 Notebook:
`notebooks/2.0-forecast-inadimplencia.ipynb`

---

# 🛠️ Tech Stack Geral

* Python 3.12
* Pandas, NumPy
* Scikit-Learn
* Statsmodels
* Seaborn
* Dados: Kaggle + API SGS (BCB)

---

# 📞 Contato

**Valvitor Santos**
Economista & Data Scientist

* 💼 [LinkedIn](https://www.linkedin.com/in/valvitor-santos/)
* 📧 [valvitorscf@gmail.com](mailto:valvitorscf@gmail.com)
* 🐱 [GitHub](https://github.com/Valvitor)

---