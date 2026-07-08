# Manual 1 — Chequeo de seguridad

Manual paso a paso para realizar una revisión de seguridad de un proyecto (código, configuración y dependencias) usando un modelo de IA actual.

---

## 1. Qué es

Un chequeo de seguridad asistido por IA consiste en darle al modelo acceso de lectura a tu proyecto y pedirle que busque vulnerabilidades de forma sistemática: inyecciones, manejo inseguro de secretos, validación de entradas, control de acceso, dependencias vulnerables y configuraciones peligrosas. Los modelos actuales pueden ejecutar este proceso de forma casi autónoma: leen el código, ejecutan herramientas de análisis, correlacionan hallazgos y priorizan por severidad.

**Qué puede ejecutar el modelo hoy:**
- Leer y entender bases de código completas (múltiples lenguajes a la vez).
- Ejecutar linters y escáneres (`npm audit`, `pip-audit`, `semgrep`, `gitleaks`, etc.) e interpretar su salida.
- Rastrear el flujo de datos desde una entrada de usuario hasta un punto peligroso (base de datos, shell, HTML).
- Proponer y aplicar correcciones, y verificar que los tests siguen pasando.

**Qué NO debes esperar:** que reemplace un pentest profesional, que garantice ausencia de vulnerabilidades, o que evalúe infraestructura a la que no tiene acceso.

## 2. Cuándo usarlo

- Antes de fusionar (merge) una rama con cambios sensibles: autenticación, pagos, manejo de archivos, endpoints nuevos.
- Antes de publicar un proyecto o abrir su código.
- Periódicamente (por ejemplo, una vez por sprint) como higiene general.
- Después de incorporar dependencias nuevas.

## 3. Requisitos

- [ ] Acceso del modelo al código fuente (repositorio local o remoto autorizado).
- [ ] Autorización para revisar ese código (debe ser tuyo o de tu organización).
- [ ] Idealmente: tests que pasen en verde antes de empezar, para poder verificar correcciones.
- [ ] Definir el **alcance**: ¿todo el repo, solo el diff de una rama, solo un módulo?

## 4. Procedimiento paso a paso

### Paso 1 — Define el alcance y el modelo de amenaza
Dile al modelo *qué* revisar y *contra qué*. Un chequeo sin alcance produce ruido.

> **Prompt de ejemplo:**
> "Haz un chequeo de seguridad del directorio `src/api/`. La aplicación es una API pública en internet con usuarios autenticados por JWT. Me preocupan especialmente: inyección SQL, autorización rota entre usuarios (IDOR) y fuga de secretos. No revises los tests."

### Paso 2 — Pide un inventario de superficie de ataque
Antes de buscar bugs, el modelo debe mapear por dónde entra la información.

> "Antes de buscar vulnerabilidades, lista todos los puntos de entrada: endpoints HTTP, parámetros que aceptan, colas o webhooks que se consumen, archivos que se leen, y variables de entorno usadas."

Revisa esa lista: si falta un punto de entrada que tú conoces, corrígelo ahora.

### Paso 3 — Ejecuta el análisis automatizado
Pide al modelo que corra las herramientas disponibles y consolide resultados:

> "Ejecuta las herramientas de auditoría disponibles para este stack (auditoría de dependencias, escáner de secretos, análisis estático). Resume los hallazgos y descarta los falsos positivos evidentes, explicando por qué los descartas."

El modelo puede instalar y ejecutar estas herramientas él mismo si tiene acceso a la terminal.

### Paso 4 — Pide la revisión manual guiada por categorías
Esta es la parte donde el modelo aporta más valor que un escáner: entiende el contexto.

> "Ahora revisa manualmente el código con estas categorías, en orden:
> 1. **Inyección** (SQL, comandos de shell, plantillas, XSS).
> 2. **Autenticación y sesiones** (tokens, expiración, comparaciones inseguras).
> 3. **Autorización** (¿cada endpoint verifica que el recurso pertenece al usuario?).
> 4. **Secretos** (claves en el código, en logs, en mensajes de error).
> 5. **Validación de entradas** (tamaños, tipos, rutas de archivo, deserialización).
> 6. **Configuración** (CORS, cabeceras, modo debug, permisos de archivos).
>
> Para cada hallazgo dame: archivo y línea, severidad (crítica/alta/media/baja), un escenario concreto de explotación y la corrección propuesta."

### Paso 5 — Exige el escenario de explotación
Un hallazgo sin escenario suele ser un falso positivo. Pide siempre:

> "Para el hallazgo #2, descríbeme paso a paso cómo un atacante lo explotaría. Si no puedes construir un escenario realista, reclasifícalo como 'defensa en profundidad' en vez de vulnerabilidad."

### Paso 6 — Prioriza y decide
Con la lista final, decide tú qué se corrige ahora, qué se agenda y qué se acepta como riesgo. El modelo puede ayudarte a priorizar, pero la decisión de aceptar un riesgo es humana.

### Paso 7 — Aplica correcciones (opcional, una por una)
Si quieres que el modelo corrija:

> "Corrige el hallazgo #1 con el cambio mínimo necesario. No refactorices nada más. Después ejecuta los tests y muéstrame el diff."

Corregir de a un hallazgo por vez hace el diff revisable y evita regresiones.

### Paso 8 — Verifica
- Pide al modelo que re-ejecute los tests y el escáner sobre el código corregido.
- Pide una re-revisión del diff de la corrección misma (una corrección puede introducir otro bug).
- Registra los hallazgos aceptados-sin-corregir en un documento del proyecto.

## 5. Lista de verificación final

- [ ] El alcance revisado coincide con el que definí.
- [ ] Cada hallazgo crítico/alto tiene escenario de explotación verificado.
- [ ] Las correcciones aplicadas tienen tests en verde.
- [ ] No quedaron secretos en el historial de git (si aparecieron, hay que rotarlos, no solo borrarlos).
- [ ] Los riesgos aceptados quedaron documentados con responsable y fecha.

## 6. Límites conocidos

- El modelo solo ve lo que le das: no detecta problemas de infraestructura, red o procesos humanos.
- Puede producir falsos positivos con confianza; por eso el Paso 5 es obligatorio.
- Un chequeo en verde **no** certifica seguridad; reduce riesgo. Para sistemas críticos, complementa con auditoría profesional.
- Nunca pidas ni uses estas técnicas contra sistemas que no te pertenecen o sin autorización escrita.
