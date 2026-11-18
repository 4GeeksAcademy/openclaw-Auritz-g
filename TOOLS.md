# TOOLS.md — Conexiones actuales de Zapier

Inventario consultado el 2026-11-18. Describe las conexiones y acciones habilitadas en este momento; Zapier puede cambiar su catálogo o requerir volver a autorizar una cuenta.

## Reglas generales de uso

- Usar Zapier para acceder a estas cuentas autenticadas; no sustituirlo por una llamada web genérica.
- Antes de ejecutar una acción, inspeccionar la acción exacta para confirmar su nombre, tipo, parámetros requeridos y valores dinámicos. No adivinar nombres ni campos.
- Las acciones de lectura/búsqueda consultan datos. Las de escritura crean o modifican datos externos. Antes de enviar/alterar correos, modificar tareas, repositorios, archivos, carpetas, documentos o eventos, consultar al usuario y esperar confirmación, de acuerdo con sus preferencias.
- Usar la conexión predeterminada salvo que el usuario especifique otra cuenta. Si hay varias opciones aplicables, mostrar las opciones y preguntar cuál usar.
- No incluir identificadores internos de Zapier (IDs de conexión o `selected_api`) en respuestas visibles ni en documentos compartidos. Los IDs no se guardan en este archivo.
- Si falta autorización, la conexión está caducada o la acción no está habilitada, decirlo y solicitar la conexión/autorización necesaria; no afirmar que el trabajo se completó sin una respuesta de éxito.

## Google Docs

### Cuándo usarlo y valores predeterminados

Usar para buscar, leer, crear o editar documentos de Google Docs.

- Cuenta conectada y predeterminada: `eter0664@gmail.com`.
- Si no se indica una cuenta distinta, usar esa conexión.
- La carpeta de destino debe resolverse de forma inequívoca antes de crear o cargar un documento; no asumirla por su nombre si hay varias coincidencias.
- No hay un formato de documento global impuesto por la conexión: respetar el contenido, título, plantilla y formato pedidos. La creación desde texto admite texto y HTML limitado.

### Acciones habilitadas

**Lectura / búsqueda / disparadores**

- `get_latest_document` — disparador de documento nuevo en cualquier carpeta.
- `document_in_folder` — disparador de documento nuevo en una carpeta específica, no sus subcarpetas.
- `document_updated` — disparador de documento actualizado.
- `document` (Find a Document) — buscar documento por nombre.
- `get_document_content` — obtener contenido y metadatos por ID.
- `get_document_tabs_content` — obtener contenido y metadatos con información de pestañas.
- `find_text` — localizar texto y devolver índices de inicio y fin.
- `_zap_raw_request` (GET) — solicitud de lectura a la API de Google Docs.

**Escritura / modificación**

- `newtxtdocument` — crear documento desde texto.
- `create_from_template` — crear desde plantilla y sustituir marcadores.
- `document` (Upload Document) — cargar un archivo ya existente y convertirlo a Google Docs si es posible.
- `insert_text`, `append` — insertar o añadir texto.
- `find_and_replace` — buscar y reemplazar texto.
- `format_text` — aplicar formato, colores, enlaces y estilos.
- `insert_image`, `replace_image_v0` — insertar o reemplazar imágenes.
- `update_document_properties` — cambiar propiedades de página, márgenes y fondo.
- `_zap_raw_request` (mutating request) — solicitud de escritura a la API; solo si las acciones normales no cubren la operación y con confirmación.

Nota: el identificador `document` se usa para dos acciones distintas; distinguirlas siempre por el nombre y `tool_name` inspeccionados, nunca solo por la clave.

## Google Calendar

### Cuándo usarlo y valores predeterminados

Usar para consultar calendarios/eventos o, con confirmación, crear, cambiar, mover, cancelar o borrar eventos.

- Cuenta conectada y predeterminada: `eter0664@gmail.com`.
- Calendario principal confirmado: `eter0664@gmail.com`.
- Zona horaria del calendario principal: `UTC`. Si el usuario especifica otra zona horaria, respetarla explícitamente; no convertir silenciosamente horas locales.
- Los eventos detallados requieren hora de inicio y fin, excepto los de día completo; confirmar duración si no puede deducirse con seguridad.
- Búsqueda de eventos: la acción `event_v2` puede devolver hasta 250 coincidencias. Acotar fechas y criterios para evitar confundir eventos similares.

### Acciones habilitadas

**Lectura / búsqueda / disparadores**

- `find_calendars` — listar calendarios accesibles y sus propiedades (hasta 250).
- `get_calendar_info` — obtener zona horaria, permisos y metadatos de un calendario.
- `event_v2` — buscar eventos (hasta 250 coincidencias).
- `event_by_id` — recuperar un evento por ID.
- `find_busy_periods` — consultar periodos ocupados.
- `search` — disparador de eventos nuevos o actualizados que coincidan con criterios.
- `new_event`, `event_updated`, `event_cancelled` — disparadores de cambios de eventos.
- `event_start`, `event_ended_v2` — disparadores relativos al inicio o fin de un evento.
- `calendar` (New Calendar) — disparador de calendario creado.
- `_zap_raw_request` (GET) — solicitud de lectura a la API de Google Calendar.

**Escritura / modificación**

- `detailed_event` — crear evento con campos detallados.
- `event` (Quick Add Event) — crear evento interpretando texto natural; preferir campos explícitos si fecha/hora o zona son importantes.
- `update_event` — actualizar campos indicados; los campos vacíos no se actualizan.
- `delete_event` — borrar un evento.
- `move_event` — mover a otro calendario.
- `add_attendees`, `remove_attendees` — añadir o quitar participantes; invitar a alguien es una acción externa, confirmar destinatarios.
- `calendar` (Create Calendar) — crear calendario.
- `_zap_raw_request` (mutating request) — solicitud de escritura a la API; evitar si existe una acción específica.

Nota: las claves `calendar` y `_zap_raw_request` corresponden a acciones distintas. Identificarlas por nombre y `tool_name` inspeccionados.

## Google Drive

### Cuándo usarlo y valores predeterminados

Usar para localizar carpetas y crear carpetas vacías. Para contenido de documentos, usar Google Docs cuando proceda.

- Cuenta conectada: `eter0664@gmail.com`; usarla por defecto si no se indica otra.
- La cuenta aparece conectada y sin caducidad registrada.
- Antes de crear una carpeta, buscar primero para evitar duplicados y confirmar ruta/nombre si hay ambigüedad.

### Acciones habilitadas

**Lectura / búsqueda / disparador**

- `folder_v2` — buscar una carpeta por nombre.
- `folder` (New Folder) — disparador de una carpeta nueva directamente dentro de una carpeta determinada, no sus subcarpetas.

**Escritura**

- `folder` (Create Folder) — crear una carpeta vacía.

Nota: `folder` se utiliza tanto para el disparador como para la creación. Distinguir por nombre y `tool_name` inspeccionados.

## Gmail

### Cuándo usarlo y valores predeterminados

Usar para buscar y leer correos y conversaciones, consultar adjuntos, crear borradores, organizar mensajes y, con confirmación explícita, enviar, responder, reenviar, archivar o eliminar correos.

- Cuenta conectada: `eter0664@gmail.com` (una conexión disponible); usarla salvo que se solicite otra cuenta.
- Las búsquedas usan la sintaxis y operadores de Gmail; especificar filtros como `in:inbox`, `from:` o `subject:` cuando convenga acotar.
- Por defecto, los mensajes nuevos y borradores son texto plano (`body_type: plain`); `send_to_groups` es `false`, delimitador de firma `true`, y conservar HTML de firma `false`.
- Obtener conversación usa formato `full` por defecto; incluye encabezados, cuerpo codificado y metadatos de adjuntos, pero no descarga los archivos.

### Acciones habilitadas

**Lectura / búsqueda / disparadores**

- `get_latest_attachment` — nuevo adjunto.
- `label` (New Label), `label_message_v2` (New Labeled Email), `labeled_conversation` (New Labeled Conversation), `get_latest_message` (Get Latest Email), `search` (New Email Matching Search), `star_message` (New Starred Email), `thread_v2` (New Conversation) — disparadores de cambios de Gmail.
- `attachment` — obtener adjunto por ID de mensaje y nombre de archivo (tamaño opcional si el nombre no basta).
- `message` (Find Email) — buscar mensaje mediante consulta de Gmail.
- `get_conversation` — recuperar una conversación por ID de hilo (Zapier expone esta lectura a través de su herramienta de escritura).

**Escritura / modificación**

- `add_label`, `remove_label`, `remove_thread_label`, `label` (Create Label) — gestionar etiquetas.
- `archive_email`, `delete_email` — archivar o enviar un mensaje a la papelera.
- `draft_v2`, `draft_v2_reply` — crear borrador o borrador de respuesta.
- `message` (Send Email), `reply_to_message`, `forward_email` — enviar, responder o reenviar; requieren confirmación antes del envío.

Nota: las claves `label` y `message` corresponden a acciones distintas; distinguirlas por nombre y `tool_name` inspeccionados.

## Google Tasks

### Cuándo usarlo y valores predeterminados

Usar para listar y buscar tareas, observar tareas/listas nuevas o completadas y, con confirmación, crear o modificar tareas y listas.

- Una conexión disponible, titulada `Google Tasks`; usarla salvo que se solicite otra cuenta.
- Las tareas se asocian a una lista específica; resolver la lista mediante sus opciones dinámicas antes de leer o escribir. No asumir una lista predeterminada.
- `Get Tasks by List` excluye las completadas por defecto (`show_completed: false`).
- La fecha de vencimiento es opcional; no inventar una fecha si no se indicó.

### Acciones habilitadas

**Lectura / búsqueda / disparadores**

- `tasks` — nueva tarea en una lista.
- `completed_task` — tarea completada en una lista.
- `task_list` — nueva lista de tareas.
- `find_task` — buscar tarea incompleta por título y lista.
- `get_tasks_by_list` — listar tareas de una lista, opcionalmente incluyendo completadas.
- `_zap_raw_request` (GET) — solicitud GET a la API de Google Tasks.

**Escritura / modificación**

- `task` — crear tarea (título obligatorio, lista obligatoria; vencimiento y notas opcionales).
- `update_task` — cambiar título, vencimiento, notas o estado de una tarea identificada en su lista.
- `create_task_list` — crear una lista.
- `_zap_raw_request` (mutating request) — solicitud de escritura a la API; reservar para operaciones sin acción específica y con confirmación.

Nota: `_zap_raw_request` identifica tanto una acción GET como otra mutativa. Diferenciarlas por nombre y `tool_name` inspeccionados.

## GitHub

### Cuándo usarlo y valores predeterminados

Usar para buscar repositorios, ramas, issues, pull requests, usuarios u organizaciones; consultar cambios/eventos y, con confirmación, crear o modificar contenido y elementos del repositorio.

- Una conexión disponible, titulada `Auritz-g`; usarla salvo que se solicite otra cuenta.
- `Find Repository` usa el usuario/cuenta conectada como propietario si se omite `owner`; indicar propietario u organización si corresponde a otro.
- Para buscar issue, el estado predeterminado es `all`; elegir número o título como criterio de búsqueda.
- Para crear pull request, la rama base predeterminada declarada por Zapier es `master`; `instant_merge` es `no`. No asumir que esa rama existe: resolver y verificar ramas antes de operar.
- Las operaciones que modifican el repositorio requieren elegir explícitamente el repositorio y, según la acción, rama, issue o pull request correctos.

### Acciones habilitadas

**Lectura / búsqueda / disparadores**

- Disparadores: `release` (New Release), `repo` (New Repository), `pull` (New Pull Request), `get_latest_branch` (New Branch), `collaborator` (New Collaborator), `comment` (New Commit Comment), `commit` (New Commit), `event` (New Repo Event), `gist` (New Gist), `global_event` (New Global Event), `issue_closed` (Issue Closed), `issue_v2` (New Issue), `label` (New Label), `mention` (New Mention), `milestone` (New Milestone), `notification` (New Notification), `get_latest_organization` (New Organization), `review_request` (New Review Request), `user_team` (New Team), `watcher` (New Watcher).
- Búsquedas: `user` (Find User), `branch` (Find Branch), `get_file_contents` (Get File Contents), `members` (Check Organization Membership), `organization` (Find Organization), `repository_v2` (Find Repository), `repo_issue` (Find Issue), `repo_pull` (Find Pull Request).

**Escritura / modificación**

- `create_file` — crear o actualizar archivo en repositorio (actualizar requiere SHA del archivo existente).
- `delete_branch`, `create_branch` — borrar o crear rama.
- `gist` — crear Gist.
- `issue`, `update_issue`, `add_labels_to_issue` — crear/actualizar issue y añadir etiquetas.
- `comment` — comentar en issue o pull request.
- `pull_request`, `update_pull_request` — crear o actualizar pull request; la creación permite merge inmediato, desactivado por defecto.
- `setProfileStatus` — cambiar estado del perfil.
- `submit_review` — enviar una revisión de pull request.

Nota: ciertas claves (`comment`, `gist`, `issue`, `pull_request`) se comparten entre disparadores/lecturas y acciones de escritura; distinguir siempre por nombre y `tool_name`.

## Convenciones

### 3.1 Lista por defecto

- No existe una lista universal de todos los registros para estas conexiones; elegir la acción de búsqueda adecuada y delimitarla por nombre, ID, calendario o intervalo de fechas.
- Google Calendar: `find_calendars` y `event_v2` admiten hasta 250 resultados; reducir el intervalo o criterios cuando sea posible.
- Google Docs: `document` (Find a Document) busca por nombre. Para recuperar contenido se necesita un ID de documento.
- Google Drive: `folder_v2` busca por nombre; no equivale a listar todo el Drive.
- Gmail: `message` busca con operadores Gmail; no hay una carpeta/lista universal implícita para una búsqueda.
- Google Tasks: no se ha configurado una lista de tareas por defecto; elegir una lista concreta antes de listar, buscar o crear tareas.
- GitHub: no hay un repositorio predeterminado documentado; elegir y verificar el repositorio antes de una operación. En `Find Issue`, el estado por defecto es `all`.
- No presentar un resultado único de búsqueda como inequívoco si la solicitud puede referirse a varios elementos; mostrar candidatos y pedir selección.
- Los disparadores de Zapier (`New`, `Updated`, `Event Start`, etc.) son para observar cambios/workflows, no sustituyen una búsqueda histórica general.

### 3.2 Formato

- Fechas/horas para Calendar: usar ISO 8601 con zona u offset explícito; si se usa el calendario principal, expresar `UTC` (por ejemplo, `2026-11-17T12:00:00Z`). Incluir inicio, fin y zona al confirmar eventos.
- Consultas de Calendar: indicar el intervalo y calendario considerados; diferenciar hora local de UTC.
- Documentos: confirmar título y carpeta de destino; conservar el formato solicitado y declarar si HTML es limitado al crear desde texto.
- Carpetas: indicar nombre y ubicación/ruta; no confundir una carpeta homónima con la ubicación correcta.
- Gmail: usar la consulta de búsqueda en sintaxis Gmail; redactar por defecto en texto plano. Confirmar destinatarios, asunto y contenido antes de enviar, responder o reenviar.
- Google Tasks: indicar título y lista; fechas de vencimiento con formato de fecha/hora válido y explícito, sin inferir fechas ambiguas.
- GitHub: referirse a repositorios como `propietario/repositorio`; incluir rama base/cabecera y título para pull requests, y diferenciar número de issue/PR. No hacer merge instantáneo salvo instrucción expresa.
- Al informar resultados: respuesta breve con qué se encontró o modificó, cuenta/calendario/carpeta pertinente y enlace o identificador compartible cuando Zapier lo devuelva. Nunca mostrar IDs internos de conexión o de catálogo.
- Distinguir siempre acciones de lectura, disparadores y escritura. La escritura requiere confirmación previa del usuario, incluso cuando la acción parezca rutinaria.
