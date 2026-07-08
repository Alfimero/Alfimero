# Manual 3 — La opinión con crítica constructiva

Manual paso a paso para obtener de un modelo de IA una opinión **honesta, específica y accionable** sobre un trabajo: código, un texto, un diseño, una decisión técnica o un plan. El problema a resolver: por defecto, los modelos tienden a la amabilidad y validan demasiado. Este manual configura la conversación para que la crítica sea útil.

---

## 1. Qué es

Una sesión de crítica constructiva con IA es una revisión estructurada donde el modelo actúa como revisor experto: evalúa contra criterios explícitos, señala problemas concretos con su ubicación exacta, distingue lo grave de lo cosmético, y para cada problema propone una mejora accionable. Los modelos actuales pueden ejecutar esto sobre casi cualquier artefacto que puedan leer.

**Qué puede ejecutar el modelo hoy:**
- Revisar código real (correr los tests, leer el diff, verificar sus propias objeciones antes de dártelas).
- Evaluar textos y documentos contra una audiencia y un propósito definidos.
- Adoptar perspectivas distintas ("léelo como lo leería un cliente enojado", "como un revisor de seguridad").
- Priorizar: separar los 2 problemas que importan de los 20 detalles menores.

**Qué NO debes esperar:** gusto personal infalible, conocimiento de tu contexto político/organizacional si no se lo cuentas, ni que la crítica dura sea siempre correcta — también se verifica.

## 2. Cuándo usarlo

- Antes de publicar, enviar o presentar algo importante.
- Cuando llevas tanto tiempo en un trabajo que ya no lo ves con ojos frescos.
- Cuando necesitas una segunda opinión y no quieres (o no puedes) gastar el tiempo de un colega.
- Antes de una decisión con alternativas: pedir la crítica de cada opción.

## 3. Requisitos

- [ ] El artefacto a revisar, completo y en su versión actual.
- [ ] Claridad sobre **para quién** es el trabajo y **qué debe lograr**.
- [ ] Disposición real a recibir objeciones (si solo quieres validación, este manual no aplica).

## 4. Procedimiento paso a paso

### Paso 1 — Neutraliza la amabilidad por defecto
Dile explícitamente que la crítica es el objetivo. Esto cambia el comportamiento del modelo de forma medible:

> **Prompt de ejemplo:**
> "Voy a mostrarte un trabajo. No quiero ánimo ni elogios de cortesía: quiero encontrar los problemas antes que mis lectores. Si algo está bien, dilo en una línea y pasa a lo que está mal. Tu utilidad se mide por los problemas reales que encuentres."

### Paso 2 — Da el contexto que un revisor humano tendría
La crítica sin contexto evalúa contra un estándar genérico. Especifica:

- **Audiencia**: quién lo va a leer/usar.
- **Propósito**: qué debe lograr (convencer, enseñar, funcionar, vender).
- **Etapa**: ¿borrador temprano (critica la estructura) o versión final (critica el detalle)?
- **Restricciones**: qué no se puede cambiar ("el plazo es mañana", "la API es pública").

> "Es un correo para un cliente que está a punto de cancelar el contrato. Objetivo: retenerlo sin regalar descuentos. Es la versión final, sale hoy."

### Paso 3 — Pide la crítica en dos niveles
Estructura la respuesta para separar lo esencial de lo cosmético:

> "Dame la crítica en dos niveles:
> **Nivel 1 — Problemas de fondo** (máximo 3): cosas que comprometen el objetivo. Para cada uno: dónde está, por qué es un problema para ESTA audiencia, y qué harías en su lugar.
> **Nivel 2 — Mejoras menores**: lista rápida, sin desarrollar.
> Si no hay problemas de fondo, dilo explícitamente — no infles el nivel 1 para parecer riguroso."

La última frase es importante: evita que el modelo fabrique gravedad.

### Paso 4 — Exige especificidad y evidencia
Una crítica que no puedes localizar no es accionable. Reglas para el modelo:

> "Cada objeción debe citar la parte exacta (línea, párrafo, sección) y describir el efecto concreto: qué entendería mal el lector, qué caso rompería el código, qué pregunta quedaría sin responder. Prohibido decir 'podría ser más claro' sin decir qué, dónde y cómo."

En código, súbelo un nivel: pide que **verifique** sus objeciones ejecutando el código o los tests antes de reportarlas.

### Paso 5 — Usa perspectivas para encontrar ángulos ciegos
Repite la revisión desde miradas distintas; cada una encuentra cosas diferentes:

> - "Ahora léelo como el lector más escéptico posible: ¿dónde dejaría de creerme?"
> - "Ahora como alguien sin contexto técnico: ¿dónde se pierde?"
> - "Ahora como mi competidor: ¿qué debilidad atacaría?"

### Paso 6 — Discute las objeciones, no las aceptes en bloque
La crítica del modelo también puede estar equivocada. Con las objeciones de fondo:

> "No estoy de acuerdo con la objeción 2 porque [razón]. Defiéndela o retírala."

Un modelo actual bien usado **sostiene** la objeción si tiene razón y la retira si no — pero solo si tú abres ese espacio. Si cede a la primera, pregúntale: "¿la retiras porque me diste la razón o porque tengo razón?".

### Paso 7 — Convierte la crítica en acciones
Cierra transformando lo aceptado en una lista de cambios concretos y ordenados por impacto:

> "De todo lo discutido, dame la lista final de cambios que haría, ordenada por impacto. Marca cuáles puedes aplicar tú directamente y aplícalos si te lo confirmo."

En código y documentos, el modelo puede aplicar los cambios él mismo; en decisiones, el resultado es tu resolución informada.

### Paso 8 — Segunda pasada sobre la versión corregida
Una revisión de la versión final cierra el ciclo. Pídela limpia, sin el historial sesgando:

> "Aquí está la versión corregida. Olvida la crítica anterior y revísala de cero con los mismos criterios."

## 5. Lista de verificación final

- [ ] Definí audiencia, propósito y etapa antes de pedir la crítica.
- [ ] Recibí problemas de fondo separados de detalles menores.
- [ ] Cada objeción de fondo tenía ubicación exacta y efecto concreto.
- [ ] Desafié al menos una objeción y la discusión fue real.
- [ ] La versión final pasó una segunda revisión limpia.

## 6. Límites conocidos

- El modelo no conoce la política interna, la historia con esa audiencia ni el tono de tu relación: dale ese contexto o descuenta la crítica que dependa de él.
- Pedir dureza en exceso produce el vicio contrario: objeciones infladas para cumplir la cuota. El paso 3 (permiso para decir "no hay problemas de fondo") lo contrarresta.
- En temas de puro gusto (estética, humor, voz personal), usa la crítica como radar de riesgos, no como árbitro.
