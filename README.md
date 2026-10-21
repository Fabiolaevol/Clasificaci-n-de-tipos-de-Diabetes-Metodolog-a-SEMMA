# 📊 Predicción del riesgo de diabetes mediante la metodología SEMMA

## 📌 Descripción del proyecto

Este proyecto aplica la metodología **SEMMA (Sample, Explore, Modify, Model y Assess)** para desarrollar y evaluar un modelo de clasificación supervisada multiclase capaz de clasificar el riesgo de diabetes en tres categorías: `Low`, `Moderate` y `High`.

El análisis se realizó sobre una base de datos de **15,000 registros**, utilizando un árbol de decisión implementado en R. El objetivo es identificar patrones en las variables demográficas, antropométricas, de estilo de vida y de medición clínica para predecir la categoría de riesgo registrada en los datos.

El proyecto se desarrolló para la materia de **Metodología de la Investigación**, de la Facultad de Estadística e Informática de la Universidad Veracruzana.

## 🎯 Objetivos

- Aplicar las cinco etapas de la metodología SEMMA a un problema de clasificación.
- Explorar y preparar los datos para su modelado.
- Construir un árbol de decisión multiclase utilizando el paquete `rpart`.
- Evaluar el desempeño del modelo mediante métricas de clasificación.
- Identificar las variables con mayor importancia predictiva.
- Analizar las limitaciones y posibles mejoras del modelo.

## 🧪 Metodología SEMMA

### 1. Sample — Muestreo

Se trabajó con una base de datos de 15,000 observaciones. No se detectaron registros duplicados.

La variable objetivo presenta la siguiente distribución:

| Categoría | Registros | Porcentaje |
|---|---:|---:|
| Low | 9,000 | 60 % |
| Moderate | 3,750 | 25 % |
| High | 2,250 | 15 % |
| **Total** | **15,000** | **100 %** |

Los datos se dividieron mediante `createDataPartition`, utilizando una semilla de 123 y una proporción de 70 % para entrenamiento y 30 % para prueba.

- Entrenamiento: 10,500 registros.
- Prueba: 4,500 registros.

### 2. Explore — Exploración

Se analizaron las distribuciones de las variables, los valores faltantes, los estadísticos descriptivos y las diferencias entre categorías de riesgo.

Los principales valores faltantes identificados fueron:

| Variable | Valores faltantes | Porcentaje |
|---|---:|---:|
| `smoking_status` | 472 | 3.15 % |
| `alcohol_consumption` | 3,788 | 25.25 % |
| `income_bracket` | 463 | 3.09 % |

Se observó que las medias de glucosa en ayunas (`fasting_blood_sugar`) y hemoglobina glucosilada (`hba1c_level`) aumentan de la categoría Low a High.

### 3. Modify — Modificación

El preprocesamiento incluyó las siguientes acciones:

- Revisión de registros duplicados.
- Eliminación de `patient_id`, por ser un identificador y no una característica predictiva.
- Sustitución de los valores faltantes de las variables categóricas por la categoría `Unknown`.
- Conversión de las variables categóricas y de la variable objetivo `diabetes_risk` a factores.

Este procedimiento permitió conservar las observaciones con información incompleta en las variables categóricas consideradas.

### 4. Model — Modelado

Se construyó un árbol de decisión multiclase mediante el paquete `rpart` de R.

Los parámetros iniciales fueron:

| Parámetro | Valor |
|---|---|
| `method` | `class` |
| `cp` | `0.001` |
| `minsplit` | `20` |
| `maxdepth` | `4` |
| `xval` | `10` |

Se utilizó validación cruzada interna para analizar el error relativo en función de la complejidad del árbol y seleccionar el parámetro de complejidad (`cp`).

El modelo final conservó **11 hojas terminales**. Entre las variables utilizadas en las divisiones del árbol se encuentran la glucosa en ayunas, la hemoglobina glucosilada, la edad y la circunferencia de cintura.

### 5. Assess — Evaluación

La evaluación se realizó sobre las 4,500 observaciones reservadas para prueba.

| Métrica | Resultado |
|---|---:|
| Accuracy | 77.76 % |
| Intervalo de confianza del 95 % | 76.51 % – 78.96 % |
| Kappa de Cohen | 0.5957 |
| Clasificaciones correctas | 3,499 de 4,500 |

#### Resultados por categoría

| Categoría | Sensibilidad | Precisión | F1-score | Exactitud balanceada |
|---|---:|---:|---:|---:|
| High | 71.56 % | 77.40 % | 74.36 % | 83.93 % |
| Low | 88.15 % | 86.61 % | 87.37 % | 83.85 % |
| Moderate | 56.53 % | 56.38 % | 56.46 % | 70.98 % |

Los resultados muestran un mejor desempeño en la clasificación de las categorías Low y High. La categoría Moderate representa el principal desafío del modelo, debido a su menor sensibilidad y precisión.

## 🔎 Importancia de las variables

Las variables con mayor importancia predictiva en el árbol fueron:

| Variable | Importancia relativa |
|---|---:|
| `fasting_blood_sugar` | 54.62 % |
| `hba1c_level` | 35.51 % |
| `age` | 3.11 % |
| `waist_circumference_cm` | 2.60 % |
| `bmi` | 2.36 % |

Las variables `fasting_blood_sugar` y `hba1c_level` concentran conjuntamente el **90.13 % de la importancia total calculada**.

Estos resultados representan la contribución predictiva relativa de las variables dentro del modelo; no demuestran por sí mismos una relación causal.

## 🛠️ Tecnologías y herramientas

- **R:** lenguaje utilizado para el análisis estadístico y el modelado.
- **rpart:** construcción y poda del árbol de decisión.
- **caret:** partición de datos y evaluación del clasificador.
- **Metodología SEMMA:** marco de trabajo para organizar el proceso de minería de datos.

## 📂 Estructura sugerida del repositorio

```text
SEMMA-Diabetes-Risk/
├── README.md
├── data/
│   └── README.md
├── scripts/
│   └── semma_diabetes.R
├── results/
│   ├── arbol_decision.png
│   ├── matriz_confusion.png
│   └── importancia_variables.png
└── docs/
    └── SEMMA_FLV_JIOB_JLOL.pdf
```

> **Nota:** Esta estructura es una propuesta para organizar el repositorio. Los nombres de los scripts y archivos de resultados son ilustrativos; deben ajustarse a los archivos que realmente se incorporen al proyecto.

## 🚀 Instalación y ejecución

Para reproducir el análisis, es necesario contar con R, la base de datos utilizada y el código fuente del proyecto.

Instala los paquetes empleados:

```r
install.packages(c("rpart", "caret"))
```

Después, abre el script de análisis en R o RStudio y verifica las rutas de acceso a los datos antes de ejecutarlo.

**Importante:** el documento de referencia describe el procedimiento y sus resultados, pero no incluye el código fuente completo. Por ello, las instrucciones anteriores preparan el entorno, pero no constituyen por sí solas una reproducción integral del modelo.

## 📈 Conclusiones

La aplicación de SEMMA permitió estructurar el proceso de análisis desde la exploración y preparación de los datos hasta la evaluación del modelo.

El árbol de decisión alcanzó un accuracy de 77.76 % y un Kappa de 0.5957 en el conjunto de prueba. Las variables relacionadas con la glucosa en ayunas y la hemoglobina glucosilada fueron las de mayor importancia predictiva.

Sin embargo, la clasificación de la categoría Moderate continúa siendo un área de oportunidad. Entre las posibles líneas de mejora se encuentran evaluar otras técnicas de clasificación, estudiar el desequilibrio entre clases y realizar validaciones adicionales.

**Este modelo tiene fines académicos y analíticos. No constituye una herramienta de diagnóstico médico ni debe utilizarse para tomar decisiones clínicas sin validación independiente.**

## 📚 Referencias

- Breiman, L., Friedman, J. H., Olshen, R. A., & Stone, C. J. (1984). *Classification and Regression Trees*. Wadsworth.
- Therneau, T., Atkinson, B., & Ripley, B. *rpart: Recursive Partitioning and Regression Trees*.
- Kuhn, M. *caret: Classification and Regression Training*.
- SAS Institute. *SEMMA: Sample, Explore, Modify, Model, Assess*.
- Aggarwal, Mansi. *Diabetes Risk Prediction*. Kaggle: https://www.kaggle.com/datasets/mansiaggarwal88/diabetes-risk-prediction

## 👥 Autores

Proyecto realizado en la Universidad Veracruzana, Facultad de Estadística e Informática.

- Fabiola López Velázquez
- Joshua Iván Olivares Blanco
- Jorge Luis Ortiz Libreros

**Materia:** Metodología de la Investigación  
**Actividad:** Metodología SEMMA  
**Profesora:** Lorena López Lozada  
**Fecha del documento:** 28 de septiembre de 2026

---

⭐ Si este proyecto te resulta útil para estudiar la metodología SEMMA, el análisis exploratorio de datos o los árboles de decisión, puedes utilizarlo como referencia académica, respetando las fuentes originales.
