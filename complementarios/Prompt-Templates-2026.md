# Prompt Templates 2026
## Plantillas de Prompts Efectivos para Analítica con IA

### Universidad UdeColombia - Electiva Analítica de Datos con IA

---

## Introducción

Este documento contiene **plantillas de prompts probadas** para análisis de datos con IA. Usa estas plantillas como punto de partida y adáptalas a tu contexto específico.

**Actualizado para:** Enero 2026
**Compatible con:** ChatGPT, Claude, Qwen, Gemini, Grok, Kimi K2, Julius AI

---

## Estructura de un Prompt Efectivo

```
CONTEXTO: [Breve descripción del problema y dataset]

PREGUNTA DE NEGOCIO: [Pregunta específica que quieres responder]

TAREA: [Qué quieres que la IA haga exactamente]

FORMATO DE RESPUESTA: [Cómo quieres recibir la respuesta]
```

---

## Categorías de Plantillas

1. **Exploración de Datos** - Entender qué tienes
2. **Limpieza de Datos** - Preparar datos para análisis
3. **Análisis Estadístico** - Extraer insights numéricos
4. **Visualización** - Crear gráficos efectivos
5. **Análisis de Negocio** - Responder preguntas estratégicas
6. **Machine Learning Básico** - Modelos simples
7. **Optimización** - Encontrar mejores opciones

---

## 1. Exploración de Datos

### Plantilla 1.1: Exploración Inicial

```
CONTEXTO:
Tengo un DataFrame de pandas llamado 'df' con datos de [describir contexto].
Las columnas son: [listar columnas principales].

PREGUNTA DE NEGOCIO:
¿Qué estructura tienen estos datos y qué problemas de calidad debo conocer?

TAREA:
1. Muestra df.info() para ver tipos de datos y valores faltantes
2. Muestra df.describe() para ver estadísticas descriptivas
3. Muestra df.head(10) para ver las primeras filas
4. Identifica columnas con valores faltantes
5. Identifica posibles outliers (valores extremos)
6. Verifica si hay duplicados

FORMATO DE RESPUESTA:
- Primero muestra el código, luego los resultados
- Interpreta cada sección: qué significa para la calidad de los datos
- Identifica los 3 problemas de calidad más importantes que debo abordar
```

### Plantilla 1.2: Perfil de Columna

```
CONTEXTO:
Tengo un DataFrame 'df' con una columna llamada '[NOMBRE_COLUMNA]'.

PREGUNTA DE NEGOCIO:
¿Cuál es la distribución y características de esta columna?

TAREA:
1. Identifica el tipo de dato (numérica, categórica, fecha)
2. Si es numérica: muestra distribución, percentiles, outliers
3. Si es categórica: muestra valores únicos, frecuencia de cada uno
4. Si es fecha: muestra rango temporal, distribución temporal
5. Identifica valores faltantes y qué hacer con ellos

FORMATO DE RESPUESTA:
Tabla con:
- Tipo de dato
- Valores únicos / posibles valores
- Valores faltantes (count y %)
- Para numéricas: min, max, media, mediana, std
- Para categóricas: top 5 valores con conteo
- Recomendación de cómo tratar valores faltantes
```

---

## 2. Limpieza de Datos

### Plantilla 2.1: Manejo de Valores Faltantes

```
CONTEXTO:
Tengo un DataFrame 'df' con valores faltantes en [NOMBRE_COLUMNA].

PREGUNTA DE NEGOCIO:
¿Cómo debo manejar estos valores faltantes sin perder información importante?

TAREA:
1. Calcula el % de valores faltantes
2. Analiza si los valores faltantes son aleatorios o tienen patrón
3. Compara 3 estrategias:
   a) Eliminar filas con valores faltantes
   b) Reemplazar con promedio/mediana (si numérica)
   c) Reemplazar con valor específico (ej: "Desconocido")
4. Recomienda la mejor estrategia con justificación

FORMATO DE RESPUESTA:
- Código para implementar la estrategia recomendada
- Antes y después: cantidad de filas, % de valores faltantes
- Justificación de por qué esta estrategia es la mejor
```

### Plantilla 2.2: Eliminar Duplicados

```
CONTEXTO:
Tengo un DataFrame 'df' que puede tener filas duplicadas.

PREGUNTA DE NEGOCIO:
¿Cómo identifico y elimino duplicados sin perder datos legítimos?

TAREA:
1. Identifica si hay filas completamente duplicadas
2. Si hay columnas ID, verifica si hay duplicados en esas columnas
3. Muestra ejemplos de duplicados encontrados
4. Elimina duplicados manteniendo la primera ocurrencia
5. Reporte: antes (n filas) → después (n filas), eliminadas (n filas)

FORMATO DE RESPUESTA:
- Código paso a paso con comentarios
- Antes/después con números
- Verificación de que no quedan duplicados
```

### Plantilla 2.3: Transformación de Tipos

```
CONTEXTO:
Tengo un DataFrame 'df' con columnas que tienen el tipo incorrecto.

PREGUNTA DE NEGOCIO:
¿Cómo convierto estas columnas al tipo correcto?

Columnas a transformar:
- [COLUMNA1]: actualmente [TIPO], debería ser [TIPO]
- [COLUMNA2]: actualmente [TIPO], debería ser [TIPO]

TAREA:
1. Para cada columna, muestra el tipo actual
2. Transforma al tipo correcto
3. Maneja errores de conversión si existen
4. Verifica que la transformación fue exitosa

FORMATO DE RESPUESTA:
- Código con manejo de errores (try/except)
- Muestra filas problemáticas si hay errores
- Confirmación de tipos finales
```

---

## 3. Análisis Estadístico

### Plantilla 3.1: Análisis Comparativo (Dos Grupos)

```
CONTEXTO:
Tengo un DataFrame 'df' con datos de [DESCRIPCIÓN].
Quiero comparar dos grupos: [GRUPO1] vs [GRUPO2].

PREGUNTA DE NEGOCIO:
¿Existe una diferencia significativa entre [GRUPO1] y [GRUPO2] en [MÉTRICA]?

TAREA:
1. Filtra datos para cada grupo
2. Calcula estadísticas para cada grupo (media, mediana, desviación)
3. Compara los dos grupos
4. Si aplica, realiza prueba estadística (t-test o similar)
5. Interpreta el resultado en términos de negocio

FORMATO DE RESPUESTA:
Tabla comparativa:
| Métrica | Grupo 1 | Grupo 2 | Diferencia |
|---------|---------|---------|------------|
| Media   | X       | Y       | ±Z         |

Conclusión:
- ¿Hay diferencia significativa?
- ¿Qué significa esto para el negocio?
- ¿Qué acción recomendar?
```

### Plantilla 3.2: Correlación Entre Variables

```
CONTEXTO:
Tengo un DataFrame 'df' con variables numéricas.
Quiero entender la relación entre [VARIABLE1] y [VARIABLE2].

PREGUNTA DE NEGOCIO:
¿Existe correlación entre [VARIABLE1] y [VARIABLE2]? Si existe, ¿qué significa?

TAREA:
1. Calcula el coeficiente de correlación de Pearson
2. Crea un scatter plot para visualizar la relación
3. Si hay correlación, interpreta la dirección y fuerza
4. Identifica outliers que puedan afectar la correlación
5. Verifica si la correlación es espuria o tiene sentido causal

FORMATO DE RESPUESTA:
- Coeficiente de correlación (r)
- Gráfico scatter con línea de tendencia
- Interpretación: "Hay una correlación [positiva/negativa] [débil/moderada/fuerte]"
- Explicación en lenguaje de negocio: "Cuando X aumenta, Y tiende a..."
- Advertencia sobre correlación ≠ causalidad
```

### Plantilla 3.3: Análisis de Tendencia Temporal

```
CONTEXTO:
Tengo un DataFrame 'df' con una columna de fecha '[FECHA]' y una métrica '[MÉTRICA]'.

PREGUNTA DE NEGOCIO:
¿Cuál es la tendencia de [MÉTRICA] a lo largo del tiempo?

TAREA:
1. Asegúrate de que la columna de fecha esté en formato datetime
2. Agrupa por período relevante (día, semana, mes, trimestre)
3. Calcula métrica por período
4. Identifica tendencia general (creciente, decreciente, estable)
5. Identifica estacionalidad (patrones que se repiten)
6. Destaca picos y valles con posibles explicaciones

FORMATO DE RESPUESTA:
- Gráfico de línea con [MÉTRICA] vs tiempo
- Línea de tendencia si aplica
- Análisis de estacionalidad
- Lista de 3-5 insights clave:
  * Insight 1: [Descripción con evidencia numérica]
  * Insight 2: [Descripción con evidencia numérica]
  * ...
```

---

## 4. Visualización

### Plantilla 4.1: Histograma (Distribución)

```
CONTEXTO:
Tengo un DataFrame 'df' con columna numérica '[COLUMNA]'.

PREGUNTA DE NEGOCIO:
¿Cuál es la distribución de [COLUMNA]?

TAREA:
1. Crea un histograma de [COLUMNA]
2. Ajusta el número de bins para mejor visualización
3. Añade línea vertical con la media
4. Añade líneas verticales con percentiles 25 y 75 (Q1, Q3)
5. Interpreta la distribución (simétrica, sesgada, normal, etc.)

FORMATO DE RESPUESTA:
- Código de matplotlib/seaborn para el histograma
- Título: "Distribución de [NOMBRE]"
- Eje X: [COLUMNA]
- Eje Y: Frecuencia
- Anotaciones con media, Q1, Q3
- Interpretación de la forma de la distribución
- Qué significa para el negocio
```

### Plantilla 4.2: Gráfico de Barras (Comparación)

```
CONTEXTO:
Tengo un DataFrame 'df' con columna categórica '[CATEGORIA]' y numérica '[MÉTRICA]'.

PREGUNTA DE NEGOCIO:
¿Cómo se compara [MÉTRICA] entre las diferentes [CATEGORIA]?

TAREA:
1. Agrupa [MÉTRICA] por [CATEGORIA]
2. Ordena resultados de mayor a menor (o al revés según el caso)
3. Crea gráfico de barras horizontales (mejor para leer etiquetas)
4. Añade colores para destacar valores sobre/bajo umbral
5. Interpreta: ¿qué categorías destacan?

FORMATO DE RESPUESTA:
- Código de gráfico de barras
- Eje Y: [CATEGORIA]
- Eje X: [MÉTRICA]
- Colores: por ejemplo, rojo si está por debajo del promedio
- Top 3 y Bottom 3 identificados
- Insight de negocio sobre las diferencias
```

### Plantilla 4.3: Boxplot (Outliers)

```
CONTEXTO:
Tengo un DataFrame 'df' con [COLUMNA_NUMERICA] y [COLUMNA_CATEGORICA].

PREGUNTA DE NEGOCIO:
¿Existen outliers en [COLUMNA_NUMERICA]? ¿Cómo varía entre [COLUMNA_CATEGORICA]?

TAREA:
1. Crea un boxplot de [COLUMNA_NUMERICA] por [COLUMNA_CATEGORICA]
2. Identifica visualmente outliers
3. Calcula IQR (rango intercuartílico) para definir outliers numéricamente
4. Lista los outliers encontrados
5. Recomienda qué hacer con ellos (mantener, investigar, eliminar)

FORMATO DE RESPUESTA:
- Código de seaborn boxplot
- Eje X: [CATEGORIA]
- Eje Y: [NUMERICA]
- Lista de outliers con sus valores
- Recomendación accionable para cada outlier o grupo
```

### Plantilla 4.4: Gráfico Narrativo

```
CONTEXTO:
Tengo un DataFrame 'df' y quiero crear un gráfico que cuente una historia.

PREGUNTA DE NEGOCIO:
¿Cómo visualizo [INSIGHT] de manera que sea claro y accionable?

TAREA:
Crea un gráfico narrativo con:
1. HEADLINE: Título claro que comunica el insight principal
2. ANOTACIONES: Flechas o texto que destacan puntos clave
3. EVIDENCIA: Números específicos en el gráfico
4. CALL-TO-ACTION: Qué debería hacer el audiencia

FORMATO DE RESPUESTA:
- Código completo del gráfico
- Elementos narrativos integrados
- Ejemplo:
  * Headline: "Los tickets de Billing tardan 3x más que otros tipos"
  * Anotación: Flecha hacia barra de Billing con texto "72h vs 24h promedio"
  * Evidencia: "72 horas promedio (n=234 tickets)"
  * Call-to-action: "Priorizar documentación de facturación y automatizar respuestas"
```

---

## 5. Análisis de Negocio

### Plantilla 5.1: Análisis de Pareto (80/20)

```
CONTEXTO:
Tengo un DataFrame 'df' con [CATEGORIA] y [MÉTRICA].

PREGUNTA DE NEGOCIO:
¿Qué [CATEGORIA] explican el 80% de [MÉTRICA]? (Regla 80/20)

TAREA:
1. Calcula [MÉTRICA] total
2. Calcula [MÉTRICA] por [CATEGORIA]
3. Ordena de mayor a menor
4. Calcula el % acumulado
5. Identifica qué [CATEGORIA] suman el 80%
6. Crea gráfico de barras con línea acumulada

FORMATO DE RESPUESTA:
- Lista de categorías que explican 80%
- Código de gráfico (barras + línea acumulada)
- Insight: "X categorías (de Y total) explican el 80% de [MÉTRICA]"
- Recomendación: "Enfocar esfuerzos en estas X categorías"
```

### Plantilla 5.2: Análisis de Cohortes

```
CONTEXTO:
Tengo un DataFrame 'df' con datos de clientes a lo largo del tiempo.
Columnas: [CLIENTE_ID], [FECHA_INICIO], [FECHA_EVENTO], [MÉTRICA].

PREGUNTA DE NEGOCIO:
¿Cómo se comportan diferentes cohorts de clientes en el tiempo?

TAREA:
1. Define cohorts basados en [CRITERIO] (ej: mes de inicio)
2. Crea matriz de cohorts: período de inicio vs período actual
3. Calcula [MÉTRICA] para cada celda de la matriz
4. Crea heatmap para visualizar
5. Interpreta patrones (retención, crecimiento, declive)

FORMATO DE RESPUESTA:
- Matriz de cohorts (DataFrame)
- Heatmap de seaborn
- Interpretación de patrones principales
- Recomendaciones basadas en patrones
```

### Plantilla 5.3: Análisis What-If

```
CONTEXTO:
Tengo un DataFrame 'df' con [VARIABLES] y [RESULTADO].

PREGUNTA DE NEGOCIO:
¿Qué pasaría con [RESULTADO] si cambio [VARIABLE] en X%?

TAREA:
1. Calcula [RESULTADO] actual
2. Simula escenarios cambiando [VARIABLE]:
   - Escenario 1: [VARIABLE] +10%
   - Escenario 2: [VARIABLE] -10%
   - Escenario 3: [VARIABLE] al mejor valor histórico
3. Para cada escenario, calcula nuevo [RESULTADO]
4. Compara escenarios vs actual
5. Identifica escenario óptimo

FORMATO DE RESPUESTA:
- Tabla comparativa de escenarios
| Escenario | [VARIABLE] | [RESULTADO] | Variación |
|-----------|------------|------------|-----------|
| Actual    | X          | Y          | -         |
| +10%      | X+10%      | Y'         | ±Z%       |
| ...       | ...        | ...        | ...       |

- Recomendación: "El escenario óptimo es [X] porque..."
- Sensibilidad: "Por cada 1% de cambio en [VARIABLE], [RESULTADO] cambia en Z%"
```

---

## 6. Machine Learning Básico

### Plantilla 6.1: Regresión Lineal Simple

```
CONTEXTO:
Tengo un DataFrame 'df' con [VARIABLE_X] (independiente) y [VARIABLE_Y] (dependiente).

PREGUNTA DE NEGOCIO:
¿Puedo predecir [VARIABLE_Y] basado en [VARIABLE_X]? ¿Qué tan precisa es la predicción?

TAREA:
1. Divide datos en train (80%) y test (20%)
2. Entrena modelo de regresión lineal
3. Evalúa con R² y RMSE
4. Interpreta coeficientes
5. Crea gráfico con puntos reales y línea de predicción
6. Identifica residuales (errores) grandes

FORMATO DE RESPUESTA:
- Ecuación del modelo: Y = aX + b
- R² (qué % de varianza explica)
- RMSE (error promedio en unidades de Y)
- Interpretación: "Por cada unidad adicional de X, Y cambia en Z"
- Gráfico con dispersión + línea
- Limitaciones del modelo
```

### Plantilla 6.2: Clasificación Simple

```
CONTEXTO:
Tengo un DataFrame 'df' con [VARIABLES_PREDICTORAS] y [VARIABLE_OBJETIVO] (binaria).

PREGUNTA DE NEGOCIO:
¿Puedo clasificar correctamente [CATEGORÍA] basado en [VARIABLES]?

TAREA:
1. Divide datos en train (80%) y test (20%)
2. Entrena modelo de clasificación (Logistic Regression o Decision Tree)
3. Evalúa con accuracy, precision, recall, F1
4. Muestra matriz de confusión
5. Identifica qué variables son más importantes

FORMATO DE RESPUESTA:
- Accuracy: X% (qué % clasifica correctamente)
- Matriz de confusión con interpretación
- Variables más importantes
- Recomendación: ¿Es este modelo útil para el negocio?
- Limitaciones
```

---

## 7. Optimización

### Plantilla 7.1: Optimización de Portfolio

```
CONTEXTO:
Tengo un DataFrame 'df' con [ITEMS], [RETORNO_ESPERADO], [RIESGO], [CORRELACIONES].

PREGUNTA DE NEGOCIO:
¿Qué combinación de [ITEMS] maximiza retorno para un nivel de riesgo dado?

TAREA:
1. Define restricciones (ej: sum de pesos = 100%)
2. Define objetivo (maximizar retorno para nivel de riesgo)
3. Simula múltiples escenarios de combinaciones
4. Identifica frontera eficiente
5. Recomienda portfolio óptimo

FORMATO DE RESPUESTA:
- Portfolio óptimo: [composición]
- Retorno esperado: X%
- Riesgo: Y
- Ratio retorno/riesgo: Z
- Comparación vs portfolio actual (si existe)
```

### Plantilla 7.2: Optimización de Precios

```
CONTEXTO:
Tengo datos de [PRODUCTO] con [PRECIO], [VOLUMEN_VENTAS], [COSTO].

PREGUNTA DE NEGOCIO:
¿Qué precio maximiza el margen total considerando elasticidad de demanda?

TAREA:
1. Calcula margen actual: (precio - costo) × volumen
2. Estima elasticidad: cómo cambia volumen con precio
3. Simula escenarios de precios (±10%, ±20%)
4. Para cada escenario, calcula nuevo volumen y margen
5. Identifica precio óptimo

FORMATO DE RESPUESTA:
- Precio actual: $X, margen total: $Y
- Precio óptimo: $X', margen total: $Y'
- Incremento: +$Z (±%)
- Elasticidad estimada: cada 1% de cambio en precio cambia volumen en Z%
- Recomendación con riesgos
```

---

## 8. Promots de Verificación

### Plantilla 8.1: Verificación de Cálculos

```
Verifica tu respuesta anterior:

1. Muestra el código que usaste
2. Muestra las primeras 10 filas de datos que usaste
3. Calcula el mismo resultado de otra forma diferente
4. ¿Ambos métodos dan el mismo resultado?
5. Si no coinciden, explica por qué

Si hay error, corrige tu respuesta.
```

### Plantilla 8.2: Validación de Suposiciones

```
Sobre tu análisis anterior:

1. ¿Qué suposiciones hiciste?
2. ¿Son esas suposiciones válidas según los datos?
3. ¿Qué pasaría si las suposiciones son incorrectas?
4. ¿Cómo podemos verificar las suposiciones en el mundo real?
5. ¿Qué tan confiantes deberíamos estar en los resultados?

Se específico sobre limitaciones y riesgos.
```

---

## 9. Prompts de Corrección

### Plantilla 9.1: Corrección de Errores

```
Tu respuesta tiene un error:

[EXPLICACIÓN DEL ERROR]

Por favor:
1. Reconoce el error
2. Explica por qué ocurrió
3. Propón la solución correcta
4. Muestra el código corregido
5. Verifica que la corrección funciona
```

### Plantilla 9.2: Refinamiento de Respuesta

```
Tu respuesta es técnicamente correcta pero necesito más detalle:

Sobre [TEMA específico]:
1. Explica con más detalle
2. Da ejemplos concretos
3. Muestra el código paso a paso
4. Interpreta los resultados en contexto de negocio
5. Haz recomendaciones específicas
```

---

## 10. Promots Multi-Turn

### Plantilla 10.1: Análisis Iterativo

```
TURNO 1:
[PLANTILLA INICIAL DE ANÁLISIS]

TURNO 2 (basado en respuesta):
Basado en tus resultados, ahora quiero profundizar en [INSIGHT específico]:
- Muéstrame los datos detrás de ese insight
- ¿Hay otros factores que expliquen este patrón?
- ¿Qué recommendación específica darías?

TURNO 3 (basado en respuesta):
Ahora quiero validar las recommendaciones:
- ¿Cómo verificaríamos que esto funciona?
- ¿Qué métricas monitorearíamos?
- ¿En qué timeframe esperaríamos ver resultados?
```

---

## Quick Reference: Comandos Útiles

### Para Pandas

```
# Exploración
df.head()
df.info()
df.describe()
df.shape
df.columns

# Limpieza
df.dropna()
df.drop_duplicates()
df.fillna(value)
df.astype(tipo)

# Filtrado
df[df['columna'] > valor]
df[df['columna'].isin([val1, val2])]
df[df['columna'].str.contains('texto')]

# Agrupación
df.groupby('columna').agg({'otra': 'mean'})
df.pivot_table(index='x', columns='y', values='z')

# Ordenamiento
df.sort_values('columna', ascending=False)
df.nlargest(n, 'columna')
df.nsmallest(n, 'columna')
```

### Para Visualización

```
# Imports necesarios
import matplotlib.pyplot as plt
import seaborn as sns

# Configuración
plt.figure(figsize=(12, 6))
sns.set_style("whitegrid")

# Guardar gráfico
plt.savefig('grafico.png', dpi=300, bbox_inches='tight')
plt.show()
```

---

## Tips para Prompts Efectivos

1. **Sé específico con nombres de columnas**
   - ✅ "df['precio_promedio']"
   - ❌ "la columna de precios"

2. **Muestra ejemplos de datos**
   - ✅ "Las fechas están en formato 'YYYY-MM-DD'"
   - ❌ "tengo fechas"

3. **Define el formato de salida deseado**
   - ✅ "Quiero una tabla con columnas X, Y, Z"
   - ❌ "organízalo bien"

4. **Pide verificación**
   - ✅ "Muestra el código y valida que funciona"
   - ❌ "hazlo"

5. **Incluye contexto de negocio**
   - ✅ "Esto es para decidir qué productos discontinuar"
   - ❌ "analiza productos"

---

## Ejemplo Completo: Análisis de Ventas

```
CONTEXTO:
Soy analista de una PyME de retail en Colombia.
Tengo un DataFrame 'df' con datos de ventas de los últimos 12 meses.

Columnas principales:
- producto: nombre del producto (50 productos únicos)
- categoria: categoría (Lácteos, Bebidas, Snacks, Limpieza, Cereal)
- fecha: fecha de venta (formato YYYY-MM-DD)
- cantidad: unidades vendidas
- precio_unitario: precio por unidad
- costo_unitario: costo por unidad
- descuento_pct: porcentaje de descuento aplicado (0-25%)

PREGUNTA DE NEGOCIO:
¿Qué productos tienen menor rentabilidad y deberíamos considerar
aumentar precio o discontinuar?

TAREA:
1. Calcula margen % por producto: (precio - costo) / precio × 100
2. Calcula revenue total por producto
3. Identifica los 10 productos con menor margen
4. Para esos 10 productos, muestra:
   - Margen %
   - Revenue total
   - Categoría
   - Volumen vendido
5. Clasifica en 3 grupos:
   - discontinuar (bajo margen + bajo volumen)
   - aumentar precio (bajo margen + alto volumen)
   - mantener (bajo margen pero producto estratégico)
6. Crea gráfico de barras horizontales con margen % de todos los productos,
   resaltando en rojo los 10 con menor margen
7. Calcula: si aumentamos precio 10% en productos a aumentar precio,
   ¿qué impacto tendría en revenue y margen?

FORMATO DE RESPUESTA:
- Código Python completo con pandas y matplotlib
- Tabla con los 10 productos analizados
- Gráfico de barras solicitado
- Análisis de sensibilidad de aumento de precio
- Recomendaciones específicas con impacto financiero cuantificado
```

---

## Recursos Adicionales

- **Business-Question-Canvas.md** - Cómo estructurar buenas preguntas
- **AI-Reliability-Checklist.md** - Cómo verificar respuestas de IA
- **Data-Ethics-Guide.md** - Qué datos compartir con IA

---

## Conclusión

Estas plantillas son un **punto de partida**, no reglas rígidas. Adáptalas a tu contexto específico, tus datos, y tus necesidades de negocio.

**Mejor práctica:** Combina múltiples plantillas para análisis complejos.

**Ejemplo:** Plantilla 3.3 (Análisis Temporal) + Plantilla 4.2 (Gráfico de Barras) + Plantilla 5.1 (Pareto)

---

*Universidad UdeColombia - Especialización en Analítica de Datos - 2026*
