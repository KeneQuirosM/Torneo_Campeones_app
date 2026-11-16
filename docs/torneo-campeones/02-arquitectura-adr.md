# 02 · Arquitectura y amenazas — Torneo de Campeones

- **Fase:** 2 — Arquitectura y amenazas.
- **Estado del documento:** **R1 APROBADO — 2026-11-16 (v0.5-R1).** Aprobación formal del bloque R1 otorgada por el usuario y registrada en `aprobaciones.md` y `bitacora.md`; alcance: exclusivamente el alcance documental de este documento. **La Fase 2 NO está aprobada globalmente.** Las decisiones técnicas de R2, R3 y R4 siguen abiertas; ninguna decisión de este documento selecciona proveedor, stack ni mecanismo.
- **Estado de la fase:** Fase 2 **AUTORIZADA, NO APROBADA globalmente**. R1 autorizado el 2026-11-15 y **APROBADO el 2026-11-16**; **R2, R3 y R4: NO INICIADOS Y SIN AUTORIZACIÓN**.
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
