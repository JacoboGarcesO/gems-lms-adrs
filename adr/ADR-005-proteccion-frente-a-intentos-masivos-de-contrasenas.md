# ADR-005: Protección frente a intentos masivos de contraseñas

- **Estado:** Propuesto
- **Escenario:** S2 (Seguridad)
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

### A. Límite general de solicitudes por dirección (lo actual)

Se limita el total de solicitudes por minuto por dirección, sin distinguir intentos fallidos de inicio de sesión.

- **Gana:** Ya existe y no afecta el uso normal.
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

Se adopta la **opción C**: tras varios fallos de inicio de sesión se exige un captcha, y el ingreso se completa con verificación en dos pasos (MFA). La opción A se descarta porque permite hasta 600 intentos por minuto por dirección y no cumple el máximo de 10. La opción B se descarta como medida única: frena la fuerza bruta con esperas, pero no distingue a una persona de un programa ni protege la cuenta si la contraseña ya fue adivinada.

## Consequences

- **Gana:** frena la automatización y protege la cuenta aunque se adivine la contraseña, y la fricción del captcha aparece solo ante sospecha.
- **Paga:** otros pasos en el flujo de inicio de sesión (captcha y código de MFA) y dependencias nuevas que mantener; si el captcha es visual, hay problemas de accesibilidad.
- **No resuelve:** la alerta al equipo en 5 min o menos, que requiere un aviso aparte.
- **Supuesto abierto:** los estudiantes, incluidos los de 14 años y los de celulares de gama baja, pueden completar el captcha y el MFA sin abandonar el inicio de sesión.
