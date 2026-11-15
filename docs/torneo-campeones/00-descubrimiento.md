# 00 · Descubrimiento y validación del informe — Torneo de Campeones

- **Fase:** 0 (Descubrimiento y validación del informe)
- **Estado:** **APROBADA** por el usuario el 2026-11-15 (v1.1 + aclaraciones de la §18), con **Q-19 confirmada** (cierre definitivo, §19). La Fase 1 **no** está autorizada.
- **Versión:** v1.3 (cierre definitivo, 2026-11-15; v1.2 = cierre de la Fase 0 con Q-19 pendiente, §18; v1.1 = decisiones Q-01…Q-07, §12–§17; §19 confirma Q-19). v1.0 se versionó en el commit `9f54e6d`; v1.1 incorpora las decisiones del usuario sobre Q-01…Q-07 (§12–§17) y marca qué secciones anteriores quedan superadas (§13). Las secciones 1–11 conservan el análisis original como historial.
- **Fecha:** 2026-11-15
- **Fuente principal:** `.claude/skills/torneo-campeones/references/Informe_Torneo_de_Campeones.pdf` (30 págs., v1.0, 14/11/2026)
- **Tipo de trabajo:** aplicación nueva. El repositorio solo contiene la skill y un `README.md`; no hay código, esquema ni configuración del proyecto.

## 0. Cómo leer este documento

Cada afirmación lleva una etiqueta de origen:

| Etiqueta | Significado |
|---|---|
| **[DECL]** | Declarado por el informe como dicho por la organización («según lo conversado»). No hay evidencia directa en esta sesión; se trata como dato a reconfirmar. |
| **[PROP]** | Propuesta o recomendación del informe. No está aprobada. |
| **[SUP]** | Supuesto del informe o del análisis, pendiente de validar con la organización. |
| **[HALL]** | Hallazgo de este análisis (contradicción, vacío o defecto) con su referencia al informe. |
| **[CONF]** | **Decisión confirmada por el usuario** como responsable de las decisiones del proyecto (mensaje de consolidación de Q-01…Q-07, 2026-11-15). Solo el usuario puede cambiarla. |
| **[DER]** | Consecuencia derivada por el análisis de una decisión [CONF]. No es una decisión; requiere confirmación. |
| **[REC]** | Recomendación del análisis. No vinculante y nunca sustituye a una decisión [CONF]. |
| **[PEND]** | Validación o decisión que sigue abierta (incluye la revisión jurídica). |

**Estado de las confirmaciones.** En la versión 1.0 nada estaba confirmado. Desde la v1.1, **solo lo marcado [CONF] en la sección 12 está confirmado por el usuario**; es la única fuente de decisiones vinculantes del proyecto. Todo lo demás sigue siendo [DECL], [PROP], [SUP], [HALL], [DER], [REC] o [PEND]. La Fase 0 fue aprobada el 2026-11-15 (ver §18).

### Alcance y límites de la revisión

- Se leyó el texto completo del PDF (extraído con `pdftotext`; 30 págs.). Las figuras 1 y 2 y los gráficos solo se leyeron como texto extraído. **No se inspeccionaron visualmente las páginas.** La portada es una imagen.
- **El mockup interactivo de 13 pantallas no está adjunto.** Solo se dispone del PDF.
- **No se verificó ninguna fuente externa** (precios, límites de Supabase/Vercel, OWASP 2025, leyes de Guatemala, soporte PWA en iOS). Todas esas cifras son históricas y están sin validar.
- **El SQL no se ejecutó.** Los hallazgos de la sección 7 son análisis estático. Los que dependen del comportamiento real de PostgreSQL/Supabase están marcados «verificar en Fase 4».
- Solo se verificaron por cálculo los ratios de contraste del informe (sección 5.4).

---

## 1. Objetivos, alcance y necesidades

### 1.1 Objetivo del producto
- **[DECL]** Torneo de Campeones organiza ligas, copas y torneos relámpago de fútbol, normalmente varios a la vez, y hoy lleva el control «presumiblemente» por redes sociales y herramientas informales (el propio informe admite que no pudo verificarlo).
- **[DECL]** Necesidades expresadas: control más visual; consulta desde el celular por cualquier persona; que los organizadores puedan crear editores y gestionar roles.
- **[PROP]** PWA instalable con dos caras: vista pública (tablas, calendario, llaves, goleadores, resultados en vivo) y panel con roles (dueño, organizador, editor, delegado).

### 1.2 Alcance propuesto por el informe (§1.2)

| Dentro (v1) [PROP] | Fuera por ahora [PROP] |
|---|---|
| Liga, copa y relámpago, varios en paralelo | Cobro en línea de inscripciones |
| Equipos, plantillas, jugadores | Venta de boletos |
| Generación de calendario y llaves | Transmisión de video |
| Resultados y eventos (goles, tarjetas) | App nativa en tiendas (se evalúa tras el piloto) |
| Tabla, goleadores y llaves públicas | Estadísticas avanzadas (xG, mapas) |
| Roles por torneo, historial de cambios | |
| Avisos push y compartir en WhatsApp/Facebook | |

### 1.3 Dimensionamiento — todo [SUP]
3–6 torneos activos, 6–16 equipos cada uno; 3–10 personas con acceso de edición; 100–500 consultas públicas simultáneas en jornada; conectividad variable en cancha. Orden de magnitud de datos: baja (≈ 240 partidos por liga de 16 equipos a doble vuelta). El riesgo no es el volumen sino los picos y la conectividad.

### 1.4 MVP según el informe
- **[PROP]** (§12.2, fase 1 del informe): «liga completa»: torneos, equipos, calendario, captura de resultados, tabla pública, roles básicos (dueño, organizador, editor), historial, PWA instalable.
- La skill añade como regla: pagos, boletería, video y estadísticas avanzadas quedan fuera del MVP.

---

## 2. Requisitos

### 2.1 Funcionales (derivados de las historias MoSCoW del §4.2)

Los IDs `RF-xx` son provisionales para trazabilidad; la Fase 1 los formaliza.

| ID | Requisito | Prio. informe | Origen | Estado | En MVP del informe |
|---|---|---|---|---|---|
| RF-01 | Ver tabla de posiciones de una liga | Debe | [PROP] | Sin validar reglas de desempate | Sí |
| RF-02 | Ver calendario y resultados por jornada | Debe | [PROP] | — | Sí |
| RF-03 | Crear torneo desde plantilla (liga, copa, relámpago) | Debe | [PROP] | Contradice MVP liga-only | Solo liga |
| RF-04 | Generar calendario automáticamente (con vista previa) | Debe | [PROP] | Algoritmo: método del círculo | Sí |
| RF-05 | Registrar resultado en < 30 s desde el celular | Debe | [PROP] | Meta sin línea base | Sí |
| RF-06 | Dueño invita usuarios y asigna rol por torneo | Debe | [PROP] | Contradice §1.1 (ver C-01) | Sí |
| RF-07 | Dueño ve quién cambió qué y cuándo | Debe | [PROP] | Modelo de auditoría incompleto (ver H-07) | Sí |
| RF-08 | Ver llaves de copa/relámpago | Debe | [PROP] | Contradice MVP liga-only | No |
| RF-09 | Seguir partido en vivo con cronología | Debería | [PROP] | — | No |
| RF-10 | Avisos de equipo y compartir por WhatsApp | Debería | [PROP] | iOS exige instalación | No |
| RF-11 | Delegado administra plantilla de su equipo | Debería | [PROP] | RLS no escrita | No |
| RF-12 | Goleadores, tarjetas, perfil de jugador | Podría | [PROP] | — | No |
| RF-13 | Patrocinadores y galería | Podría | [PROP] | — | No |
| RF-14 | Corregir/reabrir resultado con recálculo y bloqueo si hay rondas posteriores | (§7.3) | [PROP] | Regla sin pantalla ni historia | Parcial |
| RF-15 | Alerta al organizador si un partido no tiene editor a la hora de inicio | (N-12) | [PROP] | Sin canal definido | No definido |
| RF-16 | Captura offline con reintento sin duplicados | (N-13) | [PROP] | Sin diseño de idempotencia | No definido |
| RF-17 | Compartir tabla/llave/partido como imagen con marca | (§8.1) | [PROP] | Meta, no historia | No |
| RF-18 | Instalar la app (guía iPhone/Android) | — | [PROP] | — | PWA sí |

### 2.2 No funcionales (§4.3, todos [PROP] salvo indicación)

| Atributo | Meta del informe | Observación del análisis |
|---|---|---|
| Rendimiento | CWV «bueno» al percentil 75: LCP ≤ 2,5 s, INP ≤ 200 ms, CLS ≤ 0,1 | Medible solo con datos de campo reales; Lighthouse es laboratorio (el informe lo reconoce, §10.5). |
| Accesibilidad | WCAG 2.2 AA; objetivos táctiles 44 px (mínimo AA 24 px) | Herramientas automáticas cubren una fracción; requiere revisión manual. |
| Disponibilidad | 99,5 % mensual (≈ 3,6 h/mes de caída permitida) | **[HALL]** Ningún plan citado ofrece SLA verificado en el informe. Meta sin respaldo contractual. |
| Seguridad | Autorización en BD, registro por invitación, auditoría | Ver sección 7. |
| Integridad | Ningún resultado se pierde por dos editores | Versionado optimista cubre colisiones, no reintentos propios (H-08). |
| Compatibilidad | Android 9+/Chrome; iOS 16.4+/Safari para push; escritorio actual | Sin datos de la base instalada real de los usuarios. |
| Privacidad | Datos mínimos, especialmente menores | Ver sección 7.3. |

---

## 3. Roles y permisos

### 3.1 Actores [PROP] (§4.1)
Aficionado/público · Dueño · Organizador (por torneo) · Editor/anotador (partidos asignados) · Delegado (su equipo).

### 3.2 Matriz del Anexo A [PROP]

| Permiso | Dueño | Organizador | Editor | Delegado | Público |
|---|---|---|---|---|---|
| Ver tablas, calendario, resultados | Sí | Sí | Sí | Sí | Sí |
| Registrar resultados y eventos | Sí | Asignados | Asignados | No | No |
| Crear y editar torneos | Sí | Asignados | No | No | No |
| Gestionar equipos y jugadores | Sí | Asignados | No | Su equipo | No |
| Publicar calendario y jornadas | Sí | Asignados | No | No | No |
| Ver historial de cambios | Sí | Asignados | No | No | No |
| Invitar usuarios y asignar roles | Sí | No | No | No | No |

### 3.3 Huecos y conflictos de la matriz [HALL]
1. **Organizadores y roles:** §1.1 dice que los organizadores deben poder «crear editores y gestionar roles»; el Anexo A, §6.4 y §7.2 dicen que **solo el dueño** invita y asigna. Con el delegado por equipo, el dueño se vuelve cuello de botella operativo. **Decisión pendiente (D-01).**
2. **Crear torneos:** la historia «Como organizador, creo un torneo» choca con «Asignados» (un torneo inexistente no puede estar asignado) y con el SQL (no hay política de `INSERT` en `tournaments`, y `tournament_members` solo la gestiona el dueño). No está definido quién crea torneos ni cómo el creador queda como organizador.
3. **Editor «Asignados»:** el modelo tiene `matches.assigned_editor` (un solo editor por partido, sin historial); la función `report_result` no lo usa (H-02). No se define quién asigna editores a partidos ni si puede haber más de uno o relevos.
4. **Un solo rol por usuario y torneo:** la clave primaria `(tournament_id, user_id)` impide que una persona sea, por ejemplo, organizador y delegado en el mismo torneo, o editor y delegado.
5. **Delegado sin alta definida:** el registro es «solo por invitación» y solo el dueño invita; con 6–16 equipos por torneo la carga recae en una persona.
6. **Faltan en la matriz:** reabrir un partido final (§7.3 dice organizador), cambiar reglas con el torneo en curso (§7.3 dice dueño con confirmación), borrar/anonimizar un jugador a solicitud (§9.4), gestionar patrocinadores, asignar editores, ver/gestionar suscripciones push, recuperar el rol de dueño.
7. **2FA:** §9.5 la exige para dueño y organizadores; los **editores** (que escriben resultados) quedan sin 2FA, y el login previsto es «correo y contraseña».
8. **Dueño único:** `is_owner` solo por migración (§6.4). Sin proceso de sucesión, un solo dueño es punto único de falla (riesgo «pérdida de la persona que mantiene el sistema», §13).

---

## 4. Modalidades de competición y reglas de negocio

### 4.1 Formatos [PROP]
- **Liga:** una etapa `liga`; una o dos vueltas; calendario por método del círculo (con descanso si n es impar).
- **Copa:** una etapa `eliminatoria`; byes hasta potencia de dos; llaves enlazadas por `next_match_id`/`next_slot`.
- **Relámpago:** `grupos` + `eliminatoria`.

**[HALL] Vacíos de formato** (ninguno resuelto en el informe): pase de grupos a eliminatoria (cuántos clasifican, cruces, mejores terceros); copa con fase de grupos; partidos de ida y vuelta y regla de gol de visitante; partido por el tercer lugar; criterio de siembra y de asignación de byes; número de equipos máximo y mínimo; categorías y torneos por edad; reprogramaciones.

### 4.2 Reglas de negocio [PROP] y su estado

| Regla | Fuente | Estado |
|---|---|---|
| Puntos 3/1/0 por defecto, configurable | §6.3, §6.5 | [SUP]; **la vista SQL lo fija en 3/1/0** en lugar de leer `rules` (H-10). |
| Desempate: puntos → mini-tabla → diferencia → goles a favor → sorteo | §6.5 | [SUP] «a confirmar con los organizadores». Falta tarjetas/disciplina («fair play»), que el propio §14 menciona. |
| Empate circular a tres sin bucle (caso «círculo de la muerte») | §6.5, N-03 | [PROP]. Cuidado: el informe lo describe con «uno gana, uno empata, uno pierde» entre cuatro equipos; el criterio de resolución debe definirse con la organización. |
| Un jugador no puede estar en dos equipos del mismo torneo | §6.2, §7.3, N-09 | [PROP]; la restricción `unique (tournament_id, player_id)` es evadible (H-05, H-09). |
| Partido «final» no se edita sin rastro; reabrir exige organizador | §7.3 | [PROP]; no implementado en `report_result`. |
| Corregir resultado jugado: recalcula tabla; en eliminatoria revierte rondas siguientes si no se jugaron; si se jugaron, se bloquea y pide confirmación | §7.3, N-07, N-08 | [PROP]. «Confirmación» y «bloqueo» son contradictorios si no se define quién confirma y qué efecto tiene. |
| Empate en eliminatoria exige penales | §7.2, N-05 | [PROP]; sin validación en SQL y sin check de penales ≥ 0. |
| Criterios de desempate no cambian en curso sin confirmación del dueño | §7.3 | [PROP]; sin mecanismo de cumplimiento en BD. |
| Alerta al organizador si un partido no tiene editor a la hora de inicio | N-12 | [PROP]; sin canal, destinatario ni proceso. |
| Resultado local con reintento sin duplicados | N-13 | [PROP]; sin clave de idempotencia (H-08). |

**No definidos:** walkover/W.O., partido aplazado o cancelado (solo existe `suspendido`), sanciones y suspensiones por acumulación de tarjetas, jugadores elegibles por categoría/edad, cambio de jugadores, tiempo reglamentario y prórroga (el check de minuto llega a 150), posibilidad de eliminar o reasignar un equipo ya inscrito, límites de plantilla, fechas de cierre de inscripción.

---

## 5. Arquitectura tecnológica propuesta

### 5.1 Stack [PROP] (todo por validar en Fase 2 mediante ADR)
Next.js + React + TypeScript (SSR/ISR) en Vercel; Supabase (Auth con invitación y 2FA, Postgres con RLS, Realtime/Broadcast, Storage, Edge Functions + `pg_cron`); Web Push con VAPID; Sentry y monitor de uptime; Supabase CLI para migraciones; Vitest, pgTAP, Playwright, axe, Lighthouse CI, k6; entornos local / staging / producción.

### 5.2 Decisiones A1–A8: evaluación preliminar

| ID | Decisión [PROP] | Evaluación |
|---|---|---|
| A1 | PWA y no APK | Razonable; iOS depende de instalación manual y push solo desde iOS 16.4 en app instalada. **Validar con dispositivos reales.** |
| A2 | Postgres/Supabase | Coherente con datos relacionales. Comparación con Firebase apoyada en una fuente de baja calidad ([17]). Revalidar límites vigentes. |
| A3 | Next.js | Razonable para páginas públicas cacheables. Versiones y modo de despliegue sin fijar. |
| A4 | Autorización en RLS | Correcta como principio; **la implementación presentada es insuficiente** (sección 7). |
| A5 | Roles en tabla, no en JWT | Razonable a esta escala. |
| A6 | Tabla de posiciones como función pura | Correcta, pero **hay dos fuentes de verdad** (vista SQL y función TS) sin definir cuál manda (H-10). |
| A7 | Versionado optimista | Correcta; falta idempotencia para reintentos (H-08). |
| A8 | Realtime solo en partido en vivo | Razonable; depende de caché con invalidación al publicar, aún sin diseño (modelo de «publicación» ausente, H-12). |

### 5.3 Costos propuestos (§11.3) — [SUP], sin verificar
Supabase Pro US$ 25 + Vercel Pro US$ 20 por usuario + dominio ≈ US$ 45/mes. **[HALL]** El total excluye Sentry (0–26/mes según el propio informe) y asientos adicionales de Vercel; el máximo realista del propio informe es ≈ US$ 70+. Los precios se citan de fuentes secundarias, incluida una réplica no oficial de la documentación de Vercel ([48]). Confirmar en páginas oficiales y definir quién paga (pregunta del §14).

### 5.4 Identidad visual [PROP] — verificación realizada
Se recalcularon con la fórmula WCAG los contrastes declarados (§8.3): blanco/azul 17,2 · azul/oro 12,2 · secundario/azul 9,4 · secundario/tarjeta 7,5 · azul/naranja 7,3 · naranja/tarjeta 5,9. **Todos coinciden con el informe.** Adicional: verde `#3DDBB0` sobre tarjeta `#0B2A63` = 7,8:1. Los pares de escudos de ejemplo («Halcones», «Titanes») no se recalcularon por no tener el color exacto. La validación de marca con los propietarios y el logo original sigue pendiente. Esto no prueba cumplimiento WCAG de las pantallas completas.

---

## 6. Modelo de datos y relaciones

### 6.1 Entidades [PROP] (§6.1–6.2)
`profiles` · `teams` · `tournaments` · `tournament_members` · `tournament_teams` · `players` · `rosters` · `stages` · `matches` · `match_events` · `audit_log` · `push_subscriptions` (12 tablas) + vista `standings_base`. Tipos enumerados: `tournament_type`, `tournament_status`, `member_role`, `match_status`.

Relaciones clave: equipo ≠ torneo (`tournament_teams`); plantilla por torneo (`rosters`); etapas flexibles (`stages`); llaves enlazadas (`next_match_id`/`next_slot`); reglas en `tournaments.rules` (jsonb); control de versión (`matches.version`); auditoría (`audit_log`).

### 6.2 Evaluación del modelo [HALL]
- **Torneo ≠ equipo y plantilla por torneo:** el principio es correcto.
- **El modelo no impone la coherencia entre torneos**, ver H-05.
- **Faltan** concepto de publicación por jornada/partido, ida y vuelta, grupo→eliminatoria, estados de partido (W.O., aplazado, cancelado), categorías, historial de asignación de editores, identidad de jugador, consentimiento de menores, `tournament_id` en `match_events` y `audit_log`, borrado lógico y retención.
- **`on delete cascade`** desde `tournaments` borra etapas, partidos, plantillas y equipos inscritos; sin borrado lógico ni respaldo probado es un riesgo operativo.

---

## 7. Seguridad, privacidad e integridad de resultados

### 7.1 Hallazgos sobre el SQL base (§6.3) — análisis estático, no ejecutado

El informe reconoce que el script solo pasó por un analizador sintáctico y que falta ejecutarlo. **No debe adoptarse tal cual.**

| ID | Sev. | Hallazgo | Verificación futura |
|---|---|---|---|
| H-01 | **Crítico** | Solo `matches` tiene RLS (1 de 12 tablas), con 2 políticas. `profiles`, `teams`, `tournaments`, `tournament_members`, `tournament_teams`, `players`, `rosters`, `stages`, `match_events`, `audit_log` y `push_subscriptions` no tienen RLS ni políticas en el script. En Supabase, las tablas del esquema expuesto quedan accesibles por la API según los privilegios por defecto; sin RLS pueden quedar abiertas a `anon`. Contradice el resumen («RLS en todas las tablas») y la tabla de §6.4. | Revisar privilegios por defecto vigentes; pgTAP por tabla, rol y operación. |
| H-02 | **Alto** | `report_result` valida rol de editor **por torneo**, no por `assigned_editor`. Todo editor de un torneo puede modificar cualquier partido del torneo. Contradice Anexo A («Asignados») y §6.4. Es `security definer`, así que RLS no lo limita. | pgTAP: editor asignado / no asignado / de otro torneo (N-10). |
| H-03 | Alto | No hay política `SELECT` para editores, delegados ni organizadores en torneos no públicos salvo el `for all` del organizador. Un editor no podría leer su propio partido en un torneo no publicado. Además, la lectura pública de `matches` expone columnas internas (`assigned_editor`, `updated_by`): RLS filtra filas, no columnas. | Vistas públicas o `GRANT` por columna. |
| H-04 | Alto | La vista `standings_base` no declara `security_invoker`; por defecto una vista se ejecuta con los permisos de su dueño y puede saltarse RLS, exponiendo torneos no públicos. | Verificar en PostgreSQL/Supabase real. |
| H-05 | Alto | **Integridad entre torneos no garantizada.** `rosters.tournament_id` y `rosters.tournament_team_id` son claves foráneas independientes: se puede insertar una fila con `tournament_id` de otro torneo, evadiendo `unique (tournament_id, player_id)`. Lo mismo ocurre en `matches` (`stage_id`, `home_id`, `away_id`, `next_match_id` pueden ser de otro torneo), `match_events` (equipo y jugador sin vínculo con el partido o la plantilla) y `tournament_members.team_id` (apunta a `teams`, no a `tournament_teams`). | Claves compuestas, restricciones o triggers; pruebas con datos cruzados. |
| H-06 | Medio | `check (home_id is distinct from away_id)` evalúa **falso** cuando ambos son `NULL` (`NULL IS DISTINCT FROM NULL` es falso). Eso impediría crear partidos de eliminatoria con ambos lados por definir (semifinales, final). | Probar el `INSERT` en Postgres real. |
| H-07 | **Alto** | Auditoría incompleta: el trigger solo existe `AFTER UPDATE` en `matches`; no hay auditoría de `INSERT`/`DELETE` ni de otras tablas (equipos, plantillas, roles, reglas, eventos). `row_id` queda `NULL` en borrados; no hay `tournament_id` para que el organizador vea «sus» torneos (§6.4); `audit_log` no tiene RLS, privilegios revocados ni protección de inmutabilidad; guarda filas completas (puede copiar datos personales). «Historial inmutable» no está demostrado. | pgTAP: intentos de `UPDATE`/`DELETE` sobre `audit_log` por cada rol. |
| H-08 | Alto | `report_result` no tiene clave de idempotencia. En un reintento tras pérdida de conexión (N-13), si el primer envío sí llegó, el segundo falla con conflicto de versión aunque sea el mismo cambio. La sincronización en segundo plano **no existe en iOS** (§2.1), así que el reintento solo ocurre con la app abierta. Además: no valida transiciones de estado, permite `final` con marcador nulo, no gestiona penales ni avance de llaves, no distingue «reabrir» (solo organizador), y reutiliza `40001` (serialización) para un conflicto de aplicación. Los `EXECUTE` por defecto a `PUBLIC` deben revisarse. | Diseñar contrato de captura con `client_mutation_id`. |
| H-09 | Medio | `players` es global y no tiene clave de identidad: la misma persona dada de alta dos veces evade «un jugador, un equipo». Con datos mínimos (sin documento) no hay forma natural de deduplicar. | Decisión de negocio (Q-12). |
| H-10 | Medio | Reglas en `jsonb` validadas solo en la aplicación; la vista fija 3/1/0; el desempate existe en TypeScript pero la vista SQL ya calcula la tabla. Dos fuentes de verdad. No hay cumplimiento en BD de «no cambiar criterios con el torneo en curso». | ADR de ubicación de la lógica. |
| H-11 | Medio | La función `has_role` es `security definer` en `public` y probablemente expuesta como RPC; revisar `EXECUTE`, esquema privado y comportamiento del propietario (un rol con `BYPASSRLS` ignora incluso `FORCE ROW LEVEL SECURITY`). | Verificar roles y privilegios reales de Supabase. |
| H-12 | Medio | «Publicar jornada/torneo» (Anexo A, §7.2) no tiene respaldo en el modelo: solo existe `tournaments.is_public`. No hay estado de publicación por jornada ni por partido, ni diseño de invalidación de caché. | Definir ciclo de vida y caché (Fase 2). |
| H-13 | Medio | `push_subscriptions`: sin RLS en el script; suscripción pública «sin cuenta» y con `topics` libres; el `endpoint` es un identificador del dispositivo (dato personal potencial); riesgo de abuso por inundación. | Límites de tasa; política de retención. |

### 7.2 Riesgos de seguridad y cobertura (§9) [PROP]
Los controles del informe (RLS forzado, claves de servicio solo en servidor, registro cerrado, 2FA, CSP/HSTS, escaneo de dependencias, límites de tasa, respaldos) son adecuados en principio. Falta: modelo de amenazas por rol con casos de abuso, política de sesiones y recuperación de cuenta, rotación de claves, gestión de la invitación (vencimiento, reenvío), protección de `service_role` en Edge Functions, CORS/CSRF según cómo se escriba (RPC directa desde navegador vs rutas de API), y alcance de «cabeceras de seguridad» con Next.js. **Las categorías A07 y A08 de OWASP 2025 el propio informe las marca como no verificadas.**

### 7.3 Privacidad
- **[HALL]** El informe propone publicar nombre completo y año de nacimiento de jugadores (incluidos menores) y habla de «perfil de jugador» público. Esto contradice la regla de minimización: **ningún dato de menores se publica por defecto** hasta que la organización y un abogado resuelvan jurisdicción, consentimiento del tutor y base legal. Alternativa propuesta a evaluar: mostrar alias/iniciales o número de camiseta, y categoría en lugar de año de nacimiento.
- **[HALL]** El país de operación **no está confirmado**: el informe asume Guatemala (§2.1, §9.4) y cita iniciativas y leyes (6572/6464, 6347) con fuentes que discrepan entre sí o con una sola fuente. No es asesoría legal. Hasta confirmar país, ninguna cita legal se usa en requisitos.
- Faltan: titular del tratamiento, aviso de privacidad, flujos de acceso/rectificación/cancelación/oposición, retención de `audit_log` y de suscripciones push, consentimiento para imágenes/escudos/patrocinadores.

### 7.4 Integridad de resultados
Controles propuestos: versión optimista, historial, alertas por cambios en partidos finalizados. **Brechas:** H-02, H-07, H-08; no hay segundo factor para editores; no hay proceso de validación o cierre del partido (¿quién da por final un resultado?); no hay política de disputa de resultados ni de reversión documentada (el informe lo pide en §9.5 como plan de incidentes).

---

## 8. Contradicciones, supuestos y requisitos pendientes de validación

### 8.1 Contradicciones y desalineaciones

> **Actualización v1.1:** C-01 y C-02 quedan resueltas en lo esencial por Q-02; C-12 por Q-05 (con pendientes); las referencias a Guatemala no aplican (Q-01). Ver §13.

| ID | Descripción | Referencia |
|---|---|---|
| C-01 | Organizadores «crean editores y gestionan roles» vs solo el dueño invita/asigna. | §1.1 vs §6.4, §7.2, Anexo A |
| C-02 | «Organizador crea torneo» vs Anexo A «Asignados» vs SQL sin política de creación. | §4.2, Anexo A |
| C-03 | MoSCoW «Debe»: llaves y plantillas copa/relámpago; MVP del informe: solo liga. | §4.2 vs §12.2 |
| C-04 | Alcance v1 (§1.2) incluye push, goleadores, llaves; fases los sitúan en la fase 2 del informe. | §1.2 vs §12.2 |
| C-05 | Editor «Asignados» (matriz) vs `report_result` por torneo. | Anexo A vs §6.3 |
| C-06 | «RLS en todas las tablas» vs el SQL que solo protege una. | Resumen, §9.2 vs §6.3 |
| C-07 | Plazo «plataforma completa 4–5 meses» vs suma de fases 12–15 semanas (≈ 3–3,5 meses) con 2–3 desarrolladores; con una persona ×1,5–2 daría ≈ 4,5–7 meses. | Resumen vs §12.2 |
| C-08 | Costo ≈ US$ 45 vs monitoreo hasta US$ 26 y asientos Vercel adicionales. | §11.3 |
| C-09 | Numeración: el informe usa fases 0–4; esta skill usa Fases 0–8 y módulos 5A–5G. «Ejecutar el SQL contra una instancia real» (informe: fase 1) corresponde aquí a validación en Fase 4 (no destructiva) y a 5A+ (implementación). | §6.3 vs skill |
| C-10 | «13 pantallas»: el inventario lista 7 públicas + 2 móviles + 4 de panel, pero la navegación menciona Torneos, Equipos, Calendario global, Goleadores, Partidos e Historial de cambios como pantallas sin entrada propia en el inventario. | §8.2 vs §8.5 |
| C-11 | Login «correo y contraseña» vs 2FA obligatoria para dueño y organizadores. | §8.2 vs §9.5 |
| C-12 | Datos públicos de jugadores (nombre completo, año de nacimiento) vs minimización de menores. | §6.2, §6.4 vs §9.1, §9.4 |
| C-13 | La tabla de §6.4 da lectura pública de `players`, pero `players` no tiene `tournament_id`; la política no es expresable sin pasar por `rosters`. | §6.4 vs §6.3 |
| C-14 | «Nuevo torneo (paso 4)» genera calendario; «se puede rehacer mientras no haya partidos jugados» sin definir qué ocurre con asignaciones de editor y publicaciones ya hechas. | §4.2, §7.2 |
| C-15 | Disponibilidad 99,5 % mensual sin SLA contractual del hosting verificado. | §4.3 |

### 8.2 Supuestos pendientes de validar
Dimensionamiento (§1.3) · reglas de puntos y desempate (§6.5) · base instalada de dispositivos y calidad de red en canchas · «registro de resultados en menos de 30 s» (sin línea base ni prueba de usabilidad) · adopción por editores · uso de push en iPhone · precios y límites de plan (todo de fuentes secundarias) · soporte actual de PWA en iOS · país y normativa · quién posee dominio/cuentas · identidad visual y logo en formato original · historial de torneos a migrar · costos de operación a cargo de la organización.

### 8.3 Calidad de las fuentes del informe
El informe se apoya en blogs, agregadores y réplicas (por ejemplo, [3], [17], [19], [20], [28]–[32], [44]–[48]) para precios, límites y buenas prácticas. Útiles como orientación, **insuficientes para decisiones vinculantes**: antes de la Fase 2 deben contrastarse con documentación oficial de Supabase, Vercel, W3C/WCAG, OWASP y normativa oficial.

---

## 9. Recomendaciones y prioridades para el MVP (propuesta del análisis, no decisión)

### 9.1 Orden de prioridad sugerido (a discutir en Fase 1)
1. **Reglas reales de la organización** (puntos, desempates, formatos, quién edita) antes de modelar.
2. **Modelo de datos + autorización** reescritos con integridad entre torneos y pruebas por rol (reemplaza al SQL base; no se adopta).
3. **Captura de resultados** con versionado, idempotencia, asignación de editor, historial y reintento local.
4. **Tabla y calendario públicos** con caché e invalidación al publicar.
5. **PWA instalable mínima** y guía de instalación.
6. Después: delegados, goleadores/eventos, llaves, push, imágenes para compartir.

### 9.2 Candidato de MVP (reducción propuesta) — **SUPERADO por la decisión Q-04 (§12); se conserva como historial**
- **Incluir:** una liga (una etapa), equipos/inscripciones, plantilla básica (dependiendo de privacidad), calendario por método del círculo con vista previa, captura de marcador y estado, historial de cambios, tabla pública, calendario público, roles dueño/organizador/editor asignado, invitación cerrada con 2FA para dueño/organizador, PWA instalable y captura con reintento.
- **Diferir:** goleadores y tarjetas, llaves, copa y relámpago (con los vacíos de §4.1), delegados, push, tiempo real, imágenes para compartir, patrocinadores, histórico.
- **Condición:** Si la organización necesita eventos (goles/tarjetas) o llaves desde el piloto, se justificará con la historia de usuario correspondiente y se reevaluará el plazo.

### 9.3 Mejoras al diseño propuesto (a decidir en Fases 2 y 4)
- Escribir la RLS tabla por tabla con matriz CRUD × rol × torneo, y exponer lecturas públicas por vistas con columnas permitidas.
- Claves compuestas o restricciones para vincular torneo, etapa, equipo inscrito, plantilla, partido y evento.
- Mover `has_role` y funciones de apoyo a un esquema no expuesto; revisar `EXECUTE`.
- Auditoría genérica con `tournament_id`, `INSERT/UPDATE/DELETE`, tabla de solo anexado, y bloqueo explícito de escritura.
- Contrato de captura con `client_mutation_id`, estados válidos y transición de reapertura solo para organizador.
- Una sola fuente de verdad para la clasificación y para la validación de `rules`.
- Estado de publicación por jornada y estrategia de invalidación de caché.
- Borrado lógico para torneos, partidos y jugadores; retención definida.
- Prueba de restauración de respaldo antes del piloto.

---

## 10. Preguntas a resolver antes de la Fase 1

> **Actualización v1.1:** Q-01…Q-07 fueron respondidas por el usuario (§12). Q-08…Q-16 siguen abiertas y se suman Q-17…Q-25 (§14.2).

**Bloqueantes (afectan alcance y modelo) — prioridad alta**

| ID | Pregunta | Motiva |
|---|---|---|
| Q-01 | ¿En qué país opera la organización y quién es el responsable legal del tratamiento de datos? | C-12, 7.3 |
| Q-02 | ¿Quién crea torneos, quién invita usuarios y quién asigna editores a partidos: solo el dueño o también los organizadores? | C-01, C-02 |
| Q-03 | ¿Cuáles son las reglas exactas de puntos y desempate en liga, y los formatos de copa y relámpago (clasificación de grupos, cruces, ida y vuelta, tercer lugar, W.O.)? | §4 |
| Q-04 | ¿Qué debe incluir el primer piloto: una liga completa, solo calendario y tabla, o también llaves/eventos? ¿Quién la ejecutará y cuándo? | 9.2 |
| Q-05 | ¿Hay jugadores menores de edad? ¿Qué se publica de ellos (nombre completo, año, alias) y con qué consentimiento? | 7.3 |
| Q-06 | ¿Cuántos torneos, equipos, canchas, editores y público simultáneo esperan realmente? ¿Hay señal de datos en las canchas? | §1.3 |
| Q-07 | ¿Quién será dueño técnico, quién posee y paga dominio, hosting, correo y cuentas, y cuál es el presupuesto máximo mensual? | §11.3 |

**Importantes — resolver en Fase 1**

| ID | Pregunta |
|---|---|
| Q-08 | ¿Cómo registran hoy los resultados y quién lo hace? ¿Aceptan 2FA para editores o solo para dueño/organizadores? |
| Q-09 | ¿Quién puede reabrir o corregir un partido final y en qué plazo? ¿Cómo se manejan disputas? |
| Q-10 | ¿Qué canal usa la alerta N-12 (correo, WhatsApp, push) y quién la recibe? |
| Q-11 | ¿Qué historial de temporadas pasadas se desea cargar y con qué datos? |
| Q-12 | ¿Cómo se identificará a un jugador de forma única sin recolectar documentos (para evitar duplicados y garantizar «un jugador, un equipo»)? |
| Q-13 | ¿Qué prefieren compartir (tablas, resultados, llaves) y por qué canal (Facebook, WhatsApp)? ¿Hay patrocinadores o cobros de inscripción? |
| Q-14 | ¿Disponen del logo en formato original y del mockup de 13 pantallas? ¿Los colores propuestos son oficiales? |
| Q-15 | ¿Se permite que una persona tenga más de un rol en el mismo torneo (organizador y delegado)? |
| Q-16 | ¿Qué disponibilidad es aceptable realmente en jornada y qué pasa si el sistema cae durante un partido? |

---

## 11. Criterios para aprobar o rechazar la Fase 0

**Se recomienda aprobar si:**
1. Está de acuerdo en que el informe se considera una propuesta y que el SQL base **no se adopta tal cual**.
2. Acepta la clasificación de requisitos en [DECL]/[PROP]/[SUP]/[HALL] y la lista de contradicciones C-01…C-15.
3. Acepta la lista de hallazgos H-01…H-13 como insumo de la Fase 4 (diseño técnico), sin haberlos verificado en base real.
4. Decide, o delega con plazo, las preguntas bloqueantes Q-01…Q-07, o autoriza continuar con supuestos explícitos.
5. Valida, ajusta o rechaza el candidato de MVP de la sección 9.2.

**Se recomienda rechazar o pedir cambios si:**
1. Alguna afirmación de este documento no coincide con su conocimiento del proyecto.
2. Falta un hallazgo, rol, formato o restricción que usted considera crítico.
3. No está de acuerdo con tratar como no confirmado lo que el informe dice que «se conversó».
4. Desea ampliar la verificación (por ejemplo, validar fuentes oficiales o inspeccionar visualmente las páginas) antes de cerrar la fase.

**Esta fase no ha hecho:** código, esquema, migraciones, instalación de dependencias, servicios externos, commits (más allá del de v1.0 autorizado), push ni despliegues. **No verificó:** fuentes externas, comportamiento real del SQL, mockup, legislación.

> Los criterios de esta sección se evalúan con las decisiones recibidas en la **sección 17**.

---

## 12. Decisiones confirmadas por el usuario (Q-01…Q-07)

Fuente: mensaje «CONSOLIDACIÓN DE DECISIONES DE FASE 0» del 2026-11-15. Cada decisión se transcribe sin alterar su sentido. Las consecuencias derivadas, las recomendaciones y los pendientes se separan explícitamente.

### Q-01 — País y responsabilidad de datos
**[CONF]**
- País de operación: **Costa Rica**.
- Responsable previsto del tratamiento: **la organización Torneo de Campeones**.
- Función del usuario: desarrollador tecnológico, con acceso técnico autorizado.
- Se aplica el marco jurídico costarricense correspondiente.
- La formalización de responsabilidades y la revisión jurídica **siguen pendientes**.

**[DER]** Quedan sin efecto para el proyecto las referencias a Guatemala (§2.1 del informe, §9.4, fuentes [35]–[39]). Zona horaria y formatos regionales de Costa Rica (por confirmar en Fase 1). Si el desarrollador accede a datos personales reales, actúa en la práctica como encargado o proveedor técnico del responsable.

**[PEND-JUR]** Referencias de conocimiento general del analista, **no verificadas en esta sesión y no constituyen asesoría legal**: Ley N.º 8968 de Protección de la Persona frente al Tratamiento de sus Datos Personales y su Reglamento; la Agencia de Protección de Datos de los Habitantes (PRODHAB); el Código de la Niñez y la Adolescencia (Ley N.º 7739) en lo relativo a imagen y privacidad de menores. Debe confirmarse su vigencia, reformas y proyectos de ley en curso, y qué obligaciones concretas aplican (consentimiento informado, aviso, inscripción de bases de datos, transferencias fuera del país, seguridad).

**[PEND]** Formalizar por escrito: responsable del tratamiento, relación con el desarrollador (confidencialidad y encargo de tratamiento), proveedores en la nube y ubicación de los datos.

### Q-02 — Roles y permisos
**[CONF]** Modelo de control delegado:

| Rol | Alcance confirmado |
|---|---|
| Dueño | Administración global. |
| Organizador | Crea torneos y gestiona los que tiene autorizados; puede invitar editores y delegados. |
| Editor | Registra resultados y eventos **exclusivamente de sus partidos asignados**. |
| Delegado | Administra **únicamente** la plantilla de su equipo. |
| Público | Consulta lo publicado sin autenticación. |

- Los permisos se validan en el **servidor y en la base de datos**.
- Se **prohíbe la escalada de privilegios** y el acceso entre torneos no autorizados.

**[DER]** Resuelve C-01 (los organizadores sí invitan editores y delegados) y parcialmente C-02 (los organizadores crean torneos). El Anexo A del informe queda superado en estos puntos. El requisito «editor solo partidos asignados» confirma que H-02 es un defecto a corregir, no un matiz.

**[PEND]** (no decididos por el usuario; la invitación de organizadores quedó resuelta por Q-19, §19) cómo queda «autorizado» el organizador que crea un torneo; quién asigna editores a partidos; si puede revocar accesos; si puede haber varios roles por persona; 2FA para editores; quién corrige o reabre partidos finales.

### Q-03 — Reglamento deportivo
**[CONF]**
- Victoria 3 puntos, empate 1, derrota 0.
- Criterios de desempate **configurables por torneo antes de comenzar** la competición.
- Orden predeterminado propuesto: **puntos, diferencia de goles, goles a favor, enfrentamiento directo y resolución manual**.
- Liga configurable a una vuelta o ida y vuelta.
- W.O. configurable por torneo, con marcador predeterminado **3-0**; las adjudicaciones por W.O. **quedan auditadas y no generan goles individuales ficticios**.
- Se permite liga tradicional, liga con fases finales y grupos más eliminatorias.
- Cantidad de clasificados configurable por torneo.
- Cruces configurables: clasificación, sorteo o selección manual.
- Resolución de empates en eliminatorias configurable, con soporte para eliminatorias de ida y vuelta.
- Los cambios al reglamento de un torneo iniciado se **controlan, validan y auditan**.

**[DER]**
- El orden por defecto confirmado **difiere del propuesto en el informe** (mini-tabla en 2.º lugar). Prevalece la decisión del usuario; N-02 y N-03 se reformulan en Fase 1/7 con este orden.
- Con los puntos 3/1/0 como valores de la decisión, la vista `standings_base` del informe los fija en el mismo valor, pero el diseño debe leerlos del reglamento del torneo (H-10).
- Las adjudicaciones por W.O. alteran GF/GC del equipo sin goles de jugadores; por tanto, la suma de goles de jugadores **no tiene por qué igualar** el marcador. Cualquier validación «marcador = suma de eventos» debe excluir W.O.
- El «enfrentamiento directo» en 4.º lugar exige definir su aplicación con más de dos equipos empatados y en torneos de una sola vuelta.

**[PEND]** puntos configurables o fijos; W.O. de ambos equipos; efecto del W.O. sobre GF/GC, tarjetas y sanciones; quién y cómo aplica la «resolución manual» y cómo se publica; qué cambios al reglamento se permiten tras el inicio, quién los aprueba y si recalculan el histórico.

### Q-04 — Alcance del MVP
**[CONF]** MVP ampliado: gestión de torneos de liga; equipos y jugadores; calendarios y resultados; tablas de posiciones; **goleadores y tarjetas**; **W.O.**; una o dos vueltas; criterios de desempate configurables; administración de usuarios y permisos; vista pública y panel administrativo. Copa, relámpago, eliminatorias avanzadas y otras capacidades quedan para fases posteriores.

**[DER]** Este alcance **sustituye** el candidato de MVP de §9.2 (que difería goleadores y tarjetas). Entran los eventos de partido (`match_events`) y su auditoría al MVP. La estimación de 5–7 semanas del informe (§12.2) ya no es aplicable; con además el modo sin conexión (Q-06) debe re-estimarse.

**[PEND]** Frontera exacta entre Q-03 y Q-04: Q-03 permite liga con fases finales y grupos más eliminatorias, mientras Q-04 limita el MVP a «torneos de liga» y difiere las eliminatorias avanzadas. **Se asume [DER]** que Q-03 define el reglamento que el modelo debe poder soportar y Q-04 define lo que se construye primero; debe confirmarse qué piezas de Q-03 (fases finales simples, grupos) están dentro del MVP.

### Q-05 — Menores de edad
**[CONF]**
- Perfiles públicos limitados a la información deportiva necesaria.
- Protección de la información privada.
- Autorización válida del representante legal cuando corresponda.
- Controles de acceso y minimización de datos.
- Documentar requisitos de consentimiento, revocación y revisión jurídica antes del lanzamiento.

**[DER]** Reemplaza lo propuesto en el informe de publicar año de nacimiento (C-12) y obliga a definir qué se considera «información deportiva necesaria». La revocación afecta tablas, resultados históricos y auditoría; debe diseñarse (anonimización sin romper la integridad de resultados).

**[PEND]** campos públicos permitidos (nombre completo, alias, dorsal, posición, foto); edad de corte; quién recolecta el consentimiento, dónde se guarda su evidencia y cómo se revoca; retención; revisión jurídica previa al lanzamiento (decisión confirmada, ejecución pendiente).

### Q-06 — Dimensionamiento y conectividad
**[CONF]**
- Entre 3 y 6 torneos simultáneos; entre 6 y 16 equipos por torneo.
- Captura de resultados **sin conexión a Internet obligatoria**.
- Sincronización posterior al recuperar conexión.
- Prevención de operaciones duplicadas y de conflictos entre editores.
- Diferenciar visualmente las operaciones pendientes de los resultados confirmados por el servidor.
- La arquitectura offline-first se diseña formalmente en la **Fase 2** y se verifica con pruebas de integración y de conflictos.

**[DER]** Interpretación a confirmar (Q-17): la captura **no debe requerir conexión** para funcionar. Esto eleva N-13 de caso de prueba a requisito central del MVP. La limitación de iOS sin sincronización en segundo plano (§2.1) implica que la sincronización ocurre con la app abierta; debe validarse en dispositivos reales.

### Q-07 — Propiedad e infraestructura
**[CONF]**
- La organización Torneo de Campeones es propietaria de las cuentas de infraestructura y del dominio.
- El usuario tiene acceso técnico como desarrollador.
- La organización asume los costos operativos.
- En el piloto se priorizan planes gratuitos que sean legal, técnica y operativamente adecuados.
- Costos, límites, restricciones comerciales y respaldos se verifican antes de contratar o publicar.

**[DER]** Responde parcialmente las preguntas 5 y 6 del §14 del informe. Queda por contestar el presupuesto máximo y la persona de la organización que administrará las cuentas.

**[PEND]** El informe afirma, sin verificar, que Vercel Hobby prohíbe uso comercial y que Supabase Free pausa proyectos inactivos y exige comprobar respaldos. Si esas condiciones se confirman, el plan gratuito podría no ser «legal, técnica y operativamente adecuado» para el piloto de una organización; se evaluará con fuentes oficiales en Fase 2. Esto no cambia la decisión del usuario.

---

## 13. Efecto de las decisiones sobre las secciones anteriores

| Sección | Efecto |
|---|---|
| §3.3 puntos 1 y 2; C-01; C-02 | Resueltos en lo esencial por Q-02. Persisten los pendientes de §12 Q-02. |
| §4.2 (desempate), N-02, N-03 | Orden por defecto reemplazado por el de Q-03. |
| §7.3 y C-12 | Datos de menores: prevalece Q-05. País: Costa Rica; las referencias a Guatemala no aplican. |
| §9.2 «candidato de MVP» | **Superado** por Q-04. Se conserva solo como registro histórico. |
| §10 | Q-01…Q-07 respondidas. Q-08…Q-16 siguen abiertas. Nuevas: Q-17…Q-25 (§14). |
| §11 criterios | Se reevalúan en §17. |
| §5.3 costos | Los costos los asume la organización (Q-07); los importes siguen sin verificar. |

---

## 14. Asuntos abiertos y tensiones entre decisiones

### 14.1 Tensiones detectadas (no se modifican las decisiones; se piden aclaraciones)

| ID | Tensión | Aclaración solicitada |
|---|---|---|
| T-01 | Q-03 admite fases finales, grupos y eliminatorias configurables; Q-04 limita el MVP a liga y difiere eliminatorias avanzadas. | **Resuelta por Q-18 (§18):** el MVP es solo liga; eliminatorias, copas, relámpagos y fases de grupos quedan fuera. |
| T-02 | Q-04 (MVP ampliado con goleadores, tarjetas y W.O.) + Q-06 (offline-first) amplían notablemente el esfuerzo; el informe estimó 5–7 semanas para un MVP menor. | Re-estimar en Fase 1/2 (no se altera el alcance). |
| T-03 | Q-02 exige que cada editor solo toque sus partidos; Q-06 exige sincronización posterior. Una desasignación mientras el editor está sin conexión produce operaciones rechazadas al sincronizar. | Definir en Fase 2 la política (rechazo visible, conservar borrador). |
| T-04 | Q-05 exige minimización y revocación; el modelo del informe guarda filas completas en auditoría. | Anonimización compatible con el historial (Fase 2/4). |
| T-05 | Q-03 «W.O. sin goles individuales» frente a «goleadores en el MVP»: los totales de equipo y de jugadores no coinciden en W.O. | Regla explícita en Fase 1. |
| T-06 | Q-07 «priorizar planes gratuitos» frente a requisitos de respaldo, restricciones comerciales y disponibilidad. | Verificar antes de contratar (Fase 2). |

### 14.2 Preguntas nuevas

| ID | Pregunta |
|---|---|
| Q-17 | **[RESUELTA — §18]** ¿Confirma que «sin conexión a Internet obligatoria» significa que la captura debe funcionar totalmente sin Internet? ¿Hasta qué punto (consultar, capturar, finalizar un partido)? |
| Q-18 | **[RESUELTA — §18]** ¿Qué parte de Q-03 entra en el MVP: fases finales simples, grupos, ninguna? |
| Q-19 | **[CONFIRMADA — §19]** ¿Quién invita organizadores y cómo queda autorizado quien crea un torneo? ¿Puede un organizador invitar a otro organizador? |
| Q-20 | [ESPECIFICACIÓN POSTERIOR] ¿Qué campos públicos de jugador se permiten (nombre completo, alias, dorsal, posición, foto) y desde/hasta qué edad aplican las reglas de menores? |
| Q-21 | [ESPECIFICACIÓN POSTERIOR] W.O.: ¿qué ocurre si no se presentan ambos equipos? ¿Hay sanción adicional? ¿Cuenta para GF/GC? |
| Q-22 | [ESPECIFICACIÓN POSTERIOR] «Resolución manual» de un desempate: ¿quién la ejecuta, con qué justificación y cómo se publica? |
| Q-23 | [ESPECIFICACIÓN POSTERIOR] Cambios de reglamento en un torneo iniciado: ¿cuáles se permiten, quién los aprueba y se recalcula el histórico? |
| Q-24 | [ESPECIFICACIÓN POSTERIOR] ¿Quién recolecta el consentimiento del representante legal, dónde se conserva la evidencia y quién atiende solicitudes de acceso, rectificación o cancelación? |
| Q-25 | [ESPECIFICACIÓN POSTERIOR] ¿Hay restricciones sobre la ubicación de los datos (región de nube, transferencias fuera de Costa Rica)? |

### 14.3 Preguntas previas que siguen abiertas
[ESPECIFICACIÓN POSTERIOR, según corresponda] Q-08 (2FA para editores), Q-09 (reabrir o corregir resultados finales), Q-10 (canal de alerta N-12), Q-11 (históricos), Q-12 (identidad única de jugador sin recolectar documentos), Q-13 (compartir, patrocinadores, cobros), Q-14 (logo y mockup), Q-15 (múltiples roles por persona), Q-16 (disponibilidad aceptable).

---

## 15. Requisitos que necesitan especificación adicional en la Fase 1

### 15.1 Funcionamiento sin conexión (Q-06)
Especificar, como mínimo:
- Alcance offline: qué se consulta y qué se captura; qué datos deben estar en el dispositivo y cómo llegan antes del partido.
- Cola de operaciones (outbox) con identificador de cliente por operación, orden, reintentos y estados: *pendiente / enviando / confirmada / en conflicto / rechazada*.
- Reglas de conflicto entre editor (offline) y organizador (correcciones) y entre dos dispositivos del mismo editor; qué gana y qué se muestra.
- Qué ocurre si se desasigna al editor o se revoca su acceso con operaciones pendientes; caducidad de sesión sin conexión.
- Marcar «final» sin conexión: permitido o solo propuesto; qué ve el público (solo lo confirmado).
- Seguridad de los datos almacenados en el dispositivo (alcance mínimo, borrado al cerrar sesión, pérdida del dispositivo).
- Relojes del dispositivo y minuto de eventos; duplicados por doble envío.
- Pruebas de aceptación: pérdida de red en el guardado, reintentos, doble envío, dos editores, revocación a mitad, iOS real.

### 15.2 Resolución de empates eliminatorios e ida y vuelta (Q-03)
Aunque queden fuera del MVP (Q-04), la Fase 1 debe especificar el requisito para que el modelo lo admita:
- Opciones configurables: prórroga, penales, marcador global, regla de goles de visitante (si se adopta) y decisión manual con auditoría.
- Orden de aplicación por torneo, por ronda y para la final.
- Estructura de partido de ida y vuelta, marcador global, qué hace el avance de llave y qué ocurre al corregir un partido jugado (§7.3 del informe, N-05 y N-08).
- Qué datos exige cada tipo de empate (penales ≥ 0, ganador final).

### 15.3 Protección de menores y datos personales (Q-01, Q-05)
- Definición de «información deportiva necesaria» y de los campos públicos.
- Edad de corte, definición de representante legal y casos en que aplica la autorización «cuando corresponda».
- Modelo de consentimiento: evidencia, fecha, alcance, vigencia, revocación y su efecto sobre lo ya publicado y sobre el historial de auditoría.
- Acceso por rol a datos de menores; retención y eliminación; solicitudes de acceso, rectificación y cancelación.
- Aviso de privacidad, responsable del tratamiento y revisión jurídica previa al lanzamiento.
- Tratamiento de las suscripciones push (identificadores de dispositivo) si se activan más adelante.

### 15.4 Otros requisitos que necesitan precisión
Desempate configurable y su congelación al iniciar el torneo; control de cambios del reglamento (Q-03); W.O. y su auditoría; matriz de permisos v2 con invitaciones, asignación de editores y revocación (Q-02); eventos de partido y consistencia marcador/eventos (Q-04); inventario de pantallas del MVP ampliado (Q-04); presupuesto y adecuación de planes gratuitos (Q-07).

---

## 16. Matriz de trazabilidad Q-01…Q-07 → requisitos previstos

Los identificadores RP-xx son **provisionales**. La Fase 1 los convertirá en historias con criterios de aceptación; ninguno está aprobado como requisito definitivo.

| Decisión | Requisito previsto | Se especifica / diseña en | Verificación prevista | Hallazgos relacionados |
|---|---|---|---|---|
| Q-01 | RP-01 Marco jurídico costarricense recogido en los requisitos de privacidad | F1; revisión jurídica previa al lanzamiento (F8) | Revisión jurídica documentada | C-12, §7.3 |
| Q-01 | RP-02 Responsable del tratamiento, aviso de privacidad y encargo con el desarrollador | F1, F8 | Documentos firmados | §7.3 |
| Q-01 | RP-03 Ejercicio de derechos de acceso, rectificación y cancelación | F1, F4, F5 | UAT y pruebas de flujo | §7.3 |
| Q-01 | RP-04 Ubicación de datos y transferencias | F2 | ADR y verificación oficial | Q-25 |
| Q-02 | RP-05 Matriz de permisos v2 (cinco roles) | F1; F4 (RLS) | pgTAP por rol, tabla y operación | C-01, C-02, H-01 |
| Q-02 | RP-06 Invitaciones con alcance y sin escalada | F2, F4; módulo 5C | pgTAP y pruebas de abuso | H-11 |
| Q-02 | RP-07 Organizador crea torneo y queda autorizado | F4; módulos 5B/5C | pgTAP, E2E | C-02 |
| Q-02 | RP-08 Asignación de editores a partidos y verificación en `report_result` | F4; módulos 5D/5E | pgTAP (asignado / no asignado / otro torneo), N-10 | H-02 |
| Q-02 | RP-09 Delegado solo plantilla de su equipo | F4; módulo 5C | pgTAP | H-05 |
| Q-02 | RP-10 Aislamiento entre torneos y validación servidor + BD | F2 (amenazas), F4, F7 | pgTAP de acceso cruzado | H-05 |
| Q-03 | RP-11 Puntos 3/1/0 y vuelta simple o doble | F1, F4; módulos 5D/5F | Pruebas unitarias | H-10, N-01 |
| Q-03 | RP-12 Desempate configurable antes del inicio, con orden por defecto | F1, F4; módulo 5F | Pruebas unitarias (N-02, N-03 reformuladas) | H-10 |
| Q-03 | RP-13 W.O. configurable, 3-0 por defecto, auditado, sin goles de jugadores | F1, F4; módulo 5E | pgTAP, pruebas unitarias | T-05, H-07 |
| Q-03 | RP-14 Formatos previstos: fases finales, grupos y eliminatorias, clasificados y cruces configurables | F1 (especificar), F2/F4 (modelo), F6 (construir) | Pruebas unitarias y de integración | §4.1 vacíos |
| Q-03 | RP-15 Empates en eliminatorias e ida y vuelta | F1 (especificar), F6 | N-05, N-08 | §15.2 |
| Q-03 | RP-16 Control, validación y auditoría de cambios de reglamento en torneo iniciado | F1, F2, F4 | pgTAP y pruebas de integración | §7.3, H-10 |
| Q-04 | RP-17 Goleadores y tarjetas (`match_events`) en el MVP | F1; módulos 5E/5F | Pruebas unitarias y E2E | T-05 |
| Q-04 | RP-18 Administración de usuarios y permisos | F1; módulo 5C | E2E | RP-05 |
| Q-04 | RP-19 Vista pública y panel administrativo | F3 (wireframes y sistema visual); módulo 5F | E2E, axe, 390 px | C-10 |
| Q-04 | RP-20 Copa, relámpago y otras capacidades diferidas | F6 | — | C-03, C-04 |
| Q-04 | RP-21 Re-estimación de plazo y costo del MVP ampliado | F1, F2 | Plan aprobado | T-02 |
| Q-05 | RP-22 Perfil público limitado a información deportiva | F1 (campos), F3, F4 (vistas) | pgTAP, revisión manual | C-12 |
| Q-05 | RP-23 Consentimiento del representante legal, vigencia y evidencia | F1, F4 | UAT; revisión jurídica | Q-24 |
| Q-05 | RP-24 Revocación y su efecto en lo publicado y en la auditoría | F2, F4 | pgTAP, pruebas de integración | T-04 |
| Q-05 | RP-25 Minimización y control de acceso a datos de menores | F4, F7 | pgTAP, revisión de privacidad | H-09, H-13 |
| Q-05 | RP-26 Revisión jurídica antes del lanzamiento | F8 | Evidencia documental | Q-01 |
| Q-06 | RP-27 Capacidad 3–6 torneos × 6–16 equipos | F2 | Prueba de carga (k6) en F7 | §1.3 |
| Q-06 | RP-28 Captura sin conexión con sincronización posterior | F1 (especificar), F2 (arquitectura formal); módulo 5E | Integración con red cortada; iOS real | H-08, N-13 |
| Q-06 | RP-29 Idempotencia y deduplicación de operaciones | F2, F4; módulo 5E | pgTAP e integración | H-08 |
| Q-06 | RP-30 Conflictos entre editores y con correcciones | F2, F4; módulo 5E | N-06 y casos de conflicto | T-03 |
| Q-06 | RP-31 Distinción visual: pendiente vs confirmado | F3; módulos 5E/5G | E2E, revisión manual | — |
| Q-06 | RP-32 Pruebas de integración y de conflictos | F7 | Suite en CI | — |
| Q-07 | RP-33 Cuentas y dominio de la organización; acceso técnico del desarrollador | F2, F8 | Inventario de cuentas | Q-07 |
| Q-07 | RP-34 Verificación de costos, límites, restricciones comerciales y respaldos antes de contratar | F2 (ADR), F8 | Fuentes oficiales | T-06, §5.3 |
| Q-07 | RP-35 Plan gratuito del piloto condicionado a adecuación legal, técnica y operativa | F2, F7 | Revisión documentada | T-06 |
| Q-07 | RP-36 Continuidad: sucesión de accesos y runbook | F8 | Revisión del checklist | §3.3 punto 8 |

Cobertura: cada una de Q-01…Q-07 tiene al menos tres requisitos previstos. Los hallazgos H-01…H-13 y C-01…C-15 se asignan a una fase en la tabla anterior o en §13; los que no aparecen siguen en §7–§8 como insumo de las Fases 2 y 4.

---

## 17. Evaluación de los criterios de aceptación de la Fase 0 y recomendación

### 17.1 Estado frente a los criterios de §11

| Criterio | Estado |
|---|---|
| El informe se trata como propuesta y el SQL no se adopta tal cual | **Pendiente de aceptación explícita.** Las decisiones recibidas no lo contradicen, pero el usuario no lo ha afirmado. |
| Clasificación de requisitos y contradicciones C-01…C-15 aceptada | **Pendiente de aceptación explícita.** C-01, C-02 y C-12 quedan resueltas (parcial o totalmente) por Q-02 y Q-05. |
| H-01…H-13 aceptados como insumo de Fase 4 | **Pendiente de aceptación explícita.** Q-02 refuerza H-02. |
| Preguntas bloqueantes Q-01…Q-07 decididas o delegadas | **Cumplido.** Todas decididas ([CONF], §12). |
| MVP validado, ajustado o rechazado | **Cumplido.** Sustituido por el MVP ampliado de Q-04. |

### 17.2 Asuntos aún abiertos
- Revisión jurídica y formalización de responsabilidades (decisión confirmada, ejecución pendiente).
- Tensiones T-01…T-06 y preguntas Q-17…Q-25 (Q-17, Q-18 y Q-19 ya resueltas; ver §18 y §19).
- Preguntas previas Q-08…Q-16.
- Verificación oficial de costos, límites, respaldos y condiciones comerciales de los proveedores.
- Re-estimación del MVP ampliado con modo sin conexión.
- Ninguna verificación en base real del SQL del informe (corresponde a la Fase 4).

### 17.3 Recomendación [REC]
Las siete preguntas bloqueantes están respondidas y las decisiones son coherentes entre sí, con las tensiones T-01…T-06 identificadas y ninguna de ellas bloqueante para empezar la Fase 1. Por lo tanto, **recomiendo considerar que la Fase 0 cumple sus criterios de aceptación sustantivos**, con estas condiciones:
1. Que usted confirme expresamente los tres puntos de aceptación pendientes de §17.1 (informe como propuesta con SQL no adoptado, contradicciones, hallazgos como insumo).
2. Que la Fase 1 empiece resolviendo Q-17, Q-18 y Q-19 (resueltas después en §18 y §19), porque definen el alcance real del MVP, y que las demás preguntas se cierren dentro de la Fase 1 según su prioridad.
3. Que la revisión jurídica y la verificación oficial de costos queden registradas como puertas obligatorias antes de publicar, no como requisito para iniciar la Fase 1.

Esta recomendación no es una aprobación. *(Nota de cierre: el usuario aprobó la Fase 0 el 2026-11-15; ver §18. Esta sección se conserva como registro de la recomendación en v1.1.)*


---

## 18. Cierre de la Fase 0 — aprobación y aclaraciones del usuario

Fuente: mensaje «CIERRE DE FASE 0» del 2026-11-15. Todo lo de esta sección es **[CONF]** salvo lo marcado como pendiente.

### 18.1 Estado
- **Fase 0: APROBADA** el 2026-11-15 (entregables v1.1, con las aclaraciones de abajo).
- **Q-19:** pendiente de confirmación final al momento de v1.2; **confirmada después (§19)**.
- **Q-08 a Q-16 y Q-20 a Q-25:** asuntos de especificación posterior, según corresponda.
- **Fase 1: no autorizada.** Tampoco están autorizados commit, push, merge, migraciones, instalación de dependencias ni despliegues.

### 18.2 Validez del informe original [CONF]
- `Informe_Torneo_de_Campeones.pdf` es una **propuesta inicial**, no una especificación definitiva.
- El SQL del informe **no está aprobado para implementación directa**: deberá revisarse, corregirse, diseñarse y probarse en las fases técnicas.
- C-01…C-15 y H-01…H-13 se aceptan como **insumos de investigación y validación**, no como decisiones de implementación aprobadas automáticamente.

### 18.3 Q-17 — Funcionamiento sin conexión [CONF]
- La plataforma debe permitir capturar **marcadores y eventos deportivos** sin conexión a Internet, conservarlos localmente y sincronizarlos al recuperar la conexión.
- Los resultados locales deben diferenciarse claramente de los **confirmados y publicados por el servidor**.
- Prevención de duplicados, resolución de conflictos, revocación de permisos y seguridad de los datos locales se especifican en las fases correspondientes (insumo: §15.1).

### 18.4 Q-18 — Alcance del MVP [CONF]
El MVP ampliado incluye **únicamente la modalidad de liga**, con: equipos y jugadores; calendarios; resultados; tablas de posiciones; goleadores y tarjetas; W.O. configurable; una o dos vueltas; desempates configurables; usuarios y roles; captura offline y sincronización posterior; vista pública y panel administrativo.

**Fuera del MVP** (se incorporan después): eliminatorias, copas, relámpagos y **fases de grupos**.

**[DER]** Lo de Q-03 sobre fases finales, grupos más eliminatorias, clasificados, cruces y ida y vuelta eliminatoria pasa a «previsto, no construido» (RP-14 y RP-15 se construyen en la Fase 6; se conservan como requisitos de diseño para que el modelo no los impida). La decisión Q-03 sobre reglamento no se modifica.

### 18.5 Q-19 — Invitación de organizadores [PROPUESTA en v1.2; CONFIRMADA en §19]
- Solo el **dueño** crea o invita organizadores.
- Los organizadores invitan editores y delegados **exclusivamente en los torneos que administran**.
- *(Estado en v1.2: propuesta. El usuario la confirmó el mismo día; ver §19.)*

### 18.6 Condiciones para las siguientes fases [CONF]
1. La revisión jurídica de privacidad y menores es obligatoria antes del lanzamiento.
2. Condiciones comerciales, costos y restricciones de infraestructura se verifican con fuentes oficiales.
3. La estimación de duración se actualiza considerando el MVP ampliado y el funcionamiento offline.
4. Ninguna recomendación técnica puede sustituir las decisiones aprobadas.

### 18.7 Insumos que pasan a la Fase 1 (cuando se autorice)
- **Matriz de permisos:** Q-19 está confirmada (§19); la matriz completa y los casos límite se especifican en la Fase 1.
- **Especificar:** modo sin conexión (§15.1, con las decisiones de §18.3), protección de menores y consentimiento (§15.3), reglamento, desempates y W.O. (§15.4), con las preguntas Q-08…Q-16 y Q-20…Q-25 según corresponda.
- **Actualizar** la estimación de duración y el inventario de pantallas del MVP.
- **Diferido a Fase 6:** empates eliminatorios e ida y vuelta (§15.2).
- Los requisitos RP-xx de §16 siguen siendo provisionales.

### 18.8 Lo que la Fase 0 no hizo
Código, esquema, migraciones, dependencias, despliegues; verificación del SQL en base real; verificación de fuentes oficiales, costos o legislación. La §17.3 era una recomendación; la aprobación es del usuario.


---

## 19. Cierre definitivo de la Fase 0 — Q-19 confirmada

Fuente: mensaje «CIERRE DEFINITIVO DE FASE 0» del 2026-11-15. Todo lo de esta sección es **[CONF]**.

### 19.1 Decisión Q-19 — Aprobada
| Rol | Alcance |
|---|---|
| Dueño | Único rol autorizado para **invitar y designar organizadores**. Conserva el control global de usuarios y permisos. |
| Organizador | Invita y administra **editores y delegados** exclusivamente dentro de los torneos que tiene asignados. |
| Editor | No puede invitar usuarios ni asignar roles. |
| Delegado | No puede invitar usuarios ni asignar roles. |

### 19.2 Restricciones obligatorias
1. Ningún organizador puede otorgar el rol de dueño.
2. Ningún organizador puede crear, invitar o designar otros organizadores.
3. Los permisos siempre están limitados al torneo correspondiente.
4. Toda asignación, modificación o revocación de permisos queda **auditada**.
5. La autorización se comprueba en el **backend y en la base de datos**, no solo en la interfaz.
6. La matriz completa de permisos y los casos límite se especifican en la **Fase 1**.

### 19.3 Efecto sobre secciones anteriores
- Resuelve C-01 y, junto con Q-02, fija el alcance de invitación (§12 Q-02, §18.5). RP-05 (matriz de permisos v2) y RP-06 (invitaciones con alcance y sin escalada) se especifican en la Fase 1 y se diseñan en las Fases 2 y 4.
- **[DER]** Los requisitos de auditoría de H-07 se extienden a roles y asignaciones; «organizador asignado» deberá vincularse a un torneo concreto.
- **Sigue sin decidirse (Fase 1):** cómo queda autorizado el organizador que crea un torneo (Q-02 pendiente), quién asigna editores a partidos, revocación, múltiples roles por persona (Q-15) y 2FA para editores (Q-08).

### 19.4 Coherencia de decisiones confirmadas (verificada)
| Decisión | Estado |
|---|---|
| Q-01 Costa Rica; organización responsable; revisión jurídica pendiente | Vigente |
| Q-02 Control delegado con cinco roles | Vigente; Q-19 lo precisa para organizadores |
| Q-03 Reglamento (3/1/0, desempates configurables, W.O. 3-0, formatos previstos) | Vigente; los formatos de eliminatoria y grupos quedan fuera del MVP por Q-18 |
| Q-04 MVP ampliado | Vigente; acotado a liga por Q-18 |
| Q-05 Menores | Vigente |
| Q-06 Dimensionamiento y offline | Vigente; precisado por Q-17 |
| Q-07 Propiedad e infraestructura | Vigente |
| Q-17 Offline | Vigente |
| Q-18 MVP solo liga | Vigente |
| Q-19 Invitación de organizadores | Vigente (este cierre) |

No se detectaron contradicciones entre estas decisiones.

### 19.5 Estado de las fases
- **Fase 0: APROBADA.** Fases 1 a 8 y módulos 5A a 5G: **NO INICIADAS**.
- Asuntos de especificación posterior, según corresponda: Q-08…Q-16 y Q-20…Q-25.
