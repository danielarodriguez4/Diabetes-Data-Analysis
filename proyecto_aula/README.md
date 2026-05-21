# Comprensión y Contextualización del Problema
## Unidades 1 y 2 — Análisis Exploratorio de Datos

## 1. Definición del propósito analítico

### 1.1 Planteamiento del problema

La diabetes mellitus representa una de las enfermedades crónicas de mayor impacto 
en los sistemas de salud a nivel mundial. Su manejo hospitalario implica altos costos, 
estancias prolongadas y reingresos frecuentes, lo que genera una carga significativa 
tanto para los pacientes como para las instituciones prestadoras de servicios de salud.

En este contexto, surge la siguiente pregunta analítica:

> **¿Qué perfiles de pacientes diabéticos hospitalizados pueden identificarse a partir 
> de sus características demográficas, clínicas y de uso de servicios de salud?**

Esta pregunta es de naturaleza **descriptiva y exploratoria**: no busca predecir un 
resultado específico, sino descubrir estructuras latentes en los datos que permitan 
comprender mejor la heterogeneidad de esta población.


### 1.2 Objetivo general

Caracterizar los perfiles de pacientes diabéticos hospitalizados mediante el análisis 
exploratorio de sus variables demográficas, clínicas y de uso hospitalario, con el fin 
de identificar patrones relevantes en su comportamiento.


### 1.3 Objetivos específicos

| # | Objetivo | Técnica asociada |
|---|----------|-----------------|
| 1 | Analizar la distribución de las variables demográficas y clínicas | Estadística descriptiva, histogramas, boxplots |
| 2 | Identificar relaciones entre variables clave (tiempo de hospitalización, medicamentos, uso previo de servicios) | Matrices de correlación, análisis bivariado |
| 3 | Detectar valores atípicos y evaluar su impacto en el análisis | IQR, Z-score, visualizaciones |
| 4 | Aplicar técnicas de reducción de dimensionalidad o agrupamiento | PCA, K-Means o clustering jerárquico |
| 5 | Interpretar los patrones desde una perspectiva clínica y operativa | Análisis cualitativo de clústeres |


### 1.4 Tipo de análisis

El presente proyecto corresponde a un análisis **no supervisado de tipo exploratorio**. 
A diferencia de los enfoques predictivos (donde se busca clasificar o estimar una 
variable objetivo conocida), aquí el propósito es descubrir agrupaciones naturales 
dentro de los datos sin una etiqueta preestablecida. Esto es pertinente dado que no se 
dispone de una definición a priori de qué constituye un "perfil" de paciente; esa 
definición emerge del propio análisis.


## 2. Descripción del conjunto de datos

### 2.1 Origen y contexto

El conjunto de datos utilizado corresponde al **Diabetes 130-US Hospitals Dataset** 
(Strack et al., 2014), disponible públicamente en el repositorio UCI Machine Learning 
Repository. Contiene registros de encuentros hospitalarios de pacientes con diagnóstico 
de diabetes en 130 hospitales de Estados Unidos durante el periodo **1999–2008**.

Cada fila representa un **encuentro hospitalario único**, con información recolectada 
al momento del ingreso y egreso del paciente.


### 2.2 Dimensiones del dataset

| Característica | Valor |
|----------------|-------|
| Número de registros | 101.766 |
| Número de variables | 50 |
| Periodo de recolección | 1999 – 2008 |
| Unidad de observación | Encuentro hospitalario |
| Fuente | UCI Machine Learning Repository |


### 2.3 Clasificación de variables

Las 50 variables del dataset pueden agruparse en cuatro categorías:

#### Variables demográficas
- `race`: raza del paciente
- `gender`: género
- `age`: rango de edad en intervalos de 10 años (ej. [50-60))

#### Variables clínicas
- `diag_1`, `diag_2`, `diag_3`: códigos de diagnóstico primario y secundarios (ICD-9)
- `max_glu_serum`: resultado de prueba de glucosa en suero
- `A1Cresult`: resultado de hemoglobina glicosilada
- Medicamentos: 23 variables binarias/categóricas indicando si se prescribió y si 
  hubo cambio de dosis para cada fármaco antidiabético (ej. `metformin`, `insulin`)

#### Variables de uso hospitalario
- `time_in_hospital`: días de hospitalización (1–14)
- `num_lab_procedures`: número de procedimientos de laboratorio
- `num_procedures`: número de procedimientos distintos realizados
- `num_medications`: número de medicamentos distintos administrados
- `number_diagnoses`: número de diagnósticos registrados

#### Variables de historial de servicios
- `number_outpatient`: visitas ambulatorias en el año previo
- `number_emergency`: visitas a urgencias en el año previo
- `number_inpatient`: hospitalizaciones en el año previo
- `readmitted`: indicador de reingreso (<30 días, >30 días, o sin reingreso)


### 2.4 Consideraciones sobre calidad de datos

Antes del análisis exploratorio, se anticipan los siguientes aspectos de calidad:

- **Valores faltantes**: variables como `race`, `max_glu_serum` y `A1Cresult` 
  presentan valores `?` o `None` que requieren tratamiento.
- **Variables de baja varianza**: varios medicamentos tienen una distribución muy 
  sesgada hacia "No" (no prescrito), lo que puede reducir su utilidad analítica.
- **Múltiples registros por paciente**: el dataset puede contener varios encuentros 
  del mismo paciente (`patient_nbr`). Dependiendo del objetivo, puede ser necesario 
  filtrar o agregar por paciente.
- **Codificación de diagnósticos**: los campos `diag_1`–`diag_3` contienen códigos 
  ICD-9, que deberán ser agrupados en categorías clínicas para su análisis.


  ## 3. Justificación del enfoque metodológico

### 3.1 ¿Por qué un enfoque exploratorio no supervisado?

La pregunta de investigación no postula una variable de respuesta conocida, sino que 
busca **descubrir estructura** en los datos. Esto descarta los enfoques supervisados 
(clasificación, regresión) como alternativa principal. En cambio, un análisis 
exploratorio combinado con técnicas de agrupamiento permite:

1. Describir la población de manera integral antes de plantear hipótesis específicas.
2. Identificar subgrupos clínicamente coherentes sin sesgos derivados de etiquetas 
   predefinidas.
3. Generar conocimiento accionable para la gestión hospitalaria (ej. personalización 
   de protocolos por perfil de paciente).

### 3.2 Etapas metodológicas

El análisis se estructura en las siguientes fases, alineadas con los objetivos 
específicos del proyecto:

Fase 1: Preparación y limpieza de datos
├── Identificación y tratamiento de valores faltantes
├── Codificación de variables categóricas
└── Filtrado de registros duplicados por paciente

Fase 2: Análisis Exploratorio de Datos (EDA)
├── Distribuciones univariadas (variables numéricas y categóricas)
├── Análisis bivariado y matrices de correlación
└── Detección y análisis de valores atípicos

Fase 3: Reducción de dimensionalidad
└── Análisis de Componentes Principales (PCA)

Fase 4: Identificación de perfiles (Clustering)
├── K-Means sobre componentes principales
└── Validación con índice de silueta

Fase 5: Interpretación clínica
└── Caracterización de cada clúster en términos médicos y operativos


### 3.3 Justificación de las técnicas seleccionadas

| Técnica | Justificación |
|---------|--------------|
| **Estadística descriptiva** | Permite una comprensión inicial de la distribución de cada variable y la detección de anomalías antes de aplicar modelos. |
| **Análisis de correlación** | El dataset mezcla variables numéricas continuas y ordinales; identificar sus relaciones evita redundancia en fases posteriores. |
| **PCA** | Con 50 variables (muchas categóricas codificadas), la reducción de dimensionalidad facilita la visualización y mejora la estabilidad del clustering. |
| **K-Means** | Algoritmo de partición eficiente para datasets de gran tamaño (>100K registros), con resultados interpretables y reproducibles. |
| **Índice de silueta** | Métrica estándar para evaluar la cohesión y separación de los clústeres obtenidos sin depender de etiquetas externas. |


### 3.4 Herramientas tecnológicas

El análisis se implementa en **Python 3** haciendo uso de las siguientes bibliotecas:

- `pandas` y `numpy`: manipulación y transformación de datos
- `matplotlib` y `seaborn`: visualización estadística
- `scikit-learn`: PCA, K-Means, métricas de validación
- `scipy`: pruebas estadísticas complementarias
