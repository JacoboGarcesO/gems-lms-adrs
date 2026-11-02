# ADR-003: Absorber un pico de visitas públicas sin afectar a las instituciones

- **Estado:** Propuesto
- **Escenario:** E2 (Escalabilidad)
- **En la entrega (README):** E5
- **Documentos:** `GEMS_Escenarios_arquitectonicos.html`, `GEMS_ISO25010_priorizacion.html`, `GEMS_ADR.html`

## Context

| Parte | Escenario |
|---|---|
| Atributo de calidad | Escalabilidad |
| Fuente del estímulo | Canal aliado con audiencia masiva |
| Estímulo | Publica un video que dirige 20.000 visitantes en una hora a la portada y a un curso gratis |
| Artefacto | Portada pública y acceso de invitados |
| Entorno | Domingo a las 16:00, toda la plataforma compartida entre instituciones y visitantes, 300 estudiantes de instituciones con sesión activa |
| Respuesta | Atiende a los visitantes, deja entrar como invitados a quienes inician un curso y, si se satura, rechaza lo excedente con un mensaje que pide volver a intentar en unos minutos en vez de caerse |
| Medida de la respuesta | Al menos 95 % de los visitantes ven la portada en 3 s o menos; al menos 90 % de las entradas de invitado terminan en 5 s o menos; 0 caídas de servicio; los usuarios con sesión institucional responden en 2 s o menos en al menos 95 % de sus solicitudes durante el evento |

## Opciones consideradas

### A. Dar más recursos a la plataforma actual

Aumentar los recursos de la plataforma actual y el límite de cada servicio.

- **Gana:** No cambia el diseño.
- **Paga:** Fiabilidad: el pico sigue compartiendo recursos con las instituciones; el tope lo pone la capacidad de la plataforma y el costo sube aunque el pico dure una hora.
- **¿Cumple la medida?** Depende: hay que medir cuánto soporta la plataforma actual.

### B. Servir la portada y el listado de cursos como contenido ya generado

La portada y el listado público se generan cada pocos minutos y se entregan como archivos, sin pasar por los servicios de educación en cada visita.

- **Gana:** La mayor parte del pico no toca los servicios; el tiempo de respuesta es casi constante.
- **Paga:** Mantenibilidad: hay que regenerar el contenido y decidir qué tan viejo puede estar (ej. hasta 5 minutos). Se pierde la personalización de la portada de cada institución.
- **¿Cumple la medida?** Sí para la portada; el acceso de invitados sigue pasando por autenticación y debe medirse.

### C. Separar el tráfico público y escalarlo automáticamente

La portada y el acceso de invitados se atienden con recursos distintos a los de las instituciones, y esos recursos crecen automáticamente cuando llega un pico de visitas y se reducen al terminar.

- **Gana:** Aislamiento total: un pico público no puede afectar a las instituciones, y la capacidad pública se ajusta sola a la demanda.
- **Paga:** Mantenibilidad y costo: dos entornos que operar, sincronizar y desplegar, y reglas de crecimiento y reducción que definir y vigilar.
- **¿Cumple la medida?** Sí, con más costo operativo.

## Decision

Pendiente: se elige entre A, B, C cuando se cuente con las tácticas.

> Lectura preliminar (no es la decisión): B protege a las instituciones con el menor costo si la portada puede ser contenido estático regenerado; C es la única que aísla del todo. A es la más barata de probar y debe medirse primero.

## Consequences

Pendiente. Queda abierto: Medir la capacidad actual de la plataforma (prueba de carga) para saber cuánta brecha hay realmente.
