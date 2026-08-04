# Estadística básica explicada fácil

**Curso:** Fundamentos para Inteligencia Artificial — NRC 94103
**Semana 5:** Estadística básica y visualización de datos
**Datos que vamos a usar:** `StudentsPerformance.csv`

## ¿Por qué nos importa esto?

Imagina que un profesor termina de calificar un examen de 1000 estudiantes. Tiene una hoja gigante con 1000 notas y no puede leerlas una por una para entender cómo le fue al curso. La **estadística** es, básicamente, el conjunto de trucos para resumir muchísimos datos en unos pocos números que sí podemos entender de un vistazo: "en promedio el curso sacó 66", "la mitad sacó más de 66 y la mitad menos", "los que estudiaron más sacaron mejor nota", etc.

Estos mismos trucos son la base de la inteligencia artificial: antes de que una máquina "aprenda" algo de unos datos, alguien tiene que entender primero qué hay en esos datos. Por eso esta semana es la base de las siguientes.

## Conociendo los datos: `StudentsPerformance.csv`

Es una tabla con información de **1000 estudiantes**. Cada fila es un estudiante, y cada columna es un dato sobre ese estudiante:

| Columna | Qué significa | Ejemplo |
|---|---|---|
| `gender` | Género del estudiante | female, male |
| `race/ethnicity` | Grupo al que pertenece (dato codificado, sin nombres reales) | group A, group B... |
| `parental level of education` | Nivel de estudios de los papás | "high school", "bachelor's degree"... |
| `lunch` | Tipo de almuerzo que recibe (pista indirecta del nivel socioeconómico) | standard, free/reduced |
| `test preparation course` | Si hizo o no un curso de preparación antes del examen | none, completed |
| `math score` | Nota en matemáticas (0 a 100) | 72 |
| `reading score` | Nota en lectura (0 a 100) | 72 |
| `writing score` | Nota en escritura (0 a 100) | 74 |

Fíjate que hay dos "familias" de columnas:

- **Columnas de categorías** (`gender`, `lunch`, etc.): son palabras, no números. No tiene sentido "sumarlas" o "promediarlas".
- **Columnas numéricas** (`math score`, `reading score`, `writing score`): son números del 0 al 100. Con estas sí podemos hacer cuentas como promedios.

Esta distinción es la primera cosa importante que hay que aprender en estadística, porque cada tipo de dato admite operaciones distintas.

---

## 1. Contar cosas: frecuencias

Lo más simple que podemos hacer con una columna de categorías es **contar cuántas veces aparece cada valor**. A eso se le llama **frecuencia**.

**Ejemplo:** ¿cuántos estudiantes son hombres y cuántos mujeres?

```python
import pandas as pd

df = pd.read_csv('StudentsPerformance.csv')
df['gender'].value_counts()
```

Resultado real:

| gender | cuántos hay (frecuencia) | qué porcentaje del total (frecuencia relativa) |
|---|---|---|
| female | 518 | 51.8% |
| male | 482 | 48.2% |

Es literalmente lo mismo que hace un profesor cuando dice "en mi salón hay 20 mujeres y 15 hombres". La **frecuencia relativa** es simplemente convertir ese conteo en porcentaje, para poder comparar aunque el total de estudiantes cambie.

---

## 2. El "dato típico": media, mediana y moda

Cuando hablamos de una columna numérica como `math score`, lo primero que queremos saber es: **¿cuál es un valor "normal" o "típico" en esta columna?** Hay tres formas de responder eso, y no siempre dan el mismo número.

### Media (el promedio de toda la vida)

Es lo que todo el mundo conoce como "promedio": sumar todos los valores y dividir entre cuántos son.

```python
df['math score'].mean()
```

Resultado real: **66.09**. Es decir, si repartiéramos las notas de matemáticas en partes iguales entre los 1000 estudiantes, a cada uno le tocarían 66.09 puntos.

**Ojo:** la media se deja "engañar" fácilmente por valores muy raros. Si en un salón de 5 estudiantes cuatro sacan 8 y uno saca 0, la media es 6.4 — un número que no representa bien a "casi todo el mundo sacó 8".

### Mediana (el del medio)

Si ordenas todas las notas de menor a mayor y te paras justo en la del medio, esa es la mediana. La mitad de los estudiantes sacó menos que ese valor, y la otra mitad sacó más.

```python
df['math score'].median()
```

Resultado real: **66.0**. Casi idéntica a la media, lo cual nos dice algo importante: **las notas de matemáticas están repartidas de forma bastante pareja**, sin un grupo extremo que "descuadre" el promedio.

### Moda (la más repetida)

Es, literalmente, el valor que más veces se repite.

```python
df['math score'].mode()
```

Resultado real: **65**. Es la única de las tres medidas que también sirve para columnas de categorías: por ejemplo, la moda de `gender` es "female", porque hay más mujeres que hombres en la tabla.

---

## 3. ¿Qué tan parejos son los datos? (dispersión)

Saber el "promedio" no es suficiente. Dos salones pueden tener la misma nota promedio (digamos, 70) y ser completamente distintos: en uno todos sacaron entre 65 y 75, y en el otro la mitad sacó 40 y la otra mitad sacó 100. Para distinguir esto necesitamos medir **qué tan dispersos (repartidos)** están los datos.

### Rango

La distancia entre el valor más bajo y el más alto.

```python
df['math score'].max() - df['math score'].min()
```

Resultado real: **100** (hay al menos un estudiante con 0 y otro con 100).

### Desviación estándar

Es, en resumen, **"en promedio, qué tan lejos está cada nota de la media"**. Si es un número pequeño, casi todos sacaron notas parecidas. Si es grande, las notas están muy repartidas.

```python
df['math score'].std(ddof=0)
```

Resultado real: **≈15.16**. Esto significa que, en general, las notas de los estudiantes se alejan de la media (66) en unos 15 puntos, más o menos. Entonces la mayoría de los estudiantes sacó entre 51 y 81.

*(Dato técnico para más adelante: hay dos formas de calcular esto, `ddof=0` y `ddof=1`. Cuando ya tienes **todos** los datos que te interesan —como aquí, los 1000 estudiantes completos— usa `ddof=0`.)*

---

## 4. Cuartiles: dividir el curso en 4 grupos iguales

Imagina que ordenas a los 1000 estudiantes de la nota más baja a la más alta en matemáticas, y los divides en 4 grupos del mismo tamaño (250 estudiantes cada uno). Los puntos de corte entre esos grupos son los **cuartiles**:

- **Q1 (25%):** el 25% de los estudiantes sacó menos que este valor.
- **Q2 (50%):** es la mediana, la de la sección anterior.
- **Q3 (75%):** el 75% de los estudiantes sacó menos que este valor.

```python
df['math score'].quantile([0.25, 0.5, 0.75])
```

Resultado real: **Q1 = 57**, **Q2 = 66**, **Q3 = 77**.

Con esto podemos saber, por ejemplo, si un estudiante que sacó 80 en matemáticas quedó en el grupo de mejores notas del curso (sí: 80 está por encima de Q3 = 77, así que está entre el 25% con mejores notas).

La distancia entre Q3 y Q1 se llama **rango intercuartílico (IQR)** y sirve para detectar notas "raras" (outliers): cualquier nota muy por fuera de ese rango central llama la atención y merece revisarse.

---

## 5. Comparar grupos: ¿el curso de preparación sirvió?

Una de las preguntas más comunes en estadística es comparar el promedio de un grupo contra el de otro. Aquí podemos comparar a los estudiantes que **sí** hicieron el curso de preparación contra los que **no** lo hicieron:

```python
df.groupby('test preparation course')['math score'].mean()
```

Resultado real:

| ¿Hizo el curso de preparación? | Promedio en matemáticas |
|---|---|
| No (`none`) | 64.08 |
| Sí (`completed`) | 69.70 |

**Lectura:** los que hicieron el curso sacaron, en promedio, casi 6 puntos más. Esto **no prueba** que el curso sea la causa directa (a lo mejor los estudiantes más aplicados son los mismos que deciden tomar el curso de preparación) — pero sí es una pista fuerte de que vale la pena investigarlo más.

---

## 6. Correlación: si mejoras en una materia, ¿mejoras en otra?

La **correlación** mide qué tanto se "mueven juntas" dos columnas numéricas. Va de -1 a 1:

- Cerca de **+1**: cuando una sube, la otra también sube (ejemplo esperado: lectura y escritura).
- Cerca de **-1**: cuando una sube, la otra baja.
- Cerca de **0**: no hay relación clara entre las dos.

```python
df['reading score'].corr(df['writing score'])
```

Resultado real:

| Comparación | Correlación |
|---|---|
| Matemáticas vs. Lectura | 0.818 |
| Matemáticas vs. Escritura | 0.803 |
| Lectura vs. Escritura | **0.955** |

**Lectura:** lectura y escritura están extremadamente ligadas (0.955): un estudiante que lee bien casi siempre también escribe bien. Tiene sentido, porque son habilidades muy parecidas. Matemáticas también se relaciona bastante con las otras dos, aunque un poco menos.

**Importante:** correlación **no** significa que una cosa *cause* la otra. Solo dice que se mueven juntas. Puede haber una tercera razón detrás (por ejemplo, hábitos de estudio en general) que explique por qué ambas notas suben o bajan a la vez.

---

## 7. Un vistazo a la predicción (regresión lineal)

Si dos variables están fuertemente correlacionadas, podemos trazar una línea que las relacione y usarla para **predecir** una a partir de la otra:

```python
import numpy as np

m, b = np.polyfit(df['reading score'], df['math score'], 1)
print(f'math score ≈ {m:.2f} * reading score + {b:.2f}')
```

Esto encuentra la línea recta que mejor "atraviesa" la nube de puntos (lectura, matemáticas). Con esa línea, si un estudiante nuevo solo nos da su nota de lectura, podríamos estimar aproximadamente cuál sería su nota de matemáticas. Esta idea —encontrar una línea o una fórmula que prediga un valor a partir de otro— es exactamente el punto de partida de los modelos de inteligencia artificial que se verán más adelante en el curso.

---

## 8. Probabilidad básica

La probabilidad de que algo pase, si eliges un estudiante al azar de la tabla, es simplemente su frecuencia relativa (lo que vimos en la Sección 1):

```python
(df['test preparation course'] == 'completed').mean()
```

Esto responde a la pregunta: "si elijo un estudiante cualquiera de los 1000, ¿qué tan probable es que haya hecho el curso de preparación?". El resultado es un número entre 0 y 1 (o entre 0% y 100%).

---

## Glosario rápido

| Palabra | En una frase |
|---|---|
| Media | El promedio de toda la vida |
| Mediana | El valor que queda justo en la mitad al ordenar los datos |
| Moda | El valor que más se repite |
| Desviación estándar | Qué tan lejos están, en promedio, los datos de la media |
| Cuartil | Punto de corte que divide los datos ordenados en 4 partes iguales |
| Correlación | Qué tanto se mueven juntas dos variables numéricas |
| Frecuencia | Cuántas veces aparece un valor |
| Regresión lineal | Una línea recta que resume y predice la relación entre dos variables |

## Resumen — ¿qué uso según la pregunta?

| Pregunta | Herramienta |
|---|---|
| ¿Cuál es la nota "normal" del curso? | Media o mediana |
| ¿Qué tan parejas son las notas? | Desviación estándar, rango |
| ¿Cuál es la categoría más común? | Moda / conteo de frecuencias |
| ¿Un estudiante quedó entre los mejores? | Cuartiles |
| ¿Un grupo saca mejor nota que otro? | Promedio agrupado (`groupby`) |
| ¿Dos materias suben y bajan juntas? | Correlación |
| ¿Puedo estimar una nota a partir de otra? | Regresión lineal |
| ¿Qué tan probable es un caso al azar? | Frecuencia relativa (probabilidad) |
