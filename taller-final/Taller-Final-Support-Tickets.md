# Taller Final: Customer Support Intelligence & Product Improvement
## Opción C de Proyecto Final - Electiva Analítica de Datos con IA

**Modalidad:** Individual/Grupal (máx. 3 personas)
**Entregable:** Notebook de Google Colab (`.ipynb`)
**Peso:** 60% de la nota final

---

## CONTEXTO DEL NEGOCIO

### El Problema

Imagina que eres **Analista de Soporte al Cliente** en una empresa SaaS B2B que vende software de gestión empresarial. La empresa tiene **miles de clientes** y recibe **cientos de tickets de soporte** cada semana.

### El Desafío

El equipo de soporte está **sobrecargado** y el director necesita reducir el volumen de tickets mientras mejora la satisfacción del cliente:

> El volumen de tickets ha crecido un **40% en los últimos 6 meses**
> El tiempo de resolución promedio ha aumentado de **24h a 72h**
> La satisfacción del cliente (CSAT) ha caído de **4.2 a 3.5/5**
> El equipo de producto no sabe qué features causan más problemas

### Tu Misión

Como analista, tienes acceso a datos históricos de tickets de soporte. Tu trabajo:

1. **Analizar datos de tickets** para identificar drivers principales de volumen
2. **Descubrir patrones** en tipos de problemas, canales y tiempos de resolución
3. **Correlacionar problemas** con features del producto
4. **Recomendar acciones** para reducir volumen y mejorar satisfacción

### Tu Audiencia

Presentarás tus hallazgos al **Director de Soporte y al Director de Producto**, quienes necesitan decisiones claras basadas en datos para priorizar el roadmap del producto.

---

## OBJETIVOS DE APRENDIZAJE

Al completar este taller, serás capaz de:

| Habilidad | Cómo la Desarrollarás |
|-----------|-----------------------|
| **Análisis de Datos Categóricos** | Trabajar con variables categóricas (tipo, canal, prioridad) |
| **Análisis de Tiempos y Duraciones** | Manejar timestamps, calcular duraciones |
| **Análisis de Correlaciones** | Descubrir relaciones entre variables |
| **Segmentación** | Agrupar datos por múltiples dimensiones |
| **Visualización de Datos** | Crear 4+ gráficos con insights de negocio |
| **Comunicación de Producto** | Escribir recomendaciones accionables para roadmap |

---

## FUENTE DE DATOS

### Opción A: Dataset Sintético (Generado con Python)

Si no tienes acceso a datos reales, puedes generar un dataset sintético realista:

```python
import pandas as pd
import numpy as np
from datetime import datetime, timedelta

np.random.seed(42)
n_tickets = 1000

# Categorías de tickets
categorias = [
    'Login/Authentication', 'Performance', 'Feature Request', 'Bug/Error',
    'Billing/Payment', 'Integration', 'Data Export', 'UI/UX',
    'Security', 'Mobile App'
]

canales = ['Email', 'Chat', 'Phone', 'Portal']
prioridades = ['Low', 'Medium', 'High', 'Critical']
estados = ['Open', 'In Progress', 'Resolved', 'Closed']

# Generar datos
df = pd.DataFrame({
    'ticket_id': range(1, n_tickets + 1),
    'categoria': np.random.choice(categorias, n_tickets, p=[0.15, 0.12, 0.10, 0.20, 0.08, 0.10, 0.08, 0.10, 0.03, 0.04]),
    'canal': np.random.choice(canales, n_tickets, p=[0.35, 0.30, 0.15, 0.20]),
    'prioridad': np.random.choice(prioridades, n_tickets, p=[0.30, 0.40, 0.20, 0.10]),
    'estado': np.random.choice(estados, n_tickets, p=[0.10, 0.20, 0.40, 0.30]),
    'cliente_id': np.random.randint(1, 200, n_tickets),
    'satisfaccion': np.random.choice([1, 2, 3, 4, 5], n_tickets, p=[0.05, 0.10, 0.25, 0.35, 0.25]),
    'fecha_creacion': pd.date_range('2025-07-01', periods=n_tickets, freq='1h'),
    'hora_creacion': np.random.randint(0, 24, n_tickets),
    'dia_semana': np.random.randint(0, 7, n_tickets)
})

# Calcular tiempo de resolución (en horas)
# Tickets resueltos: entre 1h y 120h
# Tickets no resueltos: NaN
df['tiempo_resolucion_horas'] = np.where(
    df['estado'].isin(['Resolved', 'Closed']),
    np.random.uniform(1, 120, n_tickets),
    np.nan
)

# Añadir correlación: bugs críticos tardan más
mask_bug_critical = (df['categoria'] == 'Bug/Error') & (df['prioridad'] == 'Critical')
df.loc[mask_bug_critical, 'tiempo_resolucion_horas'] = np.random.uniform(48, 168, mask_bug_critical.sum())

# Añadir correlación: billing tickets tienen menor satisfacción
df.loc[df['categoria'] == 'Billing/Payment', 'satisfaccion'] = df.loc[
    df['categoria'] == 'Billing/Payment', 'satisfacción'
].apply(lambda x: min(x, 3))

# Añadir columna mes
df['mes'] = df['fecha_creacion'].dt.month_name()

# Guardar
df.to_csv('datos/support-tickets.csv', index=False)
print(f"Dataset generado: {len(df)} tickets")
print(df.head())
```

### Opción B: Dataset Real (si tienes acceso)

Si tienes acceso a datos reales de tickets de soporte (anónimos), úsalos. Asegúrate de:

1. **Anonizar datos** - Remover nombres, emails, teléfonos
2. **Redactar contenido sensible** - No compartir con chatbots gratuitos
3. **Verificar columnas** - Debe tener al menos: categoría, canal, tiempo_resolucion, satisfaccion

### Opción C: Dataset de Respaldo (Backup)

Si no puedes generar datos, usa el dataset de respaldo disponible en:
```
datos/support-tickets-backup.csv
```

```python
# Cargar dataset de respaldo
import pandas as pd

df = pd.read_csv('datos/support-tickets-backup.csv')
print(f"Dataset cargado: {len(df)} tickets")
print(df.head())
```

---

## INSTRUCCIONES PASO A PASO

### Paso 1: Carga y Exploración de Datos (15 minutos)

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Cargar dataset
df = pd.read_csv('support-tickets-backup.csv')

# Exploración inicial
print("Dataset shape:", df.shape)
print("\nColumnas:", df.columns.tolist())
print("\nPrimeras filas:")
display(df.head())

print("\nInformación del dataset:")
print(df.info())

print("\nEstadísticas descriptivas:")
print(df.describe())
```

**Validaciones:**
- [ ] Dataset cargado correctamente
- [ ] No hay valores faltantes críticos
- [ ] Tipos de datos correctos

---

### Paso 2: Limpieza y Preparación de Datos (20 minutos)

```python
# Manejar valores faltantes
print("Valores faltantes por columna:")
print(df.isnull().sum())

# Si hay tickets sin tiempo de resolución (abiertos), crear indicador
df['es_resuelto'] = df['tiempo_resolucion_horas'].notna().astype(int)

# Convertir columnas de fecha si existen
if 'fecha_creacion' in df.columns:
    df['fecha_creacion'] = pd.to_datetime(df['fecha_creacion'])
    df['dia_semana'] = df['fecha_creacion'].dt.day_name()
    df['mes'] = df['fecha_creacion'].dt.month_name()

# Verificar resultado
print("\nDataset limpio:")
print(f"- Total tickets: {len(df)}")
print(f"- Tickets resueltos: {df['es_resuelto'].sum()}")
print(f"- Tickets abiertos: {(1 - df['es_resuelto']).sum()}")
```

**Validaciones:**
- [ ] Valores faltantes manejados
- [ ] Columnas de fecha convertidas
- [ ] Indicadores creados

---

### Paso 3: Análisis y Visualización (90 minutos)

Crea **mínimo 4 visualizaciones** para responder las preguntas de negocio.

#### Visualización 1: Top 10 Categorías por Volumen

```python
# Contar tickets por categoría
top_categorias = df['categoria'].value_counts().head(10)

plt.figure(figsize=(12, 6))
top_categorias.plot(kind='barh', color='steelblue')
plt.title('Top 10 Categorías de Tickets por Volumen')
plt.xlabel('Número de Tickets')
plt.ylabel('Categoría')
plt.tight_layout()
plt.show()

print("\n📊 Distribución por categoría:")
for cat, count in top_categorias.items():
    pct = count / len(df) * 100
    print(f"- {cat}: {count} tickets ({pct:.1f}%)")
```

**¿Qué significa esto para el negocio?**
- Identifica las áreas que generan mayor carga de trabajo
- Puede indicar features problemáticas del producto
- Guía prioridades de documentación y mejoras de UX

---

#### Visualización 2: Tiempo de Resolución por Categoría (Boxplot)

```python
# Filtrar solo tickets resueltos
df_resueltos = df[df['es_resuelto'] == 1].copy()

plt.figure(figsize=(14, 8))
sns.boxplot(data=df_resueltos, x='categoria', y='tiempo_resolucion_horas', palette='Set2')
plt.title('Tiempo de Resolución por Categoría (Solo Tickets Resueltos)')
plt.xlabel('Categoría')
plt.ylabel('Tiempo de Resolución (horas)')
plt.xticks(rotation=45, ha='right')
plt.axhline(y=24, color='red', linestyle='--', label='SLA: 24h')
plt.axhline(y=72, color='orange', linestyle='--', label='Promedio actual: 72h')
plt.legend()
plt.tight_layout()
plt.show()

# Estadísticas por categoría
print("\n📈 Tiempo promedio de resolución por categoría:")
por_categoria = df_resueltos.groupby('categoria')['tiempo_resolucion_horas'].agg(['mean', 'median', 'count'])
por_categoria = por_categoria.sort_values('mean', ascending=False)
print(por_categoria)
```

**¿Qué significa esto para el negocio?**
- Identifica categorías que requieren más tiempo especializado
- Puede indicar falta de documentación o formación
- Guía asignación de recursos del equipo de soporte

---

#### Visualización 3: Satisfacción del Cliente por Categoría

```python
# Calcular satisfacción promedio por categoría
sat_por_categoria = df.groupby('categoria')['satisfaccion'].mean().sort_values(ascending=True)

plt.figure(figsize=(12, 6))
colors = ['coral' if x < 3.5 else 'steelblue' for x in sat_por_categoria]
sat_por_categoria.plot(kind='barh', color=colors)
plt.title('Satisfacción del Cliente Promedio por Categoría')
plt.xlabel('Satisfacción Promedio (1-5)')
plt.ylabel('Categoría')
plt.axvline(x=3.5, color='red', linestyle='--', label='Umbral crítico: 3.5')
plt.legend()
plt.tight_layout()
plt.show()

print("\n⭐ Categorías con menor satisfacción:")
baja_sat = sat_por_categoria.head(5)
for cat, sat in baja_sat.items():
    print(f"- {cat}: {sat:.2f}/5")
```

**¿Qué significa esto para el negocio?**
- Categorías con baja satisfacción = insatisfacción del cliente
- Correlaciona con churn (pérdida de clientes)
- Prioridad alta para improvements de producto

---

#### Visualización 4: Heatmap de Volumen (Día x Hora)

```python
# Crear matriz de volumen por día y hora
heatmap_data = df.groupby(['dia_semana', 'hora_creacion']).size().unstack(fill_value=0)

# Ordenar días correctamente
dias_orden = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
heatmap_data = heatmap_data.reindex(dias_orden)

plt.figure(figsize=(14, 8))
sns.heatmap(heatmap_data, cmap='YlOrRd', annot=False, cbar_kws={'label': 'Número de Tickets'})
plt.title('Heatmap de Volumen de Tickets (Día de Semana x Hora del Día)')
plt.xlabel('Hora del Día')
plt.ylabel('Día de la Semana')
plt.tight_layout()
plt.show()
```

**¿Qué significa esto para el negocio?**
- Identifica horas/días de mayor demanda
- Permite ajustar staffing del equipo de soporte
- Puede correlacionarse con lanzamientos de features o problemas

---

### Paso 4: Responder Preguntas de Negocio (30 minutos)

Basado en tu análisis, responde las siguientes preguntas con **evidencia numérica clara**:

#### Pregunta 1: Drivers de Volumen

> **¿Cuáles son los 5 drivers principales de volumen de tickets?**

**Tu respuesta:**
```
Top 5 Categorías por Volumen:
1. [Categoría] - X tickets (X% del total)
2. [Categoría] - X tickets (X% del total)
3. [Categoría] - X tickets (X% del total)
4. [Categoría] - X tickets (X% del total)
5. [Categoría] - X tickets (X% del total)

INSIGHT: [Qué significa esto para el negocio?
- ¿Son bugs recurrentes de ciertas features?
- ¿Falta de documentación en áreas específicas?
- ¿Problemas de UX que causan confusión?]

RECOMENDACIÓN: [Qué acción tomar para reducir volumen]
```

---

#### Pregunta 2: Tiempos de Resolución

> **¿Qué categorías toman más tiempo resolver y por qué?**

**Tu respuesta:**
```
Categorías con mayor tiempo de resolución:
1. [Categoría] - X horas promedio
2. [Categoría] - X horas promedio
3. [Categoría] - X horas promedio

ANÁLISIS CAUSA-RAÍZ:
- [¿Falta de documentación interna?]
- [¿Requiere especialistas?]
- [¿Depende de otros equipos?]

RECOMENDACIÓN: [Cómo reducir tiempos]
```

---

#### Pregunta 3: Satisfacción del Cliente

> **¿Existe correlación entre tipo de problema y satisfacción del cliente?**

**Tu respuesta:**
```
Categorías con menor satisfacción:
1. [Categoría] - X.X/5 ⭐
2. [Categoría] - X.X/5 ⭐
3. [Categoría] - X.X/5 ⭐

Categorías con mayor satisfacción:
1. [Categoría] - X.X/5 ⭐
2. [Categoría] - X.X/5 ⭐
3. [Categoría] - X.X/5 ⭐

CORRELACIÓN IDENTIFICADA:
- [¿Qué patrón observas?]
- [¿Hay canales con mejor satisfacción?]
- [¿Hay horarios con peor satisfacción?]

IMPACTO EN CHURN: [Cómo esto afecta la retención]
```

---

#### Pregunta 4: Recomendaciones de Roadmap

> **Basado en tu análisis, ¿qué 3 mejoras de producto deberían priorizarse en el roadmap?**

**Tu respuesta:**
```
Prioridad 1: [Feature/Mejora específica]
- Categoría afectada: [nombre]
- Tickets por mes: [X]
- Impacto en satisfacción: [-X.X puntos]
- Esfuerzo estimado: [Low/Medium/High]
- ROI esperado: [Alto/Medio/Bajo]

Prioridad 2: [Feature/Mejora específica]
- Categoría afectada: [nombre]
- Tickets por mes: [X]
- Impacto en satisfacción: [-X.X puntos]
- Esfuerzo estimado: [Low/Medium/High]
- ROI esperado: [Alto/Medio/Bajo]

Prioridad 3: [Feature/Mejora específica]
- Categoría afectada: [nombre]
- Tickets por mes: [X]
- Impacto en satisfacción: [-X.X puntos]
- Esfuerzo estimado: [Low/Medium/High]
- ROI esperado: [Alto/Medio/Bajo]
```

---

### Paso 5: Resumen Ejecutivo (30 minutos)

Escribe un resumen ejecutivo profesional dirigido al **Director de Soporte y Director de Producto**.

```markdown
# Customer Support Intelligence - Resumen Ejecutivo

**Para:** Director de Soporte, Director de Producto
**De:** Analista de Datos
**Fecha:** [Fecha actual]
**Tema:** Análisis de Tickets de Soporte y Recomendaciones de Roadmap

---

## 📊 Contexto

[2-3 frases explicando el análisis realizado: dataset, período, alcance]

## 🎯 Hallazgos Clave

### Hallazgo 1: [Título descriptivo]
[Evidencia numérica específica]
[Interpretación del hallazgo y su impacto]

### Hallazgo 2: [Título descriptivo]
[Evidencia numérica específica]
[Interpretación del hallazgo y su impacto]

### Hallazgo 3: [Título descriptivo]
[Evidencia numérica específica]
[Interpretación del hallazgo y su impacto]

## 💡 Recomendaciones de Roadmap

### Recomendación 1: [Feature/Mejora prioridad alta]
- **Problema:** [Descripción del problema actual]
- **Solución:** [Qué construir/cambiar]
- **Evidencia:** [X tickets/mes, -X.X satisfacción]
- **Impacto esperado:** [-X% volumen, +X.X CSAT]
- **Esfuerzo:** [Low/Medium/High]
- **Timeline:** [X semanas]

### Recomendación 2: [Feature/Mejora prioridad media]
- **Problema:** [Descripción del problema actual]
- **Solución:** [Qué construir/cambiar]
- **Evidencia:** [X tickets/mes, -X.X satisfacción]
- **Impacto esperado:** [-X% volumen, +X.X CSAT]
- **Esfuerzo:** [Low/Medium/High]
- **Timeline:** [X semanas]

### Recomendación 3: [Mejora de Proceso/Documentación]
- **Problema:** [Descripción del problema actual]
- **Solución:** [Qué mejorar]
- **Evidencia:** [X tickets/mes]
- **Impacto esperado:** [-X% volumen]
- **Esfuerzo:** [Low]
- **Timeline:** [X semanas]

## 📈 Métricas de Seguimiento

Para evaluar el éxito de estas recomendaciones, monitorear mensualmente:

| Métrica | Baseline Actual | Objetivo 3 meses | Objetivo 6 meses |
|---------|-----------------|------------------|------------------|
| Volumen de tickets | [X/sem] | [X/sem] | [X/sem] |
| Tiempo resolución promedio | [Xh] | [Xh] | [Xh] |
| Satisfacción (CSAT) | [X.X/5] | [X.X/5] | [X.X/5] |
| Tickets por categoría prioritaria | [X] | [X] | [X] |

---

## 🚀 Próximos Pasos

1. [Acción inmediata - semana 1]
2. [Acción corto plazo - mes 1]
3. [Acción mediano plazo - meses 2-3]

---

**Conclusión:** [1-2 frases de cierre persuasivas sobre el impacto esperado]
```

---

## RÚBRICA DE EVALUACIÓN

| Criterio | Peso | Excelente (100%) | Bueno (75%) | Aceptable (50%) | Insuficiente (0%) |
|----------|------|------------------|-------------|-----------------|-------------------|
| **1. Carga y Limpieza** | 15% | Dataset cargado, limpieza exhaustiva, validaciones completas | Dataset cargado, limpieza adecuada | Dataset cargado, limpieza básica | No logra cargar dataset |
| **2. Análisis Visual** | 25% | 4+ gráficos con títulos, interpretación detallada, insights de negocio | 4 gráficos con interpretación aceptable | 3-4 gráficos con mínima interpretación | Menos de 3 gráficos o sin interpretación |
| **3. Preguntas de Negocio** | 30% | 4 preguntas respondidas con evidencia numérica clara e insights profundos | 4 preguntas respondidas con evidencia adecuada | 3-4 preguntas respondidas con evidencia básica | Menos de 3 preguntas o sin evidencia |
| **4. Recomendaciones Roadmap** | 15% | 3 recomendaciones priorizadas con evidencia, impacto, esfuerzo, timeline | 3 recomendaciones con formato adecuado | 2-3 recomendaciones básicas | Menos de 2 recomendaciones |
| **5. Resumen Ejecutivo** | 15% | Resumen profesional con 3 hallazgos + 3 recomendaciones + métricas de seguimiento | Resumen con 3 hallazgos + 3 recomendaciones | Resumen incompleto | Sin resumen ejecutivo |

**Bonus (+10%):** Uno de los gráficos es un "gráfico narrativo" con anotaciones, headline y call-to-action

---

## RECURSOS Y AYUDA

### Chatbots de IA para Ayuda

Si te quedas atascado, puedes usar:
- **Qwen** (100% gratuito) - Recomendado
- **ChatGPT** (GPT-4o mini)
- **Google AI Studio** (Gemini)
- **Claude**
- **Julius AI** (para análisis de datos conversacional)

**Ejemplo de prompt para pedir ayuda:**
```
"Estoy analizando datos de tickets de soporte al cliente.
Tengo un DataFrame con columnas: 'categoria', 'tiempo_resolucion_horas', 'satisfaccion'.
Quiero crear un boxplot que muestre el tiempo de resolución por categoría,
pero solo para tickets resueltos. ¿Cómo genero el código con seaborn?"
```

---

## TIPS PARA EL ÉXITO

### ✅ Buenas Prácticas

1. **Enfócate en el "por qué":** No solo describir patrones, sino explicar causas
2. **Prioriza por impacto:** No todas las categorías merecen la misma atención
3. **Conecta con roadmap:** Las recomendaciones deben ser accionables para producto
4. **Usa evidencia numérica:** Siempre respalda tus afirmaciones con datos
5. **Piensa en ROI:** Las recomendaciones deben considerar esfuerzo vs beneficio

### ❌ Errores Comunes

1. **Describir en lugar de analizar:** "El gráfico muestra barras azules" no es un insight
2. **Ignorar contexto de negocio:** Tu audiencia es directiva, no técnica
3. **Olvidar el costo de oportunidad:** Recomendar todo = no priorizar
4. **No validar suposiciones:** Asegúrate de que las correlaciones sean reales
5. **Ignorar la satisfacción:** El volumen no lo es todo, la calidad importa

---

## CHECKLIST DE ENTREGA

Antes de entregar, verifica que tienes:

- [ ] Notebook con todas las celdas ejecutadas (sin errores)
- [ ] Dataset de tickets cargado y limpio
- [ ] Mínimo 4 gráficos con títulos y etiquetas
- [ ] 4 preguntas de negocio respondidas con evidencia numérica
- [ ] 3 recomendaciones de roadmap priorizadas
- [ ] Resumen ejecutivo con hallazgos + recomendaciones + métricas
- [ ] Interpretación de "qué significa esto para el producto" en cada sección
- [ ] Notebook organizado con secciones claras (usa celdas markdown)

---

**¡Buena suerte! 🎫📊**

Recuerda: El objetivo no es escribir código perfecto. El objetivo es **extraer insights de soporte al cliente** que permitan mejorar el producto y reducir la carga del equipo de soporte.

La IA es tu herramienta para escribir código. **TÚ eres el experto que interpreta los datos y recomienda mejoras del producto.**

---

*Universidad UdeColombia - Especialización en Analítica de Datos - 2026*
