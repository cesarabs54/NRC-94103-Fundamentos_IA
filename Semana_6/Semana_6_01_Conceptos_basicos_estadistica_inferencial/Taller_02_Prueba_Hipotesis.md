# Taller en clase: Prueba de Hipótesis

**Fundamentos para IA · NRC 94103 · Semana 6**

**Dataset de trabajo:** `StudentsPerformance.csv` (1000 registros de estudiantes). Este taller retoma los temas de la presentación *Prueba de Hipótesis: decidir con evidencia*.

**Instrucciones generales:** trabajen en parejas o grupos pequeños. Donde se pida "calcula", usen Python (pandas/scipy) en un notebook — lo importante es justificar la respuesta, no solo dar el número. El ejemplo guiado de la presentación usó `gender` vs. `math score`; en este taller van a aplicar los mismos pasos con **otras variables**, para practicar por su cuenta.

---

## Parte 1 — ¿Qué es una prueba de hipótesis?

**1.1** Explica con tus propias palabras (2-3 líneas) qué problema resuelve una prueba de hipótesis, usando una analogía distinta a la de la moneda cargada (puede ser de la vida diaria, el deporte, la salud, etc.).

**1.2** Marca verdadero o falso y justifica:

a) "Una prueba de hipótesis nos dice con certeza absoluta si algo es verdad." ( )

b) "Una prueba de hipótesis usa una muestra para decidir si un patrón observado es real o producto del azar." ( )

c) "Si tengo los datos de toda la población, ya no necesito una prueba de hipótesis." ( )

---

## Parte 2 — Hipótesis nula (H0) y alternativa (H1)

**2.1** Para cada pregunta de investigación, escribe H0 y H1:

a) ¿El puntaje de escritura (`writing score`) difiere según el tipo de almuerzo (`lunch`: `standard` vs. `free/reduced`)?

b) ¿El puntaje de lectura (`reading score`) difiere entre hombres y mujeres (`gender`)?

c) ¿El nivel educativo de los padres (`parental level of education`, 6 grupos) influye en el puntaje de matemáticas (`math score`)?

**2.2** En el inciso a), identifica la variable dependiente y la independiente.

---

## Parte 3 — Significancia (α) y valor p

**3.1** Para cada resultado, decide si se rechaza o no se rechaza H0 (usando α = 0.05) y explica en una frase qué significa la decisión:

a) p-value = 0.001

b) p-value = 0.08

c) p-value = 0.05 (exactamente)

**3.2** ¿Por qué 0.05 es una *convención* y no una "ley matemática"? Da un ejemplo de otra área (no estadística) donde también usemos un umbral acordado por convención.

---

## Parte 4 — Ejemplo aplicado guiado: `writing score` según `lunch`

Van a repetir, paso a paso, el mismo procedimiento del ejemplo de la presentación (`gender` vs. `math score`), pero ahora con `lunch` vs. `writing score`.

**4.1 Planteamiento.** Escribe la pregunta de investigación y las hipótesis H0/H1 para esta comparación (ya las escribiste en 2.1-a; cópialas aquí).

**4.2 Descripción de la muestra.** Calcula: tamaño de cada grupo (`standard` y `free/reduced`), media y desviación estándar de `writing score` en cada uno.

**4.3 Validación de supuestos.** Calcula Shapiro-Wilk para cada grupo y Levene entre los dos grupos. ¿Se cumple la normalidad? ¿Se cumple la homogeneidad de varianzas?

**4.4 Aplicar la prueba.** Con base en 4.3, decide si usar t de Student o Mann-Whitney U (o ambas, como verificación). Ejecuta la prueba y anota el estadístico y el p-value.

**4.5 Decisión e interpretación.** Compara el p-value con α = 0.05, decide sobre H0, y escribe la conclusión **en lenguaje natural** (sin jerga), conectada con la pregunta de 4.1. No olvides aclarar si es una asociación o si se puede hablar de causalidad.

---

## Parte 5 — Errores comunes al interpretar resultados

**5.1** Un compañero de otro grupo concluye: "Como mi p-value fue 0.30, eso prueba que no hay ninguna diferencia entre los grupos." ¿Qué error de interpretación está cometiendo? Corrige la afirmación.

**5.2** Otro compañero dice: "Mi p-value fue 0.0000001, ¡eso significa que la diferencia entre los grupos es enorme!" ¿Qué está confundiendo? Explica con tus palabras la diferencia entre significancia estadística y tamaño del efecto.

**5.3** Una persona prueba 20 combinaciones distintas de variables del *dataset* hasta encontrar una con p < 0.05, y solo reporta esa. ¿Qué problema tiene esta práctica?

---

## Reto integrador (para terminar fuera de clase, si no alcanza el tiempo)

Elige una variable numérica y una variable categórica del *dataset* **distintas** a las usadas en este taller y en la presentación (por ejemplo, `math score` según `parental level of education`, que tiene 6 grupos en lugar de 2). Con ellas:

1. Plantea una pregunta investigable y define H0/H1.
2. Calcula la estadística descriptiva por grupo.
3. Valida los supuestos (normalidad y homogeneidad de varianzas).
4. Elige y aplica la prueba inferencial adecuada (si hay más de 2 grupos, investiga qué prueba usar en vez de t/Mann-Whitney).
5. Interpreta el resultado en función de tu pregunta, evitando los errores comunes de la Parte 5.
