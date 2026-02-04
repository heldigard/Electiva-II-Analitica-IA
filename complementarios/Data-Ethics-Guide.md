# Data Ethics & Privacy Guide
## Guía de Ética y Privacidad para Analítica con IA

### Universidad UdeColombia - Electiva Analítica de Datos con IA

---

## ¿Qué es esta Guía?

Esta guía te enseña **qué datos puedes y NO puedes compartir** con herramientas de IA gratuitas (ChatGPT, Claude, Qwen, Gemini, etc.), y cómo **proteger la privacidad** cuando usas IA para análisis de datos.

---

## Regla de Oro

> **"NUNCA compartas datos sensibles en chatbots gratuitos"**
>
> Las versiones gratuitas de chatbots generalmente **usan tus datos para entrenar**. Lo que pegues puede ser visto por otros usuarios en el futuro.

---

## La Regla del 80/20

```
80% de los datos que necesitas analizar son COMPARTIBLES
20% de los datos son SENSIBLES y requieren protección
```

Tu trabajo es identificar el 20% y protegerlo.

---

## Qué Datos Son SENSIBLES

### Información Personal Identificable (PII)

❌ **NUNCA compartas esto en chatbots gratuitos:**

| Tipo | Ejemplos |
|------|----------|
| **Nombres completos** | "Juan Pablo García Rodríguez" |
| **Documentos de identidad** | Cédulas, pasaportes, RUT, DNI |
| **Direcciones exactas** | "Calle 123 #45-67, Bogotá" |
| **Teléfonos** | "+57 300 123 4567" |
| **Emails** | "nombre@gmail.com" |
| **Tarjetas de crédito** | "4532-1234-5678-9010" |
| **Datos biométricos** | Huellas, reconocimiento facial |
| **Salud detallada** | "Diagnóstico: cáncer, tratamiento X" |

### Información Comercial Sensible

❌ **NUNCA compartas esto en chatbots gratuitos:**

| Tipo | Ejemplos |
|------|----------|
| **Información financiera detallada** | Estados de cuenta completos |
| **Secretos de negocio** | Fórmulas, algoritmos propietarios |
| **Estrategia confidencial** | Planes de lanzamiento no públicos |
| **Datos de empleados** | Salarios, evaluaciones de desempeño |
| **Contratos completos** | Con términos y condiciones |
| **Negociaciones en curso** | Ofertas de compra/venta |
| **Información de proveedores** | Precios de costo confidenciales |

---

## Qué Datos Son COMPARTIBLES

✅ **Generalmente seguro compartir en chatbots gratuitos:**

| Tipo | Ejemplos |
|------|----------|
| **Datos agregados/anónimos** | Promedios, totales, conteos |
| **Datos de productos** | Catálogo de productos, precios públicos |
| **Datos demográficos generales** | "25-35 años", "Nivel socioeconómico medio" |
| **Métricas de negocio** | Revenue total, margen promedio |
| **Categorías de productos** | "Electrónica", "Ropa", "Alimentos" |
| **Datos de tiempo** | Fechas, horas, días de la semana |
| **Ratings y reseñas** | Calificación 1-5, comentarios públicos |
| **Ubicación geográfica general** | "Bogotá", "Medellín", "Norte de Cali" |

---

## Cómo Anonimizar Datos

### Técnica 1: Eliminar Directamente

```python
# ANTES (con PII)
df = pd.DataFrame({
    'nombre': ['Juan García', 'María Rodríguez', 'Pedro López'],
    'email': ['juan@gmail.com', 'maria@hotmail.com', 'pedro@yahoo.com'],
    'edad': [25, 30, 35],
    'satisfaccion': [4, 5, 3]
})

# DESPUÉS (sin PII)
df_limpio = df[['edad', 'satisfaccion']].copy()

# Ahora es seguro compartir con IA
```

### Técnica 2: Reemplazar con Categorías

```python
# ANTES (direcciones exactas)
df['direccion'] = ['Calle 123 #45-67', 'Cra 7 #12-34', 'Av 5 #67-89']

# DESPUÉS (zonas generales)
df['zona'] = ['Norte', 'Centro', 'Sur']
df = df.drop('direccion', axis=1)

# Ahora es seguro compartir
```

### Técnica 3: Reemplazar con IDs

```python
# ANTES (nombres completos)
df['cliente'] = ['Juan Pablo García', 'María Isabel Rodríguez', 'Pedro José López']

# DESPUÉS (IDs anónimos)
df['cliente_id'] = ['C001', 'C002', 'C003']
df = df.drop('cliente', axis=1)

# Ahora es seguro compartir
```

### Técnica 4: Agregar

```python
# ANTES (datos individuales - COMPROMETIDO)
df_ventas = pd.DataFrame({
    'vendedor': ['Juan', 'María', 'Pedro', 'Juan', 'María'],
    'ventas': [100, 150, 200, 120, 180]
})

# DESPUÉS (agregado - SEGURO)
ventas_por_vendedor = df_ventas.groupby('vendedor')['ventas'].sum().reset_index()
# Solo tienes 3 filas en lugar de 5, menos granularidad

# AÚN MÁS SEGURO - total general
total_ventas = df_ventas['ventas'].sum()
# Solo un número, imposible de rastrear a individuos
```

### Técnica 5: Redacción de Texto

```python
# ANTES (texto con información sensible)
texto = "El cliente Juan García (juan@gmail.com) reportó un problema con su pedido #12345"

# DESPUÉS (texto anonimizado)
texto_limpio = "El cliente reportó un problema con su pedido"

# O con placeholders
texto_anonimo = "El cliente [NOMBRE] ([EMAIL]) reportó un problema con su pedido #[ID]"

# Usa el primero para compartir con IA
```

---

## Plantilla de Redacción (Copy-Paste)

Usa esta plantilla para preparar datos antes de compartir con IA:

```python
import pandas as pd

def anonimizar_dataframe(df):
    """
    Anonimiza un DataFrame removiendo información sensible.
    Ajusta según tus datos específicos.
    """
    df_anonimo = df.copy()

    # 1. Remover columnas con PII (Personal Identifiable Information)
    columnas_pii = ['nombre', 'apellido', 'email', 'telefono', 'direccion',
                    'cedula', 'dni', 'rut', 'pasaporte', 'id_personal']
    for col in columnas_pii:
        if col in df_anonimo.columns:
            df_anonimo = df_anonimo.drop(col, axis=1)

    # 2. Reemplazar identificadores con IDs genéricos
    if 'cliente' in df_anonimo.columns:
        df_anonimo['cliente_id'] = ['C' + str(i).zfill(4) for i in range(len(df_anonimo))]
        df_anonimo = df_anonimo.drop('cliente', axis=1)

    # 3. Reducir precisión de fechas si es necesario (mes en lugar de día)
    if 'fecha_nacimiento' in df_anonimo.columns:
        df_anonimo['fecha_nacimiento'] = df_anonimo['fecha_nacimiento'].dt.to_period('M')

    # 4. Remover columnas de texto libre que pueden contener PII
    columnas_texto = ['comentarios', 'notas', 'descripcion_detallada']
    for col in columnas_texto:
        if col in df_anonimo.columns:
            df_anonimo = df_anonimo.drop(col, axis=1)

    return df_anonimo

# Uso
df_limpio = anonimizar_dataframe(df_original)
print(f"Original: {df_original.shape}")
print(f"Anonimizado: {df_limpio.shape}")

# Ahora df_limpio es más seguro para compartir con IA
```

---

## Decision Tree: ¿Puedo Compartir Este Dato?

```
¿Es información personal (nombres, emails, teléfonos)?
├─ Sí → ¿Está anonimizada/agregada?
│        ├─ Sí → ¿Se puede identificar a la persona de todas formas?
│        │        ├─ Sí → NO COMPARTIR
│        │        └─ No → COMPARTIR CON PRECAUCIÓN
│        └─ No → NO COMPARTIR
└─ No → ¿Es información financiera detallada (salarios, cuentas)?
         ├─ Sí → ¿Está agregada (promedios, totales)?
         │        ├─ Sí → COMPARTIR
         │        └─ No → NO COMPARTIR
         └─ No → ¿Es información confidencial de negocio?
                  ├─ Sí → NO COMPARTIR (usar versión de pago con privacidad garantizada)
                  └─ No → COMPARTIR
```

---

## Ejemplos Prácticos

### Ejemplo 1: Dataset de Ventas

❌ **NO compartir:**
```python
df = pd.DataFrame({
    'vendedor': ['Juan García', 'María Rodríguez'],
    'email': ['juan@empresa.com', 'maria@empresa.com'],
    'telefono': ['3001234567', '3009876543'],
    'ventas': [15000000, 22000000],
    'salario': [3500000, 4200000]
})
```

✅ **SÍ compartir:**
```python
df_seguro = pd.DataFrame({
    'vendedor_id': ['V001', 'V002'],
    'ventas': [15000000, 22000000],
    'region': ['Norte', 'Centro']
})
```

### Ejemplo 2: Dataset de Soporte

❌ **NO compartir:**
```python
df = pd.DataFrame({
    'cliente': ['TechSolutions SAS', 'Logística Express Ltda'],
    'contacto': ['Carlos (carlos@tech.com)', 'Ana (ana@logistica.com)'],
    'problema': ['No puedo acceder a mi cuenta con usuario carlos2024', 'Error al facturar al tarjeta de crédito 4532-1234'],
    'contrato': ['Contrato Premium $50M/año', 'Contrato Enterprise $120M/año']
})
```

✅ **SÍ compartir:**
```python
df_seguro = pd.DataFrame({
    'categoria': ['Authentication', 'Billing'],
    'prioridad': ['High', 'Medium'],
    'tiempo_resolucion_horas': [48, 24],
    'satisfaccion': [3, 4]
})
```

### Ejemplo 3: Dataset de Empleados

❌ **NUNCA compartir:**
```python
df = pd.DataFrame({
    'empleado': ['Juan Pérez', 'María González'],
    'cargo': ['Desarrollador Senior', 'Gerente de Producto'],
    'salario': [8500000, 15000000],
    'evaluacion': ['Tiene problemas de rendimiento', 'Excelente desempeño'],
    'edad': [28, 35]
})
```

✅ **Alternativa segura:**
```python
# Solo compartir métricas agregadas
metricas = {
    'salario_promedio': 11750000,
    'salario_min': 8500000,
    'salario_max': 15000000,
    'conteo_empleados': 2
}

# O usar herramienta interna corporativa con garantías de privacidad
```

---

## Herramientas y Alternativas

### Para Datos Sensibles

Si necesitas analizar datos sensibles con IA:

1. **Versiones de pago con privacidad garantizada**
   - ChatGPT Team/Enterprise (no usa datos para entrenamiento)
   - Claude Pro (opciones de privacidad mejoradas)
   - Herramientas corporativas internas

2. **Herramientas de IA local**
   - Instalar modelos localmente (Ollama, LM Studio)
   - Los datos nunca salen de tu computadora

3. **Agregación manual**
   - Calcular métricas agregadas tú mismo
   - Solo compartir los resultados agregados con IA

4. **Sintetización de datos**
   - Crear dataset sintético con las mismas características estadísticas
   - Analizar el sintético, no el real

### Para Validar Anonimización

Herramientas para verificar que los datos están correctamente anonimizados:

```python
# Verificar que no hay PII obvia
def verificar_anonimizacion(df):
    """Verifica que un DataFrame no tenga PII obvia."""

    # 1. Verificar que no hay columnas con nombres sospechosos
    columnas_peligrosas = ['nombre', 'email', 'telefono', 'direccion', 'cedula',
                           'dni', 'rut', 'salario', 'documento', 'identificacion']

    encontradas = [col for col in df.columns if any(peligro in col.lower() for peligro in columnas_peligrosas)]

    if encontradas:
        print(f"⚠️ ADVERTENCIA: Columnas sospechosas encontradas: {encontradas}")
        return False

    # 2. Verificar que no hay valores que parezcan emails
    for col in df.select_dtypes(include=['object']).columns:
        if df[col].astype(str).str.contains('@').any():
            print(f"⚠️ ADVERTENCIA: La columna '{col}' puede contener emails")
            return False

    # 3. Verificar que no hay valores que parezcan teléfonos
    for col in df.select_dtypes(include=['object']).columns:
        if df[col].astype(str).str.contains(r'\d{10,}').any():
            print(f"⚠️ ADVERTENCIA: La columna '{col}' puede contener números largos (teléfonos/documentos)")
            return False

    print("✅ Verificación básica pasada")
    return True

# Uso
verificar_anonimizacion(df_limpio)
```

---

## Casos Especiales

### Caso 1: Datos de Salud

❌ **NUNCA compartas:**
- Diagnósticos específicos con identificadores
- Historias clínicas completas
- Imágenes médicas con metadatos

✅ **Puedes compartir:**
- Estadísticas agregadas (casos totales, promedios)
- Categorías generales (enfermedad X: 100 casos)
- Datos públicos (estadísticas gubernamentales)

### Caso 2: Datos de Menores de Edad

⚠️ **EXTRA CAuteloso:**
- Los datos de menores tienen protección legal adicional
- Generalmente NO compartir nunca sin consentimiento explícito
- Consultar políticas institucionales específicas

### Caso 3: Datos Financieros Regulados

⚠️ **Verificar regulaciones:**
- Algunos datos financieros tienen requisitos legales específicos
- Verificar normativas locales (Ley de Protección de Datos, etc.)
- Consultar con legal/compliance si tienes dudas

---

## Checklist Antes de Compartir

```
Antes de pegar datos en un chatbot gratuito:

☐ 1. No hay nombres completos
☐ 2. No hay emails, teléfonos, direcciones
☐ 3. No hay documentos de identidad
☐ 4. No hay información financiera detallada
☐ 5. No hay secretos de negocio
☐ 6. Los datos están agregados o anonimizados
☐ 7. No hay texto libre con información personal
☐ 8. Los identificadores son genéricos (ID001, no nombres)
☐ 9. Se podría razonablemente reconstruir la identidad de alguien?

SI RESPONDISTES SÍ A #9 → NO COMPARTIR
SI TODO BIEN → COMPARTIR CON PRECAUCIÓN
```

---

## Qué Hacer si Cometes un Error

Si accidentalmente compartiste datos sensibles:

1. **Inmediatamente** borra el chat/historial
2. **Contacta** al soporte del chatbot para solicitar eliminación de datos
3. **Documenta** qué fue compartido (para mitigación)
4. **Notifica** a quien corresponda (legal, compliance, seguridad)
5. **Revisa** políticas de privacidad de la herramienta específica

---

## Recursos Adicionales

- **AI-Reliability-Checklist.md** - Cómo verificar respuestas de IA
- **Business-Question-Canvas.md** - Cómo hacer buenas preguntas
- **Prompt-Templates-2026.md** - Prompts efectivos y seguros

---

## Puntos Clave

1. **NUNCA compartas PII en chatbots gratuitos**
2. **Anonimiza antes de compartir**: elimina o reemplaza
3. **Agrega cuando puedas**: promedios, totales, conteos
4. **UsaIDs genéricos**: C001 en lugar de nombres
5. **Si tienes dudas**: NO COMPARTIR o usa herramienta de pago

---

**Recuerda:** Una vez que compartes datos en un chatbot gratuito, pierdes el control sobre ellos. Piensa dos veces antes de pegar.

*Universidad UdeColombia - Especialización en Analítica de Datos - 2026*
