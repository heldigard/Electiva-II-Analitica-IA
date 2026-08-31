# 🏆 Taller Final: Análisis de Catálogo E-Commerce

## Universidad UdeColombia - Especialización en Analítica de Datos
### Electiva II: Analítica de Datos con Inteligencia Artificial

---

## 📋 Información General

| Aspecto | Detalle |
|---------|---------|
| **Duración** | Se inicia en clase y se termina en casa hasta la fecha definida en el curso |
| **Modalidad** | Individual/Grupal (máx. 3 personas) |
| **Entregable** | Notebook de Google Colab (.ipynb) |
| **Uso** | Práctica integradora de preparación para el Quiz 3, sin ponderación independiente |

---

## 🎯 Objetivo

Demostrar tu capacidad para realizar un análisis exploratorio completo de datos, utilizando herramientas de IA como copiloto para la generación de código, y comunicando tus hallazgos de manera profesional.

---

## 📊 Contexto de Negocio

> **Escenario:** Eres el nuevo analista de datos de **"Fake Store"**, una tienda online en crecimiento.
>
> La gerencia necesita entender el catálogo de productos para tomar decisiones estratégicas sobre:
> - 💰 Estrategia de precios
> - 📦 Gestión de inventario  
> - 📣 Marketing y promociones
>
> Tu misión es realizar un **análisis exploratorio completo** y presentar tus hallazgos con **recomendaciones accionables**.

---

## 🔌 Fuente de Datos

**API:** Fake Store API  
**Endpoint:** `https://fakestoreapi.com/products`  
**Documentación:** [fakestoreapi.com/docs](https://fakestoreapi.com/docs)

### Estructura de los Datos

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | int | Identificador único del producto |
| `title` | string | Nombre del producto |
| `price` | float | Precio en dólares |
| `description` | string | Descripción detallada |
| `category` | string | Categoría del producto |
| `image` | string | URL de la imagen |
| `rating` | object | `{rate: float, count: int}` - Calificación y número de votos |

---

## ✅ Rúbrica de Evaluación

> **Nota:** Esta rúbrica unificada aplica a todas las opciones de Taller Final (A, B, C y D).

### Criterio 1: Conexión a API y Extracción de Datos (15%)

> **Nota:** La conexión a APIs es un **medio**, no el **fin**. Lo valioso es lo que haces con los datos.

| Nivel | Descripción | Puntos |
|-------|-------------|--------|
| **Excelente** | Conexión exitosa, verifica status code, maneja errores | 15 |
| **Bueno** | Conexión exitosa, verifica status code | 12 |
| **Básico** | Conexión exitosa sin verificación | 9 |
| **Insuficiente** | No logra conectar o extraer datos | 0-6 |

**Qué debe incluir:**
```python
# Ejemplo de código esperado
import requests
import pandas as pd

url = "https://fakestoreapi.com/products"
response = requests.get(url)

if response.status_code == 200:
    print("✅ Conexión exitosa")
    datos = response.json()
else:
    print(f"❌ Error: {response.status_code}")
```

---

### Criterio 2: Limpieza y Transformación de Datos (20%)

| Nivel | Descripción | Puntos |
|-------|-------------|--------|
| **Excelente** | Transforma datos anidados, tipos correctos, sin errores | 20 |
| **Bueno** | Transforma datos anidados correctamente | 16 |
| **Básico** | Intenta transformar pero con errores menores | 12 |
| **Insuficiente** | No transforma la columna rating o tiene errores graves | 0-8 |

**El reto clave:** La columna `rating` viene anidada como:
```json
"rating": {"rate": 4.1, "count": 120}
```

Debe separarse en:
- `rating_rate` (la calificación)
- `rating_count` (número de votos)

---

### Criterio 3: Análisis y Visualización (20%)

> **Nota:** Las visualizaciones son herramientas para comunicar, no el objetivo principal. Lo valioso es el **insight** que comunican, no el gráfico en sí.

| Nivel | Descripción | Puntos |
|-------|-------------|--------|
| **Excelente** | 4+ visualizaciones relevantes, bien formateadas, con interpretación | 20 |
| **Bueno** | 3-4 visualizaciones correctas con interpretación | 16 |
| **Básico** | 2-3 visualizaciones sin interpretación completa | 12 |
| **Insuficiente** | Menos de 2 visualizaciones o sin sentido | 0-8 |

**Visualizaciones requeridas (mínimo 4):**

1. **Histograma** - Distribución de precios
2. **Gráfico de barras** - Productos por categoría
3. **Gráfico de barras** - Precio promedio por categoría
4. **Scatter plot** - Relación precio vs calificación

**Bonus (hasta +10 puntos, sin superar 100):** Crear una métrica `score = rating_rate × rating_count` y visualizar los productos estrella.

---

### Criterio 4: Respuestas a Preguntas de Negocio (25%)

| Nivel | Descripción | Puntos |
|-------|-------------|--------|
| **Excelente** | Responde todas las preguntas con insights claros y accionables | 25 |
| **Bueno** | Responde todas las preguntas con interpretación básica | 20 |
| **Básico** | Responde algunas preguntas sin profundidad | 15 |
| **Insuficiente** | No responde o respuestas incorrectas | 0-10 |

**Preguntas que debes responder:**

#### Pregunta 1: Análisis de Precios
- ¿Cuál es la distribución de precios?
- ¿Cuáles son los 5 productos más caros y más baratos?
- **Insight:** ¿Qué tipo de tienda es? ¿Económica o premium?

#### Pregunta 2: Análisis de Categorías
- ¿Cuántos productos hay por categoría?
- ¿Cuál es el precio promedio por categoría?
- **Insight:** ¿Qué categoría es más rentable?

#### Pregunta 3: Análisis de Calidad
- ¿Cómo se distribuyen las calificaciones?
- ¿Existe relación entre precio y calificación?
- **Insight:** ¿Pagar más garantiza mejor producto?

#### Pregunta 4: Productos Estrella
- ¿Cuáles son los 5 productos más populares (mayor `rating_count`)?
- ¿Cuáles son los 5 productos con mejor `score`?
- **Insight:** ¿Son los mismos? ¿Qué significa?

---

### Criterio 5: Resumen Ejecutivo (20%)

> **Nota importante:** Este criterio ahora es OBLIGATORIO (antes era bonus). El resumen ejecutivo es donde **demuestras el valor real de tu análisis**. Es tu oportunidad de persuadir y tomar decisiones.

| Nivel | Descripción | Puntos |
|-------|-------------|--------|
| **Excelente** | Resumen claro, 3 hallazgos con evidencia, 2 recomendaciones accionables | 20 |
| **Bueno** | Resumen con hallazgos pero recomendaciones genéricas | 16 |
| **Básico** | Resumen incompleto o sin recomendaciones claras | 12 |
| **Insuficiente** | No incluye resumen ejecutivo | 0-8 |

**Estructura esperada:**
```markdown
## Resumen Ejecutivo

### Contexto
[1-2 oraciones sobre qué se analizó]

### Hallazgos Clave
1. [Hallazgo 1 - dato + interpretación]
2. [Hallazgo 2 - dato + interpretación]
3. [Hallazgo 3 - dato + interpretación]

### Recomendaciones
1. [Acción específica basada en los datos]
2. [Acción específica basada en los datos]
```

---

## 📝 Plantilla del Taller

### Estructura sugerida del notebook:

```
1. 📋 Información del Estudiante
   - Nombre
   - Fecha

2. 🔌 Parte 1: Conexión a la API
   - Importar librerías
   - Conectar al endpoint
   - Verificar status code
   - Convertir a DataFrame

3. 🧹 Parte 2: Limpieza de Datos
   - Exploración inicial (.head(), .info())
   - Transformar columna rating
   - Verificar tipos de datos

4. 📊 Parte 3: Análisis Exploratorio
   - Pregunta 1: Análisis de Precios (histograma + tabla)
   - Pregunta 2: Análisis de Categorías (barras)
   - Pregunta 3: Análisis de Calidad (scatter)
   - Pregunta 4: Productos Estrella (score)

5. 📝 Parte 4: Resumen Ejecutivo
   - Hallazgos clave
   - Recomendaciones
```

---

## 🤖 Uso de Herramientas de IA

### ✅ Permitido:
- Usar ChatGPT, Qwen, Claude, Gemini u otros chatbots para generar código
- Pedir explicaciones sobre errores
- Solicitar ayuda con la sintaxis de Python/pandas/matplotlib

### ⚠️ Importante:
- **NO** compartas el código con compañeros durante el taller
- **Debes entender** el código que generas
- El instructor puede preguntar sobre cualquier parte de tu análisis

### 💡 Tips para buenos prompts:

**Prompt malo:**
```
Analiza mis datos
```

**Prompt bueno:**
```
Tengo un DataFrame 'df' con las columnas:
- price: precio del producto (float)
- category: categoría (string)
- rating_rate: calificación 1-5 (float)

TAREA: Crea un gráfico de barras mostrando el precio promedio 
por categoría, ordenado de mayor a menor. Incluye título y etiquetas.
```

---

## 📌 Checklist de Entrega

Antes de entregar, verifica:

- [ ] El notebook corre de principio a fin sin errores
- [ ] Incluye tu nombre y fecha al inicio
- [ ] La conexión a la API funciona correctamente
- [ ] La columna `rating` está transformada en `rating_rate` y `rating_count`
- [ ] Tienes al menos 4 visualizaciones
- [ ] Cada visualización tiene título y etiquetas
- [ ] Respondes las 4 preguntas de negocio
- [ ] Incluyes interpretación/insight para cada análisis
- [ ] **Incluyes un resumen ejecutivo (criterio 5 de la rúbrica de práctica)**

---

## 📚 Diccionario de Términos

| Término | Descripción |
|---------|-------------|
| **API** | Interfaz para obtener datos de un servidor |
| **Endpoint** | URL específica de la API |
| **JSON** | Formato de datos tipo texto estructurado |
| **Status Code 200** | La petición fue exitosa |
| **DataFrame** | Tabla de datos en pandas |
| **json_normalize** | Función para aplanar datos anidados |
| **Histograma** | Gráfico de distribución de una variable |
| **Scatter plot** | Gráfico de dispersión (relación entre 2 variables) |
| **Insight** | Descubrimiento o conclusión de valor |
| **Accionable** | Que puede convertirse en una acción concreta |

---

## 🎓 ¡Éxitos en tu Taller!

Recuerda: El objetivo no es escribir código perfecto, sino **demostrar tu capacidad de análisis** y **comunicar hallazgos de forma profesional**.

La IA es tu copiloto, pero **tú eres el analista** que interpreta y da valor a los datos.

---

*Universidad UdeColombia - Especialización en Analítica de Datos - 2026*
