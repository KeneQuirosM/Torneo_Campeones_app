# 01 · Requisitos y MVP ampliado — Torneo de Campeones

- **Fase:** 1 (Requisitos funcionales y definición del MVP ampliado)
- **Estado:** **PENDIENTE DE APROBACIÓN.** Ningún requisito de este documento está aprobado. Aprobar la fase no aprueba por sí sola las recomendaciones `[REC]` ni las decisiones pendientes `[PEND]`; solo el usuario puede aprobarlas expresamente (ver §13 y §17).
- **Versión:** v1.0 (borrador para revisión) · **Fecha:** 2026-11-15
- **Autorización de la fase:** mensaje «AUTORIZACIÓN FORMAL DE FASE 1» del 2026-11-15 (alcance: solo la Fase 1; sin código, esquema, migraciones, dependencias, cambios a la skill, Fase 2, commit, push, merge ni despliegues).
- **Base vinculante:** `00-descubrimiento.md` v1.3 (Fase 0 aprobada): decisiones Q-01…Q-07, Q-17, Q-18, Q-19, marcadas aquí como **[CONF]**.
- **Fuentes revisadas en esta fase:** `00-descubrimiento.md`, `aprobaciones.md`, `bitacora.md`; `references/puertas-de-aprobacion.md`, `qa.md`, `seguridad.md`, `puntos-a-validar.md`, `mapa-informe.md`; plantilla `brief-proyecto.md`; del informe PDF se releyeron pp. 13–15 (§6.5, §6.6, §7.2, §7.3) y pp. 22–23 (§10, casos N-01…N-13). **No se leyeron** `arquitectura.md`, `ui-ux.md` ni `direccion-visual-profesional.md` (corresponden a las Fases 2 y 3). **No se verificó** ninguna fuente externa ni legislación.

---

## 0. Cómo leer este documento

### 0.1 Etiquetas de origen (las mismas de la Fase 0)

| Etiqueta | Significado |
|---|---|
| **[CONF]** | Decisión confirmada por el usuario (Fase 0). Vinculante. Solo el usuario la cambia. |
| **[DER]** | Consecuencia derivada de una decisión [CONF]. Requiere confirmación al aprobar la fase. |
| **[REC]** | Recomendación de este análisis. **No es una decisión.** Se redacta como requisito para que sea verificable, pero no está aprobada. |
| **[PEND]** | Decisión del usuario o de la organización que sigue abierta (ver §13). |
| **[PEND-JUR]** | Requiere revisión jurídica. Las referencias legales son conocimiento general **no verificado** y no son asesoría legal. |

### 0.2 Columnas de los requisitos

- **Prio.:** **M** = obligatorio del MVP · **S** = deseable del MVP (se recorta primero si hay que reducir, con aprobación) · **P** = posterior (fuera del MVP; no se construye).
- **Estado:** `CONF` (proviene directamente de una decisión confirmada) · `DER` (derivado) · `REC` (propuesta nueva por aprobar) · `COND:Q-xx` (**condicionado** a una decisión pendiente; el requisito se redacta con la recomendación y **no puede aprobarse** hasta que se decida).
- **Prueba:** `U` unitaria · `DB` pgTAP/PostgreSQL real · `I` integración · `E2E` Playwright · `A` accesibilidad (axe + manual) · `M` manual/UAT · `L` carga · `D` revisión documental. Las pruebas se **prevén** aquí; ninguna se ha ejecutado.
- **Regla de calidad aplicada:** un requisito sin criterio de aceptación medible, ambiguo o contradictorio queda como `COND` o `[PEND]`, nunca como listo.

### 0.3 Convención de identificadores

`HU-` historias · `RF-ACC/TOR/EQP/PUB/ADM/AUD/PWA` funcionales generales · `RD-` reglas deportivas · `OFF-` captura sin conexión · `PRV-` privacidad · `RNF-` no funcionales · `RT-` restricciones transversales de permisos · `CL-` casos límite · `Q-` preguntas/decisiones · `RK-` riesgos. Los `RF-xx` y `RP-xx` provisionales de la Fase 0 se reasignan en §14.4 y §14.5.

---

## 1. Resumen ejecutivo

**Qué se define.** El MVP ampliado de Torneo de Campeones es una PWA para gestionar y publicar **ligas de fútbol** (una o dos vueltas) con equipos, plantillas, calendario, resultados, goleadores, tarjetas, W.O., tabla de posiciones con desempates configurables, usuarios con cuatro roles autenticados más el público, auditoría y **captura de marcadores y eventos sin conexión** con sincronización posterior. Copas, relámpagos, eliminatorias y fases de grupos quedan fuera (Q-18).

**Tamaño de la especificación.** 16 requisitos de acceso, 7 de torneos, 5 de equipos, 9 públicos, 5 de administración, 5 de auditoría y 4 de PWA; 66 reglas deportivas (RD); 22 requisitos de captura sin conexión; 16 de privacidad; 17 no funcionales; 10 restricciones transversales de permisos y una matriz de 34 acciones por rol; 34 casos límite; 42 historias de usuario. Cada requisito lleva prioridad, estado de aprobación y prueba prevista.

**Lo más importante para decidir.**
1. **28 decisiones abiertas** (§13): las 18 preguntas de la Fase 0 que seguían abiertas (Q-02 residual, Q-08…Q-16, Q-20…Q-25) y 10 nuevas surgidas al especificar (Q-26…Q-35). Para cada una hay opciones y una recomendación; **ninguna se tomó por usted**. Las **14 marcadas «Bloquea F2»** condicionan la arquitectura.
2. El modo sin conexión es el mayor generador de complejidad y riesgo (§9). Se especifica con cola local de operaciones, idempotencia, política de conflictos, revocación y límites de iOS. La estimación (§15) lo trata como partida propia.
3. **Estimación actualizada del MVP ampliado:** 107–163 días-persona de ingeniería (valor probable ≈ 134), es decir, unas **24–36 semanas** con una persona a dedicación completa o **43–65 semanas** a media jornada. El MVP de 5–7 semanas del informe **ya no es aplicable**. Es una estimación inicial, no un compromiso (§15).
4. **Privacidad:** por defecto ningún menor aparece con nombre en lo público; la revisión jurídica sigue siendo puerta obligatoria antes del lanzamiento (§10).

**Lo que este documento no hace.** No elige tecnología (Fase 2), no diseña pantallas (Fase 3), no escribe esquema ni SQL (Fase 4), no verifica leyes, costos ni límites de proveedores, y no ejecuta pruebas.

---

## 2. Base vinculante (decisiones confirmadas que esta fase respeta)

| ID | Decisión [CONF] (resumen fiel) | Dónde se traduce |
|---|---|---|
| Q-01 | País: Costa Rica; responsable previsto: la organización; revisión jurídica pendiente. | PRV-01, PRV-15, RNF-10 |
| Q-02 | Cinco roles con control delegado; editor solo partidos asignados; delegado solo plantilla de su equipo; autorización en servidor **y** base de datos; sin escalada ni acceso entre torneos. | §7, RF-ACC, RT-01…RT-10 |
| Q-03 | Puntos 3/1/0; desempates configurables antes de comenzar, orden por defecto: puntos, diferencia de goles, goles a favor, enfrentamiento directo, resolución manual; una o dos vueltas; W.O. configurable (3-0 por defecto), auditado y sin goles individuales ficticios; cambios de reglamento en torneo iniciado controlados, validados y auditados. | §8 |
| Q-04 / Q-18 | MVP **solo liga**, con equipos y jugadores, calendarios, resultados, tablas, goleadores y tarjetas, W.O., una/dos vueltas, desempates configurables, usuarios y roles, captura offline, vista pública y panel. Fuera: eliminatorias, copas, relámpagos, fases de grupos. | §3 |
| Q-05 | Perfiles públicos limitados a información deportiva necesaria; autorización del representante legal cuando corresponda; minimización; revisión jurídica previa al lanzamiento. | §10 |
| Q-06 | 3–6 torneos simultáneos; 6–16 equipos por torneo; captura sin conexión obligatoria; sincronización posterior; sin duplicados ni conflictos; pendientes diferenciados de confirmados. | §9, RNF-04 |
| Q-07 | Cuentas y dominio de la organización; el usuario es desarrollador con acceso técnico; la organización asume costos; prioridad a planes gratuitos adecuados; verificar costos y respaldos antes de contratar. | RNF-12, RNF-14 |
| Q-17 | La captura de marcadores y eventos funciona sin Internet, se conserva localmente y se sincroniza al recuperar la conexión; lo local se distingue de lo confirmado por el servidor. | §9 |
| Q-19 | Solo el dueño designa organizadores; el organizador invita y administra editores y delegados solo en sus torneos; editor y delegado no invitan; asignaciones auditadas; verificación en backend y base de datos. | RF-ACC-03…05, 11, 15 |

Condiciones vigentes de la Fase 0: revisión jurídica obligatoria antes del lanzamiento; costos y condiciones verificados con fuentes oficiales; estimación actualizada; ninguna recomendación técnica sustituye decisiones aprobadas.

---

## 3. Alcance del MVP y lo que queda para después

| Capacidad | MVP | Posterior (fase) |
|---|---|---|
| Torneo de **liga**, 1 o 2 vueltas, reglamento versionado | Sí | — |
| Equipos, inscripción por torneo, plantillas y jugadores | Sí | Escudos/imágenes, categorías por edad (Q-31), importación masiva |
| Calendario por método del círculo con vista previa, ajustes manuales auditados, reprogramación | Sí | Optimización de localías avanzada |
| Resultados, estados de partido, correcciones, W.O. | Sí | Sanciones automáticas por W.O. reiterados (Q-21) |
| Eventos: gol, autogol, tarjeta amarilla, tarjeta roja; goleadores y tarjetas | Sí | Suspensiones automáticas por acumulación; fair play; perfil público completo de jugador; penal/asistencias |
| Tabla de posiciones con desempates configurables y resolución manual | Sí | Fair play como criterio (Q-28) |
| Usuarios: invitaciones, 4 roles + público, revocación, asignación de editores, auditoría | Sí | — |
| **Captura sin conexión** con cola, idempotencia, conflictos, revocación | Sí | Operaciones administrativas sin conexión |
| Vista pública (tabla, calendario, resultados, goleadores, tarjetas, plantillas con nombre público) y panel administrativo | Sí | Marcadores en vivo (tiempo real), cronología |
| PWA instalable, caché, estados de red | Sí | Notificaciones push |
| Compartir por enlace, QR y botón nativo de compartir | Sí (S) | Imágenes con marca para compartir |
| Privacidad base: nombre público condicionado, consentimiento (metadatos), anonimización | Sí | Archivo digital de evidencia de consentimiento (Q-24) |
| Copa, relámpago, grupos, eliminatorias, ida y vuelta eliminatoria, empates eliminatorios, llaves | **No** (Q-18) | Fase 6 (el modelo no debe impedirlos) |
| Pagos, boletería, video, estadísticas avanzadas, patrocinadores, galería | **No** | Evaluar tras el piloto |
| Histórico de temporadas anteriores | **No** (Q-11, recomendación) | A decidir |
| Disputas formales de resultados (flujo) | **No**; se resuelven con corrección auditada (Q-09) | A decidir |

---

## 4. Glosario mínimo

- **Torneo:** competición de liga con reglamento, equipos inscritos y calendario. **Equipo inscrito:** participación de un equipo en un torneo (plantilla por torneo).
- **Jornada:** conjunto de partidos de una vuelta que se programan juntos; **vuelta:** ciclo completo en que todos enfrentan a todos una vez.
- **Partido confirmado:** estado del partido tal como lo aceptó el servidor. **Dato local/pendiente:** captura guardada en el dispositivo y aún no aceptada.
- **Operación (op):** unidad de cambio generada en el dispositivo (p. ej. «gol de X al minuto 34»), con identificador único.
- **Torneo iniciado:** el torneo en que algún partido ya salió del estado `programado` (RD-REG-02).
- **Nombre público:** el nombre con que un jugador aparece en vistas públicas (PRV-02).

---

## 5. Historias de usuario (cinco perfiles)

Formato: «Como [rol], quiero [acción], para [valor]». Los criterios de aceptación están en los requisitos vinculados. **Todas son propuestas pendientes de aprobación de la fase.**

### 5.1 Dueño

| ID | Historia | Requisitos | Prio. |
|---|---|---|---|
| HU-DUE-01 | Como dueño, quiero invitar y designar organizadores con un torneo asignado, para delegar la operación. | RF-ACC-02, 03 | M |
| HU-DUE-02 | Como dueño, quiero revocar el acceso de cualquier usuario y que surta efecto de inmediato, para contener un abuso. | RF-ACC-07 | M |
| HU-DUE-03 | Como dueño, quiero ver el historial completo de cambios de todos los torneos, para responder preguntas y disputas. | RF-AUD-01…04 | M |
| HU-DUE-04 | Como dueño, quiero autorizar un cambio de reglamento en un torneo iniciado viendo su impacto antes de confirmar, para no alterar la tabla por error. | RD-REG-03 | M (COND Q-23) |
| HU-DUE-05 | Como dueño, quiero atender una solicitud de acceso o anonimización de un jugador, para cumplir la privacidad. | PRV-08, 09 | M |
| HU-DUE-06 | Como dueño, quiero poder designar a un segundo dueño o recuperar el acceso, para que la plataforma no dependa de una sola cuenta. | RF-ACC-09 | S (COND Q-35) |
| HU-DUE-07 | Como dueño, quiero corregir cualquier resultado como medida de contingencia, para no depender de otros. | RD-RES-06 | M |

### 5.2 Organizador

| ID | Historia | Requisitos | Prio. |
|---|---|---|---|
| HU-ORG-01 | Como organizador, quiero crear un torneo de liga y quedar autorizado sobre él, para administrarlo. | RF-TOR-01, RF-ACC-12 | M (COND Q-02a) |
| HU-ORG-02 | Como organizador, quiero configurar vueltas, desempates y W.O. antes de iniciar, para que la tabla refleje nuestro reglamento. | RD-REG-01, 04, RD-TAB-05 | M |
| HU-ORG-03 | Como organizador, quiero inscribir equipos y retirar uno si abandona, para mantener la competición. | RF-EQP-02, 03 | M (retiro COND Q-26) |
| HU-ORG-04 | Como organizador, quiero invitar delegados y editores solo en mis torneos, para repartir el trabajo. | RF-ACC-04 | M |
| HU-ORG-05 | Como organizador, quiero generar el calendario con vista previa y ajustarlo a mano con rastro, para adaptarlo a las canchas. | RD-CAL-01…06 | M |
| HU-ORG-06 | Como organizador, quiero publicar jornada por jornada, para controlar lo que ve el público. | RD-CAL-08, RF-PUB-04 | M |
| HU-ORG-07 | Como organizador, quiero asignar editores a partidos concretos, para que cada partido tenga responsable. | RF-ACC-06 | M (COND Q-02b) |
| HU-ORG-08 | Como organizador, quiero ver alertas de partidos próximos sin editor, para reaccionar antes del inicio. | RF-ADM-04 | M (COND Q-10) |
| HU-ORG-09 | Como organizador, quiero corregir un resultado finalizado indicando el motivo, para enmendar errores con trazabilidad. | RD-RES-06 | M (COND Q-09) |
| HU-ORG-10 | Como organizador, quiero declarar un W.O. con motivo, para aplicar el reglamento. | RD-WO-01…05 | M (COND Q-21) |
| HU-ORG-11 | Como organizador, quiero resolver manualmente un empate que los criterios no deshacen, con motivo visible, para cerrar la tabla. | RD-TAB-09 | M (COND Q-22) |
| HU-ORG-12 | Como organizador, quiero ver el historial de mis torneos, para supervisar. | RF-AUD-04, RF-ADM-03 | M |
| HU-ORG-13 | Como organizador, quiero ver operaciones en conflicto o rechazadas de mis editores, para resolverlas. | RF-ADM-04, OFF-09, OFF-10 | M |

### 5.3 Editor

| ID | Historia | Requisitos | Prio. |
|---|---|---|---|
| HU-EDI-01 | Como editor, quiero ver solo mis partidos asignados, para saber dónde debo capturar. | RF-ADM-02, RT-08 | M |
| HU-EDI-02 | Como editor, quiero preparar mi dispositivo con los datos de mis partidos antes de ir a la cancha, para trabajar sin señal. | OFF-02 | M |
| HU-EDI-03 | Como editor, quiero capturar goles, tarjetas y el marcador sin conexión, para no depender de la señal. | OFF-01, RD-EVT-01 | M |
| HU-EDI-04 | Como editor, quiero distinguir lo pendiente de lo confirmado, para saber qué ya llegó al servidor. | OFF-04 | M |
| HU-EDI-05 | Como editor, quiero que mis datos se envíen solos al recuperar conexión y sin duplicarse, para no repetir trabajo. | OFF-05, 06 | M |
| HU-EDI-06 | Como editor, quiero ver qué pasó si mi captura entra en conflicto o se rechaza y qué puedo hacer, para no perder lo anotado. | OFF-09, 10 | M |
| HU-EDI-07 | Como editor, quiero deshacer o anular un evento mal registrado, para corregirme antes de finalizar. | RD-EVT-05, OFF-14 | M |
| HU-EDI-08 | Como editor, quiero registrar un gol en pocos toques, para no perder el ritmo del partido. | RNF-13 | M |
| HU-EDI-09 | Como editor, quiero finalizar el partido con un resumen para confirmar, para evitar errores. | RD-RES-03, OFF-21 | M |

### 5.4 Delegado

| ID | Historia | Requisitos | Prio. |
|---|---|---|---|
| HU-DEL-01 | Como delegado, quiero ver y mantener la plantilla de mi equipo (altas, bajas, dorsales), para tener a mis jugadores listos. | RD-JUG-01…06 | M |
| HU-DEL-02 | Como delegado, quiero registrar la autorización de publicación de nombre de un jugador (o de su representante legal), para que aparezca con nombre en lo público. | PRV-02…05 | M (COND Q-24) |
| HU-DEL-03 | Como delegado, quiero ver el calendario y los resultados de mi equipo, para planificar. | RF-ADM-01 | M |
| HU-DEL-04 | Como delegado, quiero que el sistema me avise si un jugador ya está en otro equipo del torneo, para evitar rechazos. | RD-JUG-02 | M |
| HU-DEL-05 | Como delegado, quiero aceptar mi invitación y acceder solo a mi equipo, para trabajar sin ver otros. | RF-ACC-02, 13 | M |

### 5.5 Público

| ID | Historia | Requisitos | Prio. |
|---|---|---|---|
| HU-PUB-01 | Como aficionado, quiero ver la tabla de posiciones sin crear cuenta, para seguir mi equipo. | RF-PUB-02, RD-TAB-01 | M |
| HU-PUB-02 | Como aficionado, quiero ver el calendario por jornada y los resultados, para saber cuándo juega mi equipo. | RF-PUB-02, RD-CAL-04 | M |
| HU-PUB-03 | Como aficionado, quiero ver los goleadores y tarjetas, para seguir a los jugadores. | RD-EVT-07, 08 | M |
| HU-PUB-04 | Como aficionado, quiero ver las plantillas con los nombres que se pueden publicar, para conocer al equipo. | PRV-02, RF-PUB-02 | S |
| HU-PUB-05 | Como aficionado, quiero compartir el enlace o el QR de un torneo o una tabla, para enviarlo por WhatsApp. | RF-PUB-05 | S |
| HU-PUB-06 | Como aficionado, quiero instalar la app en mi celular, para abrirla rápido. | RF-PWA-01 | M |
| HU-PUB-07 | Como aficionado, quiero ver la última versión guardada y su hora cuando no tengo señal, para no quedarme sin información. | RF-PUB-06, RF-PWA-02 | M |
| HU-PUB-08 | Como aficionado, quiero entender por qué dos equipos con los mismos puntos están ordenados así, para confiar en la tabla. | RD-REG-05, RD-TAB-07 | S |

---

## 6. Requisitos funcionales generales

### 6.1 Acceso, usuarios e invitaciones (RF-ACC)

| ID | Requisito | Prio. | Estado | Aceptación (medible) | Prueba |
|---|---|---|---|---|---|
| RF-ACC-01 | El registro de cuentas es **solo por invitación**; no existe auto-registro. | M | REC (informe §9) | Un intento de crear cuenta sin invitación vigente falla en la interfaz **y** en la API/base de datos; 0 cuentas creadas. | DB, E2E |
| RF-ACC-02 | Toda invitación define: correo destinatario, rol, ámbito (torneo o equipo inscrito), quién invita, fecha de vencimiento, un solo uso y estado (pendiente / aceptada / vencida / revocada). Vencimiento propuesto: 7 días. | M | REC (plazo [PEND]) | Invitación vencida, usada o revocada no permite alta (3 casos); aceptada con un correo distinto del invitado se rechaza; el rol y el ámbito no pueden modificarse tras emitirla (hay que revocar y emitir otra). | DB, I |
| RF-ACC-03 | **Solo el dueño** crea o invita organizadores y les asigna torneos. | M | CONF (Q-19) | Un organizador, editor o delegado que intenta invitar o designar un organizador recibe denegación (interfaz oculta la acción; API y BD la rechazan). | DB, I |
| RF-ACC-04 | El organizador invita y administra **editores y delegados exclusivamente en los torneos que tiene asignados**. | M | CONF (Q-19) | Invitar a un editor al torneo B siendo organizador solo del A → denegado. Invitar a un delegado de un equipo inscrito en A → permitido. | DB, I |
| RF-ACC-05 | Editor y delegado **no** pueden invitar usuarios ni asignar roles. | M | CONF (Q-19) | Los cuatro intentos (editor/delegado × invitar/asignar) son denegados en BD. | DB |
| RF-ACC-06 | La **asignación de editores a partidos** la realizan el dueño y el organizador del torneo; el editor no puede asignarse partidos. Un partido puede tener un editor asignado vigente (varios, solo si se decide). Se conserva el historial de asignaciones (quién, cuándo, hasta cuándo). | M | COND:Q-02b (recomendado) | Editor intenta asignarse → denegado. Asignar un editor de otro torneo a un partido → rechazado. El historial muestra altas y bajas. | DB, I |
| RF-ACC-07 | **Revocación:** el dueño puede revocar a cualquier usuario; el organizador puede revocar editores y delegados de sus torneos (no organizadores ni dueño). Surte efecto en el servidor de inmediato: las operaciones posteriores se rechazan y las sesiones del afectado se invalidan en ≤ 60 s (valor propuesto, a confirmar en Fase 2). | M | COND:Q-02c | Tras revocar, una operación de escritura del afectado se rechaza; su siguiente petición autenticada falla en ≤ 60 s; todo queda auditado. | DB, I |
| RF-ACC-08 | **Varios roles por persona:** una cuenta puede tener roles distintos en torneos distintos. Dentro del mismo torneo se admite más de un rol solo si se decide; recomendado: sí, con la regla de conflicto de interés RT-05. | M | COND:Q-15 | Un usuario con rol de editor en A y delegado en B opera con el rol correcto en cada uno; ningún permiso se «suma» entre torneos. | DB |
| RF-ACC-09 | **Continuidad del dueño:** el sistema impide eliminar o degradar al último dueño; existe un procedimiento documentado de recuperación y (si se decide) un segundo dueño. | S | COND:Q-35 | Intento de revocar al único dueño → rechazado; runbook de recuperación revisado. | DB, D |
| RF-ACC-10 | **Autenticación:** correo y contraseña; segundo factor obligatorio para dueño y organizador; para editores según Q-08 (recomendado: opcional en el MVP, reevaluar tras el piloto); límites de intentos y recuperación de contraseña. Parámetros (longitud, expiración, método del 2FA) se fijan en la Fase 2. | M | REC; editores COND:Q-08 | Dueño/organizador sin 2FA activado no accede al panel; 5 intentos fallidos activan el límite definido en Fase 2. | I, E2E |
| RF-ACC-11 | **Toda** invitación, aceptación, asignación, modificación o revocación de rol o de asignación a partido queda auditada. | M | CONF (Q-19) | Cada una de esas acciones genera un registro con actor, destino, torneo, antes/después y hora del servidor; 0 acciones sin registro en la suite. | DB |
| RF-ACC-12 | **Creación de torneo y autorización del creador:** el organizador puede crear torneos; al crear uno, queda automáticamente como organizador de **ese** torneo y de ninguno más, en la misma operación atómica; el dueño puede crear y asignar torneos. Esto no equivale a «designar organizadores» (Q-19). | M | COND:Q-02a (recomendado) | Tras crear, el organizador ve y administra solo ese torneo; otro organizador no lo ve; si falla la asignación, el torneo no se crea. | DB, I |
| RF-ACC-13 | El **delegado** queda vinculado a **un equipo inscrito en un torneo**; solo administra la plantilla de ese equipo. | M | CONF (Q-02) | Delegado del equipo X edita la plantilla de X (permitido) y de Y, aun en el mismo torneo (denegado). | DB |
| RF-ACC-14 | El público consulta lo publicado **sin autenticación**; un usuario autenticado sin rol tiene los mismos permisos que el público. | M | CONF (Q-02) | Mismos resultados de lectura para anónimo y autenticado sin rol (comparación automatizada). | DB, E2E |
| RF-ACC-15 | **Anti-escalada:** ningún organizador puede otorgar el rol de dueño ni crear organizadores; nadie puede otorgarse roles a sí mismo; el alcance de una invitación nunca excede el de quien invita; un rol no se modifica por parámetros del cliente. | M | CONF (Q-02, Q-19) | Pruebas de abuso: manipular `rol`, `torneo` o `usuario` en la petición produce denegación en el 100 % de los casos probados. | DB, I |
| RF-ACC-16 | **Sesión y recuperación:** cierre por inactividad, cierre de sesión por el usuario, invalidación de sesiones al revocar y al cambiar contraseña, recuperación de cuenta. Tiempos y mecanismos: Fase 2. | M | REC | Tras cambiar la contraseña, las sesiones anteriores dejan de ser válidas. | I |

### 6.2 Torneos (RF-TOR)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-TOR-01 | Crear un torneo de liga con nombre, temporada, descripción, número de vueltas (1 o 2) y reglamento inicial por defecto (RD-REG-01). Estado inicial `borrador`. | M | CONF (Q-18) | Se crea en `borrador`, no visible al público. | I, E2E |
| RF-TOR-02 | **Ciclo de vida:** `borrador` → `publicado` → `en curso` → `finalizado` → `archivado`. `en curso` ocurre automáticamente cuando el torneo está iniciado (RD-REG-02). Publicar/despublicar: dueño y organizador. Finalizar: organizador o dueño, con confirmación. Un torneo `finalizado` solo admite correcciones del dueño (RD-RES-06). | M | REC | Transiciones inválidas rechazadas (p. ej. `borrador` → `finalizado`); todas auditadas. | U, DB |
| RF-TOR-03 | El torneo tiene un **reglamento versionado** (ver RD-REG). | M | CONF (Q-03) | Ver RD-REG. | — |
| RF-TOR-04 | **Aislamiento:** los datos de un torneo (equipos inscritos, plantillas, partidos, eventos, usuarios y auditoría) no son visibles ni modificables desde otro torneo; la plataforma soporta 3–6 torneos simultáneos. | M | CONF (Q-02, Q-06) | Con 6 torneos sembrados, ninguna consulta ni escritura de un rol de A alcanza datos de B (pruebas por rol); ver RT-08. | DB, L |
| RF-TOR-05 | **Sin borrado físico** de torneos con partidos: se archivan. Solo se eliminan torneos en `borrador` sin partidos, por el dueño. | M | REC | Intentar eliminar un torneo con partidos → rechazado; archivar conserva datos y auditoría. | DB |
| RF-TOR-06 | Cada torneo publicado tiene una dirección pública estable y un código QR. | M | CONF (skill: vistas por link/QR) | La URL no cambia al renombrar el torneo; el QR abre la misma página. | E2E |
| RF-TOR-07 | Crear torneos de copa, relámpago o con grupos **no** es posible en el MVP; el diseño no debe impedirlo. | P | CONF (Q-18) | La interfaz y la API solo ofrecen `liga`. | I |

### 6.3 Equipos e inscripciones (RF-EQP)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-EQP-01 | **Equipo e inscripción son entidades distintas**: la inscripción (equipo en un torneo) tiene nombre mostrado, abreviatura y colores propios. Un organizador solo modifica inscripciones de sus torneos y nunca datos que afecten la presentación del equipo en otro torneo. Si el equipo conserva identidad entre torneos se define con Q-11. | M | DER; alcance COND:Q-11 | Cambiar el nombre mostrado en A no cambia lo mostrado en B. | DB |
| RF-EQP-02 | Inscribir equipos: nombre único por torneo (sin distinguir mayúsculas ni tildes), abreviatura de 3–4 caracteres, dos colores. Número de equipos: mínimo técnico 2; máximo de plataforma a fijar en Fase 2 (≥ 16; propuesto 20). | M | REC | Duplicado de nombre rechazado; inscribir equipo 17 (límite propuesto 20) permitido, equipo 21 rechazado. | I, DB |
| RF-EQP-03 | **Retiro/expulsión de un equipo** con torneo en curso: política a decidir. Opciones en Q-26. Recomendado: el equipo pasa a `retirado`; se conservan sus resultados jugados; los partidos pendientes se marcan W.O. a favor del rival o se anulan según configuración; todo auditado. | M | COND:Q-26 | Según la opción elegida; mientras no se decida, el requisito no se aprueba. | U, DB |
| RF-EQP-04 | Escudos e imágenes: fuera del MVP (el equipo se identifica por abreviatura y colores). | P | REC | La interfaz no ofrece carga de imágenes. | — |
| RF-EQP-05 | Un equipo puede tener uno o más delegados (invitados por organizador/dueño). | S | REC | Dos delegados del mismo equipo editan la plantilla; ninguno edita otro equipo. | DB |

### 6.4 Experiencia pública (RF-PUB)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-PUB-01 | Lista pública de torneos `publicado`/`en curso`/`finalizado`; los `borrador` y `archivado` no aparecen. | M | DER | Un torneo en `borrador` no aparece ni se accede por URL (404 indistinguible de inexistente). | E2E, DB |
| RF-PUB-02 | Página del torneo con: tabla de posiciones, calendario por jornada, resultados, goleadores, tarjetas y plantillas por equipo (con nombre público según PRV-02), y resumen del reglamento. | M | CONF (Q-04) | Cada sección muestra únicamente datos publicados y confirmados (fixtures). | E2E |
| RF-PUB-03 | Sin inicio de sesión; sin datos privados: no se expone identidad de editores asignados, correos, ni campos internos. | M | CONF (Q-02, Q-05) | La respuesta de cualquier endpoint público no contiene campos de la lista de «nunca público» (PRV-03); revisión por esquema. | DB, I |
| RF-PUB-04 | **Visibilidad:** un partido es visible solo si el torneo está publicado **y** su jornada está publicada; un resultado es visible solo si está **confirmado por el servidor** (§9.6). | M | DER (Q-17) | Matriz de visibilidad (§9.6) probada: 12 combinaciones. | I, E2E |
| RF-PUB-05 | Compartir: enlace estable, código QR y botón nativo de compartir; vista previa con título y descripción del torneo al pegar el enlace en mensajería. | S | REC (Q-13 [PEND]) | El botón abre la hoja nativa de compartir en móvil compatible; la vista previa muestra el nombre del torneo. | E2E, M |
| RF-PUB-06 | Cada vista pública indica «Actualizado a las hh:mm»; sin conexión muestra la última versión guardada con ese sello y aviso de que puede estar desactualizada. | M | DER (Q-17) | En modo sin conexión (Playwright) se muestra la última versión visitada con aviso y hora. | E2E |
| RF-PUB-07 | Estados de carga, vacío y error en toda vista pública (sin torneos, jornada sin partidos, error del servidor, red lenta). | M | REC | Cada estado alcanzable y probado en 390 px. | E2E, A |
| RF-PUB-08 | Aviso de privacidad accesible desde el pie de toda vista pública. El texto lo redacta/valida el responsable con asesoría jurídica. | M | CONF (Q-05) + PEND-JUR | Enlace presente en todas las páginas públicas; texto final pendiente. | M |
| RF-PUB-09 | Un resultado confirmado aparece en las vistas públicas en ≤ 60 s (propuesto) sin que el visitante recargue manualmente más de una vez. | M | REC | Medición en staging: percentil 95 ≤ 60 s. | I, L |

### 6.5 Panel administrativo (RF-ADM)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-ADM-01 | Cada rol ve un inicio adaptado: **dueño** (todos los torneos, usuarios, alertas), **organizador** (sus torneos, próximos partidos, alertas, conflictos), **editor** (mis partidos y estado de preparación sin conexión), **delegado** (mi equipo, plantilla, calendario). | M | CONF (Q-04) | Cada rol ve solo sus entidades; revisión de pantalla por rol. | E2E |
| RF-ADM-02 | «Mis partidos»: el editor ve únicamente partidos asignados, ordenados por fecha, con el estado de preparación sin conexión y de sincronización. | M | CONF (Q-02) | Un editor con 3 partidos asignados en un torneo de 40 ve exactamente 3. | DB, E2E |
| RF-ADM-03 | Historial de cambios con filtros (torneo, entidad, actor, fecha). Dueño: todo. Organizador: sus torneos. Editor y delegado: no tienen acceso al historial (el editor ve su propia cola de operaciones). | M | DER (Anexo A) | Un organizador no ve registros de torneos ajenos (prueba de acceso cruzado). | DB, E2E |
| RF-ADM-04 | **Alertas** (a) partidos sin editor asignado dentro de las próximas 48 h y a la hora de inicio (N-12) y (b) operaciones en conflicto/rechazadas de los editores. Canal: recomendado, indicador dentro del panel; correo opcional; push/WhatsApp, posteriores. | M | COND:Q-10 | Un partido sin editor aparece en el panel del organizador del torneo ≤ 1 min después de entrar en la ventana; no aparece a organizadores ajenos. | I, E2E |
| RF-ADM-05 | Gestión de usuarios: ver, invitar, reenviar, revocar y cambiar asignaciones según la matriz §7. | M | CONF (Q-19) | Las acciones visibles coinciden con la matriz §7 para cada rol. | E2E, DB |

### 6.6 Auditoría (RF-AUD)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-AUD-01 | Cada registro contiene: actor (cuenta y rol con que actuó), hora **del servidor**, torneo, entidad, acción, valores antes/después **minimizados**, motivo (cuando aplica) y origen (en línea / sincronizado, con identificador de operación y hora del cliente). | M | CONF (Q-03, Q-19) + REC | 100 % de las mutaciones de la lista de RF-AUD-02 producen un registro con todos los campos. | DB |
| RF-AUD-02 | Se auditan: roles, invitaciones y asignaciones; torneo y reglamento; inscripciones y retiros; altas/bajas/anonimización de jugadores y consentimientos; generación, ajuste y publicación de calendario; estados de partido, resultados, eventos, correcciones y W.O.; resoluciones manuales de desempate. | M | CONF (Q-03, Q-19) | Prueba de cobertura: cada operación mutante del catálogo (Fase 4) genera registro. | DB |
| RF-AUD-03 | **Inmutabilidad:** ningún rol de la aplicación (incluido el dueño y el rol de servicio de la aplicación) puede actualizar ni borrar registros de auditoría; solo anexar. Se documenta el límite frente a un administrador de base de datos. | M | REC (H-07) | Intentos de `UPDATE`/`DELETE` por los 8 actores de prueba → denegados. | DB |
| RF-AUD-04 | Visibilidad por rol (ver RF-ADM-03). El registro no contiene datos personales de menores más allá de identificadores internos (T-04). | M | CONF (Q-05) | Revisión de esquema y de una muestra de 50 registros: sin nombres, fechas de nacimiento ni contactos. | DB, D |
| RF-AUD-05 | Los intentos denegados de escalada o acceso cruzado se registran en una bitácora de seguridad separada (no visible a organizadores) para el dueño. | S | REC | Un intento de organizador de invitar organizador genera un evento de seguridad. | DB |

### 6.7 PWA (RF-PWA)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-PWA-01 | Instalable en Android (Chrome) y iPhone (Safari, «Añadir a pantalla de inicio»), con ícono, nombre y pantalla de inicio; pantalla guía de instalación por plataforma. | M | CONF (PWA) | Instalación comprobada en 1 Android y 1 iPhone reales (iOS ≥ 16.4); lanzamiento standalone. | M |
| RF-PWA-02 | Tras la primera visita, el «cascarón» de la app y las vistas visitadas abren sin conexión con aviso de desactualizado. | M | DER (Q-17) | Con red cortada, la app abre en ≤ 3 s (dispositivo de gama media). | E2E |
| RF-PWA-03 | Las actualizaciones de la app no pierden la cola de operaciones (OFF-16). | M | DER (Q-06) | Ver OFF-16. | E2E |
| RF-PWA-04 | Notificaciones push: fuera del MVP. | P | CONF (Q-18) | No se solicita permiso de notificaciones. | — |

---

## 7. Matriz de permisos v2 (Q-02, Q-19)

**Leyenda:** **Sí** = permitido · **Sí\*** = permitido con la restricción indicada · **No** = denegado · **PEND** = depende de una decisión · «—» = no aplica. Todos los «suyos/asignados» se refieren a torneos o partidos **concretos**; ningún permiso se hereda entre torneos. El rol «Autenticado sin rol» se comporta como Público.

### 7.1 Matriz por acción

| # | Acción | Dueño | Organizador | Editor | Delegado | Público | Origen |
|---|---|---|---|---|---|---|---|
| 1 | Invitar/designar **organizador** | Sí | **No** | No | No | No | CONF Q-19 |
| 2 | Invitar **editor** a un torneo | Sí | Sí\* solo sus torneos | No | No | No | CONF Q-19 |
| 3 | Invitar **delegado** a un equipo de un torneo | Sí | Sí\* solo sus torneos | No | No | No | CONF Q-19 |
| 4 | Otorgar rol de **dueño** | PEND (Q-35) | **No** | No | No | No | CONF Q-19 |
| 5 | Revocar organizador | Sí | **No** | No | No | No | REC Q-02c |
| 6 | Revocar editor/delegado | Sí | Sí\* sus torneos (REC) | No | No | No | COND Q-02c |
| 7 | Asignar/desasignar editor a partido | Sí | Sí\* sus torneos (REC) | **No** (ni a sí mismo) | No | No | COND Q-02b |
| 8 | Ver usuarios y roles | Todos | Los de sus torneos | No | No | No | REC |
| 9 | Ver historial de cambios | Todo | Sus torneos | No (solo su cola) | No | No | DER Anexo A |
| 10 | Crear torneo | Sí | Sí\* queda como organizador de ese torneo | No | No | No | CONF Q-02 + COND Q-02a |
| 11 | Editar torneo y reglamento **antes de iniciar** | Sí | Sí\* suyos | No | No | No | CONF Q-03 |
| 12 | Cambiar reglamento de torneo **iniciado** | Sí (con confirmación) | PEND (Q-23) | No | No | No | COND Q-23 |
| 13 | Publicar/despublicar torneo y jornadas | Sí | Sí\* suyos | No | No | No | DER Anexo A |
| 14 | Archivar/finalizar torneo | Sí | Sí\* suyos | No | No | No | REC |
| 15 | Eliminar torneo en `borrador` sin partidos | Sí | No | No | No | No | REC |
| 16 | Inscribir/retirar equipo | Sí | Sí\* suyos | No | No | No | DER |
| 17 | Editar datos de la inscripción (nombre, colores) | Sí | Sí\* suyos | No | **No** | No | CONF Q-02 |
| 18 | Ver plantilla con datos internos | Sí | Sí\* suyos | Parcial\*\* | Sí\* su equipo | No | REC |
| 19 | Alta/baja/edición de jugadores en plantilla | Sí | Sí\* suyos | **No** | Sí\* su equipo | No | CONF Q-02 |
| 20 | Registrar/revocar autorización de publicación o consentimiento | Sí | Sí\* suyos | No | PEND (Q-24) | No | COND Q-24 |
| 21 | Anonimizar/eliminar un jugador (derechos del titular) | Sí | PEND | No | No | No | COND Q-24 |
| 22 | Generar/regenerar calendario | Sí | Sí\* suyos | No | No | No | DER Anexo A |
| 23 | Ajustar fecha, hora, sede, localía, jornada | Sí | Sí\* suyos | No | No | No | REC |
| 24 | Registrar marcador y eventos (en línea o sin conexión) | Sí | Sí\* suyos (REC) | Sí\* **solo partidos asignados** | No | No | CONF Q-02 (editor) / REC (organizador) |
| 25 | Cambiar estado del partido (iniciar, finalizar, suspender) | Sí | Sí\* suyos | Sí\* solo asignados | No | No | CONF Q-02 |
| 26 | Declarar/revertir W.O. | Sí | Sí\* suyos (REC) | **No** | No | No | COND Q-21 |
| 27 | **Corregir** o reabrir un partido finalizado | Sí | Sí\* suyos (REC) | **No** | No | No | COND Q-09 |
| 28 | Anular un evento ya confirmado | Sí | Sí\* suyos | Sí\* solo asignados y **antes de finalizar** | No | No | REC |
| 29 | Resolución manual de un desempate | Sí | Sí\* suyos (REC) | No | No | No | COND Q-22 |
| 30 | Ver torneos publicados, tablas, calendarios publicados, resultados confirmados, goleadores, tarjetas | Sí | Sí | Sí | Sí | **Sí** | CONF Q-02 |
| 31 | Ver torneo **no publicado** o jornada no publicada | Sí | Sí\* suyos | Solo sus partidos asignados | Solo su equipo y sus partidos | No | REC |
| 32 | Ver quién es el editor asignado de un partido | Sí | Sí\* suyos | Solo el propio | No | **No** | REC |
| 33 | Ver cola de operaciones y conflictos | Sí | Sí\* de sus torneos | Solo la propia | No | No | DER Q-17 |
| 34 | Exportar datos personales de un jugador | Sí | No | No | No | No | REC (PRV-08) |

\*\* El editor ve, de los equipos de sus partidos asignados, solo nombre público/etiqueta y dorsal, sin datos internos (PRV-11).

### 7.2 Restricciones transversales

| ID | Restricción | Estado | Aceptación |
|---|---|---|---|
| RT-01 | Todo permiso se evalúa por la tupla (usuario, torneo, rol, ámbito); no hay permisos globales salvo el dueño. | CONF Q-02 | Auditoría de esquema: ninguna regla de acceso omite el torneo, salvo la del dueño. |
| RT-02 | La autorización se comprueba en el **servidor y en la base de datos**; ocultar un control en la interfaz no es un control de seguridad. | CONF Q-02/Q-19 | Las pruebas de permisos se ejecutan **contra la API/BD sin usar la interfaz**. |
| RT-03 | Ningún rol puede conceder un rol igual o superior al propio, salvo que el dueño designe organizadores. | CONF Q-19 | Casos de §12 CL-19. |
| RT-04 | Nadie puede otorgarse a sí mismo un rol, una asignación ni ampliar su ámbito. | CONF Q-02 | Intento propio → denegado y registrado (RF-AUD-05). |
| RT-05 | **Conflicto de interés (recomendado):** un delegado no puede ser editor de partidos en que juega su propio equipo. | COND Q-15 | Asignar a un delegado como editor de un partido de su equipo → rechazado. |
| RT-06 | No se puede revocar ni degradar al último dueño. | REC | Ver RF-ACC-09. |
| RT-07 | El ámbito de una invitación nunca excede el de quien invita. | CONF Q-19 | Organizador invita con un torneo que no administra → rechazado. |
| RT-08 | **Integridad entre torneos:** toda referencia entre entidades (inscripción, plantilla, partido, evento, asignación) debe pertenecer al **mismo torneo**; no basta con claves foráneas independientes. | CONF (H-05 aceptado como insumo) | Intentos de insertar un partido de A con equipo de B, un evento con jugador de B, o una asignación de editor de B → rechazados por la base de datos. |
| RT-09 | Las credenciales de servicio con privilegios totales nunca llegan al navegador ni a la cola local. | REC (seguridad.md) | Revisión de código y de bundle en Fase 7. |
| RT-10 | Toda asignación, modificación o revocación de permisos queda auditada. | CONF Q-19 | Ver RF-ACC-11. |

### 7.3 Casos de prueba de permisos mínimos (esquema CRUD completo en Fase 4)

Actores a probar en **cada operación mutante y cada vista de datos no pública**: (1) anónimo, (2) autenticado sin rol, (3) delegado de su equipo, (4) delegado de otro equipo del mismo torneo, (5) editor asignado al partido, (6) editor del mismo torneo no asignado, (7) editor de otro torneo, (8) organizador del torneo, (9) organizador de otro torneo, (10) dueño. Pruebas con los roles reales de la aplicación (no superusuario). Esta es la matriz que exige la skill; la suite se escribe en la Fase 4 y se ejecuta desde el módulo 5C.

---

## 8. Reglas deportivas del MVP (liga)

Todo lo que sigue se limita a la **liga**. Los valores de Q-03 son [CONF]; lo marcado `REC` o `COND` no está decidido.

### 8.1 Formato

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-FOR-01 | Un torneo del MVP tiene una única etapa de liga (todos contra todos) con 1 o 2 vueltas fijadas antes de generar el calendario. | M | CONF (Q-03, Q-18) | No existe forma de crear grupos ni eliminatorias en interfaz ni API. | I |
| RD-FOR-02 | El diseño no debe impedir añadir después fases finales, grupos, eliminatorias con ida y vuelta, y su resolución de empates (Q-03 «previsto, no construido»). | P | CONF (Q-18) | Revisión del ADR y del modelo en Fases 2 y 4. | D |
| RD-FOR-03 | La plataforma soporta 6 a 16 equipos por torneo sin degradación; casos de prueba con 2, 7, 8, 16 equipos. | M | CONF (Q-06) | N-01 y límites. | U |

### 8.2 Calendario

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-CAL-01 | **Método del círculo.** Con n par: n−1 jornadas por vuelta y n/2 partidos por jornada. Con n impar: n jornadas por vuelta, (n−1)/2 partidos por jornada y **un descanso por equipo y vuelta**. | M | CONF (informe, Q-03) | Para n = 2…16 y 1 o 2 vueltas: (a) cada pareja se enfrenta exactamente una vez por vuelta; (b) ningún equipo juega dos veces en una jornada; (c) con n impar cada equipo descansa una sola jornada por vuelta; (d) total = n(n−1)/2 × vueltas; (e) en la vuelta 2 los mismos cruces con la localía invertida. | U (N-01: 7 y 8 equipos) |
| RD-CAL-02 | **Vista previa** antes de confirmar: no se persiste nada hasta confirmar; confirmar crea todos los partidos en una transacción o ninguno. | M | CONF (informe §7.2) | Cancelar la vista previa deja 0 filas nuevas; un fallo a mitad de la confirmación deja 0 filas. | I, DB |
| RD-CAL-03 | **Orden de equipos:** el organizador elige orden manual o sorteo aleatorio; el sorteo registra su semilla para poder reproducirlo y auditarlo. | M | REC | Misma semilla → mismo calendario; la semilla consta en la auditoría. | U |
| RD-CAL-04 | Cada partido tiene fecha, hora y sede (texto libre) **opcionales**; «Por definir» es un valor válido. Zona horaria `America/Costa_Rica` (UTC−6, sin horario de verano), formato dd/mm/aaaa y 24 h. | M | DER (Q-01) | Un partido sin fecha se muestra «Por definir»; las horas se muestran en hora de Costa Rica con independencia de la zona del dispositivo. | E2E |
| RD-CAL-05 | **Advertencias de programación:** mismo equipo en dos partidos a la misma fecha y hora, y misma sede a la misma hora. Requieren confirmación del organizador; no bloquean. | S | REC | Ambas advertencias se muestran y quedan en la auditoría si se confirman. | U, E2E |
| RD-CAL-06 | **Ajustes manuales** (cambiar de jornada, localía, fecha/hora/sede): solo en partidos `programado`; cada ajuste se audita; se impide que un equipo quede con dos partidos en la misma jornada; el calendario se marca «modificado manualmente» si deja de cumplir RD-CAL-01 (a). | M | CONF (informe §6.6) | Mover un partido provocando dos partidos del mismo equipo en la jornada → rechazado; ajustes válidos auditados. | U, I |
| RD-CAL-07 | **Regeneración:** solo si **todos** los partidos están en `programado`; antes de confirmar se listan las consecuencias (asignaciones de editor perdidas, jornadas publicadas retiradas, ajustes manuales perdidos); los partidos anteriores se archivan (no se pierden en la auditoría). | M | REC (C-14) | Con un partido `en_juego`, regenerar → rechazado. Con asignaciones, el aviso las enumera y tras confirmar quedan sin editor. | I |
| RD-CAL-08 | **Publicación por jornada:** estados `borrador` y `publicada`. Despublicar una jornada exige motivo y queda auditado; una jornada con resultados confirmados no se despublica salvo el dueño. | M | REC (H-12) | Un partido de una jornada `borrador` no aparece en el sitio público aunque el torneo esté publicado. | E2E, DB |
| RD-CAL-09 | La vista previa informa por equipo: número de partidos de local y de visitante y la racha máxima consecutiva. **No se garantiza** equilibrio perfecto (limitación documentada del método). | S | REC (informe §6.6) | El informe de localías es exacto en las fixtures. | U |

### 8.3 Reglamento del torneo

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-REG-01 | Cada torneo tiene un **reglamento versionado**: puntos por victoria/empate/derrota (3/1/0), número de vueltas, orden de criterios de desempate, W.O. (activo o no, marcador). Cada cambio crea una versión con actor, fecha, motivo y valores antes/después. | M | CONF (Q-03) | Después de 3 cambios existen 4 versiones consultables. | DB, I |
| RD-REG-02 | **Torneo iniciado:** pasa a ese estado en el instante en que **el primer partido sale de `programado`** (incluido un W.O.). Antes, el reglamento es libremente editable por organizador/dueño; las vueltas solo se editan si no hay calendario generado. | M | DER (Q-03) — definición por confirmar | El primer partido `en_juego` o `wo` bloquea las ediciones libres. | I |
| RD-REG-03 | **Cambios con el torneo iniciado** (controlados, validados y auditados): recomendado — solo el dueño; con motivo obligatorio; vista previa «tabla antes/después»; criterios de desempate se recalculan sobre todo el historial; **los puntos 3/1/0 y las vueltas no se modifican**; el marcador de W.O. aplica solo a W.O. futuros; se notifica a los organizadores del torneo. | M | COND:Q-23 | Un organizador intenta cambiar criterios con el torneo iniciado → denegado (si se adopta la recomendación); el dueño ve la vista previa y confirma; queda auditado con motivo. | I, DB |
| RD-REG-04 | **Validación:** criterios sin duplicados y dentro del catálogo; «resolución manual» siempre último y no removible; marcador de W.O. con enteros ≥ 0 y ganador > perdedor; puntos con enteros ≥ 0 (si se hicieran configurables, Q-33). | M | REC | Configuraciones inválidas rechazadas con mensaje claro. | U |
| RD-REG-05 | El **resumen del reglamento** (puntos, vueltas, criterios en orden, W.O.) es público en la página del torneo. | S | REC | La página pública lista los criterios en el orden configurado. | E2E |
| RD-REG-06 | **Puntos:** el MVP aplica 3/1/0 (confirmado). Hacerlos editables por torneo queda como decisión (Q-33); si no se aprueba, no se expone esa edición y el valor se lee del reglamento (una sola fuente, H-10). | M | CONF; edición COND:Q-33 | La tabla usa el valor del reglamento; no existe valor «cableado» en el cálculo. | U |

### 8.4 Tabla de posiciones y desempates

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-TAB-01 | Columnas: posición, equipo, PJ, G, E, P, GF, GC, DG, Pts. | M | CONF (Q-04) | Valores verificados contra fixtures. | U, E2E |
| RD-TAB-02 | Puntos: victoria 3, empate 1, derrota 0. Un W.O. suma como victoria (3) para el presente y derrota (0) para el ausente, y cuenta PJ. | M | CONF (Q-03) | Fixtures de liga completa de 6 y 8 equipos. | U |
| RD-TAB-03 | Solo cuentan los partidos `finalizado` y `wo` con resultado **confirmado por el servidor**; `programado`, `en_juego` y `suspendido` no cuentan. | M | DER (Q-17) | Una operación local pendiente no altera la tabla pública. | I |
| RD-TAB-04 | **Orden por defecto:** (1) Puntos; (2) Diferencia de goles; (3) Goles a favor; (4) Enfrentamiento directo; (5) Resolución manual. Los puntos son siempre el primer criterio y no son configurables en orden. Difiere del informe (que ponía la mini-tabla en 2.º lugar): prevalece la decisión. | M | CONF (Q-03) | N-02 reformulado (ver RD-TAB-07). | U |
| RD-TAB-05 | **Configuración** del orden y selección de criterios por torneo **antes de iniciar** (RD-REG-02). Catálogo recomendado para el MVP: diferencia de goles, goles a favor, goles en contra (menos), victorias, enfrentamiento directo; «resolución manual» fija al final. Fair play queda fuera. | M | CONF (configurable); catálogo COND:Q-28 | Reordenar criterios antes de iniciar cambia la tabla de forma predecible. | U |
| RD-TAB-06 | **Algoritmo determinista y terminante.** (1) Agrupar por puntos. (2) Para cada grupo empatado (≥ 2) aplicar los criterios en el orden configurado; los criterios globales comparan la tabla general; el enfrentamiento directo considera **solo los partidos entre los equipos del grupo empatado vigente** (ambas vueltas): puntos, luego diferencia y goles a favor de esa mini-tabla (detalle Q-27). (3) Si un criterio separa el grupo en subgrupos, cada subgrupo con empate restante continúa con el criterio siguiente; el enfrentamiento directo se recalcula para el subgrupo restante. (4) Agotados los criterios automáticos queda «empate sin resolver». Cada iteración reduce el tamaño del grupo o avanza de criterio: no hay bucles. | M | CONF (orden) + COND:Q-27 (semántica) | Propiedades: termina siempre; mismo resultado con cualquier orden de inserción de partidos; ninguna posición asignada dos veces a equipos separados por un criterio. | U (propiedades) |
| RD-TAB-07 | **Casos N-02 y N-03 reformulados para el orden de Q-03:** (a) dos equipos con igual puntaje: decide diferencia de goles; si igual, goles a favor; si igual, el resultado entre ambos; si igual, resolución manual. (b) Tres equipos A>B, B>C, C>A, mismos puntos, **misma DG y mismos GF**: la mini-tabla es idéntica; el empate queda sin resolver, sin bucle. (c) Mismo ciclo pero con DG distinta: resuelve la DG. (d) Tres empatados donde el enfrentamiento directo separa a uno y deja a dos: el par restante se recalcula con su propio partido. (e) Cuatro equipos con un 1G-1E-1P cada uno (analogía «círculo de la muerte»). | M | DER (Q-03) | Los cinco casos tienen fixture y resultado esperado documentado antes de 5F. | U |
| RD-TAB-08 | **Empate sin resolver:** se muestran con la misma posición compartida (p. ej. «3»), una marca «Empate sin resolver» y orden estable alfabético que **no** implica ventaja. | M | REC | La interfaz pública muestra la marca; el orden alfabético no se presenta como desempate. | E2E |
| RD-TAB-09 | **Resolución manual:** quien la ejecuta, con qué motivo y cómo se publica se define en Q-22. Recomendado: organizador del torneo o dueño; define el orden del grupo empatado; motivo obligatorio; etiqueta pública «Resolución manual de la organización»; reversible; queda **obsoleta** (vuelve a «sin resolver» con aviso) si cambian los resultados que determinaban ese empate. | M | COND:Q-22 | Ejecutar, revertir y volver obsoleta una resolución con una corrección posterior; todo auditado. | U, I |
| RD-TAB-10 | **Una sola fuente de verdad** para calcular la tabla (H-10): la decisión del lugar de cálculo se toma en la Fase 2; toda otra vista la consume. Si hubiera dos implementaciones, se exige prueba de equivalencia. | M | CONF (H-10 aceptado) | ADR aprobado; si hay dos, equivalencia sobre ≥ 10 fixtures. | D, U |
| RD-TAB-11 | **Recalculo** automático al confirmar, corregir o anular resultados o eventos de gol (N-07); la tabla pública refleja el cambio según RF-PUB-09. | M | CONF (informe N-07) | Corregir un resultado de una jornada anterior cambia posiciones y deja registro. | I, E2E |
| RD-TAB-12 | Rendimiento: calcular la tabla de un torneo de 16 equipos a doble vuelta (240 partidos) en ≤ 200 ms en el servidor (p95, propuesto). | S | REC | Medición en staging. | L |

### 8.5 W.O. (walkover)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-WO-01 | W.O. **configurable por torneo**: activado o no, y marcador de adjudicación; predeterminado **3-0** a favor del equipo presente. | M | CONF (Q-03) | Configuración por defecto 3-0; cambiarla antes de iniciar surte efecto en W.O. nuevos. | U, I |
| RD-WO-02 | **Declaración:** el partido pasa de `programado` a `wo` indicando el equipo ausente; motivo obligatorio. Lo declaran el organizador del torneo y el dueño; el editor no (puede dejar una observación). | M | COND:Q-21 (recomendado) | Editor intenta declarar → denegado. | DB, I |
| RD-WO-03 | **Efectos:** resultado = marcador W.O.; suma PJ, puntos (3/0) y GF/GC con ese marcador; **no genera eventos, goleadores ni tarjetas**; se identifica como «W.O.» en las vistas públicas. | M | CONF (Q-03); GF/GC por defecto DER, COND:Q-21 | Tras un W.O., el ranking de goleadores y tarjetas no cambia y la tabla sí. | U, I |
| RD-WO-04 | **Ausencia de ambos equipos** (doble W.O.): recomendado — ambos 0 puntos, PJ +1 cada uno, marcador 0-0 sin GF/GC, etiqueta «Doble W.O.». Sanciones adicionales (descuento de puntos, expulsión por W.O. reiterados): fuera del MVP; el sistema muestra el conteo de W.O. por equipo. | M (doble W.O.) / P (sanciones) | COND:Q-21 | Fixtures de W.O. simple y doble. | U |
| RD-WO-05 | **Auditoría:** actor, motivo, configuración aplicada, hora; reversible por el organizador o el dueño con motivo (`wo` → `programado`). | M | CONF (Q-03 «auditadas») | Todo W.O. y su reversión tienen registro. | DB |
| RD-WO-06 | Un W.O. solo puede declararse sobre un partido **sin eventos activos** y en `programado`; si ya empezó debe corregirse por RD-RES-06. | M | REC | Intento sobre un partido con eventos → rechazado. | I |
| RD-WO-07 | **Consistencia marcador–eventos:** la regla «goles de jugadores = marcador» (RD-EVT-04) **excluye** los partidos `wo`. | M | DER (T-05) | Un `wo` 3-0 sin eventos es válido. | U, DB |

### 8.6 Jugadores y plantillas

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-JUG-01 | Datos mínimos de un jugador **por inscripción**: nombre y apellidos (internos), nombre público (opcional; ver PRV-02), dorsal (opcional), posición (opcional), indicador «es menor de edad» (sí/no; ver PRV-04 y Q-20). Sin documento, teléfono, correo, dirección ni foto. | M | CONF (Q-05 minimización) + COND:Q-20 | La interfaz y el esquema no admiten otros campos personales. | D, I |
| RD-JUG-02 | **Un jugador, un equipo por torneo:** el sistema impide que la misma persona figure en dos equipos inscritos del mismo torneo, con la garantía en base de datos y tolerancia a envíos simultáneos. Sin documento no hay identidad perfecta (Q-12): se avisa de **posibles duplicados** (mismo nombre normalizado) y se exige confirmación del organizador; el límite queda documentado. | M | CONF (informe N-09); mecanismo COND:Q-12 | (a) alta de un jugador existente en otro equipo del torneo → rechazada; (b) dos altas simultáneas → una gana; (c) variante de escritura → advertencia. | DB, I |
| RD-JUG-03 | Dorsal único por equipo inscrito cuando se indica. | M | REC | Duplicado rechazado. | DB |
| RD-JUG-04 | **Bajas sin borrar** si el jugador tiene eventos: pasa a `baja`; sus eventos históricos se conservan; no puede recibir eventos nuevos. | M | REC | Un jugador con 2 goles dado de baja sigue en la tabla de goleadores; un gol nuevo para él se rechaza. | DB |
| RD-JUG-05 | **Cierre de plantilla, mínimo/máximo de jugadores y elegibilidad por categoría/edad:** Q-31. Recomendado: fecha de cierre configurable por torneo (después del cierre solo el organizador modifica), sin mínimos/máximos obligatorios en el MVP y sin elegibilidad por edad. | M (cierre) | COND:Q-31 | Después del cierre, el delegado no puede editar; el organizador sí, con auditoría. | I |
| RD-JUG-06 | El delegado edita **solo** la plantilla de su equipo; organizador y dueño la de equipos de su ámbito; el editor no la edita. Cambios auditados. | M | CONF (Q-02) | Ver §7.1 filas 18–19 y CL-20. | DB |
| RD-JUG-07 | Alta masiva (importar lista): fuera del MVP. | P | REC | — | — |

### 8.7 Partidos, estados y resultados

**Estados del MVP:** `programado`, `en_juego`, `finalizado`, `wo`, `suspendido`. «Aplazado» se trata como **reprogramación** (cambio de fecha, RD-RES-09); «cancelado» queda pendiente (Q-29).

| De → A | Quién | Condición |
|---|---|---|
| `programado` → `en_juego` | editor asignado, organizador, dueño | — |
| `programado` → `finalizado` | ídem | captura directa del resultado final (necesaria para captura tardía o sin conexión) |
| `programado` → `wo` | organizador, dueño | RD-WO-02 |
| `en_juego` → `finalizado` | editor asignado, organizador, dueño | RD-RES-03 |
| `en_juego` → `suspendido` | editor asignado, organizador, dueño | motivo |
| `suspendido` → `en_juego` o `finalizado` | organizador, dueño | motivo; define qué pasa con el marcador (Q-29) |
| `finalizado` → corrección (sigue `finalizado`) | organizador, dueño | RD-RES-06 |
| `finalizado` → `en_juego` (reabrir) | organizador, dueño | motivo |
| `wo` → `programado` | organizador, dueño | RD-WO-05 |

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-RES-01 | Solo son válidas las transiciones de la tabla; cualquier otra se rechaza. | M | REC (H-08) | Tabla de transiciones probada completa (válidas e inválidas). | U, DB |
| RD-RES-02 | Marcador: enteros ≥ 0 con tope técnico definido en Fase 4 (propuesto 99). | M | REC | Negativos y no enteros rechazados. | U, DB |
| RD-RES-03 | **Finalizar** exige marcador de ambos equipos y una confirmación explícita con resumen («Local 2 – 1 Visitante; 3 goles registrados; 0 sin autor»). | M | REC | No existe `finalizado` con marcador nulo. | DB, E2E |
| RD-RES-04 | **Concurrencia (N-06):** toda modificación de marcador/estado envía la versión conocida; si el servidor tiene otra, rechaza con un código de **conflicto de aplicación** propio (no un error de base de datos) y devuelve la versión actual; no sobrescribe. | M | CONF (informe N-06, Q-06) | Dos sesiones guardan sobre la misma versión: la segunda recibe conflicto y los datos no cambian. | I, DB |
| RD-RES-05 | **Idempotencia:** ver OFF-06. | M | CONF (Q-06) | OFF-06. | I |
| RD-RES-06 | **Corrección (N-07):** el organizador del torneo o el dueño puede corregir marcador y eventos de un partido finalizado en **una sola operación atómica**, con motivo obligatorio (≥ 10 caracteres, propuesto); se audita antes/después; se recalcula la tabla; en un torneo `finalizado` solo el dueño. Disputas: sin flujo propio en el MVP; se resuelven con una corrección y su motivo. | M | COND:Q-09 | Corregir no deja un estado intermedio público; el historial muestra el antes y el después. | I, E2E |
| RD-RES-07 | **Publicación:** al aceptar el servidor un resultado y estar visible la jornada, se publica automáticamente; no hay paso de validación previa. | M | COND:Q-32 | Ver RF-PUB-04 y §9.6. | I |
| RD-RES-08 | Con el partido `en_juego`, el público ve «En juego» y el último marcador confirmado, **sin tiempo real**. | S | REC (Q-04 excluye en vivo) | Se actualiza al refrescar o en el ciclo de RF-PUB-09. | E2E |
| RD-RES-09 | **Reprogramación:** cambiar fecha/hora/sede no cambia el estado y deja historial; un partido reprogramado conserva su jornada salvo ajuste manual. | M | REC | El historial muestra fecha anterior y nueva. | I |
| RD-RES-10 | **Suspensión:** `suspendido` requiere motivo; continuar, o cerrar con el marcador vigente, lo decide el organizador/dueño. Estados «aplazado» y «cancelado» y su efecto en la tabla: Q-29. | M (suspensión) | COND:Q-29 | — | I |
| RD-RES-11 | Cada cambio de marcador o estado registra su origen: en línea o sincronizado, identificador de operación y hora del cliente (OFF-13). | M | DER (Q-17) | Auditoría muestra el origen. | DB |

### 8.8 Eventos, goleadores y tarjetas

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-EVT-01 | Tipos de evento del MVP: `gol`, `autogol`, `amarilla`, `roja`. Penales convertidos, asistencias, cambios: fuera del MVP. | M | CONF (Q-04: goleadores y tarjetas); tipos REC | Otro tipo se rechaza. | U, DB |
| RD-EVT-02 | Campos: partido, equipo del jugador, jugador, minuto (entero 0–150, **opcional**), sin conexión también la hora local. Jugador obligatorio en `amarilla`, `roja`, `gol` y `autogol`; excepción «gol sin autor» solo si Q-30 lo aprueba. | M | REC; excepción COND:Q-30 | Casos válidos e inválidos. | U, DB |
| RD-EVT-03 | **Integridad:** el jugador debe estar **activo en la plantilla de uno de los dos equipos del partido, en el mismo torneo**; el equipo del evento debe ser uno de los dos del partido. | M | CONF (H-05) | Insertar un evento con un jugador de otro torneo/equipo → rechazado por la base de datos. | DB |
| RD-EVT-04 | **Consistencia marcador–eventos:** marcador de un equipo = goles de sus jugadores + autogoles de jugadores rivales. Al finalizar se advierte si la suma de eventos difiere del marcador; el tratamiento (bloquear, permitir con «goles sin autor») lo decide Q-30; los `wo` están exentos. | M | COND:Q-30 | Fixtures: suma coincide, suma menor, suma mayor, `wo`. | U |
| RD-EVT-05 | **Anulación, no borrado:** un evento se anula (`anulado`); el editor asignado puede anular eventos antes de finalizar; después solo organizador/dueño por corrección. Lo anulado no cuenta en estadísticas y queda en la auditoría. | M | REC | Anular un gol cambia marcador/estadísticas y conserva el evento marcado. | I, DB |
| RD-EVT-06 | **Tarjetas:** `amarilla` y `roja`. Una segunda amarilla del mismo jugador en el partido sugiere registrar también la `roja` (el usuario confirma); eventos posteriores de un jugador expulsado generan **advertencia**, no bloqueo. **Suspensiones automáticas por acumulación: fuera del MVP.** | M / P | REC | Prompt de doble amarilla probado. | U, E2E |
| RD-EVT-07 | **Goleadores:** ranking por goles (sin autogoles, sin anulados, sin `wo`); posiciones compartidas con igual cantidad; orden estable alfabético por nombre público; filtro por equipo. | M | CONF (Q-04) | Fixture con empates de goles y un jugador con nombre oculto (PRV-02). | U, E2E |
| RD-EVT-08 | **Tarjetas:** listado por jugador (amarillas, rojas) y total por equipo. Sin puntaje de fair play. | M | CONF (Q-04) | Fixture. | U, E2E |
| RD-EVT-09 | Un partido `wo` no admite eventos. | M | CONF (Q-03) | Intento → rechazado. | DB |
| RD-EVT-10 | Orden de eventos: por minuto (sin minuto al final) y luego por orden de captura; estable ante reenvíos. | M | REC | Reenviar la cola no cambia el orden. | U, I |
| RD-EVT-11 | Tope de eventos por partido (propuesto 200) para evitar abuso o errores en cola. | S | REC | Evento 201 rechazado con mensaje. | I |

---

## 9. Captura sin conexión (Q-06, Q-17)

### 9.1 Alcance [CONF/DER]

- **Dentro:** capturar **marcador, eventos y cambios de estado del partido** sin conexión, guardarlos en el dispositivo y sincronizarlos al recuperar conexión; consultar los datos preparados de los partidos del usuario. [CONF Q-17]
- **Fuera (requieren conexión):** crear/editar torneos, equipos, jugadores, reglamento, calendario, usuarios y roles, correcciones de partidos finalizados, resolución manual. Los controles aparecen deshabilitados con explicación. [DER de Q-17]
- Quienes pueden capturar sin conexión son quienes pueden registrar resultados de ese partido (editor asignado; organizador/dueño en sus torneos). Todo permiso se valida de nuevo en el servidor al sincronizar (OFF-22).

### 9.2 Estados de una operación local

| Estado | Significado | Visible al público |
|---|---|---|
| `pendiente` | Guardada en el dispositivo; no enviada | **No** |
| `enviando` | En tránsito | No |
| `confirmada` | El servidor la aceptó y es parte del estado oficial | Sí, cuando el partido es visible (§9.6) |
| `en conflicto` | El servidor no pudo aplicarla sin pisar un cambio posterior; esperando decisión | No |
| `rechazada` | El servidor la denegó (permiso, asignación, validación) | No |

### 9.3 Requisitos

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| OFF-01 | Capturar sin conexión: marcador, eventos (RD-EVT-01), iniciar, suspender y finalizar el partido. | M | CONF (Q-17) | Con red cortada se completa un partido de punta a punta (inicio, 3 goles, 2 tarjetas, final) sin errores. | E2E |
| OFF-02 | **Preparación del dispositivo:** con conexión, se descargan los partidos asignados no finalizados del usuario (por defecto los próximos 7 días y los en juego; ajustable, Q-34), las plantillas de ambos equipos limitadas a etiqueta/nombre público y dorsal, y el resumen del reglamento. La interfaz indica «Listo para trabajar sin conexión — preparado hh:mm» o «Sin preparar». | M | DER (Q-17) + parámetros COND:Q-34 | Tras preparar y activar modo avión, el editor abre sus partidos y captura; un partido no preparado muestra el aviso. | E2E |
| OFF-03 | **Cola local persistente**: sobrevive a cerrar la app, a un reinicio del dispositivo y a perder la pestaña. Cada operación tiene identificador único generado en el cliente, número de secuencia por partido, tipo, datos, versión base conocida, dispositivo, usuario y hora local. | M | CONF (Q-06, Q-17) | Cerrar a la fuerza la app con 5 operaciones pendientes y reabrirla: las 5 siguen ahí, en orden. | E2E |
| OFF-04 | **Diferenciación visual** de pendiente / enviando / confirmada / en conflicto / rechazada, por operación y con un resumen por partido («2 pendientes»), usando texto e icono además de color. El marcador local se muestra junto al confirmado cuando difieren. | M | CONF (Q-06, Q-17) | Revisión manual + axe: no se depende solo del color. | E2E, A |
| OFF-05 | **Disparadores de sincronización:** al recuperar conexión con la app abierta, al abrir/volver a la app, por botón «Sincronizar ahora» y reintento periódico con espera creciente. **No** se depende de sincronización en segundo plano (no existe en iOS). Indicador permanente del número de pendientes. | M | DER (§2.1 informe) | En iPhone real, con la app en primer plano, la cola se sincroniza al volver la red; con la app cerrada no se promete envío. | E2E, M |
| OFF-06 | **Idempotencia en el servidor:** cada operación se aplica **como máximo una vez**; reenviar el mismo identificador (también concurrentemente) devuelve el resultado original sin repetir efectos ni registros de auditoría. | M | CONF (Q-06, N-13) | Enviar la misma operación 1, 2 y 10 veces (secuencial y simultánea): 1 efecto, 1 registro, misma respuesta. | I, DB |
| OFF-07 | **Orden:** las operaciones de un mismo partido se aplican en su orden de secuencia; si una falla, las siguientes de **ese partido** quedan «bloqueadas por operación anterior» (siguen `pendiente`) hasta resolverla; las de otros partidos continúan. | M | REC | Falla la op 3 de 6: 4–6 no se aplican y se muestra la causa; otras pruebas en otro partido sí. | I |
| OFF-08 | **Atomicidad:** cada operación se aplica completa o no se aplica; una interrupción de la red durante el envío no deja estados parciales. | M | REC | Corte simulado en mitad de un lote: al reanudar, cada operación está en un solo estado coherente. | I |
| OFF-09 | **Política de conflictos** (tabla §9.4). Regla general: **lo confirmado por el servidor nunca se sobrescribe en silencio** y **ninguna captura local se pierde en silencio**. | M | CONF (Q-06) + política REC | Cada fila de §9.4 tiene una prueba automatizada. | I, E2E |
| OFF-10 | **Nunca descarte silencioso:** las operaciones en conflicto o rechazadas se conservan localmente hasta que el usuario (a) las reintente cuando proceda, (b) las descarte con confirmación o (c) copie/exporte un resumen en texto para el organizador. Retención máxima propuesta: 30 días. | M | REC; plazo COND:Q-34 | No existe ruta de código que borre una op rechazada sin acción explícita. | I, E2E |
| OFF-11 | **Revocación y desasignación:** al sincronizar, operaciones de un usuario desasignado o sin rol se rechazan con motivo claro. Si se revocó solo la asignación/rol: se conservan en estado `rechazada` (OFF-10). Si se **desactivó la cuenta**: la app informa, ofrece exportar el resumen y borra los datos locales. | M | CONF (T-03, Q-02) + política COND:Q-34 | Pruebas con desasignación y con cuenta desactivada a mitad de captura. | I, E2E |
| OFF-12 | **Datos y sesión locales:** solo los datos mínimos necesarios (OFF-02); sin datos internos de jugadores (PRV-11); cerrar sesión con pendientes exige confirmación explícita y borra los datos locales tras sincronizar o descartar; pérdida del dispositivo: el dueño/organizador revoca la cuenta (el servidor deja de aceptar); **una PWA no puede borrar datos a distancia** — se documenta el riesgo residual. El cifrado local se evalúa en Fase 2 y no se promete. | M | CONF (Q-17) + REC | Inspección del almacenamiento local en pruebas: ningún campo de la lista «nunca público»/interno. | I, D |
| OFF-13 | **Relojes:** el minuto del partido lo ingresa o deriva el usuario; las horas del dispositivo son informativas; el orden se basa en secuencia, no en reloj; el servidor registra su propia hora de recepción; si el reloj del dispositivo difiere > 5 min del servidor al sincronizar, se avisa y se registra. | M | REC | Reloj adelantado 2 h: el orden y la tabla no cambian. | I |
| OFF-14 | **Duplicados por la interfaz:** protección contra doble toque (espera ≤ 500 ms), «Deshacer» durante 10 s tras cada evento, aviso de «posible duplicado» (mismo tipo, jugador y minuto en ≤ 60 s). Un duplicado por contenido con identificador distinto **no** se rechaza automáticamente (podría ser legítimo). | M | REC | Doble toque rápido genera 1 evento; el aviso aparece. | E2E |
| OFF-15 | **Lo local jamás es público:** el público solo ve operaciones `confirmada` de partidos visibles (§9.6). | M | CONF (Q-17) | Ver §9.6. | I, E2E |
| OFF-16 | **Actualización de la app con cola pendiente:** una nueva versión no descarta la cola; el formato de la cola tiene versión y se migra; si no es compatible, se bloquea la actualización hasta sincronizar. | M | REC | Pruebas versión N → N+1 con 5 operaciones pendientes. | E2E |
| OFF-17 | **Persistencia del almacenamiento:** se solicita almacenamiento persistente y se avisa si no se concede; riesgo residual documentado (el navegador puede limpiar datos; comportamiento de iOS a verificar en dispositivo real en Fase 2). | S | REC | Aviso visible cuando no se concede. | M |
| OFF-18 | Consulta sin conexión: el editor ve sus partidos preparados; el público ve la última versión visitada con hora (RF-PUB-06). | M | CONF (Q-17) | RF-PUB-06, OFF-02. | E2E |
| OFF-19 | **Rendimiento de sincronización:** 50 operaciones pendientes se sincronizan en ≤ 15 s con perfil de red 3G lenta (propuesto). | S | REC | Medición con red limitada. | I |
| OFF-20 | **Métrica de operación:** el servidor cuenta operaciones aplicadas, en conflicto y rechazadas por motivo (sin datos personales) para vigilar el piloto. | S | REC | Panel/consulta con conteos. | I |
| OFF-21 | **Finalizar sin conexión:** se permite como «finalización pendiente»; el partido solo se considera finalizado y público cuando el servidor la confirma. Si el servidor la rechaza, el partido conserva su estado oficial y la operación pasa a `en conflicto`/`rechazada`. | M | COND:Q-34 | Final offline + sincronización con y sin conflicto. | E2E |
| OFF-22 | **La autorización definitiva es siempre del servidor/BD**; ningún dato local concede permisos. | M | CONF (Q-02) | Alterar manualmente la cola local para apuntar a otro partido/torneo → rechazado. | I, DB |

### 9.4 Política de conflictos (recomendada, COND:Q-34)

| # | Escenario | Resultado de la operación | Lo que ve el usuario | Resuelve |
|---|---|---|---|---|
| 1 | Operación de **evento** (gol, tarjeta, anulación) y el partido sigue sin finalizar, el usuario sigue asignado | Se **acepta** (los eventos se anexan; no se pisan) y se ordena por minuto | `confirmada` | — |
| 2 | Operación de **marcador/estado** con versión base desactualizada porque otro actor cambió el partido | **No se aplica**; `en conflicto` | Versión local y versión del servidor lado a lado | Usuario descarta o reaplica sobre la versión actual; el organizador decide si hay duda |
| 3 | El partido fue **finalizado o corregido** en el servidor por otro actor | `en conflicto`; no se aplica nada | «El partido ya fue finalizado por [rol]»; opciones de OFF-10 | Organizador (corrección RD-RES-06) |
| 4 | Editor **desasignado** | `rechazada` («sin asignación») | Motivo + exportar resumen | Organizador reasigna o corrige |
| 5 | **Rol o cuenta revocados** | `rechazada`; cuenta desactivada → OFF-11 | Aviso + exportar | Dueño/organizador |
| 6 | **Jugador** del evento dado de baja o anonimizado mientras tanto | `en conflicto` | «Jugador no disponible»; elegir otro jugador o «sin autor» si se aprobó (Q-30) | Editor/organizador |
| 7 | Partido **reprogramado** | Se acepta | `confirmada` | — |
| 8 | Partido pasa a **W.O.** o **cancelado** (si se aprueba) | `en conflicto`/`rechazada` | Motivo | Organizador |
| 9 | **Mismo identificador** recibido otra vez (reintento) | Idempotente: devuelve el primer resultado | `confirmada` | — |
| 10 | **Mismo editor, dos dispositivos** | Se fusionan por identificador y secuencia; las de marcador/estado con versión desactualizada → caso 2 | Aviso si hay colisión | Editor |
| 11 | Dos editores asignados al mismo partido (solo si se permite) | Eventos: caso 1; marcador/estado: caso 2 (N-06) | Según caso | Editor/organizador |
| 12 | Contenido duplicado con otro identificador | Se acepta (podría ser legítimo); aviso previo al capturar | `confirmada` | Anulación (RD-EVT-05) |

### 9.5 Parámetros por decidir (Q-34) — propuestas

| Parámetro | Propuesta |
|---|---|
| Ventana de preparación | Partidos asignados no finalizados de los próximos 7 días y en juego |
| Antigüedad de los datos preparados antes de avisar | 24 h |
| Retención de operaciones rechazadas/en conflicto | 30 días o hasta acción del usuario |
| ¿Finalizar sin conexión? | Sí, como «finalización pendiente» |
| Cuenta desactivada con operaciones pendientes | Informar, ofrecer exportar, borrar datos locales |
| Capturar después de que expire la sesión | Sí (la captura no necesita servidor); para **sincronizar** se exige sesión válida |

### 9.6 Visibilidad pública (publicación) — matriz

Un resultado es público solo si se cumplen **las tres**: torneo `publicado`/`en curso`/`finalizado`; jornada `publicada`; operación `confirmada` por el servidor.

| Torneo | Jornada | Dato | ¿Público? |
|---|---|---|---|
| borrador | cualquiera | cualquiera | No |
| publicado | borrador | cualquiera | No |
| publicado | publicada | operación `pendiente`/`enviando`/`en conflicto`/`rechazada` | No |
| publicado | publicada | `confirmada` | **Sí** |
| archivado | — | cualquiera | No (salvo decisión posterior de histórico, Q-11) |

---

## 10. Privacidad y consentimiento de menores (Q-01, Q-05)

> **Aviso.** Las referencias legales de esta sección son conocimiento general **no verificado** y **no son asesoría legal**: Ley N.º 8968 (protección de datos personales) y su Reglamento, PRODHAB, Código de la Niñez y la Adolescencia (Ley N.º 7739). Toda cifra de edad, plazo o base legal está marcada **[PEND-JUR]**. La revisión jurídica previa al lanzamiento es una **puerta obligatoria** [CONF].

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| PRV-01 | La responsable del tratamiento es la organización Torneo de Campeones; el marco aplicable es el de Costa Rica. Los requisitos de esta sección se revisan jurídicamente antes del lanzamiento. | M | CONF (Q-01) | Documento de revisión jurídica archivado antes del lanzamiento (puerta de Fase 8). | D |
| PRV-02 | **Nombre público condicionado.** Un jugador **menor de edad** aparece en las vistas públicas solo como «Jugador n.º {dorsal} ({equipo})» **mientras no conste una autorización de publicación vigente**; con autorización aparece con su nombre público. Para adultos se propone la misma regla por defecto conservadora, ajustable por la organización tras la revisión jurídica. Las estadísticas (goles, tarjetas) cuentan igual con etiqueta o con nombre. | M | CONF (Q-05 «información deportiva necesaria»); regla de adultos COND:Q-20 / PEND-JUR | Fixture: jugador menor sin autorización → etiqueta en plantilla, goleadores y tarjetas; con autorización → nombre; al revocar → vuelve a etiqueta. | E2E, DB |
| PRV-03 | **Nunca públicos** (en ninguna vista ni respuesta pública): fecha de nacimiento o edad exacta, documento de identidad, teléfono, correo, dirección, fotografías, datos y evidencia de consentimiento, identidad del editor asignado, correos de usuarios, nombre completo interno de un jugador sin autorización. | M | CONF (Q-05; skill) | Revisión de esquema y pruebas de contrato de cada vista/endpoint público (ningún campo de la lista). | DB, I, D |
| PRV-04 | **Condición de menor:** el sistema guarda un indicador «es menor de edad» (sí/no) declarado por quien registra al jugador, **no la fecha de nacimiento**, para minimizar datos. Edad de corte propuesta: menor de 18 años [PEND-JUR]. Limitación documentada: el indicador debe actualizarse manualmente cuando el jugador cumple la mayoría de edad. | M | REC; Q-20 [PEND] | El esquema no contiene fecha de nacimiento; el indicador gobierna PRV-02. | D, DB |
| PRV-05 | **Registro de autorización/consentimiento** (por jugador): tipo (publicación del nombre / tratamiento de datos), otorgante (jugador adulto o representante legal — solo su relación, no sus datos de contacto), fecha, versión del texto aceptado, quién la registró, estado (vigente / revocada) y fecha de revocación. **La evidencia original (papel o archivo) la conserva la organización**; el sistema no almacena el documento en el MVP. | M | COND:Q-24 (recomendado) / PEND-JUR | Sin registro vigente no hay nombre público de menores (PRV-02); toda alta/revocación se audita. | DB, E2E |
| PRV-06 | **Revocación y efectos.** Dos acciones distintas: **ocultar** (vuelve a etiqueta; reversible) y **anonimizar** (borra irreversiblemente nombre y datos personales; conserva identificador interno, dorsal, estadísticas y resultados). Tras anonimizar, los totales de equipo, tabla y goleadores no cambian. La auditoría solo guarda identificadores internos (T-04). | M | CONF (Q-05 revocación) | Anonimizar un jugador con 3 goles: tabla y goleadores iguales; ningún campo personal queda en base de datos ni en auditoría; vista pública con etiqueta. | DB, I |
| PRV-07 | **Aviso de privacidad** y texto de autorización redactados con asesoría jurídica, versionados; la responsable y los canales de contacto se indican en el aviso. | M | CONF (Q-01, Q-05) / PEND-JUR | Texto aprobado y enlazado antes del lanzamiento. | D, M |
| PRV-08 | **Ejercicio de derechos** (acceso, rectificación, cancelación u oposición): la organización designa a la persona que atiende; el dueño puede **exportar** los datos de un jugador y **anonimizarlo**; plazos de respuesta: los que fije la revisión jurídica. | M | COND:Q-24 / PEND-JUR | Exportar y anonimizar un jugador de prueba en un entorno de pruebas, con auditoría. | I, M |
| PRV-09 | **Control de acceso a datos de menores:** nombre interno y su indicador solo los ven dueño, organizador del torneo y delegado de su equipo; el editor ve etiqueta/nombre público y dorsal; el público, según PRV-02. | M | CONF (Q-05 «controles de acceso») | Pruebas de lectura por los 10 actores de §7.3. | DB |
| PRV-10 | **Retención:** se definen plazos para jugadores dados de baja, invitaciones vencidas, operaciones locales (OFF-10), bitácora de seguridad y auditoría; la auditoría se conserva mientras exista el torneo más el plazo jurídico. Valores: PEND-JUR. | M | PEND-JUR | Política escrita y aplicada en Fase 8. | D |
| PRV-11 | **Minimización en el dispositivo y en el editor:** los datos descargados para trabajar sin conexión contienen solo etiqueta/nombre público y dorsal, nunca los campos internos. | M | CONF (Q-05) + DER (Q-17) | Inspección del almacenamiento local en pruebas (OFF-12). | I |
| PRV-12 | **Registros y monitoreo sin datos personales:** los registros técnicos y el monitoreo de errores omiten nombres, correos e identificadores de dispositivo cuando sea posible. | M | REC | Revisión de una muestra de 100 registros: 0 datos personales. | D |
| PRV-13 | **Ubicación y proveedores:** se documentan los proveedores y la región de almacenamiento y se evalúa si hay transferencia fuera de Costa Rica (Q-25) con asesoría jurídica antes de contratar. | M | CONF (Q-07: verificar antes de contratar) + PEND-JUR | ADR de la Fase 2 con la región y los proveedores. | D |
| PRV-14 | **Datos de usuarios del sistema** (correo y rol): visibles solo para dueño y, de sus torneos, organizadores; no públicos. | M | DER | Pruebas de lectura por actor. | DB |
| PRV-15 | **Puerta de lanzamiento:** no se publica ni se carga ningún dato real de menores sin revisión jurídica concluida y texto de autorización aprobado. | M | CONF (Q-05, §18.6 Fase 0) | Evidencia documental en el checklist de Fase 8. | D |
| PRV-16 | **Relación con el desarrollador:** formalizar por escrito responsable, encargo de tratamiento y confidencialidad antes de manejar datos reales. | M | CONF (Q-01 pendiente de formalizar) | Documento firmado antes del piloto. | D |

**Preguntas de privacidad abiertas:** Q-20 (campos públicos y edad de corte), Q-24 (quién recolecta, dónde queda la evidencia, quién atiende solicitudes) y Q-25 (ubicación de los datos); ver §13.

---

## 11. Requisitos no funcionales

| ID | Atributo | Requisito | Prio. | Estado | Aceptación / medición | Prueba |
|---|---|---|---|---|---|---|
| RNF-01 | Rendimiento web | Core Web Vitals «bueno» en p75: LCP ≤ 2,5 s, INP ≤ 200 ms, CLS ≤ 0,1 en vistas públicas, en celular de gama media con red 3G rápida. Los datos de laboratorio no sustituyen los de campo. | M | REC (informe §4.3) | Medición en staging con dispositivo real; después del piloto, datos de campo. | L, M |
| RNF-02 | Accesibilidad | WCAG 2.2 AA en todas las pantallas del MVP; objetivos táctiles ≥ 44 px; foco visible; teclado completo; contraste verificado. Ninguna herramienta sola demuestra cumplimiento. | M | CONF (skill) | axe sin críticos/serios + revisión manual con lista de verificación + prueba con lector de pantalla en 3 flujos. | A |
| RNF-03 | Disponibilidad y degradación | La **captura** no depende de la disponibilidad del servidor (OFF); la **lectura pública** degrada a la última versión en caché con hora; objetivo numérico de disponibilidad: Q-16 (el 99,5 % del informe no tiene respaldo contractual verificado). | M | COND:Q-16 | Prueba de caída simulada del servidor: captura y lectura en caché siguen funcionando. | I, E2E |
| RNF-04 | Capacidad | 3–6 torneos simultáneos, 6–16 equipos por torneo, hasta 240 partidos por torneo; lectura pública simultánea de 100–500 usuarios en jornada [SUP, no confirmada]. | M | CONF (Q-06) / SUP (público) | Carga con 500 lectores simultáneos sin errores > 1 %, p95 de lectura ≤ 1 s (propuesto). | L |
| RNF-05 | Seguridad | Autorización en servidor y BD (RT-02); RLS/equivalente probada por rol, tabla y operación; credenciales de servicio solo en servidor; cabeceras de seguridad; dependencias escaneadas; límites de frecuencia en autenticación e invitaciones. | M | CONF (Q-02) | pgTAP completo (Fase 4/7); 0 hallazgos críticos o altos abiertos antes del piloto. | DB, I |
| RNF-06 | Integridad | Ningún resultado confirmado se pierde por concurrencia, reintento o desconexión; las restricciones críticas están en la base de datos y no solo en la aplicación. | M | CONF (Q-06) | N-06, N-13, RD-RES-04, OFF-06. | DB, I |
| RNF-07 | Compatibilidad | Android 9+ con Chrome; iPhone con iOS 16.4+ y Safari; escritorio con Chrome y Edge actuales (panel) y Safari/Firefox (vista pública). Dispositivos reales: 1 Android de gama baja, 1 de gama media, 1 iPhone. Base instalada real: [SUP]. | M | REC | Matriz de dispositivos ejecutada en Fase 7. | M |
| RNF-08 | Respaldo y recuperación | Respaldos automáticos verificables y **restauración probada antes del piloto**. Objetivos propuestos: pérdida de datos máxima (RPO) 24 h y tiempo de recuperación (RTO) 8 h, a confirmar según el plan contratado. | M | CONF (Q-07) + REC (valores) | Restauración en un entorno de pruebas con medición de tiempos. | M |
| RNF-09 | Observabilidad | Errores del cliente y del servidor, disponibilidad externa y alerta al responsable técnico; sin datos personales (PRV-12); métricas OFF-20. | M | REC | Alerta de prueba recibida. | M |
| RNF-10 | Localización | Español de Costa Rica; zona `America/Costa_Rica` (UTC−6, sin horario de verano); fechas dd/mm/aaaa, 24 h; sin otros idiomas en el MVP. | M | DER (Q-01) | Revisión de textos y fechas. | E2E |
| RNF-11 | Mantenibilidad y pruebas | Toda regla de §8 tiene pruebas unitarias con fixtures; toda restricción de §7 tiene prueba contra PostgreSQL real; esquema y migraciones versionados; no se modifica producción a mano. | M | CONF (skill) | Trazabilidad requisito→prueba (§14). | U, DB |
| RNF-12 | Costos y planes | Priorizar planes gratuitos solo si son legal, técnica y operativamente adecuados; verificar con fuentes oficiales límites, restricciones comerciales, pausas por inactividad y respaldos **antes de contratar o publicar**; la organización asume los costos. | M | CONF (Q-07) | ADR de Fase 2 con fuentes oficiales y costo mensual confirmado por la organización. | D |
| RNF-13 | Usabilidad de captura | Registrar un gol en ≤ 3 toques desde la pantalla del partido; registrar un resultado final directo en < 30 s (meta del informe) medido en UAT con ≥ 3 editores. | M | REC (informe, sin línea base) | Cronometraje en UAT; la línea base se mide con el proceso actual (Q-08). | M |
| RNF-14 | Propiedad y acceso | Cuentas, dominio, base de datos y hosting a nombre de la organización; el desarrollador accede con credenciales propias revocables; inventario de cuentas y administradores. | M | CONF (Q-07) | Inventario firmado en Fase 8. | D |
| RNF-15 | Red lenta | Las vistas públicas son utilizables con perfil 3G lento; la aplicación indica carga y no bloquea acciones de captura. | M | REC (informe §10.4) | Prueba con red limitada. | E2E |
| RNF-16 | Entornos y despliegue | Entornos local, staging y producción; migraciones versionadas y reversibles según el caso; despliegues solo con autorización específica. | M | REC / CONF (skill) | Checklist de Fase 8. | D |
| RNF-17 | Continuidad | Manual de operación y sucesión de accesos; procedimiento si el sistema cae durante un partido (seguir capturando sin conexión y sincronizar después). | M | REC (RP-36) | Runbook revisado. | D |

---

## 12. Casos límite

| ID | Escenario | Comportamiento requerido | Requisitos | Prueba |
|---|---|---|---|---|
| CL-01 | Editor sin conexión captura goles; entretanto el organizador finaliza el partido con otro marcador | Las operaciones quedan `en conflicto` (caso 3); no se aplican; el editor ve el motivo y puede exportar; el organizador decide | OFF-09, RD-RES-06 | I, E2E |
| CL-02 | Editor desasignado mientras trabaja sin conexión | Al sincronizar: `rechazada` por falta de asignación; datos conservados y exportables | OFF-11 | I |
| CL-03 | Rol o cuenta revocados mientras hay operaciones pendientes | Rechazo en el servidor; cuenta desactivada → aviso, exportación y borrado local | RF-ACC-07, OFF-11 | I, E2E |
| CL-04 | Se pierde la respuesta del servidor y el cliente reenvía | La operación se aplica una sola vez; respuesta idéntica (N-13) | OFF-06 | I |
| CL-05 | Mismo editor en dos dispositivos con operaciones sobre el mismo partido | Fusión por identificador y secuencia; marcador/estado con versión vieja → conflicto | §9.4 caso 10 | I |
| CL-06 | Dos editores guardan el mismo partido (N-06) | El segundo recibe conflicto; el dato no se pisa | RD-RES-04 | I, DB |
| CL-07 | Reloj del dispositivo adelantado/atrasado | Orden por secuencia; aviso si la diferencia > 5 min | OFF-13 | I |
| CL-08 | Cerrar sesión con operaciones pendientes | Confirmación explícita; sin descarte silencioso | OFF-10, OFF-12 | E2E |
| CL-09 | Pérdida o robo del dispositivo con operaciones y datos locales | Revocar cuenta; el servidor rechaza; riesgo residual documentado | OFF-12 | M, D |
| CL-10 | Jugador dado de baja o anonimizado mientras el editor trabajaba sin conexión | `en conflicto`; elegir otro jugador (o «sin autor» si se aprueba) | RD-JUG-04, §9.4 caso 6 | I |
| CL-11 | Partido reprogramado o cancelado mientras hay operaciones locales | Reprogramado: se aceptan; cancelado: conflicto | RD-RES-09, Q-29 | I |
| CL-12 | El navegador limpia el almacenamiento local con operaciones pendientes | Pérdida posible; mitigación: almacenamiento persistente, indicador de pendientes, sincronizar al terminar; riesgo residual documentado | OFF-17 | M |
| CL-13 | Nueva versión de la app con cola pendiente | La cola se conserva y migra, o se bloquea la actualización | OFF-16 | E2E |
| CL-14 | Sincronización interrumpida a mitad de un lote | Cada operación en un solo estado coherente; se reanuda | OFF-08 | I |
| CL-15 | Corrección de una jornada anterior altera desempates y la tabla pública | Recalculo atómico; sin estado intermedio público; auditoría antes/después | RD-RES-06, RD-TAB-11 | I, E2E |
| CL-16 | Empate circular de tres o cuatro equipos (N-03) | Sin bucle; resuelve por criterio o queda «sin resolver» → resolución manual | RD-TAB-06…09 | U |
| CL-17 | Torneo con número impar de equipos; un equipo se retira en curso | Descansos correctos; retiro según Q-26; tabla recalculada | RD-CAL-01, RF-EQP-03 | U, I |
| CL-18 | W.O. sobre partido con eventos previos; doble W.O. | W.O. rechazado si hay eventos; doble W.O. según Q-21 | RD-WO-04, 06 | I |
| CL-19 | Organizador intenta invitar a un organizador o dar rol de dueño; modifica el alcance de una invitación | Denegado en servidor y BD; evento de seguridad | RF-ACC-03, 15, RT-03, 07 | DB, I |
| CL-20 | Editor se asigna a sí mismo un partido; delegado edita otro equipo; editor de otro torneo escribe | Denegado | RF-ACC-06, RD-JUG-06, RT-04 | DB |
| CL-21 | Manipulación de identificadores para acceder a otro torneo; partido de A con equipo de B | Denegado / rechazado por la base de datos | RT-08, RF-TOR-04 | DB |
| CL-22 | Un jugador en dos equipos del mismo torneo (N-09): por delegado, por organizador, alta simultánea, variante de escritura | Una sola alta gana; advertencia por posibles duplicados | RD-JUG-02 | DB, I |
| CL-23 | La misma persona participa en dos torneos distintos | Permitido; sin vínculo ni exposición cruzada de datos | RF-EQP-01, RD-JUG-01 | DB |
| CL-24 | Invitación reutilizada, vencida o reenviada a otro correo | Rechazo y registro | RF-ACC-02 | DB |
| CL-25 | Público intenta ver borrador, jornada no publicada o enumerar torneos | 404 indistinguible; sin campos internos | RF-PUB-01, 03, 04 | E2E, DB |
| CL-26 | Revocación de autorización después de publicar estadísticas del jugador | Vuelve a etiqueta; estadísticas intactas | PRV-02, 06 | DB, E2E |
| CL-27 | Regenerar calendario con editores asignados y jornadas publicadas | Solo si todo está `programado`; aviso de consecuencias; auditado | RD-CAL-07 | I |
| CL-28 | Revocar al último dueño; pérdida del segundo factor del dueño | Rechazo; procedimiento de recuperación | RF-ACC-09, RT-06 | DB, D |
| CL-29 | Marcador distinto de la suma de eventos; partido W.O. | Según Q-30; W.O. exento | RD-EVT-04, RD-WO-07 | U |
| CL-30 | Pico de lectura pública en el instante de confirmar un resultado | Ninguna vista sirve datos anteriores > 60 s tras la confirmación | RF-PUB-09 | L |
| CL-31 | Cambio de reglamento con el torneo iniciado | Solo el dueño (si se adopta); vista previa; auditado; recalculo | RD-REG-03 | I |
| CL-32 | Partido sin editor a la hora de inicio (N-12) | Alerta al organizador | RF-ADM-04 | I |
| CL-33 | Segunda amarilla o evento posterior a una roja | Sugerencia de roja; advertencia (no bloqueo) | RD-EVT-06 | E2E |
| CL-34 | Corrección en torneo `finalizado` | Solo el dueño, con motivo | RD-RES-06, RF-TOR-02 | I |

---

## 13. Decisiones abiertas y preguntas (Q-08…Q-16, Q-20…Q-25, residuales de Q-02 y nuevas Q-26…Q-35)

**Ninguna de estas decisiones se tomó en esta fase.** Cada fila trae opciones y una recomendación `[REC]` para facilitar su respuesta; el usuario puede aceptar, modificar o rechazar. Las marcadas **«Bloquea F2»** condicionan la arquitectura y deben decidirse antes de aprobar la Fase 1 o a más tardar al iniciar la Fase 2.

### 13.1 Residuales de Q-02 (control delegado)

| ID | Pregunta | Opciones | Recomendación [REC] | Si no se decide | Necesaria antes de |
|---|---|---|---|---|---|
| Q-02a | ¿Cómo queda autorizado el organizador que crea un torneo? | A) Queda automáticamente como organizador de **ese** torneo. B) Lo asigna el dueño después de crearlo. | **A.** No equivale a designar organizadores (Q-19); es atómico y auditado. | RF-ACC-12, HU-ORG-01 y su prueba quedan bloqueados | **Bloquea F2** |
| Q-02b | ¿Quién asigna editores a partidos? | A) Dueño y organizador del torneo. B) Solo el dueño. C) El editor «toma» partidos libres. | **A.** C contradice «partidos asignados» (Q-02). | RF-ACC-06 bloqueado | **Bloquea F2** |
| Q-02c | ¿Quién revoca accesos? | A) Dueño a cualquiera; organizador a editores y delegados de sus torneos. B) Solo el dueño. | **A**, con efecto inmediato y auditoría. | RF-ACC-07 bloqueado | **Bloquea F2** |

### 13.2 Preguntas abiertas de la Fase 0

| ID | Pregunta | Opciones | Recomendación [REC] | Si no se decide | Necesaria antes de |
|---|---|---|---|---|---|
| Q-08 | ¿2FA para editores? Además: ¿cómo registran hoy los resultados y quién lo hace? | A) Obligatorio. B) Opcional/recomendado, reevaluar tras el piloto. C) No. | **B**: obligatorio solo para dueño y organizador; mitigar con invitación de un solo uso, sesiones revocables y auditoría. Describir el proceso actual de captura (línea base de RNF-13). | RF-ACC-10 queda como «dueño/organizador»; editores sin definir | **Bloquea F2** |
| Q-09 | ¿Quién corrige o reabre partidos finalizados, en qué plazo, y cómo se manejan disputas? | A) Organizador del torneo o dueño, sin plazo, con motivo. B) A + plazo (p. ej. 7 días; luego solo dueño). C) Solo el dueño. | **A** (plazo opcional si la organización lo quiere); torneo finalizado → solo dueño; **sin flujo de disputas** en el MVP: una corrección con motivo. | RD-RES-06 bloqueado | **Bloquea F2** |
| Q-10 | ¿Canal y destinatarios de la alerta N-12? | A) Indicador en el panel del organizador. B) A + correo. C) WhatsApp/push. | **A** (+ B si ya hay correo transaccional). C es posterior (costo/complejidad; push fuera del MVP). Destinatarios: organizadores del torneo y dueño. | RF-ADM-04 queda en A | F3 / módulo 5D |
| Q-11 | ¿Qué histórico de temporadas se cargará? ¿Los equipos conservan identidad entre torneos? | A) Sin histórico. B) Captura manual de tablas finales. C) Importación con datos mínimos. | **A** en el MVP; el modelo no debe impedir que un equipo se repita entre torneos (RF-EQP-01). | RF-EQP-01 alcance indefinido | **Bloquea F2** |
| Q-12 | ¿Cómo identificar a un jugador sin recolectar documentos (para «un jugador, un equipo»)? | A) Sin identificador; aviso de duplicados por nombre normalizado. B) Identificador interno verificable (p. ej. cédula) solo para validación. C) Confirmación manual del organizador. | **A + C**: advertencia automática y confirmación del organizador; el límite (no previene fraude con nombres distintos) queda documentado. B recolecta un dato sensible y requiere revisión jurídica. | RD-JUG-02 solo con garantía por equipo, no por persona | **Bloquea F2** |
| Q-13 | ¿Qué se comparte, por qué canal y hay patrocinadores o cobros? | — | MVP: enlace estable, QR y botón nativo de compartir. **Pagos, boletería y patrocinadores fuera del MVP.** Se pregunta a la organización por canales (WhatsApp, Facebook, Instagram) y patrocinadores. | RF-PUB-05 en nivel S | F3 |
| Q-14 | ¿Logo en formato original, mockup de 13 pantallas y colores oficiales? | — | La organización los aporta o confirma. **No bloquea la Fase 1; bloquea la Fase 3.** | Fase 3 | F3 |
| Q-15 | ¿Una persona puede tener más de un rol en el mismo torneo? | A) Uno por persona y torneo. B) Varios, con RT-05 (conflicto de interés). C) Varios sin restricción. | **B** (los roles no se suman entre torneos). | RF-ACC-08 bloqueado | **Bloquea F2** |
| Q-16 | ¿Qué disponibilidad es aceptable y qué pasa si el sistema cae durante un partido? | A) Mejor esfuerzo en el piloto. B) 99,5 % mensual. | Definir **comportamientos** (la captura sigue sin conexión; la lectura pública usa caché) y fijar el objetivo numérico solo cuando se verifique el SLA del plan (Q-07). | RNF-03 sin cifra | **Bloquea F2** |

### 13.3 Privacidad (Q-20…Q-25)

| ID | Pregunta | Opciones | Recomendación [REC] | Si no se decide | Necesaria antes de |
|---|---|---|---|---|---|
| Q-20 | ¿Qué campos públicos de jugador y desde/hasta qué edad aplican las reglas de menores? | A) Nombre público + dorsal + equipo + estadísticas; menores con etiqueta salvo autorización (PRV-02). B) Nombre siempre público para adultos y menores con autorización. | **A/B combinados** según PRV-02; **nunca** foto, fecha de nacimiento ni documento. Corte de 18 años [PEND-JUR]. | PRV-02/04 no se aprueban | F3 y F4; jurídico antes del lanzamiento |
| Q-21 | W.O.: ¿qué ocurre si ambos equipos no se presentan, hay sanciones y cuenta para GF/GC? | A) Doble W.O. con 0 puntos y 0-0. B) Ambos derrotados con marcador configurable. | **A**; GF/GC cuentan según el marcador de adjudicación; **sin sanciones automáticas** (se muestra el conteo de W.O.). | RD-WO-02…04 bloqueados | F4 / módulo 5E |
| Q-22 | «Resolución manual»: ¿quién, con qué justificación y cómo se publica? | — | Organizador del torneo o dueño; motivo obligatorio; etiqueta pública; reversible; queda obsoleta si cambian los resultados implicados (RD-TAB-09). | RD-TAB-09 bloqueado | F4 / módulo 5F |
| Q-23 | Cambios de reglamento en torneo iniciado: ¿cuáles, quién aprueba y se recalcula el histórico? | — | RD-REG-03: solo dueño; criterios se recalculan; puntos y vueltas no cambian; W.O. solo hacia adelante. | RD-REG-03 bloqueado | F4 |
| Q-24 | ¿Quién recolecta el consentimiento, dónde queda la evidencia y quién atiende solicitudes? | A) Evidencia en poder de la organización; el sistema guarda metadatos. B) Archivo digital en el sistema. | **A** + designar una persona responsable; B añade almacenamiento y riesgo. Quién recolecta: delegado/organizador. | PRV-05, PRV-08 bloqueados | **Bloquea F2** |
| Q-25 | ¿Restricciones de ubicación de datos o transferencias fuera de Costa Rica? | — | Documentar región y proveedores; consultar a jurídico; minimizar datos personales almacenados. | PRV-13, ADR | **Bloquea F2** |

### 13.4 Nuevas preguntas surgidas al especificar

| ID | Pregunta | Opciones | Recomendación [REC] | Si no se decide | Necesaria antes de |
|---|---|---|---|---|---|
| Q-26 | Equipo que se retira o es expulsado en curso: ¿qué pasa con sus resultados y partidos restantes? | A) Anular todos sus resultados. B) Conservar los jugados y adjudicar W.O. a los restantes. C) Conservar los jugados y anular los restantes. D) Configurable por torneo. | **D**, definido antes de iniciar; defecto a confirmar con la organización. | RF-EQP-03 bloqueado | F4 / módulo 5B |
| Q-27 | «Enfrentamiento directo» con más de dos equipos y subgrupos: ¿qué se compara y se recalcula? | A) Mini-tabla (puntos, DG, GF) entre los empatados, recalculada para el subgrupo restante. B) Solo puntos entre ellos. | **A** (RD-TAB-06). Confirmar contra el reglamento real de la organización. | RD-TAB-06 con semántica provisional | F4 / módulo 5F |
| Q-28 | Catálogo de criterios de desempate configurables y fair play | — | Catálogo: DG, GF, GC, victorias, enfrentamiento directo; fair play fuera (exige ponderar tarjetas). | RD-TAB-05 con el catálogo recomendado | F3 / módulo 5F |
| Q-29 | Estados `aplazado`, `cancelado` y `suspendido`: ¿efecto en la tabla? | — | Aplazado = reprogramación; cancelado = no cuenta y exige motivo; suspendido conserva el marcador hasta decisión del organizador. | RD-RES-10, CL-11 incompletos | F4 |
| Q-30 | Consistencia marcador–eventos | A) Estricta. B) Advertencia y «goles sin autor». C) Independientes. | **B**: permite finalizar con aviso al organizador; los «sin autor» no cuentan en goleadores. | RD-EVT-02/04 con comportamiento provisional | F4 / módulo 5E |
| Q-31 | Plantillas: cierre, mínimos/máximos, categorías o edad | — | Fecha de cierre configurable; sin mínimos/máximos ni elegibilidad por edad en el MVP. | RD-JUG-05 bloqueado | F4 / módulo 5B |
| Q-32 | ¿Los resultados se publican automáticamente al confirmarse o requieren validación? | A) Automática. B) Validación del organizador. | **A**, con corrección auditada (RD-RES-06). | RD-RES-07; diseño de caché | **Bloquea F2** |
| Q-33 | ¿Puntos 3/1/0 fijos o configurables? | A) Fijos (leídos del reglamento). B) Configurables antes de iniciar. | **A** en el MVP. | RD-REG-06 | F4 |
| Q-34 | Parámetros del modo sin conexión (ventana, retención, finalizar sin conexión, cuenta desactivada) | Ver §9.5 | Valores de §9.5 y política de §9.4. | OFF-02, 10, 11, 21 bloqueados | **Bloquea F2** |
| Q-35 | Continuidad del dueño: ¿un dueño único o un segundo dueño? ¿Quién otorga el rol de dueño? | A) Dueño único con procedimiento de recuperación. B) Un segundo dueño designado por el dueño. | **B**; solo un dueño existente puede otorgar el rol (jamás un organizador). | RF-ACC-09 bloqueado | **Bloquea F2** |

**Resumen:** 28 decisiones; **14 bloquean la Fase 2** (Q-02a, Q-02b, Q-02c, Q-08, Q-09, Q-11, Q-12, Q-15, Q-16, Q-24, Q-25, Q-32, Q-34, Q-35). Para agilizar, puede responder por identificador, por ejemplo «acepto las recomendaciones salvo Q-09 (opción B) y Q-15 (opción A)». Mientras no se decida, la fila queda `COND` y el requisito no se considera aprobado.

---

## 14. Matriz de trazabilidad

### 14.1 Decisiones confirmadas → requisitos → riesgos → pruebas

| Decisión | Requisitos | Riesgos | Pruebas previstas | Se diseña / construye |
|---|---|---|---|---|
| Q-01 Costa Rica / responsable | PRV-01, 07, 13, 15, 16; RNF-10 | RK-05, RK-08 | D (revisión jurídica), E2E (aviso) | F2 ADR; F8 |
| Q-02 Roles y permisos | RF-ACC-01…16; RT-01…10; §7.1; RD-JUG-06 | RK-03, RK-04 | DB (matriz de 10 actores), I (abuso) | F2, F4; 5C |
| Q-03 Reglamento | RD-REG, RD-TAB, RD-WO, RD-CAL, RD-FOR | RK-07, RK-09 | U (N-01, N-02, N-03 reformuladas), DB | F4; 5D, 5E, 5F |
| Q-04 / Q-18 MVP liga ampliado | §3; RD-EVT; RF-PUB; RF-ADM | RK-01, RK-10 | E2E, U | 5B–5G |
| Q-05 Menores | PRV-02…06, 09, 11; RD-JUG-01 | RK-05, RK-06 | DB, E2E, D | F3, F4; 5B |
| Q-06 Dimensiones y offline | OFF-01…22; RNF-04; RD-FOR-03 | RK-02, RK-11 | I (red cortada), E2E (iOS real), L | F2, F4; 5E |
| Q-07 Propiedad e infraestructura | RNF-08, 12, 14, 16, 17 | RK-08, RK-12 | D | F2, F8 |
| Q-17 Offline | OFF-01…22; RF-PUB-06; RF-PWA-02 | RK-02, RK-11 | I, E2E | F2; 5E, 5G |
| Q-19 Invitaciones | RF-ACC-02…05, 11, 15; RT-03, 07, 10; §7.1 filas 1–6 | RK-03 | DB, I | F4; 5C |

### 14.2 Casos del informe N-01…N-13

| Caso | Requisito(s) | Alcance en el MVP | Prueba |
|---|---|---|---|
| N-01 Calendario 7 y 8 equipos, 1 y 2 vueltas | RD-CAL-01 | Sí | U |
| N-02 Empate a puntos entre dos equipos | RD-TAB-04, 06, 07 (a) | Sí, **reformulado** al orden de Q-03 | U |
| N-03 Empate circular de 3 equipos | RD-TAB-06, 07 (b)–(e), 08, 09 | Sí, reformulado | U |
| N-04 Eliminatoria con 6 equipos (byes) | — | **No** (Q-18); Fase 6 | — |
| N-05 Empate en eliminatoria exige penales | — | **No** (Q-18); Fase 6 | — |
| N-06 Dos editores guardan el mismo partido | RD-RES-04; OFF-09 | Sí | I, DB |
| N-07 Corregir un resultado de jornada anterior | RD-RES-06; RD-TAB-11 | Sí | I |
| N-08 Corregir semifinal con final jugada | — | **No** (sin eliminatorias); la regla de bloqueo con rondas posteriores se retoma en Fase 6 | — |
| N-09 Jugador en dos equipos del mismo torneo | RD-JUG-02 | Sí | DB |
| N-10 Editor intenta modificar un torneo no asignado | §7.1 fila 24, RT-08, CL-20, CL-21 | Sí (ampliado a partido no asignado) | DB |
| N-11 Aficionado sin sesión intenta escribir | §7.3 actor 1 | Sí | DB |
| N-12 Partido sin editor a la hora de inicio | RF-ADM-04 | Sí (canal COND Q-10) | I |
| N-13 Pérdida de conexión al guardar | OFF-03, 06, 08 | Sí; es requisito central | I, E2E |

### 14.3 Hallazgos H-01…H-13 y contradicciones C-01…C-15 (Fase 0)

| Hallazgo / contradicción | Requisito(s) que lo cubre | Fase donde se verifica |
|---|---|---|
| H-01 RLS solo en una tabla | RT-02, §7.3, RNF-05 | F4 (pgTAP), 5C |
| H-02 `report_result` ignora asignación | RF-ACC-06; §7.1 filas 24–25; RD-RES-01 | F4, 5E |
| H-03 Lectura de editores/delegados y columnas internas | RF-PUB-03; §7.1 filas 31–32; PRV-03, 09 | F4 |
| H-04 Vista sin `security_invoker` | RF-PUB-03 (revisión de vistas públicas) | F4 |
| H-05 Integridad entre torneos | RT-08; RD-EVT-03; RD-JUG-02 | F4 |
| H-06 Restricción de partidos con equipos nulos | No aplica al MVP de liga (todos los partidos tienen equipos definidos); se retoma con eliminatorias | F6 |
| H-07 Auditoría incompleta | RF-AUD-01…05; RF-ACC-11 | F4, 5C |
| H-08 Sin idempotencia ni transiciones | OFF-06, 07, 08; RD-RES-01, 04 | F2, F4, 5E |
| H-09 Identidad de jugador | RD-JUG-02; Q-12 | F2 |
| H-10 Dos fuentes de verdad | RD-TAB-10; RD-REG-06 | F2 |
| H-11 `has_role`/`EXECUTE` | RT-09 | F4 |
| H-12 Publicación sin modelo | RD-CAL-08; §9.6; RF-PUB-04 | F2 |
| H-13 Suscripciones push | Fuera del MVP (RF-PWA-04) | F6 |
| C-01 / C-02 | Resueltas por Q-19 y Q-02; residual Q-02a | — |
| C-03 / C-04 | Resueltas por Q-18 (§3) | — |
| C-05 / C-06 | = H-02 / H-01 | — |
| C-07 / C-08 | Estimación (§15); RNF-12 | F2 |
| C-10 Inventario de 13 pantallas | Se actualiza al MVP ampliado en la Fase 3 | F3 |
| C-11 2FA vs correo y contraseña | RF-ACC-10 | F2 |
| C-12 / C-13 | PRV-02, 03; vistas públicas en F4 | F4 |
| C-14 Regenerar calendario | RD-CAL-07 | 5D |
| C-15 Disponibilidad sin SLA | RNF-03; Q-16 | F2 |

### 14.4 Requisitos provisionales de la Fase 0 (`RF-01…RF-18`, `00-descubrimiento.md` §2.1) → identificadores definitivos

| Provisional | Definitivo |
|---|---|
| RF-01 Tabla | RD-TAB-01…12 |
| RF-02 Calendario y resultados | RD-CAL-01…09, RF-PUB-02 |
| RF-03 Crear torneo desde plantilla | RF-TOR-01 (solo liga); copa y relámpago: RF-TOR-07 (P) |
| RF-04 Generar calendario | RD-CAL-01…03 |
| RF-05 Resultado en < 30 s | RD-RES-01…05, RNF-13 |
| RF-06 Invitar y asignar roles | RF-ACC-02…07 (actualizado por Q-19) |
| RF-07 Historial | RF-AUD-01…05 |
| RF-08 Llaves | P (Fase 6) |
| RF-09 Partido en vivo | RD-RES-08 (mínimo); en vivo real: P |
| RF-10 Avisos y compartir | Compartir: RF-PUB-05; push: P |
| RF-11 Delegado y plantilla | RD-JUG-06, RF-ACC-13 |
| RF-12 Goleadores y tarjetas | RD-EVT-07, 08 (perfil de jugador: P) |
| RF-13 Patrocinadores | P |
| RF-14 Corregir/reabrir | RD-RES-06 |
| RF-15 Alerta sin editor | RF-ADM-04 |
| RF-16 Captura sin conexión | OFF-01…22 |
| RF-17 Compartir como imagen | P |
| RF-18 Instalar la app | RF-PWA-01 |

### 14.5 Requisitos previstos de la Fase 0 (`RP-01…RP-36`) → definitivos

| RP | Definitivo | RP | Definitivo |
|---|---|---|---|
| RP-01 | PRV-01, 15 | RP-19 | RF-PUB, RF-ADM |
| RP-02 | PRV-07, 16 | RP-20 | §3 (P) |
| RP-03 | PRV-08 | RP-21 | §15 |
| RP-04 | PRV-13, Q-25 | RP-22 | PRV-02…04 |
| RP-05 | §7 | RP-23 | PRV-05 |
| RP-06 | RF-ACC-02…05, 15; RT-03, 07 | RP-24 | PRV-06 |
| RP-07 | RF-ACC-12 | RP-25 | PRV-09, 11 |
| RP-08 | RF-ACC-06; RD-RES; §7.1 filas 24–25 | RP-26 | PRV-15 |
| RP-09 | RD-JUG-06 | RP-27 | RNF-04 |
| RP-10 | RT-08; RF-TOR-04 | RP-28 | OFF-01…22 |
| RP-11 | RD-TAB-02; RD-CAL-01 | RP-29 | OFF-06 |
| RP-12 | RD-TAB-04…07; RD-REG | RP-30 | OFF-09; §9.4 |
| RP-13 | RD-WO-01…07 | RP-31 | OFF-04 |
| RP-14 | RD-FOR-02 (P) | RP-32 | Pruebas de §14.1 (se ejecutan en F7) |
| RP-15 | P (Fase 6) | RP-33 | RNF-14 |
| RP-16 | RD-REG-03 | RP-34 | RNF-12 |
| RP-17 | RD-EVT-01…11 | RP-35 | RNF-12 |
| RP-18 | RF-ACC; RF-ADM-05 | RP-36 | RF-ACC-09; RNF-17 |

---

## 15. Estimación de esfuerzo actualizada (MVP ampliado con funcionamiento sin conexión)

**Naturaleza de la cifra.** Es una estimación **inicial** de ingeniería en días-persona, elaborada por este análisis sin datos históricos de velocidad del equipo; confianza **baja a media**. No es un compromiso de plazo ni de costo. Debe recalibrarse tras el módulo 5A y cuando se resuelvan las decisiones bloqueantes. Se obtuvo por analogía y descomposición por fases y módulos de la skill.

**Supuestos:** una persona desarrolladora (Q-01); dedicación completa = 4,5 días productivos por semana; media jornada = 2,5; el esfuerzo incluye las pruebas de cada módulo, pero no el tiempo de espera de decisiones, de la **revisión jurídica**, de verificación de proveedores ni de la organización para pruebas de aceptación; el piloto exige además que se juegue un torneo real, cuya duración depende del calendario deportivo.

| Fase / módulo | Mínimo | Probable | Máximo | Notas |
|---|---|---|---|---|
| Fase 2 · Arquitectura y amenazas | 5 | 7 | 8 | ADR de stack, offline-first, modelo de amenazas |
| Fase 3 · UX/UI | 10 | 12 | 15 | Inventario ampliado al MVP; wireframes, sistema visual, estados offline |
| Fase 4 · Diseño técnico | 8 | 10 | 12 | Esquema, matriz RLS, contratos, plan pgTAP |
| 5A · Infraestructura local y autenticación | 6 | 7 | 9 | Incluye 2FA y entornos |
| 5B · Torneos, equipos, inscripciones, jugadores | 8 | 10 | 12 | Incluye indicadores de privacidad |
| 5C · Roles, RLS, invitaciones, revocación, auditoría | 10 | 12 | 15 | Núcleo de seguridad |
| 5D · Calendario de liga, jornadas, asignación de editores | 8 | 10 | 12 | |
| 5E · Resultados, eventos, correcciones, W.O. (en línea) | 8 | 10 | 12 | |
| 5E · **Modo sin conexión** (cola, idempotencia, conflictos, revocación, iOS) | 12 | 16 | 20 | Mayor incertidumbre |
| 5F · Tabla, goleadores, tarjetas y vista pública con caché | 8 | 10 | 12 | |
| 5G · PWA, red débil, E2E y piloto | 8 | 10 | 12 | |
| Fase 7 · QA residual y endurecimiento | 12 | 15 | 18 | pgTAP completo, iOS real, carga, restauración |
| Fase 8 · Entrega y operación | 4 | 5 | 6 | |
| **Total** | **107** | **134** | **163** | días-persona |

| Dedicación | Mínimo | Probable | Máximo |
|---|---|---|---|
| Completa (4,5 d/semana) | ≈ 24 semanas | ≈ 30 semanas | ≈ 36 semanas |
| Media jornada (2,5 d/semana) | ≈ 43 semanas | ≈ 54 semanas | ≈ 65 semanas |

**Comparación con el informe.** El informe estimaba 5–7 semanas para su MVP (liga sin goleadores, tarjetas, W.O. configurable, desempates configurables ni modo sin conexión) con 2–3 desarrolladores (≈ 50–105 días-persona a 5 d/semana); el alcance confirmado es mayor y la estimación anterior **ya no es aplicable**. Con una sola persona, además, no hay paralelismo.

**Sensibilidad (informativa; no propone cambiar decisiones confirmadas).**

| Variación | Efecto aproximado |
|---|---|
| Sustituir el modo sin conexión completo por captura en línea con reintento | −8 a −14 días (contradice Q-17; solo referencia) |
| 2FA obligatorio para editores | +2 a +3 días |
| Segundo dueño y recuperación (Q-35 = B) | +1 a +2 días |
| Importación de histórico (Q-11 = C) | +5 a +8 días |
| Archivo digital de consentimientos (Q-24 = B) | +4 a +6 días |
| Fotos y escudos | +4 a +6 días |
| Resultado de la verificación de proveedores/planes gratuitos desfavorable | Puede añadir días de migración/diseño en F2; no estimable ahora |

**Puertas externas que pueden alargar el calendario:** revisión jurídica (PRV-15), formalización de responsabilidades (PRV-16), verificación oficial de costos y respaldos (RNF-12), logotipo y colores (Q-14), dispositivos reales para pruebas, y disponibilidad de la organización para el piloto.

---

## 16. Riesgos principales de la especificación

| ID | Riesgo | Probabilidad / impacto | Mitigación prevista | Seguimiento |
|---|---|---|---|---|
| RK-01 | Crecimiento de alcance (el MVP ya es amplio) | Alta / Alto | Prioridades M/S/P; cualquier recorte o adición pasa por aprobación; S se recorta primero | F1 aprobación; cada módulo |
| RK-02 | Complejidad del modo sin conexión (conflictos, idempotencia, migraciones de cola) | Alta / Alto | Política §9.4; ADR en F2; pruebas de integración con red cortada; prototipo temprano en 5E | F2, 5E |
| RK-03 | Fallo de autorización o fuga entre torneos | Media / Muy alto | Matriz de 10 actores en BD real; RT-08; revisión de seguridad; sin superusuario en pruebas | F4, 5C, F7 |
| RK-04 | Reglas ambiguas de asignación, revocación o varios roles | Media / Alto | Decisiones Q-02a/b/c, Q-15 antes de F2 | F1 |
| RK-05 | Incumplimiento de privacidad de menores | Media / Muy alto | PRV-02, 03, 06; revisión jurídica obligatoria; sin datos reales antes | PRV-15 |
| RK-06 | Pérdida de la evidencia de consentimiento o de su validez | Media / Alto | Responsable designado; metadatos; revisión jurídica | Q-24 |
| RK-07 | Reglas deportivas distintas del reglamento real (desempates, W.O., retiro) | Media / Alto | Contrastar RD-TAB-06, RD-WO, Q-26/27 con el reglamento escrito de la organización; fixtures aprobados | F4 |
| RK-08 | Planes gratuitos inadecuados (restricciones comerciales, pausas, respaldos, ubicación) | Media / Alto | Verificación oficial en F2; ADR con alternativa | F2 |
| RK-09 | Dos fuentes de verdad para la tabla | Media / Medio | RD-TAB-10 | F2 |
| RK-10 | Expectativa del tamaño del MVP frente a una sola persona (24–36 semanas a tiempo completo) | Alta / Medio | Comunicar la estimación; recalibrar tras 5A | F1 |
| RK-11 | Comportamiento de iOS: sin sincronización en segundo plano y posible limpieza de almacenamiento | Alta / Medio | OFF-05, OFF-17; pruebas en iPhone real | F2, 5G |
| RK-12 | Persona única mantiene el sistema (continuidad) | Media / Alto | RF-ACC-09, RNF-14, RNF-17 | F8 |
| RK-13 | Meta de captura (< 30 s) sin línea base | Media / Medio | Medir proceso actual (Q-08) y UAT | F3, F7 |
| RK-14 | Equilibrio de localías del método del círculo | Baja / Bajo | RD-CAL-09; ajustes manuales | 5D |

---

## 17. Criterios para aprobar la Fase 1 y qué se pide al usuario

### 17.1 Lista de verificación de revisión

1. ¿Los requisitos `CONF` y `DER` reflejan fielmente sus decisiones Q-01…Q-07, Q-17, Q-18 y Q-19? (§2, §14.1)
2. ¿Acepta la matriz de permisos v2 (§7), en particular las filas `COND`?
3. ¿Aprueba, ajusta o rechaza cada recomendación `[REC]` y decide las **14 decisiones que bloquean la Fase 2** (§13)? Los requisitos `COND` no se consideran aprobados hasta que decida.
4. ¿Acepta la separación MVP / posterior (§3) y las prioridades M/S/P?
5. ¿Acepta las reglas deportivas de §8, especialmente el algoritmo de desempate (RD-TAB-06), el efecto del W.O. y las transiciones de estado?
6. ¿Acepta el diseño de captura sin conexión (§9) y los parámetros propuestos (§9.5)?
7. ¿Acepta el tratamiento de privacidad (§10) como borrador sujeto a revisión jurídica?
8. ¿Acepta la estimación (§15) como referencia inicial?

### 17.2 Criterios de aceptación de la fase (autoevaluación; la aprobación es suya)

| Criterio | Evaluación de este análisis |
|---|---|
| Requisitos funcionales y no funcionales con ID, prioridad y aceptación medible | Cumplido en forma de borrador; los `COND` indican dónde falta decisión |
| Historias para los cinco perfiles | Cumplido (42 historias, §5) |
| Matriz de permisos con torneo, partidos asignados, invitaciones y revocación | Cumplido (34 acciones, 10 restricciones, §7); filas `COND` pendientes |
| Reglas deportivas del MVP | Cumplido (§8); semántica de enfrentamiento directo y W.O. doble pendientes de confirmación |
| Offline: sincronización, duplicados, conflictos, revocación, publicación | Cumplido (§9); parámetros pendientes (Q-34) |
| Privacidad y menores con puntos [PEND-JUR] | Cumplido como borrador (§10) |
| MVP vs posterior | Cumplido (§3) |
| Preguntas abiertas resueltas o documentadas sin decidir por el usuario | Documentadas; **ninguna decidida** (§13) |
| Trazabilidad decisiones–requisitos–riesgos–pruebas | Cumplido (§14, §16) |
| Estimación actualizada | Cumplido (§15), con confianza baja a media |
| Coherencia con Q-01…Q-07 y Q-17…Q-19 | Verificada (§2); ninguna contradicción detectada. Observación: Q-03 «cambios de reglamento controlados» se concreta en RD-REG-03 como `COND:Q-23` |

---

## 18. Lo que esta fase no hizo ni garantiza

- No se escribió código, esquema, migraciones ni se instalaron dependencias; no se modificó la skill; no se inició la Fase 2; no hubo commit, push, merge ni despliegue.
- Ninguna prueba se ejecutó: todas las pruebas son **previstas**. Ningún requisito está verificado por ejecución.
- No se verificó ninguna fuente externa: precios, límites y condiciones de proveedores, compatibilidad PWA en iOS, ni normativa de Costa Rica. Las referencias legales son orientativas y [PEND-JUR].
- No se inspeccionaron el mockup de 13 pantallas (no está adjunto) ni imágenes del informe.
- Las cifras propuestas (plazos de invitación, retención, tiempos de actualización, topes, estimación) son propuestas `REC` o `SUP`, no hechos medidos.
- El algoritmo de desempate y el método del círculo se especifican por propiedades y casos; su implementación y prueba corresponden a las fases técnicas.
