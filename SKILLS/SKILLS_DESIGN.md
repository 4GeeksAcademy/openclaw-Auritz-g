# Diseño de skills

Este archivo reúne las especificaciones de cada skill antes de implementarla. Cada implementación irá en un archivo Markdown independiente.

## Skill 1: Gmail Triage & Summary

**Archivo previsto:** `gmail-triage-summary.md`

### 1. ¿Qué hace esta skill?

Revisa, filtra y clasifica los correos no leídos de Gmail al inicio y al final del día, genera un resumen estructurado que prioriza los mensajes importantes, lista candidatos a spam y cuenta los correos informativos que no requieren prisa.

### 2. ¿Qué input necesita el agente? ¿Qué le das, en qué formato y qué ya sabe?

**Inputs en tiempo de ejecución:**

- Disparador programado de inicio o fin de jornada, o petición explícita del usuario en lenguaje natural.
- Resultados de la consulta a Gmail: lista de objetos JSON con remitente, asunto, fecha y cuerpo del mensaje.

**Contexto de configuración que se espera consultar:**

- `TOOLS.md`: cómo buscar en Gmail y criterios de desempate.
- `IDENTITY.md` y `USER.md`: identidad, contexto del usuario y remitentes o proyectos críticos.
- Reglas para priorizar urgencias y guardar reportes en la ruta designada.
- Ruta por defecto del workspace para guardar archivos.

**Disparadores automáticos solicitados:** revisar los correos todos los días a las **08:00 y 20:00**, en la zona horaria `Europe/Madrid` (horario de Madrid, España).

### 3. ¿Cómo es un buen output? Formato, destino y cómo sabrás que funcionó.

- **Formato:** archivo Markdown llamado `gmail-triage-YYYY-MM-DD-[inicio|fin].md`, con tres secciones: (1) correos prioritarios e importantes con extracto breve, (2) candidatos propuestos para marcar como spam y (3) número de correos pendientes sin prisa.
- **Destino:** sistema de archivos local, dentro de `workspace/`.
- **Verificación de éxito:** el archivo existe en la ruta prevista, incluye la fecha correcta, clasifica los mensajes de acuerdo con su contenido y no hay errores de conexión en Zapier/Gmail.

## Skill 2: Gmail Critical Draft Generator

**Archivo previsto:** `gmail-critical-draft-generator.md`

### 1. ¿Qué hace esta skill?

Genera un documento estructurado en la carpeta de Google Drive `BORRADORES-GMAIL` por cada correo crítico detectado, con un resumen, aspectos clave y una propuesta de respuesta para revisión del usuario.

### 2. ¿Qué input necesita el agente? ¿Qué le das, en qué formato y qué ya sabe?

**Inputs en tiempo de ejecución:** listado de correos marcados como importantes o críticos por la skill anterior, en texto estructurado o JSON, incluyendo el contenido íntegro del correo.

**Contexto de configuración que se espera consultar:**

- `TOOLS.md`: acciones disponibles para Google Drive/Docs y sus permisos.
- `IDENTITY.md` y `USER.md`: tono profesional y estilo de comunicación del usuario.
- Reglas y configuración del workspace: ubicación de destino `BORRADORES-GMAIL` y convención para el título del Google Doc: `YYYY-MM-DD - [Asunto/Remitente]` (sin extensión de archivo).

### 3. ¿Cómo es un buen output? Formato, destino y cómo sabrás que funcionó.

- **Formato:** documento con tres apartados: resumen del correo recibido, aspectos críticos/puntos clave y propuesta de borrador de respuesta.
- **Destino:** carpeta `BORRADORES-GMAIL` de Google Drive.
- **Tipo de archivo:** documento nativo de Google Docs (no archivo Markdown), con título `YYYY-MM-DD - [Asunto/Remitente]`. Debe permitir al usuario revisar primero el correo y decidir si usa la propuesta; generar el borrador para revisión, sin enviarlo automáticamente.
- **Verificación de éxito:** el documento de Google Docs se creó en la carpeta correcta, su contenido refleja fielmente las preguntas o necesidades del correo, y la propuesta está lista para que el usuario la revise, copie o edite.

## Decisiones pendientes

- Las referencias a `rules.md` se mantienen por ahora; no crear ni sustituir ese archivo en esta etapa.
- `workspace.md` también se menciona como fuente de la ruta por defecto, pero no existe actualmente en el workspace. No asumir ni crear ese archivo sin indicación.
