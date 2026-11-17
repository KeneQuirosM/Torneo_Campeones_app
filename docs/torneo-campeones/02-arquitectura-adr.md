# 02 · Arquitectura y amenazas — Torneo de Campeones

- **Fase:** 2 — Arquitectura y amenazas.
- **Estado del documento:** **R1 APROBADO — 2026-11-16 (v0.5-R1).** Aprobación formal del bloque R1 otorgada por el usuario y registrada en `aprobaciones.md` y `bitacora.md`; alcance: exclusivamente el alcance documental de este documento. **La Fase 2 NO está aprobada globalmente.** Las decisiones técnicas de R2, R3 y R4 siguen abiertas; ninguna decisión de este documento selecciona proveedor, stack ni mecanismo.
- **Estado de la fase:** Fase 2 **AUTORIZADA, NO APROBADA globalmente**. R1 autorizado el 2026-11-15 y **APROBADO el 2026-11-16**; **R2, R3 y R4: NO INICIADOS Y SIN AUTORIZACIÓN**.
- **Estado de R2-A (añadido el 2026-11-16 y actualizado el 2026-11-17):** **R2-A v0.5-R2A: APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-17), por acto expreso del usuario, registrado en `aprobaciones.md` y `bitacora.md`.** Es una aprobación documental: **no aprueba las formulaciones, capacidades, mecanismos ni alternativas propuestas para implementación; ADR-03.C y ADR-09.C siguen como PROPUESTA; PAR-01…06 sin fijar; RES-01…12 sin aceptar; D-2 sin excepción; 12 decisiones abiertas (la 11 atendida); no aprueba R2 en conjunto ni la Fase 2.** Aprobada **sin una nueva revisión independiente posterior a las últimas correcciones** (corregido tras una auditoría y tres revisiones focalizadas; **sin nueva revisión independiente**), autorizado exclusivamente para el modelo inicial de amenazas, los casos de abuso, ADR-03.C y ADR-09.C. **R2-B y R2-C: NO INICIADOS Y SIN AUTORIZACIÓN.** La línea anterior describe el estado a la fecha de aprobación de R1; para R2-A prevalece esta línea y §16.0. **El contenido de §0 a §15 es el de R1 (v0.5-R1): 0 líneas eliminadas o modificadas, con dos inserciones editoriales (esta línea y una fila de §13); el trabajo de R2-A está en la Parte II (§16 en adelante).**
- **Versión:** 0.5-R1 (identificación conservada para coherencia con `aprobaciones.md` y `bitacora.md`; incluye una sincronización editorial posterior, §13) · **Fecha:** 2026-11-15 (creación); v0.4 y v0.5: 2026-11-16 · **Rama:** `claude/fase-2-arquitectura-amenazas`.
- **Fuente de verdad:** `01-requisitos.md` v1.3.1 (Fase 1 APROBADA CON CONDICIONES) y `00-descubrimiento.md` (Fase 0 APROBADA). Este documento no modifica ninguno de los dos.
- **Autorizaciones que lo sustentan:** (1) bloque R1: «AUTORIZO EL INICIO Y DESARROLLO EXCLUSIVAMENTE DEL BLOQUE R1 DE LA FASE 2 — ARQUITECTURA Y AMENAZAS.»; (2) ronda de correcciones: «AUTORIZO EXCLUSIVAMENTE UNA RONDA DE CORRECCIONES DOCUMENTALES DEL BORRADOR R1.», que incluye la decisión sobre AUD-02 (R2 antes de R3) y la excepción limitada sobre AUD-14 (§2.5); (3) «AUTORIZO EXCLUSIVAMENTE UNA SEGUNDA RONDA DE CORRECCIONES DOCUMENTALES DEL BLOQUE R1.» (condiciones C1–C8 de la segunda auditoría; registro en §15); (4) «AUTORIZO EXCLUSIVAMENTE UNA CORRECCIÓN DOCUMENTAL FOCALIZADA DEL BORRADOR R1.» (NUEVO-10, 13, 16 a 20 de la revisión focalizada; registro en §15.3); (5) «AUTORIZO EXCLUSIVAMENTE UNA ÚLTIMA EDICIÓN DOCUMENTAL FOCALIZADA DEL BORRADOR R1.» (NUEVO-21 a NUEVO-26 de la verificación final; registro en §15.4). (6) «AUTORIZO EXCLUSIVAMENTE UNA SINCRONIZACIÓN DOCUMENTAL DEL BORRADOR R1.» (solo referencias a AUD-20, sin cambiar la versión; registro en §13 y §15.5). **El registro de estas autorizaciones en `aprobaciones.md` y `bitacora.md` (AUD-20) fue atendido documentalmente el 2026-11-16 mediante una autorización documental independiente; R1 fue APROBADO formalmente por el usuario el 2026-11-16 (registro en `aprobaciones.md` y `bitacora.md`; la Fase 2 sigue sin aprobación global).**
- **Qué no es:** no es la aprobación de la Fase 2 ni de ningún proveedor o stack, y no sustituye el registro de la aprobación de R1 (que consta en `aprobaciones.md` y `bitacora.md`); la aprobación de R1 no es una validación de implementación; no es diseño técnico detallado (Fase 4); no contiene código, esquemas, prototipos ni servicios provisionados.

---

## 0. Cómo leer este documento

### 0.1 Etiquetas

| Etiqueta | Significado |
|---|---|
| **[CONF]** | Decisión confirmada por el usuario en las Fases 0, 1 o 2. Vinculante; este documento la respeta y no la cambia. |
| **[DER]** | Consecuencia derivada de una decisión confirmada o requisito aprobado. |
| **[SUP]** | Supuesto no confirmado (por ejemplo, cifras de capacidad). |
| **[REC]** | Propuesta de este análisis. **No es una decisión.** |
| **[ABIERTO]** | Pregunta, requisito `COND` o ambigüedad abiertos en `01 §13`. Se **aíslan** en el diseño; **no se resuelven aquí**. |

### 0.2 Convención de referencias

- **«§n» sin prefijo** designa una sección **de este documento**.
- **«`01 §n`»** designa una sección de `01-requisitos.md`; **«`00 §n`»**, de `00-descubrimiento.md`.
- Identificadores propios: `RC-` restricciones confirmadas · `PA-` principios · `CMP-` componentes lógicos · `TB-` límites de confianza · `FL-` flujos · `UI-` unidades indivisibles · `ADR-` decisiones previstas · `HR1-` hallazgos de R1 · `CR1-` criterios de revisión · `AUD-` hallazgos de la auditoría de R1.
- Los identificadores `RF-`, `RD-`, `OFF-`, `PRV-`, `RNF-`, `RT-`, `CL-`, `Q-`, `A-`, `D-` y `RK-` son los de `01-requisitos.md`.

### 0.3 Glosario de estados y términos

Los datos de una captura pasan por tres niveles. **Cada término se usa solo con su significado.**

| Nivel | Término | Significado | ¿Público? |
|---|---|---|---|
| N0 | **Local** | Operación en el dispositivo: `pendiente`, `enviando`, `en conflicto` o `rechazada` (`01 §9.2`) | **Nunca** |
| N1 | **Confirmada** (aceptada por el servidor) | El servidor aplicó la operación y es parte del estado oficial | Solo si además está visible |
| N2 | **Visible o publicado** | Confirmada y, además, torneo `publicado`/`en curso`/`finalizado`, jornada publicada (`01 §9.6`) | **Sí** |

«Publicación» tiene en los requisitos **tres significados distintos**, que este documento nombra por separado:

| Término | Significado | Fuente |
|---|---|---|
| **Habilitar la visibilidad** | Publicar o despublicar un torneo o una jornada | RD-CAL-08, RF-PUB-04 |
| **Propagación automática** | Un resultado confirmado pasa a las vistas públicas sin validación manual | CONF Q-32, RD-RES-07 |
| **Autorización de nombre público** | Permiso vigente para mostrar el nombre de un jugador (menores) | PRV-02, PRV-05 |

### 0.4 Regla de este documento

**No se seleccionan lenguajes, frameworks, proveedores, esquemas SQL ni servicios.** Los nombres de tecnología solo aparecen en el inventario de ADR (fila de ADR-01, §8.1) como candidatos no vinculantes que fijó el usuario. **Única excepción:** HR1-13 cita textualmente el nombre del motor que figura en dos requisitos aprobados (`01 RNF-11` y `01 RT-13`) para hacer visible que acota ADR-01; no es una elección de este documento.

---

## 1. Alcance y límites de R1

### 1.1 Qué cubre R1

1. Línea base de calidad (17 RNF), restricciones confirmadas y excepción registrada (§2).
2. Contexto y actores del sistema (§3).
3. Arquitectura lógica independiente del stack (§4, §5).
4. Componentes, responsabilidades únicas, unidades indivisibles, clasificación de datos y límites de confianza (§5).
5. Aislamiento multi-torneo (§6).
6. Flujos conceptuales: lectura pública, administración, captura sin conexión y publicación (§7).
7. Inventario de ADR previstos y matriz de dependencias, sin resolverlos (§8).
8. Trazabilidad inicial de las 18 preguntas abiertas, 23 requisitos `COND` y 14 ambigüedades (§9).
9. Criterios verificables de revisión de R1 (§10).

### 1.2 Qué no cubre

| Fuera de R1 | Dónde se tratará |
|---|---|
| Modelo de amenazas detallado y casos de abuso | R2 |
| Requisitos, restricciones e invariantes de seguridad **independientes del proveedor** (autorización, identidad, visibilidad, sincronización, auditoría, almacenamiento local) | R2 |
| Estrategia de actualización, caché y tiempo real como decisión; evaluación de proveedores, región y costos con fuentes oficiales; ADR principal | R3 |
| Entornos, respaldo, observabilidad, continuidad; auditoría de cierre por revisor separado del autor y con contexto nuevo | R4 |
| Esquema normalizado, migraciones, matriz de políticas de acceso por tabla, contratos de API, plan de pruebas detallado | Fase 4 |
| Wireframes, sistema visual, prototipos de interfaz | Fase 3 |
| Código, prototipos ejecutables, servicios, cuentas, gastos | Ninguna fase en este bloque |

Este borrador **identifica** puntos de atención para R2 (§5.7) y **asigna** ADR a bloques (§8); no los desarrolla.

---

## 2. Línea base de calidad y restricciones

### 2.1 Requisitos no funcionales como exigencias de diseño

«Elementos» remite a los componentes de §5. «Verifica» indica dónde se comprobará el atributo (no se ejecutó ninguna prueba).

| RNF | Atributo | Exigencia de diseño | Elementos | Estado | Verifica |
|---|---|---|---|---|---|
| RNF-01 | Rendimiento web | Vistas públicas ligeras y servibles de forma eficiente; datos públicos derivados, no consultas pesadas por visita | CMP-01, CMP-14, CMP-15 | REC (informe) | F7 (campo y laboratorio) |
| RNF-02 | Accesibilidad | La arquitectura no impone interfaces que impidan WCAG 2.2 AA; estados de operación con texto e icono (OFF-04) | CMP-01, CMP-02, CMP-03 | CONF (skill) | F3, F7 |
| RNF-03 | Degradación y disponibilidad | Mejor esfuerzo sin porcentaje contractual; captura y consulta sin conexión; errores y recuperación explícitos | CMP-01, CMP-03, CMP-04, CMP-05, CMP-15 | CONF (Q-16) | F7 |
| RNF-04 | Capacidad | Dimensionar para 3–6 torneos, 6–16 equipos, ≤ 240 partidos por torneo, 100–500 lectores simultáneos [SUP] | CMP-13, CMP-14, CMP-15 | CONF / SUP | F7 (carga) |
| RNF-05 | Seguridad | Autorización en servidor y en la capa de datos (CMP-08); credenciales de servicio solo en servidor; **cabeceras de seguridad** (CMP-15 y clientes); **dependencias escaneadas** (CMP-19); **límites de frecuencia** en autenticación (CMP-06) e invitaciones (CMP-07) | CMP-06, CMP-07, CMP-08, CMP-15, CMP-16, CMP-19 | CONF (Q-02) | F4, F7 |
| RNF-06 | Integridad | Sin pérdida por concurrencia, reintento o desconexión; restricciones críticas en la capa de datos | CMP-08, CMP-11, CMP-13 | CONF (Q-06) | F4, F7 |
| RNF-07 | Compatibilidad | Android 9+ (Chrome), iPhone iOS 16.4+ (Safari), escritorio; iOS sin sincronización en segundo plano | CMP-01…CMP-05 | REC | F7 (dispositivos reales) |
| RNF-08 | Respaldo y recuperación | Respaldos verificables y restauración probada antes del piloto; RPO 24 h y RTO 8 h propuestos | CMP-19 (respaldos en Z3) | CONF + REC | F7, F8 |
| RNF-09 | Observabilidad | Errores y disponibilidad con alerta, sin datos personales (PRV-12) | CMP-19 | REC | F7 |
| RNF-10 | Localización | Español de Costa Rica; zona `America/Costa_Rica`; fechas dd/mm/aaaa, 24 h | CMP-01…CMP-03, CMP-09, CMP-11 | DER (Q-01) | F7 |
| RNF-11 | Mantenibilidad y pruebas | **Resumen de `01 RNF-11` (el motor exacto que fija el requisito se señala en HR1-13):** pruebas unitarias con fixtures para las reglas, pruebas de restricciones contra un almacén relacional real, esquema y migraciones versionados. **Derivado [DER] de PA-02:** reglas de dominio separadas de la infraestructura y motor de clasificación puro | CMP-12 y transversal | CONF (skill) / DER | F4, F7 |
| RNF-12 | Costos y planes | Priorizar planes gratuitos solo si son legal, técnica y operativamente adecuados; verificar con fuentes oficiales | ADR-01, ADR-10 | CONF (Q-07, Q-25) | R3 |
| RNF-13 | Usabilidad de captura | ≤ 3 toques por gol; resultado final directo < 30 s (sin línea base, D-12) | CMP-03 | REC | F3, F7 (UAT) |
| RNF-14 | Propiedad y acceso | Cuentas, dominio y datos a nombre de la organización; desarrollador con credenciales propias revocables | CMP-19, ADR-01 | CONF (Q-07) | F8 |
| RNF-15 | Red lenta | Vistas públicas utilizables con red 3G lenta; la captura no se bloquea | CMP-01, CMP-03, CMP-05, CMP-15 | REC | F7 |
| RNF-16 | Entornos y despliegue | Local, staging y producción; despliegues solo con autorización específica | CMP-19, ADR-11 | REC / CONF (skill) | F8 |
| RNF-17 | Continuidad | Manual de operación y sucesión de accesos; continuar capturando sin conexión si cae el sistema | CMP-19, CMP-03, CMP-04 | REC | F8 |

### 2.2 Restricciones confirmadas que vinculan la arquitectura

| ID | Restricción | Fuente |
|---|---|---|
| RC-01 | Operación en Costa Rica; la organización es la responsable del tratamiento | CONF Q-01, PRV-01 |
| RC-02 | MVP: solo liga, 1 o 2 vueltas, reglamento versionado; copa, relámpago y eliminatorias fuera del MVP (el modelo no debe impedirlos) | CONF Q-04, Q-18, RF-TOR-07 |
| RC-03 | PWA instalable, mobile-first, con vistas públicas por enlace y código QR | CONF (skill), RF-PWA-01, RF-TOR-06 |
| RC-04 | La autorización definitiva se comprueba en el servidor **y** en la capa de datos; ocultar controles de interfaz no es control de seguridad. **Ninguna alternativa de una sola capa es admisible** [DER: consecuencia de que RT-02 exige ambas] | CONF Q-02, RT-02 |
| RC-05 | Captura sin conexión como requisito central: marcador, eventos y cambios de estado; sincronización posterior segura. **No se depende de la sincronización en segundo plano** (iOS no la ofrece); con la app cerrada no se promete envío | CONF Q-06, Q-17; DER OFF-05 |
| RC-06 | Parámetros y 12 escenarios de conflicto del modo sin conexión aprobados íntegramente | CONF Q-34 (`01 §9.4`, `01 §9.5`) |
| RC-07 | Lo local jamás es público; el público solo ve operaciones confirmadas de partidos visibles | CONF Q-17, OFF-15, `01 §9.6` |
| RC-08 | Se rechaza toda operación sin autorización vigente al sincronizar, aunque se capturara legítimamente | CONF D-11, OFF-22 |
| RC-09 | Un único editor vigente por partido; reasignación atómica y auditada | CONF Q-36 A, RF-ACC-06 |
| RC-10 | Autorización atómica (RT-13); conflicto de interés de dueño u organizador con rol de delegado evaluado **por efecto** y con persistencia histórica (RT-14, Q-44 B, Q-45 C) | CONF |
| RC-11 | 2FA obligatorio si la cuenta tiene rol de dueño u organizador; opcional y recomendado para editores y delegados; política efectiva por todos los roles vigentes | CONF Q-08, Q-39, RF-ACC-10 |
| RC-12 | Segundo dueño; protección atómica del último dueño activo | CONF Q-35, RT-06 |
| RC-13 | Protección de menores: etiqueta por defecto; nombre público solo con autorización vigente; sin datos sensibles en vistas públicas; sin nombres en auditoría | CONF Q-05, PRV-02/03/09, RF-AUD-04 |
| RC-14 | Ningún dato personal real antes de concluir la evaluación jurídica; solo datos sintéticos en desarrollo y pruebas | CONF PRV-15, PRV-17, D-10 |
| RC-15 | Se permite **evaluar** infraestructura internacional con evaluación previa; no autoriza contratar ni desplegar | CONF Q-25, PRV-13 |
| RC-16 | Propagación automática al confirmar el resultado | CONF Q-32 |
| RC-17 | Disponibilidad de mejor esfuerzo en el piloto, sin porcentaje contractual | CONF Q-16, RNF-03 |
| RC-18 | Cuentas, dominio, base de datos y hosting a nombre de la organización; la organización asume los costos; desarrollador con acceso técnico individual | CONF Q-07, RNF-14 |
| RC-19 | Riesgo RK-15 aceptado: sin recuperación excepcional de capturas rechazadas | CONF (aceptación expresa) |
| RC-20 | RF-EQP-06 (vínculo de identidad entre torneos) diferido; Q-37 B se mantiene como regla de gobernanza; el modelo no debe impedirlo | CONF |
| RC-21 | Auditoría y historial de asignación de roles de solo anexado; ningún rol de la aplicación actualiza ni borra registros | CONF RF-AUD-03, RF-ACC-11 |
| RC-22 | **Fase 2:** sin gastos, sin cuentas, sin servicios, sin prototipos, sin código; investigación solo con fuentes oficiales | CONF (autorización de la Fase 2) |
| RC-23 | Ningún proveedor ni stack está aprobado; el stack del informe es una propuesta sujeta a ADR | CONF (skill; autorización de la Fase 2) |
| RC-24 | **Excepción limitada sobre OFF-17:** en la Fase 2 solo investigación documental sobre iOS; la prueba en dispositivo real se traslada a 5E y la Fase 7. No modifica el comportamiento funcional exigido (§2.5) | CONF (autorización de la ronda de correcciones, AUD-14) |
| RC-25 | **Orden R2 antes de R3.** En R2 se definen requisitos, restricciones e invariantes de seguridad independientes del proveedor; las decisiones de implementación que dependan del stack quedan condicionadas a ADR-01 (R3) | CONF (decisión AUD-02) |

### 2.3 Supuestos no confirmados

| ID | Supuesto | Estado |
|---|---|---|
| SUP-01 | Lectura pública simultánea de 100–500 usuarios en jornada | [SUP], RNF-04 |
| SUP-02 | Base instalada real de dispositivos | [SUP], RNF-07 |
| SUP-03 | Los objetivos de RPO 24 h, RTO 8 h, ≤ 60 s de aparición del resultado y 50 operaciones en ≤ 15 s son propuestas | [REC], RNF-08, RF-PUB-09, OFF-19 |

### 2.4 Criterios de comparación fijados por el usuario

Para los ADR se compararán las alternativas por **costos bajos, seguridad, funcionamiento sin conexión y mantenimiento** (condición 4 de la autorización de la Fase 2). Los **pesos** de esos criterios no están definidos: se propondrán en el ADR principal y quedan sujetos a su aprobación.

### 2.5 Excepción registrada: verificación de OFF-17 en dispositivo real

| Elemento | Contenido |
|---|---|
| **Requisito original** | `01 OFF-17`: «Comportamiento de iOS: verificar en dispositivo real en Fase 2» (el navegador puede eliminar los datos locales; la interfaz advierte y no promete conservación). Seguimiento de `01 RK-11`: F2, 5G |
| **Excepción autorizada** | Durante la Fase 2 se realiza **únicamente investigación documental** sobre capacidades y limitaciones de iOS. La prueba real se traslada a las fases posteriores previstas, **incluidas 5E y la Fase 7**. `01 RK-11` fija además «F2, 5G» como seguimiento: **5G se menciona como verificación adicional prevista en ese riesgo**, no como parte de la excepción autorizada por el usuario (5E y Fase 7) |
| **Motivo** | La Fase 2 no admite código, prototipos ni ejecución de pruebas (RC-22); no existe aplicación que probar |
| **Riesgos de posponer la prueba** | Un ADR (ADR-05, ADR-16) podría apoyarse en una limitación de iOS documentada pero no comprobada; el comportamiento real (persistencia, cuotas, instalación, borrado) podría diferir; un hallazgo tardío podría exigir rediseñar el almacenamiento local o la sincronización cuando ya hay código |
| **Obligación de verificar después** | Cada afirmación sobre iOS en los ADR se marca **«no verificada en dispositivo»** con su fuente documental y fecha; se verifica en iPhone y Android reales en 5E y la Fase 7 (y en 5G, por `01 RK-11`), con registro de resultado (`ejecutado/aprobado`, `falló`, `no ejecutado`, `no verificable`) |
| **Alcance de la excepción** | **No elimina ni modifica el comportamiento funcional exigido** por OFF-17 (advertencia visible, solicitud de almacenamiento persistente, ninguna promesa de conservación). Solo cambia el **calendario de la verificación en dispositivo real** |

---

## 3. Contexto y actores del sistema

### 3.1 Actores

| Actor | Descripción | Fuente |
|---|---|---|
| Visitante (público) | Consulta lo publicado, sin iniciar sesión | RF-ACC-14 |
| Usuario autenticado sin rol | Mismos permisos que el público | RF-ACC-14 |
| Dueño | Gobierno global; puede haber un segundo dueño | Q-19, Q-35 |
| Organizador | Administra sus torneos; queda asignado automáticamente al crear uno | Q-02a |
| Editor | Captura resultados de partidos asignados | Q-02, RF-ACC-06 |
| Delegado | Administra la plantilla de su equipo; una persona es delegada de un solo equipo por torneo | Q-02, RF-ACC-13 |
| Usuario con roles combinados | Dueño u organizador con rol de delegado (RT-14); editor y delegado (RT-05) | Q-15, Q-38 |
| Operación automática del sistema | **Una sola está declarada como autorizada previamente:** la asignación inicial del organizador que crea un torneo (RT-04, RF-ACC-12, D-3). **Las demás tareas automáticas** (alertas, vencimientos, retención) **no están declaradas como actor privilegiado**. RT-14 establece que **una operación automática o en cascada de una operación hereda la situación del actor que la dispara**; las tareas programadas **sin actor disparador no tienen identidad de ejecución definida**: es una **pregunta abierta de ADR-14 (su único dueño)**, no una herencia presumida. El programador (Z4) solo dispara; lo que toca datos de jugadores se ejecuta en Z2/Z3 | RT-04, RT-14, D-3 |
| Persona responsable de evidencias | **Actor organizativo** (por designar): custodia las evidencias de autorización y atiende solicitudes de derechos. Este documento **no le asigna ningún permiso** en el sistema | Q-24 |
| Responsable administrativo de cuentas | **Actor organizativo** (por designar): administra servicios y cuentas de la organización. **Sin permisos de aplicación** asignados aquí | Q-07 |
| Desarrollador | **Actor técnico:** acceso a la infraestructura individual y revocable; **sin permisos de aplicación** asignados aquí | RNF-14 |

Los 13 actores de prueba de permisos están en `01 §7.3`; este documento los usa sin redefinirlos.

### 3.2 Diagrama de contexto (conceptual)

```
                    ┌──────────────────────────────────────────────┐
  Visitante ───────►│  Vistas públicas  (lectura)                  │
  (sin sesión)      │                                              │
                    │            SISTEMA TORNEO DE CAMPEONES       │
  Dueño /           │  Administración  ·  Captura de partido       │
  Organizador ─────►│  (con o sin conexión)  ·  Publicación        │
  Editor /          │                                              │
  Delegado          └───┬───────────┬────────────┬────────────┬────┘
                        │           │            │            │
                        ▼           ▼            ▼            ▼
                 Servicio de   Almacén de   Servicio de   Observabilidad
                 identidad     datos        correo        (sin datos de
                 (sesión, 2FA) relacionales transaccional jugadores)
                        ▲                         
        Entrega web, dominio y navegador/PWA (Android, iPhone, escritorio)
```

Los recuadros externos son **categorías de servicio**, no proveedores. Cuáles se usan y si alguno se integra en el propio sistema se decide por ADR (§8).

### 3.3 Sistemas externos necesarios (categorías)

| Categoría | Para qué | Requisitos | ADR |
|---|---|---|---|
| Entrega web y dominio | Servir la aplicación y las vistas públicas con enlace y QR estables | RF-TOR-06, RF-PWA-01 | ADR-01, ADR-06 |
| Identidad, sesión y segundo factor | Autenticación, invitaciones, recuperación | RF-ACC-02, 10, 16 | ADR-03 |
| Almacén de datos relacionales (incluye auditoría y respaldos) | Datos de dominio, restricciones, auditoría | RT-08, RNF-06, RNF-08 | ADR-01, ADR-12 |
| Correo transaccional | Invitaciones y recuperación de cuenta (recibe direcciones de usuarios) | RF-ACC-02, PRV-14 | ADR-15 |
| Observabilidad | Errores y disponibilidad, sin datos de jugadores | RNF-09, PRV-12 | ADR-13 |
| Tareas programadas | Alerta de partidos sin editor, vencimientos, retención | RF-ADM-04, PRV-10 | ADR-14 |

---

## 4. Principios de arquitectura

| ID | Principio | Se deriva de |
|---|---|---|
| PA-01 | **Autoridad única:** el servidor y la capa de datos deciden; el cliente propone y nunca es fuente de verdad | RC-04, RC-08 |
| PA-02 | **Regla de negocio en un solo lugar:** la clasificación y la validación del reglamento tienen una única fuente (H-10 de la Fase 0) | `01 RNF-11`, `01 RD-TAB-10` |
| PA-03 | **Aislamiento por torneo por diseño:** toda entidad de torneo y toda referencia entre entidades pertenecen al mismo torneo; lo global se limita a las categorías de §6.2 | RT-08, RF-TOR-04 |
| PA-04 | **Lo público es una proyección:** el público lee vistas derivadas con lista blanca de campos, nunca las entidades internas | RF-PUB-03, PRV-03, RC-07 |
| PA-05 | **Lo local nunca es público:** una operación solo influye en lo oficial cuando el servidor la confirma (N0 → N1 → N2, §0.3) | OFF-15, RC-07 |
| PA-06 | **Idempotencia y secuencia:** cada operación se aplica como máximo una vez y en orden por partido | OFF-06, OFF-07, OFF-25 |
| PA-07 | **Autorización atómica:** la lectura de roles y estado, el cálculo del efecto, la decisión y la aplicación de la operación ocurren en **una sola unidad indivisible** (§7.0, §5.5) | RT-13 |
| PA-08 | **Minimización por clase de dato:** el nombre interno y el indicador de un menor nunca salen de Z2/Z3 hacia lo público; el nombre **público** solo con autorización vigente (PRV-02); nunca en la auditoría; la exportación se rige por PRV-08 y OFF-24; el dispositivo guarda lo mínimo (PRV-11). Ver §5.7 | PRV-02, 08, 09, 11, 12, RF-AUD-04, RC-13 |
| PA-09 | **Lo abierto se aísla:** toda regla pendiente de decisión (§9) se confina tras un punto de extensión; ninguna respuesta futura debe obligar a rediseñar el conjunto | Condición 7 de la Fase 2 |
| PA-10 | **Simplicidad proporcionada a la escala:** sin microservicios, colas ni cachés distribuidas sin evidencia de necesidad | skill; RNF-04 |
| PA-11 | **Historial inmutable:** los registros de auditoría y de asignación de roles son de solo anexado | RC-21 |
| PA-12 | **Degradación explícita:** toda pérdida de conexión, caducidad o rechazo se muestra, nunca se oculta | RNF-03, OFF-04, OFF-10 |
| PA-13 | **Lectura provisional por caso ante lo ambiguo [REC]:** mientras una decisión de seguridad o de conflicto de interés esté abierta (§9), el sistema aplica **la lectura provisional que `01 §13.7` fija para cada caso**, sin conceder un permiso por interpretación. La **denegación por defecto** se aplica **solo donde `01 §13.7` la indica** (acciones sensibles ambiguas cuyo efecto alcance partidos o decisiones del propio equipo de un actor en conflicto de RT-14). **No retira permisos confirmados:** se mantienen, entre otros, la revocación inmediata (RF-ACC-07), la habilitación de visibilidad (matriz fila 13, A-8) y las dos operaciones autorizadas por separado de A-9. Es un comportamiento provisional y **no una política definitiva**; no amplía ni reduce los permisos confirmados | `01 §13.7`; criterio de la ronda de correcciones |
| PA-14 | **Unidades indivisibles:** las operaciones enumeradas en §5.5 son una sola transacción; ninguna división o agrupación física de componentes puede separarlas | RT-13, RT-06, CL-15 |

---

## 5. Arquitectura lógica

Independiente del stack: describe responsabilidades, no tecnología. Un componente lógico puede agruparse o dividirse al implementarse (ADR-01, ADR-02), **sin romper las unidades indivisibles de §5.5**.

### 5.1 Zonas de confianza

| Zona | Contenido | Nivel de confianza |
|---|---|---|
| **Z0 · Dispositivo** | Navegador o PWA instalada: interfaces, almacén local y cola de operaciones | **No confiable** (el usuario controla el dispositivo; el almacenamiento puede borrarse; el reloj puede estar mal) |
| **Z1 · Borde público** | Entrega de vistas públicas, con o sin caché según ADR-06 | Semiconfiable: solo sirve proyecciones públicas |
| **Z2 · Aplicación** | Identidad, autorización, casos de uso, ingesta de sincronización | Confiable: aplica las reglas |
| **Z3 · Datos** | Almacén relacional con restricciones, auditoría y **respaldos** | Confiable: última línea de defensa de integridad. **Contiene todos los datos, incluidos los de menores**: sus respaldos tienen el mismo nivel de protección |
| **Z4 · Auxiliares** | Correo transaccional, monitoreo y **disparador** de tareas programadas (la ejecución sobre datos de jugadores ocurre en Z2/Z3) | Semiconfiable. **No reciben datos de jugadores ni de menores.** El correo recibe direcciones de usuarios (PRV-14) y el monitoreo, métricas sin datos personales (PRV-12) |

### 5.2 Componentes lógicos

Cada responsabilidad tiene **un único componente responsable** (§5.3).

| ID | Componente | Responsabilidad | Datos que maneja | Zona | Variación por ADR |
|---|---|---|---|---|---|
| CMP-01 | Cliente de lectura pública | Mostrar tabla, calendario, resultados, goleadores, tarjetas y plantillas con nombre público; avisos de actualización y estados de carga, vacío y error | Proyecciones públicas | Z0 | ADR-06 |
| CMP-02 | Cliente de administración | Gestión de torneos, equipos, jugadores, calendario, usuarios, correcciones | Datos de administración según rol | Z0 | — |
| CMP-03 | Cliente de captura de partido | Captura de marcador, eventos y cambios de estado; indicadores de estado de operación | Datos mínimos de partidos asignados | Z0 | ADR-05 |
| CMP-04 | Almacén local y cola de operaciones | Persistir operaciones y datos preparados; versión del formato; **ejecutar** en el dispositivo los plazos de retención local (30 días, `01 OFF-10`) | Operaciones pendientes, datos preparados mínimos (PRV-11) | Z0 | ADR-05, ADR-16 |
| CMP-05 | Sincronizador del cliente | Disparar envíos con la app abierta, reintentar con espera creciente, mostrar conflictos; **presentar y entregar** la exportación limitada con el contenido que fija CMP-17 | Operaciones y estados | Z0 | ADR-05 |
| CMP-06 | Identidad, sesión y segundo factor | Autenticación, recuperación, sesiones, límites de frecuencia de acceso; **emitir el nivel de autenticación** de la sesión; solicitar el envío de correos de acceso a CMP-18 | Cuentas, credenciales, segundo factor | Z2 | ADR-03 |
| CMP-07 | Gestión de accesos | Invitaciones, asignación y revocación de roles, asignaciones de editor, segundo dueño, levantamiento de restricción histórica (RF-ACC-17, S); **fuente única de las asignaciones y de su historial de solo anexado**; límites de frecuencia de invitaciones | Roles, asignaciones, historial de asignaciones, invitaciones | Z2 / Z3 | ADR-02, ADR-09 |
| CMP-08 | Evaluador de autorización | **Único decisor** de permisos por usuario, rol, torneo, equipo, partido y efecto; conflicto de interés; política efectiva de 2FA frente al nivel de autenticación que emite CMP-06; atomicidad con la operación. **Lee** roles e historial de CMP-07; **no los almacena** | Lectura de roles vigentes e históricos | Z2 / Z3 | ADR-02, ADR-03 |
| CMP-09 | Dominio de torneos, reglamento y calendario | Ciclo de vida del torneo, reglamento versionado, generación, vista previa y ajustes de calendario; **habilitar y retirar la visibilidad** (publicar y despublicar torneos y jornadas, RF-TOR-02, RD-CAL-08) según los permisos aprobados (matriz fila 13: dueño y organizador de sus torneos) | Torneos, reglamentos, jornadas | Z2 | — |
| CMP-10 | Dominio de equipos, inscripciones y jugadores | Inscripciones independientes por torneo, plantillas, verificación de duplicados con estado pendiente, retiros | Inscripciones, jugadores | Z2 | — |
| CMP-11 | Dominio de partidos, resultados y eventos | Máquina de estados del partido, resultados, eventos, correcciones, W.O., suspensión y reanudación. **Único punto que aplica los cambios de partido**, tanto de operaciones en línea como de operaciones sincronizadas | Partidos, eventos | Z2 | — |
| CMP-12 | Motor de clasificación y reglas | Cálculo de la tabla, desempates configurables, empates circulares, resolución manual; **única fuente de verdad**. Contrato en §5.9 | Reglamento, resultados | Z2 o Z3 (ADR-08) | ADR-08 |
| CMP-13 | Ingesta de sincronización | Recibir operaciones; aplicar idempotencia, orden por secuencia contigua y detección de conflictos de versión. **No contiene reglas de negocio ni decide permisos**: entrega cada operación a CMP-08 y CMP-11 por el mismo camino que las operaciones en línea | Operaciones recibidas, metadatos de incidencias (OFF-20) | Z2 | ADR-05 |
| CMP-14 | Publicación y proyecciones | **Aplicar** el modelo de visibilidad (`01 §9.6`) leyendo el estado que fija CMP-09 (CMP-14 **no decide** cuándo se publica); **proyecciones públicas** con lista blanca y etiquetas de menores calculadas en servidor; **proyección de preparación** para el editor (OFF-02, PRV-11); registrar una **señal de cambio ante cambios de visibilidad, de resultados y de privacidad, si ADR-06 la adopta** (su entrega y efecto hacia Z1 y Z0 dependen de ADR-06) | Estado de publicación, proyecciones | Z2 / Z3 | ADR-04 |
| CMP-15 | Entrega pública | Servir proyecciones públicas y mantenerlas actualizadas (**por señal de cambio, por expiración o ambas, según ADR-06**); aviso de antigüedad; cabeceras de seguridad | Proyecciones públicas | Z1 | ADR-06, ADR-07 |
| CMP-16 | Auditoría y registro de seguridad | **Registrar** de solo anexado cambios, rechazos por RT-14, intentos denegados y levantamientos. No es fuente de roles (lo es CMP-07) | Registros mínimos sin datos personales de menores | Z3 | ADR-09 |
| CMP-17 | Política de privacidad y retención | **Definir el contenido de las políticas** (retención, anonimización, exportación permitida, ocultar) y **ejecutar en el servidor** las autorizaciones (solo metadatos), la anonimización, la retención de datos del servidor y **la exportación de los datos personales de un jugador (PRV-08)**, previa autorización de CMP-08 | Metadatos de autorización, indicadores; datos personales de un jugador solo durante la exportación | Z2 / Z3 | ADR-17 |
| CMP-18 | Alertas y envío de mensajes | Alerta de partidos sin editor (N-12); **único responsable del envío** de correo transaccional, a solicitud de CMP-06 y CMP-07 | Mínimos de contacto | Z2 / Z4 | ADR-14, ADR-15 |
| CMP-19 | Observabilidad y operación | Monitoreo sin datos personales, escaneo de dependencias, entornos, continuidad; **gestión de respaldos** (almacenados en Z3) | Registros técnicos | Z4 (monitoreo) / Z3 (respaldos) | ADR-11, ADR-12, ADR-13 |

### 5.3 Responsable único por función

| Función | Responsable único | Colaboradores (sin duplicar la responsabilidad) |
|---|---|---|
| Emitir el nivel de autenticación de la sesión | CMP-06 | — |
| Decidir la política efectiva de 2FA y todo permiso | **CMP-08** | CMP-06 (aporta el nivel), CMP-07 (aporta roles) |
| Almacenar roles, asignaciones y su historial | **CMP-07** | CMP-08 lee; CMP-16 audita los cambios |
| Registrar auditoría y registro de seguridad | **CMP-16** | Los demás emiten eventos |
| Aplicar un cambio de partido (en línea o sincronizado) | **CMP-11** | CMP-13 solo entrega; CMP-08 autoriza |
| Calcular la tabla | **CMP-12** | CMP-11 lo invoca |
| Idempotencia, secuencia y conflictos de versión | **CMP-13** | CMP-11 recibe lo ya validado |
| Fijar el contenido de la exportación limitada | **CMP-17** | CMP-05 la presenta |
| Plazos de retención local (ejecutar) | **CMP-04** | CMP-17 fija la política |
| Retención y anonimización en el servidor | **CMP-17** | CMP-16 conserva la auditoría sin datos de menores |
| **Habilitar y retirar la visibilidad** de un torneo o de una jornada (publicar y despublicar) | **CMP-09** | CMP-08 autoriza; CMP-14 lee el estado y aplica el modelo `01 §9.6`; guardia opcional de RT-14 según A-8 |
| Proyecciones públicas y de preparación | **CMP-14** | CMP-15 las sirve |
| Mantenerlas actualizadas hacia el público | **CMP-15** | CMP-14 emite señales |
| Envío de correo y de alertas | **CMP-18** | CMP-06 y CMP-07 lo solicitan |
| Respaldos y su restauración | **CMP-19** | Almacenados en Z3 |
| Exportar los datos personales de un jugador (derechos del titular, PRV-08; matriz fila 34: solo el dueño, fila del organizador `COND` Q-42) | **CMP-17** (define y ejecuta el contenido) | CMP-02 la presenta; CMP-08 autoriza |

### 5.4 Cobertura de requisitos por componente

| Familia | Requisitos | Componentes principales |
|---|---|---|
| Acceso y roles | RF-ACC-01…17 | CMP-06, CMP-07, CMP-08 |
| Torneos | RF-TOR-01…07 | CMP-09, CMP-14 |
| Equipos y jugadores | RF-EQP-01…06; RD-JUG-01…10 | CMP-10 |
| Público | RF-PUB-01…09 | CMP-01, CMP-14, CMP-15 |
| Administración | RF-ADM-01…05 | CMP-02, CMP-07, CMP-18 |
| Auditoría | RF-AUD-01…06 | CMP-16 |
| PWA | RF-PWA-01…04 | CMP-01…CMP-05 (push fuera del MVP) |
| Reglas deportivas | RD-CAL, RD-REG, RD-TAB, RD-WO, RD-RES, RD-EVT, RD-FOR | CMP-09, CMP-11, CMP-12 |
| Modo sin conexión | OFF-01…25 | CMP-03, CMP-04, CMP-05, CMP-13 |
| Privacidad | PRV-01…18 | CMP-14, CMP-17, CMP-16, CMP-19 |
| No funcionales | RNF-01…17 | Ver §2.1 |

Cobertura por familia, comprobable contra las familias de `01 §1`; no sustituye la trazabilidad por requisito individual, que corresponde a la Fase 4.

### 5.5 Unidades indivisibles (restricción de diseño)

Las siguientes operaciones **son una sola transacción sobre los datos de Z2/Z3** (PA-14, RT-13). Si los componentes que intervienen se separan al implementar, la separación **no puede abrir una ventana** entre sus pasos.

**Transacción de datos y señal de cambio son cosas distintas.** Lo transaccional es el **estado de los datos de Z2/Z3** y, **si existieran**, sus proyecciones derivadas persistentes (que existan o no lo decide ADR-04). **Si ADR-06 adopta una señal de cambio**, esta se registra de forma durable dentro de la misma transacción (si ADR-06 elige solo expiración, la unidad no la registra y el resto no cambia); en cualquier caso, la **entrega y el efecto hacia la entrega pública (Z1) y hacia los dispositivos (Z0) son posteriores y no forman parte de la transacción**: ninguna atomicidad se atribuye a operaciones externas a la transacción de datos.

**Quién fija qué (para la coherencia pública):** **ADR-02.C fija la frontera atómica** (qué operaciones forman una unidad indivisible y dónde termina su transacción, UI-01…13, en particular UI-05 y UI-10 para el recálculo); **ADR-05.C aplica esa frontera a la ruta sincronizada** (UI-06 con UI-10 dentro); **ADR-04.C fija la garantía de coherencia pública** como capacidad exigida; **ADR-06 decide el mecanismo** que la cumple y su plazo; **ADR-08 consume** la frontera y la garantía para el motor de clasificación.

**Garantía exigida (no demostrada por este documento):** las vistas públicas relacionadas con un mismo torneo deben mantener **coherencia entre resultados y clasificaciones**: no se publica un resultado confirmado junto con una clasificación que no lo refleje de forma persistente (CL-15: sin estado intermedio público). **El mecanismo que cumple esta garantía queda condicionado a ADR-04 (proyecciones) y ADR-06 (entrega)**; este documento no selecciona caché, expiración, invalidación, regeneración ni ningún otro mecanismo, y **no afirma que la garantía ya se cumpla**.

**Evaluación y auditoría en toda unidad.** Toda unidad incluye la **evaluación de CMP-08** (§7.0, pasos 2 a 5) y el **registro de CMP-16** de lo que se aplica (RT-10, RF-AUD-01, RF-AUD-02), aunque la columna de componentes los nombre solo cuando intervienen de forma específica.

**La lista no es exhaustiva:** fija el mínimo derivado de los requisitos y casos límite vigentes. La enumeración definitiva por operación corresponde a la Fase 4.

| ID | Unidad | Componentes | Fuente |
|---|---|---|---|
| UI-01 | Crear torneo y asignar al creador como organizador de ese torneo | CMP-09 + CMP-07 + CMP-08 + CMP-16 | RF-ACC-12, Q-02a, RT-04 |
| UI-02 | Otorgar el rol de delegado y retirar las asignaciones de editor en conflicto | CMP-07 + CMP-08 + CMP-16 | RT-05, CL-35 |
| UI-03 | Reasignar un editor: desactivar la asignación anterior y activar la nueva | CMP-07 + CMP-08 + CMP-16 | RF-ACC-06, RT-13, CL-41 |
| UI-04 | Designar o revocar un dueño con protección del último dueño activo | CMP-07 + CMP-08 + CMP-16 | RF-ACC-09, RT-06, CL-43 |
| UI-05 | Corregir un resultado finalizado y dejar consistentes los resultados y la clasificación derivada de Z2/Z3 (materializada o calculada al servirla, según ADR-04 y ADR-08), y registrar la señal de cambio **si ADR-06 la adopta** | CMP-11 + CMP-12 + CMP-14 + CMP-08 + CMP-16 | RD-RES-06, RD-TAB-11, CL-15 |
| UI-06 | Aplicar una operación recibida: autorizar, registrar idempotencia, aplicar y auditar | CMP-13 + CMP-08 + CMP-11 + CMP-16 | OFF-06, OFF-22, RT-13 |
| UI-07 | Anonimizar un jugador: sus datos en Z2/Z3 y, si existieran, las proyecciones derivadas persistentes, y registrar la señal de cambio si ADR-06 la adopta. **Fuera de la transacción:** la propagación a Z1 y a copias locales en Z0 (HR1-11) y los respaldos (HR1-12) | CMP-17 + CMP-10 + CMP-14 + CMP-08 + CMP-16 | PRV-06 |
| UI-08 | Levantar la restricción histórica de conflicto, con motivo y auditoría | CMP-07 + CMP-08 + CMP-16 | RF-ACC-17, RT-14 |
| UI-09 | Generar o regenerar el calendario con evaluación de RT-14 por efecto | CMP-09 + CMP-08 + CMP-16 | RD-CAL-02, 07, RT-14 |
| UI-10 | **Confirmar un resultado** y dejar consistentes los resultados y la clasificación derivada afectada (materializada o calculada al servirla, según ADR-04 y ADR-08), y registrar la señal de cambio **si ADR-06 la adopta**. Es la unidad de confirmación y recálculo; UI-05 es su equivalente para correcciones. **Relación con UI-06:** cuando el resultado llega por sincronización, UI-10 se ejecuta **dentro de** UI-06 (la aplicación de la operación recibida y su recálculo son **una sola transacción de datos**); cuando se captura en línea, UI-10 es la unidad por sí misma | CMP-11 + CMP-12 + CMP-14 + CMP-08 + CMP-16 | RD-RES-07, RD-TAB-11, CL-15, Q-32 |
| UI-11 | Revocar un rol o desasignar a un editor **serializado** frente a operaciones en línea y sincronizadas: «aplicada antes y válida» o «rechazada», nunca a medias | CMP-07 + CMP-08 + CMP-13 + CMP-16 | RT-13, RF-ACC-07, CL-45, CL-61 |
| UI-12 | Aceptar una invitación de delegado respetando **un solo equipo por torneo** ante invitaciones simultáneas | CMP-07 + CMP-10 + CMP-08 + CMP-16 | RF-ACC-13, CL-54 |
| UI-13 | Dar de alta a un jugador con verificación de duplicados ante altas simultáneas: a lo sumo una alta activa | CMP-10 + CMP-08 + CMP-16 | RD-JUG-02, RD-JUG-08, CL-22, CL-36 |

**Registro de rechazos y bloqueos.** El registro de un rechazo o bloqueo por RT-14 (RF-AUD-06) **no forma parte del resultado transaccional** de la operación rechazada: debe **persistir aunque esa operación no se aplique o se revierta**. Esta exigencia es **derivada [DER]** de que RF-AUD-06 pide auditar los rechazos (`01` no la formula literalmente); su mecanismo es un punto de ADR-09 (R2). **El mismo punto está abierto, sin resolver,** para el registro de idempotencia de una operación rechazada (OFF-06: devolver «el resultado original») y para los metadatos de OFF-20: los deciden ADR-05.C y ADR-09.C.

### 5.6 Clasificación de datos sensibles por zona

Las columnas distinguen dos cosas:

- **Residencia persistente:** zonas donde el dato **se guarda** (incluye copias derivadas, cachés y almacenamiento local).
- **Presentación o tránsito transitorio:** zonas donde el dato **se muestra** a un rol autorizado, en una sesión autorizada y en línea, o **transita** (por ejemplo, la dirección de correo al enviar un mensaje), **sin guardarse** en el almacenamiento local ni en cachés.

«Prohibida» significa que el dato **no puede residir ni mostrarse** en esa zona. Esta tabla **no concede permisos**: quién puede ver cada dato lo fija `01 §7` y PRV-09 y PRV-14.

| Dato | Residencia persistente | Presentación transitoria | Prohibida | Control |
|---|---|---|---|---|
| Nombre interno e indicador de menor | Z2, Z3 (incluidos respaldos) | Z0, en CMP-02, solo para los roles que PRV-09 autoriza (dueño, organizador del torneo, delegado de su equipo) | Z1; Z4; **persistir en Z0** (almacén local y datos preparados, PRV-11) | PRV-09, PRV-11 |
| Etiqueta «Jugador n.º X (equipo)» | Z0, Z1, Z2, Z3 | Z0, Z1 | — | PRV-02 (identificación indirecta: PRV-18, abierta) |
| Nombre **público** con autorización vigente | Z2, Z3. **Copias derivadas** en Z1 y Z0 (vistas públicas y datos preparados), sujetas a ADR-06 y a HR1-11 | Z0, Z1 | Z4 | PRV-02, PRV-05; revocación: §7.4. **Mientras Q-20 esté abierta, PRV-02 es `COND` y no se publica ningún dato personal**: esta fila describe la capacidad condicionada, no una funcionalidad aprobada |
| Fecha de nacimiento, documento de identidad, fotografía, teléfono, dirección | **Ninguna: el diseño no exige recolectarlos.** PRV-04 guarda solo el indicador de menor y **«el esquema no contiene fecha de nacimiento»**; si una decisión posterior los incorporara, serían datos nuevos que exigirían revisar esta tabla | Ninguna | Z0, Z1, Z4; **nunca públicos** (PRV-03) | PRV-03, PRV-04 |
| Metadatos de autorización o consentimiento (PRV-05: tipo, otorgante como relación, fecha, versión, quién la registró, referencia de evidencia, estado) | Z2, Z3. **Sin documentos**: la aplicación no carga ni almacena la evidencia (PRV-05) | Z0, solo para quien la registra o la revoca (matriz fila 20); **no se define aquí ninguna facultad de consulta adicional** | Z1, Z4; **persistir en Z0**; **nunca públicos** (PRV-03: «datos y evidencia de consentimiento») | PRV-03, PRV-05 |
| Correo y rol de usuarios | Z2, Z3 | Z0, para dueño y organizadores de sus torneos (PRV-14); **tránsito** en Z4: la dirección necesaria para enviar y el **contenido mínimo del mensaje** (por ejemplo, rol, ámbito y enlace o token de una invitación o recuperación, RF-ACC-02), sin más guardado que el que defina ADR-15 | Z1 | PRV-14, PRV-13 |
| Credenciales de servicio (claves y secretos de la aplicación) | Solo Z2 (y la configuración del servidor) | — | Z0, Z1, Z4 [DER: RT-09 exige que no lleguen al navegador ni a la cola local; la exclusión de Z1 y Z4 es una consecuencia de PA-01] | RT-09 |
| Credenciales de usuario y segundo factor | Z2, Z3 | Z0, solo durante el acto de autenticación | **Persistir el secreto en Z0**, Z1, Z4 | RF-ACC-10, RF-ACC-16 |
| Sesión local del dispositivo (información mínima para continuar la captura, OFF-23) | Z0 (mínimo), con borrado al cerrar sesión una vez sincronizado o descartado (OFF-12); estado de la sesión en Z2 | Z0 | Z1, Z4 | OFF-12, OFF-23 |
| Operaciones locales y datos preparados | Z0 (mínimos); en el servidor, metadatos de rechazadas o en conflicto (OFF-20) | Z0 | Z1, Z4 | OFF-12, PRV-11, D-9 |
| Auditoría | Z3, sin datos personales de menores | Z0, en CMP-02, para quien RF-ADM-03 autoriza | Z1, Z4; **persistir en Z0** | RF-AUD-04, RF-ADM-03 |
| Datos personales de un jugador en la exportación PRV-08 | Ninguna fuera de Z2/Z3 | Z0, durante la exportación, para quien PRV-08 y la matriz fila 34 autorizan | Z1, Z4; **persistir en Z0** | PRV-08 |
| Respaldos | Z3 (mismo nivel de protección que los datos) | — | Z0, Z1, Z4 | PRV-13, RNF-08 |
| Telemetría y registros técnicos | Z4, sin datos personales | — | — | PRV-12 |

### 5.7 Límites de confianza

Solo se **identifican puntos de atención para R2**; el análisis de amenazas se hará en R2.

| ID | Frontera | Qué cruza | Control exigido por los requisitos | Punto de atención para R2 |
|---|---|---|---|---|
| TB-1 | Dispositivo ↔ aplicación | Operaciones, sesión, solicitudes | Validación en el límite de confianza; autorización por el servidor (RC-04) | Manipulación de identificadores, rol o torneo en la petición (RF-ACC-15); repetición de operaciones |
| TB-2 | Dispositivo ↔ almacén local | Datos preparados, cola | Datos mínimos, sin datos internos de jugadores (OFF-12, PRV-11) | Robo o pérdida del dispositivo; borrado por el navegador |
| TB-3 | Público ↔ borde público | Solicitudes anónimas | Solo proyecciones públicas (PA-04) | Enumeración de torneos, borradores, jornadas no publicadas (CL-25) |
| TB-4 | Aplicación ↔ datos | Consultas y escrituras | Restricciones de integridad; autorización en la capa de datos (RT-02, RT-08) | Escalada de privilegios, fuga entre torneos |
| TB-5 | Aplicación ↔ correo transaccional y tareas programadas | Direcciones de usuarios, mensajes | Sin datos de jugadores ni de menores (§5.6); direcciones de usuarios protegidas (PRV-14, PRV-13) | Fuga de datos hacia terceros; reenvío o suplantación |
| TB-6 | Organización ↔ desarrollador | Acceso administrativo | Credenciales individuales y revocables (RNF-14) | Continuidad y sucesión de accesos |
| TB-7 | Dispositivo ↔ observabilidad | Telemetría de errores del cliente | Sin datos personales (PRV-12, RNF-09) | Datos de jugadores en trazas o capturas de error |
| TB-8 | Aplicación ↔ borde público | Alimentación de proyecciones y señales de cambio (su mecanismo es de ADR-06) | Solo proyecciones con lista blanca (PA-04) | Contaminación de la entrega pública; señal de cambio omitida o tardía tras un cambio de privacidad |
| TB-9 | Datos ↔ almacenamiento de respaldos | Copias completas, incluidas las de menores | Mismo nivel de protección que Z3 (§5.6) | Restauración en un entorno no protegido; retención indefinida |

### 5.8 Dependencias entre componentes (comprobación de ciclos)

Se distinguen **tres tipos de dependencia**; mezclarlos hace parecer circular lo que no lo es.

| Tipo | Significado | Relaciones |
|---|---|---|
| **Control** | Un componente **invoca** a otro | **Clientes (Z0), a través de TB-1 y TB-3:** CMP-02, CMP-03 y CMP-05 → CMP-06 (autenticar); CMP-02 → CMP-07, CMP-09, CMP-10, CMP-11 y CMP-17; CMP-03 → CMP-11 (captura en línea); CMP-05 → CMP-13 (operaciones sincronizadas); CMP-01 → CMP-15 (lectura pública). **Servidor (Z2/Z3):** CMP-13 → CMP-08, CMP-11 y CMP-16 (rechazos al sincronizar); CMP-09 → CMP-07 (UI-01: asignar al creador); CMP-07 → CMP-10 (UI-12: un solo equipo por torneo); CMP-07, CMP-09, CMP-10, CMP-11 y CMP-17 (exportación PRV-08) → CMP-08 (autorizar); CMP-11 → CMP-12 (calcular) y → CMP-14 (señal de cambio); CMP-09 → CMP-12 (vista previa) y → CMP-14 (visibilidad); CMP-07, CMP-09, CMP-10, CMP-11 y CMP-17 → CMP-16 (registrar); CMP-17 → CMP-10 y → CMP-14; CMP-06 y CMP-07 → CMP-18 (enviar); CMP-14 → CMP-15 (señal). **Telemetría:** los componentes de Z2/Z3 emiten métricas y errores hacia CMP-19, sin datos personales (PRV-12); es una relación de solo salida |
| **Lectura de datos** | Un componente **lee** datos que otro posee, sin invocarlo | CMP-08 lee roles, historial y asignaciones de CMP-07, y el estado de torneo, equipo y partido de CMP-09, CMP-10 y CMP-11; CMP-12 lee resultados de CMP-11 y reglamento de CMP-09; CMP-14 lee las fuentes de las proyecciones; CMP-08 lee el nivel de autenticación que emite CMP-06; **CMP-18 lee** los partidos y las asignaciones (CMP-09, CMP-11, CMP-07) para detectar partidos sin editor (N-12) |
| **Retorno de resultado** | Un componente **devuelve** el resultado de lo que se le pidió | CMP-08 → CMP-13 y → quien lo invocó (permitida o rechazada con motivo); CMP-11 → CMP-13 (aplicada o en conflicto de versión); CMP-12 → CMP-11 (tabla calculada) |

**Acoplamientos reconocidos (no son ciclos de control):**

- **AC-1:** CMP-07 invoca a CMP-08 para autorizar un cambio de roles (UI-04, UI-08) y CMP-08 lee los datos de CMP-07. Son una invocación y una lectura; ocurren en la **misma unidad indivisible** (UI-01…UI-13) y CMP-08 **no invoca** a CMP-07.
- **AC-2:** CMP-13 invoca a CMP-08 y CMP-11 y recibe de ellos el **resultado** (necesario para devolver «el resultado original» a un reintento, OFF-06, y para registrar los metadatos de OFF-20). Es un retorno, no una invocación de vuelta.
- **AC-3:** CMP-11 invoca a CMP-12 y recibe la tabla calculada; CMP-12 **no invoca** a ningún componente.

**Grafo de control:** acíclico. Los **sumideros** (componentes que no invocan a otros de la aplicación) son CMP-08, CMP-12, CMP-15, CMP-16, CMP-18 y CMP-19 (este último solo recibe telemetría). Los clientes de Z0 no son sumideros: invocan a Z2 a través de TB-1 y TB-3. Las dependencias circulares posibles están entre **ADR** y se resuelven en §8.2.

### 5.9 Contrato lógico de CMP-12 (para ADR-08)

Se fija aquí la interfaz y las dependencias; la **decisión tecnológica y la ubicación** (Z2 o Z3) corresponden a ADR-08 (R3), condicionadas por **ADR-02.C** (frontera atómica del recálculo, §5.5) y por **ADR-04.C** (garantía de coherencia pública), ambos de R2.

| Aspecto | Contrato |
|---|---|
| Entradas | Resultados confirmados del torneo; reglamento vigente y **su versión** (RF-TOR-03); resoluciones manuales vigentes **[ABIERTO] `COND` Q-22 (RD-TAB-09)**: el contrato admite que no existan; estados de partido que el reglamento admite **[ABIERTO] Q-29**; reglas de W.O. y su efecto en GF y GC **[ABIERTO] Q-21** |
| Salidas | Tabla ordenada con puntos, GF, GC, DG; **«empate sin resolver»** cuando corresponde; **[ABIERTO] condicionado a Q-22:** indicación de resolución manual obsoleta si cambian los resultados implicados (si Q-22 no la adopta, la salida desaparece sin alterar el resto) |
| Propiedades obligatorias | Determinista; termina siempre (sin bucles en empates circulares, RD-TAB-06); no depende del orden de inserción; una sola fuente de verdad (PA-02, `01 RD-TAB-10`) |
| Quién lo invoca | CMP-11 al confirmar, corregir o anular un resultado; CMP-09 para la vista previa «antes/después» de un cambio de reglamento permitido (RD-REG-03, abierta, Q-23) |
| Versión de reglamento | **[ABIERTO] condicionado a Q-23 (`COND` RD-REG-03):** el cálculo usa la versión vigente del reglamento. El contrato admite **dos lecturas**: (a) recálculo sobre el historial con la versión vigente, con W.O. aplicados hacia adelante (recomendación de `01` para Q-23, no aprobada); (b) ningún cambio de reglamento con el torneo iniciado (condición mínima vigente). Q-21 (W.O.) también está abierta |
| Atomicidad | **UI-10** (confirmar un resultado y recalcular) y **UI-05** (corregir y recalcular) son transacciones sobre los datos; la **señal de cambio** se entrega después y fuera de la transacción. La coherencia entre resultados y clasificaciones en lo público es una **garantía exigida** a ADR-04 y ADR-06 (CL-15; §5.5), no una propiedad demostrada aquí |
| Puntos abiertos que aísla | Q-21, Q-22, Q-23, Q-26, Q-27, Q-28, Q-29, Q-30, Q-33 |
| Alternativa admitida por `01 RD-TAB-10` | Dos implementaciones con prueba de equivalencia (se evalúa en ADR-08) |

---

## 6. Aislamiento multi-torneo

### 6.1 Reglas de diseño

1. **Todo dato de torneo pertenece a exactamente un torneo:** inscripciones, plantillas, jugadores, jornadas, partidos, eventos, asignaciones de editor, reglamentos, operaciones recibidas, metadatos de incidencias y auditoría del torneo (RF-TOR-04). Los datos **globales** son solo los de la categoría «Global» de §6.2.
2. **Toda referencia entre entidades de torneo debe resolver al mismo torneo.** No basta con referencias independientes: la capa de datos debe rechazar un partido de un torneo con un equipo de otro (RT-08, CL-21), incluida la asignación de editor (CMP-07).
3. **Permisos acotados por torneo.** Los permisos no se heredan ni se transfieren entre torneos; el dueño es global pero sujeto a RT-14 (RT-01).
4. **Inscripciones independientes:** un mismo equipo puede inscribirse en varios torneos sin compartir plantillas, estadísticas, permisos ni información privada (CONF Q-37 B, CL-23, CL-40).
5. **El vínculo de identidad común está diferido (RF-EQP-06).** El modelo **no debe impedir** añadirlo después, y **no puede ser un criterio de autorización** (RT-12). En el MVP no existe ninguna operación ni interfaz de vinculación.
6. **La cuenta es global; sus roles y asignaciones son por torneo.** Un mismo jugador en dos torneos son **registros separados** sin exposición cruzada (CL-23).
7. **Las proyecciones públicas y su entrega se organizan por torneo:** una actualización o una señal de cambio de un torneo no expone ni afecta otro (RF-TOR-04, RF-PUB-03, RT-08).
8. **Los metadatos de incidencias (OFF-20) y la auditoría de un torneo se consultan solo dentro de ese torneo** por el organizador; **el dueño ve todo** (RF-ADM-03, RF-AUD-02).

### 6.2 Alcance de las entidades

| Entidad | Alcance | Nota |
|---|---|---|
| Cuenta de usuario | **Global** | Correo, 2FA, estado |
| Rol y asignación de rol | Por torneo. **Son globales** el rol de dueño y la **designación de un organizador** (y su exigencia de 2FA, RF-ACC-03, RF-ACC-10) | Historial de solo anexado (RF-ACC-11); fuente única: CMP-07 |
| Invitación | Por torneo o por equipo inscrito; **global** cuando es la designación de un organizador o de un segundo dueño | Su ámbito nunca excede el de quien invita (RT-07) |
| Eventos de gobierno | **Global** | Designación y revocación de dueños, designación de organizadores (RF-AUD-02, RF-ACC-03, RF-ACC-09) |
| Registro de seguridad | **Global**, visible al dueño | RF-AUD-05 (S) |
| Torneo, reglamento versionado, jornada, partido, evento | Torneo | |
| Inscripción, plantilla, jugador | Torneo | Inscripciones independientes entre torneos |
| Asignación de editor | Torneo (por partido) | Un único editor vigente por partido (RC-09) |
| Operación de sincronización recibida | Torneo | Metadatos solo de rechazadas o en conflicto (OFF-20) |
| Autorización de nombre público (metadatos) | Torneo | PRV-05 |
| Auditoría de operaciones del torneo | Torneo | Sin datos personales de menores |
| Registros de rechazos y bloqueos por conflicto de interés (RF-AUD-06) | **Torneo siempre:** RT-14 se define por equipo y torneo (siete clases de acción sobre el propio equipo, `01` RT-14). Los intentos denegados de escalada o de operaciones de gobierno (por ejemplo, un organizador que intenta conceder el rol de dueño, CL-19, CL-38) **no son RT-14**: van al registro de seguridad global (RF-AUD-05) | Sin datos personales de menores |
| Exportación de los datos personales de un jugador (PRV-08) | Torneo | Solo el dueño (matriz fila 34); la facultad del organizador es `COND` Q-42; responsable: CMP-17 (§5.3) |

### 6.3 Puntos abiertos que tocan el aislamiento

| Punto | Estado |
|---|---|
| «Propio equipo» sin vínculo de identidad rige solo dentro del mismo torneo | [ABIERTO] A-6 |
| Visibilidad pública de un torneo `archivado` | [ABIERTO] Q-40 (d) |
| Futuro vínculo de identidad entre torneos | Diferido (RF-EQP-06); CL-42 diferido |

### 6.4 Garantías a verificar en fases posteriores (no se ejecutó ninguna prueba)

Intentos de acceso cruzado (CL-21), organizador sin acceso a plantillas, estadísticas, usuarios o auditoría de otro torneo (CL-40), mismo jugador en dos torneos sin cruce (CL-23), entrega pública sin fuga entre torneos.

---

## 7. Flujos conceptuales

Cada flujo describe actores, pasos, puntos de control y lo que el diseño deja abierto. No describe interfaces (Fase 3) ni contratos (Fase 4).

### 7.0 Flujo transversal: evaluación de una operación sensible

Aplica a toda operación que cambie datos (FL-2, FL-3 y las operaciones de gestión).

**Entradas que la evaluación debe considerar:**

| Dimensión | Entrada |
|---|---|
| Usuario | Cuenta, sesión válida y **nivel de autenticación** emitido por CMP-06 |
| Rol | Todos los roles vigentes de la cuenta |
| Torneo | Torneo y **su estado** (`finalizado`, `archivado`: D-5, matriz filas 27 y 37) |
| Equipo | Equipo o equipos del que el actor es o fue delegado (incluido el **historial** de asignaciones revocadas) |
| Partido | Partido, su estado y la **asignación vigente** del editor |
| Efecto | **Conjunto de partidos y decisiones que la operación modificaría realmente**, con efectos automáticos o en cascada |
| Conflicto | **Levantamientos vigentes** de la restricción histórica y existencia de **otro actor sin conflicto** |

**Pasos (los pasos 2 a 5 y la aplicación del paso 6 forman UNA sola unidad indivisible, RT-13; el registro de un rechazo no forma parte de ella, ver paso 6):**

1. **Identificar al actor**: cuenta, sesión válida y nivel de autenticación.
2. **Reunir** roles vigentes, asignaciones, estado del torneo y del partido, e historial de delegaciones.
3. **Calcular el efecto** de la operación.
4. **Verificar la política efectiva de 2FA:** con los roles ya reunidos, el nivel de autenticación satisface la política más exigente de **todos los roles vigentes** de la cuenta; si tiene rol de dueño u organizador, el 2FA es obligatorio (RF-ACC-10, RC-11).
5. **Decidir** por la unión de permisos de los roles (RT-11), aplicando RT-05 y RT-14 por efecto, con historial y levantamientos vigentes. Si hay conflicto, **rechazar la operación completa**. Si no existe otro actor sin conflicto, **bloquear y auditar** (RT-14, CL-51); sin excepciones automáticas.
6. **Aplicar** la operación y registrar en la auditoría (CMP-16) lo aplicado, dentro de la misma unidad; o **rechazar**. **El registro de un rechazo o bloqueo se escribe de forma independiente de la unidad y debe persistir aunque la operación no se aplique** [DER de RF-AUD-06: `01` exige auditar los rechazos, no formula la persistencia tras una reversión]; cómo se logra es un punto de ADR-09 (R2).

**Consultar y previsualizar** no se restringen por RT-14: no persisten nada y no conceden otra autorización (RT-14, CL-58).

**Operaciones sincronizadas:** el mismo flujo se aplica al sincronizar, con el estado de permisos **vigente al sincronizar** (OFF-22). Los requisitos exigen para sincronizar sesión válida y permisos vigentes (OFF-22, OFF-23); **qué nivel de autenticación se exige al sincronizar no está fijado por los requisitos** y queda como punto de ADR-03 (R2), sin presumirlo aquí.

**Operaciones automáticas:** heredan la situación del actor que las dispara (RT-14); la única autorizada previamente es la asignación inicial del creador de un torneo (§3.1).

**Puntos abiertos:** alcance de «resultados» (A-1), de «asignaciones» (A-2), de «inscripción o retiro» (A-4), duplicados (A-5), «propio equipo» (A-6), levantamiento (A-12), interacción con revocaciones de seguridad (A-15), reglamento y desempates (A-16), facultades del organizador (Q-42). **El diseño debe poder alojar cualquiera de las lecturas posibles sin reescribir el evaluador**, y mientras estén abiertas se aplica PA-13.

### 7.1 FL-1 · Lectura pública

| Elemento | Contenido |
|---|---|
| Actores | Visitante; usuario autenticado sin rol |
| Pasos | (1) El visitante abre el enlace o escanea el QR → (2) CMP-15 entrega la vista pública, que obtiene de CMP-14 **por el mecanismo que fije ADR-06** (con o sin copia previa; este documento no lo supone) → (3) CMP-14 produce la proyección **solo a partir de datos confirmados de jornadas visibles** → (4) la vista muestra «Actualizado a las hh:mm» → (5) sin conexión, muestra la última versión guardada con su hora |
| Controles | Solo torneo `publicado` / `en curso` / `finalizado`, jornada publicada y operación confirmada (`01 §9.6`); lista blanca de campos; etiqueta de menor calculada en servidor; ninguna respuesta incluye campos internos ni identidad de editores |
| Requisitos | RF-PUB-01…09, RF-PWA-02, OFF-15, OFF-18, PRV-02, 03, 09 |
| Abiertos | Q-40 (d) visibilidad de `archivado`; Q-20 campos públicos y corte de edad; PRV-18 identificación indirecta; Q-29 efecto de estados en la tabla; Q-13 compartir |
| Dejado a R3 | Mecanismo de actualización y expiración (ADR-06, ADR-07) |

### 7.2 FL-2 · Administración

| Elemento | Contenido |
|---|---|
| Actores | Dueño, organizador (y, según el caso, editor y delegado) |
| Pasos típicos | (1) Autenticarse con 2FA si corresponde → (2) elegir torneo y acción → (3) vista previa si aplica (calendario) → (4) confirmar → (5) evaluación según §7.0 → (6) aplicar con auditoría → (7) registrar la señal de cambio de las proyecciones afectadas, **si ADR-06 la adopta** (su efecto depende de ADR-06) |
| Operaciones representativas | Crear torneo (asignación automática del creador, UI-01), invitar o revocar, asignar o reasignar editor (atómico, UI-03), corregir un resultado finalizado con motivo (UI-05), generar o regenerar calendario (UI-09), resolver duplicados, designar un segundo dueño (autenticación reforzada, UI-04) o revocarlo (autenticación reforzada **y motivo obligatorio**) |
| Controles | RT-01…14, nuevas autenticaciones sensibles, motivo obligatorio donde corresponde, auditoría de solo anexado |
| Requisitos | RF-ACC-01…17, RF-ADM-01…05, RF-AUD-01…06, RD-RES-06, RD-CAL-07 |
| Abiertos | Q-40 (cierre y archivado), Q-42 (facultades del organizador), A-1, A-2, A-4, A-5, A-6, A-12, A-15, A-16, Q-23 (cambios de reglamento con torneo iniciado) |
| Dejado a R2 | Requisitos e invariantes de autorización y autenticación (ADR-02, ADR-03); la implementación dependiente del stack queda condicionada a ADR-01 |

### 7.3 FL-3 · Captura sin conexión y sincronización

| Elemento | Contenido |
|---|---|
| Actores | Editor asignado; organizador sobre partidos de sus torneos; dueño (global); ninguno sobre los partidos de su propio equipo si es delegado (RT-14) |
| Preparación | Con conexión: CMP-14 entrega la **proyección de preparación** (partidos asignados de los próximos 7 días y en juego, plantillas con etiqueta o nombre público y dorsal, resumen del reglamento); se indica «Listo para trabajar sin conexión» y se avisa si pasan 24 h sin actualizar (OFF-02, PRV-11) |
| Captura | Registrar marcador, eventos y cambios de estado (incluidos suspender, reanudar y finalizar) en el almacén local (CMP-04): cada operación con identificador único, secuencia por partido, versión base, dispositivo, usuario y hora local (OFF-03) |
| Sincronización | **Con la app abierta; no se depende de segundo plano** (OFF-05). CMP-13 aplica **idempotencia**, **orden por secuencia contigua** y **versión** (`01 §9.4`); CMP-08 revalida con el flujo §7.0 (RT-13, RT-14 con historial); CMP-11 aplica; el servidor devuelve el estado |
| Estados de la operación | `pendiente` → `enviando` → `confirmada` / `en conflicto` / `rechazada` (`01 §9.2`); solo la **confirmada** puede ser pública (N0 → N1 → N2, §0.3) |
| Casos críticos | Reanudación rechazada: ningún evento posterior se aplica ni se publica (OFF-25); reasignación del editor: sus capturas se rechazan (`01 §9.4` filas 4 y 11); sesión vencida: la captura continúa, sincronizar exige sesión válida (OFF-23); cuenta desactivada: eliminación controlada de datos locales y exportación limitada (OFF-11, OFF-24); finalización pendiente hasta confirmación (OFF-21) |
| Requisitos | OFF-01…25, RD-RES-01, 04, 13, RD-EVT-12, RF-ACC-06, 07 |
| Abiertos | Q-29, Q-30, Q-42 (facultades de captura del organizador), Q-43 (contenido exportable), A-9, A-10, A-11, A-13, A-14, A-15 |
| Límites conocidos | iOS sin sincronización en segundo plano y con almacenamiento no garantizado (OFF-05, OFF-17, RK-11); riesgo residual aceptado RK-15; pérdida del dispositivo (OFF-12). **Toda afirmación sobre iOS es «no verificada en dispositivo»** (§2.5) |
| Dejado a R2 | Requisitos e invariantes del protocolo de sincronización y del almacén local seguro (ADR-05, ADR-16); implementación condicionada a ADR-01 |

### 7.4 FL-4 · Publicación y propagación

| Elemento | Contenido |
|---|---|
| Actores | Dueño y organizador; sistema |
| Pasos | (1) **Habilitar la visibilidad** del torneo y de sus jornadas → (2) el servidor confirma una operación de resultado (N1) → (3) dentro de la misma transacción de datos, los resultados y la clasificación derivada quedan consistentes y, **si ADR-06 adopta una señal de cambio**, esta se registra (UI-10; UI-05 para correcciones) → (4) CMP-15 mantiene lo afectado actualizado **por el mecanismo que fije ADR-06** → (5) la vista pública refleja el dato confirmado en ≤ 60 s [REC] (RF-PUB-09); la **coherencia entre resultados y clasificaciones** es una **garantía exigida**: ADR-04.C la fija y ADR-06 decide el mecanismo (§5.5); no se afirma que ya se cumpla |
| Reglas | Propagación automática al confirmar (CONF Q-32); la corrección de una jornada anterior recalcula de forma atómica sin estado intermedio público (CL-15); despublicar retira la visibilidad; ninguna operación local es pública (OFF-15) |
| **Cambios de visibilidad y de privacidad** | Despublicar, ocultar un nombre, **revocar una autorización de nombre público** o **anonimizar** a un jugador (PRV-02, PRV-06, CL-26) deben registrar también una **señal de cambio** hacia las proyecciones públicas y de preparación (UI-07 para la anonimización). **No existe una garantía definida de plazo** para estos cambios (RF-PUB-09 cubre resultados confirmados); el plazo es un punto de ADR-06 |
| Riesgo conocido | Las copias de vistas públicas y de datos preparados guardadas en el dispositivo (RF-PUB-06, RF-PWA-02, OFF-02) **no pueden purgarse a distancia**: HR1-11 |
| Requisitos | RD-RES-07, RF-PUB-04, RF-PUB-09, RD-CAL-08, `01 §9.6`, CL-15, CL-26, CL-30 |
| Abiertos | A-8 (visibilidad y RT-14), Q-40 (d), Q-29 |
| Dejado a R3 | Estrategia de actualización, expiración, caché y tiempo real (ADR-06, ADR-07) |

---

## 8. Inventario de ADR previstos

Todos en estado **PREVISTO** (sin alternativas evaluadas ni decisión). Reglas fijadas por el usuario: **≥ 3 alternativas viables** en el ADR principal; **≥ 2** en los demás, salvo excepción justificada; **ningún proveedor ni stack seleccionado**. La asignación a R2, R3 o R4 es una **propuesta de este análisis** y la confirma el usuario; se mantiene **R2 antes de R3** (RC-25).

### 8.1 Inventario

| ADR | Tema | Pregunta de decisión | Alternativas mínimas | Dependencias (preguntas, ambigüedades y requisitos abiertos; Q-07, Q-16 y Q-25 son decisiones **confirmadas** que condicionan, no abiertas) | Bloque |
|---|---|---|---|---|---|
| **ADR-01** | **Arquitectura principal: plataforma, hosting y datos** | ¿Qué combinación de entrega web, aplicación y almacén relacional soporta mejor los criterios de §2.4 **frente a los requisitos e invariantes fijados en R2**? | **≥ 3.** Candidatos **no vinculantes**: (a) Vercel + Supabase; (b) Vercel + Neon; (c) **C1**: hosting alternativo (por ejemplo Cloudflare o Netlify) con PostgreSQL gestionado. Ninguno aprobado | Q-07 (responsable de cuentas, presupuesto), PRV-13, RNF-12 | R3 |
| ADR-02 | **Reparto de la autorización entre servidor y capa de datos; frontera atómica** | ¿**Qué verifica además la aplicación y qué impone siempre la capa de datos** (RT-05, RT-06, RT-08, RT-13, RT-14)? **Toda autorización se comprueba en ambas capas**; **ningún permiso se verifica en una sola** y esas opciones quedan fuera del espacio de decisión por RT-02. **ADR-02.C fija también la frontera atómica:** qué operaciones forman una unidad indivisible y dónde termina su transacción (UI-01…13; en particular UI-05 y UI-10 para el recálculo), dejando fuera la entrega a Z1 y Z0 | ≥ 2 formas de reparto con **ambas capas siempre presentes** | A-1, A-2, A-4, A-5, A-6, A-8, A-12, A-15, A-16, Q-40 (a)–(c), Q-42 | R2 |
| ADR-03 | Identidad, sesión y segundo factor | ¿Cómo se autentica, se exige 2FA por rol y se emite el nivel de autenticación que usa CMP-08? ¿Qué nivel se exige al sincronizar? ¿Cómo se alcanza el objetivo de `01 RF-ACC-07` de reflejar una revocación en las sesiones conectadas en ≤ 60 s, «sujeto a validación en la Fase 2» (D-2)? | ≥ 2 | RF-ACC-10, D-2, HR1-04 | R2 |
| ADR-04 | Modelo de visibilidad y proyecciones | ¿Cómo se representa la visibilidad (`01 §9.6`) y se calculan las etiquetas de menores y la proyección de preparación? **ADR-04.C fija la garantía exigida de coherencia pública:** las vistas públicas de un torneo mantienen coherencia entre resultados y clasificaciones (CL-15, sin estado intermedio público); si la clasificación se materializa o se calcula al servirla es parte de su decisión, y el mecanismo que la cumple lo decide ADR-06 | ≥ 2 | Q-20, Q-40 (d), A-8, PRV-18 | R2 |
| ADR-05 | Protocolo de sincronización sin conexión | ¿Cómo se estructuran la cola, la secuencia, la idempotencia, la revalidación y los conflictos? ¿Cómo se aplica a la ruta sincronizada la frontera atómica de ADR-02.C (UI-06 con UI-10 dentro)? **Pregunta abierta [OFF-06, OFF-20]:** ¿persisten fuera de la unidad el registro de idempotencia de una operación rechazada y los metadatos de rechazadas o en conflicto, como el registro de RF-AUD-06? | ≥ 2 | Q-29, Q-30, Q-42, Q-43, A-9, A-10, A-11, A-13, A-14, A-15 | R2 |
| ADR-06 | Actualización, expiración y caché de lo público | ¿Cómo se mantienen las proyecciones públicas actualizadas (por señal de cambio, por expiración o ambas) y en qué plazo ante cambios de visibilidad y privacidad? **¿Con qué mecanismo se cumple la garantía de coherencia entre resultados y clasificaciones que fija ADR-04.C?** Este documento no supone ninguno | ≥ 2 | Q-40 (d), Q-29, HR1-11 | R3 |
| ADR-07 | Actualización de las vistas abiertas y tiempo real | ¿Cómo se cumple RF-PUB-09 (≤ 60 s «sin recargar manualmente más de una vez»)? Tres niveles: expiración con refresco manual; actualización periódica automática; empuje en tiempo real (HR1-01) | ≥ 2 (se comparan los tres niveles) | Q-16 (mejor esfuerzo), RNF-04, RNF-15 | R3 |
| ADR-08 | Motor de reglas y fuente única de clasificación | ¿Dónde vive el cálculo de la tabla y cómo se configuran los desempates? **El contrato lógico está en §5.9**; **consume** la frontera atómica de ADR-02.C y la garantía de coherencia de ADR-04.C, y no las redefine | ≥ 2 (incluye dos implementaciones con prueba de equivalencia, `01 RD-TAB-10`) | Q-21, Q-22, Q-23, Q-26, Q-27, Q-28, Q-29, Q-30, Q-33, A-16 | R3 |
| ADR-09 | Auditoría y historial de solo anexado | ¿Cómo se garantiza la inmutabilidad de la auditoría y del historial de asignaciones de roles, y cómo **persiste el registro de un rechazo o bloqueo (RF-AUD-06) aunque la operación no se aplique** [DER]? (Se pregunta lo mismo, **sin resolverlo**, para el registro de idempotencia de OFF-06 y los metadatos de OFF-20, que decide con ADR-05.C) (La **política** de retención y anonimización de ese historial es ADR-17) | ≥ 2 | PEND-JUR (RF-ACC-11) | R2 |
| ADR-10 | Región, ubicación de datos y transferencia internacional | ¿Dónde se alojan los datos y bajo qué evaluación jurídica? | ≥ 2 | PRV-13, PRV-15, Q-25 | R3 |
| ADR-11 | Entornos y despliegue | ¿Qué entornos y qué flujo de despliegue se justifican por costo y riesgo? | ≥ 2 | Q-07 | R4 |
| ADR-12 | Respaldo, restauración y continuidad | ¿Qué estrategia cumple RNF-08 y RNF-17 con RPO y RTO propuestos? | ≥ 2 | RF-ACC-09 (recuperación) | R4 |
| ADR-13 | Observabilidad sin datos personales | ¿Qué se monitorea y con qué herramienta, sin datos personales? | ≥ 2 | PRV-12 | R4 |
| ADR-14 | Tareas programadas, alertas y su identidad de ejecución | ¿Cómo se ejecutan la alerta N-12, los vencimientos y la retención? ¿Bajo qué identidad se ejecutan las tareas **sin actor disparador** (pregunta abierta; RT-14 solo rige efectos de una operación de un actor)? | ≥ 2 | Q-10 | R4 |
| ADR-15 | Correo transaccional | ¿Cómo se envían invitaciones y recuperaciones con costo y privacidad adecuados? | ≥ 2 | PRV-13, PRV-14 | R3 |
| ADR-16 | Almacenamiento local seguro en el dispositivo | ¿Qué protección local se justifica (cifrado local, límites) frente a pérdida o robo? `01 OFF-12` dice que se evalúa en la Fase 2 | ≥ 2 | RK-11, §2.5 | R2 |
| ADR-17 | Política de retención y anonimización | ¿Qué se conserva y por cuánto tiempo (incluidos los respaldos, que `01` no fija), y cómo se anonimiza sin romper el historial necesario para RT-14? (La **identidad bajo la que se ejecuta la retención sin actor disparador** es pregunta abierta de **ADR-14**, que es su único dueño; ADR-17 define la política, no el ejecutor) | ≥ 2 | PEND-JUR (PRV-10, RF-ACC-11) | R4 |

### 8.2 Matriz de dependencias entre ADR

**Cómo se lee.** La tabla lista, **por artefacto, únicamente lo que requiere** (relación **R**: la decisión necesita el resultado previo del otro). **No existe columna «alimenta»**: lo que un artefacto aporta a otro es la relación inversa y se lee en la fila del otro, de modo que **no pueden aparecer pares recíprocos**.

**Para romper el ciclo entre ADR-01 y los ADR de R2 (RC-25), cada ADR de R2 se divide en dos artefactos:**

- **ADR-xx.C — capacidades exigidas (R2):** requisitos, restricciones e invariantes **independientes del proveedor**. Pueden aprobarse en R2.
- **ADR-xx.I — implementación condicionada (tras ADR-01):** la parte que depende del stack. Solo se confirma después de aprobar ADR-01 en R3 y requiere su propio ADR-xx.C.

**ADR-01 y ADR-10 forman una decisión conjunta en R3** (la elegibilidad jurídica de la región, PRV-13, es un criterio de la plataforma); ambos requieren los mismos insumos.

| Artefacto | Bloque | Requiere |
|---|---|---|
| ADR-03.C Identidad y 2FA | R2 | — |
| ADR-02.C Autorización | R2 | ADR-03.C (nivel de autenticación); **ADR-09.C** (persistencia del registro de rechazos, que condiciona la frontera atómica); A-1…A-16 como parámetros abiertos |
| ADR-04.C Visibilidad y proyecciones | R2 | ADR-02.C (alcance de A-8 y frontera atómica) |
| ADR-05.C Sincronización sin conexión | R2 | ADR-02.C (frontera atómica), ADR-03.C, ADR-09.C (persistencia de rechazos e incidencias) |
| ADR-09.C Auditoría e historial de solo anexado | R2 | — |
| ADR-16.C Almacenamiento local seguro | R2 | ADR-05.C (formato de cola y datos locales) |
| **ADR-01 / ADR-10** (decisión conjunta) | R3 | ADR-02.C, ADR-03.C, ADR-04.C, ADR-05.C, ADR-09.C, ADR-16.C; PEND-JUR (PRV-13) |
| ADR-02.I, 03.I, 04.I, 05.I, 09.I, 16.I | R3, tras ADR-01 | Su ADR-xx.C respectivo y ADR-01 |
| ADR-06 Actualización y caché de lo público | R3 | ADR-04.C (garantía de coherencia que debe cumplir), ADR-01 |
| ADR-07 Actualización de vistas y tiempo real | R3 | ADR-06 |
| ADR-08 Motor de reglas | R3 | ADR-02.C (**dueño** de la frontera atómica del recálculo, UI-05 y UI-10), ADR-04.C (garantía de coherencia), ADR-05.C (aplicación a la ruta sincronizada), ADR-01 |
| ADR-15 Correo transaccional | R3 | ADR-03.C, ADR-01 / ADR-10 |
| ADR-11 Entornos y despliegue | R4 | ADR-01 |
| ADR-13 Observabilidad | R4 | ADR-01, ADR-11 |
| ADR-17 Política de retención y anonimización | R4 | ADR-09.C; PEND-JUR |
| ADR-14 Tareas programadas | R4 | ADR-01, ADR-02.C, ADR-17 |
| ADR-12 Respaldo y continuidad | R4 | ADR-01 / ADR-10, ADR-11, ADR-17 |

**Comprobación de ausencia de ciclos:** el orden de resolución es ADR-03.C y ADR-09.C → ADR-02.C → ADR-04.C y ADR-05.C → ADR-16.C; después ADR-01/ADR-10; después las implementaciones y los ADR de R3 (06, 07, 08, 15); y por último los ADR de R4 (11, 13, 17, 14, 12). **Cada relación va de un artefacto anterior en ese orden a uno posterior; ninguna lo hace en sentido contrario.** ADR-08 **no** alimenta a ADR-04 ni a ADR-05: los ADR de R2 fijan la frontera atómica como invariante y ADR-08 la consume.

**Delimitación ADR-09 y ADR-17:** ADR-09 trata la **inmutabilidad y la integridad** del historial; ADR-17 trata la **política** (qué se conserva, durante cuánto y cómo se anonimiza) y su ejecución. Ninguno decide lo del otro.

---

## 9. Trazabilidad inicial de lo abierto

**Esta tabla no resuelve nada.** «Tratamiento de diseño» indica cómo debe comportarse la arquitectura mientras la decisión esté abierta: **aislar** = punto de extensión que admite cualquier respuesta; **condiciona** = la respuesta afecta una decisión de arquitectura y debe conocerse antes de cerrarla; **sin efecto** = no afecta la arquitectura. Las columnas «ADR» de §9.1 y §9.3 coinciden con la columna «Dependencias» de §8.1 en lo que se refiere a preguntas y ambigüedades abiertas (trazabilidad bidireccional); las decisiones confirmadas que esa columna también cita (Q-07, Q-16, Q-25) no figuran en esta tabla porque no están abiertas.

### 9.1 Las 18 preguntas abiertas

| Q | Tema | Componentes | Tratamiento de diseño | ADR |
|---|---|---|---|---|
| Q-10 | Canal y destinatarios de la alerta N-12 | CMP-18 | Aislar: el canal es intercambiable | ADR-14 |
| Q-13 | Qué se comparte y por qué canal | CMP-01 | Sin efecto estructural (compartir por enlace, QR y botón nativo) | — |
| Q-14 | Logo, mockup y colores | — | Sin efecto en la arquitectura; bloquea la Fase 3 | — |
| Q-20 | Campos públicos de jugador y edad de corte | CMP-14, CMP-17 | Aislar: lista blanca configurable; no se publica ningún dato personal | ADR-04 |
| Q-21 | W.O. doble, sanciones y GF/GC | CMP-11, CMP-12 | Aislar en las reglas | ADR-08 |
| Q-22 | Resolución manual de desempates | CMP-12, CMP-11 | Aislar | ADR-08 |
| Q-23 | Cambios de reglamento con torneo iniciado | CMP-09, CMP-12 | Aislar: el reglamento es versionado | ADR-08 |
| Q-26 | Retiro o expulsión de un equipo | CMP-10, CMP-11, CMP-12 | Aislar: política configurable | ADR-08 |
| Q-27 | Enfrentamiento directo entre más de dos equipos | CMP-12 | Aislar | ADR-08 |
| Q-28 | Catálogo de desempates y fair play | CMP-12 | Aislar: catálogo como dato | ADR-08 |
| Q-29 | Estados `aplazado`, `cancelado`, `suspendido` y su efecto en la tabla | CMP-11, CMP-12, CMP-13, CMP-14 | **Condiciona** la máquina de estados y la sincronización; aislar el cierre desde `suspendido` | ADR-05, ADR-06, ADR-08 |
| Q-30 | Consistencia entre marcador y eventos | CMP-11, CMP-12 | Aislar | ADR-05, ADR-08 |
| Q-31 | Cierre y límites de plantillas | CMP-10 | Aislar | — |
| Q-33 | Puntos 3/1/0 fijos o configurables | CMP-09, CMP-12 | Aislar | ADR-08 |
| Q-40 | Estados `finalizado` y `archivado`; quién archiva; visibilidad pública de archivados | CMP-07, CMP-08, CMP-09, CMP-14, CMP-15 | **Condiciona** publicación y entrega; la parte (d) antes de la Fase 3; las partes (a)–(c) afectan la matriz de permisos | ADR-02, ADR-04, ADR-06 |
| Q-41 | Traspasos de jugadores | CMP-10 | Aislar | — |
| Q-42 | Facultades del organizador no cubiertas | CMP-08, CMP-11, CMP-17 | **Condiciona** el evaluador y la matriz de la Fase 4 | ADR-02, ADR-05 |
| Q-43 | Eventos en el resumen exportable | CMP-05, CMP-17 | Aislar: por defecto sin eventos | ADR-05 |

### 9.2 Los 23 requisitos `COND`

| Requisito | Pregunta | Componentes | Tratamiento de diseño |
|---|---|---|---|
| RF-TOR-02 | Q-40 | CMP-09, CMP-14 | Condiciona: transición a `archivado` sin construir hasta decidir |
| RF-EQP-03 | Q-26 | CMP-10, CMP-11, CMP-12 | Aislar |
| RF-PUB-01 | Q-40 | CMP-14, CMP-15 | Condiciona la visibilidad de `archivado` |
| RF-ADM-04 | Q-10 | CMP-18 | Aislar (el indicador en el panel es la condición mínima) |
| RD-REG-03 | Q-23 | CMP-09, CMP-12 | Aislar: sin cambios de reglamento con torneo iniciado |
| RD-REG-06 | Q-33 | CMP-09, CMP-12 | Aislar |
| RD-TAB-03 | Q-29 | CMP-12 | Aislar la parte de `suspendido` |
| RD-TAB-05 | Q-28 | CMP-12 | Aislar el catálogo |
| RD-TAB-06 | Q-27 | CMP-12 | Aislar |
| RD-TAB-09 | Q-22 | CMP-12 | Aislar: los empates sin resolver se muestran como tales |
| RD-WO-02 | Q-21 | CMP-11, CMP-12 | Aislar |
| RD-WO-03 | Q-21 | CMP-11, CMP-12 | Aislar |
| RD-WO-04 | Q-21 | CMP-11, CMP-12 | Aislar |
| RD-JUG-01 | Q-20 | CMP-10, CMP-14 | Aislar la lista blanca |
| RD-JUG-05 | Q-31 | CMP-10 | Aislar |
| RD-JUG-10 | Q-41 | CMP-10 | Aislar: sin traspasos |
| RD-RES-10 | Q-42 | CMP-08, CMP-11 | Condiciona la facultad del organizador de suspender |
| RD-RES-12 | Q-29 | CMP-11 | Aislar: cierre desde `suspendido` no habilitado |
| RD-EVT-02 | Q-30 | CMP-11, CMP-12 | Aislar |
| RD-EVT-04 | Q-30 | CMP-11, CMP-12 | Aislar |
| OFF-24 | Q-43 | CMP-05, CMP-17 | Aislar: exportación sin eventos |
| PRV-02 | Q-20 | CMP-14, CMP-17 | Aislar: ningún dato personal publicado mientras esté abierto |
| PRV-08 | Q-42 | CMP-17, CMP-08 | Condiciona las facultades del organizador sobre datos de jugadores |

### 9.3 Las 14 ambigüedades abiertas

Para cada una, el diseño debe **soportar ambas lecturas** y no elegir; la lectura provisional de `01 §13.7` es solo una interpretación de trabajo y se aplica PA-13 mientras esté abierta. La denegación por defecto que figura allí **no es política definitiva**.

| # | Tema | Componentes | Qué debe poder soportar el diseño | ADR | Bloque |
|---|---|---|---|---|---|
| A-1 | Alcance de «resultados» en RT-14 | CMP-08, CMP-11 | Clases de operación como parámetro del evaluador | ADR-02 | R2 |
| A-2 | «Asignaciones que generen conflictos» | CMP-07, CMP-08 | Alcance de la asignación como parámetro | ADR-02 | R2 |
| A-4 | «Inscripción o retiro» frente a edición de nombre y colores | CMP-08, CMP-10 | Distinguir decisión y edición | ADR-02 | R2 |
| A-5 | Duplicados: ¿a qué lado del par se aplica? | CMP-08, CMP-10 | Aplicar a uno o a ambos registros | ADR-02 | R2 |
| A-6 | «Propio equipo» sin vínculo de identidad | CMP-08, CMP-10 | Alcance dentro del torneo; extensible si se construye el vínculo | ADR-02 | R2 |
| A-8 | Publicación de jornadas y RT-14 | CMP-09, CMP-08 | Punto opcional de guardia al habilitar la visibilidad | ADR-02, ADR-04 | R2 |
| A-9 | Reanudar y finalizar emula el cierre desde `suspendido` | CMP-11 | Dos operaciones separadas o una restricción | ADR-05 | R2 |
| A-10 | Reanudación sin conexión por organizador o dueño | CMP-03, CMP-13 | Reanudación offline limitada al editor, o abierta | ADR-05 | R2 |
| A-11 | Marcador directo y anulaciones durante la suspensión | CMP-11, CMP-13 | Regla estricta o laxa | ADR-05 | R2 |
| A-12 | Aprobador del levantamiento y reotorgamiento del rol | CMP-07, CMP-08 | Condiciones del aprobador como parámetro | ADR-02 | R2 |
| A-13 | Preparación de partidos `suspendido` fuera de la ventana de 7 días | CMP-04 | Preparación manual de partidos adicionales | ADR-05 | R2 |
| A-14 | Eventos en `programado` con captura directa del final | CMP-11, CMP-13 | Regular o no regular la secuencia | ADR-05 | R2 |
| A-15 | Interacción de RT-14 con revocaciones de seguridad, desasignación y efectos indirectos | CMP-07, CMP-08, CMP-16 | Que la revocación inmediata se pueda ejecutar siempre, o quedar sujeta a RT-14; efectos indirectos auditados | ADR-02, ADR-05 | R2 |
| A-16 | Reglamento y desempates bajo RT-14 | CMP-08, CMP-09, CMP-12 | Guardia opcional en la edición del reglamento y de los criterios | ADR-02, ADR-08 | R2 |

### 9.4 Restricciones transversales (RT) y componentes

| RT | Componente principal | RT | Componente principal |
|---|---|---|---|
| RT-01 | CMP-08 | RT-08 | Capa de datos (CMP-07, 09, 10, 11) |
| RT-02 | CMP-08 y capa de datos | RT-09 | CMP-06, CMP-19 |
| RT-03 | CMP-07 | RT-10 | CMP-07, CMP-16 |
| RT-04 | CMP-07, CMP-08 | RT-11 | CMP-08 |
| RT-05 | CMP-08 | RT-12 | CMP-08, CMP-10 |
| RT-06 | CMP-07, CMP-08 | RT-13 | CMP-08, CMP-13 |
| RT-07 | CMP-07 | RT-14 | CMP-08, CMP-16 |

---

## 10. Criterios de revisión de R1

Verificables sobre este documento. La aprobación de R1 es un acto expreso del usuario y **no aprueba la Fase 2**.

| ID | Criterio | Cómo se verifica |
|---|---|---|
| CR1-01 | Los 17 RNF aparecen con exigencia de diseño, elementos, estado y fase de verificación | Contar las filas de §2.1: 17 |
| CR1-02 | Cada una de las 25 restricciones (RC-01…25) tiene una fuente; las que citan `01` coinciden con el texto aprobado; las únicas desviaciones respecto de `01` son **RC-24** (excepción de OFF-17, §2.5) y **RC-25** (orden de bloques), ambas decisiones del usuario | **Lista de comprobación:** (0) la **verificación en dispositivo de OFF-17 en 5E y Fase 7 se cita siempre junto a «5G por `01 RK-11`»**; (1) 25 filas con columna «Fuente» no vacía; (2) cotejo de **sentido** (no de redacción literal) de RC-04 con `01 RT-02`, RC-05 con `01 OFF-05`, RC-11 con `01 RF-ACC-10`, RC-21 con `01 RF-AUD-03` y RF-ACC-11, RC-13 con `01 PRV-02/03/09`; (3) toda frase propia que añada algo al requisito va marcada [DER] o [REC] (por ejemplo, RC-04); resultado esperado: 0 discrepancias de sentido salvo RC-24 y RC-25 |
| CR1-03 | Contexto y actores completos, sin proveedores seleccionados | §3 solo usa categorías de servicio |
| CR1-04 | Arquitectura lógica independiente del stack | Búsqueda de nombres de tecnología (proveedores, frameworks, lenguajes, motores y protocolos concretos): **solo en la fila ADR-01 de §8.1 y en la fila HR1-13 de §11** (cita del motor que nombran `01 RNF-11` y `01 RT-13`, §0.4); resultado esperado: 0 apariciones en el resto, incluidos §0.4 (que declara la excepción sin nombrar el motor) y §15. **No cuentan como tecnología de stack** los dispositivos, sistemas operativos y navegadores que cita `01 RNF-07` (Android, iPhone, iOS, Chrome, Safari, Edge, Firefox) ni el término genérico «SQL» al declarar lo que el documento no contiene |
| CR1-05 | **Cada familia de requisitos funcionales y no funcionales tiene al menos un componente responsable**, y cada restricción transversal (RT) tiene el suyo; la asignación por requisito individual es de la Fase 4 | §5.4 cubre las familias RF, RD, OFF, PRV y RNF (esta última remite a §2.1) y §9.4 asigna los 14 RT; sin familias ni RT huérfanos |
| CR1-06 | Límites de confianza identificados sin adelantar el análisis de R2 | §5.7 tiene 9 filas con exactamente las columnas Frontera, Qué cruza, Control exigido por los requisitos y Punto de atención; **ninguna columna** de amenaza clasificada, probabilidad, impacto ni mitigación (eso es R2) |
| CR1-07 | El aislamiento multi-torneo respeta RT-08, RT-12, RF-EQP-06 diferido y Q-37, e incluye las entidades globales | (1) §6.1 tiene 8 reglas y cada una cita al menos un requisito; (2) todas las entidades de §6.2 están clasificadas como «Torneo» o «Global»; (3) las categorías globales de §6.2 son solo: cuenta de usuario, rol de dueño y designación de organizador (con su 2FA), invitaciones globales (organizador, segundo dueño), eventos de gobierno y registro de seguridad; los registros de RF-AUD-06 son siempre por torneo; (4) ninguna regla permite referencias entre torneos ni usa el vínculo de identidad como criterio de autorización |
| CR1-08 | Los cuatro flujos solicitados están modelados con controles, requisitos y puntos abiertos | §7.1 a §7.4 |
| CR1-09 | El inventario de ADR cumple la regla de alternativas mínimas y no resuelve nada | §8.1: ADR-01 con ≥ 3 candidatos y estado PREVISTO en todos |
| CR1-10 | Trazabilidad completa de 18 preguntas, 23 `COND` y 14 ambigüedades, con identificadores iguales a `01 §13` | §9.1 tiene 18 filas, §9.2 tiene 23, §9.3 tiene 14 |
| CR1-11 | Ninguna decisión aprobada se cambia y ningún asunto abierto se resuelve | Los identificadores de §9.1, §9.2 y §9.3 coinciden **uno a uno** con `01 §13.2`, `13.3`, `13.8` y `13.7`; ninguna fila de §9 declara una pregunta, un requisito o una ambigüedad como resuelto ni elige una alternativa; las filas [ABIERTO] de §5.9 admiten todas las lecturas |
| CR1-12 | **Durante las rondas de correcciones** solo se crea o modifica `02-arquitectura-adr.md`. El **registro de R1 (AUD-20)** se hizo el 2026-11-16, **por autorización documental independiente**, en `aprobaciones.md` y `bitacora.md`, y en ningún otro archivo; `02-arquitectura-adr.md` solo se modificó en las rondas y ediciones autorizadas (v0.2 a v0.5 y la sincronización editorial) | `git status` y `git diff --stat`: solo los archivos autorizados en cada acto |
| CR1-13 | **Ningún componente ni flujo concede un permiso nuevo**: toda decisión de permiso es de CMP-08 y los puntos abiertos se rigen por PA-13 | (1) §5.3 asigna toda decisión de permiso a CMP-08; (2) los actores **con permisos en el sistema** de §3.1 son los de `01 §7.3` (público, usuario sin rol, dueño, organizador, editor, delegado y sus combinaciones) más la única operación automática autorizada previamente; los actores organizativos y técnicos de §3.1 (persona responsable de evidencias, responsable administrativo, desarrollador) figuran **sin permisos de aplicación**; (3) ninguna fila de §3 a §7 introduce un rol, una facultad o un caso sin fuente en `01 §7.1`; (4) PA-13 remite a `01 §13.7` y no retira RF-ACC-07, la fila 13 ni A-9 |
| CR1-14 | **La evaluación cubre usuario, rol, torneo, equipo, partido, efecto y conflicto** | La tabla de entradas de §7.0 tiene **siete filas** con esas siete dimensiones |
| CR1-15 | **RT-13:** lectura de roles y estado, cálculo del efecto, decisión y aplicación forman una unidad indivisible, y las unidades críticas están enumeradas | §7.0 (pasos 2 a 5 y la aplicación del paso 6 como una sola unidad; el registro de rechazos, independiente y marcado [DER]), PA-07, PA-14, §5.5 (UI-01…13 con CMP-08 y CMP-16 en cada unidad, relación UI-06/UI-10, lista no exhaustiva y distinción entre transacción de datos y señal de cambio) |
| CR1-16 | **RT-14:** se contemplan efecto, historial, levantamientos vigentes, ausencia de actor alterno, consulta y previsualización sin restricción, y herencia en operaciones automáticas | §7.0, §3.1, UI-08 |
| CR1-17 | **2FA:** hay un paso explícito que compara el nivel de autenticación con la política efectiva de **todos** los roles vigentes y un único decisor | §7.0 paso 4 (posterior a reunir los roles, paso 2), §5.3 (CMP-06 emite el nivel; CMP-08 decide), RC-11 |
| CR1-18 | **Menores y datos sensibles:** clasificación por zona, fuga imposible hacia Z1 y Z4, y respaldos protegidos | En §5.6: (1) hay una fila por clase de dato; (2) ninguna fila de datos **internos o sensibles** de menores (nombre interno, indicador, metadatos de autorización) permite residencia persistente en Z1, Z4 ni en el almacén local de Z0; **solo** la etiqueta y el nombre público con autorización pueden tener copias derivadas en Z0 y Z1, sujetas a ADR-06 y HR1-11; (3) la presentación transitoria de datos internos solo se admite en CMP-02 para los roles de PRV-09 y PRV-14, y la de los metadatos de autorización solo para quien registra o revoca (matriz fila 20); (4) los respaldos figuran en Z3; (5) §5.1 declara que Z4 no recibe datos de jugadores ni de menores |
| CR1-19 | **Responsable único por función** y sin ciclos de **control** entre componentes, con los acoplamientos declarados | §5.3 (cada función aparece una sola vez en la columna «Responsable único»); §5.8: tres tipos de dependencia, acoplamientos AC-1…AC-3 y grafo de control con los clientes de Z0 como orígenes y los sumideros CMP-08, CMP-12, CMP-15, CMP-16, CMP-18 y CMP-19; habilitar y retirar la visibilidad tiene un único responsable (CMP-09) |
| CR1-20 | **Dependencias entre ADR** explícitas y acíclicas, con R2 antes de R3 y las decisiones dependientes del stack condicionadas a ADR-01 | §8.2: la tabla solo contiene la relación «Requiere», no hay columna «alimenta», ningún artefacto se requiere recíprocamente y el orden de resolución declarado (ADR-03.C y ADR-09.C … ADR-12) tiene todas las relaciones en un solo sentido, y cada relación es **semánticamente coherente** (el requerido aporta algo que el requirente necesita, sin que el requerido dependa del requirente); RC-25 se cumple porque los ADR de R2 son artefactos «.C» que ADR-01 consume |
| CR1-21 | **Trazabilidad bidireccional Q ↔ ADR y A ↔ ADR** | Las columnas «ADR» de §9.1 y §9.3 coinciden con §8.1 |
| CR1-22 | **Excepción OFF-17** registrada con requisito original, motivo, riesgos, obligación posterior y alcance | §2.5, RC-24 |
| CR1-23 | **Referencias sin ambigüedad** a secciones de `01-requisitos.md` | Toda referencia a una **sección** de `01` usa «`01 §n`»; los **identificadores** de `01` (RT, OFF, PRV, CL, etc.) se citan sin prefijo o como «`01 RT-02`»; las secciones propias se citan como «§n» |

---

## 11. Hallazgos pendientes de R1

Son observaciones de este análisis; ninguna está resuelta y ninguna cambia un requisito aprobado.

| ID | Hallazgo | Impacto | Tratamiento propuesto |
|---|---|---|---|
| HR1-01 | **RF-PUB-09 dice textualmente:** «≤ 60 s (propuesto) sin que el visitante recargue manualmente **más de una vez**». El mecanismo de actualización no está especificado, y los marcadores en vivo en tiempo real están fuera del MVP (`01 §3`). Se distinguen **tres niveles**: **(1) expiración con refresco manual** (la proyección se actualiza o expira en ≤ 60 s y el visitante recarga una vez), que cumple el texto literal; **(2) actualización periódica automática sin recarga**, que no es tiempo real; **(3) empuje en tiempo real** (por ejemplo mediante conexiones persistentes), fuera del MVP. Deben considerarse también `01 RD-RES-08`, `01 CL-30`, RNF-04 y RNF-15 | Condiciona ADR-06 y ADR-07 | Comparar en R3 los tres niveles; el nivel 1 es la alternativa mínima compatible con lo aprobado; **no se presume tiempo real** |
| HR1-02 | **RF-ACC-17 (levantamiento) es prioridad S.** Si se recorta, la restricción histórica de RT-14 no tiene vía de levantamiento, pero el historial inmutable y la evaluación por efecto siguen siendo necesarios | El modelo de historial no puede depender de que RF-ACC-17 exista | Diseñar el historial y la evaluación como parte del núcleo, y el levantamiento (UI-08) como extensión |
| HR1-03 | `01 OFF-12` dice que el cifrado local «se evalúa en la Fase 2» sin prometerlo | Requiere un ADR explícito | ADR-16 en R2 |
| HR1-04 | El nivel de autenticación depende de **todos** los roles vigentes (RC-11): un editor con 2FA opcional que reciba un rol de organizador debe enrolar 2FA antes de usarlo. Tampoco está fijado qué nivel se exige al sincronizar | El evaluador debe conocer el nivel de autenticación por sesión y la política efectiva por cuenta | ADR-03 y ADR-02 en R2 |
| HR1-05 | La reanudación sin conexión está confirmada solo para el editor asignado (`01 RD-RES-13`); `01 §9.1` permite capturar sin conexión al organizador y al dueño (A-10) | Hay una asimetría documentada, pendiente | El diseño debe permitir ambas lecturas (A-10, §9.3) |
| HR1-06 | Q-42 (a) deja abiertas las facultades de captura y de cambio de estado del organizador | La matriz de la Fase 4 y la revalidación al sincronizar dependen de la respuesta | Aislar en el evaluador y en CMP-13; no resolver |
| HR1-07 | **La verificación de lo específico de iOS** (almacenamiento persistente, cuotas, instalación) **no se hace en dispositivo en la Fase 2.** Se rige por la **excepción autorizada de §2.5** | Las decisiones de ADR-05 y ADR-16 que dependan de ello se marcan «no verificadas en dispositivo» | Investigación documental con fuentes oficiales en R2/R3; prueba real en 5E y Fase 7 (y en 5G, por `01 RK-11`) con Android e iPhone |
| HR1-08 | Q-07 sigue sin responder el presupuesto máximo y quién administra las cuentas | No bloquea el diseño; condiciona ADR-01 | Investigar planes gratuitos y de pago antes de proponer un máximo (R3) |
| HR1-09 | El candidato (b) de ADR-01, tal como lo plantea la condición 3, se presenta como almacén de datos: no se asume que traiga identidad ni políticas de acceso integradas, ni se asume lo contrario para ningún otro candidato | Si no las trae, CMP-06 y CMP-08 se resuelven en la aplicación y en la capa de datos | Verificar capacidades en fuentes oficiales en R3, sin asumirlas ahora |
| HR1-10 | RK-15 aceptado: la arquitectura no debe añadir recuperación excepcional de eventos rechazados, aunque sería técnicamente posible | Límite de diseño | Respetarlo; cualquier cambio exige decisión nueva del usuario |
| HR1-11 | **Las copias de vistas públicas y de datos preparados guardadas en el dispositivo no pueden purgarse a distancia** (RF-PUB-06, RF-PWA-02, OFF-02, OFF-12); una revocación, anonimización u ocultamiento no las alcanza hasta la próxima sincronización o limpieza | Riesgo residual para PRV-02/06 y CL-26; no hay plazo definido para cambios de privacidad | Registrarlo para R2 (ADR-04, ADR-05, ADR-16) y para el asesoramiento jurídico; no se promete un plazo |
| HR1-12 | **Los respaldos conservan datos anteriores a una anonimización.** PRV-06 exige que, tras anonimizar, «ningún campo personal queda en base de datos ni en auditoría»; los respaldos existentes (Z3, TB-9) conservan los datos previos hasta su caducidad | Riesgo residual para PRV-06; **la retención de los respaldos no está fijada en `01`:** RNF-08 exige respaldos verificables y restauración probada, y PRV-10 define retención para otros datos (jugadores dados de baja, invitaciones, operaciones locales, metadatos de incidencias, bitácora de seguridad y auditoría) pero **no menciona los respaldos**; se trata como punto abierto y, por su naturaleza, sujeto a asesoramiento jurídico | Registrarlo para ADR-12 y ADR-17 (R4) y para el asesoramiento jurídico; no se resuelve aquí ni se promete un plazo |
| HR1-13 | **`01 RNF-11` dice «prueba contra PostgreSQL real» y `01 RT-13` pide pruebas de carrera «contra PostgreSQL real».** El documento usa «almacén relacional» sin reinterpretar el requisito: la formulación aprobada **acota ADR-01 a almacenes compatibles con ese motor** | Condiciona el espacio de ADR-01 (los tres candidatos del usuario lo cumplen según su descripción, sin verificar) | Hacerlo visible; verificar compatibilidad con fuentes oficiales en R3; no modificar el requisito |
| HR1-14 | **El archivo de una exportación llega al dispositivo de quien la solicita y no puede purgarse a distancia:** la exportación de datos personales de un jugador (PRV-08, matriz fila 34) y el resumen limitado de OFF-24 salen del control de Z2/Z3 una vez descargados; «prohibido persistir en Z0» es un control de la aplicación, no del archivo ya entregado | Riesgo residual para PRV-08, PRV-09 y PRV-18; análogo a HR1-11 | Registrarlo para R2 (ADR-04, ADR-16) y para el asesoramiento jurídico; no se resuelve aquí |

---

## 12. Siguientes bloques (no autorizados)

| Bloque | Contenido propuesto | Estado |
|---|---|---|
| R2 | Modelo de amenazas; **requisitos, restricciones e invariantes independientes del proveedor** de autorización, 2FA, visibilidad, sincronización, auditoría y almacenamiento local (ADR-02, 03, 04, 05, 09, 16) | **Sin autorización** |
| R3 | Actualización y entrega pública; evaluación de proveedores, región y costos con fuentes oficiales; ADR principal; motor de reglas (ADR-01, 06, 07, 08, 10, 15). Confirma las decisiones de R2 condicionadas a ADR-01 | **Sin autorización** |
| R4 | Entornos, respaldo, observabilidad, continuidad, tareas programadas y retención; trazabilidad final; auditoría de cierre por revisor separado del autor y con contexto nuevo (ADR-11, 12, 13, 14, 17) | **Sin autorización** |

Cada bloque requiere un mensaje del usuario que lo nombre. La aprobación de la Fase 2 es un acto separado. **AUD-20:** el registro de R1 en `aprobaciones.md` y `bitacora.md` fue **atendido documentalmente el 2026-11-16**, por autorización independiente (§15.5). **R1 fue APROBADO formalmente por el usuario el 2026-11-16; la Fase 2 sigue AUTORIZADA, NO APROBADA globalmente, y R2, R3 y R4 siguen NO INICIADOS Y SIN AUTORIZACIÓN.**

---

## 13. Control del documento

| Versión | Fecha | Estado | Cambio |
|---|---|---|---|
| 0.1-R1 | 2026-11-15 | BORRADOR R1 — pendiente de revisión | Creación del bloque R1 |
| 0.2-R1 | 2026-11-15 | BORRADOR R1 corregido — pendiente de nueva revisión técnica | Ronda de correcciones de la auditoría (AUD-01…21); decisiones AUD-02 y AUD-14 |
| 0.3-R1 | 2026-11-15 | BORRADOR R1 (segunda ronda) — pendiente de revisión técnica focalizada | Condiciones C1–C8 de la segunda auditoría y hallazgos menores NUEVO-06, 10, 11, 12, 13 y 15 (§15) |
| 0.4-R1 | 2026-11-16 | BORRADOR R1 (corrección documental final) — pendiente de revisión y aprobación | Hallazgos de la revisión focalizada: NUEVO-16 a 20 y residuos de NUEVO-10 y 13 (§15.3) |
| 0.5-R1 | 2026-11-16 | BORRADOR R1 (cierre de correcciones técnicas) — pendiente de revisión y aprobación | Hallazgos de la verificación final: NUEVO-21 a 26 (§15.4) |
| 0.5-R1 (sincronización editorial) | 2026-11-16 | BORRADOR R1 — pendiente de aprobación formal | Solo referencias a AUD-20 (encabezado, §12, §14, §15.4, §15.5, CR1-12), tras su registro documental en `aprobaciones.md` y `bitacora.md`. **Se conserva la identificación v0.5-R1** porque esos documentos la citan y no se autorizó modificarlos; sin cambios de contenido técnico |
| 0.5-R1 (sincronización posterior a la aprobación) | 2026-11-16 | **R1 APROBADO (2026-11-16)** | Solo el estado editorial de aprobación (encabezado, §12, §13, §15.5 y fila AUD-20 de §14): se reemplaza «pendiente de aprobación formal» por la aprobación formal otorgada por el usuario y registrada en `aprobaciones.md` y `bitacora.md`. **Se conserva la identificación v0.5-R1**; sin cambios de requisitos, componentes, ADR, permisos, riesgos, preguntas ni criterios técnicos. La fila anterior se mantiene como hecho histórico |
| 0.1-R2A a 0.5-R2A (borrador) | 2026-11-16 y 2026-11-17 | **R2-A: APROBADO CONDICIONALMENTE como entregable documental (2026-11-17); antes, BORRADOR sin aprobar** | **Solo se añaden** una línea de estado en el encabezado, esta fila y la Parte II (§16 en adelante): modelo inicial de amenazas, casos de abuso, ADR-03.C y ADR-09.C. La v0.1 se auditó (dictamen «R2-A REQUIERE CORRECCIONES»), la v0.2 se revisó (dictamen «R2-A APROBABLE CON CONDICIONES»; revisores del mismo tipo de modelo, con independencia solo de contexto) la v0.3 se validó (dictamen «R2-A APROBABLE CON CONDICIONES»), la v0.4 se verificó (dictamen «R2-A LISTO CON CONDICIONES») y la v0.5 aplica las correcciones **sin nueva revisión independiente**. **0 líneas de §0 a §15 eliminadas o modificadas** (identificación v0.5-R1 conservada para ese contenido). R2-B y R2-C no iniciados |

Este documento **no autoriza** commit, push, PR, merge, R2, R3, R4 ni el cierre de la Fase 2, y **no afirma que ningún control esté implementado**.

---

## 14. Registro de la auditoría de R1 y de sus correcciones

Revisor: subagente con contexto nuevo y de solo lectura (el mismo tipo de modelo que redactó el borrador; **sin independencia organizativa ni externa**). La corrección no equivale a aprobación: está **pendiente de nueva revisión técnica de solo lectura**.

| ID | Sev. | Estado | Corrección | Evidencia | Riesgo residual |
|---|---|---|---|---|---|
| AUD-01 | ALTA | Corregido | ADR-02 reformulado: reparto servidor–capa de datos con ambas capas siempre; una sola capa fuera del espacio; RC-04 lo declara | §8.1 (ADR-02), §2.2 (RC-04) | La forma concreta del reparto se decide en R2 |
| AUD-02 | ALTA | Corregido en v0.2 y **rehecho en v0.3** (la segunda auditoría halló pares recíprocos: NUEVO-01) | Matriz de artefactos ADR-xx.C / ADR-xx.I con solo la relación «Requiere»; ADR-09/17 delimitados | §8.2 (ver §15, C1), RC-25 | Si ADR-01 no cumple una capacidad exigida, hay que revisar un ADR de R2 |
| AUD-03 | MEDIA | Corregido | PA-08 reescrito por clase de dato; clasificación por zona | PA-08 (§4), §5.6 | PRV-18 sigue abierta (identificación indirecta) |
| AUD-04 | MEDIA | Corregido | RC-05 y FL-3: «no se depende de segundo plano»; fuente DER | §2.2 (RC-05), §7.3 | — |
| AUD-05 | MEDIA | Corregido | HR1-01 y ADR-07 citan el texto literal y distinguen tres niveles | §11 (HR1-01), §8.1 (ADR-07) | La elección de nivel es de R3 |
| AUD-06 | MEDIA | Corregido | §7.0 con entradas, pasos 2 a 6 en una unidad, política de 2FA, bloqueo sin autoridad alterna, consulta y previsualización | §7.0 | Nivel de autenticación al sincronizar sin fijar (ADR-03) |
| AUD-07 | MEDIA | Corregido | Una sola operación del sistema autorizada previamente; las demás heredan al actor y se definen en ADR-14/17 | §3.1, §7.0 | Identidad de ejecución de tareas automáticas por decidir |
| AUD-08 | MEDIA | Corregido | Respaldos en Z3; Z4 redefinida; TB-5 corregido; TB-7, TB-8 y TB-9 añadidos | §5.1, §5.7 | Análisis de amenazas de las fronteras en R2 |
| AUD-09 | MEDIA | Corregido | Proyección de preparación en CMP-14; señal de cambio ante cambios de visibilidad y privacidad; HR1-11 | §5.2 (CMP-14), §7.4, §11 (HR1-11) | Copias locales no purgables a distancia; plazo sin definir |
| AUD-10 | MEDIA | Corregido | Unidades indivisibles UI-01…09 y PA-14 | §5.5, §4 (PA-14) | Su implementación depende de ADR-01 y ADR-02 |
| AUD-11 | MEDIA | Corregido | Contrato lógico de CMP-12 (entradas, salidas, propiedades, invocadores, versión, atomicidad); zona «Z2 o Z3 (ADR-08)» | §5.9, §5.2 (CMP-12) | Ubicación y tecnología quedan en ADR-08 (R3) |
| AUD-12 | MEDIA | Corregido | Responsable único por función; CMP-13 sin reglas de negocio; CMP-18 solo envía | §5.2, §5.3, §5.8 | Redistribución posible al decidir ADR-01 |
| AUD-13 | MEDIA | Corregido | Categoría «Global»; reglas 1, 6 y 8 corregidas; eventos de gobierno | §6.1, §6.2 | — |
| AUD-14 | MEDIA | Corregido (decisión del usuario) | Excepción registrada con requisito original, motivo, riesgos, obligación y alcance | §2.5, RC-24, HR1-07 | Hallazgo tardío en dispositivo real |
| AUD-15 | MEDIA | Corregido | PA-13 (lectura provisional por caso, antes «fallo cerrado») y CR1-13…23; CR1-02 y CR1-05 reformulados | §4 (PA-13), §10 | — |
| AUD-16 | BAJA | Corregido | Convención «`01 §n`» frente a «§n» | §0.2, CR1-23 | — |
| AUD-17 | BAJA | Corregido | Tablas Q↔ADR y A↔ADR alineadas con §8.1; A-8 en ADR-02/04; FL-2 sin A-3 | §8.1, §9.1, §9.3, §7.2 | — |
| AUD-18 | BAJA | Corregido | RNF-05, RNF-11, RNF-15; motivo solo al revocar un dueño; «dueño global»; RT-08 con CMP-07; terminología de la auditoría | §2.1, §7.2, §7.3, §9.4, §12 | — |
| AUD-19 | BAJA | Corregido en v0.2 y **completado en v0.3** (persistían menciones de «invalidación») | «Señal de cambio (según ADR-06)» en CMP-14, UI-05, TB-8, FL-2, FL-4 y ADR-06; Z1 «con o sin caché» | §5.1, §5.2, §5.5, §5.7, §7.2, §7.4, §8.1 (ver §15, C8) | — |
| AUD-20 | BAJA | **Diferido en v0.2; atendido documentalmente el 2026-11-16** | En v0.2 no era posible corregirlo aquí: requería editar `aprobaciones.md` y `bitacora.md`, no autorizados en esa ronda. Se atendió después por autorización independiente | §12, §15.5 | Al 2026-11-16, tras la aprobación formal de R1, esta evidencia sigue siendo referencia y no archivo; las citas de las autorizaciones constan en la conversación de la sesión, no en archivos del repositorio |
| AUD-21 | BAJA | Corregido | Glosario de estados y términos y modelo N0 → N1 → N2 | §0.3 | — |

---

## 15. Segunda auditoría de R1 y segunda ronda de correcciones (v0.3)

Segundo revisor: subagente distinto del anterior, con contexto nuevo y de solo lectura (mismo tipo de modelo que redactó el borrador; **sin independencia organizativa ni externa**). Dictamen técnico recibido: **R1 APROBABLE CON CONDICIONES** (recomendación, no aprobación). Esta versión atiende sus ocho condiciones; **no se declara resuelto ningún punto sin su evidencia en el texto** y queda **pendiente de revisión técnica focalizada**.

### 15.1 Condiciones C1–C8

| ID | Hallazgo | Estado | Corrección | Evidencia (sección) | Riesgo residual |
|---|---|---|---|---|---|
| C1 | NUEVO-01 | Corregido | La matriz solo tiene la relación «Requiere»; los ADR de R2 se dividen en ADR-xx.C (capacidades exigidas) y ADR-xx.I (implementación condicionada); ADR-01/ADR-10 son una decisión conjunta; ADR-08 ya no «alimenta» a ADR-04 ni a ADR-05; ADR-11 figura como requisito de ADR-12 y ADR-13; orden de resolución declarado | §8.2 | Si ADR-01 no cumple una capacidad exigida, se revisa un ADR-xx.C |
| C2 | NUEVO-02 | Corregido | Tres tipos de dependencia (control, lectura, retorno); acoplamientos AC-1…AC-3 declarados; grafo de control acíclico con sus sumideros | §5.8 | La separación física de componentes puede alterar los acoplamientos (ADR-01, ADR-02) |
| C3 | NUEVO-03 | Corregido | PA-13 aplica la lectura provisional que `01 §13.7` fija para cada caso y la denegación por defecto solo donde ese texto la indica; mantiene RF-ACC-07, la fila 13 (A-8) y las dos operaciones de A-9 | §4 (PA-13), §9.3, CR1-13 | Provisional; no es política definitiva |
| C4 | NUEVO-04 | Corregido | Columnas «residencia persistente», «presentación transitoria» y «prohibida»; presentación del nombre interno solo en CMP-02 para los roles de PRV-09; nota sobre la tensión Q-20/PRV-02 | §5.6 | PRV-18 y Q-20 abiertas |
| C5 | NUEVO-05 | Corregido | Transacción de datos distinta de la señal de cambio; UI-10 (confirmar y recalcular), UI-05 (corregir y recalcular) y UI-07 (anonimizar) acotadas a datos y proyecciones derivadas; la propagación a Z1 y Z0 queda fuera de la transacción | §5.5, §5.9 (Atomicidad), §7.4 | Plazo de propagación sin definir (ADR-06, HR1-11) |
| C6 | NUEVO-07 | Corregido | Entradas, salidas y versión de reglamento marcadas **[ABIERTO]** (Q-22, Q-23, Q-21, Q-29) con las lecturas admitidas | §5.9 | La respuesta a Q-22 y Q-23 puede reducir el contrato |
| C7 | NUEVO-09 | Corregido | CR1-02, 04, 06, 07, 11, 12, 13, 14, 15, 17, 18, 19 y 20 con comprobaciones observables; CR1-14 con siete dimensiones; CR1-12 compatible con el cierre de AUD-20 | §10 | Los criterios se verifican sobre este texto, no sobre el producto |
| C8 | AUD-19 | Corregido | «Señal de cambio (según ADR-06)» en CMP-14, UI-05, TB-8, FL-2, FL-4, ADR-06 y §6.1 | §5.2, §5.5, §5.7, §6.1, §7.2, §7.4, §8.1 | ADR-06 aún no decidido |

### 15.2 Hallazgos menores

| ID | Estado | Corrección | Evidencia | Pendiente |
|---|---|---|---|---|
| NUEVO-06 | Corregido | Lista declarada no exhaustiva; UI-11 (revocación serializada), UI-12 (invitación de delegado) y UI-13 (alta concurrente de jugador) | §5.5 | Enumeración definitiva en la Fase 4 |
| NUEVO-10 | Corregido | La política de 2FA se verifica después de reunir los roles; el registro de un rechazo debe persistir aunque la operación se revierta | §7.0 | Mecanismo en ADR-09 (R2) |
| NUEVO-11 | Corregido (sin resolver la decisión) | El programador de tareas (Z4) solo dispara; la herencia de RT-14 se limita a efectos de una operación de un actor; la identidad de las tareas sin actor disparador queda como **pregunta abierta** | §3.1, §5.1, §8.1 (ADR-14) | ADR-14 y ADR-17 (R4) |
| NUEVO-12 | Corregido | ADR-02 pregunta qué verifica además la aplicación y qué impone siempre la capa de datos; ningún permiso en una sola capa | §8.1 (ADR-02) | Reparto concreto en R2 |
| NUEVO-13 | Corregido | Designación de organizador y 2FA globales; registros de RF-AUD-06 con alcance; exportación de datos del jugador asignada a CMP-17; HR1-12 (respaldos tras anonimizar) | §6.2, §5.3, §11 (HR1-12) | Retención de respaldos `PEND-JUR` |
| NUEVO-15 | Corregido | 5G declarado como adición de `01 RK-11`, no de la excepción autorizada; HR1-13 hace visible que `01 RNF-11` y RT-13 nombran el motor contra el que se prueba | §2.5, §11 (HR1-13) | Compatibilidad de candidatos en R3 |

### 15.3 Revisión focalizada final y corrección documental (v0.4)

Tercer revisor: subagente de contexto nuevo y solo lectura, distinto de los anteriores (mismo tipo de modelo; **sin independencia organizativa ni externa**). Dictamen técnico recibido: **R1 APROBABLE CON CONDICIONES** (recomendación). Estado de C1 a C8 según ese revisor: C1, C3, C4, C5 y C6 resueltos; C2, C7 y C8 parciales. Esta versión atiende sus observaciones; **ningún punto se da por resuelto sin evidencia en el texto**, y el documento sigue siendo **borrador, pendiente de revisión y aprobación**.

| ID | Corrección | Evidencia (sección) | Estado | Riesgo residual |
|---|---|---|---|---|
| NUEVO-16 (MEDIA) | Se retiró la afirmación de que lo público «nunca muestra una versión parcial» y de que «así se cumple» CL-15. Se sustituyó por una **garantía exigida** (coherencia entre resultados y clasificaciones en las vistas públicas de un torneo), condicionada a ADR-04 y ADR-06, y se quitó la presuposición de proyecciones persistentes y de entrega bajo demanda (la presuposición de una tabla materializada y de una señal de cambio obligatoria se retiró después, en v0.5, §15.4) | §5.5, §5.9 (Atomicidad), §7.1, §7.4, UI-05, UI-07, UI-10 | Corregido | La garantía no está demostrada; depende de ADR-04 y ADR-06 (R2 y R3) |
| NUEVO-17 | Aristas añadidas al grafo de control: CMP-13→CMP-16, CMP-17→CMP-08, CMP-14→CMP-08; lectura de CMP-18 para N-12. Grafo comprobado sin ciclos | §5.8 | Corregido | Los acoplamientos pueden cambiar al decidir ADR-01 y ADR-02 |
| NUEVO-18 | CMP-08 y CMP-16 incluidos en todas las unidades; nota general; UI-10 declarada **dentro de** UI-06 cuando el resultado llega por sincronización; ninguna atomicidad se atribuye a operaciones externas a la transacción de datos | §5.5 | Corregido | La enumeración definitiva por operación es de la Fase 4 |
| NUEVO-19 | §5.6 distingue credenciales de servicio (solo Z2), credenciales y segundo factor del usuario, sesión local del dispositivo, metadatos de autorización o consentimiento (PRV-05, nunca públicos por PRV-03), datos que PRV-04 excluye del esquema, tránsito de la dirección de correo y la exportación PRV-08; la tabla declara que no concede permisos | §5.6 | Corregido | PRV-18 y Q-20 siguen abiertas |
| NUEVO-20 | Se **mejoró el documento** para cumplir los criterios (regla 7 de §6.1 con requisitos; actores de §3.1 diferenciados; §0.4 y §15 sin nombrar el motor; RC-04 marcada [DER]) y solo se precisaron los criterios cuando la redacción no era verificable (CR1-02, 04, 07, 13, 15, 20, 23) | §0.4, §3.1, §6.1, §10, §15.2 | Corregido | Los criterios se verifican sobre el texto |
| NUEVO-10 (residual) | El registro de rechazos o bloqueos (RF-AUD-06) se separa del resultado transaccional y se marca [DER]; ADR-09 incluye su persistencia; ADR-02.C requiere ADR-09.C | §5.5, §7.0 (paso 6), §8.1 (ADR-09), §8.2 | Corregido | Mecanismo en ADR-09 (R2) |
| NUEVO-13 (residual) | El ejemplo que atribuía a RT-14 la designación o revocación de un dueño se corrigió: RT-14 es siempre por equipo y torneo, y los intentos de gobierno van al registro de seguridad (RF-AUD-05); la exportación PRV-08 consta en CMP-17; la retención de respaldos se declara **no fijada en `01`** (PRV-10 no los menciona) | §6.2, §5.2 (CMP-17), §11 (HR1-12) | Corregido | Retención de respaldos abierta (ADR-12, ADR-17, asesoramiento jurídico) |
| C2, C3, C6, C7, C8 (residuos) | CMP-17→CMP-08 y demás aristas (C2); PA-13 renombrada «lectura provisional por caso» (C3); Q-21 marcada [ABIERTO] en las entradas del contrato de CMP-12 (C6); criterios reformulados (C7); sin presuposición de mecanismo de caché (C8) | §4, §5.8, §5.9, §10 | Corregido | ADR-06 aún no decidido |
| NUEVO-11 (residual) | La pregunta de ADR-17 incluye la identidad de ejecución de la retención sin actor disparador | §8.1 (ADR-17) | Corregido | Decisión en ADR-14 y ADR-17 (R4) |
| Mejora opcional | El objetivo de revocación en ≤ 60 s de `01 RF-ACC-07` («sujeto a validación en la Fase 2») pasa a la pregunta de ADR-03 | §8.1 (ADR-03) | Registrado | Validación en R2 |

### 15.4 Verificación final y última edición documental (v0.5)

Cuarto revisor: subagente de contexto nuevo y solo lectura, distinto de los anteriores (mismo tipo de modelo; **sin independencia organizativa ni externa**). Dictamen técnico recibido: **R1 APROBABLE CON CONDICIONES** (recomendación), con NUEVO-21 y NUEVO-22 (MEDIA) como correcciones recomendadas y NUEVO-23 a NUEVO-26 (BAJA) opcionales. Esta versión los atiende; **ningún punto se da por resuelto sin evidencia en el texto** y el documento sigue siendo **borrador, pendiente de revisión y aprobación**.

| Hallazgo | Cambio realizado | Evidencia (sección) | Verificación | Riesgo residual |
|---|---|---|---|---|
| NUEVO-21 (MEDIA) | Grafo de control completo: los clientes de Z0 son orígenes (a través de TB-1 y TB-3) en lugar de «entrada del servidor»; añadidas CMP-09→CMP-07 (UI-01), CMP-07→CMP-10 (UI-12), CMP-13→CMP-16 y la telemetría hacia CMP-19; CMP-19 incluido como sumidero. **Habilitar y retirar la visibilidad tiene un único responsable: CMP-09** (RF-TOR-02, RD-CAL-08; CMP-08 autoriza; CMP-14 solo lee el estado y aplica `01 §9.6`); A-8 pasa a CMP-09 y CMP-08 | §5.2 (CMP-09, CMP-14), §5.3, §5.8, §9.3 (A-8), CR1-19 | Defecto corregido; verificación de ciclos y sumideros sobre el grafo reconstruido | Los acoplamientos pueden cambiar al decidir ADR-01 y ADR-02 |
| NUEVO-22 (MEDIA) | **ADR-02.C fija la frontera atómica** (UI-01…13, UI-05 y UI-10); **ADR-05.C** la aplica a la ruta sincronizada; **ADR-04.C fija la garantía de coherencia pública**; **ADR-06 decide el mecanismo**; **ADR-08 consume** frontera y garantía sin redefinirlas. La matriz §8.2 refleja esas relaciones | §5.5, §5.9, §8.1 (ADR-02, 04, 05, 06, 08), §8.2 | Defecto corregido; relaciones sin pares recíprocos ni ciclos | La garantía sigue **sin demostrarse**: depende de ADR-04 y ADR-06 |
| NUEVO-23 | «Recalcular la tabla» sustituido por dejar consistentes los resultados y la clasificación derivada (materializada o calculada al servirla); la señal de cambio se registra **si ADR-06 la adopta** (UI-05, UI-07, UI-10, CMP-14, FL-2, FL-4); texto de §15.3 corregido | §5.2, §5.5, §7.2, §7.4, §15.3 | Defecto corregido | ADR-04, ADR-06 y ADR-08 por decidir |
| NUEVO-24 | **Riesgo del archivo exportado en el dispositivo: HR1-14**; contenido mínimo del mensaje en tránsito por Z4; «o consulta» eliminado de los metadatos de autorización (solo registra o revoca, matriz fila 20); credenciales de servicio rotuladas [DER]; **ADR-14 es el único dueño de la identidad de ejecución sin actor disparador** (ADR-17 define solo la política), con lo que no hay pregunta compartida ni relación recíproca | §5.6, §11 (HR1-14), §3.1, §8.1 (ADR-14, ADR-17) | Defecto corregido; riesgos registrados | Decisión en ADR-14 (R4); HR1-14 a R2 y asesoramiento jurídico |
| NUEVO-25 | El punto de persistencia independiente de OFF-06 y OFF-20 queda como **pregunta abierta, sin resolver**, en ADR-05, ADR-09 y §5.5 | §5.5, §8.1 (ADR-05, ADR-09) | Decisión pendiente de R2, registrada | La decisión es de ADR-05.C y ADR-09.C |
| NUEVO-26 | CR1-04, CR1-05, CR1-18 y CR1-19 reformulados; §8.1 y §9 distinguen decisiones confirmadas de abiertas; UI-02 uniforme; «no están declaradas como actor privilegiado»; 5G citado siempre como adición de `01 RK-11`; AUD-20 sin lectura circular | §10, §8.1, §9, §3.1, §2.5, §11 (HR1-07), §15.5 | Defecto corregido | — |

**Estado técnico:** **no se declara ningún defecto material pendiente** que impida el procedimiento documental de AUD-20 (atendido el 2026-11-16, §15.5); las decisiones que quedan son las de §15.5 y las de R2, R3 y R4.

### 15.5 Estado de AUD-20 y hallazgos que permanecen abiertos

| ID | Estado | Condición |
|---|---|---|
| AUD-20 | **Atendido documentalmente (2026-11-16); no abierto** | `aprobaciones.md` y `bitacora.md` registran, por autorización documental independiente, la autorización de R1 y su alcance, las cuatro rondas de correcciones (v0.2 a v0.5), las cuatro revisiones técnicas de solo lectura (como recomendaciones, de subagentes de contexto nuevo y mismo tipo de modelo, sin independencia organizativa ni externa), las decisiones AUD-02 y AUD-14, el estado real y las condiciones pendientes, y se actualizó el texto que decía que R1 «no se ha iniciado». **Evidencia que sigue siendo referencia y no archivo:** los mensajes originales constan en la conversación de la sesión y la fecha exacta de cada revisión no se registró. **R1 fue APROBADO formalmente por el usuario el 2026-11-16** (acto posterior y distinto del registro, que no lo aprobó); la Fase 2 sigue sin aprobación global |
| HR1-01 a HR1-14 | Abiertos | Son observaciones de diseño para R2, R3 y R4 (§11); ninguna cambia un requisito aprobado |

---

# PARTE II — BLOQUE R2-A (v0.5-R2A, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL, 2026-11-17)

> **Estado: R2-A v0.5-R2A — APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-17); el contenido técnico sigue siendo PROPUESTA.** Todo lo que sigue a esta línea (§16 en adelante) es trabajo nuevo de R2-A. **No forma parte del contenido aprobado de R1** (§0 a §15, v0.5-R1), que no se modifica. Ninguna decisión técnica de esta parte está tomada: la aprobación es documental y no aprueba las propuestas. **Esta versión (v0.5) corrige la v0.4 tras una verificación focalizada y NO ha sido sometida a una nueva auditoría ni revisión independiente** (§16.0, §16.12).

## 16. Bloque R2-A: modelo inicial de amenazas, ADR-03.C y ADR-09.C

### 16.0 Alcance, autorizaciones y reglas de esta parte

**Alcance de R2-A.** Exclusivamente el **modelo inicial de amenazas**, los **casos de abuso**, **ADR-03.C** y **ADR-09.C**. **R2-B (ADR-02.C) y R2-C (ADR-04.C, ADR-05.C, ADR-16.C): NO INICIADOS Y SIN AUTORIZACIÓN.** Archivo autorizado: solo este documento.

**Autorizaciones y trabajos de R2-A** (los mensajes constan en la conversación de la sesión):

| Fecha | Acto | Mensaje | Resultado |
|---|---|---|---|
| 2026-11-16 | Inicio controlado de R2-A, con consulta de documentación oficial (sin instalar nada, sin cuentas, sin servicios, sin validar dispositivos) | «AUTORIZO EL INICIO DEL BLOQUE R2-A DE LA FASE 2 — ARQUITECTURA Y AMENAZAS.» | Borrador v0.1-R2A |
| 2026-11-17 | Auditoría técnica **de solo lectura** | «AUTORIZO EXCLUSIVAMENTE UNA AUDITORÍA TÉCNICA DE SOLO LECTURA DEL BORRADOR R2-A v0.1-R2A.» | Dictamen **«R2-A REQUIERE CORRECCIONES»** de un subagente de contexto nuevo (24 hallazgos: 11 materiales, 9 menores y 4 opcionales). **Es una recomendación, no una aprobación:** el revisor es del mismo tipo de modelo que redactó el borrador, sin independencia organizativa ni externa |
| 2026-11-17 | Una ronda de correcciones documentales | «AUTORIZO EXCLUSIVAMENTE UNA RONDA DE CORRECCIONES DOCUMENTALES DEL BORRADOR R2-A.» | Borrador **v0.2-R2A** (§16.13) |
| 2026-11-17 | Revisión técnica focalizada **de solo lectura** de la v0.2 | «AUTORIZO EXCLUSIVAMENTE UNA REVISIÓN TÉCNICA DE SOLO LECTURA DEL BORRADOR R2-A v0.2.» | Dictamen **«R2-A APROBABLE CON CONDICIONES»** de un subagente de contexto nuevo (**independencia de contexto, no organizativa ni externa**) con 20 hallazgos nuevos (R-01…R-20): 8 defectos materiales de severidad media, 3 decisiones pendientes y 9 mejoras editoriales. El revisor advirtió que, con una lectura estricta de «todo lo corregido debe ser literalmente exacto», el dictamen habría sido «REQUIERE CORRECCIONES» |
| 2026-11-17 | Una corrección documental focalizada | «AUTORIZO EXCLUSIVAMENTE UNA CORRECCIÓN DOCUMENTAL FOCALIZADA DEL BORRADOR R2-A.» | Borrador **v0.3-R2A** (§16.14) |
| 2026-11-17 | Validación técnica focalizada **de solo lectura** de la v0.3 | «AUTORIZO EXCLUSIVAMENTE UNA REVISIÓN TÉCNICA DE SOLO LECTURA DEL BORRADOR R2-A v0.3.» | Dictamen **«R2-A APROBABLE CON CONDICIONES»** de un subagente de contexto nuevo (independencia de contexto, no organizativa ni externa): 2 condiciones bloqueantes documentales (V-01, V-02) y 5 menores (V-03 a V-07); ninguna decisión técnica es imprescindible para aprobar el bloque documental |
| 2026-11-17 | Una corrección documental focalizada de la v0.3 | «AUTORIZO EXCLUSIVAMENTE UNA CORRECCIÓN DOCUMENTAL FOCALIZADA DEL BORRADOR R2-A.» | Borrador **v0.4-R2A** (§16.15) |
| 2026-11-17 | Verificación final focalizada **de solo lectura** de la v0.4 | «AUTORIZO EXCLUSIVAMENTE UNA REVISIÓN TÉCNICA DE SOLO LECTURA DEL BORRADOR R2-A v0.4.» | Dictamen **«R2-A LISTO CON CONDICIONES»** de un subagente de contexto nuevo (independencia de contexto): V-01 a V-07 resueltos en lo verificable y 4 hallazgos documentales nuevos (W-01 a W-04); ningún defecto material conocido que impida una aprobación documental |
| 2026-11-17 | Una corrección documental focalizada de la v0.4 | «AUTORIZO EXCLUSIVAMENTE UNA CORRECCIÓN DOCUMENTAL FOCALIZADA DEL BORRADOR R2-A v0.4.» | **Esta versión, v0.5-R2A** (§16.16) |

**Registro.** **Registro documental realizado en `aprobaciones.md` y `bitacora.md` el 2026-11-17** (mensaje «REGISTRO DOCUMENTAL DE R2-A»). **R2-A continúa pendiente de aprobación formal.** Quedaron registrados la autorización de R2-A, la auditoría y las revisiones, las rondas de correcciones y la ampliación de la red permitida del entorno (§16.9). El registro **no aprueba R2-A, R2 ni la Fase 2** y **no implica validación empírica de la seguridad**. Las versiones anteriores de esta parte (v0.1 a v0.4) declaraban ese registro pendiente (precedente AUD-20).

**Sin nueva revisión.** La v0.5 **no fue auditada ni revisada de nuevo** por nadie distinto del autor antes de su aprobación documental. Las comprobaciones de §16.12 son **del autor** y **no sustituyen** una revisión independiente. Nada de esta versión debe presentarse como «auditada» ni como «revisada» tras las correcciones.

**Aprobación condicional de R2-A como entregable documental (2026-11-17).** Por acto expreso del usuario (mensajes «APROBACIÓN FORMAL DE R2-A» y «REGISTRO FORMAL DE APROBACIÓN CONDICIONAL R2-A»), la v0.5-R2A quedó **APROBADA CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL**. **Comprende:** el modelo inicial de amenazas y los casos de abuso; ADR-03.C y ADR-09.C como análisis y propuestas de capacidades; las correcciones hasta la v0.5-R2A; la metodología, la trazabilidad y el registro documental. **No comprende ni implica:** aprobar para implementación las capacidades, mecanismos o alternativas propuestas; fijar PAR-01…06; aceptar RES-01…12; conceder excepción a D-2; resolver las 12 decisiones abiertas (la 11 está atendida); declarar la seguridad implementada o validada empíricamente; aprobar R2 en conjunto ni la Fase 2; autorizar R2-B, R2-C, R3 o R4. **Las decisiones que `01` asigna a la Fase 2 deberán resolverse antes del cierre global de la Fase 2, salvo modificación o excepción formal aprobada.** La v0.5 se aprobó **sin una nueva revisión independiente posterior a sus últimas correcciones**; las revisiones anteriores fueron de subagentes del mismo tipo de modelo, con independencia solo de contexto y **sin independencia organizativa ni externa**.

**Integridad.** SHA-256 del **contenido aprobado** (este archivo inmediatamente antes de añadir los metadatos de aprobación): `d53db4ac5adcefa45df13ef39747296f3af316601d3b56d8b87cdb8de0a5aaa8`. El archivo con los metadatos de aprobación tiene **otro** SHA-256, que figura en `aprobaciones.md` y `bitacora.md` (un archivo no puede contener su propio hash). Las modificaciones de esta operación son exclusivamente de estado y de metadatos editoriales; no alteran el contenido técnico (amenazas, matrices, riesgos, parámetros, requisitos ni alternativas) ni §0 a §15.

**Relación con R1.** El contenido de §0 a §15 es el aprobado el 2026-11-16 y permanece **íntegro**: esta parte solo **añade** una línea de estado en el encabezado, una fila en §13 y las secciones desde §16 (hay dos inserciones editoriales dentro de §0 a §15, pero **0 líneas eliminadas o modificadas**). Donde R1 dice que R2 «no tiene autorización» (encabezado, §12, §13), eso describe el estado **a la fecha de aprobación de R1**; para R2-A prevalece este apartado. **§8.1 sigue listando ADR-03 y ADR-09 como PREVISTO.**

**Etiquetas de esta parte** (se suman a las de §0.1):

| Etiqueta | Significado |
|---|---|
| **[CONF]**, **[DER]**, **[ABIERTO]** | Como en §0.1: lo aprobado, lo derivado de ello y lo abierto en `01 §13` |
| **[REC]** | **Propuesta de R2-A. No es una decisión** ni un requisito nuevo. Solo el usuario puede convertirla en decisión |
| **[NO VERIF]** | Afirmación apoyada en documentación consultada, **no verificada en dispositivo ni por prueba** (RC-22, RC-24). Siempre con fuente `F-xx` (§16.9) |
| **[SUP]** | Juicio del autor sin dato empírico (por ejemplo, las probabilidades de §16.1) |

**Identificadores nuevos:** `AS-` activos · `ACT-` actores de amenaza · `AM-` amenazas · `CAB-` casos de abuso · `OM-` amenazas registradas sin evaluar · `RES-` riesgos residuales · `IN-03-`/`IN-09-` invariantes · `CAP-` formulaciones de capacidad · `PAR-` parámetros propuestos · `EV-` eventos de seguridad · `F-` fuentes · `IOS-` afirmaciones sobre iOS · `H2A-` hallazgos de R2-A · `CR2A-` criterios de aceptación · `A-01…A-24` (§16.13) son los hallazgos de la primera auditoría y `R-01…R-20` (§16.14) los de la revisión focalizada de la v0.2 (no confundir `A-01…A-24` con las ambigüedades `A-1…A-16` de `01 §13.7`).

**Regla de nombres (extiende §0.4).** Esta parte **no selecciona** proveedor, producto, mecanismo ni stack. Los nombres de documentos, estándares y navegadores aparecen **solo en el registro de fuentes (§16.9)** y en las afirmaciones sobre iOS, que son referencias a fuentes y **no son selección**; en el resto se usan categorías de capacidad (CR2A-01). Los ejemplos de mecanismo son **ilustrativos** de una capacidad y no una elección.

**Alcance de la consolidación.** **Este modelo de amenazas es inicial y parcial.** Cubre TB-1…TB-9 con el conocimiento de R2-A; **será consolidado durante R2-C**, cuando existan ADR-02.C, ADR-04.C, ADR-05.C y ADR-16.C. Hasta entonces, las amenazas cuyo control depende de esos artefactos figuran con su responsable (§16.3, §16.5) y **no se consideran tratadas**.

### 16.1 Metodología, escalas y criterios de aceptación de riesgo

**Método.** Para cada frontera TB-1…TB-9 de §5.7 se identifican: activos y actores (§16.2), amenazas, casos de abuso, controles, riesgo inherente y, para los riesgos que subsisten, riesgo residual (§16.10). La **categoría** de cada amenaza (suplantación, manipulación, repudio, divulgación, denegación, elevación de privilegios) es una **etiqueta de clasificación de uso común** que el autor adopta; **no proviene de una fuente consultada en R2-A** [SUP].

**Controles exigidos, controles propuestos y mitigaciones implementadas.**
- **Exigido:** obligación de diseño que tiene su fuente en un requisito aprobado de `01` (`[CONF]`) o derivado de él (`[DER]`), **o en una restricción, un principio o un hallazgo ya aprobados en R1** (RC-xx, PA-xx, §5.6, UI-xx, HR1-xx; R1 se aprobó el 2026-11-16); se cita su fuente. Un control exigido puede carecer todavía de mecanismo o de plazo (por ejemplo, PRV-06 sin plazo para los respaldos): se señala en «Estado».
- **Propuesto [REC]:** control que **no tiene fuente en `01`**; lo propone R2-A y **no obliga** hasta su aprobación.
- **Implementado:** control que existe y se probó. **Hoy no hay ninguno** (RC-22: sin código, sin servicios, sin pruebas).

La columna «Estado» de §16.3 toma tres valores: **Exigido** (todos los controles listados tienen fuente en `01` o en R1 aprobado), **Exigido + [REC]** (hay controles exigidos y además propuestos) y **Propuesto [REC]** (ninguno tiene fuente en `01`). **Ninguna fila dice «Implementado» ni declara una amenaza mitigada.**

**Probabilidad cualitativa** [SUP]: juicio del autor sobre la posibilidad de que el evento ocurra (por ataque o por incumplimiento no malicioso) en el contexto del sistema (organización pequeña, ligas amateur, datos de menores, ~3–6 torneos, `01 RNF-04`), sin datos de ataques reales. **Se distingue el *intento* de su *resultado exitoso*:** cuando otro control debería detener el intento, ambos son amenazas distintas (AM-01, intento en TB-1, y AM-15, resultado en TB-4) y cada fila declara qué evento puntúa (§16.3.1).

**Probabilidad inherente, impacto inherente y condiciones propias del escenario (W-01).** **Probabilidad inherente** es la de que el evento descrito ocurra en el escenario **sin que ningún control —exigido, propuesto o no implementado— la reduzca**. **Impacto inherente** es el peor caso directo del evento, también sin controles. **Condiciones propias del escenario** son lo que define **quién es el actor y qué acceso tiene** (§16.2) y **qué datos existen y dónde** (§5.6, OFF-02, PRV-11): pueden justificar una probabilidad **Baja** solo si el evento exige un acceso previo o privilegiado que forma parte de la definición del actor. **Un control nunca reduce la probabilidad:** ni la lista blanca de PA-04, ni RT-09, ni RF-AUD-03, ni la segunda capa de RT-02. **Un evento que consiste en un defecto o fallo que viola una restricción de diseño es de probabilidad Media.** El riesgo inherente **no es el riesgo esperado**: mide la exposición antes de controles; el riesgo residual se estima después (R2-C y R4). §16.3.2 aplica esta convención a las 34 amenazas.

| Nivel | Criterio |
|---|---|
| **Baja** | Requiere un atacante dedicado o un acceso previo o privilegiado **que forma parte de la definición del actor** (§16.2; por ejemplo, ACT-10 tiene acceso técnico), o la coincidencia de varias condiciones **propias del escenario**. **Un control no cuenta como condición** |
| **Media** | Realizable por un usuario con cuenta o un atacante oportunista con herramientas comunes, o consecuencia previsible de un diseño sin control (**un defecto o fallo que viola una restricción de diseño**: un campo fuera de la lista blanca, un secreto en el cliente, un dato interno en un mensaje) |
| **Alta** | Realizable por cualquier usuario sin conocimientos especiales, o consecuencia común del uso (pérdida de un dispositivo, borrado por el navegador, datos públicos combinables) |

**Impacto cualitativo.** Se asigna por el **peor caso directo** de la amenaza y con la **misma regla** para todas las filas:

| Nivel | Criterio |
|---|---|
| **Bajo** | Molestia o corrección sencilla; sin datos personales ni pérdida de resultados |
| **Medio** | Datos preparados mínimos expuestos, pérdida de capturas recuperable o servicio degradado temporal |
| **Alto** | Resultados oficiales o clasificaciones alterados, acceso indebido a roles, a la auditoría o a datos internos de jugadores adultos, o pérdida de la trazabilidad |
| **Crítico** | El peor caso directo supone **(a)** exposición o identificación de **menores** (AS-04, incluidos sus respaldos AS-11), **(b)** toma de una cuenta de **dueño u organizador**, obtención de las facultades de un dueño u organizador, o toma de una credencial administrativa o de servicio (AS-09), o **(c)** pérdida del gobierno del sistema (AS-12) **que no cesa por sí sola**: solo se recupera con una intervención (por ejemplo, un bloqueo sin límite de la única cuenta de dueño, o la falta de sucesión de los accesos) |

**Convención «antes de controles» (V-01).** El impacto y la probabilidad se asignan al escenario **sin considerar ningún control de la fila, ni exigido ni propuesto**, porque **ninguno está implementado** (RC-22) y no se presupone que una protección existe. **Forman parte del escenario** y no atenúan: lo que R1 y `01` definen sobre **qué datos existen y en qué zona** (§5.1, §5.6, PA-04, PA-08, OFF-02, PRV-11) y lo que cada rol puede hacer legítimamente (`01 §7`). Se aplica a **todas** las filas: por ejemplo, AM-05 se evalúa sin el bloqueo temporal de PAR-02 y AM-32 sin la sucesión de RNF-17.

**Aplicación verificable de la regla.** Cada amenaza declara en §16.3.1 su peor caso directo y el criterio (a), (b) o (c) que cumple; el impacto es Crítico **si y solo si** cumple alguno. **Si una amenaza puede alcanzar datos de jugadores o de menores, AS-04 figura entre sus activos.** §16.3.2 hace lo mismo con la probabilidad. La tabla de §16.3.1 es una **comprobación de consistencia entre dos columnas del mismo autor**: detecta desajustes, **no valida que el peor caso esté bien escrito**; esa validación corresponde a una revisión independiente.

**Riesgo inherente** (antes de controles):

| Impacto \ Probabilidad | Baja | Media | Alta |
|---|---|---|---|
| Bajo | Bajo | Bajo | Medio |
| Medio | Bajo | Medio | Alto |
| Alto | Medio | Alto | Crítico |
| Crítico | Alto | Crítico | Crítico |

**Criterios de aceptación del riesgo** [REC]:

1. **Nadie acepta riesgos en este documento.** La aceptación es un acto **expreso y por escrito del usuario**, con motivo. **Existe hoy una sola aceptación expresa: RK-15** (`01`, RC-19), que cubre la pérdida de capturas legítimas cuando se rechaza una operación con autorización revocada (D-11). **HR1-10 (§11) no es una aceptación aparte:** es su consecuencia de diseño. **RK-11** (desalojo o limpieza del almacenamiento local) **no figura aceptado.**
2. Un riesgo **Crítico o Alto** no puede cerrarse sin un control (exigido o propuesto), un artefacto responsable y un **criterio verificable**: el caso de abuso asociado (§16.4, «caso de prueba futuro») o el criterio de aceptación del requisito en `01`. §16.11 verifica el documento, no los controles. Si el riesgo no puede eliminarse por diseño (HR1-11, HR1-12, HR1-14), queda como **riesgo residual no eliminable** (§16.10) hasta decisión del usuario o asesoramiento jurídico.
3. Un riesgo **Medio** exige un control o una aceptación explícita. Uno **Bajo** solo se registra.
4. **El riesgo residual se registra por riesgo y no por amenaza** (§16.10.1): solo se listan los que subsisten tras los controles o dependen de una decisión pendiente. Las demás amenazas figuran con su control y su responsable, y su riesgo residual **no se estima** en R2-A (se revisa en R2-C y R4).

### 16.2 Activos y actores de amenaza

**Activos**

| ID | Activo | Zona | Fuente |
|---|---|---|---|
| AS-01 | Cuentas y credenciales de usuario, incluido el segundo factor | Z2/Z3 | RF-ACC-10, §5.6 |
| AS-02 | Sesiones y nivel de autenticación | Z2 (y mínimo en Z0) | RF-ACC-16, OFF-23 |
| AS-03 | Roles, asignaciones y su historial (base de RT-14) | Z2/Z3 | RF-ACC-11 |
| AS-04 | Datos de jugadores y **menores** (nombre interno, indicador, metadatos de autorización) | Z2/Z3 | PRV-02…09, §5.6 |
| AS-05 | Resultados oficiales y clasificaciones (integridad) | Z2/Z3 | RNF-06, CL-15 |
| AS-06 | Operaciones locales, cola y datos preparados en el dispositivo | Z0 | OFF-03, OFF-12 |
| AS-07 | Auditoría, historial de solo anexado y bitácora de seguridad | Z3 | RF-AUD-01…06 |
| AS-08 | Visibilidad y proyecciones públicas (lo no publicado) | Z2/Z3/Z1 | RF-PUB-03, PRV-03 |
| AS-09 | Credenciales de servicio de la aplicación | Z2 | RT-09 |
| AS-10 | Direcciones de correo y mensajes transitorios (invitación, recuperación) | Z2/Z4 | PRV-14, RF-ACC-02 |
| AS-11 | Respaldos | Z3 | RNF-08, TB-9 |
| AS-12 | Continuidad del gobierno (al menos un dueño activo, accesos administrativos con sucesión) | Z2/Z3 | RT-06, RF-ACC-09, RNF-17 |

**Actores de amenaza** (los actores y roles son los de §3.1 y `01 §7.3`; aquí se describe su **capacidad de abuso**, sin conceder permisos):

| ID | Actor | Capacidad considerada |
|---|---|---|
| ACT-01 | Visitante anónimo malicioso | Solicitudes públicas arbitrarias, enumeración, carga |
| ACT-02 | Usuario autenticado sin rol | Lo anterior más una sesión válida sin permisos |
| ACT-03 | Editor (malicioso o negligente) | Captura en partidos asignados, cola local |
| ACT-04 | Delegado | Administra la plantilla de su equipo; ve datos internos de su equipo (PRV-09) |
| ACT-05 | Organizador que intenta escalar | Invitar, asignar, gobernar sus torneos |
| ACT-06 | Dueño u organizador con rol de delegado (conflicto, RT-14) | Operaciones con efecto sobre su propio equipo |
| ACT-07 | Persona con acceso físico al dispositivo | Robo, pérdida, préstamo, uso compartido, limpieza de datos |
| ACT-08 | Atacante externo de red o ingeniería social | Phishing, relleno de credenciales, interceptación |
| ACT-09 | Servicio auxiliar comprometido o indiscreto | Correo, monitoreo, programador (Z4) |
| ACT-10 | Persona con acceso técnico o administrativo | Desarrollador o administrador de infraestructura; límite de RF-AUD-03 |
| ACT-11 | Error operativo no malicioso | Borrado, configuración errónea, olvido de una credencial |

### 16.3 Modelo inicial de amenazas por frontera de confianza (TB-1…TB-9)

Columnas: **Cat.** categoría (§16.1) · **P/I** probabilidad e impacto · **R** riesgo inherente · **Exigidos** controles con fuente en `01` · **Propuestos [REC]** controles sin fuente en `01` · **Estado** (§16.1: ninguna mitigación está implementada) · **Casos** de abuso (§16.4; la relación es simétrica con la columna «Amenazas» de §16.4) · **Responsable documental**: artefacto o bloque que debe tratarla, **no equivale a que ya esté tratada**.

**Orden de lectura.** AM-01 y AM-15 no se duplican: AM-01 es el **intento** en TB-1 y AM-15 la **consecuencia** en TB-4. **Antes de controles, ambas son de probabilidad Alta:** sin la segunda capa de RT-02, todo intento que supere la aplicación alcanza la capa de datos. La v0.4 puntuaba AM-15 como Media porque RT-02 exige ambas capas; eso era un control que atenuaba la probabilidad y **se corrige** (W-01). La distinción entre ambas sirve para ubicar **dónde se exige cada control**, no para reducir la probabilidad.

**TB-1 · Dispositivo ↔ aplicación**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-01 | Elevación | **Intento:** manipular rol, torneo, usuario o equipo en la solicitud para obtener un permiso (el resultado exitoso es AM-15; **priorización pendiente, H2A-12**) | AS-03, AS-04, AS-05 · ACT-02…05 | Alta/Crítico | Crítico | RC-04 y RT-02 (servidor **y** capa de datos); RF-ACC-15 (ningún rol se modifica por parámetros del cliente) | — | Exigido | CAB-01, CAB-02 | **R2-B** (ADR-02.C) |
| AM-02 | Suplantación | Toma de una cuenta con rol privilegiado (dueño, organizador) o con acceso a datos internos de menores (delegado) por phishing, relleno de credenciales o recuperación abusiva | AS-01, AS-04, AS-12 · ACT-08, ACT-11 | Media/Crítico | Crítico | RF-ACC-10 (2FA obligatorio para dueño y organizador; límites de intentos); RF-ACC-09 (nueva autenticación con 2FA para designar o revocar un dueño; procedimientos de recuperación); RNF-05 | IN-03-14, IN-03-15, IN-03-16, IN-03-21 | Exigido + [REC] | CAB-09 | **ADR-03.C**. El 2FA del delegado es opcional por decisión de `01` (Q-39): RES-10 |
| AM-03 | Suplantación | Reutilizar una sesión robada, o seguir usando una tras revocar o cambiar la contraseña (peor caso: sesión de dueño u organizador) | AS-02 · ACT-08, ACT-07 | Media/Crítico | Crítico | RF-ACC-07 (revocación inmediata en el servidor); RF-ACC-16 (invalidación al revocar y al cambiar la contraseña) | IN-03-12 | Exigido + [REC] | CAB-19 | **ADR-03.C** |
| AM-04 | Manipulación | Repetir o reordenar operaciones enviadas | AS-05 · ACT-03, ACT-08 | Media/Alto | Alto | OFF-06 (idempotencia); OFF-07 y OFF-25 (orden y contigüidad); RT-13 | — | Exigido | CAB-04 | **R2-C** (ADR-05.C) |
| AM-05 | Denegación | Inundar el acceso, las invitaciones o la sincronización; bloquear una cuenta ajena —incluido el **único dueño**— abusando del límite de intentos. **Antes de controles el bloqueo no tiene límite** (PAR-02, que lo acota, es una propuesta) | AS-01, AS-12 · ACT-01, ACT-08 | Media/Crítico | Crítico | RNF-05 (límites de frecuencia en autenticación e invitaciones); RF-ACC-09 (procedimientos de recuperación de cuenta perdida). **RT-06 no lo cubre:** solo impide revocar o degradar al último dueño | PAR-02 (bloqueo temporal y nunca permanente) | Exigido + [REC] | CAB-10, CAB-15 | **ADR-03.C** |
| AM-06 | Divulgación | Averiguar si una cuenta existe por mensajes de error distintos | AS-01 · ACT-01, ACT-08 | Media/Medio | Medio | — (`01` no lo exige) | IN-03-13 | Propuesto [REC] | CAB-15 | **ADR-03.C** |
| AM-07 | Manipulación | Hacer que el navegador de la víctima —con una sesión **privilegiada** (dueño u organizador)— emita una solicitud con efecto desde un origen ajeno, por ejemplo para invitar o asignar un rol a la cuenta del atacante. **Solo aplica si el mecanismo de sesión envía la credencial de forma automática: depende de ADR-03.I; IN-03-12 [REC] favorece este caso** | AS-05, AS-03, AS-01 · ACT-08 | Media/Crítico | Crítico | — (RNF-05 no lo menciona) | Verificación de intención y de origen en toda solicitud con efecto (referencia de seguridad de la skill) | Propuesto [REC] | CAB-23 | **ADR-03.I** (R3) |
| AM-34 | Manipulación / Divulgación | **Inyección de contenido** (marcado o scripts) mediante campos de texto escritos por usuarios (nombres de equipos, jugadores y torneos; motivos) que se muestra en vistas públicas (TB-3), en el panel (TB-1), en correos (TB-5) o en las proyecciones (TB-8): robo de sesión, alteración de las vistas o suplantación | AS-02, AS-08, AS-01 · ACT-02…05, ACT-08 | Media/Crítico | Crítico | RNF-05 (cabeceras de seguridad) | Validación de la entrada y **escape según el contexto de salida** (público, panel, correo); IN-03-12 (el secreto de sesión no es legible por scripts) | Exigido + [REC] | CAB-25 | **R2-B**; **R3** (cabeceras); **Fase 4** |

**TB-2 · Dispositivo ↔ almacén local**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-08 | Divulgación | Robo, pérdida o préstamo del dispositivo con cola y datos preparados | AS-06 · ACT-07 | Alta/Medio | Alto | OFF-12 y PRV-11 (solo datos mínimos, sin datos internos); revocación de la cuenta por dueño u organizador (OFF-12); **el cifrado local es una pregunta abierta de ADR-16.C** | — | Exigido | CAB-08 | **R2-C** (ADR-16.C) |
| AM-09 | Elevación | Usar el contexto local (sesión vencida o copiada) para actuar sin autorización | AS-02, AS-05 · ACT-07, ACT-03 | Media/Alto | Alto | PA-01, PA-05 y RC-04 (lo local no autoriza); OFF-22 y OFF-23 (se rechaza lo sincronizado sin sesión y permisos vigentes): IN-03-10, IN-03-11 | — | Exigido | CAB-05, CAB-06 | **ADR-03.C** |
| AM-10 | Manipulación | Editar la cola local o el reloj del dispositivo para alterar el orden o la hora | AS-05 · ACT-03, ACT-07 | Media/Alto | Alto | OFF-13 (el orden depende de la secuencia y no del reloj; aviso si la diferencia supera 5 min); RC-08 | — | Exigido | CAB-16 | **R2-C** (ADR-05.C) |
| AM-11 | Denegación | El navegador elimina los datos locales y se pierden capturas **[NO VERIF]** IOS-01, IOS-04, IOS-05 | AS-06 · ACT-11 | Alta/Medio | Alto | OFF-17 y RC-24 (advertencia, solicitud de almacenamiento persistente, sin promesa de conservación); RK-11 en `01` | — | Exigido | CAB-08 | **R2-C** (ADR-05.C, ADR-16.C); RES-07 |
| AM-33 | Suplantación | Sincronizar la cola de otra cuenta, o atribuir las operaciones capturadas por una cuenta a la sesión de otra, con un dispositivo compartido o prestado | AS-06, AS-05, AS-02 · ACT-07, ACT-03 | Media/Alto | Alto | OFF-03 (cada operación guarda su usuario); OFF-22 (autorización por los roles de la cuenta); OFF-12 (cerrar sesión con pendientes exige confirmación y borra los datos locales tras sincronizar o descartar) | IN-03-19, IN-03-20 | Exigido + [REC] | CAB-30 | **ADR-03.C**; **R2-C** |

**TB-3 · Público ↔ borde público**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-12 | Divulgación | Enumerar o acceder a torneos, borradores y jornadas no publicados (los datos internos de jugadores y menores no forman parte de la proyección pública y su fuga es AM-13: AS-04 no figura aquí, §16.3.1) | AS-08 · ACT-01, ACT-02 | Media/Alto | Alto | PA-04 (solo proyecciones públicas); `01 §9.6` (visibilidad) | — | Exigido | CAB-03 | **R2-C** (ADR-04.C) |
| AM-13 | Divulgación | **Fuga técnica** de datos internos de menores o de datos no públicos por la proyección pública (un campo fuera de la lista blanca, una etiqueta mal calculada) | AS-04, AS-08 · ACT-01 | Media/Crítico | Crítico | PRV-02 y PRV-03 (nunca públicos); lista blanca y etiquetas calculadas en el servidor (PA-04) | — | Exigido | CAB-11, CAB-18 | **R2-C** (ADR-04.C) |
| AM-31 | Divulgación | **Identificar a un menor** por la combinación de equipo, dorsal, minuto, estadísticas, calendario o resúmenes exportados, **aunque no se publique su nombre** | AS-04, AS-08 · ACT-01, ACT-02 | Alta/Crítico | Crítico | PRV-02 (etiqueta por defecto); PRV-03; PRV-18 (evaluación con asesoría jurídica antes del lanzamiento; `01` la deja abierta); Q-20 (campos públicos, abierta) | — | Exigido | CAB-12 | **R2-C** (ADR-04.C); asesoría jurídica; RES-08 |
| AM-14 | Denegación | Sobrecargar la lectura pública | AS-08 · ACT-01 | Media/Medio | Medio | RNF-03, RNF-04, RNF-15 (degradación explícita, capacidad, red lenta) | — | Exigido | CAB-26 | **R3** (ADR-06, ADR-07) |

**TB-4 · Aplicación ↔ datos**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-15 | Elevación / Divulgación | Eludido el control de la aplicación (AM-01), escalar privilegios o acceder a datos de otro torneo —incluidos datos internos de menores— porque la capa de datos tampoco lo impide | AS-03, AS-04, AS-05 · ACT-02…05 | Alta/Crítico | Crítico | RT-08 (toda referencia pertenece al mismo torneo); RT-01 y RT-02 (capa de datos) | — | Exigido | CAB-01, CAB-07 | **R2-B** (ADR-02.C) |
| AM-16 | Manipulación | Alterar o borrar registros de auditoría o del historial de roles | AS-07, AS-03 · ACT-05, ACT-10 | Media/Alto | Alto | RF-AUD-03 y RC-21 (solo anexado); RF-ACC-11 (el historial no se altera): IN-09-01, IN-09-02 | — | Exigido | CAB-13 | **ADR-09.C** |
| AM-17 | Repudio | Una acción aplicada sin registro, o un rechazo sin registro | AS-07 · ACT-03…06 | Media/Alto | Alto | RF-AUD-02 (mutaciones listadas); RT-10 (permisos auditados); RF-AUD-06 (rechazos) y su persistencia independiente **[DER]**: IN-09-04, IN-09-05 | — | Exigido | CAB-07, CAB-14 | **ADR-09.C** |
| AM-18 | Manipulación | Carrera entre una revocación o reasignación y una operación en curso | AS-03, AS-05 · ACT-03, ACT-06 | Media/Alto | Alto | RT-13 (autorización evaluada en la misma transacción que aplica); UI-11 | — | Exigido | CAB-05 | **R2-B** (ADR-02.C) |
| AM-19 | Manipulación / Divulgación | Entradas externas que alteran consultas o datos (inyección en la capa de datos) | AS-05, AS-04 · ACT-01…05 | Media/Crítico | Crítico | — (`01` no exige la validación de entradas: RNF-06 trata la integridad frente a la concurrencia, el reintento y la desconexión, y las restricciones críticas en la base de datos, **no la inyección**) | Validación en el límite de confianza y consultas parametrizadas (referencia de seguridad de la skill) | Propuesto [REC] | CAB-24 | **R2-B**; **Fase 4** |

**TB-5 · Aplicación ↔ correo transaccional y tareas programadas**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-20 | Suplantación | Interceptar, reenviar o usar un correo comprometido para aceptar una invitación o recuperar una cuenta | AS-10, AS-01 · ACT-08, ACT-09 | Media/Crítico | Crítico | RF-ACC-02 (un solo uso, vencimiento, correo coincidente, rol y ámbito inmutables) | IN-03-15, IN-03-21; PAR-04 | Exigido + [REC] | CAB-09 | **ADR-03.C**; ADR-15 (R3) |
| AM-21 | Divulgación | Datos de jugadores o menores en mensajes o en el programador | AS-04, AS-10 · ACT-09 | Media/Crítico | Crítico | §5.1 y §5.6 (Z4 no recibe datos de jugadores ni de menores); PRV-14 | — | Exigido | CAB-11 | **R3** (ADR-15); **R4** (ADR-14) |
| AM-22 | Elevación | Tarea programada sin identidad de ejecución definida que actúa con más privilegio o evita RT-14 | AS-03, AS-05 · ACT-09, ACT-11 | Media/Alto | Alto | RT-14 (lo automático hereda la situación del actor que dispara); la identidad sin actor es una pregunta abierta de ADR-14 | IN-09-09 (se audita con su origen) | Exigido + [REC] | CAB-27 | **R4** (ADR-14); **ADR-09.C** (registro) |

**TB-6 · Organización ↔ desarrollador**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-23 | Suplantación / Elevación | **Compromiso de una credencial administrativa o técnica** (cuenta de la organización en la plataforma, acceso del desarrollador, claves de servicio) | AS-09, AS-11, AS-12 · ACT-10, ACT-08 | Media/Crítico | Crítico | RNF-14 (credenciales individuales y revocables del desarrollador); RC-18 (cuentas a nombre de la organización) | Segundo factor para las cuentas administrativas de la plataforma y revocación inmediata del acceso al terminar la relación (sin elegir mecanismo) | Exigido + [REC] | CAB-21 | **R3** (ADR-01); **R4** (ADR-11). No es de ADR-03.C |
| AM-32 | Denegación | **Falta de sucesión:** salida, indisponibilidad o pérdida de las personas con accesos administrativos y técnicos, sin inventario ni sucesión; peor caso: nadie puede administrar la plataforma ni recuperar la cuenta del dueño | AS-12, AS-09 · ACT-10, ACT-11 | Media/Crítico | Crítico | RNF-17 (manual de operación y sucesión de accesos); RNF-14 (inventario de cuentas y administradores, firmado en la Fase 8); RF-ACC-09 (segundo dueño y procedimientos de recuperación). **RT-06 no lo cubre:** solo impide revocar o degradar al último dueño, no su ausencia o pérdida | — | Exigido | CAB-22 | **R4** (ADR-11, ADR-12); **Fase 8** |
| AM-24 | Manipulación / Repudio | Alteración o borrado de la auditoría por quien tiene acceso técnico, fuera de la aplicación | AS-07 · ACT-10 | Baja/Alto | Medio | RF-AUD-03 (se documenta el límite frente al administrador de la base de datos): IN-09-03 | CAP-09-M2 y CAP-09-M3 (detección de alteración, copia externa) | Exigido + [REC] | CAB-13 | **ADR-09.C**; ADR-09.I (R3) |

**TB-7 · Dispositivo ↔ observabilidad**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-25 | Divulgación | Datos personales —incluidos los de menores—, tokens o secretos en trazas, capturas de error o registros | AS-04, AS-02 · ACT-09 | Media/Crítico | Crítico | PRV-12 y RNF-09 (registros y monitoreo sin datos personales) | Lo que no debe registrarse (§16.7.5) | Exigido + [REC] | CAB-11, CAB-20 | **R4** (ADR-13); **ADR-09.C** (lo que la auditoría no contiene) |
| AM-26 | Divulgación | Credenciales de servicio en el cliente, la cola o la telemetría | AS-09 · ACT-09, ACT-10 | Media/Crítico | Crítico | RT-09 (nunca llegan al navegador ni a la cola local) | — | Exigido | CAB-20 | **R3** (ADR-01); **Fase 7** |

**TB-8 · Aplicación ↔ borde público (proyecciones y señales de cambio)**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-27 | Manipulación / Divulgación | Contaminar la entrega pública con datos no públicos —incluidos los datos internos de jugadores y de menores que PRV-03 prohíbe publicar— o con un estado incoherente entre resultados y clasificación | AS-08, AS-05, AS-04 · ACT-11, ACT-05 | Media/Crítico | Crítico | PA-04 (lista blanca); PRV-03; garantía exigida de coherencia (§5.5, CL-15) | — | Exigido | CAB-28 | **R2-C** (ADR-04.C); **R3** (ADR-06) |
| AM-28 | Divulgación | Señal de cambio omitida o tardía tras revocar la autorización del nombre público, o tras ocultar o anonimizar: el nombre de un menor sigue visible en copias | AS-04, AS-08 · ACT-11 | Media/Crítico | Crítico | PRV-06 (ocultar y anonimizar); PRV-02; HR1-11 (no hay plazo definido y **no se promete uno**) | — | Exigido (el requisito carece de mecanismo y plazo; no eliminable por diseño) | CAB-29 | **R2-C**; **R3** (ADR-06); RES-02 |

**TB-9 · Datos ↔ almacenamiento de respaldos**

| ID | Cat. | Amenaza | Activos · Actores | P/I | R | Exigidos (fuente) | Propuestos [REC] | Estado | Casos | Responsable |
|---|---|---|---|---|---|---|---|---|---|---|
| AM-29 | Divulgación | Restaurar o copiar un respaldo en un entorno no protegido | AS-11, AS-04 · ACT-10, ACT-11 | Baja/Crítico | Alto | §5.6 y TB-9 (mismo nivel de protección que Z3); PRV-17 (datos sintéticos fuera de producción) | — | Exigido | CAB-17 | **R4** (ADR-12, ADR-11) |
| AM-30 | Divulgación | Los respaldos conservan datos anteriores a una anonimización y sin plazo definido (HR1-12) | AS-11, AS-04 · ACT-10 | Media/Crítico | Crítico | PRV-06 (ningún campo personal queda tras anonimizar); `01` no fija la retención de los respaldos (PEND-JUR) | — | Exigido (el requisito carece de mecanismo y plazo; no eliminable por diseño) | CAB-17 | **R4** (ADR-12, ADR-17); RES-03 |

**Resumen de §16.3** (cifras comprobables en §16.12): 34 amenazas; riesgo inherente **antes de controles**: 19 Críticas, 12 Altas, 3 Medias y 0 Bajas; por frontera TB-1…TB-9: 8, 5, 4, 5, 3, 3, 2, 2, 2; estado de los controles: 21 «Exigido», 10 «Exigido + [REC]» y 3 «Propuesto [REC]»; 0 «Implementado».

**Reevaluación.** *v0.2 (A-02):* con la regla única de impacto de §16.1 pasaron a Crítico de impacto AM-03, AM-15, AM-19, AM-20, AM-25, AM-28 y AM-30 (y AM-34, nueva); AM-13 conserva Baja probabilidad **solo para la fuga técnica** y la identificación indirecta, que `01` declara no resuelta (PRV-18), se separó como **AM-31** con probabilidad Alta. *v0.3 (R-01, R-02):* **AM-32 pasa a Crítico** (criterio (c): la falta de sucesión es una pérdida del gobierno que no es temporal por sí misma) y **AM-27 incorpora AS-04 y pasa a impacto Crítico** (probabilidad Baja, riesgo Alto); AM-12 mantiene su impacto con la justificación de §16.3.1. *v0.4 (V-01):* se aplica **una sola convención «antes de controles»** (§16.1) a todas las filas: **AM-05 pasa a Crítico** (el bloqueo del único dueño no tiene límite sin PAR-02, que es propuesta; criterio (c)), **AM-07 pasa a Crítico** (una sesión privilegiada falsificada puede asignar facultades al atacante; criterio (b)), **AM-01 pasa a impacto Crítico** (alineada con CAB-02) y **AM-10 pasa a Alto** (sin la revalidación del servidor la alteración llega al resultado oficial); AM-32 se mantiene Crítica con la misma convención. Los cambios **elevan** la criticidad por coherencia; no se modificó ninguna clasificación para reducir el número de Críticas. **La clasificación es proporcional al peor caso escrito, pero la priorización entre las Críticas queda registrada sin resolver (H2A-12, decisión 9).**

**Reevaluación (v0.5, W-01).** Se justifica la probabilidad de cada amenaza sin controles (§16.3.2): **AM-13, AM-21, AM-26 y AM-27 pasan de Baja a Media** (defecto previsible que viola una restricción de diseño) y quedan **Críticas**; **AM-16 pasa a Media** y a riesgo Alto; **AM-15 pasa a Alta** (su riesgo ya era Crítico); **AM-24 y AM-29 permanecen en Baja** por una condición propia del escenario (acceso técnico previo). Los cambios aplican la convención uniforme y **no se hicieron para aumentar el número de Críticas**, que resulta de ella.

#### 16.3.1 Justificación del impacto de cada amenaza (R-01, R-02)

Cada amenaza declara **qué evento puntúa**, su **peor caso directo** —**antes de controles**, según la convención de §16.1— y el **criterio** que cumple. El impacto es **Crítico si y solo si** cumple (a), (b) o (c). **Esta tabla es una comprobación de consistencia entre dos columnas del mismo autor: detecta desajustes entre el peor caso, el criterio y la columna P/I, pero no valida que el peor caso esté bien escrito;** esa validación corresponde a una revisión independiente. **Si una amenaza puede alcanzar datos de jugadores o de menores, AS-04 figura entre sus activos.** **Intento** = el evento puntuado es el intento, cuyo éxito exige que otro control falle y se modela aparte (AM-01 → AM-15); **Resultado** = el evento es el efecto logrado.

| ID | Evento puntuado | Peor caso directo | Criterio | Impacto |
|---|---|---|---|---|
| AM-01 | **Intento** (el resultado es AM-15) | **Sin controles de autorización**, quien manipula la solicitud obtiene facultades de dueño u organizador —por ejemplo, un organizador se concede el rol de dueño (CAB-02)— y con ellas el acceso a los datos internos de todos los torneos, incluidos los de menores | (a), (b) | Crítico |
| AM-02 | Resultado | Control de una cuenta de dueño u organizador; con delegados, datos internos de menores | (b) | Crítico |
| AM-03 | Resultado | Uso de la sesión de un dueño u organizador | (b) | Crítico |
| AM-04 | Resultado | Resultado oficial alterado por repetición o reordenamiento | — | Alto |
| AM-05 | Resultado | **Sin ningún límite que lo acote** (ni siquiera PAR-02, que es propuesta), el bloqueo repetido de la cuenta del **único dueño** no cesa por sí solo: la gobernanza queda sin cuenta administrativa hasta que alguien con acceso técnico intervenga | (c) | Crítico |
| AM-06 | Resultado | Se averigua qué cuentas existen | — | Medio |
| AM-07 | Resultado | Con la sesión de un dueño u organizador, una solicitud falsificada puede invitar o asignar un rol de organizador o de dueño a la cuenta del atacante y con ello el control de la gobernanza (sin presuponer la reautenticación de IN-03-04 ni ningún otro control) | (b) | Crítico |
| AM-08 | Resultado | Los datos preparados que OFF-02 y PRV-11 definen como mínimos (etiqueta o nombre público y dorsal; son parte del escenario, no un control) en un dispositivo ajeno | — | Medio |
| AM-09 | Resultado | Sin comprobación del servidor, acciones de captura sin autorización vigente alteran resultados oficiales | — | Alto |
| AM-10 | Resultado | Sin revalidación en el servidor, operaciones con orden u hora alterados alteran el resultado oficial de los partidos del editor | — | Alto |
| AM-11 | Resultado | Pérdida de las capturas pendientes del dispositivo; los resultados oficiales ya confirmados no se alteran | — | Medio |
| AM-12 | Resultado | Se revelan torneos, borradores y jornadas **no publicados**. Los datos internos de jugadores y de menores no forman parte de la proyección pública por diseño (PA-04, PRV-03) y su fuga es AM-13: por eso AS-04 no figura aquí. Si ADR-04.C admitiera datos de jugadores en lo no publicado, se reevaluaría en R2-C | — | Alto |
| AM-13 | Resultado | Un campo interno o una etiqueta de menores llega a la proyección pública | (a) | Crítico |
| AM-14 | Resultado | Degradación de la lectura pública | — | Medio |
| AM-15 | Resultado | Acceso a datos de otro torneo, incluidos datos internos de menores | (a) | Crítico |
| AM-16 | Resultado | Pérdida de la trazabilidad (auditoría alterada) | — | Alto |
| AM-17 | Resultado | Acción o rechazo sin registro (pérdida de trazabilidad) | — | Alto |
| AM-18 | Resultado | Operación aplicada con una autorización ya revocada | — | Alto |
| AM-19 | Resultado | Extracción o alteración de datos mediante consultas, incluidos los de menores | (a) | Crítico |
| AM-20 | Resultado | Control de una cuenta, también de dueño u organizador, mediante un correo comprometido | (b) | Crítico |
| AM-21 | Resultado | Datos de menores en mensajes o en el programador | (a) | Crítico |
| AM-22 | Resultado | Tarea sin identidad que actúa con más privilegio o evita RT-14 | — | Alto |
| AM-23 | Resultado | Control de una credencial administrativa, técnica o de servicio | (b) | Crítico |
| AM-24 | Resultado | Auditoría alterada por quien tiene acceso técnico | — | Alto |
| AM-25 | Resultado | Datos de menores o secretos en trazas | (a) | Crítico |
| AM-26 | Resultado | Credenciales de servicio expuestas (acceso total) | (b) | Crítico |
| AM-27 | Resultado | La entrega pública recibe datos no públicos, incluidos los internos de jugadores y menores que PRV-03 prohíbe publicar (TB-8: lo que se alimenta a la entrega puede contener cualquier dato) | (a) | Crítico |
| AM-28 | Resultado | El nombre de un menor sigue visible tras ocultarlo o anonimizarlo | (a) | Crítico |
| AM-29 | Resultado | Respaldo con datos de menores fuera de un entorno protegido | (a) | Crítico |
| AM-30 | Resultado | Respaldos con nombres de menores anteriores a una anonimización | (a) | Crítico |
| AM-31 | Resultado | Un menor es identificado por la combinación de datos públicos | (a) | Crítico |
| AM-32 | Resultado | **Nadie puede administrar la plataforma ni recuperar la cuenta del dueño** mientras no se recuperen los accesos: pérdida del gobierno que **no es temporal** por sí misma, porque depende de que alguien con autoridad recupere los accesos | (c) | Crítico |
| AM-33 | Resultado | Operaciones de una cuenta sincronizadas por otra; alcance de un editor | — | Alto |
| AM-34 | Resultado | Robo de la sesión de una cuenta privilegiada o suplantación en vistas y correos | (b) | Crítico |

#### 16.3.2 Justificación de la probabilidad de cada amenaza (W-01)

La probabilidad se asigna **sin considerar ningún control** (§16.1). Solo se justifica una **Baja** con una **condición propia del escenario** (acceso previo o privilegiado que forma parte de la definición del actor); **un control nunca la justifica**. **Las probabilidades [SUP] siguen siendo juicio del autor sin datos empíricos, y el recuento de Críticas es sensible a esta convención** (H2A-12): si se puntuara la probabilidad condicionada al fallo de los controles, habría menos; **esa lectura no se adopta**, por contradecir «antes de controles».

| ID | Probabilidad | Justificación (sin controles) |
|---|---|---|
| AM-01 | Alta | Cualquier usuario puede intentar manipular una solicitud; no exige acceso previo |
| AM-02 | Media | Phishing y relleno de credenciales están al alcance de un atacante oportunista |
| AM-03 | Media | Reutilizar una sesión robada es realizable con herramientas comunes |
| AM-04 | Media | Reenviar o reordenar solicitudes está al alcance de un usuario con cuenta |
| AM-05 | Media | Inundar el acceso o provocar bloqueos es realizable por un atacante oportunista |
| AM-06 | Media | Comparar las respuestas del acceso es trivial con herramientas comunes |
| AM-07 | Media | Si el mecanismo de sesión envía la credencial de forma automática, inducir una solicitud es realizable con ingeniería social; **la condición es propia del escenario y depende de ADR-03.I** |
| AM-08 | Alta | La pérdida o el préstamo de un dispositivo es consecuencia común del uso |
| AM-09 | Media | Exige acceso al dispositivo o a la sesión local de un editor (condición del actor) |
| AM-10 | Media | Editar la cola o el reloj de un dispositivo propio está al alcance de un editor |
| AM-11 | Alta | El borrado de datos locales por el navegador es consecuencia común del uso |
| AM-12 | Media | Enumerar con solicitudes públicas está al alcance de un atacante oportunista |
| AM-13 | Media | Un campo o una etiqueta que viola la restricción de la proyección es un defecto previsible en un diseño sin control |
| AM-14 | Media | La carga masiva de lectura pública está al alcance de un atacante oportunista |
| AM-15 | Alta | Sin la segunda capa, todo intento de AM-01 que supere la aplicación alcanza la capa de datos; **la v0.4 la puntuaba Media apoyándose en RT-02, un control: se corrige** |
| AM-16 | Media | Un defecto de la aplicación o un organizador malicioso puede alterar registros si nada lo impide; la restricción de solo anexado de RF-AUD-03 es un control y no cuenta |
| AM-17 | Media | Provocar rechazos o reversiones es posible para un usuario con cuenta |
| AM-18 | Media | Una carrera entre una revocación y una operación ocurre por simultaneidad ordinaria |
| AM-19 | Media | Entradas manipuladas son realizables con herramientas comunes |
| AM-20 | Media | Un correo comprometido o reenviado es realizable por un atacante oportunista |
| AM-21 | Media | Un dato interno en un mensaje o en el programador viola la restricción de §5.1 y §5.6: defecto previsible en un diseño sin control |
| AM-22 | Media | Una tarea sin identidad definida actúa con la identidad que se le dé: configuración errónea previsible |
| AM-23 | Media | Credenciales administrativas comprometidas por phishing o filtración: realizable por un atacante oportunista |
| AM-24 | Baja | **Condición propia del escenario:** exige acceso técnico previo a la infraestructura, que es la definición de ACT-10 |
| AM-25 | Media | Trazas y capturas de error con datos personales son un defecto previsible sin revisión |
| AM-26 | Media | Un secreto en el cliente, la cola o la telemetría es un defecto previsible en un diseño sin control; RT-09 es un control y no cuenta |
| AM-27 | Media | Una proyección que recibe datos no públicos viola PA-04: defecto previsible en un diseño sin control |
| AM-28 | Media | Una señal omitida o tardía es previsible sin mecanismo de entrega (HR1-11) |
| AM-29 | Baja | **Condición propia del escenario:** exige acceso técnico previo a los respaldos y una restauración fuera de un entorno controlado (ACT-10; ACT-11 solo si ya tiene ese acceso) |
| AM-30 | Media | Los respaldos conservan datos previos por construcción (HR1-12); el incumplimiento de PRV-06 es previsible |
| AM-31 | Alta | Cualquier asistente puede combinar equipo, dorsal y minuto sin ningún ataque |
| AM-32 | Media | La salida o la indisponibilidad de una persona es habitual en una organización pequeña |
| AM-33 | Media | Un dispositivo compartido o prestado es común; exige acceso al dispositivo (condición del actor) |
| AM-34 | Media | Los campos de texto con contenido activo son realizables con herramientas comunes |

### 16.4 Casos de abuso

«Resultado esperado» es lo que el sistema **debe** hacer ante el abuso. Lo que sale de un requisito aprobado se cita en «Requisitos»; lo que **no** tiene fuente en `01` va marcado **[REC]**. **No se ejecutó ninguna prueba.** «Caso de prueba futuro» remite a casos de `01 §12` o a criterios de aceptación de `01` que ya figuran en este documento; la lista completa es de la Fase 4 y 7. La relación con §16.3 es **simétrica**: cada caso cita las amenazas que cada amenaza cita como «Casos».

| ID | Actor | Abuso | Amenazas | Resultado esperado | Requisitos | Caso de prueba futuro | Responsable |
|---|---|---|---|---|---|---|---|
| CAB-01 | ACT-02…05 | Cambiar rol, torneo, usuario o equipo en la solicitud, o acceder al recurso de otro torneo | AM-01, AM-15 | Denegación en el 100 % de los casos probados; sin datos de otro torneo | RF-ACC-15, RT-02, RT-08 | CL-21 | R2-B |
| CAB-02 | ACT-05, ACT-03 | Invitarse o asignarse un rol a sí mismo; un organizador concede el rol de dueño o crea organizadores | AM-01 | Denegado en interfaz, API y capa de datos; registrado en la bitácora de seguridad (prioridad S) | RT-03, RT-04, RF-ACC-03, RF-ACC-15, RF-AUD-05 | CL-19, CL-38 | R2-B; ADR-09.C |
| CAB-03 | ACT-01 | Enumerar torneos o jornadas no publicados | AM-12 | Solo lo publicado es visible | PA-04, `01 §9.6` | CL-25 | R2-C |
| CAB-04 | ACT-03, ACT-08 | Reenviar la misma operación o enviarlas desordenadas | AM-04 | Un efecto, un registro, misma respuesta; secuencia contigua | OFF-06, OFF-07, OFF-25 | N-13 | R2-C |
| CAB-05 | ACT-03, ACT-06 | Sincronizar tras ser revocado, desasignado o reasignado, o durante la carrera con esa revocación | AM-09, AM-18 | Rechazada y conservada según OFF-10; nunca «a medias» | OFF-11, OFF-22, RT-13, UI-11 | CL-45 | R2-B; R2-C; ADR-03.C |
| CAB-06 | ACT-03, ACT-04 | Capturar un partido del propio equipo (editor con rol de delegado) mediante la cola local | AM-09 | Rechazada al sincronizar | RT-05, OFF-22 | CL-35 | R2-B |
| CAB-07 | ACT-06 | Operar sobre su propio equipo por efecto indirecto o en cascada | AM-15, AM-17 | Rechazo de la operación completa; bloqueo y auditoría si no hay otro actor | RT-14, RF-AUD-06 | CL-51 | R2-B; ADR-09.C |
| CAB-08 | ACT-07 | Usar un dispositivo robado, perdido o prestado, o perder capturas por borrado del navegador | AM-08, AM-11 | Datos mínimos sin internos; la cuenta se revoca; advertencia de no conservación | OFF-12, OFF-17, PRV-11 | Inspección del almacenamiento local en pruebas: ningún campo interno (OFF-12, PRV-11) | R2-C; ADR-03.C |
| CAB-09 | ACT-08 | Tomar la cuenta de un dueño u organizador (phishing, relleno de credenciales, correo comprometido, recuperación) | AM-02, AM-20 | Sin 2FA no accede el dueño u organizador; **[REC]** la recuperación no es más débil que la autenticación que sustituye (IN-03-15) y el titular recibe un aviso (IN-03-21) | RF-ACC-10, RF-ACC-09, RF-ACC-02 | «5 intentos fallidos activan el límite» (RF-ACC-10) | ADR-03.C |
| CAB-10 | ACT-11, ACT-08 | Perder el segundo factor, o quedarse sin dueño activo por un bloqueo de cuenta | AM-05 | Existe un procedimiento documentado de recuperación (RF-ACC-09). **RT-06 no cubre este caso:** solo impide revocar o degradar al último dueño | RF-ACC-09, CL-28 | «Runbook de recuperación revisado» (RF-ACC-09). **CL-28** («Revocar al último dueño activo; pérdida del segundo factor del dueño»): «rechazo de la revocación; con un segundo dueño, este puede apoyar la recuperación según el procedimiento documentado». CL-43 cubre la carrera de revocaciones mutuas. El bloqueo por intentos se verifica con el criterio de RF-ACC-10 («5 intentos fallidos activan el límite») | ADR-03.C; R2-B |
| CAB-11 | ACT-01, ACT-09 | Obtener datos de menores por la proyección pública, el correo, la telemetría o la exportación | AM-13, AM-21, AM-25 | Ninguna zona prohibida de §5.6 recibe el dato | PRV-02, PRV-03, PRV-12, §5.6 | Revisión de una muestra de 100 registros: 0 datos personales (PRV-12); lectura por los actores de `01 §7.3` (PRV-09) | R2-C; R3; R4 |
| CAB-12 | ACT-01 | Identificar a un menor por la combinación de equipo, dorsal, minuto o resúmenes | AM-31 | Evaluación jurídica y de diseño antes del lanzamiento | PRV-18 | Informe de evaluación (PRV-18) | R2-C; asesoría jurídica |
| CAB-13 | ACT-05, ACT-10 | Alterar o borrar registros de auditoría o del historial de roles | AM-16, AM-24 | Ningún rol de la aplicación puede; el límite frente a la infraestructura queda documentado | RF-AUD-03, RC-21 | Intentos de actualizar o borrar por los actores de `01 §7.3` | ADR-09.C |
| CAB-14 | ACT-03…06 | Provocar un rechazo que no deje rastro (reversión de la operación) o inundar el registro | AM-17 | El rechazo persiste de forma independiente [DER]; **[REC]** el volumen no impide registrar | RF-AUD-06, §7.0 | CL-48, CL-49, CL-51 | ADR-09.C |
| CAB-15 | ACT-01, ACT-08 | Inundar el acceso o las invitaciones, o averiguar qué correos tienen cuenta | AM-05, AM-06 | Límite de frecuencia; **[REC]** sin diferencias observables entre una cuenta existente y una inexistente y sin bloquear al titular legítimo de forma permanente | RNF-05 | «5 intentos fallidos activan el límite» (RF-ACC-10) | ADR-03.C |
| CAB-16 | ACT-03, ACT-07 | Manipular el reloj del dispositivo | AM-10 | El orden no cambia; aviso si el reloj difiere más de 5 min | OFF-13 | «Reloj adelantado 2 h: el orden y la tabla no cambian» (OFF-13) | R2-C |
| CAB-17 | ACT-10, ACT-11 | Restaurar un respaldo con datos anteriores a una anonimización, o fuera de un entorno protegido | AM-29, AM-30 | Mismo nivel de protección que Z3; retención y anonimización por decidir | PRV-06, PRV-17, TB-9, HR1-12 | **AM-29:** «revisión de las semillas, los entornos y las copias: 0 datos reales ni derivados de datos reales» (PRV-17), solo para entornos no productivos. **AM-30 y respaldos productivos: pendiente de definir** (`01` no fija la retención de los respaldos: HR1-12, PEND-JUR) | R4 |
| CAB-18 | ACT-02…05 | Usar la exportación de datos de un jugador (PRV-08) o el resumen limitado (OFF-24) para filtrar datos | AM-13 | Solo quien autoriza la matriz; el archivo entregado no es purgable (HR1-14) | PRV-08, OFF-24 | **OFF-24:** «Por defecto el archivo exportado contiene solo esos tres datos; no contiene nombres, dorsales, minutos ni eventos; una captura con eventos de un jugador menor no los exporta; no existe función de importación». **PRV-08:** «Exportar y anonimizar un jugador de prueba en un entorno de pruebas, con auditoría». Véase también CAB-11. **Pendiente de definir** solo lo que `01` no resuelve: las facultades del organizador sobre la exportación (Q-42) y los eventos que podrían incluirse (Q-43) | R2-B; R2-C |
| CAB-19 | ACT-08, ACT-07 | Seguir usando una sesión robada tras revocar la cuenta o cambiar la contraseña | AM-03 | Toda nueva operación se rechaza; las sesiones conectadas lo reflejan en ≤ 60 s como objetivo técnico | RF-ACC-07, RF-ACC-16, D-2 | «Tras cambiar la contraseña, las sesiones anteriores dejan de ser válidas» (RF-ACC-16) | ADR-03.C |
| CAB-20 | ACT-09, ACT-10 | Encontrar credenciales de servicio en el cliente, la cola, los registros o la telemetría | AM-26, AM-25 | 0 apariciones | RT-09, PRV-12 | Revisión en Fase 7 | R3; Fase 7 |
| CAB-21 | ACT-10, ACT-08 | Tomar una credencial administrativa o técnica (cuenta de la organización en la plataforma, acceso del desarrollador, claves) y operar sobre datos, respaldos o configuración | AM-23 | Credenciales individuales y revocables, a nombre de la organización; **[REC]** segundo factor administrativo | RNF-14, RC-18 | «Inventario firmado en Fase 8» (RNF-14) | R3; R4; Fase 8 |
| CAB-22 | ACT-10, ACT-11 | Perder a la persona con accesos administrativos sin sucesión: nadie puede administrar la plataforma ni recuperar la cuenta del dueño | AM-32 | Sucesión de accesos y manual de operación | RNF-17, RNF-14, RF-ACC-09, CL-28 (parcial) | «Runbook revisado» (RNF-17). **CL-28** solo cubre la pérdida del segundo factor del dueño con el apoyo de un segundo dueño; **no cubre la sucesión de las credenciales administrativas de la plataforma, que queda pendiente de definir** | R4; Fase 8 |
| CAB-23 | ACT-08 | Un sitio externo induce a una persona con sesión a emitir una solicitud con efecto **(solo si el mecanismo de sesión envía la credencial de forma automática)** | AM-07 | **[REC]** toda solicitud con efecto exige prueba de intención y de origen | RNF-05 y referencia de seguridad de la skill | **[REC]** una solicitud con efecto sin prueba de origen e intención se rechaza | ADR-03.I (R3) |
| CAB-24 | ACT-01…05 | Inyectar consultas o datos mediante campos de entrada | AM-19 | **[REC]** validación en el límite de confianza; las restricciones de integridad de la capa de datos no la sustituyen | — (`01` no exige la validación de entradas; RNF-06 no trata la inyección) | **[REC]** una entrada con consultas o datos manipulados se rechaza o se neutraliza | R2-B; Fase 4 |
| CAB-25 | ACT-02…05, ACT-08 | Escribir contenido (marcado o scripts) en un nombre o motivo para que se ejecute en una vista pública, en el panel o en un correo | AM-34 | **[REC]** escape según el contexto de salida; cabeceras de seguridad | RNF-05 | **[REC]** un nombre o motivo con contenido activo se muestra como texto inerte en cada vista | R2-B; R3; Fase 4 |
| CAB-26 | ACT-01 | Generar carga masiva de lectura pública | AM-14 | Degradación explícita; disponibilidad de mejor esfuerzo | RNF-03, RNF-04, RNF-15 | Carga (Fase 7) | R3 |
| CAB-27 | ACT-09, ACT-11 | Una tarea programada sin actor modifica datos evitando RT-14 | AM-22 | Hereda la situación del actor que la dispara; **[REC]** se audita con su origen | RT-14, §3.1 | **[REC]** una tarea sin actor no ejecuta operaciones sobre el equipo de un actor en conflicto | R4 (ADR-14); ADR-09.C |
| CAB-28 | ACT-11, ACT-05 | Publicar datos no públicos o un estado incoherente entre resultados y clasificación | AM-27 | Solo proyecciones con lista blanca; garantía exigida de coherencia | PA-04, PRV-03, CL-15 | «Revisión de esquema y pruebas de contrato de cada vista o endpoint público (ningún campo de la lista)» (PRV-03). La coherencia entre resultados y clasificación: **pendiente de definir** (ADR-04.C y ADR-06) | R2-C; R3 |
| CAB-29 | ACT-11 | Revocar una autorización de nombre público y que el nombre siga visible en copias | AM-28 | Se trata en R2-C y R3; **no se promete un plazo** (HR1-11) | PRV-06, PRV-02, HR1-11 | **No existe:** HR1-11 no fija plazo; lo fijarían R2-C y R3 con asesoría jurídica | R2-C; R3 |
| CAB-30 | ACT-07, ACT-03 | Con un dispositivo compartido, otra cuenta sincroniza la cola del titular o se la apropia | AM-33 | **[REC]** la sesión que sincroniza es la de la cuenta que capturó (IN-03-19) y la cola se aísla por cuenta (IN-03-20) | OFF-03, OFF-12, OFF-22 | **[REC]** sincronizar con la sesión de otra cuenta se rechaza | ADR-03.C; R2-C |

**Amenazas registradas sin evaluar (A-24).** No se les asignan probabilidad ni impacto en R2-A (no ampliar el alcance); se registran para que no se pierdan y se evalúan en el bloque indicado.

| ID | Amenaza | Frontera | Se evalúa en |
|---|---|---|---|
| OM-01 | Abuso con privilegios legítimos (anonimización o exportación masivas, revocaciones) por un dueño | TB-1, TB-4 | R2-B; R4 |
| OM-02 | Sesiones concurrentes y revocación por dispositivo (hoy la defensa ante un dispositivo perdido es revocar la cuenta, OFF-12) | TB-1, TB-2 | ADR-03.I (R3) |
| OM-03 | Invitaciones con texto controlado por el atacante usadas para spam o suplantación | TB-5 | ADR-15 (R3) |
| OM-04 | Cadena de suministro y dependencias (RNF-05 exige escanearlas; CMP-19 las gestiona) | TB-6 | R3; R4 |
| OM-05 | Códigos QR o enlaces públicos suplantados (RC-03) | TB-3 | R3 |
| OM-06 | Restauración de un respaldo que revive accesos o roles revocados | TB-9 | R4 (ADR-12) |
| OM-07 | Captura falsa o en colusión por un editor u organizador **legítimo** con permisos válidos (manipulación o repudio de resultados oficiales) | TB-1, TB-4 | R2-B; R4 |

### 16.5 Qué trata R2-A y qué queda para otros artefactos

Distribución de las amenazas por **responsable principal** (comprobable en §16.12). **«Trata» significa que R2-A fija en ADR-03.C o ADR-09.C la capacidad exigida; no que la amenaza esté mitigada.**

| Responsable | Amenazas (responsable principal) | Observación |
|---|---|---|
| **ADR-03.C** (R2-A) | AM-02, AM-03, AM-05, AM-06, AM-09, AM-20, AM-33 | Identidad, sesión, 2FA, recuperación, sincronización. AM-20 comparte responsable con ADR-15 (R3) y AM-33 con R2-C |
| **ADR-09.C** (R2-A) | AM-16, AM-17, AM-24 | Inmutabilidad y persistencia del rechazo. ADR-09.C también aporta el registro de AM-22 y AM-25 |
| **R2-B** (ADR-02.C), no iniciado | AM-01, AM-15, AM-18, AM-19, AM-34 | Reparto de la autorización y frontera atómica. AM-34 comparte responsable con R3 y la Fase 4 |
| **R2-C** (ADR-04.C, 05.C, 16.C), no iniciado | AM-04, AM-08, AM-10, AM-11, AM-12, AM-13, AM-27, AM-28, AM-31 | Visibilidad, sincronización y almacenamiento local. AM-28 comparte responsable con ADR-06 (R3) |
| **R3** | AM-07, AM-14, AM-21, AM-23, AM-26 | Dependen del proveedor o del mecanismo de entrega (AM-07: ADR-03.I). AM-21 comparte responsable con ADR-14 (R4) y AM-23 con R4 |
| **R4** | AM-22, AM-25, AM-29, AM-30, AM-32 | Operación, retención, respaldos y sucesión (AM-32 también de la Fase 8) |

### 16.6 ADR-03.C — Identidad, sesión y segundo factor (PROPUESTA)

#### 16.6.1 Identificación

| Campo | Contenido |
|---|---|
| **ID y bloque** | ADR-03.C · R2-A · Fase 2 |
| **Estado** | **PROPUESTA.** No aprobada. §8.1 sigue mostrando ADR-03 como PREVISTO |
| **Fecha** | 2026-11-16 (v0.1); corregida el 2026-11-17 (v0.2 a v0.5) |
| **Pregunta de decisión** (§8.1) | ¿Cómo se autentica, se exige 2FA por rol y se emite el nivel de autenticación que usa CMP-08? ¿Qué nivel se exige al sincronizar? ¿Cómo se alcanza el objetivo de `01 RF-ACC-07` (≤ 60 s, D-2)? |
| **Qué es la parte «.C»** | Capacidades, restricciones e invariantes **independientes del proveedor** (RC-25). La parte dependiente del stack es **ADR-03.I** y se condiciona a ADR-01 (R3, §16.6.9) |
| **Requiere** (§8.2) | **—** (ningún otro artefacto) |
| **Lo requieren** (§8.2) | ADR-02.C, ADR-05.C, ADR-03.I, ADR-15 y, por la decisión conjunta ADR-01 / ADR-10, ambos |
| **Evidencia de aprobación humana** | **Ninguna.** Hasta que el usuario apruebe R2-A, nada de esto es decisión |

**Contexto.** CMP-06 emite el nivel de autenticación de la sesión; CMP-08 es el único decisor de la política efectiva (§5.3). `01 RF-ACC-10` pide fijar en la Fase 2 los parámetros (longitud, expiración, método del 2FA), y `01 RF-ACC-16` pide fijar tiempos y mecanismos de sesión. HR1-04 señala que el nivel depende de **todos** los roles vigentes y que no está fijado qué nivel se exige al sincronizar.

#### 16.6.2 Invariantes

**A. Invariantes obligatorios** (vienen de un requisito aprobado de `01`, o se derivan de él):

| ID | Invariante | Tipo | Fuente |
|---|---|---|---|
| IN-03-01 | **Un solo emisor** del nivel de autenticación (CMP-06) y **un solo decisor** de la política efectiva y de todo permiso (CMP-08) | [DER] | §5.3, RC-11 |
| IN-03-02 | La política efectiva de 2FA se calcula **por cuenta, con todos los roles vigentes**: con rol de dueño u organizador el 2FA es obligatorio; para editor y delegado es opcional y recomendado | [CONF] | RF-ACC-10, RC-11, Q-08, Q-39 |
| IN-03-03 | Al otorgar dueño u organizador a una cuenta sin 2FA, debe **enrolarse antes de usar ese rol** | [CONF] | RF-ACC-10 |
| IN-03-04 | **Designar o revocar a un dueño exige nueva autenticación con 2FA**; la revocación exige además motivo | [CONF] | RF-ACC-09, D-4 |
| IN-03-05 | Las cuentas se crean **solo por invitación** (sin auto-registro); la invitación es de un solo uso, vence y se acepta con el correo invitado; su rol y ámbito no cambian tras emitirla | [CONF] | RF-ACC-01, RF-ACC-02 |
| IN-03-06 | **La revocación se aplica de inmediato en el servidor:** toda nueva operación sin autorización vigente se rechaza. El texto de `01` no distingue las escrituras de las lecturas de datos internos (PRV-09): ver H2A-09 | [CONF] | RF-ACC-07, D-2 |
| IN-03-08 | Cierre de sesión por inactividad y por el usuario; **invalidación de las sesiones anteriores** al revocar y al cambiar la contraseña; recuperación de cuenta | requisito M de `01` | RF-ACC-16 |
| IN-03-09 | **Límites de intentos fallidos** en el acceso y en el segundo factor (el criterio de aceptación de RF-ACC-10 menciona 5 intentos) | [CONF] | RF-ACC-10, RNF-05 |
| IN-03-10 | Para **sincronizar** se exige sesión válida y permisos vigentes; con sesión vencida se puede **continuar la captura local ya autorizada**, pero **no sincronizar ni preparar datos nuevos** | [CONF] | OFF-22, OFF-23, RC-08 |
| IN-03-11 | **El contexto local no autentica ni autoriza:** ningún dato guardado en el dispositivo concede acceso ni permiso en el servidor | [DER] | PA-01, PA-05, RC-04 |
| IN-03-17 | **Sin permisos nuevos y sin reducción de garantías:** ADR-03.C solo añade **condiciones** de autenticación; no concede facultad alguna y respeta PA-13 | [DER] | CR1-13, PA-13 |
| IN-03-18 | **Sin proveedor ni mecanismo seleccionado.** Toda propiedad que dependa del proveedor queda condicionada a ADR-01 | [CONF] | RC-23, RC-25 |

**B. Objetivo técnico confirmado, con validación pendiente** (no es un invariante verificable en la Fase 2):

| ID | Objetivo | Tipo | Fuente |
|---|---|---|---|
| IN-03-07 | Las sesiones **conectadas** reflejan la revocación en **≤ 60 s como objetivo técnico**, **sin garantía** para dispositivos desconectados, que reverifican sus permisos al sincronizar. **No puede medirse en la Fase 2** (H2A-01, §16.6.4) | [CONF] con precisión | RF-ACC-07, D-2, OFF-22 |

**C. Propuestas [REC]** (**no obligan** hasta su aprobación; ninguna es un requisito de `01`):

| ID | Propuesta | Fuente |
|---|---|---|
| IN-03-12 | Los plazos de inactividad y absoluto se **hacen cumplir en el servidor**; el identificador de sesión se renueva al autenticarse y al cambiar privilegios; el **secreto de portador de la sesión no reside en almacenamiento local legible por los scripts de la página** (precisa §5.6, que no menciona ese secreto: H2A-03; es también la razón de ser frente a AM-34) | F-01, F-02 |
| IN-03-13 | Ningún mensaje del acceso, de la recuperación ni de las invitaciones revela si una cuenta existe | F-03 |
| IN-03-14 | El segundo factor ofrece **al menos una opción resistente al phishing** y no depende de un único canal. **Mensajes de texto o de voz — contenido literal de las fuentes:** F-04 dice «Do not use SMS for high-value or PII-handling applications» y, si es el único factor disponible, pide documentar la aceptación del riesgo, limitar la frecuencia por cuenta, vigilar el cambio de SIM y planificar la migración; F-01 los trata como autenticador **restringido** (la organización debe evaluar y aceptar el riesgo, ofrecer una alternativa no restringida, avisar a las personas usuarias y tener un plan de migración). **Política para este proyecto: PENDIENTE DE DECISIÓN** (la aplicación guarda correos y nombres internos de jugadores y menores): (i) **no admitirlos** (lo que dice F-04); o (ii) admitirlos solo como alternativa restringida y con aceptación expresa del riesgo (**más débil que F-04**) | F-01, F-04 |
| IN-03-15 | **La recuperación no puede ser más débil que la autenticación que sustituye** y no concede un nivel superior al que el medio de recuperación puede demostrar. **Para dueño y organizador, el correo por sí solo no basta** (F-01 pide, para recuperar una cuenta de nivel de garantía 2, dos códigos obtenidos por métodos distintos, o uno más un autenticador de un factor, o repetir la identificación) | F-01, F-04 |
| IN-03-16 | Se exige **reautenticación con un factor ya enrolado** tras: cambio de contraseña o de correo; **alta, baja o cambio del segundo factor o de los medios de recuperación**; y las operaciones de IN-03-04. **Tras una recuperación de cuenta** no hay un factor enrolado utilizable (pérdida del factor): se sustituye por el procedimiento de recuperación (IN-03-15), que define cómo se enrola el nuevo factor y con qué verificación | F-02 (reautenticar tras eventos de riesgo), F-04 (reautenticar con un factor enrolado antes de cambiar factores) |
| IN-03-19 | **Cada operación capturada sin conexión queda ligada a la cuenta que la capturó** (OFF-03 guarda su usuario). Al sincronizar, CMP-08 la evalúa con **esa** cuenta (OFF-22) y **la sesión que sincroniza debe pertenecer a esa misma cuenta**; una sesión de otra cuenta nunca sincroniza ni apropia operaciones ajenas | OFF-03, OFF-22 (precisa que `01` no dice que la sesión deba ser la de la cuenta que capturó) |
| IN-03-20 | **La cola y los datos preparados se aíslan por cuenta en el dispositivo:** iniciar sesión con otra cuenta no da acceso a la cola de la primera, que sigue sujeta a OFF-12 (cerrar sesión con pendientes exige confirmación y borra los datos locales tras sincronizar o descartar) | OFF-12 |
| IN-03-21 | **Notificación al titular** por un canal independiente (distinto del modificado, cuando exista otro) ante: alta, baja o cambio del segundo factor o de los medios de recuperación; cambio de contraseña o de correo; y recuperación de cuenta. El mensaje contiene lo mínimo (§5.6) y **ningún dato de jugadores ni de menores**. **No es un requisito de `01`**; el envío es de CMP-18 (ADR-15, R3). **Propuesta separada y aún no decidida:** notificar también la **designación o revocación de un dueño**; RF-ACC-09 deja esa notificación como [REC] no decidida en `01` y R2-A **no la convierte en obligación** | F-01 (la recuperación de cuenta y la vinculación de un autenticador generan una notificación al titular), F-04 (notificar por un canal fuera de banda cuando se cambia un factor), RF-ACC-09 |

#### 16.6.3 Formulaciones de capacidad (alternativas) y su conformidad

Se presentan **al menos dos** formulaciones por decisión y **se evalúa cada una contra todos los invariantes aplicables**. Rótulos: **«No conforme»** = incumple un invariante **obligatorio** (tabla A de §16.6.2) y **no cuenta como alternativa viable**; **«No cumple IN-xx [REC]»** = incumple una **propuesta** [REC]: no la descarta, pero obliga al usuario a decidir si esa propuesta se adopta; **«Condicionada»** = cumple solo si se añade una condición explícita; **«Conforme»** = cumple. **R2-A no elige ninguna.** Cada formulación describe una capacidad, no una tecnología; **los ejemplos de mecanismo son ilustrativos**.

**Decisión D3-1 — Nivel de autenticación y su frescura**

| ID | Formulación | IN-03-02 | IN-03-04 (obligatorio) | IN-03-16 [REC] | Conclusión |
|---|---|---|---|---|---|
| CAP-03-N-A | **Nivel simple por sesión:** la sesión tiene un nivel (un factor o multifactor) emitido al autenticarse; CMP-08 lo compara con la política efectiva | Conforme | **No conforme:** CMP-08 no sabría si el segundo factor se verificó hace poco | No cumple IN-03-16 [REC] | **No conforme por sí sola.** Solo es referencia mínima: si se le añade la edad de la verificación del segundo factor, **se convierte en CAP-03-N-B** |
| CAP-03-N-B | **Nivel con frescura:** además del nivel, la sesión registra **cuándo** se verificó el segundo factor; las operaciones de IN-03-04 e IN-03-16 exigen una verificación **reciente** (PAR-05) y las demás, solo el nivel | Conforme | **Condicionada** a la lectura de «nueva autenticación» (análisis abajo, H2A-11) | Conforme | **Condicionada:** conforme si «nueva» significa «reciente»; **no conforme si significa «propia de esa operación»** |
| CAP-03-N-C | **Comprobante por operación crítica:** cada operación crítica exige un comprobante de autenticación reciente **de un solo uso**, vinculado a esa operación, que CMP-08 consume al decidir | Conforme | Conforme bajo **ambas** lecturas | Conforme | **Viable.** Los comprobantes se emiten en línea, lo que no estorba porque esas operaciones son de gobierno y no se capturan sin conexión (OFF-01 solo lista captura) |

**Análisis de CAP-03-N-B frente a RF-ACC-09 e IN-03-04 (R-05).** `01 RF-ACC-09` y CL-38 exigen «nueva autenticación con 2FA» para designar o revocar a un dueño, y el criterio de aceptación de RF-ACC-09 dice: «designar o revocar sin reverificación del 2FA … → rechazado». **`01` no define «nueva».** Puede leerse como **(L1) «reciente»** —una verificación del segundo factor dentro de una ventana— o como **(L2) «propia de esa operación»** —cada designación o revocación exige una verificación posterior a iniciarla—. Bajo **L1**, N-B cumple. Bajo **L2**, N-B **no cumple**: dentro de la ventana no hay reverificación. **Riesgo de sesión robada (AM-03):** si se roba la sesión de un dueño (o este la deja abierta) dentro de la ventana de ≤ PAR-05 desde su última verificación, el atacante podría designar o revocar a un dueño **sin reverificar el segundo factor**, justo la operación que RF-ACC-09 protege; la ventana reduce la exposición pero no la elimina. N-C cumple bajo L1 y bajo L2. **Qué significa «nueva» lo decide el usuario** (H2A-11, decisión 12); R2-A **no elige** entre N-B y N-C ni adopta ninguna lectura.

**Decisión D3-2 — Condiciones para sincronizar operaciones capturadas sin conexión** (HR1-04, §7.0)

| ID | Formulación | IN-03-02 | IN-03-10 | IN-03-11 | IN-03-19 [REC] | Conclusión |
|---|---|---|---|---|---|---|
| CAP-03-S-A | **Política efectiva vigente al sincronizar:** se exige sesión válida cuyo nivel satisface la política efectiva **vigente en ese momento** para la cuenta (aunque la captura se hiciera con otro nivel) y CMP-08 revalida cada operación. Es la lectura **más cercana al texto** de OFF-22, OFF-23 y RF-ACC-10; **los requisitos no fijan el nivel exigido al sincronizar** (§7.0) | Conforme | Conforme | Conforme | Conforme | **Viable** |
| CAP-03-S-B | **S-A más reverificación:** si la sesión se restableció después de un vencimiento con operaciones pendientes, se exige reverificar el segundo factor **antes** de revalidar las operaciones, cuando la política efectiva lo requiere | Conforme | Conforme | Conforme | Conforme | **Viable**, con más fricción al volver a la red |

**Decisión D3-3 — Recuperación y pérdida de acceso** (se evalúa contra IN-03-15 [REC] y la amenaza AM-02)

| ID | Formulación | IN-03-15 [REC] | IN-03-13 [REC] | Exposición a ingeniería social | Conclusión |
|---|---|---|---|---|---|
| CAP-03-R-A | **Recuperación autónoma del titular:** medios previamente registrados (por ejemplo, códigos de un solo uso entregados al enrolar y un segundo medio verificado), con vigencia limitada (PAR-04) y límites de intentos | **Condicionada:** cumple solo si los medios son tan fuertes como la autenticación que sustituyen; **no cumple si se basa solo en el correo** (AM-20) | Conforme | El titular puede ser engañado (phishing); un correo comprometido basta solo si se permite recuperar solo con correo | **Viable con condición** (medios fuertes, no solo el correo) |
| CAP-03-R-B | **Recuperación por procedimiento organizativo documentado** (RF-ACC-09: «cuenta perdida, pérdida del segundo factor»): la organización verifica la identidad y un acceso técnico auditado restablece la cuenta. **No es una facultad de la aplicación** ni concede permisos a ningún rol (IN-03-17) | **No cumple IN-03-15 [REC] como vía única:** la verificación humana puede ser más débil que la autenticación | Conforme | **Alta:** la persona que verifica puede ser engañada; un acceso técnico puede reponer el segundo factor de un dueño; la auditoría de ese acceso está fuera de lo que la aplicación protege (RF-AUD-03) | **Solo como último recurso**, con **[REC]**: verificación de identidad documentada por un canal distinto, doble control por dos personas y registro fuera de la aplicación |
| CAP-03-R-C | **Combinación:** R-A como vía normal y R-B como último recurso para no perder el gobierno (último dueño: RF-ACC-09; RT-06 solo impide revocar o degradar al último dueño y no cubre su ausencia) | Condicionada: hereda las condiciones de R-A y R-B | Conforme | Reducida frente a R-B sola, que queda acotada a casos extremos | **Viable con las condiciones de R-A y de R-B** |

**Escenarios de recuperación evaluados**

| Escenario | Qué exige el diseño | Exposición |
|---|---|---|
| E1. Olvido de la contraseña con el segundo factor disponible | Reautenticación con el factor enrolado (IN-03-16); no se concede un nivel superior al demostrado (IN-03-15); **[REC]** aviso al titular (IN-03-21) | Baja |
| E2. Pérdida del segundo factor con medios de recuperación disponibles | R-A con medios fuertes y vigencia limitada (PAR-04); el alta del nuevo factor se verifica con el procedimiento de recuperación (IN-03-16); **[REC]** aviso al titular (IN-03-21) | Media: el titular puede ser engañado para entregar un código |
| E3. Pérdida del segundo factor **sin** medios de recuperación | R-B como último recurso, con las condiciones de §16.6.3 | **Alta:** ingeniería social contra quien verifica |
| E4. Correo comprometido (vector de AM-20) | **El correo por sí solo no basta** para dueño u organizador (IN-03-15); **[REC]** el aviso al titular se envía también por otro canal cuando exista (IN-03-21) | Crítica si se permite recuperar solo con correo |
| E5. Ingeniería social contra quien verifica la identidad (R-B) | Doble control y registro fuera de la aplicación **[REC]**; mitigación parcial con CAP-09-M2/M3 (§16.7.4) | Alta; RES-01 |
| E6. El último dueño pierde el acceso | Procedimientos de RF-ACC-09 y segundo dueño de respaldo. **RT-06 no cubre este caso:** solo impide revocar o degradar al último dueño | Alta si no hay segundo dueño; AM-32 |

**Decisión D3-4 — Cómo reflejan la revocación las sesiones conectadas** (RF-ACC-07, D-2). **Distinción central:** el **rechazo inmediato** de toda nueva operación sin autorización vigente (IN-03-06) **no depende de esta decisión**: se deriva de que la evaluación de CMP-08 consulta el estado vigente en cada operación (§7.0 paso 1; RT-13). D3-4 solo decide **cómo se entera el cliente conectado** para reflejarlo.

**Alcance de «sesión conectada» (R-04): R2-A no lo reduce.** `01` (RF-ACC-07, D-2) solo excluye expresamente a los dispositivos **desconectados** y **no define «conectada»**. Se distinguen tres situaciones **sin decidir cuál cuenta como conectada**: **S1** con conexión y aplicación en primer plano; **S2** con conexión pero en segundo plano o con el dispositivo suspendido; **S3** sin conexión (fuera de garantía por `01`). **Si S2 cuenta como conectada es una interpretación abierta** (H2A-10).

| ID | Formulación | IN-03-06 | Objetivo documental en **S1** | Objetivo documental en **S2** | Dependencias | Conclusión |
|---|---|---|---|---|---|---|
| CAP-03-V-A | **Consulta periódica del cliente:** el cliente consulta el estado de su sesión y de sus permisos cada **P** segundos para reflejarlo en la interfaz | Conforme (el servidor comprueba en cada operación) | **Condicionado:** P ≤ 60 s − (Tred + Tsrv + Tcli); con el ejemplo de §16.6.4, P ≤ 40 s sin reintentos y ≤ 20 s con un reintento. Con P = 60 s **no se cumple** | **No demostrable en la Fase 2:** el comportamiento de los temporizadores del navegador en segundo plano o suspendido no está verificado | Ninguna | **Viable en S1 con P menor**; S2 abierto |
| CAP-03-V-B | **Credencial de sesión de vida corta con renovación** (ejemplo ilustrativo de la capacidad): la credencial dura como máximo **V** y se renueva mientras la sesión siga vigente; tras una revocación, la renovación falla | **Conforme solo si** además se consulta el estado en cada operación (si la credencial se validara sin consultar, la ventana V incumpliría IN-03-06) | **Condicionado** igual que V-A, con V en lugar de P | **No demostrable** (la renovación depende de un cliente activo) | Ninguna | **Viable en S1 con V menor y consulta por operación**; S2 abierto |
| CAP-03-V-C | **Aviso activo del servidor al cliente conectado** cuando se revoca una sesión o un permiso, complementado por la comprobación en cada operación | Conforme | Depende de la latencia de un canal que **no existe en el MVP** | **Entrega no garantizada** a un cliente suspendido; `01 RF-PWA-04` deja fuera del MVP las notificaciones push | **Requiere un canal de aviso del servidor al cliente** (ADR-07, R3); `01 §3` deja fuera del MVP el tiempo real | **No viable en el MVP sin una decisión de R3** |

**Ninguna formulación cumple documentalmente el objetivo en S2.** En S3 el objetivo no se promete (D-2).

**Comparación cualitativa con los criterios de §2.4** (juicio del autor [SUP], sin datos de costo; la evaluación por proveedor es de R3; se omiten las formulaciones no conformes):

| Formulación | Costos bajos | Seguridad | Funcionamiento sin conexión | Mantenimiento |
|---|---|---|---|---|
| CAP-03-N-B (condicionada) | Neutral | Neutral (la ventana deja una exposición, AM-03) | Neutral (exige estado de frescura al restablecer sesión) | Neutral |
| CAP-03-N-C | Desfavorable | Favorable | Neutral (las operaciones críticas no se capturan sin conexión) | Desfavorable |
| CAP-03-S-A | Favorable | Neutral | Favorable | Favorable |
| CAP-03-S-B | Neutral | Favorable | Neutral (obliga a reverificar al volver) | Neutral |
| CAP-03-R-A | Favorable | Neutral (los medios de recuperación son un riesgo, AM-20) | Favorable | Neutral |
| CAP-03-R-C | Neutral | Favorable | Favorable | Desfavorable |
| CAP-03-V-A | Neutral (consultas periódicas) | Favorable | Neutral | Favorable |
| CAP-03-V-B | Neutral | Favorable | Neutral | Neutral |

#### 16.6.4 Presupuesto temporal documental del objetivo de ≤ 60 s (H2A-01)

**Qué NO se hace:** no se afirma que el objetivo esté validado empíricamente; **no se concede excepción alguna**; **no se traslada la medición a otra fase**; no se cambia D-2 ni RF-ACC-07; no se redefine «sesión conectada». Lo que sigue es un **análisis documental de factibilidad**; **ningún valor numérico está medido**.

**Definiciones.** **t0**: instante en que el servidor confirma la revocación. **Rechazo inmediato (garantía):** toda evaluación que empiece después de t0 se rechaza (IN-03-06; RT-13 serializa las operaciones en curso: UI-11). **Reflejo en una sesión conectada (objetivo):** el cliente deja de ofrecer o ejecutar acciones de la cuenta revocada y se lo muestra al usuario, medido desde t0, en las situaciones S1 y, según la interpretación abierta de H2A-10, S2.

**Presupuesto del peor caso.** T = **Tdet** + **Tred** + **Tsrv** + **Tcli**, y debe cumplirse **T ≤ 60 s**:
- **Tdet:** tiempo hasta que el cliente se entera (V-A: hasta **P**; V-B: hasta **V**; V-C: latencia del aviso).
- **Tred:** latencia de ida y vuelta **incluidos los reintentos**: Tred = (k + 1) × TE + Σ esperas, con **TE** el tiempo de espera de una solicitud, **k** el número de reintentos y las esperas crecientes entre ellos (OFF-05: reintento periódico con espera creciente).
- **Tsrv:** procesamiento y serialización en el servidor (RT-13).
- **Tcli:** retirada de acciones en la interfaz y desfase de los temporizadores del cliente.

**Ejemplo ilustrativo [SUP]; ningún valor está medido.** TE = 15 s, una espera de 5 s antes de un reintento, Tsrv = 2 s y Tcli = 3 s. Sin reintento, Tred = 15 s; con **un** reintento, Tred = 15 + 5 + 15 = 35 s.

| Formulación | Tdet | Tred sin reintento / con uno | Tsrv | Tcli | Total sin reintento / con uno | Conclusión |
|---|---|---|---|---|---|---|
| V-A con P = 60 s | 60 | 15 / 35 | 2 | 3 | **80 s / 100 s** | **No cumple por construcción** |
| V-A con P = 40 s | 40 | 15 / 35 | 2 | 3 | 60 s / 80 s | Cumple sin margen solo sin reintento |
| V-A con P = 30 s | 30 | 15 / 35 | 2 | 3 | 50 s / 70 s | Cumple solo sin reintento |
| V-A con P = 20 s | 20 | 15 / 35 | 2 | 3 | 40 s / 60 s | Cumple incluso con un reintento, sin margen |
| V-B con V = 20 s | 20 | 15 / 35 | 2 | 3 | 40 s / 60 s | Igual que V-A, con consulta por operación |
| V-C | latencia del aviso (no evaluada) | 15 / 35 | 2 | 3 | ≥ 20 s + aviso / ≥ 40 s + aviso | Depende de un canal que no existe en el MVP |

**Conclusión documental.** **Un intervalo o una vigencia igual a 60 s no puede cumplir el objetivo**: el peor caso suma, además, la espera de red, el procesamiento y la reacción del cliente. El máximo viable es **60 s menos (Tred + Tsrv + Tcli)**: con los valores ilustrativos, **P o V ≤ 40 s sin reintentos y ≤ 20 s con un reintento**. **El peor caso estricto** (todos los componentes al máximo) es más exigente que **un percentil** (por ejemplo, p95); **el criterio medible debe decir cuál se aplica [ABIERTO]**.

**Casos fuera de garantía o sin resolver:** (1) **S3, dispositivo desconectado:** fuera de garantía por D-2; OFF-22 revalida al sincronizar; (2) **S2, segundo plano o dispositivo suspendido:** **`01` no dice si cuenta como conectada** (H2A-10); los temporizadores pueden retrasarse o detenerse **[SUP; no verificado en las fuentes leídas ni en dispositivo]**, y OFF-05 y RK-11 establecen que iOS no ofrece sincronización en segundo plano (`01 RNF-07` solo fija las versiones mínimas); (3) **red lenta** o reintentos (RNF-15): Tred crece; (4) **lo ya mostrado:** los datos que el cliente cargó antes de t0 no se retiran de la pantalla ni del contexto local (análogo a HR1-11); (5) revocación de **roles** frente a RT-14: A-15 está abierta; (6) si los plazos se miden con el **reloj del cliente**: desfase (OFF-13).

| Qué puede verificarse **durante la Fase 2** (por inspección del diseño) | Qué **exige medición real** |
|---|---|
| Que el rechazo inmediato en el servidor es consecuencia del flujo §7.0 y de RT-13 y no depende de los 60 s | Tiempo extremo a extremo desde t0 hasta el reflejo, en el percentil que se acuerde, con el proveedor y la red reales (3G lenta) |
| La desigualdad del presupuesto por formulación (arriba) y el margen necesario, con reintentos | Android y iPhone: primer plano, segundo plano y dispositivo suspendido |
| La lista de casos fuera de garantía y que ninguna formulación promete algo a los dispositivos desconectados | Propagación del proveedor y capas intermedias; carga de V-A con sesiones concurrentes |
| La definición de lo que se mide y las situaciones S1, S2 y S3 | Criterio medible propuesto **[REC]**: p95 ≤ 60 s en S1 (y en S2 si la interpretación del usuario la incluye), con informe de los casos fuera de garantía |

**Opciones de tratamiento para la decisión del usuario** (**ninguna se adopta, no se concede excepción y no cambia D-2 ni RF-ACC-07**). `01` pide validar el objetivo «en la Fase 2» y RC-22 prohíbe medir en ella: **ninguna opción cumple D-2 literalmente dentro de la Fase 2**.

| Opción | Qué implica | ¿Requiere excepción formal o modificación de `01`? |
|---|---|---|
| (a) **Trasladar la medición a 5C o a la Fase 7**, con el criterio medible de arriba y tras fijar P o V con margen | La validación sale de la Fase 2 | **Sí: excepción formal expresa del usuario**, análoga a la de OFF-17 (RC-24) |
| (b) **Validación documental reforzada** (presupuesto de tiempo más capacidades del proveedor documentadas en R3) y medir después | También traslada la medición; el refuerzo documental no sustituye la validación que pide `01` | **Sí: excepción formal**, igual que (a) |
| (c) **Reformular D-2 o RF-ACC-07** | Cambia lo que `01` exige validar | **Sí: modificación formal de `01`**, fuera de R2-A y con autorización separada |
| (d) **Mantener D-2 abierto** como condición de la aprobación de la Fase 2 | No concede nada, pero **no puede ejecutarse bajo RC-22**: D-2 queda sin cumplir y la Fase 2 no podría cerrarse sin otra de las opciones | No, pero **bloquea el cierre** de la Fase 2 |

#### 16.6.5 Parámetros propuestos (RF-ACC-10 y RF-ACC-16 los fijan en la Fase 2) — H2A-02

**Ninguno está fijado.** Todo requiere aprobación del usuario; incluso la clasificación «Obligatorio por requisito aprobado» se refiere solo al **origen en `01`**. Clasificaciones: **Obligatorio por requisito aprobado** · **Propuesto y respaldado documentalmente** · **Propuesto sin respaldo suficiente** · **Pendiente de decisión**. «Nivel de garantía» es la clasificación de autenticación de F-01 (niveles 1, 2 y 3); sus plazos son casi todos «SHOULD». Las fuentes **no son obligatorias para el proyecto** (H2A-05).

| ID | Parámetro y valor propuesto | Requisito | Referencia consultada | Justificación y diferencia respecto de la referencia | Riesgo de adoptarlo | Clasificación |
|---|---|---|---|---|---|---|
| PAR-01 | **Contraseña:** mínimo **15** caracteres para todas las cuentas; permitir ≥ 64; sin reglas de composición; rechazo de contraseñas filtradas | RF-ACC-10: la longitud «se fija en la Fase 2» (no da valor) | F-01: mínimo 15 si es el único factor y 8 si solo se usa dentro de un proceso multifactor; máximo permitido de al menos 64 (SHOULD); sin reglas de composición (SHALL NOT); lista de contraseñas filtradas | **Más estricto que F-01 para las cuentas con multifactor (8):** delegado y editor pueden no tener 2FA, así que 15 es el piso razonable y una sola regla simplifica | Fricción de uso; la lista de bloqueo depende del proveedor (ADR-03.I) | **Propuesto y respaldado documentalmente** |
| PAR-02 | **Intentos fallidos:** tras **5** consecutivos, **bloqueo temporal creciente** (por ejemplo, 15 min) con aviso al titular; nunca permanente | **«5 intentos fallidos activan el límite definido en Fase 2» (criterio de aceptación de RF-ACC-10)**; RNF-05 | F-01: tope de **100** consecutivos por autenticador; esperas crecientes (30 s hasta 1 h) como técnica para no bloquear al titular. F-03: el contador se asocia a la **cuenta** | El «5» viene de `01`. La duración (15 min) y su crecimiento son **juicio del autor** dentro del rango de esperas de F-01; el tope de 100 de F-01 es mucho más laxo | Un atacante puede bloquear al **dueño único** (AM-05); RT-06 no lo cubre | **«5»: Obligatorio por requisito aprobado. Duración y crecimiento: Propuesto sin respaldo suficiente** |
| PAR-03 | **Plazos de sesión.** Grupo 1 (dueño, organizador y delegado: ven datos internos de jugadores, PRV-09): inactividad **30 min**, absoluto **12 h**. Grupo 2 (editor: solo etiqueta o nombre público y dorsal): inactividad **2 h**, absoluto **7 días**. Una cuenta con roles de ambos grupos usa el **más restrictivo** [REC]. La captura sin conexión **continúa** al vencer la sesión | RF-ACC-16: «Tiempos y mecanismos: Fase 2»; OFF-23 | F-01: nivel de garantía 1 ≤ 30 días en total (SHOULD; inactividad no exigida); nivel 2 ≤ 24 h en total y ≤ 1 h de inactividad (SHOULD); nivel 3 ≤ 12 h (SHALL) y ≤ 15 min (SHOULD). F-02: inactividad de **2–5 min** en aplicaciones de alto valor y **15–30 min** en las de bajo riesgo; el absoluto depende del uso (para una jornada completa de oficina, **4–8 h**) | **Grupo 1:** dueño y organizador tienen 2FA obligatorio (nivel 2 de F-01): 30 min y 12 h caen **dentro** de ese nivel. El delegado, con 2FA opcional, equivaldría al nivel 1 (≤ 30 días): 30 min y 12 h son **más estrictos**. Frente a F-02, 30 min queda **fuera** del rango de alto valor (2–5 min) y 12 h **fuera** del de 4–8 h; se propone porque el panel se usa de forma continua en jornada y la captura sin conexión no depende de la sesión (OFF-23). **Grupo 2:** 2 h y 7 días caen fuera de F-02 (15–30 min) y dentro del nivel 1 de F-01 (≤ 30 días); se propone porque el editor trabaja en cancha con red débil y solo ve etiquetas, nombres públicos y dorsal | Un dispositivo robado con sesión vigente del Grupo 2 permanece utilizable hasta 7 días; fricción de inicios de sesión frecuentes en el Grupo 1 | **Propuesto sin respaldo suficiente** (valores [SUP]; las referencias difieren entre sí y de la propuesta) |
| PAR-04 | **Vigencias.** Invitación: **7 días**. Código de recuperación por correo: **≤ 1 h** y de un solo uso | RF-ACC-02: «Vencimiento propuesto: 7 días» (plazo REC [PEND]); para la recuperación no hay requisito | F-01: un código de recuperación enviado por correo vale como **máximo 24 h** y por mensaje de texto o voz **10 min** | **Invitación:** es el plazo ya propuesto en `01`. **Recuperación: se propone un valor menor que el tope de F-01 (24 h)** porque es el vector de AM-20 (Crítico) y el correo puede estar comprometido | El titular puede no leer el correo a tiempo y debe solicitar otro código | **Invitación 7 días: Pendiente de decisión** (propuesto en `01`). **Recuperación ≤ 1 h: Propuesto sin respaldo suficiente** |
| PAR-05 | **Frescura de la reautenticación** para las operaciones de IN-03-04 e IN-03-16: **≤ 5 min** | RF-ACC-09: «nueva autenticación» sin plazo | Ninguna | Juicio del autor [SUP]. **Solo tiene sentido bajo la lectura L1 de «nueva» (§16.6.3, H2A-11)**; bajo L2 no hay ventana | Una sesión robada dentro de la ventana (AM-03); fricción en el trabajo administrativo prolongado | **Propuesto sin respaldo suficiente**; la lectura que la hace aplicable: **Pendiente de decisión** |
| PAR-06 | **Segundo factor:** al menos una opción resistente al phishing. **Política sobre los mensajes de texto o de voz: pendiente de decisión** (IN-03-14). El método concreto lo decide ADR-03.I | RF-ACC-10: «método del 2FA» se fija en la Fase 2 | F-01: ofrecer al menos una opción resistente al phishing en el nivel de garantía 2; los mensajes de texto o de voz son un autenticador **restringido** (la organización debe evaluar y aceptar el riesgo, ofrecer una alternativa no restringida, avisar a las personas usuarias y tener un plan de migración). F-04: «Do not use SMS for high-value or PII-handling applications»; si es el único factor disponible, documentar la aceptación del riesgo, limitar la frecuencia por cuenta, vigilar el cambio de SIM y planificar la migración | **La v0.2 proponía excluirlos solo como factor único, más débil que F-04: se retira** y la política queda pendiente. Esta aplicación guarda correos y nombres internos de jugadores y menores | Aplaza a R3 el método que `01` pide fijar en la Fase 2 | **Opción resistente al phishing: Propuesto y respaldado documentalmente.** **Mensajes de texto o de voz y método concreto: Pendiente de decisión** |

#### 16.6.6 Condiciones abiertas

| Condición | Estado |
|---|---|
| **D-2: validar en la Fase 2 el objetivo de ≤ 60 s** | **No puede validarse por medición** en la Fase 2 (RC-22). Se hizo el análisis de factibilidad de §16.6.4. **El usuario decide el tratamiento** (opciones a–d); **no se asume excepción ni se traslada la medición** (H2A-01) |
| Alcance de «sesión conectada» (S2) | **[ABIERTO] (H2A-10):** `01` no lo define; R2-A no lo reduce |
| Alcance de «nueva autenticación» (RF-ACC-09) | **[ABIERTO] (H2A-11):** L1 «reciente» o L2 «propia de la operación»; define si CAP-03-N-B es conforme |
| Política sobre mensajes de texto o de voz | **[ABIERTO]:** pendiente de decisión (IN-03-14) |
| Nivel de autenticación exigido al sincronizar | **[ABIERTO] de ADR-03** (HR1-04, §7.0): CAP-03-S-A o S-B |
| **IN-03-06 y las lecturas de datos internos** | **[ABIERTO] (H2A-09):** `01` dice «toda nueva operación»; **se propone leerlo como incluidas las lecturas de datos internos (PRV-09)** [REC]; si no, un delegado revocado podría seguir leyendo datos internos de menores hasta que expire su sesión |
| A-10 (reanudación sin conexión por organizador o dueño) | **[ABIERTO]**: ADR-03.C soporta ambas lecturas porque solo fija el nivel para sincronizar |
| A-15 (revocaciones de seguridad y RT-14) | **[ABIERTO]**: la invalidación de **sesiones** (IN-03-06, IN-03-08) es independiente de la revocación de **roles**; ADR-03.C no resuelve A-15 |
| 2FA opcional del delegado (Q-39 B) | **Decidido en `01`** con reevaluación tras el piloto; R2-A **no lo cambia** y registra el riesgo (RES-10) |
| PEND-JUR (retención de cuentas inactivas, identidades) | **[ABIERTO]**: ADR-03.C no define plazos de conservación |

#### 16.6.7 Consecuencias y riesgos

1. Todo dueño u organizador queda sujeto a 2FA y a una vía de recuperación; **un mal diseño de la recuperación es la principal vía de toma de cuenta** (AM-02, AM-20, RES-01).
2. El correo es a la vez canal de invitación y de recuperación: **su compromiso equivale al de la cuenta** si no se exige otro medio (IN-03-15).
3. **Los delegados, que ven nombres internos e indicador de menores (PRV-09), pueden operar sin 2FA** por decisión de `01`: toma de una cuenta de delegado = acceso a datos internos de menores (RES-10).
4. **Con CAP-03-N-B**, una sesión robada dentro de la ventana de frescura podría designar o revocar a un dueño sin reverificar el segundo factor; CAP-03-N-C no tiene ese efecto.
5. Un plazo de sesión corto aumenta la fricción de las cuentas de administración; no afecta la captura sin conexión (IN-03-10).
6. Los parámetros PAR-01…06 son propuestas **sin validación empírica** (RES-09).
7. ADR-03.C **no cierra** la conservación de cuentas ni la de auditoría de accesos: son de ADR-17 (R4) y de ADR-09.C.

#### 16.6.8 Elección de formulaciones y regla de precedencia

R2-A **no elige** entre CAP-03-N-B y N-C, S-A y S-B, R-A y R-C, ni entre V-A y V-B. **Las capacidades obligatorias `.C` se definen en R2.** El usuario puede, al aprobar R2: (a) **elegir una formulación** por decisión; o (b) **admitir un conjunto de formulaciones conformes** y dejar que **ADR-03.I (R3) elija cómo implementar una de ellas con el stack seleccionado**. **Si se adopta la regla de §16.8.5** (propuesta [REC] pendiente de la decisión 6; **no relaja ninguna restricción aprobada de R1**, que rigen en cualquier caso), **ADR-03.I no podría redefinir ni relajar los invariantes** fijados en ADR-03.C, y si una capacidad admitida resultara inviable con el stack, **volvería al usuario** y no se redefiniría en la parte `.I` (§16.10.3, decisión 6).

#### 16.6.9 Condicionado a R3 (ADR-03.I)

Nada de esto se verificó, porque **no existe proveedor seleccionado**: qué métodos de segundo factor y de recuperación ofrece cada candidato; qué modelo de sesión y qué plazos admite; **cuánto tarda en propagarse una revocación**; si los límites de intentos son configurables y por cuenta; si hay eventos de seguridad exportables; qué conserva el proveedor y por cuánto tiempo; **verificación de origen y de intención** de las solicitudes con efecto (AM-07). **[ABIERTO R3.]**

### 16.7 ADR-09.C — Auditoría e historial de solo anexado (PROPUESTA)

#### 16.7.1 Identificación

| Campo | Contenido |
|---|---|
| **ID y bloque** | ADR-09.C · R2-A · Fase 2 |
| **Estado** | **PROPUESTA.** No aprobada. §8.1 sigue mostrando ADR-09 como PREVISTO |
| **Fecha** | 2026-11-16 (v0.1); corregida el 2026-11-17 (v0.2 a v0.5) |
| **Pregunta de decisión** (§8.1) | ¿Cómo se garantiza la inmutabilidad de la auditoría y del historial de asignaciones de roles, y cómo **persiste el registro de un rechazo o bloqueo (RF-AUD-06) aunque la operación no se aplique** [DER]? |
| **Requiere** (§8.2) | **—** (ningún otro artefacto) |
| **Lo requieren** (§8.2) | ADR-02.C, ADR-05.C, ADR-09.I, ADR-17 y, por la decisión conjunta ADR-01 / ADR-10, ambos |
| **Evidencia de aprobación humana** | **Ninguna** |

#### 16.7.2 Qué registros hay, de qué naturaleza y dónde van

Esta tabla **no crea registros nuevos**: ordena los que `01` ya exige y señala los casos abiertos. **Naturaleza:** *negocio* = auditoría de lo que ocurre en el dominio; *seguridad* = eventos de identidad, acceso y rechazo; *sincronización* = incidencias de las operaciones recibidas. Los eventos **[REC]** añaden alcance a RF-AUD-02 o RF-AUD-05 y requieren decisión del usuario; no reducen ninguna garantía.

| Evento | Naturaleza | Registro destino | Fuente | ¿Persistencia independiente de la unidad? |
|---|---|---|---|---|
| Operación **aplicada** (roles, torneo, reglamento, inscripciones, jugadores, calendario, partidos, resultados, correcciones, W.O., resoluciones manuales) | Negocio | Auditoría del torneo, **dentro de la misma unidad indivisible** | RF-AUD-01, RF-AUD-02 | **No:** se aplica y se registra juntos (IN-09-04) |
| Alta, baja, revocación o levantamiento de rol o asignación | Negocio | Historial de solo anexado (**fuente única: CMP-07**) y auditoría | RF-ACC-11, RF-AUD-02, RT-10 | No: dentro de la unidad |
| **Rechazo o bloqueo por RT-14** (vigente o histórico), bloqueo por falta de autoridad, intento de levantar sin autoridad | Seguridad | Registro de rechazos **del torneo** | RF-AUD-06 | **Sí [DER]:** persiste aunque la operación no se aplique |
| Intento denegado de escalada o de acceso cruzado | Seguridad | Bitácora de seguridad **global**, visible al dueño | RF-AUD-05 (prioridad **S**) | Sí (misma razón) |
| Operación sincronizada **rechazada o en conflicto** | Sincronización | Metadatos mínimos de incidencias del torneo | OFF-20, D-9 | **[ABIERTO]:** lo decide **ADR-05.C** (§16.7.4) |
| Registro de **idempotencia** de una operación | Sincronización | (de ADR-05.C) | OFF-06 | **[ABIERTO]:** lo decide **ADR-05.C** |
| **[REC] EV-01** Fallos de acceso repetidos; activación de un bloqueo temporal | Seguridad | Bitácora de seguridad | F-05 (éxitos y fallos de autenticación) | Sí |
| **[REC] EV-02** Alta, baja o uso de un segundo factor o de un medio de recuperación; cambio de contraseña o de correo | Seguridad | Bitácora de seguridad | F-05 | Sí |
| **[REC] EV-03** Revocación de sesiones | Seguridad | Bitácora de seguridad | RF-ACC-07, RF-ACC-16 | Sí |
| **[REC] EV-04** Rechazo de una operación sincronizada por falta de permisos | Sincronización | Metadatos de OFF-20 | OFF-11, OFF-22 | **[ABIERTO]** (ADR-05.C) |
| **[REC] EV-05** Exportación de datos personales de un jugador (PRV-08) | Negocio | Auditoría del torneo | PRV-08 (RF-AUD-02 ya incluye la anonimización, PRV-06) | No: dentro de la unidad |
| **[REC] EV-06** Acción de una tarea sin actor humano | Negocio | Auditoría, con su origen (IN-09-09) | RT-14, ADR-14 | No |

**Separación entre operaciones de negocio y eventos de seguridad.** La auditoría de **negocio** es del torneo, queda dentro de la unidad y la consulta quien RF-ADM-03 autoriza. Los eventos de **seguridad** son rechazos e intentos denegados; los de torneo (RF-AUD-06) se consultan por torneo y los **globales** (RF-AUD-05) solo los ve el dueño. Las **incidencias de sincronización** (OFF-20) las consulta el organizador de su torneo. **Ningún registro de seguridad contiene datos personales de menores ni secretos** (IN-09-07, IN-09-12).

#### 16.7.3 Invariantes

**A. Invariantes obligatorios** (vienen de un requisito aprobado de `01`, o se derivan de él):

| ID | Invariante | Tipo | Fuente |
|---|---|---|---|
| IN-09-01 | **Solo anexado:** ningún rol de la aplicación —incluido el dueño y el rol de servicio— actualiza ni borra registros de auditoría | [CONF] | RF-AUD-03, RC-21 |
| IN-09-02 | **El historial de asignaciones de rol no se elimina ni se altera:** una revocación cambia el estado y la fecha | [CONF] | RF-ACC-11 |
| IN-09-03 | **Se documenta el límite frente a un administrador de la base de datos** (la inmutabilidad por permisos no lo alcanza) | [CONF] | RF-AUD-03 |
| IN-09-04 | **Lo aplicado se registra dentro de la misma unidad indivisible:** si no puede registrarse, no se aplica | [DER] | §7.0 paso 6, RF-AUD-02, PA-14; RT-10 para los permisos |
| IN-09-05 | **El registro de un rechazo o bloqueo persiste aunque la operación no se aplique o se revierta** | [DER] | RF-AUD-06, §5.5, §7.0 paso 6 |
| IN-09-06 | Contenido mínimo (RF-AUD-01): actor (cuenta y rol con que actuó), **hora del servidor**, torneo, entidad, acción, valores antes y después minimizados, motivo cuando aplica y **origen (en línea o sincronizado, con identificador de la operación y hora del cliente)**. Para rechazos (RF-AUD-06): actor, acción, equipo, torneo, motivo y hora | [CONF] | RF-AUD-01, RF-AUD-06 |
| IN-09-07 | **El registro no contiene datos personales de menores más allá de identificadores internos** (RF-AUD-04; su criterio de aceptación en `01`: «sin nombres, fechas de nacimiento ni contactos»); los registros de rechazos tampoco los contienen (RF-AUD-06); tras anonimizar, ningún campo personal queda en la auditoría (PRV-06). **No se amplía el alcance a datos de adultos ni de usuarios**, cuya minimización fijan PRV-14 y ADR-09.I | [CONF] | RF-AUD-04, RF-AUD-06, PRV-06 |
| IN-09-08 | **Visibilidad por rol y por torneo:** el dueño ve todo; el organizador, solo sus torneos; editor y delegado no ven el historial; la bitácora de seguridad es global y no visible a organizadores | [CONF] | RF-ADM-03, RF-AUD-05, §6.2 |
| IN-09-10 | **Fuera de alcance:** ADR-09.C **no define** plazos de retención, anonimización ni la conservación de respaldos (ADR-17 y ADR-12, R4; PRV-10; PEND-JUR) | [DER] | §8.2, PRV-10 |
| IN-09-11 | **Sin permisos nuevos ni reducción de garantías** y **sin selección tecnológica** | [DER] / [CONF] | CR1-13, RC-23 |

**B. Propuestas [REC]** (no obligan hasta su aprobación):

| ID | Propuesta | Fuente |
|---|---|---|
| IN-09-09 | Las acciones sin actor humano se registran **con su origen**; la identidad de ejecución la define ADR-14 (R4) | RT-14, §3.1 |
| IN-09-12 | **Tampoco** se registran secretos, contraseñas, códigos del segundo factor, identificadores de sesión ni credenciales de servicio | F-05, PRV-12 |
| IN-09-13 | Los **efectos indirectos** de una operación se auditan junto con la operación que los origina (lectura **provisional** de `01 §13.7` para A-15; **no es una política definitiva**) | `01 §13.7` (A-15), RF-ACC-11, RF-AUD-06 |

#### 16.7.4 Formulaciones de capacidad (alternativas) y su conformidad

**Decisión D9-1 — Persistencia independiente del registro de un rechazo** (IN-09-05). **No se presentan como alternativas viables equivalentes**: solo P-A es plenamente conforme.

| ID | Formulación | IN-09-05 (persiste aunque no se aplique) | Redacción de R1 (§7.0 paso 6, §5.5: «de forma independiente de la unidad») | Rechazos impuestos por la capa de datos (RC-04) | Conclusión |
|---|---|---|---|---|---|
| CAP-09-P-A | **Unidad independiente posterior:** si la operación se rechaza o se revierte, **otra unidad de persistencia**, distinta de la de la operación, registra el rechazo y se confirma **antes** de devolver el rechazo | **Conforme** | **Conforme** | **Conforme:** cubre también la transacción abortada, porque el registro va en otra unidad | **Plenamente conforme** |
| CAP-09-P-B | **El rechazo como resultado confirmado:** cuando el evaluador de autorización decide rechazar, la unidad **no se revierte** y **confirma** una unidad cuyo único efecto es el registro del rechazo | Conforme **solo** para los rechazos que decide el evaluador (CMP-08) | **No conforme con el texto literal:** R1 dice que el registro es independiente de la unidad; adoptar P-B exigiría ajustar esa redacción (edición separada) | **No conforme:** un rechazo que **aborta** la transacción (restricción de integridad, política de acceso) no puede registrarse en esa misma unidad | **No es una alternativa independiente:** variante parcial que no cubre los rechazos de la capa de datos y diverge de R1 |
| CAP-09-P-C | **Canal de eventos separado con entrega garantizada al menos una vez:** el rechazo se entrega a un registro distinto de la auditoría de la operación, con reconciliación | **Condicionada:** conforme **solo si** la intención de registro se hace **durable antes de responder**; una ventana de pérdida **no** cumple IN-09-05 | Conforme | Conforme si la intención es durable antes de responder | **Viable con condición** (sin ventana de pérdida) |

**Decisión D9-2 — Integridad de la auditoría y del historial** (IN-09-01, IN-09-03). **Las tres cumplen los invariantes**; difieren en cuánto reducen el límite del administrador.

| ID | Formulación | IN-09-01 | IN-09-03 | Conclusión |
|---|---|---|---|---|
| CAP-09-M1 | **Inmutabilidad por permisos:** ningún rol de la aplicación puede actualizar ni borrar. **Es lo mínimo exigido** (RF-AUD-03); el límite frente al administrador queda **documentado** | Conforme | Conforme (lo documenta) | **Viable**; no reduce el límite |
| CAP-09-M2 | **M1 más detección de alteración:** cada registro lleva un elemento que permite **detectar** una modificación o un borrado posterior (ejemplo ilustrativo: encadenamiento o sellado verificable), de modo que el límite deja de ser silencioso | Conforme | Conforme (y lo reduce) | **Viable** |
| CAP-09-M3 | **M2 más una copia externa de solo anexado**, mantenida fuera del control del administrador | Conforme | Conforme (y lo reduce más) | **Viable**, con más complejidad |

**Comparación cualitativa** (criterios de §2.4; juicio del autor [SUP], sin datos de costo; se omiten las formulaciones no conformes):

| Formulación | Costos bajos | Seguridad | Funcionamiento sin conexión | Mantenimiento |
|---|---|---|---|---|
| CAP-09-P-A | Favorable | Neutral (exige confirmar antes de responder) | No aplica | Neutral |
| CAP-09-P-C | Desfavorable | Neutral (con la condición de durabilidad) | No aplica | Desfavorable |
| CAP-09-M1 | Favorable | Neutral (no cubre el límite del administrador) | No aplica | Favorable |
| CAP-09-M2 | Neutral | Favorable | No aplica | Neutral |
| CAP-09-M3 | Desfavorable | Favorable | No aplica | Desfavorable |

**Relación con ADR-02.C y ADR-05.C (H2A-06).** §5.5 y §8.1 atribuyen la persistencia de OFF-06 y OFF-20 a «ADR-05.C y ADR-09.C», mientras §8.2 solo tiene **ADR-05.C requiere ADR-09.C**. **Se conserva provisionalmente la separación:** **ADR-09.C define la capacidad general** (D9-1: registrar de forma independiente de una unidad) y **ADR-05.C decide si esa capacidad se aplica** al registro de idempotencia (OFF-06) y a los metadatos de incidencias (OFF-20), y cómo. ADR-09.C **no resuelve** esos dos casos y **no depende** de ADR-05.C. **ADR-02.C requiere ADR-09.C** (§8.2): R2-B tomará como entrada lo que ADR-09.C proponga; si se adopta P-B, ADR-02.C y la redacción de R1 deberán ajustarse (§16.8.4).

#### 16.7.5 Eventos de seguridad: qué registrar y qué no

Según F-05, deben registrarse al menos los éxitos y fallos de autenticación, los fallos de autorización y los cambios de privilegios; **no deben registrarse** los tokens de acceso, las contraseñas ni los datos personales sensibles, y los registros deben protegerse contra manipulación y accesos no autorizados. Aplicado a este proyecto **[REC]**:

| Se registra (mínimo) | No se registra |
|---|---|
| Identificador interno de la cuenta, tipo de evento, torneo cuando aplica, hora del servidor, resultado y motivo | Contraseñas, códigos del segundo factor, identificadores o secretos de sesión, credenciales de servicio (IN-09-12) |
| Origen de la operación (en línea o sincronizada) | Datos personales de menores más allá de identificadores internos (RF-AUD-04) |
| Intentos denegados de escalada y de acceso cruzado (RF-AUD-05) | Más direcciones de correo que las imprescindibles (PRV-14); la auditoría **no es telemetría** (PRV-12) |

#### 16.7.6 Separación entre auditoría, retención, anonimización y decisiones reservadas

| Asunto | Dueño | ¿Lo decide ADR-09.C? |
|---|---|---|
| Qué se registra y su inmutabilidad | **ADR-09.C** | Sí (propone) |
| Persistencia independiente del rechazo | **ADR-09.C** (capacidad general) | Sí (propone) |
| Persistencia de idempotencia (OFF-06) y de OFF-20 | **ADR-05.C** (R2-C) | **No** |
| Qué se conserva, por cuánto tiempo y cómo se anonimiza; respaldos | **ADR-17**, **ADR-12** (R4); PEND-JUR | **No** |
| Identidad de ejecución sin actor humano | **ADR-14** (R4) | **No** (solo exige el origen, IN-09-09) |
| Implementación dependiente del proveedor (ADR-09.I) | R3, tras ADR-01 | **No** |

#### 16.7.7 Consecuencias y riesgos

1. La inmutabilidad por permisos **no protege frente a quien administra la base de datos o la infraestructura** (AM-24, RES-06): solo M2 y M3 reducen ese riesgo, a cambio de complejidad.
2. Si **RF-AUD-05** (prioridad S) se recortara, los intentos denegados de escalada no tendrían bitácora; el núcleo (IN-09-01, 04 y 05) **no depende de ella** (H2A-04).
3. P-A impone una **disciplina de diseño** sobre la frontera atómica; **ADR-02.C (R2-B) toma como entrada** lo que ADR-09.C proponga y deberá confirmarla. ADR-09.C **no depende** de ADR-02.C (§8.2).
4. El volumen de registros por inundación de rechazos (CAB-14) debe considerarse en ADR-09.I y ADR-13.
5. **Se conserva la auditoría mientras exista el torneo más el plazo jurídico** (PRV-10); ese plazo no está fijado (PEND-JUR).

#### 16.7.8 Condicionado a R3 (ADR-09.I)

Qué ofrece el proveedor para un registro de solo anexado, para sellado o detección de alteración, para copias externas y para conservar registros en la región elegida; si la unidad de datos permite P-A; cómo se protege el acceso del administrador. **[ABIERTO R3.]** Nada se verificó.

#### 16.7.9 Fallo de la persistencia de un rechazo y relación con OFF-06 (R-11) — [ABIERTO]

**Problema.** IN-09-05 exige que el registro de un rechazo persista aunque la operación no se aplique. **No se dice qué ocurre si ese registro no puede persistirse** (falla del almacén de auditoría, saturación u otra causa) **ni cómo se concilia con OFF-06** («reenviar el mismo identificador … sin repetir efectos ni registros de auditoría»). Esta sección **solo presenta alternativas y consecuencias de seguridad**: **no asume ningún mecanismo, no adopta ninguna alternativa y no declara aceptable la pérdida de auditoría.**

**Alternativas ante el fallo del registro**

| ID | Alternativa | Consecuencia de seguridad | Conformidad con IN-09-05 |
|---|---|---|---|
| F-1 | El rechazo se devuelve igualmente y se continúa sin registro | **Pérdida de auditoría de un rechazo** (RT-14, intentos de escalada): quien pueda provocar el fallo podría intentar acciones sin dejar rastro. **No se declara aceptable** | **No conforme** |
| F-2 | No se devuelve un rechazo definitivo: se devuelve un error recuperable y la operación queda sin resolver hasta poder registrar | Se conserva la auditoría. **Riesgo de disponibilidad:** una falla del registro bloquea las operaciones rechazadas y permite una denegación inducida; las operaciones sincronizadas siguen «pendiente» y se reintentan (OFF-05) | Conforme |
| F-3 | La intención de registro se persiste primero en un medio durable distinto y se concilia después; si ese medio tampoco está disponible, se aplica F-2 | Reduce la ventana de pérdida y la dependencia de un único almacén; **añade complejidad y una ventana hasta la conciliación** | Condicionada a que la intención sea durable antes de responder |

**Aviso de operación [REC].** Un fallo del registro de rechazos debería generar una alerta (ADR-13, R4) y contarse (CAB-14).

**Relación con OFF-06.** OFF-06 exige que reenviar el mismo identificador devuelva el resultado original sin repetir efectos ni registros de auditoría. **`01` no fija cómo se aplica a una operación rechazada.** Alternativas:

| ID | Alternativa | Consecuencia |
|---|---|---|
| O-1 | **Un único registro por identificador de operación:** el reenvío devuelve el rechazo original y no genera otro registro | Cumple OFF-06 al pie de la letra y limita la inundación (CAB-14); **requiere conservar el resultado de las rechazadas**, punto abierto de ADR-05.C (§5.5, OFF-20); se pierde la visibilidad de los reintentos abusivos |
| O-2 | **Un registro por cada intento**, incluidos los reenvíos | Conserva la visibilidad de los reintentos, pero **repite registros** (OFF-06 lo prohíbe para lo aplicado y no dice si aplica a lo rechazado) y agrava la inundación |
| O-3 | **Un registro por identificador más un contador de reenvíos**, sin registros nuevos | Conserva la visibilidad sin repetir registros; añade un campo a decidir |

**Quién decide:** ADR-05.C (R2-C) y el usuario (decisión 10 de §16.10.3). **No se resuelve en R2-A.**

### 16.8 Trazabilidad y verificación de dependencias

#### 16.8.1 Requisitos y decisiones aprobadas → artefacto → amenazas

| Requisito o decisión | ADR-03.C | ADR-09.C | Amenazas | Casos de abuso |
|---|---|---|---|---|
| RF-ACC-01, 02 (invitación) | IN-03-05 | — | AM-20 | CAB-09 |
| RF-ACC-07, D-2 (revocación) | IN-03-06, 07 · CAP-03-V · §16.6.4 | — | AM-03, AM-18 | CAB-05, CAB-19 |
| RF-ACC-09, D-4 (dueño) | IN-03-04 · CAP-03-N · CAP-03-R | — | AM-02, AM-05, AM-32 | CAB-09, CAB-10, CAB-22 |
| RF-ACC-10, RC-11, HR1-04 (2FA) | IN-03-02, 03, 09 · PAR-01/02/06 | — | AM-02, AM-05, AM-06 | CAB-09, CAB-15 |
| Notificación al titular (F-01, F-04; RF-ACC-09 deja la del dueño como [REC] no decidida) | IN-03-21 [REC] | — | AM-02, AM-20 | CAB-09 |
| RF-ACC-16 (sesión) | IN-03-08, 12, 16 · PAR-03 | — | AM-03 | CAB-19 |
| OFF-03, OFF-12, OFF-22, OFF-23, RC-08 | IN-03-10, 11, 19, 20 · CAP-03-S | — | AM-09, AM-33 | CAB-05, CAB-06, CAB-30 |
| RF-ACC-11, RC-21 | — | IN-09-01, 02 | AM-16 | CAB-13 |
| RF-AUD-01, 04, 06 | — | IN-09-06, 07 | AM-17 | CAB-14 |
| RF-AUD-03 | — | IN-09-01, 03 · CAP-09-M | AM-16, AM-24 | CAB-13 |
| RF-AUD-05 (S) | — | §16.7.2 | AM-17 | CAB-02 |
| RT-10, RT-13, §7.0 paso 6 | — | IN-09-04, 05 · CAP-09-P | AM-17, AM-18 | CAB-07, CAB-14 |
| PRV-10, PEND-JUR | — | IN-09-10 (fuera de alcance) | AM-30 | CAB-17 |
| PRV-12 | — | IN-09-07, 12 · §16.7.5 | AM-25 | CAB-11, CAB-20 |
| RNF-14, RNF-17 | — (no es de aplicación) | — | AM-23, AM-32 | CAB-21, CAB-22 |
| PRV-18 | — | — (es de ADR-04.C) | AM-31 | CAB-12 |

#### 16.8.2 Elementos abiertos de Fase 1 y su tratamiento en R2-A

**R2-A no resuelve ninguno.** Ni ADR-03 ni ADR-09 figuran en §8.1 como dependientes de una pregunta abierta; las interacciones indirectas se aíslan.

| Elemento | ¿Toca ADR-03.C o ADR-09.C? | Tratamiento |
|---|---|---|
| Preguntas abiertas (las 18) | **No como dependencia directa.** Q-42 y Q-43 afectan ADR-02 y ADR-05 | Siguen abiertas; ADR-03.C no fija quién puede hacer qué (IN-03-17) |
| Requisitos `COND` (los 23) | **No** | Siguen condicionados |
| A-10 | Indirecta (sincronizar) | ADR-03.C soporta ambas lecturas (§16.6.6) |
| A-15 | Indirecta (revocación y auditoría de efectos indirectos) | ADR-03.C separa sesiones de roles. **IN-09-13 [REC] recoge la lectura provisional** de `01 §13.7` (no definitiva) y IN-09-06 define los campos del registro; se resuelve en ADR-02.C y ADR-05.C |
| A-1, A-2, A-4, A-5, A-6, A-12, A-16 | Indirecta (qué se rechaza y se audita) | ADR-09.C registra el rechazo sea cual fuere la lectura |
| HR1-02 (RF-ACC-17 es S) | Sí | El historial y su inmutabilidad son núcleo (IN-09-01, 02) |
| HR1-03, HR1-11, HR1-14 | Indirecta | Se tratan en R2-C; ver RES-02 y RES-04 |
| HR1-04 | **Sí** | CAP-03-S, IN-03-02 |
| HR1-07 (iOS) | Indirecta | §16.9; IOS-01…08 |

#### 16.8.3 Dependencias entre ADR (contraste con la matriz de §8.2)

La matriz vigente de §8.2 **no se modifica**. Contraste:

| Artefacto | Requiere (§8.2) | ¿Cambia R2-A? | Lo requieren (§8.2) |
|---|---|---|---|
| ADR-03.C | — | **No** | ADR-02.C, ADR-05.C, ADR-03.I, ADR-15; ADR-01 y ADR-10 (decisión conjunta) |
| ADR-09.C | — | **No** | ADR-02.C, ADR-05.C, ADR-09.I, ADR-17; ADR-01 y ADR-10 (decisión conjunta) |

**Contradicciones del esquema preparatorio** (la propuesta de trabajo de R2, presentada antes de la autorización de R2-A) **corregidas solo en este material nuevo:**

1. El diagrama preparatorio mostraba una flecha **ADR-04.C → ADR-05.C**. **No existe en §8.2**: ADR-05.C requiere ADR-02.C, ADR-03.C y ADR-09.C; ADR-04.C requiere ADR-02.C y ninguno de los dos depende del otro. **Se descarta** la flecha del esquema.
2. El esquema atribuía a ADR-09.C la decisión de la persistencia de OFF-06 y OFF-20. Eso, junto con «ADR-05.C requiere ADR-09.C», habría creado una dependencia recíproca. **Se corrige** con la partición de §16.7.4.
3. El esquema listaba el «nivel exigido al sincronizar» como asunto de ADR-05.C. §8.1 lo asigna a **ADR-03**. **Se corrige:** ADR-03.C fija el nivel (CAP-03-S) y ADR-05.C **consume** esa decisión (ADR-05.C requiere ADR-03.C).

**Verificación mecánica** de la matriz de §8.2 (23 nodos, 52 relaciones, sin ciclos ni pares recíprocos): resultado en §16.12.

#### 16.8.4 Ajustes editoriales futuros que requieren autorización separada (H2A-06)

R2-A **no modifica R1** y conserva provisionalmente la separación entre ADR-05.C y ADR-09.C. Si el usuario lo decide, cada ajuste necesita su propia autorización:

| Ajuste | Dónde | Cuándo se necesitaría |
|---|---|---|
| Alinear la atribución de la persistencia de OFF-06 y OFF-20 («ADR-05.C y ADR-09.C») con la partición de §16.7.4 | §5.5 y §8.1 | Si se acepta la partición |
| Precisar en §2.1 que «iOS sin sincronización en segundo plano» procede de OFF-05 y RK-11 y no del texto de `01 RNF-07`, que solo fija versiones mínimas | §2.1 (resumen de RNF-07) | Opcional, con autorización separada |
| Ajustar la redacción «de forma independiente de la unidad» | §7.0 paso 6 y §5.5 | Solo si se adoptara CAP-09-P-B |
| Actualizar el estado de R2, ADR-03 y ADR-09 | Encabezado (línea 5), §8.1, §12, §13 | Al aprobar R2-A |
| Registrar las autorizaciones y trabajos de R2-A | `aprobaciones.md` y `bitacora.md` | **Realizado el 2026-11-17** (decisión 11) |

#### 16.8.5 Regla de precedencia entre `.C` y `.I` (R-12) — [REC], propuesta pendiente de decisión

**Esta regla es una propuesta [REC] pendiente de decisión (decisión 6), no una decisión ni un requisito, y no relaja ninguna restricción aprobada de R1:** la matriz de §8.2 y RC-25 siguen vigentes tal como se aprobaron. Las partes `.C` definen en R2 las **capacidades e invariantes obligatorios**; las partes `.I` (R3) determinan **cómo implementarlos con el stack seleccionado**. Para que la matriz de §8.2 siga sin ciclos y RC-25 se cumpla:

1. **ADR-xx.I requiere ADR-xx.C** y ADR-01 (§8.2); **ADR-xx.C no requiere ni espera a ninguna parte `.I` ni a ADR-01**.
2. **Una parte `.I` no puede redefinir, relajar ni reinterpretar un invariante `.C` aprobado.** Solo elige entre las formulaciones que el usuario haya admitido en R2 y las implementa.
3. Si una capacidad admitida resulta **inviable o desproporcionada** con el stack seleccionado, **no se resuelve en la parte `.I`**: se devuelve al usuario, que decide si se modifica el `.C` (con una autorización específica) o se elige otra formulación; ADR-01 no puede dar por cumplida una capacidad `.C` que el stack no cumple sin esa decisión.
4. Lo que el `.C` deja **expresamente pendiente de proveedor** (§16.6.9, §16.7.8) es lo único que la parte `.I` determina por sí misma, siempre dentro de los invariantes.

Esta regla **no añade relaciones** a §8.2.

### 16.9 Registro de fuentes consultadas y limitaciones

**Autorización:** consultar documentación oficial en internet para verificar capacidades generales de seguridad, autenticación e iOS. **Fecha de consulta del autor: 2026-11-16 (descarga a las 04:24 UTC).** Los revisores de la auditoría y de la revisión focalizada volvieron a consultar las fuentes el 2026-11-17 y **confirmaron las cifras citadas** (con las salvedades de §16.14); su lectura también fue parcial. **Método y limitaciones:**

- La herramienta de lectura web **no pudo resolver los dominios** (error de DNS). Con **aprobación expresa del usuario** se añadieron a la lista de red permitida del entorno solo estos hosts: `pages.nist.gov`, `csrc.nist.gov`, `cheatsheetseries.owasp.org`, `webkit.org`, `developer.mozilla.org` y `www.w3.org`. Las páginas se descargaron después con `curl` por el proxy del entorno y se leyeron como **texto extraído automáticamente**.
- La lectura fue **parcial y por palabras clave**, no una revisión completa de cada documento. **Los pasajes citados son paráfrasis**; hay que confirmarlos en la fuente antes de usarlos como requisito. **No se afirma que se leyó íntegramente ninguna publicación.**
- **Ningún documento consultado es obligatorio para el proyecto.** F-01 está escrita para sistemas del gobierno de EE. UU.; se usa solo como **referencia general** (H2A-05).
- **No se instaló nada, no se accedió a cuentas ni a servicios, y no se validó ningún dispositivo.**

| ID | Fuente | URL | Fecha o versión que muestra la página | Afirmaciones de esta parte que respalda | Limitaciones |
|---|---|---|---|---|---|
| F-01 | NIST SP 800-63B (revisión 4), *Digital Identity Guidelines: Authentication and Authenticator Management* | `https://pages.nist.gov/800-63-4/sp800-63b.html` | Marca de fecha de la página: «26 Aug 2025»; **no se verificó la fecha de publicación de la revisión** | PAR-01, 02, 03, 04, 06; IN-03-12, 14 (autenticadores restringidos), 15, 21 | Lectura parcial; texto extraído automáticamente |
| F-02 | OWASP Cheat Sheet Series, *Session Management* | `https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html` | Sin fecha visible en el texto extraído | PAR-03; IN-03-12, 16 | Guía de buenas prácticas, no norma |
| F-03 | OWASP, *Authentication* | `https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html` | Sin fecha visible | PAR-02; IN-03-13 | Ídem |
| F-04 | OWASP, *Multifactor Authentication* | `https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html` | Sin fecha visible | PAR-06; IN-03-14, 15, 16, 21 | Su texto sobre mensajes de texto y de voz es literal («Do not use SMS for high-value or PII-handling applications»); la frase de que NIST «discourages their use for applications containing PII» aparece en F-04 y **no** en F-01, que los clasifica como autenticador restringido |
| F-05 | OWASP, *Logging* | `https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html` | Sin fecha visible | §16.7.5; IN-09-12; EV-01, 02; CAP-09-M2 (recomienda detectar la alteración) | Ídem |
| F-06 | NIST, página de la publicación SP 800-53 Rev. 5 | `https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final` | «Date Published: September 2020 (includes updates as of Dec. 10, 2020)» | **Ninguna:** consultada y no utilizada; **el texto de los controles AU-9 a AU-12 no se obtuvo** | Solo la ficha de la publicación; **controles AU no verificados** |
| F-07 | WebKit, *Updates to Storage Policy* | `https://webkit.org/blog/14403/updates-to-storage-policy/` | **10 ago. 2023** (Safari 17.0, iOS 17) | IOS-03, 04, 05, 07 | Describe versiones concretas |
| F-08 | WebKit, *Tracking Prevention* | `https://webkit.org/tracking-prevention/` | Sin fecha visible en el texto extraído | IOS-01, 02 | Ídem |
| F-09 | MDN, *Storage quotas and eviction criteria* | `https://developer.mozilla.org/en-US/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria` | «Last modified Jan 5, 2026» | IOS-01, 04, 05, 08 | Documentación de terceros, no del fabricante |
| F-10 | MDN, *StorageManager: persist()* | `https://developer.mozilla.org/en-US/docs/Web/API/StorageManager/persist` | «Last modified Oct 12, 2026» | IOS-04 | Ídem |
| F-11 | MDN, *Background Synchronization API* | `https://developer.mozilla.org/en-US/docs/Web/API/Background_Synchronization_API` | «Last modified Apr 22, 2024»; marcada «Limited availability» | IOS-06 | **La tabla de compatibilidad requiere JavaScript y no se leyó:** no se verificó qué navegadores la ofrecen |
| F-12 | W3C, *Web Authentication Level 3* | `https://www.w3.org/TR/webauthn-3/` | **W3C Recommendation, 25 agosto 2026** | Contexto de IN-03-14 (existencia de credenciales de clave pública resistentes al phishing como estándar) | Documento largo: lectura muy parcial |
| F-13 | MDN, *Web Authentication API* | `https://developer.mozilla.org/en-US/docs/Web/API/Web_Authentication_API` | «Last modified Oct 27, 2026» | Contexto de IN-03-14 (resistencia al phishing vinculada al origen) | Ídem F-09 |
| F-14 | MDN, *HTTP cookies* | `https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies` | «Last modified Sep 17, 2026» | **Ninguna afirmación de esta parte:** contexto para ADR-03.I (atributos de seguridad de las credenciales de sesión) | Ídem F-09 |

**Afirmaciones sobre iOS** (excepción de §2.5, RC-24): **todas [NO VERIF]: no verificadas en dispositivo.** La documentación describe lo que el fabricante o el autor de la referencia **dice**; una prueba real debe confirmar lo que ocurre en el dispositivo. Se verifican en iPhone y Android reales en 5E y la Fase 7 (y en 5G, por `01 RK-11`).

| ID | Afirmación según la fuente (documental) | Fuente | Qué exige la **prueba real** | Efecto en R2-A |
|---|---|---|---|---|
| IOS-01 | Safari elimina los datos creados por scripts de un origen **sin interacción del usuario en los últimos 7 días**; las cookies fijadas por el servidor están exentas | F-08, F-09 | Medir en un iPhone real tras 7 días sin uso, en el navegador y en la aplicación instalada | AM-11: la cola local puede perderse. Afecta ADR-05.C y ADR-16.C (R2-C) |
| IOS-02 | La **aplicación web de la pantalla de inicio** de iOS queda **exenta de ese límite de 7 días** | F-08 | Confirmar la exención con la aplicación instalada | Favorece la instalación como PWA (RF-PWA-01) |
| IOS-03 | Una aplicación web de la pantalla de inicio tiene las **mismas cuotas** que en el navegador | F-07 | Medir cuota real | Sin efecto en R2-A |
| IOS-04 | El modo predeterminado es de **mejor esfuerzo** (puede desalojarse); WebKit concede `persist()` según heurísticas, **como que sea una aplicación de la pantalla de inicio**; Safari aprueba o rechaza según el historial de interacción, **sin preguntar** | F-07, F-09, F-10 | Comprobar si se concede y cuándo | OFF-17: la solicitud puede no concederse y la interfaz debe avisar |
| IOS-05 | El desalojo **elimina todos los datos de un origen** a la vez, no una parte | F-07, F-09 | Provocar presión de almacenamiento en un dispositivo real | Un desalojo borra la cola entera: refuerza el aviso de OFF-17 |
| IOS-06 | La sincronización en segundo plano tiene «**disponibilidad limitada**» según MDN; la tabla de compatibilidad no se leyó. **OFF-05 y RK-11 de `01` ya establecen que iOS no la ofrece; RNF-07 solo fija las versiones mínimas (iOS 16.4)** | F-11 | Confirmar en dispositivo | Sin efecto: RC-05 ya no depende de ella |
| IOS-07 | **El mínimo de `01 RNF-07` es iOS 16.4**, pero F-07 describe cuotas y la API de almacenamiento «plenamente soportada» **desde Safari 17.0 e iOS 17**. El comportamiento de `persist()`, de la cuota y del desalojo en **iOS 16.4 no está documentado en las fuentes leídas** | F-07 | Probar en un iPhone con iOS 16.4 | ADR-05.C y ADR-16.C no pueden suponer lo de iOS 17 para el mínimo soportado |
| IOS-08 | En la UE (iOS 17.4 en adelante) los navegadores alternativos pueden usar su propio motor y sus propias políticas | F-09 | Comprobar los navegadores alternativos de usuarios en la UE (si aplica) | Sin efecto directo: el piloto es en Costa Rica (RC-01) |

**Capacidad dependiente del proveedor (ADR-03.I, ADR-09.I):** ninguna se verificó. Queda condicionada a R3 (§16.6.9, §16.7.8).

### 16.10 Riesgos residuales, hallazgos y decisiones pendientes

#### 16.10.1 Riesgos residuales

«Residual» es lo que queda aunque los controles se cumplan. **Ninguno está aceptado** (§16.1): la aceptación es del usuario. El nivel residual es una **estimación [SUP]**. «Responsable» es documental.

| ID | Riesgo residual | Amenazas | Nivel residual estimado [SUP] | Responsable | Aceptación |
|---|---|---|---|---|---|
| RES-01 | El compromiso del correo de una persona equivale a la toma de su cuenta si la recuperación no exige otro medio; la recuperación por procedimiento organizativo (R-B) es vulnerable a la ingeniería social; **el aviso al titular (IN-03-21) reduce la ventana de detección pero no la elimina** | AM-02, AM-20 | Medio | ADR-03.C; ADR-15 (R3) | Pendiente |
| RES-02 | **Las copias locales de vistas públicas y datos preparados no pueden purgarse a distancia** (HR1-11) | AM-28 | Alto, **no eliminable por diseño** | R2-C; asesoría jurídica | Pendiente |
| RES-03 | **Los respaldos conservan datos anteriores a una anonimización** (HR1-12) | AM-30 | Alto, **no eliminable por diseño** | R4 (ADR-12, ADR-17); asesoría jurídica | Pendiente |
| RES-04 | **El archivo exportado llega al dispositivo y no puede purgarse** (HR1-14) | CAB-18 (AM-13) | Medio, **no eliminable por diseño** | R2-B, R2-C; asesoría jurídica | Pendiente |
| RES-05 | **D-2:** el objetivo de ≤ 60 s **no es una garantía** para dispositivos desconectados, **no puede medirse en la Fase 2** (H2A-01) y **no se demuestra en segundo plano o suspendido (S2)**, cuyo estatus `01` no define (H2A-10) | AM-03 | Medio | ADR-03.C; usuario (§16.6.4) | Pendiente |
| RES-06 | **Quien administra la base de datos o la infraestructura puede alterar la auditoría** sin que la aplicación lo impida (límite de RF-AUD-03) | AM-24 | Medio con M1; Bajo con M2 o M3 | ADR-09.C | Pendiente |
| RES-07 | **Pérdida de capturas por desalojo del almacenamiento local** (OFF-17, RK-11). **RK-15 no la cubre:** acepta la pérdida por rechazo de operaciones con autorización revocada (D-11); **RK-11 no figura aceptado** | AM-11 | Medio | R2-C | **Pendiente** |
| RES-08 | Identificación indirecta de menores por combinación de datos (PRV-18) | AM-31 | Alto | R2-C; asesoría jurídica | Pendiente |
| RES-09 | Los parámetros PAR-01…06 son propuestas **sin validación empírica**; su efecto real se conocerá en la Fase 7 | AM-02, AM-03, AM-05 | Medio | Usuario; Fase 7 | Pendiente |
| RES-10 | **Los delegados, con 2FA opcional (decisión de `01`), acceden a nombres internos e indicador de menores (PRV-09):** la toma de una cuenta de delegado expone datos internos de menores. `01` prevé reevaluar la obligatoriedad tras el piloto | AM-02 | Alto | ADR-03.C; usuario (reevaluación de RF-ACC-10) | **Pendiente** |
| RES-11 | **Compromiso de credenciales administrativas y falta de sucesión** de accesos de la plataforma | AM-23, AM-32 | Medio | R3 (ADR-01); R4 (ADR-11, ADR-12); Fase 8 | Pendiente |
| RES-12 | **El bloqueo por intentos fallidos puede usarse para dejar sin acceso al único dueño**, incluso de forma temporal (PAR-02 es una propuesta); la recuperación depende de RF-ACC-09 y de los procedimientos de §16.6.3 | AM-05 | Medio | ADR-03.C | Pendiente |

#### 16.10.2 Hallazgos de R2-A

| ID | Hallazgo | Tratamiento |
|---|---|---|
| H2A-01 | **D-2 pide validar el objetivo de ≤ 60 s «en la Fase 2»** y la trazabilidad de `01` lo asocia a «I: medición del objetivo de 60 s en Fase 2», pero la Fase 2 no admite código ni pruebas (RC-22) | **No se concede excepción ni se traslada la medición.** §16.6.4 documenta lo verificable y lo que exige medición, indica **qué opciones requieren excepción formal** y que **ninguna cumple D-2 literalmente en la Fase 2**. **Decisión del usuario** |
| H2A-02 | RF-ACC-10 y RF-ACC-16 dicen que los parámetros «se fijan en la Fase 2» | **No se fija ninguno.** PAR-01…06 están clasificados (§16.6.5); todos requieren aprobación |
| H2A-03 | §5.6 prohíbe persistir el **secreto de credenciales** en Z0 y admite la «sesión local» mínima de OFF-23, pero **no menciona el secreto de portador de la sesión** | IN-03-12 lo precisa como **[REC]**; **no modifica §5.6** |
| H2A-04 | RF-AUD-05 es de prioridad **S** | El núcleo de ADR-09.C no depende de ella (§16.7.7) |
| H2A-05 | Las fuentes de §16.9 son referencias generales; **ninguna es obligatoria** y la lectura fue parcial | Los valores de §16.6.5 son propuestas del autor, no cumplimiento de una norma |
| H2A-06 | R1 atribuye la persistencia de OFF-06 y OFF-20 a «ADR-05.C y ADR-09.C», mientras §8.2 solo tiene ADR-05.C → ADR-09.C | **Se conserva provisionalmente la partición** de §16.7.4 sin modificar R1 ni introducir dependencias recíprocas; **no es una decisión definitiva** (decisión 3); los ajustes editoriales futuros se listan en §16.8.4 |
| H2A-07 | La lectura web requirió ampliar la lista de red permitida del entorno (6 hosts) | Con aprobación expresa del usuario; consta en §16.9 |
| H2A-08 | Muchas capacidades de ADR-03.I y ADR-09.I podrían **no ser viables** con el proveedor que se elija | Se condicionan a R3; ninguna formulación presupone viabilidad; si una capacidad admitida resulta inviable **se propone que vuelva al usuario** (§16.8.5, propuesta [REC] pendiente de la decisión 6) |
| H2A-09 | **El alcance de «toda nueva operación» (RF-ACC-07, D-2) respecto de las lecturas de datos internos** (PRV-09) no está fijado | **[ABIERTO].** §16.6.6 propone leerlo como incluido [REC]. Decisión del usuario o de ADR-02.C |
| H2A-10 | **`01` no define «sesión conectada»**: no dice si una aplicación en segundo plano o un dispositivo suspendido con conexión cuenta como conectado (S2) | **[ABIERTO].** R2-A **no reduce el alcance**; §16.6.3 y §16.6.4 distinguen S1, S2 y S3. Decisión del usuario |
| H2A-11 | **`01` no define «nueva autenticación con 2FA»** (RF-ACC-09, CL-38): ¿«reciente» (L1) o «propia de la operación» (L2)? | **[ABIERTO].** De ello depende la conformidad de CAP-03-N-B (§16.6.3). Decisión del usuario |
| H2A-12 | **Proporcionalidad y priorización (R-03):** con la matriz de §16.1 y la convención «antes de controles», todo impacto Crítico con probabilidad Media o Alta da riesgo Crítico y solo la probabilidad Baja discrimina; **19 de las 34 amenazas son Críticas** y solo 4 de ellas son de R2-A (AM-02, AM-03, AM-05, AM-20). El recuento **depende de la convención** (§16.3.2): puntuar la probabilidad condicionada al fallo de los controles daría menos Críticas, y **esa lectura no se adopta**. Además, AM-01 puntúa el **intento**, no el éxito (§16.3.1) | **Registrado sin resolver.** Opciones para el usuario: mantener la clasificación; añadir un orden secundario dentro de las Críticas; refinar la escala de probabilidad; reclasificar AM-01 como parte de AM-15. Decisión 9 |
| H2A-13 | **Fallo de la persistencia de un rechazo y relación con OFF-06 (R-11)** | **Registrado sin resolver:** alternativas y consecuencias de seguridad en §16.7.9. Decisión 10 |
| H2A-14 | **Registro documental de R2-A (R-18):** las versiones anteriores señalaban que `aprobaciones.md` y `bitacora.md` seguían mostrando R2 «NO INICIADOS» | **Atendido:** registro documental realizado en `aprobaciones.md` y `bitacora.md` el 2026-11-17. R2-A continúa pendiente de aprobación formal |

#### 16.10.3 Decisiones que requieren aprobación del usuario

Siguen **PENDIENTES**: R2-A no resuelve ninguna.

| # | Decisión | Opciones | Dónde |
|---|---|---|---|
| 1 | **Tratamiento formal de D-2** (H2A-01; asunto 1a) y alcance de «sesión conectada» (H2A-10; asunto 1b, distinto) | (a) excepción formal para trasladar la medición a 5C o a la Fase 7; (b) validación documental reforzada y medición posterior (también requiere excepción formal); (c) reformular D-2 o RF-ACC-07 con modificación formal de `01`; (d) mantener D-2 abierto (bloquea el cierre de la Fase 2). **Ninguna cumple D-2 literalmente dentro de la Fase 2** | §16.6.4 |
| 2 | **Parámetros PAR-01…06** (H2A-02) | Aprobar todos; aprobar solo los respaldados por `01` y por las fuentes; dejar el resto a ADR-03.I; pedir revisión de PAR-03, 04 y 05 | §16.6.5 |
| 3 | **Partición definitiva ADR-05.C / ADR-09.C** (H2A-06) | Aceptar la partición y dejar R1 como está; autorizar después una edición mínima de R1; rechazarla (sin crear una relación recíproca) | §16.7.4, §16.8.4 |
| 4 | **2FA de los delegados** (RES-10) | Mantener lo decidido en `01` (opcional) hasta la reevaluación tras el piloto, o pedir antes la obligatoriedad (cambiaría `01`) | §16.6.7 |
| 5 | **Lecturas de datos internos en IN-03-06** (H2A-09) | Leerlo como incluido [REC]; o dejarlo a ADR-02.C | §16.6.6 |
| 6 | **Selección de formulaciones de los ADR** (R-12). Las capacidades obligatorias `.C` se definen en R2; ADR-03.I y ADR-09.I solo determinan **cómo implementarlas** con el stack | (a) **elegir una formulación** por decisión al aprobar R2; (b) **admitir un conjunto de formulaciones conformes** y dejar que `.I` elija cómo implementar una de ellas, **sin poder redefinir ni relajar invariantes, si se adopta §16.8.5** (propuesta [REC]). Si se adopta, una capacidad admitida que resulte inviable con el stack **vuelve al usuario** | §16.6.8, §16.8.5 |
| 7 | **Controles propuestos [REC]** (IN-03-12…16, 19, 21; IN-09-09, 12, 13; EV-01…06; OM) | Adoptar como requisitos, adoptar parcialmente o descartar | §16.3, §16.6.2, §16.7 |
| 8 | **Aceptación de riesgos residuales** (RES-01…12) | Solo el usuario, por escrito y con motivo | §16.10.1 |
| 9 | **Proporcionalidad y priorización de las amenazas Críticas** (R-03, H2A-12) | Mantener; orden secundario; refinar la escala; reclasificar AM-01 (**19 de 34 amenazas son Críticas** antes de controles) | §16.1, §16.3.1 |
| 10 | **Fallo de la persistencia de un rechazo y OFF-06** (R-11, H2A-13) | Alternativas de §16.7.9; ninguna asume mecanismo ni declara aceptable la pérdida de auditoría | §16.7.9 |
| 11 | **Registro documental de R2-A** en `aprobaciones.md` y `bitacora.md` (R-18, H2A-14) — **ATENDIDA (2026-11-17)** | Registro documental realizado en `aprobaciones.md` y `bitacora.md` el 2026-11-17. **Ya no requiere decisión;** R2-A continúa pendiente de aprobación formal | §16.0 |
| 12 | **Lectura de «nueva autenticación»** (RF-ACC-09; R-05, H2A-11) | L1 «reciente»; L2 «propia de la operación». Define la conformidad de CAP-03-N-B | §16.6.3 |
| 13 | **Política sobre mensajes de texto o de voz como segundo factor** (R-06) | No admitirlos (lo que dice F-04); admitirlos como alternativa restringida con aceptación expresa del riesgo | IN-03-14, §16.6.5 |

### 16.11 Criterios de aceptación de R2-A

Verificables sobre este documento. **La aprobación de R2-A es un acto expreso del usuario y no aprueba R2 en su conjunto ni la Fase 2. R2-A fue aprobado condicionalmente como entregable documental el 2026-11-17 (§16.0).** Las comprobaciones de §16.12 son **del autor** y no sustituyen una auditoría independiente.

| ID | Criterio | Cómo se verifica |
|---|---|---|
| CR2A-01 | **Ausencia de selección tecnológica** | Búsqueda de nombres de tecnología (proveedores, productos, frameworks, lenguajes, motores, estándares y protocolos concretos): **0 apariciones fuera de §16.9**; solo se admiten en §16.9 (fuentes e iOS), en las filas ADR-01 y HR1-13 de R1 y en las **citas literales de una fuente entre comillas** (IN-03-14, PAR-06), que atribuyen un texto y no seleccionan nada. No cuentan los dispositivos, sistemas operativos y navegadores de `01 RNF-07` |
| CR2A-02 | ADR-03.C y ADR-09.C figuran como **PROPUESTA** con invariantes, formulaciones evaluadas contra **todos** los invariantes aplicables, condiciones abiertas y consecuencias; **ninguna formulación no conforme se presenta como viable** y las condicionadas lo declaran | §16.6.3 y §16.7.4 (columnas de conformidad) |
| CR2A-03 | Modelo inicial de amenazas para **TB-1…TB-9** | §16.3: cada frontera tiene al menos una amenaza |
| CR2A-04 | Probabilidad, impacto y **riesgo inherente coherentes con la matriz** de §16.1; **el impacto Crítico sigue una sola regla verificable**: es Crítico si y solo si el peor caso directo cumple (a), (b) o (c) | §16.3.1 (peor caso y criterio por amenaza) frente a la columna P/I de §16.3: recomputar todas las filas; **la probabilidad de cada fila se justifica sin controles en §16.3.2**. **La tabla de §16.3.1 es una comprobación de consistencia entre dos columnas del mismo autor y no valida que el peor caso esté bien escrito** |
| CR2A-05 | **Controles exigidos ≠ propuestos ≠ implementados:** ninguna fila declara una mitigación implementada; **«Exigido» solo si todos los controles listados tienen fuente en `01` o en R1 aprobado** | Columnas «Estado» y «Propuestos [REC]»: ninguna fila «Exigido» tiene controles propuestos |
| CR2A-06 | **Trazabilidad bidireccional amenaza ↔ caso de abuso:** cada amenaza tiene al menos un caso y la relación es simétrica entre §16.3 y §16.4 | Comparación mecánica de ambas columnas |
| CR2A-07 | Trazabilidad con requisitos y decisiones aprobadas; cada invariante cita su fuente | §16.8.1; columnas «Fuente» |
| CR2A-08 | **R1 intacto:** el contenido de §0 a §15 no cambia | Comparación con la versión aprobada: **0 líneas eliminadas o modificadas**; las inserciones son las dos editoriales de §16.0 y la Parte II |
| CR2A-09 | **Las 18 preguntas, 23 `COND` y 14 ambigüedades siguen abiertas**; ninguna fila de §16 declara una resuelta | §16.8.2 y comparación de §9 |
| CR2A-10 | **Sin permisos nuevos ni reducción de garantías**, incluidas las cuentas privilegiadas | IN-03-17, IN-09-11; ninguna propuesta [REC] relaja 2FA, sesión ni recuperación de dueño u organizador |
| CR2A-11 | **La matriz de §8.2 no cambia y sigue siendo acíclica**; ADR-03.C y ADR-09.C requieren «—»; **si se adopta §16.8.5 (propuesta [REC]), `.I` no redefine `.C`; las restricciones de §8.2 y RC-25 de R1 rigen en cualquier caso** | §16.8.3, §16.8.5 y verificación mecánica (§16.12) |
| CR2A-12 | Toda afirmación externa lleva fuente, fecha y limitaciones, y **no se atribuye a una fuente una recomendación más débil ni más fuerte que la citada**; las de iOS están marcadas **[NO VERIF]** | §16.9; IN-03-14; PAR-06 |
| CR2A-13 | **R2-B y R2-C no iniciados:** ADR-02.C, ADR-04.C, ADR-05.C y ADR-16.C **no están desarrollados**, solo referenciados | §16.5 y búsqueda de «ADR-02.C», «ADR-04.C», «ADR-05.C», «ADR-16.C»: solo como responsables, entradas o dependencias |
| CR2A-14 | Solo se modificó `02-arquitectura-adr.md` | `git status` y `git diff --stat` |
| CR2A-15 | Se declara que el modelo de amenazas **será consolidado en R2-C** | §16.0 y §16.12 |
| CR2A-16 | Los riesgos residuales tienen responsable y **ninguno figura como aceptado**; no se atribuye ninguna aceptación a RK-15 ni a HR1-10 fuera de lo que `01` dice | §16.10.1 |
| CR2A-17 | Los hallazgos H2A-01…14 están registrados y las decisiones de §16.10.3 siguen **pendientes**, sin conceder excepciones, fijar parámetros ni modificar R1 | §16.6.4, §16.6.5, §16.7.4, §16.8.4, §16.10 |
| CR2A-18 | **No se declara que el borrador superó una auditoría independiente ni que fue revisado de nuevo tras las correcciones** | §16.0 y §16.12 |
| CR2A-19 | **Cada caso de abuso vinculado a una amenaza Crítica o Alta tiene un criterio verificable** (caso de prueba futuro, criterio de `01` o propuesta [REC] marcada) **o declara expresamente «pendiente de definir» o «no existe»**; ninguno muestra «—» | §16.4, columna «Caso de prueba futuro» |
| CR2A-20 | Cada hallazgo de la primera auditoría, de la revisión focalizada y de la validación de la v0.3 tiene corrección con evidencia, o está **registrado como pendiente** sin declararse corregido | §16.13, §16.14 y §16.15 |

### 16.12 Verificación y lo que no se hizo

**Verificación realizada por el autor** (comprobaciones mecánicas sobre este documento; **no hay ejecución de pruebas del sistema**, que no existe). **Son del autor y no sustituyen una revisión independiente; la v0.5-R2A no ha sido auditada ni revisada de nuevo.**

| Comprobación | Resultado (del autor) |
|---|---|
| **R1 intacto** (CR2A-08) | Comparación línea a línea con la copia de la versión aprobada (897 líneas; SHA-256 `b8c95bfa6940452a745e90b442a8067b8ce74e08a4ad01c5581c4a34bb6045fe`): **0 líneas eliminadas y 0 modificadas**; 1050 insertadas (una línea de estado en el encabezado, una fila en §13 y la Parte II) |
| **Amenazas** (CR2A-03 a 05) | 34; riesgo inherente antes de controles: 19 Críticas, 12 Altas, 3 Medias, 0 Bajas; **probabilidad, impacto y riesgo coherentes con la matriz en las 34 filas** (0 incoherencias); estados: 21 «Exigido», 10 «Exigido + [REC]», 3 «Propuesto [REC]»; **ninguna fila «Exigido» tiene controles propuestos, ninguna «Propuesto [REC]» tiene controles exigidos y ninguna dice «Implementado»** (0 incoherencias) |
| **Impacto Crítico** (CR2A-04) | §16.3.1 cubre las 34 amenazas (0 sin cubrir); impacto Crítico si y solo si el peor caso cumple (a), (b) o (c): **0 incoherencias** entre §16.3.1 y §16.3; toda amenaza con criterio (a) lista AS-04 entre sus activos (0 excepciones). **Es una comprobación de consistencia entre dos columnas del mismo autor y no valida que el peor caso esté bien escrito** |
| **Fronteras** | TB-1…TB-9 con 8, 5, 4, 5, 3, 3, 2, 2, 2 amenazas |
| **Trazabilidad amenaza ↔ caso de abuso** (CR2A-06) | 30 casos; 41 pares amenaza-caso; **0 asimetrías** entre §16.3 y §16.4; 0 amenazas sin caso; 0 casos sin amenaza |
| **Criterios verificables** (CR2A-19) | Casos de abuso vinculados a una amenaza Crítica o Alta con «—» en «Caso de prueba futuro»: **0**; los que no tienen criterio en `01` lo declaran «pendiente de definir» o «no existe» |
| **Responsables** (§16.5) | ADR-03.C 7, ADR-09.C 3, R2-B 5, R2-C 9, R3 5, R4 5 (suma 34); la tabla de §16.5 coincide con la de §16.3 (0 discrepancias) |
| **Invariantes** (CR2A-07) | IN-03: 21 (12 obligatorios, 1 objetivo confirmado y 8 propuestas [REC]); IN-09: 13 (10 obligatorios y 3 propuestas [REC]); 0 sin fuente; 0 referidos sin definir |
| **Otros identificadores** | 12 riesgos residuales · 14 hallazgos H2A · 8 afirmaciones sobre iOS · 6 parámetros · 6 eventos EV · 20 criterios · 14 fuentes · 7 amenazas registradas sin evaluar · 13 decisiones pendientes; ninguna referencia sin definir; 0 referencias obsoletas al número anterior de riesgos residuales |
| **Formulaciones** (CR2A-02) | ADR-03.C: D3-1 con 3 (1 no conforme y 1 condicionada), D3-2 con 2, D3-3 con 3 (1 no cumple IN-03-15 [REC] como vía única) y D3-4 con 3 (1 no viable en el MVP); ADR-09.C: D9-1 con 3 (1 no conforme y 1 condicionada) y D9-2 con 3 |
| **Selección tecnológica** (CR2A-01) | Búsqueda de 53 nombres de proveedores, productos, estándares, protocolos y siglas fuera de §16.9: **sin apariciones**, salvo «iOS» y «Android» (tolerados por `01 RNF-07`) y la **cita literal** de F-04 entre comillas |
| **Dependencias** (CR2A-11) | Matriz de §8.2: 23 nodos, 52 relaciones, **0 ciclos y 0 pares recíprocos**; ADR-03.C y ADR-09.C **requieren «—»**; la regla de §16.8.5 es una **propuesta [REC]** y **no añade relaciones** |
| **Elementos abiertos** (CR2A-09) | §9 sin cambios (18 preguntas y 14 ambigüedades; §9.2 intacta); ninguna fila de §16 declara resuelto ninguno |
| **Aceptaciones y lenguaje** (CR2A-16, 18) | Ninguna aceptación de riesgos; RK-15 es la única aceptación expresa y no se atribuye a RK-11; el documento **no** declara el borrador como auditado ni revisado tras las correcciones |
| **Probabilidad** (W-01) | §16.3.2 justifica la probabilidad de las 34 amenazas (0 sin cubrir); **0 incoherencias** con §16.3; las 2 probabilidades Baja (AM-24, AM-29) declaran una condición propia del escenario (0 sin declararla) |
| **Hallazgos tratados** (CR2A-20) | 20 filas R-xx en §16.14, 24 filas A-xx en §16.13, 7 filas V-xx en §16.15 y 4 filas W-xx en §16.16; R-03 y R-11 siguen **pendientes**; R-18 quedó **atendido** por el registro documental del 2026-11-17; V-02 se corrige solo en lo que `01` permite |
| **Tablas** | Todas las tablas de la Parte II tienen un número consistente de columnas (0 inconsistencias) |
| **Archivos** (CR2A-14) | En la entrega de la v0.5, solo `02-arquitectura-adr.md` modificado (`git status`); sin commit ni push. El registro posterior (2026-11-17) modificó además `aprobaciones.md` y `bitacora.md`; sin commit ni push |

**No se realizó:** ADR-02.C, ADR-04.C, ADR-05.C ni ADR-16.C; la consolidación del modelo de amenazas (será en R2-C); ninguna prueba, prototipo ni medición (incluida la de D-2); ninguna verificación en dispositivo (todo lo de iOS es **[NO VERIF]**); la lectura completa de los documentos consultados; la verificación de capacidades de ningún proveedor; ninguna modificación de `01-requisitos.md`, de `00-descubrimiento.md`, de `aprobaciones.md`, de `bitacora.md` (en la entrega de la v0.5; se registraron después, el 2026-11-17) ni de la skill; commit, push, PR ni merge; **ninguna aceptación de riesgos, fijación de parámetros ni concesión de excepciones**; ninguna decisión sobre las cuestiones de §16.10.3.

**Estado:** **R2-A (v0.5-R2A) — APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-17); el contenido técnico (ADR-03.C, ADR-09.C y sus formulaciones) sigue siendo PROPUESTA** (aprobado sin nueva revisión independiente posterior a las últimas correcciones; registro documental realizado el 2026-11-17). La aprobación **no** es automática para R2 ni para la Fase 2. **R2-B y R2-C: NO INICIADOS Y SIN AUTORIZACIÓN.** Fase 2: **AUTORIZADA, NO APROBADA globalmente.**

### 16.13 Auditoría de v0.1-R2A y correcciones (v0.2 a v0.5)

**Origen:** dictamen «R2-A REQUIERE CORRECCIONES» de un subagente de contexto nuevo (2026-11-17). **Limitación:** mismo tipo de modelo, sin independencia organizativa ni externa. **Estado «Corregido» significa que el autor aplicó la corrección en el texto; no que un revisor independiente la haya confirmado de nuevo.** La revisión focalizada de la v0.2 (§16.14) confirmó como resueltos 15 hallazgos y halló otros parciales; este cuadro refleja esa revisión.

| Hallazgo | Corrección | Evidencia | Estado actual | Riesgo residual |
|---|---|---|---|---|
| A-01 «Exigido» sin fuente en `01` | Tres estados de control y columna separada de propuestos [REC]; definición de «Exigido» alineada (incluye R1 aprobado) | §16.1, §16.3, §16.4 | Corregido en v0.2 | Ninguno nuevo |
| A-02 impacto desigual sobre menores | Regla única y verificable por amenaza (§16.3.1); AM-25, 28 y 30 pasan a Crítico en v0.2; AM-27 y AM-32 en v0.3 (R-01, R-02); en v0.4, una sola convención «antes de controles» para todas las filas (V-01) | §16.1, §16.3, §16.3.1 | **Parcial en v0.2; corregido en v0.3** | RES-02, 03, 08; H2A-12 |
| A-03 AM-23 sin caso ni residual | AM-23 y AM-32 separadas; CAB-21, CAB-22; RES-11 | §16.3, §16.4, §16.10.1 | Corregido en v0.2 | RES-11 |
| A-04 CAP-03-N-A | Marcada no conforme por sí sola | §16.6.3 | Corregido en v0.2 | Ninguno |
| A-05 CAP-09-P-B y P-C | P-B no conforme; P-C condicionada; solo P-A plenamente conforme | §16.7.4 | Corregido en v0.2; modo de fallo en §16.7.9 (R-11) | Si se adopta P-B, ajuste editorial de R1 |
| A-06 factibilidad de D-2 | Presupuesto temporal; en v0.3, con reintentos, S1/S2/S3 y opciones con su necesidad de excepción formal | §16.6.4 | **Parcial en v0.2; corregido en v0.3 (R-04); decisión del usuario pendiente** | RES-05 |
| A-07 recuperación | Evaluada contra IN-03-15 y seis escenarios; rótulos corregidos (R-10) | §16.6.3 | Corregido | RES-01 |
| A-08 aceptación atribuida a RK-15 | RES-07 Pendiente; solo RK-15 está aceptado y no cubre RK-11 | §16.1, §16.10.1 | Corregido en v0.2 | RES-07 |
| A-09 operaciones ligadas a su cuenta | AM-33, IN-03-19, IN-03-20, CAB-30 | §16.3, §16.6.2 | Corregido en v0.2 | Ninguno |
| A-10 inyección de contenido | AM-34 y CAB-25 | §16.3, §16.4 | Corregido en v0.2 | Fase 4 y R3 |
| A-11 delegados sin 2FA | RES-10 y decisión 4 | §16.10 | Corregido en v0.2; **aceptación pendiente** | RES-10 |
| A-12 reautenticación y mensajes de texto | IN-03-16 ampliado en v0.2; en v0.3, IN-03-14 contrastado con F-04 (R-06), notificación (R-07) y aclaración de IN-03-16 (R-08) | §16.6.2 | **Parcial en v0.2; corregido en v0.3** | RES-01; decisión 13 |
| A-13 parámetros frente a las fuentes | Tabla con requisito, referencia, justificación, riesgo y clasificación; en v0.3, niveles de garantía y absoluto de F-02 (R-20) | §16.6.5 | **Parcial en v0.2; corregido en v0.3** | RES-09 |
| A-14 IN-09-06 frente a RF-AUD-01 | Campos completos; IN-09-13 [REC] | §16.7.3 | Corregido en v0.2 | Ninguno |
| A-15 citas y dependencias | RT-06 y RT-10 corregidos en v0.2; en v0.3, también en CAB-10 y AM-32 (R-09) | §16.3, §16.4, §16.7.3 | **Parcial en v0.2 (la fila decía haber corregido CAB-10 sin que lo estuviera); corregido en v0.3** | Ninguno |
| A-16 estado de registro | §16.0 declara el registro pendiente | §16.0, encabezado, §13 | Corregido en el texto; **el registro se realizó el 2026-11-17** (decisión 11 atendida); R2-A continúa pendiente de aprobación formal | H2A-14 |
| A-17 compatibilidad de iOS | IOS-07, IOS-08 y columna de la prueba real | §16.9 | Corregido en v0.2 | RES-07 |
| A-18 AM-01 frente a AM-15; AM-07 | Diferencia explicada; en v0.3, intento frente a resultado y AM-07 frente a IN-03-12 (R-03, R-13) | §16.3, §16.3.1 | Corregido; **priorización pendiente** (H2A-12) | H2A-12 |
| A-19 método de riesgo residual | Residual por riesgo; criterio verificable; en v0.3, criterio para las Críticas y Altas (R-17) | §16.1, §16.4 | Corregido | Ninguno |
| A-20 trazabilidad amenaza ↔ caso | Mapeo único y simétrico verificado mecánicamente | §16.3, §16.4, §16.12 | Corregido en v0.2 | Ninguno |
| A-21 fuentes sin uso | F-06 «consultada y no utilizada»; columna de afirmaciones por fuente | §16.9 | Diferido con justificación válida | Ninguno |
| A-22 ejemplos de mecanismo | Marcados como ilustrativos | §16.0, §16.6.3, §16.7.4 | Diferido con justificación válida | Ninguno |
| A-23 obligatorios mezclados con [REC] | Tablas A, B y C separadas | §16.6.2, §16.7.3 | Corregido en v0.2 | Ninguno |
| A-24 amenazas menores | Registradas sin evaluar como OM-01…07 | §16.4 | Registrado; **no evaluadas** | R2-B, R3, R4 |

### 16.14 Revisión focalizada de v0.2-R2A y correcciones de v0.3-R2A

**Origen:** dictamen «R2-A APROBABLE CON CONDICIONES» de otro subagente de contexto nuevo (2026-11-17), con **independencia solo de contexto**. **Estado «Corregido» significa que el autor aplicó la corrección; la v0.3 no ha sido revisada de nuevo.** Los hallazgos R-03, R-11 y R-18 se **registraron sin resolver** por instrucción expresa (**R-18 quedó atendido el 2026-11-17** por el registro documental; R-03 y R-11 siguen pendientes).

| Hallazgo | Estado anterior | Corrección | Evidencia | Estado actual | Riesgo residual |
|---|---|---|---|---|---|
| R-01 AM-32 incumplía la regla (c) | Alto | Pasa a **Crítico**; peor caso y criterio escritos en §16.3.1; se quitó RT-06 de sus controles | §16.3, §16.3.1 | Corregido | RES-11 |
| R-02 AM-27 y AM-12 sin AS-04; comprobación circular | AM-27 Medio; CR2A-04 circular | AM-27 incorpora AS-04 y pasa a impacto Crítico (riesgo Alto); AM-12 mantiene su impacto con justificación escrita; CR2A-04 comprueba el peor caso por amenaza (§16.3.1); **en v0.4 se precisa que es una comprobación de consistencia y no una validación del peor caso (V-01)** | §16.1, §16.3, §16.3.1, §16.11 | Corregido | H2A-12 |
| R-03 AM-01 puntúa el intento; poca discriminación entre Críticas | Sin registrar | **Registrado sin resolver:** AM-01 se rotula **intento**; H2A-12 y decisión 9 | §16.3, §16.3.1, §16.10 | **Pendiente de decisión** | H2A-12 |
| R-04 D-2 estrechado sin marcarlo; Tred; opciones | Parcial | No se reduce «sesión conectada» (S1, S2, S3; H2A-10); Tred con reintentos; formulaciones que cumplen o no cumplen en S1 y S2; opciones con la excepción formal que requieren; ninguna excepción concedida | §16.6.3, §16.6.4, §16.10 | Corregido; **decisión del usuario pendiente** | RES-05 |
| R-05 conformidad de CAP-03-N-B | «Conforme» | **Condicionada** a la lectura de «nueva» (L1, L2); riesgo de sesión robada en la ventana; H2A-11, decisión 12 | §16.6.3 | Corregido; **decisión pendiente** | AM-03 |
| R-06 IN-03-14 y PAR-06 más débiles que F-04 | «Respaldado» | Se separa lo literal de F-04 y F-01 de la política del proyecto; la política sobre mensajes de texto o de voz pasa a **pendiente de decisión** | §16.6.2, §16.6.5, §16.9 | Corregido; **decisión 13 pendiente** | AM-02 |
| R-07 falta notificación al titular | Omitida | IN-03-21 [REC], trazada con AM-02, AM-20, F-01 y F-04; no es requisito de `01` | §16.6.2, §16.3 | Corregido (como propuesta) | RES-01 |
| R-08 IN-03-16 frente a E2 y E3 | Contradicción | IN-03-16 distingue el factor ya enrolado de la recuperación por pérdida del factor | §16.6.2 | Corregido | Ninguno |
| R-09 RT-06 mal citado en CAB-10 y AM-32; fila A-15 inexacta | «Corregido» sin estarlo | Se retira RT-06 como control; se cita RF-ACC-09; la fila A-15 de §16.13 corrige su estado | §16.3, §16.4, §16.13 | Corregido | Ninguno |
| R-10 «No conforme» contra invariantes [REC] | Rótulo equívoco | Rótulos distintos: «No conforme» (obligatorio), «No cumple IN-xx [REC]», «Condicionada» | §16.6.3 | Corregido | Ninguno |
| R-11 fallo de persistencia de un rechazo y OFF-06 | Sin tratar | **Registrado sin resolver** con alternativas y consecuencias de seguridad; no declara aceptable la pérdida de auditoría ni asume mecanismo | §16.7.9, §16.10 | **Pendiente de decisión** | H2A-13 |
| R-12 decisión 6 reabría .C → .I | «Delegar en R3» | Decisión reformulada: `.C` se define en R2; `.I` solo implementa y, **si se adopta la regla propuesta**, no puede redefinir invariantes; regla de precedencia de §16.8.5 | §16.6.8, §16.8.5, §16.10.3 | Corregido; **decisión 6 pendiente** | H2A-08 |
| R-13 AM-07 frente a IN-03-12 | Matiz omitido | AM-07 indica que IN-03-12 favorece el caso | §16.3 | Corregido | Ninguno |
| R-14 amenaza omitida: captura falsa con permisos válidos | No registrada | Registrada sin evaluar como OM-07 | §16.4 | Registrado; no evaluado | R2-B |
| R-15 «Exigido» definido de forma dispar | Dos definiciones | Una sola definición (incluye R1 aprobado); AM-28 y AM-30 señalan que el requisito carece de mecanismo y plazo | §16.1, §16.3 | Corregido | Ninguno |
| R-16 Una sigla de nivel de garantía de F-01 aparecía fuera de §16.9 | Contradice CR2A-01 | Se sustituye por «nivel de garantía» y la sigla se añade a la búsqueda de §16.12 | §16.6.5, §16.12 | Corregido | Ninguno |
| R-17 Críticos y Altos sin criterio verificable | Faltaban | Criterio de `01` o propuesta [REC] marcada, o declaración de que no existe | §16.4 | **Parcial en v0.3 (CAB-17 y CAB-28 conservaban «—»); corregido en v0.4 (V-02)** | Ninguno |
| R-18 registro de R2-A pendiente | Sin listar | En la v0.3 se registró sin resolver. **Atendido:** registro documental realizado en `aprobaciones.md` y `bitacora.md` el 2026-11-17; R2-A continúa pendiente de aprobación formal | §16.0, §16.10 | **Atendido (2026-11-17)** | H2A-14 |
| R-19 CAB-10 frente a CL-43 | Cobertura parcial | CAB-10 aclara que CL-43 solo cubre la carrera de revocaciones y cita RF-ACC-09 | §16.4 | Corregido | Ninguno |
| R-20 PAR-03 frente al nivel de garantía | Matiz omitido | PAR-03 distingue dueño y organizador (2FA obligatorio) del delegado y del editor, y cita el absoluto de F-02 | §16.6.5 | Corregido | RES-09 |


### 16.15 Validación de v0.3-R2A y correcciones de v0.4-R2A

**Origen:** dictamen «R2-A APROBABLE CON CONDICIONES» de otro subagente de contexto nuevo (2026-11-17), con **independencia solo de contexto**: 2 condiciones bloqueantes documentales (V-01, V-02) y 5 menores (V-03 a V-07). **Estado «Corregido» significa que el autor aplicó la corrección; la v0.4 no ha sido revisada de nuevo.** **R2-A sigue siendo borrador pendiente de aprobación;** las 13 decisiones de §16.10.3 siguen abiertas.

| Hallazgo | Corrección | Evidencia | Resultado | Riesgo residual |
|---|---|---|---|---|
| V-01 regla (c) aplicada de forma desigual (AM-05 y AM-32); peor caso de AM-01 y AM-07; etiqueta «no circular» | Una sola convención «antes de controles» (sin considerar ningún control, exigido ni propuesto, porque ninguno está implementado) aplicada a las 34 filas. **AM-05 pasa a Crítico** (criterio (c): el bloqueo del único dueño no tiene límite sin PAR-02); **AM-07 a Crítico** (sesión privilegiada falsificada: criterio (b)); **AM-01 a impacto Crítico** (alineada con CAB-02) y **AM-10 a Alto**. El criterio (c) se define como pérdida que no cesa por sí sola. §16.3.1 se declara comprobación de consistencia, no validación del peor caso. No se redujo ninguna clasificación | §16.1, §16.3, §16.3.1, CR2A-04, H2A-12 | Corregido | RES-12; priorización pendiente (decisión 9): 15 de 34 Críticas en la v0.4 y 19 en la v0.5 (W-01) |
| V-02 CAB-17 y CAB-28 sin criterio verificable (CR2A-19) | CAB-17, CAB-18, CAB-28, CAB-15 y CAB-16 reciben el criterio de `01` (PRV-17, PRV-03, RF-ACC-10, OFF-13) o declaran «pendiente de definir» donde `01` no lo aporta (retención de respaldos y coherencia de la clasificación pública; para la exportación, **OFF-24 y PRV-08 sí traen criterio** y solo quedan pendientes las facultades de Q-42 y los eventos de Q-43). CR2A-19 exige ahora ese texto y prohíbe «—» | §16.4, CR2A-19 | **Corregido en lo que `01` permite; el resto queda declarado «pendiente de definir»** | AM-30 y la coherencia de CL-15 siguen sin criterio hasta ADR-12/17 y ADR-04.C/06; en la exportación, solo Q-42 y Q-43 |
| V-03 RNF-07 citado para la ausencia de sincronización en segundo plano | Se separan las fuentes: RNF-07 (versiones mínimas), OFF-05 y RK-11 (sin sincronización en segundo plano). El resumen de R1 §2.1 no se modifica; el ajuste editorial se lista en §16.8.4 | §16.6.4, IOS-06, §16.8.4 | Corregido | Ajuste de R1 §2.1 pendiente de autorización separada |
| V-04 notificación de designación o revocación de dueño | IN-03-21 la incorpora como **propuesta separada [REC] no decidida**, sin convertirla en obligación | §16.6.2 | Corregido (como propuesta) | RES-01 |
| V-05 RNF-06 como control de inyección; RT-06 en CAP-03-R-C | AM-19 y CAB-24 ya no atribuyen la inyección a RNF-06 (estado «Propuesto [REC]»); CAP-03-R-C cita RF-ACC-09 y aclara que RT-06 no cubre la ausencia del último dueño | §16.3, §16.4, §16.6.3 | Corregido | Fase 4 |
| V-06 §16.8.5 redactada como norma | Marcada como **propuesta [REC] pendiente de decisión (decisión 6)** que **no relaja ninguna restricción aprobada de R1** (§8.2 y RC-25 vigentes); la decisión 1 distingue los asuntos 1a y 1b sin cambiar el número de decisiones | §16.8.5, §16.10.3 | Corregido | Decisión 6 pendiente |
| V-07 IN-09-07 frente a RF-AUD-04 | IN-09-07 usa el alcance exacto de RF-AUD-04 («datos personales de menores más allá de identificadores internos») y cita su criterio de aceptación; §16.7.5 se alinea | §16.7.3, §16.7.5 | Corregido | Minimización de datos de adultos y usuarios: PRV-14 y ADR-09.I |

**Qué bloquea el cierre documental de R2-A y qué no.** Con estas correcciones, las dos condiciones bloqueantes que señaló la validación (V-01, V-02) están **aplicadas por el autor**, pero **solo una revisión independiente puede confirmarlo**. **No bloquean R2-A y pertenecen a decisiones posteriores:** las 13 decisiones de §16.10.3 (R2-B, R2-C, cierre de R2, R3, R4, excepción formal o modificación de `01`) y, ya realizado, el registro documental (decisión 11, atendida el 2026-11-17).


### 16.16 Verificación final de v0.4-R2A y correcciones de v0.5-R2A

**Origen:** dictamen «R2-A LISTO CON CONDICIONES» de otro subagente de contexto nuevo (2026-11-17), con **independencia solo de contexto**: V-01 a V-07 resueltos en lo verificable y 4 hallazgos documentales nuevos (W-01 a W-04). **Estado «Corregido» significa que el autor aplicó la corrección; la v0.5 no ha sido revisada de nuevo.** R2-A sigue siendo borrador pendiente de aprobación; las 13 decisiones de §16.10.3 siguen abiertas.

| Hallazgo | Corrección realizada | Evidencia | Estado | Riesgo residual |
|---|---|---|---|---|
| W-01 la probabilidad no seguía la convención «antes de controles» | Se distinguen probabilidad inherente, impacto inherente y condiciones propias del escenario (§16.1); **ningún control —exigido, propuesto o no implementado— reduce la probabilidad**. §16.3.2 justifica las 34 probabilidades. **AM-13, AM-21, AM-26 y AM-27 pasan de Baja a Media** (defecto previsible que viola una restricción de diseño) y de riesgo Alto a Crítico; **AM-16 pasa a Media** y a riesgo Alto; **AM-15 pasa a Alta** (su riesgo ya era Crítico; se retira la atenuación por RT-02). **AM-24 y AM-29 permanecen en Baja** por una condición propia del escenario (acceso técnico previo, definición de ACT-10). Resultado: 19 Críticas, 12 Altas y 3 Medias. **El cambio aplica la convención y no se hizo para aumentar el recuento** | §16.1, §16.3, §16.3.2, H2A-12, decisión 9, CR2A-04 | Corregido | La priorización entre 19 Críticas sigue pendiente (decisión 9); el recuento depende de la convención |
| W-02 CAB-18 y V-02 afirmaban que OFF-24 y PRV-08 no traían criterio | CAB-18 cita los criterios de OFF-24 y de PRV-08 y deja pendiente solo lo que `01` no resuelve (facultades del organizador, Q-42; eventos incluibles, Q-43). La fila V-02 de §16.15 se corrige | §16.4 (CAB-18), §16.15 | Corregido | Q-42 y Q-43 siguen abiertas |
| W-03 CAB-10 y CAB-22 omitían CL-28 | Ambos citan CL-28 con su alcance literal (pérdida del segundo factor del dueño y revocación del último dueño, con un segundo dueño de apoyo). CAB-22 aclara que **CL-28 no cubre la sucesión de las credenciales administrativas de la plataforma**, que sigue pendiente de definir | §16.4 (CAB-10, CAB-22) | Corregido | La sucesión de accesos de la plataforma: R4 y Fase 8 |
| W-04 §16.8.5 [REC] presentada como vigente | §16.6.8, H2A-08, la decisión 6, CR2A-11 y la fila R-12 de §16.14 dicen «si se adopta»; se aclara que las restricciones de §8.2 y RC-25 de R1 rigen en cualquier caso. §8.2 no cambia | §16.6.8, §16.10.2, §16.10.3, §16.11, §16.14 | Corregido | Decisión 6 pendiente |

**¿Queda algún defecto material conocido que impida pasar al registro documental de R2-A?** **Ninguno que el autor conozca.** Solo una revisión independiente puede confirmarlo. Los asuntos pendientes (las decisiones de §16.10.3 distintas de la 11, que ya está atendida, y los RES-01…12) pertenecen a etapas posteriores o a decisiones del usuario.
