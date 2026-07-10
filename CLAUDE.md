# Guía del proyecto para el agente

Este repositorio contiene manuales de procedimientos que el agente debe seguir cuando la tarea corresponda. Los manuales viven en `manuales/` y también están disponibles como Skills en `.claude/skills/`.

## Principio rector

El modelo propone y ejecuta; el usuario define el objetivo, aprueba lo irreversible y valida el resultado. Ante lo dudoso o difícil de revertir, consulta antes de actuar.

## Procedimientos disponibles

Cuando una tarea encaje con uno de estos temas, sigue el manual correspondiente:

| Situación | Manual a seguir |
|-----------|-----------------|
| Revisar seguridad (vulnerabilidades, secretos, dependencias) antes de un merge/deploy | [`manuales/01-chequeo-de-seguridad.md`](manuales/01-chequeo-de-seguridad.md) |
| Diseñar un cambio grande, ambiguo o difícil de revertir antes de ejecutarlo | [`manuales/02-fable-plan.md`](manuales/02-fable-plan.md) |
| Dar opinión o crítica honesta y accionable sobre un trabajo | [`manuales/03-opinion-con-critica-constructiva.md`](manuales/03-opinion-con-critica-constructiva.md) |
| Diagnosticar y reparar un bug, test rojo o build roto | [`manuales/04-fixer.md`](manuales/04-fixer.md) |

## Reglas transversales

- **Reproduce antes de reparar** y **verifica antes de dar por terminado** (tests + flujo real, no solo "parece que funciona").
- **Cambio mínimo:** no refactorices de más al corregir; lista aparte lo que veas de paso.
- **Nada de silenciar tests** para poner CI en verde: eso es decisión del usuario, no un arreglo.
- **Seguridad:** nunca apliques técnicas ofensivas contra sistemas ajenos o sin autorización; si aparecen secretos en git, hay que rotarlos.
