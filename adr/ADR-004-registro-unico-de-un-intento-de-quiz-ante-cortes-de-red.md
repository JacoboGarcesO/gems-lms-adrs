# ADR-004: Registro único de un intento de quiz ante cortes de red

- **Estado:** Propuesto
- **Escenario:** F1 (Fiabilidad)
- **En la entrega (README):** E4
- **Documentos:** `GEMS_Escenarios_arquitectonicos.html`, `GEMS_ISO25010_priorizacion.html`, `GEMS_ADR.html`

## Context

| Parte | Escenario |
|---|---|
| Atributo de calidad | Fiabilidad |
| Fuente del estímulo | Estudiante con conexión inestable en su celular |
| Estímulo | Su conexión se corta justo después de enviar la última respuesta de un quiz |
| Artefacto | Servicio de calificación de quizzes |
| Entorno | Quiz de 30 min con intento único, estudiante con pérdida de señal de 20 s |
| Respuesta | Registra el intento una sola vez y, al reconectar, el estudiante ve su resultado sin repetir el quiz |
| Medida de la respuesta | 0 intentos duplicados; 0 intentos perdidos; resultado visible en 5 s o menos tras reconectar, en el 99 % de los casos |

## Opciones consideradas

### A. Identificar cada envío con la sesión del quiz y aceptarlo una sola vez

Cada envío lleva el identificador de la sesión del quiz; si ya fue recibido, el sistema devuelve el mismo resultado en lugar de crear otro intento.

- **Gana:** Reenvíos seguros; apoya en la sesión única abierta y en la unicidad del número de intento que ya existen.
- **Paga:** Rendimiento: una comprobación más por envío en el pico de cierre.
- **¿Cumple la medida?** Sí: 0 duplicados por construcción.

### B. Bloquear el quiz del estudiante mientras se califica

El sistema impide cualquier otro envío del mismo estudiante y quiz hasta terminar de calificar el anterior.

- **Gana:** Simple de entender.
- **Paga:** Rendimiento: serializa y puede dejar al estudiante esperando; riesgo de bloqueos si el proceso falla a medio camino.
- **¿Cumple la medida?** Depende: hay que probar los bloqueos con fallas.

### C. Detectar y limpiar duplicados después

Se aceptan todos los envíos y una tarea periódica elimina los duplicados.

- **Gana:** No añade costo al envío.
- **Paga:** Fiabilidad: durante un tiempo hay intentos duplicados que se ven en notas y reportes.
- **¿Cumple la medida?** No: viola «0 intentos duplicados».

## Decision

Pendiente: se elige entre A, B, C cuando se cuente con las tácticas.

> Lectura preliminar (no es la decisión): A aprovecha la sesión de quiz y las restricciones que ya existen y cumple la medida con poco costo; B no se justifica salvo concurrencia extrema; C no cumple con 0 duplicados en todo momento.

## Consequences

Pendiente. Queda abierto: Probar el reenvío del mismo envío y el cierre de tiempo simultáneo; definir qué ve el estudiante si su envío ya fue recibido.
