# Manuales de capacidades de IA

Colección de manuales paso a paso pensados para aprovechar las capacidades que los modelos de IA actuales (como la familia Claude 5) pueden ejecutar hoy: analizar código y documentos, planificar trabajo, dar retroalimentación estructurada y diagnosticar/corregir problemas de punta a punta.

Cada manual sigue la misma estructura: **qué es**, **cuándo usarlo**, **requisitos**, **procedimiento paso a paso**, **ejemplos de instrucciones (prompts)**, **lista de verificación** y **límites conocidos**.

## Índice

| # | Manual | Descripción |
|---|--------|-------------|
| 1 | [Chequeo de seguridad](manuales/01-chequeo-de-seguridad.md) | Cómo pedirle a un modelo de IA que revise código, configuraciones y dependencias en busca de vulnerabilidades, y cómo validar sus hallazgos. |
| 2 | [Fable Plan](manuales/02-fable-plan.md) | Cómo usar el modo de planificación de un modelo avanzado para diseñar el trabajo antes de ejecutarlo: explorar, proponer, aprobar y ejecutar. |
| 3 | [La opinión con crítica constructiva](manuales/03-opinion-con-critica-constructiva.md) | Cómo obtener del modelo una opinión honesta y útil sobre un trabajo (código, texto, diseño o decisión), con crítica que se pueda accionar. |
| 4 | [Fixer](manuales/04-fixer.md) | Cómo usar el modelo como "reparador": reproducir un problema, diagnosticar la causa raíz, aplicar la corrección mínima y verificarla. |

## Cómo usar estos manuales

1. Elige el manual que corresponde a tu necesidad del momento.
2. Sigue los pasos en orden; cada paso indica qué haces tú y qué hace el modelo.
3. Copia y adapta los prompts de ejemplo — están escritos para funcionar con asistentes de codificación agénticos (Claude Code, etc.) pero también sirven en un chat normal.
4. Usa la lista de verificación final antes de dar el trabajo por terminado.

## Principio común a los cuatro manuales

Los modelos actuales pueden **leer, razonar, ejecutar herramientas y verificar**, pero el criterio final es tuyo. La regla general es:

> El modelo propone y ejecuta; tú defines el objetivo, apruebas lo irreversible y validas el resultado.
