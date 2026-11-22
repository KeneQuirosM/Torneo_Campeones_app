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


---

# PARTE III — BLOQUE R2-B1 (v0.5-R2B1, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL, 2026-11-18)

> **Estado: R2-B1 v0.5-R2B1 — APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-18); el contenido técnico (ADR-02.C y sus formulaciones) sigue siendo PROPUESTA.** Todo lo que sigue a esta línea (§17 en adelante) es trabajo nuevo de R2-B1. **No forma parte del contenido aprobado de R1** (§0 a §15, v0.5-R1) **ni del de R2-A** (§16, v0.5-R2A, aprobado condicionalmente como entregable documental el 2026-11-17), que **no se modifican**. **ADR-02.C permanece como PROPUESTA:** ninguna formulación está elegida ni aprobada, no se fija ningún parámetro, no se acepta ningún riesgo y no se concede ninguna excepción. **Esta versión (v0.5) aplica dos ajustes (E-1 y E-2) a la v0.4 tras su verificación final de solo lectura (dictamen «aprobable como entregable documental condicionado») y NO ha sido revisada de nuevo por nadie distinto del autor; se aprobó sin una auditoría posterior a esos ajustes** (§17.0, §17.12, §17.14 a §17.17).

## 17. R2-B1 — ADR-02.C (parte 1): modelo de autorización y permisos

### 17.0 Alcance, autorizaciones y reglas de esta parte

**Autorizaciones y trabajos de R2-B1** (los mensajes constan en la conversación de la sesión, no en archivos del repositorio):

| Fecha | Acto | Mensaje | Resultado |
|---|---|---|---|
| 2026-11-17 | Inicio controlado de R2-B1, solo `02-arquitectura-adr.md` | «AUTORIZO EXCLUSIVAMENTE EL INICIO DE R2-B1 DE LA FASE 2.» y «NO AUTORIZO INICIAR R2-B2.» | Borrador v0.1-R2B1 |
| 2026-11-17 | Auditoría técnica **de solo lectura** | «AUTORIZO EXCLUSIVAMENTE UNA AUDITORÍA TÉCNICA DE SOLO LECTURA DEL BORRADOR R2-B1.» | Dictamen **«R2-B1 REQUIERE CORRECCIONES»** de un subagente de contexto nuevo (**independencia de contexto, no organizativa ni externa**, mismo tipo de modelo que el autor): 14 hallazgos B-01…B-14 (§17.14). **Es una recomendación, no una aprobación** |
| 2026-11-17 | Una ronda de correcciones documentales | «AUTORIZO EXCLUSIVAMENTE UNA RONDA DE CORRECCIONES DOCUMENTALES DEL BORRADOR R2-B1, para resolver los hallazgos B-01 a B-12 y los ajustes editoriales B-13 y B-14.» | Borrador v0.2-R2B1 (§17.14) |
| 2026-11-18 | Validación focalizada **de solo lectura** de la v0.2 | «AUTORIZO EXCLUSIVAMENTE UNA REVISIÓN TÉCNICA DE SOLO LECTURA DEL BORRADOR R2-B1 v0.2.» | Dictamen **«R2-B1 APROBABLE CON CONDICIONES»** de un subagente de contexto nuevo (**independencia de contexto, no organizativa ni externa**): 8 hallazgos nuevos C-01…C-08, de los cuales C-01, C-02 y C-03 condicionaban (§17.15). **Es una recomendación, no una aprobación** |
| 2026-11-18 | Una corrección documental focalizada de la v0.2 | «AUTORIZO EXCLUSIVAMENTE UNA CORRECCIÓN DOCUMENTAL FOCALIZADA DEL BORRADOR R2-B1 v0.2.» | Borrador v0.3-R2B1 (§17.15) |
| 2026-11-18 | Validación focalizada **de solo lectura** de la v0.3 | «AUTORIZO EXCLUSIVAMENTE UNA REVISIÓN TÉCNICA DE SOLO LECTURA DEL BORRADOR R2-B1 v0.3.» | Dictamen **«R2-B1 APROBABLE CON CONDICIONES»** de un subagente de contexto nuevo (**independencia de contexto, no organizativa ni externa**): 9 hallazgos nuevos D-01…D-09, de los cuales D-01, D-02 y D-03 condicionaban (§17.16). **Es una recomendación, no una aprobación** |
| 2026-11-18 | Una corrección documental focalizada de la v0.3 | «AUTORIZO EXCLUSIVAMENTE CORREGIR LOS HALLAZGOS D-01 A D-09 DEL BORRADOR R2-B1 v0.3.» | Borrador v0.4-R2B1 (§17.16) |
| 2026-11-18 | Verificación final focalizada **de solo lectura** de la v0.4 | «AUTORIZO EXCLUSIVAMENTE UNA VERIFICACIÓN FINAL, FOCALIZADA Y DE SOLO LECTURA DE R2-B1 v0.4.» | Dictamen **«APROBABLE COMO ENTREGABLE DOCUMENTAL CONDICIONADO»** de un subagente de contexto nuevo (**independencia de contexto, no organizativa ni externa**): sin defectos materiales; dos ajustes menores (E-1 y E-2) y una observación (E-3) (§17.17). **Es una recomendación, no una aprobación** |
| 2026-11-18 | Dos correcciones documentales focalizadas de la v0.4 | «AUTORIZO EXCLUSIVAMENTE DOS CORRECCIONES DOCUMENTALES FOCALIZADAS, E-1 Y E-2, DEL BORRADOR R2-B1 v0.4.» | **Esta versión, v0.5-R2B1** (§17.17) |
| 2026-11-18 | Decisión de aprobación documental | «APRUEBO CONDICIONALMENTE R2-B1 v0.5 COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL.» (mensaje «DECISIÓN DE APROBACIÓN DOCUMENTAL R2-B1») | **R2-B1 v0.5 aprobado condicionalmente como entregable documental** (SHA-256 del contenido aprobado: `0ea253e18824f4c95a2437997bb6c84da1363464f6c47b462d578784e0000fc7`) |
| 2026-11-18 | Registro de la aprobación | «AUTORIZO EXCLUSIVAMENTE REGISTRAR LA APROBACIÓN CONDICIONAL DE R2-B1 v0.5 COMO ENTREGABLE DOCUMENTAL.» (mensaje «REGISTRO DE APROBACIÓN CONDICIONAL R2-B1 v0.5») | Esta actualización de los metadatos de estado de la Parte III |

Archivo autorizado: solo este documento, editando únicamente la Parte III. **No se modificaron `aprobaciones.md`, `bitacora.md` ni `01-requisitos.md`.** La aprobación de R2-A no autoriza por sí misma R2-B1 ni R2-B2.

**Qué comprende R2-B1** (y qué no):

| Comprende (R2-B1) | No comprende (se reserva) |
|---|---|
| Invariantes IN-02 (§17.2) | Especificación detallada de UI-01…UI-13 y diseño de la frontera atómica → **R2-B2** |
| Formulaciones de capacidad CAP-02-A, B, C y D y su conformidad (§17.3) | Modelo adicional de amenazas y casos de abuso de R2-B → **R2-B2** (las amenazas asignadas a R2-B siguen como están en §16.3 y §16.4) |
| Modelo de capacidades por rol, a nivel arquitectónico (§17.4) | Matriz detallada de permisos y diseño de datos → **Fase 4** |
| Aislamiento entre torneos, acumulación de roles y separación de facultades (§17.5) | Políticas de acceso concretas, esquemas, migraciones e interfaces de servicio → **Fase 4** y **ADR-02.I (R3)** |
| Ambigüedades A-1, A-2, A-4, A-5, A-6, A-8, A-12, A-15 y A-16 con ambas lecturas; soporte de Q-40 y Q-42 y de las preguntas que condicionan celdas (§17.6) | Visibilidad y proyecciones públicas (A-8 como punto de guardia; Q-40 (d)) → **ADR-04.C (R2-C)** |
| Dependencias de ADR-03.C y ADR-09.C como propuestas condicionadas y registro de la relación con ADR-05.C (§17.7) | Sincronización sin conexión (A-9…A-14, UI-06 aplicada) → **ADR-05.C (R2-C)**; reglamento y desempates (A-16, motor) → **ADR-08 (R3)** |
| Condiciones abiertas, riesgos de diseño y criterios (§17.8 a §17.17) | Consolidación del modelo de amenazas → **R2-C**; selección de proveedor o mecanismo → **R3** |

**Precedencia de estado.** Varias líneas de este documento dicen que R2-B «no está iniciado» o que R2, R3 y R4 están «no iniciados»: la **línea 5 del encabezado** (estado de la fase a la fecha de aprobación de R1), la **línea 6** (estado de R2-A), la **§12** (línea 783), la fila de R2-A de **§13** (línea 798), **§16.0** y la fila de **§16.5** que describe R2-B como «no iniciado». Todas describen el estado **a la fecha de aprobación de R1 o de R2-A**. Para R2-B1 y R2-B2 prevalece este apartado: **R2-B1 está APROBADO CONDICIONALMENTE como entregable documental; R2-B2-a está APROBADO CONDICIONALMENTE como entregable documental (2026-11-19, v0.3-R2B2a, Parte IV, §18); R2-B2-b1 está APROBADO CONDICIONALMENTE como entregable documental (2026-11-19, v0.3-R2B2b1, Parte V, §19); R2-B2-b2 está APROBADO CONDICIONALMENTE como entregable documental (2026-11-20, v0.3-R2B2b2, Parte VI, §20); R2-B2 completo tiene cierre documental condicionado aprobado (2026-11-21), pero no aprobación global de R2 ni de Fase 2; R2-C, R3 y R4 siguen NO INICIADOS Y SIN AUTORIZACIÓN.** El registro de esas aprobaciones figura en `aprobaciones.md` y `bitacora.md`. Este documento no incorporó una línea de estado de R2-B1 al encabezado ni una fila a §13 porque eso habría modificado contenido aprobado; esos ajustes requieren autorización separada.

**Aprobación condicional de R2-B1 como entregable documental (2026-11-18).** Por acto expreso del usuario (mensajes «DECISIÓN DE APROBACIÓN DOCUMENTAL R2-B1» y «REGISTRO DE APROBACIÓN CONDICIONAL R2-B1 v0.5»), la v0.5-R2B1 quedó **APROBADA CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL.** **Comprende:** los invariantes IN-02; el análisis de CAP-02-A, B, C y D; el modelo de capacidades por rol y ámbito; las combinaciones de roles; las ambigüedades documentadas; los riesgos de diseño; las dependencias arquitectónicas; y las correcciones hasta la v0.5-R2B1. **No implica:** (1) elegir una formulación; (2) declarar demostrada la independencia de CAP-02-D; (3) dar por cumplido R1 §8.1; (4) resolver DB1-1 a DB1-6; (5) aprobar definitivamente ADR-02.C; (6) aprobar o implementar K-2d; (7) fijar PAR-01 a PAR-06; (8) aceptar RES-01 a RES-12 ni los riesgos RB1; (9) conceder excepción a D-2; (10) aprobar globalmente R2 ni la Fase 2; (11) autorizar R2-B2, R2-C, R3 o R4. **Precisiones:** la frontera atómica de K-2d queda pendiente de R2-B2; R2-B2 debe aportar el análisis necesario para evaluar la independencia de CAP-02-D, sin que se presuma que vaya a demostrarla; DB1-1 y el déficit frente a R1 §8.1 permanecen abiertos, sin excepción; las decisiones que los requisitos asignan a la Fase 2 deben resolverse antes de su cierre global, salvo modificación o excepción formal aprobada. **Limitación de la revisión:** la v0.4 recibió un dictamen favorable de un subagente con independencia solo de contexto; la v0.5 incorpora los ajustes E-1 y E-2 y **no recibió una nueva auditoría posterior**; la aprobación documental **no demuestra seguridad implementada ni validada.**

**Integridad.** SHA-256 del **contenido aprobado** (este archivo inmediatamente antes de añadir los metadatos de aprobación): `0ea253e18824f4c95a2437997bb6c84da1363464f6c47b462d578784e0000fc7`. El archivo con los metadatos de aprobación tiene **otro** SHA-256, que figura en `aprobaciones.md` y `bitacora.md` (un archivo no puede contener su propio hash). Las modificaciones de esta operación son exclusivamente de estado y de metadatos editoriales de la Parte III: no alteran invariantes, matriz de conformidad, formulaciones, condiciones K, modelo de permisos, riesgos ni decisiones pendientes, ni §0 a §16.

**Relación con R1 y R2-A.** El contenido anterior a esta Parte (líneas 1 a 1951 del archivo de partida) es el versionado en el commit `e246508559d4b10cb2a0c9ef3a8b4fbcf46c3b9b` (SHA-256 del archivo en ese commit: `2a4eca42cb2110aefc5f8268f3f4a57b504f561de01cd56ae8490b719549955d`) y permanece **íntegro**: esta parte solo **añade** texto al final. **§8.1 y §8.2 no cambian:** ADR-02 sigue como PREVISTO y no se añade ninguna relación.

**Lectura previa efectuada** (el autor la declara; no la verificó ningún tercero): de la skill, `SKILL.md` completo y `arquitectura.md`, `seguridad.md` y `puertas-de-aprobacion.md` completos; de `01-requisitos.md`, RF-ACC-01…17 (con sus criterios de aceptación), RF-TOR-02, RF-EQP-06, RF-AUD-01…06, PRV-09, PRV-11, OFF-22, §7 (matriz de 41 filas, RT-01…14 y roles combinados; los casos mínimos de §7.3 en lo citado), §9.6, §13.7 (A-1…A-16), las filas de Q-40 y Q-42 y los casos CL-19…22, 34…47 y 48…60; de `02-arquitectura-adr.md`, §3.1, §5.3 (tabla de componentes), §5.5 (tabla de UI), §7.0, §8.1 (fila de ADR-02), §8.2, §9.1 y §9.3, HR1-04…06, CR1-13, §16.0 a §16.2, §16.5, §16.6.1 a §16.6.3 y §16.6.6, §16.7.3 y §16.7.4 y §16.10.3. **En la v0.3 y la v0.4 se contrastaron además** R1 §5.3 (CMP-07 y CMP-08), CR1-13 y §5.8 (AC-1); en la v0.3, R1 §5.3 y §5.5 (la cláusula sobre proyecciones derivadas persistentes y la lista no exhaustiva), RF-ACC-12, RF-AUD-05 y los títulos de Q-21, Q-22, Q-23 y Q-29 en `02 §9.1`. **No se releyeron íntegros `01` ni `02`:** se contrastaron los textos citados con su fuente, y lo no leído no se afirma. Las fuentes literales de §17.2 se comprobaron por búsqueda del extracto en el texto de `01`, de R1 y R2-A o de la skill (§17.12).

**Etiquetas de esta parte** (las de §0.1 y §16.0): [CONF] decisión confirmada por el usuario · [DER] consecuencia derivada de una decisión o requisito aprobado · [REC] propuesta que **no es una decisión** · [ABIERTO] abierto en `01` · [SUP] juicio del autor. **Cada invariante lleva una sola etiqueta** y, por cada una, un **respaldo literal** (§17.2). **Identificadores nuevos:** `IN-02-` invariantes · `CAP-02-` formulaciones · `K-` condiciones de conformidad · `CF-` familias de capacidades por rol · `H2B1-` condiciones y hallazgos · `DB1-` decisiones del usuario · `RB1-` riesgos de diseño · `CR2B1-` criterios · `B-01…B-14` hallazgos de la auditoría de R2-B1 (no confundir con otros identificadores). **Regla de nombres:** esta parte no selecciona proveedor, producto, mecanismo ni stack; usa categorías de capacidad («servidor» y «capa de datos» son las capas que ya nombran RT-02 y RC-04). Los ejemplos de mecanismo son **ilustrativos**.

### 17.1 ADR-02.C (parte 1) — Identificación

| Campo | Contenido |
|---|---|
| **ID y bloque** | ADR-02.C · R2-B1 (parte 1 de R2-B) · Fase 2 |
| **Estado** | **PROPUESTA.** ADR-02.C no está aprobado (R2-B1 se aprobó solo como entregable documental, §17.0). §8.1 sigue mostrando ADR-02 como PREVISTO |
| **Fecha** | 2026-11-17 (v0.1); corregida el 2026-11-17 (v0.2) y el 2026-11-18 (v0.3, v0.4 y v0.5) |
| **Pregunta de decisión** (§8.1) | ¿Qué verifica además la aplicación y qué impone siempre la capa de datos (RT-05, RT-06, RT-08, RT-13, RT-14)? Toda autorización se comprueba en ambas capas; ninguna se verifica en una sola. **La parte de la pregunta sobre la frontera atómica (UI-01…13) es de R2-B2** |
| **Qué es la parte «.C»** | Capacidades, restricciones e invariantes **independientes del proveedor** (RC-25). La parte dependiente del stack es **ADR-02.I** y se condiciona a ADR-01 (R3) |
| **Formulaciones exigidas por §8.1** | «≥ 2 formas de reparto con **ambas capas siempre presentes**». **Esta exigencia NO está declarada cumplida: §17.3.3 y DB1-1** |
| **Requiere** (§8.2) | ADR-03.C (nivel de autenticación) y ADR-09.C (persistencia del registro de rechazos). **Ambos son PROPUESTA (R2-A, aprobados solo como entregable documental)** |
| **Lo requieren** (§8.2) | ADR-04.C, ADR-05.C, ADR-08, ADR-14 y, por la decisión conjunta ADR-01 / ADR-10, ambos |
| **Parámetros que debe soportar** | A-1, A-2, A-4, A-5, A-6, A-8, A-12, A-15, A-16, Q-40 (a)–(c), Q-42 y las preguntas que condicionan celdas (§17.6) |
| **Evidencia de aprobación humana** | **La aprobación condicional de R2-B1 como entregable documental (2026-11-18, §17.0) no aprueba ADR-02.C ni sus formulaciones:** nada de esto es decisión |

**Contexto.** CMP-08 es el **único decisor** de permisos por usuario, rol, torneo, equipo, partido y efecto; **lee** roles e historial de CMP-07 y **no los almacena**; CMP-06 emite el nivel de autenticación (§5.3). `01 RT-02` y R1 `RC-04` exigen la comprobación en el servidor **y** en la capa de datos, y excluyen las alternativas de una sola capa. R1 PA-13 manda aplicar la lectura provisional por caso de `01 §13.7` mientras una decisión esté abierta, sin conceder un permiso por interpretación.

### 17.2 Invariantes IN-02

**Cómo se leen los tipos.** **[CONF]**: el texto citado es un requisito o restricción **confirmado** de `01` (o una restricción aprobada de R1) y el invariante lo recoge sin añadir nada. **[DER]**: el invariante **se deriva** de lo citado mediante un paso que el autor explica en la columna «Paso derivado»; **no está escrito literalmente** en `01` ni en R1. **[REC]**: propuesta. **Respaldo literal:** extracto textual (sin formato) de la fuente citada; §17.12 comprueba que cada extracto existe.

**A. Invariantes obligatorios** (26: 16 [CONF] y 10 [DER]):

| ID | Invariante | Tipo | Fuente | Respaldo literal | Paso derivado (solo [DER]) |
|---|---|---|---|---|---|
| IN-02-01 | **Toda autorización se comprueba en el servidor y en la capa de datos; ningún permiso se verifica en una sola capa.** Ocultar un control, o confiar en lo que declara el cliente o guarda el dispositivo, no es un control | [CONF] | RT-02, RC-04, RF-ACC-15, IN-03-11; CL-48 | «La autorización se comprueba en el servidor y en la base de datos» (RT-02); «un rol no se modifica por parámetros del cliente» (RF-ACC-15) | — |
| IN-02-02 | **Un solo decisor y una sola definición de la decisión:** las dos capas aplican la **misma** definición de permiso (CMP-08); no existen dos definiciones que puedan discrepar | [DER] | CR1-13, §5.3 (CMP-08) | «toda decisión de permiso es de CMP-08» (CR1-13) | Si cada capa aplicara una definición propia habría **dos decisores**, contra CR1-13. Es la lectura más estricta de «único decisor»; la propiedad de implementación es la condición K-1 |
| IN-02-03 | **La autorización vigente se evalúa en la misma unidad que aplica la operación:** ninguna operación se aplica con una autorización ya revocada y confirmada, y ninguna queda a medias (**qué operaciones forman cada unidad es de R2-B2**) | [DER] | RT-13 (estado DER en `01`), OFF-22; CL-45 | «la autorización de una operación se evalúa en la misma transacción que la aplica» (RT-13) | RT-13 está confirmado como derivado en `01`; aquí se aplica a las dos capas |
| IN-02-04 | **Aislamiento entre torneos:** todo permiso, salvo el del dueño, se limita a un torneo; toda referencia entre entidades pertenece al **mismo** torneo, y la capa de datos lo impone aunque el cliente y el servidor se equivoquen | [CONF] | RT-01, RT-08, RF-ACC-08; CL-21, CL-40 | «debe pertenecer al mismo torneo» (RT-08); «rechazados por la base de datos» (RT-08, criterio de aceptación) | — |
| IN-02-05 | **Sin transferencia:** ningún permiso pasa de un torneo a otro, ni a través de un vínculo de identidad de equipo, que **no es criterio de autorización** | [CONF] | RT-01, RT-12, RF-EQP-06 (diferido) | «los permisos no se transfieren entre torneos» (RT-01); «no es un criterio de autorización» (RT-12) | — |
| IN-02-06 | Todo permiso se evalúa por **usuario, rol, torneo, equipo, partido y efecto**, según corresponda | [CONF] | RT-01 (las cinco primeras dimensiones), RT-14 (el efecto) | «usuario, rol, torneo, equipo y partido» (RT-01) | — |
| IN-02-07 | **Acumulación de roles:** los permisos de una cuenta con varios roles son la **unión** de los de cada rol evaluados por separado en su propio ámbito; ninguna combinación concede más que esa unión ni evade RT-05 o RT-14; revocar un rol o una asignación adicional no elimina los permisos legítimos que procedan de otros roles vigentes | [CONF] | RT-11, RF-ACC-08, RF-ACC-15; CL-50, CL-53 | «ninguna combinación concede más que esa unión ni evade las restricciones de §7» (RT-11) | — |
| IN-02-08 | **Separación de roles y facultades (anti-escalada):** un rol no confiere por sí mismo una facultad fuera de su ámbito; ningún rol concede uno igual o superior al propio, **salvo las excepciones de RT-03 y RT-06** (el dueño designa organizadores y un segundo dueño); nadie se concede roles ni asignaciones a sí mismo (única excepción: la asignación inicial del creador de un torneo, RF-ACC-12); el ámbito de una invitación nunca excede el de quien invita | [CONF] | RT-03, RT-04, RT-06, RT-07, RF-ACC-02, 03, 15; CL-19, CL-20 | «Ningún usuario —incluido el dueño— puede invitarse, asignarse ni concederse roles o asignaciones adicionales a sí mismo» (RT-04) | — |
| IN-02-09 | **Facultades reservadas al dueño:** crear o invitar organizadores, otorgar el rol de dueño (solo a un segundo dueño), revocar organizadores y revocar o degradar a otro dueño; ningún organizador las ejerce. **La nueva autenticación con segundo factor y el motivo** de las operaciones sobre dueños se exigen en **IN-03-04 (ADR-03.C)**; ADR-02.C exige que el evaluador no autorice sin ese nivel (IN-02-15) | [CONF] | RF-ACC-03, RT-06, matriz `01 §7.1` filas 1, 4, 5 y 35; CL-38 | «ningún organizador concede ni revoca el rol de dueño» (RT-06) | — |
| IN-02-10 | **Protección del último dueño activo:** no se revoca ni degrada, **de forma atómica incluso con operaciones simultáneas**; no hay jerarquía técnica entre dueños | [CONF] | RT-06, RF-ACC-09; CL-43 | «No se puede revocar ni degradar al último dueño activo, de forma atómica incluso con operaciones simultáneas» (RT-06) | — |
| IN-02-11 | **RT-05:** un delegado no actúa como editor en partidos de su propio equipo; se verifica **al asignar**, **al otorgar el rol de delegado** (las asignaciones en conflicto se retiran con auditoría) y **al sincronizar**; no se asigna como editor a un delegado de un equipo del partido | [CONF] | RT-05, RF-ACC-06; CL-35, CL-49 | «un delegado no puede actuar como editor en partidos en que juega su propio equipo» (RT-05) | — |
| IN-02-12 | **RT-14:** la restricción se evalúa **por efecto** (todo partido que la operación modificaría, incluidos efectos automáticos o en cascada; las operaciones automáticas heredan la situación del actor); si alcanza un equipo del que el actor es o fue delegado en ese torneo sin restricción levantada, la operación se **rechaza por completo**; consultar y previsualizar no se restringen; sin actor alterno la operación se **bloquea**, sin excepciones automáticas ni afectar la protección del último dueño activo (RT-06); **se aplica también al sincronizar** operaciones sin conexión. **La persistencia histórica no se extiende a editores**, que conservan RT-05. **El levantamiento** solo lo aprueba otro dueño activo y sin conflicto, con motivo, limitado a ese equipo y torneo, sin autoaprobación y sin permisos adicionales; **no cubre el conflicto vigente y pierde efecto si el rol de delegado de ese equipo se otorga de nuevo** | [CONF] | RT-14, RF-ACC-17; CL-48…CL-51, CL-56…CL-60 | «se rechaza por completo» (RT-14); «y al sincronizar operaciones sin conexión» (RT-14); «ni se afecta la protección del último dueño activo (RT-06)» (RT-14) | — |
| IN-02-13 | **El historial de asignaciones es la fuente de RT-14:** CMP-07 es la fuente única y de solo anexado; el evaluador **lee** las asignaciones vigentes y las revocadas y no las almacena | [DER] | RF-ACC-11, IN-09-02, §5.3 (CMP-07, CMP-08) | «no elimina ni altera el historial necesario para evaluar RT-14» (RF-ACC-11) | RF-ACC-11 fija el historial; que el evaluador lo **lea** y no lo guarde es la división de CMP-07 y CMP-08 en R1 |
| IN-02-14 | **Mínimo privilegio en el alcance:** cada rol recibe únicamente las facultades que la matriz de `01 §7.1` le concede, en su ámbito concreto; ninguna función concede a un rol un alcance mayor que el de la matriz; **ninguna formulación concede un permiso por interpretación** de lo ambiguo (rige PA-13: lectura provisional por caso; la denegación por defecto solo donde `01 §13.7` la indica) | [DER] | RT-01, introducción de `01 §7`, PA-13, `SKILL.md` (reglas críticas de dominio) | «ningún permiso se hereda entre torneos» (`01 §7`, introducción); «No aceptar una función que otorgue a todos los editores permiso sobre todo el torneo» (`SKILL.md`) | «Mínimo privilegio» es el nombre del principio que expresan ambos textos (alcance concreto de cada permiso y rechazo de una función más amplia que la matriz); **no es un requisito de `01` con ese nombre**. Se distingue de IN-02-18 (que ADR-02.C no concede ni retira facultades) en que IN-02-14 limita el **alcance de cada rol**. Las credenciales de servicio son [REC] (IN-02-29) |
| IN-02-15 | **El nivel de autenticación se consume, no se emite:** el evaluador compara el nivel que emite CMP-06 con la política efectiva calculada con **todos** los roles vigentes (dueño u organizador exigen segundo factor) | [DER] | RC-11, RF-ACC-10, IN-03-01, IN-03-02, HR1-04; CL-55 | «La política efectiva se calcula por cuenta con todos los roles vigentes» (RF-ACC-10) | La política es de `01`; que CMP-08 la compare con un nivel emitido por CMP-06 es la división de R1 §5.3 |
| IN-02-16 | **Toda concesión, modificación o revocación de permisos, y todo rechazo o bloqueo por RT-14, producen el evento que debe auditarse;** ADR-02.C **no define** dónde persiste ese evento (ADR-09.C: IN-09-04 e IN-09-05) | [DER] | RT-10, RF-ACC-11, RF-AUD-06; CL-48, CL-49 | «Toda asignación, modificación o revocación de permisos queda auditada» (RT-10) | `01` exige la auditoría; que la **decisión** produzca el evento y su persistencia sea de ADR-09.C es la partición de R2-A |
| IN-02-17 | **Las ambigüedades abiertas de ADR-02 (A-1, A-2, A-4, A-5, A-6, A-8, A-12, A-15, A-16) y las preguntas Q-40 y Q-42 se soportan como parámetros del evaluador:** el modelo **no elige** una lectura | [DER] | R1 §9.3, PA-13, `01 §13.7` | «el diseño debe soportar ambas lecturas y no elegir» (R1 §9.3) | R1 §9.3 pide soportar ambas lecturas de las ambigüedades abiertas; aquí se aplica a las de ADR-02 y a Q-40 y Q-42 como parámetros del evaluador (PA-13) |
| IN-02-18 | **Sin permisos nuevos y sin reducción de garantías:** ADR-02.C organiza y condiciona facultades ya confirmadas en `01 §7`; no concede ninguna ni retira ninguna | [DER] | CR1-13, PA-13, IN-03-17 | «Ningún componente ni flujo concede un permiso nuevo» (CR1-13) | CR1-13 dice que ningún componente ni flujo concede un permiso nuevo; aquí se aplica a ADR-02.C y se añade que tampoco retira garantías (IN-03-17 lo dice de ADR-03.C) |
| IN-02-19 | **Sin proveedor, producto, mecanismo ni stack:** toda propiedad que dependa del proveedor queda condicionada a ADR-01 (ADR-02.I) | [CONF] | RC-22, RC-23, RC-25 | «Ningún proveedor ni stack está aprobado» (RC-23) | — |
| IN-02-20 | **Los permisos dependen del estado del torneo y del partido** donde `01` lo establece: corregir un resultado con el torneo `archivado` solo lo hace el dueño; archivar, reabrir y cerrar desde `suspendido` quedan en sus filas; el evaluador recibe el estado como entrada y **las operaciones bloqueadas en `archivado` son el parámetro de Q-40 (c)** | [CONF] | RF-TOR-02, D-5, matriz `01 §7.1` filas 27, 37 y 41; CL-34, CL-44 | «desde ese estado únicamente el dueño corrige resultados» (RF-TOR-02) | — |
| IN-02-21 | **Un delegado lo es de un solo equipo por torneo,** también con invitaciones o altas simultáneas; puede serlo de equipos de torneos distintos | [CONF] | RF-ACC-13, RF-ACC-02; CL-54 | «una persona solo puede ser delegada de un equipo por torneo» (RF-ACC-13) | — |
| IN-02-22 | **Cada partido tiene un único editor vigente;** la reasignación desactiva la anterior y activa la nueva de forma atómica, sin ventana con dos editores vigentes, también ante reasignaciones simultáneas; el editor asignado tiene rol de editor vigente en ese torneo | [CONF] | RF-ACC-06; CL-41, CL-47 | «Cada partido tiene un único editor vigente.» (RF-ACC-06) | — |
| IN-02-23 | **Restricciones del delegado y del editor:** el delegado solo administra la plantilla de su equipo, no edita la inscripción y no invita ni asigna roles; el editor solo actúa en los partidos que se le asignaron, no se autoasigna partidos y no invita ni asigna roles; ninguno de los dos actúa en otro torneo con ese rol | [CONF] | RF-ACC-05, RF-ACC-06, RF-ACC-13, matriz `01 §7.1` filas 7, 17, 19 y 24; CL-20 | «Editor y delegado no pueden invitar usuarios ni asignar roles.» (RF-ACC-05) | — |
| IN-02-24 | **El alta de cuentas solo es posible con una invitación vigente y se impone en las dos capas** (sin auto-registro); la definición de la invitación y su ciclo de vida son de ADR-03.C (IN-03-05) | [DER] | RF-ACC-01 (requisito M de `01`, estado REC), RF-ACC-02, IN-03-05 | «El registro de cuentas es solo por invitación; no existe auto-registro.» (RF-ACC-01) | RF-ACC-01 exige que falle en la interfaz y en la capa de servicio y de datos; aquí se reduce a la parte de autorización de las dos capas |
| IN-02-25 | **La capa de datos conoce la identidad del actor y su nivel de autenticación de forma verificable,** no declarada por el cliente ni guardada en el dispositivo; sin ello no hay segunda capa de autorización. **Qué mecanismo lo logra es de R3 (H2B1-02)** | [DER] | RT-02, RC-04, IN-03-11, PA-01 | «La autorización se comprueba en el servidor y en la base de datos» (RT-02); «ningún dato guardado en el dispositivo concede acceso ni permiso en el servidor» (IN-03-11) | Para que la base de datos «compruebe» una autorización necesita saber quién actúa; sin un actor verificable esa comprobación sería un trámite (RB1-02) |
| IN-02-26 | **Creación de torneo:** el organizador autorizado que crea un torneo queda asignado como organizador de **ese torneo y de ninguno más**, una sola vez; esa asignación es una **operación del sistema autorizada previamente** y no una autoasignación discrecional (excepción de IN-02-08); no obtiene permisos sobre torneos ajenos ni puede crear o designar organizadores; si la asignación falla, el torneo no se crea (**qué unidad lo garantiza es de R2-B2**) | [CONF] | RF-ACC-12, RT-04; matriz `01 §7.1` fila 10 | «queda automáticamente asignado como organizador de ese torneo y de ninguno más» (RF-ACC-12); «es una operación del sistema autorizada previamente» (RF-ACC-12) | — |

**B. Propuestas [REC]** (**no obligan** hasta su aprobación; no cuentan para la conformidad de §17.3):

| ID | Propuesta | Tipo | Fuente | Respaldo literal |
|---|---|---|---|---|
| IN-02-27 | Las respuestas de denegación **no distinguen** entre «el recurso no existe» y «pertenece a otro torneo», para no permitir la enumeración de recursos ajenos | [REC] | CL-21; `seguridad.md` | «respuestas que no filtren datos internos» (`seguridad.md`) |
| IN-02-28 | Los intentos denegados de escalada y de acceso cruzado generan el evento de la bitácora de seguridad del dueño (distinto de los rechazos por RT-14) | [REC] | RF-AUD-05 (prioridad S, estado REC) | «Los intentos denegados de escalada o acceso cruzado se registran en una bitácora de seguridad» (RF-AUD-05) |
| IN-02-29 | Las credenciales de servicio con privilegios totales no llegan al cliente ni sustituyen la decisión de permiso de un usuario | [REC] | RT-09 (estado REC en `01`), `seguridad.md` | «nunca llegan al navegador ni a la cola local» (RT-09) |

**Numeración.** Los invariantes IN-02-01…19 conservan su identificador de la v0.1; **IN-02-20…25 se añadieron en la v0.2** (hallazgo B-04) e **IN-02-26 en la v0.3** (hallazgo C-07, RF-ACC-12). Las propuestas [REC], que eran IN-02-20 e IN-02-21 en la v0.1 y IN-02-26, 27 y 28 en la v0.2, son ahora **IN-02-27, 28 y 29**. **Total: 26 obligatorios y 3 [REC] = 29.**

### 17.3 Formulaciones de capacidad (alternativas) y su conformidad

**Decisión D2-1 — Reparto de la autorización entre las dos capas.** Se evalúa **cada formulación contra los 26 invariantes obligatorios** de §17.2 (104 celdas = 26 × 4). **Ninguna se elige aquí.**

**Criterio uniforme de los rótulos** (el mismo para todas las celdas):

| Rótulo | Criterio |
|---|---|
| **Conforme** | Cumple la obligación **sin depender de una decisión pendiente ni de un supuesto no aprobado**. Una celda que escribe una condición **no** puede llamarse «Conforme». **El texto que sigue a «Conforme:» es una justificación, no una condición** |
| **Condicionada (K-n)** | Puede cumplir, pero **depende de una condición explícita** K-n (más abajo) |
| **No conforme** | **Omite** una verificación exigida o incumple una garantía obligatoria; **no cuenta como alternativa viable**. Una formulación en la que la capa de datos omita una verificación exigida es **siempre** «No conforme» |
| **No aplica** | No se usa: ninguna celda lo justifica |

**Convención sobre el stack.** Si un mecanismo concreto puede realizar una capacidad con el stack seleccionado se comprueba en **ADR-02.I** (R3, §16.8.5 [REC]); **ninguna celda presupone esa viabilidad ni la condiciona**. IN-02-25 es parte de la **definición** de las cuatro formulaciones; **su conformidad depende de la formulación de ADR-03.C que se adopte (K-5)**, que es una decisión pendiente y no una cuestión de stack.

**Condiciones de conformidad:**

| ID | Condición |
|---|---|
| **K-1** | Una **sola definición** de la decisión, ejecutada de forma idéntica en todas las capas que la ejecutan (propiedad exigida a ADR-02.I; su incumplimiento es la deriva de RB1-01) |
| **K-2** | (solo CAP-02-D) Conjunción de cuatro componentes. **K-2a Completitud:** la proyección contiene los **hechos derivados** que necesita la parte de la decisión que depende de roles, asignaciones y restricciones (las entradas de IN-02-06, 12, 15, 23 y 26); **el estado del torneo y del partido, las reglas de integridad y la invitación vigente no forman parte de la proyección: se leen de sus fuentes** (§17.3.5). **K-2b Derivación:** deriva de la fuente única de CMP-07 y del historial de solo anexado, la calcula la **definición única de CMP-08** y no es un segundo decisor (§17.3.5); **debe poder reconstruirse desde las fuentes** (propiedad exigida; su demostración, R2-B2). **K-2c Actualización coherente:** se actualiza con **toda** operación que modifica permisos (lista en H2B1-10); las operaciones que modifican estados **no** la actualizan en esta formulación. **K-2d Garantías transaccionales:** la lectura de la proyección y la aplicación de la operación, y la actualización de la proyección con cada cambio de permisos, ocurren en la misma unidad, sin ventana con una proyección obsoleta (RT-13); **corresponde exclusivamente a R2-B2** |
| **K-3** | La persistencia del rechazo **impuesto por la capa de datos** depende de la formulación de ADR-09.C que se adopte: **CAP-09-P-A o P-C**; CAP-09-P-B solo cubre los rechazos que decide el evaluador. **Decisión 10 de R2-A y D9-1: abiertas** |
| **K-4** | *Retirada en la v0.4 (D-05).* B no incluye la invitación vigente entre sus reglas de integridad, de modo que la **omite**; B·IN-02-24 es «No conforme» y no «Condicionada». El identificador no se reutiliza |
| **K-5** | La capacidad depende de la **formulación de ADR-03.C que se adopte** (PROPUESTA; decisiones 1, 5 y 12 de R2-A abiertas): CAP-03-N-B exige que la capa de datos reciba de forma verificable el instante de verificación del segundo factor; CAP-03-N-C, que consuma el comprobante en la misma unidad (§17.7) |

**Formulaciones.** *Decisión completa:* alcance, rol, torneo, equipo, partido y efecto; reservas del dueño; último dueño; RT-05; RT-14; estado del torneo y del partido; política efectiva de segundo factor; **invitación vigente para el alta de cuentas (IN-02-24)**; **reglas de creación de torneo (IN-02-26)**. En las cuatro, **la capa de datos recibe identidad y nivel verificables** (IN-02-25).

| ID | Formulación |
|---|---|
| **CAP-02-A** | **Decisión completa en ambas capas, con una sola definición.** CMP-08 define una vez la decisión completa. El **servidor** la ejecuta antes de aplicar (rechazo temprano) y la **capa de datos** la ejecuta **de nuevo con sus propios datos** en la unidad que aplica la operación. La capa de datos nunca toma la decisión del servidor como prueba |
| **CAP-02-B** | **Reparto por naturaleza de la regla.** El **servidor** decide los permisos de negocio (alcance, reservas, RT-05, RT-14 por efecto, política de segundo factor, estado). La **capa de datos** impone solo las **reglas de integridad**: referencias del mismo torneo, unicidades (delegado por torneo, editor vigente por partido), último dueño activo y solo anexado. **La invitación vigente para el alta de cuentas no figura entre las reglas de integridad de B** |
| **CAP-02-C** | **Decisión del servidor ligada a una versión, con un piso independiente parcial en la capa de datos.** El servidor decide con la definición completa y la asocia a la **versión** del estado de roles, asignaciones e historial. La capa de datos (1) rechaza si esa versión ya no es la vigente al confirmar y (2) impone un **piso**: torneo y rol vigente del actor, equipo y partido asignado, reservas del dueño, anti-escalada, último dueño, RT-05 también al sincronizar, estado, unicidades, **invitación vigente para el alta de cuentas y reglas de creación de torneo**. **El efecto de RT-14 y la política de segundo factor los decide solo el servidor.** Si el piso se completara con esas dos verificaciones, C pasaría a ser **A más la comprobación de versión** (§17.3.3) |
| **CAP-02-D** | **Consulta de permisos efectivos derivados.** CMP-07 mantiene, junto con **cada** cambio de roles, asignaciones o historial, una **proyección de hechos derivados sobre permisos** (roles vigentes por cuenta, torneo, equipo y partido; equipos de los que la cuenta es delegada vigente; restricciones históricas de RT-14 no levantadas; asignaciones de editor vigentes; política efectiva de segundo factor por cuenta), calculada con la **definición única de CMP-08** y derivada del historial, que sigue siendo la fuente única (§17.3.5). El **servidor** y la **capa de datos** deciden **consultando esa proyección** para la parte de la decisión que depende de roles, asignaciones y restricciones, y **leyendo de sus fuentes** el estado del torneo y del partido, las reglas de integridad y la invitación vigente; el efecto de RT-14 se obtiene cruzando los partidos que la operación modifica con las restricciones de la proyección. **El estado no forma parte de la proyección:** incorporarlo sería una variante distinta, **no evaluada aquí** (H2B1-10) |

**Matriz de conformidad** (26 invariantes × 4 formulaciones):

| Invariante | CAP-02-A | CAP-02-B | CAP-02-C | CAP-02-D |
|---|---|---|---|---|
| IN-02-01 Ambas capas | **Conforme** | **No conforme:** los permisos que no son reglas de integridad (alcance, reservas, RT-14, segundo factor) solo se verificarían en el servidor; RF-ACC-03 (la rechazan el servicio y la base de datos), RF-ACC-05 («denegados en BD») y CL-48 («Denegado en servidor y BD») piden la denegación en la base de datos | **No conforme:** el efecto de RT-14 y la política de segundo factor solo se decidirían en el servidor (CL-48) | **Condicionada (K-2):** ambas capas consultan la proyección; depende de que contenga todo lo que la decisión necesita |
| IN-02-02 Un solo decisor | **Condicionada (K-1)** | **Condicionada (K-1):** las reglas de integridad deben derivar de la misma definición | **Condicionada (K-1):** el piso debe derivar de la misma definición | **Condicionada (K-1, K-2):** la proyección es derivada y no debe ser un segundo decisor |
| IN-02-03 Misma unidad | **Conforme** | **No conforme:** la decisión del servidor es previa y externa a la unidad que aplica (CL-45) | **No conforme:** el efecto de RT-14 y la política de segundo factor se decidirían antes y fuera de la unidad | **Condicionada (K-2):** la consulta y la actualización de la proyección deben ir en la unidad |
| IN-02-04 Aislamiento entre torneos | **Conforme** | **No conforme (parcial):** conforme para la integridad referencial (RT-08); no conforme para el **alcance del actor** (una solicitud bien formada de un organizador del torneo A sobre B) | **Conforme:** el piso incluye el torneo del actor y la integridad referencial | **Condicionada (K-2)** |
| IN-02-05 Sin transferencia | **Conforme** | **No conforme (parcial):** conforme respecto del vínculo de identidad (ninguna formulación lo usa); no conforme respecto de «los permisos no se transfieren entre torneos» (RT-01), porque el alcance del actor por torneo solo se verificaría en el servidor | **Conforme** | **Condicionada (K-2):** los permisos de cada torneo deben mantenerse separados en la proyección |
| IN-02-06 Usuario, rol, torneo, equipo, partido, efecto | **Conforme** | **No conforme:** partido y efecto solo en el servidor | **No conforme:** el efecto solo en el servidor | **Condicionada (K-2)** |
| IN-02-07 Acumulación de roles | **Conforme** | **No conforme:** la unión de permisos por rol solo en el servidor | **Conforme:** el piso evalúa cada rol en su ámbito | **Condicionada (K-2):** al revocar un rol, la proyección debe conservar los permisos de los otros roles |
| IN-02-08 Anti-escalada | **Conforme** | **No conforme:** RF-ACC-05 («denegados en BD») y RF-ACC-15 (prueba de tipo «DB»: contra la base de datos real) no son reglas de integridad | **Conforme:** el piso incluye quién concede qué | **Condicionada (K-2)** |
| IN-02-09 Reservas del dueño | **Condicionada (K-5):** las operaciones sobre dueños exigen un nivel de autenticación reciente (IN-02-15), que depende de la formulación de ADR-03.C | **No conforme (parcial):** RT-06 sí es regla de integridad; las demás reservas y el nivel de autenticación de las operaciones sobre dueños (IN-02-15) solo se verificarían en el servidor | **No conforme (parcial):** el piso cubre las reservas, pero el nivel de autenticación de las operaciones sobre dueños solo se evaluaría en el servidor (IN-02-15) | **Condicionada (K-2, K-5)** |
| IN-02-10 Último dueño activo | **Conforme** | **Conforme:** regla de integridad atómica | **Conforme** | **Conforme:** regla de integridad independiente de la proyección |
| IN-02-11 RT-05 | **Conforme** | **No conforme:** RT-05 no figura entre las reglas de integridad de B y no se verificaría en la capa de datos al asignar, al otorgar el rol ni al sincronizar | **Conforme:** el piso la incluye también al sincronizar | **Condicionada (K-2)** |
| IN-02-12 RT-14 | **Conforme** | **No conforme:** el efecto sobre partidos e historial no es una regla de integridad; CL-48 pide «Denegado en servidor y BD» | **No conforme:** el efecto se evaluaría solo en el servidor | **Condicionada (K-2):** incluidas las restricciones históricas no levantadas |
| IN-02-13 Historial como fuente | **Conforme** | **Conforme** | **Conforme** | **Condicionada (K-2):** la proyección no sustituye al historial de solo anexado |
| IN-02-14 Mínimo privilegio en el alcance | **Conforme** | **No conforme (parcial):** el alcance de cada rol solo se limitaría en el servidor (IN-02-04, 06 y 23) | **Conforme** | **Condicionada (K-2):** el alcance por rol sale de la proyección |
| IN-02-15 Nivel consumido | **Condicionada (K-5):** el nivel que llega a la capa de datos y su frescura dependen de la formulación de ADR-03.C | **No conforme:** el nivel solo se evalúa en el servidor; para las operaciones de IN-03-04 queda en una capa | **No conforme:** la política de segundo factor solo en el servidor | **Condicionada (K-2, K-5):** la política efectiva por cuenta forma parte de la proyección y el nivel depende de ADR-03.C |
| IN-02-16 Evento auditable | **Condicionada (K-3)** | **Condicionada (K-3)** | **Condicionada (K-3)** | **Condicionada (K-3)** |
| IN-02-17 Ambigüedades como parámetros | **Conforme:** parámetros de la definición única | **No conforme:** las lecturas alternativas solo se aplicarían en el servidor (hereda IN-02-01) | **No conforme:** las lecturas del efecto de RT-14 solo se aplicarían en el servidor | **Condicionada (K-2):** la proyección debe poder construirse con ambas lecturas |
| IN-02-18 Sin permisos nuevos | **Conforme** | **Conforme** | **Conforme** | **Conforme** |
| IN-02-19 Sin selección tecnológica | **Conforme** | **Conforme** | **Conforme** | **Conforme** |
| IN-02-20 Estado del torneo y del partido | **Conforme** | **No conforme:** el estado solo se evaluaría en el servidor (CL-44; las pruebas de §7.3 se repiten por estado) | **Conforme:** el piso incluye el estado | **Condicionada (K-2):** el estado se lee de su fuente, pero que solo el dueño corrija con el torneo `archivado` depende del rol, que sale de la proyección |
| IN-02-21 Un equipo por delegado y torneo | **Conforme** | **Conforme** | **Conforme** | **Conforme** |
| IN-02-22 Un editor vigente por partido | **Conforme** | **Conforme** | **Conforme** | **Conforme** |
| IN-02-23 Restricciones del delegado y del editor | **Conforme** | **No conforme:** el alcance por equipo y por partido es un permiso, no una regla de integridad (RF-ACC-05, CL-20) | **Conforme** | **Condicionada (K-2)** |
| IN-02-24 Alta solo con invitación | **Conforme:** la decisión completa incluye la invitación vigente | **No conforme:** la invitación vigente no figura entre las reglas de integridad de B y la capa de datos no la verificaría (RF-ACC-01 pide el rechazo en las dos capas) | **Conforme:** el piso incluye la invitación vigente | **Conforme:** la invitación vigente se lee de su fuente, ajena a la proyección |
| IN-02-25 Identidad y nivel verificables | **Condicionada (K-5):** forma parte de la definición, pero qué identidad, qué nivel y qué instante llegan a la capa de datos depende de la formulación de ADR-03.C | **Condicionada (K-5)** | **Condicionada (K-5)** | **Condicionada (K-5)** |
| IN-02-26 Creación de torneo | **Conforme** | **No conforme (parcial):** «si falla la asignación, el torneo no se crea» sí es una regla de integridad; que el creador quede asignado solo a ese torneo y sin permisos sobre los ajenos es alcance del actor y solo se verificaría en el servidor | **Conforme:** el piso incluye el alcance del actor, la anti-escalada y las reglas de creación | **Condicionada (K-2):** la asignación inicial debe actualizar la proyección en la misma unidad (UI-01) |

*Nota sobre IN-02-22.* La matriz evalúa de IN-02-22 la **unicidad del editor vigente y la atomicidad de la reasignación** (reglas de integridad). La exigencia de que el editor asignado tenga rol de editor vigente en el torneo (RF-ACC-06) se evalúa en IN-02-06 e IN-02-23.

#### 17.3.1 Recuento y conclusión de D2-1

| Formulación | Conforme | Condicionada | No conforme | Conclusión (ninguna se elige) |
|---|---|---|---|---|
| **CAP-02-A** | 21 | 5 (K-1, K-3 y K-5 tres veces) | 0 | **Sin celdas «No conforme»; conformidad condicionada a K-1, K-3 y K-5** (ADR-02.I, ADR-09.C y ADR-03.C). Es la única formulación sin celdas «No conforme» **y** sin la condición K-2 |
| **CAP-02-B** | 6 | 3 (K-1, K-3, K-5) | 17 | **No conforme.** No cuenta como alternativa viable. Se conserva como referencia para mostrar por qué RT-02 excluye el reparto por naturaleza de la regla |
| **CAP-02-C** | 16 | 3 (K-1, K-3, K-5) | 7 | **No conforme** tal como está definida (el efecto de RT-14, la política de segundo factor y el nivel de las operaciones sobre dueños solo en el servidor). **Completada, es A más la versión** (§17.3.3) |
| **CAP-02-D** | 6 | 20 (18 con K-2, una de ellas además con K-1 y dos con K-5; 1 con K-3; 1 con K-5 sola) | 0 | **Condicionada.** Sin celdas «No conforme», pero 18 celdas dependen de K-2, de la que **solo una parte (K-2d y la demostración de K-2b) corresponde exclusivamente a R2-B2** (§17.3.3), y **su independencia respecto de A no está demostrada** (§17.3.3) |

#### 17.3.2 Las propuestas [REC] no cuentan

IN-02-27, 28 y 29 son propuestas: **no se evalúan para la conformidad** y ninguna formulación se declara conforme o no conforme por ellas.

#### 17.3.3 Evaluación de la independencia de las alternativas (B-02, C-01)

**CAP-02-C frente a CAP-02-A.** C solo cumple RT-02 (IN-02-01), RT-13 (IN-02-03) y CL-48 si su piso incluye **las mismas verificaciones completas** que A: el efecto de RT-14 y la política de segundo factor. Con esas dos verificaciones en la capa de datos, el piso **es** la segunda ejecución de A, y la comprobación de versión es **redundante** porque la capa de datos ya evalúa en la misma unidad que aplica (IN-02-03). **Se declara la equivalencia: C completa ≡ A + comprobación de versión; C no es un modelo de reparto independiente.** Tal como está definida (piso parcial), C es **No conforme** en 7 celdas.

**CAP-02-B.** Es una referencia: RT-02 y RC-04 excluyen que un permiso se verifique en una sola capa, y B lo hace con todos los permisos que no son reglas de integridad.

**CAP-02-D frente a CAP-02-A: dos interpretaciones.** R2-B1 **no decide cuál es la correcta**; **la independencia de D respecto de A no está demostrada.**

| Interpretación | Argumento | Qué la sustenta |
|---|---|---|
| **I. D es una forma de reparto distinta** | A evalúa las reglas sobre las fuentes (roles, asignaciones, historial, estado) en cada operación; D **consulta una proyección derivada** mantenida con cada cambio. Difieren en dónde vive la complejidad (en las lecturas frente a las escrituras), en el riesgo que añaden (divergencia de la proyección, RB1-13), en lo que debe conservarse (D exige un estado derivado de permisos) y en la frontera atómica (D añade el mantenimiento de la proyección a las unidades que modifican permisos, K-2c) | Las diferencias de riesgo, de mantenimiento y de obligación transaccional son reales y no son cosméticas |
| **II. D es una variante de A** | En ambas formulaciones **el servidor y la capa de datos toman la decisión completa, con las mismas comprobaciones**, y **la división entre las dos capas es la misma**. Lo único que cambia es la **representación de la entrada** (las fuentes o una proyección derivada de ellas). Las reglas que dependen de la operación (último dueño, unicidades, invitación vigente) D las evalúa sobre las fuentes (celdas D·IN-02-10, 21, 22 y 24); las demás dependen de que la proyección sea equivalente a las fuentes (K-2) | Si la proyección es equivalente a las fuentes (K-2b), D produce las mismas decisiones que A |

**Qué demostraría la independencia** [SUP]: que D cambie **qué verifica cada capa** o la **composición de las unidades atómicas** de modo que no se reduzca a «A con una entrada derivada». Eso depende de la frontera atómica (R2-B2) y de la decisión del usuario sobre qué cuenta como «forma de reparto» (DB1-1).

**Otras candidatas examinadas y descartadas** [SUP]:

| Candidata | Por qué se descarta |
|---|---|
| Decisión solo en la capa de datos; el servidor solo transporta | Deja al servidor sin verificar: **No conforme** con IN-02-01 (RT-02 exige ambas capas) |
| Mandato firmado por operación: el servidor decide y la capa de datos solo verifica el mandato | La capa de datos debe seguir evaluando el **efecto real** de la operación (RT-14, CL-48); con esa evaluación es A más una firma (un mecanismo, no un modelo); sin ella es C parcial |
| Permisos fijados al iniciar la sesión y consultados sin revalidar | **No conforme** con IN-02-03 y con RT-13: no habría revocación inmediata ni serialización con cambios de rol (D-2, D-11) |

**Qué puede establecer R2-B1 de K-2** (C-02) y qué queda para R2-B2:

| Componente | Qué puede hacer R2-B1 | Qué corresponde a R2-B2 |
|---|---|---|
| **K-2a Completitud** | **Enunciar** los hechos que la proyección debe contener (las entradas de IN-02-06, 12, 15, 23 y 26) y los que **no** contiene (estado, reglas de integridad, invitación vigente) | Comprobar la completitud sobre las unidades concretas |
| **K-2b Derivación** | Declarar la **lectura** que exige D (§17.3.5: CMP-08 define, CMP-07 mantiene los hechos derivados sin dejar de ser fuente única, CMP-08 los lee sin ser su origen) y someterla al usuario (H2B1-10); fijar la **propiedad exigida** de que la proyección se pueda reconstruir desde las fuentes. **No se presupone que modificar R1 sea obligatorio** | **Demostrar** esa reconstrucción (con K-2d) |
| **K-2c Actualización coherente** | **Listar** las operaciones que modifican **permisos** (H2B1-10); las que modifican estados no actualizan la proyección en esta formulación. **La enumeración definitiva por operación corresponde a la Fase 4** (R1 §5.5: «La lista no es exhaustiva… La enumeración definitiva por operación corresponde a la Fase 4») | Incorporar la actualización a las unidades que correspondan |
| **K-2d Garantías transaccionales** | Nada más que **enunciar** las propiedades exigidas (misma unidad, sin ventana con proyección obsoleta, RT-13) | **Exclusivamente R2-B2:** composición de las unidades, serialización de revocaciones (UI-11, CL-45), protección del último dueño (UI-04, RT-06) y reconstrucción desde el historial |

**Resultado.** Hay **una** formulación evaluada sin celdas «No conforme» y sin K-2 (A). D es una **candidata**: su independencia respecto de A tiene dos interpretaciones y su conformidad depende de K-2. **La exigencia de R1 §8.1 («≥ 2 formas de reparto con ambas capas siempre presentes») NO está declarada cumplida.** El déficit se mantiene **abierto** como **dependencia documentada de R2-B2** y como decisión del usuario (DB1-1): **no se concede excepción, no se modifica R1 §8.1 y no se da por demostrada la independencia.**

#### 17.3.4 Comparación cualitativa

Criterios de §2.4; juicio del autor [SUP], sin datos de costo; se omiten las formulaciones «No conforme» (B y C):

| Formulación | Costos bajos | Seguridad | Funcionamiento sin conexión | Mantenimiento |
|---|---|---|---|---|
| CAP-02-A | Desfavorable (ejecución doble de la decisión) | Favorable (dos ejecuciones independientes de una misma definición) | Neutral (la sincronización usa el mismo flujo, OFF-22) | Neutral (una definición, K-1) |
| CAP-02-D | Neutral (consultas simples; el costo está en mantener la proyección) | Neutral (depende de K-2; una divergencia equivale a un permiso erróneo en ambas capas) | Neutral | Desfavorable (la proyección debe mantenerse en cada cambio y reconstruirse desde el historial) |

**Lo que D2-1 no decide.** No decide dónde se ejecuta cada capa, con qué mecanismo ni la forma de una proyección o de una versión; eso es de ADR-02.I (R3). No decide la frontera atómica (R2-B2). **No orienta la selección:** A y D se presentan con sus condiciones, y la elección es del usuario (DB1-2).

#### 17.3.5 Responsabilidades de CMP-07 y CMP-08 en CAP-02-D (D-03)

**Lo que dice R1** (extractos literales): CMP-08 es «Único decisor de permisos por usuario, rol, torneo, equipo, partido y efecto» y «Lee roles e historial de CMP-07; no los almacena» (§5.3); CMP-07 es «fuente única de las asignaciones y de su historial de solo anexado» (§5.3); CR1-13 dice que «toda decisión de permiso es de CMP-08»; AC-1 (§5.8) dice que «CMP-07 invoca a CMP-08 para autorizar un cambio de roles (UI-04, UI-08) y CMP-08 lee los datos de CMP-07» y que «CMP-08 no invoca a CMP-07».

| Pregunta | Respuesta documentada | Dependencia pendiente |
|---|---|---|
| **¿Quién calcula la proyección?** | Interpretaciones **compatibles** con R1: **P-1:** CMP-07 la mantiene **ejecutando la definición única de CMP-08** (CMP-07 invoca a CMP-08, como ya hace en AC-1); CMP-08 sigue siendo el único que define qué es un permiso y CMP-07 solo conserva el resultado derivado. **P-2:** CMP-08 la calcula y la conserva, lo que exige leer «no los almacena» como referido a roles e historial y no a datos derivados. **P-3, incompatible:** CMP-07 la calcula con reglas propias; sería un segundo decisor contra CR1-13 y **no cuenta como lectura admisible** | P-1 y P-2 requieren que el usuario **confirme la lectura de R1** (DB1-6). **No se demuestra que R1 deba modificarse:** R1 no dice que una proyección derivada sea una segunda fuente ni que CMP-08 no pueda conservar datos derivados, pero tampoco lo permite expresamente |
| **¿Quién conserva la responsabilidad de decidir permisos?** | **CMP-08 en todo caso** (CR1-13). La proyección es un **dato** que CMP-08 consulta; cada operación se decide por CMP-08 con la definición única, en las dos capas | K-1 |
| **¿Qué datos pueden mantenerse como proyección derivada?** | **Hechos derivados** del historial y de las asignaciones: roles vigentes por ámbito (cuenta, torneo, equipo, partido), equipos de los que la cuenta es delegada vigente, restricciones históricas de RT-14 no levantadas, asignaciones de editor vigentes y política efectiva de segundo factor por cuenta (derivada de los roles). **No forman parte:** el estado del torneo y del partido, las reglas de integridad, la invitación vigente, el nivel de la sesión (CMP-06) ni el historial mismo, que sigue siendo la fuente única | K-2a |
| **¿Puede guardar decisiones por clase de operación («permitido» o «denegado»)?** | **No en esta formulación:** una decisión materializada sería un segundo momento de decisión. Una variante que lo hiciera solo sería admisible si fuera la salida **conservada y versionada de CMP-08**, recalculada por CMP-08 ante cualquier cambio de sus entradas, incluido el estado; **no se evalúa aquí** | Variante no evaluada |
| **¿Qué garantías impiden que sea un segundo decisor no autorizado?** | (g1) contiene **datos, no reglas**; (g2) toda decisión sobre una operación la toma CMP-08 con la definición única, en las dos capas (K-1); (g3) deriva del historial de solo anexado y debe poder reconstruirse desde él (K-2b); (g4) **ante una discrepancia entre la proyección y las fuentes prevalecen las fuentes** [REC]: garantía que este documento exige a la formulación y que no fija `01` ni R1; (g5) se actualiza en la misma unidad que cada cambio de permisos (K-2d) | g4 y K-2d: R2-B2 |

**Conclusión.** Con esas garantías, P-1 y P-2 son **técnicamente compatibles con R1 tal como está redactado**, a condición de que el usuario confirme la lectura (DB1-6 a). **No se demuestra que R1 deba modificarse.** R1 **no menciona** estas proyecciones: si el usuario no confirmara la lectura, CAP-02-D sería No conforme con IN-02-02 e IN-02-13 o requeriría una precisión editorial de R1 (que este documento no propone).

### 17.4 Modelo de capacidades por rol

**Alcance de esta sección.** Describe **qué familias de capacidades tiene cada rol y con qué límites**, tal como `01 §7.1` ya las confirma o las deja abiertas. **No es la matriz detallada de la Fase 4** y **no concede ni retira nada** (IN-02-18). **Regla de las celdas:** una fila de `01` con origen `COND` o `PEND` figura **[ABIERTO]** y **no se convierte en permiso provisional**: se cita lo que dice la fila y se soporta como parámetro. Las referencias «fila N» son a la matriz de `01 §7.1`; «Sí\*» = con la restricción indicada.

**Alcance base de cada rol** (qué dimensiones de IN-02-06 lo limitan):

| Rol | Alcance base | Fuente |
|---|---|---|
| **Dueño** | Global (todos los torneos y usuarios), **sujeto a RT-14 por equipo**; el segundo dueño tiene los mismos privilegios | RT-01, RF-ACC-09 |
| **Organizador** | Los torneos que **crea** o se le **asignan**; sin acceso automático a otros | RF-ACC-03, RF-ACC-12 |
| **Editor** | **Partidos concretos asignados** (un editor vigente por partido), en torneos donde tiene el rol | RF-ACC-06, RF-ADM-02 |
| **Delegado** | **Un equipo inscrito por torneo** y la plantilla de ese equipo | RF-ACC-13 |
| **Público** | Solo lo **publicado y confirmado**; el usuario autenticado sin rol equivale al público | RF-ACC-14, §9.6 |

**Familias de capacidades** (11 familias × 5 roles; las 41 filas de `01 §7.1` están asignadas a una familia en la tabla de cobertura siguiente):

| ID | Familia (filas) | Dueño | Organizador | Editor | Delegado | Público |
|---|---|---|---|---|---|---|
| CF-01 | **Cuentas y roles** (1 a 6, 35, 40) | Invita o designa a todos los roles (1 a 3); otorga el rol de dueño **solo** a un segundo dueño, con autenticación reforzada y auditoría (4); revoca organizadores y a cualquier usuario (5, 6); revoca o degrada a otro dueño (35) con **nueva autenticación con segundo factor y motivo (IN-03-04, ADR-03.C)** y la protección de IN-02-10; levanta la restricción histórica de otro dueño u organizador, siendo otro dueño activo y sin conflicto (40; IN-02-12) | Invita editores y delegados **solo** de **sus** torneos (2, 3); revoca editores y delegados de sus torneos y, de otro organizador, **solo el rol adicional** de editor o delegado en torneos comunes (6); **no** organizadores (1, 5), **no** el rol de dueño (4), **no** revoca ni degrada a un dueño (35), **no** levanta restricciones (40) | No (RF-ACC-05) | No (RF-ACC-05) | No (cuentas solo por invitación, RF-ACC-01) |
| CF-02 | **Asignación de editores a partidos** (7) | Sí\*: **no** asigna editores a partidos de un equipo del que es delegado (RT-14; A-2 **[ABIERTO]**) ni a un delegado de un equipo del partido (RT-05); el editor asignado debe tener rol de editor vigente en ese torneo | Sí\* solo partidos de **sus** torneos, con las mismas restricciones (RT-14; A-2 **[ABIERTO]**; RT-05) | **No** (ni a sí mismo) | No | No |
| CF-03 | **Administración del torneo** (10 a 15, 37) | Crea (10); edita y fija el reglamento antes de iniciar (11); publica y despublica (13); finaliza (14); elimina un `borrador` sin partidos (15). Cambiar el reglamento del torneo iniciado (12): la fila dice «Sí (con confirmación)» con origen `COND` Q-23 → **[ABIERTO]**. Archivar (37): la fila dice «Sí» con origen `COND` Q-40 → **[ABIERTO]** | Crea y queda asignado **solo** a ese torneo (10); edita antes de iniciar (11), publica y despublica (13) y finaliza (14) **los suyos**; **no** elimina (15). Reglamento iniciado (12): **PEND Q-23 [ABIERTO]**. Archivar (37): **PEND Q-40 (a) [ABIERTO]** | No | No | No |
| CF-04 | **Inscripciones y equipos** (16, 17) | Fila 16 (inscribir o retirar un equipo): Sí\* sujeta a RT-14 (A-4 **[ABIERTO]**). Fila 17 (editar nombre y colores): Sí; la lectura provisional de A-4 **no** la sujeta a RT-14 (la alternativa sí; **[ABIERTO]**) | Igual que el dueño, **solo en sus torneos** | No (filas 16 y 17) | **No** (filas 16 y 17) | No |
| CF-05 | **Jugadores y datos internos** (18 a 21, 34) | Sí (18, 19, 20, 21, 34); fila 21 con origen `COND` Q-42 → **[ABIERTO]** | Sí\* sus torneos: ver plantilla con datos internos (18), alta, baja y edición (19), consentimiento (20). Anonimizar o eliminar (21): **PEND Q-42 (b) [ABIERTO]**. **Exportar datos personales (34): No**, según la fila; Q-42 (b) **[ABIERTO]** sobre una facultad del organizador que `01` no concede | **Parcial** (18): solo nombre público o etiqueta y dorsal de los equipos de sus partidos asignados (PRV-11); **No** (19, 20, 21, 34) | Sí\* **su** equipo: ver plantilla con datos internos (18), alta, baja y edición (19), registrar autorización o consentimiento y su revocación (20); **No** (21, 34) | No (solo proyecciones públicas) |
| CF-06 | **Calendario** (22, 23) | Sí\* sujeto a RT-14 por efecto (A-3 está resuelta en `01`) | Sí\* **solo sus torneos**, sujeto a RT-14 por efecto | No | No | No |
| CF-07 | **Resultados y estados de partido** (24 a 29, 39, 41) | Registrar (24), cambiar estado (25), anular un evento (28), reanudar (39): Sí\* sujeto a RT-14; **corregir o reabrir** un finalizado, **también con el torneo `archivado`** (27). W.O. (26) y resolución manual de desempates (29): la fila tiene origen `COND` (Q-21, Q-22) → **[ABIERTO]** (el origen `COND` concierne a la regla deportiva (Q-21) y, en Q-22, también a quién resuelve: la fila queda abierta entera y su «Sí» no se toma como permiso ni como denegación). Cerrar desde `suspendido` (41): **no habilitada** (Q-29 **[ABIERTO]**) | Registrar (24) y cambiar estado (25): la fila dice «Sí\* suyos (COND Q-42)» → **[ABIERTO] Q-42 (a)**. W.O. (26): la fila dice «Sí\* suyos (REC)» con origen `COND` Q-21 → **[ABIERTO]**. Desempate manual (29): «Sí\* suyos (REC)» con origen `COND` Q-22 → **[ABIERTO]**. Corregir (27): Sí\* suyos **mientras el torneo no esté `archivado`**; con `archivado`: **No**. Anular (28): Sí\* suyos. Reanudar (39): Sí\* suyos (Q-46; la facultad de suspender y registrar del organizador depende de Q-42 (a)). Cerrar desde `suspendido` (41): **no habilitada** (Q-29) | Sí\* **solo partidos asignados**: registrar (24), cambiar estado (25), anular un evento **antes de finalizar** (28), reanudar, también sin conexión (39); **No** declara W.O. (26), **No** corrige (27), **No** resuelve desempates (29); cerrar desde `suspendido` (41): no habilitada; rige RT-05 | No | No |
| CF-08 | **Verificación de duplicados** (38) | Sí\* sujeto a RT-14 (A-5 **[ABIERTO]**) | Sí\* sus torneos, sujeto a RT-14 (A-5 **[ABIERTO]**) | No | No | No |
| CF-09 | **Lectura interna** (8, 9, 31, 32, 33) | Todo: usuarios y roles (8), historial (9), torneo o jornada no publicados (31), editor asignado de un partido (32), cola y conflictos (33) | Lo de **sus** torneos (8, 9, 31, 32, 33) | Fila 31: solo sus partidos asignados; fila 32: solo el propio; fila 33: solo la propia cola; filas 8 y 9: **No** (solo su cola) | Fila 31: solo su equipo y sus partidos; filas 8, 9, 32 y 33: **No** | No (filas 8, 9, 31, 32, 33) |
| CF-10 | **Lectura pública** (30) | Sí | Sí | Sí | Sí | **Sí** (torneos publicados, tablas, calendarios publicados, resultados confirmados, goleadores y tarjetas) |
| CF-11 | **Vínculo de identidad entre inscripciones** (36) | **No existe en el MVP** (RF-EQP-06 diferido); regla futura: solo el dueño | No | No | No | No |

**Tabla de cobertura: cada fila de `01 §7.1` y su familia.** «Abierta» marca las filas cuyo origen en `01` es `COND` o `PEND` (al menos para un rol):

| Fila | Acción | Familia | Abierta en `01` |
|---|---|---|---|
| 1 | Invitar o designar organizador | CF-01 | No |
| 2 | Invitar editor | CF-01 | No |
| 3 | Invitar delegado | CF-01 | No |
| 4 | Otorgar rol de dueño | CF-01 | No |
| 5 | Revocar organizador | CF-01 | No |
| 6 | Revocar editor o delegado | CF-01 | No |
| 7 | Asignar, desasignar y reasignar editor | CF-02 | No |
| 8 | Ver usuarios y roles | CF-09 | No |
| 9 | Ver historial de cambios | CF-09 | No |
| 10 | Crear torneo | CF-03 | No |
| 11 | Editar torneo y reglamento antes de iniciar | CF-03 | No |
| 12 | Cambiar reglamento de torneo iniciado | CF-03 | Sí (Q-23) |
| 13 | Publicar o despublicar | CF-03 | No |
| 14 | Finalizar torneo | CF-03 | No |
| 15 | Eliminar torneo en `borrador` | CF-03 | No |
| 16 | Inscribir o retirar equipo | CF-04 | No |
| 17 | Editar datos de la inscripción | CF-04 | No |
| 18 | Ver plantilla con datos internos | CF-05 | No |
| 19 | Alta, baja y edición de jugadores | CF-05 | No |
| 20 | Autorización o consentimiento | CF-05 | No |
| 21 | Anonimizar o eliminar un jugador | CF-05 | Sí (Q-42) |
| 22 | Generar o regenerar calendario | CF-06 | No |
| 23 | Ajustar fecha, hora, sede, localía o jornada | CF-06 | No |
| 24 | Registrar marcador y eventos | CF-07 | Sí (Q-42) |
| 25 | Cambiar estado del partido | CF-07 | Sí (Q-42) |
| 26 | Declarar o revertir W.O. | CF-07 | Sí (Q-21) |
| 27 | Corregir o reabrir un partido finalizado | CF-07 | No |
| 28 | Anular un evento confirmado | CF-07 | No |
| 29 | Resolución manual de un desempate | CF-07 | Sí (Q-22) |
| 30 | Ver lo publicado | CF-10 | No |
| 31 | Ver torneo o jornada no publicados | CF-09 | No |
| 32 | Ver quién es el editor asignado | CF-09 | No |
| 33 | Ver cola de operaciones y conflictos | CF-09 | No |
| 34 | Exportar datos personales de un jugador | CF-05 | Sí (Q-42) |
| 35 | Revocar o degradar a un dueño | CF-01 | No |
| 36 | Vincular o desvincular inscripciones | CF-11 | No (diferido) |
| 37 | Archivar un torneo | CF-03 | Sí (Q-40) |
| 38 | Resolver verificación de duplicados | CF-08 | No |
| 39 | Reanudar un partido `suspendido` | CF-07 | No |
| 40 | Levantar la restricción histórica (RT-14) | CF-01 | No |
| 41 | Cerrar un partido desde `suspendido` | CF-07 | Sí (Q-29) |

**Observaciones.**

- **La lectura de datos internos (CF-09, CF-05) y la revocación.** `01` dice que la revocación se aplica «de inmediato en el servidor: toda nueva operación sin autorización vigente se rechaza» y no distingue las lecturas de datos internos (PRV-09). R2-A propone leerlo como incluidas las lecturas [REC] (H2A-09, decisión 5 de R2-A, **abierta**). R2-B1 **soporta ambas lecturas** (H2B1-05).
- **Una pregunta abierta no es un permiso provisional.** Las celdas **[ABIERTO]** se soportan como parámetros; ninguna formulación puede tomar la columna «provisional» de una fila `COND` como un permiso concedido.
- **Nada en esta sección es una matriz de la Fase 4.**
- **La bitácora de seguridad no es una fila de `01 §7.1`** (C-05). RF-AUD-05 (prioridad S, estado REC) la sitúa «para el dueño» y no visible a organizadores, e IN-09-08 de R2-A (PROPUESTA) la recoge; R2-B1 **no la incorpora a ninguna familia ni concede su lectura**: sigue siendo una propuesta [REC] fuera de las 41 filas.

**Roles acumulados.** Las **ocho filas de `01 §7.4`** (en su orden) y **dos casos adicionales** que `01` exige probar (no son filas de §7.4); todas se evalúan con IN-02-07:

| Combinación (`01 §7.4`) | Qué evalúa el modelo | Invariantes | Casos |
|---|---|---|---|
| 1. Organizador + Editor (mismo torneo) | Unión de permisos; el rol adicional lo concede otro actor (RT-04); la asignación a partidos sigue RF-ACC-06 y RT-05 | IN-02-07, IN-02-08, IN-02-22 | CL-50 |
| 2. Editor + Delegado (mismo torneo) | Como delegado no actúa de editor en partidos de **su** equipo (al asignar, al otorgar el rol y al sincronizar) | IN-02-11 | CL-35, CL-49 |
| 3. Organizador + Delegado (mismo torneo) | No ejecuta acciones de RT-14 sobre su equipo; la restricción **persiste** tras perder el rol salvo levantamiento | IN-02-12, IN-02-13 | CL-48, CL-59, CL-60 |
| 4. Dueño + Delegado o Editor | El dueño es global **pero no está exento** de RT-14 como delegado; como editor, solo RT-05 | IN-02-12, IN-02-11 | CL-44, CL-51 |
| 5. Delegado de dos equipos del mismo torneo | **No permitido;** sí de equipos de torneos distintos | IN-02-21 | CL-54 |
| 6. Editor que fue delegado del equipo | **Sin persistencia histórica:** solo RT-05 (la persistencia de RT-14 no se extiende a editores) | IN-02-11, IN-02-12 | — (la regla es RT-14 y `01 §7.4`; CL-35 solo cubre RT-05) |
| 7. Roles en torneos distintos | Los permisos no se transfieren | IN-02-04, IN-02-05 | CL-21, CL-40 |
| 8. Dueño + segundo dueño | Mismos privilegios; protección del último dueño; sin jerarquía | IN-02-10 | CL-43 |

| Caso adicional | Qué evalúa el modelo | Invariantes | Casos |
|---|---|---|---|
| A1. Organizador + Editor + Delegado (`01 §7.3`, actor 12) | Ningún rol evade RT-14 ni RT-05: el rol de editor asignado no habilita sobre el propio equipo lo que RT-14 niega al organizador | IN-02-07, IN-02-11, IN-02-12 | CL-50 |
| A2. Revocación parcial entre organizadores (RF-ACC-07, Q-38 d) | Solo roles adicionales de editor o delegado y solo en torneos comunes; conserva los permisos de otros roles vigentes | IN-02-07, IN-02-09 | CL-52, CL-53 |

### 17.5 Aislamiento entre torneos, acumulación de roles y separación de facultades

Las reglas siguientes **no añaden permisos**: concretan, como capacidades exigidas, lo que `01` ya impone. **«Ambas capas»** significa que se verifican en el servidor y en la capa de datos (IN-02-01).

| Situación | Requisito de capacidad | Invariantes | Casos de prueba de `01` |
|---|---|---|---|
| Una solicitud bien formada de un organizador del torneo A contra un recurso del torneo B | Denegada en ambas capas; la respuesta no revela la existencia del recurso [REC] | IN-02-04, IN-02-27 | CL-21, CL-40 |
| Un partido de A con un equipo de B; un evento con un jugador de B; una asignación de editor de B | Rechazados por la capa de datos aunque el servidor los admitiera | IN-02-04 | CL-21 |
| Un editor asignado al partido P intenta capturar en un partido no asignado; un editor de otro torneo escribe | Denegado | IN-02-06, IN-02-23 | CL-20 |
| Un organizador intenta invitar a otro organizador, otorgar el rol de dueño o ampliar el ámbito de una invitación | Denegado en ambas capas; evento de seguridad [REC] | IN-02-08, IN-02-09, IN-02-28 | CL-19, CL-38 |
| Un usuario se invita o se asigna a sí mismo; el dueño se concede un rol | Denegado, salvo la asignación inicial del creador de un torneo | IN-02-08 | CL-20 |
| Un usuario con dos roles intenta, con uno, lo que el otro le deniega | Denegado: la unión no concede más | IN-02-07 | CL-50, CL-53 |
| Dos invitaciones simultáneas harían delegado de dos equipos del mismo torneo a la misma persona | Una prospera y la otra se rechaza | IN-02-21 | CL-54 |
| Dos reasignaciones simultáneas del mismo partido | Una prevalece; nunca hay dos editores vigentes | IN-02-22 | CL-41 |
| Consultas con el vínculo de identidad de equipo (diferido) | 0 filas de otros torneos; el vínculo no es criterio de autorización | IN-02-05 | CL-40, CL-42 (diferido) |
| Lectura de datos internos de otro torneo por un organizador (historial, plantillas, usuarios) | Denegada; el organizador solo ve sus torneos | IN-02-04 | CL-40; RF-ADM-03 |

### 17.6 Ambigüedades y condiciones abiertas

**Naturaleza de esta tabla.** Para cada ambigüedad se documentan **ambas lecturas** y cómo las soporta el modelo. **«Lectura provisional»** es la que fija `01 §13.7` (**no es una decisión del usuario**); **«Lectura alternativa»** es la que el autor considera para completar el par, con la formulación que usa R1 §9.3 [SUP]. **Ninguna se elige y todas siguen [ABIERTO].** `01` asigna su decisión a la Fase 4 (A-15, «revisable en el modelo de amenazas de F2»).

| # | Ambigüedad | Lectura provisional (`01 §13.7`) | Lectura alternativa | Cómo la soporta el modelo | Reservado a |
|---|---|---|---|---|---|
| A-1 | Alcance de «resultados» en RT-14 | Toda operación que cree o modifique datos de resultado de partidos del propio equipo, con denegación por defecto | Solo el registro y la confirmación del marcador y del estado final del partido propio; el resto (eventos, correcciones, W.O.) se rige por sus propias cláusulas de RT-14 | **Clases de operación como parámetro del evaluador** (§9.3) | Usuario (Fase 4); efecto en el motor: ADR-08 |
| A-2 | «Asignaciones que generen conflictos» | Solo la asignación de editores a partidos del propio equipo | Toda asignación (editor, delegado u otra) cuyo efecto alcance al propio equipo | **Alcance de la asignación como parámetro** | Usuario (Fase 4) |
| A-4 | «Inscripción o retiro» | Solo las decisiones de inscripción o retiro, no la edición de nombre y colores | También la edición de nombre y colores de la inscripción del propio equipo | **Distinguir decisión y edición** como dos clases de operación con regla propia (CF-04) | Usuario (Fase 4) |
| A-5 | Verificación de duplicados: ¿a qué lado del par? | Cualquiera de los dos registros del par (denegación por defecto) | Solo el registro nuevo que se verifica | **Aplicar a uno o a ambos registros** como parámetro | Usuario (Fase 4) |
| A-6 | «Propio equipo» sin vínculo de identidad | Rige **dentro del mismo torneo**; sin identidad común no cruza torneos | Extensible a las inscripciones vinculadas en otros torneos si se construyera el vínculo. **En conflicto con reglas confirmadas:** RT-12 («no es un criterio de autorización: ninguna política de acceso, vista ni consulta lo utiliza ni concede acceso a otro torneo») y RF-EQP-06 («el vínculo no se usará en ninguna política de acceso, vista, plantilla, estadística ni permiso»). Una restricción es una política de acceso: **la lectura solo podría activarse si el usuario modificara formalmente esas reglas**, y R2-B1 no lo propone | **Alcance dentro del torneo; extensible** sin cambiar el evaluador (R1 §9.3 pide que el diseño no lo impida). **R2-B1 no usa el vínculo como criterio** | Usuario; solo si se construyera RF-EQP-06 y se modificaran RT-12 y RF-EQP-06 |
| A-8 | Publicación y despublicación de jornadas | **No restringida** por RT-14 | Con un **punto opcional de guardia** al habilitar la visibilidad de jornadas del propio equipo | **Punto opcional de guardia** parametrizable; la visibilidad misma es de ADR-04.C | Usuario (Fase 4); **ADR-04.C (R2-C)** |
| A-12 | «Sin conflicto» del aprobador del levantamiento | Sin conflicto respecto del equipo y torneo del levantamiento; no cubre el conflicto vigente; se anula si se otorga de nuevo el rol | Condiciones adicionales del aprobador (por ejemplo, sin rol de delegado en ningún equipo del torneo, o reautenticación reciente) | **Condiciones del aprobador como parámetro**; IN-02-12 fija el mínimo de `01` | Usuario (Fase 4) |
| A-15 | RT-14 frente a revocaciones de seguridad, desasignación y efectos indirectos | **Sin resolver.** Provisional: no se altera el permiso de revocar; asignar y reasignar sobre partidos propios siguen denegados; los efectos indirectos se auditan | (i) La revocación inmediata se ejecuta **siempre**, aunque su efecto alcance partidos propios; (ii) la revocación o desasignación **queda sujeta a RT-14** y se bloquea si alcanza partidos propios | **Que la revocación inmediata se pueda ejecutar siempre, o quedar sujeta a RT-14**, y auditoría de efectos indirectos (IN-09-13 [REC]). Riesgo de (ii): RB1-12 | Usuario (Fase 4, revisable en la Fase 2); **R2-B2** (frontera) y **ADR-05.C** (R1 §9.3 lista ADR-02 y ADR-05) |
| A-16 | Reglamento y desempates bajo RT-14 | **Sin resolver.** Provisional: no se añade ni se retira restricción; los cambios de reglamento se auditan; la resolución manual de desempates sí es de RT-14 | Guardia en la edición del reglamento y de los criterios cuando el cambio afecte al propio equipo | **Guardia opcional** parametrizable | Usuario; **ADR-08 (R3)** |

**Preguntas abiertas que condicionan capacidades** (C-06). Se separan **dos clases**; en ambas las opciones son las de `01`, **no se adopta ninguna**, las recomendaciones de `01` no son decisiones y **ninguna se convierte en permiso provisional**.

*Clase 1: preguntas sobre facultades (quién puede hacer algo que `01` ya define).*

| Pregunta | Qué soporta el modelo | Celdas afectadas |
|---|---|---|
| **Q-40 (a)** ¿Quién archiva? (A: solo dueño; B: dueño y organizador) | La facultad de archivar como parámetro de rol | CF-03 (fila 37) |
| **Q-40 (b)** Reabrir `finalizado`; desarchivar `archivado` (A: solo dueño con motivo y auditoría; B: dueño y organizador; C: no se permite; desarchivar: A no se permite, B solo dueño) | Reapertura como parámetro: ninguna, solo dueño, o dueño y organizador, siempre con motivo y auditoría | CF-03, CF-07 |
| **Q-42 (a)** Facultades de captura y de cambio de estado del organizador (A: sí; B: no) | La facultad del organizador sobre CF-07 como parámetro | CF-07 (filas 24, 25) |
| **Q-42 (b)** Anonimizar y exportar datos de jugadores (A: solo el dueño; B: también el organizador de su torneo) | La facultad del organizador sobre CF-05 como parámetro | CF-05 (filas 21, 34) |

*Clase 2: preguntas sobre reglas deportivas o estados (qué operación existe o cómo funciona).* Mientras estén abiertas, **la fila queda `COND` en `01`** y R2-B1 **no decide quién la ejecuta**: su respuesta puede crear, modificar o eliminar la operación misma. **Q-22 y Q-23, además, preguntan expresamente por «quién»** (Q-22: «¿quién, con qué justificación y cómo se publica?»; Q-23: «¿cuáles, quién aprueba y se recalcula el histórico?»), de modo que en ellas la regla **y** la facultad están abiertas. El modelo **solo reserva la fila**; no adopta ni un «Sí» ni un «No».

| Pregunta | Qué está abierto | Efecto sobre el modelo | Filas afectadas |
|---|---|---|---|
| **Q-40 (c)** | Qué operaciones se bloquean en `archivado` (A: solo lectura para todos salvo las correcciones del dueño; B: solo se restringen las correcciones del organizador) | El conjunto de operaciones bloqueadas por estado como parámetro (IN-02-20) | CF-03 a CF-08 |
| **Q-21** W.O. doble, sanciones y GF/GC | Las reglas del W.O. | La fila 26 queda abierta entera | CF-07 (fila 26) |
| **Q-22** Resolución manual de desempates | La regla (cómo se publica) **y la facultad** (¿quién y con qué justificación?) | La fila 29 queda abierta entera, en la regla y en la facultad; ni «Sí» ni «No» | CF-07 (fila 29) |
| **Q-23** Cambios de reglamento con torneo iniciado | La regla (¿cuáles?, ¿se recalcula el histórico?) **y la facultad** (¿quién aprueba?) | La fila 12 queda abierta entera, en la regla y en la facultad (también el «Sí con confirmación» del dueño); ni «Sí» ni «No» | CF-03 (fila 12) |
| **Q-29** Estados `aplazado`, `cancelado`, `suspendido` y su efecto en la tabla | Los estados y el cierre desde `suspendido` | La fila 41 sigue «no habilitada»; R2-B1 no la habilita | CF-07 (fila 41) |

**Q-40 (d)** (visibilidad de `archivado`) **no pertenece a R2-B1**: es de ADR-04.C y debe resolverse antes de la Fase 3 (`01 §13.5`).

### 17.7 Dependencias y límites de responsabilidad

**Relaciones de §8.2 que usa ADR-02.C** (no se añade ninguna): requiere **ADR-03.C** y **ADR-09.C**; **no requiere** ni espera a ninguna parte `.I`, ni a ADR-01, ADR-04.C o ADR-05.C.

| Artefacto | Qué consume o qué le deja ADR-02.C (parte 1) | Qué **no** hace R2-B1 |
|---|---|---|
| **ADR-03.C** (PROPUESTA) | Consume el nivel de autenticación (IN-02-15, IN-02-25); IN-03-17 impide que ADR-03.C conceda facultades. IN-03-04 (nueva autenticación y motivo en operaciones sobre dueños) y IN-03-05 (invitación) son de ADR-03.C. Depende de las decisiones de R2-A **1, 5 y 12** | No elige entre CAP-03-N-A, N-B, N-C ni entre S-A y S-B; no fija PAR-01…06; no resuelve D-2 |
| **ADR-09.C** (PROPUESTA) | Consume IN-09-04, IN-09-05 e IN-09-06 (evento y su contenido) y la disciplina de CAP-09-P-A. Depende de la decisión **10** de R2-A | No elige entre CAP-09-P-A, P-B y P-C; no define el fallo del registro ni su relación con OFF-06 |
| **ADR-05.C** (R2-C) | **Solo se registra la relación:** ADR-05.C requiere ADR-02.C y aplica la misma decisión a la ruta sincronizada (OFF-22, IN-02-03, IN-02-11, IN-02-12); A-15 figura también en ADR-05 | No desarrolla la sincronización sin conexión, A-9…A-14 ni la frontera de UI-06 |
| **ADR-04.C** (R2-C) | Le deja A-8 como punto de guardia y las lecturas internas de CF-05 y CF-09 | No decide visibilidad, proyecciones ni Q-40 (d) |
| **ADR-08** (R3) | Le deja A-16 como guardia opcional | No define el motor de reglas |
| **R2-B2** | Recibe UI-01…13, la frontera atómica (IN-02-03, IN-02-10, IN-02-22), la auditoría de rechazos en la frontera, K-2d de CAP-02-D (y la lista de operaciones de H2B1-10) y las amenazas asignadas a R2-B | — |
| **R2-C, R3, R4, Fase 4** | La consolidación del modelo de amenazas (R2-C); ADR-02.I y los mecanismos (R3); la identidad de ejecución de las tareas automáticas (ADR-14, R4); la matriz detallada y las políticas de acceso (Fase 4) | — |

**Compatibilidad condicionada con las formulaciones de ADR-03.C y ADR-09.C.** Ninguna formulación pendiente se convierte en decisión; estas son las **condiciones que CAP-02-A y CAP-02-D** (las dos no descartadas por la matriz) imponen a las de R2-A si esas fueran las elegidas:

| Si el usuario elige… | Condición para ADR-02.C | Efecto |
|---|---|---|
| **CAP-03-N-B** (nivel con frescura) | La capa de datos debe recibir de forma verificable el instante de verificación del segundo factor para las operaciones de IN-03-04 (IN-02-25) | Condiciona K-5 y H2B1-02 |
| **CAP-03-N-C** (comprobante de un solo uso) | El comprobante se consume **en la misma unidad** que aplica la operación (IN-02-03) | Condiciona K-5 y R2-B2 |
| **CAP-03-N-A** | Ya es no conforme por sí sola en R2-A; **no se evalúa aquí como viable** | — |
| **CAP-03-S-A o S-B** (sincronizar) | La decisión completa se evalúa **al sincronizar** con el estado vigente (OFF-22); S-B añade la reverificación previa | Sin conflicto con IN-02-11 e IN-02-12 |
| **CAP-09-P-A** | La capa de datos debe poder **responder un rechazo solo después** de que otra unidad lo haya registrado | Disciplina sobre la frontera: R2-B2 |
| **CAP-09-P-B** | Cubre solo los rechazos que decide el evaluador; **no cubre** los impuestos por la capa de datos (RC-04), que existen en A, C y D | **K-3** de §17.3 |
| **CAP-09-P-C** | Conforme solo si la intención de registro es **durable antes de responder** | Condiciona R2-B2 y K-3 |

### 17.8 Condiciones abiertas, hallazgos y decisiones de R2-B1

**Condiciones y hallazgos** (ninguno se resuelve en R2-B1):

| ID | Condición o hallazgo | Estado |
|---|---|---|
| H2B1-01 | **Selección de formulación de D2-1:** CAP-02-A conforme con K-1, K-3 y K-5; CAP-02-D condicionada a K-2 y con su independencia respecto de A **no demostrada**; CAP-02-B y CAP-02-C no conformes (§17.3.1) | **[ABIERTO]**, decisión del usuario (se relaciona con la decisión 6 de R2-A) |
| H2B1-02 | **Viabilidad de IN-02-25 con el stack:** que la capa de datos reciba identidad y nivel de forma verificable y lea roles e historial sin una credencial que evada la decisión (IN-02-29). Qué mecanismo lo logra no se decide aquí. **Aparte, la conformidad de IN-02-15 e IN-02-25 depende de la formulación de ADR-03.C que se adopte (K-5)** | **[ABIERTO]** condicionado a ADR-03.I, ADR-02.I y ADR-01 (R3), y a las decisiones 1, 5 y 12 de R2-A |
| H2B1-03 | **K-1: una sola definición ejecutada en dos capas:** riesgo de deriva si la lógica se escribe dos veces (IN-02-02) | **[ABIERTO]**; propiedad exigida a ADR-02.I |
| H2B1-04 | **RT-14 y RT-02.** `01 RT-14` dice «Se aplica en el servidor, en la misma transacción (RT-13) y al sincronizar»; **no dice «solo» en el servidor**, y esa transacción es la que aplica la operación. RT-02 y RC-04 exigen servidor **y** capa de datos, y CL-48 pide «Denegado en servidor y BD». **R2-B1 no interpreta RT-14 como «solo servidor»:** una formulación que lo hiciera elimina una protección aprobada y es **No conforme** (CAP-02-B y CAP-02-C lo son por esa causa). **No hay una tensión que el usuario deba resolver;** sustituir RT-02 o RC-04 exigiría modificarlos formalmente, y R2-B1 no lo propone | **Sin decisión pendiente** (registrado como observación) |
| H2B1-05 | **Alcance de las lecturas de datos internos tras revocar** (decisión 5 de R2-A, H2A-09): determina si CF-05 y CF-09 se revocan con la sesión | **[ABIERTO]**; el modelo soporta ambas lecturas |
| H2B1-06 | **A-6 frente a RT-12 y RF-EQP-06:** la lectura alternativa usaría el vínculo de identidad, que ambas reglas (confirmadas) excluyen de toda política de acceso (§17.6). No se modifica ninguna de las dos | **[ABIERTO]**; irrelevante en el MVP (vínculo diferido) |
| H2B1-07 | **Operaciones automáticas:** heredan la situación del actor que las dispara (RT-14); la identidad de ejecución de las tareas programadas es de ADR-14 (R4, IN-09-09 [REC]) | **[ABIERTO]** (R4) |
| H2B1-08 | **Facultades:** Q-40 (a) y (b) y Q-42; **reglas deportivas o estados:** Q-40 (c), Q-21, Q-22, Q-23 y Q-29 (Q-22 y Q-23, además, preguntan por quién). Todas siguen abiertas en `01`; `01` las asigna a la Fase 4 (Q-40 (d) antes de la Fase 3). Condicionan la matriz de la Fase 4 (§17.6) | **[ABIERTO]** |
| H2B1-09 | **Propuestas [REC] IN-02-27, 28 y 29** no tienen aprobación | **[ABIERTO]** |
| H2B1-10 | **CAP-02-D: responsabilidades de CMP-07 y CMP-08 (§17.3.5).** R1 asigna toda decisión de permiso a CMP-08 (§5.3, CR1-13), hace de CMP-07 la fuente única de las asignaciones y su historial y dice que CMP-08 los lee y no los almacena; **no menciona proyecciones derivadas de permisos.** R1 §5.5 dice que lo transaccional incluye «si existieran, sus proyecciones derivadas persistentes (que existan o no lo decide ADR-04)» y que «la lista no es exhaustiva», pero **ninguno de los dos pasajes trata de proyecciones de permisos**. Dos interpretaciones son compatibles con R1 (P-1: CMP-07 mantiene la proyección con la definición única de CMP-08; P-2: CMP-08 la conserva) y una no (P-3: reglas propias de CMP-07, segundo decisor); **no se presupone que modificar R1 sea obligatorio.** **Operaciones que actualizarían la proyección (K-2c):** las que modifican **permisos**: UI-01, 02, 03, 04, 08, 11 y 12 de §5.5 y las que §5.5 no enumera (aceptar invitaciones de editor u organizador, designar organizadores, desactivar cuentas y asignar torneos existentes a un organizador). **Las que modifican estados no actualizan la proyección,** porque el estado se lee de su fuente; incorporar el estado a la proyección sería una **variante no evaluada** que sumaría el ciclo de vida del torneo (RF-TOR-02), los estados del partido (RD-RES) y las unidades UI-05, UI-06 (también por la ruta sincronizada) y UI-10. Si el usuario no confirmara la lectura, D sería No conforme con IN-02-02 e IN-02-13 | **[ABIERTO]**, decisión del usuario |
| H2B1-11 | **Déficit frente a R1 §8.1** («≥ 2 formas de reparto con ambas capas siempre presentes»): hoy solo CAP-02-A está evaluada sin celdas «No conforme» y sin K-2; la independencia de D respecto de A tiene dos interpretaciones y **no está demostrada** (§17.3.3). **Dependencia documentada de R2-B2 y decisión del usuario; sin excepción concedida** | **[ABIERTO]** |

**Decisiones que corresponden al usuario en R2-B1** (**no alteran ni resuelven las decisiones de R2-A: de las 13 originales, la 11 está atendida y las otras 12 siguen abiertas**):

| # | Decisión | Opciones | Dónde |
|---|---|---|---|
| DB1-1 | **Déficit frente a R1 §8.1** (H2B1-11) | (a) **mantenerlo abierto como dependencia documentada de R2-B2:** CAP-02-D se conserva como candidata condicionada a K-2 y **no se cuenta como segunda forma** hasta que se demuestren su independencia respecto de A y K-2; (b) decidir que D **cuenta** como segunda forma (interpretación I de §17.3.3) y condicionar ADR-02.C a K-2; (c) una **decisión formal del usuario** sobre el déficit (excepción o modificación de R1): es una vía que le corresponde a él; **este documento no la propone, no está aprobada y no se ha tomado ninguna decisión en ese sentido**; (d) pedir una formulación adicional | §17.3.3 |
| DB1-2 | **Formulación de D2-1** (H2B1-01) | CAP-02-A; CAP-02-D (con K-2 y H2B1-10); CAP-02-B y CAP-02-C no son viables. Se relaciona con la decisión 6 de R2-A | §17.3 |
| DB1-3 | **Lecturas de A-1, A-2, A-4, A-5, A-6, A-8, A-12, A-15 y A-16** | Provisional de `01` o alternativa de §17.6 (`01` las decide en la Fase 4; A-15, revisable en la Fase 2) | §17.6 |
| DB1-4 | **Preguntas de `01`:** facultades (Q-40 (a) y (b), Q-42) y reglas deportivas o estados (Q-40 (c), Q-21, Q-22, Q-23 y Q-29; Q-22 y Q-23, además, preguntan por quién) | Las opciones de `01 §13` | §17.6 |
| DB1-5 | **Adoptar o descartar IN-02-27, 28 y 29** [REC] | Adoptar, adoptar parcialmente o descartar | §17.2 |
| DB1-6 | **Confirmación de la lectura de R1 §5.3 para CAP-02-D** (H2B1-10, §17.3.5), solo si se elige D | (a) confirmar que P-1 o P-2 son lecturas admisibles de R1 (**sin modificar R1**); (b) autorizar una precisión editorial de R1; (c) no admitir D | §17.8 |

### 17.9 Riesgos de diseño identificados

**R2-B1 no puntúa riesgos ni modifica §16.3.** Los riesgos de **diseño** siguientes se detectaron al elaborar la parte 1 [SUP]; **ninguno se puntúa ni se acepta**; **R2-B2** los evaluará con la convención de §16.1:

| ID | Riesgo | Relación | Tratamiento |
|---|---|---|---|
| RB1-01 | **Deriva entre las dos ejecuciones de la decisión** (A, B, C, D): una capa permite lo que la otra niega | IN-02-02; AM-01, AM-15 | K-1; H2B1-03 |
| RB1-02 | **La capa de datos no puede confiar en la identidad que recibe** y la decisión se vuelve un trámite | IN-02-25, IN-03-11; AM-09 (de ADR-03.C: el contexto local no autentica) | H2B1-02 |
| RB1-03 | **Una credencial de servicio con privilegios totales hace de la capa de datos una capa sin control** | IN-02-29 [REC]; AM-23 | R3 (mecanismo); R4 (operación) |
| RB1-04 | **Un piso incompleto en CAP-02-C** deja un permiso en una sola capa sin que nadie lo note | IN-02-01 | La matriz lo declara «No conforme» |
| RB1-05 | **Bloqueo operativo por RT-14 sin actor alterno** (dueño único con rol de delegado; torneo `archivado`) deja operaciones sin ejecutar | CL-51, RT-06; AM-32 | `01` lo define como bloqueo sin excepciones; R4 para la sucesión; no se acepta |
| RB1-06 | **Lecturas de datos internos tras la revocación** si IN-03-06 se limita a escrituras: un delegado revocado vería datos de menores hasta que expire su sesión | H2A-09, decisión 5 de R2-A | H2B1-05 |
| RB1-07 | **Sobrecarga de parámetros:** soportar todas las lecturas y preguntas abiertas aumenta la complejidad y la superficie de pruebas | IN-02-17 | La Fase 4 debe acotar los parámetros que queden abiertos |
| RB1-08 | **Operaciones automáticas que heredan al actor** pueden ejecutar un cambio que el actor no podría hacer a mano si la herencia se pierde | RT-14, H2B1-07 | R4 (identidad de ejecución) |
| RB1-09 | **Colusión o segunda cuenta de dueño:** el levantamiento de RT-14 solo exige «otro dueño activo… distinto del interesado» y sin conflicto; dos cuentas de dueño controladas por la misma persona, o la colusión entre dos dueños, anularían la restricción | IN-02-12; RF-ACC-17, A-12 | A-12 como parámetro (condiciones adicionales del aprobador) |
| RB1-10 | **Elusión de RT-14:** por otro rol (CL-50), por otra cuenta, por fragmentar una operación para que cada parte no alcance partidos propios, o por una operación automática que pierde la herencia del actor | IN-02-07, IN-02-12; CL-50, CL-57 | IN-02-12 evalúa **el efecto de la operación completa**; R2-B2 (frontera) |
| RB1-11 | **Anonimización o conservación del historial:** la anonimización o la desactivación de cuentas puede romper la persistencia de RT-14 y permitir su evasión (la validación jurídica de la conservación del historial sigue pendiente) | IN-02-13; RF-ACC-11 (PEND-JUR) | `01` conserva el vínculo técnico «mientras exista el torneo»; PEND-JUR |
| RB1-12 | **Bloqueo de revocaciones de seguridad:** con la lectura (ii) de A-15, una revocación de seguridad bloqueada por RT-14 retrasa la contención de una cuenta comprometida | A-15; IN-02-12 | A-15 como parámetro; decisión del usuario (DB1-3) |
| RB1-13 | **Divergencia de la proyección** de CAP-02-D respecto del historial: un permiso erróneo se aplicaría igual en ambas capas | IN-02-02, IN-02-13; K-2 | H2B1-10; R2-B2 |

### 17.10 Trazabilidad

**Amenazas asignadas a R2-B en §16.5: cuáles trata realmente ADR-02.C y cuáles son contexto.**

| Amenaza | Tipo de relación | Por qué |
|---|---|---|
| AM-01 (manipular rol, torneo o usuario en la solicitud: intento) | **Tratada por IN-02** (01, 04, 06, 08) | El control es la autorización en ambas capas |
| AM-15 (acceso a datos de otro torneo: resultado) | **Tratada por IN-02** (01, 04, 05, 06) | El control es el aislamiento en la capa de datos |
| AM-18 (carrera entre revocación u operación) | **Tratada por IN-02 en lo conceptual** (03, 10, 22); la frontera es de **R2-B2** | El control es la autorización en la misma unidad |
| AM-19 (inyección en la capa de datos) | **Contexto:** la asigna §16.5 a R2-B, pero su control (validación de entradas) no es un permiso; R2-B1 no le atribuye invariante | R2-B2 y R2-C decidirán su tratamiento |
| AM-34 (inyección de contenido en vistas públicas) | **Contexto:** ídem; su control es la codificación de salidas | Comparte responsable con R3 y la Fase 4 |
| OM-01 (abuso con privilegios legítimos por un dueño: anonimización o exportación masivas, revocaciones) | **Contexto:** el actor **tiene** el permiso; la autorización no lo impide [SUP] | R2-B2 y R4; auditoría (ADR-09.C) |
| OM-07 (captura falsa o en colusión por un editor u organizador legítimo) | **Contexto:** ídem; RT-05 y RT-14 limitan **dónde** actúa, no si actúa con mala fe [SUP] | R2-B2 y R4 |

**Invariantes → fuente → casos de prueba de `01`:**

| Invariante | Fuente principal | Casos de prueba de `01` |
|---|---|---|
| IN-02-01, 02, 25 | RT-02, RC-04, CR1-13 | CL-48 |
| IN-02-03 | RT-13 | CL-41, CL-45, CL-47 |
| IN-02-04, 05 | RT-01, RT-08, RT-12 | CL-21, CL-40, CL-42 (diferido) |
| IN-02-06, 07 | RT-01, RT-11 | CL-50, CL-53 |
| IN-02-08, 09 | RT-03, 04, 06, 07; RF-ACC-03, 15 | CL-19, CL-20, CL-38 |
| IN-02-10 | RT-06, RF-ACC-09 | CL-43 |
| IN-02-11 | RT-05 | CL-35, CL-49 |
| IN-02-12, 13 | RT-14, RF-ACC-11, RF-ACC-17 | CL-48…51, CL-56…60 |
| IN-02-14 | RT-01, PA-13 | CL-20, CL-21 |
| IN-02-15 | RC-11, RF-ACC-10 | CL-38, CL-55 |
| IN-02-16 | RT-10, RF-ACC-11, RF-AUD-06 | CL-48, CL-49, CL-51, CL-59, CL-60 |
| IN-02-17, 18, 19 | R1 §9.3, PA-13, CR1-13, RC-23 | — |
| IN-02-20 | RF-TOR-02, D-5 | CL-34, CL-44 |
| IN-02-21 | RF-ACC-13 | CL-54 |
| IN-02-22 | RF-ACC-06 | CL-41, CL-47 |
| IN-02-23 | RF-ACC-05, 06, 13 | CL-20 |
| IN-02-24 | RF-ACC-01 | — (la aceptación de RF-ACC-01 es «DB, E2E») |
| IN-02-26 | RF-ACC-12 | — (aceptación de RF-ACC-12: «DB, I»; fila 10 de `01 §7.1`) |

### 17.11 Criterios de aceptación de R2-B1

Verificables sobre este documento. **La aprobación de R2-B1 es un acto expreso del usuario y no aprueba R2-B en su conjunto, R2 ni la Fase 2. R2-B1 fue aprobado condicionalmente como entregable documental el 2026-11-18 (§17.0).** Las comprobaciones de §17.12 son **del autor** y no sustituyen una revisión independiente.

| ID | Criterio | Cómo se verifica |
|---|---|---|
| CR2B1-01 | **La matriz de §17.3 está completa y es uniforme:** 26 invariantes × 4 formulaciones = 104 celdas, cada una con uno de los tres rótulos; **ninguna celda con condición escrita se rotula «Conforme»**; los recuentos de §17.3.1 coinciden con las celdas | §17.12: recuento recalculado de las celdas frente a §17.3.1 |
| CR2B1-02 | **Cobertura de la matriz de permisos:** las 41 filas de `01 §7.1` están asignadas a exactamente una familia, y toda fila con origen `COND` o `PEND` figura [ABIERTO] en su familia | §17.12: coincidencia exacta de la tabla de cobertura con las 41 filas y comprobación de las filas abiertas |
| CR2B1-03 | **Doble capa:** toda formulación en la que la capa de datos omita una verificación exigida figura como «No conforme» y no como condicionada o viable | §17.3: B y C |
| CR2B1-04 | **Ningún permiso nuevo:** cada celda de §17.4 coincide con la fila de `01 §7.1` que cita | **Comparación manual del autor, no mecánica** (§17.12); requiere revisión independiente |
| CR2B1-05 | **Independencia de las alternativas:** la equivalencia C ≡ A + versión, las dos interpretaciones sobre D y el déficit frente a R1 §8.1 figuran, **sin declarar cumplida la exigencia ni conceder excepción** | §17.3.3 y DB1-1 |
| CR2B1-06 | **Ambigüedades y preguntas:** las nueve ambigüedades figuran con lectura provisional literal de `01 §13.7` y lectura alternativa y siguen abiertas; A-6 cita RT-12 y RF-EQP-06; Q-40, Q-42, Q-21, Q-22, Q-23 y Q-29 siguen abiertas sin permiso provisional | §17.6 |
| CR2B1-07 | **Etiquetas y respaldo:** cada invariante lleva **una sola** etiqueta; todo extracto de «Respaldo literal» existe en la fuente citada; los [DER] declaran su paso derivado | §17.12 (búsqueda mecánica de los extractos) |
| CR2B1-08 | **Límites respetados:** no hay especificación de unidades UI-01…13 ni frontera atómica; no se desarrollan visibilidad, sincronización ni motor de reglas | §17.0 y §17.7 |
| CR2B1-09 | **Sin selección tecnológica:** búsqueda de nombres de tecnología en la Parte III sin apariciones | §17.12 |
| CR2B1-10 | **R1 y R2-A íntegros:** las líneas 1 a 1951 del archivo de partida no cambian | §17.12 (comparación con el commit `e246508`) |
| CR2B1-11 | **Estado intacto:** no se fija ningún parámetro, no se acepta ningún riesgo, no se concede ninguna excepción ni se resuelve ninguna de las 12 decisiones de R2-A que siguen abiertas (la 11, de las 13 originales, está atendida); los riesgos de §17.9 no están puntuados | §17.8, §17.9 y §17.13 |
| CR2B1-12 | **Trazabilidad consistente:** las amenazas se distinguen entre «tratadas» y «contexto»; los identificadores citados existen | §17.10 y §17.12 |

### 17.12 Verificación del autor

**Son comprobaciones del autor, no una auditoría ni una revisión independiente.** Resultados de la ejecución mecánica sobre esta versión (v0.5):

| Comprobación | Resultado |
|---|---|
| **R1 y R2-A íntegros:** las líneas 1 a 1951 del archivo de partida son idénticas al archivo versionado en el commit `e246508` | **Sí.** El SHA-256 de ese prefijo es `2a4eca42cb2110aefc5f8268f3f4a57b504f561de01cd56ae8490b719549955d`, igual al del commit |
| **Identificadores definidos y sin referencias huérfanas** | IN-02-01…29 (29 = 26 obligatorios + 3 [REC]), H2B1-01…11, CF-01…11, RB1-01…13, CR2B1-01…12, DB1-1…6, B-01…14 y D-01…09: **todos definidos; 0 referencias sin definición**. K-1…5 definidas (K-4 retirada) |
| **Matriz de §17.3** | 26 invariantes × 4 formulaciones = **104 celdas**. Recuento recalculado de las celdas: **A** 21 conforme, 5 condicionada, 0 no conforme; **B** 6, 3 y **17**; **C** 16, 3 y **7**; **D** 6, **20** y 0. **Coincide con §17.3.1.** Condiciones por formulación: A (K-1: 1, K-3: 1, K-5: 3); B (K-1, K-3 y K-5: 1 cada una); C (K-1, K-3 y K-5: 1 cada una); D (**K-2: 18**, K-1: 1, K-3: 1, K-5: 3; las celdas con más de una condición se cuentan en cada una). 0 celdas «Conforme» con una condición escrita; 0 celdas «Condicionada» sin condición K-n. Celda revisada en esta versión: D·IN-02-20 (E-2); el texto de §17.3.3 sobre C se corrigió a 7 celdas (E-1) |
| **Etiquetas de §17.2** | 16 [CONF], 10 [DER] y 3 [REC]; **0 invariantes con doble etiqueta**; los 10 [DER] declaran su paso derivado |
| **Respaldo literal** | **37 extractos** entre comillas en la columna «Respaldo literal» de §17.2 (29 invariantes): **todos existen** en el texto de `01`, de R1 y R2-A o de la skill (búsqueda mecánica, sin formato). Los extractos de §17.3.5 y de H2B1-10 (CMP-07, CMP-08, CR1-13, AC-1, R1 §5.5, Q-22 y Q-23) también existen |
| **Cobertura de `01 §7.1`** | Las **41 filas** están asignadas a exactamente una de las 11 familias (8, 1, 7, 2, 5, 2, 8, 1, 5, 1 y 1 filas). Las **9 filas con origen `COND` o `PEND`** en `01` (12, 21, 24, 25, 26, 29, 34, 37 y 41) coinciden con las marcadas «Abierta» y su familia lleva [ABIERTO] |
| **Modelo de capacidades de §17.4** | 11 familias × 5 roles (todas las celdas escritas); **ocho** combinaciones de `01 §7.4` más **dos** casos adicionales rotulados |
| **Comparación de §17.4 con `01 §7.1`** | **Comparación manual del autor, fila por fila, no mecánica:** 0 discrepancias detectadas en la v0.2; repetida en la v0.3 para CF-07 y CF-09 y en la v0.4 para CF-01 y CF-07, que son las familias modificadas. Requiere revisión independiente (CR2B1-04) |
| **Ambigüedades de §17.6** | 9 filas (A-1, A-2, A-4, A-5, A-6, A-8, A-12, A-15 y A-16), cada una con lectura provisional y alternativa; todas **[ABIERTO]**. La fila de A-6 cita RT-12 y RF-EQP-06. Las preguntas se separan en facultades (4) y reglas deportivas o estados (5; Q-22 y Q-23, además, preguntan por quién) |
| **Nombres de tecnología** | **0 apariciones** en la Parte III de una lista amplia de nombres de proveedores, productos, frameworks, lenguajes, estándares, protocolos y siglas (la lista no se reproduce en el repositorio; el recuento no es reproducible con otra lista) |
| **Referencias a `01` y a R1/R2-A** | 73 identificadores de `01` (familias RF-ACC, RF-AUD, RF-ADM, RF-EQP, RF-TOR, RT, CL, OFF, PRV, RD-* y Q) y 38 de R1 y R2-A (familias RC, PA, HR1, CR1, IN-03, IN-09, H2A, AM, CAB, OM y CMP) citados en esta parte: **todos existen**; con otras familias de identificadores la cifra sería distinta. Las formulaciones CAP-03 y CAP-09 citadas existen en §16.6 y §16.7 |
| **Alcance de Git** | Solo `02-arquitectura-adr.md` figura modificado y el diff respecto del commit **solo añade líneas** (0 eliminadas); `aprobaciones.md`, `bitacora.md` y `01-requisitos.md` no cambian |
| **Límite de estas comprobaciones** | Son mecánicas y del mismo autor: detectan desajustes de forma, **no validan** que las evaluaciones de conformidad, los alcances de rol, las lecturas alternativas ni los pasos derivados estén bien razonados. La v0.2, la v0.3 y la v0.4 fueron validadas o verificadas por revisores de contexto nuevo (§17.15, §17.16 y §17.17); **la v0.5 no ha sido revisada por nadie distinto del autor** |

### 17.13 Estado de esta parte y lo que no se hizo

**Estado:** **R2-B1 (v0.5-R2B1) — APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-18); ADR-02.C sigue como PROPUESTA** (aprobado sin una auditoría posterior a los ajustes E-1 y E-2). La aprobación **no es automática** para R2-B, R2 ni la Fase 2. **R2-B2, R2-C, R3 y R4: NO INICIADOS Y SIN AUTORIZACIÓN.** Fase 2: **AUTORIZADA, NO APROBADA globalmente.**

**No se realizó:** la especificación de UI-01…UI-13 ni la frontera atómica (R2-B2); el modelo adicional de amenazas de R2-B; ADR-04.C, ADR-05.C ni ADR-16.C; diseño de datos, políticas de acceso concretas, migraciones ni interfaces de servicio; selección de proveedores, productos o mecanismos; código; ninguna prueba, prototipo ni medición; ninguna modificación de `01-requisitos.md`, de `aprobaciones.md`, de `bitacora.md`, de R1 ni de R2-A; commit, push, PR ni merge; **ninguna aceptación de riesgos, fijación de parámetros ni concesión de excepciones**; ninguna decisión sobre las 12 decisiones de R2-A que siguen abiertas (la 11, de las 13 originales, está atendida) ni sobre las de §17.8; ninguna puntuación de los riesgos de §17.9.

**Siguiente paso:** cualquiera que se tome (revisión o auditoría de la v0.5, correcciones, decisiones, R2-B2, registro documental, versionado) requiere una autorización expresa e independiente.

### 17.14 Hallazgos de la auditoría de R2-B1 (B-01…B-14) y correcciones de la v0.2

**Origen:** dictamen «R2-B1 REQUIERE CORRECCIONES» de un subagente de contexto nuevo (2026-11-17), con **independencia solo de contexto** (mismo tipo de modelo que el autor; sin independencia organizativa ni externa); es una recomendación, no una aprobación. **«Corregido» significa que el autor aplicó la corrección en la v0.2.** La validación posterior (§17.15) confirmó la mayoría y señaló reservas, que la v0.3 corrige; esta tabla no es una verificación independiente.

| ID | Hallazgo | Corrección aplicada | Dónde | Estado |
|---|---|---|---|---|
| B-01 | La matriz no seguía sus reglas de rotulado; los recuentos no servían | Criterio uniforme de rótulos y condiciones K-1 a K-4; matriz rehecha (25 × 4 = 100 celdas en la v0.2; 26 × 4 = 104 en la v0.3, C-03 y C-07); recuentos recalculados | §17.3, §17.3.1, §17.12 | Corregido |
| B-02 | CAP-02-C no era independiente de CAP-02-A y el borrador no declaraba el déficit frente a R1 §8.1 | Equivalencia C completa ≡ A + versión declarada; **CAP-02-D** formulada y evaluada como candidata a alternativa independiente, condicionada (la validación posterior precisó que su independencia no está demostrada: C-01); candidatas descartadas; déficit registrado como DB1-1, sin excepción; comparación reescrita y frase orientadora retirada | §17.3.3, §17.3.4, §17.8 | Corregido |
| B-03 | H2B1-04 y DB1-2 ofrecían «solo servidor» como opción para RT-14 | H2B1-04 reformulada (RT-14 no dice «solo»; compatible con RT-02 y CL-48; una formulación que lo hiciera es No conforme); la opción se retiró de las decisiones | §17.8 | Corregido |
| B-04 | Faltaban invariantes respaldados por `01` | IN-02-20 a IN-02-25 (estado, unicidad del delegado y del editor, restricciones de delegado y editor, RF-ACC-01, identidad verificable); IN-02-12 completado; garantías de IN-03-04 separadas de IN-02-10 | §17.2, §17.4 (CF-01) | Corregido |
| B-05 | CF-02 concedía más que `01 §7.1`; CF-04 sujetaba a RT-14 la fila 17 | CF-02 con RT-14, RT-05 y A-2; CF-04 separa las filas 16 y 17 según A-4 | §17.4 | Corregido |
| B-06 | Trazabilidad con amenazas mal asignada; OM-01 omitida | Distinción «tratada» y «contexto»; AM-19, AM-34, OM-01 y OM-07 como contexto; AM-34 y AM-19 retiradas de RB1-03 y RB1-06 | §17.9, §17.10 | Corregido |
| B-07 | Tipado de IN-02-02 y etiquetas dobles | Una sola etiqueta por invariante; IN-02-02 justificado con CR1-13 y su paso derivado; «Respaldo literal» y «Paso derivado» añadidos; IN-02-14 y las credenciales de servicio separadas (IN-02-29 [REC]) | §17.2 | Corregido |
| B-08 | La lectura alternativa de A-6 contradecía RF-EQP-06 y RT-12 | Se citan literalmente ambas reglas; la lectura se declara en conflicto; H2B1-06 reformulada | §17.6, §17.8 | Corregido |
| B-09 | Criterios CR2B1 débiles o circulares | CR2B1 rehechos con comprobaciones mecánicas y una comparación manual declarada como tal; tabla de cobertura de las 41 filas | §17.4, §17.11, §17.12 | Corregido |
| B-10 | Faltaban riesgos de diseño | RB1-09 a RB1-13 (colusión, elusión de RT-14, anonimización histórica, bloqueo de revocaciones de seguridad, divergencia de la proyección); sin puntuar ni aceptar | §17.9 | Corregido |
| B-11 | Los roles acumulados no calcaban `01 §7.4` | Las ocho filas de §7.4 en su orden y dos casos adicionales rotulados (combinación triple y revocación parcial) | §17.4 | Corregido |
| B-12 | Celdas más laxas que `01` | CF-09 desglosado por fila; W.O. y desempate como [ABIERTO] con el texto de la fila; exportación del organizador «No»; Q-21, Q-22, Q-23 y Q-29 incorporadas | §17.4, §17.6 | Corregido |
| B-13 | La precedencia de estado omitía líneas | §17.0 enumera la línea 5, la línea 6, §12, §16.0 y §16.5 y el «Próximo paso» de `aprobaciones.md` | §17.0 | Corregido |
| B-14 | Imprecisiones menores | IN-02-17 limitado a las ambigüedades de ADR-02; A-15 con ADR-05; IN-02-08 con las excepciones de RT-03 y RT-06; CL-55 ligado a IN-02-15, CL-41 y CL-49 añadidos; RF-ACC-15 se cita por su tipo de prueba «DB» (definido en `01 §0.2`) y no como «denegación en la base de datos» | §17.2, §17.6, §17.10 | Corregido |


### 17.15 Validación de la v0.2 y correcciones de la v0.3

**Origen:** dictamen «R2-B1 APROBABLE CON CONDICIONES» de un subagente de contexto nuevo (2026-11-18), con **independencia solo de contexto** (mismo tipo de modelo que el autor; sin independencia organizativa ni externa); es una recomendación, no una aprobación. Clasificó 9 de los 12 primeros hallazgos como resueltos y 3 como parcialmente resueltos, y formuló 8 hallazgos nuevos. **«Corregido» significa que el autor aplicó la corrección en la v0.3.** La validación posterior (§17.16) confirmó cinco como resueltos y tres como parcialmente resueltos; los estados de la última columna reflejan esa validación y la v0.4 corrige los residuos. Esta tabla no es una verificación independiente.

| ID | Hallazgo | Corrección aplicada | Dónde | Estado |
|---|---|---|---|---|
| C-01 | La independencia de D respecto de A se afirmaba sin exponer la lectura contraria; DB1-1 (a) podía leerse como cumplimiento condicional de «≥ 2» | §17.3.3 presenta **dos interpretaciones** (D distinta; D variante de A), declara que la independencia **no está demostrada** y qué la demostraría; D no se cuenta como segunda forma; DB1-1 y H2B1-11 reformuladas como dependencia documentada de R2-B2, sin excepción; la fila B-02 de §17.14 deja de llamar «independiente» a D | §17.3.1, §17.3.3, §17.8, §17.14 | Corregido |
| C-02 | La lista de unidades de D omitía las operaciones que cambian el estado; H2B1-10 no citaba que §5.5 no es exhaustiva ni su cláusula sobre proyecciones derivadas; K-2 era demasiado categórica | K-2 separada en K-2a a K-2d; H2B1-10 enumera las operaciones que modifican permisos y estados (con la condición de que el estado esté o no en la proyección); cita R1 §5.3 y §5.5 y precisa que ninguno trata de proyecciones de permisos y que **no se presupone modificar R1**; cuadro de lo que puede establecer R2-B1 y lo que es exclusivo de R2-B2; DB1-6 ofrece confirmar la lectura sin modificar R1 | §17.3 (K-2), §17.3.3, §17.8 | Corregido (con un residuo: D-01, corregido en la v0.4) |
| C-03 | Rotulado incoherente con la regla de §17.3 en B·IN-02-05, IN-02-24 (A, C, D) e IN-02-15 e IN-02-25 (las cuatro) | B·IN-02-05 pasa a «No conforme (parcial)» y D·IN-02-05 a «Condicionada (K-2)»; la «decisión completa», el piso de C y D incluyen la invitación vigente y las reglas de creación de torneo (IN-02-24 y IN-02-26); IN-02-15 e IN-02-25 pasan a «Condicionada (K-5)», nueva condición por la dependencia de ADR-03.C; recuentos recalculados | §17.3, §17.3.1 | Corregido en parte (D-02 y D-05, corregidos en la v0.4) |
| C-04 | Recuentos inexactos: celdas de D con K-2, extractos literales, términos de la lista de tecnologías | Recuentos recalculados por script y reescritos en §17.3.1 y §17.12; el número de extractos se cuenta solo en la columna «Respaldo literal»; la lista de términos se describe sin atribuirla a R2-A | §17.3.1, §17.12 | Corregido en parte (la cifra de extractos era inexacta, D-06; corregida en la v0.4) |
| C-05 | CF-09 atribuía al dueño «en exclusiva» la bitácora de seguridad, que no figura en las 41 filas | La bitácora se retiró de CF-09 y se registra como propuesta [REC] fuera de las 41 filas (RF-AUD-05, IN-09-08), sin conceder su lectura | §17.4 | Corregido |
| C-06 | Q-21 y Q-29 se presentaban como parámetros de «quién»; no es lo que preguntan | §17.6 separa **preguntas sobre facultades** y **preguntas sobre reglas deportivas o estados**; la segunda clase solo reserva la fila, sin «Sí» ni «No»; CF-07 explica por qué el «Sí» del dueño queda [ABIERTO] | §17.4, §17.6, §17.8 | Corregido en parte (Q-22 y Q-23, D-04; corregidos en la v0.4) |
| C-07 | Respaldos débiles en IN-02-14, IN-02-05 e IN-02-12; faltaba el invariante de RF-ACC-12 | IN-02-14 se respalda en la introducción de `01 §7.1` y en `SKILL.md` y declara que «mínimo privilegio» no es un requisito literal de `01`; IN-02-05 cita RT-01 y RT-12; IN-02-12 incorpora «al sincronizar» y la protección del último dueño; **IN-02-26** recoge RF-ACC-12 (obligatorios: 26); las propuestas [REC] pasan a IN-02-27, 28 y 29 | §17.2 | Corregido |
| C-08 | RB1-02 se mapeaba a AM-02 (toma de cuenta), un mapeo débil | RB1-02 se relaciona con IN-02-25, IN-03-11 y AM-09 (el contexto local no autentica; es de ADR-03.C) | §17.9 | Corregido |


### 17.16 Validación de la v0.3 y correcciones de la v0.4

**Origen:** dictamen «R2-B1 APROBABLE CON CONDICIONES» de un subagente de contexto nuevo (2026-11-18), con **independencia solo de contexto** (mismo tipo de modelo que el autor; sin independencia organizativa ni externa); es una recomendación, no una aprobación. Declaró resueltos C-01, C-02, C-05, C-07 y C-08 y parcialmente resueltos C-03, C-04 y C-06, y formuló 9 hallazgos nuevos; D-01, D-02 y D-03 condicionaban la aprobación. **«Corregido» significa que el autor aplicó la corrección en la v0.4; la v0.4 no ha sido revisada de nuevo** y esta tabla no es una verificación independiente.

| ID | Hallazgo | Corrección aplicada | Dónde | Estado |
|---|---|---|---|---|
| D-01 | La definición de CAP-02-D era contradictoria sobre el estado: lo incluía entre los cambios que actualizan la proyección y a la vez lo excluía de ella | Una sola formulación: **el estado se lee de su fuente y no forma parte de la proyección**; las operaciones que modifican estados no actualizan la proyección; K-2a y K-2c, H2B1-10 y la definición coinciden; el estado incorporado a la proyección queda como variante no evaluada. D·IN-02-20 pasa a «Conforme» (como en A); **rotulado revisado en la v0.5 por E-2 (§17.17)** | §17.3 (K-2, CAP-02-D, matriz), §17.3.3, §17.8 | Corregido |
| D-02 | IN-02-09 incorpora IN-02-15 y sus rótulos no eran coherentes con él | Se refleja la dependencia: A·IN-02-09 «Condicionada (K-5)»; B y C «No conforme (parcial)» por el nivel de autenticación solo en el servidor; D «Condicionada (K-2, K-5)»; recuentos recalculados | §17.3, §17.3.1 | Corregido |
| D-03 | H2B1-10 omitía que R1 asigna toda decisión de permiso a CMP-08 y no decía quién calcula la proyección | Nueva §17.3.5: textos de R1 (§5.3, CR1-13, AC-1); quién calcula la proyección (P-1, P-2 compatibles; P-3 incompatible); quién decide (CMP-08); qué datos admite; garantías contra un segundo decisor; sin presuponer que R1 deba modificarse; DB1-6 reformulada | §17.3.5, §17.8 | Corregido |
| D-04 | Q-22 y Q-23 se describían como si no preguntaran por «quién» | Se citan sus preguntas literales y se declara que la regla y la facultad están abiertas; CF-07, H2B1-08 y DB1-4 coherentes | §17.4, §17.6, §17.8 | Corregido |
| D-05 | Rótulos de B·IN-02-14, D·IN-02-14 y B·IN-02-24 | B·IN-02-14 «No conforme (parcial)»; D·IN-02-14 «Condicionada (K-2)»; B·IN-02-24 «No conforme» (B omite la verificación); K-4 retirada; recuentos recalculados | §17.3, §17.3.1 | Corregido |
| D-06 | «37 extractos» inexacto; «Corregido» de C-04 inexacto; cifras no reproducibles | Los extractos se cuentan en la columna «Respaldo literal» y se verifican con el script del autor; el estado de C-04 en §17.15 pasa a «Corregido en parte»; la cifra de términos de tecnología se retira y la de identificadores se define con las familias usadas | §17.12, §17.15 | Corregido |
| D-07 | «Las doce decisiones abiertas de R2-A»: la 11 figura como atendida | Se distingue: 13 decisiones originales, de las que la 11 está atendida y 12 siguen abiertas | §17.8, §17.11, §17.13 | Corregido |
| D-08 | DB1-1 (c) listaba «excepción o modificación de R1» y decía a la vez que no se proponía | DB1-1 (c) aclara que es una vía que corresponde al usuario, que no está aprobada ni propuesta y que no se ha tomado ninguna decisión | §17.8 | Corregido |
| D-09 | Ajustes editoriales menores | Conclusión de A («sin celdas No conforme; conformidad condicionada»); CF-01 con «autenticación reforzada y auditoría» (fila 4); combinación 6 sin CL-35 como prueba de la ausencia de persistencia; K-2b y K-2d coherentes sobre la reconstrucción; respaldos añadidos a IN-02-01 (RF-ACC-15), IN-02-04 (RT-08) e IN-02-25 (RT-02); IN-02-14 cita la introducción de `01 §7` | §17.2, §17.3.1, §17.4 | Corregido |

### 17.17 Verificación de la v0.4 y ajustes de la v0.5

**Origen:** dictamen «APROBABLE COMO ENTREGABLE DOCUMENTAL CONDICIONADO» de un subagente de contexto nuevo (2026-11-18), con **independencia solo de contexto** (mismo tipo de modelo que el autor; sin independencia organizativa ni externa); es una recomendación, no una aprobación. Declaró resueltos D-01 a D-09, sin defectos materiales, y señaló dos ajustes menores y una observación. **«Corregido» significa que el autor aplicó el ajuste en la v0.5; la v0.5 no ha sido revisada de nuevo** y esta tabla no es una verificación independiente.

| ID | Hallazgo | Ajuste aplicado | Dónde | Estado |
|---|---|---|---|---|
| E-1 | §17.3.3 decía que CAP-02-C es «No conforme» en 6 celdas; la matriz y §17.3.1 dan 7 (IN-02-01, 03, 06, 09, 12, 15 y 17) | El texto de §17.3.3 dice 7; los demás recuentos de C (16, 3 y 7) ya eran coherentes | §17.3.3 | Corregido |
| E-2 | D·IN-02-20 figuraba «Conforme», pero IN-02-20 incluye que solo el dueño corrige con el torneo `archivado` y el rol de dueño sale de la proyección (K-2). **Dependencia confirmada** por el texto literal de IN-02-20 («solo lo hace el dueño») y por la definición de CAP-02-D (roles vigentes en la proyección) | D·IN-02-20 pasa a «Condicionada (K-2)», con la explicación mínima; CAP-02-D pasa a 6 conformes, 20 condicionadas (18 con K-2) y 0 no conformes; la lista de celdas que D evalúa sobre las fuentes pasa a D·IN-02-10, 21, 22 y 24 | §17.3, §17.3.1, §17.3.3, §17.12 | Corregido |
| E-3 | IN-02-24 figura «Conforme» en A, C y D aunque la definición de la invitación es de ADR-03.C (IN-03-05) | Sin cambio: el revisor no lo calificó de defecto y coincide con el alcance declarado de IN-02-24 | — | Observación sin ajuste |

---

# PARTE IV — BLOQUE R2-B2-a (v0.3-R2B2a, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL, 2026-11-19)

## 18. R2-B2-a — ADR-02.C (parte 2a): frontera atómica de las operaciones que modifican permisos

### 18.0 Alcance, autorización y reglas de esta parte

**Estado: v0.3-R2B2a — APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-19), por acto expreso del usuario (ver «Aprobación condicional» más abajo).** La v0.3 corrigió la v0.2 tras la revisión focalizada de contexto nuevo (§18.16) y la v0.2 corrigió la v0.1 (§18.15). Esta parte no altera ninguna línea de R1 (§0–§15), R2-A (§16) ni R2-B1 (§17); solo se **añade** al final del archivo.

**Autorización recibida.** Mensaje «TORNEO DE CAMPEONES — INICIO CONTROLADO DE R2-B2-a» (2026-11-18): «AUTORIZO EXCLUSIVAMENTE EL INICIO DE R2-B2-a DE LA FASE 2. APRUEBO EL PLAN DE TRABAJO DE R2-B2 COMO GUÍA DOCUMENTAL, PERO NO AUTORIZO TODAVÍA R2-B2-b.»

**Autorización de la corrección (v0.2).** Mensaje «CORRECCIÓN INTEGRAL DE R2-B2-a v0.1»: autoriza «EXCLUSIVAMENTE UNA RONDA DE CORRECCIONES DOCUMENTALES DE LOS HALLAZGOS A-01 A A-18» limitada a la Parte IV (§18); R2-B2-a continúa como **borrador pendiente de aprobación**.

**Autorización de la corrección focalizada (v0.3).** Mensaje «CORRECCIÓN FOCALIZADA B-01 Y E-01…E-07» (2026-11-19): autoriza «EXCLUSIVAMENTE CORREGIR EL DEFECTO MATERIAL B-01 Y LAS OBSERVACIONES E-01 A E-07 DEL BORRADOR R2-B2-a v0.2», limitada a la Parte IV (§18).

**Aprobación condicional de R2-B2-a como entregable documental (2026-11-19).** Por acto expreso del usuario (mensajes «TORNEO DE CAMPEONES — APROBACIÓN DOCUMENTAL CONDICIONADA DE R2-B2-a» y «TORNEO DE CAMPEONES — REGISTRO FORMAL DE APROBACIÓN CONDICIONAL R2-B2-a»), la v0.3-R2B2a quedó **APROBADA CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL**: «APRUEBO CONDICIONALMENTE R2-B2-a v0.3 COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL.»; registro autorizado con «AUTORIZO EXCLUSIVAMENTE REGISTRAR LA APROBACIÓN CONDICIONAL DE R2-B2-a COMO ENTREGABLE DOCUMENTAL.»

- **Comprende:** las siete fichas UI-01, 02, 03, 04, 08, 11 y 12; los invariantes de frontera IF; los estados intermedios prohibidos ES; las garantías transaccionales GT; la protección global del último dueño; la matriz de escenarios de concurrencia y revocaciones; el análisis documental de K-2; las evidencias de CAP-02-A frente a CAP-02-D; el inventario de operaciones candidatas; los riesgos, condiciones y decisiones documentados; y las correcciones hasta la v0.3-R2B2a.
- **Exclusiones (la aprobación NO implica):** (1) elegir CAP-02-A ni CAP-02-D; (2) declarar demostrada la independencia de CAP-02-D; (3) dar por cumplido R1 §8.1; (4) resolver DB1-1 a DB1-6; (5) resolver DB2a-1 a DB2a-10; (6) resolver OC-01 a OC-17; (7) resolver las doce decisiones abiertas de R2-A; (8) aprobar definitivamente ADR-02.C; (9) considerar K-2d implementada o validada; (10) fijar PAR ni aceptar RES o riesgos RB; (11) conceder excepción a D-2; (12) aprobar globalmente R2 ni la Fase 2; (13) autorizar R2-B2-b, R2-C, R3 o R4.
- **Limitación de la revisión.** La verificación favorable la hizo un subagente del mismo tipo de modelo que redactó el borrador: su independencia fue **solo de contexto, no organizativa ni externa**. El dictamen se limita a documentación y **no acredita seguridad implementada, pruebas de concurrencia ni validación empírica**.
- **Ocho observaciones editoriales no bloqueantes** (identificadas para una eventual conciliación documental; no son condiciones técnicas): (1) GT-09 y GT-10 listan «Las siete» unidades aunque ES-13 alcanza más operaciones (§18.7 y §18.7.1 declaran el límite); (2) en UI-02, «el estado resultante se verifica sobre las fuentes» podría leerse como que ES-04 es una regla de integridad (es [DER]); (3) UI-08, campo 4, dice «a partir del historial» sin cualificar el caso D; (4) EV-04 omite UI-01 entre las unidades con integridad atómica (EV-01 la incluye); (5) IF-07 es [CONF] pero contiene una cláusula [DER] que remite a IF-02; (6) IF-15 dice «todas las fichas» y su lista excluye UI-01; (7) en UI-12 la invitación aparece en dos columnas; (8) la afirmación de UI-01, campo 8, sobre dos creaciones concurrentes no está rotulada [DER].
- **Integridad.** SHA-256 del **contenido aprobado** (el archivo inmediatamente antes de añadir estos metadatos): `f07b4d9ffc363035bb8fc9284f8efd28b6c2c6f398eaaedaffa50abaa4cbf644`. El hash del **archivo con los metadatos de aprobación** se registra en `aprobaciones.md` y `bitacora.md`; **no se inserta en este archivo**. Los metadatos de aprobación son solo editoriales: título y estado de la Parte IV, §18.0, la fila de estado de §18.1, referencias de §18.13 y §18.14, notas de cierre de §18.15 y §18.16 y la frase de estado de §17.0.
- **Estado de versionado.** **Pendiente:** sin commit, push, PR ni merge.
- **Condición de cierre de la Fase 2.** Las decisiones que la Fase 1 asignó a la Fase 2 deberán resolverse antes de cerrar globalmente la Fase 2, salvo modificación o excepción formalmente aprobada.

**Precedencia de estado.** Otras líneas de este documento, `aprobaciones.md` y `bitacora.md` (en sus entradas históricas) dicen que R2-B2 **no está iniciado** porque describen una fecha anterior. **Vigente (2026-11-20): R2-B2-a APROBADO CONDICIONALMENTE como entregable documental (esta Parte IV); R2-B2-b1 APROBADO CONDICIONALMENTE como entregable documental (Parte V, §19); R2-B2-b2 APROBADO CONDICIONALMENTE como entregable documental (Parte VI, §20); R2-B2 completo, con cierre documental condicionado aprobado (2026-11-21), pero sin aprobación global de R2 ni de Fase 2; R2-C, R3 y R4 NO INICIADOS Y SIN AUTORIZACIÓN.** El registro figura en `aprobaciones.md` y `bitacora.md`.

**Qué contiene.** La frontera atómica de las operaciones que **modifican permisos**: invariantes de frontera (§18.2), las siete fichas UI-01, UI-02, UI-03, UI-04, UI-08, UI-11 y UI-12 (§18.4), las operaciones que modifican permisos y **no** están en UI-01…13 como candidatas a la enumeración de la Fase 4, y las que pueden alterar el conjunto de dueños (§18.5), la matriz de concurrencia y revocaciones (§18.6), el análisis de K-2d (§18.7), la evaluación de CAP-02-A frente a CAP-02-D (§18.8), dependencias y decisiones abiertas (§18.9), riesgos nuevos sin aceptar (§18.10), las secciones reservadas a R2-B2-b (§18.11), trazabilidad y criterios (§18.12), verificaciones del autor (§18.13), estado de Git (§18.14) los hallazgos de la auditoría con sus correcciones (§18.15) y los de la revisión focalizada (§18.16).

**Qué NO contiene ni hace.**

| # | Exclusión |
|---|---|
| X-1 | No redacta R2-B2-b (UI-05, UI-06, UI-07, UI-09, UI-10, UI-13), ni R2-C, R3 ni R4 |
| X-2 | No aprueba R2-B2-b, R2-B2 completo, R2 ni la Fase 2 (la aprobación condicional de R2-B2-a como entregable documental figura en este apartado y no amplía su alcance) |
| X-3 | No resuelve DB1-1…DB1-6 ni ninguna decisión abierta de R2-A (de las **13 decisiones originales**, la 11 está atendida y **12 siguen abiertas**) |
| X-4 | **No elige** CAP-02-A ni CAP-02-D; **no declara demostrada la independencia de CAP-02-D**; **no declara cumplido R1 §8.1**; no concede excepción |
| X-5 | No fija PAR, no acepta RES ni RB1; no concede excepción a D-2 |
| X-6 | No elige tecnología, proveedor, producto, mecanismo de bloqueo, de serialización ni de consistencia; no escribe SQL, políticas, rutas ni contratos de interfaz |
| X-7 | No modifica `00`, `01`, `aprobaciones.md`, `bitacora.md`, ni R1, R2-A o R2-B1; no crea ramas, commits, pushes, PR ni merges |
| X-8 | No implementa ni da por aprobada K-2d: **la describe como garantía arquitectónica exigida** |

**Lectura previa directa.** El autor leyó directamente (no solo por resúmenes) el skill y sus referencias pertinentes; en `01`: RF-ACC-02, 06, 07, 09, 12, 13, 17, RF-AUD-02 y los casos CL-35, 38, 41, 43, 45, 47, 53, 54, 60 y 61; en R1: §5.3, §5.5, §7.0 y §8.1; en R2-A: IN-03-04 a IN-03-08 e IN-03-11 (ADR-03.C) e IN-09-01 a IN-09-06 y CAP-09-P-A/B/C (ADR-09.C); en R2-B1: IN-02, K-1…K-3, §17.3.3, §17.3.5, DB1-1…6 y RB1.

**Convención de estados de una afirmación.** [CONF]: texto confirmado de `01` o R1. [DER]: derivada por el autor (se explica el paso). [ABIERTO]: sin resolver; ninguna se elige. Las citas literales se comprueban en §18.13.

### 18.1 ADR-02.C (parte 2a) — Identificación

| Campo | Valor |
|---|---|
| ADR | ADR-02.C (continuación de §17.1): frontera atómica de las operaciones que modifican permisos |
| Entradas | R1 §5.3, §5.5, §7.0, §8.1; R2-A (ADR-03.C, ADR-09.C); R2-B1 (IN-02, CAP-02-A…D, K-1…K-3, DB1-1…6) |
| Salida de esta parte | Frontera por unidad, estados intermedios prohibidos, resultado ante concurrencia, garantías exigidas (K-2d) y evidencia para DB1-1 |
| Estado | v0.3-R2B2a: APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-19); ADR-02.C **no** queda aprobada definitivamente |
| Reservado a R2-B2-b | §18.11 |

### 18.2 Invariantes de frontera (IF-)

Son propiedades que **toda** unidad de §18.4 debe cumplir. **Transversales:** IF-04 (sin estados intermedios observables), IF-08 (permisos remanentes) e IF-15 (el historial no se altera) se aplican a **todas** las fichas y escenarios; no se citan una por una. IF-15 se verifica en toda unidad que revoca, retira, desactiva o levanta (UI-02, 03, 04, 08, 11, 12 y CAND-10). Las etiquetas se contrastan con la fuente **primaria** (`01`, R1); cuando un invariante de R2-A o R2-B1 es [DER] se cita solo como respaldo. No introducen permisos ni retiran garantías (IN-02-18). Las que se derivan se marcan [DER] con el paso.

| ID | Invariante de frontera | Tipo | Fuente | Paso derivado (solo [DER]) |
|---|---|---|---|---|
| IF-01 | **Una unidad indivisible por operación:** la reunión de entradas, el cálculo del efecto, la política de segundo factor, la decisión y la aplicación (incluido el evento de auditoría de lo aplicado) forman **una sola unidad**; o se aplica todo o no se aplica nada | [CONF] (de R1) | R1 §7.0 («los pasos 2 a 5 y la aplicación del paso 6 forman UNA sola unidad indivisible») y PA-07 (R1). RT-13 es [DER] en `01` y IN-02-03 [DER] en R2-B1: se citan solo como respaldo | — |
| IF-02 | **Lo decidido es lo aplicado:** las entradas con las que se decide (roles, asignaciones, historial, restricciones, estado de invitación) son las vigentes en el momento de aplicar; no se aplica una decisión tomada sobre entradas que otra operación confirmada ya cambió | [DER] | RT-13, IN-02-03, CL-45 | RT-13 exige evaluar la autorización «en la misma transacción que la aplica»; si entre la lectura y la aplicación otra operación confirmara un cambio de esas entradas, la autorización evaluada no sería la de la aplicación |
| IF-03 | **Las dos capas verifican:** el servidor y la capa de datos comprueban la autorización de cada unidad | [CONF] | RT-02, RC-04 (IN-02-01 lo recoge) | — |
| IF-04 | **Sin estados intermedios observables:** ninguna lectura ni decisión de otra operación observa un estado a medias de una unidad (catálogo de estados prohibidos: §18.3) | [DER] | RT-13, §5.5 («unidades indivisibles»), CL-45 | Una unidad «indivisible» solo lo es si no admite observadores del interior |
| IF-05 | **Equivalencia secuencial:** el resultado de dos o más unidades concurrentes sobre los mismos hechos equivale a **alguna** ordenación secuencial de ellas, con el resultado «aplicada antes y válida» o «rechazada» para cada una, nunca «a medias» | [CONF] en lo que dice RF-ACC-07 y CL-45; [DER] en la generalización a **cualquier** par de unidades | RF-ACC-07 (criterio: «aplicada antes y válida» o «rechazada», «nunca a medias»), CL-45, CL-61; RT-13 ([DER] en `01`) como respaldo | Los textos citados lo dicen de una revocación frente a una operación; el autor lo extiende a las unidades de §18.4 que compiten por un mismo hecho, porque RT-13 pide serializar «revocaciones, reasignaciones y cambios de rol» |
| IF-06 | **Revocación efectiva:** desde que una revocación se confirma, toda **nueva** operación sin autorización vigente se rechaza; una operación en curso se resuelve como aplicada antes (válida y auditada) o rechazada | [CONF] | RF-ACC-07, IN-03-06, CL-45 | — |
| IF-07 | **Último dueño activo:** no se revoca ni se degrada al último dueño activo, de forma atómica incluso con operaciones simultáneas; no hay jerarquía técnica entre dueños. La protección se evalúa sobre el conjunto vigente en el momento de aplicar (IF-02). *El alcance global y el significado de «activo» están en IF-19 y OC-15* | [CONF] | RT-06 («No se puede revocar ni degradar al último dueño activo, de forma atómica incluso con operaciones simultáneas»), RF-ACC-09, IN-02-10, CL-43 | — |
| IF-08 | **Permisos remanentes:** revocar un rol o una asignación no elimina los permisos legítimos de los demás roles vigentes; la operación pendiente se evalúa por los roles restantes | [CONF] | RT-11, IN-02-07, CL-53 | — |
| IF-09 | **El rechazo queda fuera de la unidad:** el registro de un rechazo o bloqueo no forma parte de ella y persiste aunque la operación no se aplique; **cómo** persiste es la decisión 10 de R2-A (CAP-09-P) y la condición K-3 | [DER] | R1 §7.0 paso 6 («[DER de RF-AUD-06: `01` exige auditar los rechazos, no formula la persistencia tras una reversión]»), IN-09-05, K-3 | `01` (RF-AUD-06) exige auditar los rechazos; que el registro sea independiente de la unidad y persista tras una reversión es la derivación de R1 §7.0 |
| IF-10 | **Lo aplicado se audita dentro de la unidad:** si el evento de una concesión, modificación o revocación de permisos no puede registrarse, la operación no se aplica | [DER] | RT-10, IN-09-04, IN-02-16, RF-AUD-02 | IN-09-04 lo establece para toda operación aplicada; RT-10 exige la auditoría de las operaciones sobre permisos |
| IF-11 | **RT-14 y RT-05 se evalúan por efecto dentro de la unidad:** también sobre las asignaciones que la unidad retira o crea | [CONF] | RT-05, RT-14, R1 §7.0 paso 5 | — |
| IF-12 | **Nivel de autenticación consumido, no emitido:** CMP-08 compara el nivel emitido por CMP-06 con la política efectiva de **todos** los roles vigentes **antes** de decidir; designar o revocar un dueño exige nueva autenticación con segundo factor (lectura de «nueva» abierta: decisión 12 de R2-A) | [CONF] | RF-ACC-10, RC-11, RF-ACC-09, R1 §7.0 paso 4 (IN-02-15 e IN-03-04 los recogen; IN-02-15 es [DER]) | — |
| IF-13 | **Aislamiento dentro de la unidad:** toda referencia entre entidades que la unidad crea o modifica pertenece al **mismo torneo** | [CONF] | RT-08, IN-02-04 | — |
| IF-14 | **Efectos compuestos completos:** toda consecuencia que la regla exige para dejar el estado coherente (retirar asignaciones en conflicto, desactivar asignaciones de un rol revocado, anular un levantamiento) pertenece a la **misma** unidad que su causa | [DER] | RT-05, CL-35, RF-ACC-17, IN-02-22 | Si esas consecuencias se aplicaran en una unidad posterior, existiría una ventana en la que la causa está confirmada y la consecuencia no (estado prohibido de §18.3) |
| IF-15 | **El historial no se altera:** una revocación o retirada cambia el estado y la fecha y añade historial; no elimina | [CONF] | RF-ACC-11, IN-09-02 | — |
| IF-16 | **Una sola definición de la decisión en ambas capas:** las dos capas aplican la **misma** definición de CMP-08 (K-1) | [DER] | CR1-13, IN-02-02 | Es la lectura más estricta de «único decisor»; su propiedad de implementación es la condición K-1 |
| IF-17 | **Las lecturas ambiguas (A-2, A-12, A-15, Q-40, Q-42) son parámetros del evaluador, no elecciones;** ninguna unidad fija una lectura | [DER] | R1 §9.3, PA-13, IN-02-17 | R1 §9.3 pide soportar ambas lecturas de las ambigüedades abiertas |
| IF-18 | **La capa de datos verifica además el estado resultante de cada unidad** (los estados prohibidos de §18.3 no deben poder producirse aunque el servidor y el cliente se equivoquen) | [DER] | RF-ACC-13 (criterio: «BD, incluso con altas concurrentes»), RT-08, IN-02-04, IN-02-21, IN-02-22 | `01` marca la verificación en la capa de datos para varias reglas concretas (RF-ACC-13, RT-08); el autor lo generaliza al estado resultante de las unidades de §18.4, que `01` no formula así |
| IF-19 | **Alcance global de la protección del último dueño:** ninguna operación —las de §18.4, las candidatas de §18.5.1 ni ninguna aún no enumerada— ni ningún conjunto de operaciones concurrentes deja al sistema sin un dueño activo autorizado. *«Activo» y «autorizado» no se definen aquí (OC-15); el máximo de dueños tampoco (OC-16)* | [DER] | RT-06 y RF-ACC-09 («contra eliminación o degradación, incluso ante operaciones simultáneas»), IF-07, CL-43 | RT-06 y RF-ACC-09 protegen al último dueño sin limitar la protección a una operación concreta; el autor la aplica a toda operación que pueda producir el efecto, incluidas las candidatas y las no enumeradas. La extensión a CAND-08 y CAND-09 depende de OC-15 |

### 18.3 Convenciones de las fichas y catálogo de estados intermedios prohibidos

**Contenido de cada ficha.** (1) Disparador; (2) componentes según R1 §5.5; (3) requisitos aplicables; (4) autorización en el servidor y en la capa de datos; (5) secuencia lógica (pasos de R1 §7.0, sin mecanismo); (6) cambios indivisibles; (7) estados intermedios prohibidos; (8) resultado ante concurrencia; (9) relación con la auditoría; (10) decisiones y condiciones abiertas. **La lista de §5.5 no es exhaustiva y su enumeración definitiva por operación es de la Fase 4** (R1 §5.5).

**Catálogo de estados prohibidos (ES-).** Estados que **ningún observador** debe ver y que **ninguna ordenación de operaciones concurrentes** debe producir. La columna **Tipo** indica si el estado está respaldado literalmente en `01` o R1 ([CONF]) o lo deriva el autor ([DER]); los que dependen de una decisión abierta se rotulan **provisionales**:

| ID | Estado prohibido | Tipo | Fuente |
|---|---|---|---|
| ES-01 | Dos editores vigentes en el mismo partido | [CONF] | RF-ACC-06, IN-02-22 |
| ES-02 | Ningún dueño activo | [CONF] | RT-06, IN-02-10 |
| ES-03 | La misma persona delegada de dos equipos del mismo torneo | [CONF] | RF-ACC-13, IN-02-21 |
| ES-04 | Un delegado de un equipo con una asignación de editor vigente en un partido de ese equipo. **Es una garantía derivada, no una regla literal:** RT-05 prohíbe «actuar como editor» y exige retirar las asignaciones en conflicto al otorgar el rol; ES-04 expresa el estado que esa retirada debe producir. Otras operaciones podrían producirlo (cambios de los equipos de un partido con editor ya asignado: UI-09, reservada a R2-B2-b, §18.11) | [DER] | RT-05, CL-35 |
| ES-05 | Cuando el creador es **organizador** (RF-ACC-12): torneo creado sin la asignación de su creador, asignación sin torneo, o asignación del creador a otro torneo. **El caso en que crea el dueño no está definido en `01` y no se amplía aquí (OC-17)** | [CONF] (creador organizador) | RF-ACC-12, IN-02-26 |
| ES-06 | Invitación consumida sin rol otorgado, rol otorgado sin invitación consumida, o invitación aceptada dos veces | [CONF] en el uso único; [DER] en el resto | RF-ACC-02, IN-03-05 |
| ES-07 | Levantamiento registrado sin motivo, aprobado por quien tiene conflicto o por el propio afectado, o con ámbito mayor que el equipo y el torneo | [CONF] | RF-ACC-17 |
| ES-08 | Restricción levantada y vigente a la vez que el rol de delegado se otorga de nuevo | [CONF] (RF-ACC-17; lectura de A-12 abierta) | RF-ACC-17 |
| ES-09 | Cambio de permisos aplicado sin su evento de auditoría | [DER] | RT-10, IN-09-04 |
| ES-10 | Operación aplicada y confirmada con una autorización ya revocada y confirmada | [CONF] | RT-13, CL-45 |
| ES-11 | Revocación a medias: rol revocado y asignaciones que dependen de él aún vigentes | [DER] | IN-02-22, IF-14 |
| ES-12 | **Provisional (OC-09, DB2a-4):** partido sin editor vigente **por una ventana** entre la desactivación y la activación de una reasignación | [DER], **provisional** (OC-09, DB2a-4) | R1 §5.5 (UI-03: «desactivar la asignación anterior y activar la nueva»); ver OC-09 |
| ES-13 | (**solo CAP-02-D**) Una decisión que lee una proyección derivada que no refleja un cambio de permisos ya confirmado | [DER] (solo CAP-02-D) | K-2d (§17.3), RB1 |

### 18.4 Fichas de las siete unidades

**Provisionalidad.** Todo lo que en una ficha depende de condiciones o decisiones abiertas es **provisional** hasta que el usuario decida: la composición de UI-02 con UI-12 y la anulación del levantamiento (OC-01, OC-02, OC-10; DB2a-2, DB2a-3), el estado ES-12 y la semántica de CL-41 (OC-08, OC-09; DB2a-4), la delimitación de UI-04 con UI-11 (OC-03; DB2a-1), la asignación al crear cuando el creador es dueño (OC-17) y el significado y el máximo de dueños (OC-15, OC-16). **Ninguna interpretación del autor se convierte en decisión aprobada.** ESC-18 y ESC-20 se aplican a **toda** ficha bajo CAP-02-D.

**Cómo leer "bajo D".** Cada ficha indica solo qué **añadiría** CAP-02-D (§17.3) a la unidad: un paso de mantenimiento de la proyección derivada dentro de la misma unidad (K-2c, K-2d). No se elige ninguna formulación. Las **dos capas** verifican en todas las fichas (IF-03).

#### 18.4.1 Ficha UI-01 — Crear torneo y asignar al creador como organizador de ese torneo

| Campo | Contenido |
|---|---|
| 1. Disparador | Un actor autorizado por la matriz de `01 §7.1` (fila 10) crea un torneo. Cuando el creador es **organizador**, su asignación es una operación **del sistema autorizada previamente**, no una autoasignación discrecional (RF-ACC-12; excepción de IN-02-08). **Qué asignación corresponde cuando crea el dueño no lo dice `01` y no se amplía aquí (OC-17)** |
| 2. Componentes (R1 §5.5) | CMP-09 + CMP-07 + CMP-08 + CMP-16 |
| 3. Requisitos | RF-ACC-12, Q-02a, RT-04; IN-02-08, IN-02-26; RF-AUD-02 (asignación de organizador al crear el torneo) |
| 4. Autorización | **Servidor:** CMP-08 decide que el actor puede crear el torneo y, **cuando el creador es organizador**, que la asignación resultante es **solo** la de organizador de **ese** torneo para el creador. **Capa de datos:** verifica el mismo permiso y, para el creador organizador, que el estado resultante contiene una única asignación de organizador del creador, limitada a ese torneo, sin permisos sobre torneos ajenos ni capacidad de crear o designar organizadores (IN-02-26). **Cuando crea el dueño, no se presupone ninguna asignación (OC-17)** |
| 5. Secuencia lógica | (1) identificar al actor; (2) reunir sus roles vigentes y la política de segundo factor; (3) calcular el efecto: nuevo torneo y, **si el creador es organizador**, una asignación (el caso del dueño: OC-17); (4) política efectiva de 2FA; (5) decidir; (6) aplicar torneo, asignación, historial y evento de auditoría |
| 6. Cambios indivisibles | Registro del torneo + **(si el creador es organizador)** asignación del creador (con su entrada de historial en CMP-07) + evento de auditoría de la asignación. «si la asignación falla, el torneo no se crea» (IN-02-26). **Bajo D:** + actualización de la proyección (la asignación de organizador, cuando existe). **Regla de integridad:** la creación indivisible del torneo y de su asignación (verificada sobre las fuentes). **Hecho derivado:** la asignación vigente del creador, que la proyección contendría (IN-02-26 en K-2a) |
| 7. Estados prohibidos | ES-05 (para el creador organizador), ES-09, ES-10 (creación con el rol del actor ya revocado) y, bajo D, ES-13 |
| 8. Concurrencia | **ESC-16:** una revocación del rol del actor que coincide con la creación se resuelve como creación aplicada antes (el torneo y, para el creador organizador, su asignación, válidos y auditados) o creación rechazada (IF-05, IF-06); **si la creación se aplica primero, una revocación posterior alcanza también la asignación del creador creada por esta unidad, cuando existe** (ES-11). Dos creaciones concurrentes del mismo actor producen dos torneos con sus propias asignaciones, cuando proceden; **no hay conflicto de permisos entre ellas** |
| 9. Auditoría | Se audita la asignación de organizador al crear el torneo (RF-AUD-02) dentro de la unidad (IF-10). Un rechazo se registra fuera de ella (IF-09; K-3) |
| 10. Abiertas | **OC-12:** duplicación de una misma solicitud de creación (reintento): su tratamiento corresponde a la idempotencia de la frontera de sincronización (R2-B2-b / Fase 4) y no se decide aquí. **OC-17:** asignación al crear cuando el creador es dueño (RF-ACC-12 define solo la del organizador; no se amplía). **OC-14:** cómo se identifica la asignación del sistema frente a una autoasignación (IN-02-08) es un asunto de verificación en la capa de datos; el mecanismo es de R3 |

#### 18.4.2 Ficha UI-02 — Otorgar el rol de delegado y retirar las asignaciones de editor en conflicto

| Campo | Contenido |
|---|---|
| 1. Disparador | Un dueño u organizador del torneo T otorga a la cuenta U el rol de delegado del equipo X (por ejemplo, a un editor con partidos asignados de X: CL-35). **La relación con UI-12** (aceptación de invitación) **está abierta (OC-01)** |
| 2. Componentes (R1 §5.5) | CMP-07 + CMP-08 + CMP-16 |
| 3. Requisitos | RT-05, CL-35, OFF-22; RF-ACC-13, RF-ACC-17 (anulación), RT-04, RT-14; IN-02-11, IN-02-21 |
| 4. Autorización | **Servidor:** CMP-08 decide que el actor puede otorgar ese rol en T (sin autoasignación: RT-04), que U no es ya delegado de otro equipo de T (RF-ACC-13) y calcula el efecto, incluidas las asignaciones de U como editor en partidos de X. **Capa de datos:** verifica el mismo permiso y que **el estado resultante** no contiene a U como delegado de dos equipos de T (ES-03) ni a U con una asignación de editor vigente en un partido de X (ES-04); referencias del mismo torneo (IF-13) |
| 5. Secuencia lógica | (2) reunir roles, asignaciones de U en T, historial y restricciones; (3) efecto: concesión + retiradas + posible anulación de un levantamiento; (4) política de 2FA; (5) decidir con RT-05 y RT-14 por efecto; (6) aplicar y auditar |
| 6. Cambios indivisibles (**composición provisional hasta DB2a-2 y DB2a-3**) | Concesión del rol + **todas** las retiradas de asignaciones de editor en conflicto + anulación de un levantamiento de RT-14 de X en T, si existía (RF-ACC-17: se anula «si el rol de delegado de X se otorga de nuevo»; ver OC-10) + eventos de auditoría de la concesión y de **cada** retirada (CL-35). **Bajo D:** + actualización de la proyección (delegado vigente, asignaciones retiradas, restricción) |
| 7. Estados prohibidos | ES-03, ES-04, ES-08, ES-09, ES-10, ES-11 y, bajo D, ES-13. **Parcial:** concesión sin retiradas; retiradas sin concesión |
| 8. Concurrencia | **ESC-06:** asignación concurrente de U como editor en un partido de X: o la asignación se aplica primero y UI-02 la retira, o la concesión se aplica primero y la asignación se rechaza por RT-05; nunca ambas vigentes. **ESC-11** (aplica también a la concesión directa): concesión a U de dos equipos del mismo torneo: una prospera, la otra se rechaza. **ESC-10:** concesión y levantamiento concurrentes (ver UI-08). **ESC-07:** las operaciones sin conexión de U como editor en los partidos retirados se rechazan al sincronizar (CL-35): la sincronización pertenece a R2-B2-b, aquí solo se exige que la unidad deje el estado que esa verificación necesita |
| 9. Auditoría | Concesión y cada retirada auditadas dentro de la unidad (IF-10); rechazo fuera (IF-09) |
| 10. Abiertas | **OC-01** (composición con UI-12; DB2a-2); **OC-06** (A-2: alcance de las «asignaciones que generen conflictos»); **OC-10** (si la anulación del levantamiento pertenece a esta unidad: se documenta como [DER] por IF-14); **OC-05** (A-15 si el actor tiene conflicto propio). DB1-1 intacta |

#### 18.4.3 Ficha UI-03 — Reasignar un editor

| Campo | Contenido |
|---|---|
| 1. Disparador | Un dueño u organizador (del torneo del partido M) reasigna M del editor E1 al editor E2; puede coincidir con otra reasignación de M (CL-41) |
| 2. Componentes (R1 §5.5) | CMP-07 + CMP-08 + CMP-16 |
| 3. Requisitos | RF-ACC-06, RT-13, CL-41, CL-47; RT-05, RT-04, RT-14; IN-02-11, IN-02-22, IN-02-23 |
| 4. Autorización | **Servidor:** CMP-08 decide que el actor puede asignar en el torneo, que E2 tiene rol de editor vigente en T (IN-02-22), que E2 no es delegado de un equipo de M (RT-05), que el actor no asigna a sí mismo ni asigna editores en partidos de su propio equipo (RF-ACC-06, RT-14). **Capa de datos:** verifica lo mismo y que el estado resultante tiene **a lo sumo un** editor vigente en M («Cada partido tiene un único editor vigente.»: RF-ACC-06), de un rol de editor vigente del mismo torneo |
| 5. Secuencia lógica | (2) reunir la asignación vigente de M, los roles de E2, las asignaciones de delegado y el historial; (3) efecto: desactivar la de E1 y activar la de E2; (4) 2FA; (5) decidir; (6) aplicar y auditar |
| 6. Cambios indivisibles | Desactivación de la asignación de E1 + activación de la de E2 + evento(s) de auditoría. **Bajo D:** + proyección (asignaciones de editor vigentes de E1 y E2) |
| 7. Estados prohibidos | ES-01, ES-12 (**provisional**: ventana sin editor entre ambos cambios; OC-09), ES-04, ES-09, ES-10 y, bajo D, ES-13. **Parcial:** desactivar sin activar; activar sin desactivar |
| 8. Concurrencia | **ESC-04 (CL-41):** dos reasignaciones del mismo partido a la vez: **una prevalece; nunca dos editores vigentes; ambas intenciones quedan auditadas**. **ESC-05:** revocación del rol de editor de E2 concurrente: o E2 queda asignado antes de revocarse (y la revocación desactiva su asignación: UI-11) o la reasignación se rechaza. **ESC-06:** concesión del rol de delegado a E2 concurrente (RT-05). **ESC-07:** E1 sincroniza capturas hechas cuando aún estaba asignado: se rechazan (CL-47; frontera de R2-B2-b) |
| 9. Auditoría | Reasignación con valores antes y después dentro de la unidad (IF-10); «ambas intenciones quedan auditadas» (CL-41): la que no prevalece se audita como rechazo, fuera de la unidad (IF-09) |
| 10. Abiertas | **OC-08:** CL-41 dice que una prevalece y que ambas se auditan; este documento **no decide** si la segunda se rechaza por haber leído una asignación ya cambiada o se aplica en orden secuencial; ambas lecturas respetan IF-05 y el resultado final tiene un único editor. **OC-09:** si «único editor vigente» admite partidos sin editor (CL-35 lo produce); **OC-06** (A-2) |

#### 18.4.4 Ficha UI-04 — Designar o revocar un dueño con protección del último dueño activo

| Campo | Contenido |
|---|---|
| 1. Disparador | Un dueño designa a un segundo dueño o revoca a otro dueño. Las operaciones de dos dueños pueden coincidir (CL-43) |
| 2. Componentes (R1 §5.5) | CMP-07 + CMP-08 + CMP-16 |
| 3. Requisitos | RF-ACC-09, RT-06, CL-43, CL-38; IN-02-09, IN-02-10; IN-03-04 (nueva autenticación con 2FA; motivo en la revocación); decisión 12 de R2-A |
| 4. Autorización | **Servidor:** CMP-08 decide que el actor es dueño vigente (nunca un organizador), que el nivel de autenticación consumido satisface la nueva autenticación exigida (IF-12), que la revocación lleva motivo y que el resultado conserva al menos un dueño activo. **Capa de datos:** verifica el rol vigente del actor, el nivel de autenticación verificable (IN-02-25), el motivo y que **el estado resultante contiene al menos un dueño activo** |
| 5. Secuencia lógica | (2) reunir el conjunto de dueños activos, el rol del actor y su nivel de autenticación; (3) efecto sobre el conjunto de dueños; (4) política de 2FA; (5) decidir con RT-06 y RT-11; (6) aplicar cambio de rol, historial y auditoría con motivo |
| 6. Cambios indivisibles | Cambio del rol de dueño (alta o baja) + entrada de historial + evento de auditoría con motivo + **la comprobación del conjunto de dueños activos sobre el mismo conjunto en que se aplica** (IF-02, IF-07). **Bajo D:** + proyección (roles vigentes de la cuenta afectada, política de 2FA derivada) |
| 7. Estados prohibidos | ES-02, ES-09, ES-10 (un dueño ya revocado aplica una operación) y, bajo D, ES-13. **Parcial:** rol retirado sin historial o sin motivo; alta sin nueva autenticación |
| 8. Concurrencia | **ESC-01 (CL-43):** dos dueños se revocan mutuamente a la vez: **como máximo una revocación prospera y queda al menos un dueño activo**; el resultado equivale a una ordenación secuencial (la segunda, hecha por quien ya no es dueño, se rechaza). **ESC-02:** con tres o más dueños en revocaciones cruzadas, el resultado equivale a alguna ordenación secuencial y deja al menos un dueño; **no basta una comprobación por pares** (RB2a-10); **condicionado a OC-16** (si puede haber más de dos dueños). **ESC-03:** una designación hecha por un dueño al que otra revocación concurrente le quita el rol: aplicada antes y válida, o rechazada |
| 9. Auditoría | Designación y revocación auditadas con motivo en la revocación, dentro de la unidad (RF-AUD-02, IF-10); rechazo fuera (IF-09). La **invalidación de las sesiones** del dueño revocado (IN-03-08) es consecuencia posterior de CMP-06 y **no forma parte** de la unidad: el rechazo de sus nuevas operaciones **no depende** de ella (IN-03-06) |
| 10. Abiertas | **OC-03:** delimitación con UI-11 (**provisional**: revocar el rol de dueño es UI-04; la «degradación» de RT-06 y la **autorrevocación** de un dueño no están descritas en `01` como operaciones separadas: candidatas de §18.5). **OC-15** (qué es «dueño activo»); **OC-16** (máximo de dueños; ESC-02); **Decisión 12 de R2-A** (lectura de «nueva autenticación»). **K-3** |

#### 18.4.5 Ficha UI-08 — Levantar la restricción histórica de conflicto

| Campo | Contenido |
|---|---|
| 1. Disparador | Un dueño activo distinto del afectado aprueba el levantamiento de la restricción de RT-14 que alcanza a un dueño u organizador S respecto de un equipo X del torneo T |
| 2. Componentes (R1 §5.5) | CMP-07 + CMP-08 + CMP-16 |
| 3. Requisitos | RF-ACC-17 (prioridad S), RT-14, CL-60, CL-61; A-12; IN-02-12; RF-AUD-02 |
| 4. Autorización | **Servidor:** CMP-08 decide que el aprobador es **otro dueño activo y sin conflicto** respecto de X y T, que no hay autoaprobación, que hay motivo y que el alcance se limita a X y T «sin permisos adicionales». **Capa de datos:** verifica el rol y la ausencia de conflicto del aprobador **a partir del historial**, que el aprobador no es el afectado, el motivo y que el efecto registrado no excede a X y T. Sin dueño alterno la restricción permanece (CL-60) |
| 5. Secuencia lógica | (2) reunir roles, historial de delegaciones y levantamientos vigentes de S y del aprobador; (3) efecto: registrar el levantamiento; (4) 2FA del aprobador; (5) decidir (RT-14 por efecto sobre el propio aprobador); (6) aplicar y auditar con motivo |
| 6. Cambios indivisibles | Levantamiento (entrada en CMP-07, de solo anexado) + evento de auditoría con motivo y aprobador. **Bajo D:** + proyección (la restricción deja de estar vigente) |
| 7. Estados prohibidos | ES-07, ES-08, ES-09, ES-10 y, bajo D, ES-13. **Parcial:** levantamiento sin motivo o sin auditoría; levantamiento que afecta a otro equipo o torneo |
| 8. Concurrencia | **ESC-09 (CL-61):** levantamiento, revocación y acción del afectado simultáneos: resultado **serializado**; **nunca se aplica una acción con la restricción vigente y confirmada**. **ESC-10:** el rol de delegado de X se otorga de nuevo (UI-02/UI-12) a la vez que se levanta: el estado resultante equivale a una ordenación secuencial y nunca deja el levantamiento vigente tras la nueva concesión (ES-08). **ESC-17:** revocación concurrente del rol de dueño del aprobador (UI-04): el levantamiento aplicado antes es válido; si se aplica después, se rechaza |
| 9. Auditoría | Levantamiento con motivo y aprobador dentro de la unidad (IF-10). La autoaprobación y el intento sin dueño alterno son rechazos o bloqueos registrados **fuera** (IF-09) |
| 10. Abiertas | **Prioridad:** RF-ACC-17 es de prioridad **S** en `01`; si se difiriera, la restricción permanecería y OC-07, OC-10, ESC-10 y ESC-17 quedarían sin objeto (no se decide). **OC-07** (A-12: qué significa «sin conflicto» del aprobador y que el levantamiento no cubre el conflicto vigente); **OC-15** (el aprobador debe ser un «dueño activo»); **OC-10** (anulación al reotorgar el rol); **OC-05** (A-15); H2B1-04 (RT-14 en las dos capas). DB1-1 intacta |

#### 18.4.6 Ficha UI-11 — Revocar un rol o desasignar a un editor, serializado

| Campo | Contenido |
|---|---|
| 1. Disparador | Un dueño revoca a cualquier usuario; un organizador revoca editores y delegados exclusivamente de sus torneos, o las **asignaciones adicionales** que otro organizador posea en torneos comunes (revocación parcial, Q-38 d); desasignación de un editor de un partido. Coincide con operaciones en línea o sincronizadas del afectado |
| 2. Componentes (R1 §5.5) | CMP-07 + CMP-08 + CMP-13 + CMP-16 |
| 3. Requisitos | RF-ACC-07, RT-13, CL-45, CL-61, CL-53, OFF-22, D-2; RT-11; IN-02-07, IN-02-22; IN-03-06, IN-03-07 |
| 4. Autorización | **Servidor:** CMP-08 decide la facultad de revocar según la matriz (el organizador no revoca al dueño ni a otros organizadores salvo la revocación parcial de asignaciones adicionales) y calcula el efecto, incluidos los permisos que **permanecen** por otros roles (RT-11). **Capa de datos:** verifica la facultad del actor y el estado resultante: rol o asignación con estado y fecha de revocación, **sin asignaciones dependientes vigentes** (ES-11) y sin eliminar historial |
| 5. Secuencia lógica | (2) reunir roles del actor y del afectado, asignaciones, historial; (3) efecto: revocación + desactivación de las asignaciones que dependen del rol revocado; (4) 2FA; (5) decidir (A-15 como parámetro); (6) aplicar y auditar |
| 6. Cambios indivisibles | Revocación del rol o asignación + desactivación de las asignaciones dependientes (IF-14) + evento(s) de auditoría. **Y la propiedad de serialización:** toda operación del afectado que coincida con la revocación (en línea o sincronizada) se resuelve como aplicada antes o rechazada. **Bajo D:** + proyección (roles vigentes y asignaciones del afectado) |
| 7. Estados prohibidos | ES-10, ES-11, ES-09, ES-02 (si el rol es de dueño y OC-03 lo asigna a UI-04, la unidad es UI-04: **provisional**) y, bajo D, ES-13. **Parcial:** revocada pero la proyección aún concede (ES-13); revocada con editor aún asignado |
| 8. Concurrencia | **ESC-08 (CL-45):** revocación o desasignación al mismo tiempo que una operación en línea o una sincronización: **aplicada antes (válida y auditada) o rechazada; nunca a medias ni aplicada con la autorización ya revocada**. **ESC-14:** el revocador pierde su propia facultad a la vez que revoca: aplicada antes o rechazada. **ESC-15 (CL-53):** el afectado conserva los permisos de otros roles; una operación pendiente autorizada por otro rol se evalúa por el rol restante y, si ninguno la autoriza, se rechaza. **ESC-09 (CL-61)** como en UI-08 |
| 9. Auditoría | Revocación (y cada desactivación) auditada dentro de la unidad (IF-10); el rechazo de la operación del afectado se registra fuera (IF-09). Sesión conectada: reflejo en ≤ 60 s como **objetivo** sin garantía fuera de línea (IN-03-07); la verificación inmediata en el servidor no depende de él (IN-03-06) |
| 10. Abiertas | **OC-03** (delimitación con UI-04); **OC-15** (si el rol revocado es el de dueño, qué es «dueño activo»); **OC-04** («antes/después» de una operación sincronizada: OFF-22, D-2; se fija en R2-B2-b); **OC-05** (A-15: si un actor en conflicto puede revocar o desasignar cuando el efecto alcanza partidos de su equipo; **no se elige**); decisión 1 de R2-A (D-2); K-3 |

#### 18.4.7 Ficha UI-12 — Aceptar una invitación de delegado respetando un solo equipo por torneo

| Campo | Contenido |
|---|---|
| 1. Disparador | La persona invitada acepta una invitación de delegado del equipo X del torneo T; puede coincidir con otras invitaciones suyas del mismo torneo (CL-54) o con la misma invitación aceptada dos veces |
| 2. Componentes (R1 §5.5) | CMP-07 + CMP-10 + CMP-08 + CMP-16 |
| 3. Requisitos | RF-ACC-13, RF-ACC-02, CL-54, RF-ACC-01; IN-02-21, IN-02-24, IN-03-05 (invitación de un solo uso, vence y se acepta con el correo invitado; su rol y ámbito no cambian); RT-05 |
| 4. Autorización | **Servidor:** CMP-08 decide que la invitación está vigente, no usada, no vencida y es la del correo de quien acepta, que el ámbito aceptado es el emitido (sin ampliación) y que la persona no es ya delegada de otro equipo de T. **Capa de datos:** verifica el estado de la invitación (de su fuente, no de una proyección), la unicidad resultante (ES-03) y las referencias del mismo torneo |
| 5. Secuencia lógica | (2) reunir la invitación, las asignaciones de la persona en T, sus roles y el historial; (3) efecto: consumir la invitación, otorgar el rol, retirar asignaciones en conflicto; (4) 2FA si procede; (5) decidir; (6) aplicar y auditar |
| 6. Cambios indivisibles (**composición provisional hasta DB2a-2 y DB2a-3**) | Consumo de la invitación + concesión del rol de delegado + (si la persona tenía asignaciones de editor en partidos de X) las retiradas de UI-02 + anulación de un levantamiento de RT-14 si existía (OC-01, OC-10) + eventos de auditoría. **Bajo D:** + proyección (delegado vigente) |
| 7. Estados prohibidos | ES-06, ES-03, ES-04, ES-08, ES-09, ES-10 y, bajo D, ES-13 |
| 8. Concurrencia | **ESC-11 (CL-54):** dos invitaciones simultáneas harían a la misma persona delegada de dos equipos del mismo torneo: **una prospera y la otra se rechaza**; en torneos distintos ambas son válidas. **ESC-12:** la misma invitación aceptada dos veces a la vez: una sola aplicación (un solo uso). **ESC-13:** aceptación concurrente con el vencimiento, la revocación de la invitación o la revocación de quien invitó: aplicada antes o rechazada (efecto sobre la validez de una invitación emitida por quien después pierde la facultad: **no decidido**, OC-11) |
| 9. Auditoría | Aceptación y concesión auditadas dentro de la unidad (IF-10); rechazo (segundo equipo, invitación consumida) fuera (IF-09) |
| 10. Abiertas | **OC-01** (composición con UI-02); **OC-02** (creación de la cuenta, CMP-06, no figura entre los componentes de UI-12 y RF-ACC-01 exige invitación: si la aceptación crea la cuenta, esa creación pertenece a la unidad o a otra anterior); **OC-11** (invitación y revocación de quien invitó); DB2a-3 |

### 18.5 Operaciones que modifican permisos y no están en UI-01…13 (candidatas a la Fase 4)

R1 §5.5 declara la lista **no exhaustiva** y deja la enumeración definitiva por operación a la Fase 4. Las siguientes operaciones **modifican permisos o condicionan su concesión** y **no tienen unidad propia en UI-01…13**. El inventario se **concilia con H2B1-10 (R2-B1)**, que lista las que actualizarían la proyección de CAP-02-D: UI-01, 02, 03, 04, 08, 11 y 12 y, fuera de §5.5, «aceptar invitaciones de editor u organizador, designar organizadores, desactivar cuentas y asignar torneos existentes a un organizador» (CAND-02, CAND-04, CAND-10 y CAND-11). Se presentan **solo como candidatas**: **sin ficha**, sin añadir alcance y sin decidir si exigen una unidad. Su inclusión es una decisión del usuario (DB2a-7).

| ID | Operación candidata | Fuente | Relación con las unidades de §18.4 |
|---|---|---|---|
| CAND-01 | Emitir una invitación (fija ámbito y rol que luego se aceptan) | RF-ACC-02, RF-ACC-03; IN-03-05 | Condiciona UI-12; la emisión no concede el rol |
| CAND-02 | Aceptar una invitación de **organizador o de editor** (UI-12 solo cubre la de delegado) | RF-ACC-01, RF-ACC-02, RF-ACC-03; H2B1-10 | Mismos riesgos de un solo uso y de ámbito que UI-12 |
| CAND-03 | Revocar una invitación o su vencimiento | RF-ACC-02 | ESC-13 |
| CAND-04 | Designar o revocar organizadores (reservado al dueño) | RF-ACC-03, IN-02-09; H2B1-10 | Revocación: ¿UI-11 o unidad propia? (OC-03) |
| CAND-05 | Otorgar el rol de editor y la **primera** asignación de un editor a un partido (UI-03 cubre la reasignación) | RF-ACC-06 | ESC-19; mismo riesgo de ES-01 y ES-11 |
| CAND-06 | Degradar a un dueño y que un dueño se revoque a sí mismo | RT-06, RF-ACC-09 | OC-03; IF-07; GT-05 |
| CAND-07 | Revocación parcial entre organizadores, si no se considera cubierta por UI-11 | RF-ACC-07 (Q-38 d) | OC-03 |
| CAND-08 | Enrolamiento del segundo factor tras otorgar dueño u organizador a una cuenta sin él | RF-ACC-10, IN-03-03 | Condición de uso del rol, no concesión; relación con OC-15 |
| CAND-09 | Recuperación de cuenta y de la pérdida del segundo factor | RF-ACC-09 (procedimientos), IN-03-08 | Procedimientos; interactúa con IF-06 y OC-15 |
| CAND-10 | **Desactivar o eliminar una cuenta** con roles vigentes | H2B1-10 («desactivar cuentas»); RF-ACC-09 («eliminación») | Puede dejar sin dueño activo (ES-02) o dejar permisos vigentes de una cuenta inactiva (IF-06); IF-07 y GT-05 |
| CAND-11 | **Asignar torneos existentes a un organizador** | RF-ACC-03 («el dueño puede asignarle además torneos existentes»); H2B1-10 | Amplía el ámbito de un organizador: mismas exigencias de autorización en las dos capas, auditoría y aislamiento entre torneos (IF-03, IF-10, IF-13) |

**No son candidatas** las operaciones que modifican **estados** del torneo o del partido: cambian las entradas de la decisión, no los permisos, y bajo CAP-02-D no actualizan la proyección (§17.3).

#### 18.5.1 Operaciones conocidas que pueden alterar el conjunto de dueños activos

**Garantía de alcance global (GT-05, IF-07, IF-19).** Toda operación de la lista siguiente —y **toda otra que la Fase 4 enumere**— debe cumplir que **ninguna ejecución, aislada o concurrente con otras, deja al sistema sin un dueño activo autorizado.** Esta exigencia **no es exclusiva de UI-04**. El significado de «activo» (OC-15) y si existe un máximo de dueños (OC-16) están **abiertos**.

| Operación | Cómo puede alterar el conjunto de dueños | Estado |
|---|---|---|
| UI-04 (designar o revocar un dueño) | Alta o baja del rol de dueño | Ficha §18.4.4 |
| UI-11 (revocar un rol) | Si OC-03/DB2a-1 la usa para el rol de dueño | Ficha §18.4.6; delimitación abierta |
| CAND-06 (degradación, autorrevocación) | Baja o reducción del rol del propio actor | Candidata; ESC-22 |
| CAND-10 (desactivar o eliminar una cuenta) | Una cuenta de dueño deja de estar activa | Candidata; ESC-21 |
| CAND-09 y CAND-08 (recuperación; enrolamiento del segundo factor) | Una cuenta con rol de dueño puede no poder usarlo: depende de qué signifique «activo» | Candidatas; OC-15; ESC-23 |
| Operaciones aún no enumeradas | Cualquier otra que modifique o desactive el rol o la cuenta de un dueño | Fase 4 |

### 18.6 Matriz de concurrencia y revocaciones

#### 18.6.1 Escenarios de concurrencia (ESC-)

"Admisible" significa que **equivale a alguna ordenación secuencial** (IF-05). Las garantías GT- se definen en §18.7. **Ningún escenario se resuelve aquí por un mecanismo.** **Origen:** son **literales** en `01` ESC-01 (CL-43), ESC-04 (CL-41), ESC-07 (CL-35, CL-47), ESC-08 (CL-45), ESC-09 (CL-61), ESC-11 (CL-54, RF-ACC-02) y ESC-15 (CL-53); son **[DER]** (el texto de `01` no menciona esa simultaneidad) ESC-02, 03, 05, 06, 10, 12, 13, 14, 16, 17, 19, 21, 22 y 23; ESC-18 y ESC-20 son **propios de CAP-02-D**. ESC-02 depende de OC-16; ESC-21 de OC-15 y OC-16; ESC-22 de OC-03 y OC-15; ESC-23 de OC-15. **Ninguno define «dueño activo» ni el máximo de dueños.**

| ID | Escenario | Unidades | Origen | Resultados admisibles | Resultados prohibidos | Garantías |
|---|---|---|---|---|---|---|
| ESC-01 | Dos dueños se revocan mutuamente a la vez | UI-04 × UI-04 | CL-43, RT-06 | Una revocación prospera y la otra se rechaza (el revocador ya no es dueño); queda un dueño | Ambas aplicadas (ES-02); ambas rechazadas por error de ventana sin explicación auditable | GT-02, GT-03, GT-05 |
| ESC-02 | **Condicionado a OC-16 (máximo de dueños):** tres o más dueños con revocaciones cruzadas simultáneas | UI-04 × n | Derivado de CL-43 [DER] | El conjunto resultante equivale a una ordenación secuencial y tiene al menos un dueño | Un conjunto vacío porque cada comprobación miró solo a su pareja | GT-02, GT-03, GT-05 |
| ESC-03 | Un dueño designa a otro mientras una revocación concurrente le quita el rol | UI-04 × UI-04 | CL-38, RT-13 [DER] | Designación aplicada antes y válida, o rechazada | Designación aplicada por quien ya no era dueño (ES-10) | GT-02, GT-04 |
| ESC-04 | Dos reasignaciones del mismo partido | UI-03 × UI-03 | CL-41 | Una prevalece; ambas intenciones auditadas | Dos editores vigentes (ES-01); partido sin editor por ventana (ES-12, **provisional**: OC-09) | GT-02, GT-03 |
| ESC-05 | Reasignación al editor E2 mientras se revoca su rol de editor | UI-03 × UI-11 | RF-ACC-06, RF-ACC-07, IN-02-22 [DER] | Asignación aplicada antes y luego desactivada por la revocación, o reasignación rechazada | E2 asignado con rol de editor ya revocado (ES-10, ES-11) | GT-02, GT-04, GT-06 |
| ESC-06 | Asignación o reasignación de U como editor mientras se le otorga el rol de delegado del equipo del partido | UI-03 × UI-02 / UI-12 | RT-05, CL-35 | Asignación primero y retirada por UI-02, o concesión primero y asignación rechazada | U delegado y editor vigente en un partido de su equipo (ES-04) | GT-02, GT-03, GT-06 |
| ESC-07 | Sincronización de las operaciones de un editor tras retirarse su asignación por UI-02 o desactivarse por UI-03 (el editor anterior sincroniza capturas hechas cuando aún estaba asignado) | UI-02 / UI-03 × sincronización | CL-35, CL-47, OFF-22 | Rechazadas al sincronizar | Aplicadas con la asignación ya retirada (ES-10) | GT-04 (fronteras en R2-B2-b) |
| ESC-08 | Revocación o desasignación al mismo tiempo que una operación en línea o una sincronización | UI-11 × cualquier operación del afectado | CL-45, RF-ACC-07, RT-13 | Operación aplicada antes (válida y auditada) o rechazada | A medias; aplicada con la autorización ya revocada y confirmada (ES-10) | GT-02, GT-04 |
| ESC-09 | Levantamiento, revocación y acción del afectado simultáneos | UI-08 × UI-11 × acción | CL-61, RT-13, RT-14 | Resultado serializado | Acción aplicada con la restricción vigente y confirmada | GT-02, GT-03, GT-04 |
| ESC-10 | Levantamiento y reotorgamiento del rol de delegado de X simultáneos | UI-08 × UI-02 / UI-12 | RF-ACC-17, A-12 | Levantamiento primero y anulado por la concesión, o concesión primero y levantamiento rechazado o sin efecto sobre el conflicto vigente (A-12, abierta) | Levantamiento vigente tras la nueva concesión (ES-08) | GT-03, GT-06 |
| ESC-11 | Dos invitaciones simultáneas hacen a la misma persona delegada de dos equipos del mismo torneo | UI-12 × UI-12 (y UI-02 × UI-12) | CL-54, RF-ACC-13, RF-ACC-02 | Una prospera y la otra se rechaza; en torneos distintos, ambas | Dos equipos del mismo torneo (ES-03) | GT-02, GT-03 |
| ESC-12 | La misma invitación aceptada dos veces a la vez | UI-12 × UI-12 | RF-ACC-02, IN-03-05 | Una sola aplicación | Dos usos (ES-06) | GT-02, GT-03 |
| ESC-13 | Aceptación a la vez que el vencimiento o revocación de la invitación, o la revocación de quien invitó | UI-12 × CAND-03 / UI-11 | RF-ACC-02; OC-11 | Aceptada antes y válida, o rechazada | Rol otorgado con invitación ya vencida o revocada (ES-06) | GT-02, GT-04 |
| ESC-14 | El revocador pierde su propia facultad mientras revoca | UI-11 × UI-04 / UI-11 | RT-13 [DER] | Revocación aplicada antes y válida, o rechazada | Revocación aplicada con la facultad ya revocada (ES-10) | GT-02, GT-04 |
| ESC-15 | Revocación parcial: el afectado conserva permisos de otros roles; una operación pendiente depende de otro rol | UI-11 × operación | CL-53, RT-11 | Se evalúa por el rol restante; sin ninguno que la autorice, se rechaza | Rechazar lo que otro rol vigente autoriza, o autorizar lo que ninguno autoriza | GT-04 |
| ESC-16 | Se revoca el rol del **creador organizador** mientras crea el torneo (el caso del dueño: OC-17) | UI-01 × UI-11 / UI-04 | RF-ACC-12, RT-13 [DER] | Creación aplicada antes (el torneo y la asignación del creador organizador) o rechazada; si se aplicó antes, una revocación posterior alcanza también la asignación creada por la unidad | Torneo sin la asignación del creador organizador, o asignación sin torneo (ES-05); una revocación que deja vigente la asignación recién creada (ES-11) | GT-01, GT-02, GT-06 |
| ESC-17 | El aprobador de un levantamiento pierde el rol de dueño mientras aprueba | UI-08 × UI-04 | RF-ACC-17, RT-13 [DER] | Levantamiento aplicado antes y válido, o rechazado | Levantamiento aprobado por quien ya no es dueño (ES-07) | GT-02, GT-04 |
| ESC-18 | (**solo CAP-02-D**) Una decisión coincide con el mantenimiento de la proyección de un cambio de permisos | cualquier operación × unidad de §18.4 | K-2d, IN-02-02 | La decisión ve el estado anterior completo o el posterior completo | Ver un estado que ya no refleja un cambio confirmado (ES-13) | GT-09, GT-10 |
| ESC-19 | **Primera asignación de editor** a un partido concurrente con otra asignación inicial del mismo partido, con la revocación del rol de editor de E o con la concesión del rol de delegado del equipo del partido a E | CAND-05 × CAND-05 / UI-11 / UI-02 / UI-12 | RF-ACC-06, RT-05 [DER] | La asignación se aplica antes y la otra operación la alcanza (desactivación o retirada), o se rechaza | Dos editores vigentes (ES-01); asignación vigente de un rol revocado (ES-10, ES-11); delegado y editor en un partido de su equipo (ES-04) | GT-02, GT-03, GT-06 |
| ESC-20 | (**solo CAP-02-D**) Una decisión coincide con la **reconstrucción** de la proyección desde las fuentes | cualquier operación × reconstrucción | K-2b, K-2d, IN-02-02 [DER] | La decisión ve la proyección anterior completa o la reconstruida completa, o se resuelve sobre las fuentes (g4, [REC], no adoptada) | Decidir sobre una proyección parcial o discrepante con las fuentes (ES-13) | GT-09, GT-10 |
| ESC-21 | Desactivación de la cuenta de un dueño concurrente con la revocación de otro dueño, o con otras desactivaciones (**depende de OC-15 y, con tres o más dueños, de OC-16**) | CAND-10 × UI-04 / CAND-10 | RT-06, RF-ACC-09 («eliminación o degradación»), CL-43 [DER] | El resultado equivale a una ordenación secuencial y deja **al menos un** dueño activo autorizado: **pueden prosperar varias** desactivaciones o revocaciones mientras quede otro dueño; las que dejarían al sistema sin dueño se rechazan | Sin dueño activo autorizado (ES-02); rechazar sin necesidad una operación que deja al menos un dueño | GT-02, GT-03, GT-05 |
| ESC-22 | Dos dueños se autorrevocan o se degradan a la vez, o uno de ellos se degrada mientras el otro lo revoca (CAND-06) (**depende de OC-03 y OC-15**) | CAND-06 × CAND-06 / UI-04 | RT-06, RF-ACC-09 («eliminación o degradación»), CL-43 [DER] | Equivale a una ordenación secuencial; queda al menos un dueño activo autorizado | Sin dueño activo autorizado (ES-02) | GT-02, GT-03, GT-05 |
| ESC-23 | Un enrolamiento del segundo factor o una recuperación de cuenta (CAND-08, CAND-09) pendiente de un dueño coincide con la revocación o desactivación de otro dueño (**depende de OC-15**: si «activo» exige segundo factor enrolado u otra condición) | CAND-08 / CAND-09 × UI-04 / CAND-10 | RF-ACC-10, IN-03-03, IN-03-08, RT-06 [DER] | Equivale a una ordenación secuencial; el resultado deja al menos un dueño activo **en el sentido que OC-15 fije**, que este documento no fija | Sin dueño activo autorizado (ES-02) bajo cualquier lectura de OC-15 que el usuario admita | GT-02, GT-03, GT-05 |

#### 18.6.2 Revocaciones y operaciones con permisos obsoletos

Por cada clase de revocación se declara qué debe dejar de ser posible **desde la confirmación**, qué permanece y qué queda fuera. El tiempo de reflejo en sesiones conectadas (≤ 60 s) es un **objetivo** sin garantía para dispositivos desconectados y **no puede medirse en la Fase 2** (IN-03-07).

| Revocación | Unidad | Desde la confirmación deja de ser posible | Permanece | Fuera de esta parte |
|---|---|---|---|---|
| Rol de dueño | UI-04 (**provisional**: OC-03) | Toda operación nueva del revocado que exija ese rol; sus designaciones y revocaciones pendientes se aplican antes o se rechazan | Sus permisos por otros roles vigentes (RT-11); al menos un dueño (RT-06) | Invalidación de sesiones (CMP-06); sincronización de sus dispositivos |
| Rol de organizador | CAND-04 / UI-11 (OC-03) | Operaciones sobre sus torneos | Permisos por otros roles | Sincronización |
| Rol de editor y sus asignaciones | UI-11 | Capturas y operaciones nuevas del editor en los partidos desasignados; las desasignaciones están en la misma unidad (ES-11) | Permisos por otros roles (CL-53) | Qué ocurre con lo capturado sin conexión: CL-47, OFF-10, OFF-11 (R2-B2-b) |
| Rol de delegado | UI-11 | Administración de la plantilla del equipo | Permisos por otros roles | Restricción histórica de RT-14: **persiste** como historial (RT-14) |
| Asignación adicional de otro organizador (revocación parcial) | UI-11 | Esa asignación adicional | Su rol de organizador y los permisos de sus otros roles (RF-ACC-07) | A-15 |
| Levantamiento de la restricción | (anulación) UI-02 / UI-12 | Operaciones permitidas solo por el levantamiento | El historial | OC-10 |
| Invitación | CAND-03 | Aceptar esa invitación | Cuentas ya creadas | OC-11 |
| Desactivación o eliminación de una cuenta | CAND-10 (sin ficha) | Toda operación nueva de esa cuenta; si es de un dueño, solo es posible si queda otro dueño activo (GT-05) | El historial (IN-09-02) | Sesiones (CMP-06); OC-15; enumeración de la Fase 4 |

**Regla común.** Una operación sin conexión o en tránsito se evalúa con la autorización vigente **cuando se aplica**, y lo que no se aplica se rechaza (IF-06). La definición precisa de «antes» y «después» para operaciones capturadas sin conexión (OFF-22, D-2) **no se fija aquí** (OC-04, R2-B2-b).

### 18.7 Análisis de K-2d: garantías transaccionales exigidas

K-2d (§17.3, R2-B1) **solo** enuncia propiedades y corresponde a R2-B2. Esta sección las desarrolla **a nivel de capacidad** para las siete unidades de permisos. **K-2d se describe como garantía arquitectónica exigida: no se implementa, no se aprueba y no se declara cumplida.** Las garantías GT-09 y GT-10 solo aplican a CAP-02-D. **GT-07 y GT-08 son transversales:** se aplican a **todas** las filas de §18.6.1 y no se ligan a un escenario concreto.

| ID | Garantía exigida | Unidades | Fuente | Aplica a |
|---|---|---|---|---|
| GT-01 | **Misma unidad:** el cambio en la fuente única (CMP-07), los efectos compuestos, el evento de auditoría de lo aplicado y, bajo D, la actualización de la proyección se confirman juntos o ninguno | Las siete | IF-01, IF-10, IF-14, K-2c | A y D (la proyección, solo D) |
| GT-02 | **Lo decidido es lo aplicado:** la decisión y la aplicación de una unidad no admiten un cambio confirmado de los hechos que la decisión usó (bajo D, incluida la lectura de la proyección: primera cláusula de K-2d) | Las siete | IF-02, RT-13 | A y D |
| GT-03 | **Serialización de las unidades que compiten por un mismo hecho o conjunto:** dueños activos, editor de un partido, delegado de un torneo, invitación, restricción | UI-02, 03, 04, 08, 12 | IF-05, IF-07, CL-41, CL-43, CL-54 | A y D |
| GT-04 | **Revocación efectiva frente a operaciones en línea y sincronizadas:** aplicada antes y válida, o rechazada | UI-11 y toda unidad cuyo actor puede perder su facultad | IF-06, CL-45, CL-61, OFF-22 | A y D |
| GT-05 | **Garantía de alcance global sobre el último dueño:** **ninguna operación, aislada o concurrente con otras, deja al sistema sin un dueño activo autorizado;** el conjunto de dueños se comprueba sobre el conjunto en que se aplica, no sobre una copia anterior ni por pares. Aplica a **toda** operación de §18.5.1 y a toda otra que altere ese conjunto; **no es exclusiva de UI-04** | UI-04; UI-11 (si OC-03 la incluye); CAND-06, 08, 09, 10; las no enumeradas | IF-07, IF-19, RT-06, RF-ACC-09, CL-43 | A y D |
| GT-06 | **Efectos compuestos completos:** retiradas, desactivaciones de asignaciones dependientes y anulaciones de levantamientos se confirman con su causa | UI-02, 11, 12 (y UI-08 como contraparte) | IF-14, CL-35, RF-ACC-17 | A y D |
| GT-07 | **Todo o nada con la auditoría:** si el evento de lo aplicado no puede registrarse, no se aplica; el registro de un rechazo es independiente y persiste (K-3) | Las siete | IF-09, IF-10, IN-09-04, IN-09-05 | A y D |
| GT-08 | **Las dos capas deciden sobre el mismo estado confirmado:** no existe un momento en que el servidor y la capa de datos vean estados distintos de los hechos de la decisión | Las siete | IF-03, K-1, RB1-01 | A y D |
| GT-09 | **Sin ventana con proyección obsoleta:** la confirmación del cambio de permisos y la de la proyección derivada son un solo hecho; ninguna decisión lee un estado intermedio (ES-13) | Las siete | K-2d, ES-13 | **Solo D** |
| GT-10 | **La proyección se reconstruye desde las fuentes** y, ante una discrepancia, **prevalecen las fuentes** (g4, [REC] de R2-B1: no fija `01` ni R1) | Las siete | K-2b, §17.3.5 (g3, g4); ESC-18, ESC-20 | **Solo D** |

**Qué demuestra esta sección.** Que, para las siete unidades, K-2d puede **enunciarse** como un conjunto completo de propiedades verificables en los escenarios de §18.6.1. **Qué no demuestra:** que exista una forma de lograrlas, ni su suficiencia sobre las unidades de R2-B2-b (UI-06, UI-10) ni sobre las candidatas de §18.5. La demostración de K-2b y la composición con R2-B2-b siguen **abiertas**. La demostración definitiva de K-2d requiere R3 (mecanismo, condicionado a ADR-01) y la verificación de la Fase 4.

#### 18.7.1 Trazabilidad K-2 → GT → ESC y lo no demostrado

Solo **CAP-02-D** incluye K-2. **Nada de lo siguiente está implementado ni demostrado.**

| Componente de K-2 | Qué exige | GT | ESC | Dónde se trata | Qué NO está demostrado |
|---|---|---|---|---|---|
| **K-2a** Completitud | La proyección contiene los hechos derivados de la decisión y **no** el estado, las reglas de integridad ni la invitación vigente | (no es una garantía transaccional: no tiene GT) | — | §18.8.1 (hechos por unidad) | La completitud **no está comprobada**: §18.8.1 inventaría los hechos de las siete unidades, pero no cubre las candidatas de §18.5 ni las operaciones aún no enumeradas |
| **K-2b** Derivación | Deriva de CMP-07 con la definición única de CMP-08; se reconstruye desde las fuentes; no es un segundo decisor | GT-10, GT-08 | ESC-18, ESC-20 | §18.7, §18.8.2 | La **reconstruibilidad**, la adopción de g4 ([REC]) y la lectura de R1 §5.3 (P-1 o P-2, DB1-6) |
| **K-2c** Actualización coherente | Se actualiza con **toda** operación que modifica permisos | GT-01, GT-06 | ESC-05, ESC-06, ESC-10, ESC-16, ESC-19 | §18.4, §18.5 | La **enumeración definitiva** de las operaciones (Fase 4); las candidatas no tienen ficha |
| **K-2d** Garantías transaccionales | Lectura de la proyección y aplicación en la misma unidad, actualización con cada cambio de permisos y sin ventana con proyección obsoleta | GT-02, GT-01, GT-09 (con GT-03, GT-04, GT-05) | ESC-01…23 | §18.4, §18.6, §18.7 | Su suficiencia sobre UI-06 y UI-10 (R2-B2-b), el mecanismo (R3, condicionado a ADR-01) y la verificación (Fase 4) |

### 18.8 Evaluación de CAP-02-A frente a CAP-02-D sobre las unidades de permisos

**Alcance y reserva.** Esta evaluación (corregida en la v0.2, A-04 y A-05, y en la v0.3, B-01) aporta **evidencia** para DB1-1. **No elige** CAP-02-A ni CAP-02-D, **no declara demostrada la independencia de CAP-02-D** y **no declara cumplido R1 §8.1** («≥ 2 formas de reparto con ambas capas siempre presentes»). DB1-1 y DB1-6 siguen abiertas.

#### 18.8.1 Reglas de integridad, hechos derivados y garantías que los vinculan

**Tres categorías (K-2a, §17.3.5; no se modifican K-2a ni las matrices de R2-B1).**

1. **Reglas de integridad verificadas sobre las fuentes**, tanto bajo A como bajo D: último dueño activo (IN-02-10), un equipo por delegado y torneo (IN-02-21), un editor vigente por partido (IN-02-22), la invitación vigente (IN-02-24) y la **indivisibilidad** de la creación del torneo y de su asignación («si la asignación falla, el torneo no se crea», IN-02-26). K-2a las excluye de la proyección.
2. **Hechos derivados que CAP-02-D obtiene de su proyección:** los de las entradas de IN-02-06, 12, 15, 23 y 26 (K-2a): roles vigentes por ámbito (también **la facultad del actor**), delegado vigente, **asignaciones de editor vigentes**, restricciones de RT-14 no levantadas, política efectiva de segundo factor y **la asignación vigente del creador de un torneo** (IN-02-26, que la matriz de §17.3 marca «Condicionada (K-2)» en D).
3. **Garantías de atomicidad que vinculan ambos elementos:** GT-01 (un solo cambio confirmado), GT-02 (lo decidido es lo aplicado), GT-03 (competencia por un mismo hecho), GT-05 (último dueño), GT-06 (efectos compuestos completos) y GT-09 (sin proyección obsoleta). **Una garantía puede apoyarse a la vez en una regla de integridad (sobre las fuentes) y en un hecho derivado (bajo D)**: por eso no se afirma que una unidad se compruebe "exclusivamente sobre las fuentes" cuando consulta además hechos derivados.

| Unidad | Reglas de integridad e invitación (sobre las fuentes, A y D) | Hechos derivados (bajo D, de la proyección) | Otros leídos de su fuente | Garantías que vinculan ambos | ES-13 bajo D | Qué añade D |
|---|---|---|---|---|---|---|
| UI-01 | Creación indivisible del torneo y de la asignación (IN-02-26, parte de integridad) | Asignación vigente del creador organizador (IN-02-26); roles vigentes del actor; política de 2FA | Estado del torneo, si procede | GT-01 (torneo y asignación juntos) y GT-09 (la consulta de la asignación vigente refleja lo confirmado) | **Aplica** (facultad del actor; asignación vigente) | Una escritura de mantenimiento en la unidad |
| UI-02 | Un equipo por delegado y torneo (IN-02-21) | Roles de U; **asignaciones de editor vigentes de U** (de ellas se calcula el conjunto a retirar y el estado de ES-04); restricciones de RT-14 no levantadas; delegado vigente; facultad del actor | Pertenencia de los partidos a los equipos; estado del partido | GT-03 (unicidad, **sobre las fuentes**); GT-06 (retiradas completas: **el conjunto a retirar se calcula desde un hecho derivado bajo D y el estado resultante se verifica sobre las fuentes**); GT-09 | **Aplica** (asignaciones vigentes; facultad del actor) | Mantenimiento de tres hechos derivados |
| UI-03 | Un editor vigente por partido (IN-02-22) | Rol de editor vigente de E2; delegaciones vigentes de E2 (RT-05); restricciones del actor; facultad del actor | Equipos y estado del partido M | GT-03 (único editor, **sobre las fuentes**) y GT-09 | **Aplica** (rol de E2, delegaciones, facultad del actor) | Mantenimiento de las asignaciones de editor vigentes de E1 y E2 |
| UI-04 | **Último dueño activo (IN-02-10)**, leído de la fuente (K-2a). Una variante que lo leyera de la proyección **contradiría K-2a** y no se evalúa | **Facultad del actor** (su rol de dueño vigente), roles vigentes de la cuenta afectada, política de 2FA | Nivel de autenticación (CMP-06); motivo | GT-05 (sobre la fuente) y GT-09 (la facultad del actor) | **Aplica** a la facultad del actor: un dueño ya revocado no debe operar con una proyección obsoleta | Mantenimiento de los roles vigentes |
| UI-08 | Ninguna regla de integridad atómica propia | Restricciones de RT-14 no levantadas; roles vigentes del aprobador y del afectado | Historial (fuente única); motivo | GT-02 y GT-04 | **Aplica** (restricción y rol del aprobador) | Mantenimiento de la restricción levantada |
| UI-11 | Ninguna propia (si el rol es de dueño y OC-03 lo asigna a UI-04: último dueño, §18.4.4) | Roles y asignaciones del afectado; facultad del actor | — | GT-04 y GT-06 (desactivaciones) | **Aplica de forma directa:** la revocación confirmada debe reflejarse en la proyección en la misma unidad | Mantenimiento de roles y asignaciones |
| UI-12 | Un equipo por delegado y torneo (IN-02-21); **invitación vigente y de un solo uso** (IN-02-24) | Asignaciones de editor vigentes y restricciones de la persona (si compone UI-02); delegado vigente; facultad de quien acepta | La invitación (por definición de D) | GT-03 (sobre las fuentes), GT-06 (si compone UI-02) y GT-09 | **Aplica** (asignaciones vigentes; delegado vigente) | Mantenimiento del delegado vigente |

#### 18.8.1.1 Alcance de ES-13

ES-13 (decisión sobre una proyección que no refleja un cambio de permisos ya confirmado) **aplica bajo CAP-02-D a toda operación que consume hechos derivados**, no solo a las que modifican permisos:

| Operaciones que consumen hechos derivados | Qué consumen | Estado |
|---|---|---|
| Las siete unidades de §18.4 | Facultad del actor y, según la unidad, asignaciones, restricciones o delegado vigentes (tabla de §18.8.1) | Analizadas |
| Candidatas CAND-01…11 de §18.5 | La facultad del actor y, en las que modifican permisos, los roles y asignaciones que cambian | Sin ficha; ES-13 aplicaría |
| Operaciones que modifican **estados** (no actualizan la proyección, K-2c) | La facultad del actor (rol vigente), que sí es un hecho derivado | Identificadas; **R2-B2-b** (UI-05, 06, 07, 09, 10, 13) |
| Operaciones aún no enumeradas | Cualquier decisión de CMP-08 que lea roles, asignaciones o restricciones | Fase 4 |

La **única** excepción son las lecturas que la proyección no contiene (estado del torneo y del partido, reglas de integridad, invitación vigente, nivel de la sesión), que se leen de su fuente. Este alcance es **compatible con las dos interpretaciones** de CAP-02-D: no prejuzga si D es una variante de A (II) o una forma distinta (I).

#### 18.8.2 Responsabilidades de CMP-07 y CMP-08 en las unidades

| Aspecto | Bajo A | Bajo D con P-1 | Bajo D con P-2 | Dependencia |
|---|---|---|---|---|
| Quién decide (en todas) | CMP-08 | CMP-08 | CMP-08 | CR1-13, K-1 |
| Quién aplica el cambio en la fuente | CMP-07 | CMP-07 | CMP-07 | R1 §5.3 |
| Quién calcula y conserva los hechos derivados | — | CMP-07, ejecutando la definición única de CMP-08 (AC-1 ya hace que CMP-07 invoque a CMP-08) | CMP-08 (exige leer «no los almacena» como referido a roles e historial) | **DB1-6** (confirmación de la lectura); **R1 no se modifica** |
| Quién lee en la decisión | CMP-08 lee de CMP-07 | CMP-08 lee la proyección (y la fuente para estado, integridad e invitación) | Ídem | GT-08 |
| Fallo propio | — | Proyección obsoleta (ES-13) | Ídem | GT-09, GT-10 |

#### 18.8.3 Evidencia registrada (sin conclusión sobre la elección)

| ID | Observación | Alcance | Naturaleza |
|---|---|---|---|
| EV-01 | Las garantías atómicas que protegen **reglas de integridad** —último dueño (UI-04), único editor (UI-03), un equipo por delegado y torneo (UI-02, UI-12), invitación de un solo uso (UI-12) y la indivisibilidad de torneo y asignación (UI-01)— **versan sobre las fuentes** y se exigen igual bajo A y D, porque K-2a las excluye de la proyección. **Pero en las siete unidades la decisión consume además al menos un hecho derivado** (como mínimo la facultad del actor; en varias, asignaciones o restricciones vigentes), de modo que **bajo D todas quedan expuestas a ES-13** (§18.8.1.1) | Las siete | Aplicación de K-2a a las unidades |
| EV-02 | **Propiedad ya conocida, no evidencia nueva:** bajo D las siete unidades modifican permisos y exigen el paso de mantenimiento dentro de la unidad (K-2c, H2B1-10; interpretación I de §17.3.3) | Las siete | Ya conocida |
| EV-03 | El modo de fallo propio de D (proyección obsoleta, ES-13) y sus obligaciones (GT-09, GT-10, K-2b) **ya estaban enunciados en K-2d**; lo que añade el análisis es su **alcance:** afecta a **toda operación que consume hechos derivados** (las siete unidades, las candidatas y las de R2-B2-b), con consecuencia más directa donde el hecho derivado condiciona una revocación o una restricción vigente (UI-11, UI-08), **sin limitarse a ellas** | Las siete; candidatas; R2-B2-b | Ya enunciada; alcance precisado |
| EV-04 | Bajo D siguen leyéndose de la fuente las reglas de integridad atómicas (UI-02, 03, 04, 12), la invitación (UI-12) y el estado (p. ej. el del partido en UI-03): D no sustituye la lectura de las fuentes | UI-02, 03, 04, 12 | Aplicación de K-2a |
| EV-05 | **Mixta.** (i) Si se adoptara g4 («prevalecen las fuentes», [REC], no adoptada), la decisión ante una discrepancia se tomaría **sobre las fuentes**, lo que **acerca** D a A con una copia derivada (**apoya la interpretación II**). (ii) Sin g4, D tiene un modo de fallo que A no tiene (**apoya la interpretación I**) | Las siete (más visible en UI-08 y UI-11) | Mixta |
| EV-06 | Los resultados observables de A y D **coinciden** cuando la proyección está al día; **difieren** solo ante una proyección obsoleta o ante la necesidad de reconstruirla | Las siete | Análisis |
| EV-07 | El análisis **no evalúa rendimiento ni lecturas masivas**, que son lo que motivaría una proyección; esa evaluación depende de decisiones fuera de esta parte | — | Límite |
| EV-08 | **Contraste con el criterio de independencia de §17.3.3,** que exige «que D cambie qué verifica cada capa o la composición de las unidades atómicas» de modo que no se reduzca a "A con una entrada derivada". (a) *Qué verifica cada capa:* bajo A y D ambas capas verifican la misma decisión completa (IF-03, IF-16); cambia la representación de las entradas, **no lo que verifica cada capa**. (b) *Composición de las unidades:* las siete conservan sus cambios indivisibles y D les añade un paso de mantenimiento (EV-02). Si ese añadido cuenta como cambio de composición y si el resultado "se reduce a A con una entrada derivada" **lo decide el usuario en DB1-1**, como dice §17.3.3. **No se concluye** | Las siete | Contraste |

**Lectura de la evidencia (sin elegir).** EV-01 (en lo que toca a las reglas de integridad), EV-04, EV-06 y EV-08 (a) son coherentes con la **interpretación II** de §17.3.3 («D es una variante de A»): misma decisión, mismas dos capas, mismas unidades; difiere la representación de las entradas. EV-03, EV-05 (ii) y EV-08 (b) muestran obligaciones y modos de fallo **propios** de D, coherentes con la **interpretación I**; EV-05 (i) apoya la II. **La evidencia no zanja la cuestión y no aporta evidencia nueva sobre el criterio de §17.3.3 más allá de EV-01, EV-03 y EV-08.** Se mantienen **las dos interpretaciones**. La independencia de D respecto de A **sigue sin estar demostrada**, D **no se cuenta como segunda forma**, **R1 §8.1 no se declara cumplido** y **DB1-1 sigue abierta** (con las vías de §17.8).

### 18.9 Dependencias y decisiones abiertas

#### 18.9.1 Condiciones abiertas de R2-B2-a (OC-)

| ID | Condición abierta | Unidades | Relación |
|---|---|---|---|
| OC-01 | Relación entre UI-02 y UI-12: si la aceptación de la invitación de delegado incluye los efectos de UI-02 (retiradas, anulación) en **una** unidad, o si el rol de delegado puede otorgarse por otros medios | UI-02, UI-12 | IF-14, DB2a-2 |
| OC-02 | La creación de la cuenta (CMP-06) no figura entre los componentes de UI-12 y RF-ACC-01 exige invitación | UI-12 | DB2a-2 |
| OC-03 | Delimitación entre UI-04 y UI-11 para el rol de dueño; degradación y autorrevocación de RT-06; y si la **revocación de organizadores y la revocación parcial entre organizadores** (CAND-04, CAND-07) son UI-11 o unidades propias | UI-04, UI-11 | DB2a-1, CAND-04, CAND-06, CAND-07 |
| OC-04 | «Antes» y «después» de una operación sincronizada (OFF-22, D-2) | UI-11 y R2-B2-b | Decisión 1 de R2-A; **reservado a R2-B2-b** |
| OC-05 | A-15: si un actor en conflicto puede revocar o desasignar cuando el efecto alcanza partidos de su propio equipo | UI-02, 03, 08, 11 | IN-02-17; **no se elige** |
| OC-06 | A-2: alcance de las «asignaciones que generen conflictos» | UI-02, 03 | IN-02-17 |
| OC-07 | A-12: «sin conflicto» del aprobador; el levantamiento y el reotorgamiento | UI-08 | IN-02-17 |
| OC-08 | CL-41: si la reasignación que no prevalece se rechaza por haber leído una asignación ya cambiada o se aplica en orden secuencial | UI-03 | DB2a-4 |
| OC-09 | Si «único editor vigente» admite partidos sin editor (CL-35 lo produce) y si ES-12 es un estado prohibido | UI-02, 03, 11 | DB2a-4 |
| OC-10 | Si la anulación del levantamiento al reotorgar el rol forma parte de las unidades UI-02 y UI-12 (IF-14 [DER]) | UI-02, 08, 12 | DB2a-3 |
| OC-11 | Validez de una invitación tras revocarse a quien invitó, o tras revocarse la invitación | UI-12, CAND-03 | DB2a-5 |
| OC-12 | Duplicación de una solicitud de creación de torneo (reintento) | UI-01 | Idempotencia: R2-B2-b / Fase 4 |
| OC-13 | Persistencia del rechazo (K-3) en las unidades de permisos | Las siete | Decisión 10 de R2-A; CAP-09-P |
| OC-14 | Cómo se distingue la asignación del sistema de una autoasignación | UI-01 | R3 |
| OC-15 | **Significado de «dueño activo»** (RT-06, RF-ACC-09): rol vigente, cuenta activa, segundo factor enrolado u otra lectura; condiciona IF-07, GT-05, CAND-08, 09 y 10 | UI-04, UI-11 y candidatas de §18.5.1 | DB2a-9; **no se define aquí** |
| OC-16 | **Si existe un máximo de dueños.** `01` habla de «un segundo dueño» y no declara un máximo; **no se supone que solo puedan existir dos** (ESC-02 y ESC-21 dependen de la respuesta) | UI-04 | DB2a-8 |
| OC-17 | Asignación al crear un torneo cuando el creador es **dueño** (RF-ACC-12 define la asignación automática solo para el organizador; **no se amplía RF-ACC-12**) | UI-01 | DB2a-10 |

#### 18.9.2 Decisiones propuestas al usuario (DB2a-)

Ninguna se resuelve aquí. Las DB1-1…DB1-6 y las decisiones de R2-A (13 originales, la 11 atendida y 12 abiertas) permanecen **como estaban**.

| ID | Decisión | Opciones | Efecto |
|---|---|---|---|
| DB2a-1 | Delimitación UI-04 / UI-11 (OC-03) | (a) UI-04 abarca toda alta o baja del rol de dueño, incluidas degradación y autorrevocación, y UI-11 el resto; (b) diferirlo a la enumeración de la Fase 4 conservando IF-07 | Fija la frontera de la protección del último dueño |
| DB2a-2 | Composición UI-02 / UI-12 y creación de cuenta (OC-01, OC-02) | (a) **una** unidad compuesta con todos los efectos; (b) diferirlo a la Fase 4 con la condición IF-14 | Evita una ventana entre causa y consecuencia |
| DB2a-3 | Anulación del levantamiento dentro de la unidad (OC-10) | (a) incluirla en UI-02 y UI-12; (b) diferir su ubicación a la enumeración de la Fase 4 **conservando ES-08 (RF-ACC-17)**. *Se retiró la opción de dejarla como operación separada con una ventana, porque contradice ES-08* | Define la unidad de la contraparte de UI-08 |
| DB2a-4 | Semántica de la reasignación obsoleta y del partido sin editor (OC-08, OC-09) | Las lecturas de §18.4.3 | Fija el resultado de CL-41 |
| DB2a-5 | Validez de una invitación tras revocar al invitador (OC-11) | Seguir vigente / dejar de serlo / decidir en la Fase 4; **toda opción debe respetar IN-02-08** (el ámbito de una invitación nunca excede el de quien invita) | Fija el ámbito de UI-12 |
| DB2a-6 | Recibir §18.8.3 como **evidencia** para DB1-1 sin resolverla | Recibir / pedir más análisis | No modifica DB1-1 |
| DB2a-7 | Candidatas de §18.5 para la enumeración de la Fase 4 | Aceptar la lista / ampliarla / reducirla | Alcance de la Fase 4 |
| DB2a-8 | Máximo de dueños (OC-16) | No hay máximo / hay un máximo (a fijar por el usuario) / decidir en la Fase 4 | Acota ESC-02 y ESC-21 |
| DB2a-9 | Significado de «dueño activo» (OC-15) | Las lecturas de OC-15 / decidir en la Fase 4 | Fija el alcance de GT-05 |
| DB2a-10 | Asignación al crear un torneo cuando el creador es dueño (OC-17) | Sin asignación automática (la del dueño ya es global) / otra lectura de RF-ACC-12 (con Q-02a) / decidir en la Fase 4 | Delimita ES-05 y UI-01; **no modifica RF-ACC-12 por sí mismo** |

#### 18.9.3 Dependencias

| Depende de | Qué |
|---|---|
| R2-B2-b | UI-06 (sincronización) para ESC-07, ESC-08 y OC-04; UI-10 y UI-13; UI-09 y su relación con ES-04; **las amenazas AM-18 y CAB-05** y, junto con R2-C, **AM-19 y AM-34** (asignadas por R2-A a R2-B; §18.12.1; no se desarrollan aquí); validación conjunta de K-2d |
| R2-A | Decisión 1 (D-2), decisión 10 (K-3), decisión 12 (nueva autenticación), CAP-09-P |
| R2-B1 | DB1-1 (déficit frente a R1 §8.1), DB1-6 (lectura de R1 §5.3), H2B1-04 (RT-14 en las dos capas), K-1…K-3 |
| R3 | Mecanismos de las garantías GT-01…GT-10 (condicionados a ADR-01) |
| Fase 4 | Enumeración definitiva por operación; lecturas de A-2, A-12 y A-15 |

### 18.10 Riesgos nuevos (sin aceptar)

Los riesgos se **registran sin valorar ni aceptar**: la valoración y la aceptación son del usuario (convenciones de §16.1). Ninguno sustituye a RB1-01…13 ni a los riesgos de R2-A.

| ID | Riesgo | Unidades | Relación |
|---|---|---|---|
| RB2a-01 | Una ventana entre la lectura y la aplicación hace que dos unidades decidan sobre el mismo estado previo (último dueño, único editor, delegado por torneo) | UI-02, 03, 04, 12 | GT-02, GT-03 |
| RB2a-02 | Un estado sin editor, o con una asignación obsoleta, no queda definido y se acepta por omisión | UI-02, 03, 11 | OC-09 |
| RB2a-03 | Una proyección obsoleta concede lo ya revocado (solo CAP-02-D) | UI-11, UI-04 | GT-09 |
| RB2a-04 | Las dos capas ven versiones distintas del estado y discrepan (extensión de RB1-01) | Las siete | GT-08 |
| RB2a-05 | Una consecuencia exigida por la regla queda fuera de la unidad por enumeración incompleta (lista no exhaustiva) | UI-02, 11, 12 | IF-14, §18.5 |
| RB2a-06 | Una invitación sigue vigente tras perder la facultad quien invitó | UI-12 | OC-11 |
| RB2a-07 | Una sesión conectada se confunde con una autorización vigente (reflejo ≤ 60 s frente a rechazo inmediato) | UI-11, UI-04 | IN-03-06, IN-03-07 |
| RB2a-08 | El rechazo impuesto por la capa de datos aborta la unidad y no se registra (K-3) | Las siete | OC-13, CAP-09-P |
| RB2a-09 | Bloqueo administrativo: sin actor alterno sin conflicto, RT-14 bloquea (CL-51) y la protección de RT-06 impide resolverlo | UI-04, UI-08 | RT-14, RT-06 |
| RB2a-10 | Revocaciones cruzadas de tres o más dueños (si OC-16 lo admite): una comprobación por pares deja la cuenta sin dueño | UI-04 | ESC-02, GT-05 |
| RB2a-11 | Una candidata o una operación no enumerada (desactivar o eliminar una cuenta, degradar, recuperar) altera el conjunto de dueños sin la garantía global | CAND-06, 08, 09, 10; Fase 4 | IF-07, IF-19, GT-05, ESC-21, ESC-22, ESC-23 |

### 18.11 Secciones reservadas a R2-B2-b (no redactadas)

| Reservado | Contenido | Dependencia con esta parte |
|---|---|---|
| UI-05, UI-10 | Corrección y confirmación de resultados con recálculo y señal de cambio | GT-01, GT-07 |
| UI-06 | Aplicar una operación recibida (autorizar, idempotencia, aplicar, auditar) | ESC-07, ESC-08, OC-04, OC-12 |
| UI-07 | Anonimización | — |
| UI-09 | Calendario con RT-14 por efecto; **relación con ES-04** (cambios de los equipos de un partido con editor ya asignado) | IF-11, ES-04 |
| UI-13 | Alta de jugador con duplicados concurrentes | GT-03 |
| Frontera de sincronización | Definición de «antes» y «después» (OFF-22, D-2) | OC-04 |
| Auditoría de rechazos en la frontera | Cierre de K-3 por unidad | OC-13 |
| Amenazas AM-18 y CAB-05 | **Dependencias registradas;** tratamiento completo (no desarrollado en R2-B2-a) | ESC-08, ESC-09 |
| Amenazas AM-01, AM-15, AM-19 y AM-34 | Relacionadas con R2-B (§16.5), ver §18.12.1: AM-01 y AM-15 las trata R2-B1 por IN-02 (no se desarrollan aquí); AM-19 y AM-34, contexto en R2-B1, **quedan para R2-B2-b y R2-C** | IF-03, IF-13 |
| Conclusión final de K-2d y de DB1-1 | Evidencia de R2-B2-b sumada a §18.8 | §18.7, §18.8 |

### 18.12 Trazabilidad y criterios de aceptación

| Elemento | Origen |
|---|---|
| IF-01…IF-19 | R1 §7.0, RT-02, 05, 06, 10, 11, 13, 14; RF-ACC-07, 11; IN-02, IN-03, IN-09 |
| ES-01…ES-13 | RF-ACC-06, 12, 13, 17; RT-05, 06, 10, 13; CL-35, 41, 43, 45, 54, 60, 61 |
| Fichas §18.4.1…7 | R1 §5.5 (UI-01, 02, 03, 04, 08, 11, 12) |
| ESC-01…23 | CL-35, 38, 41, 43, 45, 47, 53, 54, 61; RF-ACC-02, 06, 07, 17; derivados marcados [DER] (§18.6.1) |
| GT-01…10 | K-2d (§17.3), K-1, K-3, IF-; trazabilidad K-2 → GT → ESC en §18.7.1 |
| Amenazas AM-01, 15, 18, 19, 34 y CAB-05 | Ver §18.12.1: AM-18 y CAB-05 son **dependencias de R2-B2-b**; no se desarrollan |
| CAND-01…11 | R1 §5.5 (lista no exhaustiva), H2B1-10, RF-ACC-02, 03, 06, 07, 09, 10 |
| §18.8 | DB1-1, DB1-6, §17.3.3, §17.3.5 |

| ID | Criterio de aceptación | Cumplimiento según el autor (a revisar por el usuario) |
|---|---|---|
| CR2B2a-01 | Las siete fichas coinciden con R1 §5.5 (disparador, componentes, requisitos) | Cumple: §18.4 y comprobación §18.13 |
| CR2B2a-02 | Ambas capas verifican la autorización en cada ficha | Cumple: campo 4 de cada ficha y IF-03 |
| CR2B2a-03 | Los estados intermedios prohibidos están definidos | Cumple: §18.3 y campo 7 |
| CR2B2a-04 | Las revocaciones no permiten operar con permisos obsoletos | Cumple como **exigencia**: IF-06, §18.6.2, GT-04 |
| CR2B2a-05 | K-2d se describe como garantía exigida, no implementada | Cumple: §18.7 |
| CR2B2a-06 | Ninguna condición pendiente aparece resuelta | Cumple: §18.9, §18.8 (DB1-1…6 y decisiones de R2-A intactas) |
| CR2B2a-07 | Sin tecnologías, proveedores ni mecanismos | Cumple según la exploración de §18.13 |
| CR2B2a-08 | R1, R2-A y R2-B1 intactos; solo cambia el archivo autorizado | Cumple según §18.13 |
| CR2B2a-09 | Las operaciones fuera de UI-01…13 solo como candidatas | Cumple: §18.5 |
| CR2B2a-10 | R2-B2-b claramente reservado | Cumple: §18.11 (AM-18 y CAB-05 como dependencias) |
| CR2B2a-11 | El último dueño se protege con una garantía de alcance global, no exclusiva de UI-04 | Cumple como **exigencia**: IF-07, GT-05, §18.5.1, ESC-21 |

#### 18.12.1 Amenazas de R2-B relacionadas (registro; no se desarrolla el modelo adicional)

R2-A (§16.5) asignó a R2-B las siguientes amenazas. **Aquí solo se registra su relación y qué queda para R2-B2-b**; no se amplía el modelo de amenazas.

| Amenaza | Qué es | Relación con R2-B2-a | Queda para |
|---|---|---|---|
| AM-01 | Manipular rol, torneo, usuario o equipo en la solicitud (intento) | Controlada por la autorización en ambas capas: IF-03, IF-13; tratada por IN-02 en R2-B1 (§17.10) | Su aplicación a las unidades de R2-B2-b |
| AM-15 | Acceso a datos de otro torneo (resultado de AM-01) | Controlada por el aislamiento en la capa de datos: IF-13; tratada por IN-02 | Su aplicación a las unidades de R2-B2-b |
| AM-18 | Carrera entre una revocación o reasignación y una operación en curso | ESC-08, ESC-09, GT-04 | **R2-B2-b** (frontera de sincronización) |
| CAB-05 | Sincronizar tras ser revocado, desasignado o reasignado, o durante la carrera con esa revocación | ESC-07, ESC-08 | **R2-B2-b** |
| AM-19 | Inyección en la capa de datos | Contexto: su control (validación de entradas) no es un permiso (§17.10) | **R2-B2-b y R2-C** |
| AM-34 | Inyección de contenido en vistas públicas, panel, correos y proyecciones | Contexto: su control es la codificación de salidas (§17.10) | **R2-B2-b y R2-C** (con R3 y la Fase 4) |

### 18.13 Verificación del autor

**Naturaleza.** Verificaciones **mecánicas de lectura** hechas por el autor con un guion de solo lectura. **No son una auditoría independiente:** el autor no puede verificar su propio trabajo con independencia. La auditoría de la v0.1 (§18.15) y la revisión focalizada de la v0.2 (§18.16) son independientes **solo de contexto**: mismo tipo de modelo, sin independencia organizativa ni externa. La v0.3 recibió una verificación final focalizada de contexto nuevo (nota de cierre de §18.16), también independiente **solo de contexto**. V-1…V-11 son las comprobaciones del autor sobre el **contenido aprobado** (SHA-256 `f07b4d9f…cbf644`); no se actualizaron tras añadir los metadatos de aprobación.

| # | Verificación | Resultado |
|---|---|---|
| V-1 | Las líneas 1 a 2585 son **idénticas** a la versión confirmada `eb0255c` (R1, R2-A y R2-B1 intactos); el cambio de todo el archivo son solo líneas añadidas | Cumple: **572 líneas añadidas y 0 eliminadas** respecto de `eb0255c` (el archivo pasa de 2585 a 3157 líneas) |
| V-2 | Solo cambió `02-arquitectura-adr.md`; `00`, `01`, `aprobaciones.md` y `bitacora.md` no cambiaron | Cumple (`git status` y sumas SHA-256) |
| V-3 | Identificadores definidos y sin referencias sin definición: IF-01…19, ES-01…13, GT-01…10, ESC-01…23, OC-01…17, DB2a-1…10, RB2a-01…11, CAND-01…11, EV-01…08, CR2B2a-01…11 | Cumple (guion de lectura: sin huérfanos; IF-04, IF-08, IF-15, GT-07 y GT-08 declarados transversales) |
| V-4 | Cada una de las siete fichas tiene los diez campos y sus componentes coinciden **literalmente** con R1 §5.5; las siete siguen siendo las únicas fichas | Cumple |
| V-5 | Las citas literales entre «» se buscaron en `00`, `01` y las partes previas | Todas existen, **salvo** los mensajes del usuario y los dictámenes de las revisiones citados en §18.0, §18.15 y §18.16, que proceden de la conversación y no de un archivo |
| V-6 | Exploración de términos tecnológicos (SQL, RLS, trigger, lock, índice, constraint, API, endpoint, nombres de productos o proveedores, token, caché y similares) | Fuera de esta propia fila, una sola aparición, en X-6, como **exclusión**; ninguna elección ni mecanismo |
| V-7 | Ninguna decisión abierta aparece resuelta: no se elige A ni D; no se declara demostrada la independencia de D ni cumplido R1 §8.1; DB1-1…6 y las decisiones de R2-A (13 originales, la 11 atendida y 12 abiertas) intactas; OC-15, OC-16 y OC-17 siguen abiertas | Cumple (lectura del autor) |
| V-8 | A-01…A-18 (§18.15) y B-01, E-01…E-07 (§18.16) están aplicadas en el lugar indicado | Cumple (lectura del autor; sin auditoría posterior) |
| V-9 | **B-01:** §18.8.1 y EV-01/EV-03 no contradicen K-2a ni la matriz de §17.3 (IN-02-26 «Condicionada (K-2)» en D; reglas de integridad excluidas de la proyección); **ES-13 aplica bajo D a toda operación que consume hechos derivados** (§18.8.1.1) | Cumple (comparación del autor con K-2a, línea de K-2 y matriz de §17.3) |
| V-10 | **Garantía global del último dueño intacta:** IF-07, IF-19 y GT-05 no se redujeron; ESC-21, 22 y 23 la ejercen sin definir «activo» ni el máximo | Cumple |
| V-11 | Correspondencia GT ↔ ESC de §18.7.1 **calculada** a partir de la columna de garantías de §18.6.1 (no escrita a mano) | Cumple: K-2b → ESC-18, ESC-20; K-2c → ESC-05, 06, 10, 16, 19; K-2d → ESC-01…23 |

**Límites.** Las **lecturas** de los casos CL y de los requisitos son del autor; los escenarios [DER] están rotulados en §18.6.1 y deben revisarse. R1 §8.1 **sigue sin cumplirse** (DB1-1 abierta).

### 18.14 Estado de esta parte y lo que no se hizo

**Estado: v0.3-R2B2a, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-19; §18.0).** Registrado en `aprobaciones.md` y `bitacora.md`; **no versionado**.

**Qué se hizo en la v0.3.** Se corrigieron el defecto material B-01 (proyección e integridad en §18.8.1, EV-01, EV-03 y el alcance de ES-13) y las observaciones E-01…E-07 (§18.16): condiciones abiertas conciliadas, creación de torneos sin presuponer la asignación del dueño, escenarios del último dueño (ESC-21 reformulado, ESC-22 y ESC-23), trazabilidad K-2d completa, provisionalidad, etiquetas de IF-03 e IF-07 (con IF-18 e IF-19) y registro de amenazas (§18.12.1).

**Qué no se hizo.** No se redactó R2-B2-b, R2-C, R3 ni R4; no se aprobó R2-B2-a, R2-B2, R2 ni la Fase 2; no se resolvió ninguna DB1 ni DB2a ni decisión de R2-A; no se eligió formulación; no se declaró demostrada la independencia de D ni cumplido R1 §8.1; no se fijó PAR ni se aceptó RES o RB; no se concedió excepción a D-2; no se modificaron otros archivos; no se crearon ramas, commits, pushes, PR ni merges; no se tocó `main`; no se actualizaron `aprobaciones.md` ni `bitacora.md`; no se desarrolló el modelo adicional de amenazas.

**Git.** Rama `claude/fase-2-arquitectura-amenazas`; HEAD `eb0255c17d728d02003839338b4cefa18e55468d`; los cambios (esta Parte IV y los registros) están **sin confirmar**; el SHA-256 del contenido aprobado figura en §18.0. Cualquier commit, push o PR requiere autorización expresa y separada.

### 18.15 Hallazgos de la auditoría de R2-B2-a v0.1 (A-01…A-18) y correcciones de la v0.2

**Origen.** Auditoría de solo lectura de un subagente de contexto nuevo (independencia **solo de contexto**), dictamen «R2-B2-a REQUIERE CORRECCIONES». Clases: **DM** defecto material; **DAL** decisión abierta legítima; **DEP** dependencia posterior; **EDI** editorial.

| ID | Sev. | Clase | Corrección aplicada en la v0.2 | Dónde |
|---|---|---|---|---|
| A-01 | Alta | DM | CAND-10 (desactivar o eliminar cuentas) y CAND-11 (asignar torneos existentes) añadidas y conciliadas con H2B1-10; fila en §18.6.2; ESC-21 | §18.5, §18.6 |
| A-02 | Alta | DM + DAL | IF-07 y GT-05 de **alcance global**; §18.5.1 con las operaciones que pueden alterar el conjunto de dueños; OC-15 («activo»); RB2a-11 | §18.2, §18.5.1, §18.7, §18.9 |
| A-03 | Media | DAL | OC-16 y DB2a-8 (máximo de dueños); ESC-02 condicionado | §18.6.1, §18.9 |
| A-04 | Media | DM | §18.8.1 separa reglas de integridad (fuente) de hechos derivados; EV-01, EV-04 corregidas; UI-04 ya no depende de la proyección | §18.8.1, §18.8.3 |
| A-05 | Media | DM leve | EV-08 contrasta con el criterio de §17.3.3; EV-02 "ya conocida"; EV-05 "mixta"; EV-03 localizada | §18.8.3 |
| A-06 | Media | DM | IF-09 [DER]; fuentes primarias en IF-01, 03, 05, 11, 12; IF-16 e IF-17 separan lo derivado; columna Tipo en ES | §18.2, §18.3 |
| A-07 | Media | DEP + DM | ES-04 como garantía derivada; vínculo con UI-09 en §18.11 y en ES-04 | §18.3, §18.11 |
| A-08 | Media | DM leve | ESC-16 precisado; ESC-19 (primera asignación de editor) | §18.6.1 |
| A-09 | Media | DAL | OC-17 y DB2a-10; ES-05 y UI-01 acotados al creador organizador; RF-ACC-12 no se amplía | §18.3, §18.4.1, §18.9 |
| A-10 | Media | DM leve | Apartado "Provisionalidad"; ES-12 provisional; UI-02 y UI-12 "composición provisional"; DB2a-3 sin la opción que contradice ES-08 | §18.3, §18.4, §18.9.2 |
| A-11 | Media | DM leve | §18.7.1 (K-2a…d → GT → ESC y lo no demostrado); ESC-20 (reconstrucción); GT-02 con la lectura de la proyección | §18.6.1, §18.7 |
| A-12 | Baja | EDI | UI-03 cita ESC-07 (con CL-47); ESC-11 cubre UI-02; OC-03 abarca CAND-04 y CAND-07 | §18.4, §18.6, §18.9 |
| A-13 | Baja | EDI | IF-04, IF-08, GT-07 y GT-08 declarados transversales; ESC-18 y ESC-20 aplican a toda ficha | §18.2, §18.4, §18.7 |
| A-14 | Baja | EDI | Origen de cada ESC (literal, [DER] o propio de D) | §18.6.1 |
| A-15 | Baja | EDI | "13 decisiones originales, la 11 atendida y 12 abiertas" | §18.0, §18.9.2, §18.13 |
| A-16 | Baja | EDI/DEP | Nota de «Precedencia de estado» en §18.0; `aprobaciones.md` y `bitacora.md` sin tocar | §18.0 |
| A-17 | Baja | EDI | Cita de la persistencia de RT-14 corregida; "reactiva" eliminado de IF-11; nota de la prioridad S de RF-ACC-17 en UI-08; DB2a-5 con IN-02-08 | §18.2, §18.4.5, §18.6.2, §18.9.2 |
| A-18 | Baja | DEP | AM-18 y CAB-05 registradas como dependencias de R2-B2-b | §18.9.3, §18.11, §18.12 |

**Nota de cierre (2026-11-19).** Esta matriz no se modifica. Las correcciones de la v0.2 fueron revisadas por la revisión focalizada de §18.16 (A-01…A-18 resueltos, con A-04 y A-09 parciales que la v0.3 completó). La aprobación condicional de R2-B2-a figura en §18.0.

### 18.16 Revisión focalizada de la v0.2 (B-01, E-01…E-07) y correcciones de la v0.3

**Origen.** Revisión focalizada de solo lectura de un subagente de contexto nuevo (independencia **solo de contexto**), dictamen «REQUIERE CORRECCIONES MATERIALES» por un único defecto material (B-01); A-01…A-18 quedaron resueltos salvo A-04 y A-09 (parciales), que esta ronda completa.

| ID | Clase | Corrección aplicada en la v0.3 | Dónde |
|---|---|---|---|
| B-01 | Defecto material | §18.8.1 rehecha con tres categorías (reglas de integridad sobre las fuentes, hechos derivados de la proyección y garantías que los vinculan); IN-02-26 como hecho derivado (asignación vigente) y la indivisibilidad de torneo y asignación como integridad; UI-02 deja de decir "exclusivamente sobre las fuentes"; EV-01 y EV-03 ya no limitan ES-13 a UI-08 y UI-11; §18.8.1.1 fija el alcance de ES-13; K-2a y las matrices de R2-B1 no se modifican | §18.8.1, §18.8.1.1, §18.8.3 |
| E-01 | Editorial | OC-15 y OC-16 conciliadas en UI-04, OC-15 en UI-08 y UI-11; ESC-02 condicionado también en UI-04 | §18.4 |
| E-02 | Editorial | UI-01 (campos 4, 5, 6 y 8) y ESC-16 acotados al creador organizador; OC-17 y DB2a-10 siguen abiertas | §18.4.1, §18.6.1 |
| E-03 | Editorial | ESC-21 admite que prosperen varias operaciones si queda un dueño; ESC-22 (CAND-06) y ESC-23 (CAND-08 y 09) añadidos sin fichas | §18.6.1, §18.5.1 |
| E-04 | Editorial | K-2b, K-2c y K-2d calculadas desde la columna de garantías de §18.6.1 (K-2d: ESC-01…23) | §18.7.1 |
| E-05 | Editorial | ES-12 provisional en ESC-04; UI-11 ↔ UI-04 condicionado a OC-03 en UI-11 §7 y §18.6.2 | §18.4.6, §18.6 |
| E-06 | Editorial | IF-03 sin el estado resultante (ahora IF-18 [DER]); IF-07 [CONF] separado del alcance global (IF-19 [DER]); IF-15 transversal | §18.2 |
| E-07 | Editorial | AM-01, 15, 19 y 34 registradas junto con AM-18 y CAB-05; cuáles quedan para R2-B2-b y R2-C | §18.9.3, §18.11, §18.12.1 |

**Nota de cierre (2026-11-19).** Esta matriz no se modifica. La v0.3 recibió una verificación final focalizada de solo lectura de otro subagente de contexto nuevo (mensaje «TORNEO DE CAMPEONES — VERIFICACIÓN DE CIERRE R2-B2-a v0.3»): B-01 RESUELTO, E-01…E-07 resueltas, sin defectos materiales bloqueantes y ocho observaciones editoriales no bloqueantes (§18.0); dictamen «APROBABLE COMO ENTREGABLE DOCUMENTAL CONDICIONADO». Independencia **solo de contexto**. La aprobación condicional de R2-B2-a como entregable documental figura en §18.0.

---

# PARTE V — BLOQUE R2-B2-b1 (v0.3-R2B2b1, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL, 2026-11-19)

## 19. R2-B2-b1 — ADR-02.C (parte 2b-1): frontera atómica de las operaciones de datos

### 19.0 Alcance, autorización y reglas de esta parte

**Estado: v0.3-R2B2b1 — APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-19), por acto expreso del usuario (ver «Aprobación condicional» más abajo).** La v0.3 corrigió la v0.2 tras la revisión focalizada de contexto nuevo (§19.15) y la v0.2 corrigió la v0.1 (§19.14). Esta parte no altera ninguna línea de R1 (§0–§15), R2-A (§16), R2-B1 (§17) ni R2-B2-a (§18); solo se **añade** al final del archivo.

**Autorización recibida.** Mensaje «TORNEO DE CAMPEONES — INICIO CONTROLADO DE R2-B2-b1» (2026-11-19): «APRUEBO EL PLAN DE TRABAJO DE R2-B2-b COMO GUÍA DOCUMENTAL.» «AUTORIZO EXCLUSIVAMENTE EL INICIO Y LA REDACCIÓN DEL BORRADOR R2-B2-b1 — OPERACIONES DE DATOS.» «NO AUTORIZO INICIAR R2-B2-b2.» Archivo autorizado: solo `02-arquitectura-adr.md` (Parte V, §19, limitada a R2-B2-b1). La fecha es la del aviso de fecha de la sesión; no hay marca de tiempo individual del mensaje.

**Autorización de la corrección (v0.2).** Mensaje «TORNEO DE CAMPEONES — CORRECCIÓN INTEGRAL DE R2-B2-b1 v0.1» (2026-11-19): «AUTORIZO EXCLUSIVAMENTE UNA RONDA INTEGRAL DE CORRECCIONES DOCUMENTALES DE LOS HALLAZGOS C-01 A C-15 DE R2-B2-b1.» C-16 solo se corrige si es necesario para la coherencia, sin ampliar el alcance. Limitada a la Parte V (§19); R2-B2-b1 continúa como **borrador pendiente de aprobación**.

**Autorización de la corrección mínima (v0.3).** Mensaje «TORNEO DE CAMPEONES — CORRECCIÓN MÍNIMA DE CIERRE R2-B2-b1» (2026-11-19): «AUTORIZO EXCLUSIVAMENTE CORREGIR D-01, D-02 Y D-03 DEL BORRADOR R2-B2-b1 v0.2.» Limitada a la Parte V (§19); sin aprobación, registro ni versionado; R2-B2-b1 continúa como **borrador pendiente de aprobación**.

**Aprobación condicional de R2-B2-b1 como entregable documental (2026-11-19).** Por acto expreso del usuario (mensajes «TORNEO DE CAMPEONES — APROBACIÓN DOCUMENTAL CONDICIONADA DE R2-B2-b1» y «TORNEO DE CAMPEONES — REGISTRO FORMAL DE APROBACIÓN CONDICIONAL R2-B2-b1»), la v0.3-R2B2b1 quedó **APROBADA CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL**: «APRUEBO CONDICIONALMENTE R2-B2-b1 v0.3 COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL.»; registro autorizado con «AUTORIZO EXCLUSIVAMENTE REGISTRAR LA APROBACIÓN CONDICIONAL DE R2-B2-b1 COMO ENTREGABLE DOCUMENTAL.»

- **Comprende:** las cinco fichas UI-05, UI-07, UI-09, UI-10 y UI-13; los invariantes IF-20…31; los estados prohibidos ES-14…24; las garantías GT-11…18; los escenarios de concurrencia ESC-24…42; las operaciones candidatas CAND-12…24; la separación entre transacción y efectos externos; el análisis documental de CAP-02-A y CAP-02-D; la trazabilidad con RT-14 y RD-RES-04; y las condiciones, decisiones y riesgos documentados en §19.
- **Exclusiones (la aprobación NO implica):** (1) elegir CAP-02-A ni CAP-02-D; (2) declarar demostrada la independencia de CAP-02-D; (3) dar por cumplido R1 §8.1; (4) resolver DB1-1 a DB1-6; (5) resolver DB2a-1 a DB2a-10; (6) resolver DB2b-1 a DB2b-9; (7) resolver OC-01 a OC-33; (8) resolver las doce decisiones abiertas de R2-A; (9) aprobar definitivamente ADR-02.C; (10) considerar las garantías implementadas ni verificadas empíricamente; (11) fijar PAR ni aceptar RES o riesgos RB; (12) conceder excepción a D-2; (13) aprobar globalmente R2-B2, R2 ni la Fase 2; (14) autorizar R2-B2-b2, R2-C, R3 o R4.
- **Limitación de la revisión.** La auditoría, la revisión focalizada y la verificación final las hicieron subagentes del mismo tipo de modelo que redactó el borrador: su independencia fue **solo de contexto, no organizativa ni externa**. La aprobación es exclusivamente documental: **no acredita implementación ni pruebas empíricas** y no constituye una auditoría externa.
- **Observaciones editoriales no bloqueantes** (identificadas para una eventual conciliación documental, sin modificar el contenido técnico aprobado): las registradas en §19.15 (ES-22 [CONF] en bloque aunque "corrección encubierta" es una derivación; IF-24 [CONF] cita RD-CAL-07, que es [REC]; IF-24 remite a IF-17, que no lista A-1; CAND-24 cita RD-RES-10 en lugar de RD-RES-12; OC-33 solo trata asignaciones de editor; el inventario no menciona eliminar un torneo en borrador; DB2b-4 dice "completar K-2c"; RF-AUD-04 respalda IF-31 y no se cita) y las de la verificación final (UI-09 campo 7 dice "la regeneración modifica asignaciones de editor vigentes" sin recordar [REC]; EV-10 menciona solo una de las dos consecuencias que DB2b-4 (b) expone; GT-12 cita ESC-40 aunque ESC-40 lista GT-02, GT-04 y GT-17; V-12 es verificación del propio autor).
- **Integridad.** SHA-256 del **contenido aprobado** (el archivo inmediatamente antes de añadir estos metadatos): `f72d1170f4936827fdf6da174812fe3b027ef1b2886e2378326d8177a04d0db7`. El hash del **archivo con los metadatos de aprobación** se registra en `aprobaciones.md` y `bitacora.md`; **no se inserta en este archivo**. Los metadatos de aprobación son solo editoriales: título y estado de la Parte V, este apartado, la fila de estado de §19.1, referencias de §19.12 y §19.13, la nota de cierre de §19.15 y las frases de estado de la precedencia de §17.0 y §18.0.
- **Estado de versionado.** **Pendiente:** sin commit, push, PR ni merge.
- **Condición de cierre de la Fase 2.** Las decisiones que la Fase 1 asignó a la Fase 2 deberán resolverse antes de cerrar globalmente la Fase 2, salvo modificación o excepción formalmente aprobada. Las condiciones pendientes de R2-B2-b2, R2-C, R3 y la Fase 4 conservan su alcance.

**Qué contiene.** Los invariantes de frontera y estados prohibidos adicionales para las operaciones de datos (§19.2), los efectos que quedan fuera de la unidad (§19.3), las **cinco fichas** UI-10, UI-05, UI-13, UI-09 y UI-07 (§19.4), las operaciones candidatas que no tienen unidad propia (§19.5), la relación con CAP-02-A y CAP-02-D (§19.6), la matriz de concurrencia (§19.7), la matriz de dependencias (§19.8), condiciones, decisiones y riesgos (§19.9), lo reservado a R2-B2-b2 (§19.10), trazabilidad y criterios (§19.11), verificaciones del autor (§19.12), estado (§19.13), los hallazgos de la auditoría con sus correcciones (§19.14) y los de la revisión focalizada (§19.15).

**Qué NO contiene ni hace.**

| # | Exclusión |
|---|---|
| X-1 | No redacta R2-B2-b2 (UI-06, la frontera completa de sincronización, la auditoría de rechazos y K-3, la síntesis final de K-2d, la evaluación consolidada de CAP-02-A/D, el modelo adicional de amenazas), ni R2-C, R3 ni R4 |
| X-2 | No aprueba R2-B2-b2, R2-B2 completo, R2 ni la Fase 2 (la aprobación condicional de R2-B2-b1 como entregable documental figura en este apartado y no amplía su alcance) |
| X-3 | No resuelve DB1-1…DB1-6, DB2a-1…DB2a-10, OC-01…OC-17 ni las 12 decisiones abiertas de R2-A (de las 13 originales, la 11 está atendida) |
| X-4 | **No elige** CAP-02-A ni CAP-02-D; **no declara demostrada la independencia de CAP-02-D**; **no declara cumplido R1 §8.1**; no concede excepción |
| X-5 | No fija PAR ni acepta RES o riesgos RB; no concede excepción a D-2 |
| X-6 | No elige tecnología, proveedor, producto ni mecanismo (de bloqueo, serialización, consistencia, almacenamiento ni propagación); no escribe SQL, políticas, rutas ni contratos de interfaz |
| X-7 | No modifica `00`, `01`, `aprobaciones.md`, `bitacora.md`, ni R1, R2-A, R2-B1 ni R2-B2-a; no crea ramas, commits, pushes, PR ni merges |
| X-8 | **No presenta ninguna garantía como implementada ni validada:** todas son **exigidas**, a nivel de capacidad |

**Lectura previa directa.** El autor leyó directamente (no solo por resúmenes) en esta sesión: el skill del proyecto; en `01` RT-13, RT-14 (texto completo), RT-10, la matriz §7.1 completa, RD-RES-06 y 07, RD-TAB-09, 10 y 11, RD-CAL-02 y 07, RD-REG-03, RD-JUG-02, 08, 09 y 10, PRV-02, 06 y 18, RF-AUD-01, 02 y 06, A-1, A-2 y A-8, CL-15, 22, 26, 34, 36, 44 y 51, Q-05, Q-20, Q-40, Q-41 y Q-42; en R1 §5.3, §5.5 (UI-01…13 y sus notas), §5.6, §5.9, §7.0, §7.4 y §8.1, HR1-11 y HR1-12; en R2-A las 13 decisiones, AM-01, 15, 18, 19 y 34 y CAB-05, 07, 18 y 24; en R2-B1 K-1…K-3, CAP-02-A/D, H2B1-10 y las dependencias; en R2-B2-a las siete fichas, IF, ES, GT, §18.8 y las reservas de §18.11; y `aprobaciones.md` y `bitacora.md`. Para la corrección de la v0.2 se leyeron además directamente RD-RES-04 y 05, RD-CAL-06, RD-JUG-02 (texto completo), 03, 04 y 05, RF-AUD-03, RF-EQP-02 y 03, A-4, A-15 y A-16, Q-26, Q-29 y Q-31, y la asignación de ADR-12 y ADR-17 en R1 §8.1.

**Convención.** [CONF]: texto confirmado de `01` o R1. [REC]: recomendación o propuesta de `01` (no confirmada). [DER]: derivada por el autor (se explica el paso). [ABIERTO]: sin resolver; ninguna se elige. Los identificadores continúan los de R2-B2-a: IF-20…, ES-14…, GT-11…, ESC-24…, OC-18…, CAND-12…, EV-09….

### 19.1 ADR-02.C (parte 2b-1) — Identificación

| Campo | Valor |
|---|---|
| ADR | ADR-02.C (continuación de §17.1 y §18.1): frontera atómica de las operaciones de datos |
| Entradas | R1 §5.3, §5.5, §7.0, §8.1; R2-A (ADR-09.C); R2-B1 (IN-02, CAP-02-A…D, K-1…K-3, DB1-1…6); R2-B2-a (IF-01…19, ES-01…13, GT-01…10) |
| Salida de esta parte | Frontera por unidad, estados intermedios prohibidos, efectos externos, concurrencia y relación con la proyección de CAP-02-D |
| Reservado a R2-B2-b2 | §19.10 |
| Estado | v0.3-R2B2b1: APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-19); ADR-02.C **no** queda aprobada definitivamente |

### 19.2 Invariantes de frontera y estados prohibidos adicionales

Se **suman** a IF-01…19 y ES-01…13 de R2-B2-a (IF-03, IF-04, IF-05, IF-06, IF-09, IF-10, IF-11, IF-12, IF-13 e IF-15 se aplican también a estas unidades; **IF-23** se aplica de forma específica a UI-10, **IF-29** a toda unidad de varios cambios y **GT-18** es transversal a las cinco). No introducen permisos ni retiran garantías (IN-02-18).

**Invariantes (IF).**

| ID | Invariante de frontera | Tipo | Fuente | Paso derivado (solo [DER]) |
|---|---|---|---|---|
| IF-20 | **Resultado, eventos, estado, historial y recálculo forman una sola unidad:** la corrección del marcador y de los eventos de un partido finalizado se hace «en una sola operación atómica», con motivo, historial del resultado anterior y el nuevo, usuario responsable, fecha del servidor y «recálculo consistente» de las estadísticas afectadas; confirmar un resultado exige el mismo recálculo | [CONF] | RD-RES-06, RD-RES-07, RD-TAB-11, CL-15; R1 §5.5 (UI-05, UI-10) | — |
| IF-21 | **Una sola fuente de verdad para la clasificación:** el recálculo lo hace CMP-12 con una definición única; si existieran dos implementaciones, se exige prueba de equivalencia | [CONF] | RD-TAB-10, R1 §5.9 | — |
| IF-22 | **Lo transaccional es el estado de los datos de Z2/Z3 y, si existieran, sus proyecciones derivadas persistentes** (que existan o no lo decide ADR-04). Si ADR-06 adopta una señal de cambio, se registra de forma durable dentro de la misma unidad; **la entrega y el efecto hacia Z1 y Z0 son posteriores y no forman parte de la unidad** | [CONF] | R1 §5.5 («Transacción de datos y señal de cambio son cosas distintas») | — |
| IF-23 | **Nada capturado sin conexión es oficial ni público hasta la validación del servidor;** un resultado rechazado o en conflicto nunca es visible | [CONF] | RD-RES-07, OFF-22 | — |
| IF-24 | **RT-14 se evalúa sobre el conjunto completo de partidos que la operación modificaría efectivamente** (crear, archivar, eliminar, reprogramar, mover de jornada o localía, cambiar rivales, estado, resultado, eventos y asignación de editores), incluidos los efectos en cascada. Si incluye algún partido de un equipo del que el actor **es o ha sido delegado en ese torneo, con la restricción no levantada**, la operación se rechaza **por completo**. La restricción se evalúa sobre las asignaciones de delegado **vigentes y las revocadas** del historial inmutable (persistencia histórica) y **no se extiende a editores** (RT-05). El levantamiento solo lo aprueba otro dueño activo sin conflicto, con motivo, limitado a ese equipo y torneo y sin autoaprobación; pierde efecto si el rol de delegado se otorga de nuevo. Una operación automática hereda la situación del actor que la dispara; consultar y previsualizar no se restringen. **Las lecturas de A-1 (alcance de «resultados») y de A-12 (levantamiento) son parámetros (IF-17), no se resuelven aquí** | [CONF] | RT-14 («quien haya sido delegado del equipo X en el torneo T conserva la restricción»; «se evalúa sobre las asignaciones de delegado vigentes y las revocadas»), Q-38 (a), Q-44 B, Q-45 C, RF-ACC-17, RD-CAL-02, RD-CAL-07, RD-RES-06 | — |
| IF-25 | **Todo o nada en la generación de calendario:** confirmar crea todos los partidos «en una transacción o ninguno»; la generación inicial y la regeneración «no están exentas de RT-14»; una vista previa no persiste nada | [CONF] en RD-CAL-02 y RT-14; **[REC]** en la regeneración (RD-CAL-07: solo si todos los partidos están `programado`, listado de consecuencias, archivo de los partidos anteriores) | RD-CAL-02, RD-CAL-07 (estado REC en `01`), RT-14 | — |
| IF-26 | **Una misma identidad confirmada no puede estar activa en dos equipos del mismo torneo;** ante dos altas simultáneas de nombres equivalentes en equipos distintos, «a lo sumo una queda activa sin verificación» y la otra queda pendiente; **la garantía la impone la capa de datos, no solo la aplicación** | [CONF] | RD-JUG-08, RD-JUG-02, CL-22, CL-36 | — |
| IF-27 | **Anonimizar es irreversible y distinto de ocultar;** conserva el identificador interno, el dorsal, las estadísticas y los resultados; tras anonimizar, tabla y goleadores no cambian; «ningún campo personal queda en base de datos ni en auditoría» | [CONF] | PRV-06, Q-05, CL-26 | — |
| IF-28 | **Ninguna atomicidad se atribuye a procesos externos a la transacción de datos:** la propagación a Z1 y a copias locales en Z0 (HR1-11) y los respaldos (HR1-12) quedan fuera | [CONF] | R1 §5.5 (UI-07, nota de transacción) | — |
| IF-29 | **Rechazo completo ante fallo de autorización o de integridad:** si cualquier componente de una unidad no puede aplicarse o verificarse, no se aplica ninguno de sus cambios (el registro del rechazo queda fuera de la unidad: IF-09) | [DER] | RT-13, RD-CAL-02, IN-09-04, IF-01 | Una unidad indivisible no admite aplicación parcial; IF-01 e IF-09 ya lo establecen para toda operación y aquí se explicita para las operaciones multi-cambio de datos |
| IF-30 | **Control de versión sin sobrescritura:** toda modificación de marcador o estado envía la versión conocida; si el servidor tiene otra, la operación se «rechaza con un código de conflicto de aplicación propio», se devuelve la versión actual y «no sobrescribe»; dos sesiones sobre la misma versión: la segunda recibe conflicto y los datos no cambian. La idempotencia de las operaciones repetidas (RD-RES-05, OFF-06) y el conflicto de versión de la ruta sincronizada se completan en **R2-B2-b2** | [CONF] | RD-RES-04 (N-06, Q-06), RD-RES-05 | — |
| IF-31 | **Minimización desde el origen en la auditoría de jugadores:** como el registro es de solo anexado y no puede reescribirse, los eventos que se refieren a jugadores se crean **solo con identificadores internos y los datos mínimos necesarios**, sin datos personales innecesarios; los plazos y la retención son de ADR-17 (R4) y de la asesoría jurídica y **no se definen aquí** | [DER] | RF-AUD-01 («valores antes/después minimizados»), RF-AUD-03, PRV-06, T-04 | Si un evento escrito con datos personales no puede modificarse después (RF-AUD-03), PRV-06 («ningún campo personal queda en base de datos ni en auditoría») solo puede cumplirse escribiendo desde el origen solo identificadores internos |

**Estados prohibidos (ES).** Ningún observador debe verlos y ninguna ordenación de operaciones concurrentes debe producirlos.

| ID | Estado prohibido | Tipo | Fuente |
|---|---|---|---|
| ES-14 | Un resultado confirmado cuya clasificación derivada persistente (si existe) no lo refleja, o una clasificación derivada que refleja un resultado no confirmado; **la coherencia pública es garantía exigida a ADR-04.C, no una propiedad de la unidad** | [CONF] | CL-15, RD-TAB-11; R1 §5.5 |
| ES-15 | Una corrección aplicada a medias: marcador sin eventos corregidos (o al revés), cambio sin historial del resultado anterior, sin motivo o sin usuario responsable | [CONF] | RD-RES-06 |
| ES-16 | Dos resultados oficiales vigentes para el mismo partido; un resultado confirmado sobre un partido que la operación ya no puede modificar (archivado por una regeneración); o **una corrección aplicada con el torneo `archivado` por un actor distinto del dueño** (el dueño sí corrige: D-5, matriz §7.1 fila 27) | [DER] | RD-RES-07, RD-CAL-07, RD-RES-06, IF-05 |
| ES-17 | (**solo si ADR-06 adopta una señal de cambio**) Señal de cambio registrada sin que el cambio de datos esté confirmado, o cambio confirmado sin su señal | [DER] | R1 §5.5 |
| ES-18 | Una misma identidad confirmada activa en dos equipos del mismo torneo; dos altas activas sin verificación para nombres equivalentes | [CONF] | RD-JUG-08, CL-22, CL-36 |
| ES-19 | Un calendario parcial: partidos creados sin los demás, partidos anteriores archivados sin los nuevos, partidos de dos generaciones vigentes a la vez, o una generación o regeneración confirmada que incluye un partido de un equipo del que el actor **es o ha sido delegado en ese torneo con la restricción no levantada** | [CONF] en "todos o ninguno" y en RT-14; [REC] en el archivo de los anteriores sin los nuevos (RD-CAL-07); [DER] en la coexistencia de dos generaciones | RD-CAL-02, RD-CAL-07, RT-14, Q-45 |
| ES-20 | Un jugador anonimizado a medias en la fuente: datos personales borrados en Z2/Z3 pero presentes en una proyección derivada persistente, estadísticas o resultados alterados, o datos personales en un registro de auditoría | [CONF] | PRV-06; R1 §5.5 (UI-07) |
| ES-21 | Una anonimización, una corrección o una confirmación aplicada por un actor sin facultad vigente sobre ese jugador o partido, o con la facultad ya revocada y confirmada | [CONF] | RT-13, CL-45, matriz §7.1 filas 21, 24, 25 y 27 |
| ES-22 | Una segunda confirmación o corrección que **sobrescribe en silencio** una versión más reciente, o una segunda confirmación de un partido ya finalizado que actúa como una **corrección encubierta** (sin motivo, sin historial ni la facultad que solo tienen dueño y organizador; el editor no corrige) | [CONF] | RD-RES-04, RD-RES-06, CL-34 |
| ES-23 | Una asignación de editor que permanece **vigente sobre un partido archivado** (por una regeneración u otra operación que lo archive) | [DER] | RD-CAL-07, RF-ACC-06, IF-14 | 
| ES-24 | Un evento nuevo aplicado a un jugador en `baja` o `pendiente de verificación` | [CONF] en el jugador pendiente (RD-JUG-02 (e)); [REC] en la baja (RD-JUG-04) | RD-JUG-02, RD-JUG-04 |

### 19.3 Efectos externos que no forman parte de ninguna unidad

R1 §5.5 declara que **ninguna atomicidad se atribuye a operaciones externas a la transacción de datos**. Para las cinco unidades:

| Efecto | Zona | Por qué queda fuera | Quién lo decide |
|---|---|---|---|
| Entrega de lo confirmado a las vistas públicas y su actualización | Z1 | Posterior a la unidad; su plazo y mecanismo son de ADR-06 (el objetivo de RF-PUB-09 es ≤ 60 s [REC]) | ADR-06 (R3); garantía de coherencia: ADR-04.C (R2-C) |
| Actualización de copias de datos preparados y de vistas guardadas en el dispositivo | Z0 | No pueden purgarse a distancia hasta la próxima sincronización (HR1-11) | ADR-04, ADR-05, ADR-16 (R2-C) y asesoría jurídica |
| Respaldos existentes | Z3 | Conservan datos anteriores a una anonimización hasta su caducidad; la retención no está fijada en `01` (HR1-12) | **ADR-12 y ADR-17 (R4), con asesoría jurídica**; la retención no se define aquí |
| Exportaciones y metadatos ya entregados antes de la operación | Z0, Z4 | Salen del control de la unidad | OFF-24, OFF-20 (R2-C, R2-B2-b2) |
| Invalidación y notificación a otros dispositivos y sesiones | Z0 | Posterior; el objetivo de reflejo es ≤ 60 s sin garantía fuera de línea (IN-03-07) | ADR-03, ADR-05 |

### 19.4 Fichas de las cinco unidades

**Contenido de cada ficha** (el mismo de §18.3): (1) disparador; (2) componentes según R1 §5.5; (3) requisitos aplicables; (4) autorización en el servidor y en la capa de datos; (5) secuencia lógica; (6) cambios indivisibles; (7) estados intermedios prohibidos; (8) resultado ante concurrencia; (9) relación con la auditoría; (10) decisiones y condiciones abiertas. **La lista de §5.5 no es exhaustiva y su enumeración definitiva por operación es de la Fase 4.**

**Provisionalidad.** Lo que depende de condiciones o decisiones abiertas (OC-18…, DB2b-…) es **provisional** hasta que el usuario decida; **ninguna interpretación del autor se convierte en decisión aprobada.** Las facultades que `01` deja pendientes (Q-42) y las lecturas ambiguas (A-1, A-2, A-8) se tratan como **parámetros del evaluador** (IF-17). **Bajo CAP-02-D** cada ficha **consume** hechos derivados (como mínimo la facultad del actor y, según la unidad, asignaciones, roles o restricciones): **ES-13 aplica a las cinco** (§18.8.1.1); esas unidades modifican datos, no permisos, y por K-2c **no actualizan la proyección**; **si la regeneración de calendario (y otras operaciones que retiran o alteran asignaciones) debe actualizarla permanece ABIERTO** (OC-24, DB2b-4; §19.6.2). ESC-18 y ESC-20 (R2-B2-a) y ESC-41 (§19.7) se aplican a toda ficha bajo D.

#### 19.4.1 Ficha UI-10 — Confirmar un resultado y recalcular

| Campo | Contenido |
|---|---|
| 1. Disparador | El editor **asignado** al partido, el dueño y, si Q-42 (a) lo habilita, el organizador del torneo confirman el marcador, los eventos y el estado final de un partido **en línea** (matriz §7.1, filas 24 y 25). **Cuando el resultado llega por sincronización, UI-10 se ejecuta dentro de UI-06** (una sola transacción de datos): esa composición pertenece a **R2-B2-b2** y aquí solo se identifica |
| 2. Componentes (R1 §5.5) | CMP-11 + CMP-12 + CMP-14 + CMP-08 + CMP-16 |
| 3. Requisitos | RD-RES-07, RD-TAB-11, CL-15, Q-32; RF-ACC-06 (editor asignado), RT-05, RT-13, RT-14 (clase «resultados»; A-1; persistencia histórica, Q-45); **RD-RES-04 (control de versión)**; RD-JUG-02 (e) y RD-JUG-04 (un jugador pendiente o en baja no recibe eventos); IN-02-11, IN-02-12, IN-02-20, IN-02-22; IF-20…IF-24, IF-30; OFF-22 |
| 4. Autorización | **Servidor:** CMP-08 decide, con la definición única, que el actor tiene rol vigente y **asignación vigente a ese partido** (editor), o la facultad del dueño o del organizador de ese torneo; que el torneo y el partido están en un estado que admite confirmar (el estado es una entrada que se lee de su fuente: IN-02-20); que un editor que es delegado de un equipo del partido no actúa (RT-05); y que un dueño u organizador que **es o ha sido** delegado de un equipo del partido en ese torneo, con la restricción histórica no levantada, **no confirma** (RT-14; Q-45; RF-ACC-17; clase «resultados»: la lectura de A-1 es parámetro y la del levantamiento, A-12). **Capa de datos:** verifica la misma decisión y el **estado resultante**: un único resultado oficial por partido, de un partido del mismo torneo, con la asignación vigente al partido cuando quien confirma es el editor (el dueño y el organizador actúan por su facultad, sin asignación). El servidor verifica además autenticación, integridad y **conflictos de versión: la operación envía la versión conocida y, si el servidor tiene otra, se rechaza y no sobrescribe** (RD-RES-04, RD-RES-07) |
| 5. Secuencia lógica | (1) identificar al actor; (2) reunir roles, asignación al partido, estado del torneo y del partido, historial de delegaciones y restricciones; (3) **calcular el efecto:** el resultado confirmado y la clasificación y estadísticas que CMP-12 recalcula con la definición única; (4) política efectiva de segundo factor con todos los roles vigentes (IF-12); (5) decidir (RT-05, RT-14, unión de roles); (6) aplicar y auditar, o rechazar |
| 6. Cambios indivisibles | Marcador + eventos + estado del partido + **clasificación y estadísticas derivadas persistentes, si existen** (que existan o no lo decide ADR-04; si se calculan al servir, no hay estado derivado que escribir y la coherencia descansa en un único resultado confirmado) + **señal de cambio, solo si ADR-06 la adopta** + evento de auditoría. **Si Q-22 adopta la resolución manual de desempates, esta queda obsoleta (vuelve a «sin resolver» con aviso) cuando cambian los resultados que la determinaban** (RD-TAB-09; R1 §5.9): su actualización pertenece al recálculo de esta unidad, **condicionada a Q-22**. **Fuera de la unidad:** entrega a Z1, actualización de vistas y de copias en Z0 (§19.3) |
| 7. Estados prohibidos | ES-14, ES-16, **ES-22**, ES-24, ES-17 (si hay señal), ES-21 y ES-10 (resultado aplicado con la asignación o el rol ya revocados y confirmados); **parcial:** resultado sin recálculo; recálculo sin resultado. **Bajo D:** ES-13 sobre la asignación vigente y la facultad del actor |
| 8. Concurrencia | **ESC-24:** dos confirmaciones del mismo partido (dos dispositivos o actores) sobre la misma versión: una sola resulta oficial; **la otra recibe un conflicto de versión, no sobrescribe y no actúa como corrección encubierta** (RD-RES-04). **ESC-42:** baja o paso a pendiente de un jugador concurrente con una confirmación que le asigna eventos. **ESC-25:** confirmaciones o correcciones de partidos distintos que alteran la misma clasificación: el resultado equivale a una ordenación secuencial y la clasificación final refleja **ambas**. **ESC-27:** reasignación o revocación del editor (UI-03, UI-11) concurrente: aplicada antes y válida o rechazada (CL-45). **ESC-29:** cambio de reglamento concurrente (Q-23). **ESC-30:** regeneración del calendario concurrente. **ESC-31:** concesión del rol de delegado o levantamiento concurrentes con un actor dueño u organizador. **ESC-38:** anonimización concurrente |
| 9. Auditoría | El registro de lo confirmado (resultado, actor y rol, hora del servidor, origen en línea) va **dentro** de la unidad (IF-10); si no puede registrarse, no se aplica. Un rechazo (incluidos los de RT-14, RF-AUD-06) se registra **fuera** de la unidad (IF-09); su persistencia es K-3, reservada a R2-B2-b2 |
| 10. Abiertas | **OC-18** (A-1, alcance de «resultados» en RT-14); **OC-19** (Q-42: si el organizador confirma); **OC-20** (Q-21, Q-22, Q-23, Q-29: reglas que alimentan el recálculo, y si ADR-04/ADR-08 materializan o calculan al servir); **OC-30** (composición con UI-06, R2-B2-b2); **OC-31** (si el recálculo de la clasificación de equipos ajenos amplía el alcance de RT-14); **OC-32** (estados de plantilla y jugadores en baja o pendientes); ADR-06 (¿señal?); decisión 10 de R2-A y K-3. DB1-1 intacta |

#### 19.4.2 Ficha UI-05 — Corregir un resultado finalizado

| Campo | Contenido |
|---|---|
| 1. Disparador | El dueño o el organizador del torneo corrigen marcador y eventos de un partido finalizado, «sin plazo límite», con motivo obligatorio (RD-RES-06). Con el torneo `archivado`, **solo el dueño** (D-5). El editor **no** corrige (CL-34) |
| 2. Componentes (R1 §5.5) | CMP-11 + CMP-12 + CMP-14 + CMP-08 + CMP-16 |
| 3. Requisitos | RD-RES-06, RD-TAB-11, **RD-RES-04**, CL-15, CL-34, CL-44, CL-51; matriz §7.1 fila 27; RT-14 (clase «resultados»; persistencia histórica, Q-45); RD-JUG-02 (e) y RD-JUG-04; IN-02-12, IN-02-20; IF-20…IF-24, IF-30, IF-31 |
| 4. Autorización | **Servidor:** CMP-08 decide que el actor es dueño u organizador de ese torneo, que el estado del torneo lo permite (organizador: solo mientras no esté `archivado`; dueño: también `archivado`), que hay motivo, y que el actor **no es ni ha sido delegado de un equipo del partido en ese torneo con la restricción histórica no levantada** (RT-14; Q-45; RF-ACC-17; A-1 y A-12 son parámetros). **Además, la corrección envía la versión actual del resultado: con una versión obsoleta se rechaza por conflicto y no sobrescribe** (RD-RES-04). **Capa de datos:** verifica lo mismo y el estado resultante: un resultado oficial por partido con historial del anterior y del nuevo, usuario responsable y fecha del servidor. Sin otro actor sin conflicto, la corrección **queda bloqueada y auditada** (CL-51), sin afectar la protección del último dueño (IF-07, IF-19) |
| 5. Secuencia lógica | (2) reunir roles, estado del torneo, resultado vigente y su historial, historial de delegaciones y restricciones; (3) efecto: el nuevo resultado, el historial y el recálculo de tabla, goleadores y tarjetas de **toda** la clasificación afectada (también de jornadas anteriores); (4) política de 2FA; (5) decidir; (6) aplicar y auditar, o rechazar completo |
| 6. Cambios indivisibles | Marcador + eventos + motivo + historial anterior y nuevo + usuario y fecha del servidor + recálculo de las estadísticas afectadas («en una sola operación atómica», «recálculo consistente») + señal de cambio si ADR-06 la adopta + evento de auditoría con valores antes y después. **Si Q-22 adopta la resolución manual de desempates, esta queda obsoleta cuando cambian los resultados que la determinaban** (RD-TAB-09; R1 §5.9), dentro del mismo recálculo y **condicionado a Q-22**. **Fuera:** Z1, Z0 (§19.3) |
| 7. Estados prohibidos | ES-14, ES-15, ES-16, **ES-22**, ES-24, ES-17 (si hay señal), ES-21, ES-10; **parcial:** marcador corregido sin eventos; corrección sin historial o sin motivo. **Bajo D:** ES-13 |
| 8. Concurrencia | **ESC-26:** dos correcciones del mismo partido (dueño y organizador) sobre la misma versión: una prevalece y **la otra recibe conflicto de versión sin sobrescribir** (RD-RES-04); **una segunda corrección solo se aplica con la versión actualizada, su motivo y su historial.** **ESC-42:** baja o paso a pendiente de un jugador concurrente con una corrección que le asigna eventos. **ESC-25:** correcciones o confirmaciones concurrentes sobre la misma clasificación. **ESC-27:** pérdida de la facultad del actor concurrente. **ESC-28:** el torneo pasa a `archivado` mientras un organizador corrige: aplicada antes o rechazada (D-5). **ESC-29:** cambio de reglamento concurrente (Q-23). **ESC-30**, **ESC-31** como en UI-10. **ESC-38:** anonimización concurrente |
| 9. Auditoría | Corrección y valores antes y después **dentro** de la unidad (IF-10; RF-AUD-02: «estados de partido, resultados, eventos, correcciones»), **con valores minimizados y solo identificadores internos de los jugadores** (IF-31: minimización desde el origen; RF-AUD-01, RF-AUD-03, PRV-06); el rechazo o el bloqueo por RT-14 se registra **fuera** (RF-AUD-06; IF-09; K-3 en R2-B2-b2) |
| 10. Abiertas | **OC-18**; **OC-20** (Q-23 y reglas); **OC-21** (Q-40 (c): qué otras operaciones se bloquean en `archivado`); **OC-31** (clasificación de equipos ajenos y alcance de RT-14); **OC-32** (jugadores en baja o pendientes); DB2b-3; K-3. **Relación con UI-10:** ambas comparten la frontera del recálculo (IF-20, IF-21, GT-11) y deben leerse juntas; UI-10 aplica a un resultado nuevo y UI-05 a uno finalizado. **Candidatas que comparten su recálculo:** CAND-14, CAND-15 y CAND-16 (§19.5); **OC-29** (señal de cambio) |

#### 19.4.3 Ficha UI-13 — Alta de jugador con verificación de duplicados concurrentes

| Campo | Contenido |
|---|---|
| 1. Disparador | El delegado (de su equipo), el organizador (de sus torneos) o el dueño dan de alta a un jugador en una plantilla (matriz §7.1, fila 19). Pueden coincidir altas simultáneas de nombres equivalentes en equipos distintos (CL-36) |
| 2. Componentes (R1 §5.5) | CMP-10 + CMP-08 + CMP-16 |
| 3. Requisitos | RD-JUG-02, RD-JUG-03 (dorsal único, REC), RD-JUG-04 (REC), RD-JUG-05 (COND Q-31), RD-JUG-08, RD-JUG-09, RD-JUG-10 (COND Q-41), CL-22, CL-36, Q-12; IN-02-04, IN-02-23; IF-26, IF-31; RF-AUD-02 («altas/bajas/anonimización de jugadores»); PRV-09 |
| 4. Autorización | **Servidor:** CMP-08 decide que el actor puede dar de alta en esa plantilla (delegado: **solo su equipo**; organizador: sus torneos; dueño: todos) y que la referencia es del mismo torneo. **Capa de datos:** verifica la misma facultad y el **estado resultante**: una misma identidad confirmada no está activa en dos equipos del mismo torneo, y a lo sumo una de dos altas de nombres equivalentes queda activa sin verificación (la otra, `pendiente de verificación`). La comprobación de duplicados **examina todas las plantillas del torneo**, pero **la respuesta al delegado no revela datos de otro equipo** (RD-JUG-02) |
| 5. Secuencia lógica | (2) reunir la facultad del actor, las altas del torneo en todos los equipos y el estado de cada una; (3) efecto: la alta y el estado que resulta (activa o `pendiente de verificación`); (4) política efectiva de segundo factor con todos los roles vigentes del actor (IF-12); (5) decidir; (6) aplicar y auditar, o rechazar |
| 6. Cambios indivisibles | Alta del jugador + su estado resultante (`activa` o `pendiente de verificación`) + evento de auditoría. **La comprobación de duplicados y la decisión del estado forman parte de la misma unidad**, de modo que dos altas concurrentes no decidan sobre el mismo estado previo |
| 7. Estados prohibidos | ES-18, ES-21 (alta por actor sin facultad vigente) y ES-10; **parcial:** alta sin estado resultante. **Bajo D:** ES-13 sobre la facultad del actor (la regla de duplicados es de integridad y se lee de la fuente: K-2a) |
| 8. Concurrencia | **ESC-32 (CL-36):** dos altas simultáneas de nombres equivalentes en equipos distintos: a lo sumo una activa y la otra pendiente. **ESC-33:** alta concurrente con la resolución de duplicados o con una baja. **ESC-34:** alta o resolución de duplicados frente a un registro que se anonimiza a la vez (la relación con la detección posterior de duplicados queda abierta). **ESC-42:** baja o paso a pendiente de un jugador concurrente con eventos que le asignan UI-10 o UI-05. **ESC-27:** pérdida de la facultad del actor concurrente |
| 9. Auditoría | La alta y su estado **dentro** de la unidad (IF-10), **solo con identificadores internos y los datos mínimos necesarios** (IF-31: minimización desde el origen, porque el registro no puede reescribirse; plazos y retención: ADR-17, R4, con asesoría jurídica). **Distinción:** un rechazo **técnico** de la operación (autorización o integridad) se registra **fuera** de la unidad (IF-09); en cambio, la **inscripción duplicada rechazada** tras confirmarse que es la misma persona es el **resultado aplicado** de la resolución del organizador (RD-JUG-02 (c); CAND-12) y se audita **dentro** de la unidad de esa resolución |
| 10. Abiertas | **OC-22** («nombre equivalente» y «misma identidad»: sin definir aquí; falsos positivos y negativos de RD-JUG-09); **OC-23** (alta por un actor con varios roles: `01` no enumera el alta entre las clases de RT-14 y A-4 se limita a equipos; se conservan las lecturas alternativas, sin precedencia); **OC-32** (cierre de plantilla, dorsal único, baja y pendiente); RD-JUG-10 (COND Q-41); DB2b-7; DB2b-9. **Complemento:** la **resolución de duplicados** es una operación distinta (matriz §7.1 fila 38; RT-14) y figura como candidata CAND-12 |

#### 19.4.4 Ficha UI-09 — Generar o regenerar el calendario

| Campo | Contenido |
|---|---|
| 1. Disparador | El dueño o el organizador del torneo confirman la generación inicial o la regeneración del calendario, tras una vista previa que no persiste nada (RD-CAL-02) |
| 2. Componentes (R1 §5.5) | CMP-09 + CMP-08 + CMP-16 |
| 3. Requisitos | RD-CAL-02, RD-CAL-07 (REC), RT-14, RF-AUD-06; matriz §7.1 fila 22; A-2, A-8; IF-24, IF-25; RT-05 |
| 4. Autorización | **Servidor:** CMP-08 decide que el actor es dueño u organizador de ese torneo y **evalúa RT-14 sobre todos los partidos que se crean, se archivan o se modifican por efecto** (incluidos los de todos los equipos): un dueño u organizador que **es o ha sido** delegado de **cualquier** equipo inscrito en ese torneo, con la restricción histórica no levantada, no puede confirmar (RT-14; Q-45; RF-ACC-17); la confirma otro dueño u organizador sin conflicto y, si no existe, la operación queda **bloqueada y auditada**. **Capa de datos:** verifica lo mismo sobre el conjunto completo y el estado resultante (todo el calendario o nada). **La vista previa** se permite con la autorización que la operación ya tiene y no concede otra |
| 5. Secuencia lógica | (2) reunir el estado de los partidos (regeneración: **todos** `programado`), las asignaciones de editor, las jornadas publicadas, las delegaciones y las restricciones; (3) efecto: partidos nuevos, partidos archivados, asignaciones de editor perdidas, jornadas publicadas retiradas, **ajustes manuales perdidos** (RD-CAL-07); (4) política efectiva de segundo factor con todos los roles vigentes del actor (IF-12); (5) decidir con RT-14 sobre el conjunto; (6) aplicar y auditar, o rechazar completo |
| 6. Cambios indivisibles | **Todos** los partidos nuevos + archivo de los partidos anteriores (que «no se pierden en la auditoría») + las asignaciones de editor que se pierden (**se desactivan con su historial, no se eliminan**: RF-ACC-06, IF-15) + los **ajustes manuales** que se pierden (su efecto sobre la marca «modificado manualmente» del calendario, RD-CAL-06, no lo define `01`: OC-25) + las jornadas publicadas que se retiran + evento de auditoría + señal de cambio si ADR-06 la adopta. «confirmar crea todos los partidos en una transacción o ninguno» (RD-CAL-02) |
| 7. Estados prohibidos | ES-19, ES-16 (resultado sobre un partido archivado), **ES-23** (asignación de editor vigente sobre un partido archivado) y ES-04 (solo si la operación crea o conserva una asignación sobre un partido de su propio equipo), ES-21, ES-10. **Parcial:** partidos creados sin archivar los anteriores; asignaciones perdidas sin el archivo. **Bajo D:** ES-13 y, **si se elige D**, la cuestión de §19.6.2 (la regeneración modifica asignaciones de editor vigentes, un hecho derivado; OC-24) |
| 8. Concurrencia | **ESC-35:** dos confirmaciones simultáneas: una sola. **ESC-30:** confirmación de resultados o cambio de estado de un partido concurrente con una regeneración: «solo si todos los partidos están en `programado`», por lo que una de las dos se rechaza. **ESC-36:** concesión del rol de delegado o levantamiento concurrentes con un actor dueño u organizador (RT-14 sobre el conjunto). **ESC-37:** reasignación de editor (UI-03) concurrente con la regeneración. **ESC-27:** pérdida de la facultad del actor. **ESC-41** (solo D) |
| 9. Auditoría | La generación o regeneración y sus consecuencias **dentro** de la unidad (RF-AUD-02: «generación, ajuste y publicación de calendario»); el rechazo y el bloqueo (RF-AUD-06) **fuera** (IF-09; K-3 en R2-B2-b2) |
| 10. Abiertas | **OC-24** (solo si se elige D: si la regeneración y las demás operaciones que retiran o alteran asignaciones actualizan la proyección: H2B1-10 no las lista; DB2b-4); **OC-25** (ajustes de calendario fuera de la generación, CAND-18, y ES-04); **A-2** (alcance de «asignaciones que generen conflictos»: **no se resuelve**); **A-8** (publicar y despublicar jornadas: no restringida por RT-14 en esta versión; parámetro); RD-CAL-07 es **[REC]** (su regla puede cambiar sin tocar RT-14). **Relación con UI-03 y ES-04:** una regeneración equivale, en su efecto, a desasignar en bloque (matriz §7.1 fila 7) y no crea asignaciones; por tanto **no produce ES-04 por sí misma**, pero las asignaciones posteriores sobre los partidos nuevos deben verificar RT-05 (IN-02-11), y el cambio de rivales de un partido ya asignado (CAND-18) **sí podría producirlo**. **CAND-22** (publicar o despublicar jornadas; A-8) y **OC-29** (señal de cambio) |

#### 19.4.5 Ficha UI-07 — Anonimizar a un jugador

| Campo | Contenido |
|---|---|
| 1. Disparador | El dueño anonimiza a un jugador (derechos del titular, matriz §7.1 fila 21). **El organizador solo si Q-42 (b) lo habilita** (COND Q-42; hoy PEND); editor y delegado no |
| 2. Componentes (R1 §5.5) | CMP-17 + CMP-10 + CMP-14 + CMP-08 + CMP-16 |
| 3. Requisitos | PRV-06, PRV-02, PRV-18 (abierta), Q-05, CL-26, Q-20 (abierta), Q-42 (b); IN-02-23; IF-27, IF-28; RF-AUD-02 («altas/bajas/anonimización de jugadores»); HR1-11, HR1-12; T-04 |
| 4. Autorización | **Servidor:** CMP-08 decide que el actor tiene la facultad de la fila 21 sobre ese jugador y torneo (la del organizador es **parámetro**, Q-42 (b)). **Capa de datos:** verifica la misma facultad y el **estado resultante en la fuente**: nombre y datos personales borrados, e identificador interno, dorsal, estadísticas y resultados conservados |
| 5. Secuencia lógica | (2) reunir roles, el jugador, sus referencias (estadísticas, resultados, eventos) y las proyecciones derivadas persistentes, si existen; (3) efecto: los datos personales a borrar y lo que se conserva; (4) política efectiva de segundo factor con todos los roles vigentes del actor (IF-12); (5) decidir; (6) aplicar y auditar, o rechazar completo |
| 6. Cambios indivisibles (**de la fuente**) | Borrado irreversible de nombre y datos personales **en Z2/Z3** + el mismo borrado en las **proyecciones derivadas persistentes, si existieran** (ADR-04) + **señal de cambio, solo si ADR-06 la adopta** + evento de auditoría **solo con identificadores internos**. Las estadísticas y los resultados no cambian |
| 7. Estados prohibidos | ES-20, ES-21, ES-10; **parcial:** nombre borrado y datos personales presentes en una derivada persistente; estadísticas alteradas; dato personal en el registro. **Bajo D:** ES-13 sobre la facultad del actor |
| 8. Concurrencia | **ESC-38:** corrección o confirmación de resultados que mencionan al jugador: la anonimización no altera estadísticas ni resultados. **ESC-34:** alta o resolución de duplicados frente a un registro que se anonimiza a la vez (queda abierto cómo afecta a la detección posterior de duplicados). **ESC-39:** ocultar o revocar la autorización del nombre público (reversible) frente a anonimizar (irreversible). **ESC-40:** pérdida de la facultad del actor concurrente (o cambio de la facultad del organizador si Q-42 se habilita) |
| 9. Auditoría | La anonimización **dentro** de la unidad (IF-10), **sin datos personales** (PRV-06, T-04): el evento solo guarda identificadores internos, lo que debe conciliarse con «valores antes/después minimizados» de RF-AUD-01 (tensión que se **registra**, no se resuelve). El rechazo, **fuera** (IF-09) |
| 10. Abiertas | **OC-19** (Q-42 (b)); **OC-26** (alcance de «Anonimizar/eliminar» en la fila 21 de la matriz); **OC-27** (Q-20 abierta: mientras lo esté «no se publica ningún dato personal»; PRV-18; asesoría jurídica pendiente); **OC-28** (qué cuentan como datos personales en la fuente y si existen derivadas persistentes: ADR-04); HR1-11 y HR1-12 sin resolver (la retención de respaldos corresponde a ADR-12 y ADR-17, R4, con asesoría jurídica); DB2b-8. **Efectos externos que NO forman parte de la unidad:** propagación a Z1 y a copias en Z0, respaldos, exportaciones anteriores (§19.3) |

### 19.5 Operaciones candidatas que no tienen unidad propia

R1 §5.5 declara la lista **no exhaustiva** y deja la enumeración definitiva por operación a la Fase 4. Las siguientes operaciones **modifican datos que alimentan o dependen de las cinco unidades, o retiran o alteran asignaciones y delegaciones** y **no tienen unidad propia en UI-01…13**. Se presentan **solo como candidatas**: **sin ficha**, sin añadir alcance y sin decidir si exigen una unidad. Su inclusión es una decisión del usuario (DB2b-5).

| ID | Operación candidata | Fuente | Relación con las unidades de §19.4 |
|---|---|---|---|
| CAND-12 | Resolver la verificación de posibles duplicados de jugadores | Matriz §7.1 fila 38; RD-JUG-02 (incluido su apartado (c): la inscripción duplicada se rechaza y se audita), RD-JUG-08, RD-JUG-09; RT-14 (la enumera expresamente); RF-AUD-02 | Complementa UI-13; ESC-33, ESC-34. **Su evento de auditoría nace solo con identificadores internos** (IF-31) |
| CAND-13 | Baja o edición de un jugador y reinscripción tras una baja | Fila 19; RD-JUG-10 (COND Q-41) | Condiciona UI-13 |
| CAND-14 | Anular un evento ya confirmado | Fila 28; RD-TAB-11 (recálculo) | Mismo recálculo que UI-05 y UI-10 |
| CAND-15 | Declarar o revertir un W.O. | Fila 26; Q-21 (COND); RT-14 | Recálculo; clase «W.O.» de RT-14 |
| CAND-16 | Resolución manual de un desempate | Fila 29; Q-22 (COND); RD-TAB-09; RT-14 | Se vuelve obsoleta si cambian los resultados (UI-05, UI-10) |
| CAND-17 | Cambiar el estado de un partido, reanudar uno suspendido y reabrir uno finalizado | Filas 25, 27 («corregir o reabrir») y 39; Q-46 | Relación con UI-10, UI-05 y ESC-30 |
| CAND-18 | Ajustar fecha, hora, sede, localía y jornada, o intercambiar rivales o mover una jornada fuera de la generación | Fila 23; **RD-CAL-06** (solo en partidos `programado`; se rechaza por completo si alguno es de un equipo del que el actor es delegado); RT-14 («ajustes de calendario que lo afecten»); RT-05 | **Puede producir ES-04** sobre un partido ya asignado (OC-25); su efecto sobre la asignación no lo define `01` (EV-10) |
| CAND-19 | Cambiar el reglamento de un torneo iniciado, con vista previa «antes/después» y recálculo | Fila 12; RD-REG-03 (COND Q-23); A-16 | ESC-29; condiciona el recálculo (OC-20) |
| CAND-20 | Ocultar un nombre o revocar la autorización de nombre público | PRV-02, PRV-06; fila 20; CL-26 | ESC-39 |
| CAND-21 | Finalizar, archivar o reabrir un torneo | Filas 14 y 37; Q-40; D-5 | ESC-28; cambia quién corrige (UI-05); **su efecto sobre las asignaciones no está definido en `01`** (OC-21, EV-10) |
| CAND-22 | Publicar o despublicar un torneo o sus jornadas | Fila 13; A-8 (no restringida por RT-14 en esta versión) | Señal de cambio (ADR-06); UI-09 retira jornadas publicadas |
| CAND-23 | Inscribir o retirar un equipo | Fila 16; RF-EQP-02; RF-EQP-03 (COND Q-26: partidos pendientes W.O. o anulados; efectos en cascada evaluados por RT-14); A-4 (solo decisiones de inscripción o retiro de equipos) | Puede alterar partidos y asignaciones de editor; **efecto sobre las asignaciones no definido** (OC-33, EV-10) |
| CAND-24 | Anular, cancelar o aplazar un partido con editor asignado | Filas 25 y 27; Q-29 (COND: aplazado = reprogramación; cancelado no cuenta y exige motivo); RD-RES-10 | **Efecto sobre la asignación no definido** (OC-33, EV-10); relación con UI-10 y el recálculo |

### 19.6 Garantías exigidas (K-2d aplicado a las operaciones de datos) y relación con CAP-02-A y CAP-02-D

#### 19.6.1 Garantías GT-11…18

**Todas son exigidas: no se implementan, no se aprueban y no se declaran cumplidas.** Se **suman** a GT-01…10 de R2-B2-a (GT-02, 04, 07, 08 y 09 se aplican también a estas unidades).

| ID | Garantía exigida | Unidades | Fuente | Aplica a |
|---|---|---|---|---|
| GT-11 | **Una sola unidad para el resultado:** marcador, eventos, estado, historial, recálculo, derivadas persistentes (si existen), señal de cambio (si ADR-06 la adopta) y auditoría se confirman juntos o ninguno | UI-05, UI-10 | IF-20, IF-22, ES-14, ES-15, ES-17 | A y D |
| GT-12 | **Serialización de las operaciones que compiten por un mismo partido, por una misma clasificación o por un mismo registro** (confirmar, corregir, anular, regenerar, anonimizar, dar de alta): el resultado equivale a alguna ordenación secuencial y la decisión y la aplicación se hacen sobre el mismo estado confirmado. **El control de versión de RD-RES-04 (una operación con versión obsoleta recibe conflicto y no sobrescribe) es una garantía confirmada solo para el marcador y el estado del partido, es decir, para UI-05 y UI-10** (IF-30, ES-22). **Para UI-07, UI-09 y UI-13 no se extiende RD-RES-04:** su necesidad de concurrencia (dos confirmaciones del calendario, ESC-35; dos altas simultáneas, ESC-32; anonimización frente a otras operaciones, ESC-34, 38, 39 y 40) se conserva como garantía **derivada** de IF-05 y GT-02 (y, en UI-13, de RD-JUG-08), sin atribuirle un control de versión que `01` no establece | UI-05, 07, 09, 10, 13 | IF-05, GT-02, ES-16; IF-30 y ES-22 **solo para UI-05 y UI-10** | A y D |
| GT-13 | **Coherencia pública entre resultados y clasificaciones:** exigida a ADR-04.C y cuyo mecanismo decide ADR-06; estas unidades solo aportan **un estado confirmado único**. **No se afirma que se cumpla** | UI-05, UI-07, UI-10 | CL-15, R1 §5.5; ES-14 | A y D |
| GT-14 | **RT-14 se evalúa sobre el conjunto completo de partidos afectados antes de aplicar y se rechaza por completo si alcanza un equipo del que el actor es o ha sido delegado en ese torneo, con la restricción no levantada** (Q-45, RF-ACC-17) | UI-05, UI-09, UI-10 | IF-24, RT-14 | A y D |
| GT-15 | **Todo o nada del calendario:** los partidos nuevos, el archivo de los anteriores, las asignaciones perdidas y las jornadas retiradas se confirman juntos | UI-09 | IF-25, ES-19 | A y D |
| GT-16 | **Unicidad de la alta activa bajo concurrencia, impuesta en la capa de datos** (no solo en la aplicación): la comprobación de duplicados y el estado resultante forman parte de la misma unidad | UI-13 | IF-26, ES-18 | A y D |
| GT-17 | **Completitud de la anonimización sobre la fuente y sus derivadas persistentes,** con los invariantes de PRV-06 (estadísticas y resultados intactos; sin datos personales en la auditoría) | UI-07 | IF-27, ES-20 | A y D |
| GT-18 | **Los efectos externos se identifican y quedan fuera de la unidad;** ninguna atomicidad se les atribuye (§19.3) | Las cinco | IF-22, IF-28 | A y D |

**Qué demuestra esta sección.** Que las propiedades de las cinco unidades pueden **enunciarse** como un conjunto verificable en los escenarios de §19.7. **Qué no demuestra:** que exista una forma de lograrlas, su suficiencia sobre UI-06 (R2-B2-b2), la coherencia pública (ADR-04.C y ADR-06) ni la viabilidad de la composición UI-10 dentro de UI-06.

#### 19.6.2 Relación con CAP-02-A y CAP-02-D (evidencia, sin elegir)

**Reserva.** Esta sección aporta **evidencia** para DB1-1. **No elige** CAP-02-A ni CAP-02-D, **no declara demostrada la independencia de CAP-02-D**, **no declara cumplido R1 §8.1** y **no pretende ser la evaluación consolidada**, que corresponde a R2-B2-b2. DB1-1 y DB1-6 siguen abiertas. Recibir esta evidencia sin resolver DB1-1 es DB2b-6.

| Unidad | Hechos derivados que consume bajo D | Reglas de integridad (sobre las fuentes, A y D) | ¿Actualiza la proyección bajo D? | ES-13 |
|---|---|---|---|---|
| UI-10 | Facultad del actor; asignación vigente al partido; roles y restricciones de RT-14 | Un resultado oficial por partido (integridad de datos) | **No** (modifica datos, K-2c) | **Aplica** |
| UI-05 | Facultad del actor; restricciones de RT-14 | Ídem | No | **Aplica** |
| UI-13 | Facultad del actor | Unicidad de la alta activa (RD-JUG-08) | No | **Aplica** |
| UI-09 | Facultad del actor; asignaciones de editor vigentes; restricciones de RT-14 | Todo o nada del calendario | **ABIERTO (OC-24, DB2b-4), y solo si se elige D:** según RD-CAL-07 ([REC] en `01`) la regeneración retira asignaciones de editor vigentes, un hecho derivado de K-2a; H2B1-10 **no la lista**; **si la proyección se actualiza o no con ella no está decidido** | **Aplica** |
| UI-07 | Facultad del actor | Completitud de la anonimización | No | **Aplica** |

**Inventario conceptual de operaciones que retiran o alteran asignaciones de editor o delegaciones.** Es relevante **solo si se elige CAP-02-D**, cuya condición K-2c exige actualizar la proyección con toda operación que modifica permisos. **No es una lista cerrada** (R1 §5.5; Fase 4).

| Operación | Efecto documentado sobre asignaciones o delegaciones | ¿Listada en H2B1-10? | Tratamiento |
|---|---|---|---|
| UI-01, 02, 03, 04, 08, 11 y 12 | Concesión, retirada, reasignación, revocación y levantamiento | Sí | R2-B2-a (§18.4) |
| Aceptar invitaciones de editor u organizador, designar organizadores, desactivar cuentas y asignar torneos existentes (CAND-02, 04, 10 y 11 de §18.5) | Conceden o retiran roles y asignaciones | Sí | R2-B2-a (§18.5) |
| **UI-09, regeneración** | RD-CAL-07 ([REC] en `01`): las asignaciones de editor se pierden al archivar los partidos anteriores | **No** | §19.4.4; OC-24 (abierta) |
| **CAND-18**, ajustes e intercambio de rivales | Pueden producir ES-04 sobre un partido ya asignado; el efecto sobre la asignación no está definido | **No** | §19.5; OC-25 |
| **CAND-23**, inscribir o retirar un equipo | RF-EQP-03: partidos pendientes W.O. o anulados; efecto sobre asignaciones no definido | **No** | §19.5; OC-33 |
| **CAND-24**, anular, cancelar o aplazar partidos con editor asignado | Q-29: efecto sobre la asignación no definido | **No** | §19.5; OC-33 |
| **CAND-21**, finalizar, archivar o reabrir un torneo | Efecto sobre las asignaciones no definido (Q-40 (c)) | **No** | §19.5; OC-21 |
| CAND-05 (primera asignación de editor) | Crea una asignación (R2-B2-a) | No consta explícitamente | §18.5 |

**Qué ocurre si, bajo D, la proyección no se actualiza con alguna de estas operaciones.** La proyección seguiría concediendo una asignación o una delegación ya retirada o alterada: una decisión tomada sobre un hecho obsoleto (ES-13; GT-09), con riesgo de operar sobre permisos obsoletos (y de ES-23 si el partido está archivado). **Se exponen las alternativas sin seleccionar una:** (i) actualizar la proyección en la misma unidad, ampliando K-2c; (ii) leer esas asignaciones de su fuente, lo que **contradiría K-2a** (que las incluye en la proyección) y sería una variante no evaluada; (iii) no elegir CAP-02-D. **Bajo CAP-02-A no existe proyección y la cuestión no se plantea.** K-2a, K-2c, K-2d y ES-13 siguen como **condiciones exigidas** si se elige D; este documento no modifica K-2a ni H2B1-10 ni las matrices de R2-B1.

| ID | Observación | Alcance | Naturaleza |
|---|---|---|---|
| EV-09 | Las cinco unidades **consumen** hechos derivados (como mínimo la facultad del actor): **ES-13 aplica a las cinco bajo D**, aunque modifiquen datos y no permisos | Las cinco | Aplicación de §18.8.1.1 |
| EV-10 | **Hallazgo condicional (aplica solo si se elige CAP-02-D); la actualización de la proyección permanece ABIERTA (OC-24, DB2b-4):** las operaciones que retiran o alteran asignaciones de editor o delegaciones vigentes —la regeneración (RD-CAL-07, que es **[REC]** en `01`, de modo que la pérdida de asignaciones depende de esa recomendación) y, según su efecto sobre las asignaciones, CAND-18, CAND-21, CAND-23 y CAND-24— afectan hechos derivados de la proyección (K-2a). H2B1-10 lista las operaciones que actualizarían la proyección (K-2c) y **no las incluye**. **Si** se decidiera reflejarlas, tendrían que hacerlo en la misma unidad (GT-01, GT-09) o la proyección quedaría obsoleta (ES-13); **si** se mantuvieran fuera, habría que leer esas asignaciones de su fuente, lo que contradiría K-2a. **No decide si D se elige ni cómo se completa K-2c** | UI-09 y las candidatas citadas | Nueva; condicional a D; condiciona K-2c (OC-24; DB2b-4) |
| EV-11 | Las reglas de integridad de las cinco unidades (resultado oficial único, alta activa única, calendario completo, anonimización completa) son de **datos**, no de permisos: no figuran en K-2a ni en IN-02 y se verifican sobre las fuentes **igual bajo A y bajo D** | Las cinco | Aplicación de K-2a |
| EV-12 | Salvo EV-10, **D no añade pasos a estas unidades** respecto de A: ambas capas verifican la misma decisión (IF-03, IF-16). **No aporta evidencia nueva sobre el criterio de independencia de §17.3.3** (qué verifica cada capa; composición de las unidades) más allá de EV-10. La independencia de D **sigue sin estar demostrada**, D **no se cuenta como segunda forma**, **R1 §8.1 no se declara cumplido** y **DB1-1 sigue abierta** | Las cinco | Contraste; sin conclusión |

### 19.7 Matriz de concurrencia

«Admisible» significa que **equivale a alguna ordenación secuencial** (IF-05). Las garantías GT- se definen en §18.7 y §19.6.1. **Origen:** son **literales** en `01` ESC-32 (CL-36, RD-JUG-08) y, en lo que dicen RD-RES-04 y RD-RES-06 (la segunda sesión recibe conflicto y no sobrescribe), ESC-24 y ESC-26; son **[DER]** (el texto de `01` no menciona esa simultaneidad) ESC-25, 27…31, 33…40 y 42; ESC-41 es **propio de CAP-02-D**. **Ningún escenario se resuelve aquí por un mecanismo.**

| ID | Escenario | Unidades | Origen | Resultados admisibles | Resultados prohibidos | Garantías |
|---|---|---|---|---|---|---|
| ESC-24 | Dos confirmaciones del mismo partido sobre la misma versión (dos dispositivos o dos actores) | UI-10 × UI-10 | RD-RES-04 [CONF]; RD-RES-07, CL-15 [DER] | Una resulta oficial; **la segunda se rechaza con conflicto de versión, sin sobrescribir y devolviendo la versión actual**; una confirmación posterior sobre un partido ya finalizado no es una corrección (el editor no corrige) | Dos resultados oficiales (ES-16); sobrescritura silenciosa o corrección encubierta (ES-22) | GT-11, GT-12 |
| ESC-25 | Confirmaciones o correcciones de partidos distintos que alteran la misma clasificación | UI-10 / UI-05 × UI-10 / UI-05 | CL-15, RD-TAB-11 [DER] | La clasificación final refleja **ambos** cambios en un orden secuencial | Una clasificación que refleja solo uno (ES-14) | GT-11, GT-12, GT-13 |
| ESC-26 | Dos correcciones del mismo partido sobre la misma versión (dueño y organizador) | UI-05 × UI-05 | RD-RES-04, RD-RES-06 [CONF] | Una prevalece; **la otra recibe conflicto de versión y no sobrescribe; solo se aplica una segunda corrección con la versión actualizada, su motivo y su historial** | Dos resultados oficiales (ES-16); sobrescritura silenciosa (ES-22); historial perdido (ES-15) | GT-11, GT-12 |
| ESC-27 | El actor pierde su facultad o su asignación mientras confirma, corrige, da de alta, regenera o anonimiza | UI-10 / UI-05 / UI-13 / UI-09 / UI-07 × UI-11 / UI-03 / UI-04 | RT-13, CL-45 [DER] | Aplicada antes y válida, o rechazada | Aplicada con la autorización ya revocada y confirmada (ES-21, ES-10) | GT-02, GT-04, GT-12 |
| ESC-28 | El torneo pasa a `archivado` mientras un organizador corrige | UI-05 × CAND-21 | D-5, CL-44 [DER] | Corrección aplicada antes o rechazada | Corrección del organizador aplicada con el torneo `archivado` (ES-16, ES-21) | GT-02, GT-12 |
| ESC-29 | Cambio de reglamento concurrente con una corrección o una confirmación (**depende de Q-23**) | UI-05 / UI-10 × CAND-19 | RD-REG-03 [DER] | Recálculo con una versión de reglamento coherente en un orden secuencial | Clasificación mezclada de dos versiones (ES-14) | GT-11, GT-12 |
| ESC-30 | Regeneración del calendario concurrente con una confirmación, una corrección o un cambio de estado de un partido | UI-09 × UI-10 / UI-05 / CAND-17 | RD-CAL-07 (REC) [DER] | La regeneración exige todos los partidos `programado`: una de las dos se rechaza | Un resultado oficial sobre un partido archivado (ES-16, ES-19) | GT-12, GT-15 |
| ESC-31 | Concesión del rol de delegado o levantamiento concurrentes con un dueño u organizador que confirma o corrige | UI-10 / UI-05 × UI-02 / UI-12 / UI-08 | RT-14, ESC-09, ESC-10 [DER] | Aplicada antes y válida, o rechazada por completo | Operación aplicada con la restricción vigente y confirmada (ES-10) | GT-02, GT-14 |
| ESC-32 | Dos altas simultáneas de nombres equivalentes en equipos distintos | UI-13 × UI-13 | CL-36, RD-JUG-08 | A lo sumo una activa; la otra queda `pendiente de verificación`; el delegado no ve datos del otro equipo | Dos altas activas sin decisión del organizador (ES-18) | GT-16 |
| ESC-33 | Alta concurrente con la resolución de duplicados o con una baja del mismo jugador | UI-13 × CAND-12 / CAND-13 | RD-JUG-08, RD-JUG-10 [DER] | Equivale a un orden secuencial; una identidad confirmada activa en un solo equipo | Una identidad activa en dos equipos (ES-18) | GT-12, GT-16 |
| ESC-34 | Una alta o una resolución de duplicados concurrente con la anonimización del registro con el que se compara | UI-13 / CAND-12 × UI-07 | PRV-06, RD-JUG-08 [DER] | Equivale a un orden secuencial; **qué ocurre con la detección de duplicados cuando el registro de comparación ya está anonimizado queda abierto (OC-22, OC-26)** | Un jugador anonimizado a medias (ES-20) | GT-12, GT-17 |
| ESC-35 | Dos confirmaciones simultáneas del calendario | UI-09 × UI-09 | RD-CAL-02 [DER] | Una sola generación resulta vigente | Dos generaciones vigentes (ES-19) | GT-12, GT-15 |
| ESC-36 | Concesión del rol de delegado o levantamiento concurrentes con un dueño u organizador que genera o regenera | UI-09 × UI-02 / UI-12 / UI-08 | RT-14, RD-CAL-07 [DER] | Aplicada antes y válida, o rechazada por completo | Una generación confirmada con un partido de su equipo (ES-19) | GT-02, GT-14, GT-15 |
| ESC-37 | Reasignación o primera asignación de editor concurrente con la regeneración | UI-09 × UI-03 / CAND-05 | RD-CAL-07, RF-ACC-06 [DER] | La asignación se aplica antes y la regeneración la desactiva con su historial, o la asignación se rechaza | Un editor vigente sobre un partido archivado (ES-23, ES-16) | GT-12, GT-15 |
| ESC-38 | Anonimización concurrente con una corrección o una confirmación de resultados que mencionan al jugador | UI-07 × UI-05 / UI-10 | PRV-06 [DER] | Estadísticas y resultados intactos tras ambas, en un orden secuencial | Estadísticas alteradas por la anonimización (ES-20) | GT-12, GT-17 |
| ESC-39 | Anonimización concurrente con ocultar un nombre o revocar la autorización de nombre público | UI-07 × CAND-20 | PRV-06, CL-26 [DER] | La anonimización, irreversible, prevalece sobre lo reversible | Datos personales restaurados tras anonimizar (ES-20) | GT-12, GT-17 |
| ESC-40 | Anonimización concurrente con la pérdida de la facultad del actor o con un cambio de la facultad del organizador (**depende de Q-42**) | UI-07 × UI-11 | RT-13, Q-42 [DER] | Aplicada antes y válida, o rechazada | Anonimización por un actor sin facultad vigente (ES-21) | GT-02, GT-04, GT-17 |
| ESC-41 | (**solo CAP-02-D**) Una decisión de cualquiera de las cinco unidades coincide con la regeneración que retira asignaciones de editor, **en el supuesto de que se decidiera que actualiza la proyección** (OC-24, DB2b-4) | UI-09 × cualquier unidad | K-2d, EV-10 | La decisión ve el estado anterior completo o el posterior completo | Decidir sobre una proyección que aún concede una asignación retirada (ES-13) | GT-09, GT-10 |
| ESC-42 | Baja o paso a `pendiente de verificación` de un jugador concurrente con una confirmación o una corrección que le asigna eventos | UI-13 / CAND-13 × UI-10 / UI-05 | RD-JUG-02 (e) [CONF], RD-JUG-04 [REC] [DER] | Equivale a un orden secuencial; un evento nuevo para un jugador en baja o pendiente se rechaza | Un evento aplicado a un jugador en baja o pendiente (ES-24) | GT-12, GT-16 |

### 19.8 Matriz de dependencias por unidad

| Unidad | Depende de lo aprobado | ADR-04.C (R2-C) | ADR-05.C (R2-C) | ADR-09.C | R3 (mecanismo) | Fase 4 |
|---|---|---|---|---|---|---|
| UI-10 | IF-03, 05, 09, 10, 11; ES-04, 13; GT-02, 04, 09 | Coherencia pública y proyecciones | Composición dentro de UI-06 (**R2-B2-b2**) | Persistencia de rechazos (K-3) | ADR-06 (señal de cambio), ADR-08 (ubicación del motor) | Pruebas de carrera; enumeración |
| UI-05 | Ídem; DB1-1 | Ídem | — | Ídem | Ídem | Ídem; Q-23 y reglas |
| UI-13 | IF-03, 13; GT-02 | — | — | Registro de la alta | Mecanismo de la unicidad | Definición de «nombre equivalente» |
| UI-09 | IF-11, IF-24, IF-25; **ES-23** (asignación de editor vigente sobre un partido archivado) y **ES-04** (solo si una operación crea o conserva una asignación sobre un partido del propio equipo del actor); GT-02, GT-14, GT-15; ESC-09, 10 | — | — | Registro de la regeneración y del rechazo | Mecanismo del todo o nada | Enumeración; ajustes de calendario (CAND-18) |
| UI-07 | IF-03; GT-02 | Existencia de derivadas persistentes | Copias en Z0 (HR1-11) | Auditoría sin datos personales | ADR-06 (copias) | Asesoría jurídica; Q-20 |

**Dependencias funcionales** (decisiones del usuario o de la Fase 4 que afectan a las unidades): A-1 (UI-05, UI-10), A-2 y A-8 (UI-09), Q-21, Q-22, Q-23 y Q-29 (recálculo), Q-40 (c) (UI-05), Q-41 (UI-13), Q-42 (UI-07, UI-10), Q-20 y PRV-18 (UI-07). **Dependencias de estado** (Fase 4): cierre de plantilla (RD-JUG-05, COND Q-31) y dorsal único (RD-JUG-03, REC) para UI-13, y jugadores en baja (RD-JUG-04, REC) o pendientes (RD-JUG-02 (e)) para UI-05 y UI-10 (OC-32, ES-24, ESC-42). **Retención de respaldos (HR1-12):** ADR-12 y ADR-17 (R4), con asesoría jurídica; no se define aquí.

### 19.9 Condiciones, decisiones y riesgos

#### 19.9.1 Condiciones abiertas (OC-)

| ID | Condición abierta | Unidades | Relación |
|---|---|---|---|
| OC-18 | A-1: alcance de «resultados» en RT-14 (lectura provisional de `01`: toda operación que cree o modifique datos de resultado de partidos del propio equipo); **parámetro, no se elige** | UI-05, UI-10 | DB2b-1; IF-17 |
| OC-19 | Q-42: si el organizador confirma resultados (a) o anonimiza jugadores (b); **parámetro del evaluador** | UI-10, UI-07 | DB2b-2 |
| OC-20 | Q-21, Q-22, Q-23 y Q-29 (W.O., resoluciones manuales, reglamento, estados) y si ADR-04 y ADR-08 **materializan** la clasificación o la calculan al servir; la unidad se documenta para ambas lecturas | UI-05, UI-10 | DB2b-3 |
| OC-21 | Q-40 (c): qué otras operaciones se bloquean con el torneo `archivado` | UI-05 | DB2b-3 |
| OC-22 | Definición de «nombre equivalente» y de «misma identidad» (falsos positivos y negativos de RD-JUG-09) | UI-13 | DB2b-7; Fase 4 |
| OC-23 | Alta de jugador por un actor con varios roles (dueño u organizador y delegado): RT-14 no enumera el alta y A-4 se limita a las decisiones de inscripción o retiro de **equipos**. **Lecturas alternativas, sin precedencia:** (i) unión de roles (RT-11) sin restricción adicional; (ii) el criterio provisional de A-15 de leer con denegación por defecto las acciones sensibles no cubiertas expresamente (A-15 está sin resolver y ese criterio no es política definitiva) | UI-13 | DB2b-9 |
| OC-24 | **Solo si se elige D:** si la regeneración y las demás operaciones que retiran o alteran asignaciones de editor o delegaciones (CAND-18, 21, 23, 24) actualizan la proyección (H2B1-10 no las lista) o si esas asignaciones se leen de la fuente (lo que contradiría K-2a) | UI-09 y candidatas | DB2b-4; EV-10 |
| OC-25 | Ajustes de calendario fuera de la generación y cambio de rivales (CAND-18) y su relación con ES-04 | UI-09 | DB2b-5 |
| OC-26 | Alcance de «Anonimizar/eliminar» (fila 21 de la matriz) | UI-07 | DB2b-8 |
| OC-27 | Q-20, PRV-18 y asesoría jurídica: no se publica ningún dato personal mientras Q-20 esté abierta | UI-07 | HR1-11, HR1-12; DB2b-8 |
| OC-28 | Qué cuentan como datos personales en la fuente y si existen derivadas persistentes (ADR-04) | UI-07 | R2-C |
| OC-29 | Si ADR-06 adopta una señal de cambio y su plazo (RF-PUB-09 cubre resultados, no cambios de privacidad) | UI-05, UI-07, UI-09, UI-10 | R3 |
| OC-30 | Composición de UI-10 dentro de UI-06 para el resultado sincronizado | UI-10 | **R2-B2-b2** |
| OC-31 | Si el recálculo de la clasificación de equipos que no son del actor amplía el alcance de RT-14: RT-14 define «modificar» sobre **partidos**; la clasificación derivada no es un partido y `01` no dice si su recálculo cuenta; **parámetro** (A-1, A-16); no se extiende RT-14 | UI-05, UI-10 | DB2b-1 |
| OC-32 | Dependencias de estado de la plantilla: cierre (RD-JUG-05, COND Q-31), dorsal único (RD-JUG-03, REC), jugadores en baja (RD-JUG-04, REC) o pendientes (RD-JUG-02 (e)) | UI-13, UI-05, UI-10 | Fase 4; ES-24, ESC-42 |
| OC-33 | Efecto sobre las asignaciones de editor de inscribir o retirar un equipo (RF-EQP-03, COND Q-26) y de anular, cancelar o aplazar partidos (Q-29): `01` no lo define | CAND-23, CAND-24 | Fase 4; EV-10 |

#### 19.9.2 Decisiones propuestas al usuario (DB2b-)

Ninguna se resuelve aquí. Las DB1-1…DB1-6, DB2a-1…DB2a-10 y las decisiones de R2-A (13 originales, la 11 atendida y 12 abiertas) permanecen **como estaban**.

| ID | Decisión | Opciones | Efecto |
|---|---|---|---|
| DB2b-1 | Lectura de A-1 y alcance de RT-14 sobre resultados y sobre la clasificación de equipos ajenos (OC-18, OC-31) | La provisional de `01` / otra lectura / decidirlo en la Fase 4 | Fija el alcance de RT-14 sobre resultados |
| DB2b-2 | Facultades del organizador (Q-42 a y b) (OC-19) | Las opciones de `01 §13` / decidirlo en la Fase 4 | Quién confirma y quién anonimiza |
| DB2b-3 | Lecturas de Q-21, Q-22, Q-23, Q-29 y Q-40 (c) (OC-20, OC-21) | Las opciones de `01 §13` | Qué alimenta el recálculo y qué se bloquea en `archivado` |
| DB2b-4 | **Solo si se elige D:** cómo completar K-2c con las operaciones que retiran o alteran asignaciones o delegaciones (OC-24) | (a) incluirlas entre las que actualizan la proyección en la misma unidad (consecuencia: más pasos de mantenimiento, cubiertos por GT-01, GT-09 y ES-13); (b) dejarlas fuera y declararlo (consecuencia: la proyección quedaría obsoleta tras ellas, ES-13 y ES-23, salvo leerlas de la fuente, lo que contradice K-2a: variante no evaluada); (c) decidirlo solo si se elige D | No elige D; no modifica K-2a, H2B1-10 ni las matrices de R2-B1 |
| DB2b-5 | Candidatas de §19.5 para la enumeración de la Fase 4 (OC-25) | Aceptar la lista / ampliarla / reducirla | Alcance de la Fase 4 |
| DB2b-6 | Recibir §19.6.2 como **evidencia** para DB1-1 sin resolverla | Recibir / pedir más análisis | No modifica DB1-1 |
| DB2b-7 | Criterio de «nombre equivalente» y «misma identidad» (OC-22) | Decidirlo en la Fase 4 con Q-12 / fijarlo antes | Precisa UI-13 |
| DB2b-8 | Alcance de la anonimización, Q-20 y asesoría jurídica (OC-26, OC-27) | Las lecturas de `01` / decidirlo con asesoría jurídica | Alcance de UI-07 |
| DB2b-9 | Alta de jugador por un actor con varios roles (OC-23) | (a) unión de roles sin restricción adicional (RT-11); (b) el criterio provisional de denegación por defecto de A-15; (c) decidirlo en la Fase 4. **Sin precedencia entre ellas** | Delimita UI-13 frente a RT-14 |

#### 19.9.3 Riesgos documentales (sin aceptar)

Los riesgos se **registran sin valorar ni aceptar**: la valoración y la aceptación son del usuario. Ninguno sustituye a RB1-01…13, RB2a-01…11 ni a los de R2-A.

| ID | Riesgo | Unidades | Relación |
|---|---|---|---|
| RB2b-01 | Una ventana pública entre un resultado confirmado y su clasificación, porque la unidad no cubre la entrega (no se atribuye atomicidad a lo externo) | UI-05, UI-10 | GT-13, ES-14 |
| RB2b-02 | Dos recálculos concurrentes producen una clasificación que no refleja ambos | UI-05, UI-10 | ESC-25 |
| RB2b-03 | Bajo D, la regeneración y otras operaciones que retiran o alteran asignaciones o delegaciones podrían dejar permisos derivados obsoletos en la proyección si esta no las refleja (OC-24) | UI-09 y candidatas | EV-10, ESC-41, ES-13, ES-23 |
| RB2b-04 | Un efecto en cascada sobre partidos de otros equipos no se evalúa con RT-14 y el actor en conflicto opera | UI-05, UI-09, UI-10 | GT-14 |
| RB2b-05 | Alta duplicada o rechazo indebido porque «nombre equivalente» no está definido | UI-13 | OC-22 |
| RB2b-06 | Anonimización incompleta en derivadas persistentes o en la auditoría; copias en Z0 y Z1 y respaldos conservan datos | UI-07 | HR1-11, HR1-12, GT-17 |
| RB2b-07 | Una facultad pendiente (Q-42) se habilita por omisión en la implementación | UI-07, UI-10 | OC-19 |
| RB2b-08 | Una clasificación recalculada con una versión de reglamento no decidida | UI-05, UI-10 | OC-20, ESC-29 |
| RB2b-09 | Una corrección o un resultado aplicados con el torneo ya `archivado` | UI-05, UI-10 | ESC-28, OC-21 |
| RB2b-10 | La composición con UI-06 (resultado sincronizado) contradice lo documentado para la ruta en línea | UI-10 | OC-30; **R2-B2-b2** |
| RB2b-11 | Una segunda confirmación o corrección sobrescribe en silencio, o una confirmación posterior actúa como corrección encubierta | UI-05, UI-10 | IF-30, ES-22, ESC-24, ESC-26 |
| RB2b-12 | Eventos de auditoría de jugadores escritos con datos personales que, por ser de solo anexado, ya no pueden retirarse | UI-05, UI-07, UI-13, CAND-12 | IF-31, ES-20 |

### 19.10 Secciones reservadas a R2-B2-b2 (no redactadas)

| Reservado | Contenido | Dependencia con esta parte |
|---|---|---|
| Ficha UI-06 | Aplicar una operación recibida (autorizar, idempotencia, aplicar, auditar) | UI-10 dentro de UI-06 (OC-30, RB2b-10) |
| Frontera completa de sincronización | «Antes» y «después» (OFF-22, D-2), orden, contigüidad e idempotencia | OC-04 de R2-B2-a |
| Auditoría de rechazos y K-3 | Persistencia del rechazo por unidad (CAP-09-P) | IF-09; decisión 10 de R2-A |
| Síntesis final de K-2d | Evidencia de b1 y b2 | GT-01…18 |
| Evaluación consolidada de CAP-02-A/D | Conclusión sobre DB1-1, sin elegir | §18.8, §19.6.2 |
| Modelo adicional de amenazas | AM-18, CAB-05, AM-19, AM-34 y las amenazas de las unidades de datos | §18.12.1 |

### 19.11 Trazabilidad y criterios de aceptación

| Elemento | Origen |
|---|---|
| IF-20…31 | RD-RES-04, 05, 06, 07; RD-TAB-10, 11; RD-CAL-02, 07; RD-JUG-08; PRV-06; RF-AUD-01, 03; RT-14; Q-45; R1 §5.5 y §5.9. IF-12 (política de 2FA) de §18.2 |
| ES-14…24 | CL-15, 22, 36; RD-RES-04, 06; RD-CAL-02; RD-JUG-02, 04, 08; PRV-06 |
| Fichas §19.4 | R1 §5.5 (UI-05, 07, 09, 10 y 13) |
| ESC-24…42 | CL-15, 22, 36, 44, 45; RD-RES-04; RD-CAL-07; RD-JUG-02, 04; derivados marcados [DER] |
| GT-11…18 | K-2d (§17.3), §18.7, R1 §5.5 |
| CAND-12…24 | R1 §5.5 (lista no exhaustiva), matriz §7.1, RT-14, RD-CAL-06, RF-EQP-03, Q-26, Q-29 |

| ID | Criterio de aceptación | Cumplimiento según el autor (a revisar por el usuario) |
|---|---|---|
| CR2B2b1-01 | Cinco fichas completas con los diez campos | Cumple: §19.4 |
| CR2B2b1-02 | Componentes idénticos a R1 §5.5 | Cumple: comprobación de §19.12 |
| CR2B2b1-03 | Autorización en ambas capas | Cumple: campo 4 de cada ficha |
| CR2B2b1-04 | Fronteras atómicas documentadas | Cumple: campo 6 y §19.6.1 |
| CR2B2b1-05 | RT-14 aplicado al efecto completo, incluida la persistencia histórica y el levantamiento | Cumple: IF-24 ("es o ha sido" delegado; Q-45; RF-ACC-17), GT-14, UI-05, UI-09 y UI-10 |
| CR2B2b1-06 | Estados intermedios prohibidos y escenarios concurrentes identificados | Cumple: §19.2 y §19.7 |
| CR2B2b1-07 | UI-05 y UI-10 coherentes entre sí | Cumple: IF-20, IF-21, GT-11; relación explícita en la ficha UI-05 |
| CR2B2b1-08 | UI-07 sin atribuir atomicidad a copias externas | Cumple: IF-28, §19.3 y campo 10 de UI-07 |
| CR2B2b1-09 | Ninguna decisión abierta resuelta unilateralmente | Cumple: §19.9; DB1, DB2a, OC-01…17 y las de R2-A intactas |
| CR2B2b1-10 | R1, R2-A, R2-B1 y R2-B2-a intactos | Cumple según §19.12 |
| CR2B2b1-11 | Solo el archivo autorizado modificado | Cumple según §19.12 |
| CR2B2b1-12 | Borrador pendiente de revisión y aprobación | Cumple: §19.0 y §19.13 |
| CR2B2b1-13 | Control de versión (RD-RES-04) sin sobrescritura ni corrección encubierta | Cumple: IF-30, ES-22, ESC-24, ESC-26 |
| CR2B2b1-14 | Auditoría de jugadores con minimización desde el origen | Cumple: IF-31, UI-05, UI-13, UI-07 y CAND-12 |

### 19.12 Verificación del autor

**Naturaleza.** Verificaciones **mecánicas de lectura** hechas por el autor con un guion de solo lectura. **No son una auditoría independiente:** el autor no puede verificar su propio trabajo con independencia. La **auditoría de la v0.1 (§19.14)** fue de un subagente de contexto nuevo, independiente **solo de contexto**: mismo tipo de modelo, sin independencia organizativa ni externa. La **revisión focalizada de la v0.2 (§19.15)** fue de otro subagente de contexto nuevo, también independiente solo de contexto. La **verificación final de la v0.3** (nota de cierre de §19.15) fue de un tercer subagente de contexto nuevo, también independiente solo de contexto. V-1…V-12 son las comprobaciones del autor sobre el **contenido aprobado** (SHA-256 `f72d1170…d0db7`); no se actualizaron tras añadir los metadatos de aprobación.

| # | Verificación | Resultado |
|---|---|---|
| V-1 | Las líneas 1 a 3171 son **idénticas** a la versión confirmada `5bfc930` (R1, R2-A, R2-B1 y R2-B2-a intactos); el cambio son solo líneas añadidas | Cumple: **449 líneas añadidas y 0 eliminadas** |
| V-2 | Solo cambió `02-arquitectura-adr.md`; `00`, `01`, `aprobaciones.md` y `bitacora.md` no cambiaron | Cumple (`git status` y sumas SHA-256) |
| V-3 | Identificadores definidos y sin referencias sin definición: IF-20…31, ES-14…24, GT-11…18, ESC-24…42, OC-18…33, DB2b-1…9, RB2b-01…12, CAND-12…24, EV-09…12, CR2B2b1-01…14 | Cumple (guion de lectura; IF-23, IF-29 y GT-18 específicos o transversales) |
| V-4 | Las cinco fichas tienen los diez campos y sus componentes coinciden **literalmente** con R1 §5.5 | Cumple |
| V-5 | Las citas literales entre «» se buscaron en `00`, `01` y las partes previas | Todas existen, **salvo** los mensajes del usuario citados en §19.0, que proceden de la conversación y no de un archivo |
| V-6 | Exploración de términos tecnológicos (SQL, RLS, trigger, lock, índice, constraint, API, endpoint, nombres de productos o proveedores, token, caché y similares) | Fuera de esta propia fila, una sola aparición, en X-6, como **exclusión**; ninguna elección ni mecanismo |
| V-7 | Ninguna decisión abierta aparece resuelta: no se elige A ni D; no se declara demostrada la independencia de D ni cumplido R1 §8.1; DB1-1…6, DB2a-1…10, OC-01…17 y las decisiones de R2-A (13 originales, la 11 atendida y 12 abiertas) intactas; OC-18…33 y DB2b-1…9 abiertas | Cumple (lectura del autor) |
| V-8 | Los permisos de UI-07 se comprobaron contra la matriz §7.1 de `01` (fila 21) y los de las demás unidades contra las filas 19, 22, 24, 25 y 27 | Cumple (lectura directa del autor) |
| V-9 | **RT-14 y RD-RES-04:** IF-24, ES-19, GT-14 y los campos 4 de UI-05, UI-09 y UI-10 reflejan "es o ha sido delegado", la persistencia histórica y el levantamiento (RT-14, Q-45, RF-ACC-17); ESC-24 y ESC-26 exigen conflicto de versión sin sobrescritura (RD-RES-04) | Cumple (comparación del autor con el texto de RT-14 y RD-RES-04) |
| V-10 | **CAP-02-D:** §19.6.2 condiciona EV-10, OC-24 y DB2b-4 a la elección de D, expone las alternativas sin seleccionar una y no modifica K-2a, H2B1-10 ni las matrices de R2-B1 | Cumple (lectura del autor) |
| V-11 | Ninguna garantía se presenta como implementada; los efectos externos de §19.3 quedan fuera de la unidad; no se define retención ni plazo alguno | Cumple (lectura del autor) |
| V-12 | **D-01, D-02 y D-03:** GT-12 limita el control de versión de RD-RES-04 a UI-05 y UI-10; la actualización de la proyección ante la regeneración figura como ABIERTA (OC-24, DB2b-4) con RD-CAL-07 como [REC]; §19.8 usa ES-23 y ES-04 conforme al catálogo | Cumple (lectura del autor; sin revisión posterior) |

**Límites.** Las **lecturas** de los requisitos y de los casos CL son del autor; los escenarios [DER] están rotulados en §19.7 y deben revisarse. **El hallazgo EV-10** (las operaciones que retiran o alteran asignaciones y delegaciones no figuran en H2B1-10) es una observación del autor, condicional a D, que requiere revisión. R1 §8.1 **sigue sin cumplirse** (DB1-1 abierta).

### 19.13 Estado de esta parte y lo que no se hizo

**Estado: v0.3-R2B2b1, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-19; §19.0).** Registrado en `aprobaciones.md` y `bitacora.md`; **no versionado**.

**Qué se hizo.** La Parte V (§19) contiene las cinco fichas de las operaciones de datos, los invariantes y estados prohibidos adicionales, los efectos externos, las garantías GT-11…18, la relación con CAP-02-A y CAP-02-D como evidencia condicional, la matriz de concurrencia, las dependencias, las condiciones, decisiones y riesgos. La v0.2 corrigió los hallazgos C-01…C-15 de la auditoría (§19.14) y la v0.3 los residuos D-01…D-03 de la revisión focalizada (§19.15).

**Qué no se hizo.** No se redactó R2-B2-b2 (UI-06, sincronización, auditoría de rechazos, síntesis de K-2d, evaluación consolidada y amenazas), R2-C, R3 ni R4; no se aprobó R2-B2 completo, R2 ni la Fase 2; no se resolvió ninguna DB1, DB2a ni DB2b ni OC ni decisión de R2-A; no se eligió formulación; no se declaró demostrada la independencia de D ni cumplido R1 §8.1; no se fijó PAR ni se aceptó RES o RB; no se concedió excepción a D-2; no se definió retención ni plazo; no se modificó el contenido técnico aprobado; los registros de `aprobaciones.md` y `bitacora.md` y las frases de estado de §17.0 y §18.0 son metadatos de aprobación; no se crearon ramas, commits, pushes, PR ni merges; no se tocó `main`.

**Git.** Rama `claude/fase-2-arquitectura-amenazas`; HEAD `5bfc9305e29ab3c3988e932e4f62bfa3d5278f2b`; los cambios (esta Parte V, las frases de estado de §17.0 y §18.0 y los registros) están **sin confirmar**; el SHA-256 del contenido aprobado figura en §19.0. Cualquier commit, push o PR requiere autorización expresa y separada.

### 19.14 Hallazgos de la auditoría de R2-B2-b1 v0.1 (C-01…C-16) y correcciones de la v0.2

**Origen.** Auditoría de solo lectura de un subagente de contexto nuevo (independencia **solo de contexto**), dictamen «REQUIERE CORRECCIONES MATERIALES». Clases: **DM** defecto material; **DAL** decisión abierta; **DEP** dependencia posterior; **EDI** editorial.

| ID | Sev. | Clase | Corrección aplicada en la v0.2 | Dónde |
|---|---|---|---|---|
| C-01 | Media | DM | IF-24, ES-19, GT-14 y los campos 4 de UI-05, UI-09 y UI-10 reflejan "es o ha sido" delegado, la persistencia histórica y el levantamiento; se cita Q-45 y RF-ACC-17; A-1 y A-12 siguen como parámetros | §19.2, §19.4, §19.6.1 |
| C-02 | Media | DM | IF-30 y ES-22; RD-RES-04 en UI-10 y UI-05; ESC-24 y ESC-26 exigen conflicto de versión sin sobrescritura y sin corrección encubierta | §19.2, §19.4.1, §19.4.2, §19.7 |
| C-03 | Media | DM | ES-23 [DER] (asignación vigente sobre partido archivado) en UI-09 y ESC-37; ES-16 solo impide la corrección en `archivado` a quien no es el dueño | §19.2, §19.4.4, §19.7 |
| C-04 | Media | DM | §19.6.2, EV-10, OC-24 y DB2b-4 condicionados a la elección de D, con las alternativas y consecuencias sin seleccionar una | §19.6.2, §19.9 |
| C-05 | Media | DM | CAND-23 y CAND-24 nuevas; CAND-18 y CAND-21 vinculadas; inventario conceptual de operaciones que retiran o alteran asignaciones; OC-33 | §19.5, §19.6.2, §19.9.1 |
| C-06 | Media | DM | Paso (4) de la política de 2FA en UI-13, UI-09 y UI-07; IF-12 en §19.2 y en la trazabilidad | §19.2, §19.4, §19.11 |
| C-07 | Baja/Media | DAL | OC-31 (clasificación de equipos ajenos y RT-14); obsolescencia de la resolución manual condicionada a Q-22 en UI-05 y UI-10; DB2b-1 ampliada | §19.4.1, §19.4.2, §19.9 |
| C-08 | Media | DAL | OC-23 y DB2b-9 conservan las dos lecturas (unión de roles; criterio provisional de A-15), sin precedencia; A-4 no se extiende | §19.9 |
| C-09 | Media | DM | IF-31 (minimización desde el origen); UI-13, UI-05 y CAND-12 crean eventos solo con identificadores internos; RB2b-12 | §19.2, §19.4, §19.5 |
| C-10 | Baja | EDI | UI-13 distingue el rechazo técnico (fuera de la unidad) de la inscripción duplicada rechazada como resultado aplicado de la resolución (dentro de la unidad de CAND-12) | §19.4.3 |
| C-11 | Baja | EDI | ESC-30 [DER]; ES-19 con tipos separados; UI-10 campo 4 sin asignación para dueño u organizador | §19.2, §19.4.1, §19.7 |
| C-12 | Baja | EDI | Retención de respaldos asignada a ADR-12 y ADR-17 (R4), con asesoría jurídica | §19.3, §19.8, §19.4.5 |
| C-13 | Baja | EDI | UI-09 incluye los ajustes manuales perdidos (RD-CAL-07) y las asignaciones desactivadas con historial; el efecto sobre la marca de RD-CAL-06 queda en OC-25 | §19.4.4 |
| C-14 | Baja | DEP | OC-32, ES-24 y ESC-42 (estados de plantilla, dorsal único, baja y pendiente) | §19.2, §19.7, §19.8, §19.9 |
| C-15 | Baja | DAL | ESC-34 reformulado; la relación entre anonimización y detección posterior de duplicados queda abierta | §19.7 |
| C-16 | Editorial | EDI | Solo lo necesario para la coherencia: GT-12 lista UI-13; UI-10 cita ESC-29 y ESC-38 | §19.6.1, §19.4.1 |

### 19.15 Revisión focalizada de la v0.2 (D-01…D-03) y correcciones de la v0.3

**Origen.** Revisión focalizada de solo lectura de otro subagente de contexto nuevo (independencia **solo de contexto**), dictamen «APROBABLE COMO ENTREGABLE DOCUMENTAL CONDICIONADO», con tres residuos de severidad baja. C-01…C-15 quedaron resueltos, salvo C-03 y C-04 (parciales), que esta ronda completa.

| ID | Sev. | Corrección aplicada en la v0.3 | Dónde |
|---|---|---|---|
| D-01 | Baja | GT-12 limita el control de versión de RD-RES-04 (IF-30, ES-22) a UI-05 y UI-10; para UI-07, UI-09 y UI-13 la necesidad de concurrencia se conserva como garantía **derivada** de IF-05, GT-02 y RD-JUG-08, con sus escenarios, sin extender RD-RES-04 | §19.6.1 |
| D-02 | Baja | La actualización de la proyección ante la regeneración figura como **ABIERTA** (OC-24, DB2b-4) en el apartado de provisionalidad, la celda de §19.6.2, EV-10, ESC-41 y RB2b-03; RD-CAL-07 se conserva como [REC]; las alternativas y sus consecuencias se exponen sin elegir; K-2a, H2B1-10 y las matrices de R2-B1 no se modifican | §19.4, §19.6.2, §19.7, §19.9.3 |
| D-03 | Baja | §19.8, fila UI-09: ES-23 para asignaciones vigentes sobre partidos archivados y ES-04 solo cuando la operación crea o conserva una asignación sobre un partido del propio equipo del actor | §19.8 |

**Observaciones editoriales que permanecen** (no bloqueantes; no se abre una nueva ronda): ES-22 es [CONF] en bloque aunque "corrección encubierta" es una derivación; IF-24 [CONF] cita RD-CAL-07, que es [REC]; IF-24 remite a IF-17, que no lista A-1; CAND-24 cita RD-RES-10 en lugar de RD-RES-12 (Q-29); OC-33 solo trata asignaciones de editor y no la delegación del equipo retirado; el inventario no menciona eliminar un torneo en borrador; DB2b-4 dice "completar K-2c" aunque K-2a, H2B1-10 y las matrices no se modifican; RF-AUD-04 respalda IF-31 y no se cita.

**Nota de cierre (2026-11-19).** Esta matriz no se modifica. La v0.3 recibió una verificación final focalizada de solo lectura de otro subagente de contexto nuevo (mensaje «TORNEO DE CAMPEONES — VERIFICACIÓN FINAL DE R2-B2-b1 v0.3»): D-01, D-02 y D-03 RESUELTOS, sin regresiones materiales nuevas y con observaciones editoriales no bloqueantes (§19.0); dictamen «APROBABLE COMO ENTREGABLE DOCUMENTAL CONDICIONADO». Independencia **solo de contexto**. La aprobación condicional de R2-B2-b1 como entregable documental figura en §19.0.

---

# PARTE VI — BLOQUE R2-B2-b2 (v0.3-R2B2b2, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL, 2026-11-20)

## 20. R2-B2-b2 — ADR-02.C (parte 2b-2): sincronización, auditoría de rechazos y consolidación de las fronteras atómicas

### 20.0 Alcance, autorización y reglas de esta parte

**Estado: v0.3-R2B2b2 — APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-20), por acto expreso del usuario (ver «Aprobación condicional» más abajo).** La v0.3 corrigió la v0.2 tras la revisión focalizada de contexto nuevo (M-1 y M-2, §20.15.2) y la v0.2 corrigió la v0.1 (E-01…E-14, §20.15.1). Esta parte no altera el contenido técnico de R1 (§0–§15), R2-A (§16), R2-B1 (§17), R2-B2-a (§18) ni R2-B2-b1 (§19); se **añade** al final del archivo. **Única excepción, de estado y no técnica:** las frases de estado de la precedencia de §17.0 y §18.0 (registro de esta aprobación).

**Autorización recibida.** Mensaje «TORNEO DE CAMPEONES — INICIO CONTROLADO DE R2-B2-b2» (2026-11-19): «APRUEBO EL PLAN DE R2-B2-b2 COMO GUÍA DE TRABAJO, CON UNA MODIFICACIÓN:» «NO SUBDIVIDIR R2-B2-b2 EN DOS ENTREGABLES INDEPENDIENTES.» «AUTORIZO EXCLUSIVAMENTE EL INICIO Y LA REDACCIÓN DE UN ÚNICO BORRADOR R2-B2-b2 v0.1.» Archivo autorizado: solo `02-arquitectura-adr.md` (Parte VI, §20). La fecha es la del aviso de fecha de la sesión; no hay marca de tiempo individual del mensaje.

**Autorización de la corrección (v0.2).** Mensaje «TORNEO DE CAMPEONES — CORRECCIÓN INTEGRAL R2-B2-b2 v0.1» (2026-11-19): «AUTORIZO EXCLUSIVAMENTE UNA RONDA INTEGRAL DE CORRECCIONES DOCUMENTALES DE LOS HALLAZGOS E-01 A E-14.» Limitada a la Parte VI (§20); sin aprobación, registro ni versionado; R2-B2-b2 continúa como **borrador pendiente de aprobación**.

**Autorización de la corrección mínima (v0.3).** Mensaje «TORNEO DE CAMPEONES — CORRECCIÓN MÍNIMA M-1 Y M-2 DE R2-B2-b2» (2026-11-19): «AUTORIZO EXCLUSIVAMENTE UNA CORRECCIÓN DOCUMENTAL MÍNIMA DE LOS HALLAZGOS M-1 Y M-2.» Limitada a la Parte VI (§20); sin aprobación, registro ni versionado.

**Aprobación condicional de R2-B2-b2 como entregable documental (2026-11-20).** Por acto expreso del usuario (mensajes «TORNEO DE CAMPEONES — APROBACIÓN DOCUMENTAL CONDICIONADA DE R2-B2-b2» y «TORNEO DE CAMPEONES — REGISTRO FORMAL DE R2-B2-b2»), la v0.3-R2B2b2 quedó **APROBADA CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL**: «APRUEBO CONDICIONALMENTE R2-B2-b2 v0.3 COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL.»; registro autorizado con «AUTORIZO EXCLUSIVAMENTE REGISTRAR LA APROBACIÓN CONDICIONAL DE R2-B2-b2 COMO ENTREGABLE DOCUMENTAL.» La fecha 2026-11-20 es la del aviso de fecha de la sesión; no hay marca de tiempo individual del mensaje.

- **Comprende:** (1) la ficha UI-06 y sus diez campos; (2) la frontera lógica de autorización, aplicación y auditoría compartida con las operaciones en línea; (3) la composición de UI-10 dentro de UI-06, sin segunda unidad atómica; (4) los invariantes IF-32…41; (5) los estados prohibidos ES-25…32; (6) las garantías GT-19…24; (7) los escenarios de concurrencia ESC-43…56; (8) la clasificación de registros de rechazo R-1…R-7, respetando sus fuentes y su carácter confirmado, derivado, recomendado o abierto; (9) la síntesis documental de K-2a a K-2d y la matriz de las 13 unidades; (10) la evaluación comparativa de CAP-02-A y CAP-02-D; (11) las evidencias EV-13…17; (12) las amenazas y dependencias documentadas; (13) las condiciones OC-34…42, las decisiones DB2c-1…11 y los riesgos RB2c-01…11, abiertos o sin aceptar según corresponda; (14) la trazabilidad y los criterios de aceptación documental de §20.
- **Exclusiones (la aprobación NO implica):** (1) elegir CAP-02-A ni CAP-02-D; (2) elegir CAP-09-P-A, B ni C, ni las alternativas F-1…F-3 ni O-1…O-3; (3) resolver K-3, OC-04, OC-08, OC-13, OC-24 ni OC-35; (4) resolver DB1-1 a DB1-6, DB2a-1 a DB2a-10, DB2b-1 a DB2b-9 ni DB2c-1 a DB2c-11; (5) resolver OC-01 a OC-42 ni las doce decisiones abiertas de R2-A; (6) declarar demostrada la independencia de CAP-02-D; (7) dar por cumplido R1 §8.1; (8) declarar K-2d implementada ni probada; (9) fijar parámetros; (10) aceptar riesgos; (11) conceder excepciones, en particular a D-2; (12) aprobar definitivamente ADR-02.C; (13) aprobar globalmente R2-B2, R2 ni la Fase 2; (14) autorizar R2-C, R3 ni R4. La aprobación **no acredita implementación, ejecución ni pruebas empíricas** y **no constituye una auditoría externa**; las garantías documentadas son **exigidas, no implementadas ni probadas**.
- **Limitación de la revisión.** La auditoría de la v0.1, la revisión focalizada de la v0.2 y la verificación final de la v0.3 las hicieron subagentes del mismo tipo de modelo que redactó el borrador: su independencia fue **solo de contexto, no organizativa ni externa**. La verificación final de la v0.3 fue **favorable** («APROBABLE COMO ENTREGABLE DOCUMENTAL CONDICIONADO»).
- **Observaciones editoriales no bloqueantes** (identificadas para una conciliación documental posterior, sin modificar el contenido técnico aprobado): las de §20.15.2 y las de la verificación final: redacción genérica de «registro del rechazo» (§20.3.1 fila h, UI-06 campos 5 (f) y 6); precisiones pendientes de ES-30 y ESC-55; tensión entre la ficha UI-03 de R2-B2-a («se audita como rechazo») y OC-08, que sigue abierta; y las demás de trazabilidad, componentes y clasificación documental. El usuario **no autoriza modificar el contenido técnico aprobado de R2-B2-a ni de R2-B2-b2** para atenderlas.
- **Nota posterior sobre §20.13.** §20.13 se conserva **intacto** como descripción del estado al redactar el borrador. Su afirmación de que la v0.3 «no ha sido auditada ni revisada por nadie más» quedó superada: la v0.3 **sí recibió una verificación focalizada favorable** de un agente de contexto nuevo (independencia solo de contexto). Lo mismo vale para V-5, CR2B2b2-15 y §20.14 («Qué no se hizo»): describen el estado a la fecha del borrador, y prevalece este apartado. §19.0 se conserva intacto; el versionado de R2-B2-b1 se aclara en `aprobaciones.md`.
- **Integridad.** SHA-256 del **contenido técnico aprobado** (el archivo inmediatamente antes de añadir estos metadatos): `f1e1fb34ed1bdbd68628d0ab83ebec8a5ee912f0e49625fce5b124f57fddf30c`. El hash del **archivo con los metadatos de aprobación** se registra en `aprobaciones.md` y `bitacora.md`; **no se inserta en este archivo**. Los metadatos de aprobación son solo editoriales: el título y el estado de la Parte VI, este apartado, la fila de estado de §20.1, el estado y el párrafo «Git» de §20.14 y las frases de estado de la precedencia de §17.0 y §18.0.
- **Estado de versionado.** **Pendiente:** sin commit, push, PR ni merge.
- **Condición de cierre de la Fase 2.** Las decisiones que la Fase 1 asignó a la Fase 2 deberán resolverse antes de cerrar globalmente la Fase 2, salvo modificación o excepción formalmente aprobada. La consolidación de las 13 unidades no equivale a aprobación global de R2-B2. Las obligaciones de R2-C, R3 y la Fase 4 conservan su alcance.

**Un único entregable en dos etapas internas.** La **etapa 1** (§20.2 a §20.5) documenta UI-06, la frontera de sincronización y la auditoría de rechazos. La **etapa 2** (§20.6 a §20.11) consolida K-2, CAP-02-A frente a CAP-02-D, las amenazas, las condiciones y las operaciones candidatas sobre las **13 unidades UI-01…13**. Ambas forman **un solo borrador pendiente de revisión y aprobación**; no son dos entregables.

**Qué NO contiene ni hace.**

| # | Exclusión |
|---|---|
| X-1 | No redacta R2-C, R3 ni R4; las formulaciones de ADR-05.C, ADR-04.C y ADR-16.C son de R2-C y los mecanismos de R3 |
| X-2 | No aprueba R2-B2-b2, R2-B2 completo, R2 ni la Fase 2; no registra aprobación alguna |
| X-3 | No resuelve DB1-1…DB1-6, DB2a-1…DB2a-10, DB2b-1…DB2b-9, OC-01…OC-33 ni las 12 decisiones abiertas de R2-A (de las 13 originales, la 11 está atendida) |
| X-4 | **No elige** CAP-02-A ni CAP-02-D; **no declara demostrada la independencia de CAP-02-D**; **no declara cumplido R1 §8.1**; no concede excepción |
| X-5 | No fija PAR ni acepta RES o riesgos RB; **no concede excepción a D-2** ni resuelve OC-04 |
| X-6 | No elige tecnología, proveedor, producto ni mecanismo (de cola, secuencia, idempotencia, serialización, almacenamiento ni registro); no escribe SQL, políticas, rutas ni contratos de interfaz |
| X-7 | No modifica `00`, `01`, `aprobaciones.md`, `bitacora.md`, ni R1, R2-A, R2-B1, R2-B2-a ni R2-B2-b1; no crea ramas, commits, pushes, PR ni merges |
| X-8 | **No presenta ninguna garantía como implementada ni validada,** incluida K-2d: todas son **exigidas**, a nivel de capacidad |
| X-9 | **No decide** la partición ADR-05.C / ADR-09.C (decisión 3 de R2-A), K-3 ni la decisión 10 de R2-A: presenta alternativas y consecuencias |

**Lectura previa directa.** El autor leyó directamente en esta sesión: el skill del proyecto y sus reglas de dominio; en `01` OFF-01…OFF-25 (incluidos OFF-02, 10, 11, 12, 13, 17, 20, 22 y 25 completos), la política de conflictos §9.4 con sus precisiones, los estados de una operación §9.2, RT-13, RT-14, RD-RES-01, 04, 05, 07, 10 y 13, RD-EVT-12, RF-AUD-01…06 y la matriz §7.1; en R1 §5.3 (CMP-13), §5.5 (UI-01…13 y notas), §7.0, §7.3 (FL-3) y §8.1 (ADR-05); en R2-A §16.7 (IN-09-01…13, CAP-09-P-A/B/C, §16.7.9) y §16.8.4 (H2A-06) y las filas de amenazas y casos de abuso asignadas a R2-B y R2-C; en R2-B1 K-1…K-3, CAP-02-A/D y §17.3.3; en R2-B2-a y R2-B2-b1 las fichas, IF, ES, GT, EV, OC y CAND; y `aprobaciones.md` y `bitacora.md`.

**Convención.** [CONF]: texto confirmado de `01` o R1. [REC]: recomendación o propuesta de `01` (no confirmada). [DER]: derivada por el autor (se explica el paso). [ABIERTO]: sin resolver. Los identificadores continúan los de las partes previas: IF-32…, ES-25…, GT-19…, ESC-43…, OC-34…, EV-13….

### 20.1 ADR-02.C (parte 2b-2) — Identificación

| Campo | Valor |
|---|---|
| ADR | ADR-02.C (continuación de §17.1, §18.1 y §19.1): frontera atómica de la operación sincronizada y consolidación de R2-B2 |
| Entradas | R1 §5.3, §5.5, §7.0, §7.3, §8.1; R2-A (ADR-09.C, §16.7.9, §16.8.4); R2-B1 (K-1…K-3, CAP-02-A…D, DB1-1…6); R2-B2-a (IF-01…19, ES-01…13, GT-01…10); R2-B2-b1 (IF-20…31, ES-14…24, GT-11…18) |
| Salida de esta parte | Ficha UI-06; frontera de sincronización; requisitos de la auditoría de rechazos; síntesis de K-2 sobre las 13 unidades; evidencia consolidada sobre DB1-1; amenazas, condiciones y candidatas |
| Estado | v0.3-R2B2b2: APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-20); ADR-02.C **no** queda aprobada definitivamente |

### 20.2 Invariantes de frontera y estados prohibidos adicionales (etapa 1)

Se **suman** a IF-01…31 y ES-01…24 (IF-03, IF-05, IF-06, IF-09, IF-10, IF-11, IF-12, IF-13, IF-17, IF-20, IF-23, IF-24 e IF-30 se aplican también a UI-06; GT-02, GT-04, GT-07, GT-08, GT-09 y GT-14 se aplican también a UI-06, §20.6.2). No introducen permisos ni retiran garantías (IN-02-18).

**Invariantes (IF).**

| ID | Invariante de frontera | Tipo | Fuente | Paso derivado (solo [DER]) |
|---|---|---|---|---|
| IF-32 | **Misma frontera lógica en línea y sincronizada.** CMP-13: «No contiene reglas de negocio ni decide permisos»: entrega cada operación a CMP-08 y CMP-11 «por el mismo camino que las operaciones en línea». **No existe una segunda ruta de negocio con reglas distintas** | [CONF] en lo que dicen CMP-13 y FL-3; [DER] en la consecuencia de que no hay otra ruta de negocio | R1 §5.3 (CMP-13), §7.3 (FL-3) | CMP-13 entrega por el mismo camino y no contiene reglas de negocio; de ahí se deduce que no existe una segunda ruta con reglas distintas |
| IF-33 | **Autorización vigente al sincronizar:** «La autorización definitiva es siempre del servidor/BD y se verifica al sincronizar con el estado vigente»; evaluada de forma atómica con la aplicación (RT-13). «Se rechaza toda operación sin autorización vigente, aunque se capturara legítimamente antes»; una operación que sigue autorizada por otro rol vigente se evalúa por ese rol. «Ningún dato local concede permisos.» | [CONF] | OFF-22, OFF-11 (D-11), §9.4 filas 4 y 5 | — |
| IF-34 | **Idempotencia:** «cada operación se aplica como máximo una vez»; reenviar el mismo identificador, también de forma concurrente, «devuelve el resultado original sin repetir efectos ni registros de auditoría». **Para una operación rechazada, `01` no fija cómo se aplica (OC-35)** | [CONF] en lo aplicado; [ABIERTO] en lo rechazado | OFF-06, RD-RES-05, §9.4 fila 9 | — |
| IF-35 | **Orden y contigüidad por partido:** las operaciones se validan en orden de secuencia contra el estado resultante de las anteriores aceptadas, partiendo del estado oficial; la secuencia debe ser contigua; «Un hueco bloquea las operaciones posteriores de ese partido y se registra (OFF-20)». Que las siguientes queden «bloqueadas por operación anterior» y las de otros partidos continúen es **[REC]** | [CONF] en la validación en orden y en el hueco (OFF-25); [REC] en el bloqueo por operación anterior (OFF-07) | OFF-25, OFF-07, OFF-13 (el orden depende de la secuencia y no del reloj) | — |
| IF-36 | **Dependencia de la reanudación:** si la reanudación se rechaza o queda en conflicto, «ningún evento posterior de la secuencia se aplica ni se publica»; un evento con el estado oficial `suspendido` y sin reanudación aceptada previa en su secuencia se rechaza | [CONF] | OFF-25, RD-EVT-12, RD-RES-13, Q-46 | — |
| IF-37 | **Conflicto de versión sin sobrescritura:** una operación de marcador o estado con versión base desactualizada no se aplica y queda `en conflicto`; «lo confirmado por el servidor nunca se sobrescribe en silencio» y «ninguna captura local se pierde en silencio» | [CONF] | OFF-09, §9.4 filas 2, 3 y 10, RD-RES-04 | — |
| IF-38 | **Atomicidad por operación sincronizada:** «cada operación se aplica completa o no se aplica»; «una interrupción de la red durante el envío no deja estados parciales» | [REC] en OFF-08; [DER] en su aplicación a la unidad | OFF-08, IF-01, IF-29 | La unidad indivisible de R1 §7.0 ya exige todo o nada; OFF-08 lo recomienda para la ruta sincronizada |
| IF-39 | **Una sola operación prevalece ante carreras incompatibles:** ante una carrera entre una revocación y la sincronización, «una sola de las dos prevalece, sin estados intermedios» | [CONF] | OFF-22 (criterio de aceptación), RT-13, CL-45 | — |
| IF-40 | **Lo rechazado o en conflicto no modifica lo oficial ni lo público:** no se publica ni se sobrescribe información oficial; no hay recuperación ni publicación automática de operaciones rechazadas; solo la operación `confirmada` de un partido visible puede ser pública | [CONF] | OFF-22, OFF-15, OFF-21, §9.2, RD-RES-07 | — |
| IF-41 | **Las variantes de estado (iniciar, suspender, reanudar, finalizar) recorren la misma frontera que su equivalente en línea;** su enumeración definitiva como unidades corresponde a la Fase 4 | [DER] | R1 §5.5 (lista no exhaustiva), OFF-01, RD-RES-01, 10 y 13, OFF-21 | OFF-01 permite capturar esos cambios sin conexión y R1 §5.5 no los enumera como unidades; se tratan como variantes de UI-06 sin asumir unidad definitiva (OC-34) |

**Estados prohibidos (ES).** Ningún observador debe verlos y ninguna ordenación de operaciones concurrentes debe producirlos.

| ID | Estado prohibido | Tipo | Fuente |
|---|---|---|---|
| ES-25 | Una operación recibida **aplicada dos veces** (efectos o registros de auditoría duplicados) por reenvíos secuenciales o concurrentes | [CONF] | OFF-06, §9.4 fila 9 |
| ES-26 | Una operación sincronizada **aplicada con una autorización ya revocada y confirmada**, o aplicada por una ruta con reglas de negocio distintas de las de su equivalente en línea | [CONF] en la autorización; [DER] en las reglas distintas | OFF-22, RT-13, IF-32 |
| ES-27 | Un evento aplicado o publicado tras una reanudación rechazada o en conflicto, o una operación posterior aplicada saltando un hueco de la secuencia | [CONF] | OFF-25 |
| ES-28 | Marcador o estado oficial **sobrescrito** por una operación con versión base desactualizada | [CONF] | OFF-09, RD-RES-04, §9.4 filas 2 y 3 |
| ES-29 | Una operación rechazada o en conflicto **visible en lo público**, o aplicada a medias (resultado sin su recálculo, o recálculo sin resultado) | [CONF] | OFF-15, OFF-22, ES-14 |
| ES-30 | Un registro de rechazo o bloqueo **exigido por RF-AUD-06 (R-2)** que no persiste porque la unidad se abortó, o una pérdida de auditoría provocada de forma inducida. **No cubre** la persistencia independiente de R-4, que está [ABIERTA] (ADR-05.C, ADR-09.C, decisión 3 de R2-A), ni la de R-6 [REC], salvo decisión formal | [DER] | IN-09-05, RF-AUD-06, CAB-14; R2-A §16.7.2 |
| ES-31 | Un reenvío de una operación **ya aplicada** que devuelve un resultado distinto del original o genera un efecto o un registro nuevo. **Para una operación rechazada el alcance de OFF-06 está [ABIERTO] (OC-35) y este estado no lo cubre** | [CONF] en lo aplicado | OFF-06, §9.4 fila 9 |
| ES-32 | Un resultado recibido por sincronización cuyo recálculo se confirma en una unidad distinta de la que confirma el resultado (**dos unidades atómicas**) | [DER] | R1 §5.5 (UI-10) |

### 20.3 Frontera de sincronización (etapa 1)

#### 20.3.1 Camino lógico de la operación recibida

Es el camino de R1 §7.3 (FL-3) y de la columna de componentes de UI-06, **sin elegir mecanismo**:

| Paso | Qué ocurre | Componente | Dentro de la unidad |
|---|---|---|---|
| a | Recepción: identificador, secuencia, versión base y datos de la operación (OFF-03) | CMP-13 | Sí, en lo que decide (idempotencia, orden, versión) |
| b | Idempotencia: ¿ya se aplicó? Orden y contigüidad por partido; versión | CMP-13 | Sí |
| c | **Revalidación completa con el flujo §7.0** (pasos 2 a 5): roles, asignaciones, estado, historial de delegaciones y restricciones; política de segundo factor; RT-05 y RT-14 por efecto | CMP-08 | **Sí** (unidad indivisible: R1 §7.0) |
| d | Aplicación de la operación (la de UI-10 si es un resultado, con su recálculo; o la variante de estado de §20.3.5) | CMP-11 (CMP-12 y CMP-14 por la composición de UI-10) | **Sí** |
| e | Registro de la idempotencia de lo aplicado y evento de auditoría de lo aplicado, con **origen sincronizado**, identificador de la operación y hora del cliente | CMP-13, CMP-16 | **Sí** (R1 §5.5 para la idempotencia; IN-09-04 e IN-09-06 para la auditoría) |
| f | Señal de cambio, solo si ADR-06 la adopta | CMP-14 | Sí (IF-22) |
| g | Respuesta al cliente y estado local de la operación (`confirmada`, `en conflicto`, `rechazada`: §9.2) | CMP-13, CMP-05 | **No** |
| h | **Registro del rechazo** (si se rechazó) | CMP-16 | **No** (IF-09) |
| i | Conservación local de lo rechazado o en conflicto (≤ 30 días), exportación limitada y constancia de eliminación | CMP-04, CMP-05 | **No** (cliente; OFF-10, OFF-24) |
| j | Entrega pública y copias en Z0 | CMP-15 | **No** (§19.3) |

**Reparto de responsabilidades (R1 §5.3).** **CMP-08** decide **permisos y conflictos de interés** (RT-05, RT-14 con historial) con la definición única. **CMP-13** detecta **idempotencia, orden por secuencia contigua y conflictos de versión** y **no decide permisos**. **CMP-11** aplica y **CMP-16** audita. OFF-22 incluye «integridad y versión» entre lo que se verifica al sincronizar: **quién lo ejecuta** lo reparte R1 §5.3 (versión, idempotencia y secuencia: CMP-13; permisos: CMP-08). Los componentes de la fila de UI-06 en R1 §5.5 son CMP-13 + CMP-08 + CMP-11 + CMP-16; **CMP-12 y CMP-14** intervienen por la composición de UI-10; la cola y el sincronizador del cliente (CMP-04, CMP-05) y la entrega (CMP-15) son **externos a la unidad**. Las **decisiones de protocolo** (cómo se estructuran la cola, la secuencia, la idempotencia, la revalidación y los conflictos) son de **ADR-05.C (R2-C)** y no se toman aquí.

#### 20.3.2 Lo confirmado y lo pendiente sobre «antes» y «después»

| CONFIRMADO en `01` o R1 | PENDIENTE (no se resuelve aquí) |
|---|---|
| La autorización se evalúa con el **estado vigente al sincronizar** (OFF-22; D-11; §9.4 filas 4 y 5) | **El instante que determina el resultado de una operación en tránsito:** si cuenta el de la recepción, el de la aplicación o el de la confirmación de la revocación cuando la sincronización y la revocación coinciden (**OC-04**) |
| Se **rechaza** toda operación sin autorización vigente al sincronizar, **aunque se capturara legítimamente antes** (OFF-22, OFF-11) | Las **condiciones de D-2** y la **medición del objetivo de revocación ≤ 60 s** en sesiones conectadas (IN-03-07), no medible en la Fase 2: decisión 1 de R2-A (DB2c-10); **sin excepción concedida** |
| Ante una carrera incompatible **una sola operación prevalece**, sin estados intermedios (OFF-22; RT-13; CL-45) | El **nivel de autenticación exigido al sincronizar** (CAP-03-S-A o S-B de ADR-03.C; OFF-23): OC-39 |
| Con la sesión vencida, la captura continúa pero **sincronizar exige sesión válida y permisos vigentes** (OFF-23) | Las **decisiones de protocolo reservadas a R2-C:** cola, secuencia, idempotencia, revalidación y conflictos (ADR-05.C) y la partición ADR-05.C / ADR-09.C (decisión 3 de R2-A) |
| Una operación que sigue autorizada por otro rol vigente se evalúa por ese rol (OFF-22; RT-11) | Si el **organizador** captura sin conexión (Q-42 (a)) y si organizador y dueño pueden **reanudar** sin conexión (A-10): OC-38 |

**Lectura del autor [DER].** La partición entre CONFIRMADO y PENDIENTE es una **lectura del autor** de OFF-22, §9.4 e IN-03-07. Lo confirmado fija **cuándo** se evalúa la autorización (al sincronizar) y **qué** ocurre con lo no autorizado (se rechaza); lo pendiente fija **qué instante cuenta** para una operación en tránsito y **cómo se mide** el reflejo de una revocación en sesiones conectadas. **OC-04 permanece abierta y D-2 no recibe excepción.**

#### 20.3.3 Mapa de los requisitos OFF a capacidades

| Requisito | Capacidad que exige | Dónde se documenta | Estado en `01` | Decide |
|---|---|---|---|---|
| OFF-01 | Capturar sin conexión marcador, eventos, iniciar, suspender, reanudar y finalizar | Variantes de §20.3.5; IF-41 | CONF | Fase 4 (OC-34) |
| OFF-03 | Identificador único, secuencia por partido, versión base, dispositivo, usuario y hora local en cada operación | Cliente (CMP-04); IF-34, IF-35 | CONF | ADR-05.C (R2-C) |
| OFF-05 | Disparo de la sincronización con la app abierta; reintento con espera creciente | Cliente; reintentos y rechazos (§20.4) | DER | ADR-05.C |
| OFF-06 | Idempotencia en el servidor | IF-34, ES-25, ES-31 | CONF | ADR-05.C |
| OFF-07 | Orden y bloqueo por operación anterior | IF-35 | REC | ADR-05.C; OC-37 |
| OFF-08 | Atomicidad por operación | IF-38 | REC | R3 |
| OFF-09 | Política de conflictos | IF-37 | CONF + REC | ADR-05.C |
| OFF-10 | No descartar en silencio; retención ≤ 30 días | Cliente (fuera de la unidad) | CONF | ADR-05.C, ADR-16.C |
| OFF-11 | Revocación y desasignación al sincronizar | IF-33 | CONF | — |
| OFF-13 | El orden depende de la secuencia y no del reloj | IF-35 | REC | — |
| OFF-15 | Lo local jamás es público: solo la operación `confirmada` de un partido visible | IF-40; ES-29 | CONF | — |
| OFF-20 | Metadatos de operaciones aplicadas, en conflicto y rechazadas | §20.5 (R-4) | CONF (D-9) | ADR-05.C (R2-C); ADR-09.C (R2-A); partición: decisión 3 de R2-A |
| OFF-24 | Exportación limitada de lo rechazado o en conflicto | Cliente (fuera de la unidad; R-7) | CONF | ADR-05.C, ADR-16.C |
| OFF-21 | Finalización pendiente hasta que el servidor la confirme | IF-40; variantes de estado (§20.3.5) | CONF | — |
| OFF-22 | Autorización definitiva al sincronizar, atómica con la aplicación | IF-33, IF-39 | CONF | — |
| OFF-23 | Sesión vencida: captura sí, sincronización exige sesión válida | §20.3.2; OC-39 | CONF | ADR-03.C (R2-A) |
| OFF-25 | Secuencias con reanudación | IF-35, IF-36, ES-27 | CONF | ADR-05.C |

#### 20.3.4 Composición de UI-10 dentro de UI-06 (OC-30)

R1 §5.5 dice que, cuando el resultado llega por sincronización, UI-10 se ejecuta **dentro de** UI-06: «la aplicación de la operación recibida y su recálculo son una sola transacción de datos». **Regla fundamental: la operación recibida recorre la misma frontera lógica de autorización, aplicación y auditoría que su equivalente en línea; no se crea una segunda ruta ni una segunda unidad atómica de UI-10.**

| Elemento | Pertenece a UI-06 (envoltura) | Pertenece a UI-10 (contenido) | Se evalúa una sola vez |
|---|---|---|---|
| Idempotencia, orden, contigüidad y versión | Sí | — | — |
| Autorización vigente (§7.0, pasos 2 a 5) | Sí: **una sola evaluación** de CMP-08 | Las reglas que aplica son las de UI-10 (asignación vigente, RT-05, RT-14, estado del partido) | **Sí** |
| Marcador, eventos, estado, clasificación derivada, señal de cambio | — | Sí (IF-20, IF-21, IF-22, GT-11) | — |
| Registro de la idempotencia de lo aplicado (R1 §5.5) | Sí | — | — |
| Evento de auditoría de lo aplicado (IN-09-04, IN-09-06) | **Uno solo**, con origen sincronizado | Su contenido es el de UI-10 | **Sí** |
| Conflicto de versión (RD-RES-04) | Se detecta al recibir | Es el mismo que el de la ruta en línea (IF-30, IF-37) | **Sí** |

**Componentes incorporados por composición.** CMP-12 y CMP-14 no figuran en la fila de UI-06 de R1 §5.5: intervienen porque UI-10 se ejecuta dentro de UI-06. La frontera lógica es **una sola** para UI-06 y UI-10.

**Consecuencia documental.** Las garantías de UI-10 (GT-11, GT-12, GT-14) siguen siendo las de la ruta sincronizada; UI-06 **añade** las garantías de idempotencia, orden y persistencia del rechazo (GT-19…24) y **no sustituye ni duplica** las de UI-10. La **coherencia pública** sigue siendo garantía exigida a ADR-04.C (GT-13).

#### 20.3.5 Iniciar, suspender, reanudar y finalizar como variantes de operaciones recibidas

OFF-01 permite capturar sin conexión marcador, eventos, iniciar, suspender, reanudar y finalizar. R1 §5.5 enumera UI-10 para confirmar un resultado y **no enumera** unidades específicas para iniciar, suspender ni reanudar (CAND-17). Se tratan aquí **como variantes de UI-06**, sin asumir una unidad definitiva:

| Variante | Requisitos | Facultad y restricciones | Frontera que necesita | Particularidad de la ruta sincronizada |
|---|---|---|---|---|
| **Iniciar** (`programado` → `en_juego`) | RD-RES-01 (transiciones válidas) [REC (H-08)] | Las de captura (matriz §7.1 filas 24 y 25) | Cambio de estado + auditoría, sin recálculo | Se rechaza si el estado oficial ya cambió (versión) |
| **Suspender** (`en_juego` → `suspendido`) | RD-RES-10 [REC en la transición; controles CONF]; RD-EVT-12 | Editor asignado y dueño; organizador **COND Q-42 (a)**; RT-14, RT-05 | Cambio **solo de estado** (no modifica marcador ni eventos) + auditoría | Se sincroniza con OFF-22; sin asignación vigente → rechazada (CL-47) |
| **Reanudar** (`suspendido` → `en_juego`) | RD-RES-13 [CONF, Q-46 B]; OFF-25 | Editor asignado (también sin conexión); organizador y dueño solo en línea hasta decidir (A-10) | Cambio de estado + auditoría; **condiciona los eventos posteriores de su secuencia** (IF-36) | Rechazada o en conflicto → ningún evento posterior se aplica ni se publica |
| **Finalizar** (→ `finalizado`) | OFF-21 [CONF]; RD-RES-07 | Las de captura; **finalización pendiente** hasta confirmación del servidor | Se evalúa **como UI-10** (confirmar el resultado con recálculo), sin dar por definitiva su enumeración (OC-34) | El partido solo se considera finalizado y público cuando el servidor la confirma |

**Condición abierta (OC-34):** su enumeración definitiva como unidades (una por transición, una única, o variantes de UI-10) corresponde a la **Fase 4**. Mientras tanto, cada variante cumple IF-32 a IF-41 y se evalúa como **parte de UI-06**.

### 20.4 Ficha UI-06 — Aplicar una operación recibida por sincronización (etapa 1)

| Campo | Contenido |
|---|---|
| 1. Disparador | Una operación capturada sin conexión llega al servidor al sincronizar (con la app abierta, al recuperar la conexión, por «Sincronizar ahora» o por reintento: OFF-05). El actor es el editor asignado, el dueño (matriz §7.1, fila 24) o el organizador si Q-42 (a) lo habilita (la reanudación sin conexión de organizador y dueño depende de A-10); **ninguno sobre los partidos de su propio equipo** si es delegado (RT-14). Requiere **sesión válida** (OFF-23) |
| 2. Componentes (R1 §5.5) | CMP-13 + CMP-08 + CMP-11 + CMP-16 (CMP-12 y CMP-14 intervienen por la composición de UI-10; la cola y el sincronizador del cliente y la entrega son externos: §20.3.1) |
| 3. Requisitos | «Aplicar una operación recibida: autorizar, registrar idempotencia, aplicar y auditar» (R1 §5.5); OFF-06, OFF-22, RT-13; OFF-03, 07, 08, 09, 10, 11, 13, 20, 21, 23, 25; §9.2 y §9.4; RD-RES-04, 05, 07, 10 y 13; RD-EVT-12; RT-05, RT-14; IF-32…IF-41 y los de §19 que correspondan |
| 4. Autorización | **Servidor:** **CMP-08** decide **permisos y conflictos de interés** con la definición única, con el estado **vigente al sincronizar**: sesión válida, **autorización vigente por alguno de los roles de la cuenta** (rol vigente y asignación vigente al partido) y ausencia de conflicto de interés (RT-05 y RT-14, incluida la restricción histórica no levantada, Q-45). **CMP-13 no decide permisos:** detecta idempotencia, orden y contigüidad por partido y conflictos de versión (R1 §5.3; OFF-22 incluye «integridad y versión» en lo que se verifica al sincronizar). **Capa de datos:** verifica la misma decisión de permisos y el **estado resultante** (un único resultado oficial por partido, asignación vigente al partido). **Que la capa de datos impusiera también la secuencia contigua no tiene fuente en `01` ni en R1:** si se requiriera, sería una derivación [DER] a justificar en ADR-05.C (R2-C; OC-41). Ningún dato local concede permisos |
| 5. Secuencia lógica | (a) recepción; (b) idempotencia, orden, contigüidad y versión; (c) **flujo §7.0:** (1) identificar al actor y la sesión, (2) reunir roles, asignaciones, estado y restricciones, (3) calcular el efecto, (4) política efectiva de segundo factor con todos los roles vigentes (IF-12), (5) decidir; (d) aplicar la operación (UI-10 si es un resultado; la variante de estado que corresponda); (e) registrar la idempotencia de lo aplicado y auditar; (f) señal de cambio si ADR-06 la adopta; o **rechazar** (el registro del rechazo queda fuera: IF-09) |
| 6. Cambios indivisibles | Registro de la idempotencia de lo aplicado + **la aplicación de la operación recibida y, si es un resultado, su recálculo (una sola transacción de datos: UI-10 dentro de UI-06)** + señal de cambio (si ADR-06) + **un solo** evento de auditoría de lo aplicado con origen sincronizado, identificador de la operación y hora del cliente. **Todo o nada** (IF-38). **Fuera de la unidad:** respuesta y estado local de la operación, registro del rechazo, conservación local, exportación limitada, entrega pública y copias (§20.3.1) |
| 7. Estados prohibidos | ES-25, ES-26, ES-27, ES-28, ES-29, ES-30, ES-31, ES-32; ES-10, ES-21, ES-22 y ES-24 (evento para un jugador en baja o pendiente); **bajo D:** ES-13 (decidir con una proyección que no refleja una revocación ya confirmada) |
| 8. Concurrencia | **ESC-43** a **ESC-56** (§20.8): reenvíos, dos dispositivos, revocación o desasignación concurrentes, hueco o reanudación rechazada, corrección o finalización por otro actor, jugador dado de baja o anonimizado, W.O. o cancelación, regeneración del calendario, sesión vencida o cuenta desactivada, concesión de delegado o levantamiento, y fallo del registro del rechazo |
| 9. Auditoría | **Dentro:** lo aplicado, con origen sincronizado, identificador de la operación, hora del cliente y hora del servidor (IN-09-06; el servidor registra su propia hora: OFF-13). **Fuera:** los registros de rechazo que una fuente exige (RF-AUD-06 para RT-14; OFF-20 para lo rechazado o en conflicto; R-3 en su persistencia, IN-09-05): tipos y alternativas en §20.5 |
| 10. Abiertas | **OC-04** (instante que cuenta para una operación en tránsito); **D-2** (decisión 1 de R2-A; DB2c-10; sin excepción); **OC-30** (composición con UI-10: documentada en §20.3.4, sin resolver las dependencias de R2-C); **OC-13**, **OC-35** y **OC-36** (K-3, registro de idempotencia de rechazadas, fallo del registro); **OC-34** (variantes de estado); **OC-37** (OFF-07 [REC]); **OC-38** (Q-42 (a) y A-10); **OC-39** (nivel de autenticación al sincronizar); **OC-40**, **OC-41** y **OC-42** (reasignación de AM-19 y AM-34; secuencia entre dos dispositivos; estado local tras un aborto técnico); decisiones 1, 3 y 10 de R2-A; Q-29, Q-30, Q-42, Q-43; A-9…A-11, A-13…A-15. DB1-1 intacta |

### 20.5 Auditoría de rechazos como capacidad (etapa 1)

**Reserva.** Esta sección documenta **requisitos de capacidad** de la persistencia de los rechazos. **No elige** entre CAP-09-P-A, B ni C, ni entre las alternativas F-1…F-3 y O-1…O-3 de R2-A (§16.7.9); **no decide** la partición ADR-05.C / ADR-09.C (decisión 3 de R2-A); **no resuelve K-3 ni la decisión 10 de R2-A.**

#### 20.5.1 Tipos de registro y su fuente

| # | Registro | Qué es | Fuente | Tipo de la exigencia | Dentro de la unidad |
|---|---|---|---|---|---|
| R-1 | **Lo aplicado** | Evento de auditoría de lo que la unidad aplicó (actor, rol, hora del servidor, torneo, entidad, acción, valores antes y después minimizados, motivo y origen) | RF-AUD-01, RF-AUD-02, RT-10; IN-09-04, IN-09-06 | [CONF] el contenido; [DER] que forme parte de la misma unidad (IN-09-04) | **Sí** |
| R-2 | **Rechazos y bloqueos por conflicto de interés (RT-14)** | Registro «con actor, acción, equipo, torneo, motivo y hora, sin datos personales de menores»; también el bloqueo por falta de autoridad habilitada y «los intentos de levantar la restricción sin autoridad» | RF-AUD-06 (obligatorio, Q-44, Q-45), CL-51 | [CONF] | **No** |
| R-3 | **Persistencia independiente de un registro de rechazo cuando existe un registro exigido** | Que, **si** una fuente exige registrar un rechazo o bloqueo, ese registro persista aunque la operación no se aplique o se revierta. **R-3 no crea por sí misma una obligación de registrar.** Su alcance por registro: **R-2** (RF-AUD-06): persistencia independiente conforme a su fuente (R2-A §16.7.2: «Sí [DER]»); **R-4** (OFF-20): persistencia independiente **[ABIERTO]**, de ADR-05.C y ADR-09.C y de la decisión 3 de R2-A; **R-6** [REC]: solo generaría esa obligación si se adopta formalmente | IN-09-05; R1 §7.0 paso 6 («DER de RF-AUD-06»); R2-A §16.7.2 | [DER] como propiedad de persistencia para R-2; [ABIERTO] para R-4; [REC] para R-6; IN-09-05 no establece por sí solo qué rechazos se registran | **No** (IF-09) |
| R-4 | **Metadatos de operaciones rechazadas o en conflicto** | «Metadatos mínimos de las rechazadas o en conflicto que efectivamente recibe»; solo del torneo del organizador; no autoriza publicar | OFF-20 (D-9), §9.4 precisión 2 | [CONF] | **No**; su persistencia independiente es **[ABIERTO]** (R2-A §16.7.2; ADR-05.C, ADR-09.C y decisión 3 de R2-A) |
| R-5 | **Registro de idempotencia de una operación rechazada** | Conservar el resultado de una operación rechazada para devolver «el resultado original» ante un reenvío | OFF-06; R1 §5.5; §16.7.9 (O-1…O-3) | [ABIERTO]: `01` no fija cómo se aplica a lo rechazado | **No** (abierto) |
| R-6 | **Bitácora de seguridad** | Intentos denegados de escalada o acceso cruzado; **distinta** de R-2 | RF-AUD-05; IN-02-28 | **[REC]** (prioridad S) | **No**; obligaciones solo condicionales si se adopta formalmente |
| R-7 | **Estado local y conservación en el dispositivo** | Estado `en conflicto` o `rechazada` de la operación, conservación ≤ 30 días, exportación limitada y constancia de eliminación | §9.2, OFF-10, OFF-11, OFF-24 | [CONF] | **No** (cliente; §20.3.1 paso i) |

**Obligación de registrar frente a obligación de persistir.** Son dos cosas distintas. (1) **Qué rechazos hay que registrar** lo fijan las fuentes: RF-AUD-06 (solo los rechazos y bloqueos de RT-14, incluido el bloqueo por falta de autoridad habilitada y los intentos de levantar la restricción sin autoridad), OFF-20 (metadatos de operaciones **sincronizadas** rechazadas o en conflicto) y, como [REC], RF-AUD-05. **Ninguna fuente exige registrar todo rechazo técnico** (por ejemplo, un rechazo en línea por falta de facultad que no sea de RT-14); IN-09-05 no es una obligación universal de registrar todos los rechazos. (2) **R-3** es una propiedad de persistencia: **cuando** existe un registro exigido, este debe persistir aunque la operación se aborte; **no crea por sí misma una obligación de registrar.** La **persistencia independiente** se conserva para **R-2** conforme a su fuente (R2-A §16.7.2); para **R-4** permanece **[ABIERTA]** (ADR-05.C, ADR-09.C y decisión 3 de R2-A); **R-6** continúa [REC] y solo generaría obligaciones si se adopta formalmente. **K-3**, CAP-09-P-A/B/C, F-1…F-3 y O-1…O-3 permanecen abiertas. Qué rechazos adicionales se registrarían, si alguno, **no está fijado** y se deja a ADR-09.C y a la decisión 10 de R2-A.

**Cuatro situaciones que no deben confundirse.** (1) **Resultado aplicado:** cambio que la unidad confirma; su registro es R-1, dentro de la unidad. (2) **Operación que no prevalece:** una operación concurrente que no llega a ser la vigente (por ejemplo, la segunda reasignación de CL-41); `01` exige que su intención quede auditada, pero **no fija** si se trata como rechazo o como resultado aplicado en orden secuencial. (3) **Rechazo técnico formalmente definido:** la decisión de no aplicar por autorización, integridad, versión o secuencia, con los registros de R-2 y R-4 según su fuente. (4) **Decisión todavía abierta:** la clasificación de (2) es OC-08 (DB2a-4); **no se resuelve aquí.**

**Distinción entre rechazo técnico y resultado aplicado.** Un **rechazo técnico** es la decisión de no aplicar una operación (autorización, integridad, versión, secuencia): su registro, **cuando una fuente lo exige**, se hace **fuera** de la unidad (R-2, R-4; R-3 en su persistencia). Un **resultado aplicado** es un cambio que la unidad sí confirma aunque "rechace" algo en el dominio (por ejemplo, la inscripción duplicada rechazada tras la resolución del organizador, CAND-12; la retirada de asignaciones de UI-02): su registro es R-1 y va **dentro** de la unidad.

#### 20.5.2 Qué registro de rechazo corresponde a cada unidad UI-01…13

La tabla indica **qué fuente exige cada registro**. **No elige mecanismo ni formulación.** **R-3 no obliga a registrar:** es la propiedad de persistencia de un registro exigido; se conserva para R-2, está [ABIERTA] para R-4 y solo sería condicional para R-6 [REC] (§20.5.1). Donde "ninguna fuente exige registrar el rechazo", la tabla lo dice y no lo convierte en obligación.

| Unidad | Rechazos o bloqueos posibles | Registro que corresponde | Resultado aplicado (R-1, dentro) | Particularidad |
|---|---|---|---|---|
| UI-01 Crear torneo | Sin facultad; integridad de la asignación | Ninguna fuente exige registrar el rechazo; R-6 [REC]; R-3 si se registra | Torneo y asignación del creador organizador | "RT-14 no aplica a la creación" es lectura del autor [DER] |
| UI-02 Delegado y retiradas | Autoasignación (RT-04); segundo equipo del mismo torneo; RT-05 | **R-2** si el efecto alcanza RT-14; R-6 [REC]; R-3 sobre lo que se registre | Concesión y **cada** retirada | CL-35 |
| UI-03 Reasignar editor | RT-14 («asignaciones que generen conflicto», A-2); RT-05; versión; **la reasignación concurrente que no prevalece (CL-41; OC-08)** | **R-2** (RT-14); para la intención que no prevalece, el **tipo de registro está [ABIERTO]** (OC-08) | Reasignación con valores antes y después (R-1), la que se aplica | CL-41 dice «ambas intenciones quedan auditadas». **OC-08 abierta:** no se decide si la que no prevalece es un rechazo técnico (por leer una asignación ya cambiada) o un resultado aplicado en orden secuencial; este documento **no la clasifica** como rechazo. Ver la distinción de §20.5.1 |
| UI-04 Dueños | Organizador intenta conceder o revocar dueño; sin segundo factor reciente; sin motivo; último dueño | R-6 [REC]; ninguna otra fuente exige registrar el rechazo; R-3 si se registra | Designación o revocación con motivo | RF-AUD-05 [REC] |
| UI-05 Corregir | **RT-14**, bloqueo sin otro actor (CL-51); versión obsoleta (RD-RES-04); torneo `archivado` | **R-2** (RT-14 y bloqueo); R-3 sobre R-2 | Corrección con valores antes y después, **minimizados** (IF-31) | R-4 no aplica a la ruta en línea |
| **UI-06 Operación recibida** | Sin asignación, rol o cuenta revocados (§9.4 filas 4 y 5); **RT-14**; versión (filas 2 y 3); hueco o reanudación rechazada (OFF-25); jugador no disponible (fila 6) | **R-2** (RT-14); **R-4** (OFF-20); R-5 (abierto); R-6 [REC] (acceso cruzado); R-7 (cliente); persistencia de R-2 conforme a su fuente (R-3); la de R-4 está [ABIERTA] | Lo aplicado, con origen sincronizado | Estado local `en conflicto` o `rechazada` (§9.4) |
| UI-07 Anonimizar | Sin facultad (Q-42 (b)) | Ninguna fuente exige registrar el rechazo; R-3 si se registra | Anonimización sin datos personales (IF-27, IF-31) | R-1 solo con identificadores internos |
| UI-08 Levantar restricción | **Autoaprobación**; sin dueño alterno (intentos sin autoridad); «sin motivo» | **R-2** solo para los intentos sin autoridad («los intentos de levantar la restricción sin autoridad»); R-3 sobre R-2 | Levantamiento con aprobador y motivo (RF-AUD-02) | RF-AUD-06 nombra los intentos sin autoridad; **«sin motivo» no es un caso nombrado en RF-AUD-06** (ninguna fuente exige registrarlo) |
| UI-09 Calendario | **RT-14** sobre cualquier partido afectado; bloqueo sin actor alterno | **R-2**; R-3 sobre R-2 | Generación o regeneración y sus consecuencias | RF-AUD-06 incluye el bloqueo |
| UI-10 Confirmar | **RT-14**; RT-05; sin asignación; versión | **R-2** (RT-14); R-4 si llegó por sincronización; persistencia de R-2 (R-3) conforme a su fuente y la de R-4 [ABIERTA] | Resultado confirmado | Dentro de UI-06 cuando llega por sincronización |
| UI-11 Revocar | Organizador revoca dueño u otro organizador; efecto indirecto sobre partidos propios (A-15) | **R-2** si hay RT-14; R-6 [REC]; R-3 si se registra | Revocación y desactivaciones | A-15 sin resolver; IN-09-13 [REC] |
| UI-12 Invitación | Invitación vencida, usada o revocada; segundo equipo | Ninguna fuente exige registrar el rechazo de la invitación; **R-2** solo si UI-12 compone UI-02 y el efecto alcanza RT-14 (composición provisional, OC-01); R-3 si se registra | Consumo de la invitación y concesión | CL-54 |
| UI-13 Alta | Sin facultad; integridad; **la segunda inscripción de la misma persona tras la resolución es un resultado aplicado (CAND-12)** | Ninguna fuente exige registrar el rechazo técnico; R-1 para el resultado de la resolución | Alta y estado resultante | IF-31: solo identificadores internos |

#### 20.5.3 Los rechazos de UI-06 y la política de conflictos (§9.4)

| Escenario de §9.4 | Resultado | Estado local | Registro del servidor |
|---|---|---|---|
| Fila 2: marcador o estado con versión desactualizada | No se aplica | `en conflicto` | R-4 (OFF-20; persistencia independiente [ABIERTA]) |
| Fila 3: el partido fue finalizado o corregido por otro actor | No se aplica nada | `en conflicto` | R-4 (OFF-20; persistencia independiente [ABIERTA]) |
| Filas 4 y 5: editor desasignado; rol o cuenta revocados | Rechazada | `rechazada` | R-4 (OFF-20; persistencia independiente [ABIERTA]); R-2 si interviene RT-14 (R-3 en la persistencia de R-2) |
| Fila 6: jugador dado de baja o anonimizado | No se aplica | `en conflicto` | R-4 (OFF-20; persistencia independiente [ABIERTA]) |
| Fila 8: partido pasa a W.O. o cancelado (si se aprueba) | No se aplica | `en conflicto` o `rechazada` | R-4 (OFF-20; persistencia independiente [ABIERTA]) |
| Fila 9: mismo identificador recibido otra vez | Idempotente: devuelve el primer resultado | — | **Sin registro nuevo de lo aplicado** (OFF-06); para lo rechazado: R-5 abierto |
| Fila 11 (sustituta): reasignación; el editor anterior sincroniza | Rechazada; no se publica | `rechazada` | R-4 (OFF-20; persistencia independiente [ABIERTA]) |
| OFF-25: hueco o reanudación rechazada | Bloqueo o no se aplican los eventos posteriores | `pendiente` (bloqueada) o `en conflicto` | R-4 («se registra», OFF-25) |
| RT-14 sobre el propio equipo (vigente o histórico) | Rechazada al sincronizar | `rechazada` | **R-2** (R-3 en su persistencia); **R-4** relacionado (OFF-20: entre los motivos se registra RT-14), con persistencia independiente **no decidida** [ABIERTA] |

#### 20.5.4 Alternativas y consecuencias, sin elegir

**A. Persistencia independiente (D9-1; K-3).** CAP-09-P-A: unidad independiente posterior; CAP-09-P-B: el rechazo como resultado confirmado; CAP-09-P-C: canal separado con entrega al menos una vez. **K-3** de R2-B1 condiciona la persistencia del rechazo impuesto por la capa de datos a CAP-09-P-A o P-C; P-B solo cubre los que decide el evaluador.

| Alternativa | Conformidad con IN-09-05 | Conformidad con la redacción de R1 («de forma independiente de la unidad») | Rechazos impuestos por la capa de datos (RC-04) | Conclusión de R2-A (§16.7.4) | Consecuencia sobre UI-06 [DER] |
|---|---|---|---|---|---|
| CAP-09-P-A | Conforme | Conforme | Conforme: cubre también la transacción abortada, porque el registro va en otra unidad | **Plenamente conforme** | El registro se confirma **antes** de devolver el rechazo; el estado local `rechazada` solo se devolvería después |
| CAP-09-P-B | Conforme **solo** para los rechazos que decide el evaluador (CMP-08) | **No conforme con el texto literal**: adoptarla exigiría ajustar esa redacción (edición separada) | **No conforme**: un rechazo que aborta la transacción no puede registrarse en esa misma unidad | **No es una alternativa independiente**: variante parcial que no cubre los rechazos de la capa de datos y diverge de R1 | No cubre los rechazos de la capa de datos sincronizados |
| CAP-09-P-C | **Condicionada**: conforme solo si la intención de registro es **durable antes de responder**; una ventana de pérdida no cumple IN-09-05 | Conforme | Conforme si la intención es durable antes de responder | **Viable con condición** (sin ventana de pérdida) | Con la condición de durabilidad, el estado local `rechazada` no depende de una ventana de pérdida |

Se conservan **todas las condiciones de R2-A** (la condición de durabilidad de P-C, el ajuste editorial de R1 si se adoptara P-B y K-3). **Este documento no elige** entre P-A, P-B y P-C.

**B. Fallo del registro del rechazo (§16.7.9).**

| ID | Alternativa | Consecuencia sobre la ruta sincronizada | Conformidad con IN-09-05 |
|---|---|---|---|
| F-1 | Devolver el rechazo y continuar sin registro | **Pérdida de auditoría** de un rechazo (RT-14, intentos de escalada) | **No conforme** |
| F-2 | Devolver un error recuperable y dejar la operación sin resolver hasta poder registrar | La operación sigue `pendiente` y se reintenta (OFF-05); **riesgo de denegación inducida** | Conforme |
| F-3 | Persistir primero la intención en un medio durable distinto y conciliar después; si no está disponible, aplicar F-2 | Reduce la ventana de pérdida; añade complejidad y una ventana hasta la conciliación | Condicionada a que la intención sea durable antes de responder |

**C. Idempotencia de una operación rechazada (OFF-06; R-5).**

| ID | Alternativa | Consecuencia | Fuente |
|---|---|---|---|
| O-1 | Un único registro por identificador: el reenvío devuelve el rechazo original | Cumple OFF-06 al pie de la letra y **limita la inundación (CAB-14)**; **exige conservar el resultado de las rechazadas** (punto abierto de ADR-05.C); se pierde la visibilidad de reintentos abusivos | R2-A §16.7.9 |
| O-2 | Un registro por cada intento | Conserva la visibilidad; repite registros (OFF-06 lo prohíbe para lo aplicado) y agrava la inundación (CAB-14) | R2-A §16.7.9 |
| O-3 | Un registro por identificador más un contador de reenvíos | Conserva la visibilidad sin repetir registros; añade un campo a decidir | R2-A §16.7.9 |

**Alcance de OFF-06 sobre lo rechazado: [ABIERTO] (OC-35).** `01` no fija cómo se aplica la idempotencia a una operación rechazada; **no se elige** entre O-1, O-2 y O-3.

**Quién decide.** ADR-05.C (R2-C) y el usuario (decisión 10 de R2-A; DB2c-2); la partición ADR-05.C / ADR-09.C es la decisión 3. **K-3 queda abierta**: este documento no la cierra.

### 20.6 Síntesis de K-2d sobre las 13 operaciones (etapa 2)

**Reserva.** K-2d se describe como **garantía arquitectónica exigida**: **no se implementa, no se aprueba, no se valida y no se declara cumplida.** Esta síntesis solo reúne lo que las tres partes documentaron y señala lo que **no** está demostrado.

#### 20.6.1 Matriz de las 13 unidades

"Consume hechos derivados" se refiere a CAP-02-D: ES-13 aplica **bajo D** a toda operación que consume hechos derivados (§18.8.1.1); «Actualiza la proyección» se refiere a K-2c.

| Unidad | Tipo | Cambio indivisible (resumen) | GT principales | Consume hechos derivados bajo D | Actualiza la proyección bajo D (K-2c) | Registro de rechazo (§20.5.2; R-3 es una propiedad de persistencia, no una obligación de registrar; la persistencia de R-4 está [ABIERTA] y R-6 es [REC]) | Ficha |
|---|---|---|---|---|---|---|---|
| UI-01 Crear torneo | Permisos | Torneo + asignación del creador organizador + auditoría | GT-01, 02, 09 | Sí (facultad del actor; asignación vigente) | **Sí** (H2B1-10) | Ninguna fuente exige registrar el rechazo; R-6 [REC] | §18.4.1 |
| UI-02 Delegado | Permisos | Concesión + retiradas de editor + anulación de levantamiento + auditoría | GT-01, 03, 06 | Sí | **Sí** (H2B1-10) | R-2 si el efecto alcanza RT-14; R-6 [REC] | §18.4.2 |
| UI-03 Reasignar | Permisos | Desactivar + activar la asignación | GT-02, 03 | Sí | **Sí** (H2B1-10) | R-2 (RT-14); intención que no prevalece: [ABIERTO] (OC-08) | §18.4.3 |
| UI-04 Dueños | Permisos | Cambio del rol de dueño + historial + motivo, sobre el conjunto de dueños | GT-05, 02 | Sí (facultad del actor) | **Sí** (H2B1-10) | R-6 [REC]; ninguna otra fuente exige registrar el rechazo | §18.4.4 |
| UI-05 Corregir | Datos | Marcador, eventos, historial, motivo y recálculo | GT-11, 12, 13, 14 | Sí | No | **R-2** (RT-14, bloqueo) | §19.4.2 |
| **UI-06 Operación recibida** | Sincronización | Idempotencia + autorización + aplicación (UI-10) + auditoría de lo aplicado | **GT-19…24**, GT-11, 12 | Sí (facultad y asignación al partido) | No por sí misma (aplica resultados y estados) | **R-2**, **R-4**, R-5 abierto, R-6 [REC], R-7 | §20.4 |
| UI-07 Anonimizar | Datos | Anonimización en la fuente y derivadas persistentes | GT-12, 17 | Sí | No | Ninguna fuente exige registrar el rechazo | §19.4.5 |
| UI-08 Levantar | Permisos | Levantamiento con aprobador y motivo | GT-02, 04 | Sí | **Sí** (H2B1-10) | **R-2** (solo intentos sin autoridad) | §18.4.5 |
| UI-09 Calendario | Datos | Partidos nuevos + archivo de los anteriores + asignaciones perdidas + auditoría | GT-12, 14, 15 | Sí | **ABIERTO (OC-24, DB2b-4)** | **R-2** | §19.4.4 |
| UI-10 Confirmar | Datos | Resultado + recálculo + señal de cambio (si ADR-06) | GT-11, 12, 13, 14 | Sí | No | **R-2**; R-4 si llega por sincronización | §19.4.1 |
| UI-11 Revocar | Permisos | Revocación + desactivación de las asignaciones dependientes | GT-04, 06 | Sí | **Sí** (H2B1-10) | R-2 si hay RT-14; R-6 [REC] | §18.4.6 |
| UI-12 Invitación | Permisos | Consumo de la invitación + concesión + retiradas | GT-03, 06 | Sí | **Sí** (H2B1-10) | Ninguna fuente exige registrar el rechazo; R-2 solo si compone UI-02 y alcanza RT-14 (OC-01) | §18.4.7 |
| UI-13 Alta | Datos | Alta + estado resultante (activa o pendiente) | GT-12, 16 | Sí | No | Ninguna fuente exige registrar el rechazo técnico | §19.4.3 |

**Qué muestra la matriz.** Bajo D **las 13 unidades consumen hechos derivados** (ES-13 aplica a todas); **siete** las actualizan según H2B1-10 (UI-01, 02, 03, 04, 08, 11 y 12); **una** (UI-09) lo tiene **abierto**; **cinco** no (UI-05, 06, 07, 10 y 13). Bajo A no hay proyección. **No se declara que la lista de H2B1-10 esté completa:** el inventario conceptual de §19.6.2 y las candidatas de §20.11 señalan otras operaciones que retiran o alteran asignaciones o delegaciones.

#### 20.6.2 Garantías GT-01…24 sobre las 13 unidades

Todas son **exigidas, no implementadas**. GT-19…24 son nuevas y corresponden a UI-06.

| ID | Garantía exigida (resumen) | Unidades | Aplica a |
|---|---|---|---|
| GT-01 | Misma unidad para la fuente, los efectos compuestos, la auditoría y, bajo D, la proyección | UI-01, 02, 03, 04, 08, 11, 12 | A y D |
| GT-02 | Lo decidido es lo aplicado (bajo D, incluida la lectura de la proyección) | Las 13 | A y D |
| GT-03 | Serialización de las unidades que compiten por un mismo hecho o conjunto | UI-02, 03, 04, 08, 12 | A y D |
| GT-04 | Revocación efectiva frente a operaciones en línea y sincronizadas | UI-11 y toda unidad cuyo actor puede perder su facultad; **UI-06** | A y D |
| GT-05 | Protección global del último dueño | UI-04 y toda operación que altere el conjunto de dueños | A y D |
| GT-06 | Efectos compuestos completos | UI-02, 11, 12 (y UI-08) | A y D |
| GT-07 | Todo o nada con la auditoría de lo aplicado | Las 13 (IF-10) | A y D |
| GT-08 | Las dos capas deciden sobre el mismo estado confirmado | Las 13 | A y D |
| GT-09 | Sin ventana con proyección obsoleta | Las siete de §18.7; por ES-13, toda operación que consume hechos derivados bajo D (§18.8.1.1) | **Solo D** |
| GT-10 | La proyección se reconstruye desde las fuentes; prevalecen las fuentes (g4 [REC]) | Las siete de §18.7; por ES-13, toda operación que consume hechos derivados bajo D (§18.8.1.1) | **Solo D** |
| GT-11 | Una sola unidad para el resultado | UI-05, 10 (y UI-06 por composición) | A y D |
| GT-12 | Serialización por partido, clasificación o registro. El control de versión de RD-RES-04 está confirmado **solo en UI-05 y UI-10**; en la ruta sincronizada se aplica por GT-24, dentro de UI-06 y solo a marcador y estado, **sin extender RD-RES-04** a otras operaciones | UI-05, 07, 09, 10, 13 (y UI-06 por composición de UI-10) | A y D |
| GT-13 | Coherencia pública (exigida a ADR-04.C; no demostrada) | UI-05, 07, 10 | A y D |
| GT-14 | RT-14 sobre el conjunto completo antes de aplicar | UI-05, 09, 10 (y UI-06 por composición de UI-10) | A y D |
| GT-15 | Todo o nada del calendario | UI-09 | A y D |
| GT-16 | Unicidad de la alta activa bajo concurrencia, en la capa de datos | UI-13 | A y D |
| GT-17 | Completitud de la anonimización en la fuente | UI-07 | A y D |
| GT-18 | Efectos externos fuera de la unidad | Las cinco de §19 | A y D |
| **GT-19** | **Misma unidad para la envoltura y el contenido:** la aplicación de la operación recibida, su recálculo, el registro de la idempotencia de lo aplicado y la auditoría se confirman juntos o ninguno; **sin segunda unidad de UI-10** | UI-06 | A y D |
| **GT-20** | **Idempotencia bajo reenvío concurrente:** una sola aplicación y un solo registro de lo aplicado; para una operación **ya aplicada**, la misma respuesta (el alcance sobre lo rechazado es [ABIERTO]: OC-35) | UI-06 | A y D |
| **GT-21** | **Orden y contigüidad por partido,** con la dependencia de la reanudación respetada y los huecos bloqueados | UI-06 | A y D |
| **GT-22** | **Autorización vigente al aplicar frente a carreras con revocación o reasignación:** una sola operación prevalece, sin estados intermedios | UI-06 (con UI-11 y UI-03) | A y D |
| **GT-23** | **El registro de rechazo o bloqueo exigido por RF-AUD-06 (R-2) persiste fuera de la unidad y no se pierde en silencio** (K-3 abierta; alternativas sin elegir). No obliga a registrar todo rechazo. **La persistencia de R-4 [ABIERTA] y la de R-6 [REC] no son garantía exigida** hasta decisión formal | UI-06 y las unidades con R-2 (§20.5.2) | A y D |
| **GT-24** | **Conflicto de versión sin sobrescritura en la ruta sincronizada** (la misma garantía de RD-RES-04, sin extenderla a otras operaciones) | UI-06 (marcador y estado) | A y D |

#### 20.6.3 Estado de K-2a, K-2b, K-2c y K-2d

| Componente | Qué exige (R2-B1) | Qué documentan las tres partes | **Qué NO está demostrado** |
|---|---|---|---|
| **K-2a** Completitud | La proyección contiene los hechos derivados que necesita la decisión; no contiene el estado, las reglas de integridad ni la invitación vigente | Hechos por unidad (§18.8.1; las cinco de §19.6.2; UI-06 consume la facultad del actor y la asignación al partido) | La **completitud** sobre las operaciones candidatas y las no enumeradas |
| **K-2b** Derivación | Deriva de la fuente única de CMP-07 con la definición única de CMP-08; se reconstruye desde las fuentes; no es un segundo decisor | GT-10, GT-08, ESC-18, ESC-20; responsabilidades P-1 y P-2 (§18.8.2) | La **reconstruibilidad**, la adopción de g4 ([REC]) y la lectura de R1 §5.3 (DB1-6) |
| **K-2c** Actualización coherente | Se actualiza con toda operación que modifica permisos | Siete unidades de H2B1-10; **hallazgo condicional EV-10** (UI-09 y candidatas: OC-24) | La **enumeración definitiva** (Fase 4) y la decisión de DB2b-4 |
| **K-2d** Garantías transaccionales | Lectura y aplicación en la misma unidad; actualización con cada cambio de permisos; sin ventana con proyección obsoleta | GT-01…24; ESC-01…56 | Que exista una forma de lograrlas; su **mecanismo (R3, condicionado a ADR-01)**; su **verificación (Fase 4)** y, para las revocaciones en sesiones conectadas, la **medición del ≤ 60 s** (D-2) |

#### 20.6.4 Protección global frente a permisos obsoletos

**Propiedad exigida [DER]** (derivada de IF-06, IF-33 e IF-39, que citan RF-ACC-07, OFF-22 y RT-13). Desde que una revocación se confirma, **ninguna operación nueva, por ninguna vía de entrada, se aplica con ella;** una operación en curso se resuelve como aplicada antes (válida y auditada) o rechazada, nunca a medias.

| Vía de entrada | Garantía y estado prohibido que la protegen | Qué queda pendiente |
|---|---|---|
| En línea | IF-06, IF-33; GT-02, GT-04; ES-10, ES-21 | — |
| **Sincronizada** | IF-32, IF-33, IF-39; **GT-22**; **ES-26** | **OC-04** (instante que cuenta para una operación en tránsito); **D-2** (DB2c-10) |
| Automática | Hereda la situación del actor que la dispara (IF-24) | — |
| Sesión conectada | IN-03-06 (rechazo inmediato en el servidor); el ≤ 60 s de IN-03-07 es un **objetivo sin garantía fuera de línea** | La **medición** (decisión 1 de R2-A) |
| Bajo D, cualquier vía | GT-09, GT-10; **ES-13** | Elección de CAP-02-A o D; K-2c completa |

### 20.7 Evidencia consolidada de CAP-02-A frente a CAP-02-D (etapa 2)

**Reserva.** Esta sección **consolida la evidencia** de §18.8 y §19.6.2 para DB1-1. **No elige** CAP-02-A ni CAP-02-D, **no declara demostrada la independencia de D**, **no declara cumplido R1 §8.1** (el déficit frente a «≥ 2 formas de reparto con ambas capas siempre presentes» sigue abierto, sin excepción) y **no modifica K-2a, H2B1-10 ni las matrices de R2-B1.** DB1-1 y DB1-6 siguen abiertas.

| ID | Observación | Alcance | Naturaleza |
|---|---|---|---|
| EV-13 | En las 13 unidades **ambas capas verifican la misma decisión completa** bajo A y bajo D (IF-03, IF-16): lo que cambia es la **representación de las entradas**, no **qué verifica cada capa**. Primer elemento del criterio de §17.3.3 | Las 13 | Contraste |
| EV-14 | Bajo D, **las 13 unidades consumen hechos derivados** y quedan expuestas a ES-13; **siete** añaden un paso de mantenimiento de la proyección (K-2c); **una** (UI-09) lo tiene abierto; las cinco restantes no lo añaden. Es el **segundo elemento** del criterio de §17.3.3 (composición de las unidades): D la modifica solo en esas siete u ocho unidades | Las 13 | Contraste; sin conclusión |
| EV-15 | **La ruta sincronizada recorre la misma frontera bajo A y bajo D** (IF-32). Bajo D, rechazar una operación capturada antes de una revocación **depende de que la revocación (UI-11) haya actualizado la proyección en la misma unidad** (GT-09; ES-13): es **un caso concreto** de un hecho derivado obsoleto que produciría una aplicación indebida; ESC-54 lo extiende a UI-03 y UI-09 (reasignación y regeneración), y el mismo caso, bajo A, se resuelve sobre las fuentes | UI-06 con UI-11 (caso concreto); UI-03 y UI-09 (ESC-54) | Aplicación de K-2d a la sincronización |
| EV-16 | Las reglas de integridad **de las unidades que las tienen** se verifican sobre las fuentes **igual bajo A y bajo D** (K-2a); D **no sustituye** la lectura de las fuentes para el estado, la invitación ni esas reglas. UI-08 y UI-11 no tienen reglas de integridad propias en esta consolidación | Las unidades con reglas de integridad propias | Aplicación de K-2a |
| EV-17 | **Estado de la evidencia [SUP: síntesis del autor].** Clasificación de EV-01…EV-16 según la interpretación de §17.3.3 con la que son coherentes: **II** («D es una variante de A»): EV-01, EV-04, EV-06, EV-08 (a), EV-11, EV-13 y EV-16; **I** (obligaciones y modos de fallo propios de D): EV-02, EV-03, EV-08 (b), EV-09, EV-10, EV-14 y EV-15; **mixta**: EV-05 ((i) apoya la II; (ii) apoya la I); **sin conclusión o de límite**: EV-07 (límite) y EV-12 (contraste). **La evidencia no zanja la cuestión.** Se mantienen **las dos interpretaciones** | EV-01…EV-16 | Síntesis; sin elegir |

**Lo que resolvería DB1-1.** Según §17.3.3, el criterio de independencia («que D cambie qué verifica cada capa o la composición de las unidades atómicas», de modo que no se reduzca a "A con una entrada derivada") **depende de la decisión del usuario sobre qué cuenta como «forma de reparto»** (DB1-1) y, si se elige D, de la lectura de R1 §5.3 (DB1-6). La evidencia consolidada **no sustituye esa decisión.**

### 20.8 Matriz de concurrencia de la ruta sincronizada (etapas 1 y 2)

«Admisible» significa que **equivale a alguna ordenación secuencial** (IF-05). **Origen:** son **literales** en `01` ESC-43, 44, 46, 47, 49 y 52 (§9.4 filas 9, 10, 3 y 6 y OFF-25, OFF-06, OFF-11, OFF-23 respectivamente) y, en lo que dice OFF-22, ESC-45; son **[REC] o [DER]**: ESC-48 (OFF-07 es [REC]; OFF-25 [CONF] solo para el partido con hueco) y ESC-50, 51, 53 y 55 [DER] (el texto de `01` no menciona esa simultaneidad); ESC-56 es **[CONF] en lo aplicado y [ABIERTO] en el alcance sobre lo rechazado**; ESC-54 es **propio de CAP-02-D**. **Ningún escenario se resuelve aquí por un mecanismo.**

| ID | Escenario | Unidades | Origen | Resultados admisibles | Resultados prohibidos | Garantías |
|---|---|---|---|---|---|---|
| ESC-43 | Reenvío del mismo identificador, secuencial y simultáneo | UI-06 × UI-06 | OFF-06, §9.4 fila 9 [CONF] | Un efecto, un registro de lo aplicado y la misma respuesta | Doble aplicación; registro duplicado; respuesta distinta (ES-25, ES-31) | GT-20 |
| ESC-44 | Mismo editor en dos dispositivos | UI-06 × UI-06 | §9.4 fila 10 [CONF] | Se fusionan por identificador; marcador o estado con versión desactualizada: conflicto. **La secuencia contigua entre los dos dispositivos del mismo editor es [ABIERTO] (OC-41)** | Sobrescritura (ES-28) | GT-20, GT-24 |
| ESC-45 | Revocación, desasignación o reasignación concurrente con la sincronización | UI-06 × UI-11 / UI-03 / UI-04 | OFF-22, §9.4 filas 4, 5 y 11 [CONF] | Aplicada antes y válida, o rechazada; **una sola prevalece** | Aplicada con la autorización ya revocada y confirmada (ES-26) | GT-22, GT-04 |
| ESC-46 | Hueco en la secuencia o reanudación rechazada con eventos posteriores | UI-06 | OFF-25 [CONF] | Las operaciones posteriores de ese partido quedan bloqueadas o no se aplican ni se publican; las de otros partidos continúan [REC] | Un evento aplicado o publicado tras ellas (ES-27) | GT-21 |
| ESC-47 | El partido fue finalizado o corregido por otro actor durante la sincronización | UI-06 × UI-05 / UI-10 | §9.4 fila 3 [CONF] | `en conflicto`; no se aplica nada | Sobrescritura (ES-28); dos resultados oficiales (ES-16) | GT-24, GT-12 |
| ESC-48 | Operaciones de partidos distintos del mismo lote | UI-06 | OFF-07 [REC]; OFF-25 [CONF] (partido con hueco) | Las de otros partidos continúan | Bloquear todo el lote por un partido | GT-21 |
| ESC-49 | Jugador del evento dado de baja o anonimizado mientras tanto | UI-06 × UI-07 / CAND-13 | §9.4 fila 6 [CONF] | `en conflicto`; «Jugador no disponible» | Evento aplicado a un jugador en baja o pendiente (ES-24); estadísticas alteradas (ES-20) | GT-12, GT-17 |
| ESC-50 | El partido pasa a W.O. o cancelado (si se aprueba) durante la sincronización | UI-06 × CAND-15 / CAND-24 | §9.4 fila 8; Q-21 y Q-29 (COND) [DER] | `en conflicto` o `rechazada` | Resultado aplicado sobre un partido ya resuelto (ES-16) | GT-12 |
| ESC-51 | Regeneración del calendario durante la sincronización | UI-06 × UI-09 | RD-CAL-07 (REC) [DER] | Una de las dos se rechaza; el partido archivado no recibe operaciones | Un resultado oficial sobre un partido archivado (ES-16); un editor vigente sobre un partido archivado (ES-23) | GT-12, GT-15 |
| ESC-52 | Sesión vencida o cuenta desactivada durante la sincronización | UI-06 | OFF-23, OFF-11 [CONF] | La captura local continúa; no se sincroniza sin sesión válida; con cuenta desactivada nada se aplica ni se publica | Aplicación con sesión inválida o cuenta desactivada (ES-26) | GT-22 |
| ESC-53 | Concesión del rol de delegado o levantamiento concurrentes con la sincronización de un dueño u organizador | UI-06 × UI-02 / UI-12 / UI-08 | RT-14 [DER] | Aplicada antes y válida, o rechazada por completo | Operación aplicada con la restricción vigente y confirmada (ES-10) | GT-22, GT-14 |
| ESC-54 | (**solo CAP-02-D**) Una sincronización se decide con una proyección que aún no refleja una revocación o una retirada de asignación ya confirmadas | UI-06 × UI-11 / UI-09 / UI-03 | K-2d, EV-15 | La decisión ve el estado anterior completo o el posterior completo | Decidir sobre una proyección obsoleta (ES-13) | GT-09, GT-10, GT-22 |
| ESC-55 | Un rechazo no puede registrarse (fallo del registro) mientras la operación se reenvía | UI-06 | IN-09-05, CAB-14 [DER] | **Sin elegir:** F-2 y F-3 conservan la auditoría; F-1 no es conforme con IN-09-05 (R2-A) | Pérdida de auditoría (ES-30); denegación inducida sin control (RB2c-06) | GT-23 |
| ESC-56 | Reintento de una operación ya rechazada | UI-06 | OFF-06 [CONF en lo aplicado]; [ABIERTO] sobre lo rechazado | **Sin elegir:** O-1, O-2 u O-3 (alcance de OFF-06 sobre lo rechazado [ABIERTO]: OC-35) | **[ABIERTO: OC-35]**: lo prohibido sobre lo rechazado no está fijado por `01`; lo aplicado se rige por ES-31 | GT-20, GT-23 |

### 20.9 Amenazas y casos de abuso de R2-B (etapa 2)

**No se amplía el modelo de amenazas:** esta sección registra cómo las capacidades de R2-B2 tratan las amenazas asignadas a R2-B y su relación con las de R2-C. El modelo consolidado es de R2-C.

| Amenaza o caso | Qué es | Cómo la tratan las fronteras documentadas | Queda para |
|---|---|---|---|
| **AM-18 / CAB-05** | Carrera entre una revocación o reasignación y una operación en curso; sincronizar tras ser revocado, desasignado o reasignado | IF-33, IF-39, GT-22, ES-26; ESC-45, 52 y 53; el resultado es "rechazada y conservada según OFF-10; nunca a medias" | **OC-04**, D-2 (medición) y la protección bajo D (ES-13) |
| AM-04 / CAB-04 | Repetir o reordenar operaciones enviadas | IF-34, IF-35, ES-25, ES-27; GT-20, GT-21 | **R2-C** (ADR-05.C) |
| AM-10 | Editar la cola local o el reloj del dispositivo para alterar el orden o la hora | IF-35 (el orden depende de la secuencia y no del reloj, OFF-13); IF-33 (nada local concede permisos) | **R2-C** (ADR-05.C) |
| AM-33 | Sincronizar la cola de otra cuenta o atribuir las operaciones a la sesión de otra | IF-33 (autorización con el estado vigente; «Alterar la cola local para apuntar a otro partido/torneo → rechazado») | ADR-03.C (R2-A); **R2-C** |
| CAB-06 | Capturar un partido del propio equipo (editor con rol de delegado) mediante la cola local | Rechazada al sincronizar (RT-05, OFF-22); ES-04 y ES-26 | — |
| CAB-07 | Operar sobre su propio equipo por efecto indirecto o en cascada | IF-24, GT-14: RT-14 sobre el efecto completo; R-2 | A-15 (Fase 4); ADR-09.C (R2-A) |
| CAB-14 | Provocar un rechazo que no deje rastro o inundar el registro | IF-09, GT-23, ES-30; alternativas F y O (§20.5.4) | Decisión 10 de R2-A; ADR-09.C (R2-A) |
| AM-19 | Inyección en la capa de datos (R2-B; Fase 4) | **Contexto:** su control (validación de entradas) no es un permiso; no se trata aquí | **R2-C** y **Fase 4**; R2-A la asignó a R2-B: **reasignación propuesta, no aplicada** (OC-40, DB2c-11) |
| AM-34 | Inyección de contenido en vistas públicas, panel, correos y proyecciones | **Contexto:** su control es la codificación de salidas; no se trata aquí | **R2-C**, **R3** (cabeceras) y **Fase 4**; R2-A la asignó a R2-B: **reasignación propuesta, no aplicada** (OC-40, DB2c-11) |

### 20.10 Dependencias, condiciones, decisiones y riesgos

#### 20.10.1 Dependencias

| Elemento | Depende de lo aprobado | Queda para R2-C | R3 | Fase 4 | Decisiones del responsable |
|---|---|---|---|---|---|
| UI-06 y la frontera de sincronización | IF-05, 06, 09, 10, 11, 12, 20, 24, 30; GT-02, 04, 09; ES-13; UI-10 y UI-05 (§19) | **ADR-05.C** (cola, secuencia, protocolo, conflictos), que **recibe** la frontera de ADR-02.C (R1 §8.2) | ADR-05.I | Pruebas de §9.4 y de carrera | Decisión 1 (D-2: DB2c-10), decisión 3 |
| Idempotencia y orden | RD-RES-04 y 05, OFF-06, OFF-25 | ADR-05.C | ADR-05.I | Pruebas con 1, 2 y 10 reenvíos | OC-35 |
| Auditoría de rechazos | IN-09-04, IN-09-05, K-3; **ADR-09.C es de R2-A (§16.7)** | La partición con ADR-05.C (decisión 3 de R2-A) | ADR-09.I | Pruebas de cobertura | **Decisión 10**, decisión 3 |
| Síntesis de K-2d | GT-01…24; K-2a a K-2d | — | Mecanismo | Verificación | — |
| Evidencia sobre DB1-1 | EV-01…17, §17.3.3 | — | — | — | **DB1-1**, DB1-6 |
| Amenazas | AM-18, CAB-05, CAB-07, CAB-14; ADR-03 (R2-A) para AM-33 | AM-04, AM-10, AM-33 | Cabeceras | Pruebas | DB2c-11 (reasignación propuesta de AM-19 y AM-34) |

#### 20.10.2 Condiciones abiertas nuevas (OC-)

| ID | Condición abierta | Unidades | Relación |
|---|---|---|---|
| OC-34 | Enumeración definitiva de iniciar, suspender y reanudar (y finalizar) como unidades: una por transición, una única o variantes de UI-10 | UI-06 y CAND-17 | DB2c-4; Fase 4 |
| OC-35 | Registro de idempotencia de una operación **rechazada** (O-1…O-3) y conservación de su resultado: `01` no fija cómo se aplica OFF-06 a lo rechazado | UI-06 | DB2c-2; ADR-05.C |
| OC-36 | Qué ocurre si el registro del rechazo no puede persistirse (F-1…F-3) | UI-06 y las 13 | DB2c-2; decisión 10 |
| OC-37 | OFF-07 es [REC]: el bloqueo de las siguientes operaciones «por operación anterior» no está confirmado; OFF-25 confirma el bloqueo por hueco | UI-06 | DB2c-9 |
| OC-38 | Pendiente solo de **Q-42 (a)** (si el organizador captura sin conexión) y de **A-10** (reanudación sin conexión de organizador y dueño, no confirmada); el dueño captura sin conexión según la matriz §7.1, fila 24 | UI-06 | DB2c-5 |
| OC-39 | Nivel de autenticación exigido al sincronizar (CAP-03-S-A o S-B de ADR-03.C, R2-A; OFF-23) | UI-06 | DB2c-8; decisión 6 de R2-A |
| OC-40 | Reasignación de AM-19 y AM-34: R2-A las asignó a R2-B, pero sus controles (validación de entradas, codificación de salidas) no son permisos; se **propone** dejarlas en R2-C y la Fase 4; **no se aplica** | UI-06 (contexto) | DB2c-11 |
| OC-41 | Secuencia contigua **entre dos dispositivos del mismo editor** (ESC-44; OFF-25): quién la garantiza y cómo se reconcilia **no está fijado** | UI-06 | Dependencia abierta de ADR-05.C (R2-C) |
| OC-42 | Estado local de una operación tras un **aborto técnico** distinto de un rechazo (ES-30; R-7): `01` solo define `en conflicto` y `rechazada` | UI-06 | Dependencia abierta de ADR-05.C (R2-C) |

**Heredadas que esta parte documenta sin resolver:** OC-04 (§20.3.2), OC-12 (duplicación de una solicitud de creación de torneo reintentada: es de UI-01, en línea), OC-13 y K-3 (§20.5), OC-24 (§20.6) y OC-30 (§20.3.4).

#### 20.10.3 Decisiones propuestas al usuario (DB2c-)

Ninguna se resuelve aquí. DB1-1…DB1-6, DB2a-1…DB2a-10, DB2b-1…DB2b-9 y las decisiones de R2-A (13 originales, la 11 atendida y 12 abiertas) permanecen **como estaban**.

| ID | Decisión | Opciones | Efecto |
|---|---|---|---|
| DB2c-1 | **Borde temporal de «antes y válida»** para operaciones en tránsito (OC-04): qué instante cuenta | Decidirlo en la Fase 4 / fijarlo antes | Fija el borde para operaciones en tránsito; **no trata D-2** (DB2c-10) |
| DB2c-2 | Persistencia del rechazo (K-3, OC-13, OC-35, OC-36) | CAP-09-P-A, B o C; F-1…F-3; O-1…O-3, con las consecuencias de §20.5.4 | Decisión 10 de R2-A |
| DB2c-3 | Partición ADR-05.C / ADR-09.C | Las de la decisión 3 de R2-A | Quién decide la persistencia de OFF-06 y OFF-20 |
| DB2c-4 | Enumeración de iniciar, suspender y reanudar (OC-34) | Decidirla en la Fase 4 / fijarla antes | Delimita UI-06 y CAND-17 |
| DB2c-5 | Captura sin conexión del organizador (Q-42 (a)) y reanudación sin conexión de organizador y dueño (A-10) (OC-38) | Las opciones de `01 §13` | Quién sincroniza |
| DB2c-6 | Recibir §20.7 como **evidencia** para DB1-1 sin resolverla | Recibir / pedir más análisis | No modifica DB1-1 |
| DB2c-7 | Inventario consolidado de candidatas (§20.11) para la enumeración de la Fase 4 | Aceptar / ampliar / reducir | Alcance de la Fase 4 |
| DB2c-8 | Nivel de autenticación al sincronizar (OC-39) | CAP-03-S-A o S-B de ADR-03.C (R2-A; decisión 6) / decidirlo en R2-C | Condición de OFF-23 |
| DB2c-10 | **Tratamiento formal de D-2** (decisión 1 de R2-A, asunto 1a; distinto del alcance de «sesión conectada», 1b) | Las de R2-A: (a) excepción formal para trasladar la medición a 5C o a la Fase 7; (b) validación documental reforzada y medición posterior (también requiere excepción formal); (c) reformular D-2 o RF-ACC-07 con modificación formal de `01`; (d) mantener D-2 abierto (bloquea el cierre de la Fase 2). **Ninguna cumple D-2 literalmente dentro de la Fase 2.** Toda excepción exige aprobación formal: **no se concede ninguna** | Fija cómo se trata el ≤ 60 s |
| DB2c-11 | Reasignación propuesta de AM-19 y AM-34 (OC-40) | Aceptar la reasignación a R2-C y la Fase 4 / mantener la asignación de R2-A | Alcance de R2-B2 frente a R2-C; **no aplicada** |
| DB2c-9 | Estado de OFF-07 [REC] (OC-37) | Adoptarlo / descartarlo / decidirlo en R2-C | Bloqueo por operación anterior |

#### 20.10.4 Riesgos documentales (sin aceptar)

Los riesgos se **registran sin valorar ni aceptar**. Ninguno sustituye a RB1, RB2a ni RB2b.

| ID | Riesgo | Relación |
|---|---|---|
| RB2c-01 | Describir UI-10 dentro de UI-06 como una segunda unidad atómica, contra R1 §5.5 | ES-32, OC-30 |
| RB2c-02 | Invadir ADR-05.C (R2-C) al detallar la cola, la secuencia o el protocolo | §20.3.3 |
| RB2c-03 | Elegir de hecho entre CAP-09-P, F u O al describir la auditoría de rechazos | §20.5.4; decisión 10 |
| RB2c-04 | Confundir lo confirmado de «vigente al sincronizar» con la medición de ≤ 60 s de D-2 | §20.3.2; OC-04 |
| RB2c-05 | Iniciar, suspender y reanudar quedan sin frontera enumerada | IF-41, OC-34 |
| RB2c-06 | Denegación inducida si el rechazo no puede registrarse y la operación queda sin resolver (F-2) | ESC-55 |
| RB2c-07 | Inundación del registro por reintentos abusivos (CAB-14) | O-1…O-3 |
| RB2c-08 | Bajo D, una proyección obsoleta aplica o rechaza mal una sincronización tras una revocación | ES-13, ESC-54 |
| RB2c-09 | Presentar la consolidación como si cerrara DB1-1 o R1 §8.1 | §20.7 |
| RB2c-10 | Resultado devuelto en un reenvío de una operación rechazada no definido | OC-35, ES-31 |
| RB2c-11 | Incoherencia entre este borrador y las partes ya aprobadas (§17 a §19) | §20.12 |

### 20.11 Inventario consolidado de candidatas y de condiciones (etapa 2)

#### 20.11.1 Operaciones candidatas

R1 §5.5 declara la lista **no exhaustiva** y deja la enumeración definitiva a la Fase 4. **Esta parte no añade candidatas operativas**: las variantes de estado (iniciar, suspender, reanudar y finalizar) se tratan como variantes de UI-06 (§20.3.5; CAND-17, OC-34). La inclusión de las candidatas es una decisión del usuario (DB2a-7, DB2b-5, DB2c-7).

| Origen | Candidatas | Tema |
|---|---|---|
| R2-B2-a (§18.5) | CAND-01…11 | Invitaciones, designación y revocación de organizadores, primera asignación de editor, degradación y autorrevocación, enrolamiento y recuperación, desactivar cuentas, asignar torneos existentes |
| R2-B2-b1 (§19.5) | CAND-12…24 | Resolver duplicados, baja de jugadores, anular eventos, W.O., resolución manual de desempates, estados de partido, ajustes de calendario, cambio de reglamento, ocultar nombres, estados del torneo, publicar jornadas, inscribir o retirar equipos, anular o aplazar partidos |
| R2-B2-b2 | Ninguna nueva; **CAND-17** cubre iniciar, suspender y reanudar | §20.3.5 |

**Operaciones que retiran o alteran asignaciones o delegaciones** (relevantes solo si se elige D; §19.6.2): UI-09 (regeneración), CAND-18, CAND-21, CAND-23 y CAND-24, además de las listadas en H2B1-10.

#### 20.11.2 Condiciones abiertas heredadas y nuevas

| Tema | Condiciones | Quién decide o depende |
|---|---|---|
| Lecturas de `01` (A-1, A-2, A-4, A-8, A-12, A-15, A-16) | OC-05, 06, 07, 18, 25, 31 | Usuario; Fase 4 |
| Facultades del organizador y dueño (Q-42, A-10) | OC-19 | Usuario; Fase 4 |
| Reglas deportivas y estados (Q-21, 22, 23, 29, 40, 41) | OC-20, 21, 32, 33 | Usuario; Fase 4 |
| Delimitaciones de operaciones | OC-01, 02, 03, 09, 10, 34 | Usuario; Fase 4 |
| Reasignación e invitaciones | OC-08 (CL-41), OC-11 (validez de la invitación) | Usuario; Fase 4 (DB2a-4, DB2a-5) |
| Último dueño | OC-15, 16 | Usuario |
| Creación y alta (OC-12: duplicación de una solicitud de creación de torneo, UI-01) | OC-12, 14, 17, 22, 23 | Usuario; Fase 4; R3 |
| Proyección de CAP-02-D | OC-24 | Solo si se elige D |
| Privacidad | OC-26, 27, 28 | Asesoría jurídica; R2-C |
| Sincronización y auditoría de rechazos | OC-04, 13, 30, 35, 36, 37, 38, 39, 40, 41, 42 | **Usuario (decisiones 1, 3, 10); R2-C** |
| Señal de cambio | OC-29 | R3 (ADR-06) |
| Composición con UI-06 | OC-30 | Documentada en §20.3.4; sin resolver las dependencias de R2-C |

**Totales de decisiones abiertas** (ninguna se resuelve aquí): DB1-1…6, DB2a-1…10, DB2b-1…9 y DB2c-1…11; OC-01…42; las 12 decisiones abiertas de R2-A.

### 20.12 Trazabilidad y criterios de aceptación

| Elemento | Origen |
|---|---|
| IF-32…41 | OFF-03 a OFF-25, §9.4, CMP-13, RT-13, RD-RES-04, 05, 13; R1 §5.3, §5.5, §7.3 |
| ES-25…32 | OFF-06, OFF-09, OFF-15, OFF-22, OFF-25; IN-09-05; R1 §5.5 |
| GT-19…24 | K-2d (§17.3), IF-32…41, K-3 |
| ESC-43…56 | §9.4, OFF-06, OFF-11, OFF-22, OFF-23, OFF-25; derivados marcados [DER] |
| EV-13…17 | §17.3.3, §18.8, §19.6.2 |
| Ficha UI-06 | R1 §5.5 (UI-06) |
| Amenazas | R2-A (AM-04, 10, 18, 19, 33, 34; CAB-04, 05, 06, 07, 14) |

| ID | Criterio de aceptación | Cumplimiento según el autor (a revisar por el usuario) |
|---|---|---|
| CR2B2b2-01 | UI-06 documentada con los diez campos | Cumple: §20.4 |
| CR2B2b2-02 | Las 13 unidades UI incluidas en la consolidación | Cumple: §20.6.1 |
| CR2B2b2-03 | Misma frontera lógica en línea y sincronizada | Cumple: IF-32, §20.3.4, ES-26, ES-32 |
| CR2B2b2-04 | Autorización vigente en ambas capas | Cumple: IF-33 y campo 4 de UI-06 |
| CR2B2b2-05 | Idempotencia y orden definidos como capacidades | Cumple: IF-34, IF-35, GT-20, GT-21 |
| CR2B2b2-06 | Estados intermedios prohibidos identificados | Cumple: ES-25…32 |
| CR2B2b2-07 | Auditoría de rechazos diferenciada por tipo | Cumple: §20.5.1 y §20.5.2 |
| CR2B2b2-08 | K-2d documentada sin afirmar implementación | Cumple: §20.6.3 |
| CR2B2b2-09 | CAP-02-A y CAP-02-D sin elección | Cumple: §20.7 |
| CR2B2b2-10 | R1 §8.1 sin declarar cumplido | Cumple: §20.7 |
| CR2B2b2-11 | Amenazas y dependencias identificadas | Cumple: §20.9, §20.10.1 |
| CR2B2b2-12 | Decisiones abiertas conservadas | Cumple: §20.10.2, §20.10.3 (incluye OC-40…42 y DB2c-10, DB2c-11) |
| CR2B2b2-13 | R1, R2-A, R2-B1, R2-B2-a y R2-B2-b1 intactos | Cumple según §20.13 |
| CR2B2b2-14 | Solo el archivo autorizado modificado | Cumple según §20.13 |
| CR2B2b2-15 | Borrador pendiente de aprobación | Cumple: §20.0 y §20.14 |

### 20.13 Verificación del autor

**Naturaleza.** Verificaciones **mecánicas de lectura** hechas por el autor con un guion de solo lectura. **No son una auditoría independiente:** el autor no puede verificar su propio trabajo con independencia. **Esta v0.3 no ha sido auditada ni revisada por nadie más; la v0.1 y la v0.2 fueron revisadas por agentes de contexto nuevo (§20.15), lo que tampoco equivale a una auditoría independiente.**

| # | Verificación | Resultado |
|---|---|---|
| V-1 | Las líneas 1 a 3632 son **idénticas** a la versión confirmada `c87f578` (R1, R2-A, R2-B1, R2-B2-a y R2-B2-b1 intactos); el cambio son solo líneas añadidas | Cumple: **586 líneas añadidas y 0 eliminadas** (`git diff --stat`) |
| V-2 | Solo cambió `02-arquitectura-adr.md`; `00`, `01`, `aprobaciones.md` y `bitacora.md` no cambiaron | Cumple (`git status` y sumas SHA-256) |
| V-3 | Identificadores definidos y sin referencias sin definición: IF-32…41, ES-25…32, GT-19…24, ESC-43…56, OC-34…42, DB2c-1…11, RB2c-01…11, EV-13…17, CR2B2b2-01…15 | Cumple (guion de lectura) |
| V-4 | La ficha UI-06 tiene los diez campos y sus componentes (CMP-13 + CMP-08 + CMP-11 + CMP-16) coinciden **literalmente** con R1 §5.5 | Cumple |
| V-5 | Las citas literales entre «» se buscaron en `00`, `01` y las partes previas | Todas existen, **salvo** los mensajes del usuario citados en §20.0, que proceden de la conversación y no de un archivo |
| V-6 | Exploración de términos tecnológicos (SQL, RLS, trigger, lock, índice, constraint, API, endpoint, nombres de productos o proveedores, token, caché y similares) | Los términos solo aparecen en X-6 (como **exclusión**) y en esta fila (lista de términos explorados); ninguna elección ni mecanismo |
| V-7 | Ninguna decisión abierta aparece resuelta: no se elige A ni D; no se declara demostrada la independencia de D ni cumplido R1 §8.1; no se resuelve OC-04, K-3 ni la decisión 10; DB1-1…6, DB2a-1…10, DB2b-1…9, OC-01…33 y las decisiones de R2-A intactas; OC-34…42 y DB2c-1…11 abiertas; ninguna excepción concedida a D-2 | Cumple (lectura del autor) |
| V-8 | **Misma frontera:** UI-10 dentro de UI-06 se describe como una sola transacción de datos (§20.3.4) y no como segunda unidad; la ruta sincronizada no tiene reglas de negocio propias (IF-32) | Cumple (comparación con R1 §5.3 y §5.5) |
| V-9 | **Auditoría de rechazos:** los siete registros R-1…R-7 se distinguen por su fuente y su tipo ([CONF], [DER], [REC], [ABIERTO]); la tabla cubre las 13 unidades; las alternativas CAP-09-P, F y O se exponen sin elegir | Cumple (comparación con RF-AUD-06, OFF-20, IN-09-04, IN-09-05 y §16.7.9) |
| V-10 | **Las 13 unidades** figuran en §20.6.1 y GT-01…24 cubren la matriz; K-2d se presenta como exigida y no demostrada | Cumple (lectura del autor) |
| V-11 | Ninguna garantía se presenta como implementada; los efectos externos quedan fuera de la unidad; no se define mecanismo, parámetro ni retención | Cumple (lectura del autor) |

**Límites.** Las **lecturas** de los requisitos y de los casos son del autor; los escenarios [DER] están rotulados en §20.8 y deben revisarse. La regla de que **OC-04 está en parte confirmada** (autorización con el estado vigente al sincronizar) y en parte pendiente es una **lectura del autor** [DER] (§20.3.2) que debe revisarse. R1 §8.1 **sigue sin cumplirse** (DB1-1 abierta).

### 20.14 Estado de esta parte y lo que no se hizo

**Estado: v0.3-R2B2b2, APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-20).** Corrigió los hallazgos E-01…E-14 (§20.15.1) y M-1 y M-2 (§20.15.2). Aprobación registrada en §20.0, `aprobaciones.md` y `bitacora.md`; **no versionado**.

**Qué se hizo.** Se redactó la Parte VI (§20) como **un único borrador en dos etapas internas**: la ficha UI-06, la frontera de sincronización y los requisitos de la auditoría de rechazos (etapa 1); y la síntesis de K-2 sobre las 13 unidades, la evidencia consolidada de CAP-02-A y CAP-02-D, la matriz de concurrencia, las amenazas, las dependencias, condiciones, decisiones y riesgos, y el inventario consolidado (etapa 2).

**Qué no se hizo.** No se redactó R2-C, R3 ni R4; no se aprobó R2-B2-b2, R2-B2 completo, R2 ni la Fase 2; no se resolvió ninguna DB1, DB2a, DB2b ni DB2c ni OC ni decisión de R2-A; no se resolvió K-3 ni se eligió entre CAP-09-P, F u O; no se decidió la partición ADR-05.C / ADR-09.C; no se eligió formulación; no se declaró demostrada la independencia de D ni cumplido R1 §8.1; no se fijó PAR ni se aceptó RES o RB; no se concedió excepción a D-2 ni se resolvió OC-04; no se aplicó la reasignación propuesta de AM-19 y AM-34 (OC-40); no se resolvieron OC-41 ni OC-42; no se modificaron otros archivos; no se crearon ramas, commits, pushes, PR ni merges; no se tocó `main`; no se actualizaron `aprobaciones.md` ni `bitacora.md`.

**Git.** Rama `claude/fase-2-arquitectura-amenazas`; HEAD `c87f57843bd5d497d820571937edc21c3f8c362a`; los cambios son `02-arquitectura-adr.md`, `aprobaciones.md` y `bitacora.md` **sin confirmar** (registro de la aprobación). Cualquier commit, push o PR requiere autorización expresa y separada.

### 20.15 Hallazgos y correcciones

#### 20.15.1 Auditoría de R2-B2-b2 v0.1 (E-01…E-14) y correcciones de la v0.2

**Origen.** Revisión de contexto nuevo de la v0.1 (SHA-256 `358607b4504760d7d3c2a30be10b4825bd82bb26e6a5bbf07e95bf93791fef86`), con resultado "requiere correcciones materiales". Esa revisión tiene independencia solo de contexto. Las correcciones fueron autorizadas por el usuario en una ronda única (§20.0). **No equivalen a aprobación.**

| ID | Hallazgo | Corrección en la v0.2 | Dónde |
|---|---|---|---|
| E-01 | La auditoría de rechazos trataba R-3 como obligación de registrar todo rechazo | Se separa registrar (fuentes: RF-AUD-06 solo RT-14, OFF-20, RF-AUD-05 [REC]) de persistir correctamente (R-3); las unidades sin fuente lo dicen; GT-23 y ES-30 acotados | §20.5.1, §20.5.2, §20.6.1, GT-23, ES-30 |
| E-02 | ES-31 y ESC-56 cubrían lo rechazado | ES-31 solo para lo aplicado; ESC-56 y el alcance de OFF-06 sobre lo rechazado [ABIERTO: OC-35]; no se elige O-1, O-2 ni O-3 | ES-31, IF-34, GT-20, ESC-56, §20.5.4 |
| E-03 | Conformidad de CAP-09-P-A/B/C reproducida sin fidelidad | Tabla con las conformidades de R2-A (P-A plenamente conforme; P-B no conforme con el texto literal; P-C viable con condición), condiciones conservadas, beneficio de O-1 frente a CAB-14, consecuencia sobre UI-06 en [DER]; sin elegir | §20.5.4 |
| E-04 | Reparto CMP-08 / CMP-13 y garantía de secuencia en la capa de datos | CMP-08 decide permisos y conflictos de interés; CMP-13 versión, idempotencia y secuencia; la secuencia en la capa de datos sin fuente: [DER] a justificar en ADR-05.C (OC-41) | UI-06 campo 4, §20.3.1 |
| E-05 | OC-04 y D-2 mezclados; "diferir a Fase 4" para D-2 | DB2c-1 solo trata el borde (OC-04); DB2c-10 solo las alternativas (a)–(d) de la decisión 1 de R2-A; sin excepción | §20.3.2, §20.10.3, UI-06 campo 10 |
| E-06 | Partición confirmado / pendiente de «antes y después» | Confirmado: se rechaza lo no autorizado al sincronizar aunque se capturara antes; pendiente: instante en tránsito y medición de D-2; partición del autor [DER]; OC-04 abierta | §20.3.2 |
| E-07 | Composición UI-06 / UI-10, paso d y citas | Paso d sin UI-05; componentes por composición (CMP-12, CMP-14) distinguidos; R1 §5.5 para idempotencia e IN-09-04/06 para auditoría | §20.3.1, §20.3.4, UI-06 |
| E-08 | Matriz de dependencias y ubicación de ADR-09.C y ADR-03 | ADR-09.C y ADR-03 son de R2-A; ADR-05.C (R2-C) recibe la frontera; AM-19 y AM-34: reasignación propuesta, **no aplicada** (OC-40, DB2c-11) | §20.9, §20.10 |
| E-09 | Tablas R-1…R-7 | R-4 en RT-14 propio equipo vía OFF-20; R-6 [REC] en UI-06; R-2 solo en intentos sin autoridad de UI-08; UI-01 [DER]; R-2 en UI-12 si compone UI-02 | §20.5, §20.6.1 |
| E-10 | Inventario de OC | OC-08 y OC-11 incluidas; OC-12 uniforme (UI-01, en línea); OC-38 limitada a Q-42 (a) y A-10; OC-01…42 | §20.10.2, §20.11.2 |
| E-11 | Clasificación y alcance de EV | EV-15 como caso concreto; EV-16 solo unidades con reglas; EV-17 clasifica EV-01…EV-16 [SUP] | §20.7 |
| E-12 | GT-12, GT-24, GT-14, GT-09, GT-10 | RD-RES-04 solo UI-05 y UI-10; GT-24 en UI-06 sin extenderlo; GT-14 con UI-06 por composición; GT-09 y GT-10 acotadas | §20.2, §20.6.2 |
| E-13 | Rótulos y referencias | IF-32, ES-26, ES-32, fila «Finalizar», origen de ESC-43…56, propiedad de §20.6.4 y filas OFF-01, 15, 24 corregidos | §20.2, §20.3.3, §20.3.5, §20.8 |
| E-14 | Dependencias de ADR-05.C no registradas | OC-41 (secuencia entre dos dispositivos del mismo editor) y OC-42 (estado local tras aborto técnico) abiertas; sin elegir mecanismo | §20.10.2 |

#### 20.15.2 Revisión focalizada de la v0.2 (M-1, M-2) y correcciones de la v0.3

**Origen.** Revisión de solo lectura de la v0.2 (SHA-256 `4dbcca3c8900a031474992112fb18c7dc012902e5f19bb63dcb8c65c27c0f84f`) por un agente de contexto nuevo (independencia solo de contexto), con resultado "requiere correcciones materiales" por M-1 y M-2; el resto de E-01…E-14 se clasificó como resuelto. **Las correcciones de la v0.3 no equivalen a aprobación.**

| ID | Hallazgo | Corrección en la v0.3 | Dónde |
|---|---|---|---|
| M-1 | R-3, ES-30 y GT-23 extendían la persistencia independiente a R-4 (abierta en R2-A §16.7.2) y a R-6 [REC] | R-3 es propiedad de persistencia sin crear obligación de registrar; se conserva para R-2; para R-4 está [ABIERTA] (ADR-05.C, ADR-09.C, decisión 3); R-6 sigue [REC] y condicional; ES-30 y GT-23 limitados a R-2; §20.5.2, §20.5.3 y §20.6.1 alineadas; K-3 y las alternativas sin elegir | R-3, R-4, R-6, §20.5.1–§20.5.3, ES-30, GT-23, §20.6.1 |
| M-2 | La fila de RT-14 sobre el propio equipo omitía R-4; la fila UI-03 afirmaba que CL-41 exige auditar un «rechazo» | Fila de §20.5.3 con R-4 relacionado y su persistencia no decidida; UI-03 cita CL-41 literalmente, deja el tipo de registro de la intención que no prevalece [ABIERTO] (OC-08) y distingue cuatro situaciones en §20.5.1 | §20.5.3, §20.5.2 (UI-03), §20.5.1 |

**Observaciones editoriales no bloqueantes (no corregidas en esta ronda).** Omisión del «Aviso de operación [REC]» de R2-A; OC-41 citada en el campo 4 de UI-06 junto a un asunto distinto; OC-30 duplicada en §20.11.2; clasificación [SUP] discutible de EV-01 y EV-06; ESC-50 rotulado [DER] aunque §9.4 fila 8 es literal; "respectivamente" confuso en el origen de §20.8; [SUP] ausente de la convención de rótulos; campo 7 de UI-06 sin ES-04, 16, 20 y 23; EV-16 y las reglas de integridad de UI-08 y UI-11 (ES-07, ES-11); paso h con CMP-16 frente a R1 línea 276; ESC-53 con ES-10; orden de DB2c-10 y DB2c-11 antes de DB2c-9.

---

# REGISTRO DE CIERRE DOCUMENTAL DE R2-B2 (metadatos; fuera de las Partes I–VI)

**Naturaleza.** Este apartado contiene **únicamente metadatos de aprobación y de versionado**. No añade ni modifica contenido arquitectónico, fichas, invariantes, estados prohibidos, garantías, escenarios, evidencias, amenazas, riesgos, decisiones ni condiciones de §0 a §20.

**Decisión expresa del responsable (2026-11-21, fecha inferida: la del aviso de fecha de la sesión; no hay marca de tiempo individual del mensaje).** Mensaje «TORNEO DE CAMPEONES — APROBACIÓN DEL CIERRE DOCUMENTAL CONDICIONADO DE R2-B2»: «APRUEBO EL CIERRE DOCUMENTAL CONDICIONADO DEL CONJUNTO R2-B2.» Registro autorizado con el mensaje «TORNEO DE CAMPEONES — REGISTRO DEL CIERRE DOCUMENTAL CONDICIONADO DE R2-B2»: «AUTORIZO EXCLUSIVAMENTE REGISTRAR EL CIERRE DOCUMENTAL CONDICIONADO DEL CONJUNTO R2-B2.» Estos mensajes constan en la conversación de la sesión, no en archivos del repositorio.

**Alcance aprobado.** Exclusivamente la **coherencia y suficiencia documental condicionada de R2-B2 como conjunto** (R2-B2-a, R2-B2-b1 y R2-B2-b2, todos aprobados condicionalmente y versionados): las fronteras atómicas de las 13 unidades UI-01…UI-13; IF-01…41; ES-01…32; GT-01…24; ESC-01…56; EV-01…17; la composición de UI-10 dentro de UI-06; la separación entre aplicación, auditoría de resultados y registros de rechazo; la consolidación de K-2 y K-3 como condiciones documentadas; la evaluación de CAP-02-A y CAP-02-D; y las amenazas, riesgos, operaciones candidatas y dependencias.

**Dictamen de la evaluación conjunta.** Evaluación de cierre documental de solo lectura por un subagente de contexto nuevo: «APROBABLE COMO CIERRE DOCUMENTAL CONDICIONADO»; **ningún hallazgo material de integración**; cobertura de las 13 unidades; numeración continua sin colisiones; decisiones y condiciones abiertas conservadas; aprobación global no registrada.

**Limitación de independencia.** El revisor es del mismo tipo de modelo que redactó los borradores: **independencia solo de contexto, no organizativa ni externa**. El dictamen es una recomendación y no una aprobación. La aprobación es exclusivamente documental: **no acredita implementación ni pruebas empíricas** y no constituye una auditoría externa.

**Condiciones y exclusiones (la aprobación NO implica):** seleccionar CAP-02-A ni CAP-02-D; seleccionar CAP-09-P-A, B ni C, ni las alternativas F-1…F-3 ni O-1…O-3; demostrar la independencia de CAP-02-D; declarar cumplido R1 §8.1; declarar K-2d implementada ni probada; resolver K-3; resolver DB1, DB2a, DB2b ni DB2c; resolver OC-01…42 ni las doce decisiones pendientes de R2-A; conceder excepción a D-2; fijar parámetros; aceptar riesgos; conceder excepciones; aprobar globalmente R2, ADR-02.C ni la Fase 2; autorizar R2-C, R3 ni R4. **Permanecen abiertas** DB1-1…6, DB2a-1…10, DB2b-1…9, DB2c-1…11, OC-01…42, las doce decisiones de R2-A, K-3, R1 §8.1, la independencia de CAP-02-D y la elección entre CAP-02-A y CAP-02-D.

**Observaciones editoriales no bloqueantes** (conservadas para conciliación documental posterior; no se autoriza modificar contenido técnico aprobado): las de §18.0, §19.0, §19.15, §20.0 y §20.15.2, más las de la evaluación de cierre: diferencias de formulación de OC-12 (§18.4.1 campo 10 y §20.10.2); diferencias de redacción de la auditoría en las fichas históricas («rechazo fuera (IF-09)» frente a «ninguna fuente exige registrar», §20.5.2); posible ambigüedad entre los prefijos R de §20.5 (R-1…R-7) y los R-03, R-05, R-11 y R-18 de R2-A; referencias cruzadas editoriales pendientes; y metadatos de versionado históricos desactualizados (ver la nota de precedencia).

**Commits existentes** (verificados con `git log`; cada uno toca solo `02-arquitectura-adr.md`, `aprobaciones.md` y `bitacora.md`):

| Bloque | Commit | Fecha (UTC; verificable por Git) |
|---|---|---|
| R2-A | `e246508559d4b10cb2a0c9ef3a8b4fbcf46c3b9b` | 2026-11-17T04:40:26 |
| R2-B1 | `eb0255c17d728d02003839338b4cefa18e55468d` | 2026-11-18T03:47:26 |
| R2-B2-a | `5bfc9305e29ab3c3988e932e4f62bfa3d5278f2b` | 2026-11-19T01:49:20 |
| R2-B2-b1 | `c87f57843bd5d497d820571937edc21c3f8c362a` | 2026-11-19T04:06:47 |
| R2-B2-b2 | `bbfff28b5f2d0b373aed509ae6028c8debed0de6` | 2026-11-20T04:02:35 |

**Nota de precedencia sobre estados históricos de versionado.** Las expresiones «sin versionar», «pendiente», «sin commit» o «HEAD …» de §18.0, §18.14, §19.0, §19.13, §20.0 y §20.14 describen el estado del documento **al momento de redactarse o registrarse** cada bloque y **no se reescriben**. Los commits posteriores verificables de la tabla anterior prevalecen como evidencia del versionado.

**Hash histórico del documento.** SHA-256 de `02-arquitectura-adr.md` **inmediatamente antes de este registro** (archivo versionado en `bbfff28`, con los metadatos de aprobación de R2-B2-b2): `8616b8c5bc20860bad9f5cfa29a972cdba0495bb02a87d5d41699b70c8da5675`. El contenido técnico aprobado de R2-B2-b2 conserva el hash `f1e1fb34ed1bdbd68628d0ab83ebec8a5ee912f0e49625fce5b124f57fddf30c`. Los hashes anteriores se conservan en `aprobaciones.md` y `bitacora.md`. **El hash del archivo con este registro se consigna en `aprobaciones.md` y `bitacora.md` y no se inserta aquí.**

**Estado de versionado de este registro.** **Pendiente:** sin commit, push, PR ni merge. *[Nota posterior, 2026-11-21 (fecha inferida): este registro se versionó en el commit `a13b2139042ccb8bfac8f88121a3bb26dc642ce5` (verificable por Git, 2026-11-21T01:43:24Z). Esta línea conserva el estado a la fecha de su registro.]*

**Condiciones para el cierre de la Fase 2.** Las decisiones que la Fase 1 asignó a la Fase 2 deberán resolverse antes de cerrar globalmente la Fase 2, salvo modificación o excepción formalmente aprobada. El cierre documental condicionado de R2-B2 **no equivale a aprobación global** de R2, de ADR-02.C ni de la Fase 2. Las obligaciones de R2-C, R3 y la Fase 4 conservan su alcance.


---

# PARTE VII — BLOQUE R2-C1 (v0.2-R2C1, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL, 2026-11-21 inferida)

## 21. R2-C1 — ADR-05.C: sincronización sin conexión (capacidades exigidas)

### 21.0 Alcance, autorización y reglas de esta parte

**Estado: v0.2-R2C1 — APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-21, fecha inferida), por acto expreso del responsable (ver «Aprobación condicional» más abajo).** La v0.2 corrigió la v0.1 tras la autoevaluación H-01…H-17 (§21.15). **No está versionado.** Esta parte **se añade al final del archivo, después del "Registro de cierre documental de R2-B2"**; no modifica ninguna línea de R1 (§0–§15), R2-A (§16), R2-B1 (§17), R2-B2 (§18–§20) ni de ese registro. **No aprueba ADR-05.C, R2-C, R2 ni la Fase 2.**

**Autorización recibida.** Mensaje «TORNEO DE CAMPEONES — R2-C1: BORRADOR DOCUMENTAL DE ADR-05.C» (2026-11-21, fecha inferida: la del aviso de fecha de la sesión; no hay marca de tiempo individual del mensaje): «AUTORIZO EXCLUSIVAMENTE INICIAR EL SUBBLOQUE DOCUMENTAL R2-C1 Y ELABORAR UN BORRADOR DE ADR-05.C.» Archivo autorizado: solo `02-arquitectura-adr.md`. Sin commit, push, PR ni merge. Precedió un «R2-C: RECONOCIMIENTO PRELIMINAR (SOLO LECTURA)».

**Autorización de la corrección (v0.2).** Mensaje «TORNEO DE CAMPEONES — R2-C1: CORRECCIÓN CONTROLADA H-01…H-17» (2026-11-21, fecha inferida): «AUTORIZO EXCLUSIVAMENTE CORREGIR LOS HALLAZGOS H-01 A H-17 DE LA AUTOEVALUACIÓN DE ADR-05.C.» Limitada a la Parte VII (§21); sin commit, push, PR ni merge. La autoevaluación fue del autor, **no una auditoría independiente**.

**Aprobación condicional de R2-C1 como entregable documental (2026-11-21).** Por acto expreso del responsable (mensaje «TORNEO DE CAMPEONES — REGISTRO DE APROBACIÓN DOCUMENTAL CONDICIONADA DE R2-C1»), la v0.2-R2C1 quedó **APROBADA CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL**: «APRUEBO EXCLUSIVAMENTE EL BORRADOR DOCUMENTAL R2-C1 v0.2 COMO ENTREGABLE CONDICIONADO.» Significa que el borrador puede conservarse como base documental para continuar el proceso. La fecha es la del aviso de fecha de la sesión; no hay marca de tiempo individual del mensaje.

- **La aprobación NO implica:** aprobar una formulación de D5-1…7; resolver decisiones abiertas; aprobar definitivamente ADR-05.C; aprobar globalmente R2-C, R2 ni la Fase 2; acreditar implementación ni pruebas; elegir CAP-02-A ni CAP-02-D ni CAP-09-P-A/B/C; resolver K-3 ni las decisiones 3 y 10 de R2-A; fijar parámetros, aceptar riesgos ni conceder excepciones; autorizar R2-C2, R2-C3 ni R2-C4.
- **Condiciones que permanecen abiertas:** (1) OFF-20 frente a CAP-05-M-B, pendiente de DC1-2; (2) ubicación definitiva de IN-05-21; (3) las decisiones D5-1…7, sin seleccionar formulaciones; (4) las decisiones DB, las OC (incluidas OC-43 y OC-44), K-3 y las decisiones heredadas de R2-A; (5) D-2 sin excepción; (6) un ajuste editorial no bloqueante de IN-05-08 (añadir A-10 y RD-RES-13 a lo que la condiciona); (7) la limitación de revisión indicada abajo.
- **Limitación de la revisión.** El borrador solo recibió una **autoevaluación del autor** (H-01…H-17, corregida en la v0.2) y una **comprobación final focalizada del autor** (OFF-20 frente a CAP-05-M-B: condicionado; OFF-11 frente a IN-05-21: conforme; RT-14, R1 §7.0 y fichas UI-06 y UI-10: conforme). **No hubo auditoría independiente ni revisión de contexto nuevo.** La aprobación es exclusivamente documental: **no acredita implementación ni pruebas empíricas.**
- **Integridad.** SHA-256 del **contenido técnico aprobado** (el archivo inmediatamente antes de añadir estos metadatos): `0c62bc112c65a63bd643055c40207e768b191548821e70e51c17daab2356e938`. El hash del **archivo con los metadatos de aprobación** se registra en `aprobaciones.md` y `bitacora.md`; **no se inserta en este archivo**. Los metadatos de aprobación son solo editoriales: el título y el estado de la Parte VII, este apartado, la fila de estado de §21.1, el estado y el párrafo «Git» de §21.14 y una nota posterior de trazabilidad del commit `a13b213` en el registro de cierre de R2-B2. **El contenido arquitectónico y las formulaciones de §21.1–§21.13 y §21.15 no se modificaron.**
- **Precedencia.** CR2C1-12 de §21.12 y el apartado «Qué no se hizo» de §21.14 describen el estado a la fecha del borrador; prevalece este apartado en cuanto al estado de aprobación.
- **Estado de versionado.** **Pendiente:** sin commit, push, PR ni merge. *[Nota posterior, 2026-11-22 (fecha inferida): la Parte VII y sus registros se versionaron en el commit `a468166210e0472e07dd5d0800985d3db1c6b940` (verificable por Git, 2026-11-21T22:57:42Z). Esta línea conserva el estado a la fecha de su registro.]*
- **Condición de cierre de la Fase 2.** Las decisiones que la Fase 1 asignó a la Fase 2 deberán resolverse antes de cerrar globalmente la Fase 2, salvo modificación o excepción formalmente aprobada. R2-C2, R2-C3 y R2-C4 no están iniciados; R2-C no tiene aprobación global.

**Qué contiene.** Las capacidades y garantías **exigidas** de la sincronización sin conexión (cola, secuencia, idempotencia, revalidación, conflictos, rechazos y fallos de registro, recuperación tras interrupciones), su relación con la frontera atómica de ADR-02.C y con UI-06 (UI-10 dentro), y **siete decisiones con dos formulaciones cada una**, **sin elegir ninguna**.

**Qué NO contiene ni hace.**

| # | Exclusión |
|---|---|
| X-1 | No elige entre las formulaciones CAP-05-I, M, F, S, B, E ni C (A o B): las presenta con sus ventajas, limitaciones, riesgos y dependencias |
| X-2 | No resuelve las decisiones 3 y 10 de R2-A, DB2c-2, DB2c-3, K-3, OC-35, OC-36, OC-37, OC-38, OC-39, OC-41, OC-42, OC-04 ni D-2, ni ninguna otra decisión abierta; no concede excepción a D-2 |
| X-3 | No presenta como garantía aprobada ninguna afirmación que dependa de una decisión abierta |
| X-4 | No elige base de datos, tecnología de almacenamiento, mecanismo de sincronización, librería, servicio, algoritmo de cifrado, estrategia de infraestructura ni parámetro numérico no aprobado; todo eso es de ADR-05.I y R3 (condicionado a ADR-01) |
| X-5 | No redacta ADR-04.C ni ADR-16.C (R2-C2 y R2-C3 siguen sin iniciar), ni la consolidación del modelo de amenazas, ni R3 ni R4 |
| X-6 | No modifica ADR-02.C, ADR-03.C ni ADR-09.C, ni UI-06 ni UI-10, ni ninguna ficha, invariante, estado prohibido, garantía o escenario aprobados; los **consume** |
| X-7 | No acepta riesgos, no fija PAR, no concede excepciones y no declara cumplido R1 §8.1 |
| X-8 | No modifica `00`, `01`, `aprobaciones.md` ni `bitacora.md`; **no actualiza todavía la trazabilidad histórica del commit `a13b213`** |

**Fuentes leídas directamente y límites de la lectura (H-04).** **Releídas directamente al preparar la v0.2:** en `01`, OFF-01…25 (§9.3), §9.2, la tabla de §9.4, §9.5 (filas 5 y 6 y las reglas adicionales), RD-RES-04, 05, 10 y 13, RD-EVT-12, RT-13, el inicio de RT-14, RF-AUD-05, RF-AUD-06, PRV-10 y las preguntas Q-30, Q-42 y Q-43; en R1, §5.3, §5.5 (filas UI-06 y UI-10), §7.3 (FL-3) y §8.1 y §8.2 (filas de ADR-04, ADR-05 y ADR-16 y la matriz de dependencias); en R2-A, IN-03-10, 11, 17…21, CAP-03-S-A y B, IN-09-01…13, §16.7.4, §16.7.6 y §16.7.9 y la tabla de las 13 decisiones (en su registro de `aprobaciones.md`); en R2-B1, el texto de K-2 y K-3 (§17); en R2-B2, §20.2 (IF-32…41, ES-25…32), §20.3.1, GT-19…24 y la lista de ESC-43…56. **Limitaciones:** no se releyeron íntegros el resto de RT-14, el flujo §7.0 de R1, PRV-02, 03, 06, 11 y 18, §16.8.4 de R2-A, las fichas completas de UI-06 y UI-10 de §18 a §20 ni `bitacora.md`; las afirmaciones de §21 sobre esas fuentes se apoyan en lo que citan R2-A y R2-B2 (aprobados condicionalmente) y se tienen por **no comprobadas de nuevo** hasta su relectura. Del skill se consultaron SKILL.md y las puertas de aprobación; `seguridad.md` solo en sus encabezados.

**Convención.** [CONF]: texto confirmado de `01` o R1. [REC]: recomendación o propuesta (no confirmada). [DER]: derivada por el autor (se explica el paso). [ABIERTO]: sin resolver; ninguna se elige. Identificadores nuevos (continúan o crean series sin colisión): **IN-05-nn** (invariantes y garantías de ADR-05.C), **ES-33…** (estados prohibidos), **ESC-57…** (escenarios), **CAP-05-x-A/B** (formulaciones), **D5-n** (decisiones de formulación), **OC-43…**, **DC1-n** (decisiones para el responsable), **RR5-nn** (riesgos documentales) y **CR2C1-nn** (criterios de aceptación).

### 21.1 ADR-05.C — Identificación

| Campo | Valor |
|---|---|
| ADR | ADR-05.C — Sincronización sin conexión: capacidades exigidas (R1 §8.1, ADR-05; R1 §8.2) |
| Pregunta de R1 §8.1 | «¿Cómo se estructuran la cola, la secuencia, la idempotencia, la revalidación y los conflictos?» y cómo se aplica a la ruta sincronizada la frontera atómica de ADR-02.C (UI-06 con UI-10 dentro); **pregunta abierta [OFF-06, OFF-20]**: si persisten fuera de la unidad el registro de idempotencia de una operación rechazada y los metadatos de rechazadas o en conflicto |
| Alternativas exigidas | **≥ 2** (R1 §8.1, ADR-05) |
| Entradas | R1 §5.3, §5.5, §7.0, §7.3, §8.1, §8.2; `01` OFF-01…25, §9.2, §9.4, §9.5; R2-A (ADR-03.C, ADR-09.C, §16.7.9); R2-B1 (K-2, K-3); R2-B2 (§20) |
| Parámetros abiertos asignados por R1 §8.1 | Q-29, Q-30, Q-42, Q-43, A-9, A-10, A-11, A-13, A-14, A-15 |
| Requiere (R1 §8.2) | ADR-02.C (frontera atómica), ADR-03.C, ADR-09.C (persistencia de rechazos e incidencias) |
| Alimenta | ADR-16.C (formato de cola y datos locales; R1 §8.2), ADR-08 y ADR-01/ADR-10 (R3), ADR-05.I |
| Estado | v0.2-R2C1: APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-21, inferida); ADR-05.C **no** queda aprobada definitivamente |

### 21.2 Dependencias y conflictos entre fuentes

**Qué consume ADR-05.C de lo ya aprobado (como entrada, sin modificarlo):**

| Fuente | Qué aporta a ADR-05.C | Estado |
|---|---|---|
| ADR-02.C / R2-B2 (§20) | Frontera atómica de UI-06 con UI-10 dentro (§20.3.1, §20.3.4); IF-32…41; ES-25…32; GT-19…24; ESC-43…56; registros de rechazo R-1…R-7 (§20.5.1) | Aprobado condicionalmente como entregable documental; formulaciones como PROPUESTA |
| ADR-03.C / R2-A | IN-03-10 (sincronizar exige sesión válida y permisos vigentes); IN-03-11 (el contexto local no autentica ni autoriza); IN-03-19 y IN-03-20 ([REC]: la cuenta que sincroniza es la que capturó; la cola se aísla por cuenta); CAP-03-S-A y B (nivel exigido al sincronizar, OC-39 abierta) | Aprobado condicionalmente; PROPUESTA |
| ADR-09.C / R2-A | IN-09-04 (lo aplicado se registra dentro de la misma unidad); IN-09-05 (el registro de un rechazo persiste aunque la operación no se aplique); IN-09-06; D9-1 (CAP-09-P-A/B/C); §16.7.6 (la persistencia de la idempotencia de OFF-06 y de OFF-20 es de **ADR-05.C**, no de ADR-09.C); §16.7.9 (F-1…F-3, O-1…O-3) | Aprobado condicionalmente; PROPUESTA; decisiones 3 y 10 abiertas |
| R1 §5.3 | CMP-13 («Idempotencia, secuencia y conflictos de versión»; «No contiene reglas de negocio ni decide permisos»), CMP-04 (almacén local y cola), CMP-05 (sincronizador del cliente), CMP-08 (decide permisos), CMP-11 (aplica), CMP-16 (audita), CMP-17 (contenido de la exportación) | Aprobado (R1) |
| `01` §9.2, §9.4, §9.5 | Estados de una operación; política de conflictos (12 filas); parámetros del modo sin conexión (Q-34) | [CONF] |

**Conflictos o tensiones entre fuentes (se señalan, no se resuelven):**

| # | Tensión | Fuentes | Tratamiento en este borrador |
|---|---|---|---|
| T-1 | R1 §5.5 y §8.1 atribuyen la persistencia de OFF-06 y OFF-20 a «ADR-05.C y ADR-09.C»; §8.2 solo hace que ADR-05.C requiera ADR-09.C | R1 §5.5, §8.1, §8.2; R2-A §16.7.6, H2A-06 | Se conserva la partición **provisional** de R2-A (H2A-06; decisión 3, **abierta**: no es una decisión definitiva); D5-1 y D5-2 la dejan explícita |
| T-2 | OFF-07 («bloqueadas por operación anterior») es [REC]; OFF-25 (hueco bloquea; dependencia de la reanudación) es [CONF] | `01` OFF-07, OFF-25 | D5-5 |
| T-3 | §9.4 fila 10 dice que, con el mismo editor en dos dispositivos, las operaciones «Se fusionan por identificador y secuencia»; OFF-03 asigna un «número de secuencia por partido» y OFF-25 exige una secuencia «contigua», sin precisar si es por dispositivo o por editor | `01` §9.4 fila 10, OFF-03, OFF-25 | D5-4 (OC-41) |
| T-4 | OFF-06 exige devolver «el resultado original» ante un reenvío, pero no dice cómo se aplica a lo rechazado | `01` OFF-06; R2-A §16.7.9 | D5-1 (OC-35) |
| T-5 | La ficha UI-03 de R2-B2-a dice que la intención que no prevalece «se audita como rechazo»; OC-08 la deja abierta | R2-B2-a §18.4.3, OC-08 | No afecta a ADR-05.C; se conserva abierta |
| T-6 | IN-03-19 es [REC]: OFF-22 no dice que la sesión que sincroniza deba ser la de la cuenta que capturó | R2-A IN-03-19; `01` OFF-22 | IN-05-11 lo trata como [REC] |
| T-7 | `01` OFF-12 dice que el cifrado local «se evalúa en la Fase 2» sin prometerlo; el formato de la cola es de ADR-05.C y la protección local, de ADR-16.C | `01` OFF-12; R1 §8.1, §8.2 | ADR-05.C no decide protección local; se señala la dependencia con ADR-16.C |
| T-8 | OFF-09 dice que «ninguna captura local se pierde en silencio»; OFF-17 reconoce que el navegador puede eliminar los datos locales y que **no se promete conservación** | `01` OFF-09, OFF-17 | IN-05-12 acota la exigencia a lo que depende de la aplicación y del servidor; **no promete una persistencia imposible**; la protección y el desalojo del almacén son de ADR-16.C |

### 21.3 Qué decide ADR-05.C y qué no

| Asunto | ¿Lo decide ADR-05.C? | Quién |
|---|---|---|
| Qué capacidades exige la cola, la secuencia, la idempotencia, la revalidación, los conflictos y la recuperación | **Sí** (como capacidades exigidas, sin mecanismo) | ADR-05.C |
| Si la idempotencia y los metadatos de OFF-20 de lo rechazado persisten de forma independiente | **Propone alternativas** (D5-1, D5-2); decide el responsable | Decisiones 3 y 10 de R2-A; DB2c-2 y DB2c-3 |
| Frontera atómica de UI-06 y UI-10 | **No**: la **consume** de ADR-02.C | ADR-02.C |
| Autenticación, nivel exigido al sincronizar, sesiones | **No** | ADR-03.C (OC-39) |
| Qué se audita, inmutabilidad, persistencia independiente de R-2 | **No** | ADR-09.C |
| Protección del almacén local, cifrado, límites de almacenamiento | **No** | ADR-16.C (R2-C3) |
| Visibilidad y proyecciones públicas | **No** | ADR-04.C (R2-C2); ADR-06 (R3) |
| Mecanismo concreto de cola, sincronización, orden, reintentos o reconciliación | **No** | ADR-05.I (R3, tras ADR-01) |
| Cálculo de la tabla | **No** | ADR-08 (R3) |

### 21.4 Invariantes y garantías exigidas de ADR-05.C (IN-05-nn)

Son **capacidades exigidas** cuando su tipo es [CONF] o [DER]; las marcadas [REC] son **propuestas condicionadas** a una decisión del responsable (DC1-8) y las [ABIERTO] **no fijan** garantía alguna. **Ninguna está implementada ni probada.** Se **suman** a IF-01…41, ES-01…32 y GT-01…24 y no los modifican. La columna "Condicionada por" indica qué formulación o decisión abierta limita el alcance de la garantía; donde figura una decisión abierta, **la garantía vale solo en la parte confirmada** (X-3).

| ID | Invariante o garantía exigida | Tipo | Fuente | Condicionada por |
|---|---|---|---|---|
| IN-05-01 | **Cola local persistente:** sobrevive a cerrar la app, a un reinicio y a perder la pestaña; cada operación lleva identificador único generado en el cliente, número de secuencia por partido, tipo, datos, versión base conocida, dispositivo, usuario y hora local | [CONF] | OFF-03, OFF-02 | Protección y límites del almacén local: ADR-16.C; OFF-17 |
| IN-05-02 | **Estados de la operación local** (`pendiente`, `enviando`, `confirmada`, `en conflicto`, `rechazada`) diferenciados por operación y con resumen por partido; solo `confirmada` puede ser pública | [CONF] | OFF-04, OFF-15, §9.2 | D5-6 (estado tras un aborto técnico), OC-42, OC-44 |
| IN-05-03 | **Disparadores de sincronización:** al recuperar la conexión con la app abierta, al abrir o volver a la app, por «Sincronizar ahora» y por reintento periódico con espera creciente; no se depende de sincronización en segundo plano; indicador permanente de pendientes. **Los plazos y la espera creciente no se fijan aquí** | [DER] (OFF-05 es DER en `01`) | OFF-05, RK-11 | iOS sin verificar en dispositivo (HR1-07) |
| IN-05-04 | **Idempotencia de lo aplicado:** cada operación se aplica como máximo una vez; un reenvío del mismo identificador, también concurrente, devuelve el resultado original sin repetir efectos ni registros de auditoría | [CONF] | OFF-06, §9.4 fila 9, IF-34, GT-20 | — |
| IN-05-05 | **Idempotencia de lo rechazado:** el alcance de OFF-06 sobre una operación rechazada **no está fijado por `01`** | [ABIERTO] | OFF-06; R2-A §16.7.9 | D5-1, OC-35, decisión 10 de R2-A |
| IN-05-06 | **Orden por secuencia y contigüidad por partido:** las operaciones se validan en orden de secuencia contra el estado resultante de las anteriores aceptadas, partiendo del estado oficial; un hueco bloquea las operaciones posteriores de ese partido y se registra | [CONF] | OFF-25, IF-35, GT-21 | D5-4 (contigüidad entre dispositivos del mismo editor, OC-41) |
| IN-05-07 | **Bloqueo por operación fallida:** que, si una operación falla, las siguientes del mismo partido queden «bloqueadas por operación anterior» y las de otros partidos continúen | [REC] | OFF-07 | D5-5, OC-37 |
| IN-05-08 | **Dependencia de la reanudación:** si la reanudación se rechaza o queda en conflicto, ningún evento posterior de la secuencia se aplica ni se publica; un evento con el estado oficial `suspendido` y sin reanudación aceptada previa en su secuencia se rechaza | [CONF] | OFF-25, RD-EVT-12, RD-RES-13, IF-36, GT-21 | A-9, A-11, A-14 (lecturas provisionales) |
| IN-05-09 | **Atomicidad por operación:** cada operación se aplica completa o no se aplica; una interrupción de la red durante el envío no deja estados parciales | [REC] en OFF-08; [DER] en su aplicación a la unidad | OFF-08, IF-38, GT-19 | D5-7 (qué ve el cliente tras una interrupción) |
| IN-05-10 | **La revalidación es la de la frontera:** al sincronizar, cada operación recorre el mismo camino lógico que su equivalente en línea (autorización con el estado vigente al sincronizar, evaluada de forma atómica con la aplicación); CMP-13 no decide permisos; ningún dato local concede permisos | [CONF] en OFF-22 y RT-13 (autorización con el estado vigente al sincronizar, atómica con la aplicación, y que ningún dato local concede permisos); [DER] en IN-03-11 y en la consecuencia de que no hay otra ruta | OFF-22, OFF-23, RT-13, IF-32, IF-33, IF-39, IN-03-10, IN-03-11 | OC-04 y D-2 (instante y medición); OC-39 (nivel de autenticación) |
| IN-05-11 | **La cuenta que sincroniza es la que capturó:** cada operación queda ligada a la cuenta que la capturó, se evalúa con esa cuenta y una sesión de otra cuenta nunca sincroniza ni apropia operaciones ajenas; la cola se aísla por cuenta en el dispositivo | [REC] | IN-03-19, IN-03-20, OFF-03, OFF-12, CAB-30 | ADR-03.C; ADR-16.C (aislamiento local) |
| IN-05-12 | **Conflictos sin sobrescritura y sin pérdida silenciosa:** lo confirmado por el servidor nunca se sobrescribe en silencio y ninguna captura local se pierde en silencio **por la aplicación ni por el servidor**; las filas de §9.4 conservan sus resultados. **No cubre la eliminación del almacenamiento local por el navegador**, que `01` reconoce sin garantía de conservación (OFF-17; T-8) | [CONF] | OFF-09, OFF-17, §9.4, IF-37, GT-24, RD-RES-04 | D5-3 y D5-6 para los casos de fallo técnico; ADR-16.C (desalojo) |
| IN-05-13 | **Conservación local de lo en conflicto o rechazado:** hasta que el usuario lo reintente, lo descarte con confirmación o use la exportación limitada, con retención máxima de 30 días, plazo informado y constancia mínima; sin garantía si el navegador elimina el almacenamiento local o el dispositivo permanece desconectado | [CONF] | OFF-10, OFF-17, PRV-10 | ADR-16.C; RES-07 |
| IN-05-14 | **Lo local jamás es público;** finalizar sin conexión es «finalización pendiente» y el partido solo se considera finalizado y público cuando el servidor la confirma | [CONF] | OFF-15, OFF-21, IF-40 | — |
| IN-05-15 | **Relojes:** el orden se basa en secuencia, no en reloj; el servidor registra su propia hora de recepción; si el reloj del dispositivo difiere más de 5 minutos del servidor al sincronizar, se avisa y se registra | [REC] | OFF-13, AM-10 | — |
| IN-05-16 | **Actualización de la app con cola pendiente:** una nueva versión no descarta la cola; el formato de la cola tiene versión y se migra; si no es compatible, se bloquea la actualización hasta sincronizar | [REC] | OFF-16 | ADR-16.C; ADR-05.I |
| IN-05-17 | **Exportación limitada:** el contenido lo fija CMP-17 y lo presenta CMP-05; por defecto solo identificador del partido, abreviaturas de equipos y marcador; no concede permisos, no se reimporta ni se aplica como resultado oficial | [CONF] | OFF-24, OFF-11, PRV-18 | Q-43 (criterio de autorización de eventos, minutos, dorsales y equipos); PRV-18 |
| IN-05-18 | **Registro de rechazos, bloqueos y metadatos:** ADR-05.C **aplica** la capacidad de ADR-09.C sin redefinirla, **según la partición provisional de R2-A (decisión 3 abierta)**: los registros que una fuente exige (R-2, RF-AUD-06) persisten conforme a IN-09-05; la persistencia independiente de **R-4** (OFF-20) y del registro de idempotencia de lo rechazado (R-5) está **[ABIERTA]**; R-6 (RF-AUD-05) sigue [REC] | [CONF] el registro de R-2 (RF-AUD-06); [DER] su persistencia independiente (IN-09-05); [ABIERTO] en R-4 y R-5; [REC] en R-6 | IN-09-05, RF-AUD-06, OFF-20, §20.5.1, R2-A §16.7.4 y §16.7.6 | D5-1, D5-2, D5-3; K-3; decisiones 3 y 10 de R2-A |
| IN-05-19 | **Recuperación tras interrupciones:** tras un corte durante el envío, un cierre forzado o una pérdida de respuesta, cada operación queda en un único estado coherente y puede reconciliarse sin duplicar efectos ni perderse; **el mecanismo no se fija** | [DER] de OFF-08, OFF-06 y OFF-09 | OFF-05, OFF-06, OFF-08, OFF-09 | D5-7, OC-43, OC-44 |
| IN-05-20 | **Sin permisos nuevos, sin reducción de garantías y sin selección tecnológica:** ADR-05.C no concede facultad alguna y respeta PA-13 y RC-23 | [DER] / [CONF] | R1 RC-23, RC-25; R2-A IN-03-17, IN-09-11 | — |
| IN-05-21 | **Conducta del cliente ante la pérdida de autorización [CONF en OFF-11]:** el cliente deja de enviar las operaciones de un usuario al saber que perdió autorización; con la cuenta desactivada, al primer contacto con el servidor cierra la sesión, bloquea el acceso a los datos locales, ofrece **una sola vez** la exportación limitada de OFF-24 y elimina de forma controlada los datos locales (tras exportar o confirmar, nunca antes de mostrar el aviso); una PWA no puede borrar datos de un dispositivo que nunca se conecta (riesgo residual) | [CONF] | OFF-11, OFF-12, OFF-24, §9.4 fila 5 | La ejecución local puede estar condicionada por ADR-16.C; **no se traslada definitivamente**: la ubicación de esta exigencia no está decidida y no hay respaldo documental para asignarla |

### 21.5 Formulaciones alternativas, sin elegir

Cada decisión presenta **dos formulaciones**. Ninguna se aprueba, ninguna se recomienda y ninguna desplaza a las demás. «Dependencias» enumera lo que impide cerrar la formulación. Las referencias a ESC-nn de §20.8 son escenarios aprobados; las nuevas (ESC-57…) están en §21.7.

#### D5-1 — Alcance de la idempotencia sobre una operación rechazada (OC-35; decisión 10 de R2-A)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-05-I-A — Resultado conservado por identificador** | El servidor conserva el resultado de las operaciones rechazadas o en conflicto y devuelve el rechazo original ante un reenvío del mismo identificador, también concurrente | R2-A §16.7.9 dice de O-1: «Cumple OFF-06 al pie de la letra» (OFF-06 exige devolver «el resultado original», y `01` no fija si alcanza a lo rechazado); un reenvío no re-ejecuta la evaluación; limita la inundación de registros (CAB-14) | Exige conservar un resultado por cada operación rechazada (punto abierto de ADR-05.C, OFF-20); la retención de esos resultados depende de PRV-10 y de ADR-17 | Se pierde la visibilidad de los reintentos abusivos si no se añade un contador (O-3); crece lo conservado | O-1 u O-3 de R2-A §16.7.9; D5-2; decisión 10; PRV-10 |
| **CAP-05-I-B — Idempotencia solo de lo aplicado; lo rechazado se reevalúa** | Solo se conserva el resultado de lo aplicado; un reenvío de una operación rechazada se evalúa de nuevo con el estado vigente en ese momento | No obliga a conservar resultados de rechazadas; el resultado puede cambiar si el estado cambió (por ejemplo, si una asignación se restablece) | Reevaluar **no devuelve «el resultado original»** de una operación rechazada; `01` no fija si OFF-06 alcanza a lo rechazado (OC-35), por lo que no hay conformidad ni disconformidad establecida; cada reenvío exige una nueva decisión de CMP-08 | Aumenta la carga de evaluación y de registro por reenvío (O-2); un resultado que cambia entre reintentos puede confundir al usuario | O-2 de R2-A; D5-3; OC-35; K-3; decisión 10 |

#### D5-2 — Persistencia de los metadatos de operaciones rechazadas o en conflicto, R-4 (OFF-20; decisión 3 de R2-A)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-05-M-A — Persistencia independiente de R-4** | Los metadatos mínimos de lo rechazado o en conflicto que el servidor efectivamente recibe persisten aunque la operación no se aplique, con la misma garantía que IN-09-05 da a R-2 | El organizador ve incidencias aunque la unidad se haya abortado (OFF-20, D-9); coherente con CAP-09-P-A o P-C | Exige que ADR-09.C aplique su capacidad también a R-4 (decisión 3); aumenta el volumen del registro independiente | Inundación del registro (CAB-14); dependencia de K-3 | CAP-09-P-A o P-C; decisión 3; decisión 10; DB2c-2; DB2c-3 |
| **CAP-05-M-B — Metadatos sin independencia de la unidad** | Los metadatos se registran cuando la decisión de rechazo no aborta la unidad; no se exige que persistan si se aborta | No amplía la capacidad general de ADR-09.C; menor volumen | Puede perder metadatos de operaciones rechazadas por abortos (restricciones de integridad); **posible no conformidad con OFF-20 [CONF]**, que dice que el servidor registra metadatos mínimos de las operaciones rechazadas o en conflicto que efectivamente recibe y no prevé excepción por aborto; **no se crea ninguna excepción a OFF-20** y la conformidad la decide el responsable | El organizador no vería ciertas incidencias; incompatible con tratar R-4 igual que R-2 | CAP-09-P-B **[DER: asociación inferida por el autor; R2-A no la formula para R-4]** (R2-A la califica como variante parcial, no conforme con el texto literal de R1); decisión 3; K-3 |

#### D5-3 — Fallo del registro de un rechazo exigido (OC-36; R2-A §16.7.9)

F-1 (devolver el rechazo y continuar sin registro) es **no conforme con IN-09-05** según R2-A y no se presenta como formulación viable.

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-05-F-A — Error recuperable y operación sin resolver (F-2)** | Si un registro **exigido por R-2 (RF-AUD-06)** no puede persistirse, no se devuelve un rechazo definitivo: la operación sigue `pendiente` y se reintenta | Conserva la auditoría; no añade un medio durable adicional | Un fallo del registro bloquea las operaciones rechazadas **cuyo registro exige R-2**; no alcanza a los rechazos sin registro exigido, ni a R-4 mientras su persistencia siga [ABIERTA] | **Denegación inducida** (RB2c-06); reintentos masivos (OFF-05); estado local del cliente durante el fallo (D5-6) | F-2 de R2-A; decisión 10; D5-6; D5-7 |
| **CAP-05-F-B — Intención durable previa y conciliación (F-3)** | La intención de registro **exigido por R-2** se hace durable antes de responder y se concilia después; si no está disponible, se aplica CAP-05-F-A | Reduce la ventana de pérdida y la dependencia de un único almacén | Añade complejidad y una ventana hasta la conciliación; **solo conforme si la intención es durable antes de responder** | Una ventana de pérdida incumple IN-09-05; más superficie de fallo | F-3 de R2-A; CAP-09-P-C; decisión 10; ADR-05.I / ADR-09.I (R3) |

#### D5-4 — Contigüidad de la secuencia entre dos dispositivos del mismo editor (OC-41; §9.4 fila 10)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-05-S-A — Una secuencia contigua por partido y editor, compartida por sus dispositivos** | La contigüidad de OFF-25 se evalúa sobre la secuencia del partido sin distinguir dispositivo | OFF-03 habla de un «número de secuencia por partido» y OFF-25 dice «La secuencia debe ser contigua», sin mencionar el dispositivo; un solo criterio de hueco | Exige que los dispositivos del editor coordinen la numeración; un dispositivo sin conexión no puede conocer los números del otro | Huecos aparentes que bloquearían operaciones legítimas (OFF-25); más conflictos que resolver (§9.4 filas 2, 3 y 10) | OC-41; OFF-03; OFF-25; §9.4 fila 10; ADR-05.I |
| **CAP-05-S-B — Una secuencia contigua por dispositivo y partido, con reconciliación declarada entre ellas** | La contigüidad se evalúa por dispositivo; el cruce entre dispositivos se resuelve por identificador y por versión (§9.4 fila 10) | Cada dispositivo numera de forma autónoma sin conocer al otro; compatible con «Se fusionan por identificador y secuencia» | **Requiere una lectura de OFF-03 y OFF-25 que `01` no fija** (contigüidad por dispositivo, frente a un «número de secuencia por partido»); hay que declarar cómo se ordenan las operaciones de distintos dispositivos | Un hueco por dispositivo no cubre dependencias entre dispositivos (p. ej. una reanudación capturada en uno y eventos en otro); riesgo de contradecir OFF-25 si se lee por partido | OC-41; OFF-03; OFF-25; IF-35; ES-27; posible aclaración de `01` |

#### D5-5 — Bloqueo tras una operación fallida (OFF-07 [REC]; OC-37)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-05-B-A — Adoptar OFF-07** | Si falla una operación, las siguientes **del mismo partido** quedan «bloqueadas por operación anterior» (siguen `pendiente`); las de otros partidos continúan | Evita aplicar operaciones sobre un estado incierto; simple de explicar al usuario | Bloquea operaciones que podrían ser independientes (p. ej. eventos no dependientes de la reanudación); OFF-07 es [REC], no [CONF] | Pérdida de captura aparente si el bloqueo se prolonga; presión sobre OFF-10 (retención de 30 días) | OFF-07; OC-37; DB2c-9; D5-6 |
| **CAP-05-B-B — Bloquear solo lo que `01` confirma** | Se bloquea por hueco (OFF-25) y por dependencia de la reanudación (OFF-25, RD-EVT-12); el resto se evalúa operación por operación | Se limita a lo [CONF]; no frena operaciones independientes | Exige definir qué operaciones dependen de cuál (A-9, A-11, A-14); más casos que probar | Aplicar una operación sobre un estado que una anterior fallida habría invalidado, si el criterio de dependencia es incompleto | OFF-25; RD-EVT-12; OC-37; DB2c-9; A-9, A-11, A-14 |

#### D5-6 — Estado local de una operación tras un aborto técnico distinto de un rechazo (OC-42; OC-44)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-05-E-A — Permanece `pendiente` y se reintenta** | Un aborto técnico (sin rechazo ni conflicto) no cambia el estado local: la operación sigue `pendiente` y se reintenta (OFF-05) | **No requiere un estado nuevo**: `01` solo define `en conflicto` y `rechazada` como resultados negativos; coherente con F-2 | **Requiere la transición `enviando` → `pendiente` tras un aborto técnico, que §9.2 y OFF-04 no definen (OC-44)**; el usuario no distingue un aborto técnico de una espera normal; reintentos indefinidos si el fallo persiste | Denegación inducida (RB2c-06); presión sobre OFF-10; indicadores de pendientes engañosos | §9.2; OFF-04; OFF-05; D5-3; OC-44 |
| **CAP-05-E-B — Indicación adicional visible del fallo técnico** | El cliente muestra, sin cambiar el significado de los cinco estados, una indicación del fallo técnico y del número de reintentos | El usuario conoce la causa; ayuda a diagnosticar un fallo persistente | **Puede exigir modificar OFF-04 y §9.2** si se considera un estado nuevo (cambio de `01`, fuera de este bloque); más trabajo de interfaz (Fase 3) | Contradecir los cinco estados confirmados si se interpreta como un sexto estado | OFF-04; §9.2; UX/UI (Fase 3); modificación formal de `01` solo si el responsable la autoriza |

#### D5-7 — Reconciliación tras una interrupción o pérdida de respuesta (IN-05-19; OC-43, OC-44)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-05-C-A — El reenvío idempotente como única reconciliación** | El cliente reenvía la operación hasta recibir una respuesta; la idempotencia del servidor devuelve el resultado | No requiere un canal de consulta adicional; usa OFF-06 | Para una operación rechazada depende de D5-1 (si el rechazo no se conserva, la respuesta puede cambiar); la espera puede ser larga | Reenvíos repetidos que agravan la carga o la inundación del registro | OFF-05; OFF-06; D5-1 (CAP-05-I-A o B); OC-35; OC-44 |
| **CAP-05-C-B — Reconciliación explícita del estado de lo enviado** | Tras una interrupción, el cliente consulta al servidor el estado de las operaciones `enviando` antes de reenviar | Evita reenvíos innecesarios; el estado local se alinea con el del servidor | Añade una capacidad de consulta que el servidor debe sostener (qué conserva y por cuánto tiempo) y **podría exigir una responsabilidad nueva de CMP-13, no prevista en R1 §5.3 (donde figura como ingesta de sincronización); se deja abierta, sin modificar R1**; más superficie de revelación (el servidor responde sobre lo que recibió) | Revelar información a una sesión no legitimada si no se aplica IN-05-11; mayor complejidad | OFF-08; IN-05-11; D5-1; R1 §5.3 (responsabilidad de CMP-13); ADR-05.I; OC-43 |

#### Interdependencias entre las decisiones

| Decisión | Se condiciona con | Observación |
|---|---|---|
| D5-1 | D5-2, D5-3, D5-7, K-3, decisión 10 | CAP-05-I-A exige conservar resultados; CAP-05-I-B interactúa con CAP-05-C-A |
| D5-2 | decisión 3 de R2-A, DB2c-3, K-3, CAP-09-P | CAP-05-M-A presupone CAP-09-P-A o P-C |
| D5-3 | decisión 10, D5-6 | CAP-05-F-A fija el estado local `pendiente` durante el fallo |
| D5-4 | OC-41; D5-5 | La contigüidad por dispositivo cambia qué es un «hueco» |
| D5-5 | OC-37, D5-4, D5-6 | OFF-07 es [REC] |
| D5-6 | OC-42, OC-44, D5-3 | CAP-05-E-B puede exigir modificar `01` |
| D5-7 | D5-1, OC-43, OC-44 | CAP-05-C-A depende de lo que se conserve de lo rechazado |

### 21.6 Relación con la frontera atómica de ADR-02.C: UI-06 con UI-10 dentro

**Regla fundamental (consumida de §20.3.1 y §20.3.4, sin modificarla).** La operación recibida recorre la misma frontera lógica de autorización, aplicación y auditoría que su equivalente en línea; **no existe una segunda ruta de negocio ni una segunda unidad atómica de UI-10** (IF-32, ES-32, GT-19). ADR-05.C **aplica** esa frontera a la ruta sincronizada; no la redefine.

| Paso (literal de §20.3.1) | Componente | Dentro de la unidad (literal de §20.3.1) | Qué aporta ADR-05.C (sin cambiar la clasificación) |
|---|---|---|---|
| a. Recepción: identificador, secuencia, versión base y datos de la operación (OFF-03) | CMP-13 | Sí, en lo que decide (idempotencia, orden, versión) | IN-05-01 (contenido mínimo de cada operación) |
| b. Idempotencia: ¿ya se aplicó? Orden y contigüidad por partido; versión | CMP-13 | Sí | IN-05-04…IN-05-08, IN-05-12; D5-1, D5-4, D5-5; CMP-13 detecta y no decide permisos |
| c. Revalidación completa con el flujo §7.0 (pasos 2 a 5) | CMP-08 | **Sí** (unidad indivisible: R1 §7.0) | IN-05-10 (la revalidación es la de CMP-08 con el estado vigente al sincronizar) |
| d. Aplicación de la operación (UI-10 si es un resultado, con su recálculo; o la variante de estado de §20.3.5) | CMP-11 (CMP-12 y CMP-14 por la composición de UI-10) | **Sí** | Ninguna regla de negocio: CMP-13 solo entrega |
| e. Registro de la idempotencia de lo aplicado y evento de auditoría de lo aplicado | CMP-13, CMP-16 | **Sí** (R1 §5.5 para la idempotencia; IN-09-04 e IN-09-06 para la auditoría) | IN-05-04; IN-05-18 (R-1 se registra dentro, IN-09-04) |
| f. Señal de cambio, solo si ADR-06 la adopta | CMP-14 | Sí (IF-22) | Nada: es de ADR-06 (R3) |
| g. Respuesta al cliente y estado local de la operación (§9.2) | CMP-13, CMP-05 | **No** | IN-05-02, IN-05-09, IN-05-19; D5-6, D5-7 |
| h. Registro del rechazo (si se rechazó) | CMP-16 | **No** (IF-09) | IN-05-18: el registro de R-2 es [CONF] (RF-AUD-06) y su persistencia independiente [DER] (IN-09-05); R-4 y R-5 [ABIERTOS]; D5-1, D5-2, D5-3 |
| i. Conservación local de lo rechazado o en conflicto (≤ 30 días), exportación limitada y constancia de eliminación | CMP-04, CMP-05 | **No** (cliente; OFF-10, OFF-24) | IN-05-13, IN-05-17, IN-05-21 |
| j. Entrega pública y copias en Z0 | CMP-15 | **No** (§19.3) | IN-05-14 (lo local jamás es público); la entrega es de ADR-04.C y ADR-06 |

**Quién decide cada aporte:** ADR-05.C (a, b, g, h y la parte de i que le asigna R1 §5.3), ADR-02.C (c, d), ADR-03.C (c, el nivel de autenticación), ADR-09.C (e, h, persistencia independiente de R-2) y ADR-16.C (el almacén de i). La ubicación definitiva de la conducta del cliente de OFF-11 (IN-05-21) no está decidida.

**Límites que ADR-05.C no puede cruzar:** (1) no mueve a la unidad nada que R1 deje fuera (respuesta, estado local, registros de rechazo, exportación, entrega pública); (2) no divide la unidad ni crea una unidad adicional para UI-10 (ES-32); (3) no relaja GT-19…24 ni ES-25…32; (4) no define qué verifica cada capa de autorización (ADR-02.C); (5) no elige el mecanismo de aislamiento ni de serialización de las operaciones concurrentes sobre un mismo partido (ADR-05.I y R3).

### 21.7 Estados prohibidos adicionales y casos límite

#### Estados prohibidos (se suman a ES-01…32)

| ID | Estado prohibido | Tipo | Fuente |
|---|---|---|---|
| ES-33 | Una operación enviada cuya respuesta se pierde queda sin un único estado coherente en el cliente (duplicada, descartada o `enviando` sin posibilidad de reconciliarse) | [DER] | OFF-08, OFF-09, IN-05-19 |
| ES-34 | Una captura local **descartada en silencio** por un conflicto, un rechazo, un fallo técnico o una actualización de la app | [CONF] en OFF-09 y OFF-10 (conflicto o rechazo); [DER] en el fallo técnico; [REC] en OFF-16 (actualización de la app) | OFF-09, OFF-10, OFF-16 |
| ES-35 | La cola o los datos preparados de una cuenta **accesibles o sincronizables** por la sesión de otra cuenta | [REC] | IN-03-19, IN-03-20, CAB-30 |
| ES-36 | Una «finalización pendiente» tratada como finalizada o pública antes de la confirmación del servidor | [CONF] | OFF-21, OFF-15 |
| ES-37 | Una operación aplicada porque el cliente alteró el orden, la secuencia, el reloj o el contenido local **sin que el servidor revalide** autorización, versión y contigüidad | [DER] | OFF-13, OFF-22, IF-33, AM-10 |

#### Escenarios de concurrencia y de interrupción (se suman a ESC-43…56)

«Admisible» significa que **equivale a alguna ordenación secuencial** (IF-05). **Ningún escenario se resuelve aquí por un mecanismo**; donde hay formulaciones, se indican sin elegir.

| ID | Escenario | Origen | Resultados admisibles | Resultados prohibidos | Formulaciones / condiciones |
|---|---|---|---|---|---|
| ESC-57 | Corte de red durante el envío de un lote | OFF-08 [REC], OFF-06 [DER] | Cada operación queda en un único estado coherente; al volver la red se reenvían las pendientes sin duplicar efectos | Estado parcial; operación aplicada dos veces (ES-25, ES-33) | D5-7; IN-05-09, IN-05-19; OC-43, OC-44 |
| ESC-58 | Reenvío de una operación rechazada después de que cambió el estado (p. ej. se restableció la asignación) | OFF-06 (alcance abierto); OFF-22 [DER] | **Sin elegir:** devolver el rechazo original, o reevaluar con el estado vigente | Aplicar sin revalidar (ES-26); un resultado distinto del original para lo ya aplicado (ES-31) | CAP-05-I-A o B; OC-35; K-3 |
| ESC-59 | Dos dispositivos del mismo editor con numeraciones solapadas o con huecos aparentes | §9.4 fila 10 [CONF]; OFF-25 | **Sin elegir:** contigüidad por partido o por dispositivo, con marcador o estado desactualizado en conflicto (§9.4 fila 2) | Sobrescritura (ES-28); aplicar saltando un hueco (ES-27) | CAP-05-S-A o B; OC-41 [DER] |
| ESC-60 | Falla una operación y existen posteriores del mismo partido independientes de ella | OFF-07 [REC]; OFF-25 [CONF] | **Sin elegir:** bloquearlas (OFF-07) o evaluarlas una por una salvo las dependientes | Aplicar una posterior que depende de una rechazada (ES-27) | CAP-05-B-A o B; OC-37 |
| ESC-61 | El registro de un rechazo exigido no puede persistirse durante la sincronización | IN-09-05 [DER]; CAB-14 | **Sin elegir:** operación `pendiente` y reintento, o intención durable y conciliación | Devolver un rechazo definitivo sin registro (F-1); pérdida de auditoría (ES-30) | CAP-05-F-A o B; OC-36; decisión 10 |
| ESC-62 | Sesión vencida con operaciones pendientes y otra cuenta inicia sesión en el dispositivo | IN-03-19, IN-03-20 [REC]; OFF-12; OFF-23; la continuidad de la captura es [DER] | La cola de la primera cuenta no es accesible ni se sincroniza con la sesión de la segunda; **[DER]** la captura local previamente autorizada de la primera cuenta no se altera por el inicio de sesión de la otra (inferencia del autor: OFF-23 solo dice que la captura continúa con la sesión vencida y OFF-12 exige confirmación al cerrar sesión con pendientes) | La cola de una cuenta sincronizada por otra (ES-35) | IN-05-11 [REC]; ADR-03.C; ADR-16.C |
| ESC-63 | Actualización de la app con cola pendiente y formato de cola incompatible | OFF-16 [REC] | Se bloquea la actualización hasta sincronizar | Cola descartada o corrompida (ES-34) | IN-05-16; ADR-16.C; ADR-05.I |
| ESC-64 | La «finalización pendiente» es rechazada o queda en conflicto | OFF-21 [CONF], §9.4 filas 3 y 8 | El partido conserva su estado oficial; la operación pasa a `en conflicto` o `rechazada` y no se publica | Partido tratado como finalizado o público (ES-36) | IN-05-14 |
| ESC-65 | El navegador elimina los datos locales con operaciones pendientes | OFF-17 [CONF]; AM-11 | Las pendientes se pierden **sin que se prometa conservación**; la advertencia de OFF-17 ya se mostró | Prometer conservación; perder una operación **confirmada** (lo confirmado está en el servidor) | ADR-16.C (almacenamiento y desalojo); RES-07; ADR-05.C solo exige la advertencia |

### 21.8 Amenazas y casos de abuso relevantes

**No se amplía el modelo de amenazas:** esta sección registra cómo las capacidades de ADR-05.C tratan las amenazas que R2-A asigna a R2-C y a otros artefactos. La **consolidación del modelo** corresponde a R2-C (subbloque posterior) y no se hace aquí.

| Amenaza o caso | Qué es | Cómo la tratan las capacidades exigidas | Queda para |
|---|---|---|---|
| **AM-04 / CAB-04** | Repetir o reordenar operaciones enviadas | IN-05-04 (idempotencia), IN-05-06 y IN-05-08 (orden, contigüidad y dependencia de la reanudación), IN-05-10 (revalidación); ESC-43, ESC-57 | D5-1, D5-4, D5-5 y D5-7 (formulaciones abiertas); R3 (mecanismo) |
| **AM-10 / CAB-16** | Editar la cola local o el reloj para alterar el orden o la hora | IN-05-15 (orden por secuencia, no por reloj; aviso si difiere > 5 min), IN-05-10 (revalidación), ES-37 | R3 (mecanismo); ADR-16.C para la protección del almacén |
| **AM-11 / CAB-08** | El navegador elimina los datos locales y se pierden capturas [NO VERIF] | IN-05-13 (advertencia, sin promesa de conservación), ESC-65 | **ADR-16.C** (almacenamiento y desalojo), RES-07, iOS sin verificar (HR1-07; IOS-01, IOS-04, IOS-05) |
| **AM-33 / CAB-30** | Sincronizar la cola de otra cuenta o apropiarse de operaciones ajenas | IN-05-11 [REC] (IN-03-19 e IN-03-20), ES-35, ESC-62 | ADR-03.C; decisión de adoptar o no lo [REC] |
| **AM-08** | Robo, pérdida o préstamo del dispositivo con cola y datos preparados | IN-05-11 e IN-05-13 solo en lo que toca a la cola | **ADR-16.C** (cifrado local y límites; HR1-03); revocación de la cuenta (OFF-12) |
| **AM-18 / CAB-05** | Carrera entre una revocación o reasignación y la sincronización | IN-05-10 (consume IF-33, IF-39, GT-22, ESC-45) | OC-04 y D-2 (decisión 1 de R2-A; DB2c-1 y DB2c-10) |
| **CAB-14** | Provocar un rechazo que no deje rastro o inundar el registro | D5-1, D5-3 e IN-05-18 (alternativas F y O) | Decisión 10 de R2-A; ADR-09.C |
| AM-19, AM-34 | Reasignación propuesta a R2-C y la Fase 4 (OC-40, DB2c-11) | **No se tratan aquí**; la reasignación no está decidida | DB2c-11 |

### 21.9 Matriz de trazabilidad

| Fuente | Cubierta por | Observación |
|---|---|---|
| OFF-01 | IN-05-01, IN-05-14; variantes de estado de §20.3.5 | Capturar sin conexión, incluidos iniciar, suspender, reanudar y finalizar |
| OFF-02 | IN-05-01 | Preparación de dispositivo: ADR-04.C (proyección de preparación) y ADR-16.C; ADR-05.C solo la consume |
| OFF-03 | IN-05-01 | Contenido mínimo de cada operación |
| OFF-04 | IN-05-02; D5-6 | Cinco estados diferenciados |
| OFF-05 | IN-05-03; IN-05-19 | Disparadores; no depende del segundo plano |
| OFF-06 | IN-05-04, IN-05-05; D5-1 | Lo aplicado [CONF]; lo rechazado [ABIERTO] |
| OFF-07 [REC] | IN-05-07; D5-5 | Bloqueo por operación anterior |
| OFF-08 [REC] | IN-05-09, IN-05-19; D5-7 | Atomicidad por operación |
| OFF-09 | IN-05-12; ES-34 | Política de conflictos |
| OFF-10 | IN-05-13; ES-34 | Nunca descarte silencioso; 30 días |
| OFF-11 | IN-05-10 (revalidación en el servidor); IN-05-21 (conducta del cliente); ESC-45, ESC-52 (§20.8) | Revocación, desasignación y cuenta desactivada; la ubicación definitiva de la conducta del cliente no está decidida |
| OFF-12 | IN-05-11 [REC], IN-05-13; ADR-16.C | Datos y sesión locales |
| OFF-13 [REC] | IN-05-15; ES-37 | Relojes |
| OFF-14 [REC] | — | Duplicados por la interfaz: UX (Fase 3); fuera de ADR-05.C |
| OFF-15 | IN-05-02, IN-05-14 | Lo local jamás es público |
| OFF-16 [REC] | IN-05-16; ESC-63 | Actualización con cola pendiente |
| OFF-17 | IN-05-13; ESC-65 | Almacenamiento local no garantizado; iOS sin verificar |
| OFF-18 | — | Consulta sin conexión: ADR-04.C y ADR-16.C |
| OFF-19 [REC] | — | Rendimiento de sincronización: no se fija parámetro; ADR-05.I y Fase 4 |
| OFF-20 | IN-05-18; D5-2 | Metadatos de rechazadas y en conflicto: persistencia [ABIERTA] |
| OFF-21 | IN-05-14; ESC-64 | Finalizar sin conexión |
| OFF-22 | IN-05-10; §21.6 | Autorización vigente al sincronizar |
| OFF-23 | IN-05-10; OC-39 | Vencimiento de sesión; nivel de autenticación [ABIERTO] |
| OFF-24 | IN-05-17 | Exportación limitada |
| OFF-25 | IN-05-06, IN-05-08; D5-4 | Secuencias con reanudación |
| §9.2, §9.4 | IN-05-02, IN-05-12; ESC-59, ESC-64 | Estados y política de conflictos |
| RD-RES-04, 05, 10, 13; RD-EVT-12 | IN-05-08, IN-05-12 | Versión, resultado único y dependencia de la reanudación |
| RT-13, RT-14 | IN-05-10, IN-05-18 | Autorización atómica y conflicto de interés |
| IN-09-04, 05, 06 | IN-05-18; D5-2, D5-3 | Consumidos de ADR-09.C |
| IN-03-10, 11, 19, 20 | IN-05-10, IN-05-11 | Consumidos de ADR-03.C |
| IF-32…41; ES-25…32; GT-19…24; ESC-43…56 | §21.6; IN-05-04…IN-05-10 | Consumidos de ADR-02.C / R2-B2 |

### 21.10 Dependencias, condiciones abiertas y decisiones

#### 21.10.1 Dependencias

| Elemento | Depende de lo aprobado | Alimenta (sin decidirlo) | Decisiones del responsable |
|---|---|---|---|
| IN-05-01…IN-05-03 (cola, estados, disparadores) | OFF-01…05; R1 §5.3 (CMP-04, CMP-05) | ADR-16.C (formato de cola y datos locales); ADR-05.I | — |
| IN-05-04…IN-05-09 (idempotencia, orden, atomicidad) | OFF-06…08, OFF-25; IF-34…38; GT-20, GT-21 | ADR-05.I; ADR-08 (aplicación a la ruta sincronizada) | D5-1, D5-4, D5-5, D5-7; OC-35, OC-37, OC-41 |
| IN-05-10, IN-05-11 (revalidación, cuenta) | OFF-22, OFF-23; IF-33, IF-39; IN-03-10, 11, 19, 20 | ADR-03.I; ADR-01 | OC-04, D-2 (decisión 1), OC-39 |
| IN-05-12…IN-05-17 y IN-05-21 (conflictos, conservación, relojes, versión, exportación, conducta del cliente) | OFF-09, OFF-10, OFF-13, OFF-16, OFF-21, OFF-24 | ADR-16.C; CMP-17 | D5-6; Q-43 |
| IN-05-18 (rechazos y metadatos) | IN-09-05; RF-AUD-06; OFF-20; §20.5.1 | ADR-09.C (consume la decisión); ADR-13 (alerta, R4) | D5-1, D5-2, D5-3; decisiones 3 y 10; K-3 |
| IN-05-19 (recuperación) | OFF-05, OFF-06, OFF-08 | ADR-05.I | D5-7; OC-43, OC-44 |

#### 21.10.2 Condiciones y decisiones heredadas (conservadas abiertas)

**Ninguna se resuelve ni se infiere.** Se listan porque condicionan a ADR-05.C.

| Elemento | Estado | Cómo afecta a ADR-05.C |
|---|---|---|
| Decisión 3 de R2-A / DB2c-3 | Abierta | Partición ADR-05.C / ADR-09.C de la persistencia de OFF-06 y OFF-20 (D5-1, D5-2) |
| Decisión 10 de R2-A / DB2c-2 / K-3 | Abiertas | Fallo del registro y relación con OFF-06; CAP-09-P-A/B/C, F-1…F-3 y O-1…O-3 sin elegir |
| OC-35 / OC-36 | Abiertas | D5-1 y D5-3 |
| OC-37 / DB2c-9 | Abierta | OFF-07 [REC] (D5-5) |
| OC-38 / DB2c-5 (Q-42 (a), A-10) | Abiertas | Quién captura sin conexión y quién reanuda sin conexión |
| OC-39 / DB2c-8 | Abierta | Nivel de autenticación al sincronizar (CAP-03-S-A o B, de ADR-03.C) |
| OC-41, OC-42 | Abiertas (dependencias de ADR-05.C) | D5-4 y D5-6 |
| OC-04, D-2 / DB2c-1, DB2c-10 / decisión 1 de R2-A | Abiertas; **D-2 sin excepción** | Instante que cuenta para una operación en tránsito; medición de la revocación en sesiones conectadas |
| OFF-06, OFF-07, OFF-20 | Alcance sobre lo rechazado abierto; OFF-07 [REC]; OFF-20 con persistencia [ABIERTA] | D5-1, D5-2, D5-5 |
| A-9, A-10, A-11, A-13, A-14, A-15; Q-29, Q-30, Q-42, Q-43 | Abiertas (parámetros que R1 §8.1 asigna a ADR-05) | Delimitan reanudar y finalizar, la preparación manual, eventos en `programado`, efectos indirectos y el contenido exportable |
| OC-30; OC-34 | Abiertas | Composición con UI-10; enumeración de las variantes de estado |
| DB1-1…6, DB2a-1…10, DB2b-1…9, DB2c-1…11, OC-01…42 y las 12 decisiones de R2-A | Abiertas | Conservadas; ninguna se toca |
| Verificación de iOS (HR1-07; IOS-01, IOS-04, IOS-05 [NO VERIF]); RES-07 | Sin verificar; sin aceptar | Lo que dependa del comportamiento de iOS se marca "no verificado en dispositivo" |

#### 21.10.3 Condiciones abiertas nuevas de este borrador

| ID | Condición abierta | Relación |
|---|---|---|
| OC-43 | Interacción entre lo que el cliente consulta tras una interrupción (D5-7) y lo que el servidor conserva de lo rechazado (D5-1): `01` no fija qué debe poder preguntar el cliente | D5-1, D5-7; OC-35 |
| OC-44 | Estado local `enviando` cuando no llega respuesta tras una interrupción prolongada: §9.2 define los cinco estados, no la transición de `enviando` cuando no hay respuesta | D5-6, D5-7; OC-42 |

#### 21.10.4 Decisiones propuestas al responsable (DC1-)

**Ninguna se resuelve aquí.** Las decisiones heredadas (DB1, DB2a, DB2b, DB2c, decisiones de R2-A) permanecen como estaban.

| ID | Decisión | Opciones | Efecto |
|---|---|---|---|
| DC1-1 | Alcance de la idempotencia sobre lo rechazado (D5-1; OC-35) | CAP-05-I-A / CAP-05-I-B / mantener abierta | Fija la lectura de OFF-06 para lo rechazado; decisión 10 |
| DC1-2 | Persistencia de R-4 (D5-2; decisión 3; DB2c-3) | CAP-05-M-A / CAP-05-M-B / mantener abierta | Quién decide la persistencia de OFF-20 |
| DC1-3 | Fallo del registro de un rechazo exigido (D5-3; OC-36) | CAP-05-F-A / CAP-05-F-B / mantener abierta | Conformidad con IN-09-05 |
| DC1-4 | Contigüidad entre dispositivos del mismo editor (D5-4; OC-41) | CAP-05-S-A / CAP-05-S-B / pedir una aclaración formal de `01` | Qué es un hueco |
| DC1-5 | Bloqueo tras una operación fallida (D5-5; OC-37; DB2c-9) | CAP-05-B-A / CAP-05-B-B / mantener abierta | Estado de OFF-07 [REC] |
| DC1-6 | Estado local tras un aborto técnico (D5-6; OC-42, OC-44) | CAP-05-E-A / CAP-05-E-B (puede requerir modificar `01`) / mantener abierta | Estados visibles al usuario |
| DC1-7 | Reconciliación tras una interrupción (D5-7; OC-43) | CAP-05-C-A / CAP-05-C-B / mantener abierta | Qué consulta el cliente |
| DC1-8 | Estado de las propuestas [REC] que ADR-05.C consume: OFF-07, OFF-08, OFF-13, OFF-16, IN-03-19 e IN-03-20 | Adoptar / descartar / decidirlo en una etapa posterior | Si IN-05-07, 09, 11, 15 y 16 pasan a exigencias |
| DC1-9 | Orientación sobre las tensiones T-1…T-8 de §21.2 | Recibir sin resolver / pedir aclaración de `01` o de R1 por edición separada | Alcance de ADR-05.C frente a ADR-09.C y ADR-16.C |
| DC1-10 | Alcance y orden de R2-C2 (ADR-04.C) y R2-C3 (ADR-16.C), que requieren autorización separada | Autorizar / posponer | ADR-16.C depende del formato de cola |

### 21.11 Riesgos y limitaciones

Los riesgos se **registran sin valorar ni aceptar**. Ninguno sustituye a RB1, RB2a, RB2b, RB2c ni RES-01…12.

| ID | Riesgo | Relación |
|---|---|---|
| RR5-01 | Elegir de hecho una formulación al describir ventajas y limitaciones | §21.5; DC1-1…DC1-7 |
| RR5-02 | Resolver K-3 o las decisiones 3 y 10 por inferencia (p. ej. tratar R-4 como persistente) | IN-05-18; D5-1, D5-2 |
| RR5-03 | Tratar como confirmadas las propuestas [REC] (OFF-07, 08, 13, 16; IN-03-19, 20) | DC1-8 |
| RR5-04 | Invadir ADR-05.I y R3 al describir cola, secuencia o reconciliación | X-4; §21.3 |
| RR5-05 | Contradecir OFF-25 u OFF-04 con CAP-05-S-B o CAP-05-E-B | D5-4, D5-6 |
| RR5-06 | Denegación inducida o carga por reintentos (CAP-05-F-A, CAP-05-I-B, CAP-05-C-A) | RB2c-06; ESC-61 |
| RR5-07 | Condicionar OFF-05 y OFF-17 a un comportamiento de iOS no verificado | HR1-07; IOS-01, IOS-04, IOS-05; RES-07 |
| RR5-08 | Incoherencia con partes aprobadas (§18–§20), p. ej. la ficha UI-03 y OC-08 | T-5; §21.2 |
| RR5-09 | Presentar este borrador como aprobación de ADR-05.C, de R2-C o de R1 §8.1 | §21.0; X-7 |
| RR5-10 | Pérdida de capturas por desalojo del almacenamiento local, fuera del alcance de ADR-05.C | AM-11; ESC-65; ADR-16.C |

**Limitaciones del análisis.** (1) Es un borrador del autor, **sin revisión de contexto nuevo ni independiente**. (2) Se leyeron las secciones pertinentes de las fuentes, no todas línea a línea. (3) No se realizó ninguna prueba, medición ni verificación en dispositivo (iOS). (4) Las lecturas marcadas [DER] son del autor y deben revisarse. (5) Las ventajas, limitaciones y riesgos de las formulaciones son un análisis documental, no una evaluación empírica.

### 21.12 Criterios de aceptación del borrador y preguntas para el responsable

| ID | Criterio de aceptación | Cumplimiento según el autor (a revisar) |
|---|---|---|
| CR2C1-01 | Alcance, fuentes y dependencias identificados | Cumple: §21.0–§21.2 |
| CR2C1-02 | Al menos dos formulaciones diferenciadas por decisión, con ventajas, limitaciones, riesgos y dependencias | Cumple: §21.5 (D5-1…D5-7) |
| CR2C1-03 | Ninguna formulación elegida ni aprobada | Cumple: §21.0 (X-1), §21.5 |
| CR2C1-04 | Invariantes y garantías propuestas, con tipo y fuente | Cumple: §21.4 |
| CR2C1-05 | Relación con la frontera atómica de ADR-02.C y con UI-06 con UI-10 dentro, sin segunda unidad | Cumple: §21.6 |
| CR2C1-06 | Estados prohibidos, escenarios y amenazas relevantes | Cumple: §21.7 y §21.8 |
| CR2C1-07 | Matriz de trazabilidad con OFF-01…25 | Cumple: §21.9 |
| CR2C1-08 | Decisiones 3 y 10, DB2c-2, DB2c-3, K-3, OC-35…OC-42 y D-2 conservadas abiertas | Cumple: §21.10.2 |
| CR2C1-09 | Conflictos entre fuentes señalados sin resolver | Cumple: §21.2 (T-1…T-8) |
| CR2C1-10 | Sin mecanismos, tecnologías ni parámetros no aprobados | Cumple según §21.13 |
| CR2C1-11 | Solo se modifica el archivo autorizado y los bloques previos permanecen intactos | Cumple según §21.13 |
| CR2C1-12 | Borrador pendiente de revisión del responsable; no aprobado, registrado ni versionado | Cumple: §21.0 y §21.14 |

**Preguntas concretas para el responsable** (no se responden por inferencia): (1) ¿Qué formulación, si alguna, desea evaluar con más detalle en cada D5-n (DC1-1…DC1-7)? (2) ¿Desea resolver antes las decisiones 3 y 10 de R2-A, o que ADR-05.C las conserve abiertas? (3) ¿Se adoptan o se descartan las propuestas [REC] que este borrador consume (DC1-8)? (4) ¿Desea pedir una aclaración de `01` sobre la contigüidad entre dispositivos (OFF-25, §9.4 fila 10) y sobre el estado local tras un fallo técnico (OFF-04, §9.2)? (5) ¿Autoriza R2-C2 (ADR-04.C) y R2-C3 (ADR-16.C), cuyo orden depende de este borrador (DC1-10)?

### 21.13 Verificación del autor

**Naturaleza.** Verificaciones **mecánicas de lectura** hechas por el autor con un guion de solo lectura. **No son una auditoría independiente ni una revisión de contexto nuevo:** el autor no puede verificar su propio trabajo con independencia. **Este borrador no ha sido auditado ni revisado por nadie más.**

| # | Verificación | Resultado |
|---|---|---|
| V-1 | Las líneas anteriores a este apartado (todo el archivo del commit `a13b213`) son **idénticas**; el cambio son solo líneas añadidas al final; ninguna eliminada | Cumple (comparación con `git show HEAD:`) |
| V-2 | Solo cambió `02-arquitectura-adr.md`; `00`, `01`, `aprobaciones.md` y `bitacora.md` no cambiaron | Cumple (`git status` y sumas SHA-256) |
| V-3 | Identificadores definidos y sin referencias huérfanas: IN-05-01…21, ES-33…37, ESC-57…65, OC-43…44, DC1-1…10, RR5-01…10, CR2C1-01…12, D5-1…7 y las catorce formulaciones CAP-05-I, M, F, S, B, E y C (A y B) | Cumple (guion de lectura) |
| V-4 | Las citas literales entre «» se buscaron en `00`, `01` y las partes previas | Todas existen, **salvo** los mensajes del responsable citados en §21.0, que proceden de la conversación y no de un archivo |
| V-5 | Exploración de términos tecnológicos (nombres de bases de datos, productos, proveedores, librerías, algoritmos, mecanismos de sincronización o de almacenamiento y similares) | Solo aparecen en X-4 y en esta fila como **exclusión**; ninguna elección ni mecanismo |
| V-6 | Parámetros numéricos: solo los que cita `01` (30 días, 5 minutos, 7 días, 24 horas) como referencias; ninguno nuevo | Cumple (lectura del autor) |
| V-7 | Ninguna decisión abierta aparece resuelta: no se elige ninguna formulación; K-3, las decisiones 3 y 10 de R2-A, DB2c-2, DB2c-3, OC-35, OC-36, OC-37, OC-38, OC-39, OC-41, OC-42, OC-04 y D-2 permanecen abiertas; no se declara cumplido R1 §8.1; no se concede excepción | Cumple (lectura del autor) |
| V-8 | Las tablas están alineadas (mismo número de columnas por fila) | Cumple (guion de lectura) |
| V-9 | **Coherencia con la frontera:** UI-06 con UI-10 dentro se describe sin segunda unidad ni segunda ruta (§21.6); los registros de rechazo respetan §20.5.1 (R-3 solo persistencia; R-4 [ABIERTA]; R-6 [REC]) | Cumple (comparación con §20.3 y §20.5) |
| V-10 | Cada decisión D5-n presenta **dos** formulaciones con ventajas, limitaciones, riesgos y dependencias | Cumple (§21.5) |
| V-11 | §21.6 reproduce los pasos a–j de §20.3.1 con su clasificación literal "dentro o fuera de la unidad"; no cambia la frontera | Cumple (comparación con §20.3.1) |

**Límites.** Las lecturas [DER] y la clasificación de ventajas, limitaciones y riesgos son del autor y deben revisarse. Las ocho tensiones de §21.2 son una identificación del autor.

### 21.14 Estado de esta parte y lo que no se hizo

**Estado: v0.2-R2C1, APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-21, inferida).** Corrigió los hallazgos H-01…H-17 (§21.15). Aprobación registrada en §21.0, `aprobaciones.md` y `bitacora.md`; **no versionado**.

**Qué se hizo.** Se redactó la Parte VII (§21) como borrador de ADR-05.C: alcance, fuentes y dependencias (§21.0–§21.3), invariantes y garantías exigidas (§21.4), siete decisiones con dos formulaciones cada una (§21.5), relación con la frontera atómica de ADR-02.C (§21.6), estados prohibidos y escenarios (§21.7), amenazas relevantes (§21.8), matriz de trazabilidad (§21.9), dependencias, condiciones y decisiones abiertas (§21.10), riesgos (§21.11) y criterios de aceptación y preguntas (§21.12).

**Qué no se hizo.** No se redactó ADR-04.C ni ADR-16.C (R2-C2 y R2-C3), ni la consolidación del modelo de amenazas, ni R3 ni R4; no se aprobó ADR-05.C, R2-C, R2 ni la Fase 2; no se eligió ninguna formulación; no se resolvió ninguna decisión ni condición abierta (K-3, decisiones 3 y 10 de R2-A, DB2c-2, DB2c-3, OC-35…OC-42, OC-04, D-2); no se eligió tecnología, mecanismo ni parámetro; no se aceptó ningún riesgo ni se concedió excepción; no se declaró cumplido R1 §8.1; no se modificaron otros archivos; no se actualizó la trazabilidad histórica de `a13b213`; no se crearon ramas, commits, pushes, PR ni merges; no se tocó `main`.

**Git.** Rama `claude/fase-2-arquitectura-amenazas`; HEAD `a13b2139042ccb8bfac8f88121a3bb26dc642ce5`; los cambios son `02-arquitectura-adr.md`, `aprobaciones.md` y `bitacora.md` **sin confirmar** (registro de la aprobación). Cualquier commit, push o PR requiere autorización expresa y separada.

### 21.15 Hallazgos de la autoevaluación de la v0.1 (H-01…H-17) y correcciones de la v0.2

**Origen.** Autoevaluación crítica del autor sobre la v0.1 (SHA-256 `f31182b706fea05ef7d10b06449e56f8d9cef71194f8881ba1efe41ae9c1db1a`): **no es una auditoría independiente ni una revisión de contexto nuevo**. Las correcciones fueron autorizadas en una ronda única (§21.0). **No equivalen a aprobación.**

| ID | Corrección en la v0.2 | Dónde |
|---|---|---|
| H-01 | §21.6 reproduce los pasos a–j de §20.3.1 con su clasificación literal y añade solo lo que aporta ADR-05.C | §21.6 |
| H-02 | IN-05-12 acotada frente a la eliminación del almacenamiento por el navegador; nueva tensión T-8 | §21.4, §21.2 |
| H-03 | Registro de R-2 [CONF] separado de su persistencia independiente [DER] | IN-05-18, §21.6 |
| H-04 | Declaración de fuentes leídas corregida, con las relecturas hechas y las limitaciones | §21.0 |
| H-05 | OFF-11 completada con la conducta del cliente (IN-05-21), sin trasladarla ni crear requisitos | §21.4, §21.9 |
| H-06 | Referencia a ES-31 eliminada de CAP-05-I-B | D5-1 |
| H-07 | Sesgo neutralizado en CAP-05-I-A/I-B y CAP-05-S-A, con las referencias documentales | D5-1, D5-4 |
| H-08 | OFF-03 incorporada a D5-4 y T-3 | D5-4, §21.2 |
| H-09 | Posible no conformidad de CAP-05-M-B con OFF-20 señalada; asociación con CAP-09-P-B marcada [DER] | D5-2 |
| H-10 | Partición ADR-05.C / ADR-09.C identificada como provisional (decisión 3 abierta) | T-1, IN-05-18 |
| H-11 | [CONF] y [DER] de IN-05-10 corregidos | §21.4 |
| H-12 | Transición `enviando` → `pendiente` declarada en CAP-05-E-A (OC-44) | D5-6 |
| H-13 | Posible responsabilidad nueva de CMP-13 en CAP-05-C-B dejada abierta | D5-7 |
| H-14 | CAP-05-F-A acotada a los rechazos cuyo registro exige R-2 | D5-3 |
| H-15 | Inferencia de ESC-62 marcada [DER] | §21.7 |
| H-16 | ES-34 con [CONF], [DER] y [REC] separados | §21.7 |
| H-17 | Encabezado de §21.4 corregido: [REC] y [ABIERTO] no se presentan como exigidos | §21.4 |


---

# PARTE VIII — BLOQUE R2-C2 (v0.2-R2C2, APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL, 2026-11-22 inferida)

## 22. R2-C2 — ADR-04.C: visibilidad, proyecciones públicas y coherencia pública (capacidades exigidas)

### 22.0 Alcance, autorización y reglas de esta parte

**Estado: v0.2-R2C2 — APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-22, fecha inferida), por acto expreso del responsable (ver «Aprobación condicional» más abajo).** La v0.2 corrigió la v0.1 tras la revisión focalizada F-01…F-06 (§22.15). **No está versionado.** Esta parte **se añade al final del archivo, después de la Parte VII (R2-C1)**; no modifica ninguna línea de R1 (§0–§15), R2-A (§16), R2-B1 (§17), R2-B2 (§18–§20), R2-C1 (§21) ni de los registros de metadatos. **No aprueba ADR-04.C, R2-C, R2 ni la Fase 2.**

**Autorización recibida.** Mensaje «TORNEO DE CAMPEONES — R2-C2: BORRADOR DE ADR-04.C» (2026-11-21, fecha inferida: la del aviso de fecha de la sesión; no hay marca de tiempo individual del mensaje): «AUTORIZO EXCLUSIVAMENTE INICIAR R2-C2 Y REDACTAR UN BORRADOR DOCUMENTAL DE ADR-04.C.» Archivo autorizado: solo `02-arquitectura-adr.md`. Sin commit, push, PR ni merge. **No se actualiza la trazabilidad histórica del commit `a468166`.**

**Autorización de la corrección (v0.2).** Mensaje «TORNEO DE CAMPEONES — R2-C2: CORRECCIÓN FOCALIZADA DE ADR-04.C» (2026-11-22, fecha inferida: la del aviso de fecha de la sesión): «AUTORIZO EXCLUSIVAMENTE CORREGIR LOS HALLAZGOS F-01…F-06 Y LAS TRES OBSERVACIONES EDITORIALES DE LA REVISIÓN FOCALIZADA DE ADR-04.C.» Limitada a la Parte VIII (§22); sin commit, push, PR ni merge. La revisión fue una **autoevaluación del autor, no una auditoría independiente**.

**Aprobación condicional de R2-C2 como entregable documental (2026-11-22).** Por acto expreso del responsable (mensaje «TORNEO DE CAMPEONES — REGISTRO DE APROBACIÓN DOCUMENTAL CONDICIONADA DE R2-C2»), la v0.2-R2C2 quedó **APROBADA CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL**: «APRUEBO EXCLUSIVAMENTE R2-C2 v0.2 (ADR-04.C) COMO ENTREGABLE DOCUMENTAL CONDICIONADO.» La fecha es la del aviso de fecha de la sesión; no hay marca de tiempo individual del mensaje.

- **La aprobación NO implica:** seleccionar ninguna de las 16 formulaciones; validar alternativas no conformes; aprobar definitivamente ADR-04.C; aprobar globalmente R2-C, R2 ni la Fase 2; acreditar implementación ni pruebas; resolver Q-20, PRV-18, Q-40 (d), A-8, K-3 ni las decisiones de R2-A; fijar parámetros, aceptar riesgos ni conceder excepciones; autorizar R2-C3 ni R2-C4.
- **Condiciones que permanecen abiertas:** (1) D4-1…8 sin elegir formulaciones; (2) CAP-04-C-B no conforme con R1 §5.5 salvo aclaración formal de OC-46; (3) CAP-04-M-B condicionada por RF-PUB-02, Q-20 y PRV-18; (4) CAP-04-X-B limitada a copias futuras, sin garantía sobre copias anteriores; (5) CAP-04-G-B sin adoptar la ampliación de RT-14; (6) Q-20: mientras permanezca abierta, ninguna formulación publica, prepara ni copia nombres personales de jugadores, menores o adultos; (7) A-8 y la circularidad T-8 sin resolver; (8) OC-45…47, DC2-1…11 y las demás decisiones abiertas; (9) RES-02 y RES-08 sin aceptar; (10) sin parámetros fijados, riesgos aceptados ni excepciones concedidas; (11) la limitación de revisión indicada abajo.
- **Limitación de la revisión.** El borrador solo recibió una **revisión focalizada del autor** (F-01…F-06 y tres observaciones editoriales, corregidas en la v0.2). **No hubo auditoría independiente ni revisión de contexto nuevo.** La aprobación es exclusivamente documental: **no acredita implementación ni pruebas empíricas** y no sustituye la asesoría jurídica (PEND-JUR).
- **Integridad.** SHA-256 del **contenido técnico aprobado** (el archivo inmediatamente antes de añadir estos metadatos): `2a6b03cec0c6ad9d8fff43ec3016f31374f624013ef05472d3d6cb816acdcbea`. El hash del **archivo con los metadatos de aprobación** se registra en `aprobaciones.md` y `bitacora.md`; **no se inserta en este archivo**. Los metadatos de aprobación son solo editoriales: el título y el estado de la Parte VIII, este apartado, la fila de estado de §22.1, el estado y el párrafo «Git» de §22.14 y una nota posterior de trazabilidad del commit `a468166` en la Parte VII. **El contenido arquitectónico y las formulaciones de §22.1–§22.13 y §22.15 no se modificaron.**
- **Precedencia.** CR2C2-12 de §22.12 y el apartado «Qué no se hizo» de §22.14 describen el estado a la fecha del borrador; prevalece este apartado en cuanto al estado de aprobación.
- **Estado de versionado.** **Pendiente:** sin commit, push, PR ni merge.
- **Condición de cierre de la Fase 2.** Las decisiones que la Fase 1 asignó a la Fase 2 deberán resolverse antes de cerrar globalmente la Fase 2, salvo modificación o excepción formalmente aprobada. R2-C3 y R2-C4 no están iniciados; R2-C no tiene aprobación global.

**Qué contiene.** Las capacidades y garantías **exigidas** de la visibilidad y de las proyecciones: garantía de coherencia de la información pública, visibilidad y etiquetas de menores, proyección de preparación y estados de publicación, consistencia entre lo privado y lo público, dependencia de ADR-02.C y sus fronteras atómicas, casos límite, amenazas y decisiones pendientes; y **ocho decisiones con dos formulaciones cada una, sin elegir ninguna**.

**Qué NO contiene ni hace.**

| # | Exclusión |
|---|---|
| X-1 | No elige entre las formulaciones CAP-04-P, C, V, M, R, X, A ni G (A o B): las presenta con sus ventajas, limitaciones, riesgos y dependencias |
| X-2 | No resuelve Q-20, PRV-18, Q-40 (d), A-8, DB2b-8, DB2b-3, OC-20, OC-26, OC-27, OC-28 ni OC-29, ni las decisiones de privacidad y autorización de menores, ni ninguna otra decisión abierta; no concede excepción a D-2 |
| X-3 | No presenta como garantía aprobada ninguna afirmación que dependa de una decisión abierta; **la garantía de coherencia pública sigue sin demostrarse** y su mecanismo y plazo son de ADR-06 |
| X-4 | No elige base de datos, tecnología de almacenamiento, caché, expiración, invalidación, regeneración, tiempo real, red de entrega, librería, servicio, cifrado ni parámetro numérico no aprobado; todo eso es de ADR-04.I, ADR-06, ADR-07 y R3 (condicionado a ADR-01) |
| X-5 | No redacta ADR-16.C ni R2-C4 (la consolidación del modelo de amenazas), ni R3 ni R4; no trata AM-19 ni AM-34, cuya reasignación está propuesta y abierta (OC-40, DB2c-11) |
| X-6 | No modifica ADR-02.C, ADR-03.C, ADR-05.C, ADR-09.C ni ninguna ficha, invariante, estado prohibido, garantía o escenario aprobados; los **consume** |
| X-7 | No acepta riesgos (RES-02 y RES-08 siguen sin aceptar), no fija PAR, no concede excepciones y no declara cumplido R1 §8.1 |
| X-8 | No modifica `00`, `01`, `aprobaciones.md` ni `bitacora.md`; **no garantiza el cumplimiento legal ni sustituye la asesoría jurídica** (PEND-JUR) |

**Lectura previa directa y límites de la lectura.** **Leídos directamente para este borrador:** en `01`, §9.6 (matriz de visibilidad), RF-PUB-01…09, RF-PWA-02, PRV-01…18, las preguntas Q-05, Q-11, Q-20 y Q-40, A-8, CL-15, CL-25, CL-26 y CL-30, RD-CAL-08, RD-RES-07, RD-TAB-11 y RK-16 (en parte); en R1, §5.3 (CMP-09, CMP-14, CMP-15, CMP-17), §5.5 (coherencia pública), §5.6 (clasificación de datos por zona), PA-04 y PA-08, FL-4 (§7.4) y la fila de ADR-04, ADR-06, ADR-07 y ADR-08 de §8.1 y §8.2; en R2-A, AM-12, AM-13, AM-27, AM-28 y AM-31, RES-02 y RES-08, CAB citados y HR1-11 y HR1-12 de R1; en R2-B2, IF-20…24, IF-31, ES-14…24, GT-13, OC-20, OC-26…29, DB2b-8 y CAND-14…19; y en R2-C1, §21 (en particular IN-05-01, IN-05-14 y OFF-02). **Limitaciones:** no se releyeron íntegros PRV-02 (solo su estado), RD-JUG-01, RF-TOR-02, RD-CAL-06/07, la matriz §7.1 ni R1 §5.7; las citas a esas fuentes se apoyan en lo que dicen R1, R2-A y R2-B2 y se tienen por **no comprobadas de nuevo**. Del skill se consultaron SKILL.md y las puertas de aprobación.

**Convención.** [CONF]: texto confirmado de `01` o R1. [REC]: recomendación o propuesta (no confirmada). [DER]: derivada por el autor (se explica el paso). [ABIERTO]: sin resolver; ninguna se elige. [COND]: condicionado a una decisión abierta. Identificadores nuevos (sin colisión con las series existentes): **IN-04-nn** (invariantes y garantías de ADR-04.C), **ES-38…** (estados prohibidos), **ESC-66…** (escenarios), **CAP-04-x-A/B** (formulaciones), **D4-n** (decisiones de formulación), **OC-45…**, **DC2-n** (decisiones para el responsable), **RR4-nn** (riesgos documentales) y **CR2C2-nn** (criterios de aceptación).

### 22.1 ADR-04.C — Identificación

| Campo | Valor |
|---|---|
| ADR | ADR-04.C — Visibilidad y proyecciones: capacidades exigidas (R1 §8.1, ADR-04; R1 §8.2) |
| Pregunta de R1 §8.1 | «¿Cómo se representa la visibilidad (`01 §9.6`) y se calculan las etiquetas de menores y la proyección de preparación?»; **ADR-04.C fija la garantía exigida de coherencia pública** (CL-15, sin estado intermedio público); si la clasificación se materializa o se calcula al servirla es parte de su decisión, y el mecanismo lo decide ADR-06 |
| Alternativas exigidas | **≥ 2** (R1 §8.1, ADR-04) |
| Parámetros abiertos asignados por R1 §8.1 | Q-20, Q-40 (d), A-8, PRV-18 |
| Requiere (R1 §8.2) | ADR-02.C (alcance de A-8 y frontera atómica) |
| Alimenta | ADR-06, ADR-07 y ADR-08 (R3), ADR-16.C (copias y datos preparados en el dispositivo), ADR-04.I |
| Estado | v0.2-R2C2: APROBADO CONDICIONALMENTE COMO ENTREGABLE DOCUMENTAL (2026-11-22, inferida); ADR-04.C **no** queda aprobada definitivamente |

### 22.2 Dependencias y conflictos entre fuentes

**Qué consume ADR-04.C de lo ya aprobado (como entrada, sin modificarlo):**

| Fuente | Qué aporta a ADR-04.C | Estado |
|---|---|---|
| ADR-02.C / R2-B2 (§18–§20) | Frontera atómica de UI-05, UI-10 y UI-07 (IF-20, IF-22, ES-14, ES-20; GT-13 exige la coherencia pública **a ADR-04.C**); UI-09 (generación y regeneración); CAND-14…19; OC-20, OC-28, OC-29 | Aprobado condicionalmente como entregable documental; PROPUESTA |
| ADR-05.C / R2-C1 (§21) | IN-05-01 (cola), IN-05-14 (lo local jamás es público), consumo de la proyección de preparación (OFF-02) y de las copias locales (OFF-18) | Aprobado condicionalmente; PROPUESTA |
| ADR-09.C / R2-A | IN-09-07 (la auditoría no contiene datos personales de menores más allá de identificadores internos) | Aprobado condicionalmente; PROPUESTA |
| R1 §5.3, §5.6, §7.4 | Responsabilidades de CMP-09 (habilitar y retirar visibilidad), CMP-14 (aplicar el modelo y proyectar), CMP-15 (servir), CMP-17 (políticas de privacidad); zonas de datos; propagación (FL-4) | Aprobado (R1) |
| `01` §9.6, PRV-02…06, 09, 11, 18, RF-PUB-01…09, CL-15, CL-25, CL-26, CL-30 | Matriz de visibilidad; reglas de menores; requisitos públicos | [CONF] salvo lo marcado [REC], [DER] o [COND] |

**Conflictos o tensiones entre fuentes (se señalan, no se resuelven):**

| # | Tensión | Fuentes | Tratamiento en este borrador |
|---|---|---|---|
| T-1 | RF-PUB-09 pide el resultado confirmado en vistas públicas en ≤ 60 s, pero es [REC] y no cubre cambios de visibilidad ni de privacidad; R1 FL-4 dice que para estos **no existe una garantía de plazo** y que el plazo es de ADR-06 | `01` RF-PUB-09, CL-30; R1 §7.4 | IN-04-09; D4-6 |
| T-2 | El estado de PRV-02 se lee de tres maneras: `01` lo rotula «CONF (Q-05 «información deportiva necesaria»)» para los menores y [COND] Q-20 / PEND-JUR para los adultos; la fila de Q-20 dice que «PRV-02/04 no se aprueban»; y R1 §5.6 dice que PRV-02 es [COND] y que «no se publica ningún dato personal» mientras Q-20 esté abierta. **No se resuelve Q-20**; ADR-04.C aplica el cierre más conservador (solo etiquetas) | `01` PRV-02, Q-20; R1 §5.6 | IN-04-03; D4-4 |
| T-3 | RF-PUB-02 [CONF] publica goleadores, tarjetas y plantillas; PRV-18 [CONF] exige evaluar la identificación indirecta con asesoría jurídica antes del lanzamiento y su resultado es [PEND-JUR] | `01` RF-PUB-02, PRV-18 | D4-4; OC-45 |
| T-4 | RF-PUB-06 y RF-PWA-02 exigen vistas disponibles sin conexión; PRV-06 y CL-26 exigen que una revocación vuelva a etiqueta y HR1-11 reconoce que las copias locales no se pueden purgar a distancia | `01` RF-PUB-06, PRV-06; R1 HR1-11 | D4-6; RES-02 |
| T-5 | RF-PUB-01 dice que un torneo `archivado` no aparece públicamente «por ahora»; Q-40 (d) deja abierta su visibilidad | `01` RF-PUB-01, Q-40 | D4-7 |
| T-6 | A-8 tiene una lectura provisional («No restringida por RT-14 en esta versión») y R1 §5.3 dice que CMP-09 aplica una guardia **opcional** según A-8 | `01` A-8, RD-CAL-08; R1 §5.3 | D4-8 |
| T-7 | R1 y `01` dicen que ADR-04.C «fija la garantía exigida» y que ADR-06 decide el mecanismo; R1 §5.5 declara además la garantía como "exigida (no demostrada)" | R1 §5.5, §8.1; R2-B2 GT-13 | IN-04-06: se conserva «no demostrada» |
| T-8 | R1 §8.2 dice que ADR-04.C requiere ADR-02.C «(alcance de A-8 y frontera atómica)»; R2-B1 §17.6 (fila de A-8) y su reparto dejan a ADR-04.C el punto opcional de guardia y la lectura de A-8 al responsable (DB1-3; Fase 4). La dependencia queda **circular** entre R1 §8.2 y R2-B1 | R1 §8.2; R2-B1 §17.6, DB1-3 | D4-8: se **registra** la circularidad; **no se resuelve** y no se atribuye la decisión de A-8 a ADR-02.C |

### 22.3 Qué decide ADR-04.C y qué no

| Asunto | ¿Lo decide ADR-04.C? | Quién |
|---|---|---|
| Garantía exigida de coherencia pública | **Sí** (como capacidad exigida; ya anticipada por R1 §5.5) | ADR-04.C |
| Si la clasificación se materializa o se calcula al servir | **Propone alternativas** (D4-1); decide el responsable | OC-20; DC2-1 |
| Representación de la visibilidad y de las etiquetas de menores | **Propone alternativas** (D4-3, D4-4) | ADR-04.C |
| Campos públicos de jugador y edad de corte (Q-20) | **No** | Responsable; PEND-JUR |
| Evaluación de la identificación indirecta (PRV-18) | **No** | Asesoría jurídica; RES-08 |
| Visibilidad de `archivado` (Q-40 (d)) | **Propone alternativas** (D4-7); decide el responsable | Responsable; antes de la Fase 3 |
| Alcance de A-8 frente a RT-14 | **Propone alternativas** (D4-8); decide el responsable | Responsable (DB1-3; Fase 4); R2-B1 §17.6 |
| Frontera atómica de UI-05, UI-07, UI-09, UI-10 | **No** | ADR-02.C |
| Mecanismo de actualización, expiración, caché, señal de cambio y plazo | **No** | ADR-06, ADR-07 (R3) |
| Cálculo de la clasificación | **No** | ADR-08 (R3) |
| Protección del almacén local y de las copias | **No** | ADR-16.C (R2-C3) |
| Implementación de proyecciones | **No** | ADR-04.I (R3, tras ADR-01) |

### 22.4 Invariantes y garantías exigidas de ADR-04.C (IN-04-nn)

Son **capacidades exigidas** cuando su tipo es [CONF] o [DER]; las [REC] son **propuestas condicionadas** a una decisión del responsable (DC2-8), las [COND] valen solo si se cumple la condición y las [ABIERTO] **no fijan** garantía alguna. **Ninguna está implementada ni probada.** Se suman a IF-01…41, ES-01…37 y GT-01…24, e IN-05-01…21, y no los modifican.

| ID | Invariante o garantía exigida | Tipo | Fuente | Condicionada por |
|---|---|---|---|---|
| IN-04-01 | **Lo público es una proyección con lista blanca de campos:** el público lee vistas derivadas, nunca las entidades internas; sin inicio de sesión; sin identidad de editores, correos ni campos internos | [CONF] | PA-04, RF-PUB-03, PRV-03 | D4-3, D4-4 |
| IN-04-02 | **Nunca públicos** (en ninguna vista ni respuesta pública): fecha de nacimiento o edad exacta, documento de identidad, teléfono, correo, dirección, fotografías, datos y evidencia de consentimiento, identidad del editor asignado, correos de usuarios y el nombre completo interno de un jugador sin autorización | [CONF] | PRV-03, RF-PUB-03 | — |
| IN-04-03 | **Menores y nombre público:** un menor aparece como «Jugador n.º {dorsal} ({equipo})» mientras no conste una autorización de publicación vigente; con autorización, con su nombre público; las estadísticas cuentan igual. La etiqueta y su cálculo se hacen **en el servidor**. **Mientras Q-20 permanezca abierta, ninguna formulación de este borrador permite publicar, preparar ni copiar nombres personales de jugadores, menores o adultos: solo etiquetas** (R1 §5.6: «no se publica ningún dato personal»). Las reglas de PRV-02 son de dos tipos: la de **menores** ([CONF] en Q-05) y la de **adultos** ([COND] Q-20 / PEND-JUR; `01` propone la misma regla conservadora por defecto) | [CONF] en la regla de menores de PRV-02 y en el cálculo en servidor; [COND] en la regla de adultos y en toda publicación de nombre (Q-20, PEND-JUR) | PRV-02, PRV-04, PA-04, R1 §5.6 | Q-20, PRV-18; D4-4 |
| IN-04-04 | **Visibilidad:** un resultado es público solo si el torneo está `publicado`, `en curso` o `finalizado`, la jornada está `publicada` y la operación está `confirmada`; las doce combinaciones que RF-PUB-04 prueba sobre la matriz de §9.6 se cumplen; un borrador o una jornada no publicada no se enumera ni se accede por URL (404 indistinguible de inexistente) | [CONF] en §9.6 (las tres condiciones) y RF-PUB-01; [DER] en RF-PUB-04, **fuente de las doce combinaciones** | `01` §9.6, RF-PUB-01, RF-PUB-04, CL-25 | D4-3, D4-7 |
| IN-04-05 | **Lo local jamás es público:** solo la operación `confirmada` de un partido visible puede ser pública; el registro de incidencias, la exportación limitada y la auditoría **no** autorizan publicar | [CONF] | OFF-15, OFF-20, OFF-24, IF-23, IN-05-14 | — |
| IN-04-06 | **Coherencia pública (garantía exigida, no demostrada):** las vistas públicas relacionadas con un mismo torneo mantienen **coherencia entre resultados y clasificaciones**; no se publica un resultado confirmado junto con una clasificación que no lo refleje de forma persistente (CL-15, sin estado intermedio público). **ADR-04.C la fija; ADR-06 decide el mecanismo; este borrador no afirma que se cumpla.** CAP-04-C-B (D4-2) **no relaja** esta garantía: se presenta como alternativa **no conforme con este texto**, salvo aclaración formal de OC-46 | [CONF] como garantía exigida; [ABIERTO] su demostración y mecanismo | R1 §5.5, §7.4; CL-15; RD-TAB-11; ES-14; GT-13 | D4-1, D4-2; ADR-06 |
| IN-04-07 | **Entrada única:** la coherencia pública parte de **un estado confirmado único** por partido y de una **única fuente de verdad** de la clasificación (CMP-12 con definición única); ADR-04.C no redefine la frontera atómica | [CONF] | IF-20, IF-21, IF-22, GT-13, RD-TAB-10, ADR-02.C | OC-20 |
| IN-04-08 | **Proyecciones derivadas persistentes, si existen,** quedan dentro de la misma unidad transaccional que su fuente; **que existan o no lo decide ADR-04.C** | [CONF] en lo transaccional; [ABIERTO] que existan | IF-22, ES-14, R1 §5.5 | D4-1; OC-20 |
| IN-04-09 | **Cambios de visibilidad y de privacidad:** despublicar, ocultar un nombre, revocar una autorización de nombre público y anonimizar deben registrar también una **señal de cambio** hacia las proyecciones públicas y de preparación **si ADR-06 la adopta**; **no existe una garantía definida de plazo** para estos cambios (RF-PUB-09 cubre resultados confirmados y es [REC]) y no se promete uno | [CONF] en la ausencia de plazo; [COND] en la señal (ADR-06) | R1 §7.4; RF-PUB-09, CL-30; HR1-11; OC-29 | D4-6; ADR-06 |
| IN-04-10 | **Ocultar y anonimizar son acciones distintas:** ocultar vuelve a etiqueta y es reversible; anonimizar borra irreversiblemente nombre y datos personales y conserva identificador interno, dorsal, estadísticas y resultados; tras anonimizar, las tablas y goleadores no cambian y ninguna proyección derivada persistente conserva datos personales | [CONF] | PRV-06, CL-26, ES-20 | OC-26, OC-28; DB2b-8 |
| IN-04-11 | **Datos internos de menores:** el nombre interno y el indicador de menor residen solo en Z2 y Z3, no en Z1 ni en Z4 y no se persisten en Z0; el nombre público con autorización puede tener copias derivadas en Z1 y Z0, sujetas a ADR-06 y HR1-11; **mientras Q-20 esté abierta, ninguna de esas copias contiene nombre público, solo etiqueta** [COND] | [CONF]; [COND] en lo relativo al nombre público (Q-20) | R1 §5.6, PA-08, PRV-09, PRV-11 | HR1-11; ADR-16.C |
| IN-04-12 | **Proyección de preparación:** contiene solo lo necesario para trabajar sin conexión (partidos asignados de los próximos 7 días y en juego, plantillas con etiqueta y dorsal —y con nombre público solo cuando Q-20 lo permita [COND]—, resumen del reglamento) y **nunca campos internos**; los partidos del propio equipo de un dueño u organizador con rol de delegado no se preparan para captura (RT-14); el editor ve etiqueta y dorsal, y el nombre público solo cuando Q-20 lo permita [COND] | [CONF] en OFF-02, PRV-09, PRV-11 y RT-14; [COND] en el nombre público (Q-20) | OFF-02, PRV-09, PRV-11, RT-14 | ADR-05.C (consumo); A-13 |
| IN-04-13 | **Aviso de antigüedad:** cada vista pública indica «Actualizado a las hh:mm» y, sin conexión, la última versión guardada con ese sello y aviso de que puede estar desactualizada; los datos preparados avisan tras 24 horas sin actualizarse | [DER] en RF-PUB-06; [CONF] en OFF-02 | RF-PUB-06, RF-PWA-02, OFF-02, OFF-18 | ADR-16.C; ADR-06 |
| IN-04-14 | **Vista previa al compartir (RF-PUB-05 [REC])** limitada al título y la descripción del torneo; no incluye datos de jugadores | [REC] en RF-PUB-05; [DER] la exclusión de jugadores | RF-PUB-05, PRV-03 | Q-13 |
| IN-04-15 | **Aviso de privacidad** accesible desde el pie de toda vista pública; su texto lo redacta o valida el responsable con asesoría jurídica | [CONF] | RF-PUB-08, PRV-07 | PEND-JUR |
| IN-04-16 | **Identificación indirecta de menores:** la combinación de equipo, dorsal, minuto, estadísticas, calendario o resúmenes exportados puede identificar a un menor aunque no se publique su nombre; se evalúa con asesoría jurídica antes del lanzamiento y **el contenido público definitivo depende de Q-20 y de ese resultado** | [CONF] la evaluación exigida; [ABIERTO] su resultado | PRV-18, PRV-02, Q-20, AM-31 | OC-45; RES-08 |
| IN-04-17 | **`archivado`:** un torneo `archivado` no aparece públicamente «por ahora»; no se asume un archivo histórico público | [CONF] en RF-PUB-01; [ABIERTO] Q-40 (d) | RF-PUB-01, §9.6, Q-11, Q-40 | D4-7 |
| IN-04-18 | **Sin permisos nuevos, sin reducción de garantías y sin selección tecnológica:** ADR-04.C no concede facultad alguna y respeta PA-13 y RC-23 | [DER] / [CONF] | R1 RC-23; R2-A IN-03-17, IN-09-11 | — |

### 22.5 Formulaciones alternativas, sin elegir

Cada decisión presenta **dos formulaciones**. Ninguna se aprueba, ninguna se recomienda y ninguna desplaza a las demás. «Dependencias» enumera lo que impide cerrar la formulación. Las formulaciones describen **capacidades**, no mecanismos. Los escenarios ESC-66… están en §22.7.

#### D4-1 — Materialización de la clasificación y de las proyecciones derivadas (IN-04-08; OC-20)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-04-P-A — Clasificación y proyecciones derivadas persistentes** | La clasificación y las estadísticas públicas existen como estado derivado persistente, actualizado dentro de la misma unidad que el resultado que lo origina (UI-05, UI-10) | Un único estado confirmado y derivado coherente por construcción dentro de la unidad (ES-14); la lectura pública no recalcula | Exige que UI-05, UI-07, UI-09 y UI-10 mantengan lo derivado; la anonimización debe alcanzar lo derivado (ES-20); más estado que reconstruir y verificar | Un derivado persistente con datos personales tras anonimizar (ES-20); deriva si la unidad no lo actualiza; mayor superficie para IN-04-10 | ADR-02.C (IF-22, GT-13); ADR-08; OC-20, OC-28; DB2b-3 |
| **CAP-04-P-B — Clasificación calculada al servir** | No existe estado derivado persistente: la clasificación se calcula al servir a partir del estado confirmado único | No hay estado derivado que escribir ni anonimizar; la coherencia descansa en un único resultado confirmado (IF-22) | La lectura pública exige calcular con la definición única de CMP-12; la coherencia entre vistas depende de calcular sobre el mismo estado (D4-2) | Calcular con estados distintos en vistas distintas; carga de lectura (RF-PUB-09, CL-30) | ADR-08 (dónde vive el cálculo); ADR-06; OC-20 |

#### D4-2 — Formulación de la garantía de coherencia entre vistas públicas (IN-04-06)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-04-C-A — Un único estado confirmado en las respuestas servidas por torneo** | Las respuestas públicas que el sistema sirve para un torneo reflejan, en el momento en que se sirven, el mismo conjunto de resultados confirmados y la clasificación que lo refleja; ninguna respuesta servida presenta un resultado confirmado junto con una clasificación que no lo refleje. **Quedan fuera de la garantía las copias locales y las vistas ya almacenadas, y se conservan las tolerancias documentadas** (RF-PUB-09 [REC] y CL-30: hasta 60 s; RF-PUB-06: última versión guardada con su sello; HR1-11) | Reproduce el texto de R1 §5.5 («coherencia entre resultados y clasificaciones» en las vistas relacionadas con un mismo torneo); verificable con CL-15 | Exige que la entrega pública presente un estado coherente por torneo, lo que condiciona a ADR-06; es la garantía más exigente **entre las respuestas servidas** | Un plazo de propagación más largo o un mecanismo más complejo; no se puede probar en esta fase (no demostrada) | ADR-06; ADR-07; CL-15; ES-14; IN-04-06 |
| **CAP-04-C-B — Coherencia dentro de cada respuesta pública (no conforme con el texto literal de R1 §5.5 y con ES-42, salvo aclaración formal de OC-46)** | Cada respuesta pública es coherente consigo misma (un resultado y la clasificación que ella muestra se corresponden); entre respuestas distintas puede haber un desfase acotado, indicado con «Actualizado a las hh:mm» | Menor exigencia sobre la entrega: la coherencia se exige dentro de cada respuesta | **No conforme con el texto literal de R1 §5.5**, que exige coherencia entre las vistas públicas relacionadas con un mismo torneo, **ni con ES-42**, salvo que una aclaración formal de OC-46 limite esas vistas a una sola respuesta; requeriría una aclaración o una edición separada de R1; **no se crea ninguna excepción** y la decisión es del responsable | Un visitante ve un resultado en una vista y una clasificación sin él en otra (ES-42) | R1 §5.5; CL-15; ES-42; OC-46; ADR-06; DC2-2 |

#### D4-3 — Representación de la visibilidad (§9.6; IN-04-01, IN-04-04)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-04-V-A — La proyección pública contiene solo lo visible** | Lo no visible (borrador, jornada no publicada, `archivado` si aplica, operación no confirmada) no se incorpora a ninguna proyección pública | Reduce el riesgo de fuga por una regla de filtrado omitida (AM-12, AM-13); coherente con PA-04 | Despublicar exige retirar lo ya incorporado, y las copias previas no se purgan a distancia (HR1-11); depende de una señal de cambio (ADR-06) | Un retiro tardío deja datos visibles en copias; trabajo de actualización por cada cambio de visibilidad | PA-04; ADR-06; HR1-11; OC-29 |
| **CAP-04-V-B — La proyección de uso público contiene datos y la visibilidad se aplica al servir** | La **proyección de uso público** guarda también lo no visible y el estado de publicación de CMP-09 se aplica al servir cada respuesta | Despublicar tiene efecto inmediato sobre lo servido desde el origen; no hay que retirar contenido de las proyecciones | Lo no visible reside en las proyecciones de uso público (Z1 podría contenerlo), lo que podría chocar con la lista blanca de PA-04 si la proyección llega a Z1; mayor riesgo de fuga por un fallo del filtro | AM-12 y AM-13 (un campo o un estado fuera de la lista blanca); la regla debe probarse en cada vista (§9.6; las doce combinaciones de RF-PUB-04) | PA-04; R1 §5.3 (CMP-14 lee el estado de CMP-09); `01` §9.6; ADR-06 |

#### D4-4 — Campos públicos y etiquetas de menores (PRV-02, PRV-18; Q-20)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-04-M-A — Lista blanca de PRV-02 y RF-PUB-02** | La proyección pública incluye solo los campos de una lista blanca cerrada, con las plantillas, goleadores y tarjetas de RF-PUB-02; los menores aparecen con etiqueta salvo autorización vigente; la etiqueta se calcula en el servidor; **mientras Q-20 esté abierta, ningún nombre de jugador (menor o adulto) se publica y solo se muestran etiquetas** [COND] | Es lo que dicen PRV-02 y RF-PUB-02; sin recorte adicional de contenido | Deja la identificación indirecta (equipo + dorsal + estadísticas) a PRV-18, **abierta**; la parte de nombre público es [COND] a Q-20 | AM-31 (identificación indirecta) sin mitigación propia; RES-08 | PRV-02, PRV-18, Q-20; RF-PUB-02; asesoría jurídica |
| **CAP-04-M-B — Lista blanca más reducción adicional de la combinación identificadora** | Además de la lista blanca, se limita la granularidad pública de los datos de menores (por ejemplo, el nivel de detalle de eventos, minutos o estadísticas individuales) hasta que PRV-18 concluya | Reduce el riesgo de AM-31 mientras la evaluación está abierta | **Contradice RF-PUB-02 [CONF] y la regla de PRV-02 de que las estadísticas cuentan igual con etiqueta o con nombre, si reduce goleadores, tarjetas o plantillas**; no contradice PRV-18, que prevé mitigaciones; **anticiparía el contenido público que fijan Q-20 y la evaluación de PRV-18** (ambas abiertas) y este borrador **no fija ninguna granularidad**; exigiría una lectura formal de `01` o su modificación, que no se propone ni se concede | Una restricción adicional no respaldada por `01`; fricción con lo que el organizador espera ver | PRV-18; Q-20; RF-PUB-02; modificación de `01` solo si el responsable la autoriza |

#### D4-5 — Proyección de preparación (OFF-02; IN-04-12)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-04-R-A — Proyección de preparación propia, por editor** | Una proyección distinta de la pública, construida para cada editor con sus partidos asignados y los campos mínimos de OFF-02; **mientras Q-20 esté abierta, con etiquetas y sin nombres de jugador** [COND] | Permite excluir de origen los partidos del propio equipo con RT-14 y los campos internos; ajustada a PRV-09 y PRV-11 | Una proyección más que mantener y sincronizar con cambios de visibilidad y de privacidad; depende de la asignación vigente | Preparar datos obsoletos tras una revocación de asignación (OFF-11); fuga de campos internos por error de construcción | OFF-02; PRV-09, PRV-11; RT-14; ADR-05.C (IN-05-01); ADR-16.C |
| **CAP-04-R-B — Preparación derivada de la proyección pública** | La preparación reutiliza lo público (plantillas con etiqueta y dorsal —y nombre público solo cuando Q-20 lo permita [COND]—, resumen del reglamento) más los datos de asignación de cada editor | Menos proyecciones distintas; lo que sale ya cumple la lista blanca | Los partidos del propio equipo con RT-14 y la asignación del editor deben filtrarse por separado; lo público puede no contener todo lo que OFF-02 requiere | Mezclar datos de asignación con una proyección pública (IN-04-02: identidad del editor nunca pública); dependencia entre vistas | OFF-02; PRV-03; RT-14; ADR-05.C |

#### D4-6 — Efecto de la revocación, ocultación y anonimización sobre las copias (IN-04-09; HR1-11; RES-02)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-04-X-A — Retiro en origen y señal de cambio, sin promesa de plazo** | El cambio se aplica en origen y se registra una señal de cambio si ADR-06 la adopta; las copias locales anteriores **pueden persistir sin plazo garantizado** hasta la próxima sincronización o limpieza (HR1-11), y un dispositivo que no se conecte puede conservarlas indefinidamente | Coherente con lo confirmado en `01` y R1; no cambia el contenido de las copias | El nombre de un menor puede seguir visible en una copia local hasta que el dispositivo se actualice (AM-28, RES-02, **sin aceptar**); **solo aplica a nombres ya publicados, y mientras Q-20 esté abierta no se publica ninguno** [COND] | AM-28; riesgo residual alto no eliminable por diseño | ADR-06; HR1-11; RES-02 (aceptación solo con asesoría jurídica) |
| **CAP-04-X-B — Contenido restringido de las copias locales** | **Desde su adopción**, las copias locales **nuevas** de vistas públicas y de datos preparados contienen etiqueta y no nombre público de menores, aunque exista autorización vigente; **no modifica las copias ya guardadas**, que siguen sujetas a HR1-11 | Reduce el riesgo residual de AM-28 **en las copias que se generen desde su adopción** | **Cambia el comportamiento de PRV-02 sin conexión** (el nombre autorizado no se vería en las copias); requiere una lectura formal de `01` o su modificación, que no se propone aquí; menor utilidad sin conexión; **no ofrece garantía sobre las copias existentes ni sobre lo que un dispositivo ya haya guardado** | Una autorización vigente no se refleja en la copia; fricción con RF-PUB-06 | PRV-02; RF-PUB-06; RF-PWA-02; ADR-16.C; modificación de `01` solo si el responsable la autoriza |

#### D4-7 — Visibilidad de un torneo `archivado` (Q-40 (d); IN-04-17)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-04-A-A — `archivado` no público** | Un torneo `archivado` no aparece en lo público (lectura actual de RF-PUB-01, «por ahora»); no se asume archivo histórico público | Coincide con RF-PUB-01 y con Q-11 (sin histórico); sin proyecciones adicionales que mantener | Un enlace compartido previamente deja de funcionar | Enlaces rotos; un torneo ya publicado desaparece de lo público | Q-40 (d); RF-PUB-01; Q-11 |
| **CAP-04-A-B — `archivado` público en solo lectura** | Un torneo `archivado` sigue visible al público en solo lectura | Mantiene los enlaces y la consulta histórica | **Requiere decidir Q-40 (d)**; añade un estado visible que debe respetar la lista blanca y las revocaciones de privacidad; contradice «no se asume archivo histórico público» salvo decisión formal | Datos de menores visibles durante más tiempo; más retiros si cambian autorizaciones | Q-40 (d); PRV-06; RES-02 |

#### D4-8 — Publicación y RT-14 (A-8; CMP-09)

| Formulación | Qué exige | Ventajas | Limitaciones | Riesgos | Dependencias |
|---|---|---|---|---|---|
| **CAP-04-G-A — Publicar y despublicar no restringidos por RT-14** | Habilitar y retirar la visibilidad no está sujeto a RT-14 (lectura provisional de A-8, RD-CAL-08) | Coincide con la lectura vigente; sin guardia adicional | El conflicto de interés se evalúa solo sobre lo que modifica partidos, no sobre su exposición pública | RK-16 (roles combinados) si se interpreta que publicar equivale a una decisión sobre el propio equipo | A-8; RD-CAL-08; RT-14; R2-B1 §17.6; DB1-3 |
| **CAP-04-G-B — Guardia opcional de RT-14 al habilitar la visibilidad** | Al habilitar la visibilidad de partidos de un equipo del que el actor es o ha sido delegado, se aplica la restricción de RT-14 | Cierra una posible vía de exposición de un propio equipo; es la guardia que R1 §5.3 deja prevista como opcional | **Ampliaría el alcance de RT-14** más allá de lo confirmado en `01` (RT-14 se evalúa sobre los partidos que la operación modifica y RD-CAL-08 [REC] dice que la publicación no los modifica); **reproduce la alternativa de A-8 que R2-B1 §17.6 ya lista** (punto opcional de guardia); la lectura de A-8 **corresponde al responsable (DB1-3; Fase 4)**, no a ADR-02.C | Bloqueos de publicación inesperados; extensión de RT-14 no autorizada | A-8; RT-14; R2-B1 §17.6; DB1-3; responsable (Fase 4); T-8 |

#### Interdependencias entre las decisiones

| Decisión | Se condiciona con | Observación |
|---|---|---|
| D4-1 | D4-2, D4-4, OC-20, OC-28 | CAP-04-P-A exige que la anonimización alcance lo derivado; CAP-04-P-B traslada la coherencia al cálculo |
| D4-2 | D4-1, D4-6, ADR-06 | CAP-04-C-B puede no ser conforme con R1 §5.5 |
| D4-3 | D4-7, D4-6 | CAP-04-V-A exige retirar lo incorporado; CAP-04-V-B exige filtrar al servir |
| D4-4 | Q-20, PRV-18, OC-45 | CAP-04-M-B puede exigir modificar `01` |
| D4-5 | ADR-05.C, A-13, D4-6 | La proyección de preparación es la que consume IN-05-01 |
| D4-6 | HR1-11, RES-02, ADR-06, ADR-16.C | CAP-04-X-B puede exigir modificar `01` |
| D4-7 | Q-40 (d), D4-3 | Condición antes de la Fase 3 |
| D4-8 | A-8, DB1-3, T-8 | CAP-04-G-B amplía el alcance de RT-14 |

### 22.6 Relación con la frontera atómica de ADR-02.C

**Regla fundamental (consumida de R1 §5.5, §7.4 y de IF-22, sin modificarla).** Lo transaccional es el **estado de los datos de Z2 y Z3** y, **si existieran**, sus proyecciones derivadas persistentes; la **entrega y el efecto hacia Z1 y Z0 son posteriores y no forman parte de la transacción**: ninguna atomicidad se atribuye a operaciones externas a ella. **ADR-02.C fija la frontera; ADR-04.C fija la garantía de coherencia; ADR-06 decide el mecanismo y el plazo; ADR-08 consume ambas.**

| Operación o unidad (de ADR-02.C) | Qué entrega a lo público | Dentro de la unidad (según §19 y §20) | Qué aporta ADR-04.C (sin cambiar la clasificación) |
|---|---|---|---|
| UI-10 Confirmar un resultado (también dentro de UI-06) | Resultado confirmado único; clasificación derivada | Marcador, eventos, estado, recálculo, **derivadas persistentes si existen**, señal de cambio si ADR-06 la adopta y auditoría | IN-04-06, IN-04-07, IN-04-08: la garantía de coherencia que esa unidad no puede demostrar por sí sola; D4-1, D4-2 |
| UI-05 Corregir un resultado | Resultado corregido y clasificación recalculada sin estado intermedio público | Igual que UI-10 (equivalente para correcciones) | Igual; CL-15 |
| UI-07 Anonimizar un jugador | Etiqueta en lugar del nombre; estadísticas intactas | Anonimización en la fuente y en las **derivadas persistentes, si existen**; señal de cambio si ADR-06 la adopta | IN-04-10, IN-04-11; D4-1, D4-6; OC-26, OC-28 |
| UI-09 Generar o regenerar el calendario | Calendario público de las jornadas visibles | Partidos nuevos, archivo de los anteriores, asignaciones perdidas y auditoría | IN-04-04: lo no publicado no se expone; efecto sobre jornadas publicadas |
| Habilitar y retirar la visibilidad (CAND-22) | Estado de publicación de torneos y jornadas | **No está enumerada como unidad**; su enumeración corresponde a la Fase 4 (R1 §5.5 es no exhaustiva; DB2b-5) | IN-04-04; D4-3, D4-7, D4-8 |
| Ocultar o revocar un nombre público (CAND-20) | Vuelve a etiqueta | **No enumerada como unidad**; Fase 4 | IN-04-03, IN-04-09, IN-04-10; D4-6 |

**Límites que ADR-04.C no puede cruzar:** (1) no mueve a la unidad nada que R1 deje fuera (entrega pública, señal entregada, efecto sobre Z1 y Z0); (2) no redefine las unidades UI-05, UI-07, UI-09 ni UI-10 ni crea una segunda unidad; (3) no afirma que la coherencia se cumple (X-3); (4) no elige el mecanismo (X-4); (5) no concede permisos ni reduce garantías (IN-04-18).

### 22.7 Estados prohibidos adicionales y casos límite

#### Estados prohibidos (se suman a ES-01…37)

| ID | Estado prohibido | Tipo | Fuente |
|---|---|---|---|
| ES-38 | Un campo de la lista «nunca público» (PRV-03) en cualquier vista o respuesta pública | [CONF] | PRV-03, RF-PUB-03 |
| ES-39 | El nombre de un menor publicado **sin autorización vigente**, o tras ocultarlo o anonimizarlo en una proyección bajo control del sistema (las copias locales no purgables son un riesgo residual: HR1-11) | [CONF] | PRV-02, PRV-06, CL-26 |
| ES-40 | Un torneo en `borrador`, una jornada no publicada o un torneo `archivado` (por ahora) accesibles o enumerables públicamente | [CONF] | RF-PUB-01, §9.6, CL-25 |
| ES-41 | Una operación `pendiente`, `enviando`, `en conflicto` o `rechazada` visible al público | [CONF] | OFF-15, §9.6 |
| ES-42 | Un resultado confirmado publicado junto con una clasificación que no lo refleja, o una clasificación que refleja un resultado no confirmado, **en vistas públicas relacionadas con un mismo torneo** (extiende ES-14, que cubre la clasificación derivada persistente) | [CONF] en CL-15 y R1 §5.5 como garantía exigida; [DER] en su extensión a las vistas | CL-15, R1 §5.5, ES-14 |
| ES-43 | Una proyección de preparación con campos internos o con partidos del propio equipo de un actor con restricción de RT-14 | [CONF] | OFF-02, PRV-11, RT-14 |
| ES-44 | Una vista previa de compartir o una exportación pública que incluya datos de jugadores | [DER] | RF-PUB-05, PRV-03, OFF-24 |

#### Escenarios de concurrencia y de privacidad (se suman a ESC-43…65)

«Admisible» significa que **equivale a alguna ordenación secuencial** (IF-05). **Ningún escenario se resuelve aquí por un mecanismo**; donde hay formulaciones, se indican sin elegir.

| ID | Escenario | Origen | Resultados admisibles | Resultados prohibidos | Formulaciones / condiciones |
|---|---|---|---|---|---|
| ESC-66 | Una lectura pública coincide con la confirmación de un resultado que cambia la clasificación | CL-15, CL-30 [CONF]; R1 §5.5 | **Sin elegir:** se ve el estado anterior completo o el posterior completo (CAP-04-C-A), o la respuesta es coherente consigo misma con un desfase acotado (CAP-04-C-B, posible no conformidad) | Resultado nuevo con clasificación antigua en vistas relacionadas (ES-42) | D4-1, D4-2; ADR-06 |
| ESC-67 | Corrección de una jornada anterior que altera desempates y la tabla pública | CL-15, RD-TAB-11, UI-05 [CONF] | La corrección y el recálculo se aplican como una sola unidad; ningún estado intermedio público | Tabla que mezcla datos previos y posteriores (ES-14, ES-42) | IN-04-06, IN-04-07; D4-1, D4-2 |
| ESC-68 | Se despublica una jornada o un torneo mientras un editor sincroniza resultados de esos partidos | §9.6, OFF-15 [CONF]; RD-CAL-08 [REC] (una jornada con resultados confirmados no se despublica salvo el dueño); CAND-22 | El resultado puede quedar `confirmada` pero **no es público** mientras la jornada no esté publicada; sin estados intermedios (IF-05) | Publicar un resultado de una jornada no publicada (ES-40); una operación no confirmada visible (ES-41) | IN-04-04; D4-3 |
| ESC-69 | Se oculta, se revoca la autorización o se anonimiza un jugador mientras una vista pública se sirve o existe una copia local | PRV-02, PRV-06, CL-26 [CONF]; HR1-11 | Las respuestas nuevas usan la etiqueta; las estadísticas no cambian; una copia local puede conservar el nombre hasta su actualización (riesgo residual, **sin plazo prometido**) | Datos personales en una proyección derivada persistente bajo control del sistema (ES-39, ES-20) | CAP-04-X-A o B; OC-29; RES-02 [sin aceptar] |
| ESC-70 | Una autorización de nombre público se revoca y se vuelve a otorgar | PRV-02, PRV-06 (ocultar es reversible; anonimizar no) [CONF] | Ocultar y reaplicar la autorización es reversible; anonimizar no | Tratar anonimizar como reversible | IN-04-10 |
| ESC-71 | Alguien comparte o consulta el enlace de un torneo en `borrador`, de una jornada no publicada o de uno `archivado` | CL-25, RF-PUB-01 [CONF]; RF-PUB-05 [REC] | 404 indistinguible de inexistente; la vista previa no revela el nombre del torneo no publicado [DER] | Enumeración o fuga por la vista previa (ES-40, ES-44) | IN-04-04, IN-04-14; D4-7 |
| ESC-72 | Un editor prepara el dispositivo y luego cambian la visibilidad o la privacidad de lo preparado | OFF-02, OFF-18, RF-PUB-06 [CONF]; HR1-11 | La preparación avisa tras 24 horas sin actualización; la copia local puede quedar desactualizada hasta sincronizar | Promesa de purga a distancia; campos internos en la preparación (ES-43) | IN-04-12, IN-04-13; D4-5, D4-6; ADR-05.C, ADR-16.C |
| ESC-73 | Un menor con etiqueta cuyo equipo, dorsal y estadísticas lo hacen identificable | PRV-18, CAB-12 [CONF la evaluación]; resultado [PEND-JUR] | **Sin resolver:** la evaluación de PRV-18 decide el contenido público definitivo | Publicar datos que la evaluación no haya aprobado cuando Q-20 esté abierta | D4-4; Q-20; RES-08; OC-45 |
| ESC-74 | Un editor con un partido del propio equipo pide prepararlo o publicarlo | RT-14, OFF-02 [CONF]; A-8 [provisional] | Los partidos del propio equipo de un dueño u organizador con rol de delegado no se preparan para captura | Preparar el partido propio (ES-43) | D4-5, D4-8; A-8 |

### 22.8 Amenazas y casos de abuso relevantes

**No se amplía el modelo de amenazas:** esta sección registra cómo las capacidades de ADR-04.C tratan las amenazas que R2-A asigna a R2-C. La **consolidación del modelo** corresponde al subbloque posterior (R2-C4) y no se hace aquí.

| Amenaza o caso | Qué es | Cómo la tratan las capacidades exigidas | Queda para |
|---|---|---|---|
| **AM-12 / CAB-03** | Enumerar o acceder a torneos, borradores y jornadas no publicados | IN-04-01, IN-04-04; ES-40; ESC-68, ESC-71 | D4-3, D4-7; Fase 4 (pruebas de §9.6) |
| **AM-13 / CAB-11, CAB-18** | Fuga técnica de datos internos de menores o no públicos por una proyección | IN-04-01, IN-04-02, IN-04-11; ES-38; ES-44 | D4-3, D4-4; pruebas de contrato (Fase 4) |
| **AM-27 / CAB-28** | Contaminar la entrega pública con datos no públicos o con un estado incoherente | IN-04-01, IN-04-06, IN-04-07; ES-42; ESC-66, ESC-67 | D4-2; ADR-06 (R3) |
| **AM-28 / CAB-29** | Señal de cambio omitida o tardía tras revocar, ocultar o anonimizar: el nombre de un menor sigue visible en copias | IN-04-09, IN-04-10; ES-39; ESC-69 | D4-6; ADR-06; **RES-02 sin aceptar** |
| **AM-31 / CAB-12** | Identificar a un menor por la combinación de equipo, dorsal, minuto, estadísticas o calendario | IN-04-16; ESC-73 | D4-4; PRV-18; **RES-08 sin aceptar**; asesoría jurídica |
| AM-19, AM-34 | Inyección en la capa de datos y de contenido en vistas públicas | **No se tratan aquí**: su reasignación está propuesta y abierta (OC-40, DB2c-11) | DB2c-11 |

### 22.9 Matriz de trazabilidad

| Fuente | Cubierta por | Observación |
|---|---|---|
| §9.6 (matriz de visibilidad) | IN-04-04; ES-40, ES-41; ESC-68, ESC-71; D4-3 | Las tres condiciones de publicación y las filas de la matriz; las doce combinaciones son de RF-PUB-04 |
| RF-PUB-01 | IN-04-04, IN-04-17; D4-7 | `archivado` no público «por ahora» |
| RF-PUB-02 | IN-04-01, IN-04-03; D4-4 | Contenido de la página del torneo; tensión con PRV-18 (T-3) |
| RF-PUB-03 | IN-04-01, IN-04-02 | Sin datos privados |
| RF-PUB-04 | IN-04-04 | Visibilidad (DER) |
| RF-PUB-05 [REC] | IN-04-14; ES-44 | Vista previa al compartir |
| RF-PUB-06; RF-PWA-02 | IN-04-13; D4-6; ESC-72 | Sello de antigüedad y copias sin conexión |
| RF-PUB-08 | IN-04-15 | Aviso de privacidad |
| RF-PUB-09 [REC]; CL-30 | IN-04-09; ESC-66; T-1 | Resultado confirmado visible en ≤ 60 s; sin plazo para cambios de privacidad |
| RF-PUB-07 [REC] | — | Estados de carga, vacío y error: UX (Fase 3); fuera de ADR-04.C |
| PRV-02, PRV-04 | IN-04-03; D4-4 | PRV-04 es [REC]; PRV-02 es [COND] mientras Q-20 esté abierta |
| PRV-03 | IN-04-02; ES-38 | Nunca públicos |
| PRV-05 | IN-04-02 (los metadatos de consentimiento no son públicos) | Registro de autorizaciones: ADR-09.C y CMP-17 |
| PRV-06; CL-26 | IN-04-10; ES-39; ESC-69, ESC-70; D4-6 | Ocultar y anonimizar |
| PRV-09, PRV-11 | IN-04-11, IN-04-12; D4-5 | Control de acceso y minimización |
| PRV-18 | IN-04-16; D4-4; ESC-73 | Identificación indirecta |
| OFF-02, OFF-18 | IN-04-12, IN-04-13; D4-5 | Preparación del dispositivo y consulta sin conexión |
| OFF-15; OFF-20; OFF-24 | IN-04-05; ES-41 | Lo local no es público; registros y exportación no autorizan publicar |
| CL-15; RD-TAB-11; RD-RES-07 | IN-04-06, IN-04-07; ES-42; ESC-66, ESC-67 | Coherencia y publicación automática |
| CL-25 | IN-04-04; ESC-71 | 404 indistinguible |
| RD-CAL-08 [REC]; A-8 | D4-8; ESC-68 | Publicación por jornada |
| Q-20; Q-40 (d); Q-11 | IN-04-03, IN-04-16, IN-04-17; D4-4, D4-7 | Parámetros abiertos que R1 §8.1 asigna a ADR-04 |
| PA-04, PA-08; R1 §5.6 | IN-04-01, IN-04-11 | Proyección con lista blanca; datos por zona |
| IF-20…24, IF-31; ES-14, ES-20; GT-13 | IN-04-06…IN-04-08, IN-04-10 | Consumidos de ADR-02.C / R2-B2 |
| IN-05-01, IN-05-14 | IN-04-05, IN-04-12 | Consumidos de ADR-05.C / R2-C1 |
| IN-09-07 | IN-04-10 | La auditoría no contiene datos personales de menores |

### 22.10 Dependencias, condiciones abiertas y decisiones

#### 22.10.1 Dependencias

| Elemento | Depende de lo aprobado | Alimenta (sin decidirlo) | Decisiones del responsable |
|---|---|---|---|
| IN-04-01…IN-04-05 (lista blanca, visibilidad, lo local) | PA-04, PRV-03, §9.6, OFF-15; R1 §5.6 | ADR-04.I; ADR-16.C | D4-3, D4-7; Q-40 (d) |
| IN-04-06…IN-04-08 (coherencia, entrada única, derivadas) | IF-20…22, ES-14, GT-13; CL-15 | ADR-06, ADR-07, ADR-08 (R3) | D4-1, D4-2; OC-20, OC-46 |
| IN-04-09…IN-04-11 (cambios de privacidad, ocultar y anonimizar, datos internos) | PRV-02, PRV-06, CL-26, ES-20; R1 §7.4 | ADR-06; ADR-16.C; ADR-17 (retención) | D4-6; OC-26, OC-28, OC-29, OC-47; RES-02 |
| IN-04-12…IN-04-15 (preparación, antigüedad, compartir, aviso) | OFF-02, OFF-18, RF-PUB-05, 06, 08 | ADR-05.C (consumo); ADR-16.C | D4-5; Q-13 |
| IN-04-16 (identificación indirecta) | PRV-18, PRV-02, Q-20 | Asesoría jurídica; ADR-17 | D4-4; OC-45; RES-08 |
| IN-04-17 (`archivado`) | RF-PUB-01, Q-40 | — | D4-7 |

#### 22.10.2 Condiciones y decisiones heredadas (conservadas abiertas)

**Ninguna se resuelve ni se infiere.** Se listan porque condicionan a ADR-04.C.

| Elemento | Estado | Cómo afecta a ADR-04.C |
|---|---|---|
| Q-20 (campos públicos y edad de corte); PRV-02, PRV-04 [COND] | Abierta; **no se publica ningún dato personal mientras esté abierta** | Contenido público de jugador (D4-4) |
| PRV-18; RES-08; AM-31 | Evaluación jurídica pendiente; riesgo sin aceptar | Identificación indirecta (D4-4; OC-45) |
| Q-40 (d) | Abierta; antes de la Fase 3 | Visibilidad de `archivado` (D4-7) |
| A-8 | Lectura provisional; DB1-3 abierta | D4-8; T-8 |
| OC-20 / DB2b-3 | Abiertas | Materialización de la clasificación (D4-1) |
| OC-26, OC-27, OC-28 / DB2b-8 | Abiertas | Alcance de la anonimización y de lo derivado persistente |
| OC-29 | Abierta | Señal de cambio y plazo (ADR-06) |
| HR1-11, HR1-12; RES-02 | Riesgos sin aceptar | Copias locales y respaldos |
| RF-PUB-05, RF-PUB-09, RD-CAL-08 [REC]; PRV-04 [REC] | Recomendaciones no confirmadas | IN-04-14, IN-04-09, D4-8, IN-04-03 |
| Decisión 3 y 10 de R2-A; K-3; D-2; DB1, DB2a, DB2b, DB2c, DC1 y OC-01…44 | Abiertas | Conservadas; ninguna se toca |
| OC-40 / DB2c-11 | Reasignación propuesta de AM-19 y AM-34, no aplicada | AM-34 (contenido en vistas públicas) no se trata aquí |

#### 22.10.3 Condiciones abiertas nuevas de este borrador

| ID | Condición abierta | Relación |
|---|---|---|
| OC-45 | Contenido público definitivo de los datos de jugadores (combinación de equipo, dorsal, minuto, estadísticas y calendario): `01` deja el resultado de la evaluación de PRV-18 abierto y pendiente de asesoría jurídica | D4-4; Q-20; PRV-18; RES-08 |
| OC-46 | Qué vistas públicas son «relacionadas con un mismo torneo» a efectos de la garantía de coherencia: R1 §5.5 y CL-15 no las enumeran | D4-2; IN-04-06 |
| OC-47 | Si las copias locales de vistas públicas y de datos preparados pueden contener el nombre público de un menor con autorización vigente: PRV-02 no distingue entre la vista servida y su copia | D4-6; HR1-11; RF-PUB-06 |

#### 22.10.4 Decisiones propuestas al responsable (DC2-)

**Ninguna se resuelve aquí.** Las decisiones heredadas permanecen como estaban.

| ID | Decisión | Opciones | Efecto |
|---|---|---|---|
| DC2-1 | Materialización de la clasificación (D4-1; OC-20) | CAP-04-P-A / CAP-04-P-B / mantener abierta | Si existen derivadas persistentes; DB2b-3 |
| DC2-2 | Formulación de la coherencia pública (D4-2; OC-46) | CAP-04-C-A / CAP-04-C-B (posible no conformidad con R1 §5.5) / pedir una aclaración de R1 | Alcance de la garantía |
| DC2-3 | Representación de la visibilidad (D4-3) | CAP-04-V-A / CAP-04-V-B / mantener abierta | Dónde se aplica §9.6 |
| DC2-4 | Campos públicos y etiquetas (D4-4; Q-20; PRV-18; OC-45) | CAP-04-M-A / CAP-04-M-B (puede exigir modificar `01`) / mantener abierta | Contenido público de jugador |
| DC2-5 | Proyección de preparación (D4-5) | CAP-04-R-A / CAP-04-R-B / mantener abierta | Qué consume ADR-05.C |
| DC2-6 | Efecto sobre las copias (D4-6; OC-47; RES-02) | CAP-04-X-A / CAP-04-X-B (puede exigir modificar `01`) / mantener abierta | Riesgo residual de AM-28 |
| DC2-7 | Visibilidad de `archivado` (D4-7; Q-40 (d)) | CAP-04-A-A / CAP-04-A-B / mantener abierta | Antes de la Fase 3 |
| DC2-8 | Publicación y RT-14 (D4-8; A-8) | CAP-04-G-A / CAP-04-G-B (amplía RT-14) / mantener abierta | Alcance de A-8 |
| DC2-9 | Estado de las propuestas [REC] que ADR-04.C consume (RF-PUB-05, RF-PUB-09, RD-CAL-08, PRV-04) | Adoptar / descartar / decidirlo en una etapa posterior | Si IN-04-03, 09 y 14 pasan a exigencias |
| DC2-10 | Orientación sobre las tensiones T-1…T-8 de §22.2 | Recibir sin resolver / pedir aclaración de `01` o de R1 por edición separada | Alcance frente a ADR-06 y ADR-16.C |
| DC2-11 | Alcance y orden de R2-C3 (ADR-16.C) y R2-C4 (consolidación del modelo de amenazas), que requieren autorización separada | Autorizar / posponer | ADR-16.C consume la proyección de preparación y las copias |

### 22.11 Riesgos y limitaciones

Los riesgos se **registran sin valorar ni aceptar**. Ninguno sustituye a RB1, RB2a, RB2b, RB2c, RR5 ni RES-01…12.

| ID | Riesgo | Relación |
|---|---|---|
| RR4-01 | Elegir de hecho una formulación al describir ventajas y limitaciones | §22.5; DC2-1…DC2-8 |
| RR4-02 | Resolver Q-20, PRV-18, Q-40 (d) o A-8 por inferencia | D4-4, D4-7, D4-8 |
| RR4-03 | Tratar como confirmadas las propuestas [REC] (RF-PUB-05, RF-PUB-09, RD-CAL-08, PRV-04) | DC2-9 |
| RR4-04 | Invadir ADR-06, ADR-07 y R3 al describir actualización, expiración o entrega | X-4; §22.3 |
| RR4-05 | Contradecir `01` o R1 con CAP-04-M-B, CAP-04-X-B, CAP-04-G-B o CAP-04-C-B | D4-2, D4-4, D4-6, D4-8 |
| RR4-06 | Presentar la coherencia pública como demostrada o cumplida | IN-04-06; X-3 |
| RR4-07 | Publicar datos que permitan identificar a un menor sin que PRV-18 haya concluido | AM-31; RES-08; OC-45 |
| RR4-08 | Nombre de un menor visible en copias locales tras una revocación | AM-28; RES-02; HR1-11 |
| RR4-09 | Incoherencia con las fronteras aprobadas de UI-05, UI-07, UI-09 y UI-10 | §22.6 |
| RR4-10 | Interpretar este borrador como cumplimiento legal o sustituto de la asesoría jurídica | X-8; PEND-JUR |

**Limitaciones del análisis.** (1) Es un borrador del autor, **sin revisión de contexto nuevo ni independiente**. (2) Se leyeron las secciones pertinentes de las fuentes (§22.0), no todas línea a línea. (3) No se realizó ninguna prueba ni medición (RF-PUB-09, CL-30). (4) Las lecturas [DER] y la clasificación de ventajas, limitaciones y riesgos son del autor y deben revisarse. (5) Las referencias jurídicas de `01` §10 son conocimiento general no verificado y no son asesoría legal.

### 22.12 Criterios de aceptación del borrador y preguntas para el responsable

| ID | Criterio de aceptación | Cumplimiento según el autor (a revisar) |
|---|---|---|
| CR2C2-01 | Alcance, fuentes y dependencias identificados | Cumple: §22.0–§22.3 |
| CR2C2-02 | Al menos dos formulaciones diferenciadas por decisión, con ventajas, limitaciones, riesgos y dependencias | Cumple: §22.5 (D4-1…D4-8) |
| CR2C2-03 | Ninguna formulación elegida ni aprobada | Cumple: §22.0 (X-1), §22.5 |
| CR2C2-04 | Invariantes y garantías propuestas, con tipo y fuente | Cumple: §22.4 |
| CR2C2-05 | Relación con la frontera atómica de ADR-02.C sin redefinirla | Cumple: §22.6 |
| CR2C2-06 | Estados prohibidos, escenarios y amenazas relevantes | Cumple: §22.7 y §22.8 |
| CR2C2-07 | Matriz de trazabilidad | Cumple: §22.9 |
| CR2C2-08 | Decisiones abiertas de privacidad y autorización de menores conservadas (Q-20, PRV-18, Q-40 (d), A-8, OC-26…29) | Cumple: §22.10.2 |
| CR2C2-09 | Conflictos entre fuentes señalados sin resolver | Cumple: §22.2 (T-1…T-8) |
| CR2C2-10 | Sin mecanismos, tecnologías ni parámetros no aprobados | Cumple según §22.13 |
| CR2C2-11 | Solo se modifica el archivo autorizado y los bloques previos permanecen intactos | Cumple según §22.13 |
| CR2C2-12 | Borrador pendiente de revisión del responsable; no aprobado, registrado ni versionado | Cumple: §22.0 y §22.14 |

**Preguntas concretas para el responsable** (no se responden por inferencia): (1) ¿Qué formulación, si alguna, desea evaluar con más detalle en cada D4-n (DC2-1…DC2-8)? (2) ¿Desea que Q-20, PRV-18 y Q-40 (d) se traten antes de la Fase 3, o que ADR-04.C las conserve abiertas? (3) ¿Pide una aclaración de R1 §5.5 sobre qué vistas están «relacionadas con un mismo torneo» (OC-46)? (4) ¿Se adoptan o se descartan las propuestas [REC] que este borrador consume (DC2-9)? (5) ¿Autoriza R2-C3 (ADR-16.C) y R2-C4, cuyo orden depende de este borrador (DC2-11)?

### 22.13 Verificación del autor

**Naturaleza.** Verificaciones **mecánicas de lectura** hechas por el autor con un guion de solo lectura. **No son una auditoría independiente ni una revisión de contexto nuevo:** el autor no puede verificar su propio trabajo con independencia. **Este borrador no ha sido auditado ni revisado por nadie más.**

| # | Verificación | Resultado |
|---|---|---|
| V-1 | Las líneas anteriores a la Parte VIII (todo el archivo del commit `a468166`) son **idénticas**; el cambio son solo líneas añadidas al final; ninguna eliminada | Cumple (comparación con `git show HEAD:`) |
| V-2 | Solo cambió `02-arquitectura-adr.md`; `00`, `01`, `aprobaciones.md` y `bitacora.md` no cambiaron | Cumple (`git status` y sumas SHA-256) |
| V-3 | Identificadores definidos y sin referencias huérfanas: IN-04-01…18, ES-38…44, ESC-66…74, OC-45…47, DC2-1…11, RR4-01…10, CR2C2-01…12, D4-1…8 y las dieciséis formulaciones CAP-04-P, C, V, M, R, X, A y G (A y B) | Cumple (guion de lectura) |
| V-4 | Las citas literales entre «» se buscaron en `00`, `01` y las partes previas | Todas existen, **salvo** los mensajes del responsable citados en §22.0, que proceden de la conversación y no de un archivo |
| V-5 | Exploración de términos tecnológicos (nombres de bases de datos, productos, proveedores, librerías, algoritmos, mecanismos de caché, expiración, invalidación, tiempo real o entrega y similares) | Solo aparecen en X-4 y en esta fila como **exclusión**; ninguna elección ni mecanismo |
| V-6 | Parámetros numéricos: solo los que cita `01` (60 s, 24 horas, 7 días, 404) como referencias; ninguno nuevo | Cumple (lectura del autor) |
| V-7 | Ninguna decisión abierta aparece resuelta: no se elige ninguna formulación; Q-20, PRV-18, Q-40 (d), A-8, OC-20, OC-26…29, K-3, D-2 y las demás permanecen abiertas; no se acepta RES-02 ni RES-08; no se declara cumplido R1 §8.1 | Cumple (lectura del autor) |
| V-8 | Las tablas están alineadas (mismo número de columnas por fila) | Cumple (guion de lectura) |
| V-9 | **Coherencia con la frontera:** §22.6 reproduce lo que las fichas de UI-05, UI-07, UI-09 y UI-10 dejan dentro y fuera de la unidad y no cambia la clasificación | Cumple (comparación con §19, §20.3.1 y R1 §7.4) |
| V-10 | Cada decisión D4-n presenta **dos** formulaciones con ventajas, limitaciones, riesgos y dependencias | Cumple (§22.5) |

**Límites.** Las lecturas [DER] y la clasificación de ventajas, limitaciones y riesgos son del autor y deben revisarse. Las ocho tensiones de §22.2 son una identificación del autor.

### 22.14 Estado de esta parte y lo que no se hizo

**Estado: v0.2-R2C2, APROBADO CONDICIONALMENTE COMO ENTREGABLE EXCLUSIVAMENTE DOCUMENTAL (2026-11-22, inferida).** Corrigió los hallazgos F-01…F-06 y tres observaciones editoriales (§22.15). Aprobación registrada en §22.0, `aprobaciones.md` y `bitacora.md`; **no versionado**.

**Qué se hizo.** Se redactó la Parte VIII (§22) como borrador de ADR-04.C: alcance, fuentes y dependencias (§22.0–§22.3), invariantes y garantías exigidas (§22.4), ocho decisiones con dos formulaciones cada una (§22.5), relación con la frontera atómica de ADR-02.C (§22.6), estados prohibidos y escenarios (§22.7), amenazas relevantes (§22.8), matriz de trazabilidad (§22.9), dependencias, condiciones y decisiones abiertas (§22.10), riesgos (§22.11) y criterios de aceptación y preguntas (§22.12).

**Qué no se hizo.** No se redactó ADR-16.C ni la consolidación del modelo de amenazas (R2-C3 y R2-C4), ni R3 ni R4; no se aprobó ADR-04.C, R2-C, R2 ni la Fase 2; no se eligió ninguna formulación; no se resolvió ninguna decisión ni condición abierta (Q-20, PRV-18, Q-40 (d), A-8, OC-20, OC-26…29, K-3, D-2 y las demás); no se eligió tecnología, mecanismo ni parámetro; no se aceptó ningún riesgo ni se concedió excepción; no se declaró cumplido R1 §8.1; no se modificaron otros archivos; no se actualizó la trazabilidad histórica de `a468166`; no se crearon ramas, commits, pushes, PR ni merges; no se tocó `main`.

**Git.** Rama `claude/fase-2-arquitectura-amenazas`; HEAD `a468166210e0472e07dd5d0800985d3db1c6b940`; los cambios son `02-arquitectura-adr.md`, `aprobaciones.md` y `bitacora.md` **sin confirmar** (registro de la aprobación). Cualquier commit, push o PR requiere autorización expresa y separada.

### 22.15 Hallazgos de la revisión focalizada de la v0.1 (F-01…F-06) y correcciones de la v0.2

**Origen.** Autoevaluación del autor sobre la v0.1 (SHA-256 `72fe0c4b9f41d20b8f93bf396e360fc8bc22c95a2f52af94f1b29e68d5abf0e0`): **no es una auditoría independiente ni una revisión de contexto nuevo**. Las correcciones fueron autorizadas en una ronda única (§22.0). **No equivalen a aprobación.**

| ID | Corrección en la v0.2 | Dónde |
|---|---|---|
| F-01 | CAP-04-C-A acotada a las respuestas servidas por el sistema; las copias locales, las vistas almacenadas y las tolerancias documentadas (RF-PUB-09 [REC], CL-30, RF-PUB-06, HR1-11) quedan fuera de la garantía y se conservan | D4-2 |
| F-02 | CAP-04-C-B rotulada no conforme con el texto literal de R1 §5.5 y con ES-42, salvo aclaración formal de OC-46; ventaja sobre copias locales eliminada; se conserva sin elegirla | D4-2 |
| F-03 | CAP-04-M-B: contradice RF-PUB-02 y la regla de PRV-02 sobre estadísticas si reduce goleadores, tarjetas o plantillas; anticiparía Q-20 y PRV-18 sin fijar granularidad | D4-4 |
| F-04 | CAP-04-X-B prospectiva (copias nuevas; las existentes siguen sujetas a HR1-11); CAP-04-X-A reconoce que las copias anteriores pueden persistir sin plazo | D4-6 |
| F-05 | La decisión de A-8 se atribuye al responsable (DB1-3; Fase 4); se incorpora R2-B1 §17.6; circularidad con R1 §8.2 registrada como T-8 sin resolverla | D4-8, §22.2, §22.3 |
| F-06 | Condición de Q-20 propagada a IN-04-03, IN-04-11, IN-04-12, CAP-04-M-A, CAP-04-R-A/B y CAP-04-X-A: mientras Q-20 siga abierta solo etiquetas; reglas de menores [CONF] y de adultos [COND] distinguidas; T-2 ampliada | §22.2, §22.4, D4-4…D4-6 |
| Obs. 1 | Rótulo de IN-04-06 alineado con CAP-04-C-B, sin relajar la garantía | IN-04-06 |
| Obs. 2 | "Proyección de uso público" en CAP-04-V-B | D4-3 |
| Obs. 3 | RF-PUB-04 identificada como fuente de las doce combinaciones | IN-04-04, §22.9 |
