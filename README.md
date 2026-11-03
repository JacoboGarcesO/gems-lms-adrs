---
equipo: 9
integrantes: Angie Carolina Pareja Villa; Jacobo Garcés Oquendo; Jose Daniel López Carvallo; Jose Miguel Cortes Jaramillo; Mariana Usuga Mejía
nivel: pregrado
caso: proyecto propio
nombre_del_sistema: GEMS LMS
formato_de_entrega: widget
enlace_o_archivo_de_la_entrega: https://github.com/JacoboGarcesO/gems-lms-adrs
---

# Entrega Bloque 2: escenarios de calidad y ADR (texto plano)

Repositorio de la entrega: https://github.com/JacoboGarcesO/gems-lms-adrs

## 1. Caso y ficha de contexto

**Cliente, en su voz:**
Necesitamos una plataforma de aprendizaje para varias instituciones, con usuarios, cursos, evaluaciones, certificados y cursos gratis para invitados. Cada institución debe tener sus usuarios, cursos y marca, y puede tener su propio subdominio.

**Anclas con cifras (cinco):**
1. Hoy hay 4 instituciones y unos 1.000 usuarios.
2. Una institución nueva puede traer 5.000 estudiantes en un mes.
3. La franja de estudio fuerte es de 18:00 a 22:00, con 800 sesiones activas.
4. La plataforma debe soportar 20.000 visitantes en una hora por un pico público y 300 envíos de quiz en 60 s.
5. La disponibilidad mensual propuesta es de 99,5 % entre 6:00 y 23:00; la carga inicial tiene un presupuesto de 650 kB sin comprimir.

Las cifras de carga, las metas de tiempo de respuesta y la disponibilidad son propuestas definidas por el equipo; no son mediciones ni un acuerdo de servicio firmado.

**Tensión dominante, en una línea:**
En este sistema, seguridad choca con disponibilidad, rendimiento y usabilidad porque verificar, limitar y proteger cada operación puede impedir que los usuarios continúen estudiando o ingresen.

**Dónde está esto en la entrega:**
[gems-lms-adrs/GEMS_Escenarios_arquitectonicos.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_Escenarios_arquitectonicos.html), pestaña «El caso GEMS» (datos del sistema), y [gems-lms-adrs/GEMS_ISO25010_priorizacion.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_ISO25010_priorizacion.html), pestaña «Priorización GEMS».

## 2. Supuestos declarados

No aplica a pregrado.

## 3. Priorización con presupuesto de 18 puntos

- Adecuación funcional: [1] | Justificación: GEMS ya cubre el ciclo completo (cursos, inscripciones, evaluaciones, certificados, grupos y reportes); el foco de esta entrega está en atributos de calidad.
- Eficiencia de desempeño: [3] | Justificación: El uso es en picos (clase masiva, video de un canal aliado, envíos simultáneos de quizzes) y todos comparten los mismos recursos; un pico mal absorbido afecta a todas las instituciones.
- Compatibilidad: [1] | Justificación: La exportación e integración son importantes, pero no dominan los riesgos actuales.
- Usabilidad: [3] | Justificación: Deben poder estudiar invitados y estudiantes, incluso en celulares de gama baja y ante mensajes de seguridad.
- Fiabilidad: [3] | Justificación: No se deben perder ni duplicar intentos, y el sistema debe continuar o recuperarse ante fallas.
- Seguridad: [4] | Justificación: Hay datos privados por institución, sesiones, cuentas desactivadas e intentos masivos de contraseña.
- Mantenibilidad: [2] | Justificación: El equipo es pequeño (5 personas) y el producto cambia seguido: nuevos tipos de bloque, nuevos proveedores y diagnósticos de casos de estudiantes.
- Portabilidad: [1] | Justificación: No hay plan de mudarse de proveedor ni de soportar otros entornos; no es el riesgo dominante.

**Total:** [18] (debe ser 18)

**Qué bajamos para poder subir el atributo más alto, y qué riesgo aceptamos:**
Bajamos Adecuación funcional, Compatibilidad y Portabilidad a 1, y Mantenibilidad a 2, para subir Seguridad a 4 y mantener Eficiencia de desempeño, Usabilidad y Fiabilidad en 3. Aceptamos menor prioridad para integraciones, portabilidad y facilidad de cambio.

**Dónde está esto en la entrega:**
[gems-lms-adrs/GEMS_ISO25010_priorizacion.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_ISO25010_priorizacion.html), pestaña «Priorización GEMS».

## 4. Escenarios de calidad

En esta entrega los escenarios críticos se llaman E1 a E5. En los documentos HTML y en el CSV tienen el ID propio del atributo. Esta tabla relaciona ambos nombres y el ADR que cada uno origina:

| En esta entrega | En los documentos | Atributo y característica ISO 25010 | ADR |
|---|---|---|---|
| E1 | D1 | Disponibilidad › Disponibilidad, tolerancia a fallos | ADR-002 |
| E2 | S2 | Seguridad › Autenticidad | ADR-005 |
| E3 | S3 | Seguridad › Responsabilidad e integridad | ADR-001 |
| E4 | F1 | Fiabilidad › Madurez | ADR-004 |
| E5 | E2 | Escalabilidad › Capacidad | ADR-003 |

Estos cinco son los escenarios críticos. Los otros 19 escenarios del documento completo (24 en total, tres por atributo) no se desarrollan aquí y están en [gems-lms-adrs/GEMS_escenarios_de_calidad.csv](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_escenarios_de_calidad.csv) y en [gems-lms-adrs/GEMS_Escenarios_arquitectonicos.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_Escenarios_arquitectonicos.html).

### E1
- Escenario en los documentos: D1
- Atributo dominante: Disponibilidad
- Atributo y característica ISO 25010: Disponibilidad › Disponibilidad, tolerancia a fallos
- Otro atributo que toca (si lo hay): Seguridad
- Fuente del estímulo: Falla inesperada del servicio de autenticación
- Estímulo: El servicio de autenticación deja de responder de forma abrupta
- Artefacto: Servicio de autenticación y sesiones
- Entorno: Franja de estudio de 18:00 a 22:00 con 800 estudiantes con sesión activa
- Respuesta: Quienes ya están dentro siguen con el contenido ya cargado y ven un aviso claro; los inicios de sesión nuevos muestran un mensaje de mantenimiento; al volver el servicio, nadie debe iniciar sesión otra vez
- Medida de la respuesta: Falla detectada en 2 min o menos; servicio restablecido en 15 min o menos; como máximo 5 % de las sesiones activas obligadas a reingresar; disponibilidad mensual de al menos 99,5 % de 6:00 a 23:00
- ¿Es crítico? sí, por la brecha total: hoy la caída de autenticación detiene a toda la plataforma y arreglarla enfrenta fiabilidad con seguridad.
- Dónde está en la entrega: [gems-lms-adrs/GEMS_Escenarios_arquitectonicos.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_Escenarios_arquitectonicos.html), escenario D1; [gems-lms-adrs/adr/ADR-002-comportamiento-ante-la-caida-del-servicio-de-autenticacion.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-002-comportamiento-ante-la-caida-del-servicio-de-autenticacion.md); también en [gems-lms-adrs/GEMS_escenarios_de_calidad.csv](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_escenarios_de_calidad.csv).

### E2
- Escenario en los documentos: S2
- Atributo dominante: Seguridad
- Atributo y característica ISO 25010: Seguridad › Autenticidad
- Otro atributo que toca (si lo hay): Usabilidad
- Fuente del estímulo: Atacante externo desde una sola dirección
- Estímulo: Prueba contraseñas de forma masiva contra la pantalla de inicio de sesión
- Artefacto: Servicio de autenticación
- Entorno: Operación normal con 800 usuarios activos; el atacante envía 20 intentos por segundo
- Respuesta: Frena al atacante sin impedir el ingreso de los demás usuarios y deja registro del evento
- Medida de la respuesta: Llegan a verificarse como máximo 10 intentos fallidos por minuto por dirección; 0 cuentas ajenas bloqueadas; alerta al equipo en 5 min o menos; al menos 95 % de los inicios de sesión de usuarios legítimos en 1 s o menos durante el ataque
- ¿Es crítico? sí, porque hoy se permiten hasta 600 intentos por minuto por dirección frente al máximo propuesto de 10, y frenarlos puede bloquear a estudiantes que comparten red.
- Dónde está en la entrega: [gems-lms-adrs/GEMS_Escenarios_arquitectonicos.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_Escenarios_arquitectonicos.html), escenario S2; [gems-lms-adrs/adr/ADR-005-proteccion-frente-a-intentos-masivos-de-contrasenas.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-005-proteccion-frente-a-intentos-masivos-de-contrasenas.md); también en [gems-lms-adrs/GEMS_escenarios_de_calidad.csv](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_escenarios_de_calidad.csv).

### E3
- Escenario en los documentos: S3
- Atributo dominante: Seguridad
- Atributo y característica ISO 25010: Seguridad › Responsabilidad e integridad
- Otro atributo que toca (si lo hay): Eficiencia de desempeño
- Fuente del estímulo: Usuario que dejó la institución, con la cuenta desactivada por su administrador
- Estímulo: Intenta descargar un archivo privado de una entrega con su sesión aún abierta
- Artefacto: Servicio de archivos privados
- Entorno: Sesión emitida hace 40 minutos con vigencia de 60 minutos; la cuenta se desactivó hace 20 s
- Respuesta: Niega la descarga y cierra la sesión
- Medida de la respuesta: Acceso revocado en 10 s o menos tras la desactivación; 0 descargas posteriores a ese momento; latencia añadida por la verificación de 20 ms o menos en al menos 95 % de las solicitudes
- ¿Es crítico? sí, porque en papel se cumple (5 s o menos), pero ninguna prueba lo demuestra y su costo en latencia condiciona los ADR de sesión y de caída de autenticación.
- Dónde está en la entrega: [gems-lms-adrs/GEMS_Escenarios_arquitectonicos.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_Escenarios_arquitectonicos.html), escenario S3; [gems-lms-adrs/adr/ADR-001-revocacion-de-acceso-de-una-cuenta-desactivada.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-001-revocacion-de-acceso-de-una-cuenta-desactivada.md); también en [gems-lms-adrs/GEMS_escenarios_de_calidad.csv](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_escenarios_de_calidad.csv).

### E4
- Escenario en los documentos: F1
- Atributo dominante: Fiabilidad
- Atributo y característica ISO 25010: Fiabilidad › Madurez
- Otro atributo que toca (si lo hay): Eficiencia de desempeño
- Fuente del estímulo: Estudiante
- Estímulo: Envía varias veces la última respuesta de un quiz
- Artefacto: Servicio de calificación de quizzes
- Entorno: Quiz de 30 min con intento único
- Respuesta: Registra el intento una sola vez y devuelve el mismo resultado a cada reenvío
- Medida de la respuesta: 0 intentos duplicados; resultado visible en 5 s o menos tras el envío, en el 99 % de los casos
- ¿Es crítico? sí, porque afecta a registros con valor para la persona y toca el momento más cargado del sistema: el cierre de un examen.
- Dónde está en la entrega: [gems-lms-adrs/GEMS_Escenarios_arquitectonicos.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_Escenarios_arquitectonicos.html), escenario F1; [gems-lms-adrs/adr/ADR-004-registro-unico-de-un-intento-de-quiz.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-004-registro-unico-de-un-intento-de-quiz.md); también en [gems-lms-adrs/GEMS_escenarios_de_calidad.csv](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_escenarios_de_calidad.csv).

### E5
- Escenario en los documentos: E2
- Atributo dominante: Eficiencia de desempeño
- Atributo y característica ISO 25010: Escalabilidad › Capacidad
- Otro atributo que toca (si lo hay): Fiabilidad
- Fuente del estímulo: Canal aliado con audiencia masiva
- Estímulo: Publica un video que dirige 20.000 visitantes en una hora a la portada y a un curso gratis
- Artefacto: Portada pública y acceso de invitados
- Entorno: Domingo a las 16:00, toda la plataforma compartida entre instituciones y visitantes, 300 estudiantes de instituciones con sesión activa
- Respuesta: Atiende a los visitantes, deja entrar como invitados a quienes inician un curso y, si se satura, rechaza lo excedente con un mensaje que pide volver a intentar en unos minutos en vez de caerse
- Medida de la respuesta: Al menos 95 % de los visitantes ven la portada en 3 s o menos; al menos 90 % de las entradas de invitado terminan en 5 s o menos; 0 caídas de servicio; los usuarios con sesión institucional responden en 2 s o menos en al menos 95 % de sus solicitudes durante el evento
- ¿Es crítico? sí, porque es el mayor riesgo multi-institución: un pico público comparte recursos con las instituciones y su solución cuesta fiabilidad o mantenibilidad.
- Dónde está en la entrega: [gems-lms-adrs/GEMS_Escenarios_arquitectonicos.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_Escenarios_arquitectonicos.html), escenario E2; [gems-lms-adrs/adr/ADR-003-absorber-un-pico-de-visitas-publicas-sin-afectar-a-las-insti.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-003-absorber-un-pico-de-visitas-publicas-sin-afectar-a-las-insti.md); también en [gems-lms-adrs/GEMS_escenarios_de_calidad.csv](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_escenarios_de_calidad.csv).

## 5. Selección de escenarios críticos

**Escenarios críticos elegidos:** E1, E2, E3, E4 y E5.

**Por qué esos y no los demás (una línea):**
Son los escenarios elegidos por tener una brecha medible, un atributo dominante priorizado y un costo explícito sobre otro atributo; los demás quedan como candidatos siguientes o se cubren con pruebas.

**Dónde está esto en la entrega:**
[gems-lms-adrs/GEMS_Escenarios_arquitectonicos.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_Escenarios_arquitectonicos.html), botón «Críticos» de la pestaña «Escenarios de GEMS», y [gems-lms-adrs/GEMS_ADR.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_ADR.html), pestaña «Escenarios críticos».

## 6. ADR

Los cinco ADR están en estado **Propuesto**: traen el Context (el escenario) y las opciones consideradas; la Decision y las Consequences quedan pendientes hasta el Bloque 3. La numeración sigue la de los archivos, que se relacionan con los escenarios en [gems-lms-adrs/adr/README.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/README.md). Las decisiones que el equipo prevé están guardadas aparte en [gems-lms-adrs/adr/decisiones-bloque-3.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/decisiones-bloque-3.md).

### ADR-001
- Título: Revocación de acceso de una cuenta desactivada
- Estado: Propuesto
- Escenario que lo origina: E3 (S3 en el documento de escenarios)
- Context: igual al escenario indicado (sí)

**Opción A:**
- Qué haría el sistema: Consultaría a autenticación en cada solicitud, sin guardar respuestas.
- Gana: Revocación inmediata y lógica simple de razonar.
- Paga: Añade una llamada por solicitud, concentra carga en autenticación y la vuelve punto de fallo.
- Atributo que sacrifica: Eficiencia de desempeño
- ¿Cumple la medida de la respuesta? depende; la latencia añadida de 20 ms o menos con 800 sesiones activas debe verificarse.
- Decisión de producto o de política que encierra: ninguna

**Opción B:**
- Qué haría el sistema: Guardaría la respuesta de autenticación unos segundos por sesión y consultaría cuando caduque.
- Gana: Muy poca carga sobre autenticación.
- Paga: La revocación llega con el retraso del plazo y la memoria crece con las sesiones activas.
- Atributo que sacrifica: Seguridad
- ¿Cumple la medida de la respuesta? sí, con un plazo de 10 s como máximo.
- Decisión de producto o de política que encierra: acepta una ventana breve de acceso después de la desactivación.

**Opción C:**
- Qué haría el sistema: Avisaría a los demás servicios en el momento de la desactivación para cerrar la sesión de inmediato; entre avisos solo se confiaría en la sesión firmada.
- Gana: Revocación casi inmediata sin consultar en cada solicitud.
- Paga: Un canal nuevo entre servicios; si un aviso se pierde, la cuenta sigue activa hasta que la sesión expire.
- Atributo que sacrifica: Mantenibilidad y Fiabilidad
- ¿Cumple la medida de la respuesta? depende; exige un respaldo ante avisos perdidos.
- Decisión de producto o de política que encierra: acepta operar un canal adicional entre servicios a cambio de una revocación casi inmediata.

**Lo que nos falta saber para elegir en el Bloque 3:** Medir la latencia real de la verificación por servicio y el comportamiento con la caída de autenticación (ver ADR-002).

**Dónde está este ADR en la entrega:** [gems-lms-adrs/adr/ADR-001-revocacion-de-acceso-de-una-cuenta-desactivada.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-001-revocacion-de-acceso-de-una-cuenta-desactivada.md); también en [gems-lms-adrs/GEMS_ADR.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_ADR.html), pestaña «Los 5 ADR de GEMS».

### ADR-002
- Título: Comportamiento ante la caída del servicio de autenticación
- Estado: Propuesto
- Escenario que lo origina: E1 (D1 en el documento de escenarios)
- Context: igual al escenario indicado (sí)

**Opción A:**
- Qué haría el sistema: Negaría toda solicitud mientras autenticación no responda; una solicitud sin respuesta en 3 s se rechaza.
- Gana: Máxima seguridad y simplicidad.
- Paga: Disponibilidad; una falla de un solo servicio detiene toda la plataforma.
- Atributo que sacrifica: Disponibilidad
- ¿Cumple la medida de la respuesta? no; el 100 % de las sesiones deja de funcionar durante la falla.
- Decisión de producto o de política que encierra: ninguna

**Opción B:**
- Qué haría el sistema: Aceptaría sesiones firmadas y vigentes durante la caída, por un tiempo limitado y mostrando un aviso.
- Gana: Quien ya está dentro sigue estudiando; la falla se absorbe.
- Paga: Durante la caída no se aplican las revocaciones ni las desactivaciones.
- Atributo que sacrifica: Seguridad
- ¿Cumple la medida de la respuesta? sí; el límite de tiempo acota la ventana de riesgo.
- Decisión de producto o de política que encierra: favorece la continuidad de estudiantes ya autenticados.

**Opción C:**
- Qué haría el sistema: Aceptaría sesiones vigentes solo para lectura durante la caída y negaría las operaciones sensibles.
- Gana: Sigue el estudio y se protege lo crítico.
- Paga: Hay que clasificar cada operación y explicar al usuario por qué algo no está disponible.
- Atributo que sacrifica: Mantenibilidad y Usabilidad
- ¿Cumple la medida de la respuesta? sí, con el costo de clasificar las operaciones.
- Decisión de producto o de política que encierra: define qué operaciones se consideran sensibles y se niegan mientras autenticación no responda.

**Lo que nos falta saber para elegir en el Bloque 3:** Decidir cuánto tiempo y para qué operaciones se acepta una sesión vigente sin consultar. Relacionado con el ADR-001.

**Dónde está este ADR en la entrega:** [gems-lms-adrs/adr/ADR-002-comportamiento-ante-la-caida-del-servicio-de-autenticacion.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-002-comportamiento-ante-la-caida-del-servicio-de-autenticacion.md); también en [gems-lms-adrs/GEMS_ADR.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_ADR.html), pestaña «Los 5 ADR de GEMS».

### ADR-003
- Título: Absorber un pico de visitas públicas sin afectar a las instituciones
- Estado: Propuesto
- Escenario que lo origina: E5 (E2 en el documento de escenarios)
- Context: igual al escenario indicado (sí)

**Opción A:**
- Qué haría el sistema: Aumentaría los recursos de la plataforma actual y el límite de cada servicio.
- Gana: No cambia el diseño.
- Paga: El pico sigue compartiendo recursos con las instituciones y el costo sube aunque dure una hora.
- Atributo que sacrifica: Fiabilidad
- ¿Cumple la medida de la respuesta? depende; hay que medir cuánto soporta la plataforma actual.
- Decisión de producto o de política que encierra: acepta que el pico comparta recursos con las instituciones.

**Opción B:**
- Qué haría el sistema: Serviría la portada y el listado público como contenido ya generado cada pocos minutos.
- Gana: La mayor parte del pico no toca los servicios; el tiempo de respuesta es casi constante.
- Paga: Hay que regenerar el contenido y decidir qué tan viejo puede estar; se pierde la personalización de la portada de cada institución.
- Atributo que sacrifica: Mantenibilidad
- ¿Cumple la medida de la respuesta? sí para la portada; el acceso de invitados sigue pasando por autenticación y debe medirse.
- Decisión de producto o de política que encierra: acepta que el contenido público pueda estar desactualizado hasta 5 minutos.

**Opción C:**
- Qué haría el sistema: Separaría el tráfico público (portada y acceso de invitados) del de las instituciones, con recursos propios que crecen automáticamente en los picos y se reducen al terminar.
- Gana: Aislamiento total: un pico público no puede afectar a las instituciones, y la capacidad pública se ajusta sola a la demanda.
- Paga: Dos entornos que operar, sincronizar y desplegar, y reglas de crecimiento y reducción que definir y vigilar.
- Atributo que sacrifica: Mantenibilidad y costo
- ¿Cumple la medida de la respuesta? sí, con más costo operativo.
- Decisión de producto o de política que encierra: acepta pagar y operar un entorno público aparte a cambio de aislar a las instituciones.

**Lo que nos falta saber para elegir en el Bloque 3:** Medir la capacidad actual de la plataforma (prueba de carga) para saber cuánta brecha hay realmente.

**Dónde está este ADR en la entrega:** [gems-lms-adrs/adr/ADR-003-absorber-un-pico-de-visitas-publicas-sin-afectar-a-las-insti.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-003-absorber-un-pico-de-visitas-publicas-sin-afectar-a-las-insti.md); también en [gems-lms-adrs/GEMS_ADR.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_ADR.html), pestaña «Los 5 ADR de GEMS».

### ADR-004
- Título: Registro único de un intento de quiz
- Estado: Propuesto
- Escenario que lo origina: E4 (F1 en el documento de escenarios)
- Context: igual al escenario indicado (sí)

**Opción A:**
- Qué haría el sistema: Identificaría cada envío con la sesión del quiz y lo aceptaría una sola vez; un reenvío devolvería el mismo resultado.
- Gana: Reenvíos seguros y apoyo en la sesión única abierta y la unicidad del intento.
- Paga: Una comprobación más por envío en el pico de cierre.
- Atributo que sacrifica: Eficiencia de desempeño
- ¿Cumple la medida de la respuesta? sí; produce 0 duplicados por construcción.
- Decisión de producto o de política que encierra: ninguna

**Opción B:**
- Qué haría el sistema: Bloquearía el quiz del estudiante mientras se califica el envío anterior.
- Gana: Es simple de entender.
- Paga: Serializa, puede dejar esperando al estudiante y puede bloquearse si falla el proceso.
- Atributo que sacrifica: Eficiencia de desempeño
- ¿Cumple la medida de la respuesta? depende; hay que probar los bloqueos con fallas.
- Decisión de producto o de política que encierra: prioriza evitar duplicados aunque aumente la espera.

**Opción C:**
- Qué haría el sistema: Aceptaría todos los envíos y una tarea periódica eliminaría los duplicados.
- Gana: No añade costo al envío.
- Paga: Durante un tiempo hay intentos duplicados visibles en notas y reportes.
- Atributo que sacrifica: Fiabilidad
- ¿Cumple la medida de la respuesta? no; viola «0 intentos duplicados».
- Decisión de producto o de política que encierra: tolera duplicados temporales a cambio de no añadir costo al envío.

**Lo que nos falta saber para elegir en el Bloque 3:** Probar el reenvío del mismo envío y el cierre de tiempo simultáneo; definir qué ve el estudiante si su envío ya fue recibido.

**Dónde está este ADR en la entrega:** [gems-lms-adrs/adr/ADR-004-registro-unico-de-un-intento-de-quiz.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-004-registro-unico-de-un-intento-de-quiz.md); también en [gems-lms-adrs/GEMS_ADR.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_ADR.html), pestaña «Los 5 ADR de GEMS».

### ADR-005
- Título: Protección frente a intentos masivos de contraseñas
- Estado: Propuesto
- Escenario que lo origina: E2 (S2 en el documento de escenarios)
- Context: igual al escenario indicado (sí)

**Opción A:**
- Qué haría el sistema: Aplicaría el límite general de solicitudes por dirección, sin distinguir intentos fallidos de inicio de sesión.
- Gana: No afecta el uso normal.
- Paga: Permite hasta 600 intentos por minuto por dirección; no hay alerta ni protección por cuenta.
- Atributo que sacrifica: Seguridad
- ¿Cumple la medida de la respuesta? no; 600 intentos por minuto superan el máximo de 10.
- Decisión de producto o de política que encierra: ninguna

**Opción B:**
- Qué haría el sistema: Aplicaría esperas progresivas por cuenta y por dirección tras fallos; el éxito reinicia la espera.
- Gana: Frena fuerza bruta aunque cambien de dirección y no bloquea definitivamente la cuenta.
- Paga: Un estudiante que olvidó su clave espera unos segundos; hay que guardar contadores y avisar.
- Atributo que sacrifica: Usabilidad
- ¿Cumple la medida de la respuesta? sí, con contadores por cuenta y dirección.
- Decisión de producto o de política que encierra: traslada una fricción temporal a quien falla repetidamente.

**Opción C:**
- Qué haría el sistema: Exigiría un captcha tras varios fallos de inicio de sesión y completaría el ingreso con verificación en dos pasos (MFA).
- Gana: La fricción del captcha aparece solo ante sospecha y frena la automatización; el MFA protege la cuenta aunque se adivine la contraseña.
- Paga: Otros pasos en el flujo (captcha y código de MFA), dependencias nuevas y problemas de accesibilidad si el captcha es visual.
- Atributo que sacrifica: Usabilidad y Mantenibilidad
- ¿Cumple la medida de la respuesta? sí, con un captcha accesible.
- Decisión de producto o de política que encierra: acepta añadir pasos de verificación (captcha y MFA) al ingreso de los usuarios.

**Lo que nos falta saber para elegir en el Bloque 3:** Definir el tiempo de espera, el umbral por cuenta y por dirección, y cómo avisa el sistema a la persona y al equipo.

**Dónde está este ADR en la entrega:** [gems-lms-adrs/adr/ADR-005-proteccion-frente-a-intentos-masivos-de-contrasenas.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/ADR-005-proteccion-frente-a-intentos-masivos-de-contrasenas.md); también en [gems-lms-adrs/GEMS_ADR.html](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/GEMS_ADR.html), pestaña «Los 5 ADR de GEMS».

## 7. Lo que el equipo reconoce

**Puntos débiles de nuestra entrega que ya identificamos:**
No hay pruebas de carga ni objetivos de tiempo de respuesta; no hay un identificador que siga un caso entre servicios; las alertas no tienen destino configurado; la copia externa, su frecuencia y la restauración de prueba no están automatizadas.

**Lo que más discutimos como equipo y no resolvimos:**
Cómo equilibrar seguridad y continuidad cuando autenticación falla, cómo proteger contra intentos masivos sin bloquear estudiantes que comparten red y cómo aislar un pico público sin afectar a las instituciones.

## 8. Autochequeo del equipo (marque lo que se cumple)

- [x] Las anclas tienen cifras, no solo adjetivos.
- [x] Podemos decir en una línea cuál es la tensión dominante de nuestro caso.
- [x] Nuestra priorización suma exactamente 18 y respeta que ningún atributo vale menos de 1 ni más de 5.
- [x] Cada escenario tiene estímulo observable, respuesta concreta y medida verificable.
- [x] Los ADR corresponden a escenarios críticos y justificamos por qué esos.
- [x] Cada ADR tiene al menos dos opciones, descritas como mecanismos y no como marcas.
- [x] No elegimos todavía: la Decision y las Consequences de los cinco ADR quedan pendientes; las decisiones previstas están aparte, en [gems-lms-adrs/adr/decisiones-bloque-3.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/decisiones-bloque-3.md), para el Bloque 3.

## 9. Inconsistencias detectadas por la IA (solo la IA diligencia; no corrija lo anterior)

- Las cifras de carga, las metas de tiempo de respuesta y la disponibilidad de 99,5 % son propuestas definidas por el equipo; no hay mediciones de uso publicadas, pruebas de carga ni un acuerdo de servicio firmado.
- Los escenarios se llaman E1 a E5 en este texto y D1, S2, S3, F1 y E2 en los documentos HTML y el CSV, y los ADR tienen numeración propia (ADR-001 a ADR-005). Se conservaron los tres nombres y la sección 4 y [gems-lms-adrs/adr/README.md](https://github.com/JacoboGarcesO/gems-lms-adrs/blob/main/adr/README.md) los relacionan.
- El documento completo tiene 24 escenarios y aquí se desarrollan cinco; los otros 19 están solo en el CSV y en el documento de escenarios. La plantilla permite de 3 a 5.
- Algunos escenarios usan atributos refinados (por ejemplo, Escalabilidad) que se mapean a las categorías de la plantilla; la sección 4 indica el atributo dominante y, aparte, el atributo y la característica ISO 25010 de cada escenario.
