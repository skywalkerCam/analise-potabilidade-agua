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

## 📊 3 Análise Exploratória e Estatística dos Dados (EDA)

Esta etapa foi realizada com o objetivo de diagnosticar a qualidade dos dados, compreender a distribuição das variáveis físico-químicas e avaliar o potencial discriminatório dos atributos em relação à potabilidade da água.

### 🔍 3.1. Diagnóstico de Dados Ausentes e Desbalanceamento

Conforme destacado no escopo do projeto, o _dataset_ apresenta desafios estruturais que foram quantificados nesta análise:

- **Dados Ausentes:** Identificamos volumes significativos de omissão nas variáveis `ph`, `Sulfate` e `Trihalomethanes`, orientando a necessidade de estratégias de imputação no pipeline de pré-processamento.
- **Desbalanceamento:** A variável alvo `Potability` exibe um desbalanceamento nítido entre as classes, o que demandará métricas de avaliação robustas (como F1-Score e AUC-ROC) além da acurácia simples.

### 📈 3.2. Análise Univariada e Assimetria (Skewness)

Durante a extração das estatísticas descritivas, calculou-se o índice de assimetria para cada atributo.

- Atributos como `Solids` e `Sulfate` apresentam uma assimetria positiva acentuada.
- **Nota de Engenharia:** Como modelos lineares (Regressão Logística) e baseados em margens (SVM) são sensíveis a dados severamente assimétricos, documenta-se a recomendação de aplicar transformações matemáticas — como **Transformação Logarítmica** ou **Box-Cox** — na etapa de pré-processamento.

### 📊 3.3. Análise Visual de Variáveis Críticas (pH e Dureza)

A análise visual dos atributos trouxe revelações cruciais sobre a natureza do problema:

- **Sobreposição de Classes:** No pH (mesmo mapeando os limites recomendados pela OMS de 6.5 a 8.5) e na Dureza (`Hardness`), a distribuição das amostras potáveis e não potáveis é sobreposta de forma quase idêntica.
- **Complexidade:** Isso indica que a potabilidade da água é determinada por uma relação multidimensional complexa entre os parâmetros, e não por regras de corte simples em uma única variável.

### 🧪 3.4. Teste de Hipótese Estatística (Análise de Inferência)

Para determinar se as diferenças visuais nos grupos eram estatisticamente significativas, aplicou-se o teste não-paramétrico de **Mann-Whitney U** sobre a variável `ph`:

- **p-valor obtido:** 0.90839
- **Conclusão:** Como o $p$-valor é expressivamente superior a 0.05, **não rejeitamos a hipótese nula**. Isso prova matematicamente que o pH isolado não possui poder discriminatório linear para separar águas seguras de inseguras.

### 🔗 3.5. Matriz de Correlação Multivariada

A geração da matriz de correlação de Pearson confirmou os achados anteriores:

- Todos os coeficientes de correlação linear direta em relação à variável `Potability` situam-se próximos de zero (faixa entre -0.03 e 0.03).
- **Validação do Benchmarking:** Esses resultados justificam empiricamente a abordagem exigida pela disciplina de realizar o benchmarking com classificadores avançados baseados em árvores (como _Random Forest_ e _XGBoost_), capazes de mapear fronteiras de decisão não-lineares de alta ordem.

## 🔜 Próximas Etapas

- [ ] Análise exploratória de dados (EDA) e estatísticas
- [ ] Treinamento e comparação dos classificadores
- [ ] Tracking dos experimentos com MLflow
- [ ] Análise dos resultados e métricas
- [ ] Documentação e vídeo de apresentação
