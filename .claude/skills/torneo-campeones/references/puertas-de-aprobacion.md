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

1. Se requiere un acto expreso del usuario para iniciar CADA fase y subfase de implementación.
2. Antes de la autorización de F1, no se usan herramientas para examinar/modificar archivos del proyecto (salvo leer la solicitud escrita en chat).
3. El primer turno de cada fase enuncia su alcance. Ningún resultado autoriza a añadir tareas no descritas.
4. Una vez presentado el cierre de la fase, no se inicia la siguiente sin respuesta.
5. Los comandos con impacto externo, datos o Git requieren permisos puntuales incluso dentro de la fase autorizada.
6. Documentar un permiso no lo crea. Solo puede registrarse si consta de un mensaje inequívoco del usuario.
7. Un usuario puede ordenar trabajar más rápido; responde con propuesta de compresión, pero **no elimina** aprobaciones individuales.
8. Una aprobación de herramientas del terminal no sustituye la aprobación del plan.

## Ejemplo 1: nueva aplicación

Usuario: «/crear-webapp Crea un sistema de inventarios»

Claude: «Objetivo: ... Fases: 1 Descubrimiento; 2 Requisitos; ... ¿Autorizas iniciar Fase 1?»

Usuario: «Sí, inicia F1»

Claude: examina el contexto, completa descubrimiento y entrega hallazgos. «¿Apruebas F1 y autorizas F2?» **Se detiene.**

Usuario: «F1 aprobada, puedes iniciar F2»

Claude: ejecuta solo F2. **Se detiene** al finalizar.

## Ejemplo 2: usuario dice «hazlo todo de una vez»

Responder: «Para respetar tu modo de trabajo por fases, puedo reducir el número de etapas, pero necesito que apruebes cada una. Propongo F1 análisis + F2 diseño + F3 implementación + F4 QA, sin publicación. ¿Autorizas F1?» No comenzar código.

## Ejemplo 3: cambios dentro de la fase

Usuario: «El modelo de permisos de F3 debe incluir supervisores».

Claude: revisa **solo** el modelo de permisos dentro de F3; presenta el diseño ajustado; solicita aprobación. Si se requiere además modificar tablas productivas, separa diseño de ejecución.

## Ejemplo 4: fase 6 por módulos

Plan aprobado: 6A login, 6B inventarios, 6C reportes.

Usuario autoriza 6A; Claude implementa y prueba 6A y se detiene. Para iniciar 6B, solicita autorización **de 6B**, aunque F6 haya sido aceptada como estructura general.

## Ejemplo 5: publicación

La aprobación de F8 autoriza preparar release notes y revisar configuración. `git push`, migración de base de datos, creación de recursos de pago y despliegue **siguen necesitando permisos separados y específicos**, con destino, efecto y reversión.

## Registro sugerido para una aprobación

| Fase | Estado | Fecha | Autorización humana textual | Entregable aprobado | Riesgos abiertos |
|---|---|---|---|---|---|
| 1 | APROBADA | [fecha] | «Apruebo F1 y autorizo F2» | `00-brief.md` v1 | [riesgos] |

Si la evidencia de autorización no está accesible en la sesión, marca «por confirmar»; no rellenes una cita inventada.

## Plantilla de primer mensaje

> Entendí que quieres [objetivo]. Tipo: [nuevo/existente]. Propongo [fases pertinentes]. Durante el descubrimiento revisaré [áreas] sin cambiar código. No implementaré ni publicaré nada sin aprobaciones posteriores. **¿Autorizas iniciar la Fase 1: descubrimiento?**

## Plantilla de fin de fase

> **Fase 3 — Arquitectura | Pendiente de aprobación**. Entregables: [lista]. Decisiones: [resumen]. Evidencia: [archivos revisados, sin código modificado]. Riesgos: [lista]. **¿Apruebas la arquitectura y autorizas iniciar la Fase 4: UX/UI?**
