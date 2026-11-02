# ADR-002: Comportamiento ante la caída del servicio de autenticación

- **Estado:** Propuesto
- **Escenario:** D1 (Disponibilidad)
- **Documentos:** `GEMS_Escenarios_arquitectonicos.html`, `GEMS_ISO25010_priorizacion.html`, `GEMS_ADR.html`

## Context

| Parte | Escenario |
|---|---|
| Atributo de calidad | Disponibilidad |
| Fuente del estímulo | Falla inesperada del servicio de autenticación |
| Estímulo | El servicio de autenticación deja de responder de forma abrupta |
| Artefacto | Servicio de autenticación y sesiones |
| Entorno | Franja de estudio de 18:00 a 22:00 con 800 estudiantes con sesión activa |
| Respuesta | Quienes ya están dentro siguen con el contenido ya cargado y ven un aviso claro; los inicios de sesión nuevos muestran un mensaje de mantenimiento; al volver el servicio, nadie debe iniciar sesión otra vez |
| Medida de la respuesta | Falla detectada en 2 min o menos; servicio restablecido en 15 min o menos; como máximo 5 % de las sesiones activas obligadas a reingresar; disponibilidad mensual de al menos 99,5 % de 6:00 a 23:00 |

## Opciones consideradas

### A. Negar todo mientras autenticación no responda

Cualquier solicitud sin respuesta de autenticación en 3 s se rechaza.

- **Gana:** Máxima seguridad y simplicidad: nadie entra sin verificación.
- **Paga:** Disponibilidad: una falla de un solo servicio detiene a toda la plataforma.
- **¿Cumple la medida?** No: el 100 % de las sesiones deja de funcionar durante la falla.

### B. Aceptar sesiones firmadas y vigentes durante la caída, por un tiempo limitado

Si autenticación no responde, los servicios verifican la firma y la vigencia de la sesión y la aceptan hasta un máximo de minutos, mostrando un aviso.

- **Gana:** Quien ya está dentro sigue estudiando; la falla se absorbe.
- **Paga:** Seguridad: durante la caída no se aplican las revocaciones ni las desactivaciones de cuentas.
- **¿Cumple la medida?** Sí: el límite de tiempo acota la ventana de riesgo.

### C. Aceptar sesiones vigentes solo para lectura y negar las operaciones sensibles

Durante la caída se permite ver contenido ya cargado y avanzar lecciones; se niegan cambios de usuarios, notas, archivos privados y cualquier operación de administración.

- **Gana:** Equilibrio: sigue el estudio y se protege lo crítico.
- **Paga:** Mantenibilidad y usabilidad: hay que clasificar cada operación y explicar al usuario por qué algo no está disponible.
- **¿Cumple la medida?** Sí, con el costo de clasificar las operaciones.

## Decision

Se adopta la **opción C**: durante la caída de autenticación se aceptan las sesiones vigentes solo para lectura (ver contenido ya cargado y avanzar lecciones) y se niegan los cambios de usuarios, notas, archivos privados y cualquier operación de administración. La opción A se descarta porque deja sin servicio a todas las sesiones durante la falla y no cumple la medida de reingreso. La opción B se descarta porque durante la caída aceptaría también las operaciones sensibles sin aplicar revocaciones ni desactivaciones de cuentas.

## Consequences

- **Gana:** quienes ya están dentro siguen estudiando durante la caída, con un aviso claro, y lo crítico queda protegido; así se cumple que como máximo el 5 % de las sesiones activas deba reingresar.
- **Paga:** hay que clasificar cada operación como de lectura o sensible, y explicar al usuario por qué algo no está disponible mientras dura la falla.
- **No resuelve:** los inicios de sesión nuevos, que siguen mostrando el mensaje de mantenimiento, y la revocación de cuentas durante la caída, que queda pendiente hasta que vuelva autenticación (ver ADR-001).
- **Supuesto abierto:** la mayor parte de lo que hace un estudiante en la franja de estudio es leer contenido y avanzar lecciones, así que negar las operaciones sensibles no afecta su estudio.
