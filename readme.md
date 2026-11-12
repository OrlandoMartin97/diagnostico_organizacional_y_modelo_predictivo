
# 📊 Trabajo Práctico Integrador: Diagnóstico Organizacional & Modelo Predictivo

**Alumno / Integrante:** Martin Orlando
**Materia:** Ciencia de Datos / Análisis de Datos
**Fecha de Entrega:** Noviembre 2026
**Dataset:** `01_company_info.csv` (510 registros de empresas bursátiles)

---

## 📌 1. Caso de Negocio y Elección del Dataset

* **Justificación de la elección:**
  El dataset `01_company_info.csv` contiene información financiera, operativa, sectorial y de gobernanza de 510 empresas de gran capitalización bursátil. Su elección responde a la necesidad de resolver un problema real de toma de decisiones en instituciones de inversión. La granularidad de sus variables (métricas de valoración como P/E, endeudamiento, rentabilidad ROE y gobernanza) permite estructurar un modelo analítico de scoring predictivo para predecir la recomendación de analistas (`recommendationKey`), reemplazando la intuición por un proceso estructurado *Data-Driven*.

---

## 🏢 2. Contexto Organizacional & Cadena de Valor

* **Tipo de Organización:** Firma Privada de Gestión de Activos y Asesoría Financiera (Investment Management / Private Equity).
* **Cadena de Valor de Porter Aplicada:**
  * **Infraestructura:** Stack de datos analíticos y repositorio centralizado.
  * **Gestión de Recursos Humanos:** Formación de analistas financieros en Python, SQL y Machine Learning.
  * **Desarrollo Tecnológico:** Pipelines automáticos de ingesta (APIs financieras) y modelos predictivos.
  * **Aprovisionamiento (Procurement):** Licenciamiento e ingesta de feeds de datos bursátiles en tiempo real.
  * **Operaciones Internas:** Auditoría de calidad de datos, Feature Engineering y entrenamiento de modelos.
  * **Marketing y Ventas:** Informes ejecutivos automatizados y comités de inversión para clientes.
* **Origen Real de los Datos (Data Origin):**
  * *Precios y volúmenes:* Feeds de mercado bursátil (NYSE / NASDAQ).
  * *Métricas contables (ROE, Deuda, Margen):* Reportes trimestrales SEC (Form 10-K y 10-Q).
  * *Riesgo de Gobernanza:* Agencias de calificación y actas de asambleas.

---

## 🔍 3. Diagnóstico de las Dos Capas y Criterio de Priorización

### Capa 1: Diagnóstico del Estado Actual (Problemas existentes)

* **Cultura de datos:** Tomas de decisión basadas en "costumbre" o la intuición de analistas sénior, sin validación estadística.
* **Calidad de datos:** Presencia de valores nulos significativos (ej. `dividendRate` con 99.2% de faltantes) y falta de estandarización en reportes.
* **Stack / Data Warehouse:** Uso disperso de planillas Excel sin una fuente única de la verdad (*Single Source of Truth*).

### Capa 2: Propuesta de Oportunidad (Nuevas capacidades)

* Implementación de un **Data Warehouse analítico**, modelos de Machine Learning (Random Forest) para scoring automático de acciones e integración de **LLMs (Asistente Consultor)** para resolver preguntas en lenguaje natural sobre el portfolio.

### 🎯 Justificación del Criterio de Priorización (Evaluación con Peso)

> **Decisión:** Se atiende en primera instancia la **Capa 1 (Calidad y Gobierno de Datos)** antes de desplegar la Capa 2.
> **Argumentación:** Aplicar modelos de Machine Learning o asistentes basados en LLM sobre un repositorio con inconsistencias y sin gobierno de datos genera el riesgo de *"Garbage In, Garbage Out"*. Asegurar la integridad de la auditoría y el tratamiento de faltantes es el requisito biunívoco antes de desplegar analítica avanzada.

---

## 📊 4. Auditoría de Calidad de Datos (6 Dimensiones)

1. **Completitud (Completeness):** Identificación de variables críticas con nulos (ej. `dividendRate` 99.2%, `irWebsite` 35.7%, `trailingPE` 7.06%).
2. **Unicidad (Uniqueness):** Se verificó la inexistencia de registros duplicados por la clave `symbol` (0 duplicados).
3. **Exactitud (Accuracy):** Ratios financieros y márgenes validados dentro de rangos operativos normales.
4. **Consistencia (Consistency):** Coherencia lógica entre la capitalización de mercado y el volumen de acciones.
5. **Oportunidad (Timeliness):** Homogeneidad en los periodos fiscales de las empresas analizadas.
6. **Validez (Validity):** Tipos de datos adecuados (variables numéricas vs. cualitativas).

* **Clasificación de Faltantes:**
  * **MCAR (Missing Completely at Random):** `irWebsite` (sitios web de inversores no cargados de forma aleatoria).
  * **MAR (Missing at Random):** `dividendRate` / `fiveYearAvgDividendYield` (faltante condicionado a que la empresa pertenezca a un sector que no distribuye dividendos, ej. tecnología). Se solucionó imputando la mediana por sector.
  * **MNAR (Missing Not at Random):** `debtToEquity` en empresas con estructuras de deuda no informadas intencionalmente.

---

## 📈 5. Traducción de KPI de Negocio a Data Science

* **KPI de Negocio:** Incrementar el rendimiento (*Sharpe Ratio*) del portafolio mediante la tasa de aciertos en la selección de acciones para compra.
* **Problema de Data Science:** Clasificación Binaria (`1 = Buy / Strong Buy`, `0 = Hold / Sell`).
* **Métrica Técnica:** **ROC-AUC** y **F1-Score Ponderado**, elegidos por el desbalance de clases (80.4% de recomendaciones positivas frente a 19.6% negativas).

---

## 🛠️ 6. Resumen de Resultados del Notebook y Modelado

| Etapa                             | Descripción / Resultado Real                                                                                                                                      |
| :-------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Registros procesados**    | 509 empresas (luego de remover registros sin Target).                                                                                                              |
| **Feature Engineering**     | 1. `debt_to_marketcap` (Ratio Deuda/Cap. Bursátil)2. `pe_growth_ratio` (Forward P/E vs Trailing P/E)3. `company_size` (Categorización Small / Mid / Large) |
| **Baseline (Dummy)**        | **ROC-AUC:** `0.500` (Clase mayoritaria)                                                                                                                   |
| **Regresión Logística**   | **ROC-AUC:** `0.702`                                                                                                                                       |
| **Random Forest (Elegido)** | **ROC-AUC:** `0.786` (Supera ampliamente al baseline)                                                                                                      |
| **Módulo LLM**             | Función `asistente_llm_financiero()` que responde preguntas en lenguaje natural inyectando métricas dinámicas del dataset.                                    |

---

## 🗺️ 7. Mapeo entre Consignas y Celdas del Notebook

Para facilitar la revisión por parte del docente, a continuación se detalla la correspondencia entre las consignas y el archivo `.ipynb`:

* **Requisito 1 & 2 (EDA & Auditoría):** Celdas 1 a 5 (`Importación`, `EDA Inicial`, `Auditoría` y `Limpieza`).
* **Requisito 3 (Ingeniería de Variables):** Celda 6 (`debt_to_marketcap`, `pe_growth_ratio`, `company_size`).
* **Requisito 4 (Preprocesamiento & Modelado):** Celdas 7 u 8 (`Train/Test split`, `StandardScaler`, `Baseline`, `LogReg` y `RandomForest`).
* **Requisito 5 (Consulta con LLM):** Celda 11 (`asistente_llm_financiero()`).
