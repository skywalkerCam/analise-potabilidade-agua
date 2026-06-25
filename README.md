# analise-potabilidade-agua
Projeto de classificação supervisionada para predição de potabilidade da água, desenvolvido como avaliação final da disciplina Ciência de Dados — UFC Sobral (2026.1).

## 📁 Estrutura do Projeto

analise-potabilidade-agua/
│
├── dados/
│   ├── water_potability.csv           # Dataset original (Kaggle)
│   ├── water_potability_limpo.csv     # Dataset após limpeza
│   ├── X_train.csv                    # Features de treino (escalonadas + SMOTE)
│   ├── X_test.csv                     # Features de teste (escalonadas)
│   ├── y_train.csv                    # Labels de treino
│   └── y_test.csv                     # Labels de teste
│
├── cleaning.ipynb                     # Limpeza do dataset
├── preprocessing.ipynb                # Escalonamento e balanceamento
└── README.md
```

## 📊 Dataset
- **Fonte:** [Water Potability — Kaggle](https://www.kaggle.com/datasets/adityakadiwal/water-potability)
- **Amostras:** 3.276
- **Features:** 9 atributos físico-químicos (ph, Hardness, Solids, Chloramines, Sulfate, Conductivity, Organic_carbon, Trihalomethanes, Turbidity)
- **Variável-alvo:** `Potability` (0 = não potável, 1 = potável)

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
