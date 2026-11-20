# SKILL_LOG — Historial de diseño de las skills 4Geeks

Este documento reúne un registro cronológico de las conversaciones recuperables relacionadas con el diseño, creación, pruebas y cambios de las seis skills de 4Geeks. Es un resumen de decisiones y resultados, no una transcripción literal completa. El historial buscable de sesiones devolvió algunos resultados, pero no necesariamente todos; por eso se distingue lo recuperado explícitamente de lo que no se pudo reconstruir. No se guardan credenciales.

## Skills

1. `breathecode-auth` — visible como «Autenticar 4 Geeks».
2. `breathecode-projects` — visible como «Estado de proyectos».
3. `breathecode-pending-work` — «Obtener trabajo pendiente».
4. `breathecode-progress` — «Resumen del progreso».
5. `breathecode-daily-review` — antes `breathecode-next-class-summary`.
6. `breathecode-learning-checks` — tests y refuerzo de aprendizaje.

## Cronología recuperada

### Diseño inicial de skills — 18/11/2026 (según marcas de tiempo del historial)

- El usuario indicó que aún no quería crear ninguna skill; se acordó dejar la implementación para más adelante.
- Después pidió crear la carpeta `SKILLS` y, dentro, `SKILLS_DESIGN.md` como especificación previa: reunir respuestas a tres preguntas por skill, manteniendo la implementación de cada skill en un archivo separado.
- Se creó `SKILLS/SKILLS_DESIGN.md` con una plantilla de tres preguntas y respuestas por completar. El historial recuperado confirma esta estructura, no todas las respuestas posteriores del archivo.
- En otra conversación se pidió revisar el correo del día y, si había un correo importante, implementar una skill para crear un borrador. El fragmento recuperado no permite determinar que esa skill se incorporara al conjunto final de seis.

### Primera skill: autenticación — 19/11/2026

- El usuario pidió implementar la primera skill.
- Se creó `breathecode-auth` para comprobar la configuración de `TOKEN_4GEEKS` mediante una petición GET de solo lectura al endpoint de perfil.
- La instrucción prohíbe mostrar, solicitar por chat o guardar el token y evita conservar o imprimir campos del perfil. Diferencia token aceptado de autorización a otros endpoints.
- La prueba informada en la conversación confirmó token aceptado y sesión activa en ese endpoint; posteriormente se volvió a verificar HTTP 200 durante la auditoría.
- El usuario preguntó qué ocurriría al ejecutar la skill; se explicó que reporta estado de autenticación, sin exponer token ni datos personales.
- El usuario pidió cambiar el nombre visible a «Autenticar 4 Geeks». Se conservó el identificador técnico `breathecode-auth` y se actualizó la descripción visible por restricciones del cargador de skills.

### Segunda skill: proyectos asignados — 19/11/2026

- El usuario definió la segunda skill para recuperar la lista de proyectos asignados y su estado actual.
- Se precisó usar estados documentados, sin inferirlos. El usuario confirmó: «utilizaremos los estados documentados» y pidió crearla y probarla.
- Se creó `breathecode-projects`, con consultas GET de solo lectura, paginación acotada y salida limitada a nombre, identificador y estado documentado; ante errores debe informarlos sin modificar asignaciones.
- La prueba informó 21 proyectos: 19 `DONE` y 2 `PENDING`.
- El usuario pidió que su nombre visible fuera «Estado de proyectos». Se mantuvo `breathecode-projects` como nombre interno y se cambió la descripción visible.

### Tercera skill: trabajo pendiente — 19/11/2026

- El usuario la definió como una forma de saber específicamente qué falta completar.
- Se creó `breathecode-pending-work` usando lecturas de tareas tipo proyecto. `PENDING` se reporta como pendiente y `REJECTED` como requiere correcciones; `DONE` y `APPROVED` se excluyen del pendiente. Estados desconocidos se conservan como no documentados.
- El usuario pidió crearla y probarla. La prueba reportada encontró dos proyectos pendientes entre los 21 asignados. No modifica ni entrega tareas.

### Cuarta skill: progreso — 19/11/2026

- El usuario la definió como una visión general de cuánto ha avanzado en el curso y pidió crearla y probarla, incluyendo el porcentaje real y el estimado.
- Se creó `breathecode-progress` para contar estados de tareas asignadas, separar trabajo completado por estudiante de lo aprobado en revisión y mantener desconocidos fuera del denominador documentado.
- Se añadió una cautela: la lista de tareas asignadas no constituye el denominador de todo el curso. El porcentaje se presenta como proxy de tareas asignadas; no se inventa un progreso total del curso.
- La prueba inicial informó 108 tareas, 97 `DONE` y 11 `PENDING`; la posterior auditoría, al volver a consultar, obtuvo 111 tareas, 97 `DONE` y 14 `PENDING`. Son resultados de momentos distintos.

### Quinta y sexta skills: clases, Drive y tests — 20/11/2026

- El usuario preguntó si los documentos de Google podían servir para verificar la materia al generar un test. Se confirmó el uso de Drive conectado, con la salvedad de verificar acceso y legibilidad del contenido.
- Se pidió un resumen de la próxima clase, entregado un día antes, y se planteó la skill 5 como `breathecode-next-class-summary`; debía respetar el orden confirmado de módulos, fechas/calendario y material teórico verificable.
- Al consultar *Frontend development with Coding Agents*, se encontró que la skill de proyectos solo devuelve tareas asignadas, no la organización del temario. La consulta de cohortes activas devolvió HTTP 403 y el catálogo público no presentó coincidencia para ese título exacto; no se afirmó un temario sin fuente.
- El usuario aclaró la lista y el orden de las seis skills, y que se refería a la quinta. Se explicó el procedimiento previsto para el resumen, manteniendo claro que aún no se podía verificar el contenido del módulo.
- El usuario cambió el objetivo de la quinta: relacionarla con lo ya visto en clases, reflejado en la carpeta de Google Drive conectada a 4Geeks, y hacer cada día una pregunta de repaso sobre un archivo de allí.
- Se reescribió la quinta skill: seleccionar material legible de clases pasadas, formular una pregunta diaria de recuperación activa sin respuesta/pistas, esperar el intento para corregir, no inventar contenido, no guardar teoría localmente y mantener consultas Drive de solo lectura.
- Se renombró de `breathecode-next-class-summary` a `breathecode-daily-review`, actualizando el directorio, el metadato `name` y la referencia hallada en `breathecode-learning-checks`. La comprobación local pasó, aunque el cargador `skills_read` no reconoció el nombre nuevo en ese momento; disponibilidad en índice quedó sin verificar.
- La programación diaria no se creó: la hora no se había especificado. Se comprobó una automatización existente de tests lunes, miércoles y viernes a las 17:00 `Europe/Madrid`; se preservó y no se duplicó ni sustituyó.
- La sexta skill, `breathecode-learning-checks`, requiere identificar el módulo correcto, fundamentar preguntas en teoría oficial legible, presentar primero solo preguntas y corregir después del intento. No se completó una prueba de test con material oficial porque los PDFs encontrados no pudieron leerse por las vías probadas.

### Auditoría funcional de las seis — 20/11/2026

- El usuario pidió revisar implementación y funcionamiento desde la autorización académica hasta la creación de tests.
- Se leyeron las seis instrucciones. La verificación de perfil dio HTTP 200; esto confirma aceptación para ese endpoint, no acceso global. Las peticiones a tareas y proyectos se hicieron en solo lectura.
- En esa ejecución, proyectos y pendientes devolvieron 21 proyectos, con dos `PENDING`; progreso devolvió 111 tareas: 97 `DONE`, 14 `PENDING`, 0 `APPROVED`, 0 `REJECTED`, y 87,4 % de completitud de tareas asignadas. No se calculó porcentaje real del curso por falta de denominador.
- Los cuatro helpers Python existentes pasaron `py_compile`; se ejecutaron y funcionaron los helpers de listado de proyectos, pendientes y progreso. La quinta y sexta son instrucciones sin helper ejecutable.
- La prueba de resumen de clase y la creación de test basado en teoría oficial quedaron bloqueadas por falta de secuencia confirmada y material legible; no se improvisó contenido.
- Este registro y sus resultados se limitan a lo recuperado del historial y del turno de auditoría; no son una prueba de todos los resultados históricos.

## Reglas y decisiones que deben conservarse

- Consultas académicas y Drive en modo lectura; nunca pedir o revelar credenciales ni guardar tokens.
- Un HTTP 200 del perfil no autoriza por sí mismo otros endpoints.
- No presentar suposiciones como teoría oficial; basar preguntas/resúmenes en archivos legibles y clases ya vistas.
- En tests, entregar primero solo las preguntas, sin pistas ni respuestas; corregir tras la respuesta del estudiante.
- Una pregunta o test aislado no prueba dominio completo.
- No modificar ni duplicar la automatización de tests existente. Para el repaso diario falta una hora elegida antes de programar.
- Mantener el resumen diario como registro derivado y parcial; para recuperar literalmente todas las conversaciones hay que consultar el historial/transcripciones disponibles, que no se pudo garantizar como completo mediante la búsqueda usada.