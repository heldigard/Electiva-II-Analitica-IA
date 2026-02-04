# AI Reliability Checklist
## Lista de Verificación para Reducir Alucinaciones y Errores de IA

### Universidad UdeColombia - Electiva Analítica de Datos con IA

---

## ¿Qué es el AI Reliability Checklist?

La **IA puede alucinar** - inventar datos, interpretar mal código, o hacer cálculos incorrectos.

El **AI Reliability Checklist** es una lista de verificación que debes usar **siempre** que la IA te genere código o análisis. Te ayuda a detectar errores antes de que causen problemas.

---

## Regla de Oro

> **"Confía, pero verifica"**
>
> La IA es tu copiloto, pero **tú eres el piloto**. Siempre verifica lo que la IA genera.

---

## The 3-Check Method

Para cualquier análisis que la IA genere, haz **3 verificaciones**:

### ✅ Check 1: Summary Statistics

**Qué hacer:** Pide a la IA que muestre estadísticas resumen del análisis.

**Ejemplo de prompt:**
```
Antes de crear el gráfico, muestra estas estadísticas:
- Total de registros
- Promedio de la columna principal
- Mínimo y máximo
- Cantidad de valores faltantes

¿Estos números tienen sentido según lo que sabemos del negocio?
```

**Qué verificar:**
- [ ] Los totales coinciden con lo esperado
- [ ] Los promedios parecen razonables
- [ ] No hay valores negativos donde no deberían haber
- [ ] Los min/max no son extremos imposibles

**Señal de alerta:**
```
❌ "Total de ventas: $1,000,000" (sabemos que es ~$50,000)
❌ "Promedio de satisfacción: 7.5/5" (máximo posible es 5)
❌ "Edad mínima: -5 años" (edad no puede ser negativa)
```

---

### ✅ Check 2: Sample Rows

**Qué hacer:** Pide a la IA que muestre las primeras filas del resultado.

**Ejemplo de prompt:**
```
Muestra las primeras 10 filas del DataFrame resultante.
Verifiquemos que las columnas se calcularon correctamente.
```

**Qué verificar:**
- [ ] Los valores se ven correctos visualmente
- [ ] Los cálculos por fila tienen sentido
- [ ] No hay valores extraños (NaN, inf, null)

**Señal de alerta:**
```
❌ Columna "margen_pct" tiene valores de 5000% (debería ser 10-30%)
❌ Fecha "2026-13-45" (mes 13 no existe)
❌ Texto donde debería haber números
```

---

### ✅ Check 3: Triangulación

**Qué hacer:** Pregunta lo mismo de forma diferente y compara respuestas.

**Ejemplo de prompt:**
```
Voy a hacer la misma pregunta de dos formas:

FORMA 1:
¿Cuál es el margen promedio de todos los productos?

FORMA 2:
Suma el margen total y divide por el revenue total. ¿Qué porcentaje da?

Si los dos resultados no coinciden, explica por qué.
```

**Qué verificar:**
- [ ] Diferentes métodos dan el mismo resultado
- [ ] Si hay diferencia, la explica lógicamente
- [ ] No hay contradicciones en las respuestas

**Señal de alerta:**
```
❌ "Margen promedio: 15%"
❌ "Margen calculado: 22%"
❌ "Son diferentes porque... [explicación confusa]"

→ Uno de los dos está mal. Investiga más.
```

---

## El Checklist Completo

Usa este checklist para cada análisis importante:

```markdown
# AI Reliability Checklist

## Check 1: Summary Statistics
- [ ] Mostré .describe() o .info() del DataFrame
- [ ] Verifiqué que los totales tienen sentido
- [ ] Verifiqué que no hay outliers imposibles
- [ ] Verifiqué que no hay valores faltantes inesperados

## Check 2: Sample Rows
- [ ] Mostré .head() con las primeras filas
- [ ] Verifiqué visualmente que los cálculos se ven correctos
- [ ] Verifiqué que no hay valores extraños (NaN, inf, null)

## Check 3: Triangulación
- [ ] Calculé el mismo resultado de dos formas diferentes
- [ ] Los resultados coinciden (o entiendo por qué no)
- [ ] No hay contradicciones en las respuestas

## Check 4: Validación de Negocio
- [ ] Los resultados son consistentes con lo que sé del negocio
- [ ] Los números se ven razonables (no demasiado altos/bajos)
- [ ] Si algo parece raro, lo investigué antes de aceptarlo

## Check 5: Código Revisado
- [ ] Leí el código que la IA generó
- [ ] Entiendo qué hace cada parte importante
- [ ] No hay código sospechoso o que no entienda

## Check 6: Límites de la IA
- [ ] Identifiqué qué suposiciones hizo la IA
- [ ] Verifiqué que esas suposiciones son correctas
- [ ] La IA no está inventando datos que no existen
```

---

## Ejemplos Reales de Errores de IA

### Error 1: Alucinación de Datos

**Prompt:**
```
¿Cuál es el producto más vendido?
```

**Respuesta de IA:**
```
El producto más vendido es "Leche Entera 1L" con 15,432 unidades.
```

**Problema:** La IA inventó el número 15,432. No lo calculó de los datos.

**Verificación:**
```
PROMPT DE VERIFICACIÓN:
Antes de responder, muéstrame:
1. El código que usaste para calcularlo
2. Las primeras 10 filas ordenadas por cantidad

RESPUESTA:
df.groupby('producto')['cantidad'].sum().sort_values(ascending=False).head(10)

producto
Leche Entera 1L    234
Pan de Molde       198
...
```

**Resultado real:** "Leche Entera 1L" tenía 234 unidades, no 15,432. La IA alucinó.

**Lección:** Siempre pide ver el código y los datos, no solo el resultado final.

---

### Error 2: Interpretación Incorrecta

**Prompt:**
```
¿Cuál es la satisfacción promedio de los clientes?
```

**Respuesta de IA:**
```
La satisfacción promedio es 4.2/5, lo que indica alta satisfacción.
```

**Problema:** La IA dijo 4.2 pero los datos reales eran 3.1/5.

**Verificación:**
```
PROMPT DE VERIFICACIÓN:
Muéstrame:
1. df['satisfaccion'].describe()
2. Un histograma de la distribución de satisfacción

RESPUESTA:
count    1000.000000
mean        3.125000
std         1.234567
min         1.000000
max         5.000000
```

**Resultado real:** 3.125/5, no 4.2. La IA alucinó o usó datos equivocados.

**Lección:** Siempre verifica con summary statistics antes de aceptar conclusiones.

---

### Error 3: Código con Bugs

**Prompt:**
```
Calcula el margen % para cada producto.
```

**Respuesta de IA:**
```python
df['margen_pct'] = (df['precio'] - df['costo']) / df['precio']
```

**Problema:** El código calcula el margen en decimal (0.15 para 15%), no porcentaje (15).

**Verificación:**
```
PROMPT DE VERIFICACIÓN:
Muestra df['margen_pct'].head(10)

RESPUESTA:
0    0.1523
1    0.1845
2    0.1234
```

**Resultado:** Los valores están en decimal (0.15), no porcentaje (15).

**Corrección:**
```python
df['margen_pct'] = (df['precio'] - df['costo']) / df['precio'] * 100
```

**Lección:** Siempre muestra sample rows para verificar que los cálculos se ven correctos.

---

## Cómo Reducir Alucinaciones

### Tip 1: Sé Específico con Datos

❌ **Mal:** "Analiza mis datos"
✅ **Bien:** "Usa el DataFrame df que tiene las columnas X, Y, Z. Calcula W."

### Tip 2: Pide Verificación

❌ **Mal:** "¿Cuál es el promedio?"
✅ **Bien:** "Calcula el promedio Y muéstrame el código que usaste + summary statistics."

### Tip 3: Triangula

❌ **Mal:** Aceptar la primera respuesta
✅ **Bien:** "Pregúntame de otra forma y verifica que da el mismo resultado."

### Tip 4: Valida con Sentido Común

❌ **Mal:** Aceptar que el promedio de satisfacción es 7/5
✅ **Bien:** "7/5 es imposible. El máximo es 5. Revisa tus cálculos."

### Tip 5: Muestra No Solo Cuéntes

❌ **Mal:** "El producto más vendido tiene 15,432 unidades"
✅ **Bien:** "Muéstrame el código y las primeras filas que usaste para llegar a ese resultado."

---

## Prompts de Verificación Recomendados

Copia y pega estos prompts después de cada análisis importante:

### Para Análisis Numérico
```
Verifica tu respuesta:

1. Muestra .describe() de las columnas que usaste
2. Muestra .head() de las primeras 10 filas
3. ¿Los números tienen sentido según el contexto del negocio?
4. Si algo parece raro, explícalo o corrige tu respuesta.
```

### Para Gráficos
```
Antes de crear el gráfico:

1. Muestra los datos que vas a graficar (head 10 filas)
2. ¿Los valores mínimo y máximo son razonables?
3. ¿Hay valores NaN o inf que distorsionen el gráfico?

Si todo está bien, crea el gráfico.
```

### Para Código Generado
```
Muéstrame y explica:

1. El código que generaste
2. Qué hace cada parte importante
3. Si hay alguna suposición que estás haciendo

Si no estoy seguro de entender algo, explícalo más.
```

### Para Conclusiones
```
Valida tu conclusión:

1. ¿Qué datos específicos respaldan esta conclusión?
2. ¿Hay alguna otra interpretación posible?
3. ¿Qué suposiciones estás haciendo?
4. ¿Cómo verificaríamos que esto es correcto en el mundo real?
```

---

## Niveles de Confianza

No todos los análisis requieren el mismo nivel de verificación:

| Nivel | Cuándo Usar | Verificación Requerida |
|-------|-------------|------------------------|
| **Exploratorio** | "Estoy explorando datos por primera vez" | Check 1 (Summary stats) |
| **Toma de Decisión** | "Basado en esto voy a tomar una decisión" | Todos los checks |
| **Presentación** | "Voy a mostrar esto a directivos" | Todos + revisión manual |
| **Crítico** | "Esto afecta revenue significativo o empleo" | Todos + segunda opinión |

---

## Signos de Alucinación

 Señales de que la IA podría estar alucinando:

🚨 **Números demasiado redondos**
- "El promedio es exactamente 50.0%"
- "Tienen 1,000 clientes exactamente"

🚨 **Resultados demasiado perfectos**
- "Todos los productos tienen el mismo margen"
- "La satisfacción subió exactamente 10%"

🚨 **Falta de transparencia**
- No muestra el código que usó
- No puede explicar cómo llegó al resultado
- Evita mostrar datos intermedios

🚨 **Contradicciones**
- Dice "El promedio es 4.2" pero el histograma muestra mayoría de 1-2
- Conclusión no coincide con los datos que muestra

🚨 **Confianza excesiva**
- "Definitivamente esto es correcto" (sin verificar)
- "100% seguro del resultado"

---

## Qué Hacer si Detectas una Alucinación

1. **No entres en pánico** - Las errores de IA son comunes
2. **Pide verificación** - "Muéstrame el código y los datos"
3. **Triangula** - "Calcula esto de otra forma"
4. **Verifica manualmente** - Haz el cálculo tú mismo en una muestra
5. **Reformula el prompt** - Sé más específico sobre los datos
6. **Cambia de chatbot** - A veces otro modelo da mejor resultado

---

## Quick Reference Checklist

```
Para cada respuesta de la IA:

☐ 1. Summary statistics check (.describe(), .info())
☐ 2. Sample rows check (.head())
☐ 3. Triangulation (calcular de otra forma)
☐ 4. Business validation (¿tiene sentido?)
☐ 5. Code review (¿entiendo qué hace?)
☐ 6. Assumptions check (¿qué suposiciones hizo?)

SI TODO PASA → Aceptar respuesta
SI ALGO FALLA → Investigar más
```

---

## Ejercicio Práctico

**Instrucciones:**
1. Pide a la IA que analice algún dataset que tengas
2. Usa los 3 checks (summary, sample, triangulación)
3. Documenta cualquier error que encuentres
4. Corrige el error y valida la corrección

**Tiempo estimado:** 15 minutos

---

## Recursos Adicionales

- **Business-Question-Canvas.md** - Cómo hacer buenas preguntas
- **Data-Ethics-Guide.md** - Qué datos compartir con IA
- **Prompt-Templates-2026.md** - Prompts efectivos y verificables

---

## Puntos Clave

1. **La IA puede alucinar** - Siempre verifica
2. **3 checks mínimos:** summary stats, sample rows, triangulación
3. **Pide ver el código** - No solo el resultado
4. **Usa sentido común** - Si parece demasiado bueno para ser verdad, probablemente lo es
5. **Para decisiones importantes** - Usa todos los checks + validación manual

---

**Recuerda:** 5 minutos de verificación ahorran horas de corrección después.

*Universidad UdeColombia - Especialización en Analítica de Datos - 2026*
