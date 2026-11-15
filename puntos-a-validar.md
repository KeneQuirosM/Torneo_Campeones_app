# Fase 0 — preguntas y conflictos que impiden asumir el diseño como cerrado

Mantén estas preguntas visibles sin interrogar al usuario todas a la vez. Prioriza las bloqueantes.

1. País de operación, normativa aplicable y responsable de datos (el informe menciona Guatemala; no asumir).
2. Cantidad de torneos, equipos, jugadores, editores, asistentes concurrentes, ubicaciones y calidad de conexión.
3. Reglamento: puntos, desempates, tiempos reglamentarios, categorías, disciplina, penales y formato de relámpagos.
4. Quién puede crear torneos/usuarios, asignar partidos y corregir resultados; matriz Anexo A vs función SQL ejemplo.
5. Necesidad y legitimidad de datos de menores; consentimiento y publicación de nombres/fotos.
6. Si se aporta el mockup interactivo original de 13 pantallas, usarlo como referencia visual, sin afirmar que está adjunto (solo se adjunta el informe PDF).
7. Dominio, logotipo en formato original, correo, branding, presupuesto y cuentas que poseerá la organización.
8. Alcance real del primer piloto, datos históricos y criterios de aceptación concretos.
9. Definir reglas para reabrir partidos, deshacer avances de llaves e idempotencia bajo mala conectividad.
10. Contrastar precios/limitaciones de Vercel, Supabase y compatibilidad PWA antes de elegir planes.

## Riesgos que no se deben esconder

- El SQL mostrado en el informe no constituye una implementación completa de RLS para todas las tablas.
- `report_result` no parece aplicar directamente la condición de partido asignado al editor; requiere revisión y test.
- Integridad intertorneos exige revisar claves compuestas, restricciones y funciones.
- Los límites de Realtime y costos de hosting pueden cambiar.
- El informe es guía de arquitectura e investigación, no sustituto de pruebas reales ni autorización.
