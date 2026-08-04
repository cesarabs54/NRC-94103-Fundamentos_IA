# La estadística explicada

Esta hoja te explica, con ejemplos sencillos, las ideas que necesitas para resolver la actividad de la Semana 6 (el portafolio con el archivo `StudentsPerformance.csv`, que tiene las notas de 1000 estudiantes).

---

## 1. Estadística descriptiva vs. estadística inferencial

Imagina que cuentas cuántos dulces tiene cada compañero de tu salón, y sacas el promedio. Eso es solo **contar y resumir lo que ya viste**: eso es la **estadística descriptiva**.

Ahora imagina que, con esos datos de tu salón, quieres adivinar cuántos dulces tienen en promedio **todos los niños del país**, aunque no los hayas contado a todos. Eso es **adivinar con evidencia**, y así funciona la **estadística inferencial**: usar un grupo pequeño (lo que sí mediste) para sacar una conclusión sobre un grupo mucho más grande (lo que no mediste).

En nuestra actividad: contamos las notas de 1000 estudiantes (descriptiva) para tratar de entender qué pasa con **todos** los estudiantes que presentan ese examen (inferencial).

## 2. Población y muestra

- **Población** = TODOS los estudiantes que existen y presentan este examen en el mundo. Es imposible preguntarles a todos.
- **Muestra** = el grupito que sí alcanzamos a medir: los 1000 estudiantes del archivo.

Es como si quisieras saber qué película le gusta más a todo tu colegio, pero solo puedes preguntarle a un salón. Ese salón es tu muestra; el colegio completo es tu población.

## 3. Variables: la que "causa" y la que "se mide"

En nuestros datos hay cosas que se miden (los puntajes) y cosas que podrían explicar por qué el puntaje sube o baja (por ejemplo, si el estudiante tomó o no un curso de preparación).

- **Variable dependiente**: la nota de matemáticas (`math score`). Es la que queremos explicar.
- **Variable independiente**: si tomó el curso de preparación (`test preparation course`). Es la que podría estar influyendo.

Piénsalo como plantas: si riegas una planta y no riegas otra, "regar o no regar" es la variable independiente, y "qué tan alto creció" es la variable dependiente.

## 4. Hipótesis: H0 y H1 (como una apuesta)

Cuando queremos comprobar algo, hacemos como una apuesta con dos opciones:

- **H0 (hipótesis nula)**: "no pasa nada especial, todo es igual". Ejemplo: "el curso de preparación NO cambia la nota de matemáticas".
- **H1 (hipótesis alternativa)**: "sí pasa algo, sí hay diferencia". Ejemplo: "el curso de preparación SÍ cambia la nota de matemáticas".

Con los datos, buscamos evidencia para decidir cuál de las dos apuestas gana.

## 5. El p-value: "¿qué tan sorprendido deberías estar?"

El **p-value** es un número que te dice qué tan raro (o normal) sería el resultado que viste, **si en realidad H0 fuera cierta** (si de verdad no pasara nada especial).

- Si el p-value es **muy chiquito** (menos de 0.05), es como si dijeras: "¡qué casualidad tan rara! Esto casi no podría pasar por azar" → entonces **rechazamos H0** (creemos que sí hay diferencia real).
- Si el p-value es **grande** (0.05 o más), es como decir: "esto podría pasar fácilmente por azar, no es tan raro" → entonces **no rechazamos H0** (no hay evidencia suficiente de diferencia).

El número 0.05 es como la regla del juego que usamos siempre: "si la probabilidad de que sea pura casualidad es menor al 5%, le creemos a H1".

## 6. Dos formas de equivocarse (errores tipo I y tipo II)

Imagina la alarma contra incendios de tu casa:

- **Error tipo I** (falsa alarma): la alarma suena, pero no hay ningún incendio. Aquí sería: decir "sí hay diferencia" cuando en realidad NO la hay.
- **Error tipo II** (no darte cuenta): hay un incendio de verdad, pero la alarma NO suena. Aquí sería: decir "no hay diferencia" cuando en realidad SÍ la hay.

Nadie puede evitar los dos errores al 100% al mismo tiempo, pero mientras más datos tengamos (como nuestros 1000 estudiantes), menos probable es que nos equivoquemos sin darnos cuenta (error tipo II).

## 7. ¿Los datos se comportan "normal"? (los supuestos)

Antes de usar ciertas herramientas estadísticas, hay que revisar si los datos se comportan de una manera esperada, como si midieras la estatura de tus compañeros: la mayoría está en la mitad (ni muy bajitos ni muy altos), y pocos están en los extremos. Eso forma una especie de "campana". A esto le llamamos **normalidad**.

También revisamos si los grupos que comparamos tienen una variedad parecida (que unos no estén súper "revueltos" y otros súper "parejos"). A esto le llamamos **homogeneidad de varianzas**.

Estas revisiones son como probar el agua con el dedo antes de meterte a la piscina: te dicen qué herramienta (prueba estadística) es segura usar después.

## 8. ¿Qué herramienta uso? (elegir la prueba)

Según lo que encontremos al "probar el agua":

- Si los datos se comportan "normal" → usamos herramientas **paramétricas** (como la prueba t o ANOVA).
- Si los datos NO se comportan "normal" → usamos herramientas **no paramétricas** (como Mann-Whitney U o Kruskal-Wallis), que funcionan igual de bien sin necesitar esa forma de campana.

---

## Resumen para tu portafolio (en una frase cada uno)

1. **Descriptiva**: resumo lo que veo. **Inferencial**: adivino con evidencia sobre todos, a partir de una parte.
2. **Población**: todos. **Muestra**: el grupo que sí medí.
3. **Variable dependiente**: lo que se mide. **Independiente**: lo que podría influir.
4. **H0**: no pasa nada. **H1**: sí pasa algo.
5. **p-value**: qué tan raro sería el resultado si H0 fuera cierta. Si es menor a 0.05, le creo a H1.
6. **Error tipo I**: falsa alarma. **Error tipo II**: no darme cuenta de algo real.
7. **Supuestos**: reviso cómo se comportan los datos antes de elegir la herramienta.
8. **Prueba**: paramétrica si los datos son "normales"; no paramétrica si no lo son.
