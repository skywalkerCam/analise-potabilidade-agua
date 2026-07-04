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
  Principais Descobertas da EDA

### 3. Auditoria da Limpeza

- Dados Ausentes: O dataset final contém exatamente **3.276 amostras** e **0 valores nulos**, validando o sucesso da imputação por mediana da equipe.
- Winsorização: O boxplot confirma que os outliers de dureza foram contidos sem distorcer as medianas.

* Assimetria (Skewness): A maioria dos atributos alcançou simetria próxima a zero (`ph`: $0.05$, `Hardness`: $-0.02$), exceto `Solids` ($0.48$), o que justifica a padronização via `StandardScaler`.

- Volatilidade (CV%): A concentração de `Solids` é o fator mais instável ($39.13\%$), enquanto `Sulfate` é o mais homogêneo ($9.53\%$).

### 3.1 Comportamento e Correlação das Classes

- **O Paradoxo do pH:** A análise visual revela que há centenas de amostras potáveis totalmente fora dos limites teóricos recomendados pela OMS ($6.5$ a $8.5$).
- **Teste de Hipótese (Mann-Whitney U):** O p-valor de **0.90839** ($p > 0.05$) confirma que não há diferença estatisticamente significativa entre o pH de águas potáveis e não potáveis.
- **Correlação Linear com o Alvo:** Todos os atributos apresentam correlação linear próxima a zero com a variável `Potability` (faixa de $-0.03$ a $+0.03$).

## 🛠️ Ambiente e Execução

O projeto é fixado em **Python 3.12** (ver `.python-version`). As dependências estão congeladas em `requirements.txt`.

```bash
# criar e ativar o ambiente
python3.12 -m venv .venv
source .venv/bin/activate

# instalar dependências
pip install -r requirements.txt
```

Para treinar os modelos, execute o notebook `modelos_preditivos.ipynb`. Os experimentos são registrados via MLflow em um backend SQLite (`mlflow.db`). Para visualizar os resultados:

```bash
mlflow ui --backend-store-uri sqlite:///mlflow.db
# abra http://localhost:5000
```

> O tracking store (`mlruns/` e `mlflow.db`) **não é versionado** — é regenerado ao rodar o notebook e contém caminhos absolutos da máquina local.

### ⚠️ Limitação conhecida: Python 3.14

Não use **Python 3.14** com o MLflow 3.14.0. O _cliente_ funciona (o notebook treina e loga normalmente), mas o **servidor/UI quebra** ao subir, com:

```
ImportError: cannot import name 'Traversable' from 'importlib.abc'
```

É um bug do MLflow no Python 3.14 (o `Traversable` foi movido para `importlib.resources.abc` nessa versão), sem correção publicada até o momento — ver [mlflow/mlflow#24155](https://github.com/mlflow/mlflow/issues/24155). Por isso o projeto está fixado no **Python 3.12**, onde o servidor roda nativamente.

## 🔜 Próximas Etapas

- [x] Treinamento e comparação dos classificadores
- [x] Tracking dos experimentos com MLflow
- [ ] Análise dos resultados e métricas
- [ ] Documentação e vídeo de apresentação
