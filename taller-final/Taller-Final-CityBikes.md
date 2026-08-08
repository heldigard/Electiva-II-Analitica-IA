# Taller Final: City Bike Demand Intelligence
## Alternativa de Proyecto Final - Electiva Analítica de Datos con IA

**Modalidad:** Individual/Grupal (máx. 3 personas)
**Entregable:** Notebook de Google Colab (`.ipynb`)
**Peso:** 60% de la nota final

---

## CONTEXTO DEL NEGOCIO

### El Problema

Imagina que eres **Analista de Datos del equipo de Movilidad Urbana** de una ciudad grande. La ciudad tiene un sistema de bicicletas compartidas con **cientos de estaciones** distribuidas por toda el área urbana.

### El Desafío

Cada día, miles de ciudadanos usan estas bicicletas para desplazarse. Pero hay un **problema operativo importante**:

> Durante horas pico, muchas estaciones quedan **VACÍAS** (no hay bicicletas para usar)
> o **LLENAS** (no hay espacio para devolver la bicicleta).

Esto causa:
- 😤 **Frustración de usuarios:** Llegan a una estación y no pueden obtener/devolver bicicleta
- 📉 **Pérdida de ingresos:** Usuarios potenciales no pueden usar el servicio
- 🚨 **Quejas al servicio al cliente:** Incremento de reportes negativos

### Tu Misión

Como analista, tienes acceso a datos en **tiempo real** de todas las estaciones. Tu trabajo:

1. **Conectar a la API** de CityBikes para obtener datos actuales
2. **Analizar patrones** de disponibilidad por estación y tiempo
3. **Identificar estaciones problema** que necesitan atención
4. **Recomendar acciones operativas** para mejorar el servicio

### Tu Audiencia

Presentarás tus hallazgos al **Director de Operaciones de Movilidad**, quien necesita decisiones claras y accionables basadas en datos.

---

## OBJETIVOS DE APRENDIZAJE

Al completar este taller, serás capaz de:

| Habilidad | Cómo la Desarrollarás |
|-----------|-----------------------|
| **Conexión a APIs** | Realizar petición GET a CityBikes API |
| **Limpieza de Datos Complejos** | Manejar JSON anidado, timestamps, coordenadas geográficas |
| **Análisis Temporal** | Identificar patrones por hora del día y día de la semana |
| **Análisis Geográfico** | Usar latitud/longitud para análisis espacial |
| **Visualización de Datos** | Crear 4+ gráficos con insights claros |
| **Comunicación de Negocio** | Escribir resumen ejecutivo con recomendaciones accionables |

---

## INSTRUCCIONES PASO A PASO

### Paso 1: Conexión a la API (15 minutos)

Conecta a la API de CityBikes para obtener datos de estaciones de bicicletas.

```python
import requests
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Conectar a CityBikes API
url = "https://api.citybikes.es/v2/networks"
response = requests.get(url)

# Verificar que la conexión fue exitosa
if response.status_code == 200:
    print("✅ Conexión exitosa a CityBikes API")
    data = response.json()
else:
    print(f"❌ Error en la conexión: {response.status_code}")
```

**🎯 Objetivo:** Obtener un código de estado 200 y los datos JSON de la API.

---

### Paso 2: Exploración de Datos (15 minutos)

Explora la estructura de los datos para entender qué información está disponible.

```python
# Ver la estructura de los datos
print("Claves principales:", list(data.keys()))
print("Redes disponibles:", len(data['networks']['networks']))

# Elegir una red específica (ejemplo: ciudad de tu interés)
# Para este taller, usaremos una red específica
network_name = "bicing"  # Barcelona, o elija otra ciudad
network_url = f"https://api.citybikes.es/v2/networks/{network_name}"
network_response = requests.get(network_url)

if network_response.status_code == 200:
    network_data = network_response.json()
    stations = network_data['network']['stations']
    print(f"✅ Datos obtenidos: {len(stations)} estaciones")
else:
    print(f"❌ Error obteniendo red: {network_response.status_code}")
```

**Exploración inicial:**
```python
# Convertir a DataFrame
df = pd.json_normalize(stations)

# Los 3 comandos esenciales
print("📊 Primeras filas:")
display(df.head())

print("\n📋 Información del dataset:")
print(df.info())

print("\n📈 Estadísticas descriptivas:")
print(df.describe())
```

**🎯 Objetivo:** Entender la estructura de datos: columnas disponibles, tipos de datos, valores faltantes.

---

### Paso 3: Limpieza de Datos (30 minutos)

Prepara los datos para el análisis manejando valores faltantes, tipos de datos y campos anidados.

```python
# Seleccionar columnas relevantes
columnas_relevantes = ['name', 'latitude', 'longitude',
                       'free_bikes', 'empty_slots', 'timestamp']
df_limpio = df[columnas_relevantes].copy()

# Manejar valores faltantes
print("Valores faltantes por columna:")
print(df_limpio.isnull().sum())

# Eliminar filas con valores críticos faltantes
df_limpio = df_limpio.dropna(subset=['free_bikes', 'empty_slots'])

# Convertir timestamp a datetime
df_limpio['timestamp'] = pd.to_datetime(df_limpio['timestamp'])

# Crear columnas adicionales para análisis temporal
df_limpio['hora'] = df_limpio['timestamp'].dt.hour
df_limpio['dia_semana'] = df_limpio['timestamp'].dt.dayofweek
df_limpio['dia_nombre'] = df_limpio['timestamp'].dt.day_name()

# Crear métrica de disponibilidad
df_limpio['total_slots'] = df_limpio['free_bikes'] + df_limpio['empty_slots']
df_limpio['tasa_disponibilidad'] = df_limpio['free_bikes'] / df_limpio['total_slots']

# Verificar resultado
print("\n✅ Dataset limpio:")
print(f"- Filas: {len(df_limpio)}")
print(f"- Columnas: {list(df_limpio.columns)}")
display(df_limpio.head())
```

**🎯 Objetivo:** Dataset limpio sin valores faltantes, con columnas adicionales para análisis temporal.

---

### Paso 4: Análisis y Visualización (90 minutos)

Crea **mínimo 4 visualizaciones** para responder las preguntas de negocio.

#### Visualización 1: Top 10 Estaciones Críticas

```python
# Identificar las 10 estaciones con menor disponibilidad
top_10_bajas = df_limpio.nsmallest(10, 'tasa_disponibilidad')

plt.figure(figsize=(12, 6))
plt.barh(top_10_bajas['name'], top_10_bajas['tasa_disponibilidad'], color='coral')
plt.title('🚨 Top 10 Estaciones con Menor Disponibilidad de Bicicletas')
plt.xlabel('Tasa de Disponibilidad')
plt.ylabel('Estación')
plt.tight_layout()
plt.show()

print("\n📊 Estaciones críticas:")
for _, row in top_10_bajas.iterrows():
    print(f"- {row['name']}: {row['tasa_disponibilidad']:.1%} disponibilidad "
          f"({int(row['free_bikes'])} bicicletas de {int(row['total_slots'])})")
```

**¿Qué significa esto para el negocio?**
- Identifica estaciones que necesitan reabastecimiento URGENTE
- Estas estaciones causan la mayor frustración de usuarios

---

#### Visualización 2: Distribución de Disponibilidad por Hora

```python
# Agrupar por hora del día
por_hora = df_limpio.groupby('hora').agg({
    'free_bikes': 'mean',
    'total_slots': 'mean',
    'tasa_disponibilidad': 'mean'
}).reset_index()

plt.figure(figsize=(12, 6))
plt.plot(por_hora['hora'], por_hora['tasa_disponibilidad'],
         marker='o', linewidth=2, markersize=8, color='steelblue')
plt.title('📈 Tasa de Disponibilidad de Bicicletas por Hora del Día')
plt.xlabel('Hora del Día')
plt.ylabel('Tasa de Disponibilidad Promedio')
plt.xticks(range(0, 24))
plt.grid(True, alpha=0.3)
plt.axvspan(7, 9, alpha=0.2, color='red', label='Hora pico mañana')
plt.axvspan(17, 19, alpha=0.2, color='red', label='Hora pico tarde')
plt.legend()
plt.tight_layout()
plt.show()
```

**¿Qué significa esto para el negocio?**
- Identifica horas críticas de demanda
- Permite programar reabastecimiento preventivo

---

#### Visualización 3: Heatmap de Disponibilidad (Día x Hora)

```python
# Crear matriz de datos para heatmap
heatmap_data = df_limpio.groupby(['dia_semana', 'hora'])['tasa_disponibilidad'].mean().unstack()

# Etiquetas de días
dias = ['Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado', 'Domingo']
heatmap_data.index = heatmap_data.index.map(lambda x: dias[x])

plt.figure(figsize=(14, 8))
sns.heatmap(heatmap_data, cmap='RdYlGn', annot=False, cbar_kws={'label': 'Tasa de Disponibilidad'})
plt.title('🔥 Heatmap de Disponibilidad (Día de Semana x Hora del Día)')
plt.xlabel('Hora del Día')
plt.ylabel('Día de la Semana')
plt.tight_layout()
plt.show()
```

**¿Qué significa esto para el negocio?**
- Muestra patrones semanales completos
- Identifica momentos de mayor estrés del sistema

---

#### Visualización 4: Mapa de Estaciones (Scatter Plot Geográfico)

```python
plt.figure(figsize=(12, 10))

# Scatter plot con tamaño basado en disponibilidad
scatter = plt.scatter(
    df_limpio['longitude'],
    df_limpio['latitude'],
    s=df_limpio['total_slots'] * 2,
    c=df_limpio['tasa_disponibilidad'],
    cmap='RdYlGn',
    alpha=0.6,
    edgecolors='black',
    linewidth=0.5
)

plt.colorbar(scatter, label='Tasa de Disponibilidad')
plt.title('📍 Mapa de Estaciones de Bicicletas\n(Tamaño = Capacidad, Color = Disponibilidad)')
plt.xlabel('Longitud')
plt.ylabel('Latitud')
plt.grid(True, alpha=0.3)

# Agregar anotaciones para estaciones críticas
estaciones_criticas = df_limpio.nsmallest(5, 'tasa_disponibilidad')
for _, row in estaciones_criticas.iterrows():
    plt.annotate(f"🚨", xy=(row['longitude'], row['latitude']),
                 fontsize=15, ha='center')

plt.tight_layout()
plt.show()
```

**¿Qué significa esto para el negocio?**
- Identifica clusters geográficos con problemas
- Permite planificar redistribución geográfica de bicicletas

---

### Paso 5: Responder Preguntas de Negocio (30 minutos)

Basado en tu análisis, responde las siguientes preguntas con **evidencia numérica clara**:

#### Pregunta 1: Análisis de Disponibilidad

> **¿Cuáles son las 5 estaciones con mayor y menor disponibilidad?**

**Tu respuesta:**
```
Top 5 Estaciones con MAYOR disponibilidad:
1. [Nombre de estación] - XX% disponibilidad (XX bicicletas)
2. [Nombre de estación] - XX% disponibilidad (XX bicicletas)
3. [Nombre de estación] - XX% disponibilidad (XX bicicletas)
4. [Nombre de estación] - XX% disponibilidad (XX bicicletas)
5. [Nombre de estación] - XX% disponibilidad (XX bicicletas)

Top 5 Estaciones con MENOR disponibilidad:
1. [Nombre de estación] - XX% disponibilidad (XX bicicletas)
2. [Nombre de estación] - XX% disponibilidad (XX bicicletas)
3. [Nombre de estación] - XX% disponibilidad (XX bicicletas)
4. [Nombre de estación] - XX% disponibilidad (XX bicicletas)
5. [Nombre de estación] - XX% disponibilidad (XX bicicletas)

**INSIGHT:** [Qué significa esto para el negocio? Qué acción deberíamos tomar?]
```

---

#### Pregunta 2: Patrones Temporales

> **¿Qué horas del día y días de la semana muestran mayor desbalance?**

**Tu respuesta:**
```
Horas críticas:
- La hora con MENOR disponibilidad: [hora:00] con XX% promedio
- La hora con MAYOR disponibilidad: [hora:00] con XX% promedio

Días críticos:
- El día con MENOR disponibilidad: [nombre del día] con XX% promedio
- El día con MAYOR disponibilidad: [nombre del día] con XX% promedio

**INSIGHT:** [Qué significa esto para programar reabastecimiento?]
```

---

#### Pregunta 3: Análisis Geográfico

> **¿Hay clusters geográficos con problemas persistentes de disponibilidad?**

**Tu respuesta:**
```
Basado en el análisis del mapa:

Cluster 1 - [Ubicación geográfica, ej: "Norte de la ciudad"]:
- XX estaciones con disponibilidad promedio de XX%
- Problemática principal: [describir el problema]

Cluster 2 - [Ubicación geográfica]:
- XX estaciones con disponibilidad promedio de XX%
- Problemática principal: [describir el problema]

**INSIGHT:** [Qué acción geográfica deberíamos tomar?]
```

---

#### Pregunta 4: Volatilidad de Estaciones

> **¿Qué estaciones muestran mayor volatilidad (cambios rápidos en disponibilidad)?**

**Tu respuesta:**
```
Estaciones volátiles identificadas:
1. [Nombre] - Alta variabilidad entre [hora min] y [hora max]
2. [Nombre] - Alta variabilidad entre [hora min] y [hora max]
3. [Nombre] - Alta variabilidad entre [hora min] y [hora max]

**INSIGHT:** [Por qué estas estaciones son volátiles? Cómo deberíamos manejarlas diferente?]
```

---

#### Pregunta 5: Recomendaciones Operativas

> **Basado en tu análisis, ¿qué 3 acciones prioritarias deberíamos implementar el próximo mes?**

**Tu respuesta:**
```
Acción 1: [Título claro y accionable]
- Evidencia: [dato numérico que respalda la acción]
- Impacto esperado: [qué mejorará esta acción]
- Prioridad: [Alta/Media/Baja]

Acción 2: [Título claro y accionable]
- Evidencia: [dato numérico que respalda la acción]
- Impacto esperado: [qué mejorará esta acción]
- Prioridad: [Alta/Media/Baja]

Acción 3: [Título claro y accionable]
- Evidencia: [dato numérico que respalda la acción]
- Impacto esperado: [qué mejorará esta acción]
- Prioridad: [Alta/Media/Baja]
```

---

### Paso 6: Resumen Ejecutivo (30 minutos)

Escribe un resumen ejecutivo profesional dirigido al **Director de Operaciones de Movilidad**.

#### Estructura del Resumen Ejecutivo

```markdown
# City Bike Demand Intelligence - Resumen Ejecutivo

**Para:** Director de Operaciones de Movilidad
**De:** Analista de Datos
**Fecha:** [Fecha actual]
**Tema:** Análisis de Disponibilidad de Estaciones de Bicicletas

---

## 📊 Contexto

[2-3 frases explicando el propósito del análisis y el dataset analizado]

## 🎯 Hallazgos Clave

### Hallazgo 1: [Título descriptivo]
[Evidencia numérica específica]
[Interpretación del hallazgo]

### Hallazgo 2: [Título descriptivo]
[Evidencia numérica específica]
[Interpretación del hallazgo]

### Hallazgo 3: [Título descriptivo]
[Evidencia numérica específica]
[Interpretación del hallazgo]

## 💡 Recomendaciones

### Recomendación 1: [Título accionable]
- **Acción específica:** [qué hacer exactamente]
- **Evidencia:** [dato que respalda la recomendación]
- **Impacto esperado:** [qué mejorará]
- **Timeline:** [cuándo implementar]

### Recomendación 2: [Título accionable]
- **Acción específica:** [qué hacer exactamente]
- **Evidencia:** [dato que respalda la recomendación]
- **Impacto esperado:** [qué mejorará]
- **Timeline:** [cuándo implementar]

## 📈 Métricas de Seguimiento

Para evaluar el éxito de estas recomendaciones, monitorear:
1. [Métrica 1 con baseline actual]
2. [Métrica 2 con baseline actual]

---

**Conclusión:** [1-2 frases de cierre persuasivas]
```

---

## RÚBRICA DE EVALUACIÓN

| Criterio | Peso | Excelente (100%) | Bueno (75%) | Aceptable (50%) | Insuficiente (0%) |
|----------|------|------------------|-------------|-----------------|-------------------|
| **1. Conexión API** | 15% | Conexión exitosa, manejo robusto de errores, parseo correcto de JSON | Conexión exitosa, mínimo manejo de errores | Conexión exitosa sin manejo de errores | No logra conectar a la API |
| **2. Limpieza de Datos** | 20% | Limpieza exhaustiva: NaN, duplicados, tipos, campos anidados, timestamps | Limpieza adecuada con manejo básico de problemas | Limpieza mínima con algunos problemas sin resolver | No realiza limpieza significativa |
| **3. Análisis Visual** | 25% | 4+ gráficos con títulos, etiquetas, interpretación detallada, insight de negocio | 4 gráficos con títulos básicos e interpretación aceptable | 3-4 gráficos con mínima interpretación | Menos de 3 gráficos o sin interpretación |
| **4. Respuestas de Negocio** | 25% | 5 preguntas respondidas con evidencia numérica clara e insights profundos | 4-5 preguntas respondidas con evidencia adecuada | 3-4 preguntas respondidas con evidencia básica | Menos de 3 preguntas o sin evidencia |
| **5. Resumen Ejecutivo** | 15% | 3 hallazgos + 2 recomendaciones con evidencia, impacto, timeline; formato profesional | 3 hallazgos + 2 recomendaciones con formato adecuado | 2-3 hallazgos + 1-2 recomendaciones básicas | Menos de 2 hallazgos o sin recomendaciones |

**Bonus (+10%):** Uno de los gráficos es un "gráfico narrativo" con anotaciones, headline y call-to-action

---

## RECURSOS Y AYUDA

### APIs y Documentación

- **CityBikes API:** https://api.citybikes.es/v2/
- **Documentación:** http://api.citybikes.es/v2/
- **Lista de redes:** https://api.citybikes.es/v2/networks

### Chatbots de IA para Ayuda

Si te quedas atascado, puedes usar:
- **Qwen** (Qwen3.5, 100% gratuito) - Recomendado
- ✨ **Gemini en Colab** (integrado en tu notebook, panel ✨)
- **ChatGPT** (GPT-5.6)
- **Claude** (Sonnet 5)
- **Kimi** (K3, cuota gratuita generosa)

**Ejemplo de prompt para pedir ayuda:**
```
"Estoy trabajando en un proyecto de análisis de datos de bicicletas compartidas.
Tengo un DataFrame con columnas: 'free_bikes', 'empty_slots', 'latitude', 'longitude'.
Quiero crear un gráfico que muestre las estaciones con menor disponibilidad.
¿Qué tipo de gráfico me recomiendas y cómo genero el código con matplotlib?"
```

### Código de Referencia

> **BACKUP DISPONIBLE:** Si la API falla durante clase, hay un dataset de respaldo disponible en `datos/citybikes-backup.csv` con 100 estaciones de muestra.

Si la API falla, puedes usar este dataset de respaldo:

```python
# Opción 1: Cargar el backup CSV
import pandas as pd

df = pd.read_csv('datos/citybikes-backup.csv')
print(f"Dataset de respaldo cargado: {len(df)} estaciones")

# Opción 2: Generar datos simulados
import numpy as np

np.random.seed(42)
n_estaciones = 100

df = pd.DataFrame({
    'name': [f'Estación {i}' for i in range(n_estaciones)],
    'latitude': np.random.uniform(40.4, 40.5, n_estaciones),
    'longitude': np.random.uniform(-3.8, -3.6, n_estaciones),
    'free_bikes': np.random.randint(0, 20, n_estaciones),
    'empty_slots': np.random.randint(0, 15, n_estaciones),
    'timestamp': pd.date_range('2026-01-26 08:00', periods=n_estaciones, freq='15min')
})
```

---

## TIPS PARA EL ÉXITO

### ✅ Buenas Prácticas

1. **Itera tus prompts:** Si el chatbot no te da lo que necesitas, reformula tu pregunta
2. **Valida con criterio humano:** No confíes ciegamente en el código que genera la IA
3. **Interpreta, no solo describas:** Siempre explica "qué significa esto para el negocio"
4. **Usa anotaciones en gráficos:** Agrega títulos, etiquetas y flechas explicativas
5. **Sé específico en tus prompts:** Incluye contexto, datos que tienes, y qué quieres obtener

### ❌ Errores Comunes

1. **Copiar código sin entender:** Siempre verifica qué hace el código antes de ejecutar
2. **Describir en lugar de analizar:** "El gráfico muestra barras rojas" no es un insight
3. **Olvidar el contexto de negocio:** Tu audiencia es el Director de Operaciones, no un técnico
4. **Ignorar valores faltantes:** Siempre verifica NaN y outliers antes de analizar
5. **Quedarse atascado太久:** Si no puedes resolver un problema en 10 minutos, pide ayuda al instructor

---

## CHECKLIST DE ENTREGA

Antes de entregar, verifica que tienes:

- [ ] Notebook con todas las celdas ejecutadas (sin errores)
- [ ] Conexión exitosa a CityBikes API (o dataset de respaldo)
- [ ] Dataset limpio sin valores faltantes
- [ ] Mínimo 4 gráficos con títulos y etiquetas
- [ ] 5 preguntas de negocio respondidas con evidencia numérica
- [ ] Resumen ejecutivo con 3 hallazgos + 2 recomendaciones
- [ ] Interpretación de "qué significa esto para el negocio" en cada sección
- [ ] Notebook organizado con secciones claras (usa celdas markdown)

---

**¡Buena suerte! 🚴‍♂️📊**

Recuerda: El objetivo no es escribir código perfecto. El objetivo es **extraer insights de negocio** que permitan mejorar el servicio de bicicletas compartidas.

La IA es tu herramienta para escribir código. **TÚ eres el experto que interpreta los datos y toma decisiones.**
