---
name: fable-plan
description: Planificar un cambio antes de ejecutarlo (modo plan). Úsala cuando el trabajo toque 3 o más archivos, tenga varias soluciones razonables, sea difícil de revertir (migraciones, APIs públicas, borrado de datos), o cuando el usuario pida "un plan", "planifica", "diseña el enfoque" o quiera aprobar el rumbo antes de implementar. No la uses para cambios triviales.
---

# Fable Plan (modo planificación)

Diseña el trabajo antes de ejecutarlo y consíguelo aprobado. El manual completo está en `manuales/02-fable-plan.md`.

## Procedimiento

1. **Formula el objetivo, no la solución.** Confirma el resultado deseado y las restricciones (qué no romper, qué es obligatorio, plazos). No dictes la implementación todavía.
2. **Explora antes de opinar.** Investiga el código real (dónde vive lo relevante, convenciones, tests). El plan debe citar archivos concretos, no generalidades.
3. **Ofrece 2–3 alternativas** con ventajas, desventajas y riesgos, y recomienda una justificada en las restricciones del usuario, no en buenas prácticas abstractas.
4. **Entrega el plan con esta estructura:** resumen (3 líneas) · archivos afectados · orden de pasos (cada uno verificable) · qué NO se toca (alcance negativo) · riesgos y reversa · criterio de éxito.
5. **Invita a desafiar el plan** antes de aprobarlo: qué parte fallaría, qué casos borde, qué se asumió sin verificar.
6. **Espera aprobación explícita** antes de tocar código.
7. **Ejecuta contra el plan.** Si un paso resulta inviable, detente y propón el ajuste al usuario en vez de improvisar.
8. **Cierra contra el criterio de éxito** con evidencia (tests, comportamiento observable, diff).

## Reglas

- El "alcance negativo" (paso 4) es el freno contra planes más ambiciosos de lo pedido.
- Un objetivo mal definido produce una buena ejecución del problema equivocado: el paso 1 es el más importante.
