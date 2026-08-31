# Guía de estudio — Quiz 1
## Modelos, prompting y uso responsable

Esta guía reúne los conceptos que debes comprender y aplicar antes de presentar el Quiz 1. El objetivo no es memorizar marcas o nombres comerciales, sino elegir una herramienta adecuada, darle instrucciones claras y validar responsablemente su respuesta.

## Cómo funciona el quiz

- Peso en la nota final: **27,5%**.
- Banco disponible: **20 preguntas**.
- Cada intento presenta **5 preguntas seleccionadas aleatoriamente**.
- Tienes **3 intentos** y se conserva la **nota más alta**.
- Disponible desde el **10 de agosto de 2026 a las 00:00** hasta el **14 de septiembre de 2026 a las 23:59**.
- Las preguntas son situacionales: debes escoger la decisión más responsable, no solo recordar una definición.

## 1. Aplicación, proveedor y modelo no son lo mismo

Conviene separar tres conceptos:

| Concepto | Qué representa | Ejemplo genérico |
|---|---|---|
| Aplicación o interfaz | El lugar donde escribes, cargas archivos o ejecutas una tarea | Un chatbot o un asistente integrado en un notebook |
| Proveedor | La organización que ofrece el servicio | La empresa que desarrolla u hospeda la tecnología |
| Modelo | El sistema que procesa las instrucciones y genera la respuesta | Una versión específica, con capacidades y costos determinados |

Una misma aplicación puede ofrecer varios modelos. Un proveedor puede cambiar el modelo disponible según el plan, la región o la fecha. Por eso, para comparar herramientas, primero identifica **qué tarea**, **qué modelo** y **qué configuración** se usaron.

## 2. Elegir el tamaño del modelo según la tarea

En este curso usamos una regla práctica de tres niveles:

| Tamaño | Úsalo principalmente para | Ejemplos |
|---|---|---|
| Grande | Problemas ambiguos, planificación compleja, evaluación de alternativas o decisiones de alto impacto | Diseñar un plan de análisis, identificar riesgos, revisar una decisión financiera |
| Mediano | Implementación bien definida y trabajo técnico cotidiano | Escribir una función, transformar una tabla, implementar un plan ya aprobado |
| Pequeño | Tareas repetitivas, acotadas y de bajo riesgo | Clasificar comentarios, ajustar formato, resumir campos simples |

La secuencia recomendada para un trabajo complejo es:

1. **Modelo grande:** entiende el problema, aclara requisitos y propone el plan.
2. **Modelo mediano:** implementa pasos concretos del plan.
3. **Modelo pequeño:** automatiza tareas simples y repetitivas.
4. **Persona responsable:** verifica resultados y toma la decisión final.

El tamaño no reemplaza la validación. En una decisión financiera, legal, médica o de alto impacto, incluso una respuesta de un modelo grande necesita revisión de una persona competente.

## 3. Cómo comparar modelos con justicia

Una comparación útil controla las condiciones:

- La misma tarea y el mismo contexto.
- La misma información de entrada.
- El mismo formato esperado.
- Criterios definidos antes de leer las respuestas: exactitud, claridad, utilidad, costo, tiempo y cumplimiento de restricciones.
- Verificación independiente de afirmaciones, cálculos y referencias.

No es una comparación justa darle a un modelo un prompt detallado y a otro una frase ambigua. Tampoco basta con elegir la respuesta que “suena mejor”.

## 4. Anatomía de un prompt efectivo

Un buen prompt suele contener:

1. **Contexto:** cuál es la situación y para quién se trabaja.
2. **Objetivo:** qué pregunta debe responder o qué resultado debe producir.
3. **Datos disponibles:** columnas, unidades, periodo y ejemplos relevantes.
4. **Restricciones:** qué no debe inventar, qué fuentes puede usar o qué límites debe respetar.
5. **Formato de salida:** tabla, lista, código, pasos o resumen ejecutivo.
6. **Criterio de verificación:** cálculos, supuestos o pruebas que debe mostrar.

Ejemplo:

> Actúa como guía de análisis. Tengo una tabla mensual con las columnas `fecha`, `categoria`, `ventas` y `costo`, expresadas en pesos colombianos. Antes de escribir código, formula hasta tres preguntas para aclarar el objetivo. Después propón un plan de análisis y una tabla de salida. No inventes valores ausentes; señala los datos que falten y explica cómo validarías cada cálculo.

### Restricciones que reducen errores

Incluye instrucciones como estas cuando sean necesarias:

- “Si falta evidencia, indícalo; no inventes una conclusión”.
- “Separa hechos observados, supuestos e hipótesis”.
- “Muestra el cálculo o el código que respalda cada cifra”.
- “Cita solo fuentes que hayas podido verificar”.
- “Usa exactamente estas columnas y estas unidades”.

Un ejemplo del formato esperado también reduce la ambigüedad, porque muestra la estructura que debe seguir la respuesta.

## 5. Usar la IA como guía, no solo como generador

En modo generador, la IA intenta entregar directamente código o una respuesta. En modo guía o tutor, ayuda a pensar el proceso.

Un prompt de tutor puede pedir:

- Explicación paso a paso.
- Una sola etapa por vez.
- Preguntas de comprobación antes de avanzar.
- Pistas antes de mostrar la solución.
- Revisión de la interpretación hecha por el estudiante.

Si los requisitos son contradictorios o incompletos, la mejor acción no es programar de inmediato. Primero hay que identificar la contradicción, pedir aclaraciones y acordar un criterio.

## 6. Verificar respuestas, cifras, citas y código

Que un texto sea convincente o que el código se ejecute sin errores no demuestra que el resultado sea correcto.

Aplica estas verificaciones:

- **Estadísticas resumen:** conteos, promedio, mínimo, máximo y faltantes.
- **Filas de muestra:** comprobar que transformaciones y cálculos funcionen por registro.
- **Triangulación:** obtener el mismo resultado mediante otro cálculo o método.
- **Lógica y unidades:** confirmar fórmulas, escalas, fechas y conversiones.
- **Casos límite:** probar datos vacíos, valores extremos y entradas inválidas.
- **Fuentes:** abrir la referencia y confirmar que existe y respalda la afirmación.

Si la IA presenta una cita que no puedes localizar, trátala como **no verificada**. Busca la fuente original o elimina esa afirmación.

Para depurar código, comparte información concreta y no datos confidenciales:

- Mensaje de error exacto.
- Fragmento mínimo del código.
- Estructura y tipos de los datos.
- Resultado esperado y resultado obtenido.
- Cambios que ya intentaste.

## 7. Privacidad y responsabilidad humana

Antes de enviar información a una herramienta de IA:

1. Clasifica los datos.
2. Elimina nombres, documentos, correos, teléfonos, direcciones y credenciales.
3. Minimiza la cantidad de información compartida.
4. Anonimiza o usa datos sintéticos cuando sea posible.
5. Utiliza únicamente herramientas autorizadas para el nivel de sensibilidad.

Nunca pegues claves de API, contraseñas o información empresarial confidencial en un chatbot público. La responsabilidad por el análisis y por la decisión final sigue siendo humana.

## 8. Casos para practicar

Antes del quiz, explica en voz alta qué harías en cada caso:

1. Debes diseñar un plan para una decisión financiera ambigua y de alto impacto.
2. Ya existe un plan aprobado y necesitas implementar una transformación en Python.
3. Debes clasificar miles de comentarios con categorías previamente validadas.
4. Dos requisitos del usuario se contradicen.
5. Una respuesta incluye una cifra precisa, pero no muestra cálculo ni fuente.
6. El código se ejecuta, aunque el total obtenido es imposible para el negocio.
7. Necesitas comparar dos modelos de forma justa.
8. Un conjunto de datos incluye nombres, correos y números de identificación.

Tu explicación debe mencionar la elección del modelo, el riesgo, el prompt necesario y la forma de verificar.

## Lista de comprobación antes de presentar

- [ ] Distingo aplicación, proveedor y modelo.
- [ ] Sé cuándo usar un modelo grande, mediano o pequeño.
- [ ] Puedo proponer el flujo planificar → implementar → automatizar → revisar.
- [ ] Escribo prompts con contexto, objetivo, datos, restricciones y formato.
- [ ] Sé pedir aclaraciones cuando los requisitos son ambiguos o contradictorios.
- [ ] Verifico cifras, referencias, lógica, unidades y casos límite.
- [ ] No comparto datos personales, credenciales ni información confidencial.
- [ ] Entiendo que la decisión final y su justificación son responsabilidad humana.

## Material relacionado

- [Clase 1 — Fundamentos y prompting](../clases/Clase1-Fundamentos-Prompting.ipynb)
- [Prompt Playbook](../complementarios/Prompt-Playbook.md)
- [Plantillas de prompts 2026](../complementarios/Prompt-Templates-2026.md)
- [AI Reliability Checklist](../complementarios/AI-Reliability-Checklist.md)
- [Guía de ética y privacidad](../complementarios/Data-Ethics-Guide.md)
