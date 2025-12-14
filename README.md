# Detecção de Anomalias em Qualidade de Água

## Trabalho Final - Machine Learning

![Python](https://img.shields.io/badge/Python-3670A0?style=plastic&logo=python&logoColor=ffdd54)
![Jupyter Notebook](https://img.shields.io/badge/Jupyter-%23FA0F00.svg?style=plastic&logo=jupyter&logoColor=white)
![scikit-learn](https://img.shields.io/badge/Scikit--Learn-%23F7931E.svg?style=plastic&logo=scikit-learn&logoColor=white)

### Discentes
- **Ângelo Resplandes**
- **Aquila Morais**  
- **Luis Otávio**

## Resumo

Este projeto implementa e compara **3 algoritmos de detecção de anomalias** para identificar padrões anômalos em medições de qualidade de água. Utilizamos o dataset `water_potability.csv` contendo 3.276 amostras com 9 variáveis de qualidade.

## Algoritmos Implementados

O projeto está organizado em três grandes partes, cada uma focando em um paradigma diferente de detecção de anomalias:

### 1️. Isolation Forest - Baseado em Isolamento de Observações

**Fundamentação Teórica**: Proposto por Liu et al. (2008), baseia-se na premissa de que anomalias são poucas e diferentes, portanto mais fáceis de isolar.

| Aspecto | Descrição |
|---------|-----------|
| **Princípio** | Isolamento rápido de anomalias via particionamento recursivo |
| **Complexidade** | O(n log n) - linear |
| **Métrica** | Path Length (comprimento do caminho) |
| **Vantagem** | Eficiente para grandes volumes de dados |

**Hiperparâmetros**:
- `n_estimators=100` (árvores)
- `contamination=0.10` (10% anomalias)

### 2️. Local Outlier Factor (LOF) - Baseado em Densidade Local

**Fundamentação Teórica**: Proposto por Breunig et al. (2000), detecta anomalias comparando a densidade local de cada ponto com seus vizinhos.

| Aspecto | Descrição |
|---------|-----------|
| **Princípio** | Comparação de densidade local entre vizinhos |
| **Complexidade** | O(n²) no pior caso |
| **Métrica** | Fator LOF (LOF ≈ 1 = normal, LOF >> 1 = anomalia) |
| **Vantagem** | Detecta anomalias contextuais/locais |

**Hiperparâmetros**:
- `n_neighbors=20` (vizinhos)
- `contamination=0.10` (10% anomalias)

### 3️. Autoencoder - Rede Neural para Reconstrução

**Fundamentação Teórica**: Arquitetura de rede neural que aprende a comprimir e reconstruir dados. Anomalias apresentam alto erro de reconstrução.

| Aspecto | Descrição |
|---------|-----------|
| **Princípio** | Erro de reconstrução como métrica de anomalia |
| **Arquitetura** | 9 → 128 → 64 → 32 → **16** → 32 → 64 → 128 → 9 |
| **Métrica** | MSE (Mean Squared Error) |
| **Vantagem** | Captura relações não-lineares complexas |

**Hiperparâmetros**:
- `hidden_layer_sizes=(128, 64, 32, 16, 32, 64, 128)`
- `activation='relu'`
- `solver='adam'`
- `early_stopping=True`

## Dataset

**Arquivo**: `water_potability.csv`

| Variável | Descrição | Unidade |
|----------|-----------|---------|
| pH | Acidez/Alcalinidade | - |
| Hardness | Dureza da água | mg/L |
| Solids | Sólidos dissolvidos totais | ppm |
| Chloramines | Cloraminas | ppm |
| Sulfate | Sulfato | mg/L |
| Conductivity | Condutividade elétrica | μS/cm |
| Organic_carbon | Carbono orgânico | ppm |
| Trihalomethanes | Trihalometanos | μg/L |
| Turbidity | Turbidez | NTU |
| Potability | Potabilidade | 0/1 |

## Pipeline de Pré-processamento

1. **Tratamento de Valores Ausentes**: KNN Imputer (k=5)
2. **Normalização**: StandardScaler (μ=0, σ=1)
3. **Redução de Dimensionalidade**: PCA para visualização 2D

## Sistema de Consenso

O projeto utiliza um **sistema de votação por maioria**:

```
Consenso = (Isolation Forest + LOF + Autoencoder) ≥ 2
```

Apenas amostras identificadas por **≥2 dos 3 métodos** são consideradas anomalias finais.

## Estrutura do Projeto

```
trabalho-final/
├── .gitignore                               # Gitigone
├── anomaly_detection_water_quality.ipynb    # Notebook principal
├── README.md                                # Documentação
├── Water potability                         # PDF apresentação
├── water_potability.csv                     # Dataset de entrada
└── water_quality_anomalies_detected.csv     # Resultados exportados
```

## Como Executar

### Pré-requisitos
```bash
pip install numpy pandas scikit-learn matplotlib matplotlib-venn seaborn
```

### Execução
1. Abra o notebook no Jupyter ou Google Colab
2. Execute todas as células sequencialmente
3. Os resultados serão exportados para `water_quality_anomalies_detected.csv`

## Referências

1. Liu, F. T., Ting, K. M., & Zhou, Z. H. (2008). *Isolation Forest*. ICDM.
2. Breunig, M. M., et al. (2000). *LOF: Identifying Density-Based Local Outliers*. SIGMOD.
3. Goodfellow, I., Bengio, Y., & Courville, A. (2016). *Deep Learning*. MIT Press.
4. Kadiwal, A. (2020). Water Quality [Dataset]. Kaggle.  
Disponível em: https://www.kaggle.com/datasets/adityakadiwal/water-potability. Acesso em: 14 dez. 2025.