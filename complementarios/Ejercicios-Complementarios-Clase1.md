# Ejercicios Complementarios - Clase 1: Exploración Inicial

**Actualizado para:** Agosto 2026
**Dataset:** el mismo de la Clase 1 — catálogo de Netflix (6,234 títulos), cargado como `df = pd.read_csv(url)`.

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

**Ejemplo:** Si la IA te dice que el año promedio de estreno (`release_year`) es **2013.4**, verifica: ¿tiene sentido? El catálogo se concentra en contenido reciente (2015-2020) con una cola larga de clásicos desde 1925 → ¡Tiene sentido!

---

## 📝 Ejercicios de Práctica (5-10 min cada uno)

### **Ejercicio 1: Conteo de Categorías**

**Objetivo:** Practicar el uso de `value_counts()` para entender distribuciones.

**Pregunta:** ¿Cuántos títulos del catálogo son películas y cuántos son series de TV?

**Prompt sugerido para la IA:**
> Usando el DataFrame df, genera el código para contar cuántos registros hay de cada valor único en la columna type. Usa el método value_counts().

**Resultado esperado:**
```
Movie      4265   # 68% del catálogo
TV Show    1969   # 32% del catálogo
```

**Verificación de comprensión:**
- ✅ ¿Puedes interpretar qué significa cada número?
- ✅ ¿Qué tipo de contenido domina el catálogo? (R: las películas, con cerca de 2 de cada 3 títulos)

---

### **Ejercicio 2: Filtrado de Datos**

**Objetivo:** Aprender a filtrar el DataFrame para analizar subconjuntos específicos.

**Pregunta:** ¿Cuántos títulos tienen rating **TV-MA** (contenido solo para adultos)?

**Prompt sugerido para la IA:**
> Usando el DataFrame df, genera el código para filtrar y mostrar únicamente las filas donde la columna rating sea igual a 'TV-MA'. Luego, cuenta cuántas filas hay.

**Resultado esperado:**
```
2,027 títulos con rating TV-MA
```

**Verificación de comprensión:**
- ✅ ¿Usaste el operador `==` (igual) y no `=` (asignación)?
- ✅ ¿Notaste que `'TV-MA'` va entre comillas porque es texto? (Un número como `2020` no las necesita)
- ✅ ¿Entiendes la diferencia entre filtrar y contar?

---

### **Ejercicio 3: Análisis Temporal Básico**

**Objetivo:** Convertir texto a fecha y extraer información temporal.

**Pregunta:** ¿En qué año se agregaron más títulos al catálogo de Netflix?

**Prompt sugerido para la IA:**
> Usando el DataFrame df, la columna date_added viene como texto (ej: "September 25, 2021"). Genera el código para:
> 1. Convertir date_added a tipo datetime con pd.to_datetime()
> 2. Extraer el año en una nueva columna llamada anio_agregado
> 3. Contar cuántos títulos se agregaron por año, ordenado de mayor a menor

**Resultado esperado:**
```
2019: 2,057 títulos  # año pico de adiciones
2018: 1,629
2017: 1,184
...
```

**Verificación de comprensión:**
- ✅ ¿Convertiste la columna a `datetime` ANTES de extraer el año? (con texto plano `.dt.year` falla)
- ✅ ¿Usaste `.dt.year` para extraer el año?
- ✅ ¿El resultado tiene sentido? (Netflix expandió agresivamente su catálogo hasta 2019)

---

### **Ejercicio 4: Descifrar Códigos (Ratings)**

**Objetivo:** Interpretar códigos de clasificación y crear una columna nueva con su significado.

**Pregunta:** ¿Qué significa cada código de la columna `rating`?

**Prompt sugerido para la IA:**
> La columna rating del DataFrame df tiene códigos como 'TV-MA', 'TV-14', 'TV-PG', 'R', 'PG-13'. Primero explícame qué significa cada uno. Luego genera el código para crear una columna nueva llamada rating_descripcion que traduzca cada código a su significado usando un diccionario y el método .map().

**Resultado esperado:**
```
rating      rating_descripcion
TV-MA       Solo adultos (17+)
TV-14       Mayores de 14 años
TV-PG       Guía parental sugerida
R           Restringido (menores acompañados)
PG-13       Mayores de 13 años
...
```

**Verificación de comprensión:**
- ✅ ¿Entiendes por qué es importante descifrar los códigos ANTES de analizar?
- ✅ Después del `.map()`, ¿verificaste si quedaron valores `NaN`? (Pista: unos 10 títulos no tienen rating y quedarán vacíos — ¡buen sanity check!)

---

### **Ejercicio 5: Estadística Descriptiva Selectiva**

**Objetivo:** Calcular distribuciones porcentuales solo para un subconjunto relevante.

**Pregunta:** Dentro de las **películas** (no series), ¿qué porcentaje tiene cada rating?

**Prompt sugerido para la IA:**
> Usando el DataFrame df, genera el código para:
> 1. Filtrar solo las filas donde type sea 'Movie'
> 2. Sobre ese subconjunto, mostrar el porcentaje que representa cada valor de rating sobre el total
> Pista: usa value_counts() con el argumento normalize=True

**Resultado esperado:**
```
TV-MA     31.6%
TV-14     24.3%
R         11.9%
TV-PG     10.1%
...
```

**Verificación de comprensión:**
- ✅ ¿Filtraste PRIMERO por tipo y calculaste porcentajes DESPUÉS?
- ✅ ¿Usaste `normalize=True` para obtener porcentajes?
- ✅ ¿Qué le dirías a un directivo? (R: más de la mitad de las películas del catálogo son para público adolescente-adulto)

---

## 🔍 Verificaciones de Comprensión

### **Verificación 1: Tipos de Datos en Pandas**

**Pregunta:** ¿Por qué es importante convertir la columna `date_added` de `object` (texto) a `datetime`?

**Respuesta correcta:**
```
Porque como datetime, Python puede:
- Extraer año, mes, día fácilmente con .dt.year, .dt.month, etc.
- Realizar operaciones con fechas (ej: ¿cuántos días lleva un título en el catálogo?)
- Ordenar cronológicamente de forma correcta
- Crear gráficos de series temporales (Clase 2)

Como texto (object), Python solo ve cadenas de caracteres: "September 25, 2021"
se ordenaría alfabéticamente, no cronológicamente.
```

---

### **Verificación 2: Valores Nulos (NaN)**

**Pregunta:** La columna `director` tiene 1,969 valores `NaN` (casi un tercio del dataset). ¿Es un error de los datos?

**Respuesta correcta:**
```
NaN significa "Not a Number" (dato faltante), y en este contexto significa "no registrado".

No es un error: los NaN de director se concentran en las series de TV
(1,841 de los 1,969). Las series rara vez registran un "director" único
porque tienen muchos directores por episodio; en cambio, solo 128 películas
carecen de director.

Lección: antes de "limpiar" NaN eliminándolos, pregúntate POR QUÉ faltan.
Eliminar filas sin director borraría casi todas las series del análisis.
```

---

### **Verificación 3: Categorías vs Números**

**Pregunta:** ¿Por qué conviene convertir `type` y `rating` de `object` a `category`, pero NO `release_year`?

**Respuesta correcta:**
```
type y rating son CATEGORÍAS: un conjunto pequeño y fijo de grupos
(2 tipos; ~14 ratings). Como category, pandas:
- Usa menos memoria
- Facilita conteos y agrupaciones
- Evita tratamientos incorrectos

release_year SÍ es un número de verdad: tiene sentido calcular su promedio
(2013.4), su mínimo (1925), su máximo (2020) o restarlo del año actual
para saber la antigüedad del contenido.

Regla práctica: si calcular el promedio NO tiene sentido → es categoría.
Si el promedio te dice algo del negocio → es número.
```

---

## 🚨 Errores Comunes y Cómo Evitarlos

### **Error #1: Confundir `=` con `==`**

```python
❌ INCORRECTO:
df[df['type'] = 'Movie']  # Esto es asignación, no comparación

✅ CORRECTO:
df[df['type'] == 'Movie']  # Esto es comparación
```

**Recuerda:** Un `=` asigna valor, dos `==` comparan valores.

---

### **Error #2: Olvidar el argumento `inplace=True`**

```python
❌ INCORRECTO:
df.drop(columns=['description'])  # No modifica el DataFrame original

✅ CORRECTO:
df.drop(columns=['description'], inplace=True)  # Sí modifica el original
```

**Alternativa sin inplace:**
```python
df = df.drop(columns=['description'])  # Reasigna el resultado
```

---

### **Error #3: No verificar el resultado de la IA**

```python
❌ RIESGOSO:
# Pedir "el promedio de duración de las películas" y ejecutar sin revisar
df['duration'].mean()  # ¡FALLA! duration es texto: "90 min", "2 Seasons"

✅ MEJOR:
# Verificar primero el tipo de dato: df['duration'].dtype → object (texto)
# El código correcto debe extraer el número antes de promediar:
# df[df['type']=='Movie']['duration'].str.extract('(\d+)').astype(float).mean()
# Resultado correcto: ~99.1 minutos
```

**Lección:** La IA puede generar código que se ejecuta sin errores pero que metodológicamente no tiene sentido (o que falla por un tipo de dato inesperado). Tú eres el filtro crítico: revisa el tipo de cada columna con `.info()` antes de operar.

---

## 📚 Recursos Adicionales

- **Documentación de Pandas:** https://pandas.pydata.org/docs/
- **Tutorial de value_counts:** https://pandas.pydata.org/docs/reference/api/pandas.Series.value_counts.html
- **Tipos de datos en Pandas:** https://pandas.pydata.org/docs/user_guide/basics.html#dtypes

---

## ✅ Checklist Antes de Avanzar a la Clase 2

Antes de pasar a la Clase 2 (Análisis y Visualización), asegúrate de que puedes:

- [ ] Cargar un CSV desde una URL en un DataFrame (`pd.read_csv(url)`)
- [ ] Usar `.head()`, `.info()`, y `.describe()` para explorar datos
- [ ] Filtrar datos con condiciones (`df[df['columna'] == valor]`)
- [ ] Contar valores únicos con `.value_counts()`
- [ ] Entender la diferencia entre tipos de datos (int64, float64, object, category, datetime)
- [ ] Convertir texto a fecha con `pd.to_datetime()` y extraer el año con `.dt.year`
- [ ] Descifrar códigos (ratings) con ayuda de la IA y crear columnas nuevas con `.map()`
- [ ] Interpretar resultados en contexto de negocio

**Si completaste todos los ejercicios y entendiste las verificaciones, ¡estás listo para la Clase 2!** 🎉
