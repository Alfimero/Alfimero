# Manual 4 — Fixer

Manual paso a paso para usar un modelo de IA como **reparador**: tomar un problema (un bug, un test rojo, un build roto, un error en producción) y llevarlo hasta una corrección verificada. Es el flujo inverso al de Fable Plan: aquí no se diseña algo nuevo, se diagnostica y repara algo que ya existe y falla.

---

## 1. Qué es

Un "fixer" es un flujo de trabajo donde el modelo ejecuta el ciclo completo de reparación: **reproducir → diagnosticar → corregir mínimamente → verificar**. Los modelos agénticos actuales pueden ejecutar cada fase con herramientas reales: corren el código, leen logs y trazas de error, añaden instrumentación temporal, bisectan el historial de git y comprueban la corrección con los tests.

**Qué puede ejecutar el modelo hoy:**
- Reproducir un fallo a partir de un reporte, un log o un mensaje de error.
- Leer trazas (stack traces) y seguirlas hasta el código responsable.
- Usar `git bisect`, `git log` y el diff reciente para encontrar el cambio que rompió algo.
- Escribir primero un test que falla, luego la corrección que lo pone en verde.
- Vigilar un pull request y corregir automáticamente los fallos de CI que aparezcan.

**Qué NO debes esperar:** que repare lo que no puede observar (un fallo que solo ocurre en un entorno al que no tiene acceso), ni que "corrección mínima" y "refactor del módulo" sean lo mismo — hay que pedirle explícitamente la primera.

## 2. Cuándo usarlo

- Un test o el build fallan y no es obvio por qué.
- Un error aparece en producción con traza o log disponible.
- "Ayer funcionaba": algo se rompió con un cambio reciente.
- CI en rojo en un pull request que quieres dejar en verde.
- Comportamiento incorrecto sin error visible (el más difícil: hay resultado, pero es el equivocado).

## 3. Requisitos

- [ ] Evidencia del fallo: mensaje de error, traza, log, captura o descripción de "esperaba X, obtuve Y".
- [ ] Acceso del modelo al código y a la terminal para ejecutar y probar.
- [ ] Una forma de saber que está arreglado (test, comando, comportamiento observable).
- [ ] Rama de trabajo separada si el arreglo va a un repositorio compartido.

## 4. Procedimiento paso a paso

### Paso 1 — Entrega toda la evidencia, sin resumirla
El error literal vale más que tu interpretación del error. Pega la traza completa, no tu resumen:

> **Prompt de ejemplo:**
> "Este comando falla. Aquí está la salida completa: [pegar salida literal]. Esperaba que [comportamiento correcto]. Empezó a fallar aproximadamente [cuándo]. Repara el problema."

Si no tienes traza (comportamiento incorrecto sin error), describe el contraste: entrada exacta, resultado obtenido, resultado esperado.

### Paso 2 — Exige reproducción antes que diagnóstico
La regla de oro del fixer: **no se corrige lo que no se puede reproducir**. Pídelo como primera acción:

> "Antes de proponer ninguna causa, reproduce el fallo tú mismo y muéstrame el comando exacto y la salida. Si no puedes reproducirlo, dime qué información o acceso te falta — no adivines."

Esto elimina la clase de error más común: corregir una teoría en vez de un bug.

### Paso 3 — Pide el diagnóstico como causa raíz, no como síntoma
> "Ya que lo reproduces: ¿cuál es la causa raíz? Distingue entre dónde EXPLOTA el error y dónde se ORIGINA. Muéstrame la evidencia en el código que confirma tu diagnóstico."

Técnicas que puedes pedir explícitamente si el diagnóstico se atasca:
- **Bisección**: "Usa `git bisect` (o revisa el diff de los últimos N commits) para encontrar el cambio que lo rompió."
- **Instrumentación**: "Añade logs temporales para confirmar qué valor llega realmente a esa función, ejecútalo y muéstrame la salida. Luego quita los logs."
- **Aislamiento**: "Construye el ejemplo mínimo que reproduce el fallo, fuera de la aplicación si hace falta."

### Paso 4 — Congela el fallo en un test
Antes de corregir, pide el test que falla:

> "Escribe un test que reproduzca este bug y falle con el código actual. Ese test define qué significa 'arreglado'."

Este paso convierte el arreglo en algo verificable y evita que el bug regrese en el futuro.

### Paso 5 — Pide la corrección mínima
El instinto del modelo puede ser mejorar todo lo que ve. Acótalo:

> "Aplica la corrección mínima que ataca la causa raíz. No refactorices, no renombres, no mejores código adyacente. Si viste otros problemas por el camino, lístalos aparte — los decido yo."

Un diff pequeño se revisa en minutos; uno grande esconde regresiones.

### Paso 6 — Verifica en tres círculos
La verificación va de adentro hacia afuera; pide los tres niveles:

1. **El test del bug pasa** (paso 4 en verde).
2. **La suite completa pasa** — la corrección no rompió otra cosa.
3. **El flujo real funciona** — ejecutar la aplicación/comando original del paso 1 y ver el comportamiento correcto, no solo tests.

> "Verifica en los tres niveles y muéstrame la evidencia de cada uno: salida del test nuevo, salida de la suite, y la salida del comando original que antes fallaba."

### Paso 7 — Revisa el diff como si fuera de otra persona
Antes de integrar, lee el diff completo. Preguntas para el modelo:

> - "Explícame por qué este cambio corrige la causa raíz y no solo el síntoma."
> - "¿Qué otros lugares del código tienen el mismo patrón y podrían tener el mismo bug?"
> - "¿En qué escenario este arreglo podría romper algo?"

La segunda pregunta encuentra con frecuencia bugs hermanos gratis.

### Paso 8 — Integra y deja constancia
Commit con mensaje que explique el **porqué** (la causa), no solo el qué. Si el fixer opera sobre un pull request, los modelos actuales pueden además quedarse **vigilando**: suscribirse a los eventos del PR y corregir automáticamente los fallos de CI o atender comentarios de revisión hasta que esté en verde y fusionado.

> "Haz commit y push a la rama. Vigila el PR: si CI falla, diagnostica y corrige con este mismo procedimiento; avísame solo si te atascas o si el fallo es ajeno a mi cambio."

## 5. Lista de verificación final

- [ ] El fallo fue reproducido antes de ser diagnosticado.
- [ ] El diagnóstico distingue causa raíz de síntoma, con evidencia.
- [ ] Existe un test que fallaba antes del arreglo y pasa después.
- [ ] La suite completa y el flujo real fueron verificados, no solo el test nuevo.
- [ ] El diff es mínimo y entiendo cada línea.
- [ ] Se revisó si el mismo patrón defectuoso existe en otros lugares.

## 6. Límites conocidos

- Si el fallo no es reproducible en el entorno del modelo (dependencias de producción, datos reales, condiciones de carrera), el modelo solo puede teorizar — trata esas teorías como hipótesis a verificar, no como diagnóstico.
- Bajo presión por "dejarlo en verde", existe la tentación (humana y del modelo) de silenciar el test en vez de arreglar el bug. Prohíbelo explícitamente: deshabilitar un test es una decisión tuya, nunca un arreglo.
- Los fallos intermitentes (flaky) requieren repetir la reproducción muchas veces; pide "ejecútalo 20 veces" antes de aceptar un "ya no falla".
