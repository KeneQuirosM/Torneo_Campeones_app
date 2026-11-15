# Plan y aprobaciones — Torneo de Campeones

> Este archivo registra eventos reales. Una marca del asistente no equivale a aprobación del usuario.

## Alcance general

- **Objetivo:** PWA «Torneo de Campeones» para gestión y consulta pública de ligas, copas y torneos relámpago de fútbol.
- **Tipo:** aplicación nueva
- **Repositorio o carpeta:** `KeneQuirosM/Torneo_Campeones_app` (solo contiene la skill, el informe y `README.md`)
- **Entorno y restricciones:** sesión remota; sin código, sin migraciones, sin instalación de dependencias, sin commits, push ni despliegues.
- **Fases seleccionadas y justificación de las omitidas:** todas (0–8); ninguna omitida por ahora.

## Tabla de control

| Fase | Descripción resumida | Estado | Fecha de autorización | Evidencia textual de autorización | Entregables | Riesgos |
|---|---|---|---|---|---|---|
| 0 | Descubrimiento y validación del informe | **APROBADA** (2026-11-15). Q-19 confirmada el mismo día | 2026-11-15 | Inicio: «Autorizo iniciar exclusivamente la Fase 0…». Aprobación: «apruebo los entregables de la Fase 0 con las siguientes aclaraciones» y «Autorizo registrar la Fase 0 como APROBADA, dejando Q-19 expresamente pendiente de confirmación…» | `docs/torneo-campeones/00-descubrimiento.md` (v1.1), `aprobaciones.md`, `bitacora.md` | Insumos de validación, no decisiones de implementación: H-01…H-13, C-01…C-15 (ver `00-descubrimiento.md` §7, §8). Abiertos: Q-08…Q-16 y Q-20…Q-25 (especificación posterior) |
| 1 | Requisitos y MVP | **PENDIENTE DE APROBACIÓN** (entregables presentados 2026-11-15) | 2026-11-15 | «Autorizo expresamente iniciar exclusivamente la Fase 1: Requisitos funcionales y definición del MVP ampliado.» | `docs/torneo-campeones/01-requisitos.md` (v1.0 borrador), `aprobaciones.md`, `bitacora.md` | 14 decisiones bloquean la Fase 2 (§13); requisitos `COND` no aprobables hasta decidir; revisión jurídica pendiente; estimación inicial de confianza baja a media |
| 2 | Arquitectura y amenazas | NO INICIADA | — | — | — | — |
| 3 | UX/UI | NO INICIADA | — | — | — | — |
| 4 | Diseño técnico | NO INICIADA | — | — | — | — |
| 5A | Módulo: infraestructura local y autenticación privada | NO INICIADA | — | — | — | — |
| 5B | Módulo: torneos, equipos e inscripciones | NO INICIADA | — | — | — | — |
| 5C | Módulo: roles/RLS y administración de accesos | NO INICIADA | — | — | — | — |
| 5D | Módulo: calendario de liga y jornadas | NO INICIADA | — | — | — | — |
| 5E | Módulo: captura y corrección de resultados | NO INICIADA | — | — | — | — |
| 5F | Módulo: tabla y calendario públicos | NO INICIADA | — | — | — | — |
| 5G | Módulo: PWA, red débil, E2E y piloto | NO INICIADA | — | — | — | — |
| 6 | Formatos y capacidades adicionales | NO INICIADA | — | — | — | — |
| 7 | QA, endurecimiento y piloto | NO INICIADA | — | — | — | — |
| 8 | Entrega y operación | NO INICIADA | — | — | — | — |

Los módulos 5A–5G pueden reordenarse solo tras la Fase 4 y con aprobación. Cada módulo requiere su propia autorización.

## Última aprobación comprobable

- **Fase aprobada:** **Fase 0 APROBADA** (2026-11-15). Es la única aprobación comprobable.
- **Fase en curso:** **Fase 1 — PENDIENTE DE APROBACIÓN.** Fue autorizada (no aprobada) el 2026-11-15; sus entregables están presentados y se espera la revisión expresa del usuario. Ninguna otra fase está autorizada.
- **Permiso específico recibido para la Fase 1 (cita breve):** «Autorizo expresamente iniciar exclusivamente la Fase 1: Requisitos funcionales y definición del MVP ampliado.» … «Marcar la Fase 1 como PENDIENTE DE APROBACIÓN y detenerse hasta recibir mi revisión expresa.» (2026-11-15).
- **Ubicación de evidencia:** conversación de la sesión del 2026-11-15.
- **Alcance autorizado para la Fase 1:** análisis y documentación en `docs/torneo-campeones/`. Sin código, esquemas, migraciones, dependencias, cambios a la skill, Fase 2, commit, push, merge ni despliegues.
- **Acciones críticas autorizadas por separado:** ninguna para la Fase 1. (El commit y push de los documentos de la Fase 0 ya se ejecutaron.) Commit, push, merge, migraciones, dependencias y despliegues **no están autorizados**.
- **Condiciones registradas por el usuario para fases siguientes:** (1) revisión jurídica de privacidad y menores obligatoria antes del lanzamiento; (2) condiciones comerciales, costos y restricciones de infraestructura verificados con fuentes oficiales; (3) estimación de duración actualizada para el MVP ampliado con modo offline (presentada en `01-requisitos.md` §15); (4) ninguna recomendación técnica sustituye decisiones aprobadas.

## Eventos registrados

| Fecha | Evento | Evidencia | ¿Aprueba una fase? |
|---|---|---|---|
| 2026-11-15 | Autorización de inicio de la Fase 0 | Mensaje «Autorizo iniciar exclusivamente la Fase 0…» | No |
| 2026-11-15 | Autorización de commit y push de los tres documentos a la rama `claude/gracious-hawking-8knsb7`, sin merge a `main`. Ejecutado: commit `9f54e6d92d8d06dab56e02d937c19507c744634d` | Mensaje «Autorizo realizar commit y push exclusivamente de los siguientes archivos…» | No (el mensaje lo dice expresamente) |
| 2026-11-15 | Consolidación de decisiones Q-01…Q-07 y autorización para actualizar los documentos de la Fase 0 | Mensaje «CONSOLIDACIÓN DE DECISIONES DE FASE 0» | No. Solo registra decisiones; el usuario pidió explícitamente no aprobar la fase automáticamente. |
| 2026-11-15 | Cierre de la Fase 0: aprobación de los entregables v1.1 con aclaraciones (validez del informe, Q-17, Q-18, Q-19 como propuesta pendiente, condiciones). Se prohíbe iniciar la Fase 1 y hacer commit, push, merge, migraciones, dependencias o despliegues | Mensaje «CIERRE DE FASE 0» | **Sí: Fase 0 APROBADA. Fase 1 NO autorizada.** |
| 2026-11-15 | Cierre definitivo: Q-19 confirmada (dueño único para organizadores; organizadores solo editores y delegados de sus torneos). Autoriza actualizar los tres documentos y commit/push de esos tres archivos a la rama indicada | Mensaje «CIERRE DEFINITIVO DE FASE 0» | Mantiene Fase 0 APROBADA. Fase 1 NO autorizada. |
| 2026-11-15 | Autorización formal de la Fase 1 (alcance: requisitos funcionales y no funcionales, historias, matriz de permisos, reglas deportivas, offline, privacidad, MVP vs posterior, preguntas abiertas, trazabilidad, estimación). Restricciones: sin código, esquemas, migraciones, dependencias, cambios a la skill, Fase 2, commit, push, merge ni despliegues | Mensaje «AUTORIZACIÓN FORMAL DE FASE 1» | **Autoriza iniciar la Fase 1; no la aprueba.** |
| 2026-11-15 | Entregables de la Fase 1 presentados (`01-requisitos.md` v1.0). Estado: PENDIENTE DE APROBACIÓN | Resumen de cierre de la Fase 1 | No (a la espera de revisión del usuario) |

**Git:** commit y push de estos tres archivos autorizados el 2026-11-15 («CIERRE DEFINITIVO DE FASE 0»), exclusivamente a `claude/gracious-hawking-8knsb7`, sin Pull Request ni cambios en `main`. El hash se registra en `bitacora.md`.

**Decisiones confirmadas por el usuario:** Q-01 a Q-07 (§12), Q-17 y Q-18 (§18) y Q-19 (§19) en `00-descubrimiento.md`.

## Próxima pregunta pendiente

**Fase 1 en PENDIENTE DE APROBACIÓN.** Se necesita la revisión expresa del usuario: aprobar, pedir cambios o rechazar; y decidir las preguntas abiertas de `01-requisitos.md` §13 (14 bloquean la Fase 2). Solo con una respuesta como «Apruebo la Fase 1 y autorizo la Fase 2» podría iniciar la Fase 2.

> `estado` y `continuar` no responden esta pregunta ni autorizan nada.
