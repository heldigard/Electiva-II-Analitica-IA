# Prompt Playbook - Electiva Analítica de Datos con IA

## Plantillas Reutilizables para Trabajar con Chatbots de IA

> **💡 Tip:** Usa estas plantillas en TODAS las clases. Copia, pega, y adapta los placeholders entre corchetes `[...]`.

---

## Plantilla 1: Limpieza de Datos

Usa esta plantilla cuando necesites limpiar o preparar un dataset.

```
Tengo un DataFrame con [número] filas y estas columnas:
[lista las columnas principales: columna1, columna2, columna3...]

Necesito que me ayudes a:
1. Identificar valores faltantes (NaN)
2. [acción específica: eliminar filas con NaN / imputar con la media / llenar con "Desconocido"]
3. Verificar si hay filas duplicadas
4. [transformación específica: convertir texto a fecha / separar columnas anidadas / crear nueva columna]

Contexto del negocio: [breve descripción de qué representan los datos]
```

**Ejemplo de uso:**
```
Tengo un DataFrame con 8,800 filas y estas columnas:
- title: nombre de la película
- type: 'Movie' o 'TV Show'
- release_year: año de lanzamiento
- country: país de origen
- rating: clasificación

Necesito que me ayudes a:
1. Identificar valores faltantes (NaN)
2. Eliminar filas donde 'title' o 'type' sean NaN
3. Verificar si hay filas duplicadas
4. Crear una nueva columna 'decade' a partir de 'release_year'

Contexto del negocio: Catálogo de Netflix para análisis de contenido
```

---

## Plantilla 2: Análisis Exploratorio (EDA)

Usa esta plantilla cuando quieras explorar y entender una columna específica.

```
Quiero analizar la columna [nombre_columna] para entender [pregunta de negocio].

Por favor:
1. Muestra estadísticas descriptivas (para numéricas) o valores únicos (para texto)
2. Crea un [tipo de gráfico: histograma / gráfico de barras / boxplot] que muestre [relación o distribución]
3. Identifica los [número] valores más [alto/bajo/frecuente]
4. Resume en 1 frase qué significa esto para el negocio

Dataset: [breve descripción del dataset]
```

**Ejemplo de uso:**
```
Quiero analizar la columna 'rating' para entender qué tipo de contenido domina en Netflix.

Por favor:
1. Muestra los valores únicos de 'rating' y su frecuencia
2. Crea un gráfico de barras que muestre el conteo de cada clasificación
3. Identifica las 5 clasificaciones más frecuentes
4. Resume en 1 frase qué significa esto para la estrategia de contenido

Dataset: Catálogo de Netflix con 8,800 títulos
```

---

## Plantilla 3: Selección de Gráficos

Usa esta plantilla cuando no sepas qué tipo de gráfico usar para visualizar tus datos.

```
Quiero visualizar la relación entre [variable X] y [variable Y].

Contexto: [pregunta de negocio que quiero responder]
Datos: [describe brevemente qué tipo de datos son]

¿Qué tipo de gráfico recomiendas y por qué?

Luego, genera el código Python para crearlo con:
- Título descriptivo
- Ejes etiquetados con nombres claros
- Colores apropiados para el contexto
- Tamaño de figura adecuado (ej: figsize=(10, 6))
```

**Ejemplo de uso:**
```
Quiero visualizar la relación entre 'release_year' y la cantidad de títulos.

Contexto: Quiero ver si Netflix está agregando más contenido en años recientes
Datos: Año de lanzamiento (numérico) vs conteo de títulos

¿Qué tipo de gráfico recomiendas y por qué?

Luego, genera el código Python para crearlo con:
- Título descriptivo
- Ejes etiquetados con nombres claros
- Colores apropiados para el contexto
- Tamaño de figura adecuado (ej: figsize=(10, 6))
```

---

## Plantilla 4: Insights de Negocio

Usa esta plantilla cuando necesites comunicar hallazgos a una audiencia de negocio.

```
A partir de este análisis, necesito comunicar hallazgos a [rol: gerente/director/equipo].

Resultados del análisis:
[describe brevemente los resultados principales con números clave]

Genera:
1. Un resumen ejecutivo de 3 frases que responda: ¿Qué encontramos? ¿Por qué importa? ¿Qué debemos hacer?
2. 3 hallazgos clave, cada uno con:
   - Un título descriptivo
   - Evidencia numérica específica
   - Interpretación de qué significa para el negocio
3. 2 recomendaciones accionables con:
   - Acción específica (qué hacer)
   - Responsable (quién debe hacerlo)
   - Timeline (cuándo)
4. 1 métrica de seguimiento para medir el impacto

Estilo: [profesional / directo / técnico]
Largo: [máximo 1 página / formato presentación / memo ejecutivo]
```

**Ejemplo de uso:**
```
A partir de este análisis, necesito comunicar hallazgos al Director de Contenido.

Resultados del análisis:
- 69% del catálogo son películas, 31% son series
- Clasificación más común: TV-MA (para adultos)
- Año peak: 2019 con más agregaciones
- Géneros top: Dramas Internacionales, Comedias, Documentales

Genera:
1. Un resumen ejecutivo de 3 frases
2. 3 hallazgos clave con evidencia numérica
3. 2 recomendaciones accionables
4. 1 métrica de seguimiento

Estilo: profesional y directo
Largo: memo ejecutivo de 1 página
```

---

## 🆕 Plantilla 5: Business Question Canvas (2026)

Usa esta plantilla ANTES de empezar cualquier análisis para estructurar tu pregunta de negocio.

```
CONTEXTO:
[Describe la situación y los datos]

PROBLEMA DE NEGOCIO:
[Qué problema estás tratando de resolver]

PREGUNTA ESPECÍFICA:
[Qué quieres saber exactamente - sé específico]

MÉTRICA CLAVE:
[Qué número medirá el éxito]

AUDIENCIA:
[Quién tomará decisiones basado en este análisis]

DECISIÓN A TOMAR:
[Qué acción se tomará con base en la respuesta]

RIESGO:
[Qué pasa si tomamos la decisión incorrecta]

TAREA:
Genera código Python para [tarea específica relacionada con la pregunta]

FORMATO DE RESPUESTA:
[Cómo quieres recibir la respuesta - tabla, gráfico, recomendación]
```

**Ejemplo de uso:**
```
CONTEXTO:
Soy analista en una PyME de retail. Tengo un DataFrame 'df' con columnas:
- producto: nombre del producto
- categoria: categoría (Lácteos, Bebidas, Snacks, Limpieza, Cereal)
- precio_base: precio normal
- costo: costo unitario
- revenue: venta total
- margen: ganancia

PROBLEMA DE NEGOCIO:
Los márgenes de ganancia han caído del 18% al 12% en el último año.
No sabemos qué productos son más rentables.

PREGUNTA ESPECÍFICA:
¿Cuáles son los 5 productos con menor margen y deberíamos considerar discontinuar o aumentar precio?

MÉTRICA CLAVE:
Margen % = (Precio - Costo) / Precio × 100

AUDIENCIA:
Dueño de la PyME y Director Comercial

DECISIÓN A TOMAR:
Discontinuar productos con margen < 10% y bajo volumen
Aumentar precio 10-15% en productos con margen 10-13%

RIESGO:
Si eliminamos los productos equivocados, podemos perder clientes y reducir revenue total

TAREA:
1. Calcula margen % por producto
2. Identifica los 5 productos con menor margen
3. Clasifica en: discontinuar (bajo margen + bajo volumen) o aumentar precio (bajo margen + alto volumen)

FORMATO DE RESPUESTA:
Tabla con productos, margenes, revenue total y recomendación
```

---

## 🆕 Plantilla 6: Verificación de IA (2026)

Usa esta plantilla DESPUÉS de que la IA genere análisis importantes. La IA puede alucinar.

```
Verifica tu respuesta anterior:

1. Muestra el código que usaste
2. Muestra las primeras 10 filas de los datos que usaste
3. Calcula el resultado de otra forma diferente
4. ¿Ambos métodos dan el mismo resultado?
5. Si hay error, corrige tu respuesta

Si encuentras un error:
- Reconoce el error
- Explica por qué ocurrió
- Propón la solución correcta
- Muestra el código corregido
```

**Ejemplo de uso (cuando sospechas error):**
```
Verifica tu respuesta anterior:

Dijiste que el margen promedio es 22%, pero cuando yo calculo manualmente:
- Producto A: ($100 - $80) / $100 = 20%
- Producto B: ($150 - $120) / $150 = 20%
- Producto C: ($200 - $150) / $200 = 25%

Promedio manual = (20% + 20% + 25%) / 3 = 21.67%

Por favor:
1. Muestra el código que usaste para calcular 22%
2. Explica la discrepancia con mi cálculo manual
3. Corrije si hay error
```

---

## 🆕 Plantilla 7: Executive Decision Memo (2026)

Usa esta plantilla cuando presentes resultados a gerencia que TOMARÁN DECISIONES.

```
## EXECUTIVE DECISION MEMO

**To:** [Rol - Gerente/Director/CEO]
**From:** [Tu nombre - Analista de Datos]
**Date:** [Fecha]
**Subject:** [Título del análisis y decisión a tomar]

---

### 🎯 OBJECTIVE
[1-2 oraciones sobre el problema que se resuelve y por qué es importante ahora]

### 🔑 KEY INSIGHT
[El hallazgo más impactante del análisis con evidencia numérica]
- Dato específico que respalda el insight
- Contexto: ¿Cómo se compara con benchmarks o expectativas?

### 💡 RECOMMENDATION
[Acción específica que recomiendas]
- **Qué hacer exactamente:** [Descripción clara]
- **Por qué:** [Conexión con el key insight]
- **Impacto esperado:** [Resultado cuantificado si es posible]
- **Timeline:** [Cuándo implementar]

### ⚠️ RISKS
[Qué podría salir mal]
- **Riesgo 1:** [Descripción] - Probabilidad: [Alta/Media/Baja] - Mitigación: [Qué hacer]
- **Riesgo 2:** [Descripción] - Probabilidad: [Alta/Media/Baja] - Mitigación: [Qué hacer]

### 📈 KPIS TO MONITOR
[Cómo medir el éxito]
- **KPI 1:** [Nombre] - Baseline: [Valor actual] - Target: [Valor objetivo]
- **KPI 2:** [Nombre] - Baseline: [Valor actual] - Target: [Valor objetivo]

### ✅ NEXT STEPS
[Acciones inmediatas - próximos 30-60 días]
1. [Acción 1] - Responsable: [Nombre] - Fecha límite: [Fecha]
2. [Acción 2] - Responsable: [Nombre] - Fecha límite: [Fecha]
```

**Ejemplo de uso:**
```
## EXECUTIVE DECISION MEMO

**To:** Directora de E-commerce
**From:** Analista de Datos
**Date:** Enero 2026
**Subject:** Expansión de Categoría de Joyería

---

### 🎯 OBJECTIVE
Optimizar la composición del catálogo para maximizar el margen de ganancia.

### 🔑 KEY INSIGHT
La categoría de Joyería representa solo el 10% del catálogo pero tiene el precio promedio más alto ($150 vs $80 promedio general), sugiriendo un margen superior no explotado.

### 💡 RECOMMENDATION
Expandir Joyería de 2 a 8 productos en 6 meses.
- **Qué:** Añadir 6 productos nuevos (2 medios, 2 premium, 2 accesorios)
- **Por qué:** Mayor ticket promedio = mayor revenue por transacción
- **Impacto:** Revenue adicional estimado de $5,000/mes (+25%)
- **Timeline:** Enero - Junio 2026

### ⚠️ RISKS
- **Baja demanda:** Media probabilidad, Alto impacto → Test con 4 productos inicial, monitorear conversión semanal
- **Inventario obsoleto:** Baja probabilidad, Medio impacto → Consignación con proveedores

### 📈 KPIS TO MONITOR
- Revenue joyería: $2,000 → $8,000/mes (+300%)
- Margen promedio: 12% → 15% (mantener o mejorar)
- Conversión: Monitorear semanal, objetivo >5%

### ✅ NEXT STEPS
1. Validar proveedores (Semana 1) - Gerente de Producto
2. Definir presupuesto (Semana 2) - Director Financiero
3. Piloto con 4 productos (Mes 1-2) - Equipo E-commerce
```

---

## Tips Adicionales

### ✅ Cuándo Usar Cada Plantilla

| Situación | Usa esta plantilla |
|-----------|-------------------|
| Datos nuevos con posibles problemas | Plantilla 1: Limpieza de Datos |
| Quieres entender una variable específica | Plantilla 2: Análisis Exploratorio |
| No sabes qué gráfico usar | Plantilla 3: Selección de Gráficos |
| Necesitas reportar resultados | Plantilla 4: Insights de Negocio |
| **🆕 Antes de empezar análisis** | **Plantilla 5: Business Question Canvas (2026)** |
| **🆕 Después de recibir respuesta de IA** | **Plantilla 6: Verificación (2026)** |
| **🆕 Presentando a tomadores de decisiones** | **Plantilla 7: Executive Decision Memo (2026)** |

### 🔄 Iteración de Prompts

Si el chatbot no te da lo que necesitas:
1. **Sé más específico** con los datos de ejemplo
2. **Agrega restricciones** (ej: "sin usar loops", "con pandas", "max 10 líneas")
3. **Muestra el formato esperado** de salida
4. **Pide el código por partes** si la respuesta es muy larga

### ⚠️ Errores Comunes

| Error | Solución |
|-------|----------|
| Código con errores de sintaxis | Pide al chatbot "revisa el código y corrige errores" |
| Gráfico no se ve bien | Agrega "ajusta el tamaño de figura a (12,6)" |
| Resultados no tienen sentido | Pregunta "¿tiene sentido este resultado? explícame" |
| Código muy lento | Pide "versión optimizada del código" |

---

## Recuerda

> **Tu valor como analista no es escribir código Python.**
> **Tu valor es SABER QUÉ PEDIR y CÓMO INTERPRETAR.**

Estas plantillas te ayudan a estructurar tus pensamientos y obtener mejores resultados de la IA.

---

**Versión:** 2.0 (Actualizado 2026)
**Fecha:** Enero 2026
**Curso:** Electiva Analítica de Datos con IA

**Novedades v2.0:**
- 🆕 Plantilla 5: Business Question Canvas (para estructurar preguntas antes de analizar)
- 🆕 Plantilla 6: Verificación de IA (para detectar alucinaciones y errores)
- 🆕 Plantilla 7: Executive Decision Memo (para presentar a tomadores de decisiones)
