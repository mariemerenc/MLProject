
# Machine Learning Project - Classificação de Fotos de Gatos e Cachorros

Este repositório contém as bases de dados e os notebooks utilizados no projeto de classificação de imagens de gatos e cachorros. O objetivo foi explorar diferentes técnicas de aprendizado supervisionado e comitês de classificadores para alcançar resultados acurados e confiáveis.



### Autores: Mariana Emerenciano Miranda, Arthur Ferreira de Holanda, Artur Revoredo Pinto


# Descrição do projeto

Este projeto aborda o problema de classificação de imagens de gatos e cachorros em quatro classes:

* Gatos: Sphynx, Egyptian Mau
* Cachorros: Lulu da Pomerânia, English Setter
O objetivo principal foi avaliar diferentes algoritmos de aprendizado supervisionado e métodos de comitês de classificadores.
# Estrutura do repositório

```bash
├── CSVs PCA         # Bases de dados originais reduzidas com PCA
│   ├── HOG_128_16_PCA.csv
│   ├── HOG_256_16_PCA.csv
│   ├── CNN_VGG16_128_max_PCA.csv
│   ├── CNN_VGG19_256_max_PCA.csv
│   └── ...          # Outras bases após PCA
│
├── CSVs             # Bases de dados originais extraídas de HOG e CNN
│   ├── HOG_128_20.csv
│   ├── CNN_VGG16_128_avg.csv
│   ├── CNN_VGG16_256_avg.csv
│   ├── CNN_VGG19_256_avg.csv
│   └── ...          # Outras bases originais
│
├── Pratica01        # extrator para imagens e uso do k-NN
│ 
├── Pratica02        # árvore de decisão (AD)
│
├── Pratica03        # naive bayes (NB)
│
├── Pratica04        # multi-layer perceptron (MLP)
│
├── Pratica05        # comitês de classificadores

```


# Metodologia

O projeto foi conduzido em cinco etapas principais. Esta seção detalha as abordagens utilizadas em cada etapa.

---

## 1. Pré-processamento

### Redimensionamento das Imagens:
- As imagens foram ajustadas para resoluções de **128x128** e **256x256** pixels.

### Extração de Características:
Foram utilizadas as seguintes técnicas:
- **HOG (Histogram of Oriented Gradients):** Quatro bases geradas com diferentes configurações.
- **CNN (Convolutional Neural Networks):** Modelos VGG16 e VGG19 com métodos de pooling `avg` e `max`.

### Criação de Bases:
- Um total de 12 bases de dados foi gerado, sendo posteriormente reduzido com **PCA** (10 componentes). As seis piores bases foram escolhidas para o PCA.

---

## 2. Aprendizado Supervisionado

Foram aplicados os seguintes algoritmos de aprendizado supervisionado:

### **k-NN (k-Nearest Neighbors):**
- Variação do número de vizinhos de 1 a 10.
- Avaliação com **10-Fold Cross-Validation** e divisão **Holdout (70/30)**.
- Resultados comparados em termos de acurácia média e desvio padrão.

### **Árvores de Decisão:**
- Avaliação de diferentes profundidades máximas da árvore (2 a 10).
- Métodos avaliados com validação cruzada e Holdout.

### **Naive Bayes:**
- Implementação das variantes **GaussianNB**, **MultinomialNB**, e **ComplementNB**.
- Análise das acurácias para cada configuração.

### **Multi-Layer Perceptron (MLP):**
Configurações testadas incluem:
- **Funções de ativação:** `relu`.
- **Solvers:** `sgd` e `adam`.
- **Estruturas de camadas escondidas:** `X_train.shape[1]`, `X_train.shape[1] * 2` e `X_train.shape[1] // 2`.
- **Taxas de aprendizado:** `0.001`, `0.01`, `0.1`.
- **Número de iterações:** `500`, `1000`, `1500`.
- Uso de **Grid Search** para otimização.

---

## 3. Comitês de Classificadores

Os seguintes métodos de comitês foram implementados e analisados:

### **Bagging**
  - Bagging Padrão.
  - Bagging com Seleção de Atributos (`max_features=0.5`).

* Algoritmos incluídos:
   - `DecisionTreeClassifier` (melhor configuração).
   - `KNeighborsClassifier`.
   - `GaussianNB`.
   - `MLPClassifier`.
---

### **Boosting**
- Utilizado via `sklearn.ensemble.AdaBoostClassifier`.
* Algoritmos incluídos:
     - `DecisionTreeClassifier` (melhor configuração).
     - `GaussianNB`.
---

### **Random Forest**
- Utilizado via `sklearn.ensemble.RandomForestClassifier`.

---

### **Stacking**
- Utilizado via `sklearn.ensemble.StackingClassifier`.

- Algoritmos incluídos: **MLP**, **k-NN**, **Decision Tree**, e **Naive Bayes**.

---

### **Voting**
- Utilizado via `sklearn.ensemble.VotingClassifier`.
- Algoritmos incluídos: **MLP**, **k-NN**, **Decision Tree**, e **Naive Bayes**.

---

## 4. Pós-processamento

### **Teste Estatístico**

#### **Friedman Test:**
- Compara os desempenhos entre classificadores individuais e comitês.
- Determina se há diferenças significativas entre os métodos.
- Caso o p-valor seja maior que 0,05, conclui-se que não há diferença estatística significativa.

#### **Teste de Nemenyi (post-hoc):**
- Aplicado somente se o p-valor do Teste de Friedman fosse menor que 0,05.
- Avalia quais métodos apresentaram diferenças estatísticas significativas.

---

