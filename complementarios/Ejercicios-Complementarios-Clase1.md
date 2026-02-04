# Ejercicios Complementarios - Clase 1: Exploración Inicial

**Propósito:** Estos ejercicios refuerzan los conceptos aprendidos en la Clase 1 y te ayudan a consolidar habilidades antes de avanzar a la Clase 2.

---

## 💡 Tips Rápidos para Recordar

### **Tip #1: La Mentalidad Correcta**
```
❌ NO: "Necesito memorizar código Python"
✅ SÍ: "Necesito aprender a formular buenas preguntas a la IA"
```

**Explicación:** En este curso, tu habilidad más valiosa es saber **qué preguntarle a la IA**, no memorizar sintaxis.

---

### **Tip #2: El Ciclo de Análisis**
```
1. Formular pregunta de negocio
2. Traducir a prompt para la IA
3. Ejecutar el código generado
4. Interpretar resultados
5. Hacer nueva pregunta (volver al paso 1)
```

**Explicación:** El análisis de datos es **iterativo**. Cada respuesta genera nuevas preguntas.

---

### **Tip #3: Verificación de Sanidad (Sanity Check)**
Siempre que obtengas un resultado, pregúntate:
- ¿Tiene sentido este número?
- ¿El orden de magnitud es correcto?
- ¿Hay algo extraño en los datos?

**Ejemplo:** Si obtienes que el promedio de gravedad es 2.63, verifica: ¿Qué significa el código 2? (R: Con Heridos) → ¡Tiene sentido!

---

## 📝 Ejercicios de Práctica (5-10 min cada uno)

### **Ejercicio 1: Conteo de Códigos**

**Objetivo:** Practicar el uso de `value_counts()` para entender distribuciones.

**Pregunta:** ¿Cuántos siniestros de cada tipo de gravedad hay en el dataset?

**Prompt sugerido para la IA:**
> Usando el DataFrame df_siniestros, genera el código para contar cuántos registros hay de cada valor único en la columna GRAVEDAD. Usa el método value_counts().

**Resultado esperado:**
```
2    106764  # Con Heridos
3     72147  # Solo Daños
1     17241  # Con Muertos
```

**Verificación de comprensión:**
- ✅ ¿Puedes interpretar qué significa cada número?
- ✅ ¿Qué tipo de gravedad es el más común?

---

### **Ejercicio 2: Filtrado de Datos**

**Objetivo:** Aprender a filtrar el DataFrame para analizar subconjuntos específicos.

**Pregunta:** ¿Cuántos siniestros con muertos (GRAVEDAD = 1) ocurrieron?

**Prompt sugerido para la IA:**
> Usando el DataFrame df_siniestros, genera el código para filtrar y mostrar únicamente las filas donde la columna GRAVEDAD sea igual a 1. Luego, cuenta cuántas filas hay.

**Resultado esperado:**
```
17,241 siniestros con muertos
```

**Verificación de comprensión:**
- ✅ ¿Usaste el operador `==` (igual) y no `=` (asignación)?
- ✅ ¿Entiendes la diferencia entre filtrar y contar?

---

### **Ejercicio 3: Análisis Temporal Básico**

**Objetivo:** Extraer información de la columna datetime.

**Pregunta:** ¿En qué año ocurrieron más siniestros?

**Prompt sugerido para la IA:**
> Usando el DataFrame df_siniestros que tiene una columna FECHA_HORA de tipo datetime, genera el código para:
> 1. Extraer el año de la columna FECHA_HORA en una nueva columna llamada ANIO
> 2. Contar cuántos siniestros hubo por año
> 3. Mostrar el resultado ordenado de mayor a menor

**Resultado esperado:**
```python
# Año con más siniestros
2019: [cantidad más alta]
2018: [segunda más alta]
...
```

**Verificación de comprensión:**
- ✅ ¿Usaste `.dt.year` para extraer el año?
- ✅ ¿El resultado tiene sentido con los datos?

---

### **Ejercicio 4: Diccionario de Datos**

**Objetivo:** Practicar el uso del diccionario para descifrar códigos.

**Pregunta:** ¿Qué significa el código 1 en la columna DISENO_LUGAR?

**Prompt sugerido para la IA:**
> Usando el DataFrame df_diccionario, genera el código para filtrar y mostrar las filas donde:
> - La columna CAMPO sea 'DISENO_LUGAR'
> - La columna CODIGO sea 1

**Resultado esperado:**
```
HOJA        CAMPO         CODIGO  DESCRIPCION
SINIESTROS  DISENO_LUGAR  1       Intersección
```

**Verificación de comprensión:**
- ✅ ¿Usaste el operador `&` para combinar dos condiciones?
- ✅ ¿Entiendes por qué es importante el diccionario?

---

### **Ejercicio 5: Estadística Descriptiva Selectiva**

**Objetivo:** Aprender a calcular estadísticas solo para columnas relevantes.

**Pregunta:** ¿Cuál es la distribución de la columna CLASE?

**Prompt sugerido para la IA:**
> Usando el DataFrame df_siniestros, genera el código para mostrar:
> 1. El conteo de valores únicos de la columna CLASE
> 2. El porcentaje que representa cada valor sobre el total

**Resultado esperado:**
```
1    Choque         143,XXX  (73%)
2    Atropello       25,XXX  (13%)
3    Volcamiento     17,XXX  (9%)
...
```

**Verificación de comprensión:**
- ✅ ¿Qué tipo de siniestro es el más común?
- ✅ ¿Usaste `normalize=True` para obtener porcentajes?

---

## 🔍 Verificaciones de Comprensión

### **Verificación 1: Tipos de Datos en Pandas**

**Pregunta:** ¿Por qué es importante que la columna FECHA_HORA sea de tipo `datetime` y no `object` (texto)?

**Respuesta correcta:**
```
Porque como datetime, Python puede:
- Extraer año, mes, día, hora fácilmente con .dt.year, .dt.month, etc.
- Realizar operaciones matemáticas con fechas (ej: diferencia entre dos fechas)
- Ordenar cronológicamente de forma correcta
- Crear gráficos de series temporales

Como texto (object), Python solo ve cadenas de caracteres y no puede hacer nada de esto.
```

---

### **Verificación 2: Valores Nulos (NaN)**

**Pregunta:** ¿Qué significa un valor `NaN` en la columna CHOQUE y por qué hay tantos?

**Respuesta correcta:**
```
NaN significa "Not a Number" (No es un Número), y en este contexto significa "No aplica" o "dato faltante".

Hay muchos NaN en CHOQUE porque esta columna solo se llena cuando el siniestro ES un choque
entre vehículos. Si el siniestro fue un atropello o volcamiento, el campo CHOQUE no aplica,
por lo tanto queda vacío (NaN).

No es un error de los datos, es una característica: NO todos los siniestros son choques.
```

---

### **Verificación 3: Categorías vs Números**

**Pregunta:** ¿Por qué convertimos la columna CHOQUE de `float64` a `category`?

**Respuesta correcta:**
```
Porque CHOQUE no es una cantidad numérica (como precio o peso), es una CATEGORÍA.

Dejarla como float (número) es peligroso porque pandas podría:
- Calcular el "promedio" de CHOQUE (¡no tiene sentido!)
- Ordenar CHOQUE de menor a mayor (¿1 es "menor" que 4? No, son categorías)

Como category, pandas entiende que son grupos limitados y:
- Usa menos memoria
- Evita operaciones matemáticas incorrectas
- Permite análisis correctos (agrupaciones, conteos)
```

---

## 🚨 Errores Comunes y Cómo Evitarlos

### **Error #1: Confundir `=` con `==`**

```python
❌ INCORRECTO:
df[df['GRAVEDAD'] = 1]  # Esto es asignación, no comparación

✅ CORRECTO:
df[df['GRAVEDAD'] == 1]  # Esto es comparación
```

**Recuerda:** Un `=` asigna valor, dos `==` comparan valores.

---

### **Error #2: Olvidar el argumento `inplace=True`**

```python
❌ INCORRECTO:
df.drop(columns['DIRECCION'])  # No modifica el DataFrame original

✅ CORRECTO:
df.drop(columns=['DIRECCION'], inplace=True)  # Sí modifica el original
```

**Alternativa sin inplace:**
```python
df = df.drop(columns=['DIRECCION'])  # Reasigna el resultado
```

---

### **Error #3: No verificar el resultado de la IA**

```python
❌ RIESGOSO:
# Ejecutar código de la IA sin entenderlo
df_siniestros.groupby('CLASE').mean()

✅ MEJOR:
# Primero entender qué hace el código, luego ejecutar
# ¿.groupby('CLASE') agrupa por tipo de siniestro
# ¿.mean() calcula promedio de todas las columnas numéricas
# ¿Tiene sentido calcular promedio de CODIGO_ACCIDENTE? NO!
```

**Lección:** La IA puede generar código que se ejecuta pero que metodológicamente no tiene sentido. Tú eres el filtro crítico.

---

## 📚 Recursos Adicionales

- **Documentación de Pandas:** https://pandas.pydata.org/docs/
- **Tutorial de value_counts:** https://pandas.pydata.org/docs/reference/api/pandas.Series.value_counts.html
- **Tipos de datos en Pandas:** https://pandas.pydata.org/docs/user_guide/basics.html#dtypes

---

## ✅ Checklist Antes de Avanzar a la Clase 2

Antes de pasar a la Clase 2 (Análisis y Visualización), asegúrate de que puedes:

- [ ] Cargar un archivo Excel en un DataFrame
- [ ] Usar `.head()`, `.info()`, y `.describe()` para explorar datos
- [ ] Filtrar datos con condiciones (`df[df['columna'] == valor]`)
- [ ] Contar valores únicos con `.value_counts()`
- [ ] Entender la diferencia entre tipos de datos (int64, float64, object, category, datetime)
- [ ] Usar un diccionario de datos para descifrar códigos
- [ ] Crear una nueva columna a partir de existentes (ingeniería de características)
- [ ] Interpretar resultados en contexto de negocio

**Si completaste todos los ejercicios y entendiste las verificaciones, ¡estás listo para la Clase 2!** 🎉
