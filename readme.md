
# Trabajo Práctico Integrador - Ciencia de Datos & Diagnóstico Organizacional

## 📍 Caso de Negocio y Elección del Dataset

El dataset seleccionado (`01_company_info.csv`) contiene información financiera, operativa, sectorial y de gobernanza de 510 empresas de gran capitalización (S&P 500 y principales activos globales).

**Justificación (5 líneas):**
Este dataset permite abordar decisiones estratégicas en instituciones financieras y fondos de inversión. Contiene variables cuantitativas (métricas de valoración, rentabilidad, deuda y volumen) y cualitativas (sector, industria, riesgo de gobernanza) indispensables para evaluar la salud financiera corporativa. Su nivel de granularidad permite desarrollar modelos predictivos de clasificación/regresión (por ejemplo, predecir el rating de recomendación de analistas o el riesgo financiero) alineados con el Core Business del sector financiero.

---

## 🏢 Contexto Organizacional & Cadena de Valor

* **Tipo de Organización:** Fondo de Inversión / Firma de Asesoría Financiera Privada (Private Equity / Investment Management).
* **Cadena de Valor de Porter Aplicada:**
  * **Infraestructura de la empresa:** Plataforma de análisis de datos y stack analítico.
  * **Gestión de RRHH:** Capacitación de analistas financieros en herramientas *Data-Driven*.
  * **Desarrollo Tecnológico:** Captura automática de APIs (Yahoo Finance), pipelines de ETL y modelos analíticos predictivos.
  * **Aprovisionamiento:** Ingesta y licenciamiento de datos de mercado en tiempo real.
  * **Operaciones Internas:** Procesamiento, auditoría y modelado predictivo del rendimiento bursátil.
  * **Marketing y Ventas:** Informes ejecutivos y comités de inversión para clientes institucionales.
* **Origen Real de los Datos (Data Origin):**
  * Métricas de cotización/volumen: Sistemas de Trading / Fix Protocol.
  * Métricas contables (ROE, Deuda, Margen): Reportes trimestrales SEC (Form 10-K y 10-Q).
  * Riesgos de gobernanza: Agencias de calificación y juntas de accionistas.

---

## 🔍 Diagnóstico de Capas (Capa 1 vs Capa 2)

### Capa 1: Diagnóstico de Problema Actual (Lo que funciona mal)

* **Cultura de datos:** Decisiones tomadas por "intuición del analista sénior" o costumbre, sin validación cuantitativa rigurosa.
* **Calidad de datos:** Datos faltantes en empresas en crecimiento, duplicidad de tickers y desfasaje temporal en indicadores de liquidez.
* **Stack actual:** Planillas de cálculo descentralizadas (Excel), sin repositorio central (*Single Source of Truth*).

### Capa 2: Propuesta de Oportunidad (Lo que no se está haciendo)

* Desarrollo de un **Data Warehouse centralizado** e integración de **Modelos de Machine Learning + LLMs** para análisis del sentimiento de reportes contables y scoring automático de recomendación de inversión.

### 🎯 Priorización y Criterio de Negocio

**Prioridad:** Se decide atender primero la **Capa 1 (Calidad y Gobierno de Datos)**.

* *Justificación:* Entrenar modelos sofisticados o integrar LLMs (Capa 2) sobre planillas dispersas y datos sin auditar genera el riesgo de "Garbage In, Garbage Out". Garantizar la consistencia del stack y la calidad de los datos es la condición indispensable antes de escalar hacia analítica avanzada.

---

## 📊 Auditoría de Calidad de Datos (6 Dimensiones)

1. **Completes (Completitud):** Identificación de columnas con alto % de nulos (`dividendRate`, `fitytwoWeekChange`).
2. **Accuracy (Exactitud):** Verificación de márgenes y ratios financieros en rangos verosímiles.
3. **Consistency (Consistencia):** Coherencia entre `currentPrice`, `fiftyTwoWeekLow` y `fiftyTwoWeekHigh`.
4. **Timeliness (Oportunidad):** Fechas de corte homogéneas en reportes trimestrales.
5. **Uniqueness (Unicidad):** Ausencia de duplicados por la variable clave `symbol`.
6. **Validity (Validez):** Formatos de datos correctos (numéricos en precios, strings en sectores).

* **Clasificación de Faltantes:**
  * **MCAR (Missing Completely at Random):** `irWebsite` (sitio web de inversores no registrado).
  * **MAR (Missing at Random):** `dividendRate` / `dividendYield` (faltante condicionado a que la empresa no reparte dividendos, por ej. tecnológicas).
  * **MNAR (Missing Not at Random):** `debtToEquity` o métricas en empresas privadas/recientes que no revelan estructura de deuda por política interna.

---

## 📈 Traducción de KPI de Negocio a Data Science

* **KPI de Negocio:** Maximizar la tasa de acierto en recomendaciones de compra/venta de activos (*Sharpe Ratio / Alpha del Portafolio*).
* **Tipo de Problema de DS:** Clasificación Multiclase (Predicción del `recommendationKey`: *buy, hold, sell*).
* **Métrica Técnica Sugerida:** **F1-Score Ponderado** y **ROC-AUC Multiclase** (dado el desbalance de clases en el mercado).
