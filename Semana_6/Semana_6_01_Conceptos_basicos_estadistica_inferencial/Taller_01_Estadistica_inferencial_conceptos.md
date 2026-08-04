# Taller en clase: Estadística Inferencial y Test de Hipótesis

**Fundamentos para IA · NRC 94103 · Semana 6**
**Versión estudiante**

**Dataset de trabajo:** `StudentsPerformance.csv` (1000 registros de estudiantes: género, etnicidad, nivel educativo de los padres, tipo de almuerzo, curso de preparación, y puntajes de matemáticas, lectura y escritura).

**Instrucciones generales:** trabajen en parejas o grupos pequeños. Donde se pida "calcula", usen Python (pandas/scipy) en un notebook, Excel o calculadora — lo importante es justificar la respuesta, no solo dar el número. Este taller retoma los 6 temas de la presentación *Conceptos básicos de Estadística Inferencial y Test de Hipótesis*.

---

## Parte 1 — Estadística descriptiva vs. inferencial

**1.1** Clasifica cada afirmación como **descriptiva** o **inferencial**:

a) "El puntaje promedio de matemáticas en la muestra es 66.09."

b) "Con base en la muestra, el curso de preparación mejora el puntaje de matemáticas de la población de estudiantes."

c) "La desviación estándar del puntaje de escritura es 15.2."

d) "La diferencia de puntaje entre grupos observada en estos 1000 estudiantes es generalizable a todos los estudiantes que presentan este examen."

**1.2** Calcula con el *dataset*: media, mediana y desviación estándar de `writing score` para todo el grupo (los 1000 registros).

**1.3** En tus palabras: ¿qué tipo de pregunta puede responder la estadística inferencial que la descriptiva, por sí sola, no puede?

---

## Parte 2 — Población, muestra y variables

**2.1** Completa la tabla para las 8 columnas del *dataset*:

| Variable | Tipo (numérica / categórica) | Posible rol (dependiente / independiente) |
|---|---|---|
| gender | | |
| race/ethnicity | | |
| parental level of education | | |
| lunch | | |
| test preparation course | | |
| math score | | |
| reading score | | |
| writing score | | |

**2.2** ¿Cuál es la población y cuál la muestra en este estudio? Justifica tu respuesta.

**2.3** Calcula el tamaño de cada grupo de la variable `lunch` (`standard` vs. `free/reduced`). ¿Cuántos registros hay en total?

---

## Parte 3 — Hipótesis: H0 y H1

**3.1** Para cada pregunta de investigación, escribe H0 y H1:

a) ¿El puntaje de lectura (`reading score`) difiere entre hombres y mujeres (`gender`)?

b) ¿El puntaje de escritura (`writing score`) difiere según el tipo de almuerzo (`lunch`)?

c) ¿El nivel educativo de los padres (`parental level of education`, 6 grupos) influye en el puntaje de matemáticas (`math score`)?

d) ¿Existe relación entre el puntaje de matemáticas y el puntaje de lectura?

**3.2** Para cada inciso anterior, identifica cuál es la variable dependiente y cuál la independiente (y explica por qué en el inciso d) es un caso distinto a los demás).

---

## Parte 4 — Significancia, estadístico y p-value

**4.1** Si al aplicar una prueba obtienes un *p-value* = 0.032, con α = 0.05, ¿qué decides sobre H0? Explica.

**4.2** Si obtienes un *p-value* = 0.21, ¿qué decides? Explica.

**4.3** Calcula la media de `reading score` para el grupo `male` y para el grupo `female`. Con esa diferencia, ¿qué tipo de prueba (comparación de cuántos grupos, qué tipo de variables) sería adecuada? (Aún no calcules el p-value, solo justifica el tipo de prueba; eso se resuelve en la Parte 6).

---

## Parte 5 — Errores tipo I y tipo II

**5.1** Describe, usando este *dataset*, un escenario donde cometer un **error tipo I** tendría consecuencias negativas.

**5.2** Describe un escenario donde cometer un **error tipo II** tendría consecuencias negativas.

**5.3** Si en lugar de 1000 estudiantes solo tuviéramos una muestra de 50, ¿qué pasaría con el riesgo de error tipo II? Explica por qué.

---

## Parte 6 — Supuestos y selección de la prueba

**6.1** Aplica la prueba de **Shapiro-Wilk** a `reading score`, por separado para `male` y `female`. ¿Se cumple el supuesto de normalidad en ambos grupos?

**6.2** Aplica la prueba de **Levene** entre esos mismos dos grupos. ¿Se cumple el supuesto de homogeneidad de varianzas?

**6.3** Con base en 6.1 y 6.2, ¿qué prueba usarías para comparar `reading score` entre `male` y `female`: t de Student o Mann-Whitney U? Justifica y ejecútala.

**6.4** Para comparar `math score` entre los 6 niveles de `parental level of education` (más de 2 grupos), ¿qué prueba paramétrica usarías y cuál sería su alternativa no paramétrica? Ejecuta la que corresponda según los supuestos.

---

## Reto integrador (para terminar fuera de clase, si no alcanza el tiempo)

Elige una variable numérica y una variable categórica del *dataset* **distintas** a las usadas en los ejercicios anteriores. Con ellas:

1. Plantea una pregunta investigable.
2. Define H0 y H1.
3. Calcula la estadística descriptiva por grupo.
4. Valida los supuestos (normalidad y homogeneidad de varianzas).
5. Aplica la prueba inferencial adecuada.
6. Interpreta el resultado en función de tu pregunta.
