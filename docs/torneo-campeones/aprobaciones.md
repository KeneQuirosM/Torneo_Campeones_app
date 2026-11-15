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
| 1 | Requisitos y MVP | NO INICIADA | — | — | — | — |
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

- **Fase:** **Fase 0 APROBADA** (2026-11-15). Ninguna otra fase está aprobada ni autorizada.
- **Permiso específico recibido (cita breve):** «Autorizo registrar la Fase 0 como APROBADA, dejando Q-19 expresamente pendiente de confirmación y Q-08 a Q-16 y Q-20 a Q-25 como asuntos de especificación posterior» y «No autorizo todavía iniciar la Fase 1.» (2026-11-15).
- **Ubicación de evidencia:** conversación de la sesión del 2026-11-15.
- **Alcance autorizado:** análisis y documentación de la Fase 0 (cerrada). Para la Fase 1: **ninguno**.
- **Acciones críticas autorizadas por separado:** commit y push de v1.0 de los tres documentos (ya ejecutados, commit `9f54e6d`). Ninguna otra; commit, push, merge, migraciones, dependencias y despliegues están expresamente **no autorizados** tras el cierre.
- **Condiciones registradas por el usuario para fases siguientes:** (1) revisión jurídica de privacidad y menores obligatoria antes del lanzamiento; (2) condiciones comerciales, costos y restricciones de infraestructura verificados con fuentes oficiales; (3) estimación de duración actualizada para el MVP ampliado con modo offline; (4) ninguna recomendación técnica sustituye decisiones aprobadas.

## Eventos registrados

| Fecha | Evento | Evidencia | ¿Aprueba una fase? |
|---|---|---|---|
| 2026-11-15 | Autorización de inicio de la Fase 0 | Mensaje «Autorizo iniciar exclusivamente la Fase 0…» | No |
| 2026-11-15 | Autorización de commit y push de los tres documentos a la rama `claude/gracious-hawking-8knsb7`, sin merge a `main`. Ejecutado: commit `9f54e6d92d8d06dab56e02d937c19507c744634d` | Mensaje «Autorizo realizar commit y push exclusivamente de los siguientes archivos…» | No (el mensaje lo dice expresamente) |
| 2026-11-15 | Consolidación de decisiones Q-01…Q-07 y autorización para actualizar los documentos de la Fase 0 | Mensaje «CONSOLIDACIÓN DE DECISIONES DE FASE 0» | No. Solo registra decisiones; el usuario pidió explícitamente no aprobar la fase automáticamente. |
| 2026-11-15 | Cierre de la Fase 0: aprobación de los entregables v1.1 con aclaraciones (validez del informe, Q-17, Q-18, Q-19 como propuesta pendiente, condiciones). Se prohíbe iniciar la Fase 1 y hacer commit, push, merge, migraciones, dependencias o despliegues | Mensaje «CIERRE DE FASE 0» | **Sí: Fase 0 APROBADA. Fase 1 NO autorizada.** |
| 2026-11-15 | Cierre definitivo: Q-19 confirmada (dueño único para organizadores; organizadores solo editores y delegados de sus torneos). Autoriza actualizar los tres documentos y commit/push de esos tres archivos a la rama indicada | Mensaje «CIERRE DEFINITIVO DE FASE 0» | Mantiene Fase 0 APROBADA. Fase 1 NO autorizada. |

**Git:** commit y push de estos tres archivos autorizados el 2026-11-15 («CIERRE DEFINITIVO DE FASE 0»), exclusivamente a `claude/gracious-hawking-8knsb7`, sin Pull Request ni cambios en `main`. El hash se registra en `bitacora.md`.

**Decisiones confirmadas por el usuario:** Q-01 a Q-07 (§12), Q-17 y Q-18 (§18) y Q-19 (§19) en `00-descubrimiento.md`.

## Próxima pregunta pendiente

Fase 0 cerrada. **Sin fase en curso.** Para continuar se necesita una autorización explícita, por ejemplo «Autorizo iniciar la Fase 1: requisitos y MVP». Q-19 ya está confirmada.

> `estado` y `continuar` no responden esta pregunta ni autorizan nada.
