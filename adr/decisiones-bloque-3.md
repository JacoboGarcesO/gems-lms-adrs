# Decisiones previstas para el Bloque 3

Los ADR del Bloque 2 quedan en estado **Propuesto** con la Decision y las Consequences pendientes. Aquí se guardan las decisiones que el equipo prevé tomar cuando lleguen las tácticas, para completarlas en el Bloque 3.

## ADR-001: Revocación de acceso de una cuenta desactivada

- **ADR:** [ADR-001-revocacion-de-acceso-de-una-cuenta-desactivada.md](ADR-001-revocacion-de-acceso-de-una-cuenta-desactivada.md)
- **Escenario:** S3 (E3 en el README)
- **Opción escogida:** B

### Decision

Se adopta la **opción B**: guardar unos segundos la respuesta de autenticación sobre cada sesión y consultarla solo cuando caduca, con un plazo de 5 s. La opción A se descarta porque agrega una llamada por solicitud y no asegura la latencia añadida de 20 ms o menos. La opción C se descarta por ahora: exige un respaldo ante avisos perdidos para garantizar la revocación, y B ya cumple la medida.

### Consequences

- **Gana:** la revocación llega en 5 s o menos, dentro de los 10 s de la medida, con muy poca carga sobre autenticación.
- **Paga:** la revocación llega con el retraso del plazo, así que una cuenta desactivada puede seguir activa hasta 5 s. La memoria crece con las sesiones activas y se pierde al reiniciar, y debe poder reconstruirse.
- **No resuelve:** el comportamiento cuando autenticación cae, que queda fuera del alcance de este escenario (ver ADR-002).

## ADR-002: Comportamiento ante la caída del servicio de autenticación

- **ADR:** [ADR-002-comportamiento-ante-la-caida-del-servicio-de-autenticacion.md](ADR-002-comportamiento-ante-la-caida-del-servicio-de-autenticacion.md)
- **Escenario:** D1 (E1 en el README)
- **Opción escogida:** C

### Decision

Se adopta la **opción C**: durante la caída de autenticación se aceptan las sesiones vigentes solo para lectura (ver contenido ya cargado y avanzar lecciones) y se niegan los cambios de usuarios, notas, archivos privados y cualquier operación de administración. La opción A se descarta porque deja sin servicio a todas las sesiones durante la falla y no cumple la medida de reingreso. La opción B se descarta porque durante la caída aceptaría también las operaciones sensibles sin aplicar revocaciones ni desactivaciones de cuentas.

### Consequences

- **Gana:** quienes ya están dentro siguen estudiando durante la caída, con un aviso claro, y lo crítico queda protegido; así se cumple que como máximo el 5 % de las sesiones activas deba reingresar.
- **Paga:** hay que clasificar cada operación como de lectura o sensible, y explicar al usuario por qué algo no está disponible mientras dura la falla.
- **No resuelve:** los inicios de sesión nuevos, que siguen mostrando el mensaje de mantenimiento, y la revocación de cuentas durante la caída, que queda pendiente hasta que vuelva autenticación (ver ADR-001).

## ADR-003: Absorber un pico de visitas públicas sin afectar a las instituciones

- **ADR:** [ADR-003-absorber-un-pico-de-visitas-publicas-sin-afectar-a-las-insti.md](ADR-003-absorber-un-pico-de-visitas-publicas-sin-afectar-a-las-insti.md)
- **Escenario:** E2 (E5 en el README)
- **Opción escogida:** C

### Decision

Se adopta la **opción C**: separar el tráfico público (portada y acceso de invitados) del de las instituciones y hacer que sus recursos crezcan automáticamente durante los picos de visitas y se reduzcan al terminar. La opción A se descarta porque el pico seguiría compartiendo recursos con las instituciones y el costo sube aunque el pico dure una hora. La opción B se descarta: protege la portada, pero el acceso de invitados sigue pasando por autenticación y, al servir la portada como contenido ya generado, se pierde la personalización de la portada de cada institución.

### Consequences

- **Gana:** un pico público no puede afectar a las instituciones, y la capacidad pública se ajusta sola a la demanda de 20.000 visitantes en una hora.
- **Paga:** dos entornos que operar, sincronizar y desplegar, y reglas de crecimiento y reducción que definir y vigilar; el costo sube mientras dura el pico.
- **No resuelve:** el límite de 30 invitados nuevos por hora por red, que sigue afectando a campus y colegios con una sola salida a Internet.

## ADR-004: Registro único de un intento de quiz ante cortes de red

- **ADR:** [ADR-004-registro-unico-de-un-intento-de-quiz-ante-cortes-de-red.md](ADR-004-registro-unico-de-un-intento-de-quiz-ante-cortes-de-red.md)
- **Escenario:** F1 (E4 en el README)
- **Opción escogida:** A

### Decision

Se adopta la **opción A**: cada envío lleva el identificador de la sesión del quiz y se acepta una sola vez; si ya fue recibido, el sistema devuelve el mismo resultado en lugar de crear otro intento. La opción B se descarta porque serializa los envíos, puede dejar al estudiante esperando y arriesga bloqueos si el proceso falla a medio camino. La opción C se descarta porque durante un tiempo habría intentos duplicados visibles en notas y reportes, y no cumple la medida de 0 intentos duplicados.

### Consequences

- **Gana:** reenvíos seguros: el estudiante que se reconecta ve su resultado sin repetir el quiz, con 0 intentos duplicados y 0 perdidos.
- **Paga:** una comprobación más por cada envío, que se concentra en el cierre del tiempo del quiz, cuando llegan muchos envíos a la vez.
- **No resuelve:** el envío que se pierde antes de llegar al sistema: si la conexión se corta antes de que el servidor lo reciba, ese envío no queda registrado.

## ADR-005: Protección frente a intentos masivos de contraseñas

- **ADR:** [ADR-005-proteccion-frente-a-intentos-masivos-de-contrasenas.md](ADR-005-proteccion-frente-a-intentos-masivos-de-contrasenas.md)
- **Escenario:** S2 (E2 en el README)
- **Opción escogida:** C

### Decision

Se adopta la **opción C**: tras varios fallos de inicio de sesión se exige un captcha, y el ingreso se completa con verificación en dos pasos (MFA). La opción A se descarta porque permite hasta 600 intentos por minuto por dirección y no cumple el máximo de 10. La opción B se descarta como medida única: frena la fuerza bruta con esperas, pero no distingue a una persona de un programa ni protege la cuenta si la contraseña ya fue adivinada.

### Consequences

- **Gana:** frena la automatización y protege la cuenta aunque se adivine la contraseña, y la fricción del captcha aparece solo ante sospecha.
- **Paga:** otros pasos en el flujo de inicio de sesión (captcha y código de MFA) y dependencias nuevas que mantener; si el captcha es visual, hay problemas de accesibilidad.
- **No resuelve:** la alerta al equipo en 5 min o menos, que requiere un aviso aparte.
