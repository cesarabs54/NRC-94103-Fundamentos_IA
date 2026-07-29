# Funciones utilizadas en `EIARV011_A4_analisis_titles.ipynb`

Este documento resume, en lenguaje claro, las funciones y métodos usados en el análisis del dataset `titles.csv` (Netflix TV Shows and Movies) en la actividad de la semana 4 para que puedas entender qué hace cada uno y por qué aparece en el notebook.

## 1) Funciones de importación y configuración

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `import` | Carga una librería para poder usar sus funciones. | Importa `ast`, `numpy`, `pandas`, `scipy.stats`, `matplotlib.pyplot` y `seaborn`. | `from ... import ...` |
| `sns.set_theme()` | Define el estilo visual de Seaborn. | Aplica el tema `whitegrid` a todas las gráficas. | `sns.set_style()` |
| `pd.set_option()` | Cambia una opción global de pandas. | Muestra todas las columnas (`display.max_columns`) y amplía el ancho de impresión (`display.width`). | `pd.reset_option()` |
| `pd.read_csv()` | Lee un archivo (posiblemente comprimido) y lo convierte en un DataFrame. | Carga `titles.csv` indicando `compression="zip"`, porque el archivo viene comprimido. | `pd.read_table()` |

## 2) Funciones de inspección y calidad de datos

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `df.head()` | Muestra las primeras filas del DataFrame. | Revisa la estructura de `titles.csv` y de `data` tras seleccionar variables. | `df.sample(5)` |
| `df.shape` | Devuelve el número de filas y columnas. | Confirma el tamaño del dataset (5 850 filas). | `len(df)` |
| `df.info()` | Muestra tipos de datos, nulos y memoria usada. | Reconoce la estructura general de `titles.csv`. | `df.describe(include="all")` |
| `df.isnull()` | Detecta valores faltantes. | Cuenta nulos por columna, antes y después de la limpieza. | `df.isna()` |
| `.sum()` | Suma valores numéricos o booleanos. | Cuenta nulos por columna y filas con `runtime == 0`. | `np.count_nonzero()` |
| `df["col"].duplicated()` | Marca valores repetidos en una columna. | Verifica que no existan `id` duplicados. | `df.drop_duplicates()` |
| `print()` | Imprime texto o resultados en pantalla. | Muestra conteos, estadísticos y conclusiones en cada paso. | `display()` |

## 3) Funciones de limpieza y construcción de variables

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `df.dropna()` | Elimina filas con valores faltantes en columnas indicadas. | Quita la única fila sin `title`. | `df[df["title"].notna()]` |
| `.copy()` | Crea una copia independiente de un DataFrame. | Evita modificar `df` original al construir `data`. | `pd.DataFrame(df)` |
| `fillna()` | Rellena valores faltantes con un valor fijo o calculado. | Sustituye nulos en `age_certification` (`"No clasificado"`), `seasons` (`0`) y en las métricas de IMDb/TMDB (mediana). | `replace()` |
| `.median()` | Calcula la mediana de una serie. | Define el valor de imputación para columnas numéricas con nulos. | `np.median()` |
| `df.groupby()` | Agrupa filas según los valores de una columna. | Agrupa por `type` para imputar `runtime` con la mediana propia de películas o series. | `pd.pivot_table()` |
| `.transform()` | Aplica una función a cada grupo y devuelve un resultado del mismo tamaño que el original. | Reemplaza los `runtime` nulos (antes marcados como `NaN`) por la mediana de su grupo. | `.apply()` + reindexado manual |
| `lambda` | Función anónima breve, definida en una sola línea. | Se usa dentro de `.transform()` para aplicar `fillna()` por grupo. | `def` con nombre |
| `df.loc[]` | Selecciona y/o modifica filas y columnas por condición o etiqueta. | Convierte los `runtime == 0` en `NaN` antes de imputarlos. | `df.query()` |
| `ast.literal_eval()` | Convierte de forma segura un texto con formato de lista/tupla de Python en el objeto real. | Extrae el primer género y el primer país de `genres` y `production_countries` (guardados como texto, ej. `"['drama', 'action']"`). | `json.loads()` (si el texto fuera JSON válido) |
| `try` / `except` | Controla errores sin detener el programa. | Evita que el notebook falle si un valor de `genres` no tiene formato de lista. | Validación previa con `isinstance()` |
| `def` | Define una función propia. | Crea `primer_elemento()` para reutilizar la extracción del primer valor de una lista. | `lambda` (solo para funciones muy cortas) |
| `.apply()` | Aplica una función a cada elemento de una columna. | Ejecuta `primer_elemento()` sobre `genres` y `production_countries`. | Operación vectorizada directa (si existiera) |
| `df[[...]]` | Selecciona un subconjunto de columnas. | Construye `data` con las variables finales del análisis. | `df.filter(items=[...])` |

## 4) Funciones de NumPy y operaciones vectorizadas

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `np.percentile()` | Calcula un percentil de un arreglo. | Obtiene el percentil 99 de `tmdb_popularity` para recortar valores extremos solo en las gráficas. | `np.quantile()` |
| `np.clip()` | Limita los valores de un arreglo a un rango mínimo/máximo. | Crea `tmdb_popularity_clip`, topando los valores por encima del percentil 99. | Condicional manual con `np.where()` |
| `.to_numpy()` | Convierte una serie de pandas en un arreglo de NumPy. | Prepara `imdb_score` y `tmdb_popularity` como arreglos (`imdb_np`, `pop_np`). | `.values` |
| `np.mean()` | Calcula la media de un arreglo. | Obtiene el promedio de `imdb_score` y `tmdb_popularity`, y se usa en el z-score manual. | `statistics.mean()` |
| `np.median()` | Calcula la mediana de un arreglo. | Resume la tendencia central de ambas variables numéricas. | `statistics.median()` |
| `np.std()` | Calcula la desviación estándar. | Completa la estandarización manual (z-score) de `imdb_score`. | `statistics.pstdev()` |
| `np.round()` | Redondea los valores de un arreglo. | Presenta los primeros z-scores con 3 decimales. | `round()` elemento a elemento |
| `np.sum()` | Suma los elementos de un arreglo (o valores `True`/`False`). | Cuenta cuántos títulos tienen `\|z\| > 2` en `imdb_score`. | `.sum()` de pandas |
| `np.abs()` | Devuelve el valor absoluto. | Se usa junto con `np.sum()` para contar z-scores extremos. | `abs()` |
| `np.unique()` | Devuelve los valores únicos de un arreglo y, con `return_counts=True`, cuántas veces aparece cada uno. | Cuenta títulos por `type` y por `primary_genre`. | `pd.Series.value_counts()` |
| `np.argsort()` | Devuelve los índices que ordenarían un arreglo. | Ordena los conteos de género para quedarse con el top 5 (`[::-1][:5]`). | `pd.Series.sort_values()` |
| `zip()` | Recorre dos o más secuencias en paralelo. | Empareja valores únicos con sus conteos para imprimirlos. | `enumerate()` |
| `dict()` | Construye un diccionario. | Convierte los pares `(valor, conteo)` en un diccionario legible. | `collections.Counter()` |

## 5) Funciones estadísticas de SciPy

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `.sample()` | Toma una muestra aleatoria de una serie. | Reduce `imdb_score` a máximo 5 000 valores antes de aplicar Shapiro-Wilk (que tiene límite de muestra). | `np.random.choice()` |
| `min()` | Devuelve el menor valor entre varios. | Limita el tamaño de muestra a `min(5000, len(imdb_np))`. | `np.minimum()` |
| `stats.shapiro()` | Prueba de normalidad de Shapiro-Wilk. | Evalúa si `imdb_score` se distribuye de forma normal. | `stats.normaltest()` |
| `stats.pearsonr()` | Correlación lineal de Pearson (con su p-valor). | Relaciona `imdb_score` con `tmdb_popularity`. | `np.corrcoef()` (sin p-valor) |
| `stats.ttest_ind()` | Prueba t para dos muestras independientes. | Compara el `imdb_score` promedio entre `MOVIE` y `SHOW` (con `equal_var=False`, prueba de Welch). | `stats.mannwhitneyu()` (no paramétrica) |
| `stats.zscore()` | Calcula puntuaciones z estandarizadas para un arreglo completo. | Detecta títulos atípicos en `tmdb_popularity`. | Cálculo manual con `np.mean()`/`np.std()` |
| `int()` | Convierte un valor a entero. | Presenta conteos (atípicos, muestras) como números enteros legibles. | `round()` |
| `float()` | Convierte un valor a número decimal. | Convierte estadísticos y p-valores de NumPy/SciPy a `float` de Python antes de imprimirlos. | `np.float64` directo |
| `round()` | Redondea un número a cierta cantidad de decimales. | Hace más legibles medias, correlaciones y estadísticos de prueba. | `np.round()` |
| `len()` | Devuelve la cantidad de elementos de una secuencia. | Calcula el porcentaje de atípicos sobre el total de títulos. | `.shape[0]` |

## 6) Funciones de Matplotlib

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `plt.figure()` | Crea una figura nueva (lienzo) para graficar. | Inicia cada una de las 4 gráficas de Matplotlib. | `plt.subplots()` |
| `plt.hist()` | Dibuja un histograma. | Muestra la distribución de `imdb_score`. | `sns.histplot()` |
| `plt.scatter()` | Dibuja un diagrama de dispersión. | Compara `imdb_score` contra `tmdb_popularity_clip`. | `sns.scatterplot()` |
| `plt.boxplot()` | Genera un diagrama de caja. | Compara `imdb_score` entre `MOVIE` y `SHOW`. | `sns.boxplot()` |
| `plt.bar()` | Dibuja una gráfica de barras. | Muestra el top 8 de géneros principales (`primary_genre`). | `df.plot(kind="bar")` |
| `.value_counts()` | Cuenta cuántas veces aparece cada valor de una columna. | Obtiene la frecuencia de cada género para el top 8. | `groupby().size()` |
| `.head()` | Selecciona los primeros elementos. | Recorta el conteo de géneros al top 8. | `.nlargest()` |
| `plt.title()` | Asigna un título a la gráfica. | Describe qué muestra cada figura. | `ax.set_title()` |
| `plt.xlabel()` / `plt.ylabel()` | Etiquetan los ejes X e Y. | Identifican las variables graficadas. | `ax.set_xlabel()` / `ax.set_ylabel()` |
| `plt.xticks()` | Configura las marcas del eje X. | Rota las etiquetas de género 45° para que no se superpongan. | `ax.tick_params()` |
| `plt.tight_layout()` | Ajusta automáticamente los márgenes de la figura. | Evita que las etiquetas rotadas se corten. | `fig.tight_layout()` |
| `plt.show()` | Muestra la gráfica ya construida. | Renderiza cada una de las visualizaciones. | `plt.savefig()` |

## 7) Funciones de Seaborn

| Función | ¿Qué hace? | Uso en el notebook | Variante alternativa |
|---|---|---|---|
| `sns.histplot()` | Histograma con opciones adicionales, como curva de densidad. | Muestra la distribución de `imdb_score` con `kde=True`. | `sns.displot()` |
| `sns.boxplot()` | Diagrama de caja con estética mejorada y color por categoría. | Compara `imdb_score` por `type`, coloreando con `palette="Set2"`. | `sns.violinplot()` |
| `sns.regplot()` | Dispersión con línea de tendencia (regresión) incluida. | Analiza `imdb_score` frente a `tmdb_popularity_clip`. | `sns.lmplot()` |
| `sns.heatmap()` | Dibuja un mapa de calor a partir de una matriz. | Visualiza la matriz de correlaciones entre variables numéricas. | `plt.imshow()` |
| `.corr()` | Calcula la correlación entre columnas numéricas de un DataFrame. | Genera la matriz que alimenta el `heatmap`. | `np.corrcoef()` |

## 8) Idea general de cómo se usan juntas

En el notebook, las funciones se combinan siguiendo este flujo:

1. `pd.read_csv()` carga `titles.csv` (comprimido en ZIP).
2. `isnull()`, `duplicated()`, `dropna()`, `fillna()`, `groupby().transform()` y `ast.literal_eval()` limpian el dataset y construyen variables nuevas (`primary_genre`, `primary_country`).
3. `np.percentile()` y `np.clip()` controlan los valores atípicos solo para efectos visuales.
4. `np.mean()`, `np.median()`, `np.std()`, `np.unique()` y `np.argsort()` calculan estadísticos descriptivos y resúmenes categóricos.
5. `stats.shapiro()`, `stats.pearsonr()`, `stats.ttest_ind()` y `stats.zscore()` realizan las pruebas estadísticas inferenciales.
6. `plt.*` y `sns.*` crean las 8 gráficas (4 + 4) que permiten interpretar los resultados.

Piensa en estas funciones como herramientas complementarias: **pandas** organiza y limpia los datos, **NumPy** hace cálculos vectorizados y rápidos, **SciPy** aporta pruebas estadísticas formales, y **Matplotlib/Seaborn** presentan los resultados de forma visual.
