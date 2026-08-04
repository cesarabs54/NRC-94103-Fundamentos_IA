# Caso de estudio: gráficos de pastel y proporciones de lenguajes de programación

**Curso:** Fundamentos para Inteligencia Artificial — NRC 94103
**Semana 5:** Estadística básica y visualización de datos
**Fuente:** *Matplotlib Tutorial (Part 3)* — basado en la Stack Overflow Developer Survey 2019
**Versión:** Estudiante

## Presentación del caso

Este caso continúa el trabajado en las Partes 1 y 2 (`EIARV011_S5_CasoEstudio_estudiante.md`, en la carpeta `Semana_5`), que usó el mismo dataset —la *Stack Overflow Developer Survey 2019*— con gráficos de líneas y de barras. Aquí se retoma la tabla de frecuencias de lenguajes de programación declarados por los encuestados, pero para responder una pregunta distinta: no cuántas personas declaran cada lenguaje, sino **qué proporción del total representa cada uno**, mediante un gráfico de pastel (`plt.pie()`).

A continuación se desarrolla el caso en cuatro secciones, cada una con una explicación conceptual y un ejemplo aplicado en Python (matplotlib y pandas). Al final de cada sección hay una **pregunta de reflexión** para trabajar en clase.

> **Nota de datos.** Este material es autocontenido: `data.csv` está incluido en esta misma carpeta (`Matplotlib_Parte_3`) y el ejemplo vuelve a construir su propia tabla de frecuencias con `Counter`, sin depender de variables definidas en el notebook de las Partes 1-2.

---

## 1. Mecánica básica del gráfico de pastel

El gráfico de pastel (`plt.pie()`) responde una pregunta distinta a la de un gráfico de barras: no "¿cuánto vale cada categoría?", sino "¿qué proporción del total representa cada categoría?". Es la elección natural cuando el objetivo es leer participaciones dentro de un todo.

Una particularidad útil de `plt.pie()` es que los valores que recibe **no necesitan sumar 100**: internamente calcula la proporción de cada valor respecto de la suma total de la lista y dibuja la porción angular correspondiente. Así, `[60, 40]` y `[120, 80]` producen exactamente el mismo gráfico, porque ambas listas mantienen la proporción 60 %-40 %.

**Ejemplo aplicado.**

```python
import matplotlib.pyplot as plt

slices = [120, 80]
labels = ['60', '40']

plt.pie(slices, labels=labels,
        wedgeprops={'edgecolor': 'black'},
        colors=['#008fd5', '#fc4f30'])

plt.title('Ejemplo básico: proporciones de un todo (no de magnitudes)')
plt.tight_layout()
plt.show()
```

Aunque los valores son 120 y 80 (no suman 100), el gráfico dibuja exactamente 60 % y 40 %: `plt.pie()` normaliza cada valor contra la suma total. El parámetro `wedgeprops={'edgecolor': 'black'}` añade un borde a cada porción para separarla visualmente de las contiguas, un ajuste estético pero recomendable, pues sin él las porciones de colores similares se funden entre sí.

> **Pregunta de reflexión:** ¿por qué el hecho de que `plt.pie()` no exija que los valores sumen 100 es, al mismo tiempo, una ventaja de conveniencia y un riesgo de interpretación si el lector asume erróneamente que sí lo hacen?

---

## 2. Datos reales y el límite de legibilidad con alta cardinalidad

Se carga `data.csv` (87 569 encuestados, columnas `Responder_id` y `LanguagesWorkedWith`) y se reconstruye la tabla de frecuencias de lenguajes declarados, del mismo modo que en la Sección 7 del caso de las Partes 1-2: cada respuesta es una cadena con varios lenguajes separados por `;`, que se separa con `.split(';')` y se acumula con `Counter`.

Para comprobar en la práctica el problema de legibilidad, se construye primero un gráfico de pastel con las 15 categorías más frecuentes —el mismo conjunto que en el caso de las Partes 1-2 se representó con un gráfico de barras horizontales— y se observa qué tan legible resulta.

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
languages_15 = [item[0] for item in top_15]
popularity_15 = [item[1] for item in top_15]

plt.pie(popularity_15, labels=languages_15)
plt.title('Los 15 lenguajes más declarados representados como gráfico de pastel')
plt.tight_layout()
plt.show()
```

Con 15 categorías el resultado es notoriamente ilegible: varias porciones son tan angostas que apenas se distinguen entre sí, y las etiquetas de texto compiten por espacio alrededor del círculo, llegando incluso a superponerse. Este es precisamente el problema que el gráfico de barras horizontales (Sección 9 del caso de las Partes 1-2) resuelve mejor para esta misma cardinalidad.

> **Pregunta de reflexión:** más allá de la superposición de etiquetas, ¿qué otra limitación perceptual (no solo estética) hace que comparar dos porciones angostas de un gráfico de pastel sea más difícil que comparar dos barras horizontales de longitud similar?

---

## 3. Reducción a los 5 lenguajes más declarados y personalización del gráfico

Respetando el límite práctico de legibilidad de la Sección 2 (no más de cuatro o cinco categorías), se reduce la tabla de frecuencias a sus 5 primeras entradas y se construye un gráfico de pastel con varios recursos adicionales de `plt.pie()`:

- `colors`: una lista de colores en hexadecimal, uno por porción, en el mismo orden que las categorías.
- `explode`: una lista de números (uno por porción) que desplaza esa porción hacia afuera del centro; `0` no desplaza nada y `0.1` la desplaza el 10 % del radio. Se usa aquí para enfatizar visualmente una categoría específica, Python.
- `shadow=True`: añade una sombra a las porciones, un efecto puramente estético.
- `startangle`: el ángulo (en grados) en el que comienza a dibujarse la primera porción, medido en sentido antihorario.
- `autopct='%1.1f%%'`: cadena de formato que instruye a matplotlib para que calcule e imprima, dentro de cada porción, el porcentaje que representa respecto del total de la lista graficada.

**Ejemplo aplicado.**

```python
top_5 = language_counter.most_common(5)
languages_5 = [item[0] for item in top_5]
popularity_5 = [item[1] for item in top_5]

colors = ['#008fd5', '#fc4f30', '#e5ae37', '#6d904f', '#6a5acd']
explode = [0, 0, 0, 0.1, 0]  # se resalta 'Python' (4.o lugar)

plt.pie(popularity_5, labels=languages_5, colors=colors,
        explode=explode, shadow=True, startangle=90,
        wedgeprops={'edgecolor': 'black'},
        autopct='%1.1f%%')

plt.title('Participación de los 5 lenguajes más declarados (Stack Overflow Developer Survey 2019)')
plt.tight_layout()
plt.show()
```

Con solo 5 categorías el gráfico vuelve a ser legible: cada porción y su etiqueta se distinguen con claridad. Sobre las 234 589 menciones acumuladas entre los 5 lenguajes más declarados, JavaScript concentra el 25.2 %, HTML/CSS el 23.6 %, SQL el 20.3 %, Python el 15.5 % y Java el 15.3 %; la porción de Python queda desplazada del centro (`explode`) para resaltarla. El `startangle=90` hace que la primera categoría (JavaScript) inicie su porción en la parte superior del círculo en lugar de en un ángulo arbitrario.

> **Pregunta de reflexión:** si en lugar de resaltar a Python se quisiera resaltar el lenguaje menos declarado del top 5, ¿qué cambio puntual habría que hacer en la lista `explode`, y por qué ese cambio no afecta a las demás porciones del gráfico?

---

## 4. Advertencia metodológica: variables de respuesta múltiple y el denominador del porcentaje

La columna `LanguagesWorkedWith` es una variable de **respuesta múltiple**: cada encuestado puede declarar varios lenguajes a la vez, de modo que las categorías no son mutuamente excluyentes y no particionan un mismo total de encuestados. El porcentaje que calculó `autopct` en la Sección 3 es, en sentido estricto, la proporción de *menciones* entre las 5 categorías más frecuentes, no la proporción de *encuestados* que usan cada lenguaje.

**Ejemplo aplicado.**

```python
python_mentions = language_counter['Python']
total_respondents = len(data)
top5_total_mentions = sum(popularity_5)

pct_sobre_top5 = 100 * python_mentions / top5_total_mentions
pct_sobre_encuestados = 100 * python_mentions / total_respondents

print(f"Porcentaje sobre menciones del top 5: {pct_sobre_top5:.1f}%")
print(f"Porcentaje sobre total de encuestados: {pct_sobre_encuestados:.1f}%")
```

El porcentaje que muestra el gráfico de pastel de la Sección 3 (15.5 %) es la participación de Python dentro de las menciones del top 5; el porcentaje sobre el total de encuestados (41.6 %) responde una pregunta distinta —qué fracción de las personas declara usar Python— y es casi el triple. Ninguno de los dos números es "incorrecto"; representan denominadores distintos y responden preguntas distintas.

> **Pregunta de reflexión:** si el objetivo del análisis fuera responder "¿qué fracción de los desarrolladores encuestados sabe Python?", ¿sería apropiado usar el gráfico de pastel de la Sección 3 para comunicar esa cifra? ¿Por qué sí o por qué no?
