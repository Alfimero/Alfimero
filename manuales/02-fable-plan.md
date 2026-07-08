# Manual 2 — Fable Plan

Manual paso a paso para planificar trabajo con un modelo avanzado (clase Fable/Opus) **antes** de ejecutarlo: el modo de planificación. La idea central: separar el *pensar* del *hacer*, para que tú apruebes el plan antes de que se toque una sola línea.

---

## 1. Qué es

Los asistentes agénticos actuales tienen un "modo plan": el modelo explora el proyecto en modo solo-lectura, investiga el contexto, y produce un plan de implementación paso a paso que tú apruebas, ajustas o rechazas. Solo después de tu aprobación pasa a ejecutar. Con modelos de la gama más alta (como Fable 5), este plan puede incluir análisis de alternativas, riesgos, orden de implementación y criterios de verificación.

**Qué puede ejecutar el modelo hoy:**
- Explorar una base de código desconocida y entender su arquitectura en minutos.
- Detectar restricciones reales (convenciones del proyecto, dependencias, tests existentes) que condicionan el plan.
- Proponer 2–3 enfoques alternativos con ventajas y desventajas, y recomendar uno.
- Convertir el plan aprobado en ejecución directa, marcando el progreso paso a paso.

**Qué NO debes esperar:** que adivine requisitos que no le diste, ni que un plan perfecto elimine la necesidad de revisar el resultado.

## 2. Cuándo usarlo

Usa modo plan cuando **al menos una** de estas condiciones se cumple:

- El cambio toca 3 o más archivos, o partes del sistema que no conoces bien.
- Hay más de una forma razonable de hacerlo y la elección importa.
- El cambio es difícil de revertir (migraciones, APIs públicas, borrado de datos).
- Vas a delegar la ejecución completa y quieres controlar el rumbo antes, no después.

**No** lo uses para cambios triviales (un typo, un color, un texto): planificar costaría más que hacer.

## 3. Requisitos

- [ ] Un objetivo expresable en una o dos frases ("qué quiero que exista al final").
- [ ] Acceso del modelo al proyecto en modo lectura.
- [ ] Saber qué es negociable y qué no (plazos, tecnologías obligadas, cosas que no se pueden romper).

## 4. Procedimiento paso a paso

### Paso 1 — Formula el objetivo, no la solución
Describe el resultado deseado y las restricciones. Evita dictar la implementación: para eso pides el plan.

> **Prompt de ejemplo:**
> "Quiero añadir autenticación con Google a esta aplicación. Restricciones: no romper el login con email existente, no añadir dependencias de pago, y debe funcionar en el flujo móvil. Entra en modo plan y propón cómo hacerlo. No modifiques nada todavía."

### Paso 2 — Deja que explore antes de opinar
Un buen plan nace de la exploración, no de la memoria del modelo. Verifica que el plan cite archivos y código **reales** de tu proyecto:

> "Antes de proponer nada, explora el proyecto: dónde vive la autenticación actual, qué convenciones se usan, qué tests existen. Cita los archivos concretos en los que basas el plan."

Si el plan habla en genérico ("normalmente se haría..."), pídele que lo aterrice al código real.

### Paso 3 — Pide alternativas con recomendación
> "Dame 2 o 3 enfoques posibles con sus ventajas, desventajas y riesgos. Recomienda uno y justifica la recomendación en función de MIS restricciones, no de las mejores prácticas en abstracto."

### Paso 4 — Exige la estructura completa del plan
Un plan ejecutable debe tener estas partes. Si falta alguna, pídela:

1. **Resumen** — qué se va a hacer, en 3 líneas.
2. **Archivos afectados** — lista concreta, con qué cambia en cada uno.
3. **Orden de implementación** — pasos numerados, cada uno verificable por sí solo.
4. **Qué NO se toca** — el alcance negativo evita sorpresas.
5. **Riesgos y reversa** — qué puede salir mal y cómo se deshace.
6. **Criterio de éxito** — cómo sabremos que está terminado (tests, comportamiento observable).

### Paso 5 — Desafía el plan antes de aprobarlo
Este paso es el que más valor extrae del modelo. Preguntas útiles:

> - "¿Qué parte de este plan es la más probable que falle y por qué?"
> - "¿Qué pasa con [caso borde que a ti te preocupa]?"
> - "Si tuvieras que cortar este plan a la mitad del esfuerzo, ¿qué quitarías?"
> - "¿Qué asumiste sin verificarlo en el código?"

La última pregunta suele revelar los puntos débiles reales.

### Paso 6 — Aprueba explícitamente y por escrito
Aprueba el plan (o la versión ajustada). Desde aquí el modelo pasa a ejecución:

> "Apruebo el plan con un cambio: en el paso 3 usa la tabla existente en vez de crear una nueva. Ejecuta paso a paso y avísame al terminar cada paso mayor."

### Paso 7 — Supervisa contra el plan, no contra tu memoria
Durante la ejecución, el plan aprobado es el contrato. Si el modelo descubre algo que invalida un paso, debe **volver a ti** con la propuesta de cambio, no improvisar en silencio. Pídelo así desde el inicio:

> "Si durante la ejecución descubres que un paso del plan no es viable, detente y proponme el ajuste antes de continuar."

### Paso 8 — Cierra contra el criterio de éxito
Al final, verifica contra el punto 6 del plan (criterio de éxito), no contra la sensación de "parece que está". Pide la evidencia: salida de tests, demostración del comportamiento, diff final.

## 5. Lista de verificación final

- [ ] El plan citaba archivos y código reales del proyecto.
- [ ] Tuve al menos una alternativa y entendí por qué se descartó.
- [ ] El plan incluía qué NO se iba a tocar.
- [ ] Los desvíos durante la ejecución pasaron por mí.
- [ ] El resultado cumple el criterio de éxito definido en el plan, con evidencia.

## 6. Límites conocidos

- Un plan es una hipótesis: la exploración reduce sorpresas pero no las elimina.
- El modelo tiende a planes más ambiciosos de lo pedido; el "alcance negativo" (paso 4.4) es tu freno.
- Si el objetivo está mal definido, el plan será una buena ejecución del problema equivocado. El paso 1 es el más importante del manual.
