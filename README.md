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
Dada a natureza desbalanceada do dataset, a **Acurácia** é uma métrica enganosa (um modelo que aprova todo mundo teria 92% de acurácia, mas quebraria o banco).
O foco deste projeto foi maximizar a **ROC AUC (Area Under the Curve)**.

* **Por que AUC?** Ela mede a capacidade do modelo de *ordenar* os clientes por risco. Um bom modelo deve dar uma probabilidade de default mais alta para quem realmente vai atrasar, permitindo à mesa de crédito definir o ponto de corte (threshold) ideal baseada no apetite ao risco da instituição.

---

## 📊 Resultados do Modelo (MVP)

O modelo final, um **Random Forest Classifier** com balanceamento de classes, foi avaliado em 30% dos dados (conjunto de teste).

| Métrica | Resultado | Interpretação |
| :--- | :--- | :--- |
| **ROC AUC** | **0.7151** | Boa capacidade de discriminação entre bons e maus pagadores (Baseline sólida). |
| **Dataset** | Desbalanceado | Tratado via parâmetro `class_weight='balanced'`. |

### 1. Curva ROC
A curva demonstra que o modelo é significativamente superior a uma escolha aleatória (linha pontilhada), capturando sinal nos dados.
![Curva ROC](images/roc_curve.png)

### 2. Importância das Variáveis (Feature Importance)
O que define um cliente de risco? Segundo o modelo, dados externos (Bureau de crédito) e a idade são cruciais, seguidos pelas variáveis de engenharia criadas.
![Feature Importance](images/feature_importance.png)

### 3. Matriz de Confusão
![Matriz de Confusão](images/confusion_matrix.png)

---

## 🧠 Metodologia e Engenharia de Atributos

Como economista, a abordagem não foi apenas estatística. Houve um processo de construção de hipóteses econômicas transformadas em variáveis (**Feature Engineering**).

### Variáveis Criadas (Domain Knowledge)
Foram derivadas novas métricas para capturar a saúde financeira real do cliente:

1.  **Comprometimento de Renda (`CREDIT_INCOME_PERCENT`):**
    $$\frac{\text{Valor do Crédito}}{\text{Renda Anual}}$$
    *Hipótese:* Clientes pedindo empréstimos muitas vezes superiores à sua renda anual apresentam maior alavancagem e risco.

2.  **Peso da Parcela (`ANNUITY_INCOME_PERCENT`):**
    $$\frac{\text{Valor da Parcela (Anuidade)}}{\text{Renda Anual}}$$
    *Hipótese:* Quanto maior a parcela em relação ao salário (fluxo de caixa), maior a sensibilidade a choques financeiros.

3.  **Estabilidade Profissional (`DAYS_EMPLOYED_PERCENT`):**
    $$\frac{\text{Dias Empregado}}{\text{Idade do Cliente}}$$
    *Hipótese:* Clientes com maior tempo de emprego relativo à idade (Ciclo de Vida) tendem a apresentar menor volatilidade de renda.

---

## 🛠️ Tech Stack e Pipeline

O projeto segue um pipeline linear de Data Science:

1.  **Coleta de Dados:** Leitura de arquivos CSV (Pandas).
2.  **Limpeza (Preprocessing):**
    * Remoção de colunas irrelevantes (IDs).
    * Imputação de valores nulos utilizando a **Mediana** (para evitar distorção por outliers).
3.  **Modelagem:**
    * Uso de `RandomForestClassifier`.
    * Configuração `class_weight='balanced'` para penalizar erros na classe minoritária.
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
      * Execute todas as células para gerar o treinamento.

-----

## 🔮 Próximos Passos (Melhorias Futuras)

Para evoluir este MVP para um modelo produtivo, os próximos passos mapeados são:

  * [ ] **Testar Boosting:** Implementar XGBoost ou LightGBM, que tendem a ter melhor performance em dados tabulares.
  * [ ] **Otimização de Hiperparâmetros:** Utilizar Bayesian Search para refinar a Random Forest.
  * [ ] **Deploy:** Criar uma API com FastAPI para servir o modelo em tempo real.

-----

## 📞 Contato

**Valvitor Santos**

  * 💼 [LinkedIn](https://www.linkedin.com/in/valvitor-santos/)
  * 📧 [Email](mailto:valvitorscf@gmail.com)

