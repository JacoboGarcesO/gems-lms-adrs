# ADR-005: Protección frente a intentos masivos de contraseñas

- **Estado:** Propuesto
- **Escenario:** S2 (Seguridad)
- **En la entrega (README):** E2
- **Documentos:** `GEMS_Escenarios_arquitectonicos.html`, `GEMS_ISO25010_priorizacion.html`, `GEMS_ADR.html`

## Context

| Parte | Escenario |
|---|---|
| Atributo de calidad | Seguridad |
| Fuente del estímulo | Atacante externo desde una sola dirección |
| Estímulo | Prueba contraseñas de forma masiva contra la pantalla de inicio de sesión |
| Artefacto | Servicio de autenticación |
| Entorno | Operación normal con 800 usuarios activos; el atacante envía 20 intentos por segundo |
| Respuesta | Frena al atacante sin impedir el ingreso de los demás usuarios y deja registro del evento |
| Medida de la respuesta | Llegan a verificarse como máximo 10 intentos fallidos por minuto por dirección; 0 cuentas ajenas bloqueadas; alerta al equipo en 5 min o menos; al menos 95 % de los inicios de sesión de usuarios legítimos en 1 s o menos durante el ataque |

## Opciones consideradas

### A. Límite general de solicitudes por dirección

Se limita el total de solicitudes por minuto por dirección, sin distinguir intentos fallidos de inicio de sesión.

- **Gana:** No afecta el uso normal.
- **Paga:** Seguridad: permite hasta 600 intentos por minuto por dirección; no hay alerta ni protección por cuenta.
- **¿Cumple la medida?** No: 600 intentos por minuto superan el máximo de 10.

### B. Esperas progresivas por cuenta y por dirección tras fallos de inicio de sesión

Cada fallo de una cuenta o de una dirección aumenta el tiempo de espera para el siguiente intento; el éxito lo reinicia.

- **Gana:** Frena fuerza bruta aunque cambien de dirección; no bloquea la cuenta de forma definitiva.
- **Paga:** Usabilidad: un estudiante que olvidó su clave espera unos segundos; hay que guardar contadores y avisar.
- **¿Cumple la medida?** Sí, con contadores por cuenta y dirección.

### C. Pedir captcha tras varios fallos y verificación en dos pasos (MFA)

Tras un número de fallos de inicio de sesión, el sistema exige un captcha para comprobar que es una persona; además, el ingreso se completa con una verificación en dos pasos (MFA) con un código adicional.

- **Gana:** La fricción del captcha aparece solo ante sospecha y frena la automatización; el MFA protege la cuenta aunque se adivine la contraseña.
- **Paga:** Usabilidad y mantenibilidad: otros pasos en el flujo (captcha y código de MFA), dependencias nuevas, y problemas de accesibilidad si el captcha es visual.
- **¿Cumple la medida?** Sí, con un captcha accesible.

## Decision

Pendiente: se elige entre A, B, C cuando se cuente con las tácticas.

> Lectura preliminar (no es la decisión): B cumple la medida y protege a la cuenta aunque el ataque venga de muchas direcciones; C añade fricción solo cuando hay sospecha. A no cumple.

## Consequences

Pendiente. Queda abierto: Definir el tiempo de espera, el umbral por cuenta y por dirección, y cómo avisa el sistema a la persona y al equipo.
