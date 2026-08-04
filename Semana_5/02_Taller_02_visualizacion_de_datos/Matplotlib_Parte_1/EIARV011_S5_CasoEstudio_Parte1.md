# Caso de estudio: salario mediano por edad y lectura de tendencias con gráficos de líneas

**Curso:** Fundamentos para Inteligencia Artificial — NRC 94103
**Semana 5:** Estadística básica y visualización de datos
**Fuente:** *Matplotlib Tutorial (Part 1)* — basado en la Stack Overflow Developer Survey 2019

## Presentación del caso

El caso de estudio proviene de la *Stack Overflow Developer Survey 2019*, una encuesta anual aplicada a decenas de miles de desarrolladores de software en todo el mundo. A partir de esa base de datos se trabaja la pregunta: ¿cómo varía el salario mediano de un desarrollador según su edad, y difiere ese patrón entre desarrolladores en general, de Python y de JavaScript? La pregunta combina estadística descriptiva (medidas de tendencia central) con visualización de datos como herramienta de comunicación.

A continuación se desarrolla el caso en seis secciones, cada una con una explicación conceptual y un ejemplo aplicado en Python (matplotlib y NumPy). Al final de cada sección hay una **pregunta de reflexión** para trabajar en clase.

> **Continuación del caso.** El análisis de los lenguajes de programación más declarados (tablas de frecuencia y gráficos de barras) se desarrolla como caso independiente en la carpeta `Matplotlib_Parte_2`, y el análisis de proporciones (gráfico de pastel) en `Matplotlib_Parte_3`.

---

## 1. Contexto y planteamiento del caso de estudio

El caso está diseñado en dos fases de complejidad creciente. En la primera fase se trabaja con un subconjunto de datos reducido y ya limpio, escrito directamente en el script de Python (listas de edades y salarios), lo que permite concentrar la atención en la lógica de construcción del gráfico sin la fricción de un proceso de carga y limpieza de datos. En la segunda fase se introduce el archivo completo (`data.csv`), con cerca de 88 000 registros, cargado mediante pandas, lo que aproxima al analista al flujo de trabajo real: adquisición, limpieza, transformación y, solo al final, visualización.

Esta progresión reproduce, a escala de aula, el ciclo de análisis de datos que se emplea en proyectos de inteligencia artificial: comprender el fenómeno que genera los datos, seleccionar las variables pertinentes, aplicar estadística descriptiva para caracterizarlas y representar los hallazgos de forma interpretable para una audiencia no técnica.

**Ejemplo aplicado.** El caso trabaja con dos fuentes de datos derivadas de la misma encuesta:

- Un subconjunto de edades entre 25 y 35 años con el salario mediano (USD) para tres grupos: desarrolladores en general, de Python y de JavaScript.
- El archivo `data.csv` (87 569 encuestados), con una fila por encuestado, que registra un identificador (`Responder_id`) y la lista de lenguajes de programación que dijo utilizar (`LanguagesWorkedWith`), separados por punto y coma (por ejemplo: `"HTML/CSS;Java;JavaScript;Python"`).

> **Pregunta de reflexión:** ¿por qué es útil introducir primero una muestra reducida y ya limpia, y solo después el archivo CSV completo, en lugar de comenzar directamente con los 88 000 registros?

---

## 2. Variables y escalas de medición en el caso

Antes de graficar cualquier dato es necesario clasificar las variables involucradas, pues de ello depende el estadístico de resumen apropiado y el tipo de gráfico pertinente. En este caso conviven tres tipos de variables: la edad (`ages_x`), cuantitativa discreta, que actúa como variable independiente; el salario mediano (`dev_y`, `py_dev_y`, `js_dev_y`), cuantitativa continua medida en USD, que actúa como variable dependiente; y el lenguaje de programación utilizado, cualitativa nominal y de respuesta múltiple.

La elección de la **mediana** como estadístico de tendencia central —y no la media aritmética— no es incidental. Los salarios presentan típicamente una distribución asimétrica positiva, con una cola larga de salarios altos que desplaza la media hacia arriba. La mediana, al ser un estadístico de posición, es robusta frente a esos valores extremos.

La variable de lenguaje de programación exige un tratamiento distinto: al ser nominal y de respuesta múltiple, no admite medidas de tendencia central (no existe un "lenguaje promedio"), sino que se resume mediante una **tabla de distribución de frecuencias**.

**Ejemplo aplicado.**

```python
ages_x = [25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35]        # cuantitativa discreta
dev_y  = [38496, 42000, 46752, 49320, 53200,                 # cuantitativa continua
          56000, 62316, 64928, 67317, 68748, 73752]          # (mediana salarial, USD)

# variable cualitativa nominal, de respuesta múltiple:
# "LanguagesWorkedWith": "HTML/CSS;Java;JavaScript;Python"
```

`ages_x` y `dev_y` tienen la misma longitud y guardan correspondencia posicional (el salario en la posición *i* corresponde a la edad en la posición *i*), condición que exige `plt.plot(x, y)`.

> **Pregunta de reflexión:** ¿qué distorsión introduciría usar la media aritmética en lugar de la mediana para resumir el salario de cada grupo de edad, dado el sesgo positivo de las distribuciones de ingreso?

---

## 3. Primera visualización: el gráfico de líneas y la lectura de la tendencia

El gráfico de líneas es la representación canónica cuando la variable del eje horizontal posee un orden natural continuo —como la edad— y se desea enfatizar la tendencia de la variable dependiente a lo largo de ese orden. En matplotlib, `plt.plot(x, y)` traza segmentos rectos entre cada par ordenado (x_i, y_i).

El gráfico de líneas codifica la información mediante la posición en el eje y y mediante la pendiente entre puntos consecutivos, que se interpreta como "crecimiento", "estancamiento" o "caída". Por ello es especialmente eficaz para responder "¿cómo cambia Y a medida que aumenta X?".

Un gráfico sin título ni etiquetas de ejes carece de valor comunicativo: `plt.title()`, `plt.xlabel()` y `plt.ylabel()` son la condición mínima de legibilidad de cualquier producto estadístico.

**Ejemplo aplicado.**

```python
import matplotlib.pyplot as plt

ages_x = [25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35]
dev_y  = [38496, 42000, 46752, 49320, 53200,
          56000, 62316, 64928, 67317, 68748, 73752]

plt.plot(ages_x, dev_y)

plt.title('Salario mediano (USD) por edad')
plt.xlabel('Edades')
plt.ylabel('Salario mediano (USD)')

plt.tight_layout()
plt.show()
```

La pendiente es positiva y aproximadamente monótona creciente en todo el rango observado, lo que sugiere una asociación positiva entre edad y salario mediano en este tramo etario.

> **Pregunta de reflexión:** si el gráfico mostrara una pendiente irregular (subidas y bajadas bruscas entre edades consecutivas), ¿qué explicaciones estadísticas alternativas —además de un fenómeno real— deberían considerarse?

---

## 4. Comparación de grupos: series múltiples, leyendas y codificación visual

Una tarea frecuente en estadística aplicada es la comparación de subgrupos: aquí, contrastar el salario mediano general contra el de Python y JavaScript. Esto se resuelve superponiendo varias series sobre los mismos ejes, siempre que compartan la variable independiente (`ages_x`).

Cuando coexisten varias series, la leyenda (`plt.legend()`) deja de ser opcional. Existen dos formas de construirla: pasar una lista de etiquetas directamente a `plt.legend([...])` en el orden en que se invocaron los `plot()`, o asignar `label='...'` dentro de cada llamada a `plot()` e invocar `plt.legend()` sin argumentos.

La segunda práctica es preferible porque es autodocumentada y resiliente a reordenamientos: si luego se cambia el orden en que se trazan las series, una leyenda basada en una lista desincronizada etiquetaría incorrectamente cada serie sin generar ningún error visible.

**Ejemplo aplicado.**

```python
py_dev_y = [45372, 48876, 53850, 57287, 63016,
            65998, 70003, 70000, 71496, 75370, 83640]
js_dev_y = [37810, 43515, 46823, 49293, 53437,
            56373, 62375, 66674, 68745, 68746, 74583]

plt.plot(ages_x, dev_y, color='#444444', linestyle='--',
         label='Todos los desarrolladores')
plt.plot(ages_x, py_dev_y, color='#5a7d9a', linewidth=3,
         label='Python')
plt.plot(ages_x, js_dev_y, color='#adad3b', linewidth=3,
         label='JavaScript')

plt.legend()
```

En el tramo de 25 a 35 años, los desarrolladores de Python reportan un salario mediano sistemáticamente superior tanto a JavaScript como al conjunto general, con una brecha que se amplía levemente hacia el final del rango.

> **Pregunta de reflexión:** ¿qué riesgo de interpretación errónea se introduciría si, tras reordenar las llamadas a `plot()`, la leyenda se hubiera construido con una lista fija en lugar del argumento `label`?

---

## 5. Rigor y legibilidad gráfica: capas, cuadrícula y estilos predefinidos

Un gráfico estadístico debe garantizar que la audiencia extraiga valores aproximados con precisión razonable y que ninguna serie quede oculta por otra. Las series se dibujan en el orden en que se invocan sus métodos de trazado, de modo que una serie trazada después queda "encima" (orden de capas o *z-order*); una línea gruesa trazada después de una delgada puede ocultarla por completo.

A esto se suman dos recursos de legibilidad estándar: la cuadrícula (`plt.grid(True)`), que facilita la lectura cuantitativa aproximada; y el ajuste automático de márgenes (`plt.tight_layout()`), que evita que etiquetas o títulos queden recortados.

Finalmente, matplotlib incorpora hojas de estilo predefinidas (`plt.style.use('fivethirtyeight')`, `'ggplot'`, entre otras) que modifican de forma consistente la paleta de colores, la tipografía y el fondo del gráfico, transmitiendo señales implícitas de rigor y profesionalismo.

**Ejemplo aplicado.**

```python
plt.style.use('fivethirtyeight')

# La serie de menor grosor se traza al final para quedar visible por encima
plt.plot(ages_x, py_dev_y, linewidth=3, label='Python')
plt.plot(ages_x, js_dev_y, linewidth=3, label='JavaScript')
plt.plot(ages_x, dev_y, color='#444444', linestyle='--',
         label='Todos los desarrolladores')

plt.legend()
plt.grid(True)
plt.tight_layout()
```

El resultado conserva los mismos datos, pero mejora la interpretabilidad: la cuadrícula permite ubicar el salario aproximado a los 30 años sin ambigüedad, y ninguna serie queda oculta detrás de otra.

> **Pregunta de reflexión:** ¿por qué se afirma que el orden en que se invocan los métodos `plot()` es una decisión de diseño y no un detalle técnico irrelevante?

---

## 6. De la muestra reducida a la población completa: tamaño muestral y sesgo de selección

El caso incluye un paso metodológicamente crucial: sustituir el subconjunto de edades de 25 a 35 años por el conjunto completo disponible en la encuesta (18 a 55 años, restringido a edades con suficientes respuestas). Este paso ilustra un principio central de la inferencia estadística: las conclusiones extraídas de una ventana parcial de los datos pueden no generalizarse.

Al incorporar el rango etario completo se observa que la brecha salarial entre Python y los demás lenguajes, que parecía sustancial entre 25 y 35 años, se reduce en edades posteriores, donde otros lenguajes convergen hacia salarios medianos similares. Esto obliga a distinguir entre un patrón real de la población y un artefacto de la ventana de observación elegida.

El criterio de excluir edades con muy pocas respuestas introduce además la noción de confiabilidad del estimador en función del tamaño de la submuestra: una mediana calculada sobre pocas observaciones es un estimador de alta varianza, poco informativo.

**Ejemplo aplicado.**

```python
# Antes: ventana parcial
ages_x = [25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35]

# Después: rango completo con soporte muestral suficiente
ages_x = [18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30,
          31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43,
          44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55]
# (dev_y, py_dev_y y js_dev_y se sustituyen por las series
#  completas correspondientes a cada edad de esta lista)
```

Con el rango completo, la brecha entre Python y los demás lenguajes se concentra en el tramo de 25 a 35 años y se atenúa después. La conclusión correcta ya no es "Python paga sustancialmente más en toda la carrera", sino "la ventaja salarial de Python es más pronunciada en la etapa temprana y se modera posteriormente".

> **Pregunta de reflexión:** en un proyecto de inteligencia artificial, ¿qué paralelismo existe entre restringir arbitrariamente el rango etario de este análisis y restringir arbitrariamente el rango temporal o demográfico de un conjunto de datos de entrenamiento?
