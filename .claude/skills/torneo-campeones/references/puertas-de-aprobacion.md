# Puertas de aprobación: especificación operativa

## Modelo de estado

Cada fase puede tener uno de estos estados:

- **NO INICIADA:** todavía no hay autorización para trabajarla.
- **AUTORIZADA:** el usuario autorizó explícitamente comenzarla y se trabaja solo en su alcance.
- **PENDIENTE DE APROBACIÓN:** entregables de la fase presentados; la siguiente fase no ha comenzado.
- **EN REVISIÓN:** el usuario solicitó ajustes o rechazó entregables; conservar alcance.
- **APROBADA:** el usuario aprobó el resultado de esa fase de manera inequívoca.
- **BLOQUEADA:** existe riesgo, dato, decisión o restricción que impide completar la fase responsablemente.

**No equipares «archivos terminados» con «fase aprobada».** Las aprobaciones son humanas, no deducidas por el asistente.

## Invariantes

1. Se requiere un acto expreso del usuario para iniciar CADA fase (0 a 8) y CADA módulo de la Fase 5 (5A a 5G).
2. Antes de la autorización de la Fase 0, no se usan herramientas para examinar/modificar archivos del proyecto (salvo leer la solicitud escrita en chat).
3. El primer turno de cada fase enuncia su alcance. Ningún resultado autoriza a añadir tareas no descritas.
4. Una vez presentado el cierre de la fase, no se inicia la siguiente sin respuesta.
5. Los comandos con impacto externo, datos o Git requieren permisos puntuales incluso dentro de la fase autorizada.
6. Documentar un permiso no lo crea. Solo puede registrarse si consta de un mensaje inequívoco del usuario.
7. Un usuario puede ordenar trabajar más rápido; responde con propuesta de compresión, pero **no elimina** aprobaciones individuales.
8. Una aprobación de herramientas del terminal no sustituye la aprobación del plan.
9. **Las operaciones informativas nunca autorizan.** `estado`, `continuar`, `revisar fase N`, un saludo, un «ok» ambiguo o un «sigue» no equivalen a autorizar una fase ni un módulo. Solo cuenta un mensaje que nombre la fase o el módulo (p. ej. «Autorizo la Fase 1» o «Autorizo el módulo 5A»).
10. **`estado` es de solo lectura:** informa la fase y el módulo vigentes, su estado y la última aprobación comprobable. No escribe ni modifica archivos, no cambia estados, no ejecuta comandos con efectos y no inicia trabajo. Puede leer `docs/torneo-campeones/aprobaciones.md` si existe; antes de la autorización de la Fase 0 solo responde con lo que consta en la conversación.
11. **`continuar` sin una fase o módulo autorizado vigente** solo repite el estado y formula la pregunta de aprobación pendiente. Dentro de una fase ya autorizada, retoma únicamente el alcance de esa fase y se detiene al cierre.

## Numeración vigente

Es la misma que usa `SKILL.md`: **Fase 0** descubrimiento y validación del informe; **1** requisitos y MVP; **2** arquitectura y amenazas; **3** UX/UI; **4** diseño técnico; **5** MVP por módulos **5A–5G** (cada módulo se autoriza por separado); **6** formatos y capacidades adicionales; **7** QA, endurecimiento y piloto; **8** entrega y operación.

## Ejemplo 1: inicio

Usuario: «/torneo-campeones iniciar»

Claude: «Objetivo: ... Fases: 0 Descubrimiento; 1 Requisitos; ... ¿Autorizas iniciar la Fase 0: validación del informe y descubrimiento?»

Usuario: «Sí, autorizo la Fase 0»

Claude: examina el contexto, completa el descubrimiento y entrega hallazgos. «¿Apruebas la Fase 0 y autorizas la Fase 1?» **Se detiene.**

Usuario: «Fase 0 aprobada, autorizo la Fase 1»

Claude: ejecuta solo la Fase 1. **Se detiene** al finalizar.

## Ejemplo 2: usuario dice «hazlo todo de una vez»

Responder: «Para respetar tu modo de trabajo por fases, puedo proponer agrupar entregables, pero necesito que apruebes cada fase. ¿Autorizas la Fase 0?» No comenzar código.

## Ejemplo 3: cambios dentro de la fase

Usuario: «El modelo de permisos de la Fase 4 debe incluir supervisores».

Claude: revisa **solo** el modelo de permisos dentro de la Fase 4; presenta el diseño ajustado; solicita aprobación. Si se requiere además modificar tablas productivas, separa diseño de ejecución.

## Ejemplo 4: Fase 5 por módulos

Plan aprobado en la Fase 4: 5A infraestructura y autenticación, 5B torneos y equipos, 5C roles/RLS (y así hasta 5G).

Usuario autoriza 5A; Claude implementa y prueba 5A y se detiene. Para iniciar 5B, solicita autorización **de 5B**, aunque la Fase 5 haya sido aceptada como estructura general.

## Ejemplo 5: publicación

La aprobación de la Fase 8 autoriza preparar release notes y revisar configuración. `git push`, migración de base de datos, creación de recursos de pago y despliegue **siguen necesitando permisos separados y específicos**, con destino, efecto y reversión.

## Ejemplo 6: operaciones informativas

Usuario: «/torneo-campeones estado»

Claude: informa la última aprobación comprobable y la fase o módulo pendiente, sin modificar nada ni iniciar trabajo.

Usuario: «/torneo-campeones continuar» (con la Fase 1 en PENDIENTE DE APROBACIÓN)

Claude: no inicia la Fase 2; recuerda que la Fase 1 espera aprobación y repite la pregunta. Solo avanza con «Apruebo la Fase 1 y autorizo la Fase 2».

## Registro sugerido para una aprobación

| Fase | Estado | Fecha | Autorización humana textual | Entregable aprobado | Riesgos abiertos |
|---|---|---|---|---|---|
| 0 | APROBADA | [fecha] | «Apruebo la Fase 0 y autorizo la Fase 1» | `00-descubrimiento.md` v1 | [riesgos] |

Si la evidencia de autorización no está accesible en la sesión, marca «por confirmar»; no rellenes una cita inventada.

## Plantilla de primer mensaje

> Entendí que quieres [objetivo]. Tipo: [nuevo/existente]. Propongo [fases pertinentes]. Durante el descubrimiento revisaré [áreas] sin cambiar código. No implementaré ni publicaré nada sin aprobaciones posteriores. **¿Autorizas iniciar la Fase 0: validación del informe y descubrimiento?**

## Plantilla de fin de fase

> **Fase 2 — Arquitectura y amenazas | Pendiente de aprobación**. Entregables: [lista]. Decisiones: [resumen]. Evidencia: [archivos revisados, sin código modificado]. Riesgos: [lista]. **¿Apruebas la arquitectura y autorizas iniciar la Fase 3: UX/UI?**
