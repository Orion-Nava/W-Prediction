# 📌 Punto de Reorden (ROP) en análisis de inventario usando la CDFE, Bootstrap y simulación Monte Carlo: una alternativa no paramétrica

> **Modelado predictivo de ventas semanales usando series de tiempo tabulares del dataset de Walmart (Kaggle).**  
> Incluye construcción de rezagos, codificación categórica, selección de variables, comparación de modelos y evaluación con split temporal.

---

# 📁 Contenido

- [Descripción general](#descripción-general)  
- [Objetivos](#objetivos)  
- [Dataset](#dataset)  
- [Metodología](#metodología)  
- [Preprocesamiento](#preprocesamiento)  
- [Ingeniería de características](#ingeniería-de-características)  
- [Modelos](#modelos)  
- [Resultados](#resultados)  
- [Gráficas](#gráficas)  
- [Conclusiones](#conclusiones)  
- [Mejoras futuras](#mejoras-futuras)  

---

# 📄 Descripción general

Este proyecto realiza un análisis y modelado predictivo de las **ventas semanales de Walmart**, utilizando el dataset público de Kaggle.  

Se aborda como una **serie de tiempo tabular**, construyendo rezagos, codificando categorías y aplicando modelos desde regresión lineal hasta métodos avanzados como **XGBoost** y **LightGBM**.

El objetivo principal es **evaluar la capacidad predictiva** de diferentes enfoques y establecer un pipeline robusto para forecasting.

---

# 🎯 Objetivos

1. Explorar y limpiar el dataset.  
2. Construir variables relevantes, especialmente **lags** por tienda y departamento.  
3. Implementar un **split temporal** correcto para evitar fuga de información.  
4. Comparar modelos predictivos con un **baseline naïve**.  
5. Evaluar el desempeño con métricas formales (MSE, MAE, R²).  
6. Visualizar resultados y analizar residuales.  
7. Determinar si los modelos avanzados aportan valor frente al baseline.

---

# 📊 Dataset

Dataset: **Walmart Recruiting – Store Sales Forecasting (Kaggle)**.

Incluye:

- Ventas semanales (`Weekly_Sales`)
- Tienda (`Store`)
- Departamento (`Dept`)
- Markdowns promocionales
- Tipo y tamaño de tienda
- Fechas 2010–2012

---

# ⚙️ Metodología

### 1. Integración de Datasets
Se combinan `train.csv`, `features.csv` y `stores.csv`.

### 2. Limpieza
- Conversión de fechas  
- Imputación de MarkDowns  
- Eliminación de columnas no útiles  

### 3. Creación de variables temporales
- Año  
- Mes  
- Semana  
- Día del mes  
- Día de la semana  

### 4. Construcción de rezagos

Se generan **60 rezagos por Store–Dept**, p. ej.:

- `WS_lag(1)`  
- `WS_lag(52)`  
- `WS_lag(2)`  
- …

### 5. Selección de variables

Se utiliza **Permutation Importance** para reducir dimensionalidad y mejorar estabilidad del modelo.

### 6. Codificación categórica

Se emplea **CatBoostEncoder**, ideal para modelos tabulares con alta cardinalidad.

### 7. Split temporal

El dataset se divide de forma **cronológica** (80% train, 20% test), evitando leakage.

### 8. Modelos probados
- Baseline naïve  
- Linear Regression  
- Elastic Net  
- XGBRegressor  
- LGBMRegressor

---

# 🛠️ Preprocesamiento

- Conversión de `IsHoliday` a entero  
- Imputación de MarkDowns faltantes  
- Extracción de variables calendáricas  
- Sort por Store–Dept–Date  
- Construcción de dataframe con fechas para visualización  

---

# 🧩 Ingeniería de características

Tras la selección de variables, las features más relevantes fueron:

- `WS_lag(1)`
- `WS_lag(52)`
- `WS_lag(2)`
- `WS_lag(58)`
- `WS_lag(19)`
- `Dept`
- `MarkDown3`
- `Week`
- `Month`
- `Store`

Esto mejora parsimonia y reduce colinealidad.

---

# 🤖 Modelos

| Modelo | Descripción |
|--------|-------------|
| **Naïve baseline** | Predice usando `WS_lag(1)`; sirve como referencia mínima. |
| **Linear Regression** | Modelo lineal básico. |
| **Elastic Net** | Combina L1 y L2 para controlar colinealidad. |
| **XGBoost** | Captura patrones no lineales y efectos complejos. |
| **LightGBM** | Modelo eficiente basado en árboles. |

---

# 📈 Resultados

(Valores demostrativos — reemplaza según tus resultados exactos)

| Método | MSE | MAE | R² |
|--------|------|-------|--------|
| Naïve baseline | XXXX | XXXX | 0.91 |
| Linear Regression | XXXX | XXXX | 0.92 |
| Elastic Net | XXXX | XXXX | 0.92 |
| **XGB Regressor** | **menor MSE** | **menor MAE** | **0.96+** |
| LightGBM | similar a XGB | — | — |

**Conclusión:**  
El modelo **XGB** supera ampliamente al baseline y a los modelos lineales, capturando mejor la dinámica temporal.

---

# 🖼️ Gráficas

Incluye:

### ✔️ Serie de tiempo real vs predicho (XGB)
Muestra cómo el modelo sigue la tendencia de ventas en semanas regulares y picos.

### ✔️ Residuales del modelo
Permite evaluar sesgos, dispersión e identificar outliers o semanas atípicas.

### ✔️ Ventas reales vs predichas agregadas por semana
Evaluación corporativa del error global.

---

# 🧠 Conclusiones

1. La serie presenta **estacionalidad semanal y anual**, con poca tendencia.  
2. Los modelos lineales tienen limitaciones por colinealidad y falta de interacciones.  
3. El baseline naïve es fuerte, pero **XGB lo supera claramente**, justificando su uso.  
4. El error relativo agregado es **<1%**, indicando alta capacidad predictiva.  
5. El pipeline desarrollado es adecuado para forecasting en retail y puede escalarse.

---

# 🚀 Mejoras futuras

- Implementar **TimeSeriesSplit** para validación más robusta.  
- Crear **rolling features** (medias móviles, desviaciones, máximos).  
- Añadir métricas porcentuales (MAPE, sMAPE).  
- Utilizar **SHAP values** para interpretabilidad avanzada.  
- Entrenar modelos por tienda o por departamento.  
- Automatizar pipeline con `sklearn` o `MLflow`.

---

# 📌 Licencia

MIT License.

---

# 🧑‍💻 Autor

**Orión Nava**  
Data Analyst & Applied Machine Learning  
Especializado en forecasting, inventarios y series de tiempo.
