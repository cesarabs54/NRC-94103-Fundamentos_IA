# Funciones utilizadas en `EIARV011_A4_analisis_credits.ipynb`

Este documento resume, en lenguaje claro, las funciones y métodos usados en el análisis del dataset `credits.csv` (Netflix TV Shows and Movies) en la actividad de la semana 4 para que puedas entender qué hace cada uno y por qué aparece en el análisis.

## 1) Funciones de importación y configuración

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `pd.set_option()` | Cambia una opción global de pandas. | Muestra todas las columnas en pantalla con `display.max_columns = None`. | `pd.reset_option()` |
| `sns.set_theme()` | Define el estilo visual de Seaborn. | Aplica el tema `whitegrid` a las gráficas. | `sns.set_style()` |
| `pd.read_csv()` | Lee un archivo CSV y lo convierte en un DataFrame. | Carga `credits.csv` en `df`. | `pd.read_table()` |

## 2) Funciones básicas de inspección y limpieza

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `df.head()` | Muestra las primeras filas del DataFrame. | Revisa rápidamente la estructura de `credits.csv`. | `df.sample(5)` |
| `print()` | Imprime texto o resultados en pantalla. | Muestra conteos, resúmenes y conclusiones. | `display()` |
| `df.info()` | Muestra tipos de datos, nulos y memoria usada. | Sirve para reconocer la estructura del dataset. | `df.describe(include="all")` |
| `df.isnull()` | Detecta valores faltantes. | Se usa para contar nulos por columna. | `df.isna()` |
| `sum()` / `.sum()` | Suma valores numéricos o booleanos. | Cuenta nulos y duplicados. | `np.count_nonzero()` |
| `df.duplicated()` | Marca filas repetidas. | Detecta registros duplicados. | `df.drop_duplicates()` |
| `fillna()` | Rellena valores faltantes. | Sustituye `NaN` en `character` por `"Sin especificar"`. | `replace()` |
| `astype()` | Convierte el tipo de una columna. | Cambia `role` a tipo categórico. | `pd.Categorical()` |

## 3) Funciones de agrupación y creación de variables

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `df.groupby()` | Agrupa datos por una o varias columnas. | Calcula métricas por `id` y por `person_id`. | `pd.pivot_table()` |
| `nunique()` | Cuenta valores únicos. | Mide cuántas personas hay por título y cuántos títulos tiene cada persona. | `unique()` + `len()` |
| `rename()` | Cambia el nombre de una serie o columna. | Da nombres como `tamano_reparto` y `titulos_por_persona`. | `set_axis()` |
| `agg()` | Aplica una agregación personalizada. | Calcula el rol principal de cada persona. | `apply()` |
| `lambda` | Función anónima breve. | Se usa para definir `lambda x: x.value_counts().idxmax()`. | `def` con nombre |
| `value_counts()` | Cuenta cuántas veces aparece cada valor. | Determina el rol más frecuente por persona y la distribución de roles. | `groupby().size()` |
| `idxmax()` | Devuelve la etiqueta del valor máximo. | Encuentra el rol más repetido. | `sort_values().index[0]` |
| `pd.DataFrame()` | Crea un DataFrame nuevo desde estructuras existentes. | Construye `personas`. | `pd.concat()` |
| `unstack()` | Convierte una agrupación jerárquica en tabla. | Pasa de filas `id/role` a columnas `actors` y `directors`. | `pivot()` |
| `head()` | Muestra las primeras filas. | Se usa para revisar resultados intermedios. | `tail()` |

## 4) Funciones de NumPy y operaciones vectorizadas

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `np.array()` | Convierte datos en arreglo NumPy. | Crea `arr_reparto`. | `np.asarray()` |
| `type()` | Indica el tipo de un objeto. | Verifica que `arr_reparto` sea un `ndarray`. | `isinstance()` |
| `np.mean()` | Calcula la media. | Obtiene el promedio del tamaño de reparto. | `statistics.mean()` |
| `np.median()` | Calcula la mediana. | Resume la tendencia central. | `statistics.median()` |
| `np.percentile()` | Calcula percentiles. | Extrae percentiles 25, 50, 75, 90 y 95. | `np.quantile()` |
| `.mean()` | Media de un arreglo o serie. | Se usa en el z-score manual. | `np.average()` |
| `.std()` | Desviación estándar. | Completa la estandarización manual. | `statistics.pstdev()` |
| `np.sum()` | Suma elementos de un arreglo. | Cuenta cuántos títulos tienen z-score mayor que 3. | `.sum()` |
| `np.unique()` | Devuelve valores únicos y, opcionalmente, sus conteos. | Resume la distribución de `role`. | `pd.Series.value_counts()` |
| `to_numpy()` | Convierte una serie a arreglo NumPy. | Prepara `role` para `np.unique()`. | `values` |
| `len()` | Devuelve la cantidad de elementos. | Se usa en tamaños de muestra y porcentajes. | `shape[0]` |
| `zip()` | Recorre dos secuencias en paralelo. | Imprime valores y conteos de `role`. | `enumerate()` |
| `np.abs()` | Devuelve el valor absoluto. | Detecta atípicos con `|z| > 3`. | `abs()` |
| `np.log1p()` | Calcula `log(1 + x)`. | Mejora la lectura de distribuciones sesgadas. | `np.log(x + 1)` |

## 5) Funciones estadísticas de SciPy

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `stats.shapiro()` | Prueba de normalidad de Shapiro-Wilk. | Evalúa si `tamano_reparto` se parece a una distribución normal. | `stats.normaltest()` |
| `stats.pearsonr()` | Correlación lineal de Pearson. | Relaciona número de actores y directores. | `np.corrcoef()` |
| `stats.spearmanr()` | Correlación de rangos de Spearman. | Mide relación monotónica entre actores y directores. | `pd.Series.corr(method="spearman")` |
| `stats.zscore()` | Calcula puntuaciones z estandarizadas. | Detecta títulos atípicos por tamaño de reparto. | Cálculo manual con media y desviación |
| `stats.ttest_ind()` | Prueba t para dos muestras independientes. | Compara `titulos_por_persona` entre actores y directores. | `stats.ttest_rel()` |
| `min()` | Devuelve el menor valor entre varios. | Limita el tamaño de la muestra para Shapiro-Wilk. | `np.minimum()` |
| `int()` | Convierte a entero. | Presenta el conteo final de atípicos. | `round()` + `int()` implícito |
| `round()` | Redondea un número. | Muestra resultados más legibles. | `np.round()` |

## 6) Funciones de Matplotlib

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `plt.figure()` | Crea una figura nueva. | Inicia cada gráfica. | `plt.subplots()` |
| `plt.hist()` | Dibuja un histograma. | Muestra la distribución de `tamano_reparto`. | `sns.histplot()` |
| `plt.scatter()` | Dibuja un diagrama de dispersión. | Compara actores vs. directores. | `sns.scatterplot()` |
| `plt.boxplot()` | Genera un boxplot. | Compara títulos por persona según rol principal. | `sns.boxplot()` |
| `plt.title()` | Asigna título a la gráfica. | Mejora la interpretación de cada figura. | `ax.set_title()` |
| `plt.xlabel()` | Etiqueta el eje X. | Describe la variable horizontal. | `ax.set_xlabel()` |
| `plt.ylabel()` | Etiqueta el eje Y. | Describe la variable vertical. | `ax.set_ylabel()` |
| `plt.xticks()` | Configura las marcas del eje X. | Quita la rotación en la gráfica de barras. | `ax.tick_params()` |
| `plt.tight_layout()` | Ajusta automáticamente espacios. | Evita que etiquetas se corten. | `fig.tight_layout()` |
| `plt.show()` | Muestra la gráfica final. | Renderiza cada visualización. | `plt.savefig()` |

## 7) Funciones de Seaborn

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `sns.histplot()` | Histograma moderno con opciones extra. | Muestra `log(1 + tamaño de reparto)`. | `sns.displot()` |
| `sns.violinplot()` | Combina densidad y boxplot. | Compara títulos por persona por rol. | `sns.boxplot()` |
| `sns.regplot()` | Dispersión con línea de regresión. | Analiza actores vs. directores. | `sns.lmplot()` |
| `sns.heatmap()` | Dibuja un mapa de calor. | Visualiza correlaciones numéricas. | `plt.imshow()` |

## 8) Funciones de pandas usadas sobre Series/DataFrames

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `.corr()` | Calcula correlación entre columnas numéricas. | Genera la matriz para el heatmap. | `np.corrcoef()` |
| `.plot(kind="bar")` | Dibuja una gráfica usando la interfaz de pandas. | Grafica la distribución de `role`. | `plt.bar()` |
| `.sample()` | Toma una muestra aleatoria. | Reduce el tamaño antes de aplicar Shapiro-Wilk. | `np.random.choice()` |
| `.sort_values()` | Ordena valores de una serie. | Muestra los títulos atípicos de mayor a menor. | `sorted()` |
| `.mean()` | Calcula promedio. | Resume `titulos_por_persona` por grupo. | `np.mean()` |

## 9) Idea general de cómo se usan juntas

En el notebook, las funciones se combinan así:

1. `pd.read_csv()` carga los datos.
2. `df.groupby()`, `nunique()`, `agg()` y `rename()` construyen variables resumen.
3. `np.mean()`, `np.percentile()`, `stats.zscore()` y `stats.ttest_ind()` realizan análisis estadístico.
4. `plt.*` y `sns.*` crean las gráficas para interpretar los resultados.

Si quieres, piensa en estas funciones como herramientas complementarias: **pandas** organiza los datos, **NumPy** hace cálculos rápidos, **SciPy** aporta pruebas estadísticas y **Matplotlib/Seaborn** presentan los resultados visualmente.
