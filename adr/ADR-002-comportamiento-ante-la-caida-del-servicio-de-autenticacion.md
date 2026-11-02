# ADR-002: Comportamiento ante la caída del servicio de autenticación

- **Estado:** Propuesto
- **Escenario:** D1 (Disponibilidad)
- **En la entrega (README):** E1
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

Pendiente: se elige entre A, B, C cuando se cuente con las tácticas.

> Lectura preliminar (no es la decisión): Con A («fallar cerrado») se incumple la medida de como máximo 5 % de sesiones obligadas a reingresar. B y C la cumplen; C limita mejor el riesgo, B es más simple.

## Consequences

Pendiente. Queda abierto: Decidir cuánto tiempo y para qué operaciones se acepta una sesión vigente sin consultar. Relacionado con el ADR-001.
