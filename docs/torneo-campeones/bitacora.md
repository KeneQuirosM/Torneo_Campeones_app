# Bitácora del desarrollo — Torneo de Campeones

## 2026-11-15 — Fase 2 (apertura: arquitectura y amenazas)

- **Autorización recibida:** mensaje «AUTORIZACIÓN FORMAL DE APERTURA DE FASE 2»: «AUTORIZO EXPRESAMENTE EL INICIO DE LA FASE 2 — ARQUITECTURA Y AMENAZAS, exclusivamente bajo las condiciones siguientes.» Complementada por el mensaje «PREPARACIÓN DOCUMENTAL DE FASE 2» (rama, tercera alternativa C1, edición de `aprobaciones.md` y `bitacora.md`). **Autorizada, no aprobada.**
- **Alcance:** arquitectura lógica, componentes y límites de confianza; multi-torneo; flujos críticos y estrategia conceptual offline-first; modelo de amenazas y seguridad; ADR; estrategia conceptual de caché, publicación y tiempo real; evaluación de proveedores, infraestructura y costos mediante fuentes oficiales. **Fuera de alcance:** diseño técnico detallado (Fase 4), implementación, prototipos ejecutables, migraciones y provisión de servicios.
- **Restricciones:** sin gastos ni creación de cuentas; sin dependencias, código ni prototipos; sin `02-arquitectura-adr.md` ni contenidos de R1 todavía; sin commit, push, PR ni merge; ningún proveedor ni stack seleccionado.
- **Condiciones vinculantes:** R1–R4 con aprobación expresa antes de avanzar; ≥ 3 arquitecturas en el ADR principal y ≥ 2 alternativas en los demás ADR, salvo justificación; candidatos preferentes no vinculantes Vercel + Supabase y Vercel + Neon, más C1 (hosting alternativo con PostgreSQL gestionado); prioridad: costos bajos, seguridad, offline y mantenimiento; titularidad y administración principal de cuentas de la organización (responsable por designar); trazabilidad de 18 preguntas, 23 `COND` y 14 ambigüedades; auditoría final de solo lectura por un revisor separado del autor y con contexto nuevo, sin afirmar independencia organizativa o externa inexistente; Android e iPhone previstos para pruebas posteriores.
- **Dependencias y estado real del proyecto:** Fase 0 APROBADA; Fase 1 APROBADA CON CONDICIONES (v1.3.1), integrada en `main` por el PR #3 (merge commit `a05543417236e8fa3a5959979c5672c282385350`); Fases 3–8 y módulos 5A–5G NO INICIADOS. Siguen abiertos: 18 preguntas (Q-10, 13, 14, 20, 21, 22, 23, 26, 27, 28, 29, 30, 31, 33, 40, 41, 42, 43), 23 requisitos `COND` y 14 ambigüedades; hallazgos diferidos H-07, H-08, H-09; Q-14 y Q-40 (d) antes de la Fase 3; evaluación jurídica y verificación de proveedores pendientes; responsable administrativo de cuentas por designar.
- **Operaciones realizadas:** verificación de árbol limpio; `git fetch origin main` (actualiza solo la referencia remota; `main` en `a055434`, merge del PR #3); creación y cambio a la rama local `claude/fase-2-arquitectura-amenazas` desde `a055434` (la rama de la Fase 1 no se reutilizó ni modificó); edición de `aprobaciones.md` (incluida la corrección de la nota Git de la Fase 1) y de esta bitácora. Sin commit ni push.
- **Archivos modificados:** `aprobaciones.md`, `bitacora.md`. No se tocaron `01-requisitos.md`, `00-descubrimiento.md`, la skill ni el README.
- **Lo que NO se hizo:** `02-arquitectura-adr.md`; contenido de R1; R2, R3, R4; investigación de proveedores; código; prototipos; migraciones; servicios; cuentas; gastos; commit, push, PR, merge.
- **Estado de la fase:** Fase 2 AUTORIZADA (no aprobada). **R1 pendiente de autorización independiente.**
- **Pregunta para el siguiente paso:** ¿Autoriza el bloque R1 (línea base y arquitectura lógica) y, por separado, el versionado de esta apertura?

---

## 2026-11-15 — Fase 1 (aprobación formal con condiciones, v1.3.1)

- **Autorización recibida:** mensaje «AUTORIZACIÓN FORMAL DE CIERRE DE FASE 1» del 2026-11-15: «AUTORIZO FORMALMENTE LA APROBACIÓN CON CONDICIONES DE LA FASE 1 — REQUISITOS Y MVP, VERSIÓN DOCUMENTAL v1.3.1.» Autoriza actualizar exclusivamente el estado documental en `aprobaciones.md`, `bitacora.md` y, solo en referencias de estado, cierre y trazabilidad, `01-requisitos.md`. Antecedente: el borrador del acta (marcado «NO EJECUTAR») no se ejecutó.
- **Condiciones aceptadas:** 18 preguntas abiertas y 23 requisitos `COND` sin aprobar sus alternativas; 14 ambigüedades abiertas (incluidas A-15 y A-16), cuyas lecturas provisionales no son decisiones ni amplían permisos; hallazgos diferidos H-07, H-08, H-09 (F4; F8 para RNF-16 y RNF-17); vigentes seguridad, autorización, conflictos de interés, auditoría, protección de menores y puertas jurídicas, de proveedores y de datos reales; RK-15 aceptado (ninguna otra aceptación se presume); RF-EQP-06 diferido; Q-37 confirmada; Q-14 y Q-40 (d) antes de la Fase 3.
- **Alcance de la aprobación:** cierre documental de la Fase 1. No es aprobación de la implementación, autorización de producción, certificación legal ni verificación de que ningún control esté implementado.
- **Trabajos realizados:** estado de la Fase 1 actualizado a APROBADA CON CONDICIONES en la tabla de control, «Última aprobación comprobable», eventos y «Próximo paso» de `aprobaciones.md`; encabezado, nota de cierre y §17.6 de `01-requisitos.md`, que conserva el dictamen técnico previo como evidencia histórica; esta entrada. No se alteraron requisitos, prioridades, decisiones, reglas, matriz, historias, riesgos, pruebas ni alcance.
- **Archivos modificados:** `01-requisitos.md`, `aprobaciones.md`, `bitacora.md`. Sin commit.
- **Comandos ejecutados:** ediciones de texto y verificaciones de solo lectura (conteos, referencias, estados, `git status`, `git diff --stat`); ninguna operación de publicación o versionado.
- **Lo que NO se hizo:** Fase 2; código; skill; arquitectura; infraestructura; base de datos; migraciones; dependencias; commit, push, PR, merge, cambio de rama; no se resolvió ninguna pregunta ni ambigüedad.
- **Estado de la fase:** Fase 0 APROBADA; Fase 1 **APROBADA CON CONDICIONES**; Fases 2–8 y módulos 5A–5G NO INICIADOS.
- **Siguiente paso:** cualquier paso posterior (Fase 2, Git, decisiones pendientes) requiere una autorización expresa e independiente.

---

## 2026-11-15 — Fase 1 (corrección documental final v1.3.1)

- **Autorización recibida:** mensaje «CORRECCIÓN DOCUMENTAL FINAL v1.3.1» del 2026-11-15: corrección documental acotada, basada en los hallazgos H-01…H-11 de la auditoría independiente final, exclusivamente sobre `01-requisitos.md`, `aprobaciones.md` y `bitacora.md`. Conservar Q-38, Q-39, Q-44…Q-46, RK-15 aceptado, RF-EQP-06 fuera del MVP, las 18 preguntas abiertas, las puertas jurídicas y los 186 requisitos. La Fase 1 sigue pendiente de aprobación y la Fase 2 no está autorizada.
- **Alcance permitido:** solo documentación. Sin código, skill, infraestructura, base de datos, commit, push, PR, merge, cambio de rama, cambios en `main` ni Fase 2.
- **Trabajos realizados (`01-requisitos.md` v1.3.1, trazabilidad en §17.5):** H-01 marcadores de plantilla de §17.3 sustituidos por conteos reales (sin otros residuales); H-02 RT-14 y RK-04 distinguen 14 ambigüedades abiertas de A-3 y A-7 resueltas; H-03 §13.7 con requisitos afectados, riesgo y efecto sobre F1/F2, y aclaración de que las lecturas provisionales no son decisiones; H-04 A-15 y A-16 registradas como abiertas, sin respuesta, con denegación por defecto documentada como criterio provisional (no política) y notas cruzadas en RF-ACC-07 y en la leyenda de la matriz sin cambiar permisos; H-05 §14.1, §14.4, §14.5, RK-03 y RK-10 alineados (RF-ACC-01…17, RT-01…14, OFF-01…25, 13 actores de §7.3, ≈ 27–42 semanas); H-06, H-10, H-11 ajustes de referencias y redacción; H-07, H-08 y H-09 comprobados y **diferidos** (fase límite F4; RNF-16 y RNF-17 F8) sin inventar criterios, requisitos ni permisos.
- **Archivos inspeccionados y modificados:** `01-requisitos.md`, `aprobaciones.md`, `bitacora.md`. Sin commit.
- **Comandos ejecutados y resultados:** ediciones de texto; verificación mecánica de solo lectura sobre el documento: 186 requisitos (54 RF, 72 RD, 25 OFF, 18 PRV, 17 RNF); 169 M, 11 S, 6 P; 23 `COND`; 14 RT; matriz de 41 filas; 72 CL; 51 HU; 22 RK; 18 preguntas abiertas; 0 identificadores sin definir; 0 marcadores residuales; 0 cambios irregulares de columnas en tablas; 0 requisitos M sin criterio o prueba. No se ejecutó ninguna prueba del producto (no hay código) ni se afirma que ningún control esté implementado.
- **Lo que NO se hizo:** Fase 2; código; esquema; migraciones; dependencias; cambios a la skill; commit, push, PR o merge; no se resolvió ninguna de las 18 preguntas ni A-15 y A-16; no se cambiaron la matriz de permisos, la máquina de estados ni la aprobación de la Fase 1.
- **Riesgos o bloqueos:** los de la v1.3 (18 asuntos abiertos, evaluación jurídica, persona responsable, RK-15 aceptado, RK-22) más 14 ambigüedades abiertas con lectura provisional.
- **Estado de la fase:** PENDIENTE DE APROBACIÓN. Dictamen técnico: APROBABLE CON CONDICIONES (recomendación).
- **Pregunta para siguiente aprobación:** ¿Apruebas los entregables de la Fase 1 (v1.3.1) y autorizas iniciar la Fase 2: arquitectura y amenazas?

---

## 2026-11-15 — Fase 1 (consolidación v1.3: Q-38, Q-39, Q-44, Q-45, Q-46, RK-15 aceptado y RF-EQP-06 diferido)

- **Autorización recibida:** mensaje «EJECUCIÓN CONTROLADA DE CONSOLIDACIÓN DOCUMENTAL v1.3» del 2026-11-15: actualizar exclusivamente `01-requisitos.md`, `aprobaciones.md` y `bitacora.md` a la v1.3 conforme al Plan Definitivo y Verificable; conservar las v1.1 y v1.2; mantener las 18 preguntas abiertas sin decidirlas; recalcular los conteos; verificar identificadores, referencias, matrices, tablas, estados, requisitos M, criterios y trazabilidad; auditar el resultado. La Fase 1 sigue pendiente de aprobación y la Fase 2 no está autorizada.
- **Alcance permitido:** solo documentación. Sin código, esquemas, migraciones, dependencias, cambios a la skill, infraestructura, Fase 2, commit, push, PR, merge, cambios en `main` ni despliegues.
- **Trabajos realizados:** `01-requisitos.md` pasa a v1.3 sin descartar v1.1 ni v1.2 (historial en §0.4): Q-38, Q-39, Q-44, Q-45 y Q-46 como `CONF` (§2.1c); nuevo RT-14 (conflicto de interés por efecto, persistencia histórica y levantamiento), RF-ACC-17 (S), RF-AUD-06, RD-RES-12 y RD-RES-13, RD-EVT-12 y OFF-25; RD-RES-10 reescrito; RD-TAB-03 marcado `COND:Q-29` en la parte de `suspendido`; RF-TOR-03 con trazabilidad a las pruebas de RD-REG-01, 04 y 06; cinco requisitos M con criterio propio; matriz de 41 acciones; 72 casos límite; 51 historias; RK-15 aceptado; RF-EQP-06 diferido (P); §13 reorganizada (21 confirmadas, 18 abiertas, ambigüedades A-n y tabla de requisitos `COND` con fecha límite); trazabilidad §14.8; estimación y riesgos recalculados; dictamen. Validación jurídica de la conservación y anonimización del historial de delegaciones mantenida como pendiente.
- **Archivos inspeccionados:** `01-requisitos.md` (v1.2 íntegro), `aprobaciones.md`, `bitacora.md`.
- **Archivos modificados:** `01-requisitos.md`, `aprobaciones.md`, `bitacora.md`. Sin commit. Copia de respaldo de la v1.2 fuera del repositorio (carpeta temporal de la sesión).
- **Comandos ejecutados y resultados:** scripts de edición de texto; verificación mecánica sobre el documento resultante: 186 requisitos (54 RF generales, 72 RD, 25 OFF, 18 PRV, 17 RNF; 169 M, 11 S, 6 P); 23 con `COND`; 14 RT; matriz de 41 filas; 72 casos límite; 51 historias; 22 riesgos; 0 identificadores citados sin definir; 0 tablas con columnas irregulares; 0 requisitos M sin criterio o prueba propios. No se ejecutó ninguna prueba del producto (no hay código).
- **Lo que NO se hizo:** Fase 2; código; esquema; migraciones; dependencias; cambios a la skill; commit, push, PR o merge; verificación de fuentes externas, costos, proveedores o legislación; no se decidió ninguna de las 18 preguntas abiertas; no se declaró aprobada la Fase 1.
- **Riesgos o bloqueos:** 18 asuntos abiertos (ninguno bloquea F1; Q-14 y Q-40 (d) antes de F3); persona responsable de evidencias por designar; evaluación jurídica y verificación de proveedores pendientes; riesgo residual RK-15 aceptado por el usuario; RK-22 (bloqueo operativo por conflicto sin autoridad alterna); identificación indirecta de menores sin resolver (RK-20); estimación de confianza baja a media.
- **Estado de la fase:** PENDIENTE DE APROBACIÓN. Dictamen técnico: APROBABLE CON CONDICIONES (recomendación).
- **Pregunta para siguiente aprobación:** ¿Apruebas los entregables de la Fase 1 (v1.3) y autorizas iniciar la Fase 2: arquitectura y amenazas?

---

## 2026-11-15 — Fase 1 (consolidación de Q-36, Q-37 y D-1…D-12; v1.2)

- **Autorización recibida:** mensaje «CONSOLIDACIÓN DE Q-36, Q-37 Y D-1 A D-12» del 2026-11-15: consolidar sobre la v1.1 las decisiones Q-36 (opción A) y Q-37 (opción B), confirmar D-1…D-11 con sus precisiones y registrar D-12 como información; verificar consistencia, revisar pendientes y emitir dictamen. Autoriza solo la actualización documental de `01-requisitos.md`, `aprobaciones.md` y `bitacora.md`. Declara que la Fase 1 sigue pendiente de aprobación y que la Fase 2 no está autorizada.
- **Alcance permitido:** solo documentación. Sin código, esquemas, migraciones, dependencias, cambios a la skill, diseño de arquitectura de Fase 2, commit, push, pull request, merge, cambios en `main` ni despliegues.
- **Trabajos realizados:** `01-requisitos.md` pasa a v1.2 sin descartar la v1.1: Q-36 y Q-37 `CONF` (§2.1b; RF-ACC-06, RF-EQP-01, nuevo RF-EQP-06, §9.4 fila 11 tachada); D-1…D-11 confirmadas y D-12 como información (§2.2, con la columna «texto v1.1» como historial); **D-5** sustituye la definición anterior (`finalizado` ≠ `archivado`; RD-RES-06, RF-TOR-02, matriz 14/27/37, CL-34, CL-44); **D-6** (toda inscripción con posible duplicado queda pendiente), **D-7** (plazo informado, avisos, constancia mínima, sin garantía), **D-8** (nuevo OFF-24: exportación limitada; eventos solo con divulgación autorizada), **D-9** (OFF-20 solo operaciones rechazadas o en conflicto recibidas, por torneo), **D-10** (PRV-15, PRV-17), **D-11** (OFF-22: se rechaza toda operación con autorización revocada); nuevos RT-12 y RT-13, PRV-18, matriz de 37 acciones, CL-41…CL-47, RK-20 y RK-21, §13 reorganizada con clasificación de pendientes, §14.7, estimación (117–178 días-persona; probable ≈ 144) y dictamen. Q-38 (c) resuelto por D-3; Q-40 reformulado; nuevo Q-43.
- **Archivos inspeccionados:** `01-requisitos.md` (v1.1 íntegro), `aprobaciones.md`, `bitacora.md`.
- **Archivos modificados:** `01-requisitos.md`, `aprobaciones.md`, `bitacora.md`. Sin commit.
- **Comandos ejecutados y resultados:** scripts de edición de texto; verificación mecánica sobre el documento: 0 identificadores citados sin definir; conteos por prefijo (52 RF generales, 69 RD, 24 OFF, 18 PRV, 17 RNF, 13 RT, 37 acciones de matriz, 47 CL, 46 HU, 21 RK); requisitos sin prueba propia: RF-TOR-03 (M, remite a RD-REG) y tres de prioridad P; revisión de frases desactualizadas (solo quedan referencias históricas). No se ejecutó ninguna prueba del producto (no hay código).
- **Lo que NO se hizo:** Fase 2; código; esquema; migraciones; dependencias; cambios a la skill; commit, push, pull request o merge; verificación de fuentes externas, costos, proveedores o legislación; no se resolvió ningún asunto abierto en nombre del usuario; no se convirtió D-12 en requisito técnico ni se inventaron métodos, herramientas o personas actuales.
- **Riesgos o bloqueos:** 20 asuntos abiertos (ninguno bloquea F1; Q-38 y Q-39 recomendados antes de F2; Q-14 y Q-40 (d) antes de F3); persona responsable de evidencias por designar; evaluación jurídica y verificación de proveedores pendientes; identificación indirecta de menores sin resolver (RK-20); eventos de capturas rechazadas no recuperables (RK-15); estimación de confianza baja a media.
- **Estado de la fase:** PENDIENTE DE APROBACIÓN. Dictamen técnico: APROBABLE CON CONDICIONES (recomendación).
- **Pregunta para siguiente aprobación:** ¿Apruebas los entregables de la Fase 1 (v1.2) y autorizas iniciar la Fase 2: arquitectura y amenazas? (Antes, idealmente: decidir Q-38 y Q-39.)

---

## 2026-11-15 — Fase 1 (consolidación de las 14 decisiones bloqueantes; v1.1)

- **Autorización recibida:** mensaje «CONSOLIDACIÓN DEFINITIVA DE LAS 14 DECISIONES BLOQUEANTES DE FASE 1» del 2026-11-15: incorporar las 14 decisiones, verificar consecuencias y detectar contradicciones; actualizar solo `01-requisitos.md`, `aprobaciones.md` y `bitacora.md`; informe de validación y dictamen. Declara expresamente que no aprueba la Fase 1 ni autoriza la Fase 2.
- **Alcance permitido:** solo documentación. Sin código, esquemas, migraciones, dependencias, cambios a la skill, Fase 2, commit, push, merge ni despliegues.
- **Trabajos realizados:** `01-requisitos.md` pasa a v1.1: 14 decisiones incorporadas como `CONF` (§2.1) con historial de sustituciones (§0.4); requisitos, historias, matriz de permisos (35 acciones, nuevo §7.4), reglas (RD-JUG-02 rehecho; nuevas RD-JUG-08/09/10), modo sin conexión (§9.4 y §9.5 conservadas íntegras y confirmadas; OFF-02, 10, 11, 17, 20, 21, 22 ajustadas; nuevo OFF-23), privacidad (PRV-05, 08, 13, 15 ajustadas; nuevo PRV-17), RNF-03, RT-01/04/05/06 (nuevo RT-11), casos límite CL-35…CL-40, §13 reestructurada, trazabilidad §14.6, estimación (111–170 días-persona; probable ≈ 138), riesgos RK-15…RK-19, dictamen técnico §17.4. Detectadas 14 contradicciones o ambigüedades (V-1…V-14) y corregidas. Nuevos asuntos derivados Q-36…Q-42 (**Q-36 y Q-37 bloquean la Fase 2**) y consecuencias derivadas D-1…D-12 por confirmar.
- **Archivos inspeccionados:** `01-requisitos.md` (v1.0, íntegro), `aprobaciones.md`, `bitacora.md`.
- **Archivos modificados:** `01-requisitos.md`, `aprobaciones.md`, `bitacora.md`. Sin commit.
- **Comandos ejecutados y resultados:** scripts de edición de texto; verificación mecánica sobre el documento: 0 identificadores citados sin definir; conteos por prefijo (51 RF generales, 69 RD, 23 OFF, 17 PRV, 17 RNF, 11 RT, 35 acciones de matriz, 40 CL, 45 HU, 19 RK); 35 filas de decisión en §13; único requisito M sin prueba propia: RF-TOR-03 (remite a RD-REG). No se ejecutó ninguna prueba del producto (no hay código).
- **Lo que NO se hizo:** Fase 2; código; esquema; migraciones; dependencias; cambios a la skill; commit, push o merge; verificación de fuentes externas, costos, proveedores o legislación; no se decidió ninguna pregunta abierta en nombre del usuario; no se cambió ninguna decisión confirmada.
- **Riesgos o bloqueos:** Q-36 y Q-37 bloquean la Fase 2; 21 decisiones abiertas; persona responsable de evidencias por designar; evaluación jurídica y verificación de proveedores pendientes (condicionan el uso de datos reales); estimación de confianza baja a media.
- **Estado de la fase:** PENDIENTE DE APROBACIÓN. Dictamen técnico: APROBABLE CON CONDICIONES (recomendación).
- **Pregunta para siguiente aprobación:** ¿Apruebas los entregables de la Fase 1 (v1.1), confirmas D-1…D-12, decides Q-36 y Q-37 y autorizas iniciar la Fase 2: arquitectura y amenazas?

---

## 2026-11-15 — Fase 1 (requisitos y MVP ampliado)

- **Autorización recibida:** mensaje «AUTORIZACIÓN FORMAL DE FASE 1» del 2026-11-15: iniciar exclusivamente la Fase 1; la Fase 0 aprobada es base vinculante; entregables en `docs/torneo-campeones/`; marcar la Fase 1 PENDIENTE DE APROBACIÓN y detenerse.
- **Alcance permitido:** análisis y documentación. Sin código, esquemas, migraciones, dependencias, cambios a la skill, Fase 2, commit, push, merge ni despliegues.
- **Trabajos realizados:** redacción de `01-requisitos.md` v1.0 (borrador): resumen, base vinculante, MVP vs posterior, 42 historias (5 perfiles), requisitos generales (acceso, torneos, equipos, público, panel, auditoría, PWA), matriz de permisos v2 (34 acciones, 10 restricciones, 10 actores de prueba), 66 reglas deportivas (calendario, reglamento, tabla y desempates, W.O., jugadores, estados, eventos), 22 requisitos de captura sin conexión con política de conflictos y matriz de publicación, 16 de privacidad, 17 no funcionales, 34 casos límite, 28 decisiones abiertas con opciones y recomendación (Q-02a/b/c, Q-08…Q-16, Q-20…Q-25, nuevas Q-26…Q-35), trazabilidad (decisiones, N-01…N-13, H/C, RF y RP provisionales), estimación (107–163 días-persona; probable ≈ 134), riesgos RK-01…RK-14. Actualizados `aprobaciones.md` y esta bitácora.
- **Archivos inspeccionados:** `00-descubrimiento.md`, `aprobaciones.md`, `bitacora.md`; `references/puertas-de-aprobacion.md`, `qa.md`, `seguridad.md`, `puntos-a-validar.md`, `mapa-informe.md`; `templates/brief-proyecto.md`; informe PDF pp. 13–15 y 22–23 (texto extraído con `pdftotext`).
- **Archivos creados/modificados:** creado `01-requisitos.md`; modificados `aprobaciones.md` y `bitacora.md`. Sin commit.
- **Comandos ejecutados y resultados:** lectura de archivos; `pdftotext` sobre pp. 13–15 y 22–24 (salida a pantalla); `git status`/`git log`/`git branch` de lectura; verificación mecánica de que todos los identificadores citados en `01-requisitos.md` están definidos (0 faltantes) y conteo de requisitos por prefijo. No se ejecutó ninguna prueba del producto (no hay código).
- **Lo que NO se hizo:** Fase 2; código; esquema; migraciones; dependencias; cambios a la skill; commit, push o merge; verificación de fuentes externas, costos, límites de proveedores o legislación; ninguna decisión de las 28 abiertas se tomó por el usuario; no se leyeron `arquitectura.md`, `ui-ux.md` ni `direccion-visual-profesional.md`.
- **Riesgos o bloqueos:** 14 decisiones bloquean la Fase 2 (§13); 36 requisitos/celdas dependen de decisiones `COND`; revisión jurídica y verificación de proveedores pendientes; estimación inicial de confianza baja a media; complejidad del modo sin conexión (RK-02) y comportamiento de iOS (RK-11).
- **Estado de la fase:** PENDIENTE DE APROBACIÓN.
- **Pregunta para siguiente aprobación:** ¿Apruebas los entregables de la Fase 1 y autorizas iniciar la Fase 2: arquitectura y amenazas? (Antes: decidir las preguntas bloqueantes de §13.)

---

## 2026-11-15 — Fase 0 (cierre definitivo, Q-19)

- **Autorización recibida:** mensaje «CIERRE DEFINITIVO DE FASE 0» del 2026-11-15: Q-19 aprobada; actualizar exclusivamente `00-descubrimiento.md`, `aprobaciones.md` y `bitacora.md`; commit y push solo de esos tres archivos a `claude/gracious-hawking-8knsb7`, con mensaje `docs: finalize phase 0 decisions and approval`; sin Pull Request, sin tocar `main`, sin Fase 1.
- **Alcance permitido:** edición de esos tres documentos y el commit/push indicado tras las verificaciones.
- **Trabajos realizados:** Q-19 registrada como confirmada (§19 de `00-descubrimiento.md`, v1.3): dueño único para organizadores; organizador invita y administra editores y delegados solo en sus torneos; editor y delegado no invitan; restricciones 1–6 incluida auditoría y verificación en backend y base de datos. Estados actualizados en el encabezado, §14, §17 y §18. `aprobaciones.md`: Fase 0 APROBADA, Fases 1 a 8 y módulos 5A–5G NO INICIADOS. Coherencia Q-01…Q-07 y Q-17…Q-19 verificada (§19.4); no se detectaron contradicciones.
- **Archivos creados/modificados:** `00-descubrimiento.md`, `aprobaciones.md`, `bitacora.md`.
- **Comandos ejecutados:** ediciones; `git status`, `git branch --show-current`, `git diff --name-status`; commit y push (resultado y hash: ver mensaje final de la sesión y `git log`).
- **Lo que NO se hizo:** Fase 1; código; configuraciones; migraciones; dependencias; despliegues; Pull Request; merge; cambios en `main`; no se cambió ninguna decisión [CONF].
- **Riesgos o bloqueos:** estimación de duración desactualizada; revisión jurídica y verificación oficial de costos como puertas previas al lanzamiento; Q-08…Q-16 y Q-20…Q-25 para especificación posterior.
- **Estado de la fase:** Fase 0 APROBADA. Sin fase en curso.
- **Pregunta para siguiente aprobación:** se requiere autorización explícita para iniciar la Fase 1.

---

## 2026-11-15 — Fase 0 (cierre)

- **Autorización recibida:** mensaje «CIERRE DE FASE 0» del 2026-11-15: apruebo los entregables v1.1 con aclaraciones; «Autorizo registrar la Fase 0 como APROBADA, dejando Q-19 expresamente pendiente de confirmación…»; «No autorizo todavía iniciar la Fase 1»; no autoriza commit, push, merge, migraciones, dependencias ni despliegues.
- **Alcance permitido:** solo actualizar la documentación necesaria para registrar el cierre.
- **Trabajos realizados:** `00-descubrimiento.md` pasa a v1.2 con §18 (cierre, Q-17, Q-18, Q-19 pendiente, condiciones) y estados actualizados en T-01 y Q-17…Q-25; `aprobaciones.md` registra la Fase 0 como APROBADA y deja las demás sin iniciar; esta entrada.
- **Archivos inspeccionados:** `aprobaciones.md`, `00-descubrimiento.md`, `bitacora.md`.
- **Archivos creados/modificados:** los tres anteriores, **sin commit**.
- **Comandos ejecutados y resultados:** solo ediciones de archivos y `git status` de lectura.
- **Lo que NO se hizo:** Fase 1; código; migraciones; dependencias; commit, push o merge; no se fijó Q-19 como decisión; no se cambió ninguna decisión [CONF].
- **Riesgos o bloqueos:** Q-19 pendiente (condiciona la matriz de permisos de la Fase 1); estimación de duración desactualizada; revisión jurídica y verificación oficial de costos como puertas previas al lanzamiento.
- **Estado de la fase:** APROBADA. Sin fase en curso.
- **Pregunta para siguiente aprobación:** se requiere autorización explícita para iniciar la Fase 1 (y confirmar Q-19 antes de fijar la matriz de permisos).

---

## 2026-11-15 — Fase 0 (actualización v1.1: decisiones Q-01…Q-07)

- **Autorización recibida:** mensaje «CONSOLIDACIÓN DE DECISIONES DE FASE 0» del 2026-11-15: continuar exclusivamente en la Fase 0, incorporar las decisiones, actualizar `00-descubrimiento.md` y `bitacora.md`, mantener coherente el registro de aprobaciones, identificar requisitos para la Fase 1 y presentar la matriz de trazabilidad.
- **Alcance permitido:** solo edición de documentación de la Fase 0. Sin código ni configuración, migraciones, dependencias, Fase 1, aprobación automática de la Fase 0, commit, push ni merge.
- **Trabajos realizados:**
  - `00-descubrimiento.md` pasa a v1.1: etiquetas nuevas [CONF], [DER], [REC], [PEND]; §12 con las decisiones Q-01…Q-07 transcritas sin alterar su sentido; §13 con el efecto sobre secciones previas; §14 con tensiones T-01…T-06 y preguntas Q-17…Q-25; §15 con requisitos que necesitan especificación en Fase 1 (modo sin conexión, empates eliminatorios, menores); §16 con la matriz de trazabilidad (RP-01…RP-36); §17 con evaluación de criterios y recomendación.
  - `aprobaciones.md`: se añadió el registro de eventos y el estado de los cambios sin commit; la Fase 0 permanece PENDIENTE DE APROBACIÓN.
  - Secciones antiguas conservadas como historial, con notas de superación (§8.1, §9.2, §10).
- **Archivos inspeccionados:** `00-descubrimiento.md`, `aprobaciones.md`, `bitacora.md`.
- **Archivos creados/modificados:** los tres anteriores, sin commit.
- **Comandos ejecutados y resultados:** ninguno con efecto; solo edición de archivos.
- **Lo que NO se hizo:** no se verificó ninguna referencia jurídica ni fuente externa (las referencias legales de Costa Rica están marcadas [PEND-JUR]); no se alteró ninguna decisión [CONF]; no se aprobó la fase; no se inició la Fase 1; no hubo commit, push ni merge.
- **Riesgos o bloqueos:** tensiones T-01 (Q-03 vs Q-04) y T-02 (esfuerzo del MVP ampliado con modo sin conexión); preguntas Q-17, Q-18 y Q-19 deben resolverse al comenzar la Fase 1.
- **Estado de la fase:** PENDIENTE DE APROBACIÓN.
- **Pregunta para siguiente aprobación:** ¿Apruebas los entregables de la Fase 0 (v1.1) y autorizas iniciar la Fase 1: requisitos y MVP?

---

## 2026-11-15 — Fase 0

- **Autorización recibida:** «Autorizo iniciar exclusivamente la Fase 0: Descubrimiento y validación del informe del proyecto Torneo de Campeones.» (conversación del 2026-11-15)
- **Alcance permitido:** análisis y documentación de la Fase 0; sin código, tablas, migraciones, dependencias, commits, push ni despliegues.
- **Trabajos realizados:** lectura completa del informe (texto extraído); análisis funcional, de roles, de reglas, de arquitectura, de modelo de datos y de seguridad/privacidad; revisión estática del SQL base; recálculo de contrastes WCAG del informe; consolidación de contradicciones, supuestos, riesgos y preguntas; redacción de los documentos de la fase.
- **Archivos inspeccionados:** `.claude/skills/torneo-campeones/references/Informe_Torneo_de_Campeones.pdf`; `references/mapa-informe.md`, `puntos-a-validar.md`, `puertas-de-aprobacion.md`, `seguridad.md`, `arquitectura.md`; las cuatro plantillas de `templates/`; `README.md`; historial de git (solo lectura).
- **Archivos creados/modificados:** creados `docs/torneo-campeones/00-descubrimiento.md`, `aprobaciones.md`, `bitacora.md`. Sin cambios en la skill ni en otros archivos del repositorio. Sin commit.
- **Comandos ejecutados y resultados:**
  - `pdfinfo` → 30 páginas, título «Torneo de Campeones · Informe de investigación».
  - `pdftotext -layout` → 1 459 líneas de texto extraídas (en la carpeta temporal de la sesión, fuera del repositorio).
  - `pdfimages -list` → una imagen en la portada; las figuras 1 y 2 se leyeron solo como texto.
  - Cálculo de contrastes WCAG con Python (stdlib): los seis pares declarados en el informe coinciden (17,2; 12,2; 9,4; 7,5; 7,3; 5,9); verde sobre tarjeta 7,8:1.
  - `git log`, `git branch --show-current`: rama `claude/gracious-hawking-8knsb7`.
- **Lo que NO se hizo:** no se ejecutó ni validó el SQL; no se verificaron fuentes externas; no se inspeccionaron visualmente las páginas; no se leyeron `ui-ux.md`, `qa.md` ni `direccion-visual-profesional.md` (corresponden a fases posteriores); no se creó código, esquema ni migraciones; no se instalaron dependencias; no se hizo commit, push ni despliegue.
- **Riesgos o bloqueos:** ver `00-descubrimiento.md` §7–§8. Siete preguntas bloqueantes (Q-01…Q-07) sin respuesta.
- **Estado de la fase:** PENDIENTE DE APROBACIÓN.
- **Pregunta para siguiente aprobación:** ¿Apruebas los entregables de la Fase 0 y autorizas iniciar la Fase 1: requisitos y MVP?
