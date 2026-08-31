# Guía de estudio — Quiz 3
## Proyecto integrador y decisiones con datos

Esta guía integra el ciclo completo del curso: obtener datos de una API, validar su estructura, transformarlos de forma reproducible, analizar con cautela y comunicar una recomendación respaldada por evidencia.

## Cómo funciona el quiz

- Peso en la nota final: **30%**.
- Banco disponible: **20 preguntas**.
- Cada intento presenta **5 preguntas seleccionadas aleatoriamente**.
- Tienes **3 intentos** y se conserva la **nota más alta**.

## 1. Flujo completo de un proyecto analítico

La secuencia recomendada es:

1. Formular la pregunta y la decisión de negocio.
2. Identificar los datos necesarios y sus limitaciones.
3. Obtener los datos y conservar una copia cruda.
4. Validar estado, contenido, esquema, tipos, unidades y cobertura.
5. Limpiar y transformar con pasos reproducibles.
6. Analizar y visualizar.
7. Interpretar sin superar la evidencia.
8. Comunicar hallazgos, recomendación, riesgos y próximos pasos.
9. Realizar revisión humana antes de actuar.

Saltar la validación y pasar directamente de la descarga al gráfico puede producir una conclusión convincente basada en datos incompletos o mal interpretados.

## 2. Consumir una API de forma responsable

Una API permite que un programa solicite datos a otro sistema. Una respuesta HTTP exitosa es necesaria, pero no suficiente.

### Controles mínimos

- Código de estado.
- Tipo de contenido.
- Cuerpo de la respuesta.
- Esquema esperado: campos, niveles anidados y tipos.
- Cantidad de registros.
- Paginación y límites del servicio.
- Parámetros enviados.
- Periodo, zona horaria y cobertura geográfica.
- Valores faltantes o listas vacías.

Una respuesta `200` puede contener una lista vacía, un mensaje de error dentro del JSON o campos con nombres diferentes. Por eso debes validar el contenido antes de analizarlo.

### Fechas y periodos

Si la API recibe fechas, confirma:

- Formato requerido.
- Zona horaria.
- Si los límites son inclusivos o exclusivos.
- Granularidad: día, mes, hora u otra.
- Cobertura real de la fuente.
- Existencia de periodos faltantes.

Una lista vacía no demuestra automáticamente que “no hubo eventos”. Primero revisa filtros, parámetros, cobertura y documentación.

### Cambios de esquema

Si cambia el nombre o la estructura de un campo:

1. Detén el análisis que dependa de ese campo.
2. Inspecciona una muestra del JSON recibido.
3. Compara con el esquema esperado y la documentación.
4. Ajusta la transformación.
5. Ejecuta nuevamente las pruebas de calidad.

No renombres campos a ciegas solo para que el código vuelva a ejecutarse.

## 3. Continuidad, respaldo y seguridad

Una API pública puede cambiar o quedar temporalmente fuera de servicio. Un plan de continuidad incluye:

- Reintentos limitados y manejo claro de errores.
- Copia local o *snapshot* con fecha de obtención.
- Dataset de respaldo conocido.
- Registro de la fuente y del momento de descarga.
- Mensaje que distinga datos actuales de datos de respaldo.

Las claves de API y demás credenciales deben almacenarse en mecanismos de secretos o variables de entorno autorizadas. Nunca deben aparecer en notebooks compartidos, repositorios, capturas de pantalla o prompts.

## 4. De JSON a una tabla analizable

Una respuesta JSON puede contener:

- Listas de registros.
- Objetos anidados.
- Campos opcionales.
- Listas dentro de cada registro.

Antes de convertirla a tabla, identifica la unidad de análisis: ¿cada fila será un producto, una estación, una medición o un evento?

Para objetos anidados se puede usar `pandas.json_normalize()` o una transformación explícita. Después verifica:

- Que no se perdieron registros.
- Que los campos quedaron en la fila correcta.
- Que los tipos son adecuados.
- Que no se multiplicaron filas accidentalmente.

## 5. Uniones, unidades y transformaciones

Antes de unir fuentes:

- Verifica que la llave represente lo mismo en ambas tablas.
- Revisa unicidad y valores faltantes de la llave.
- Estandariza formatos y categorías.
- Convierte unidades a una base común.
- Compara la cobertura temporal.

Kilogramos y libras no se pueden mezclar sin conversión. Tampoco deben compararse montos en monedas distintas o porcentajes almacenados a veces como `0,15` y otras como `15`.

Valida cada transformación con:

- Conteos antes y después.
- Totales de control.
- Filas de muestra.
- Rangos y unidades.
- Duplicados inesperados.

Conserva los datos crudos sin sobrescribir. Esto permite auditar el proceso, corregir una transformación y reproducir el resultado.

## 6. Límites de la evidencia

### Una captura no muestra una tendencia

Una fotografía de un sistema de bicicletas permite describir disponibilidad en ese momento. No basta para afirmar cómo se comporta el sistema durante todo el día o toda la semana. Para eso se necesitan observaciones repetidas y comparables.

### Correlación no implica causalidad

Una asociación puede servir para formular una hipótesis, no para demostrar por sí sola una causa. Pueden existir variables omitidas, selección de casos o coincidencias temporales.

### El promedio puede ocultar segmentos

Si el promedio general mejora, pero la variabilidad aumenta, revisa la distribución y los grupos. Algunos segmentos pueden estar empeorando aunque el promedio sea favorable.

### Dos fuentes pueden usar definiciones distintas

Cuando dos cifras no coinciden, compara:

- Definición del indicador.
- Periodo y zona horaria.
- Población cubierta.
- Filtros y exclusiones.
- Fecha de actualización.
- Método de cálculo.

No elijas simplemente la cifra que conviene a la narrativa.

## 7. Reproducibilidad

Un análisis reproducible permite que otra persona repita el proceso y entienda cómo se obtuvo el resultado.

Incluye:

- Fuente y fecha de descarga.
- Dependencias e instrucciones de ejecución.
- Celdas ejecutables en orden.
- Datos crudos o mecanismo de respaldo permitido.
- Transformaciones explícitas.
- Parámetros y supuestos.
- Semillas aleatorias cuando corresponda.
- Resultados de controles de calidad.

Evita depender de cambios manuales no documentados.

## 8. Comunicar para decidir

Un resumen ejecutivo sólido responde:

1. ¿Cuál era el objetivo?
2. ¿Qué datos y periodo se analizaron?
3. ¿Cuál es el hallazgo principal y qué cifra lo respalda?
4. ¿Qué acción se recomienda?
5. ¿Qué riesgo o limitación debe considerar el decisor?
6. ¿Qué indicador permitirá evaluar la acción?
7. ¿Cuál es el siguiente paso?

Una recomendación responsable puede proponer una prueba pequeña y reversible:

> Ejecutar un piloto en un segmento definido durante cuatro semanas, medir el indicador acordado y revisar el resultado antes de ampliar la intervención.

Este enfoque es preferible a una afirmación causal no demostrada o a una implementación total sin control.

## 9. Casos para practicar

Explica la respuesta responsable en cada situación:

1. La API devuelve `200`, pero cambió los nombres de varios campos.
2. La respuesta es una lista vacía y no hay un error visible.
3. Dos fuentes registran el mismo peso en unidades diferentes.
4. Una unión duplica el número de filas.
5. El promedio mejora, pero algunos grupos empeoran.
6. Dos fuentes muestran cifras distintas para el mismo indicador.
7. Una captura instantánea parece mostrar un patrón diario.
8. Dos variables tienen una correlación alta.
9. El análisis debe seguir funcionando si la API pública falla.
10. La recomendación afectará un proceso importante de la organización.

En cada explicación incluye qué validar, qué límite comunicar y qué acción humana se necesita.

## Lista de comprobación antes de presentar

- [ ] Puedo ordenar las etapas de un proyecto analítico completo.
- [ ] Valido estado, contenido y esquema de una respuesta de API.
- [ ] Reviso parámetros, fechas, zona horaria, cobertura y paginación.
- [ ] Sé interpretar responsablemente una respuesta vacía.
- [ ] Protejo claves y credenciales.
- [ ] Normalizo JSON y verifico que no se pierdan o dupliquen filas.
- [ ] Estandarizo unidades antes de unir o comparar fuentes.
- [ ] Valido transformaciones con conteos, totales y muestras.
- [ ] Conservo datos crudos y documento los pasos.
- [ ] Distingo captura, tendencia, correlación, hipótesis y causalidad.
- [ ] Comunico evidencia, recomendación, riesgo, KPI y siguiente paso.
- [ ] Mantengo revisión humana antes de ejecutar una decisión importante.

## Material relacionado

- [Clase virtual — APIs](../clases/ClaseVirtual-API.ipynb)
- [Clase 3 — Proyecto integrador](../clases/Clase3-Proyecto-Integrador.ipynb)
- [AI Reliability Checklist](../complementarios/AI-Reliability-Checklist.md)
- [Guía de ética y privacidad](../complementarios/Data-Ethics-Guide.md)
- [Opciones de práctica integradora](../taller-final/)

