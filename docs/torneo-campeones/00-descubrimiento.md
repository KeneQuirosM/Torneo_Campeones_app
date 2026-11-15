# 00 · Descubrimiento y validación del informe — Torneo de Campeones

- **Fase:** 0 (Descubrimiento y validación del informe)
- **Estado:** PENDIENTE DE APROBACIÓN (no aprobada; este archivo no es una aprobación)
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

**Nada está «confirmado» por el usuario o la organización en esta sesión.** El único acto de consentimiento verificable es la autorización de la Fase 0. Hasta que haya respuestas a las preguntas de la sección 10, todo lo demás es [DECL], [PROP] o [SUP].

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

### 9.2 Candidato de MVP (reducción propuesta)
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

**Esta fase no ha hecho:** código, esquema, migraciones, instalación de dependencias, servicios externos, commits, push ni despliegues. **No verificó:** fuentes externas, comportamiento real del SQL, mockup, legislación.
