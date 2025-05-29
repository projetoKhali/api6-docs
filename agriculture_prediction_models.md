# Documento Técnico dos Modelos de Previsão Agrícola
Link para kaggle com a contrução do modelo: https://www.kaggle.com/code/marcosmalaquias/api6-condictional-prevision/edit/run/235061774
## 1. Introdução
O projeto visa a construção de modelos de machine learning para previsão de variáveis importantes no setor agrícola brasileiro, com base em dados históricos de produção, clima e insumos (fertilizantes e pesticidas).

Foram desenvolvidos três tipos principais de modelos:
- Previsão de **chuva anual** (`Rainfall Model`);
- Previsão de **insumos agrícolas** (`Input Models`: Fertilizizer e Pesticide);
- Previsão da **produção agrícola** (`Yield Model`).

Os modelos foram treinados usando **Random Forest Regressors**, uma técnica robusta e eficaz para dados tabulares heterogêneos.

---

## 2. Dados Utilizados
- Fonte: Base de dados de produção agrícola de estados da Índia (adaptado para estados brasileiros).
- Variáveis:
  - Estado
  - Ano da colheita
  - Cultura
  - Estação
  - Área cultivada (hectares)
  - Produção (toneladas)
  - Chuva anual (mm)
  - Uso de fertilizantes
  - Uso de pesticidas

Os dados passaram por limpeza, remoção de nulos e adaptação de nomes para o contexto brasileiro.

---

## 3. Modelos Desenvolvidos

### 3.1. 🌧️ Rainfall Model
- **Objetivo**: Prever a chuva anual baseada no estado e no ano.
- **Features**: Estado codificado e Ano da colheita.

**Métricas de avaliação**:
| Métrica | Valor |
|:---|:---|
| R² | 0.87 |
| MAE | 233 mm |
| RMSE | 361 mm |

**Conclusão**:  
O modelo apresenta alta capacidade de explicação da variabilidade da chuva anual, sendo adequado para uso em previsões climáticas regionais.

---

### 3.2. 🌿 Fertilizer Model
- **Objetivo**: Prever a quantidade de fertilizante necessária.
- **Features**: Cultura, Estado, Estação, Área, Chuva Anual.

**Métricas de avaliação**:
| Métrica | Valor |
|:---|:---|
| R² | 0.97 |
| MAE | 3.316.563 |
| RMSE | 13.988.690 |

**Conclusão**:  
O modelo prevê fertilizantes com alta precisão. O valor elevado de erro absoluto está relacionado à escala dos dados, mas o coeficiente R² próximo de 1 indica excelente performance.

---

### 3.3. 🐛 Pesticide Model
- **Objetivo**: Prever a quantidade de pesticida necessária.
- **Features**: Cultura, Estado, Estação, Área, Chuva Anual.

**Métricas de avaliação**:
| Métrica | Valor |
|:---|:---|
| R² | 0.94 |
| MAE | 9.260 |
| RMSE | 43.453 |

**Conclusão**:  
Assim como o modelo de fertilizantes, o de pesticidas também apresenta ótima capacidade preditiva.

---

### 3.4. 🌾 Yield Model
- **Objetivo**: Prever a produção agrícola total.
- **Features**: Estado, Cultura, Estação, Área, Chuva Anual, Fertilizante, Pesticida.

**Métricas de avaliação**:
| Métrica | Valor |
|:---|:---|
| R² | 0.99 |
| MAE | 1.633.730 |
| RMSE | 26.682.550 |

**Conclusão**:  
Modelo extremamente robusto, com quase 100% da variância explicada, adequado para previsão de produção em larga escala.

---

## 4. Conclusão Geral
Os modelos desenvolvidos mostraram excelente capacidade de generalização nos dados de treinamento, especialmente o modelo de previsão de produção agrícola.
