# ADR-001: Revocación de acceso de una cuenta desactivada

- **Estado:** Propuesto
- **Escenario:** S3 (Seguridad)
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
- **¿Cumple la medida?** Cumple la revocación, pero no la latencia añadida de 20 ms o menos con 800 sesiones activas (por verificar).

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

Se adopta la **opción B**: guardar unos segundos la respuesta de autenticación sobre cada sesión y consultarla solo cuando caduca, con un plazo de 5 s. La opción A se descarta porque agrega una llamada por solicitud y no asegura la latencia añadida de 20 ms o menos. La opción C se descarta por ahora: exige un respaldo ante avisos perdidos para garantizar la revocación, y B ya cumple la medida.

## Consequences

- **Gana:** la revocación llega en 5 s o menos, dentro de los 10 s de la medida, con muy poca carga sobre autenticación.
- **Paga:** la revocación llega con el retraso del plazo, así que una cuenta desactivada puede seguir activa hasta 5 s. La memoria crece con las sesiones activas y se pierde al reiniciar, y debe poder reconstruirse.
- **No resuelve:** el comportamiento cuando autenticación cae, que queda fuera del alcance de este escenario (ver ADR-002).
- **Supuesto abierto:** un plazo de 5 s cumple la medida de 10 s también con 800 sesiones activas.
