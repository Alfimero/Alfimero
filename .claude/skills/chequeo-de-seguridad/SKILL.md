---
name: chequeo-de-seguridad
description: Revisar código, configuración o dependencias en busca de vulnerabilidades de seguridad. Úsala cuando el usuario pida un chequeo o auditoría de seguridad, revisar antes de un merge/deploy sensible, o buscar inyecciones (SQL/comandos/XSS), secretos expuestos, fallos de autenticación/autorización (IDOR), validación de entradas débil, configuraciones peligrosas o dependencias vulnerables.
---

# Chequeo de seguridad

Sigue este procedimiento para revisar la seguridad de un proyecto. El manual completo y detallado está en `manuales/01-chequeo-de-seguridad.md`; léelo si necesitas el porqué de cada paso.

## Procedimiento

1. **Define alcance y modelo de amenaza.** Pregunta o confirma qué revisar (repo/diff/módulo) y contra qué (quién ataca, qué se protege). Sin alcance, hay ruido.
2. **Inventaria la superficie de ataque.** Lista puntos de entrada: endpoints, parámetros, webhooks, archivos leídos, variables de entorno. Valida la lista con el usuario.
3. **Análisis automatizado.** Ejecuta las herramientas del stack (auditoría de dependencias, escáner de secretos, análisis estático) e interpreta la salida, descartando falsos positivos evidentes con justificación.
4. **Revisión manual por categorías**, en orden: inyección → autenticación/sesiones → autorización → secretos → validación de entradas → configuración. Para cada hallazgo: archivo:línea, severidad, escenario de explotación y corrección propuesta.
5. **Exige escenario de explotación** para cada hallazgo crítico/alto. Sin escenario realista, reclasifícalo como "defensa en profundidad", no como vulnerabilidad.
6. **Prioriza.** El usuario decide qué se corrige ahora, qué se agenda y qué se acepta como riesgo.
7. **Corrige de a un hallazgo**, con el cambio mínimo, y muestra el diff.
8. **Verifica:** re-ejecuta tests y escáner, re-revisa el diff de la corrección, y documenta los riesgos aceptados.

## Reglas

- No garantices ausencia de vulnerabilidades; reduces riesgo, no lo eliminas.
- Nunca apliques estas técnicas contra sistemas que no pertenecen al usuario o sin autorización.
- Si aparecen secretos en el historial de git, hay que **rotarlos**, no solo borrarlos.
