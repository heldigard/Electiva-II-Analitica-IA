# Guía de estudio — Quiz 2
## Análisis, visualización e insights

Esta guía prepara el razonamiento necesario para el Quiz 2: comenzar con una pregunta de negocio, revisar la calidad de los datos, elegir una visualización coherente e interpretar la evidencia sin exagerarla.

## Cómo funciona el quiz

- Peso en la nota final: **27,5%**.
- Banco disponible: **20 preguntas**.
- Cada intento presenta **5 preguntas seleccionadas aleatoriamente**.
- Tienes **3 intentos** y se conserva la **nota más alta**.
- Disponible desde el **19 de octubre de 2026 a las 00:00** hasta el **9 de noviembre de 2026 a las 23:59**.

## 1. El análisis comienza con una decisión

Antes de abrir un notebook o crear un gráfico, define:

- Problema de negocio.
- Pregunta específica.
- Métrica o evidencia necesaria.
- Audiencia del análisis.
- Decisión que se tomará.
- Riesgo de interpretar mal el resultado.

“Analizar las ventas” es demasiado general. Una pregunta útil sería:

> ¿Qué categorías explicaron la mayor parte de la caída de ingresos del último trimestre, frente al trimestre anterior, para priorizar acciones comerciales?

Esta formulación indica qué comparar, en qué periodo y para qué decisión.

## 2. Exploración inicial y calidad de datos

Tres comandos de `pandas` ayudan a entender rápidamente una tabla:

| Comando | Pregunta que ayuda a responder |
|---|---|
| `df.head()` | ¿Cómo se ven los registros y las columnas? |
| `df.info()` | ¿Cuántos datos hay, qué tipos tienen y dónde faltan valores? |
| `df.describe()` | ¿Cómo se distribuyen las variables numéricas? |

Para variables de texto o categorías, usa también `df.describe(include="object")` y revisa frecuencias con `value_counts()`.

### Controles básicos

- Valores faltantes.
- Filas duplicadas.
- Tipos incorrectos: fechas guardadas como texto o números como cadenas.
- Rangos imposibles o unidades inconsistentes.
- Categorías con variantes de escritura.
- Celdas que contienen varios valores.
- Valores atípicos.

### No todos los problemas se corrigen igual

| Situación | Acción responsable |
|---|---|
| `Colombia`, `colombia`, `COLOMBIA` | Normalizar escritura después de confirmar que significan lo mismo |
| `Colombia, Perú` en una sola celda | Separar o “explotar” los valores si el análisis requiere contar países individualmente |
| Una fila repetida por error de carga | Eliminarla después de verificar que sí es un duplicado |
| Un valor extremo posible | Investigarlo; no eliminarlo automáticamente |
| Un dato faltante | Decidir según su causa, proporción y efecto en la pregunta de negocio |

Un valor atípico puede ser un error, pero también un caso de negocio importante. Primero confirma su origen y después decide conservarlo, corregirlo, segmentarlo o excluirlo con una justificación explícita.

## 3. Elegir el gráfico correcto

La elección depende de la pregunta y de los tipos de variables.

| Necesidad | Gráfico recomendado | Cuidado principal |
|---|---|---|
| Comparar categorías | Barras | Ordenar, usar una escala legible y evitar demasiadas categorías |
| Ver una evolución temporal | Líneas | Ordenar las fechas y revisar periodos faltantes |
| Entender una distribución | Histograma | Elegir intervalos razonables y leer forma, centro y dispersión |
| Explorar relación entre dos variables numéricas | Dispersión | No convertir correlación en causalidad |
| Comparar distribución y detectar atípicos entre grupos | Boxplot (diagrama de caja) | Explicar mediana, cuartiles y puntos extremos |

Si hay demasiadas categorías, suele ser mejor ordenar, mostrar las principales y agrupar el resto como “Otros”, siempre que esa agrupación no oculte información relevante.

### Series de tiempo

Antes de dibujar una línea:

1. Convierte la columna a fecha.
2. Ordena cronológicamente.
3. Comprueba que la frecuencia sea consistente.
4. Identifica periodos ausentes.
5. Evita unir con una línea observaciones que no representan periodos comparables.

## 4. Un gráfico también debe ser verificable

Un gráfico profesional necesita:

- Título que exprese la pregunta o el mensaje.
- Ejes con nombres y unidades.
- Periodo y población analizada.
- Leyenda cuando aporta información.
- Fuente de los datos.
- Escala que no exagere diferencias.

Truncar el eje vertical puede hacer que una diferencia pequeña parezca enorme. Si la escala no empieza en cero, debe existir una razón analítica clara y el lector debe poder identificarlo.

El color debe resaltar información, no decorar. Evita 3D, leyendas redundantes y elementos que dificulten la comparación.

## 5. Observación, insight y recomendación

Una **observación** describe lo que aparece en los datos:

> La categoría A representa el 42% de las ventas del periodo.

Un **insight** conecta evidencia, contexto y una posible decisión:

> La categoría A concentra el 42% de las ventas y también la mayor caída frente al trimestre anterior; conviene revisar primero disponibilidad y cambios de precio en esa categoría. Los datos actuales no permiten afirmar cuál fue la causa.

Un insight sólido contiene:

1. Hallazgo cuantificado.
2. Comparación o contexto.
3. Relevancia para la decisión.
4. Acción o siguiente análisis.
5. Limitación de la evidencia.

No presentes una causa como hecho si solo observaste una asociación. Usa expresiones como “se relaciona con”, “coincide con” o “sugiere una hipótesis que debe comprobarse”.

## 6. Validar antes de interpretar

Antes de aceptar un gráfico o una conclusión:

- Compara los totales del gráfico con una tabla resumen.
- Revisa conteos antes y después de filtrar, unir o agrupar.
- Confirma unidades y denominadores.
- Observa filas de muestra.
- Repite el cálculo principal de otra forma.
- Comprueba que el título corresponde a lo realmente medido.
- Separa los resultados de los supuestos.

Si el código falla, comparte con la IA el error exacto, el fragmento mínimo reproducible, la estructura de los datos y el resultado esperado. Si el código funciona, todavía debes validar su lógica.

## 7. Casos para practicar

Decide qué control, gráfico o interpretación corresponde:

1. Quieres comparar el total de ventas de ocho categorías.
2. Necesitas ver cómo cambió un indicador cada mes durante dos años.
3. Quieres conocer la forma de la distribución de edades.
4. Debes estudiar la relación entre precio y calificación.
5. Quieres comparar la dispersión de tiempos de atención entre equipos.
6. Una columna de país contiene dos países separados por coma.
7. Un punto aparece muy lejos del resto de observaciones.
8. El promedio mejora, pero algunos grupos empeoran.
9. El gráfico muestra una diferencia dramática porque el eje inicia muy cerca del valor mínimo.
10. Una correlación alta aparece entre dos variables, pero no existe un diseño causal.

En cada caso, explica también qué verificarías antes de comunicar el resultado.

## Lista de comprobación antes de presentar

- [ ] Puedo convertir un problema general en una pregunta de negocio específica.
- [ ] Sé para qué sirven `head()`, `info()` y `describe()`.
- [ ] Distingo duplicados, faltantes, categorías inconsistentes y celdas multivalor.
- [ ] Investigo los valores atípicos antes de eliminarlos.
- [ ] Elijo barras, líneas, histograma, dispersión o boxplot (diagrama de caja) según la pregunta.
- [ ] Reviso orden temporal, periodos faltantes, unidades, ejes y etiquetas.
- [ ] Distingo una observación de un insight accionable.
- [ ] No confundo correlación con causalidad.
- [ ] Comunico evidencia, recomendación y limitaciones.

## Material relacionado

- [Clase 2 — Análisis y visualización](../clases/Clase2-Analisis-Visualizacion.ipynb)
- [Business Question Canvas](../complementarios/Business-Question-Canvas.md)
- [Prompt Playbook](../complementarios/Prompt-Playbook.md)
- [Plantillas de prompts 2026](../complementarios/Prompt-Templates-2026.md)
- [AI Reliability Checklist](../complementarios/AI-Reliability-Checklist.md)
