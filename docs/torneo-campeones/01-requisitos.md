# 01 · Requisitos y MVP ampliado — Torneo de Campeones

- **Fase:** 1 (Requisitos funcionales y definición del MVP ampliado)
- **Estado:** **APROBADA CON CONDICIONES** (v1.3.1, 2026-11-15; ver §17.6 y `aprobaciones.md`). La aprobación es del cierre **documental** y **no** autoriza la Fase 2 ni operaciones de Git; los requisitos `COND` (23), las 18 preguntas abiertas y las 14 ambigüedades abiertas siguen **sin aprobar** en sus alternativas. Antecedentes: el usuario confirmó las 14 decisiones bloqueantes (§2.1), Q-36 y Q-37 (§2.1b), las consecuencias D-1…D-12 (§2.2) y, en la v1.3, Q-38, Q-39, Q-44, Q-45 y Q-46, el riesgo RK-15 como aceptado y el diferimiento de RF-EQP-06 (§2.1c); esas confirmaciones, por sí solas, no aprobaban la Fase 1 (la aprobación posterior consta en §17.6) ni autorizan la Fase 2. Los requisitos que dependen de decisiones aún abiertas siguen `COND` (§13).
- **Versión:** v1.3.1 (corrección documental acotada sobre la v1.3, a partir de la auditoría independiente final; la v1.3 consolidó Q-38, Q-39, Q-44, Q-45, Q-46, RK-15 aceptado y RF-EQP-06 diferido sobre la v1.2, 2026-11-15) · v1.0 fue el borrador versionado en el commit `2f1658d`; las v1.1, v1.2, v1.3 y v1.3.1 y el registro de aprobación se consolidaron en el commit `efa9c0bc8271e1f2612f26a6ef3c436f624f4b5a` (push a `claude/dreamy-pasteur-rid0gm` completado; PR y merge a `main` pendientes de autorización) · **Fecha:** 2026-11-15
- **Autorización de la fase:** mensaje «AUTORIZACIÓN FORMAL DE FASE 1» del 2026-11-15 (alcance: solo la Fase 1; sin código, esquema, migraciones, dependencias, cambios a la skill, Fase 2, commit, push, merge ni despliegues).
- **Autorización de la consolidación:** mensaje «CONSOLIDACIÓN DEFINITIVA DE LAS 14 DECISIONES BLOQUEANTES DE FASE 1» del 2026-11-15: exclusivamente documental; no aprueba la Fase 1 ni autoriza la Fase 2; sin commit, push, merge ni despliegues.
- **Segunda consolidación:** mensaje «CONSOLIDACIÓN DE Q-36, Q-37 Y D-1 A D-12» del 2026-11-15: exclusivamente documental sobre los tres documentos; la Fase 1 permanece pendiente de aprobación y la Fase 2 no está autorizada; sin commit, push, pull request, merge ni despliegues.
- **Tercera consolidación (ejecución del plan v1.3):** mensaje «EJECUCIÓN CONTROLADA DE CONSOLIDACIÓN DOCUMENTAL v1.3» del 2026-11-15: autoriza exclusivamente la actualización documental de los tres archivos conforme al Plan Definitivo y Verificable; la Fase 1 permanece pendiente de aprobación y la Fase 2 no está autorizada; sin commit, push, pull request, merge ni despliegues.
- **Corrección documental final v1.3.1:** mensaje «CORRECCIÓN DOCUMENTAL FINAL v1.3.1» del 2026-11-15: autoriza exclusivamente una corrección documental acotada de los tres archivos a partir de los hallazgos H-01…H-11 de la auditoría independiente final (§17.5); no cambia decisiones, permisos, requisitos ni la aprobación; la Fase 1 permanece pendiente de aprobación y la Fase 2 no está autorizada; sin commit, push, pull request, merge ni despliegues.
- **Base vinculante:** `00-descubrimiento.md` v1.3 (Fase 0 aprobada): decisiones Q-01…Q-07, Q-17, Q-18, Q-19, marcadas aquí como **[CONF]**.
- **Fuentes revisadas en esta fase:** `00-descubrimiento.md`, `aprobaciones.md`, `bitacora.md`; `references/puertas-de-aprobacion.md`, `qa.md`, `seguridad.md`, `puntos-a-validar.md`, `mapa-informe.md`; plantilla `brief-proyecto.md`; del informe PDF se releyeron pp. 13–15 (§6.5, §6.6, §7.2, §7.3) y pp. 22–23 (§10, casos N-01…N-13). `arquitectura.md` solo se consultó en la auditoría final, sin extraer requisitos de ella. **No se leyeron** `ui-ux.md` ni `direccion-visual-profesional.md` (corresponden a la Fase 3). **No se verificó** ninguna fuente externa ni legislación.

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
- **Estado:** `CONF` (proviene directamente de una decisión confirmada; las confirmadas en la consolidación de la v1.1 se citan como `CONF (Q-xx)`) · `DER` (derivado) · `REC` (propuesta nueva por aprobar) · `COND:Q-xx` (**condicionado** a una decisión pendiente; el requisito se redacta con la recomendación y **no puede aprobarse** hasta que se decida).
- **Prueba:** `U` unitaria · `DB` pgTAP/PostgreSQL real · `I` integración · `E2E` Playwright · `A` accesibilidad (axe + manual) · `M` manual/UAT · `L` carga · `D` revisión documental. Las pruebas se **prevén** aquí; ninguna se ha ejecutado.
- **Regla de calidad aplicada:** un requisito sin criterio de aceptación medible, ambiguo o contradictorio queda como `COND` o `[PEND]`, nunca como listo.

### 0.3 Convención de identificadores

`HU-` historias · `RF-ACC/TOR/EQP/PUB/ADM/AUD/PWA` funcionales generales · `RD-` reglas deportivas · `OFF-` captura sin conexión · `PRV-` privacidad · `RNF-` no funcionales · `RT-` restricciones transversales de permisos · `CL-` casos límite · `Q-` preguntas/decisiones · `RK-` riesgos. Los `RF-xx` y `RP-xx` provisionales de la Fase 0 se reasignan en §14.4 y §14.5.

### 0.4 Historial de versiones y sustituciones (v1.0 → v1.1 → v1.2 → v1.3 → v1.3.1)

Las v1.1, v1.2 y v1.3 **actualizan los requisitos afectados en su lugar** (los identificadores se mantienen) y dejan aquí constancia de lo que sustituyeron. La v1.3.1 es solo una corrección documental (§17.5).

| Elemento | v1.0 | v1.1 | Motivo |
|---|---|---|---|
| RF-ACC-06, 07, 08, 12 | `COND` (Q-02b, Q-02c, Q-15, Q-02a) | `CONF`; RF-ACC-06 añade desasignación y auditoría; RF-ACC-07 separa la revocación inmediata en servidor del plazo de sesiones | Decisiones confirmadas |
| RF-ACC-09 | Prioridad S, `COND:Q-35` | Prioridad **M**, `CONF` (segundo dueño, autenticación reforzada, último dueño activo) | Q-35 |
| RF-ACC-10 | Editores `COND:Q-08` | `CONF` (2FA obligatorio para dueño y organizadores; opcional y recomendado para editores); delegados pasan a Q-39 | Q-08 |
| RF-ACC-02, 03, 15 | — | Precisiones: autoinvitación rechazada; organizador designado puede crear torneos; la acumulación de roles no amplía permisos | Coherencia con Q-02a, Q-15 |
| RF-EQP-01 | `COND:Q-11` | `CONF`: identidad de equipo entre torneos; plantillas y estadísticas independientes; gobierno de la identidad → Q-37 | Q-11 |
| RD-JUG-02 | «Aviso de posibles duplicados y confirmación del organizador», sin definir qué ve el delegado | **Sustituido** por flujo `pendiente de verificación`: rechazo de la segunda inscripción si es la misma persona; el delegado no ve datos de otros equipos. Nuevos RD-JUG-08, 09, 10 | Q-12; corrige una fuga entre equipos |
| RD-RES-06, 07 | `COND:Q-09`, `COND:Q-32` | `CONF`; «cerrado definitivamente» queda como DER (Q-40) | Q-09, Q-32 |
| OFF-02, 10, 11, 17, 20, 21, 22; nuevo OFF-23 | Parámetros y política propuestos; OFF-17 prioridad S | Parámetros confirmados; OFF-17 pasa a **M**; OFF-10 concilia 30 días con «no descartar en silencio»; OFF-11 limita la exportación a un resumen deportivo mínimo; OFF-20 registra metadatos en el servidor; OFF-22 revalida al sincronizar; OFF-23 cubre la sesión vencida | Q-34 y reglas adicionales |
| §9.4 y §9.5 | Propuestas | **Confirmadas íntegras** (filas conservadas); §9.5 añade la columna con la decisión | Q-34 |
| PRV-05, 08, 13 | `COND:Q-24`, `COND:Q-24`, `PEND` | `CONF` (metadatos únicamente; persona responsable por designar; infraestructura internacional con evaluación previa) | Q-24, Q-25 |
| PRV-15 | «Ningún dato real de **menores**» | Ampliado a **todo dato personal real** (jugadores, menores y usuarios del sistema); nuevo PRV-17 (datos sintéticos) | Q-24, Q-25 |
| RNF-03 | `COND:Q-16`; meta de 99,5 % del informe | `CONF`: mejor esfuerzo en el piloto, sin porcentaje contractual; reevaluar con garantías oficiales | Q-16 |
| RT-01, 04, 05, 06; nuevo RT-11 | RT-05 `COND:Q-15` | RT-05 y RT-06 `CONF`; RT-01 por usuario-rol-torneo-equipo-partido; RT-11 acumulación de roles | Q-15, Q-35 |
| Matriz §7.1 | 34 acciones | Filas 4–7, 10, 20, 21, 24, 25, 27 y 34 actualizadas; **fila 35 nueva**; nuevo §7.4 (roles combinados) | Decisiones confirmadas |
| Historias | Estados `COND` en HU-DUE-06, HU-ORG-01, 07, 09, HU-DEL-02 | Actualizadas; **HU-DEL-04 sustituida**; nuevas HU-ORG-14, HU-EDI-10, HU-EDI-11 | Q-12, Q-34, Q-24 |
| §3, §9.6 | «Histórico: No (recomendación)»; «archivado: salvo decisión de histórico» | Histórico `CONF` (Q-11); disputas `CONF` (Q-09); documentos de consentimiento fuera del MVP `CONF` (Q-24) | Decisiones confirmadas |
| §13 | 28 decisiones, 14 bloqueantes | Reestructurada: 14 confirmadas, 14 abiertas de la v1.0, 7 nuevas (Q-36…Q-42); **bloquean la Fase 2: Q-36 y Q-37** | Consolidación |
| §14, §15, §16, §17 | v1.0 | Trazabilidad ampliada (§14.6), estimación y riesgos actualizados, dictamen técnico | Consolidación |

**Sustituciones de la v1.2 (v1.1 → v1.2):**

| Elemento | v1.1 | v1.2 | Motivo |
|---|---|---|---|
| RF-ACC-06; §9.4 filas 10–11 | Un editor por partido `COND:Q-36`; fila 11 «dos editores asignados (solo si se permite)» | `CONF`: **un único editor vigente**; reasignación atómica sin ventana de doble asignación; la fila 11 queda **tachada y no aplica**, sustituida por el escenario de reasignación | Q-36 A |
| RF-EQP-01; nuevo RF-EQP-06 | Identidad de equipo `COND:Q-37` | `CONF`: inscripciones independientes; solo el dueño vincula; el vínculo no comparte ni concede nada. RF-EQP-06 (S) | Q-37 B |
| §2.2 | «Consecuencias derivadas por confirmar» (D-1…D-12) | **Confirmadas**, con precisiones, y la columna «texto v1.1» conservada como historial | D-1…D-12 |
| D-5 / RD-RES-06 / RF-TOR-02 / Q-40 | «Cerrado definitivamente» = `finalizado` **o** `archivado` | **Sustituida:** `finalizado` = terminó la competición (dueño y organizador corrigen); `archivado` = cierre definitivo (solo el dueño corrige). Q-40 se reformula | D-5 |
| RD-JUG-02 | Pendiente de verificación solo si coincide con otro equipo; coincidencia dentro del mismo equipo = advertencia | **Toda** inscripción con posible duplicado queda pendiente y no habilitada hasta resolver; resolución auditada | D-6 |
| OFF-10, OFF-11 | Retención con avisos; resumen mínimo con eventos, minutos, dorsal y equipo | OFF-10: plazo informado al detectar, avisos «cuando sea técnicamente posible», constancia mínima, sin garantía si el navegador borra el almacenamiento. OFF-11 remite a **OFF-24 (nuevo)**: el resumen contiene solo identificador de partido, abreviaturas y marcador; los eventos solo con divulgación autorizada (Q-43) | D-7, D-8 |
| OFF-20 | Metadatos de operaciones aplicadas, en conflicto y rechazadas | Metadatos mínimos de las **rechazadas o en conflicto que efectivamente recibe**; solo del torneo del organizador; no autoriza publicar | D-9 |
| OFF-22; nuevo RT-13 | Reevaluación al sincronizar | Se rechaza toda operación con autorización revocada aunque se capturara legítimamente; sin recuperación ni publicación automática; evaluación atómica | D-11 |
| RT-04, RT-06; nuevos RT-12, RT-13 | RT-04 `DER`; RT-06 | RT-04 `CONF` (D-3); RT-06 con protección **atómica** del último dueño y sin jerarquía entre dueños; RT-12 (el vínculo no concede acceso) | D-3, D-4, Q-37 |
| RF-ACC-02, 03, 07, 09, 12 | `DER` o v1.1 | `CONF` con las precisiones de D-1, D-2, D-3 y D-4 (objetivo de 60 s sin garantía para dispositivos desconectados; motivo y 2FA para revocar a un dueño) | D-1…D-4 |
| PRV-15, 17; nuevo PRV-18 | Puerta jurídica y datos sintéticos | Se añade: sin copiar datos reales ni seudonimización superficial; la aprobación documental no autoriza tratar datos reales; evaluación de identificación indirecta | D-10, D-8 |
| Matriz §7.1 | 35 acciones | Filas 7, 14, 27 y 35 actualizadas; **filas 36 (vínculo de identidad) y 37 (archivar) nuevas** | Q-36, Q-37, D-4, D-5 |
| Q-38 | Subpunto (c) autogestión de roles abierto | **(c) resuelto por D-3**; quedan (a), (b) y (d) | D-3 |
| Q-40 | «¿Qué es cerrado definitivamente?» | Definición resuelta por D-5; **se reformula** con cuatro subpuntos (quién archiva, reapertura, efectos, visibilidad) | D-5 |
| Nuevo Q-43 | — | Criterio para incluir eventos en el resumen exportable | D-8 |
| §13, §14.7, §15, §16, §17 | v1.1 | Reorganizados; estimación y riesgos actualizados; dictamen actualizado | Consolidación |

**Sustituciones de la v1.3 (v1.2 → v1.3):**

| Elemento | v1.2 | v1.3 | Motivo |
|---|---|---|---|
| RT-14 (nuevo) | No existía | Conflicto de interés de dueño u organizador con rol de delegado: siete clases de acción, alcance por efecto, persistencia histórica y levantamiento | Q-38 (a), Q-44, Q-45 |
| RT-01, RT-05, RT-11, RT-12 | RT-11 «están pendientes»; RT-01 «salvo el dueño»; RT-12 sin nota de diferimiento | RT-01 «sujeto a RT-14»; RT-05 convive con RT-14; RT-11 reescrito (unión de permisos; revocar no elimina permisos de otros roles); RT-12 como regla de diseño | Q-38, Q-44, decisión de diferir |
| RF-ACC-08, 10, 13 | `COND:Q-38`; «2FA de delegados no decidido»; delegado de «un equipo» | `CONF`: delegado de un solo equipo por torneo; 2FA según la política más exigente de los roles vigentes | Q-38 (b), Q-39 |
| RF-ACC-07 | «no puede revocar… a otros organizadores» | Revocación parcial de roles adicionales de editor o delegado entre organizadores, solo en torneos comunes | Q-38 (d) |
| RF-ACC-02, 06, 11; nuevo RF-ACC-17 (S) | — | Segundo equipo del mismo torneo rechazado; sin asignar editores en partidos del propio equipo; historial de solo anexado; levantamiento de la restricción histórica (S) | Q-38, Q-44, Q-45 |
| RF-AUD-02, RF-AUD-05; nuevo RF-AUD-06 | — | Auditoría de rechazos y bloqueos por RT-14, de levantamientos, reanudaciones y revocaciones parciales | Q-44, Q-45, Q-46 |
| RF-EQP-01, RF-EQP-06 | RF-EQP-06 prioridad S, vínculo como mecanismo de identidad | RF-EQP-06 **diferido (P)**, sin operaciones ni interfaces en el MVP; RF-EQP-01 sin dependencia de él | Diferimiento (Q-37 sigue confirmada) |
| RD-RES-10; nuevos RD-RES-12 y RD-RES-13; RD-RES-01 | RD-RES-10 con aceptación vacía y mezclado con Q-29 | Suspender, reanudar, cierre desde `suspendido` y estados pendientes de Q-29 quedan separados; transiciones de §8.7 actualizadas | Q-46, Q-29 |
| RD-EVT-12 (nuevo); OFF-25 (nuevo); OFF-01, 07, 22 | — | Sin eventos nuevos en `suspendido`; validación de secuencias; una reanudación rechazada bloquea los eventos posteriores | Q-46 |
| RD-TAB-03 | «`suspendido` no cuenta» sin condición | La parte de `suspendido` se marca `COND:Q-29` | Q-29 |
| RD-CAL-02, 06, 07, 08; RD-RES-06, 09; RD-WO-02, 05; RD-TAB-09; RD-JUG-02; RF-EQP-02, 03 | Sin referencia a RT-14 | Cláusulas de RT-14 (alcance por efecto sobre partidos del propio equipo) | Q-44 |
| RF-TOR-03 | Aceptación «Ver RD-REG»; sin prueba | Criterio por trazabilidad a las pruebas de RD-REG-01, 04 y 06 | Auditoría de la Fase 1 |
| Matriz §7.1 | 37 acciones | Filas 6, 25 y 36 ajustadas; **filas 38 a 41 nuevas** | Q-12, Q-38, Q-45, Q-46 |
| RK-15 | Riesgo con mitigaciones | **Riesgo expresamente ACEPTADO por el usuario**; sin recuperación excepcional en el MVP | Decisión del usuario |
| Q-38, Q-39, Q-44, Q-45, Q-46 | Abiertas o propuestas | `CONF` (§2.1c); quedan 18 asuntos abiertos | Confirmaciones |
| §13, §14.8, §15, §16, §17 | v1.2 | Reorganizados; estimación y riesgos recalculados; dictamen actualizado | Consolidación |


---

## 1. Resumen ejecutivo

**Qué se define.** El MVP ampliado de Torneo de Campeones es una PWA para gestionar y publicar **ligas de fútbol** (una o dos vueltas) con equipos, plantillas, calendario, resultados, goleadores, tarjetas, W.O., tabla de posiciones con desempates configurables, usuarios con cuatro roles autenticados más el público, auditoría y **captura de marcadores y eventos sin conexión** con sincronización posterior. Copas, relámpagos, eliminatorias y fases de grupos quedan fuera (Q-18).

**Tamaño de la especificación (verificado contra el documento en la v1.3).** 54 requisitos funcionales generales (17 de acceso, 7 de torneos, 6 de equipos, 9 públicos, 5 de administración, 6 de auditoría y 4 de PWA); 72 reglas deportivas (RD); 25 requisitos de captura sin conexión; 18 de privacidad; 17 no funcionales; **186 requisitos en total** (169 M, 11 S, 6 P); 14 restricciones transversales de permisos y una matriz de 41 acciones por rol; 72 casos límite; 51 historias de usuario; 22 riesgos. Cada requisito M tiene criterio de aceptación y prueba prevista.

**Estado de las decisiones (§13).**
1. **21 decisiones confirmadas por el usuario:** las 14 bloqueantes de la v1.1, Q-36 y Q-37 (v1.2) y, en la v1.3, **Q-38** (roles combinados), **Q-39** (2FA de delegados), **Q-44** (alcance del conflicto de interés), **Q-45** (persistencia histórica del conflicto) y **Q-46** (reanudación de partidos suspendidos); además **RK-15 aceptado** y **RF-EQP-06 diferido**. **Esto no aprueba la Fase 1 ni autoriza la Fase 2.**
2. **Consecuencias derivadas D-1…D-11 confirmadas**, varias con precisiones o modificaciones expresas (en particular **D-5**: `finalizado` ≠ `archivado`, y **D-8**: exportación limitada); **D-12** es información confirmada, no una decisión (§2.2).
3. **Siguen abiertas 18 decisiones** (14 de la v1.0 y 4 derivadas: Q-40…Q-43) y dos acciones (persona responsable de evidencias; investigación de D-12), con 23 requisitos en estado `COND`. **Ninguna bloquea la aprobación de la Fase 1 ni formalmente la Fase 2**; Q-14 y Q-40 (d) deben resolverse antes de la Fase 3 (§13.5). Ninguna se decidió en nombre del usuario. Hay además 14 ambigüedades abiertas documentadas con lectura provisional (§13.7; A-3 y A-7 están resueltas; A-15 y A-16 se registraron en la v1.3.1) que **no son decisiones**.
4. El modo sin conexión es el mayor generador de complejidad y riesgo (§9); está confirmado (Q-17, Q-34, Q-46, D-7…D-11): cola local, idempotencia, 12 escenarios de conflicto, revalidación al sincronizar, rechazo de toda operación sin autorización vigente, secuencias validadas en orden (una reanudación rechazada nunca permite publicar los eventos posteriores), exportación limitada y registro de metadatos en el servidor.
5. **Estimación del MVP ampliado:** 123–187 días-persona (probable ≈ 151), es decir, unas **27–42 semanas** con una persona a dedicación completa o **49–75 semanas** a media jornada. Es una estimación inicial, no un compromiso (§15).
6. **Privacidad:** por defecto ningún menor aparece con nombre en lo público; **no se trata ningún dato personal real sin evaluación jurídica concluida** y hasta entonces solo se usan datos sintéticos (PRV-15, PRV-17); la identificación indirecta de menores (equipo + dorsal) sigue pendiente de evaluación (PRV-18); la **conservación y anonimización del historial de delegaciones siguen pendientes de validación jurídica** (RF-ACC-11, PRV-10).
7. **Dictamen técnico (recomendación, §17.4): APROBABLE CON CONDICIONES.**

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

### 2.1 Decisiones confirmadas en la consolidación del 2026-11-15 (14) — [CONF]

Fuente: mensaje «CONSOLIDACIÓN DEFINITIVA DE LAS 14 DECISIONES BLOQUEANTES DE FASE 1». Esta confirmación **no aprueba la Fase 1 ni autoriza la Fase 2**. Las consecuencias están en los requisitos citados en §13.1.

- **Q-02a (Opción A) — Asignación automática del organizador.** Cuando un organizador autorizado crea un torneo, queda automáticamente asignado como organizador de ese torneo; la operación se registra en la auditoría; el organizador no puede crear, invitar ni designar otros organizadores; no obtiene permisos sobre torneos ajenos.
- **Q-02b (Opción A) — Asignación de editores.** El dueño y los organizadores autorizados asignan y desasignan editores a partidos; el dueño conserva el control global; los organizadores administran solo partidos de sus torneos; los editores no pueden autoasignarse partidos; asignaciones y revocaciones se auditan.
- **Q-02c (Opción A) — Revocación de accesos.** El dueño revoca accesos globalmente, protegiendo al último dueño activo; los organizadores revocan editores y delegados exclusivamente de sus torneos; los organizadores no pueden revocar al dueño ni a otros organizadores; la revocación se aplica de inmediato en el servidor y se audita; las operaciones sin conexión pendientes se someten a nueva verificación de autorización al sincronizar.
- **Q-08 (Opción B) — Autenticación de dos factores.** Obligatorio para el dueño y los organizadores; opcional y recomendado para editores, con reevaluación tras el piloto; se mantienen mecanismos seguros de autenticación, recuperación y revocación de sesiones.
- **Q-09 (Opción A) — Corrección de partidos finalizados.** El dueño y el organizador autorizado corrigen resultados finalizados sin plazo límite, con motivo obligatorio, historial completo del resultado anterior y el nuevo, registro del responsable y la fecha, y recálculo consistente de las estadísticas afectadas; si el torneo está cerrado definitivamente, solo el dueño corrige (**«cerrado definitivamente» = `archivado`**, según D-5); los editores no pueden reabrir partidos finalizados; en el MVP las disputas se gestionan mediante correcciones justificadas, sin flujo formal de reclamaciones.
- **Q-11 (Opción A) — Historial de temporadas.** No se importan temporadas anteriores en el MVP; los torneos administrados desde la plataforma generan su propio historial; un equipo puede conservar su identidad entre torneos; plantillas y estadísticas son independientes por torneo; no se asume que conservar datos históricos implique desarrollar de inmediato un archivo histórico público.
- **Q-12 (Opciones A + C) — Identificación y duplicidad de jugadores.** Detección de posibles duplicados mediante normalización de nombres; confirmación del organizador; si se confirma que es el mismo jugador y ya pertenece a otro equipo del mismo torneo, se rechaza la segunda inscripción; no se recopilan documentos de identidad en el MVP; una misma identidad confirmada no puede pertenecer a dos equipos del mismo torneo, incluso ante operaciones concurrentes; se documentan las limitaciones de identificar por nombre y los falsos positivos.
- **Q-15 (Opción B) — Múltiples roles.** Una persona puede tener varios roles dentro del mismo torneo; un delegado no puede actuar como editor en partidos donde participe su propio equipo; los permisos se evalúan por usuario, rol, torneo, equipo y partido; no se transfieren entre torneos; la acumulación de roles no debe permitir evadir restricciones de seguridad ni conflictos de interés.
- **Q-16 (Opción A) — Disponibilidad.** Durante el piloto se opera con mejor esfuerzo, sin comprometer un porcentaje contractual; son obligatorios la captura sin conexión, la sincronización posterior segura, la consulta de información almacenada con fecha de actualización y el manejo explícito de errores y recuperación; el objetivo numérico se reevalúa cuando se verifiquen las garantías y los costos oficiales de los proveedores.
- **Q-24 (Opción A) — Consentimiento de menores.** El delegado o el organizador recoge la autorización; la organización conserva la evidencia original de forma segura; la aplicación almacena únicamente metadatos; la organización designará una persona responsable de custodiar las evidencias y atender solicitudes; no hay carga ni almacenamiento de documentos de consentimiento en el MVP; la validación jurídica sigue siendo obligatoria antes de publicar datos personales reales.
- **Q-25 — Ubicación y transferencia internacional.** Se permite evaluar infraestructura internacional, incluidos Supabase y Vercel; se documentan regiones, procesamiento, almacenamiento, transferencias y responsabilidades; se verifican garantías contractuales y medidas de seguridad; se minimizan los datos personales; la evaluación jurídica aplicable se completa antes de tratar datos personales reales. **No autoriza contratar proveedores ni desplegar infraestructura.**
- **Q-32 (Opción A) — Publicación de resultados.** Publicación automática cuando el servidor acepta el resultado y la jornada está visible, sin aprobación adicional del organizador; lo capturado sin conexión no es oficial ni público hasta su validación en el servidor; antes de aceptar se verifican autenticación, permisos, asignación al partido, integridad y conflictos; toda corrección posterior conserva motivo y auditoría.
- **Q-34 — Parámetros sin conexión.** Se aprueban los seis parámetros de §9.5 y la política de 12 escenarios de §9.4 (conservadas íntegras en esas secciones). Reglas adicionales: no sobrescribir resultados confirmados en silencio; no descartar capturas locales en silencio; gestionar duplicados y conflictos de versión; advertir del riesgo de que el navegador elimine el almacenamiento local; no prometer conservación indefinida; no sincronizar operaciones que hayan perdido autorización.
- **Q-35 (Opción B) — Continuidad administrativa.** Un dueño existente puede designar a un segundo dueño de respaldo; ambos tienen privilegios globales; solo un dueño existente otorga el rol de dueño; la designación exige autenticación reforzada y se audita; se protege al último dueño activo frente a eliminación o degradación; ningún organizador concede ni revoca el rol de dueño; se consideran procedimientos de recuperación y continuidad.

### 2.1b Decisiones confirmadas en la segunda consolidación (2) — [CONF]

Fuente: mensaje «CONSOLIDACIÓN DE Q-36, Q-37 Y D-1 A D-12» del 2026-11-15.

- **Q-36 (Opción A) — Editores simultáneos.** Cada partido tiene un único editor vigente. Una reasignación desactiva la asignación anterior y activa la nueva mediante una operación controlada y auditada; no debe existir una ventana de concurrencia con dos editores vigentes para el mismo partido. Las capturas sin conexión del editor anterior no se publican automáticamente si este perdió autorización y se conservan temporalmente conforme a la política sin conexión aprobada. La decisión **no elimina** el control de conflictos por versiones, cambios del organizador, operaciones retrasadas ni sincronizaciones repetidas.
- **Q-37 (Opción B) — Identidad común de equipos.** Cada torneo mantiene inscripciones independientes; únicamente el dueño puede vincular, de forma opcional, equipos de torneos diferentes a una identidad común; el vínculo no comparte automáticamente plantillas, estadísticas, permisos ni información privada; los organizadores solo administran inscripciones de sus torneos autorizados; la vinculación no concede acceso a información restringida de otros torneos.

### 2.1c Decisiones confirmadas en la tercera consolidación (v1.3) — [CONF]

Fuente: mensajes «PREPARACIÓN CONTROLADA DE FASE 1 v1.3», «AJUSTES FINALES AL PLAN v1.3», «PLAN DEFINITIVO DE CONSOLIDACIÓN DOCUMENTAL v1.3» y «EJECUCIÓN CONTROLADA DE CONSOLIDACIÓN DOCUMENTAL v1.3» (2026-11-15). Estas confirmaciones **no aprueban la Fase 1 ni autorizan la Fase 2**.

- **Q-38 (roles combinados).** (a) **Opción A:** un dueño u organizador puede ser también delegado, pero no ejecuta acciones deportivas sensibles sobre su propio equipo. (b) **Opción A:** una persona solo puede ser delegada de un equipo por torneo; puede representar equipos distintos en torneos diferentes. (c) Resuelta mediante D-3: nadie puede concederse roles adicionales a sí mismo. (d) **Opción A:** un organizador puede revocar las asignaciones adicionales de editor o delegado de otro organizador, únicamente en torneos que ambos administren, sin revocar ni modificar su rol de organizador. Las revocaciones se aplican en el servidor y se auditan; no eliminan los permisos legítimos procedentes de otros roles.
- **Q-39 (2FA de delegados, Opción B).** 2FA opcional y recomendado para delegados; obligatoriedad a reevaluar tras el piloto; si la misma cuenta tiene rol de dueño u organizador, el 2FA es obligatorio; la política efectiva considera todos los roles vigentes de la cuenta.
- **Q-44 (alcance del conflicto de interés, Opción B).** Un dueño u organizador que también sea delegado no ejecuta acciones deportivas sensibles relacionadas con su propio equipo: resultados, W.O., resoluciones manuales, asignaciones que generen conflictos, ajustes de calendario que afecten al propio equipo, decisiones de inscripción o retiro de ese equipo y verificación de jugadores duplicados de su equipo. Condiciones: aplicación efectiva en el servidor; reevaluación al sincronizar operaciones sin conexión; auditoría; sin evasión mediante otro rol; sin otro actor autorizado y sin conflicto, la operación se bloquea hasta que exista una autoridad habilitada; sin excepciones automáticas; respetando la protección del último dueño activo. Se evalúa por **efecto sobre partidos** (consultar, previsualizar sin persistir, ejecutar), incluidos efectos automáticos y en cascada, con rechazo de la operación completa; la generación y la regeneración del calendario no están exentas.
- **Q-45 (conflicto tras retirar el rol de delegado, Opción C; extensión a editores, Opción A).** Un dueño u organizador que haya sido delegado de un equipo conserva la restricción respecto de ese equipo, dentro del mismo torneo, tras perder el rol. Solo puede levantarse si otro dueño autorizado y sin conflicto lo aprueba, con motivo obligatorio, con auditoría, limitado al equipo y torneo, sin autoaprobación y sin conceder permisos adicionales; sin ese dueño la restricción permanece. La persistencia no se extiende a editores, que conservan las restricciones de Q-15. Se aplica en el servidor y en la sincronización sin conexión.
- **Q-46 (reanudación de partidos suspendidos, Opción B; eventos durante la suspensión, Opción A).** Un partido suspendido puede ser reanudado por el editor asignado, el organizador autorizado y el dueño, sujetos a permisos vigentes, asignación y restricciones de Q-44 y Q-45. El editor puede registrar la reanudación sin conexión como operación local pendiente, que no modifica el estado oficial publicado, se valida al sincronizar y puede rechazarse. Mientras un partido esté suspendido no se admiten goles, tarjetas ni otros eventos nuevos; primero debe reanudarse. Una secuencia sin conexión con reanudación y eventos posteriores se valida en orden e integridad; no se publican eventos si se rechaza la reanudación. **La facultad de cerrar definitivamente un partido desde suspendido no se presume aprobada.** Q-29 sigue pendiente.
- **RK-15 — aceptado.** El usuario aceptó expresamente el riesgo residual de pérdida de eventos sin conexión rechazados; se mantienen D-8, D-11 y las mitigaciones; no se introduce recuperación excepcional de eventos en el MVP; no se declara que toda captura rechazada pueda recuperarse.
- **RF-EQP-06 — implementación diferida.** El vínculo administrativo de identidades comunes de equipos queda fuera del MVP; Q-37 permanece confirmada (inscripciones independientes por torneo; en el futuro solo el dueño podrá vincular); no se construyen operaciones ni interfaces de vinculación.

### 2.2 Consecuencias derivadas D-1…D-12 — confirmadas por el usuario (2026-11-15)

La columna «Texto v1.1» conserva el texto propuesto antes de la confirmación. D-1…D-11 están **CONFIRMADAS** (algunas **con precisión o modificación expresa**); D-12 es **información confirmada**, no una decisión.

| # | Texto v1.1 (historial) | Confirmación del usuario (vigente) | Estado | Decisión de origen | Dónde |
|---|---|---|---|---|---|
| D-1 | «Organizador autorizado» = cuenta designada por el dueño; puede crear torneos y administrar los que se le asignen | Una cuenta de organizador es designada por el dueño; puede crear torneos y administrar los que **crea o tiene asignados**; no recibe acceso administrativo automático a otros torneos ni puede designar nuevos organizadores | CONFIRMADA | Q-02a, Q-02, Q-19 | RF-ACC-03, RF-ACC-12 |
| D-2 | La revocación es inmediata en el servidor; la invalidación de sesiones en ≤ 60 s es un parámetro de Fase 2 | Las revocaciones se aplican de inmediato en el servidor; toda nueva operación sin autorización vigente se rechaza; **60 s es un objetivo técnico** para que las sesiones conectadas reflejen la revocación, sujeto a validación en Fase 2; **no es garantía** para dispositivos desconectados, que verifican de nuevo sus permisos al sincronizar | CONFIRMADA CON PRECISIÓN | Q-02c | RF-ACC-07, OFF-22, RT-13 |
| D-3 | Nadie puede invitarse a sí mismo un rol adicional | Ningún usuario puede invitarse, asignarse o concederse roles adicionales a sí mismo; las asignaciones las hacen otras cuentas autorizadas; el servidor lo verifica; se audita. La asignación automática del organizador creador (Q-02a) es una **operación del sistema autorizada previamente**, no una autoasignación discrecional | CONFIRMADA | Q-15, Q-02, Q-02a | RF-ACC-02, RF-ACC-12, RT-04 |
| D-4 | Un dueño puede revocar a otro dueño con autenticación reforzada y auditoría, nunca al último activo | Un dueño puede revocar a otro dueño exigiendo **nueva autenticación 2FA**, **motivo obligatorio** y auditoría; se protege **de manera atómica** al último dueño activo, incluso ante operaciones simultáneas; ambos dueños conservan los mismos privilegios globales; **no hay jerarquía técnica** entre dueño principal y de respaldo | CONFIRMADA CON CONTROLES ADICIONALES | Q-35 | RF-ACC-09, RT-06, matriz fila 35 |
| D-5 | «Torneo cerrado definitivamente» = `finalizado` o `archivado` | **Sustituida.** `finalizado`: terminó la competición deportiva; el dueño y el organizador autorizado aún pueden corregir resultados con motivo obligatorio y auditoría. `archivado`: cierre definitivo; desde ese estado únicamente el dueño corrige resultados. No se conserva la equivalencia anterior | CONFIRMADA CON MODIFICACIÓN EXPRESA | Q-09 | RD-RES-06, RF-TOR-02, matriz filas 14, 27, 37, Q-40, CL-34, CL-44 |
| D-6 | Una alta con posible duplicado queda `pendiente de verificación` y el delegado no ve datos de otros equipos | **Toda** inscripción con posible duplicado queda pendiente de verificación; el jugador no queda habilitado hasta resolver la coincidencia; el delegado no consulta datos de otros equipos; el organizador verifica si es la misma persona; si son distintas, puede aprobar; si ya pertenece a otro equipo del torneo, se rechaza la inscripción duplicada; la resolución se audita; sin documentos de identidad; se mantienen las garantías de concurrencia y las limitaciones por nombre | CONFIRMADA CON PRECISIÓN | Q-12 | RD-JUG-02, 08, 09 |
| D-7 | La retención de 30 días se concilia con «no descartar en silencio» mediante avisos previos y posterior | Retención **máxima de 30 días** para operaciones rechazadas o en conflicto, sujeta a privacidad; **informar el plazo al detectar el conflicto**; avisos previos cuando sea técnicamente posible; eliminación controlada al vencimiento; constancia mínima sin datos personales protegidos cuando corresponda y sea viable; **sin garantía** de avisos ni de conservación si el navegador elimina el almacenamiento o el dispositivo permanece desconectado | CONFIRMADA CON PRECISIÓN | Q-34 | OFF-10, OFF-17, PRV-10 |
| D-8 | El «resumen deportivo mínimo» contiene identificador de partido, abreviaturas, marcador y eventos con tipo, minuto, dorsal y equipo, sin nombres | **Sustituida en parte.** El resumen puede contener identificador de partido, abreviaturas de los equipos y marcador. Los eventos, minutos, dorsales y equipos asociados solo pueden incluirse cuando su divulgación esté autorizada y no exponga datos personales protegidos, especialmente de menores; no se exportan capturas privadas o rechazadas cuya divulgación no esté autorizada; una exportación no concede permisos nuevos; se evalúa la identificación indirecta | CONFIRMADA CON PROTECCIÓN ADICIONAL | Q-34, Q-05 | OFF-24, OFF-11, PRV-18, Q-43 |
| D-9 | El servidor registra metadatos de operaciones en conflicto o rechazadas (sin datos personales) para que el organizador las vea | El servidor registra metadatos **mínimos** de las operaciones rechazadas o en conflicto que **efectivamente recibe**; los organizadores solo consultan incidencias de sus torneos; no se exponen capturas privadas ni datos personales protegidos; los identificadores técnicos están sujetos a privacidad y retención; las operaciones nunca recibidas permanecen solo en el dispositivo; el registro **no autoriza publicar** operaciones rechazadas | CONFIRMADA CON CONTROLES ADICIONALES | Q-34, Q-02c | OFF-20, RF-ADM-04, PRV-10 |
| D-10 | La puerta jurídica alcanza a todo dato personal real y exige datos sintéticos en desarrollo y pruebas | Desarrollo y pruebas exclusivamente con datos sintéticos; no se copian datos personales reales ni se usa seudonimización superficial como sustituto; antes de registrar o procesar datos personales reales se completa la evaluación jurídica (cuentas de usuarios, jugadores, delegados y menores; proveedores, transferencias internacionales, seguridad, autorizaciones y retención); **la aprobación documental de requisitos no equivale a autorización para tratar datos reales** | CONFIRMADA CON PRECISIÓN | Q-24, Q-25 | PRV-15, PRV-17 |
| D-11 | La autorización se evalúa con el estado vigente al sincronizar; una desasignación previa puede rechazar capturas legítimas (se conservan y exportan) | Se verifican los permisos vigentes en el servidor al sincronizar; se **rechaza** toda operación cuya autorización fue revocada, **incluso si se capturó legítimamente antes**; no se publica ni se sobrescribe información oficial; se conservan temporalmente conforme a D-7 cuando sea técnicamente posible; solo la exportación limitada de D-8; el organizador consulta únicamente los metadatos recibidos (D-9); **sin recuperación ni publicación automática** | CONFIRMADA CON PRECISIÓN | Q-02c, Q-34 | OFF-22, OFF-11, OFF-24, §9.4, RK-15 |
| D-12 | La segunda parte de Q-08 (cómo registran hoy los resultados) no fue respondida | **Información confirmada:** actualmente los resultados se registran mediante una **combinación de varios métodos**. No se especificó cuáles ni cómo se combinan; **no se infiere ninguna herramienta, persona, responsable ni procedimiento actual**. No es un requisito técnico. El detalle queda como **acción de investigación pendiente** (§13.4) | INFORMACIÓN CONFIRMADA (no es decisión) | Q-08 (segunda parte) | RNF-13 (línea base), §13.4 |

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
| Levantamiento de la restricción histórica de conflicto de interés (RF-ACC-17) | Sí (S): si se difiere, **todavía no existe mecanismo para levantar conflictos históricos** y la restricción permanece vigente | — |
| Vínculo opcional de identidad común de equipos entre torneos (solo dueño, RF-EQP-06) | **No** (implementación diferida por decisión del usuario; Q-37 sigue confirmada como regla de gobernanza) | Fase posterior |
| **Captura sin conexión** con cola, idempotencia, conflictos, revocación | Sí | Operaciones administrativas sin conexión |
| Vista pública (tabla, calendario, resultados, goleadores, tarjetas, plantillas con nombre público) y panel administrativo | Sí | Marcadores en vivo (tiempo real), cronología |
| PWA instalable, caché, estados de red | Sí | Notificaciones push |
| Compartir por enlace, QR y botón nativo de compartir | Sí (S) | Imágenes con marca para compartir |
| Privacidad base: nombre público condicionado, autorización (solo metadatos), anonimización | Sí | Carga y almacenamiento de documentos de consentimiento: **fuera del MVP** (CONF Q-24) |
| Copa, relámpago, grupos, eliminatorias, ida y vuelta eliminatoria, empates eliminatorios, llaves | **No** (Q-18) | Fase 6 (el modelo no debe impedirlos) |
| Pagos, boletería, video, estadísticas avanzadas, patrocinadores, galería | **No** | Evaluar tras el piloto |
| Histórico de temporadas anteriores | **No** (CONF Q-11): cada torneo genera su propio historial; los equipos pueden conservar identidad; **no** se asume un archivo histórico público | Archivo histórico público: a decidir |
| Disputas formales de resultados (flujo de reclamaciones) | **No** (CONF Q-09); se resuelven con correcciones justificadas | A decidir |

---

## 4. Glosario mínimo

- **Torneo:** competición de liga con reglamento, equipos inscritos y calendario. **Equipo inscrito:** participación de un equipo en un torneo (plantilla por torneo).
- **Jornada:** conjunto de partidos de una vuelta que se programan juntos; **vuelta:** ciclo completo en que todos enfrentan a todos una vez.
- **Partido confirmado:** estado del partido tal como lo aceptó el servidor. **Dato local/pendiente:** captura guardada en el dispositivo y aún no aceptada.
- **Operación (op):** unidad de cambio generada en el dispositivo (p. ej. «gol de X al minuto 34»), con identificador único.
- **Torneo iniciado:** el torneo en que algún partido ya salió del estado `programado` (RD-REG-02).
- **Nombre público:** el nombre con que un jugador aparece en vistas públicas (PRV-02).
- **Autenticación reforzada:** reverificación del segundo factor en el momento de una operación sensible (p. ej. designar a un segundo dueño).
- **Identidad de equipo:** vínculo opcional, creado solo por el dueño (Q-37), entre inscripciones del mismo equipo en distintos torneos; **no** comparte plantillas, estadísticas, permisos ni información privada ni concede acceso a otro torneo. La implementación del vínculo está **diferida fuera del MVP** (RF-EQP-06).
- **Restricción histórica de conflicto:** la que conserva un dueño u organizador respecto de un equipo y un torneo después de perder el rol de delegado de ese equipo (Q-45); solo la levanta otro dueño sin conflicto (RF-ACC-17).
- **Reanudar:** pasar un partido de `suspendido` a `en_juego` (RD-RES-13).
- **`finalizado` / `archivado`:** `finalizado` = terminó la competición deportiva (dueño y organizador pueden corregir resultados); `archivado` = cierre definitivo (solo el dueño corrige resultados) (D-5).
- **Pendiente de verificación:** estado de un alta de jugador que espera la decisión del organizador por posible duplicado (RD-JUG-02).

---

## 5. Historias de usuario (cinco perfiles)

Formato: «Como [rol], quiero [acción], para [valor]». Los criterios de aceptación están en los requisitos vinculados. **Todas son propuestas pendientes de aprobación de la fase.**

### 5.1 Dueño

| ID | Historia | Requisitos | Prio. |
|---|---|---|---|
| HU-DUE-01 | Como dueño, quiero invitar y designar organizadores con un torneo asignado, para delegar la operación. | RF-ACC-02, 03 | M |
| HU-DUE-02 | Como dueño, quiero revocar el acceso de cualquier usuario con efecto inmediato en el servidor, sin poder dejar la plataforma sin un dueño activo, para contener un abuso. | RF-ACC-07, RT-06 | M (CONF Q-02c) |
| HU-DUE-03 | Como dueño, quiero ver el historial completo de cambios de todos los torneos, para responder preguntas y disputas. | RF-AUD-01…04 | M |
| HU-DUE-04 | Como dueño, quiero autorizar un cambio de reglamento en un torneo iniciado viendo su impacto antes de confirmar, para no alterar la tabla por error. | RD-REG-03 | M (COND Q-23) |
| HU-DUE-05 | Como dueño, quiero atender una solicitud de acceso o anonimización de un jugador, para cumplir la privacidad. | PRV-08, 09 | M |
| HU-DUE-06 | Como dueño, quiero designar a un segundo dueño de respaldo con autenticación reforzada, para que la plataforma no dependa de una sola cuenta. | RF-ACC-09 | M (CONF Q-35) |
| HU-DUE-07 | Como dueño, quiero corregir cualquier resultado —también de un torneo `archivado` (cierre definitivo)— salvo los de mi propio equipo si soy delegado (RT-14), con motivo obligatorio, para enmendar errores con trazabilidad. | RD-RES-06 | M (CONF Q-09, D-5, Q-44) |
| HU-DUE-08 | Como dueño, quiero vincular opcionalmente inscripciones de un mismo equipo en distintos torneos a una identidad común, sin que se comparta información entre torneos. | RF-EQP-06, RT-12 | P (CONF Q-37; implementación diferida) |
| HU-DUE-09 | Como dueño sin conflicto, quiero levantar, con motivo y auditoría, la restricción histórica de otro dueño u organizador respecto de un equipo y torneo concretos, para desbloquear operaciones legítimas. | RF-ACC-17, RT-14 | S (CONF Q-45) |

### 5.2 Organizador

| ID | Historia | Requisitos | Prio. |
|---|---|---|---|
| HU-ORG-01 | Como organizador, quiero crear un torneo de liga y quedar automáticamente asignado como su organizador, sin acceso a torneos ajenos, para administrarlo. | RF-TOR-01, RF-ACC-12 | M (CONF Q-02a) |
| HU-ORG-02 | Como organizador, quiero configurar vueltas, desempates y W.O. antes de iniciar, para que la tabla refleje nuestro reglamento. | RD-REG-01, 04, RD-TAB-05 | M |
| HU-ORG-03 | Como organizador, quiero inscribir equipos y retirar uno si abandona, para mantener la competición. | RF-EQP-02, 03 | M (retiro COND Q-26) |
| HU-ORG-04 | Como organizador, quiero invitar delegados y editores solo en mis torneos, para repartir el trabajo. | RF-ACC-04 | M |
| HU-ORG-05 | Como organizador, quiero generar el calendario con vista previa y ajustarlo a mano con rastro, para adaptarlo a las canchas. | RD-CAL-01…06 | M |
| HU-ORG-06 | Como organizador, quiero publicar jornada por jornada, para controlar lo que ve el público. | RD-CAL-08, RF-PUB-04 | M |
| HU-ORG-07 | Como organizador, quiero asignar y desasignar editores a partidos de mis torneos, para que cada partido tenga responsable. | RF-ACC-06 | M (CONF Q-02b) |
| HU-ORG-08 | Como organizador, quiero ver alertas de partidos próximos sin editor, para reaccionar antes del inicio. | RF-ADM-04 | M (COND Q-10) |
| HU-ORG-09 | Como organizador, quiero corregir un resultado finalizado de mis torneos, sin plazo límite mientras el torneo no esté `archivado`, indicando el motivo, para enmendar errores con trazabilidad. | RD-RES-06 | M (CONF Q-09, D-5) |
| HU-ORG-10 | Como organizador, quiero declarar un W.O. con motivo, para aplicar el reglamento. | RD-WO-01…05 | M (COND Q-21) |
| HU-ORG-11 | Como organizador, quiero resolver manualmente un empate que los criterios no deshacen, con motivo visible, para cerrar la tabla. | RD-TAB-09 | M (COND Q-22) |
| HU-ORG-12 | Como organizador, quiero ver el historial de mis torneos, para supervisar. | RF-AUD-04, RF-ADM-03 | M |
| HU-ORG-13 | Como organizador, quiero ver el registro (metadatos) de operaciones en conflicto o rechazadas de mis editores, para resolverlas. | RF-ADM-04, OFF-09, OFF-10, OFF-20 | M |
| HU-ORG-14 | Como organizador, quiero verificar si un jugador con posible duplicado es la misma persona o un homónimo, y aprobar o rechazar la inscripción, para mantener un jugador en un solo equipo por torneo. | RD-JUG-02, RD-JUG-08 | M (CONF Q-12, D-6) |
| HU-ORG-15 | Como organizador que además soy delegado, quiero saber qué acciones no puedo ejecutar sobre mi equipo y quién puede hacerlas, para no quedar bloqueado sin explicación. | RT-14, RF-AUD-06 | M (CONF Q-44) |
| HU-ORG-16 | Como organizador, quiero revocar el rol adicional de editor o delegado de otro organizador en torneos que ambos administramos, sin afectar su rol de organizador. | RF-ACC-07 | M (CONF Q-38 d) |
| HU-ORG-17 | Como organizador o dueño, quiero reanudar en línea un partido suspendido de mis torneos, para que continúe el registro. | RD-RES-13 | M (CONF Q-46) |

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
| HU-EDI-10 | Como editor, quiero ser advertido de que el navegador puede eliminar los datos guardados en mi dispositivo y de que no se conservan indefinidamente, para sincronizar a tiempo. | OFF-17, OFF-10 | M (CONF Q-34) |
| HU-EDI-11 | Como editor, quiero seguir capturando si mi sesión vence y sincronizar cuando vuelva a iniciar sesión con permisos vigentes, para no perder el partido. | OFF-23 | M (CONF Q-34) |
| HU-EDI-12 | Como editor, quiero reanudar un partido suspendido —también sin conexión, como operación pendiente— y registrar los eventos posteriores, sabiendo que no se publican si la reanudación se rechaza. | RD-RES-13, OFF-25, RD-EVT-12 | M (CONF Q-46) |

### 5.4 Delegado

| ID | Historia | Requisitos | Prio. |
|---|---|---|---|
| HU-DEL-01 | Como delegado, quiero ver y mantener la plantilla de mi equipo (altas, bajas, dorsales), para tener a mis jugadores listos. | RD-JUG-01…06 | M |
| HU-DEL-02 | Como delegado, quiero registrar que se obtuvo la autorización de publicación del nombre de un jugador (o de su representante legal), sin subir documentos, para que aparezca con nombre en lo público. | PRV-02…05 | M (CONF Q-24) |
| HU-DEL-03 | Como delegado, quiero ver el calendario y los resultados de mi equipo, para planificar. | RF-ADM-01 | M |
| HU-DEL-04 | Como delegado, quiero saber si el alta de un jugador requiere verificación del organizador por posible duplicado, **sin ver datos de otros equipos**, para no bloquear mi plantilla sin entender el motivo. | RD-JUG-02 | M (CONF Q-12) |
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
| RF-ACC-02 | Toda invitación define: correo destinatario, rol, ámbito (torneo o equipo inscrito), quién invita, fecha de vencimiento, un solo uso y estado (pendiente / aceptada / vencida / revocada). Vencimiento propuesto: 7 días. **Ningún usuario puede invitarse, asignarse o concederse roles adicionales a sí mismo** (RT-04); una invitación al mismo correo de quien invita se rechaza; el servidor lo verifica y la concesión se audita. Se rechaza la invitación de un segundo equipo del mismo torneo a quien ya es delegado vigente (Q-38 b). | M | CONF (D-3); plazo REC [PEND] | Invitación vencida, usada o revocada no permite alta (3 casos); aceptada con un correo distinto del invitado se rechaza; el rol y el ámbito no pueden modificarse tras emitirla; autoinvitación rechazada en interfaz y API. Invitar a un delegado vigente a otro equipo del mismo torneo → rechazado, también con aceptaciones concurrentes. | DB, I |
| RF-ACC-03 | **Solo el dueño** crea o invita organizadores. Una cuenta de organizador, designada por el dueño, puede crear torneos (RF-ACC-12) y administrar los que **crea o tiene asignados**; **no recibe acceso administrativo automático a otros torneos**; el dueño puede asignarle además torneos existentes. Ningún organizador puede crear, invitar ni designar a otros organizadores. | M | CONF (Q-19, Q-02a; D-1) | Un organizador, editor o delegado que intenta invitar o designar un organizador recibe denegación (interfaz oculta la acción; API y BD la rechazan); un organizador no ve ni administra torneos que no creó ni se le asignaron. | DB, I |
| RF-ACC-04 | El organizador invita y administra **editores y delegados exclusivamente en los torneos que tiene asignados**. | M | CONF (Q-19) | Invitar a un editor al torneo B siendo organizador solo del A → denegado. Invitar a un delegado de un equipo inscrito en A → permitido. | DB, I |
| RF-ACC-05 | Editor y delegado **no** pueden invitar usuarios ni asignar roles. | M | CONF (Q-19) | Los cuatro intentos (editor/delegado × invitar/asignar) son denegados en BD. | DB |
| RF-ACC-06 | **Asignación de editores a partidos:** el dueño (global) y el organizador (solo partidos de sus torneos) asignan, desasignan y **reasignan** editores a partidos concretos. El editor **no puede autoasignarse** partidos ni asignar a otros. El editor asignado debe tener rol de editor vigente en ese torneo y no incurrir en el conflicto de RT-05. **Cada partido tiene un único editor vigente.** Una **reasignación** es una operación controlada y atómica que desactiva la asignación anterior y activa la nueva; **no existe ventana** en que dos editores estén vigentes en el mismo partido (la base de datos lo garantiza, también ante reasignaciones simultáneas). Las capturas sin conexión del editor anterior que se sincronicen después **se rechazan y no se publican** (RT-13, OFF-22) y se conservan según OFF-10. Al reasignar un partido `en_juego` o ya iniciado, la interfaz advierte [REC] que el editor anterior puede tener capturas sin sincronizar. Se conserva el historial (quién, cuándo, hasta cuándo) y **cada asignación, desasignación y reasignación se audita**. Esta decisión no sustituye el control de conflictos por versión, cambios del organizador, operaciones retrasadas ni sincronizaciones repetidas (RD-RES-04, OFF-06…09). **El dueño o el organizador que sea delegado de un equipo no asigna editores a partidos de ese equipo (RT-14).** | M | CONF (Q-02b, Q-36 A); advertencia REC | Editor intenta asignarse → denegado. Organizador asigna en partido de otro torneo → denegado. Asignar a un editor de otro torneo, o a un delegado de un equipo del partido → rechazado. **Dos reasignaciones simultáneas del mismo partido: una prevalece y nunca hay dos asignaciones vigentes** (verificado con concurrencia en BD). Historial y auditoría muestran altas, bajas y reasignaciones. Organizador que es delegado asigna un editor en un partido de su equipo → rechazado. | DB, I |
| RF-ACC-07 | **Revocación:** el dueño puede revocar a cualquier usuario; el organizador puede revocar **editores y delegados exclusivamente de sus torneos**; el organizador **no** puede revocar al dueño ni a otros organizadores. **La revocación se aplica de inmediato en el servidor:** toda nueva operación sin autorización vigente se rechaza. **Objetivo técnico:** las sesiones **conectadas** reflejan la revocación en ≤ 60 s (sujeto a validación en la Fase 2; **no es una garantía** de notificación para dispositivos desconectados). Los dispositivos sin conexión **verifican de nuevo sus permisos al sincronizar** (OFF-11, OFF-22). Se protege al último dueño activo (RT-06). Todo queda auditado. **Revocación parcial (Q-38 d):** un organizador puede revocar las asignaciones adicionales de **editor o delegado** que otro organizador posea, **únicamente en torneos que ambos administran**, **sin revocar ni modificar su rol de organizador** ni los permisos legítimos de sus otros roles; se aplica en el servidor y se audita. **Nota (v1.3.1):** la interacción de estas revocaciones con RT-14 (desasignaciones y efectos indirectos sobre partidos del propio equipo del actor) está registrada como ambigüedad abierta A-15 (§13.7); este requisito y la matriz no cambian. | M | CONF (Q-02c, Q-38 d; D-2) | Tras revocar, una operación de escritura del afectado se rechaza; la medición del tiempo en que una sesión conectada deja de funcionar se hace en Fase 2 (objetivo ≤ 60 s); un organizador que intenta revocar a otro organizador o al dueño → denegado; una revocación simultánea a una operación en línea resulta en «aplicada antes y válida» o «rechazada», nunca a medias (RT-13); todo auditado. Revocación parcial entre organizadores: permitida en torneos comunes y denegada en torneos que el revocador no administra; el rol de organizador del afectado y sus permisos de otros roles permanecen. | DB, I |
| RF-ACC-08 | **Varios roles por persona:** una cuenta puede tener varios roles dentro del mismo torneo y roles distintos en torneos distintos. Los permisos se evalúan por usuario, rol, torneo, equipo y partido (RT-01); no se transfieren entre torneos; ninguna combinación debe evadir restricciones de seguridad o de conflicto de interés (RT-05, RT-11, RT-14); una persona es delegada de **un solo equipo por torneo** (RF-ACC-13). | M | CONF (Q-15, Q-38) | Un usuario con rol de editor en A y delegado en B opera con el rol correcto en cada uno; un delegado no actúa como editor en partidos de su equipo; ningún permiso se «suma» entre torneos. | DB |
| RF-ACC-09 | **Continuidad administrativa:** un dueño existente puede designar a un **segundo dueño** de respaldo; ambos tienen **los mismos privilegios globales y no existe jerarquía técnica** entre ellos; solo un dueño existente otorga el rol de dueño (nunca un organizador); **designar o revocar a un dueño exige nueva autenticación con 2FA**, y la revocación además **motivo obligatorio**; ambas operaciones se auditan; el sistema protege **de forma atómica** al último dueño activo contra eliminación o degradación, **incluso ante operaciones simultáneas**; existen procedimientos documentados de recuperación y continuidad (cuenta perdida, pérdida del segundo factor). Notificar al dueño afectado por una designación o revocación es una recomendación [REC] no decidida. | M | CONF (Q-35; D-4); notificación REC | Organizador intenta conceder o revocar el rol de dueño → denegado; designar o revocar sin reverificación del 2FA, o revocar sin motivo → rechazado; **dos dueños que se revocan mutuamente al mismo tiempo: como máximo una revocación prospera y queda al menos un dueño activo**; intento de revocar al único dueño activo → rechazado; runbook de recuperación revisado. | DB, I, D |
| RF-ACC-10 | **Autenticación:** correo y contraseña; **segundo factor obligatorio para dueño y organizadores**; **opcional y recomendado para editores**, con reevaluación de su obligatoriedad tras el piloto; límites de intentos, recuperación de contraseña y revocación de sesiones. **Delegados (Q-39 B):** 2FA **opcional y recomendado**, con reevaluación de su obligatoriedad tras el piloto. **La política efectiva se calcula por cuenta con todos los roles vigentes:** si la cuenta tiene rol de dueño u organizador, el 2FA es obligatorio; al otorgar dueño u organizador a una cuenta sin 2FA, debe enrolarse antes de usar ese rol. Parámetros (longitud, expiración, método del 2FA) se fijan en la Fase 2. | M | CONF (Q-08, Q-39); parámetros REC | Dueño/organizador sin 2FA activado no accede al panel; un editor puede activarlo y desactivarlo según la política; 5 intentos fallidos activan el límite definido en Fase 2; tras cambiar la contraseña, las sesiones anteriores dejan de ser válidas. Un delegado sin 2FA accede a su plantilla; un delegado con rol de organizador sin 2FA no accede al rol de organizador; la política depende de todos los roles vigentes. | I, E2E |
| RF-ACC-11 | **Toda** invitación, aceptación, asignación, modificación o revocación de rol o de asignación a partido queda auditada. Los registros de asignación de rol (alta, baja, revocación, levantamiento) son de **solo anexado**: una revocación cambia el estado y la fecha, **no elimina ni altera** el historial necesario para evaluar RT-14; el retiro del rol de delegado a un dueño u organizador se audita con actor, motivo y fecha; la desactivación de una cuenta conserva el vínculo técnico (cuenta, equipo, torneo, fechas) mientras exista el torneo. **La validación jurídica de la conservación y de la anonimización de este historial sigue pendiente (PEND-JUR).** | M | CONF (Q-19, Q-45); conservación y anonimización PEND-JUR | Cada una de esas acciones genera un registro con actor, destino, torneo, antes/después y hora del servidor; 0 acciones sin registro en la suite. Revocar un rol (propio, por un organizador o por el dueño) no elimina ni altera el registro histórico; la desactivación de la cuenta tampoco. | DB |
| RF-ACC-12 | **Creación de torneo:** el organizador autorizado puede crear torneos; al crear uno queda **automáticamente asignado como organizador de ese torneo y de ninguno más**, en la misma operación atómica; la asignación se **audita**; no obtiene permisos sobre torneos ajenos ni puede crear o designar organizadores. La asignación inicial es una **operación del sistema autorizada previamente**, no una autoasignación discrecional (D-3). El dueño puede crear y asignar torneos. | M | CONF (Q-02, Q-02a; D-3) | Tras crear, el organizador ve y administra solo ese torneo; otro organizador no lo ve; si falla la asignación, el torneo no se crea; la auditoría registra creación y asignación; el creador no puede repetir la asignación sobre otro torneo. | DB, I |
| RF-ACC-13 | El **delegado** queda vinculado a **un equipo inscrito**, y **una persona solo puede ser delegada de un equipo por torneo**; puede representar equipos distintos en torneos distintos; solo administra la plantilla de ese equipo. | M | CONF (Q-02, Q-38 b) | Delegado del equipo X edita la plantilla de X (permitido) y de Y, aun en el mismo torneo (denegado); un usuario no puede ser delegado de dos equipos del mismo torneo (BD, incluso con altas concurrentes) pero sí de equipos de torneos distintos. | DB |
| RF-ACC-14 | El público consulta lo publicado **sin autenticación**; un usuario autenticado sin rol tiene los mismos permisos que el público. | M | CONF (Q-02) | Mismos resultados de lectura para anónimo y autenticado sin rol (comparación automatizada). | DB, E2E |
| RF-ACC-15 | **Anti-escalada:** ningún organizador puede otorgar el rol de dueño ni crear organizadores; nadie puede otorgarse a sí mismo roles ni asignaciones (RT-04); el alcance de una invitación nunca excede el de quien invita; un rol no se modifica por parámetros del cliente; la acumulación de roles no amplía permisos (RT-11). | M | CONF (Q-02, Q-19, Q-15) | Pruebas de abuso: manipular `rol`, `torneo` o `usuario` en la petición produce denegación en el 100 % de los casos probados. | DB, I |
| RF-ACC-16 | **Sesión y recuperación:** cierre por inactividad, cierre de sesión por el usuario, invalidación de sesiones al revocar y al cambiar contraseña, recuperación de cuenta. Tiempos y mecanismos: Fase 2. | M | REC | Tras cambiar la contraseña, las sesiones anteriores dejan de ser válidas. | I |
| RF-ACC-17 | **Levantamiento de la restricción histórica de conflicto:** un dueño activo, **distinto del interesado** y sin conflicto respecto del equipo X en el torneo T, puede levantar la restricción histórica de RT-14 de otro dueño u organizador respecto de X en T, con **motivo obligatorio** y auditoría; el levantamiento se limita a X y T, **no admite autoaprobación** y **no concede permisos adicionales**; no cubre el conflicto vigente y se anula si el rol de delegado de X se otorga de nuevo. **Prioridad S propuesta: si se difiere su implementación, todavía no existirá mecanismo para levantar conflictos históricos y la restricción permanecerá vigente.** | S | CONF (Q-45 C); prioridad S REC | Otro dueño sin conflicto levanta con motivo → el interesado puede actuar sobre X en T y solo ahí; sin motivo → rechazado; el interesado se aprueba a sí mismo → denegado; sin otro dueño → la restricción permanece; el levantamiento no cambia roles ni permisos; queda auditado. | DB, I |

### 6.2 Torneos (RF-TOR)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-TOR-01 | Crear un torneo de liga con nombre, temporada, descripción, número de vueltas (1 o 2) y reglamento inicial por defecto (RD-REG-01). Estado inicial `borrador`. | M | CONF (Q-18) | Se crea en `borrador`, no visible al público. | I, E2E |
| RF-TOR-02 | **Ciclo de vida:** `borrador` → `publicado` → `en curso` → `finalizado` → `archivado`. `en curso` ocurre automáticamente cuando el torneo está iniciado (RD-REG-02). Publicar/despublicar: dueño y organizador. **`finalizado`:** terminó la competición deportiva; el dueño y el organizador autorizado aún pueden corregir resultados con motivo obligatorio y auditoría (RD-RES-06). **`archivado`:** **cierre definitivo**; desde ese estado únicamente el dueño corrige resultados. Finalizar: organizador o dueño, con confirmación. Quién archiva, si se puede reabrir o desarchivar, qué otras operaciones se bloquean en `archivado` y su visibilidad pública: Q-40. | M | CONF (Q-09, D-5: definición de los estados); transiciones REC; archivado y reapertura COND:Q-40 | Transiciones inválidas rechazadas (p. ej. `borrador` → `finalizado`); en `finalizado` el organizador corrige; en `archivado` solo el dueño; todas las transiciones auditadas. | U, DB, I |
| RF-TOR-03 | Todo torneo tiene siempre un **reglamento vigente con versión**, que no puede eliminarse y que es el que lee el cálculo de la tabla (ver RD-REG). | M | CONF (Q-03) | Se cumple cuando pasan las pruebas ya definidas de **RD-REG-01** (3 cambios → 4 versiones consultables), **RD-REG-04** (validación de configuraciones) y **RD-REG-06** (la tabla lee el valor del reglamento del torneo). RD-REG-03 queda excluida porque depende de Q-23. | DB, U (las de RD-REG-01, 04 y 06; sin duplicar) |
| RF-TOR-04 | **Aislamiento:** los datos de un torneo (equipos inscritos, plantillas, partidos, eventos, usuarios y auditoría) no son visibles ni modificables desde otro torneo; la plataforma soporta 3–6 torneos simultáneos. | M | CONF (Q-02, Q-06) | Con 6 torneos sembrados, ninguna consulta ni escritura de un rol de A alcanza datos de B (pruebas por rol); ver RT-08. | DB, L |
| RF-TOR-05 | **Sin borrado físico** de torneos con partidos: se archivan. Solo se eliminan torneos en `borrador` sin partidos, por el dueño. | M | REC | Intentar eliminar un torneo con partidos → rechazado; archivar conserva datos y auditoría. | DB |
| RF-TOR-06 | Cada torneo publicado tiene una dirección pública estable y un código QR. | M | CONF (skill: vistas por link/QR) | La URL no cambia al renombrar el torneo; el QR abre la misma página. | E2E |
| RF-TOR-07 | Crear torneos de copa, relámpago o con grupos **no** es posible en el MVP; el diseño no debe impedirlo. | P | CONF (Q-18) | La interfaz y la API solo ofrecen `liga`. | I |

### 6.3 Equipos e inscripciones (RF-EQP)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-EQP-01 | **Equipo e inscripción son entidades distintas**: **cada torneo mantiene inscripciones independientes** (nombre mostrado, abreviatura y colores propios); plantillas y estadísticas son **independientes por torneo**. **Un equipo puede conservar su identidad entre torneos**; el vínculo administrativo que en el futuro podrá crear **solo el dueño** (RF-EQP-06) está **diferido fuera del MVP**: el modelo **no debe impedirlo** y en el MVP no existe operación ni interfaz de vinculación. El vínculo **no comparte automáticamente** plantillas, estadísticas, permisos ni información privada y **no concede acceso** a información restringida de otros torneos. Los organizadores solo administran inscripciones de sus torneos autorizados. | M | CONF (Q-11, Q-37 B) | Cambiar el nombre mostrado en A no cambia lo mostrado en B; las plantillas, estadísticas, usuarios y auditoría de B no son visibles desde A aunque exista un vínculo. | DB |
| RF-EQP-02 | Inscribir equipos: nombre único por torneo (sin distinguir mayúsculas ni tildes), abreviatura de 3–4 caracteres, dos colores. Número de equipos: mínimo técnico 2; máximo de plataforma a fijar en Fase 2 (≥ 16; propuesto 20). Inscribir o retirar un equipo del que el actor es delegado está sujeto a RT-14. | M | REC | Duplicado de nombre rechazado; inscribir equipo 17 (límite propuesto 20) permitido, equipo 21 rechazado. | I, DB |
| RF-EQP-03 | **Retiro/expulsión de un equipo** con torneo en curso: política a decidir. Opciones en Q-26. Recomendado: el equipo pasa a `retirado`; se conservan sus resultados jugados; los partidos pendientes se marcan W.O. a favor del rival o se anulan según configuración; todo auditado. Cuando se decida Q-26, los efectos automáticos del retiro sobre partidos de otros equipos se evalúan por RT-14: un actor en conflicto no puede retirar a un rival si el retiro modifica partidos de su propio equipo. | M | COND:Q-26 | Según la opción elegida; mientras no se decida, el requisito no se aprueba. | U, DB |
| RF-EQP-04 | Escudos e imágenes: fuera del MVP (el equipo se identifica por abreviatura y colores). | P | REC | La interfaz no ofrece carga de imágenes. | — |
| RF-EQP-05 | Un equipo puede tener uno o más delegados (invitados por organizador/dueño). | S | REC | Dos delegados del mismo equipo editan la plantilla; ninguno edita otro equipo. | DB |
| RF-EQP-06 | **Implementación diferida fuera del MVP (decisión del usuario).** No se construyen operaciones ni interfaces de vinculación; no se traslada al MVP ningún requisito de vínculo sin consumidor funcional. **Regla futura (Q-37 B, confirmada):** solo el dueño podrá vincular y desvincular inscripciones de torneos diferentes a una identidad común; la operación se auditará; el vínculo no se usará en ninguna política de acceso, vista, plantilla, estadística ni permiso (RT-12); el modelo no debe impedirlo. | P | CONF (Q-37 B: regla de gobernanza); implementación DIFERIDA | En el MVP la interfaz y la API no ofrecen vincular ni ver vínculos a ningún rol; el diseño no lo impide (revisión de ADR y modelo en las Fases 2 y 4). | I, D |

### 6.4 Experiencia pública (RF-PUB)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-PUB-01 | Lista pública de torneos `publicado`/`en curso`/`finalizado`; los `borrador` y `archivado` no aparecen **por ahora** (la visibilidad pública de `archivado` es la decisión Q-40 (d); no se asume un archivo histórico público, Q-11). | M | DER; `archivado` COND:Q-40 | Un torneo en `borrador` no aparece ni se accede por URL (404 indistinguible de inexistente). | E2E, DB |
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
| RF-ADM-04 | **Alertas** (a) partidos sin editor asignado dentro de las próximas 48 h y a la hora de inicio (N-12) y (b) operaciones en conflicto o rechazadas **que el servidor efectivamente recibió**, a partir de **metadatos mínimos** (OFF-20), **solo de los torneos del organizador**. Las operaciones nunca recibidas no son visibles. Canal de (a): recomendado, indicador dentro del panel; correo opcional; push/WhatsApp, posteriores. | M | (a) COND:Q-10; (b) CONF (D-9) | Un partido sin editor aparece en el panel del organizador del torneo ≤ 1 min después de entrar en la ventana; una operación rechazada aparece en el panel del organizador de ese torneo y no de otros, sin contenido de la captura. | I, E2E |
| RF-ADM-05 | Gestión de usuarios: ver, invitar, reenviar, revocar y cambiar asignaciones según la matriz §7. | M | CONF (Q-19) | Las acciones visibles coinciden con la matriz §7 para cada rol. | E2E, DB |

### 6.6 Auditoría (RF-AUD)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-AUD-01 | Cada registro contiene: actor (cuenta y rol con que actuó), hora **del servidor**, torneo, entidad, acción, valores antes/después **minimizados**, motivo (cuando aplica) y origen (en línea / sincronizado, con identificador de operación y hora del cliente). | M | CONF (Q-03, Q-19) + REC | 100 % de las mutaciones de la lista de RF-AUD-02 producen un registro con todos los campos. | DB |
| RF-AUD-02 | Se auditan: roles, invitaciones, **designación y revocación de dueños (con motivo)**, **asignación de organizador al crear torneo**, asignaciones, desasignaciones y **reasignaciones de editores**; torneo, estado del torneo y reglamento; inscripciones y retiros; altas/bajas/anonimización de jugadores, **resolución de posibles duplicados** y consentimientos; generación, ajuste y publicación de calendario; estados de partido, resultados, eventos, correcciones y W.O.; resoluciones manuales de desempate. También: **levantamientos de restricción histórica** (aprobador y motivo), **reanudaciones de partidos suspendidos** y **revocaciones parciales de roles**. | M | CONF (Q-03, Q-19, Q-02a, Q-35, Q-12, Q-36, Q-37) | Prueba de cobertura: cada operación mutante del catálogo (Fase 4) genera registro. | DB |
| RF-AUD-03 | **Inmutabilidad:** ningún rol de la aplicación (incluido el dueño y el rol de servicio de la aplicación) puede actualizar ni borrar registros de auditoría; solo anexar. Se documenta el límite frente a un administrador de base de datos. | M | REC (H-07) | Intentos de `UPDATE`/`DELETE` por los actores de prueba de §7.3 → denegados. | DB |
| RF-AUD-04 | Visibilidad por rol (ver RF-ADM-03). El registro no contiene datos personales de menores más allá de identificadores internos (T-04). | M | CONF (Q-05) | Revisión de esquema y de una muestra de 50 registros: sin nombres, fechas de nacimiento ni contactos. | DB, D |
| RF-AUD-05 | Los intentos denegados de escalada o acceso cruzado se registran en una bitácora de seguridad separada (no visible a organizadores) para el dueño. Es **distinta** de RF-AUD-06 (rechazos por conflicto de interés, obligatorios). | S | REC | Un intento de organizador de invitar organizador genera un evento de seguridad. | DB |
| RF-AUD-06 | **Rechazos y bloqueos por conflicto de interés:** se auditan los rechazos por RT-14 (vigente o histórico), los bloqueos por falta de autoridad habilitada y los intentos de levantar la restricción sin autoridad, con actor, acción, equipo, torneo, motivo y hora, sin datos personales de menores. | M | CONF (Q-44 «auditoría», Q-45) | Cada rechazo de CL-48, 49, 51, 59 y 60 genera un registro con esos campos; el registro no contiene nombres de menores. | DB |

### 6.7 PWA (RF-PWA)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RF-PWA-01 | Instalable en Android (Chrome) y iPhone (Safari, «Añadir a pantalla de inicio»), con ícono, nombre y pantalla de inicio; pantalla guía de instalación por plataforma. | M | CONF (PWA) | Instalación comprobada en 1 Android y 1 iPhone reales (iOS ≥ 16.4); lanzamiento standalone. | M |
| RF-PWA-02 | Tras la primera visita, el «cascarón» de la app y las vistas visitadas abren sin conexión con aviso de desactualizado. | M | DER (Q-17) | Con red cortada, la app abre en ≤ 3 s (dispositivo de gama media). | E2E |
| RF-PWA-03 | Las actualizaciones de la app no pierden la cola de operaciones (OFF-16). | M | DER (Q-06) | Con 5 operaciones pendientes, instalar la versión N+1 no las descarta y conservan su orden; si el formato de la cola no es compatible, la actualización se bloquea hasta sincronizar (OFF-16). | E2E |
| RF-PWA-04 | Notificaciones push: fuera del MVP. | P | CONF (Q-18) | No se solicita permiso de notificaciones. | — |

---

## 7. Matriz de permisos v2 (Q-02, Q-02a/b/c, Q-15, Q-19, Q-35)

**Leyenda:** **Sí** = permitido · **Sí\*** = permitido con la restricción indicada · **No** = denegado · **PEND** = depende de una decisión · «—» = no aplica. Todos los «suyos/asignados» se refieren a torneos o partidos **concretos**; ningún permiso se hereda entre torneos. El rol «Autenticado sin rol» se comporta como Público. **Nota (Q-44, Q-45):** en las acciones de las filas 7, 16, 22 a 29, 38 y 39, un «Sí» para dueño u organizador **excluye** los partidos y las decisiones sobre un equipo del que el actor sea o haya sido delegado en ese torneo sin restricción levantada (RT-14). Las filas 6 y 7 y la revocación inmediata (RF-ACC-07) tienen una interacción con RT-14 aún no decidida (A-15, §13.7); la matriz no se modificó por ello.

### 7.1 Matriz por acción

| # | Acción | Dueño | Organizador | Editor | Delegado | Público | Origen |
|---|---|---|---|---|---|---|---|
| 1 | Invitar/designar **organizador** | Sí | **No** | No | No | No | CONF Q-19 |
| 2 | Invitar **editor** a un torneo | Sí | Sí\* solo sus torneos | No | No | No | CONF Q-19 |
| 3 | Invitar **delegado** a un equipo de un torneo | Sí | Sí\* solo sus torneos | No | No | No | CONF Q-19 |
| 4 | Otorgar rol de **dueño** | Sí\* solo a un segundo dueño, con autenticación reforzada y auditoría | **No** | No | No | No | CONF Q-19, Q-35 |
| 5 | Revocar organizador | Sí | **No** | No | No | No | CONF Q-02c |
| 6 | Revocar editor/delegado (incluye la revocación parcial entre organizadores de roles adicionales de editor/delegado, solo en torneos comunes) | Sí | Sí\* solo sus torneos; de otro organizador solo el rol adicional, nunca su rol de organizador | No | No | No | CONF Q-02c, Q-38 d |
| 7 | Asignar, desasignar y **reasignar** editor a partido (operación atómica y auditada) | Sí | Sí\* solo partidos de sus torneos | **No** (ni a sí mismo) | No | No | CONF Q-02b, Q-36 |
| 8 | Ver usuarios y roles | Todos | Los de sus torneos | No | No | No | REC |
| 9 | Ver historial de cambios | Todo | Sus torneos | No (solo su cola) | No | No | DER Anexo A |
| 10 | Crear torneo | Sí | Sí\* queda automáticamente como organizador de ese torneo (y de ningún otro) | No | No | No | CONF Q-02, Q-02a |
| 11 | Editar torneo y reglamento **antes de iniciar** | Sí | Sí\* suyos | No | No | No | CONF Q-03 |
| 12 | Cambiar reglamento de torneo **iniciado** | Sí (con confirmación) | PEND (Q-23) | No | No | No | COND Q-23 |
| 13 | Publicar/despublicar torneo y jornadas | Sí | Sí\* suyos | No | No | No | DER Anexo A |
| 14 | Finalizar torneo (terminó la competición) | Sí | Sí\* suyos | No | No | No | REC |
| 15 | Eliminar torneo en `borrador` sin partidos | Sí | No | No | No | No | REC |
| 16 | Inscribir/retirar equipo | Sí | Sí\* suyos | No | No | No | DER |
| 17 | Editar datos de la inscripción (nombre, colores) | Sí | Sí\* suyos | No | **No** | No | CONF Q-02 |
| 18 | Ver plantilla con datos internos | Sí | Sí\* suyos | Parcial\*\* | Sí\* su equipo | No | REC |
| 19 | Alta/baja/edición de jugadores en plantilla | Sí | Sí\* suyos | **No** | Sí\* su equipo | No | CONF Q-02 |
| 20 | Registrar autorización/consentimiento (solo metadatos) y su revocación | Sí | Sí\* suyos | No | Sí\* su equipo | No | CONF Q-24 (registro); revocación REC |
| 21 | Anonimizar/eliminar un jugador (derechos del titular) | Sí | PEND (Q-42) | No | No | No | COND Q-42 |
| 22 | Generar/regenerar calendario | Sí | Sí\* suyos | No | No | No | DER Anexo A |
| 23 | Ajustar fecha, hora, sede, localía, jornada | Sí | Sí\* suyos | No | No | No | REC |
| 24 | Registrar marcador y eventos (en línea o sin conexión) | Sí | Sí\* suyos (COND Q-42) | Sí\* **solo partidos asignados** | No | No | CONF Q-02 (editor) / COND Q-42 (organizador) |
| 25 | Cambiar estado del partido (iniciar, finalizar, suspender) | Sí | Sí\* suyos (COND Q-42) | Sí\* solo asignados | No | No | DER Q-02 (editor) / COND Q-42 (organizador) |
| 26 | Declarar/revertir W.O. | Sí | Sí\* suyos (REC) | **No** | No | No | COND Q-21 |
| 27 | **Corregir** o reabrir un partido finalizado | Sí (también si el torneo está `archivado`) | Sí\* suyos mientras el torneo **no esté `archivado`**; con el torneo `archivado`: **No** | **No** | No | No | CONF Q-09, D-5 |
| 28 | Anular un evento ya confirmado | Sí | Sí\* suyos | Sí\* solo asignados y **antes de finalizar** | No | No | REC |
| 29 | Resolución manual de un desempate | Sí | Sí\* suyos (REC) | No | No | No | COND Q-22 |
| 30 | Ver torneos publicados, tablas, calendarios publicados, resultados confirmados, goleadores, tarjetas | Sí | Sí | Sí | Sí | **Sí** | CONF Q-02 |
| 31 | Ver torneo **no publicado** o jornada no publicada | Sí | Sí\* suyos | Solo sus partidos asignados | Solo su equipo y sus partidos | No | REC |
| 32 | Ver quién es el editor asignado de un partido | Sí | Sí\* suyos | Solo el propio | No | **No** | REC |
| 33 | Ver cola de operaciones y conflictos | Sí | Sí\* de sus torneos | Solo la propia | No | No | DER Q-17 |
| 34 | Exportar datos personales de un jugador | Sí | No | No | No | No | REC (PRV-08); organizador COND Q-42 |
| 35 | Revocar o degradar a un dueño | Sí\* a otro dueño, con **nueva autenticación 2FA**, **motivo obligatorio** y auditoría; **protección atómica del último dueño activo**; sin jerarquía entre dueños | **No** | No | No | No | CONF Q-35, D-4 |
| 36 | Vincular/desvincular inscripciones a una identidad común de equipo; ver el vínculo (**diferido: no existe en el MVP**) | Regla futura: solo el dueño | **No** | No | No | No | CONF Q-37 (regla futura); implementación diferida |
| 37 | Archivar un torneo (cierre definitivo) | Sí | PEND (Q-40; recomendado No) | No | No | No | COND Q-40 |
| 38 | Resolver verificación de duplicados de jugadores | Sí\* | Sí\* suyos | No | No | No | CONF Q-12, D-6; RT-14 |
| 39 | Reanudar un partido `suspendido` | Sí\* | Sí\* suyos | Sí\* solo asignados (también sin conexión, como operación pendiente) | No | No | CONF Q-46; RT-14, RT-05 |
| 40 | Levantar la restricción histórica de conflicto (RT-14) de un dueño u organizador respecto de un equipo y torneo | Sí\* otro dueño activo sin conflicto, con motivo y auditoría | No | No | No | No | CONF Q-45 |
| 41 | Cerrar un partido desde `suspendido` | No habilitada | No habilitada | No habilitada | No | No | COND Q-29; no presumida por Q-46 |

\*\* El editor ve, de los equipos de sus partidos asignados, solo nombre público/etiqueta y dorsal, sin datos internos (PRV-11).

### 7.2 Restricciones transversales

| ID | Restricción | Estado | Aceptación |
|---|---|---|---|
| RT-01 | Todo permiso se evalúa por la combinación **usuario, rol, torneo, equipo y partido** (lo que corresponda); no hay permisos globales salvo el dueño, sujeto a RT-14; los permisos no se transfieren entre torneos. | CONF Q-02, Q-15 | Auditoría de esquema: ninguna regla de acceso omite el torneo, salvo la del dueño. |
| RT-02 | La autorización se comprueba en el **servidor y en la base de datos**; ocultar un control en la interfaz no es un control de seguridad. | CONF Q-02/Q-19 | Las pruebas de permisos se ejecutan **contra la API/BD sin usar la interfaz**. |
| RT-03 | Ningún rol puede conceder un rol igual o superior al propio, salvo que el dueño designe organizadores. | CONF Q-19 | Casos de §12 CL-19. |
| RT-04 | Ningún usuario —incluido el dueño— puede invitarse, asignarse ni concederse roles o asignaciones adicionales a sí mismo, ni ampliar su ámbito; el rol lo concede otra cuenta autorizada; el servidor lo verifica y la concesión se audita. **Excepción:** la asignación inicial del organizador que crea un torneo (RF-ACC-12) es una operación del sistema autorizada previamente (Q-02a), no una autoasignación discrecional. | CONF Q-02 (prohibición de escalada), D-3 | Intento propio (interfaz y API) → denegado y registrado (RF-AUD-05); la creación de torneo asigna al creador solo a ese torneo y una sola vez. |
| RT-05 | **Conflicto de interés:** un delegado no puede actuar como editor en partidos en que juega su propio equipo. Se verifica **al asignar**, **al otorgar el rol de delegado** (las asignaciones existentes en conflicto se retiran con auditoría) y **al sincronizar** operaciones sin conexión. Convive con RT-14 (dueño u organizador con rol de delegado). | CONF Q-15 | Asignar a un delegado como editor de un partido de su equipo → rechazado; otorgar el rol de delegado a un editor asignado a partidos de ese equipo retira esas asignaciones; operación sin conexión de esa combinación → rechazada. |
| RT-06 | No se puede revocar ni degradar al **último dueño activo**, **de forma atómica incluso con operaciones simultáneas**; solo un dueño existente otorga el rol de dueño; ningún organizador concede ni revoca el rol de dueño; no hay jerarquía técnica entre dueños. | CONF Q-35, D-4 | Ver RF-ACC-09 (carrera de revocaciones mutuas). |
| RT-07 | El ámbito de una invitación nunca excede el de quien invita. | CONF Q-19 | Organizador invita con un torneo que no administra → rechazado. |
| RT-08 | **Integridad entre torneos:** toda referencia entre entidades (inscripción, plantilla, partido, evento, asignación) debe pertenecer al **mismo torneo**; no basta con claves foráneas independientes. | CONF (H-05 aceptado como insumo) | Intentos de insertar un partido de A con equipo de B, un evento con jugador de B, o una asignación de editor de B → rechazados por la base de datos. |
| RT-09 | Las credenciales de servicio con privilegios totales nunca llegan al navegador ni a la cola local. | REC (seguridad.md) | Revisión de código y de bundle en Fase 7. |
| RT-10 | Toda asignación, modificación o revocación de permisos queda auditada. | CONF Q-19 | Ver RF-ACC-11. |
| RT-11 | **Acumulación de roles:** los permisos de un usuario con varios roles son la unión de los permisos de cada rol evaluados **por separado en su propio ámbito**; ninguna combinación concede más que esa unión ni evade las restricciones de §7 (RT-05, RT-14). **Revocar un rol o una asignación adicional no elimina los permisos legítimos que procedan de otros roles vigentes.** Una persona solo es delegada de un equipo por torneo (RF-ACC-13). | CONF Q-15, Q-38 | Pruebas de matriz con usuarios de roles combinados (§7.3): ninguna acción denegada a un rol se habilita por tener otro rol en otro ámbito; revocar un rol adicional no elimina permisos legítimos de otros roles. |
| RT-12 | **Un vínculo de identidad de equipo (Q-37) no es un criterio de autorización:** ninguna política de acceso, vista ni consulta lo utiliza ni concede acceso a otro torneo. **Implementación del vínculo diferida (RF-EQP-06):** se conserva como regla de diseño; su prueba se activa cuando se construya el vínculo. | CONF Q-37 (regla de diseño) | Consultas con el rol de un organizador atravesando el vínculo devuelven 0 filas de otros torneos; revisión de todas las políticas de acceso (Fase 4). |
| RT-13 | **Autorización atómica:** la autorización de una operación se evalúa en la misma transacción que la aplica, de modo que revocaciones, reasignaciones y cambios de rol se serializan con las operaciones; ninguna operación se aplica con una autorización ya revocada y confirmada, y ninguna queda a medias. | DER (D-2, D-11, Q-36) | Pruebas de carrera (CL-41, CL-43, CL-45) contra PostgreSQL real. |
| RT-14 | **Conflicto de interés de dueño u organizador con rol de delegado.** Quien sea delegado de un equipo **no ejecuta, sobre ese equipo**: resultados, W.O., resoluciones manuales, asignaciones que generen conflicto, ajustes de calendario que lo afecten, decisiones de inscripción o retiro, ni la verificación de jugadores duplicados. **Alcance por efecto:** se evalúa sobre el conjunto de partidos que la operación **modificaría efectivamente**, incluidos sus efectos automáticos o en cascada («modificar» incluye crear, archivar, eliminar, reprogramar, mover de jornada o localía, cambiar rivales, estado, resultado, eventos y asignación de editores); si incluye algún partido de un equipo del que el actor es delegado en ese torneo, la operación se **rechaza por completo**, sea manual o automática, y una operación automática hereda la situación del actor que la dispara. **Consultar** no se restringe; **previsualizar** no persiste nada, se permite con la autorización que la operación ya tiene y no concede ninguna otra; **ejecutar** cualquier cambio efectivo queda sujeto a RT-14, y la generación y la regeneración del calendario no están exentas. **Persistencia histórica (Q-45 C):** quien haya sido delegado del equipo X en el torneo T conserva la restricción respecto de X en T al perder el rol, sea por renuncia, por revocación de otro organizador (Q-38 d) o del dueño; se evalúa sobre las asignaciones de delegado vigentes **y** las revocadas, que constan en el historial inmutable; **no se extiende a editores**, que conservan RT-05. **Levantamiento:** solo cuando **otro dueño** —distinto del interesado, activo y sin conflicto respecto de X en T— lo apruebe con **motivo obligatorio** y auditoría, limitado a X y T, **sin autoaprobación** y **sin conceder permisos adicionales**; sin ese dueño, la restricción permanece. El levantamiento no cubre el conflicto vigente y pierde efecto si el rol de delegado de X se otorga de nuevo. Se aplica en el servidor, en la misma transacción (RT-13) y al sincronizar operaciones sin conexión. Sin otro actor sin conflicto, la operación se **bloquea** hasta que exista una autoridad habilitada; no hay excepciones automáticas ni se afecta la protección del último dueño activo (RT-06). | CONF Q-38 (a), Q-44 B, Q-45 C (Q-46 para reanudar); lecturas provisionales de las 14 ambigüedades abiertas de §13.7 (A-1, A-2, A-4, A-5, A-6 y A-8…A-16; A-3 y A-7 resueltas); A-15 (interacción con revocaciones de seguridad, desasignaciones y efectos indirectos) y A-16 (reglamento y desempates) siguen abiertas y **no se resolvieron** | pgTAP por clase de acción y por combinación (organizador+delegado, dueño+delegado, organizador+editor+delegado): denegado sobre el propio equipo y permitido a otro actor sin conflicto; una operación multipartido con un partido propio se rechaza por completo; el conflicto persiste tras perder el rol (renuncia, revocación por otro organizador, revocación por el dueño); el levantamiento solo lo aprueba otro dueño sin conflicto, con motivo, auditado, sin autoaprobación y limitado a equipo y torneo; sin ese dueño la restricción permanece; el editor ex-delegado no hereda la persistencia; todo se reevalúa al sincronizar. |

### 7.3 Casos de prueba de permisos mínimos (esquema CRUD completo en Fase 4)

Actores a probar en **cada operación mutante y cada vista de datos no pública**: (1) anónimo, (2) autenticado sin rol, (3) delegado de su equipo, (4) delegado de otro equipo del mismo torneo, (5) editor asignado al partido, (6) editor del mismo torneo no asignado, (7) editor de otro torneo, (8) organizador del torneo, (9) organizador de otro torneo, (10) dueño; **adicionales (Q-15, Q-35):** (11) segundo dueño y (12) usuarios con roles combinados (organizador+editor; delegado+editor, incluidos partidos de su propio equipo; organizador+delegado, dueño+delegado y organizador+editor+delegado, con y sin restricción histórica levantada, RT-14); (13) dueño u organizador que fue delegado y perdió el rol (Q-45). Las pruebas de estado se repiten con el torneo en `finalizado` y en `archivado` (D-5). Pruebas con los roles reales de la aplicación (no superusuario). Esta es la matriz que exige la skill; la suite se escribe en la Fase 4 y se ejecuta desde el módulo 5C.

### 7.4 Roles combinados (Q-15, Q-35)

| Combinación | ¿Permitida? | Restricción | Estado |
|---|---|---|---|
| Organizador + Editor (mismo torneo) | Sí | El rol adicional lo concede otro actor con facultad (RT-04); la asignación a partidos sigue RF-ACC-06 | CONF Q-15, D-3 |
| Editor + Delegado (mismo torneo) | Sí | **El delegado no actúa como editor en partidos de su propio equipo**; se verifica al asignar, al otorgar el rol y al sincronizar (RT-05) | CONF Q-15 |
| Organizador + Delegado (mismo torneo) | Sí (Q-15) | **No ejecuta acciones deportivas sobre su propio equipo** (RT-14: resultados, W.O., resolución manual, asignaciones, ajustes de calendario, inscripción o retiro, duplicados); la restricción persiste tras perder el rol y solo la levanta otro dueño sin conflicto (RF-ACC-17) | CONF Q-38 (a), Q-44, Q-45 |
| Dueño + Delegado o Editor | Sí (el dueño es global, **sujeto a RT-14**) | Como delegado, las mismas restricciones sobre su propio equipo: no está exento; como editor, solo RT-05 | CONF Q-44 |
| Delegado de dos equipos del mismo torneo | **No** | Una persona solo es delegada de un equipo por torneo (RF-ACC-13); sí puede serlo de equipos de torneos distintos | CONF Q-38 (b) |
| Editor que fue delegado del equipo | Sí | **Sin persistencia histórica**: solo RT-05 | CONF Q-45 (extensión a editores: A) |
| Roles en torneos distintos | Sí | Los permisos no se transfieren entre torneos (RT-01) | CONF Q-15 |
| Dueño + segundo dueño | Sí | Autenticación reforzada, auditoría y protección del último dueño activo (RT-06) | CONF Q-35 |

Criterio de aceptación: con usuarios de roles combinados en la suite de permisos (§7.3), ninguna acción denegada a un rol se habilita por tener otro rol en otro ámbito (RT-11).


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
| RD-CAL-02 | **Vista previa** antes de confirmar: no se persiste nada hasta confirmar; confirmar crea todos los partidos en una transacción o ninguno. La confirmación se evalúa con RT-14 sobre todos los partidos que crea. | M | CONF (informe §7.2) | Cancelar la vista previa deja 0 filas nuevas; un fallo a mitad de la confirmación deja 0 filas. Actor en conflicto confirma → denegado; otro actor sin conflicto → permitido. | I, DB |
| RD-CAL-03 | **Orden de equipos:** el organizador elige orden manual o sorteo aleatorio; el sorteo registra su semilla para poder reproducirlo y auditarlo. | M | REC | Misma semilla → mismo calendario; la semilla consta en la auditoría. | U |
| RD-CAL-04 | Cada partido tiene fecha, hora y sede (texto libre) **opcionales**; «Por definir» es un valor válido. Zona horaria `America/Costa_Rica` (UTC−6, sin horario de verano), formato dd/mm/aaaa y 24 h. | M | DER (Q-01) | Un partido sin fecha se muestra «Por definir»; las horas se muestran en hora de Costa Rica con independencia de la zona del dispositivo. | E2E |
| RD-CAL-05 | **Advertencias de programación:** mismo equipo en dos partidos a la misma fecha y hora, y misma sede a la misma hora. Requieren confirmación del organizador; no bloquean. | S | REC | Ambas advertencias se muestran y quedan en la auditoría si se confirman. | U, E2E |
| RD-CAL-06 | **Ajustes manuales** (cambiar de jornada, localía, fecha/hora/sede): solo en partidos `programado`; cada ajuste se audita; se impide que un equipo quede con dos partidos en la misma jornada; el calendario se marca «modificado manualmente» si deja de cumplir RD-CAL-01 (a). Un ajuste que modifique varios partidos (p. ej. intercambiar rivales o mover una jornada) se rechaza por completo si alguno es de un equipo del que el actor es delegado (RT-14); los ajustes sobre partidos en que no juega su equipo no se restringen por RT-14. | M | CONF (informe §6.6) | Mover un partido provocando dos partidos del mismo equipo en la jornada → rechazado; ajustes válidos auditados. | U, I |
| RD-CAL-07 | **Regeneración:** solo si **todos** los partidos están en `programado`; antes de confirmar se listan las consecuencias (asignaciones de editor perdidas, jornadas publicadas retiradas, ajustes manuales perdidos); los partidos anteriores se archivan (no se pierden en la auditoría). La generación inicial y la regeneración modifican los partidos de todos los equipos, **incluido el del actor si es delegado**: no están exentas de RT-14; un dueño u organizador que sea delegado de cualquier equipo inscrito no puede confirmarlas, las confirma otro dueño u organizador sin conflicto y, si no existe, la operación queda bloqueada y auditada (RF-AUD-06). | M | REC (C-14) | Con un partido `en_juego`, regenerar → rechazado. Con asignaciones, el aviso las enumera y tras confirmar quedan sin editor. Actor en conflicto confirma generación o regeneración → denegada con 0 filas cambiadas. | I |
| RD-CAL-08 | **Publicación por jornada:** estados `borrador` y `publicada`. Despublicar una jornada exige motivo y queda auditado; una jornada con resultados confirmados no se despublica salvo el dueño. La publicación no modifica partidos y no se restringe por RT-14 en esta versión (A-8, decisión posterior). | M | REC (H-12) | Un partido de una jornada `borrador` no aparece en el sitio público aunque el torneo esté publicado. | E2E, DB |
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
| RD-TAB-03 | Solo cuentan los partidos `finalizado` y `wo` con resultado **confirmado por el servidor**; `programado` y `en_juego` no cuentan. **El tratamiento de `suspendido` (y de `aplazado` y `cancelado`) en la tabla lo decide Q-29**; hasta entonces su exclusión es provisional y no está aprobada. | M | DER (Q-17); `suspendido` COND:Q-29 | Una operación local pendiente no altera la tabla pública. | I |
| RD-TAB-04 | **Orden por defecto:** (1) Puntos; (2) Diferencia de goles; (3) Goles a favor; (4) Enfrentamiento directo; (5) Resolución manual. Los puntos son siempre el primer criterio y no son configurables en orden. Difiere del informe (que ponía la mini-tabla en 2.º lugar): prevalece la decisión. | M | CONF (Q-03) | N-02 reformulado (ver RD-TAB-07). | U |
| RD-TAB-05 | **Configuración** del orden y selección de criterios por torneo **antes de iniciar** (RD-REG-02). Catálogo recomendado para el MVP: diferencia de goles, goles a favor, goles en contra (menos), victorias, enfrentamiento directo; «resolución manual» fija al final. Fair play queda fuera. | M | CONF (configurable); catálogo COND:Q-28 | Reordenar criterios antes de iniciar cambia la tabla de forma predecible. | U |
| RD-TAB-06 | **Algoritmo determinista y terminante.** (1) Agrupar por puntos. (2) Para cada grupo empatado (≥ 2) aplicar los criterios en el orden configurado; los criterios globales comparan la tabla general; el enfrentamiento directo considera **solo los partidos entre los equipos del grupo empatado vigente** (ambas vueltas): puntos, luego diferencia y goles a favor de esa mini-tabla (detalle Q-27). (3) Si un criterio separa el grupo en subgrupos, cada subgrupo con empate restante continúa con el criterio siguiente; el enfrentamiento directo se recalcula para el subgrupo restante. (4) Agotados los criterios automáticos queda «empate sin resolver». Cada iteración reduce el tamaño del grupo o avanza de criterio: no hay bucles. | M | CONF (orden) + COND:Q-27 (semántica) | Propiedades: termina siempre; mismo resultado con cualquier orden de inserción de partidos; ninguna posición asignada dos veces a equipos separados por un criterio. | U (propiedades) |
| RD-TAB-07 | **Casos N-02 y N-03 reformulados para el orden de Q-03:** (a) dos equipos con igual puntaje: decide diferencia de goles; si igual, goles a favor; si igual, el resultado entre ambos; si igual, resolución manual. (b) Tres equipos A>B, B>C, C>A, mismos puntos, **misma DG y mismos GF**: la mini-tabla es idéntica; el empate queda sin resolver, sin bucle. (c) Mismo ciclo pero con DG distinta: resuelve la DG. (d) Tres empatados donde el enfrentamiento directo separa a uno y deja a dos: el par restante se recalcula con su propio partido. (e) Cuatro equipos con un 1G-1E-1P cada uno (analogía «círculo de la muerte»). | M | DER (Q-03) | Los cinco casos tienen fixture y resultado esperado documentado antes de 5F. | U |
| RD-TAB-08 | **Empate sin resolver:** se muestran con la misma posición compartida (p. ej. «3»), una marca «Empate sin resolver» y orden estable alfabético que **no** implica ventaja. | M | REC | La interfaz pública muestra la marca; el orden alfabético no se presenta como desempate. | E2E |
| RD-TAB-09 | **Resolución manual:** quien la ejecuta, con qué motivo y cómo se publica se define en Q-22. Recomendado: organizador del torneo o dueño; define el orden del grupo empatado; motivo obligatorio; etiqueta pública «Resolución manual de la organización»; reversible; queda **obsoleta** (vuelve a «sin resolver» con aviso) si cambian los resultados que determinaban ese empate. No la ejecuta un dueño u organizador cuyo equipo pertenece al grupo empatado (RT-14). | M | COND:Q-22 | Ejecutar, revertir y volver obsoleta una resolución con una corrección posterior; todo auditado. | U, I |
| RD-TAB-10 | **Una sola fuente de verdad** para calcular la tabla (H-10): la decisión del lugar de cálculo se toma en la Fase 2; toda otra vista la consume. Si hubiera dos implementaciones, se exige prueba de equivalencia. | M | CONF (H-10 aceptado) | ADR aprobado; si hay dos, equivalencia sobre ≥ 10 fixtures. | D, U |
| RD-TAB-11 | **Recalculo** automático al confirmar, corregir o anular resultados o eventos de gol (N-07); la tabla pública refleja el cambio según RF-PUB-09. | M | CONF (informe N-07) | Corregir un resultado de una jornada anterior cambia posiciones y deja registro. | I, E2E |
| RD-TAB-12 | Rendimiento: calcular la tabla de un torneo de 16 equipos a doble vuelta (240 partidos) en ≤ 200 ms en el servidor (p95, propuesto). | S | REC | Medición en staging. | L |

### 8.5 W.O. (walkover)

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-WO-01 | W.O. **configurable por torneo**: activado o no, y marcador de adjudicación; predeterminado **3-0** a favor del equipo presente. | M | CONF (Q-03) | Configuración por defecto 3-0; cambiarla antes de iniciar surte efecto en W.O. nuevos. | U, I |
| RD-WO-02 | **Declaración:** el partido pasa de `programado` a `wo` indicando el equipo ausente; motivo obligatorio. Lo declaran el organizador del torneo y el dueño; el editor no (puede dejar una observación). Un organizador o dueño que sea delegado de uno de los dos equipos no declara el W.O. (RT-14); si no hay otro actor, queda bloqueado. | M | COND:Q-21 (recomendado) | Editor intenta declarar → denegado. | DB, I |
| RD-WO-03 | **Efectos:** resultado = marcador W.O.; suma PJ, puntos (3/0) y GF/GC con ese marcador; **no genera eventos, goleadores ni tarjetas**; se identifica como «W.O.» en las vistas públicas. | M | CONF (Q-03); GF/GC por defecto DER, COND:Q-21 | Tras un W.O., el ranking de goleadores y tarjetas no cambia y la tabla sí. | U, I |
| RD-WO-04 | **Ausencia de ambos equipos** (doble W.O.): recomendado — ambos 0 puntos, PJ +1 cada uno, marcador 0-0 sin GF/GC, etiqueta «Doble W.O.». Sanciones adicionales (descuento de puntos, expulsión por W.O. reiterados): fuera del MVP; el sistema muestra el conteo de W.O. por equipo. | M (doble W.O.) / P (sanciones) | COND:Q-21 | Fixtures de W.O. simple y doble. | U |
| RD-WO-05 | **Auditoría:** actor, motivo, configuración aplicada, hora; reversible por el organizador o el dueño con motivo (`wo` → `programado`). La reversión está sujeta a RT-14 en los mismos términos. | M | CONF (Q-03 «auditadas») | Todo W.O. y su reversión tienen registro. | DB |
| RD-WO-06 | Un W.O. solo puede declararse sobre un partido **sin eventos activos** y en `programado`; si ya empezó debe corregirse por RD-RES-06. | M | REC | Intento sobre un partido con eventos → rechazado. | I |
| RD-WO-07 | **Consistencia marcador–eventos:** la regla «goles de jugadores = marcador» (RD-EVT-04) **excluye** los partidos `wo`. | M | DER (T-05) | Un `wo` 3-0 sin eventos es válido. | U, DB |

### 8.6 Jugadores y plantillas

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-JUG-01 | Datos mínimos de un jugador **por inscripción**: nombre y apellidos (internos), nombre público (opcional; ver PRV-02), dorsal (opcional), posición (opcional), indicador «es menor de edad» (sí/no; ver PRV-04 y Q-20). Sin documento, teléfono, correo, dirección ni foto. | M | CONF (Q-05 minimización) + COND:Q-20 | La interfaz y el esquema no admiten otros campos personales. | D, I |
| RD-JUG-02 | **Detección y verificación de duplicados.** Al inscribir un jugador, el sistema normaliza el nombre (mayúsculas/minúsculas, tildes, espacios y signos; reglas exactas en Fase 4) y detecta **posibles duplicados dentro del torneo**. **Toda inscripción con posible duplicado queda `pendiente de verificación`: el jugador no queda habilitado** (sin eventos, sin figurar en plantillas públicas ni en las preparadas para sin conexión) **hasta resolver la coincidencia.** El **organizador** verifica si es la misma persona y ve ambos registros. Si son **personas distintas (homónimos)**, puede aprobar la inscripción, que queda marcada para no volver a avisar por esa pareja. Si **se confirma que ya pertenece a otro equipo del torneo**, la inscripción duplicada se **rechaza**. **La resolución se audita** (actor, decisión, fecha). El **delegado no puede consultar datos de otros equipos**: solo ve que su alta «requiere verificación». No se recopilan documentos de identidad en el MVP. Si alguno de los dos registros pertenece a un equipo del que el verificador es delegado, no puede verificar (RT-14). | M | CONF (Q-12; D-6) | (a) cualquier coincidencia (mismo u otro equipo) → pendiente y no habilitada; (b) el delegado no recibe datos ajenos; (c) «misma persona» con registro activo en otro equipo → inscripción duplicada rechazada y auditada; (d) «homónimo» → habilitada y sin nuevo aviso por esa pareja; (e) mientras está pendiente no puede recibir eventos ni aparece en listas públicas. | DB, I, E2E |
| RD-JUG-03 | Dorsal único por equipo inscrito cuando se indica. | M | REC | Duplicado rechazado. | DB |
| RD-JUG-04 | **Bajas sin borrar** si el jugador tiene eventos: pasa a `baja`; sus eventos históricos se conservan; no puede recibir eventos nuevos. | M | REC | Un jugador con 2 goles dado de baja sigue en la tabla de goleadores; un gol nuevo para él se rechaza. | DB |
| RD-JUG-05 | **Cierre de plantilla, mínimo/máximo de jugadores y elegibilidad por categoría/edad:** Q-31. Recomendado: fecha de cierre configurable por torneo (después del cierre solo el organizador modifica), sin mínimos/máximos obligatorios en el MVP y sin elegibilidad por edad. | M (cierre) | COND:Q-31 | Después del cierre, el delegado no puede editar; el organizador sí, con auditoría. | I |
| RD-JUG-06 | El delegado edita **solo** la plantilla de su equipo; organizador y dueño la de equipos de su ámbito; el editor no la edita. Cambios auditados. | M | CONF (Q-02) | El delegado edita la plantilla de su equipo (permitido) y la de otro equipo del torneo (denegado); el organizador del torneo, permitido; el editor, denegado; cada cambio queda auditado (matriz §7.1 filas 18–19, CL-20). | DB |
| RD-JUG-07 | Alta masiva (importar lista): fuera del MVP. | P | REC | — | — |
| RD-JUG-08 | **Garantía bajo concurrencia:** una misma identidad confirmada no puede estar activa en dos equipos del mismo torneo; ante dos altas simultáneas de nombres equivalentes en equipos distintos, **a lo sumo una queda activa sin verificación** y la otra queda pendiente. La garantía la impone la base de datos, no solo la aplicación. | M | CONF (Q-12) | Prueba de carrera: dos altas concurrentes → una activa y una pendiente de verificación; ninguna combinación deja dos activas sin decisión del organizador. | DB, I |
| RD-JUG-09 | **Sin documentos de identidad en el MVP.** Las limitaciones de identificar por nombre se documentan en la ayuda del organizador y en el manual: **falsos negativos** (escrituras distintas, apodos, errores) y **falsos positivos** (homónimos); la verificación del organizador es la garantía final. | M | CONF (Q-12) | Texto de ayuda presente y revisado; el esquema no contiene campo de documento. | D, I |
| RD-JUG-10 | **Traspasos dentro del torneo:** el efecto de una baja en un equipo sobre una nueva alta de la misma persona en otro equipo del mismo torneo no está decidido (Q-41). Mientras no se decida, un jugador con baja no se reinscribe en otro equipo del mismo torneo. | M | COND:Q-41 | Alta de un jugador con baja en otro equipo del torneo → pendiente de verificación. | DB, I |

### 8.7 Partidos, estados y resultados

**Estados del MVP:** `programado`, `en_juego`, `finalizado`, `wo`, `suspendido`. **«Aplazado» y «cancelado» no son estados del MVP hasta decidir Q-29**; la reprogramación de fecha (RD-RES-09) es una operación independiente que no cambia el estado.

| De → A | Quién | Condición |
|---|---|---|
| `programado` → `en_juego` | editor asignado, dueño; organizador según Q-42 (a) | — |
| `programado` → `finalizado` | ídem | captura directa del resultado final (necesaria para captura tardía o sin conexión) |
| `programado` → `wo` | organizador, dueño | RD-WO-02 |
| `en_juego` → `finalizado` | editor asignado, dueño; organizador según Q-42 (a) | RD-RES-03 |
| `en_juego` → `suspendido` | editor asignado, dueño; organizador según Q-42 (a) | RD-RES-10 |
| `suspendido` → `en_juego` | editor asignado, organizador autorizado, dueño (Q-46) | RD-RES-13; sujeto a RT-14 y RT-05 |
| `suspendido` → `finalizado` | **no habilitada** (no presumida por Q-46) | RD-RES-12 (Q-29) |
| `finalizado` → corrección (sigue `finalizado`) | organizador, dueño | RD-RES-06 |
| `finalizado` → `en_juego` (reabrir) | organizador, dueño | motivo |
| `wo` → `programado` | organizador, dueño | RD-WO-05 |

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| RD-RES-01 | Solo son válidas las transiciones de la tabla; cualquier otra se rechaza. | M | REC (H-08) | Tabla de transiciones probada completa (válidas e inválidas). Incluye la reanudación y la transición no habilitada `suspendido` → `finalizado`. | U, DB |
| RD-RES-02 | Marcador: enteros ≥ 0 con tope técnico definido en Fase 4 (propuesto 99). | M | REC | Negativos y no enteros rechazados. | U, DB |
| RD-RES-03 | **Finalizar** exige marcador de ambos equipos y una confirmación explícita con resumen («Local 2 – 1 Visitante; 3 goles registrados; 0 sin autor»). | M | REC | No existe `finalizado` con marcador nulo. | DB, E2E |
| RD-RES-04 | **Concurrencia (N-06):** toda modificación de marcador/estado envía la versión conocida; si el servidor tiene otra, rechaza con un código de **conflicto de aplicación** propio (no un error de base de datos) y devuelve la versión actual; no sobrescribe. | M | CONF (informe N-06, Q-06) | Dos sesiones guardan sobre la misma versión: la segunda recibe conflicto y los datos no cambian. | I, DB |
| RD-RES-05 | **Idempotencia:** ver OFF-06. | M | CONF (Q-06) | Enviar la misma operación 1, 2 y 10 veces (secuencial y simultánea) produce 1 efecto, 1 registro de auditoría y la misma respuesta (OFF-06). | I |
| RD-RES-06 | **Corrección de resultados finalizados (N-07):** el **dueño** y el **organizador del torneo** pueden corregir marcador y eventos de un partido finalizado **sin plazo límite**, en **una sola operación atómica**, con **motivo obligatorio** (≥ 10 caracteres, valor propuesto REC), historial del resultado anterior y el nuevo, usuario responsable y fecha del servidor, y **recálculo consistente** de las estadísticas afectadas (tabla, goleadores, tarjetas). **Mientras el torneo esté `finalizado`** dueño y organizador pueden corregir; **cuando esté `archivado` (cierre definitivo)**, **solo el dueño** corrige (D-5). Los editores **no** pueden reabrir ni corregir partidos finalizados. En el MVP, las disputas se gestionan con correcciones justificadas, sin flujo formal de reclamaciones. **Conflicto (RT-14):** si el dueño o el organizador es delegado de un equipo del partido, no corrige; la corrección la hace otro organizador o dueño sin conflicto y, si no existe (p. ej. dueño único en un torneo `archivado`), queda **bloqueada** hasta que exista una autoridad habilitada. | M | CONF (Q-09; D-5) | Corregir no deja un estado intermedio público; el historial muestra antes y después; editor intenta corregir → denegado; **organizador en torneo `finalizado` → permitido; en torneo `archivado` → denegado; dueño en `archivado` → permitido con motivo**; tabla y goleadores recalculados. Actor en conflicto sobre un partido de su equipo → denegado; sin otro actor sin conflicto → bloqueo auditado. | I, DB, E2E |
| RD-RES-07 | **Publicación automática:** cuando el servidor acepta un resultado y la jornada está visible, se publica sin aprobación adicional del organizador. Antes de aceptar, el servidor verifica autenticación, permisos vigentes, asignación al partido, integridad y conflictos de versión. Lo capturado sin conexión **no es oficial ni público** hasta esa validación. Toda corrección posterior conserva motivo y auditoría (RD-RES-06). | M | CONF (Q-32) | Resultado aceptado en jornada visible → visible según RF-PUB-09 sin intervención; resultado en jornada no publicada → no visible; resultado rechazado o en conflicto → nunca visible. | I, E2E |
| RD-RES-08 | Con el partido `en_juego`, el público ve «En juego» y el último marcador confirmado, **sin tiempo real**. | S | REC (Q-04 excluye en vivo) | Se actualiza al refrescar o en el ciclo de RF-PUB-09. | E2E |
| RD-RES-09 | **Reprogramación:** cambiar fecha/hora/sede no cambia el estado y deja historial; un partido reprogramado conserva su jornada salvo ajuste manual. Reprogramar un partido del propio equipo está sujeto a RT-14. | M | REC | El historial muestra fecha anterior y nueva. | I |
| RD-RES-10 | **Suspender un partido.** La transición `en_juego` → `suspendido` cambia **solo el estado** (no modifica el marcador ni los eventos). La ejecuta quien ya puede registrar resultados de ese partido: el **editor asignado** (también sin conexión, como cualquier cambio de estado capturado, OFF-01) y el **dueño**; la facultad del **organizador** queda sujeta a Q-42 (a); se aplican RT-14 y RT-05. Quien no tenga autorización vigente es denegado. El cambio se audita (actor, hora del servidor, origen) y solo es público cuando el servidor lo confirmó y la jornada es visible (RD-RES-07). Mientras dure la suspensión rige RD-EVT-12. Reanudar, cerrar desde `suspendido` y los estados pendientes de Q-29 están en RD-RES-13 y RD-RES-12. | M | REC (la transición pertenece a la máquina de estados de RD-RES-01); controles CONF (Q-02, Q-17, Q-32, Q-34); suspensión por el organizador COND:Q-42 | (1) El editor asignado suspende un partido `en_juego` y el estado pasa a `suspendido` con marcador y eventos intactos. (2) Editor no asignado, editor de otro torneo, delegado y público → denegado. (3) La suspensión capturada sin conexión se sincroniza con OFF-22; si el editor perdió la asignación → rechazada (CL-47). (4) La auditoría registra actor, hora y origen. (5) Antes de la confirmación del servidor el público no ve `suspendido`. (6) Suspender un partido `programado` o `finalizado` → rechazado (RD-RES-01). | U, DB, I, E2E |
| RD-RES-11 | Cada cambio de marcador o estado registra su origen: en línea o sincronizado, identificador de operación y hora del cliente (OFF-13). | M | DER (Q-17) | Auditoría muestra el origen. | DB |
| RD-RES-12 | **Cierre desde `suspendido`, `aplazado`, `cancelado` y efectos sobre la tabla (Q-29).** (a) **Cerrar** un partido desde `suspendido` (`suspendido` → `finalizado`) **no está aprobado**: Q-46 no lo presume y la transición se rechaza para todos los roles hasta decidir; (b) `aplazado` y `cancelado` no son estados del MVP hasta decidir Q-29: asignarlos se rechaza (RD-RES-01); la reprogramación de fecha (RD-RES-09) es una operación independiente que no cambia el estado; (c) el efecto de `suspendido`, `aplazado` y `cancelado` en la tabla y el marcador, y la obligatoriedad de un motivo al suspender, reanudar o cerrar, se deciden en Q-29. La reanudación y los eventos posteriores **no dependen de Q-29** (RD-RES-13, RD-EVT-12). Una reanudación seguida de una finalización es una secuencia de dos operaciones autorizadas por separado (A-9). | M | COND:Q-29 | Con Q-29 abierta: `suspendido` → `finalizado` se rechaza para dueño, organizador, editor asignado y cualquier otro; establecer `aplazado` o `cancelado` se rechaza; la reanudación (RD-RES-13) sí funciona. | U, DB |
| RD-RES-13 | **Reanudar un partido suspendido.** La transición `suspendido` → `en_juego` la ejecutan el **editor asignado**, el **organizador autorizado del torneo** y el **dueño**, sujetos a permisos vigentes, a la asignación correspondiente y a las restricciones por conflicto de interés de Q-44 y Q-45 (RT-14) y de RT-05. El editor puede registrarla **sin conexión**: se muestra como **operación local pendiente** (OFF-04), **no modifica el estado oficial publicado**, se somete a la validación de permisos y versión al sincronizar (OFF-22) y puede **rechazarse** si pierde autorización o existe conflicto. Para organizador y dueño la reanudación sin conexión no está confirmada: **solo en línea** hasta decisión posterior (A-10). No concede permiso a delegado, público ni editor no asignado. Un motivo, si se captura, se audita; su obligatoriedad no está decidida (Q-29). **No equivale a cerrar el partido** (RD-RES-12). | M | CONF (Q-46 B) | (1) El editor asignado reanuda en línea → `en_juego`. (2) Organizador del torneo y dueño reanudan; organizador de otro torneo, editor no asignado o de otro torneo, delegado y público → denegado. (3) Actor en conflicto (RT-14, Q-45) o delegado-editor de su equipo (RT-05) → denegado. (4) Sin conexión: el editor la registra como pendiente, el público sigue viendo `suspendido`; al sincronizar, válida → `en_juego` oficial y visible; sin asignación vigente → rechazada; versión cambiada (p. ej. partido finalizado) → en conflicto. (5) Queda auditada con actor, hora y origen. | U, DB, I, E2E |

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
| RD-EVT-12 | Mientras un partido esté `suspendido` **no se admiten goles, tarjetas ni otros eventos nuevos**, en línea ni sincronizados; primero debe reanudarse (RD-RES-13). **Lectura estricta (DER, a confirmar, A-11):** tampoco se admiten, durante la suspensión, cambios directos de marcador ni anulaciones de eventos. | M | CONF (Q-46 A); lectura estricta DER | Evento nuevo con el partido `suspendido` → rechazado; evento recibido al sincronizar con estado oficial `suspendido` y sin reanudación aceptada previa en su secuencia → rechazado (en conflicto); evento posterior a una reanudación aceptada de la misma secuencia → aceptado; la interfaz local no permite capturar eventos mientras el estado local sea `suspendido`. | U, DB, I |

---

## 9. Captura sin conexión (Q-06, Q-17)

### 9.1 Alcance [CONF/DER]

- **Dentro:** capturar **marcador, eventos y cambios de estado del partido** sin conexión, guardarlos en el dispositivo y sincronizarlos al recuperar conexión; consultar los datos preparados de los partidos del usuario. [CONF Q-17]
- **Fuera (requieren conexión):** crear/editar torneos, equipos, jugadores, reglamento, calendario, usuarios y roles, correcciones de partidos finalizados, resolución manual. Los controles aparecen deshabilitados con explicación. [DER de Q-17]
- Quienes pueden capturar sin conexión son quienes pueden registrar resultados de ese partido (editor asignado; organizador/dueño en sus torneos). Todo permiso se valida de nuevo en el servidor al sincronizar (OFF-22). Esa capacidad excluye los partidos del propio equipo de un dueño u organizador con rol de delegado (RT-14). La **reanudación** sin conexión solo está confirmada para el editor asignado (RD-RES-13).

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
| OFF-01 | Capturar sin conexión: marcador, eventos (RD-EVT-01), iniciar, suspender, **reanudar** y finalizar el partido. | M | CONF (Q-17) | Con red cortada se completa un partido de punta a punta (inicio, 3 goles, 2 tarjetas, final) sin errores. | E2E |
| OFF-02 | **Preparación del dispositivo:** con conexión se descargan los **partidos asignados de los próximos 7 días y los partidos en juego**, las plantillas de ambos equipos limitadas a etiqueta/nombre público y dorsal, y el resumen del reglamento. La interfaz indica «Listo para trabajar sin conexión — preparado hh:mm» o «Sin preparar» y muestra una **advertencia cuando los datos llevan más de 24 horas sin actualizarse**. Adición REC que no altera la ventana confirmada: el editor puede preparar manualmente un partido asignado fuera de esa ventana (p. ej. suspendido o atrasado). Los partidos del propio equipo de un dueño u organizador con rol de delegado no se preparan para captura (RT-14). Un partido `suspendido` fuera de la ventana confirmada requiere preparación manual (A-13). | M | CONF (Q-34 parámetros 1 y 2); preparación manual REC | Tras preparar y activar modo avión, el editor abre sus partidos y captura; un partido no preparado muestra el aviso; datos de > 24 h muestran la advertencia. | E2E |
| OFF-03 | **Cola local persistente**: sobrevive a cerrar la app, a un reinicio del dispositivo y a perder la pestaña. Cada operación tiene identificador único generado en el cliente, número de secuencia por partido, tipo, datos, versión base conocida, dispositivo, usuario y hora local. | M | CONF (Q-06, Q-17) | Cerrar a la fuerza la app con 5 operaciones pendientes y reabrirla: las 5 siguen ahí, en orden. | E2E |
| OFF-04 | **Diferenciación visual** de pendiente / enviando / confirmada / en conflicto / rechazada, por operación y con un resumen por partido («2 pendientes»), usando texto e icono además de color. El marcador local se muestra junto al confirmado cuando difieren. | M | CONF (Q-06, Q-17) | Revisión manual + axe: no se depende solo del color. | E2E, A |
| OFF-05 | **Disparadores de sincronización:** al recuperar conexión con la app abierta, al abrir/volver a la app, por botón «Sincronizar ahora» y reintento periódico con espera creciente. **No** se depende de sincronización en segundo plano (no existe en iOS). Indicador permanente del número de pendientes. | M | DER (§2.1 informe) | En iPhone real, con la app en primer plano, la cola se sincroniza al volver la red; con la app cerrada no se promete envío. | E2E, M |
| OFF-06 | **Idempotencia en el servidor:** cada operación se aplica **como máximo una vez**; reenviar el mismo identificador (también concurrentemente) devuelve el resultado original sin repetir efectos ni registros de auditoría. | M | CONF (Q-06, N-13) | Enviar la misma operación 1, 2 y 10 veces (secuencial y simultánea): 1 efecto, 1 registro, misma respuesta. | I, DB |
| OFF-07 | **Orden:** las operaciones de un mismo partido se aplican en su orden de secuencia; si una falla, las siguientes de **ese partido** quedan «bloqueadas por operación anterior» (siguen `pendiente`) hasta resolverla; las de otros partidos continúan. Las operaciones que dependan de una rechazada (p. ej. los eventos posteriores a una reanudación) no se aplican (OFF-25). | M | REC | Falla la op 3 de 6: 4–6 no se aplican y se muestra la causa; otras pruebas en otro partido sí. | I |
| OFF-08 | **Atomicidad:** cada operación se aplica completa o no se aplica; una interrupción de la red durante el envío no deja estados parciales. | M | REC | Corte simulado en mitad de un lote: al reanudar, cada operación está en un solo estado coherente. | I |
| OFF-09 | **Política de conflictos** (tabla §9.4). Regla general: **lo confirmado por el servidor nunca se sobrescribe en silencio** y **ninguna captura local se pierde en silencio**. | M | CONF (Q-06) + política REC | Cada fila de §9.4 tiene una prueba automatizada. | I, E2E |
| OFF-10 | **Nunca descarte silencioso:** las operaciones en conflicto o rechazadas se conservan localmente hasta que el usuario (a) las reintente cuando proceda, (b) las descarte con confirmación o (c) use la exportación limitada de OFF-24. **Retención máxima: 30 días**, sujeta a privacidad y retención (PRV-10). La app **informa el plazo al detectar el conflicto o el rechazo**; emite **avisos previos al vencimiento cuando sea técnicamente posible** (propuesto: 7 días y 1 día antes, REC); al vencer aplica **eliminación controlada** y deja una **constancia mínima sin datos personales protegidos** («N operaciones eliminadas por vencimiento», fecha) cuando corresponda y sea viable. **No se garantizan avisos ni conservación** si el navegador elimina el almacenamiento local o el dispositivo permanece desconectado (OFF-17). **No se garantiza la recuperación de todas las capturas rechazadas** (RK-15, riesgo aceptado): el MVP no ofrece recuperación excepcional. | M | CONF (Q-34 parámetro 3; D-7); avisos REC | No existe ruta de código que borre una operación rechazada sin acción explícita del usuario o sin el plazo informado y la constancia posterior; prueba con reloj simulado a 23, 29 y 31 días; el plazo aparece al detectar el conflicto. | I, E2E |
| OFF-11 | **Revocación y desasignación:** al sincronizar, las operaciones de un usuario desasignado, reasignado o sin rol **no se aplican** y se rechazan con motivo claro, **aunque se hayan capturado legítimamente antes** (D-11); si se revocó solo la asignación o el rol: se conservan `rechazada` (OFF-10) y solo se puede usar la **exportación limitada de OFF-24**. Si se **desactivó la cuenta**: al primer contacto con el servidor la app cierra la sesión, bloquea el acceso a los datos locales, ofrece **una sola vez** la exportación limitada de OFF-24 y **elimina de forma controlada** los datos locales (tras exportar o confirmar, nunca antes de mostrar el aviso). El cliente deja de enviar las operaciones de un usuario al saber que perdió autorización. Una PWA no puede borrar datos de un dispositivo que nunca se conecta (riesgo residual, OFF-12). | M | CONF (T-03, Q-02c, Q-34 parámetro 5; D-11) | Pruebas con desasignación, con rol revocado y con cuenta desactivada a mitad de captura: nada se aplica ni se publica, el resumen exportado cumple OFF-24 y los datos locales quedan eliminados. | I, E2E |
| OFF-12 | **Datos y sesión locales:** solo los datos mínimos necesarios (OFF-02); sin datos internos de jugadores (PRV-11); cerrar sesión con pendientes exige confirmación explícita y borra los datos locales tras sincronizar o descartar; pérdida del dispositivo: el dueño/organizador revoca la cuenta (el servidor deja de aceptar); **una PWA no puede borrar datos a distancia** — se documenta el riesgo residual. El cifrado local se evalúa en Fase 2 y no se promete. | M | CONF (Q-17) + REC | Inspección del almacenamiento local en pruebas: ningún campo de la lista «nunca público»/interno. | I, D |
| OFF-13 | **Relojes:** el minuto del partido lo ingresa o deriva el usuario; las horas del dispositivo son informativas; el orden se basa en secuencia, no en reloj; el servidor registra su propia hora de recepción; si el reloj del dispositivo difiere > 5 min del servidor al sincronizar, se avisa y se registra. | M | REC | Reloj adelantado 2 h: el orden y la tabla no cambian. | I |
| OFF-14 | **Duplicados por la interfaz:** protección contra doble toque (espera ≤ 500 ms), «Deshacer» durante 10 s tras cada evento, aviso de «posible duplicado» (mismo tipo, jugador y minuto en ≤ 60 s). Un duplicado por contenido con identificador distinto **no** se rechaza automáticamente (podría ser legítimo). | M | REC | Doble toque rápido genera 1 evento; el aviso aparece. | E2E |
| OFF-15 | **Lo local jamás es público:** el público solo ve operaciones `confirmada` de partidos visibles (§9.6). | M | CONF (Q-17) | Una operación `pendiente`, `enviando`, `en conflicto` o `rechazada` nunca aparece en las vistas públicas; solo las `confirmada` de partidos visibles (matriz de §9.6, 12 combinaciones). | I, E2E |
| OFF-16 | **Actualización de la app con cola pendiente:** una nueva versión no descarta la cola; el formato de la cola tiene versión y se migra; si no es compatible, se bloquea la actualización hasta sincronizar. | M | REC | Pruebas versión N → N+1 con 5 operaciones pendientes. | E2E |
| OFF-17 | **Almacenamiento local no garantizado:** la interfaz **advierte** (al preparar el dispositivo y junto al indicador de pendientes) que el navegador puede eliminar los datos locales y que **no se conservan indefinidamente**; se solicita almacenamiento persistente y se avisa si no se concede; no se promete conservación. Comportamiento de iOS: verificar en dispositivo real en Fase 2. | M | CONF (Q-34 reglas adicionales) | La advertencia es visible y no se presenta ninguna promesa de conservación; aviso cuando no se concede el almacenamiento persistente. | E2E, M |
| OFF-18 | Consulta sin conexión: el editor ve sus partidos preparados; el público ve la última versión visitada con hora (RF-PUB-06). | M | CONF (Q-17) | Con la red cortada, el editor abre sus partidos preparados y la vista pública muestra la última versión visitada con su hora; las acciones administrativas aparecen deshabilitadas con explicación. | E2E |
| OFF-19 | **Rendimiento de sincronización:** 50 operaciones pendientes se sincronizan en ≤ 15 s con perfil de red 3G lenta (propuesto). | S | REC | Medición con red limitada. | I |
| OFF-20 | **Registro de incidencias en el servidor:** el servidor registra **metadatos mínimos** (identificador de operación, usuario, partido, tipo, motivo y hora) de las operaciones **rechazadas o en conflicto que efectivamente recibe**, y contadores agregados sin datos personales para vigilar el piloto. **Los organizadores solo consultan las incidencias de sus torneos**; no se exponen capturas privadas ni datos personales protegidos; los identificadores técnicos están sujetos a privacidad y retención (PRV-10). Las operaciones **nunca recibidas** permanecen solo en el dispositivo (OFF-10). **El registro no autoriza publicar** operaciones rechazadas ni las recupera. Entre los motivos se registra el conflicto de interés (RT-14). | M | CONF (Q-34, Q-02c; D-9) | El organizador de un torneo ve las incidencias de sus editores y no las de otros torneos; el registro no contiene nombres ni contenido de la captura; una operación nunca enviada no aparece; ninguna función publica o reaplica una operación rechazada. | I, DB |
| OFF-21 | **Finalizar sin conexión:** se permite como «finalización pendiente»; el partido solo se considera finalizado y público cuando el servidor la confirma. Si el servidor la rechaza, el partido conserva su estado oficial y la operación pasa a `en conflicto`/`rechazada`. | M | CONF (Q-34 parámetro 4) | Final offline + sincronización con y sin conflicto. | E2E |
| OFF-22 | **La autorización definitiva es siempre del servidor/BD y se verifica al sincronizar** con el estado vigente: sesión válida, **autorización vigente por alguno de los roles de la cuenta** (rol vigente y asignación vigente al partido), ausencia de conflicto de interés (RT-05 y RT-14, **incluida la restricción histórica no levantada**), integridad y versión, evaluada de forma atómica con la aplicación de la operación (RT-13). **Se rechaza toda operación sin autorización vigente, aunque se capturara legítimamente antes; una operación que sigue autorizada por otro rol vigente se evalúa por ese rol** (Q-38 d: revocar un rol no elimina permisos legítimos de otros roles; para el organizador depende de Q-42 (a)). No se publica ni se sobrescribe información oficial; no hay recuperación ni publicación automática de operaciones rechazadas. Ningún dato local concede permisos. | M | CONF (Q-02c, Q-34; D-11) | Alterar la cola local para apuntar a otro partido/torneo → rechazado; operación de un editor desasignado, reasignado o revocado antes de sincronizar → rechazada y no publicada; carrera entre revocación y sincronización → una sola de las dos prevalece, sin estados intermedios. Operación de un dueño u organizador sobre su propio equipo (conflicto vigente o restricción histórica no levantada) → rechazada al sincronizar. | I, DB |
| OFF-23 | **Vencimiento de sesión:** si la sesión vence con operaciones pendientes, el usuario puede **continuar la captura local autorizada previamente** (partidos ya preparados y permisos que tenía), pero **para sincronizar se exige sesión válida y permisos vigentes**. No se puede preparar ni descargar datos nuevos sin sesión. | M | CONF (Q-34 parámetro 6) | Con la sesión vencida: la captura continúa; el botón de sincronizar solicita inicio de sesión; tras iniciar sesión, las operaciones se revalidan (OFF-22). | E2E |
| OFF-24 | **Exportación limitada:** el resumen que puede exportar un usuario con operaciones rechazadas o en conflicto, o con cuenta desactivada, contiene **únicamente** el identificador del partido, las abreviaturas de los equipos y el marcador. Los **eventos, minutos, dorsales y equipos asociados solo se incluyen cuando su divulgación esté autorizada y no exponga datos personales protegidos, especialmente de menores**; mientras no exista ese criterio (Q-43) **no se incluyen**. No se exportan capturas privadas ni rechazadas cuya divulgación no esté autorizada. La exportación **no concede permisos nuevos**, no puede reimportarse ni aplicarse como resultado oficial, y su riesgo de identificación indirecta se evalúa (PRV-18). | M | CONF (D-8, D-11); criterio de autorización COND:Q-43 | Por defecto el archivo exportado contiene solo esos tres datos; no contiene nombres, dorsales, minutos ni eventos; una captura con eventos de un jugador menor no los exporta; no existe función de importación. | I, E2E, D |
| OFF-25 | **Secuencias sin conexión con reanudación.** Las operaciones de un partido se validan **en orden de secuencia contra el estado resultante de las anteriores aceptadas**, partiendo del estado oficial del servidor. La secuencia debe ser **contigua** (sin huecos ni duplicados; los duplicados son idempotentes, OFF-06). Los eventos capturados después de una reanudación **dependen de ella**: si la reanudación se rechaza o queda en conflicto, **ningún evento posterior de la secuencia se aplica ni se publica**. Un evento recibido con el estado oficial `suspendido` y sin una reanudación aceptada previa en su secuencia se rechaza (RD-EVT-12). Un hueco bloquea las operaciones posteriores de ese partido y se registra (OFF-20). | M | CONF (Q-46, Q-34) | Secuencia [evento, suspender, reanudar, evento, evento] aceptada en orden y publicada tras la confirmación; [reanudar rechazada, evento] → ningún evento aplicado ni publicado; [reanudar, evento] con el partido ya finalizado en el servidor → en conflicto; hueco → bloqueo; reanudación redundante → conflicto sin efecto; duplicados → un solo efecto. | I, E2E, DB |

### 9.4 Política de conflictos (CONF Q-34: aprobada íntegramente)

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
| 11 | ~~Dos editores asignados al mismo partido (solo si se permite)~~ **No aplica (CONF Q-36 A: un único editor vigente).** Escenario que lo sustituye: **reasignación** — el editor anterior sincroniza capturas hechas antes de perder la asignación | Las operaciones del editor anterior se **rechazan** (fila 4, D-11); no se publican; se conservan según OFF-10; el organizador ve los metadatos (OFF-20) | `rechazada` + exportación limitada (OFF-24) | Organizador (corrección RD-RES-06) |
| 12 | Contenido duplicado con otro identificador | Se acepta (podría ser legítimo); aviso previo al capturar | `confirmada` | Anulación (RD-EVT-05) |

**Precisiones de la consolidación (no sustituyen ninguna fila):** (1) los 12 escenarios fueron aprobados íntegramente. (2) Las filas 4 y 5 se aplican con el estado de permisos **vigente al sincronizar** (OFF-22, D-11): se rechaza toda operación cuya autorización fue revocada aunque se capturara legítimamente antes; nada se publica ni sobrescribe lo oficial; la exportación se limita a OFF-24; el organizador solo ve los metadatos de lo que el servidor recibió, de sus torneos (OFF-20, D-9). (3) La fila 6 ofrece «sin autor» solo si Q-30 lo aprueba. (4) **Q-36 A (confirmada):** cada partido tiene un único editor vigente; la fila 11 **no aplica** y se sustituye por el escenario de reasignación; los conflictos por versión, cambios del organizador, operaciones retrasadas y sincronizaciones repetidas (filas 2, 3, 9, 10) siguen vigentes. (5) La fila 1 solo se acepta si el editor sigue asignado, sin conflicto de interés (RT-05) y con el partido sin finalizar. (6) Una operación de un dueño u organizador sobre su propio equipo (RT-14, vigente o histórica) se rechaza al sincronizar. (7) La fila 1 solo se acepta si el partido no está `suspendido`; si otro actor lo suspendió en el servidor, los eventos sin reanudación previa en su secuencia no se aplican y quedan `en conflicto`, como en la fila 3. (8) La reanudación sin conexión se valida como las demás operaciones de estado y, si se rechaza, ningún evento posterior de la secuencia se publica (OFF-25).

### 9.5 Parámetros del modo sin conexión (CONF Q-34: aprobados los seis)

La columna central conserva íntegra la propuesta de la v1.0; la última recoge la decisión confirmada.

| # | Parámetro | Propuesta original (conservada) | Decisión confirmada (Q-34) |
|---|---|---|---|
| 1 | Ventana de preparación | Partidos asignados no finalizados de los próximos 7 días y en juego | Preparar sin conexión los partidos asignados de los próximos siete días y los partidos en juego (OFF-02) |
| 2 | Antigüedad de los datos preparados antes de avisar | 24 h | Advertencia después de 24 horas sin actualización (OFF-02) |
| 3 | Retención de operaciones rechazadas/en conflicto | 30 días o hasta acción del usuario | Hasta un máximo de 30 días, sujeto a las políticas de privacidad y retención (OFF-10, PRV-10); plazo informado, avisos previos «cuando sea técnicamente posible» y constancia mínima (D-7) |
| 4 | ¿Finalizar sin conexión? | Sí, como «finalización pendiente» | Permitido, mostrando estado de finalización pendiente (OFF-21) |
| 5 | Cuenta desactivada con operaciones pendientes | Informar, ofrecer exportar, borrar datos locales | Eliminación controlada de los datos locales; exportación limitada a un resumen deportivo mínimo sin datos personales protegidos (OFF-11, OFF-24; D-8) |
| 6 | Capturar después de que expire la sesión | Sí (la captura no necesita servidor); para **sincronizar** se exige sesión válida | Continuar la captura local autorizada previamente; exigir sesión válida y permisos vigentes para sincronizar (OFF-23) |

**Reglas adicionales confirmadas:** no sobrescribir resultados confirmados en silencio; no descartar capturas locales en silencio; gestionar duplicados y conflictos de versión; advertir del riesgo de que el navegador elimine el almacenamiento local; no prometer conservación indefinida; no sincronizar operaciones que hayan perdido autorización (OFF-09, 10, 17, 22).

### 9.6 Visibilidad pública (publicación) — matriz

Un resultado es público solo si se cumplen **las tres**: torneo `publicado`/`en curso`/`finalizado`; jornada `publicada`; operación `confirmada` por el servidor.

| Torneo | Jornada | Dato | ¿Público? |
|---|---|---|---|
| borrador | cualquiera | cualquiera | No |
| publicado | borrador | cualquiera | No |
| publicado | publicada | operación `pendiente`/`enviando`/`en conflicto`/`rechazada` | No |
| publicado | publicada | `confirmada` | **Sí** |
| archivado | — | cualquiera | No por ahora (visibilidad de `archivado`: COND Q-40 (d); no se asume archivo histórico público, Q-11) |

---

## 10. Privacidad y consentimiento de menores (Q-01, Q-05)

> **Aviso.** Las referencias legales de esta sección son conocimiento general **no verificado** y **no son asesoría legal**: Ley N.º 8968 (protección de datos personales) y su Reglamento, PRODHAB, Código de la Niñez y la Adolescencia (Ley N.º 7739). Toda cifra de edad, plazo o base legal está marcada **[PEND-JUR]**. La revisión jurídica previa al lanzamiento es una **puerta obligatoria** [CONF].

| ID | Requisito | Prio. | Estado | Aceptación | Prueba |
|---|---|---|---|---|---|
| PRV-01 | La responsable del tratamiento es la organización Torneo de Campeones; el marco aplicable es el de Costa Rica. Los requisitos de esta sección se revisan jurídicamente antes del lanzamiento. | M | CONF (Q-01) | Documento de revisión jurídica archivado antes del lanzamiento (puerta de Fase 8). | D |
| PRV-02 | **Nombre público condicionado.** Un jugador **menor de edad** aparece en las vistas públicas solo como «Jugador n.º {dorsal} ({equipo})» **mientras no conste una autorización de publicación vigente**; con autorización aparece con su nombre público. Para adultos se propone la misma regla por defecto conservadora, ajustable por la organización tras la revisión jurídica. Las estadísticas (goles, tarjetas) cuentan igual con etiqueta o con nombre. La combinación equipo + dorsal puede permitir identificación indirecta: su evaluación es parte de PRV-18 y de Q-20 y **no está resuelta**. | M | CONF (Q-05 «información deportiva necesaria»); regla de adultos COND:Q-20 / PEND-JUR | Fixture: jugador menor sin autorización → etiqueta en plantilla, goleadores y tarjetas; con autorización → nombre; al revocar → vuelve a etiqueta. | E2E, DB |
| PRV-03 | **Nunca públicos** (en ninguna vista ni respuesta pública): fecha de nacimiento o edad exacta, documento de identidad, teléfono, correo, dirección, fotografías, datos y evidencia de consentimiento, identidad del editor asignado, correos de usuarios, nombre completo interno de un jugador sin autorización. | M | CONF (Q-05; skill) | Revisión de esquema y pruebas de contrato de cada vista/endpoint público (ningún campo de la lista). | DB, I, D |
| PRV-04 | **Condición de menor:** el sistema guarda un indicador «es menor de edad» (sí/no) declarado por quien registra al jugador, **no la fecha de nacimiento**, para minimizar datos. Edad de corte propuesta: menor de 18 años [PEND-JUR]. Limitación documentada: el indicador debe actualizarse manualmente cuando el jugador cumple la mayoría de edad. | M | REC; Q-20 [PEND] | El esquema no contiene fecha de nacimiento; el indicador gobierna PRV-02. | D, DB |
| PRV-05 | **Registro de autorización/consentimiento (solo metadatos)** por jugador: tipo (publicación del nombre / tratamiento de datos), otorgante (jugador adulto o representante legal — solo su relación, no sus datos de contacto), fecha, versión del texto aceptado, quién la registró, referencia de la evidencia en poder de la organización (código o ubicación, sin el documento) [REC], estado (vigente / revocada) y fecha de revocación. **Quien recoge la autorización es el delegado o el organizador.** **La evidencia original la conserva la organización de forma segura**; la aplicación **no permite cargar ni almacena documentos** de consentimiento en el MVP. | M | CONF (Q-24); referencia de evidencia REC / PEND-JUR | Sin registro vigente no hay nombre público de menores (PRV-02); toda alta/revocación se audita; el esquema y la interfaz no tienen carga de archivos de consentimiento. | DB, E2E |
| PRV-06 | **Revocación y efectos.** Dos acciones distintas: **ocultar** (vuelve a etiqueta; reversible) y **anonimizar** (borra irreversiblemente nombre y datos personales; conserva identificador interno, dorsal, estadísticas y resultados). Tras anonimizar, los totales de equipo, tabla y goleadores no cambian. La auditoría solo guarda identificadores internos (T-04). | M | CONF (Q-05 revocación) | Anonimizar un jugador con 3 goles: tabla y goleadores iguales; ningún campo personal queda en base de datos ni en auditoría; vista pública con etiqueta. | DB, I |
| PRV-07 | **Aviso de privacidad** y texto de autorización redactados con asesoría jurídica, versionados; la responsable y los canales de contacto se indican en el aviso. | M | CONF (Q-01, Q-05) / PEND-JUR | Texto aprobado y enlazado antes del lanzamiento. | D, M |
| PRV-08 | **Ejercicio de derechos** (acceso, rectificación, cancelación u oposición): la organización **designará una persona responsable** de custodiar las evidencias y de atender las solicitudes; el dueño puede **exportar** los datos de un jugador y **anonimizarlo**; plazos de respuesta: los que fije la revisión jurídica. | M | CONF (Q-24); facultades del organizador COND:Q-42; plazos PEND-JUR | Exportar y anonimizar un jugador de prueba en un entorno de pruebas, con auditoría. | I, M |
| PRV-09 | **Control de acceso a datos de menores:** nombre interno y su indicador solo los ven dueño, organizador del torneo y delegado de su equipo; el editor ve etiqueta/nombre público y dorsal; el público, según PRV-02. | M | CONF (Q-05 «controles de acceso») | Pruebas de lectura por los actores de prueba de §7.3 (13). | DB |
| PRV-10 | **Retención:** se definen plazos para jugadores dados de baja, invitaciones vencidas, operaciones locales (OFF-10, máximo 30 días), **metadatos de incidencias del servidor (OFF-20)**, bitácora de seguridad y auditoría; la auditoría se conserva mientras exista el torneo más el plazo jurídico. Los identificadores técnicos de operaciones están sujetos a privacidad y retención. Valores: PEND-JUR. Incluye el **historial de asignaciones de delegado** y los levantamientos (RT-14), que se conservan mientras exista el torneo; su conservación y la anonimización de cuentas están pendientes de validación jurídica. | M | PEND-JUR; CONF (D-7, D-9: sujeción a privacidad) ; historial de delegaciones PEND-JUR | Política escrita y aplicada en Fase 8. | D |
| PRV-11 | **Minimización en el dispositivo y en el editor:** los datos descargados para trabajar sin conexión contienen solo etiqueta/nombre público y dorsal, nunca los campos internos. | M | CONF (Q-05) + DER (Q-17) | Inspección del almacenamiento local en pruebas (OFF-12). | I |
| PRV-12 | **Registros y monitoreo sin datos personales:** los registros técnicos y el monitoreo de errores omiten nombres, correos e identificadores de dispositivo cuando sea posible. | M | REC | Revisión de una muestra de 100 registros: 0 datos personales. | D |
| PRV-13 | **Infraestructura y transferencia internacional:** se permite evaluar proveedores internacionales (incluidos Supabase y Vercel). Antes de contratar o tratar datos reales se documentan regiones, procesamiento, almacenamiento, transferencias y responsabilidades; se verifican garantías contractuales y medidas de seguridad; se minimizan los datos personales; y se completa la evaluación jurídica aplicable. **Esta decisión no autoriza contratar proveedores ni desplegar infraestructura.** | M | CONF (Q-25) + PEND-JUR | ADR de la Fase 2 con regiones, flujos de datos, garantías verificadas con fuentes oficiales y conclusión jurídica antes de datos reales. | D |
| PRV-14 | **Datos de usuarios del sistema** (correo y rol): visibles solo para dueño y, de sus torneos, organizadores; no públicos. | M | DER | Pruebas de lectura por actor. | DB |
| PRV-15 | **Puerta de lanzamiento:** antes de **registrar o procesar** datos personales reales —cuentas de usuarios, jugadores, delegados y menores— se completa la evaluación jurídica aplicable, que revisa proveedores, transferencias internacionales, medidas de seguridad, autorizaciones y retención, y se aprueba el texto de autorización. **La aprobación documental de los requisitos no equivale a autorización para tratar datos reales.** | M | CONF (Q-05, Q-24, Q-25; D-10) | Evidencia documental en el checklist de Fase 8; ningún dato real en ningún entorno antes de esa evidencia. | D |
| PRV-16 | **Relación con el desarrollador:** formalizar por escrito responsable, encargo de tratamiento y confidencialidad antes de manejar datos reales. | M | CONF (Q-01 pendiente de formalizar) | Documento firmado antes del piloto. | D |
| PRV-17 | **Datos sintéticos:** desarrollo y pruebas usan **exclusivamente datos y cuentas sintéticos**; no se copian datos personales reales ni se usa **seudonimización superficial** como sustituto de datos sintéticos, hasta cumplir PRV-15. | M | CONF (D-10) | Revisión de las semillas, los entornos y las copias: 0 datos reales ni derivados de datos reales. | D |
| PRV-18 | **Identificación indirecta de menores:** antes del lanzamiento se evalúa, con asesoría jurídica, el riesgo de identificar a jugadores —en especial menores— por combinación de equipo, dorsal, minuto, estadísticas, calendario o resúmenes exportados en vistas públicas, exportaciones (OFF-24) y registros (OFF-20); se documentan las mitigaciones. | M | CONF (D-8); resultado PEND-JUR | Informe de evaluación archivado; los campos públicos aprobados (Q-20) coinciden con él. | D |

**Estado de las decisiones de privacidad:** Q-24 (consentimiento: metadatos; evidencia en poder de la organización) y Q-25 (infraestructura internacional con evaluación previa) están **confirmadas**; Q-20 (campos públicos y edad de corte) sigue **abierta**; la persona responsable de evidencias y solicitudes está **por designar** por la organización (§13.4); ver §13.

---

## 11. Requisitos no funcionales

| ID | Atributo | Requisito | Prio. | Estado | Aceptación / medición | Prueba |
|---|---|---|---|---|---|---|
| RNF-01 | Rendimiento web | Core Web Vitals «bueno» en p75: LCP ≤ 2,5 s, INP ≤ 200 ms, CLS ≤ 0,1 en vistas públicas, en celular de gama media con red 3G rápida. Los datos de laboratorio no sustituyen los de campo. | M | REC (informe §4.3) | Medición en staging con dispositivo real; después del piloto, datos de campo. | L, M |
| RNF-02 | Accesibilidad | WCAG 2.2 AA en todas las pantallas del MVP; objetivos táctiles ≥ 44 px; foco visible; teclado completo; contraste verificado. Ninguna herramienta sola demuestra cumplimiento. | M | CONF (skill) | axe sin críticos/serios + revisión manual con lista de verificación + prueba con lector de pantalla en 3 flujos. | A |
| RNF-03 | Disponibilidad y degradación | Durante el piloto se opera con **mejor esfuerzo, sin comprometer un porcentaje contractual de disponibilidad**. Obligatorios: captura sin conexión (OFF), sincronización posterior segura (OFF-05…08), consulta de información almacenada con fecha de actualización (RF-PUB-06) y manejo explícito de errores y recuperación (RF-PUB-07, RNF-17). El objetivo numérico se **reevalúa** cuando se verifiquen las garantías y costos oficiales de los proveedores. Sustituye la meta de 99,5 % del informe (C-15). | M | CONF (Q-16) | Prueba de caída simulada del servidor: captura y lectura en caché siguen funcionando con errores explícitos. | I, E2E |
| RNF-04 | Capacidad | 3–6 torneos simultáneos, 6–16 equipos por torneo, hasta 240 partidos por torneo; lectura pública simultánea de 100–500 usuarios en jornada [SUP, no confirmada]. | M | CONF (Q-06) / SUP (público) | Carga con 500 lectores simultáneos sin errores > 1 %, p95 de lectura ≤ 1 s (propuesto). | L |
| RNF-05 | Seguridad | Autorización en servidor y BD (RT-02); RLS/equivalente probada por rol, tabla y operación; credenciales de servicio solo en servidor; cabeceras de seguridad; dependencias escaneadas; límites de frecuencia en autenticación e invitaciones. | M | CONF (Q-02) | pgTAP completo (Fase 4/7); 0 hallazgos críticos o altos abiertos antes del piloto. | DB, I |
| RNF-06 | Integridad | Ningún resultado confirmado se pierde por concurrencia, reintento o desconexión; las restricciones críticas están en la base de datos y no solo en la aplicación. | M | CONF (Q-06) | N-06, N-13, RD-RES-04, OFF-06. | DB, I |
| RNF-07 | Compatibilidad | Android 9+ con Chrome; iPhone con iOS 16.4+ y Safari; escritorio con Chrome y Edge actuales (panel) y Safari/Firefox (vista pública). Dispositivos reales: 1 Android de gama baja, 1 de gama media, 1 iPhone. Base instalada real: [SUP]. | M | REC | Matriz de dispositivos ejecutada en Fase 7. | M |
| RNF-08 | Respaldo y recuperación | Respaldos automáticos verificables y **restauración probada antes del piloto**. Objetivos propuestos: pérdida de datos máxima (RPO) 24 h y tiempo de recuperación (RTO) 8 h, a confirmar según el plan contratado. | M | CONF (Q-07) + REC (valores) | Restauración en un entorno de pruebas con medición de tiempos. | M |
| RNF-09 | Observabilidad | Errores del cliente y del servidor, disponibilidad externa y alerta al responsable técnico; sin datos personales (PRV-12); métricas OFF-20. | M | REC | Alerta de prueba recibida. | M |
| RNF-10 | Localización | Español de Costa Rica; zona `America/Costa_Rica` (UTC−6, sin horario de verano); fechas dd/mm/aaaa, 24 h; sin otros idiomas en el MVP. | M | DER (Q-01) | Revisión de textos y fechas. | E2E |
| RNF-11 | Mantenibilidad y pruebas | Toda regla de §8 tiene pruebas unitarias con fixtures; toda restricción de §7 tiene prueba contra PostgreSQL real; esquema y migraciones versionados; no se modifica producción a mano. | M | CONF (skill) | Trazabilidad requisito→prueba (§14). | U, DB |
| RNF-12 | Costos y planes | Priorizar planes gratuitos solo si son legal, técnica y operativamente adecuados; verificar con fuentes oficiales límites, restricciones comerciales, pausas por inactividad y respaldos **antes de contratar o publicar**; la organización asume los costos; la evaluación de proveedores internacionales sigue PRV-13. | M | CONF (Q-07, Q-25) | ADR de Fase 2 con fuentes oficiales y costo mensual confirmado por la organización. | D |
| RNF-13 | Usabilidad de captura | Registrar un gol en ≤ 3 toques desde la pantalla del partido; registrar un resultado final directo en < 30 s (meta del informe) medido en UAT con ≥ 3 editores. | M | REC (informe, sin línea base) | Cronometraje en UAT; la línea base se mide con el proceso actual, hoy una combinación de varios métodos aún no especificados (D-12; investigación pendiente, §13.4). | M |
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
| CL-28 | Revocar al último dueño activo; pérdida del segundo factor del dueño | Rechazo de la revocación; con un segundo dueño, este puede apoyar la recuperación según el procedimiento documentado | RF-ACC-09, RT-06 | DB, D |
| CL-29 | Marcador distinto de la suma de eventos; partido W.O. | Según Q-30; W.O. exento | RD-EVT-04, RD-WO-07 | U |
| CL-30 | Pico de lectura pública en el instante de confirmar un resultado | Ninguna vista sirve datos anteriores > 60 s tras la confirmación | RF-PUB-09 | L |
| CL-31 | Cambio de reglamento con el torneo iniciado | Solo el dueño (si se adopta); vista previa; auditado; recalculo | RD-REG-03 | I |
| CL-32 | Partido sin editor a la hora de inicio (N-12) | Alerta al organizador | RF-ADM-04 | I |
| CL-33 | Segunda amarilla o evento posterior a una roja | Sugerencia de roja; advertencia (no bloqueo) | RD-EVT-06 | E2E |
| CL-34 | Corrección en torneo `finalizado` | Permitida al dueño y al organizador del torneo, con motivo obligatorio y auditoría; el editor, denegado (salvo conflicto de interés, RT-14) | RD-RES-06, RF-TOR-02 | I |
| CL-35 | Se otorga el rol de delegado a un editor con partidos asignados de ese equipo | Las asignaciones en conflicto se retiran con auditoría; sus operaciones sin conexión sobre esos partidos se rechazan al sincronizar | RT-05, OFF-22 , RT-14 | DB, I |
| CL-36 | Alta de un jugador con nombre equivalente al de un jugador activo de otro equipo; dos altas simultáneas | Queda `pendiente de verificación`; el delegado no ve datos del otro equipo; a lo sumo una alta activa; decide el organizador | RD-JUG-02, 08, 09 | DB, I, E2E |
| CL-37 | Operaciones rechazadas o en conflicto llegan a 30 días | Avisos previos, exportación posible y eliminación visible, nunca silenciosa | OFF-10 | I, E2E |
| CL-38 | Designación de un segundo dueño; organizador intenta conceder o revocar el rol de dueño; un dueño revoca a otro | Exige nueva autenticación 2FA (y motivo en la revocación); organizador denegado; nunca se revoca al último dueño activo; ambos dueños con los mismos privilegios; todo auditado | RF-ACC-09, RT-06, matriz filas 4 y 35 | DB, I |
| CL-39 | Sesión vencida con operaciones pendientes; los permisos cambian antes de volver a iniciar sesión | La captura continúa; sincronizar exige sesión válida; las operaciones se revalidan con permisos vigentes | OFF-23, OFF-22 | E2E, I |
| CL-40 | Ningún rol puede vincular inscripciones a una identidad común ni ver vínculos: la operación **no existe en el MVP** (RF-EQP-06 diferido); además, un organizador no accede a datos de otro torneo (plantillas, estadísticas, usuarios, auditoría) | Denegado o inexistente; sin exposición de plantillas ni estadísticas ajenas | RF-EQP-01, RF-EQP-06, RT-08, RT-12 | DB, I |
| CL-41 | Dos organizadores (o un organizador y el dueño) reasignan el mismo partido a la vez | Una reasignación prevalece; nunca hay dos editores vigentes en el mismo partido; ambas intenciones quedan auditadas | RF-ACC-06, RT-13 | DB, I |
| CL-42 | **[DIFERIDO: se activa cuando se construya el vínculo]** El dueño vincula inscripciones de A y B a una identidad común; un organizador de A y un delegado del equipo vinculado consultan datos de B | Siguen sin acceso: el vínculo no comparte plantillas, estadísticas, permisos ni información privada | RF-EQP-06, RT-12 | DB |
| CL-43 | Dos dueños se revocan mutuamente al mismo tiempo | Como máximo una revocación prospera; queda al menos un dueño activo | RF-ACC-09, RT-06 | DB, I |
| CL-44 | Torneo `archivado`: un organizador intenta corregir un resultado; el dueño lo corrige | Organizador denegado; dueño permitido con motivo y auditoría ; si el dueño es el único y es delegado de un equipo del partido, la corrección se bloquea (RT-14, CL-51) | RD-RES-06, RF-TOR-02, matriz filas 27 y 37 | I, DB |
| CL-45 | Una revocación o desasignación llega al mismo tiempo que una operación en línea o una sincronización | La operación se aplicó antes (válida y auditada) o se rechaza; nunca queda a medias ni se aplica con la autorización ya revocada | RT-13, RF-ACC-07, OFF-22 | DB, I |
| CL-46 | Un usuario intenta exportar eventos o dorsales de capturas rechazadas, o datos de un menor | La exportación contiene solo identificador de partido, abreviaturas y marcador; sin eventos mientras no exista el criterio de Q-43 | OFF-24, PRV-18 | I, E2E |
| CL-47 | El editor anterior sincroniza, tras una reasignación, capturas hechas cuando aún estaba asignado | Se rechazan y no se publican; se conservan según OFF-10; el organizador ve solo los metadatos; no hay recuperación automática | RF-ACC-06, OFF-11, OFF-20, OFF-22 | I, E2E |
| CL-48 | Dueño u organizador con rol de delegado intenta, sobre su propio equipo, cada una de las 7 clases de RT-14 (resultados, W.O., resolución manual, asignaciones, ajustes de calendario, inscripción o retiro, verificación de duplicados) | Denegado en servidor y BD; permitido a otro actor sin conflicto; auditado | RT-14, RF-AUD-06 | DB, I, E2E |
| CL-49 | La misma operación capturada sin conexión, o el rol de delegado otorgado después de capturar, llega al sincronizar | Rechazada al sincronizar; metadatos registrados; no se publica | RT-14, OFF-22, OFF-20 | I, E2E |
| CL-50 | Intento de evadir RT-14 mediante otro rol (editor asignado, organizador de otro torneo, segundo rol) | Denegado | RT-14, RT-05, RT-11 | DB |
| CL-51 | Sin autoridad alterna (dueño único o único organizador en conflicto; torneo `archivado`) | Operación bloqueada y auditada; sin excepción automática; el último dueño activo sigue protegido | RT-14, RF-AUD-06, RT-06 | DB, I |
| CL-52 | Un organizador revoca el rol adicional de editor o delegado de otro organizador | Permitido solo en torneos que ambos administran; su rol de organizador no cambia; fuera de esos torneos, denegado | RF-ACC-07 | DB, I |
| CL-53 | Tras la revocación parcial, el afectado conserva los permisos de otros roles vigentes; operación pendiente autorizada por otro rol | Se evalúa por el rol vigente restante; si ninguno la autoriza, se rechaza | RT-11, OFF-22 | DB, I |
| CL-54 | Dos invitaciones simultáneas harían delegado de dos equipos del mismo torneo a la misma persona | Una prospera y la otra se rechaza; en torneos distintos ambas son válidas | RF-ACC-13, RF-ACC-02 | DB, I |
| CL-55 | Cuenta con rol de delegado y otro rol: política de 2FA | La política efectiva usa todos los roles vigentes; delegado sin 2FA accede; con rol de organizador sin 2FA no accede a ese rol | RF-ACC-10 | I, E2E |
| CL-56 | Actor en conflicto confirma la generación inicial o la regeneración del calendario | Denegada con 0 filas cambiadas; otro actor sin conflicto sí puede | RT-14, RD-CAL-02, RD-CAL-07 | DB, I |
| CL-57 | Operación multipartido con un partido propio (intercambio de rivales, mover jornada) | Se rechaza por completo | RT-14, RD-CAL-06 | U, I |
| CL-58 | Consulta y vista previa del actor en conflicto | Permitidas, sin cambios persistentes; la confirmación la hace otro actor con su propia evaluación | RT-14, RD-CAL-02 | I, E2E |
| CL-59 | Pierde el rol de delegado (renuncia, revocación por otro organizador o por el dueño) e intenta actuar sobre ese equipo | La restricción persiste en ese torneo | RT-14, RF-ACC-11 | DB, I |
| CL-60 | Levantamiento de la restricción histórica | Solo otro dueño activo sin conflicto respecto del equipo, con motivo y auditoría; autoaprobación denegada; sin dueño alterno la restricción permanece; ámbito equipo y torneo; sin permisos adicionales | RF-ACC-17, RT-14 | DB, I |
| CL-61 | Levantamiento, revocación y acción del afectado simultáneos | Resultado serializado (RT-13); nunca se aplica una acción con la restricción vigente y confirmada | RT-13, RT-14 | DB, I |
| CL-62 | El rol de delegado se otorga de nuevo tras un levantamiento | El conflicto vigente (Q-44) vuelve a aplicar; el levantamiento no cubre el conflicto vigente | RT-14, RF-ACC-17 | DB |
| CL-63 | Editor que fue delegado | Sin persistencia histórica; solo rige RT-05 | RT-14, RT-05 | DB |
| CL-64 | Sincronización con restricción histórica o levantamiento vigente al sincronizar | Se evalúa el estado vigente al sincronizar | OFF-22, RT-14 | I |
| CL-65 | Revocación de un rol o desactivación de una cuenta | El historial de asignaciones no se elimina ni altera; la anonimización queda pendiente de validación jurídica | RF-ACC-11, PRV-10 | DB, D |
| CL-66 | Suspender un partido `en_juego` | Editor asignado y dueño lo permiten; no asignado, delegado y público, denegado; sin conexión se sincroniza; el público ve el estado solo tras la confirmación | RD-RES-10, OFF-01 | U, DB, I, E2E |
| CL-67 | Reanudar un partido `suspendido` | Editor asignado, organizador del torneo y dueño lo permiten; delegado, público, editor no asignado, organizador de otro torneo y actor en conflicto (RT-14, RT-05), denegado | RD-RES-13 | U, DB, I |
| CL-68 | Reanudación registrada sin conexión | Operación local pendiente; el estado oficial publicado no cambia hasta validarse; puede rechazarse | RD-RES-13, OFF-04, OFF-22 | E2E, I |
| CL-69 | Secuencia sin conexión con reanudación y eventos: la reanudación se rechaza o queda en conflicto | Ningún evento posterior se aplica ni se publica; con reanudación aceptada, los eventos se aceptan en orden | OFF-25, OFF-07, RD-EVT-12 | I, E2E |
| CL-70 | Evento nuevo con el partido `suspendido`, en línea o al sincronizar con estado oficial suspendido y sin reanudación previa en la secuencia | Rechazado | RD-EVT-12 | U, DB, I |
| CL-71 | Cierre desde `suspendido`, `aplazado` o `cancelado` | La transición `suspendido` → `finalizado` y los estados `aplazado` y `cancelado` no están disponibles para ningún rol | RD-RES-12 | U, DB |
| CL-72 | Hueco o duplicado en la secuencia; reanudación redundante (el partido ya está `en_juego`) | Hueco: bloquea las posteriores; duplicado: idempotente; reanudación redundante: conflicto sin efecto | OFF-25, OFF-06 | I, DB |

---

## 13. Decisiones: confirmadas, abiertas y asuntos derivados (Q-02a/b/c, Q-08…Q-46)

**Estado tras la tercera consolidación (v1.3, 2026-11-15):** **21 decisiones confirmadas** por el usuario (§13.1; texto en §2.1, §2.1b y §2.1c), consecuencias **D-1…D-11 confirmadas** y **D-12 como información confirmada** (§2.2), **RK-15 aceptado** y **RF-EQP-06 diferido**. **18 asuntos siguen abiertos** (14 de la v1.0 en §13.2 y 4 derivados en §13.3), más dos acciones de la organización (§13.4). **Ninguno bloquea la aprobación de la Fase 1 ni, formalmente, la Fase 2**; Q-14 y Q-40 (d) deben resolverse antes de la Fase 3 (§13.5). Ninguna decisión pendiente se tomó en nombre del usuario; cada fila trae opciones y una recomendación `[REC]`. Mientras un asunto esté abierto, sus requisitos quedan `COND` y **no se consideran aprobados**.

### 13.1 Decisiones confirmadas (21)

Se conservan las alternativas consideradas para el historial. La decisión vigente es la de §2.1, §2.1b y §2.1c.

| ID | Pregunta | Alternativas consideradas (historial íntegro) | Decisión confirmada | Requisitos actualizados |
|---|---|---|---|---|
| Q-02a | ¿Cómo queda autorizado el organizador que crea un torneo? | A) Queda automáticamente como organizador de **ese** torneo. B) Lo asigna el dueño después de crearlo. | **A** | RF-ACC-12, RF-ACC-03, RF-AUD-02, matriz fila 10, HU-ORG-01, CL-21 |
| Q-02b | ¿Quién asigna editores a partidos? | A) Dueño y organizador del torneo. B) Solo el dueño. C) El editor «toma» partidos libres. | **A** | RF-ACC-06, matriz fila 7, HU-ORG-07, RT-04, RT-05, OFF-22, CL-20, CL-35, CL-41; Q-36 |
| Q-02c | ¿Quién revoca accesos? | A) Dueño a cualquiera; organizador a editores y delegados de sus torneos. B) Solo el dueño. | **A** | RF-ACC-07, matriz filas 5–6, RT-06, OFF-11, OFF-22, HU-DUE-02, CL-02, CL-03 |
| Q-08 | ¿2FA para editores? (y ¿cómo registran hoy los resultados y quién lo hace?) | A) Obligatorio. B) Opcional/recomendado, reevaluar tras el piloto. C) No. | **B**. La segunda parte (proceso actual de captura) **sigue pendiente** como acción de la organización (§13.4) | RF-ACC-10, RF-ACC-16; RNF-13 (línea base) |
| Q-09 | ¿Quién corrige o reabre partidos finalizados, en qué plazo, y cómo se manejan disputas? | A) Organizador del torneo o dueño, sin plazo, con motivo. B) A + plazo (p. ej. 7 días; luego solo dueño). C) Solo el dueño. | **A** | RD-RES-06, matriz fila 27, HU-DUE-07, HU-ORG-09, §3, CL-15, CL-34, CL-44; D-5 |
| Q-11 | ¿Qué histórico de temporadas se cargará? ¿Los equipos conservan identidad entre torneos? | A) Sin histórico. B) Captura manual de tablas finales. C) Importación con datos mínimos. | **A**; los equipos pueden conservar identidad entre torneos | RF-EQP-01, §3, §9.6, CL-23, CL-40 |
| Q-12 | ¿Cómo identificar a un jugador sin recolectar documentos (para «un jugador, un equipo»)? | A) Sin identificador; aviso de duplicados por nombre normalizado. B) Identificador interno verificable (p. ej. cédula) solo para validación. C) Confirmación manual del organizador. | **A + C** | RD-JUG-02, 08, 09, 10; HU-DEL-04, HU-ORG-14; CL-22, CL-36 |
| Q-15 | ¿Una persona puede tener más de un rol en el mismo torneo? | A) Uno por persona y torneo. B) Varios, con RT-05 (conflicto de interés). C) Varios sin restricción. | **B** | RF-ACC-08, RT-01, 04, 05, 11; §7.4; CL-35 |
| Q-16 | ¿Qué disponibilidad es aceptable y qué pasa si el sistema cae durante un partido? | A) Mejor esfuerzo en el piloto. B) 99,5 % mensual. | **A** | RNF-03, RF-PUB-06, RF-PUB-07, RNF-17; C-15 resuelta |
| Q-24 | ¿Quién recolecta el consentimiento, dónde queda la evidencia y quién atiende solicitudes? | A) Evidencia en poder de la organización; el sistema guarda metadatos. B) Archivo digital en el sistema. | **A**; la organización designará a la persona responsable (§13.4) | PRV-05, 08, 15, 17; matriz fila 20; HU-DEL-02; §3 |
| Q-25 | ¿Restricciones de ubicación de datos o transferencias fuera de Costa Rica? | Sin opciones en la v1.0 (recomendación previa: documentar región y proveedores, consultar a jurídico, minimizar datos). | **Permitir evaluar infraestructura internacional** con evaluación previa; no autoriza contratar ni desplegar | PRV-13, PRV-15, PRV-17, RNF-12 |
| Q-32 | ¿Los resultados se publican automáticamente al confirmarse o requieren validación? | A) Automática. B) Validación del organizador. | **A** | RD-RES-07, RF-PUB-04, RF-PUB-09, OFF-15, §9.6 |
| Q-34 | Parámetros del modo sin conexión (ventana, retención, finalizar sin conexión, cuenta desactivada, sesión vencida) | Ver §9.5 (propuesta conservada) | **Aprobados los seis parámetros de §9.5 y los 12 escenarios de §9.4** | OFF-02, 10, 11, 17, 20, 21, 22, 23; §9.4, §9.5; HU-EDI-10, 11; CL-37, CL-39 |
| Q-35 | Continuidad del dueño: ¿un dueño único o un segundo dueño? ¿Quién otorga el rol de dueño? | A) Dueño único con procedimiento de recuperación. B) Un segundo dueño designado por el dueño. | **B** | RF-ACC-09, RT-06, matriz filas 4 y 35, HU-DUE-06, CL-28, CL-38 |
| Q-36 | ¿Puede un partido tener más de un editor asignado a la vez? (Q-02b decide quién asigna, no cuántos) | A) Uno solo vigente (reasignar = desasignar + asignar). B) Varios simultáneos (p. ej. relevo). | **A**: un único editor vigente; reasignación atómica y auditada; sin ventana de doble asignación | RF-ACC-06, RT-13, §9.4 filas 10–11 (la 11 no aplica), OFF-22, CL-41, CL-47 |
| Q-37 | ¿Quién crea, vincula y ve la identidad común de un equipo entre torneos? (Q-11 la permite) | A) Catálogo global gestionado solo por el dueño; los organizadores eligen entre equipos existentes. B) Inscripciones independientes; solo el dueño las vincula a una identidad común. C) El organizador reutiliza identidades de equipos que él mismo inscribió. D) Sin vínculo técnico: la identidad es solo el nombre. | **B**: inscripciones independientes; solo el dueño vincula, de forma opcional; el vínculo no comparte ni concede nada | RF-EQP-01, RF-EQP-06 (**implementación diferida fuera del MVP**), RT-12, matriz fila 36 (diferida), CL-40, CL-42 (diferido) |
| Q-38 | Roles combinados (Q-15 los permite): (a) organizador o dueño que también es delegado de un equipo —conflicto de interés fuera de la regla delegado+editor—; (b) delegado de dos equipos del mismo torneo; (c) ~~rol adicional por autogestión~~ **resuelto por D-3** (ningún usuario se concede roles a sí mismo); (d) revocación parcial: ¿un organizador puede revocar el rol de editor/delegado de otro organizador sin tocar su rol de organizador? | (a) A) Prohibir que ejecute acciones deportivas (resultados, W.O., resolución manual, asignaciones) sobre su propio equipo; las hace otro organizador o el dueño. B) Prohibir la combinación. C) Permitir sin restricción. (b) A) No permitir. B) Permitir. (c) *historial:* A) El rol adicional lo concede otro actor con facultad. B) Autogestión permitida. (d) A) Sí, solo sobre el rol de editor/delegado. B) No. | (a) **A**; (b) **A**; (c) resuelta por D-3; (d) **A** | RT-14, RT-11, RT-05; RF-ACC-07, 08, 13; §7.4; matriz fila 6; HU-ORG-15, 16; CL-50, 52, 53, 54 |
| Q-39 | ¿2FA para delegados? (Q-08 decidió dueño, organizador y editor, no delegado) | A) Obligatorio. B) Opcional/recomendado (como editores). C) No. | **B**: 2FA opcional y recomendado para delegados; obligatorio si la cuenta tiene rol de dueño u organizador; política efectiva por todos los roles vigentes | RF-ACC-10; CL-55 |
| Q-44 | Alcance del conflicto de interés de un dueño u organizador que también es delegado (propuesto como Q-44 en la auditoría de la v1.2) | (i) Solo la lista literal de Q-38 (a): resultados, W.O., resolución manual y asignaciones. (ii) Lista ampliada: además ajustes de calendario que afecten al propio equipo, inscripción o retiro de ese equipo y verificación de duplicados; sin otro actor sin conflicto, denegar hasta que exista otra autoridad | **Opción B** (lista ampliada: resultados, W.O., resoluciones manuales, asignaciones que generen conflictos, ajustes de calendario que afecten al propio equipo, inscripción o retiro, verificación de duplicados), con las condiciones de §2.1c | RT-14; RF-ACC-06; RD-CAL-02, 06, 07; RD-RES-06, 09; RD-WO-02, 05; RD-TAB-09; RD-JUG-02; RF-EQP-02, 03; RF-AUD-06; OFF-22; matriz filas 7, 16, 22–29, 38; CL-48…51, 56…58 |
| Q-45 | Tras retirar el rol de delegado de un equipo a un dueño u organizador (A-7): ¿persiste el conflicto respecto de ese equipo? ¿Por cuánto tiempo y con qué vía de levantamiento? ¿Se extiende a los editores (RT-05)? | A) Persiste durante todo el torneo. B) Solo cuenta el rol vigente, con controles detectivos (auditoría con motivo y aviso al dueño). C) Persiste hasta que otro dueño sin conflicto lo levante con motivo y auditoría. D) Persiste durante un período definido por la organización (la cifra debe aportarla la organización). Extensión a editores: A) No. B) Sí, con aviso al asignar. | **C**; extensión a editores: **A** (no se extiende) | RT-14; RF-ACC-11, RF-ACC-17; RF-AUD-02, RF-AUD-06; PRV-10; matriz fila 40; HU-DUE-09; CL-59…65 |
| Q-46 | Partidos `suspendido`: ¿quién puede reanudarlos o cerrarlos, puede hacerse sin conexión y se admiten eventos y marcador mientras están suspendidos? (El efecto en la tabla sigue siendo Q-29.) | A) Solo organizador y dueño, en línea. B) Editor asignado, organizador y dueño; el editor reanuda sin conexión como «reanudación pendiente», análoga a OFF-21. C) Solo el dueño. Eventos durante la suspensión: A) No se admiten. B) Se admiten. | **B** para la reanudación; eventos durante la suspensión: **A** (no se admiten). **El cierre desde `suspendido` no se presume aprobado** | RD-RES-01, 10, 12, 13; RD-EVT-12; OFF-01, 07, 25; matriz filas 39 y 41; HU-EDI-12, HU-ORG-17; CL-66…72 |

### 13.2 Decisiones de la v1.0 que siguen abiertas (14)

| ID | Pregunta | Opciones | Recomendación [REC] | Si no se decide | Necesaria antes de |
|---|---|---|---|---|---|
| Q-10 | ¿Canal y destinatarios de la alerta N-12? | A) Indicador en el panel del organizador. B) A + correo. C) WhatsApp/push. | **A** (+ B si ya hay correo transaccional). C es posterior (costo/complejidad; push fuera del MVP). Destinatarios: organizadores del torneo y dueño. | RF-ADM-04 queda en A | F3 / módulo 5D |
| Q-13 | ¿Qué se comparte, por qué canal y hay patrocinadores o cobros? | — | MVP: enlace estable, QR y botón nativo de compartir. **Pagos, boletería y patrocinadores fuera del MVP.** Se pregunta a la organización por canales (WhatsApp, Facebook, Instagram) y patrocinadores. | RF-PUB-05 en nivel S | F3 |
| Q-14 | ¿Logo en formato original, mockup de 13 pantallas y colores oficiales? | — | La organización los aporta o confirma. **No bloquea la Fase 1; bloquea la Fase 3.** | Fase 3 | F3 |
| Q-20 | ¿Qué campos públicos de jugador y desde/hasta qué edad aplican las reglas de menores? | A) Nombre público + dorsal + equipo + estadísticas; menores con etiqueta salvo autorización (PRV-02). B) Nombre siempre público para adultos y menores con autorización. | **A/B combinados** según PRV-02; **nunca** foto, fecha de nacimiento ni documento. Corte de 18 años [PEND-JUR]. | PRV-02/04 no se aprueban | F3 y F4; jurídico antes del lanzamiento |
| Q-21 | W.O.: ¿qué ocurre si ambos equipos no se presentan, hay sanciones y cuenta para GF/GC? | A) Doble W.O. con 0 puntos y 0-0. B) Ambos derrotados con marcador configurable. | **A**; GF/GC cuentan según el marcador de adjudicación; **sin sanciones automáticas** (se muestra el conteo de W.O.). | RD-WO-02…04 bloqueados | F4 / módulo 5E |
| Q-22 | «Resolución manual»: ¿quién, con qué justificación y cómo se publica? | — | Organizador del torneo o dueño; motivo obligatorio; etiqueta pública; reversible; queda obsoleta si cambian los resultados implicados (RD-TAB-09). | RD-TAB-09 bloqueado | F4 / módulo 5F |
| Q-23 | Cambios de reglamento en torneo iniciado: ¿cuáles, quién aprueba y se recalcula el histórico? | — | RD-REG-03: solo dueño; criterios se recalculan; puntos y vueltas no cambian; W.O. solo hacia adelante. | RD-REG-03 bloqueado | F4 |
| Q-26 | Equipo que se retira o es expulsado en curso: ¿qué pasa con sus resultados y partidos restantes? | A) Anular todos sus resultados. B) Conservar los jugados y adjudicar W.O. a los restantes. C) Conservar los jugados y anular los restantes. D) Configurable por torneo. | **D**, definido antes de iniciar; defecto a confirmar con la organización. | RF-EQP-03 bloqueado | F4 / módulo 5B |
| Q-27 | «Enfrentamiento directo» con más de dos equipos y subgrupos: ¿qué se compara y se recalcula? | A) Mini-tabla (puntos, DG, GF) entre los empatados, recalculada para el subgrupo restante. B) Solo puntos entre ellos. | **A** (RD-TAB-06). Confirmar contra el reglamento real de la organización. | RD-TAB-06 con semántica provisional | F4 / módulo 5F |
| Q-28 | Catálogo de criterios de desempate configurables y fair play | — | Catálogo: DG, GF, GC, victorias, enfrentamiento directo; fair play fuera (exige ponderar tarjetas). | RD-TAB-05 con el catálogo recomendado | F3 / módulo 5F |
| Q-29 | Estados `aplazado`, `cancelado` y `suspendido`: ¿efecto en la tabla? | — | Aplazado = reprogramación; cancelado = no cuenta y exige motivo; suspendido conserva el marcador hasta decisión del organizador. | RD-RES-10, CL-11 incompletos | F4 |
| Q-30 | Consistencia marcador–eventos | A) Estricta. B) Advertencia y «goles sin autor». C) Independientes. | **B**: permite finalizar con aviso al organizador; los «sin autor» no cuentan en goleadores. | RD-EVT-02/04 con comportamiento provisional | F4 / módulo 5E |
| Q-31 | Plantillas: cierre, mínimos/máximos, categorías o edad | — | Fecha de cierre configurable; sin mínimos/máximos ni elegibilidad por edad en el MVP. | RD-JUG-05 bloqueado | F4 / módulo 5B |
| Q-33 | ¿Puntos 3/1/0 fijos o configurables? | A) Fijos (leídos del reglamento). B) Configurables antes de iniciar. | **A** en el MVP. | RD-REG-06 | F4 |

### 13.3 Asuntos derivados que siguen abiertos (4)

Surgieron al verificar la consistencia; **no son decisiones del usuario**. Q-36, Q-37, Q-38, Q-39, Q-44, Q-45 y Q-46 están confirmadas (§13.1).

| ID | Pregunta | Opciones | Recomendación [REC] | Si no se decide | Necesaria antes de |
|---|---|---|---|---|---|
| Q-40 | Estados `finalizado` y `archivado` (D-5 los define): (a) ¿quién puede archivar un torneo? (b) ¿puede reabrirse un `finalizado` o desarchivarse un `archivado`, y quién? (c) ¿qué otras operaciones se bloquean en `archivado` además de las correcciones del organizador? (d) ¿un torneo `archivado` sigue visible al público en solo lectura? | (a) A) Solo el dueño. B) Dueño y organizador del torneo. (b) Reabrir `finalizado`: A) Solo el dueño, con motivo y auditoría. B) Dueño y organizador, con motivo y auditoría. C) No se permite. Desarchivar `archivado`: A) No se permite (cierre definitivo). B) Solo el dueño, con motivo y auditoría. (c) A) Solo lectura para todos, salvo las correcciones del dueño. B) Solo se restringen las correcciones del organizador. (d) A) No visible (coherente con Q-11). B) Visible en solo lectura. | (a) **A** (archivar quita facultades al organizador); (b) **C** y **A**; (c) **A**; (d) **A** mientras no se decida un archivo histórico (Q-11) | RD-RES-06, RF-TOR-02, RF-PUB-01 y §9.6 con la transición a `archivado` sin definir | F4; la parte (d) antes de F3 (pantallas públicas) |
| Q-41 | ¿Se permiten traspasos de jugadores entre equipos del mismo torneo? (Q-12 rechaza la segunda inscripción de la misma persona) | A) No en el MVP. B) Sí, con baja previa y autorización del organizador; estadísticas por equipo. | **A**; consultar el reglamento real de la organización. | RD-JUG-10 provisional | F4 / módulo 5B |
| Q-42 | Facultades del organizador no cubiertas por decisiones: (a) registrar marcador/eventos y cambiar estados en partidos no finalizados de sus torneos (matriz 24–25); (b) anonimizar y exportar datos de jugadores (matriz 21, 34) | (a) A) Sí. B) No (solo editor y dueño). (b) A) Solo el dueño. B) También el organizador de su torneo. | (a) **A** (contingencia N-12; ya corrige por Q-09); (b) **A** | Matriz filas 21, 24, 25, 34; PRV-08 | F4 / módulos 5C y 5E |
| Q-43 | Criterio de autorización para incluir eventos, minutos, dorsales y equipos en el resumen exportable (D-8): ¿qué se considera divulgación autorizada que no expone datos protegidos y quién la autoriza? | A) Nunca incluir eventos en el MVP (el resumen es identificador, abreviaturas y marcador). B) Incluir eventos solo de jugadores sin restricción de menor y con autorización de publicación vigente (PRV-05). C) Incluir según la evaluación jurídica de identificación indirecta (PRV-18). | **A** hasta concluir la evaluación jurídica; reevaluar después | OFF-24 queda sin eventos; PRV-18 | F4 / módulo 5E; antes de datos reales |

### 13.4 Acciones pendientes de la organización o de investigación (no son decisiones del usuario)

| Acción | Origen | Necesaria antes de |
|---|---|---|
| Designar a la persona responsable de custodiar evidencias de autorización y atender solicitudes | Q-24 | Datos personales reales |
| **Investigación (D-12):** detallar cuáles son y cómo se combinan los métodos con que hoy se registran los resultados; **hasta entonces no se asume ninguno** | D-12 (información confirmada), RNF-13 | F3 |
| Aportar logo en formato original, mockup de 13 pantallas y confirmar colores | Q-14 | F3 |
| Completar la evaluación jurídica (privacidad, menores, transferencia internacional, identificación indirecta y **conservación y anonimización del historial de delegaciones**) y formalizar responsable y encargo con el desarrollador | PRV-10, 13, 15, 16, 18; RF-ACC-11 | Datos personales reales |
| Verificar condiciones oficiales, costos, respaldos y garantías de los proveedores | RNF-12, Q-07, Q-25 | F2 |
| Contrastar con el reglamento escrito de la organización los criterios de §8 (desempates, W.O., retiro, traspasos, suspensión) | RK-07, Q-26, Q-27, Q-29, Q-41 | F4 |
| Definir el procedimiento de recuperación y continuidad de los dueños | RF-ACC-09, RNF-17 | F2 / F8 |

### 13.5 Clasificación de los asuntos abiertos

«Bloquea F1» = impide aprobar la Fase 1. Ninguno la bloquea: los requisitos afectados permanecen `COND` y **fuera de lo aprobado**. Ningún asunto se declara resuelto sin autorización expresa del usuario.

| ID | Descripción exacta | ¿Bloquea F1? | ¿Antes de F2? | ¿Diferible? (condición) | Requisitos afectados | Recomendación fundamentada |
|---|---|---|---|---|---|---|
| Q-10 | ¿Canal y destinatarios de la alerta N-12? | No | No | Sí, hasta F3/módulo 5D. Condición: RF-ADM-04 (a) solo con indicador en el panel | RF-ADM-04; HU-ORG-08 | A (+B si ya hay correo transaccional): sin costo ni dependencia externa; push y WhatsApp quedan fuera del MVP (Q-18). |
| Q-13 | ¿Qué se comparte, por qué canal y hay patrocinadores o cobros? | No | No | Sí, hasta F3. Condición: compartir solo por enlace, QR y botón nativo | RF-PUB-05; HU-PUB-05 | Mantener el MVP mínimo; pagos, boletería y patrocinadores siguen fuera del alcance. |
| Q-14 | ¿Logo en formato original, mockup de 13 pantallas y colores oficiales? | No | No, pero **bloquea la Fase 3** | No más allá de F3 | Sistema visual de F3; RNF-02 (contraste) | La organización aporta el logo original y confirma colores; sin ellos F3 no cierra el sistema visual. |
| Q-20 | ¿Qué campos públicos de jugador y desde/hasta qué edad aplican las reglas de menores? | No | No; recomendada antes de F3 | Sí, hasta F3/F4. Condición: PRV-02 y PRV-04 siguen `COND` y no se publica ningún dato personal | PRV-02, 04, 18; RD-JUG-01; RF-PUB-02; HU-PUB-04; OFF-24 | A/B combinadas (PRV-02) por minimización (Q-05); resolver junto con la evaluación de identificación indirecta (equipo + dorsal) con asesoría jurídica. |
| Q-21 | W.O.: ¿qué ocurre si ambos equipos no se presentan, hay sanciones y cuenta para GF/GC? | No | No | Sí, hasta F4/5E. Condición: RD-WO-02…04 provisionales | RD-WO-02, 03, 04; HU-ORG-10 | A: regla más simple y sin sanción automática; el conteo de W.O. permite aplicar sanciones manualmente. |
| Q-22 | «Resolución manual»: ¿quién, con qué justificación y cómo se publica? | No | No | Sí, hasta F4/5F. Condición: los empates sin resolver se muestran como tales | RD-TAB-08, 09; HU-ORG-11 | Organizador o dueño con motivo, etiqueta pública y reversibilidad: transparencia y auditoría. |
| Q-23 | Cambios de reglamento en torneo iniciado: ¿cuáles, quién aprueba y se recalcula el histórico? | No | No | Sí, hasta F4. Condición: sin cambios de reglamento con el torneo iniciado | RD-REG-03; HU-DUE-04; matriz fila 12 | Solo el dueño; puntos y vueltas inmutables: protege la integridad de la tabla. |
| Q-26 | Equipo que se retira o es expulsado en curso: ¿qué pasa con sus resultados y partidos restantes? | No | No | Sí, hasta F4/5B. Condición: RF-EQP-03 provisional | RF-EQP-03; HU-ORG-03; CL-17 | D (configurable antes de iniciar): evita imponer una regla que la organización puede tener distinta. |
| Q-27 | «Enfrentamiento directo» con más de dos equipos y subgrupos: ¿qué se compara y se recalcula? | No | No | Sí, hasta F4/5F. Condición: RD-TAB-06 provisional | RD-TAB-06, 07 | A, contrastada con el reglamento escrito de la organización. |
| Q-28 | Catálogo de criterios de desempate configurables y fair play | No | No | Sí, hasta F3/5F | RD-TAB-05 | Catálogo mínimo (DG, GF, GC, victorias, enfrentamiento directo); fair play exige ponderaciones no definidas. |
| Q-29 | Estados `aplazado`, `cancelado` y `suspendido`: ¿efecto en la tabla? | No | No | Sí, hasta F4. Condición: RD-RES-12 y la parte `suspendido` de RD-TAB-03 sin aprobar; el cierre desde `suspendido` no está habilitado | RD-RES-12; RD-TAB-03; CL-11, CL-71 | Decidir el efecto en la tabla y el marcador y la obligatoriedad del motivo; no se presume el cierre desde suspendido (Q-46). |
| Q-30 | Consistencia marcador–eventos | No | No | Sí, hasta F4/5E | RD-EVT-02, 04; RD-RES-03; CL-29 | B: permite finalizar con aviso sin inventar goleadores. |
| Q-31 | Plantillas: cierre, mínimos/máximos, categorías o edad | No | No | Sí, hasta F4/5B | RD-JUG-05 | Fecha de cierre configurable; sin mínimos, máximos ni elegibilidad por edad en el MVP. |
| Q-33 | ¿Puntos 3/1/0 fijos o configurables? | No | No | Sí, hasta F4 | RD-REG-06 | A: 3/1/0 leídos del reglamento, sin edición. |
| Q-40 | Estados `finalizado` y `archivado` (D-5 los define): (a) ¿quién puede archivar un torneo? (b) ¿puede reabrirse un `finalizado` o desarchivarse un `archivado`, y quién? (c) ¿qué otras operaciones se bloquean en `archivado` además de las correcciones del organizador? (d) ¿un torneo `archivado` sigue visible al público en solo lectura? | No | No; la parte (d) antes de F3 | Sí, hasta F4. Condición: la transición a `archivado` y sus efectos no se construyen hasta decidir | RD-RES-06; RF-TOR-02; RF-PUB-01; §9.6; matriz 14, 27, 37; CL-44 | (a) solo el dueño archiva, porque archivar quita facultades al organizador; (b) no reabrir `finalizado` ni desarchivar; (c) solo lectura salvo correcciones del dueño; (d) no visible mientras no haya archivo histórico (Q-11). Decisión suya. |
| Q-41 | ¿Se permiten traspasos de jugadores entre equipos del mismo torneo? (Q-12 rechaza la segunda inscripción de la misma persona) | No | No | Sí, hasta F4/5B. Condición: RD-JUG-10 (sin traspasos) | RD-JUG-10 | A, y consultar si el reglamento permite traspasos a mitad de torneo. |
| Q-42 | Facultades del organizador no cubiertas por decisiones: (a) registrar marcador/eventos y cambiar estados en partidos no finalizados de sus torneos (matriz 24–25); (b) anonimizar y exportar datos de jugadores (matriz 21, 34) | No | No | Sí, hasta F4/5C y 5E. Condición: las facultades del organizador en las filas 21, 24, 25 y 34 y la suspensión por el organizador no se habilitan | Matriz 21, 24, 25, 34; PRV-08; RD-RES-10 | (a) A: el organizador ya corrige por Q-09 y reanuda por Q-46, y cubre la contingencia N-12; (b) A: anonimizar y exportar datos personales exige el mínimo privilegio. |
| Q-43 | Criterio de autorización para incluir eventos, minutos, dorsales y equipos en el resumen exportable (D-8): ¿qué se considera divulgación autorizada que no expone datos protegidos y quién la autoriza? | No | No | Sí, hasta F4/5E y antes de datos reales. Condición: OFF-24 sin eventos | OFF-24; PRV-18; CL-46 | A hasta concluir la evaluación jurídica: el resumen conserva solo identificador, abreviaturas y marcador. |
| Persona responsable (acción de la organización) | La organización designará una persona responsable de custodiar las evidencias de autorización y atender solicitudes (Q-24) | No | No | Sí, hasta antes de tratar datos reales (PRV-15). Condición: solo datos sintéticos (PRV-17) | PRV-05, PRV-08 | Designarla pronto: custodia la evidencia y atiende solicitudes; sin ella no puede abrirse el piloto con datos reales. |
| Investigación de D-12 (acción de investigación) | Detallar cuáles son y cómo se combinan los métodos con que hoy se registran los resultados | No | No | Sí, hasta F3. Condición: no se asume ningún método actual | RNF-13; HU-EDI-08; UAT | Entrevistar a la organización; no inventar herramientas, personas, responsables ni procedimientos. |

### 13.6 Resumen

- **Confirmadas:** 21 decisiones (14 de la primera consolidación, Q-36, Q-37, Q-38, Q-39, Q-44, Q-45 y Q-46); consecuencias D-1…D-11 confirmadas; D-12 es información confirmada; RK-15 aceptado; RF-EQP-06 diferido.
- **Abiertas:** 18 (14 de la v1.0: Q-10, Q-13, Q-14, Q-20, Q-21, Q-22, Q-23, Q-26, Q-27, Q-28, Q-29, Q-30, Q-31, Q-33; y 4 derivadas: Q-40, Q-41, Q-42, Q-43), más dos acciones (persona responsable; investigación de D-12).
- **Bloquean la aprobación de la Fase 1:** ninguna. **Bloquean la Fase 2:** ninguna formalmente. **Antes de la Fase 3:** Q-14 y Q-40 (d); recomendada Q-20.
- Puede responder por identificador, por ejemplo «acepto las recomendaciones salvo Q-40 (a) opción B y Q-41 opción B».

### 13.7 Ambigüedades documentadas (sin decidir; no son preguntas registradas)

**Estado:** 14 ambigüedades abiertas (A-1, A-2, A-4, A-5, A-6 y A-8…A-16) y 2 resueltas (A-3, A-7). Las «lecturas provisionales» son **interpretaciones de trabajo, no decisiones del usuario**: no amplían ni reducen ningún permiso confirmado y se someten a confirmación en la fase indicada. Ninguna amplía en silencio el alcance de una restricción confirmada.

**Denegación por defecto (criterio provisional, no política definitiva).** Cuando una acción sensible es ambigua y su efecto alcanzaría partidos o decisiones sobre el propio equipo de un actor en conflicto (RT-14), mientras no haya decisión se lee con **denegación por defecto**, y la ejecuta otro actor sin conflicto. Este criterio **no cambia** la matriz, RT-14 ni RF-ACC-07: solo indica cómo leer los casos no cubiertos hasta la decisión; cada fila indica dónde se aplica. Las acciones no ambiguas siguen la matriz.

Ninguna ambigüedad bloquea la aprobación de la Fase 1; para el inicio de la Fase 2 ninguna es condición formal, y todas deben confirmarse o ajustarse a más tardar en la fase indicada (la Fase 2 puede considerarlas en el modelo de amenazas, sin resolverlas).

| # | Ambigüedad | Lectura provisional (no es decisión) | Requisitos afectados | Riesgo | Efecto sobre F1 / F2 | Decisión posterior |
|---|---|---|---|---|---|---|
| A-1 | Alcance de «resultados» en RT-14 | Toda operación que cree o modifique datos de resultado de partidos del propio equipo (denegación por defecto) | RT-14; RD-RES-06, 09; RD-WO-02, 05; matriz 24–28 | RK-16 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-2 | «Asignaciones que generen conflictos» | Solo la asignación de editores a partidos del propio equipo | RT-14; RF-ACC-06; matriz 7 | RK-16, RK-21 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-3 | Calendario | **Resuelta:** RT-14 se evalúa por efecto; generación, regeneración y ajustes quedan sujetos | RD-CAL-02, 06, 07 | — | — | — |
| A-4 | «Inscripción o retiro» | Solo las decisiones de inscripción o retiro, no la edición de nombre y colores | RT-14; RF-EQP-02, 03; matriz 16–17 | RK-16 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-5 | Verificación de duplicados: ¿a qué lado del par se aplica? | Cualquiera de los dos registros (denegación por defecto) | RT-14; RD-JUG-02, 08, 09; matriz 38 | RK-16 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-6 | «Propio equipo» sin vínculo de identidad | Rige dentro del mismo torneo; sin identidad común no cruza torneos | RT-14; RF-EQP-01, 06 (diferido); RT-12 | RK-18 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-7 | Evasión retirando el rol de delegado | **Resuelta por Q-45 (C)** | RT-14; RF-ACC-11, 17 | — | — | — |
| A-8 | Publicación y despublicación de jornadas | No restringida por RT-14 en esta versión | RD-CAL-08; §9.6; matriz 13 | RK-16 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-9 | Reanudar y luego finalizar emula el cierre desde `suspendido` | Permitido como dos operaciones autorizadas por separado, ambas auditadas; el cierre definitivo es asunto de Q-29 | RD-RES-12, 13; matriz 39, 41; Q-29 | RK-15 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-10 | Reanudación sin conexión por organizador o dueño | Solo en línea | RD-RES-13; OFF-25; matriz 39 | RK-15 | No bloquea F1 / No bloquea el inicio de F2 | F4 / 5E |
| A-11 | Alcance de «eventos nuevos» en `suspendido` frente al marcador directo y las anulaciones | Lectura estricta: tampoco se modifican durante la suspensión | RD-EVT-12; RD-RES-10 | RK-02 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-12 | «Sin conflicto» del aprobador; levantamiento y reotorgamiento del rol | Sin conflicto respecto del equipo y torneo del levantamiento; el levantamiento no cubre el conflicto vigente y se anula si se otorga de nuevo el rol | RT-14; RF-ACC-17 (S); matriz 40 | RK-22 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-13 | Preparación de partidos `suspendido` fuera de la ventana de 7 días | Solo preparación manual | OFF-02, OFF-25; §9.5 (ventana de preparación) | RK-11 | No bloquea F1 / No bloquea el inicio de F2 | F3 / 5E |
| A-14 | Eventos en `programado` con captura directa del final | RD-EVT-12 no los regula | RD-EVT-12; RD-RES-01 | RK-15 | No bloquea F1 / No bloquea el inicio de F2 | F4 |
| A-15 | Interacción de RT-14 con revocaciones de seguridad (RF-ACC-07), desasignación de editores y efectos indirectos: ¿puede un actor en conflicto revocar o desasignar a un editor cuando el efecto alcanza partidos de su propio equipo, o se bloquea una revocación de seguridad por conflicto? | **Sin resolver.** Texto vigente: RT-14 enumera siete clases de acción y evalúa por efecto; RF-ACC-07 y la matriz no se modificaron. Provisional: no se altera el permiso de revocar; la asignación y la reasignación sobre partidos propios siguen denegadas (RF-ACC-06, RT-14) y los efectos indirectos se auditan (RF-ACC-11, RF-AUD-06); las demás acciones sensibles no cubiertas expresamente se leen con denegación por defecto (criterio provisional, no política definitiva) | RT-14; RF-ACC-06, 07, 11; RF-AUD-06; matriz 6, 7; CL-35, 45, 52, 53 | RK-04, RK-16, RK-21 | No bloquea F1 / No bloquea el inicio de F2; conviene confirmarla antes de la matriz de F4 | F4 (revisable en el modelo de amenazas de F2) |
| A-16 | Reglamento y desempates: ¿puede un dueño u organizador en conflicto (RT-14) modificar el reglamento, los criterios de desempate o su configuración cuando el cambio favorece o afecta a su propio equipo? | **Sin resolver.** Texto vigente: RT-14 no enumera el reglamento ni los criterios; la edición antes de iniciar (matriz 11) está confirmada y el cambio con torneo iniciado sigue `COND` por Q-23. Provisional: no se añade restricción nueva ni se retira ninguna; los cambios de reglamento quedan auditados (RF-AUD-02) y la resolución manual de desempates (RD-TAB-09) sí es de RT-14 | RT-14; RD-REG-02, 03; RD-TAB-05, 09; matriz 11, 12, 29; Q-22, Q-23, Q-28 | RK-04, RK-07, RK-16 | No bloquea F1 / No bloquea el inicio de F2 | F4 |

Asimetría documentada: el organizador reanuda por Q-46, mientras que su facultad de suspender y de registrar resultados depende de Q-42 (a).

### 13.8 Requisitos con `COND` y fecha límite de resolución

| Requisito | Pregunta que lo condiciona | Resolver antes de |
|---|---|---|
| RF-TOR-02 | Q-40 | F4 (el punto (d) antes de F3) |
| RF-EQP-03 | Q-26 | F4 / 5B |
| RF-PUB-01 | Q-40 | F4 (el punto (d) antes de F3) |
| RF-ADM-04 | Q-10 | F3 / 5D |
| RD-REG-03 | Q-23 | F4 |
| RD-REG-06 | Q-33 | F4 |
| RD-TAB-03 | Q-29 | F4 |
| RD-TAB-05 | Q-28 | F3 / 5F |
| RD-TAB-06 | Q-27 | F4 / 5F |
| RD-TAB-09 | Q-22 | F4 / 5F |
| RD-WO-02 | Q-21 | F4 / 5E |
| RD-WO-03 | Q-21 | F4 / 5E |
| RD-WO-04 | Q-21 | F4 / 5E |
| RD-JUG-01 | Q-20 | F3 y F4 |
| RD-JUG-05 | Q-31 | F4 / 5B |
| RD-JUG-10 | Q-41 | F4 / 5B |
| RD-RES-10 | Q-42 | F4 / 5C y 5E |
| RD-RES-12 | Q-29 | F4 |
| RD-EVT-02 | Q-30 | F4 / 5E |
| RD-EVT-04 | Q-30 | F4 / 5E |
| OFF-24 | Q-43 | F4 / 5E y antes de datos reales |
| PRV-02 | Q-20 | F3 y F4 |
| PRV-08 | Q-42 | F4 / 5C y 5E |

**Total de requisitos con `COND`: 23.** Las filas de la matriz de permisos con `COND` son 12, 21, 24, 25, 26, 29, 34, 37 y 41.

---

## 14. Matriz de trazabilidad

### 14.1 Decisiones confirmadas → requisitos → riesgos → pruebas

| Decisión | Requisitos | Riesgos | Pruebas previstas | Se diseña / construye |
|---|---|---|---|---|
| Q-01 Costa Rica / responsable | PRV-01, 07, 13, 15, 16; RNF-10 | RK-05, RK-08 | D (revisión jurídica), E2E (aviso) | F2 ADR; F8 |
| Q-02 Roles y permisos (con Q-02a/b/c, Q-15, Q-35) | RF-ACC-01…17; RT-01…14; §7.1, §7.4; RD-JUG-06 | RK-03, RK-04, RK-16 | DB (actores de prueba de §7.3, 13), I (abuso) | F2, F4; 5C |
| Q-03 Reglamento | RD-REG, RD-TAB, RD-WO, RD-CAL, RD-FOR | RK-07, RK-09 | U (N-01, N-02, N-03 reformuladas), DB | F4; 5D, 5E, 5F |
| Q-04 / Q-18 MVP liga ampliado | §3; RD-EVT; RF-PUB; RF-ADM | RK-01, RK-10 | E2E, U | 5B–5G |
| Q-05 Menores | PRV-02…06, 09, 11; RD-JUG-01 | RK-05, RK-06 | DB, E2E, D | F3, F4; 5B |
| Q-06 Dimensiones y offline | OFF-01…25; RNF-04; RD-FOR-03 | RK-02, RK-11 | I (red cortada), E2E (iOS real), L | F2, F4; 5E |
| Q-07 Propiedad e infraestructura | RNF-08, 12, 14, 16, 17 | RK-08, RK-12 | D | F2, F8 |
| Q-17 Offline | OFF-01…25; RF-PUB-06; RF-PWA-02 | RK-02, RK-11 | I, E2E | F2; 5E, 5G |
| Q-19 Invitaciones | RF-ACC-02…05, 11, 15; RT-03, 07, 10; §7.1 filas 1–6 | RK-03 | DB, I | F4; 5C |

### 14.2 Casos del informe N-01…N-13

| Caso | Requisito(s) | Alcance en el MVP | Prueba |
|---|---|---|---|
| N-01 Calendario 7 y 8 equipos, 1 y 2 vueltas | RD-CAL-01 | Sí | U |
| N-02 Empate a puntos entre dos equipos | RD-TAB-04, 06, 07 (a) | Sí, **reformulado** al orden de Q-03 | U |
| N-03 Empate circular de 3 equipos | RD-TAB-06, 07 (b)–(e), 08, 09 | Sí, reformulado | U |
| N-04 Eliminatoria con 6 equipos (byes) | — | **No** (Q-18); Fase 6 | — |
| N-05 Empate en eliminatoria exige penales | — | **No** (Q-18); Fase 6 | — |
| N-06 Dos editores guardan el mismo partido | RD-RES-04; OFF-09; RF-ACC-06 | Sí, reformulado: con Q-36 A no hay dos editores vigentes; el conflicto se prueba entre editor y organizador, entre dos dispositivos del mismo editor y en la reasignación (CL-41, CL-47) | I, DB |
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
| H-09 Identidad de jugador | RD-JUG-02, 08, 09 (CONF Q-12) | F4, 5B |
| H-10 Dos fuentes de verdad | RD-TAB-10; RD-REG-06 | F2 |
| H-11 `has_role`/`EXECUTE` | RT-09 | F4 |
| H-12 Publicación sin modelo | RD-CAL-08; §9.6; RF-PUB-04 | F2 |
| H-13 Suscripciones push | Fuera del MVP (RF-PWA-04) | F6 |
| C-01 / C-02 | Resueltas por Q-19, Q-02 y Q-02a (CONF) | — |
| C-03 / C-04 | Resueltas por Q-18 (§3) | — |
| C-05 / C-06 | = H-02 / H-01 | — |
| C-07 / C-08 | Estimación (§15); RNF-12 | F2 |
| C-10 Inventario de 13 pantallas | Se actualiza al MVP ampliado en la Fase 3 | F3 |
| C-11 2FA vs correo y contraseña | RF-ACC-10 | F2 |
| C-12 / C-13 | PRV-02, 03; vistas públicas en F4 | F4 |
| C-14 Regenerar calendario | RD-CAL-07 | 5D |
| C-15 Disponibilidad sin SLA | **Resuelta** por Q-16 A (mejor esfuerzo en el piloto, sin porcentaje contractual): RNF-03 | F2 (reevaluar con garantías oficiales) |

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
| RF-16 Captura sin conexión | OFF-01…25 |
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
| RP-10 | RT-08; RF-TOR-04 | RP-28 | OFF-01…25 |
| RP-11 | RD-TAB-02; RD-CAL-01 | RP-29 | OFF-06 |
| RP-12 | RD-TAB-04…07; RD-REG | RP-30 | OFF-09; §9.4 |
| RP-13 | RD-WO-01…07 | RP-31 | OFF-04 |
| RP-14 | RD-FOR-02 (P) | RP-32 | Pruebas de §14.1 (se ejecutan en F7) |
| RP-15 | P (Fase 6) | RP-33 | RNF-14 |
| RP-16 | RD-REG-03 | RP-34 | RNF-12 |
| RP-17 | RD-EVT-01…11 | RP-35 | RNF-12 |
| RP-18 | RF-ACC; RF-ADM-05 | RP-36 | RF-ACC-09; RNF-17 |

### 14.6 Consolidación v1.1: cada decisión → requisitos → historias → riesgos → pruebas

| Decisión | Requisitos | Historias | Riesgos | Pruebas previstas | Casos límite |
|---|---|---|---|---|---|
| Q-02a | RF-ACC-12, 03, 15; RF-AUD-02; matriz 10 | HU-ORG-01 | RK-03 | DB: el creador queda organizador solo de su torneo y no ve los ajenos; I: creación y asignación atómicas; auditoría | CL-21 |
| Q-02b | RF-ACC-06; matriz 7; RT-04, 05; OFF-22 | HU-ORG-07, HU-EDI-01 | RK-03, RK-16 | DB: el editor no se autoasigna; el organizador en un partido ajeno es denegado; asignación a un delegado del equipo, rechazada; reasignaciones simultáneas | CL-20, CL-35, CL-41, CL-47 |
| Q-02c | RF-ACC-07; matriz 5–6; RT-06; OFF-11, 22 | HU-DUE-02 | RK-03, RK-15 | DB e I: revocación inmediata; un organizador no revoca organizadores ni al dueño; revalidación al sincronizar | CL-02, CL-03 |
| Q-08 | RF-ACC-10, 16 | — | RK-03 | I, E2E: 2FA obligatorio para dueño y organizador; opcional para editor | CL-28 |
| Q-09 | RD-RES-06; matriz 27; RD-TAB-11 | HU-DUE-07, HU-ORG-09 | RK-07 | I, E2E: corrección atómica con motivo e historial; torneo cerrado, solo el dueño; editor denegado (N-07) | CL-15, CL-34, CL-44 |
| Q-11 | RF-EQP-01; §3; §9.6 | — | RK-18 | DB: plantillas y estadísticas independientes por torneo | CL-23, CL-40 |
| Q-12 | RD-JUG-02, 08, 09, 10 | HU-DEL-04, HU-ORG-14 | RK-16 | DB e I: pendiente de verificación, rechazo de la segunda inscripción, carrera de altas, el delegado no ve datos ajenos (N-09) | CL-22, CL-36 |
| Q-15 | RF-ACC-08; RT-01, 04, 05, 11; §7.4 | — | RK-16 | DB: matriz de roles combinados; delegado no es editor de su equipo | CL-35 |
| Q-16 | RNF-03; RF-PUB-06, 07; RNF-17 | HU-PUB-07 | RK-02 | I, E2E: caída simulada del servidor | CL-12, CL-30 |
| Q-24 | PRV-05, 08, 15, 17; matriz 20 | HU-DEL-02 | RK-05, RK-06 | DB, E2E: solo metadatos y sin carga de archivos; D: datos sintéticos | CL-26 |
| Q-25 | PRV-13, 15, 17; RNF-12 | — | RK-05, RK-08, RK-19 | D: ADR con regiones, flujos y garantías verificadas; conclusión jurídica | — |
| Q-32 | RD-RES-07; RF-PUB-04, 09; OFF-15 | HU-PUB-02 | RK-02 | I, E2E: matriz de visibilidad §9.6 (12 combinaciones) | CL-01, CL-30 |
| Q-34 | OFF-02, 10, 11, 17, 20, 21, 22, 23, 24; §9.4, §9.5 | HU-EDI-02…11 | RK-02, RK-11, RK-15, RK-17 | I, E2E: una prueba por cada fila de §9.4; reloj simulado a 23/29/31 días; iPhone real | CL-01…14, 37, 39 |
| Q-35 | RF-ACC-09; RT-06; matriz 4, 35 | HU-DUE-06 | RK-12 | DB e I: autenticación reforzada, último dueño activo, organizador denegado | CL-28, CL-38 |

**Requisitos sin prueba propia:** RF-TOR-03 (M) remite a las pruebas de RD-REG-01…06; las demás filas con «—» son de prioridad P (RF-EQP-04, RD-JUG-07, RF-PWA-04). Ningún otro requisito M carece de prueba prevista.


### 14.7 Segunda consolidación: Q-36, Q-37 y D-1…D-12 → requisitos → historias → riesgos → pruebas

| Decisión | Requisitos | Historias | Riesgos | Pruebas previstas | Casos límite |
|---|---|---|---|---|---|
| Q-36 | RF-ACC-06; RT-13; §9.4 filas 10–11; OFF-22 | HU-ORG-07, HU-EDI-06 | RK-04, RK-15, RK-21 | DB, I: reasignación atómica con carrera; I, E2E: el editor anterior sincroniza después | CL-41, CL-47 |
| Q-37 | RF-EQP-01, RF-EQP-06 (**diferido**); RT-08, RT-12; matriz 36 (diferida) | HU-DUE-08 (P) | RK-18 | DB: aislamiento entre torneos; las pruebas del vínculo se activan cuando se construya | CL-40, CL-42 (diferido) |
| D-1 | RF-ACC-03, RF-ACC-12 | HU-ORG-01 | RK-03 | DB, I: el organizador no administra torneos ajenos | CL-21 |
| D-2 | RF-ACC-07; OFF-22; RT-13 | HU-DUE-02 | RK-03, RK-21 | I: medición del objetivo de 60 s en Fase 2; DB | CL-03, CL-45 |
| D-3 | RF-ACC-02, RF-ACC-12; RT-04 | — | RK-03, RK-16 | DB, I: autoasignación denegada; excepción de la creación de torneo | CL-20 |
| D-4 | RF-ACC-09; RT-06; matriz 35 | HU-DUE-06 | RK-12, RK-21 | DB, I: 2FA, motivo, carrera de revocaciones mutuas | CL-28, CL-38, CL-43 |
| D-5 | RD-RES-06; RF-TOR-02; matriz 14, 27, 37; Q-40 | HU-DUE-07, HU-ORG-09 | RK-07 | I, E2E: corrección en `finalizado` y en `archivado` | CL-34, CL-44 |
| D-6 | RD-JUG-02, 08, 09 | HU-DEL-04, HU-ORG-14 | RK-16 | DB, I, E2E | CL-22, CL-36 |
| D-7 | OFF-10, OFF-17; PRV-10 | HU-EDI-10 | RK-17 | I, E2E: reloj simulado a 23, 29 y 31 días | CL-37 |
| D-8 | OFF-24, OFF-11; PRV-18; Q-43 | — | RK-20 | I, E2E, D: exportación sin nombres, dorsales ni eventos | CL-46 |
| D-9 | OFF-20; RF-ADM-04; PRV-10 | HU-ORG-13 | RK-15 | I, DB: aislamiento por torneo; una operación nunca recibida no aparece | CL-02, CL-47 |
| D-10 | PRV-15, PRV-17 | — | RK-05, RK-19 | D: revisión de semillas y entornos | — |
| D-11 | OFF-22, 11, 24; RT-13; §9.4 | HU-EDI-06 | RK-15, RK-21 | I, DB | CL-01…05, CL-45, CL-47 |
| D-12 | RNF-13; §13.4 | HU-EDI-08 | RK-13 | M: línea base tras la investigación (sin requisito técnico nuevo) | — |

**Precisiones de la v1.3:** D-3 coincide con Q-38 (c). D-4, D-5 y D-11 se aplican junto con RT-14 (conflicto de interés) y con la unión de permisos de RT-11: si el único actor habilitado está en conflicto, la operación se bloquea (por ejemplo, la corrección de un torneo `archivado` cuando el dueño único es delegado de un equipo del partido). D-8 y D-11 se mantienen sin cambios (RK-15 aceptado).


### 14.8 Tercera consolidación: Q-38, Q-39, Q-44, Q-45, Q-46, RK-15 y RF-EQP-06 → requisitos → historias → riesgos → pruebas

| Decisión | Requisitos | Historias | Riesgos | Pruebas previstas | Casos límite |
|---|---|---|---|---|---|
| Q-38 | RT-14, RT-11, RT-05; RF-ACC-07, 08, 13; §7.4; matriz fila 6 | HU-ORG-15, HU-ORG-16 | RK-04, RK-16 | DB, I: matriz de roles combinados; revocación parcial; un equipo por torneo | CL-50, 52, 53, 54 |
| Q-39 | RF-ACC-10 | — | RK-03 | I, E2E: política de 2FA por todos los roles vigentes | CL-55 |
| Q-44 | RT-14; RF-ACC-06; RD-CAL-02, 06, 07; RD-RES-06, 09; RD-WO-02, 05; RD-TAB-09; RD-JUG-02; RF-EQP-02, 03; RF-AUD-06; OFF-22; matriz 7, 16, 22–29, 38 | HU-ORG-15, HU-DUE-07 | RK-16, RK-22 | DB, I, E2E por clase de acción | CL-48…51, 56…58 |
| Q-45 | RT-14; RF-ACC-11, RF-ACC-17; RF-AUD-02, 06; PRV-10; matriz 40 | HU-DUE-09 | RK-22 | DB, I: persistencia, levantamiento, sin autoaprobación, historial intacto | CL-59…65 |
| Q-46 | RD-RES-01, 10, 12, 13; RD-EVT-12; OFF-01, 07, 25; matriz 39, 41; §9.4 (6)–(8) | HU-EDI-12, HU-ORG-17 | RK-15, RK-02 | U, DB, I, E2E: reanudación, eventos en suspensión, secuencias | CL-66…72 |
| RK-15 aceptado | RK-15; OFF-10, OFF-24; D-8, D-11 | — | RK-15 | D: ninguna función de recuperación excepcional; I: reanudación rechazada → eventos no publicados | CL-47, CL-69 |
| RF-EQP-06 diferido | RF-EQP-06, RF-EQP-01; RT-12; matriz 36; HU-DUE-08 | HU-DUE-08 | RK-18 | I, D: sin operación ni interfaz de vinculación | CL-40, CL-42 (diferido) |

---

## 15. Estimación de esfuerzo actualizada (MVP ampliado con funcionamiento sin conexión) — v1.3

**Naturaleza de la cifra.** Es una estimación **inicial** de ingeniería en días-persona, elaborada por este análisis **solo a partir de los requisitos consolidados** y sin datos históricos de velocidad del equipo; confianza **baja a media**. No es un compromiso de plazo ni de costo. Debe recalibrarse tras el módulo 5A y cuando se resuelvan los asuntos abiertos. **Los totales se recalcularon sumando las filas de la tabla** (no son una copia de las proyecciones previas).

**Supuestos:** una persona desarrolladora (Q-01); dedicación completa = 4,5 días productivos por semana; media jornada = 2,5; el esfuerzo incluye las pruebas de cada módulo, pero no el tiempo de espera de decisiones, de la **evaluación jurídica** (que condiciona el uso de datos reales, PRV-15), de verificación de proveedores ni de la organización para pruebas de aceptación; el piloto exige además que se juegue un torneo real, cuya duración depende del calendario deportivo.

**Cambios respecto de la v1.2 (por Q-38, Q-39, Q-44, Q-45, Q-46 y el diferimiento de RF-EQP-06):** se retira la fila «5B vínculo de identidad» (−1/−1/−2); 5B +1/+1/+1 (RT-14 en inscripción, retiro y duplicados); 5C +2/+2/+3 (RT-14 por efecto, historial inmutable, política de 2FA por roles, revocación parcial, levantamiento RF-ACC-17); 5D +1/+1/+1; 5E en línea +1/+1/+1 (reanudación y RD-EVT-12); modo sin conexión +1/+2/+3 (OFF-25 y revalidación de RT-14); Fase 7 +1/+1/+2 (pruebas de carrera y de secuencias). **Total v1.2: 117 / 144 / 178.**

| Fase / módulo | Mínimo | Probable | Máximo | Notas |
|---|---|---|---|---|
| Fase 2 · Arquitectura y amenazas | 6 | 8 | 10 | ADR de stack, offline-first, modelo de amenazas, evaluación de proveedores y transferencias |
| Fase 3 · UX/UI | 10 | 12 | 15 | Inventario ampliado al MVP; wireframes, sistema visual, estados offline |
| Fase 4 · Diseño técnico | 8 | 10 | 12 | Esquema, matriz RLS, contratos, plan pgTAP |
| 5A · Infraestructura local y autenticación | 7 | 8 | 10 | Incluye 2FA por roles vigentes, entornos y datos sintéticos |
| 5B · Torneos, equipos, inscripciones, jugadores | 11 | 13 | 16 | Verificación de duplicados con estado pendiente, concurrencia y RT-14 (inscripción, retiro, duplicados) |
| 5C · Roles, RLS, invitaciones, revocación, auditoría | 14 | 16 | 21 | RT-14 por efecto, historial inmutable de asignaciones, revocación parcial, segundo dueño, RF-ACC-17 (S) |
| 5D · Calendario de liga, jornadas, asignación de editores | 10 | 12 | 14 | Reasignación atómica; guardias de RT-14 sobre generación y ajustes |
| 5E · Resultados, eventos, correcciones, W.O. (en línea) | 9 | 11 | 13 | Estados `finalizado`/`archivado`, suspensión y reanudación, RD-EVT-12 |
| 5E · **Modo sin conexión** (cola, idempotencia, conflictos, revocación, iOS) | 14 | 19 | 25 | Mayor incertidumbre; OFF-25 (secuencias con reanudación) y revalidación de RT-14 |
| 5F · Tabla, goleadores, tarjetas y vista pública con caché | 8 | 10 | 12 |  |
| 5G · PWA, red débil, E2E y piloto | 8 | 10 | 12 |  |
| Fase 7 · QA residual y endurecimiento | 14 | 17 | 21 | pgTAP completo, pruebas de carrera y de secuencias, iOS real, carga, restauración |
| Fase 8 · Entrega y operación | 4 | 5 | 6 |  |
| **Total** | **123** | **151** | **187** | días-persona (v1.2: 117 / 144 / 178; v1.1: 111 / 138 / 170; v1.0: 107 / 134 / 163) |

| Dedicación | Mínimo | Probable | Máximo |
|---|---|---|---|
| Completa (4,5 d/semana) | ≈ 27 semanas | ≈ 34 semanas | ≈ 42 semanas |
| Media jornada (2,5 d/semana) | ≈ 49 semanas | ≈ 60 semanas | ≈ 75 semanas |

Si se difiere RF-ACC-17 (S), el esfuerzo baja en torno a 1 día; mientras tanto no existirá mecanismo para levantar conflictos históricos.

**Comparación con el informe.** El informe estimaba 5–7 semanas para su MVP (liga sin goleadores, tarjetas, W.O. configurable, desempates configurables ni modo sin conexión) con 2–3 desarrolladores (≈ 50–105 días-persona a 5 d/semana); el alcance confirmado es mayor y esa estimación **ya no es aplicable**. Con una sola persona no hay paralelismo.

**Aún no estimado:** el efecto de los 18 asuntos abiertos de §13 (en especial Q-29, Q-40 y Q-42).

**Requisitos S (candidatos a recorte, con aprobación):** RF-ACC-17, RF-EQP-05, RF-PUB-05, RF-AUD-05, RD-CAL-05, RD-CAL-09, RD-REG-05, RD-TAB-12, RD-RES-08, RD-EVT-11, OFF-19.

**Sensibilidad (informativa; no propone cambiar decisiones confirmadas).**

| Variación | Efecto aproximado |
|---|---|
| Sustituir el modo sin conexión completo por captura en línea con reintento | −8 a −14 días (contradice Q-17; solo referencia) |
| 2FA obligatorio para editores o delegados (reevaluación posterior al piloto) | +2 a +3 días |
| Fotos y escudos (posterior) | +4 a +6 días |
| Verificación de proveedores/planes gratuitos desfavorable | Puede añadir días de migración o diseño en la Fase 2; no estimable ahora |

**Puertas externas que pueden alargar el calendario:** evaluación jurídica (PRV-15, PRV-18, historial de delegaciones), formalización de responsabilidades (PRV-16), verificación oficial de costos, respaldos y garantías (RNF-12, PRV-13), logotipo y colores (Q-14), designación de la persona responsable de evidencias (Q-24), investigación de D-12, dispositivos reales para pruebas y disponibilidad de la organización para el piloto. Mientras no concluya la evaluación jurídica solo pueden usarse datos sintéticos (PRV-17).

---

## 16. Riesgos principales de la especificación

| ID | Riesgo | Probabilidad / impacto | Mitigación prevista | Seguimiento |
|---|---|---|---|---|
| RK-01 | Crecimiento de alcance (el MVP ya es amplio) | Alta / Alto | Prioridades M/S/P; cualquier recorte o adición pasa por aprobación; S se recorta primero | F1 aprobación; cada módulo |
| RK-02 | Complejidad del modo sin conexión (conflictos, idempotencia, migraciones de cola) | Alta / Alto | Política §9.4; ADR en F2; pruebas de integración con red cortada; prototipo temprano en 5E ; OFF-25 (secuencias y reanudación) | F2, 5E |
| RK-03 | Fallo de autorización o fuga entre torneos | Media / Muy alto | Actores de prueba de §7.3 (13) en BD real; RT-08; revisión de seguridad; sin superusuario en pruebas | F4, 5C, F7 |
| RK-04 | Reglas ambiguas de asignación, revocación o varios roles | Baja / Alto (residual) | Decisiones confirmadas Q-02a/b/c, Q-15, Q-36, Q-38 y Q-44; residual: las 14 ambigüedades abiertas de §13.7 (A-1, A-2, A-4, A-5, A-6 y A-8…A-16; A-3 y A-7 están resueltas) | F2, F4 |
| RK-05 | Incumplimiento de privacidad de menores | Media / Muy alto | PRV-02, 03, 06; revisión jurídica obligatoria; sin datos reales antes | PRV-15 |
| RK-06 | Pérdida de la evidencia de consentimiento o de su validez | Media / Alto | Responsable designado; metadatos; revisión jurídica | Q-24 |
| RK-07 | Reglas deportivas distintas del reglamento real (desempates, W.O., retiro) | Media / Alto | Contrastar RD-TAB-06, RD-WO, Q-26/27 con el reglamento escrito de la organización; fixtures aprobados | F4 |
| RK-08 | Planes gratuitos o regiones inadecuados (restricciones comerciales, pausas, respaldos, ubicación) | Media / Alto | Verificación oficial y evaluación jurídica de la infraestructura internacional (Q-25, PRV-13); ADR con alternativa; sin contratar antes | F2 |
| RK-09 | Dos fuentes de verdad para la tabla | Media / Medio | RD-TAB-10 | F2 |
| RK-10 | Expectativa del tamaño del MVP frente a una sola persona (≈ 27–42 semanas a tiempo completo, §15) | Alta / Medio | Comunicar la estimación; recalibrar tras 5A | F1 |
| RK-11 | Comportamiento de iOS: sin sincronización en segundo plano y posible limpieza de almacenamiento | Alta / Medio | OFF-05, OFF-17; pruebas en iPhone real | F2, 5G |
| RK-12 | Persona única mantiene el sistema (continuidad); un dueño comprometido podría revocar al otro | Media / Alto | RF-ACC-09 (2FA, motivo, auditoría, protección atómica), RNF-14, RNF-17; notificación al dueño afectado [REC] | F8 |
| RK-13 | Meta de captura (< 30 s) sin línea base | Media / Medio | Medir proceso actual (Q-08) y UAT | F3, F7 |
| RK-14 | Equilibrio de localías del método del círculo | Baja / Bajo | RD-CAL-09; ajustes manuales | 5D |
| RK-15 | **RIESGO RESIDUAL EXPRESAMENTE ACEPTADO POR EL USUARIO.** Pérdida de capturas legítimas: se rechaza toda operación con autorización revocada (D-11) y la exportación se limita a identificador, abreviaturas y marcador (D-8), por lo que los **eventos** de una captura rechazada pueden no recuperarse; el organizador solo ve metadatos de lo que el servidor recibió (D-9) | Media / Medio | Conservar según OFF-10; metadatos visibles al organizador (OFF-20); corrección por el organizador (RD-RES-06); advertir al reasignar (RF-ACC-06); comunicar que reasignar o revocar con operaciones pendientes puede rechazarlas; opcional por decidir en F3: alerta de partidos sin resultado confirmado (no es un requisito creado) ; sin recuperación excepcional de eventos offline rechazados en el MVP (no se afirma que toda captura rechazada pueda recuperarse); incluye los eventos posteriores a una reanudación rechazada (OFF-25); se mantienen D-8 y D-11 | 5E |
| RK-16 | Conflicto de interés o fuga por roles combinados y por la verificación de duplicados (el delegado no debe ver datos de otros equipos) | Media / Alto | RT-05, RT-11, RT-14 (Q-38, Q-44, Q-45); avisos genéricos (RD-JUG-02); pruebas de matriz con roles combinados | F4, 5C |
| RK-17 | Retención de 30 días y eliminación de datos locales frente a pérdida de trabajo; el navegador o un dispositivo desconectado pueden impedir avisos y conservación | Media / Medio | Plazo informado, avisos «cuando sea técnicamente posible», constancia mínima, exportación limitada, política PRV-10; sin promesa de conservación (OFF-17) | 5E, F8 |
| RK-18 | Gobierno de la identidad de equipo entre torneos (el vínculo está diferido; riesgo de diseño: que el modelo impida o exponga el vínculo futuro) | Baja / Medio (residual) | Q-37 B confirmada; RF-EQP-06 diferido; RT-08, RT-12; CL-40, CL-42 (diferido) | F2, F4 |
| RK-19 | La puerta jurídica (PRV-15) impide cargar datos reales y retrasa el piloto | Media / Alto | Datos sintéticos (PRV-17); iniciar la evaluación jurídica en paralelo con las Fases 2–4 | F2–F8 |
| RK-20 | Identificación indirecta de menores (equipo + dorsal, minutos, estadísticas, exportaciones y registros) | Media / Muy alto | PRV-02, PRV-18, OFF-24, Q-20, Q-43; evaluación jurídica; sin eventos en exportaciones por defecto | F3, F8 |
| RK-21 | Operaciones simultáneas de autorización (revocaciones, reasignaciones, revocación mutua de dueños, altas concurrentes de jugadores) | Media / Alto | RT-13, RF-ACC-06, RF-ACC-09, RD-JUG-08; pruebas de carrera CL-41, CL-43, CL-45 | F4, 5C, 5D |
| RK-22 | Bloqueo operativo por conflicto de interés (vigente o histórico) sin autoridad alterna: dueño único en conflicto, único organizador en conflicto o torneo `archivado`; sin RF-ACC-17 no existe vía de levantamiento | Media / Medio | Segundo dueño (RF-ACC-09), otro organizador, levantamiento por otro dueño (RF-ACC-17, S); bloqueo auditado (RF-AUD-06); sin excepciones automáticas | F4, 5C |

---

## 17. Criterios para aprobar la Fase 1, auditoría final y dictamen técnico (v1.3)

### 17.1 Lista de verificación de revisión

1. ¿Las secciones `CONF` reflejan fielmente sus decisiones (§2.1, §2.1b, §2.1c) y las confirmaciones D-1…D-12 (§2.2)?
2. ¿Acepta el tratamiento de D-5 (`finalizado` frente a `archivado`), de D-8 (exportación limitada) y de RK-15 como riesgo aceptado, con sus consecuencias (§17.3)?
3. ¿Acepta la clasificación de los 18 asuntos abiertos (§13.5) y las lecturas provisionales de las 14 ambigüedades abiertas (§13.7), entendidas como interpretaciones de trabajo y no como decisiones? Los requisitos con `COND` siguen fuera de lo aprobado.
4. ¿Acepta la matriz de permisos v2 de 41 acciones, RT-14 y los roles combinados (§7)?
5. ¿Acepta la separación MVP / posterior (§3), las prioridades M/S/P y la lista de requisitos S candidatos a recorte, incluido RF-ACC-17 (§15)?
6. ¿Acepta las reglas deportivas (§8), la secuencia de suspensión y reanudación, el diseño de captura sin conexión (§9) y el tratamiento de privacidad como borrador sujeto a evaluación jurídica (§10)?
7. ¿Acepta la estimación de §15 como referencia inicial?

### 17.2 Criterios de aceptación de la fase (autoevaluación; la aprobación es suya)

| Criterio | Evaluación de este análisis |
|---|---|
| Requisitos funcionales y no funcionales con ID, prioridad y aceptación medible | Cumplido: 186 requisitos; los 169 M tienen aceptación propia y prueba prevista; 23 con `COND` indican dónde falta decisión |
| Historias para los cinco perfiles | Cumplido (51 historias, §5) |
| Matriz de permisos con torneo, partidos asignados, invitaciones y revocación | Cumplido (41 acciones, 14 restricciones, §7.4 roles combinados); filas `COND`: 12, 21, 24, 25, 26, 29, 34, 37 y 41 |
| Reglas deportivas del MVP | Cumplido (§8, 72 reglas); pendientes Q-21…Q-23, Q-26…Q-31, Q-33, Q-40, Q-41 |
| Offline: sincronización, duplicados, conflictos, revocación, publicación | Cumplido y **confirmado** (§9, 25 requisitos); pendiente Q-43 (eventos exportables) |
| Privacidad y menores con puntos [PEND-JUR] | Cumplido como borrador (§10, 18 requisitos); depende de evaluación jurídica, de Q-20 y de la persona responsable por designar |
| MVP vs posterior | Cumplido (§3); RF-EQP-06 diferido (P); RF-ACC-17 en S |
| Preguntas abiertas documentadas sin decidir por el usuario | 21 confirmadas por el usuario; 18 abiertas documentadas, clasificadas y con recomendación; **ninguna decidida por el análisis** |
| Trazabilidad decisiones–requisitos–riesgos–pruebas | Cumplido (§14, §14.6 a §14.8, §16) |
| Estimación actualizada | Cumplido (§15), con confianza baja a media |
| Coherencia con las 21 decisiones | Verificada (§17.3); sin contradicciones remanentes entre decisiones confirmadas |

### 17.3 Auditoría de consistencia de la consolidación

**Contradicciones y ambigüedades resueltas en la v1.1 (V-1…V-14)** — véase §0.4.

**Contradicciones y ambigüedades resueltas en la v1.2**

| # | Hallazgo | Corrección |
|---|---|---|
| V-15 | La definición previa de «cerrado definitivamente» (`finalizado` o `archivado`) contradice D-5 (el organizador puede corregir en `finalizado`) | Sustituida en RD-RES-06, RF-TOR-02, matriz 27, CL-34; sin definición simultánea |
| V-16 | El resumen exportable de la v1.1 incluía eventos, minutos, dorsal y equipo, contra la protección de menores de D-8 | OFF-24: solo identificador, abreviaturas y marcador; eventos únicamente con divulgación autorizada (Q-43) |
| V-17 | La coincidencia dentro del mismo equipo solo generaba advertencia, contra «toda inscripción con posible duplicado queda pendiente» (D-6) | RD-JUG-02 incluye cualquier coincidencia dentro del torneo |
| V-18 | §9.4 fila 11 («dos editores asignados») contradice Q-36 A | Fila tachada y sustituida por el escenario de reasignación; nuevo CL-47 |
| V-19 | OFF-20 registraba metadatos de operaciones aplicadas además de las rechazadas; D-9 limita a las rechazadas o en conflicto efectivamente recibidas | OFF-20 y RF-ADM-04 alineados; las nunca recibidas no son visibles |
| V-20 | «Sesiones en ≤ 60 s» se presentaba como parámetro firme; D-2 lo define como objetivo sin garantía para dispositivos desconectados | RF-ACC-07 y D-2 reformulados |
| V-21 | Q-38 (c) («autogestión de roles») estaba abierto aunque D-3 lo resuelve | Q-38 (c) marcado como resuelto por D-3 |
| V-22 | RT-04 (nadie se concede roles) frente a la asignación automática del creador de torneo (Q-02a) | Excepción expresa: operación del sistema autorizada previamente |
| V-23 | RF-EQP-01 permitía identidad común sin definir quién la gobierna; riesgo de acceso indirecto entre torneos | Q-37 B: solo el dueño; RT-12; CL-40 y CL-42 |
| V-24 | Q-40 mezclaba una definición ya resuelta (D-5) con preguntas de reapertura | Reformulado con cuatro subpuntos (a)–(d) |
| V-25 | Revocaciones, reasignaciones y revocación mutua de dueños sin requisito de atomicidad | RT-13; CL-41, CL-43, CL-45 |
| V-26 | La protección del último dueño no cubría operaciones simultáneas | RF-ACC-09 y RT-06 con protección atómica |
| V-27 | La utilidad de la exportación (D-8) y el rechazo total (D-11) dejan sin recuperación los eventos de capturas rechazadas | Consecuencia explícita en RK-15 (no se crea ningún requisito de recuperación) |
| V-28 | `archivado` ocultaba al público el resultado final de un torneo cerrado (v1.1) sin que D-5 lo decida | Queda como Q-40 (d), con `archivado` no visible «por ahora» |
| V-29 | PRV-02 etiqueta a un menor con equipo + dorsal: posible identificación indirecta (D-8) | PRV-18, RK-20, Q-20 y Q-43; no se declara resuelto |

**Contradicciones y ambigüedades detectadas y resueltas en la v1.3**

| # | Hallazgo | Corrección |
|---|---|---|
| V-30 | RT-14 se evaluaba por tipo de operación, lo que permitía eludirlo con una regeneración general del calendario | Alcance por efecto sobre partidos, con efectos automáticos y en cascada; rechazo de la operación completa; RD-CAL-02, 06 y 07 |
| V-31 | Retirar temporalmente el rol de delegado evadía el conflicto (A-7) | Q-45 C: persistencia histórica con levantamiento solo por otro dueño sin conflicto (RT-14, RF-ACC-17) |
| V-32 | RD-RES-10 tenía la aceptación vacía y mezclaba la suspensión con Q-29 | Separado en RD-RES-10 (suspender), RD-RES-13 (reanudar) y RD-RES-12 (cierre y estados pendientes de Q-29) |
| V-33 | RD-TAB-03 anticipaba el efecto de `suspendido` en la tabla | Esa parte queda `COND:Q-29` |
| V-34 | OFF-22 y D-11 («se rechaza lo revocado») frente a Q-38 (d) (no se eliminan permisos de otros roles) | OFF-22: se rechaza la operación sin autorización vigente por **cualquier** rol de la cuenta |
| V-35 | §8.7 daba «aplazado» por reprogramación y «cancelado» por pendiente, anticipando Q-29 | Ninguno es estado del MVP hasta decidir Q-29; la reprogramación es una operación independiente |
| V-36 | RF-EQP-01 dependía de RF-EQP-06 para conservar la identidad | Vínculo diferido; el modelo no debe impedirlo; sin operaciones ni interfaces en el MVP |
| V-37 | Q-46 (sin eventos en `suspendido`) frente a §9.4 fila 1 (los eventos se aceptan) | Precisión (7) sin tocar las 12 filas confirmadas; RD-EVT-12 y OFF-25 |
| V-38 | Cinco requisitos M solo remitían a otra sección como criterio de aceptación | RF-PWA-03, RD-JUG-06, RD-RES-05, OFF-15 y OFF-18 con criterio propio |
| V-39 | RF-TOR-03 sin prueba propia | Trazabilidad precisa a las pruebas de RD-REG-01, 04 y 06 |
| V-40 | El historial necesario para evaluar el conflicto podía perderse con la revocación de un rol | RF-ACC-11: registros de solo anexado; conservación y anonimización pendientes de validación jurídica |

**Revisión por tema**

| Tema | Resultado |
|---|---|
| Escaladas de privilegios | Sin vías abiertas: solo el dueño crea organizadores y dueños (RF-ACC-03, 09; RT-03, 06); nadie se concede roles (RT-04); el levantamiento de RT-14 no concede permisos (RF-ACC-17); la reanudación se limita a tres roles (RD-RES-13) |
| Roles combinados | Cubiertos: delegado+editor (RT-05), dueño u organizador con rol de delegado (RT-14), un equipo por torneo (RF-ACC-13), unión de permisos (RT-11) |
| Revocaciones y operaciones simultáneas | RT-13; CL-45, CL-52, CL-53, CL-61 (pruebas previstas, no ejecutadas) |
| Conflictos históricos y levantamiento | Persistencia por torneo; levantamiento sin autoaprobación y con otro dueño sin conflicto; sin ese dueño la restricción permanece (CL-59 a CL-62); la ausencia de RF-ACC-17 deja el conflicto sin vía de levantamiento |
| Secuencias offline | OFF-25 y CL-69, 72: una reanudación rechazada o en conflicto nunca permite aplicar ni publicar los eventos posteriores; huecos y duplicados controlados |
| Eventos en `suspendido` | RD-EVT-12 y CL-70; lectura estricta pendiente de confirmación (A-11) |
| Filtración entre torneos | RT-08, RT-12; metadatos acotados por torneo (OFF-20); CL-21, CL-40 |
| Publicación de resultados | Solo lo confirmado por el servidor en jornadas visibles (RD-RES-07, §9.6); la reanudación pendiente no cambia el estado oficial |
| Datos personales y menores | Sin cambios sustanciales: PRV-15 a PRV-18; el historial de delegaciones guarda cuenta, equipo, torneo y fechas; validación jurídica pendiente |
| Integridad de resultados | Correcciones auditadas; el cierre desde `suspendido` no está habilitado (RD-RES-12) |
| Requisitos M sin prueba o criterio | Ninguno (verificación mecánica) |
| Requisitos de fases posteriores dentro del MVP | No se detectan requisitos P en el MVP; RF-EQP-06 está diferido; RF-ACC-17 (S) es recortable |
| Duplicados o contradictorios | Duplicados controlados por referencia (RD-RES-05/OFF-06, RF-PWA-03/OFF-16, RD-WO-07/RD-EVT-04, OFF-11/OFF-24); sin reglas contradictorias detectadas |

**Asuntos aún pendientes de decisión (18):** Q-10, Q-13, Q-14, Q-20, Q-21, Q-22, Q-23, Q-26, Q-27, Q-28, Q-29, Q-30, Q-31, Q-33, Q-40, Q-41, Q-42, Q-43; y dos acciones: persona responsable de evidencias e investigación de D-12 (§13.5).

**Verificaciones de texto realizadas sobre el documento resultante (no son pruebas del producto):** 0 identificadores citados sin definir; todas las tablas con columnas consistentes; conteos por prefijo recalculados contra el documento (186 requisitos: 54 RF generales, 72 RD, 25 OFF, 18 PRV y 17 RNF; 169 M, 11 S, 6 P; 23 con `COND`; matriz de 41 filas; 14 RT; 72 casos límite; 51 historias; 22 riesgos; cifras confirmadas de nuevo en la v1.3.1, §17.5); ningún requisito M sin criterio ni prueba propios; ninguna mención de Q-38, Q-39, Q-44, Q-45 o Q-46 como abiertas; RF-EQP-06 como P.

### 17.4 Dictamen técnico independiente (recomendación, no aprobación) — **evidencia histórica, anterior a la aprobación formal (§17.6)**

**APROBABLE CON CONDICIONES.**

- *Fundamento:* las 21 decisiones, las consecuencias D-1…D-12, RK-15 aceptado y el diferimiento de RF-EQP-06 están incorporados en requisitos, matriz, reglas, modo sin conexión, privacidad, casos límite, trazabilidad, riesgos y estimación; no queda contradicción material entre decisiones confirmadas; todo requisito M tiene aceptación y prueba prevista; ningún asunto abierto bloquea la aprobación.
- *Condiciones:*
  1. Aceptar que los 18 asuntos abiertos mantienen sus requisitos en `COND` (23 requisitos), fuera de lo aprobado.
  2. Decidir Q-14 y Q-40 (d) antes de la Fase 3; recomendado Q-20.
  3. Mantener PRV-15, PRV-17 y PRV-18: ningún dato personal real hasta concluir la evaluación jurídica y designar a la persona responsable de evidencias; mantener pendiente la validación jurídica de la conservación y anonimización del historial de delegaciones.
  4. Confirmar o ajustar las lecturas provisionales de las 14 ambigüedades abiertas de §13.7 (incluidas A-15 y A-16) a más tardar en la fase indicada en cada fila.
  5. Considerar que, si se difiere RF-ACC-17, todavía no existirá mecanismo para levantar conflictos históricos.
  6. Considerar el recorte de los requisitos S si el calendario lo exige (§15).
- Este dictamen **no aprueba la Fase 1 ni autoriza la Fase 2**: ambas decisiones son suyas.

### 17.5 Corrección documental v1.3.1: trazabilidad de los hallazgos de la auditoría independiente final

Corrección **solo documental** autorizada por el mensaje «CORRECCIÓN DOCUMENTAL FINAL v1.3.1». No cambia decisiones, requisitos (siguen 186), permisos ni la máquina de estados, y no afirma que ningún control esté implementado.

| Hallazgo | Sev. | Tratamiento | Dónde | Estado |
|---|---|---|---|---|
| H-01 | MEDIO | Marcadores de plantilla de §17.3 sustituidos por los conteos reales; no se encontraron otros marcadores residuales (los `{dorsal}`, `{equipo}` de PRV-02 son texto de requisito, no plantilla) | §17.3 «Verificaciones de texto» | Corregido |
| H-02 | MEDIO | RT-14 y RK-04 distinguen las 14 ambigüedades abiertas de A-3 y A-7 (resueltas) | §7.2 RT-14; §16 RK-04 | Corregido |
| H-03 | MEDIO | §13.7 con requisitos afectados, riesgo y efecto sobre F1/F2 por fila; lecturas provisionales no son decisiones | §13.7 | Corregido |
| H-04 | MEDIO | A-15 (interacción con revocaciones de seguridad, desasignaciones y efectos indirectos) y A-16 (reglamento y desempates) registradas **como abiertas, sin respuesta elegida**; denegación por defecto documentada como criterio provisional, no política; nota cruzada en RF-ACC-07 y en la leyenda de la matriz sin cambiar permisos | §13.7; RF-ACC-07; §7.1; RT-14 | Registrado; decisión diferida a F4 |
| H-05 | MEDIO | §14.1 (RF-ACC-01…17, RT-01…14, OFF-01…25, 13 actores de §7.3); §14.4 y §14.5 (OFF-01…25); RK-03 (13 actores); RK-10 (≈ 27–42 semanas, §15) | §14, §16 | Corregido |
| H-06 | BAJO | Encabezado de fuentes y «8 actores» de RF-AUD-03; PRV-09 alineado con §7.3 | Encabezado; RF-AUD-03; PRV-09 | Corregido |
| H-07 | BAJO | Aceptaciones breves (RD-FOR-03 «N-01 y límites», RD-EVT-08 «Fixture», RNF-09, RNF-16, RNF-17, RF-PUB-08, RF-AUD-02 con «catálogo Fase 4»): tienen prueba prevista; precisar el criterio exige definirlo con fixtures, runbook y catálogo de Fase 4; **no se inventaron criterios** | RD-FOR-03; RD-EVT-08; RNF-09, 16, 17; RF-PUB-08; RF-AUD-02 | Diferido; fase límite F4 (RNF-17 y RNF-16: F8) |
| H-08 | BAJO | RT-04 cita RF-AUD-05 (S) como evidencia; si RF-AUD-05 se recortara, RT-04 conserva su control en servidor y BD pero perdería esa evidencia de registro; subir la prioridad de RF-AUD-05 sería un cambio de alcance | RT-04; RF-AUD-05; §15 | Diferido; F4 (decidir si RF-AUD-05 pasa a M o si RT-04 usa otro registro) |
| H-09 | BAJO | La fila de §8.7 «`finalizado` → `en_juego` (reabrir)» está respaldada por Q-09 y la matriz 27; Q-40 (b) sigue abierta sobre reabrir y desarchivar; no se modificó la máquina de estados | §8.7; matriz 27; Q-40 (b) | Diferido; F4 |
| H-10 | BAJO | Concordancia gramatical de §0.4 corregida; la política de 2FA efectiva de RF-ACC-10 se revisó y no requiere cambio | §0.4; RF-ACC-10 | Corregido / sin cambio |
| H-11 | BAJO | La cifra «20 asuntos abiertos» de §18 se marca como histórica de la v1.2 | §18 | Corregido |

### 17.6 Registro de cierre: Fase 1 APROBADA CON CONDICIONES (v1.3.1)

- **Acto:** mensaje «AUTORIZACIÓN FORMAL DE CIERRE DE FASE 1» del 2026-11-15: «AUTORIZO FORMALMENTE LA APROBACIÓN CON CONDICIONES DE LA FASE 1 — REQUISITOS Y MVP, VERSIÓN DOCUMENTAL v1.3.1.» Es una decisión del usuario, distinta del dictamen técnico de §17.4 (recomendación del análisis, conservada como evidencia histórica).
- **Alcance:** cierre documental. No aprueba la implementación técnica, no autoriza producción, no certifica cumplimiento legal ni implica que ningún control esté implementado.
- **Condiciones aceptadas:** 18 preguntas abiertas y 23 `COND` sin aprobar sus alternativas (plazos en §13.8); 14 ambigüedades abiertas (incluidas A-15 y A-16) con lecturas provisionales que no son decisiones ni amplían permisos; hallazgos diferidos H-07, H-08 y H-09 (F4; F8 para RNF-16 y RNF-17); restricciones de seguridad, autorización, conflicto de interés, auditoría y menores, y puertas jurídicas previas a datos reales, vigentes; RK-15 aceptado (sin otras aceptaciones presuntas); RF-EQP-06 diferido; Q-37 confirmada; Q-14 y Q-40 (d) antes de la Fase 3.
- **Estado de fases:** Fase 0 APROBADA; Fase 1 APROBADA CON CONDICIONES; Fases 2–8 y módulos 5A–5G NO INICIADOS. Cualquier paso posterior (Fase 2, commit, push, PR, merge) requiere una autorización expresa e independiente.
- **Contenido sin cambios por este cierre:** requisitos, prioridades, decisiones, reglas deportivas, matriz de permisos, historias, riesgos, pruebas y alcance.

**Verificaciones de texto de la v1.3.1 (continuación de §17.5; no son pruebas del producto):** 186 requisitos (54/72/25/18/17); 169 M, 11 S, 6 P; 23 `COND`; 14 RT; matriz de 41 filas; 72 casos límite; 51 historias; 22 riesgos; 0 identificadores sin definir; A-15 y A-16 abiertas sin respuesta elegida; Fase 1 PENDIENTE DE APROBACIÓN en los tres documentos.

---

## 18. Lo que esta fase no hizo ni garantiza

- No se escribió código, esquema, migraciones ni se instalaron dependencias; no se modificó la skill; no se inició la Fase 2; no hubo commit, push, merge ni despliegue.
- Ninguna prueba se ejecutó: todas las pruebas son **previstas**. Ningún requisito está verificado por ejecución.
- No se verificó ninguna fuente externa: precios, límites y condiciones de proveedores, compatibilidad PWA en iOS, ni normativa de Costa Rica. Las referencias legales son orientativas y [PEND-JUR].
- No se inspeccionaron el mockup de 13 pantallas (no está adjunto) ni imágenes del informe.
- Las cifras propuestas (plazos de invitación, retención, tiempos de actualización, topes, estimación) son propuestas `REC` o `SUP`, no hechos medidos.
- El algoritmo de desempate y el método del círculo se especifican por propiedades y casos; su implementación y prueba corresponden a las fases técnicas.
- La consolidación v1.1 es **solo documental**: no se ejecutó ninguna prueba ni verificación contra código o base de datos; las verificaciones de consistencia (identificadores definidos, conteos, referencias cruzadas) se hicieron sobre el texto del documento.
- La consolidación v1.2 también es **solo documental**: las verificaciones de consistencia (identificadores, conteos, referencias cruzadas) se hicieron sobre el texto; ninguna prueba prevista se ejecutó y no hay código contra el cual verificar.
- La estimación de la v1.2 (histórica) se calculó únicamente con los requisitos consolidados; no incluía el efecto de los 20 asuntos que entonces estaban abiertos.
- La consolidación v1.3 también es **solo documental**: las verificaciones (identificadores, conteos, referencias cruzadas, tablas, estados, requisitos M) se hicieron sobre el texto; ninguna prueba prevista se ejecutó y no hay código contra el cual verificarlas.
- Los totales de la estimación de la v1.3 se calcularon sumando las filas de §15, con asignaciones por módulo que son apreciaciones del análisis, y no incluyen el efecto de los 18 asuntos abiertos ni de las ambigüedades abiertas.
- La corrección v1.3.1 también es **solo documental**: corrige referencias, cifras y marcadores y registra A-15 y A-16; no cambia requisitos, permisos, la máquina de estados, las 21 decisiones ni las 18 preguntas abiertas, y no implica que ningún control esté implementado.
