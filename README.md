# 📊 Credit Risk Analytics: Pipeline de Machine Learning & Dashboard en Power BI

Solución integral de riesgo crediticio (*credit risk*) que combina ingeniería de datos, modelos predictivos en Python para estimar la probabilidad de incumplimiento (*default*) y un tablero interactivo en Power BI diseñado para la toma de decisiones financieras y control del capital.

---

## 🎯 Preguntas Guía de Negocio

### 1. Análisis Descriptivo (¿Qué pasó en la cartera?) 🏛️
* **Exposición y Pérdida:** ¿Cuál es la tasa de mora histórica y qué volumen total de dinero representó esa pérdida para la entidad?
* **Segmentación del Riesgo:** ¿Qué perfiles concentraron el mayor incumplimiento (según grado del préstamo, destino del crédito, nivel de ingresos o antigüedad laboral)?
* **Rentabilidad vs. Riesgo:** ¿La tasa de interés aplicada compensó adecuadamente el nivel de riesgo de los clientes?

### 2. Análisis Predictivo (¿Qué va a pasar con Machine Learning?) 🤖
* **Capacidad de Detección:** ¿Qué porcentaje de morosos reales logra predecir el algoritmo antes de otorgar el crédito?
* **Impacto de la Decisión:** ¿Cuántas solicitudes se aprueban o rechazan según la política de crédito actual?
* **Costo de Oportunidad vs. Protección de Capital:** ¿Cuánto capital en mora potencial se evita prestar frente a cuánto dinero se deja de colocar por clientes solventes rechazados?

---

## 🤖 Estrategia de Modelado y Resultados

El problema se abordó como una **clasificación binaria** supervisada sobre la variable objetivo `loan_status` ($0$ = Cumplidor, $1$ = Moroso), priorizando la protección del capital frente a préstamos incobrables.

### Comparativa de Modelos y Ajuste de Política

| Modelo | Umbral de Corte | Precision (Mora) | Recall (Mora) | F1-Score | Accuracy Global | Diagnóstico de Negocio |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **Regresión Logística (Baseline)** | 0.50 | 0.73 | 0.49 | 0.59 | 0.85 | Insuficiente: 51% de los morosos no fue detectado (723 fugas de capital). |
| **Random Forest (Estándar)** | 0.50 | **0.98** | 0.71 | **0.82** | **0.93** | Excelente precisión, pero deja escapar a un 29% de deudores. |
| **Random Forest (Class Weight Balanced)** | 0.50 | 0.91 | 0.72 | 0.81 | 0.92 | El balanceo forzado penalizó precisión sin mover el recall significativamente. |
| **Random Forest (Política Calibrada)** | **0.35** | 0.90 | **0.75** | 0.81 | **0.93** | **Modelo Seleccionado:** Detecta al 75% de los morosos (1.066 casos) con 90% de confiabilidad. |

> **Criterio de Negocio (Umbral 0.35):** En gestión de riesgo financiero, esperar a una certeza del 50% para frenar un préstamo es imprudente. Fijar el corte en el 35% de probabilidad de mora permitió interceptar a cientos de deudores adicionales antes del desembolso, manteniendo una tasa de falsas alarmas extremadamente baja (9 de cada 10 solicitudes frenadas son morosos reales).

---
## 🛠️ Stack Tecnológico

* **Entorno y Lenguaje:** Python 3.14 en VS Code (Linux).
* **Ciencia de Datos & ML:**
  * `pandas` & `numpy`: Limpieza, transformación y manipulación tabular.
  * `scikit-learn`: Partición estratificada (80/20), algoritmos de clasificación, calibración de umbrales con `predict_proba()` y métricas de desempeño.
* **Business Intelligence:**
  * `Power BI`: Modelado semántico, medidas operativas en DAX y visualización interactiva de KPIs de cartera.

---

## 🔄 Arquitectura del Pipeline

```text
[ Dataset Histórico (Crudo) ]
             │
             ▼
[ EDA, Limpieza y Tratamiento de Outliers ]
             │
             ▼
[ Partición Estratificada (Train 80% / Test 20%) ]
             │
             ▼
[ Entrenamiento Random Forest & Calibración de Umbral (0.35) ]
             │
             ▼
[ Exportación: dataset_creditos_scoreado.csv ]
  (Incluye: probabilidad_mora, prediccion_mora y decision_credito)
             │
             ▼
[ Dashboard Ejecutivo en Power BI (Seguimiento & KPIs de Riesgo) ]

```


## 📁 Estructura del Proyecto
```text
credit_risk_analytics/
├── dashboards/
│   ├── credit_risk_dashboard.pbix    # Reporte interactivo de Power BI
│   └── screenshots/                  # Capturas del panel ejecutivo
├── data/
│   ├── raw/                          # Dataset original (credit_risk_dataset.csv)
│   └── processed/                    # Dataset final scoreado (dataset_creditos_scoreado.csv)
├── notebooks/
│   ├── 01_eda_credit_data.ipynb      # Limpieza, imputaciones y análisis exploratorio
│   └── 02_modelado_evaluacion.ipynb  # Baseline, Random Forest, umbral y exportación
├── README.md
└── .gitignore
```