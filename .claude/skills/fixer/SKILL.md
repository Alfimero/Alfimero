---
name: fixer
description: Diagnosticar y reparar un problema de punta a punta — un bug, un test en rojo, un build roto o un error en producción. Úsala cuando algo falla, un test o CI está en rojo, "ayer funcionaba", hay una traza/stack trace o log de error, o el usuario pide arreglar/reparar/corregir un fallo.
---

# Fixer

Lleva un problema desde el reporte hasta una corrección verificada. El manual completo está en `manuales/04-fixer.md`.

## Procedimiento

1. **Recibe la evidencia literal** (traza, log, error completo; o entrada exacta + resultado obtenido vs. esperado). No trabajes sobre resúmenes.
2. **Reproduce antes de diagnosticar.** Ejecuta y muestra el comando y la salida. Si no puedes reproducir, di qué acceso o información falta — no adivines.
3. **Diagnostica la causa raíz**, distinguiendo dónde *explota* de dónde se *origina*, con evidencia en el código. Técnicas: `git bisect`/diff reciente, instrumentación temporal, ejemplo mínimo aislado.
4. **Congela el bug en un test** que falla con el código actual: eso define "arreglado".
5. **Corrección mínima** que ataca la causa raíz. No refactorices ni mejores código adyacente; lista aparte lo que veas de paso.
6. **Verifica en tres círculos:** (a) el test nuevo pasa, (b) la suite completa pasa, (c) el flujo/comando original del paso 1 ahora funciona. Muestra evidencia de cada uno.
7. **Revisa el diff** como si fuera ajeno: por qué corrige la causa (no el síntoma), qué otros lugares tienen el mismo patrón (bugs hermanos), en qué escenario podría romper algo.
8. **Integra** con un commit que explique la causa (el porqué). Si es un PR, ofrece vigilarlo y corregir CI en rojo con este mismo procedimiento.

## Reglas

- Silenciar o deshabilitar un test **no** es un arreglo: es decisión del usuario, nunca tuya.
- Para fallos intermitentes (flaky), repite la reproducción muchas veces ("ejecútalo 20 veces") antes de aceptar "ya no falla".
- Si el fallo no es reproducible en tu entorno, tus causas son hipótesis a verificar, no diagnóstico.
