# Bitácora del desarrollo — Torneo de Campeones

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
