# Caso de estudio: gráficos de área apilada (stack plots) y proporciones en el tiempo

**Curso:** Fundamentos para Inteligencia Artificial — NRC 94103
**Semana 5:** Estadística básica y visualización de datos
**Fuente:** *Matplotlib Tutorial (Part 4)* — basado en el video de Corey Schafer
**Versión:** Estudiante

## Presentación del caso

Este caso continúa el trabajado en la Parte 3 (`EIARV011_S5_CasoEstudio_Parte3_estudiante.md`, en la carpeta `Matplotlib_Parte_3`), que usó el gráfico de pastel para responder qué proporción del total representa cada categoría **en un único instante**. Aquí se introduce el gráfico de área apilada (`plt.stackplot()`), que responde la misma pregunta pero **a lo largo de una secuencia de tiempo** (minutos, días).

A continuación se desarrolla el caso en cuatro secciones, cada una con una explicación conceptual y un ejemplo aplicado en Python (matplotlib). Al final de cada sección hay una **pregunta de reflexión** para trabajar en clase.

> **Nota de datos.** Este material es autocontenido: usa datos de ejemplo (minutos y puntos de tres jugadores; días y horas de tres desarrolladores) escritos directamente en los fragmentos de código. No requiere `data.csv`, porque un stack plot exige una dimensión temporal que la tabla de lenguajes de programación de las Partes 2 y 3 no tiene.

---

## 1. De la foto a la película: por qué un pastel no alcanza para ver la evolución en el tiempo

El gráfico de pastel (Parte 3) responde "¿qué proporción del total representa cada categoría?" en un único instante: es una fotografía. El gráfico de área apilada (`plt.stackplot()`) responde la misma pregunta, pero a lo largo de una secuencia ordenada (minutos, días, años): es una película.

El caso de ejemplo: tres jugadores acumulan puntos minuto a minuto durante un partido. En el primer minuto los tres tienen 1 punto cada uno (total 3); en el noveno minuto, el jugador 1 tiene 5, el jugador 2 tiene 4 y el jugador 3 tiene 3 (total 12). Un gráfico de pastel puede mostrar la distribución en un minuto específico, pero no puede mostrar cómo esa distribución cambió minuto a minuto.

**Ejemplo aplicado.**

```python
import matplotlib.pyplot as plt
plt.style.use('fivethirtyeight')

minutes = [1, 2, 3, 4, 5, 6, 7, 8, 9]

player1 = [1, 2, 3, 3, 4, 4, 4, 4, 5]
player2 = [1, 1, 1, 1, 2, 2, 3, 3, 4]
player3 = [1, 1, 1, 2, 2, 2, 3, 3, 3]

# La "fotografía": distribución de puntos en el primer minuto (pie chart, Parte 3)
plt.pie([player1[0], player2[0], player3[0]],
        labels=['Jugador 1', 'Jugador 2', 'Jugador 3'])
plt.title('Distribución de puntos en el minuto 1 (una sola fotografía)')
plt.tight_layout()
plt.show()

# La "película": evolución de esa misma distribución a lo largo de los 9 minutos
plt.stackplot(minutes, player1, player2, player3)
plt.title('Puntos acumulados por jugador a lo largo del partido (stack plot básico)')
plt.xlabel('Minuto')
plt.ylabel('Puntos acumulados')
plt.tight_layout()
plt.show()
```

El pastel del minuto 1 muestra una distribución perfectamente equitativa (1-1-1), pero por sí solo no dice nada sobre lo que ocurre después. El stack plot, en cambio, muestra el total acumulado del equipo y, simultáneamente, cómo se reparte ese total entre los tres jugadores en cada instante. Sin etiquetas ni leyenda todavía, no es posible saber qué banda corresponde a qué jugador; eso se resuelve en la Sección 2.

> **Pregunta de reflexión:** si quisieras mostrar, usando únicamente gráficos de pastel, cómo cambió la distribución de puntos entre el jugador 1, 2 y 3 a lo largo de los 9 minutos, ¿qué tendrías que hacer, y por qué el stack plot resuelve el mismo problema de forma más compacta?

---

## 2. Etiquetas y leyenda: ubicación automática vs. ubicación manual

Un stack plot sin etiquetas ni leyenda es ilegible: no hay forma de saber qué banda de color corresponde a qué categoría. Las etiquetas se agregan con el argumento `labels` de `plt.stackplot()`, en el mismo orden en que se pasaron las series; para que aparezcan en el gráfico es necesario, además, llamar a `plt.legend()`.

Por defecto, matplotlib intenta ubicar la leyenda en un lugar que no tape los datos, pero no siempre acierta. El argumento `loc` de `plt.legend()` permite fijar la ubicación explícitamente, con una cadena de texto (`'upper left'`, `'lower left'`, etc.) o con una tupla de coordenadas `(x, y)` para un control más fino.

**Ejemplo aplicado.**

```python
labels = ['Jugador 1', 'Jugador 2', 'Jugador 3']

plt.stackplot(minutes, player1, player2, player3, labels=labels)
plt.legend(loc='upper left')
plt.title('Puntos acumulados por jugador, con etiquetas y leyenda')
plt.xlabel('Minuto')
plt.ylabel('Puntos acumulados')
plt.tight_layout()
plt.show()
```

Con `labels` y `plt.legend()` ya es posible identificar cada banda: la inferior corresponde al jugador 1 (la serie pasada primero a `stackplot`), y así sucesivamente. En este caso `loc='upper left'` funciona bien porque el total del equipo crece con el tiempo y esa esquina queda vacía; si los datos fueran decrecientes, esa misma esquina quedaría ocupada por la pila y convendría usar `'upper right'` o una esquina inferior en su lugar.

> **Pregunta de reflexión:** en la Sección 4 se trabajará un caso donde el total se mantiene constante pero la composición cambia con el tiempo. Antes de verlo, ¿qué esquina del gráfico crees que quedará libre en ese caso, y por qué no puedes usar el mismo criterio ("dónde termina la tendencia") que aquí?

---

## 3. Colores personalizados y la convención posicional

Igual que con los gráficos de barras y de pastel, un stack plot acepta un argumento `colors` con una lista de colores en hexadecimal, uno por serie, en el mismo orden en que se pasaron las series a `plt.stackplot()`. La posición en la lista determina a qué serie se aplica, no el nombre de la categoría.

**Ejemplo aplicado.**

```python
colors = ['#6d904f', '#fc4f30', '#008fd5']  # verde, rojo, azul

plt.stackplot(minutes, player1, player2, player3,
              labels=labels, colors=colors)
plt.legend(loc='upper left')
plt.title('Puntos acumulados por jugador, con colores personalizados')
plt.xlabel('Minuto')
plt.ylabel('Puntos acumulados')
plt.tight_layout()
plt.show()
```

El jugador 1 (primera serie pasada a `stackplot`) se dibuja en verde, el jugador 2 en rojo y el jugador 3 en azul, siguiendo el orden posicional de `colors`. Si se intercambiara el orden de `player2` y `player3` sin también reordenar `labels` y `colors`, cada banda seguiría dibujándose correctamente, pero la leyenda etiquetaría con el nombre equivocado a cada banda.

> **Pregunta de reflexión:** ¿por qué la convención de que `labels`, `colors` y el orden de las series pasadas a `stackplot()` deban mantenerse sincronizados posicionalmente es, al mismo tiempo, una fuente frecuente de errores silenciosos y una decisión de diseño razonable del lado de matplotlib?

---

## 4. Totales constantes y el costo perceptual de leer las capas superiores: el caso del traspaso de un proyecto

Otro uso frecuente de los stack plots es visualizar la composición de un **total que se mantiene constante**, mientras cambia cómo se reparte internamente. Un ejemplo típico: un equipo solo puede facturar 8 horas diarias a un proyecto, y esas 8 horas se reparten entre los desarrolladores asignados; el stack plot permite ver cómo migra la carga de trabajo de un desarrollador a otros a lo largo de varios días, sin que cambie el total facturado.

Como el total no crece ni decrece (siempre suma 8), no existe una esquina del gráfico que quede vacía por efecto de una tendencia neta, y ninguna ubicación estándar de `loc` resulta ideal; por eso se ubica la leyenda con una tupla de coordenadas `(x, y)`, indicando qué tan lejos, en fracción del ancho y del alto del gráfico, debe quedar su esquina inferior izquierda.

**Ejemplo aplicado.**

```python
days = [1, 2, 3, 4, 5, 6, 7, 8, 9]

dev1 = [8, 6, 5, 5, 4, 2, 1, 1, 0]
dev2 = [0, 1, 2, 2, 2, 4, 4, 4, 4]
dev3 = [0, 1, 1, 1, 2, 2, 3, 3, 4]

labels_dev = ['Desarrollador 1', 'Desarrollador 2', 'Desarrollador 3']
colors_dev = ['#6d904f', '#fc4f30', '#008fd5']

plt.stackplot(days, dev1, dev2, dev3, labels=labels_dev, colors=colors_dev)
plt.legend(loc=(0.07, 0.05))
plt.title('Traspaso de un proyecto: horas diarias por desarrollador (total constante = 8h)')
plt.xlabel('Día')
plt.ylabel('Horas facturadas')
plt.tight_layout()
plt.show()
```

El total facturado (la altura completa de la pila) es 8 en los nueve días; lo único que cambia es la composición interna. El desarrollador 1 comienza con el proyecto completo (8 horas) y lo va cediendo día a día hasta llegar a 0, mientras los desarrolladores 2 y 3 absorben progresivamente esas horas hasta terminar con 4 cada uno. La leyenda ubicada en `(0.07, 0.05)` (7 % desde la izquierda, 5 % desde abajo) queda sobre una zona del gráfico que permanece vacía durante todo el periodo.

Vale la pena notar una limitación perceptual del stack plot: la banda inferior (desarrollador 1) se puede leer directamente contra el eje vertical, con precisión, porque su límite inferior es la línea base en cero. Las bandas superiores, en cambio, solo pueden leerse como la *diferencia vertical* entre dos curvas, una operación mental más costosa y propensa a error que leer una altura contra una base fija. Por eso, si lo que más importa es leer con precisión los valores de una serie específica, un gráfico de líneas o de barras agrupadas puede comunicar esa serie con mayor precisión que un stack plot.

> **Pregunta de reflexión:** el video menciona que YouTube Analytics usa un stack plot para mostrar las vistas totales de un video, desglosadas por fuente de tráfico (recomendados, página de inicio, fuentes externas). Si quisieras leer con precisión cuántas vistas exactas provinieron de "fuentes externas" en un día específico y esa serie no es la banda inferior del stack plot, ¿qué dificultad enfrentarías, y qué alternativa de visualización usarías si esa lectura precisa fuera el objetivo principal?
