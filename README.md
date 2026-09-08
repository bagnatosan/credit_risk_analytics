# 📊 Credit Risk Analytics: Pipeline de Machine Learning & Dashboard en Power BI

Este proyecto desarrolla una solución integral de análisis de riesgo crediticio (*credit risk*), combinando modelos predictivos en Python para estimar la probabilidad de mora (*default*) y un dashboard interactivo en Power BI orientado a la toma de decisiones de negocio.

---

## 🎯 Preguntas Guía de Negocio

1. **Tasa Base de Mora:** ¿Cuál es la proporción de clientes en mora (*default rate*) en la cartera crediticia actual?
2. **Factores Críticos de Riesgo:** ¿Qué impacto tienen la tasa de interés, el nivel de ingresos, el porcentaje del préstamo sobre los ingresos y la antigüedad crediticia en el riesgo de no pago?
3. **Calidad y Tratamiento del Dato:** ¿Qué patrones de valores nulos o atípicos (*outliers*) existen en los datos históricos y cómo deben imputarse y tratarse?
4. **Capacidad Predictiva:** ¿Qué algoritmo de Machine Learning clasifica con mayor precisión y sensibilidad a los perfiles de alto riesgo?
5. **Monitoreo Ejecutivo:** ¿Cómo disponibilizar los resultados y scores de riesgo en un panel dinámico para el equipo de créditos?

---

## 🤖 Estrategia de Modelado y Selección de Algoritmos

1. 🎯 **Problema de Clasificación Binaria**:
   * Dado que la variable target `loan_status` es binaria ($0$ = Cumplió, $1$ = Moroso), el pipeline utiliza algoritmos de clasificación supervisada.
2. 🏦 **Explicabilidad Regulatoria en Banca**:
   * Las entidades financieras exigen modelos auditables que justifiquen el rechazo de solicitudes crediticias. La **Regresión Logística (`LogisticRegression`)** ofrece coeficientes interpretables y probabilidades continuas transparentes.
3. 📏 **Regla de Oro: Modelo Base (*Baseline*) vs. Ensamble**:
   * Se entrena primero una **Regresión Logística** como *Baseline* de referencia mínima.
   * **Resultados Baseline**: Accuracy 85%, Precision 73%, pero con un **Recall bajo del 49% (723 Falsos Negativos)**, lo que significa que el 51% de los morosos no fue detectado a tiempo.
4. 🌳 **Progresión hacia Ensamble (Random Forest y Gradient Boosting)**:
   * Probar **Random Forest** (ensamble paralelo por votación) y **XGBoost / LightGBM** (ensamble secuencial por corrección de errores) para elevar el Recall y recortar las pérdidas por morosidad no detectada, sin requerir escalado de características en los árboles.

---

## 🛠️ Tecnologías Utilizadas

* **Lenguaje y Entorno:** Python 3.14 en VS Code (CachyOS / Linux).
* **Ingeniería de Datos y ML:**
  * 🐼 **`pandas` / `numpy`**: Limpieza profunda, preprocesamiento y transformaciones.
  * 🤖 **`scikit-learn`**: Modelado predictivo (clasificación), codificación de variables y métricas de desempeño.
  * 📈 **`matplotlib` / `seaborn`**: Análisis exploratorio visual inicial.
* **Business Intelligence & Visualización:**
  * 📊 **Power BI**: Modelado semántico, medidas en DAX y diseño del dashboard interactivo.

---

## 🔄 Arquitectura del Pipeline

```text
[ Dataset Raw ] ──> [ EDA & Limpieza en Python ] ──> [ Modelo ML & Scoring ]
                                                              │
                                                              ▼
                                                 [ Dataset Procesado (CSV/Parquet) ]
                                                              │
                                                              ▼
                                                 [ Dashboard Interactivo en Power BI ]
```

---

## 📁 Estructura del Repositorio

```text
credit_risk_analytics/
├── dashboards/
│   ├── credit_risk_dashboard.pbix    # Reporte interactivo de Power BI
│   └── screenshots/                  # Vistas previas del panel
├── data/
│   ├── raw/                          # Dataset original (credit_risk_dataset.csv)
│   └── processed/                    # Datos limpios y enriquecidos con scoring
├── notebooks/
│   ├── 01_eda_credit_data.ipynb      # Análisis exploratorio y limpieza (EDA)
│   └── 02_modelado_evaluacion.ipynb  # Entrenamiento, validación y exportación de ML
├── README.md
└── .gitignore
```
