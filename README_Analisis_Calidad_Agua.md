# Análisis de calidad del agua en India

Este repositorio contiene un cuaderno de análisis de calidad del agua usando datos de diferentes estados de la India. El trabajo incluye limpieza de datos, visualización de parámetros, cálculo del índice WQI, análisis geográfico y un modelo de predicción usando Keras.

La idea del proyecto es revisar cómo se comportan diferentes variables de calidad del agua y usar esa información para estimar el índice WQI, que resume el estado general del agua en un solo valor.

## Objetivo

Analizar la calidad del agua a partir de parámetros físico-químicos y microbiológicos, calcular el índice WQI y construir un modelo de predicción que permita aproximar dicho índice a partir de los rangos de calidad de los parámetros.

## Variables principales

El cuaderno trabaja principalmente con estas variables:

| Variable | Descripción |
|---|---|
| `DO` | Oxígeno disuelto |
| `pH` | Nivel de acidez o alcalinidad |
| `CONDUCTIVITY` | Conductividad del agua |
| `BOD` | Demanda bioquímica de oxígeno |
| `NITRATE_N_NITRITE_N` | Nitratos y nitritos |
| `FECAL_COLIFORM` | Coliformes fecales |
| `WQI` | Índice de calidad del agua |

Estas variables permiten analizar diferentes aspectos del agua, como oxigenación, presencia de materia orgánica, sales disueltas, contaminación microbiológica y compuestos nitrogenados.

## Flujo del proyecto

El cuaderno sigue este proceso general:

1. Carga de datos de calidad del agua.
2. Limpieza y tratamiento de valores faltantes.
3. Conversión y preparación de variables numéricas.
4. Creación de rangos de calidad para los parámetros.
5. Cálculo de pesos y del índice WQI.
6. Visualización de parámetros mediante gráficas.
7. Representación geográfica del WQI por estado.
8. Creación de un modelo de predicción con Keras.
9. Análisis de resultados y limitaciones.

## Análisis exploratorio

### DO y pH

El pH presenta un comportamiento relativamente estable, con valores cercanos a un rango neutro o ligeramente alcalino. En cambio, el oxígeno disuelto muestra más variabilidad, con caídas importantes y algunos picos altos. Esto indica que la oxigenación del agua cambia bastante entre registros, mientras que el pH se mantiene más constante.

### BOD y nitrógenos

El BOD presenta picos altos en varios registros, lo cual puede indicar presencia de materia orgánica en el agua. Los nitratos y nitritos se mantienen generalmente en valores más bajos, aunque aparecen picos aislados que podrían relacionarse con fertilizantes, aguas residuales o escorrentía agrícola.

### Conductividad y coliformes fecales

Los coliformes fecales presentan valores mucho más altos que la conductividad, por lo que dominan la escala de la gráfica. Esto permite identificar registros con posible contaminación microbiológica importante. Para mejorar la interpretación, sería recomendable graficar estas variables por separado o usar una escala logarítmica.

## Interpretación del WQI

En este cuaderno es importante tener claro que un valor menor de WQI representa mejor calidad del agua. Según la clasificación usada:

| Rango WQI | Interpretación |
|---:|---|
| `< 25` | Agua potable o de mejor calidad |
| `25 - 50` | Calidad aceptable o moderada |
| `50 - 75` | Calidad deficiente o afectada |
| `75 - 100` | Calidad más crítica |

Por esta razón, los estados con WQI alto no representan mejor calidad, sino una condición menos favorable del agua.

## Resultados principales

El mapa del WQI muestra que varios estados de la India se encuentran en rangos medios o altos del índice, especialmente entre 50 y 75. Esto sugiere que muchas regiones no se encuentran dentro del rango considerado como agua potable según la escala del cuaderno.

El gráfico de barras por estado confirma esta idea, ya que pocos estados presentan valores cercanos al rango menor a 25. En general, los resultados muestran que la calidad del agua varía bastante entre estados y que existen regiones que requieren un análisis más detallado.

## Modelo predictivo

Para la parte de modelado se usa una red neuronal con Keras. El modelo busca predecir el valor del WQI usando como entradas los rangos de calidad:

- `qrPH`
- `qrDO`
- `qrCOND`
- `qrBOD`
- `qrNN`
- `qrFecal`

La salida del modelo es el valor de `WQI`.

La división de datos se realiza con 80% para entrenamiento y 20% para prueba. En el cuaderno se tienen 534 registros totales, de los cuales 427 se usan para entrenamiento y 107 para prueba.

La red neuronal está compuesta por tres capas densas de 350 neuronas y una capa de salida lineal. El modelo tiene una alta capacidad de aprendizaje, pero también puede presentar riesgo de sobreajuste debido a que tiene muchos parámetros entrenables en comparación con la cantidad de datos disponibles.

## Limitaciones del modelo

Aunque el modelo logra disminuir rápidamente el error durante el entrenamiento, es importante analizarlo con cuidado. El WQI fue calculado previamente a partir de variables relacionadas con los mismos rangos de calidad usados como entrada. Por eso, el modelo puede estar aprendiendo una relación matemática ya definida, más que un patrón completamente independiente.

Además, la gráfica de predicción usada en el cuaderno mezcla los datos de entrada con las predicciones, por lo que no es suficiente para evaluar completamente el desempeño del modelo. Para mejorar el análisis, sería recomendable agregar:

- MSE sobre datos de prueba.
- MAE.
- RMSE.
- R².
- Gráfica de WQI real vs WQI predicho.
- Curva de pérdida con entrenamiento y validación.

## Recomendaciones de mejora

Para mejorar el cuaderno se recomienda:

1. Verificar que todas las columnas necesarias existan antes de calcular el WQI y entrenar el modelo.
2. Interpretar siempre el WQI teniendo claro que valores bajos significan mejor calidad del agua.
3. Agregar métricas de evaluación sobre los datos de prueba.
4. Comparar visualmente el WQI real contra el WQI predicho.
5. Revisar posible sobreajuste por la cantidad de parámetros de la red neuronal.
6. Graficar por separado variables con escalas muy diferentes, como conductividad y coliformes fecales.

## Conclusión

El cuaderno permite realizar un análisis completo de calidad del agua, integrando limpieza de datos, visualización, cálculo del índice WQI, mapas y modelado predictivo. Los resultados muestran que varios parámetros presentan valores extremos, especialmente BOD, coliformes fecales y oxígeno disuelto, lo cual puede indicar problemas de contaminación en algunos registros.

El WQI permite resumir estas condiciones y comparar los estados de la India, aunque debe interpretarse correctamente, ya que valores más bajos indican mejor calidad. El modelo de Keras funciona como una aproximación académica para predecir el WQI a partir de rangos de calidad, pero necesita una evaluación más completa para determinar si realmente generaliza bien con datos nuevos.

## Tecnologías usadas

- Python
- PySpark
- Pandas
- NumPy
- Matplotlib
- Seaborn
- GeoPandas
- Scikit-Learn
- Keras

## Archivos principales

- `Clean_ML_Water.ipynb`: cuaderno principal del análisis.
- Archivos `Indian_States`: shapefile usado para la visualización geográfica.
- Dataset de calidad del agua: datos base procesados en el cuaderno.

