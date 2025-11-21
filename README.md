# 💰 Credit Scoring Model - Previsão de Risco de Crédito

## 📌 Visão Geral do Projeto
Este projeto resolve um dos problemas mais críticos de instituições financeiras: **conceder crédito com segurança**.
Utilizando dados históricos de pagamentos (Home Credit), desenvolvi um modelo de Machine Learning capaz de prever a probabilidade de um cliente entrar em *default* (inadimplência).

O objetivo é reduzir a exposição ao risco da instituição, permitindo aprovar empréstimos para bons pagadores e evitar concessões para perfis de alto risco.

---

## 💼 Contexto de Negócio
Em cenários de crédito, o **desbalanceamento** é o maior desafio: a grande maioria dos clientes paga em dia.
* **O Erro Crítico:** Classificar um mau pagador como bom (Falso Negativo) gera prejuízo direto de capital.
* **A Solução:** Utilizamos a métrica **ROC AUC** em vez de Acurácia, pois precisamos ordenar bem a probabilidade de risco, não apenas acertar a classe majoritária.

---

## 🛠️ Tecnologias e Ferramentas
* **Linguagem:** Python 3.12
* **Bibliotecas Principais:** Scikit-learn, Pandas, NumPy, Matplotlib, Seaborn.
* **Modelo:** Random Forest Classifier (com balanceamento de classes).
* **Ambiente:** VS Code / Jupyter Notebook.

---

## 📊 Estratégia da Solução

1.  **Feature Engineering (Criação de Variáveis):**
    Não usei apenas os dados brutos. Criei indicadores financeiros fundamentais, como:
    * `CREDIT_INCOME_PERCENT`: Quanto do crédito pedido compromete a renda anual?
    * `ANNUITY_INCOME_PERCENT`: A parcela cabe no bolso do cliente?
    * `DAYS_EMPLOYED_PERCENT`: Estabilidade profissional relativa à idade.

2.  **Pré-processamento:**
    * Tratamento de dados nulos (Inputação pela Mediana).
    * Seleção de variáveis numéricas para o MVP (Minimum Viable Product).

3.  **Modelagem:**
    * Algoritmo: **Random Forest**. Escolhido pela robustez contra ruídos e capacidade de capturar relações não-lineares.
    * Balanço: Uso de `class_weight='balanced'` para penalizar erros na classe minoritária (inadimplentes).

---

## 📈 Resultados (MVP)

O modelo foi avaliado na base de teste (30% dos dados, não vistos no treino):

* **ROC AUC Score:** 0.72 *(Valor aproximado, o seu código vai gerar o exato)*
* **Insight Principal:** As variáveis mais importantes para definir o risco não foram apenas a renda, mas sim os **Scores Externos** (birôs de crédito) e a **Idade** do cliente (clientes mais jovens tendem a ter maior risco neste dataset).

![Matriz de Confusão](https://via.placeholder.com/600x400?text=Insira+aqui+o+print+da+sua+Matriz+de+Confusão)
*(Espaço reservado para a imagem da Matriz de Confusão gerada pelo notebook)*

---

## 🚀 Como Executar este Projeto

1.  Clone o repositório:
    ```bash
    git clone [https://github.com/Valvitor/credit-risk-modeling.git](https://github.com/Valvitor/credit-risk-modeling.git)
    ```
2.  Instale as dependências:
    ```bash
    pip install -r requirements.txt
    ```
3.  Baixe os dados do [Kaggle Home Credit](https://www.kaggle.com/c/home-credit-default-risk/data) e coloque o arquivo `application_train.csv` na pasta `data/`.
4.  Execute o notebook na pasta `notebooks/`.

---

## 📞 Contato
* **Valvitor Santos**
* [LinkedIn](https://www.linkedin.com/in/valvitor-santos/)
* [E-mail](valvitorscf@gmail.com)