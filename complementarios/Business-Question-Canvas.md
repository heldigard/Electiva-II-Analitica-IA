# Business Question Canvas
## Plantilla para Estructurar Preguntas de Negocio

### Universidad UdeColombia - Electiva Analítica de Datos con IA

---

## ¿Qué es el Business Question Canvas?

El **Business Question Canvas** es una plantilla simple pero poderosa para transformar preguntas vagas en **preguntas de negocio estructuradas** que pueden responderse con datos.

Antes de pedirle a la IA que analice datos, usa este canvas para **aclarar qué quieres saber** y **por qué es importante**.

---

## Estructura del Canvas

Copia y pega esta plantilla cada vez que vayas a hacer un análisis:

```markdown
# Business Question Canvas

## 1. PROBLEMA DE NEGOCIO
[¿Qué problema estás tratando de resolver?]
- Ejemplo: "Las ventas han caído 15% en el último trimestre"
- Ejemplo: "No sabemos qué productos son más rentables"
- Ejemplo: "El equipo de soporte está sobrecargado"

## 2. PREGUNTA ESPECÍFICA
[¿Qué quieres saber exactamente?]
- ❌ Mala: "Analizar las ventas"
- ✅ Buena: "¿Qué categoría de productos tiene la menor rentabilidad y por qué?"
- ❌ Mala: "Mirar los tickets de soporte"
- ✅ Buena: "¿Qué tipo de tickets de soporte toman más tiempo resolver y cómo podemos reducirlo?"

## 3. MÉTRICA CLAVE
[¿Qué número medirá el éxito?]
- Revenue total, margen %, satisfacción cliente, tiempo de resolución
- Debe ser cuantificable y específico

## 4. AUDIENCIA
[¿Quién tomará decisiones basado en este análisis?]
- Dueño de PyME, Director de Producto, Gerente de Soporte
- Esto define el nivel de detalle y el lenguaje a usar

## 5. DECISIÓN A TOMAR
[¿Qué acción se tomará con base en la respuesta?]
- Ajustar precios, eliminar productos, capacitar equipo, cambiar procesos
- Si no hay acción posible, reformular la pregunta

## 6. RIESGO
[¿Qué pasa si tomamos la decisión incorrecta?]
- Pérdida de revenue, insatisfacción de clientes, desperdicio de recursos
- Esto define cuánta confianza necesitamos en los datos
```

---

## Ejemplos Completos

### Ejemplo 1: Retail - Análisis de Rentabilidad

```markdown
# Business Question Canvas

## 1. PROBLEMA DE NEGOCIO
Los márgenes de ganancia han caído del 18% al 12% en el último año.
No sabemos qué productos son más rentables y cuáles están perdiendo dinero.

## 2. PREGUNTA ESPECÍFICA
¿Cuáles son los 5 productos con menor margen de ganancia
y deberíamos considerar discontinuar o aumentar su precio?

## 3. MÉTRICA CLAVE
Margen de ganancia % = (Precio - Costo) / Precio × 100

## 4. AUDIENCIA
Dueño de la PyME y Director Comercial

## 5. DECISIÓN A TOMAR
- Discontinuar productos con margen < 10% y bajo volumen
- Aumentar precio 10-15% en productos con margen 10-13%
- Mantener portfolio enfocado en productos más rentables

## 6. RIESGO
Si eliminamos los productos equivocados:
- Podemos perder clientes que compran esos productos
- Podemos reducir revenue total sin mejorar margen
- Podemos dañar la percepción de variedad del negocio
```

**Prompt resultante para la IA:**
```
Tengo un DataFrame de ventas de una PyME con columnas:
- producto: nombre del producto
- categoria: categoría de producto
- precio_base: precio normal de venta
- costo: costo unitario del producto
- revenue_total: venta total del producto
- margen_pct: porcentaje de margen calculado

PREGUNTA DE NEGOCIO:
¿Cuáles son los 5 productos con menor margen de ganancia
y deberíamos considerar discontinuar o aumentar su precio?

TAREA:
1. Calcular margen % por producto
2. Identificar los 5 productos con menor margen
3. Para cada producto, mostrar:
   - Margen %
   - Revenue total
   - Categoría
4. Recomendar: discontinuar (si bajo revenue) o subir precio (si revenue alto)

Crea un gráfico de barras horizontales mostrando el margen % de todos los productos,
ordenados de menor a mayor, resaltando en rojo los 5 con menor margen.
```

---

### Ejemplo 2: Soporte al Cliente - Optimización de Tiempos

```markdown
# Business Question Canvas

## 1. PROBLEMA DE NEGOCIO
El tiempo de resolución de tickets aumentó de 24h a 72h en promedio.
El equipo de soporte está sobrecargado y la satisfacción de clientes cayó.

## 2. PREGUNTA ESPECÍFICA
¿Qué categorías de tickets de soporte toman más tiempo resolver
y qué podemos hacer para reducir esos tiempos?

## 3. MÉTRICA CLAVE
Tiempo promedio de resolución (en horas) por categoría de ticket

## 4. AUDIENCIA
Director de Soporte y Director de Producto

## 5. DECISIÓN A TOMAR
- Crear documentación para categorías con tiempos altos
- Capacitar al equipo en categorías complejas
- Priorizar mejoras de producto que generan muchos tickets lentos

## 6. RIESGO
Si no reducimos los tiempos:
- Pérdida de clientes por mala experiencia
- Aumento de costo de soporte (necesitamos más personas)
- Daño a reputación de la empresa
```

**Prompt resultante para la IA:**
```
Tengo un DataFrame de tickets de soporte con columnas:
- ticket_id: identificador único
- categoria: tipo de problema (ej: "Login/Authentication", "Performance", "Billing")
- tiempo_resolucion_horas: tiempo que tomó resolver el ticket
- estado: "Open", "In Progress", "Resolved", "Closed"
- satisfaccion: calificación 1-5 del cliente

PREGUNTA DE NEGOCIO:
¿Qué categorías de tickets de soporte toman más tiempo resolver
y qué podemos hacer para reducir esos tiempos?

TAREA:
1. Filtrar solo tickets resueltos (estado = "Resolved" o "Closed")
2. Calcular tiempo promedio de resolución por categoría
3. Identificar las 3 categorías con mayor tiempo promedio
4. Para esas categorías, también mostrar:
   - Número de tickets (volumen)
   - Satisfacción promedio de clientes
5. Crear un boxplot mostrando la distribución de tiempos de resolución por categoría

Tu respuesta debe incluir interpretación de negocio:
¿Por qué esas categorías toman más tiempo?
¿Qué acción recomiendas para reducir los tiempos?
```

---

## Cómo Usar el Canvas

### Paso 1: Llenar el Canvas (5 minutos)

Antes de abrir el notebook o el chatbot, llena el canvas a mano o en un documento. Esto te obliga a pensar.

### Paso 2: Traducir a Prompt (2 minutos)

Convierte el canvas lleno en un prompt para la IA siguiendo este formato:

```
CONTEXTO:
[Descripción breve del problema y dataset]

PREGUNTA DE NEGOCIO:
[Pregunta específica del canvas]

TAREA:
[Qué quieres que la IA haga exactamente]
- Análisis 1
- Análisis 2
- Análisis 3

FORMATO DE RESPUESTA:
[Qué esperas recibir - tablas, gráficos, recomendaciones]
```

### Paso 3: Validar con Stakeholders (opcional)

Si el análisis es para otra persona, muéstrale el canvas antes de empezar:
- "¿Es esta la pregunta correcta?"
- "¿Qué decisión tomarán con esta información?"
- "¿Hay algo más que debamos considerar?"

---

## Errores Comunes y Cómo Evitarlos

### Error 1: Preguntas Demasiado Generales

❌ **Mal:** "Analiza mis datos de ventas"
✅ **Bien:** "¿Qué productos tienen menor margen y deberíamos discontinuar?"

**Por qué:** Preguntas generales llevan a análisis genéricos que no ayudan a tomar decisiones.

### Error 2: No Pensar en la Decisión

❌ **Mal:** "¿Cuántos tickets de soporte tenemos por categoría?"
✅ **Bien:** "¿Qué categorías de tickets toman más tiempo y cómo podemos reducirlo?"

**Por qué:** Si no hay una acción asociada, el análisis es solo curiosidad, no analítica de negocio.

### Error 3: Olvidar el Riesgo

❌ **Mal:** No considerar qué pasa si la decisión es equivocada
✅ **Bien:** Si subimos precio y perdemos 20% de volumen, ¿aún es conveniente?

**Por qué:** Entender el riesgo define qué tan confiables deben ser los datos y el análisis.

### Error 4: Preguntas Sin Respuesta Posible

❌ **Mal:** "¿Por qué las ventas cayeron?" (Los datos no muestran "por qué")
✅ **Bien:** "¿Qué productos y períodos mostraron mayor caída en ventas?"

**Por qué:** Los datos muestran patrones, no causas. Las causas las infieres tú con contexto de negocio.

---

## Quick Reference: Buenas Preguntas vs Malas Preguntas

| Contexto | ❌ Pregunta Mala | ✅ Pregunta Buena |
|----------|-----------------|------------------|
| **Ventas** | "¿Cuáles son las ventas?" | "¿Qué productos tienen menor rentabilidad y por qué?" |
| **Soporte** | "¿Cómo están los tickets?" | "¿Qué categorías de tickets toman más tiempo resolver?" |
| **Marketing** | "¿Qué canales funcionan?" | "¿Qué canal tiene mayor ROI y deberíamos invertir más?" |
| **Productos** | "¿Qué productos venden más?" | "¿Qué productos generan más revenue Y margen?" |
| **Clientes** | "¿Quiénes son nuestros clientes?" | "¿Qué segmento de clientes tiene mayor LTV?" |

---

## Plantilla Rápida (Copy-Paste)

```
PROBLEMA: _______________________________________________________

PREGUNTA: _______________________________________________________

MÉTRICA: ________________________________________________________

AUDIENCIA: ______________________________________________________

DECISIÓN: _______________________________________________________

RIESGO: _________________________________________________________
```

---

## Ejercicio Práctico

**Instrucciones:**
1. Piensa en un problema de negocio real de tu trabajo o empresa
2. Llena el Business Question Canvas completo
3. Tradúcelo a un prompt para ChatGPT/Claude/Qwen
4. Comparte tu resultado con un compañero y validen si la pregunta es clara

**Tiempo estimado:** 10 minutos

---

## Recursos Adicionales

- **Prompt-Playbook.md** - Más ejemplos de prompts efectivos
- **AI-Reliability-Checklist.md** - Cómo validar que la IA te dio respuestas correctas
- **Clase1-Fundamentos-Prompting.ipynb** - Ejercicios prácticos de prompting

---

## Puntos Clave

1. **Antes de analizar, define:** ¿Qué quieres saber y por qué?
2. **Una pregunta específica** > 10 análisis genéricos
3. **Si no hay decisión asociada**, reformula la pregunta
4. **El riesgo define la confianza** que necesitas en los datos
5. **El canvas es tu primer paso** antes de abrir cualquier chatbot

---

**Recuerda:** 5 minutos de planificación con el canvas ahorran 30 minutos de análisis innecesario.

*Universidad UdeColombia - Especialización en Analítica de Datos - 2026*
