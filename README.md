# analise-potabilidade-agua
Projeto de classificação supervisionada para predição de potabilidade da água, desenvolvido como avaliação final da disciplina Ciência de Dados — UFC Sobral (2026.1).

## 📊 Dataset

- **Fonte:** [Water Potability — Kaggle](https://www.kaggle.com/datasets/adityakadiwal/water-potability)
- **Amostras:** 3.276
- **Features:** 9 atributos físico-químicos (ph, Hardness, Solids, Chloramines, Sulfate, Conductivity, Organic_carbon, Trihalomethanes, Turbidity)
- **Variável-alvo:** `Potability` (0 = não potável, 1 = potável)

## 📁 Arquivos de Dados

- **water_potability.csv** — dataset original baixado do Kaggle, mantido intacto como referência
- **water_potability_limpo.csv** — dataset após limpeza (sem duplicatas, sem valores ausentes e com outliers tratados), usado tanto como entrada para o pré-processamento quanto para análise exploratória dos dados (EDA)
- **X_train.csv / y_train.csv** — features e labels de treino, já escalonadas e balanceadas com SMOTE, usadas para treinar os modelos
- **X_test.csv / y_test.csv** — features e labels de teste, apenas escalonadas (sem SMOTE), usadas exclusivamente para avaliação final dos modelos

## ⚙️ Etapas Concluídas

### 1. Limpeza (`cleaning.ipynb`)
- Remoção de duplicatas
- Imputação de valores ausentes com mediana por classe (ph, Sulfate, Trihalomethanes)
- Correção de valores de pH fora do intervalo físico [0, 14]
- Tratamento de outliers via Winsorização IQR

### 2. Pré-processamento (`preprocessing.ipynb`)
- Divisão treino/teste (80/20) com estratificação
- Escalonamento das features com `StandardScaler`
- Balanceamento das classes com `SMOTE`

## 🔜 Próximas Etapas
- [ ] Treinamento e comparação dos classificadores
- [ ] Tracking dos experimentos com MLflow
- [ ] Análise dos resultados e métricas
- [ ] Vídeo de apresentação
