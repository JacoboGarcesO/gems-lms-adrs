# ADR-001: Revocación de acceso de una cuenta desactivada

- **Estado:** Propuesto
- **Escenario:** S3 (Seguridad)
- **En la entrega (README):** E3
- **Documentos:** `GEMS_Escenarios_arquitectonicos.html`, `GEMS_ISO25010_priorizacion.html`, `GEMS_ADR.html`

## Context

| Parte | Escenario |
|---|---|
| Atributo de calidad | Seguridad |
| Fuente del estímulo | Usuario que dejó la institución, con la cuenta desactivada por su administrador |
| Estímulo | Intenta descargar un archivo privado de una entrega con su sesión aún abierta |
| Artefacto | Servicio de archivos privados |
| Entorno | Sesión emitida hace 40 minutos con vigencia de 60 minutos; la cuenta se desactivó hace 20 s |
| Respuesta | Niega la descarga y cierra la sesión |
| Medida de la respuesta | Acceso revocado en 10 s o menos tras la desactivación; 0 descargas posteriores a ese momento; latencia añadida por la verificación de 20 ms o menos en al menos 95 % de las solicitudes |

## Opciones consideradas

### A. Consultar a autenticación en cada solicitud, sin guardar respuestas

Cada servicio pregunta a autenticación si la sesión sigue vigente antes de atender cualquier solicitud.

- **Gana:** Revocación inmediata; la lógica es la más simple de razonar.
- **Paga:** Rendimiento: añade una llamada por solicitud, concentra toda la carga en autenticación y la hace punto de fallo de cada petición.
- **¿Cumple la medida?** Cumple la revocación, pero no la latencia añadida de 20 ms o menos con 800 sesiones activas.

### B. Guardar la respuesta de autenticación unos segundos por sesión

Cada servicio recuerda durante unos segundos el estado de cada sesión y solo consulta cuando esa respuesta caduca. El plazo es un parámetro.

- **Gana:** Muy poca carga sobre autenticación.
- **Paga:** Seguridad: la revocación llega con el retraso del plazo. Memoria que crece con las sesiones activas y se pierde al reiniciar.
- **¿Cumple la medida?** Sí, con un plazo de 10 s como máximo.

### C. Avisar a los servicios en el momento de la desactivación

Cuando una cuenta se desactiva, autenticación avisa a los demás servicios, que la cierran de inmediato; entre avisos solo se confía en la sesión firmada.

- **Gana:** Revocación casi inmediata sin consultar en cada solicitud.
- **Paga:** Mantenibilidad y fiabilidad: nuevo canal entre servicios; si un aviso se pierde, la cuenta sigue activa hasta que la sesión expire.
- **¿Cumple la medida?** Depende: exige un respaldo ante avisos perdidos.

## Decision

Pendiente: se elige entre A, B, C cuando se cuente con las tácticas.

> Lectura preliminar (no es la decisión): Sobre el papel, B cumple la medida de 10 s o menos con un plazo de 5 s. Falta demostrarlo con una prueba y medir el costo en latencia para el 95 % de las solicitudes; C solo se justifica si se quiere bajar de ≈ 1 s.

## Consequences

Pendiente. Queda abierto: Medir la latencia real de la verificación por servicio y el comportamiento con la caída de autenticación (ver ADR-002).
