# Caracterización de perfiles de pacientes diabéticos hospitalizados

Proyecto final — Minería de Datos  
Universidad de Antioquia · Facultad de Ingeniería

---

## Sobre el proyecto

Este trabajo analiza el [Diabetes 130-US Hospitals Dataset](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008) (Strack et al., 2014) para identificar perfiles diferenciados de pacientes diabéticos hospitalizados a partir de sus variables demográficas, clínicas y de uso de servicios de salud.

La pregunta que guía el análisis es:
> ¿Qué perfiles de pacientes diabéticos hospitalizados pueden identificarse a partir de sus características demográficas, clínicas y de uso de servicios de salud?

El enfoque es completamente **no supervisado**: no se parte de etiquetas predefinidas; los perfiles emergen del propio análisis.

---

## Dataset

- **101.766 registros** de encuentros hospitalarios en 130 hospitales de EE. UU. (1999–2008)
- **50 variables** por registro: demográficas, clínicas, de uso hospitalario e historial de servicios
- Disponible en el [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)
- Archivo esperado: `diabetic_data.csv` en la raíz del entorno de ejecución

---

## Metodología

El análisis sigue cinco fases secuenciales:

1. **Preprocesamiento** — exclusión de variables con >80% de ausencias (`weight`, `max_glu_serum`, `A1Cresult`), imputación por moda para `race`, codificación ordinal de `age`, `readmitted` e `insulin`, y codificación binaria de `gender`.

2. **EDA** — análisis univariado y bivariado de las principales variables numéricas y categóricas; matrices de correlación.

3. **Detección de outliers** — tres métodos complementarios: IQR, Z-score (|z| > 3) e Isolation Forest. Los valores atípicos se conservan en el análisis porque pueden representar los casos más críticos.

4. **Reducción de dimensionalidad** — PCA sobre 12 variables seleccionadas; se retienen 9 componentes (>80% de varianza explicada).

5. **Clustering** — K-Means sobre el espacio PCA con k ∈ {2,…,8}; selección de k = 2 por maximizar el índice de silueta (0.1558).

---

## Resultados

Se identificaron dos perfiles clínicamente diferenciados:

| | Clúster 0 · Baja complejidad | Clúster 1 · Alta complejidad |
|---|---|---|
| Pacientes | 62.111 (61%) | 39.655 (39%) |
| Días hospitalización | ≈ 3 | ≈ 6.6 |
| Medicamentos | ≈ 12 | ≈ 22 |
| Procedimientos de lab. | ≈ 36 | ≈ 54 |
| Hosp. previas | 0.39 | 1.02 |
| Reingreso < 30 días | 8% | 17% |

La variable **edad** no diferencia entre clústeres, lo que indica que la segmentación responde a la intensidad del uso de recursos y a la trayectoria clínica previa, no a factores demográficos.

---

## Estructura del repositorio

```
├── Proyecto_final_V3.ipynb   # Notebook principal con todo el análisis
├── diabetic_data.csv         # Dataset (no incluido, ver enlace arriba)
├── images/                   # Figuras generadas durante el análisis
└── README.md
```

---

## Cómo ejecutar

El notebook está diseñado para correr en **Google Colab** con GPU/CPU estándar.

```
1. Subir diabetic_data.csv al entorno de Colab (/content/)
2. Abrir Proyecto_final_V3.ipynb
3. Ejecutar todas las celdas en orden (Runtime > Run all)
```

Dependencias (instaladas por defecto en Colab):

```
pandas · numpy · matplotlib · seaborn · scikit-learn · scipy
```

---

## Autores

María Daniela Rodríguez Chacón · `mdaniela.rodriguez@udea.edu.co`  
David Agudelo Ochoa · `david.agudeloo@udea.edu.co`

Universidad de Antioquia — 2025
