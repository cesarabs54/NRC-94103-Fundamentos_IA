# Caso de estudio: gráficos de barras y comparación de grupos categóricos

**Curso:** Fundamentos para Inteligencia Artificial — NRC 94103
**Semana 5:** Estadística básica y visualización de datos
**Fuente:** *Matplotlib Tutorial (Part 2)* — basado en la Stack Overflow Developer Survey 2019
**Versión:** Estudiante

## Presentación del caso

Este caso continúa el trabajado en la Parte 1 (`EIARV011_S5_CasoEstudio_estudiante.md`, en la carpeta `Semana_5`), que usó el mismo dataset —la *Stack Overflow Developer Survey 2019*— con gráficos de líneas. Aquí se introduce el gráfico de barras (`plt.bar()` / `plt.barh()`) como alternativa cuando el objetivo es comparar magnitudes puntuales entre categorías, y se trabaja por primera vez con datos cargados desde un archivo CSV.

A continuación se desarrolla el caso en cuatro secciones, cada una con una explicación conceptual y un ejemplo aplicado en Python (matplotlib, NumPy y pandas). Al final de cada sección hay una **pregunta de reflexión** para trabajar en clase.

> **Nota de datos.** Este material es autocontenido: los datos de salario de las Secciones 1-2 están escritos directamente en el ejemplo, y `data.csv` (Secciones 3-4) está incluido en esta misma carpeta (`Matplotlib_Parte_2`).

---

## 1. De la línea a la barra: `plt.bar()` y el problema de apilar series

Cuando la variable del eje horizontal es categórica o discreta y el objetivo es comparar magnitudes puntuales (no leer una tendencia continua), el gráfico de barras es preferible al de líneas. Cambiar de un gráfico de líneas a uno de barras es tan simple como sustituir `plt.plot()` por `plt.bar()`, manteniendo los mismos argumentos posicionales (`x`, `y`).

La dificultad aparece al intentar comparar varios grupos: si se llama a `plt.bar()` tres veces seguidas con el mismo eje `x`, las tres series de barras se dibujan exactamente en la misma posición horizontal, superponiéndose por completo.

**Ejemplo aplicado.**

```python
import matplotlib.pyplot as plt

ages_x = [25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35]

dev_y = [38496, 42000, 46752, 49320, 53200,
         56000, 62316, 64928, 67317, 68748, 73752]
py_dev_y = [45372, 48876, 53850, 57287, 63016,
            65998, 70003, 70000, 71496, 75370, 83640]
js_dev_y = [37810, 43515, 46823, 49293, 53437,
            56373, 62375, 66674, 68745, 68746, 74583]

plt.bar(ages_x, dev_y, color='#444444', label='Todos los desarrolladores')
plt.bar(ages_x, py_dev_y, color='#5a7d9a', label='Python')
plt.bar(ages_x, js_dev_y, color='#adad3b', label='JavaScript')

plt.legend()
plt.title('Intento ingenuo: tres plt.bar() sobre el mismo eje (barras superpuestas)')
plt.xlabel('Edad')
plt.ylabel('Salario mediano (USD)')
plt.tight_layout()
plt.show()
```

El resultado es engañoso: solo se distingue con claridad la serie con los valores más altos (Python), porque cada `plt.bar()` posterior dibuja sus barras exactamente encima de las anteriores, ocultándolas parcialmente.

> **Pregunta de reflexión:** ¿por qué este problema de superposición no ocurre de la misma forma cuando se trazan tres series con `plt.plot()` sobre el mismo eje, como se hizo en el caso de la Parte 1?

---

## 2. Barras agrupadas: desplazamiento con NumPy y reetiquetado del eje

La solución estándar al problema de la Sección 1 consiste en desplazar horizontalmente cada serie de barras un ancho fijo (`width`), de modo que las tres queden colocadas lado a lado en lugar de superpuestas.

Para calcular esas posiciones desplazadas se usa `numpy.arange(len(ages_x))`, que genera un arreglo de índices enteros consecutivos que reemplazan temporalmente a las categorías reales. Tras desplazar y dibujar las barras sobre esos índices, hay que reetiquetar el eje horizontal con las categorías reales mediante `plt.xticks(ticks=x_indexes, labels=ages_x)`.

**Ejemplo aplicado.**

```python
import numpy as np

x_indexes = np.arange(len(ages_x))
width = 0.25

plt.bar(x_indexes - width, dev_y, width=width,
        color='#444444', label='Todos los desarrolladores')
plt.bar(x_indexes, py_dev_y, width=width,
        color='#5a7d9a', label='Python')
plt.bar(x_indexes + width, js_dev_y, width=width,
        color='#adad3b', label='JavaScript')

plt.xticks(ticks=x_indexes, labels=ages_x)
plt.legend()
plt.title('Salario mediano (USD) por edad y lenguaje — barras agrupadas')
plt.xlabel('Edad')
plt.ylabel('Salario mediano (USD)')
plt.tight_layout()
plt.show()
```

Ahora las tres barras de cada edad son completamente visibles, colocadas una junto a otra sin ocultarse. El desplazamiento (`-width`, `0`, `+width`) centra el trío de barras en torno al índice numérico de cada edad, y `plt.xticks()` restituye las etiquetas reales sobre esos índices auxiliares.

> **Pregunta de reflexión:** si en lugar de tres grupos se quisieran comparar cinco lenguajes de programación por edad, ¿cómo debería ajustarse la fórmula de desplazamiento para mantener las cinco barras agrupadas sin superposición?

---

## 3. Datos reales desde CSV: tabla de frecuencias con `Counter` y gráfico de barras verticales

Hasta ahora los datos estaban escritos directamente en el script. En la práctica, los datos casi siempre provienen de una fuente externa como un archivo CSV, y con frecuencia requieren limpieza antes de poder graficarse. Se carga `data.csv` (87 569 encuestados, columnas `Responder_id` y `LanguagesWorkedWith`) con `pd.read_csv()`.

Cada respuesta de `LanguagesWorkedWith` es una cadena de texto con varios lenguajes separados por `;` (por ejemplo, `"HTML/CSS;Java;JavaScript;Python"`), de modo que se requiere `.split(';')` para convertirla en una lista de valores atómicos antes de poder contarlos. Para acumular las frecuencias se usa `Counter` del módulo `collections`, mediante `update()`; su método `most_common(n)` entrega directamente las *n* categorías más frecuentes, ya ordenadas, como una lista de tuplas `(lenguaje, conteo)`.

**Ejemplo aplicado.**

```python
import pandas as pd
from collections import Counter

data = pd.read_csv('data.csv')
lang_responses = data['LanguagesWorkedWith']

language_counter = Counter()
for response in lang_responses:
    language_counter.update(response.split(';'))

top_15 = language_counter.most_common(15)
languages = [item[0] for item in top_15]
popularity = [item[1] for item in top_15]

plt.bar(languages, popularity, color='#5a7d9a')
plt.title('Lenguajes de programación más utilizados (Stack Overflow Developer Survey 2019)')
plt.xlabel('Lenguajes de programación')
plt.ylabel('Número de encuestados que lo declaran')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```

El conteo ubica a JavaScript, HTML/CSS y SQL como los lenguajes más declarados, seguidos por Python y Java. Con 15 categorías, el gráfico de barras vertical ya resulta algo saturado: las etiquetas requieren rotación para no superponerse.

> **Pregunta de reflexión:** ¿por qué sería estadísticamente incorrecto representar la popularidad de los lenguajes de programación mediante un gráfico de líneas en lugar de un gráfico de barras?

---

## 4. Gráfico de barras horizontales, orden descendente y validación externa

Cuando el número de categorías es elevado (15 lenguajes), las etiquetas de texto no caben legiblemente en el eje horizontal de un gráfico de barras vertical sin recurrir a rotaciones que dificultan la lectura. La solución es invertir la orientación con `plt.barh()`: al pasar las categorías al eje vertical, cada etiqueta se lee sin rotación, en su propia fila.

Por convención de lectura (de arriba hacia abajo), conviene que el lenguaje más popular aparezca arriba; como `most_common()` devuelve la lista ordenada de mayor a menor, y `plt.barh()` dibuja de abajo hacia arriba, es necesario invertir el orden de las listas con `.reverse()` antes de graficar.

**Ejemplo aplicado.**

```python
languages.reverse()
popularity.reverse()

plt.barh(languages, popularity, color='#5a7d9a')
plt.title('Lenguajes de programación más utilizados (Stack Overflow Developer Survey 2019)')
plt.xlabel('Número de encuestados que lo declaran')
plt.tight_layout()
plt.show()

# Validación: comparar el orden y la magnitud relativa de las barras
# contra el gráfico oficial publicado por Stack Overflow Insights
# para la misma edición de la encuesta.
```

Con la orientación horizontal, las 15 etiquetas se leen sin rotación y sin superponerse. Al contrastar este resultado contra el publicado oficialmente por Stack Overflow Insights para la misma edición de la encuesta, el orden relativo de los lenguajes coincide en términos generales, con pequeñas diferencias de magnitud atribuibles a decisiones distintas de limpieza de datos.

> **Pregunta de reflexión:** si el gráfico propio y el gráfico oficial de Stack Overflow mostraran el mismo orden de lenguajes pero magnitudes sustancialmente distintas (no solo pequeñas diferencias), ¿qué hipótesis de trabajo debería explorarse antes de asumir que el análisis propio contiene un error?
