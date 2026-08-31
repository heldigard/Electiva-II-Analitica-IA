# Taller Final: SME Revenue & Pricing Optimization
## Opción D de Proyecto Final - Electiva Analítica de Datos con IA

**Modalidad:** Individual/Grupal (máx. 3 personas)
**Entregable:** Notebook de Google Colab (`.ipynb`)
**Uso:** práctica integradora de preparación para el Quiz 3, sin ponderación independiente

---

## CONTEXTO DEL NEGOCIO

### El Problema

Imagina que eres **Consultor de Analítica** para una **PyME de retail** (pequeña-mediana empresa) que vende productos de consumo masivo. La empresa tiene **50 productos** en su catálogo y opera en un mercado competitivo con márgenes ajustados.

### El Desafío

El dueño de la PyME necesita tomar decisiones estratégicas sobre precios y promociones:

> Los **márgenes de ganancia** han caído del **18% al 12%** en el último año
> No sabe si sus **descuentos y promociones** realmente generan más ventas o solo canibalizan revenue
> No identifica claramente **qué productos son más rentables**
> Sufre de **problemas de flujo de caja** en ciertos meses del año
> No tiene un equipo de datos, solo su Excel de ventas

### Tu Misión

Como consultor de analítica, tienes acceso a datos históricos de ventas, costos y promociones. Tu trabajo:

1. **Analizar márgenes** por producto y categoría
2. **Identificar patrones estacionales** en ventas y flujo de caja
3. **Evaluar impacto de descuentos** en revenue y margen
4. **Recomendar estrategia de pricing** con impacto financiero cuantificado

### Tu Audiencia

Presentarás tus hallazgos al **Dueño de la PyME y al Director Comercial**, quienes necesitan decisiones claras para mejorar la rentabilidad del negocio sin un equipo de datos dedicado.

---

## OBJETIVOS DE APRENDIZAJE

Al completar este taller, serás capaz de:

| Habilidad | Cómo la Desarrollarás |
|-----------|-----------------------|
| **Análisis de Márgenes** | Calcular revenue, costo, margen y ROI por producto |
| **Análisis Estacional** | Identificar patrones de estacionalidad en ventas |
| **Análisis de Sensibilidad** | Evaluar impacto de cambios de precio en demanda |
| **Análisis de Promociones** | Medir efectividad de descuentos y ofertas |
| **Visualización de Datos** | Crear 4+ gráficos con insights financieros |
| **Comunicación de Negocio** | Escribir recomendaciones con impacto financiero |

---

## FUENTE DE DATOS

### Opción A: Dataset Sintético (Generado con Python)

Genera un dataset realista de una PyME de retail:

```python
import pandas as pd
import numpy as np
from datetime import datetime, timedelta

np.random.seed(42)
n_ventas = 3000

# Productos y categorías
productos = {
    'Lácteos': ['Leche Entera 1L', 'Yogurt Natural', 'Queso Fresco', 'Mantequilla'],
    'Bebidas': ['Agua Mineral', 'Jugo de Naranja', 'Gaseosa Cola', 'Cerveza'],
    'Snacks': ['Papas Fritas', 'Nachos', 'Popcorn', 'Mix Frutos Secos'],
    'Limpieza': ['Detergente', 'Jabón Líquido', 'Cloro', 'Esponjas'],
    'Cereal': ['Corn Flakes', 'Avena', 'Granola', 'Cereal con Chocolate']
}

# Crear lista de productos
lista_productos = []
for categoria, items in productos.items():
    for producto in items:
        lista_productos.append({'producto': producto, 'categoria': categoria})

df_productos = pd.DataFrame(lista_productos)

# Precios y costos base
np.random.seed(42)
df_productos['precio_base'] = np.random.uniform(2000, 15000, len(df_productos))
df_productos['costo'] = df_productos['precio_base'] * np.random.uniform(0.60, 0.85, len(df_productos))
df_productos['margen_base'] = ((df_productos['precio_base'] - df_productos['costo']) / df_productos['precio_base'] * 100).round(2)

# Generar ventas
fechas = pd.date_range('2024-01-01', '2025-12-31', freq='D')

ventas_data = []
for fecha in fechas:
    # Número de ventas por día (con estacionalidad)
    # Factor estacional (diciembre +40%, enero -30%)
    mes = fecha.month
    if mes == 12:
        factor_estacional = 1.4
    elif mes == 1:
        factor_estacional = 0.7
    elif mes in [6, 7]:  # Verano
        factor_estacional = 1.1
    else:
        factor_estacional = 1.0

    # Factor día de semana (fin de semana +20%)
    if fecha.weekday() >= 5:
        factor_dia = 1.2
    else:
        factor_dia = 1.0

    # Aplicar ambos factores a la tasa del proceso de Poisson
    tasa_ventas_dia = 4 * factor_estacional * factor_dia
    n_ventas_dia = np.random.poisson(tasa_ventas_dia)

    for _ in range(n_ventas_dia):
        # Elegir producto aleatorio
        producto = df_productos.sample(1).iloc[0]

        # Aplicar descuento/promoción (20% de las ventas tienen descuento)
        tiene_descuento = np.random.random() < 0.20
        if tiene_descuento:
            descuento_pct = np.random.choice([10, 15, 20, 25])
        else:
            descuento_pct = 0

        # Calcular precio final
        precio_final = producto['precio_base'] * (1 - descuento_pct / 100)

        # Calcular cantidad (1-5 unidades)
        cantidad = np.random.randint(1, 6)

        # Calcular totales
        revenue = precio_final * cantidad
        costo_total = producto['costo'] * cantidad
        margen = revenue - costo_total
        margen_pct = (margen / revenue * 100) if revenue > 0 else 0

        ventas_data.append({
            'fecha': fecha,
            'producto': producto['producto'],
            'categoria': producto['categoria'],
            'cantidad': cantidad,
            'precio_base': producto['precio_base'],
            'descuento_pct': descuento_pct,
            'precio_final': precio_final,
            'costo_unitario': producto['costo'],
            'revenue': revenue,
            'costo_total': costo_total,
            'margen': margen,
            'margen_pct': margen_pct
        })

# Crear DataFrame
df = pd.DataFrame(ventas_data)

# Añadir columnas de tiempo
df['anio'] = df['fecha'].dt.year
df['mes'] = df['fecha'].dt.month
df['mes_nombre'] = df['fecha'].dt.month_name()
df['dia_semana'] = df['fecha'].dt.day_name()
df['trimestre'] = df['fecha'].dt.quarter

# Guardar en el directorio actual de Colab
df.to_csv('sme-ventas.csv', index=False)
df_productos.to_csv('sme-productos.csv', index=False)

print(f"Dataset generado: {len(df):,} ventas")
print(f"Período: {df['fecha'].min()} a {df['fecha'].max()}")
print(f"Productos: {len(df_productos)}")
print(f"Revenue total: ${df['revenue'].sum():,.0f}")
print(f"Margen promedio: {df['margen_pct'].mean():.1f}%")
```

### Opción B: Dataset Real (si tienes acceso)

Si tienes acceso a datos reales de una PyME, úsalos. El dataset debe tener al menos:
- fecha de venta
- producto/categoría
- cantidad vendida
- precio unitario
- costo unitario
- descuento aplicado (si existe)

### Opción C: Dataset de Respaldo (Backup)

Si no puedes generar datos, usa los datasets de respaldo públicos del repositorio.

```python
# Cargar datasets de respaldo
import pandas as pd

ventas_url = 'https://raw.githubusercontent.com/heldigard/Electiva-II-Analitica-IA/master/datos/sme-ventas-backup.csv'
productos_url = 'https://raw.githubusercontent.com/heldigard/Electiva-II-Analitica-IA/master/datos/sme-productos-backup.csv'
df = pd.read_csv(ventas_url)
df_productos = pd.read_csv(productos_url)

print(f"Ventas cargadas: {len(df)} registros")
print(f"Productos cargados: {len(df_productos)} productos")
print(df.head())
```

---

## INSTRUCCIONES PASO A PASO

### Paso 1: Carga y Exploración de Datos (15 minutos)

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Cargar backups reproducibles (o usa los CSV generados en la opción A)
ventas_url = 'https://raw.githubusercontent.com/heldigard/Electiva-II-Analitica-IA/master/datos/sme-ventas-backup.csv'
productos_url = 'https://raw.githubusercontent.com/heldigard/Electiva-II-Analitica-IA/master/datos/sme-productos-backup.csv'
df_ventas = pd.read_csv(ventas_url)
df_productos = pd.read_csv(productos_url)

# Convertir fecha
df_ventas['fecha'] = pd.to_datetime(df_ventas['fecha'])

# Exploración inicial
print("Dataset de ventas:")
print(f"- Registros: {len(df_ventas):,}")
print(f"- Período: {df_ventas['fecha'].min()} a {df_ventas['fecha'].max()}")
print(f"- Columnas: {df_ventas.columns.tolist()}")

print("\nPrimeras filas:")
display(df_ventas.head())

print("\nEstadísticas descriptivas:")
print(df_ventas.describe())

print("\nDataset de productos:")
display(df_productos)
```

**Validaciones:**
- [ ] Dataset cargado correctamente
- [ ] Fechas convertidas
- [ ] No hay valores faltantes críticos

---

### Paso 2: Preparación de Datos (20 minutos)

```python
# Añadir columnas de tiempo si no existen
if 'anio' not in df_ventas.columns:
    df_ventas['anio'] = df_ventas['fecha'].dt.year
    df_ventas['mes'] = df_ventas['fecha'].dt.month
    df_ventas['mes_nombre'] = df_ventas['fecha'].dt.month_name()
    df_ventas['trimestre'] = df_ventas['fecha'].dt.quarter

# Verificar márgenes
print("Márgenes por producto:")
por_producto = df_ventas.groupby('producto').agg({
    'revenue': 'sum',
    'margen': 'sum',
    'margen_pct': 'mean'
}).round(2)
por_producto['margen_total'] = (por_producto['margen'] / por_producto['revenue'] * 100).round(2)
print(por_producto.sort_values('margen_total', ascending=False))

# Verificar
print("\nDataset preparado:")
print(f"- Total revenue: ${df_ventas['revenue'].sum():,.0f}")
print(f"- Total margen: ${df_ventas['margen'].sum():,.0f}")
print(f"- Margen promedio: {df_ventas['margen_pct'].mean():.1f}%")
```

**Validaciones:**
- [ ] Columnas de tiempo creadas
- [ ] Márgenes calculados correctamente
- [ ] Agregaciones por producto funcionando

---

### Paso 3: Análisis y Visualización (90 minutos)

#### Visualización 1: Margen por Categoría de Producto

```python
# Calcular margen por categoría
por_categoria = df_ventas.groupby('categoria').agg({
    'revenue': 'sum',
    'margen': 'sum'
}).round(0)
por_categoria['margen_pct'] = (por_categoria['margen'] / por_categoria['revenue'] * 100).round(1)

# Ordenar por revenue
por_categoria = por_categoria.sort_values('revenue', ascending=True)

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(16, 6))

# Gráfico 1: Revenue por categoría
por_categoria['revenue'].plot(kind='barh', ax=ax1, color='steelblue')
ax1.set_title('Revenue por Categoría')
ax1.set_xlabel('Revenue ($)')
ax1.set_ylabel('Categoría')

# Gráfico 2: Margen % por categoría
colors = ['coral' if x < 13 else 'steelblue' for x in por_categoria['margen_pct']]
por_categoria['margen_pct'].plot(kind='barh', ax=ax2, color=colors)
ax2.set_title('Margen % por Categoría')
ax2.set_xlabel('Margen (%)')
ax2.axvline(x=13, color='red', linestyle='--', label='Umbral mínimo: 13%')
ax2.legend()

plt.tight_layout()
plt.show()

print("\n📊 Análisis por categoría:")
display(por_categoria)
```

**¿Qué significa esto para el negocio?**
- Identifica categorías más y menos rentables
- Puede indicar necesidad de ajuste de precios
- Guía decisiones de mix de productos

---

#### Visualización 2: Análisis Estacional de Ventas

```python
# Agrupar por mes
por_mes = df_ventas.groupby(['anio', 'mes', 'mes_nombre']).agg({
    'revenue': 'sum',
    'margen': 'sum',
    'cantidad': 'sum'
}).reset_index()

# Calcular margen %
por_mes['margen_pct'] = (por_mes['margen'] / por_mes['revenue'] * 100).round(1)

# Crear columna periodo
por_mes['periodo'] = por_mes['anio'].astype(str) + '-' + por_mes['mes'].astype(str).str.zfill(2)

# Gráfico de línea
fig, ax1 = plt.subplots(figsize=(14, 6))

ax1.plot(por_mes['periodo'], por_mes['revenue'], marker='o', linewidth=2, label='Revenue', color='steelblue')
ax1.set_xlabel('Período')
ax1.set_ylabel('Revenue ($)', color='steelblue')
ax1.tick_params(axis='y', labelcolor='steelblue')
ax1.set_xticks(range(0, len(por_mes), 3))
ax1.set_xticklabels(por_mes['periodo'][::3], rotation=45)

ax2 = ax1.twinx()
ax2.plot(por_mes['periodo'], por_mes['margen_pct'], marker='s', linewidth=2, label='Margen %', color='coral')
ax2.set_ylabel('Margen (%)', color='coral')
ax2.tick_params(axis='y', labelcolor='coral')

plt.title('Revenue y Margen % a lo largo del Tiempo')
fig.tight_layout()
plt.show()

print("\n📈 Estacionalidad identificada:")
print("\nMeses con mayor revenue:")
top_months = por_mes.nlargest(3, 'revenue')[['periodo', 'revenue', 'margen_pct']]
print(top_months.to_string(index=False))

print("\nMeses con menor revenue:")
bottom_months = por_mes.nsmallest(3, 'revenue')[['periodo', 'revenue', 'margen_pct']]
print(bottom_months.to_string(index=False))
```

**¿Qué significa esto para el negocio?**
- Identifica meses de alta/baja venta (estacionalidad)
- Permite planificar flujo de caja
- Guía decisiones de stock y promociones

---

#### Visualización 3: Impacto de Descuentos en Revenue

```python
# Comparar ventas con y sin descuento
con_descuento = df_ventas[df_ventas['descuento_pct'] > 0]
sin_descuento = df_ventas[df_ventas['descuento_pct'] == 0]

print("Ventas SIN descuento:")
print(f"- Cantidad: {len(sin_descuento):,} ventas")
print(f"- Revenue total: ${sin_descuento['revenue'].sum():,.0f}")
print(f"- Revenue promedio por venta: ${sin_descuento['revenue'].mean():.0f}")
print(f"- Margen promedio: {sin_descuento['margen_pct'].mean():.1f}%")

print("\nVentas CON descuento:")
print(f"- Cantidad: {len(con_descuento):,} ventas")
print(f"- Revenue total: ${con_descuento['revenue'].sum():,.0f}")
print(f"- Revenue promedio por venta: ${con_descuento['revenue'].mean():.0f}")
print(f"- Descuento promedio: {con_descuento['descuento_pct'].mean():.1f}%")
print(f"- Margen promedio: {con_descuento['margen_pct'].mean():.1f}%")

# Agrupar por nivel de descuento
bins = [0, 5, 10, 15, 20, 25, 100]
labels = ['0%', '1-5%', '6-10%', '11-15%', '16-20%', '21%+']
df_ventas['rango_descuento'] = pd.cut(df_ventas['descuento_pct'], bins=bins, labels=labels, include_lowest=True)

por_descuento = df_ventas.groupby('rango_descuento').agg({
    'revenue': 'sum',
    'margen': 'sum',
    'cantidad': 'sum'
})
por_descuento['margen_pct'] = (por_descuento['margen'] / por_descuento['revenue'] * 100).round(1)
por_descuento['ticket_promedio'] = (por_descuento['revenue'] / por_descuento['cantidad']).round(0)

# Gráfico
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(16, 6))

por_descuento[['revenue']].plot(kind='bar', ax=ax1, color='steelblue')
ax1.set_title('Revenue Total por Nivel de Descuento')
ax1.set_xlabel('Nivel de Descuento')
ax1.set_ylabel('Revenue ($)')
ax1.tick_params(axis='x', rotation=45)

por_descuento[['margen_pct']].plot(kind='bar', ax=ax2, color='coral')
ax2.set_title('Margen % por Nivel de Descuento')
ax2.set_xlabel('Nivel de Descuento')
ax2.set_ylabel('Margen (%)')
ax2.axhline(y=13, color='red', linestyle='--', label='Umbral mínimo: 13%')
ax2.legend()
ax2.tick_params(axis='x', rotation=45)

plt.tight_layout()
plt.show()

print("\n📊 Análisis por rango de descuento:")
display(por_descuento)
```

**¿Qué significa esto para el negocio?**
- Evalúa si los descuentos generan más volumen o solo reducen margen
- Identifica el nivel óptimo de descuento
- Mide el trade-off entre volumen y rentabilidad

---

#### Visualización 4: Matriz de Productos (Revenue vs Margen)

```python
# Calcular métricas por producto
matrix = df_ventas.groupby('producto').agg({
    'revenue': 'sum',
    'margen': 'sum',
    'cantidad': 'sum'
}).round(0)
matrix['margen_pct'] = (matrix['margen'] / matrix['revenue'] * 100).round(1)

# Cuadrantes
revenue_median = matrix['revenue'].median()
margen_median = matrix['margen_pct'].median()

def clasificar_producto(row):
    if row['revenue'] >= revenue_median and row['margen_pct'] >= margen_median:
        return 'Estrella'  # Alto revenue, alto margen
    elif row['revenue'] >= revenue_median and row['margen_pct'] < margen_median:
        return 'Caballo de Batalla'  # Alto revenue, bajo margen
    elif row['revenue'] < revenue_median and row['margen_pct'] >= margen_median:
        return 'Oculta'  # Bajo revenue, alto margen
    else:
        return 'Desempeño'  # Bajo revenue, bajo margen

matrix['cuadrante'] = matrix.apply(clasificar_producto, axis=1)

# Gráfico de dispersión
fig, ax = plt.subplots(figsize=(12, 10))

colors = {'Estrella': 'green', 'Caballo de Batalla': 'blue', 'Oculta': 'orange', 'Desempeño': 'red'}

for cuadrante, color in colors.items():
    subset = matrix[matrix['cuadrante'] == cuadrante]
    ax.scatter(subset['revenue'], subset['margen_pct'], label=cuadrante, s=100, alpha=0.7, color=color)

# Añadir etiquetas
for idx, row in matrix.iterrows():
    ax.annotate(idx, (row['revenue'], row['margen_pct']), fontsize=8, alpha=0.7)

ax.axvline(x=revenue_median, color='gray', linestyle='--', alpha=0.5)
ax.axhline(y=margen_median, color='gray', linestyle='--', alpha=0.5)

ax.set_xlabel('Revenue Total ($)')
ax.set_ylabel('Margen (%)')
ax.set_title('Matriz de Productos: Revenue vs Margen')
ax.legend()

plt.tight_layout()
plt.show()

print("\n📊 Clasificación de productos:")
print(matrix[['cuadrante', 'revenue', 'margen_pct']].sort_values('cuadrante'))
```

**¿Qué significa esto para el negocio?**
- **Estrellas:** Productos ideales, promocionar
- **Caballos de Batalla:** Alto volumen, subir precio o reducir costo
- **Ocultas:** Alto margen, bajo volumen, promocionar más
- **Desempeño:** Considerar discontinuar

---

### Paso 4: Responder Preguntas de Negocio (30 minutos)

#### Pregunta 1: Márgenes por Producto

> **¿Cuáles son los productos más y menos rentables?**

**Tu respuesta:**
```
Top 5 Productos más rentables (mayor margen %):
1. [Producto] - X.X% margen, $X,XXX revenue
2. [Producto] - X.X% margen, $X,XXX revenue
3. [Producto] - X.X% margen, $X,XXX revenue
4. [Producto] - X.X% margen, $X,XXX revenue
5. [Producto] - X.X% margen, $X,XXX revenue

Top 5 Productos menos rentables (menor margen %):
1. [Producto] - X.X% margen, $X,XXX revenue
2. [Producto] - X.X% margen, $X,XXX revenue
3. [Producto] - X.X% margen, $X,XXX revenue
4. [Producto] - X.X% margen, $X,XXX revenue
5. [Producto] - X.X% margen, $X,XXX revenue

INSIGHT: [Qué productos priorizar? Cuáles considerar discontinuar?]
```

---

#### Pregunta 2: Estacionalidad

> **¿Existen patrones estacionales en ventas y flujo de caja?**

**Tu respuesta:**
```
Meses de MAYOR venta:
- [Mes]: $X,XXX revenue
- [Mes]: $X,XXX revenue
- [Mes]: $X,XXX revenue

Meses de MENOR venta:
- [Mes]: $X,XXX revenue
- [Mes]: $X,XXX revenue
- [Mes]: $X,XXX revenue

VARIACIÓN ESTACIONAL:
- Mes pico / Mes valle = X.X veces
- Diferencia de $X,XXX entre mejor y peor mes

IMPACTO EN FLUJO DE CAJA:
[Qué meses hay restricción de liquidez?]
[Cómo planificar pagos a proveedores?]
[Cómo ajustar stock por temporada?]
```

---

#### Pregunta 3: Impacto de Descuentos

> **¿Cuál es el impacto de descuentos y promociones en revenue y margen?**

**Tu respuesta:**
```
Análisis de sensibilidad:

Ventas SIN descuento:
- Revenue promedio: $X,XXX por venta
- Margen: X.X%

Ventas CON descuento:
- Revenue promedio: $X,XXX por venta
- Descuento promedio: X.X%
- Margen: X.X%

IMPACTO:
- Los descuentos [aumentan/reducen] el volumen en X%
- Pero [aumentan/reducen] el margen en X puntos porcentuales
- Balance neto: [positivo/negativo]

RECOMENDACIÓN DE PRICING:
- [Mantener/eliminar/ajustar] política de descuentos
- Nivel óptimo de descuento: X%
- [Qué productos NO deberían tener descuento?]
```

---

#### Pregunta 4: Recomendaciones de Pricing

> **Basado en tu análisis, ¿qué 3 cambios de precios deberían implementarse?**

**Tu respuesta:**
```
Recomendación 1: [Aumentar/Disminuir] precio de [Producto(s)]
- Precio actual: $X
- Precio sugerido: $X (+X%)
- Impacto en margen: +X.X puntos porcentuales
- Impacto en volumen estimado: -X% (elasticidad)
- Impacto neto en revenue: +$X,XXX/mes
- Riesgo: [bajo/medio/alto]

Recomendación 2: [Eliminar/Reducir] descuentos en [Categoría]
- Descuento actual: X%
- Descuento sugerido: X%
- Recuperación de margen: +$X,XXX/mes
- Pérdida de volumen estimada: -X%
- Balance neto: [positivo/negativo]

Recomendación 3: [Crear paquete/bundle] de [Productos]
- Productos incluidos: [lista]
- Precio sugerido del paquete: $X
- vs Precio individual: $X
- Incentivo para cliente: -X%
- Impacto en revenue: +$X,XXX/mes
```

---

### Paso 5: Resumen Ejecutivo (30 minutos)

```markdown
# SME Revenue & Pricing Optimization - Resumen Ejecutivo

**Para:** Dueño, Director Comercial
**De:** Consultor de Analítica
**Fecha:** [Fecha actual]
**Tema:** Análisis de Rentabilidad y Recomendaciones de Pricing

---

## 📊 Contexto

[2-3 frases sobre el análisis realizado: período, datos, alcance]

## 🎯 Hallazgos Clave

### Hallazgo 1: [Título descriptivo sobre márgenes]
[Evidencia numérica específica]
[Interpretación y su impacto financiero]

### Hallazgo 2: [Título descriptivo sobre estacionalidad]
[Evidencia numérica específica]
[Interpretación y su impacto en flujo de caja]

### Hallazgo 3: [Título descriptivo sobre descuentos]
[Evidencia numérica específica]
[Interpretación y su impacto en rentabilidad]

## 💡 Recomendaciones de Pricing

### Recomendación 1: [Cambio de precio específico]
- **Acción:** [Subir/bajar precio de qué producto(s)]
- **Precio actual:** $X
- **Precio nuevo:** $X
- **Evidencia:** [Margen actual X.X%, benchmark X.X%]
- **Impacto financiero:** +$X,XXX/mes revenue, +X.X puntos margen
- **Riesgo:** [Describir riesgo y mitigación]

### Recomendación 2: [Ajuste de descuentos]
- **Acción:** [Eliminar/reducir/aumentar descuentos en qué categoría]
- **Descuento actual:** X%
- **Descuento nuevo:** X%
- **Evidencia:** [Los descuentos reducen margen X.X puntos sin aumentar volumen significativamente]
- **Impacto financiero:** +$X,XXX/mes
- **Riesgo:** [Describir]

### Recomendación 3: [Acción estacional]
- **Acción:** [Qué hacer en meses de baja/alta venta]
- **Evidencia:** [Valle: $X en mes X, Pico: $X en mes Y]
- **Impacto financiero:** [Mejorar flujo de caja en meses críticos]
- **Timeline:** [Implementar antes de mes X]

## 📈 Proyección de Impacto

| Métrica | Actual | Post-Implementación | Variación |
|---------|--------|---------------------|-----------|
| Revenue mensual | $X,XXX | $X,XXX | +X% |
| Margen promedio | X.X% | X.X% | +X.X pp |
| Margen total | $X,XXX | $X,XXX | +X% |
| Mes valle (flujo caja) | $X | $X | +X% |

## 🚀 Plan de Implementación

### Mes 1
1. [Acción concreta]
2. [Acción concreta]

### Mes 2-3
1. [Acción concreta]
2. [Acción concreta]

### Monitoreo
Métricas a revisar mensualmente:
- Revenue por producto
- Margen por categoría
- Volumen de ventas por mes
- Impacto de cambios de precio

---

**Conclusión:** [1-2 frases sobre el impacto esperado en la rentabilidad de la PyME]
```

---

## RÚBRICA DE EVALUACIÓN

| Criterio | Peso | Excelente (100%) | Bueno (75%) | Aceptable (50%) | Insuficiente (0%) |
|----------|------|------------------|-------------|-----------------|-------------------|
| **1. Carga y extracción** | 15% | Datasets generados o cargados desde fuentes reproducibles y validados | Datasets cargados y validados de forma básica | Datasets cargados con validación mínima | No logra cargar los datos |
| **2. Limpieza y transformación** | 20% | Tipos, fechas, márgenes y variables derivadas tratados y justificados | Preparación adecuada con justificación parcial | Preparación básica con problemas menores | No realiza preparación significativa |
| **3. Análisis y visualización** | 20% | 4+ gráficos con interpretación financiera detallada | 4 gráficos con interpretación aceptable | 3-4 gráficos con interpretación mínima | Menos de 3 gráficos |
| **4. Respuestas de negocio** | 25% | Preguntas respondidas y recomendaciones de pricing con evidencia, impacto cuantificado y riesgo | Respuestas y recomendaciones con evidencia adecuada | Respuestas o recomendaciones parciales | Menos de 3 respuestas o sin recomendaciones |
| **5. Resumen Ejecutivo** | 20% | Resumen profesional con hallazgos + recomendaciones + proyección financiera | Resumen con hallazgos + recomendaciones | Resumen incompleto | Sin resumen ejecutivo |

**Bonus (hasta +10 puntos, sin superar 100):** Incluye cálculo de elasticidad de precio (cuánto cambia la demanda con cada 1% de cambio en precio).

---

## RECURSOS Y AYUDA

### Chatbots de IA para Ayuda

Si te quedas atascado, puedes usar:
- **Qwen** (Qwen 3.8; acceso según plan y región)
- ✨ **Gemini en Colab** (integrado en tu notebook, panel ✨)
- **ChatGPT** (GPT-5.6)
- **Claude** (Sonnet 5)
- **Kimi** (K3, cuota gratuita generosa)
- **Julius AI** (para análisis de datos conversacional)

**Ejemplo de prompt:**
```
"Estoy analizando datos de ventas de una PyME.
Tengo un DataFrame con columnas: 'revenue', 'margen_pct', 'descuento_pct'.
Quiero calcular el impacto de los descuentos en el margen:
comparar margen promedio de ventas con descuento vs sin descuento.
¿Cómo genero el código para hacer este análisis en pandas?"
```

---

## TIPS PARA EL ÉXITO

### ✅ Buenas Prácticas

1. **Enfócate en el dinero:** Siempre traduce insights a impacto financiero
2. **Considera elasticidad:** Un precio más alto puede reducir volumen pero aumentar revenue total
3. **Piensa en flujo de caja:** La estacionalidad afecta liquidez, no solo profit
4. **Prioriza por impacto:** No todos los cambios de precio son igualmente valiosos
5. **Cuantifica riesgos:** Siempre incluye qué podría salir mal

### ❌ Errores Comunes

1. **Recomendar subir todo:** No todos los productos pueden aumentar precio
2. **Ignorar competencia:** El mercado limita cuánto puedes subir
3. **Olvidar el volumen:** Subir precio puede reducir margen total si cae volumen mucho
4. **No considerar promociones:** A veces un descuento estratégico genera más clientes leales
5. **Olvidar el costo de oportunidad:** Tiempo y esfuerzo de cambiar precios también tiene costo

---

## CHECKLIST DE ENTREGA

Antes de entregar, verifica:

- [ ] Notebook con todas las celdas ejecutadas (sin errores)
- [ ] Dataset de ventas generado/cargado y limpio
- [ ] Mínimo 4 gráficos con títulos y etiquetas
- [ ] 4 preguntas de negocio respondidas con evidencia numérica
- [ ] 3 recomendaciones de pricing con impacto financiero cuantificado
- [ ] Resumen ejecutivo con proyección de impacto
- [ ] Interpretación de "qué significa esto para el negocio" en cada sección
- [ ] Notebook organizado con secciones claras (usa celdas markdown)

---

**¡Buena suerte! 💰📊**

Recuerda: El objetivo no es escribir código perfecto. El objetivo es **extraer insights financieros** que permitan a la PyME mejorar su rentabilidad sin un equipo de datos dedicado.

La IA es tu herramienta para escribir código. **TÚ eres el consultor que interpreta los datos y recomienda estrategias de pricing.**

---

*Universidad UdeColombia - Especialización en Analítica de Datos - 2026*
