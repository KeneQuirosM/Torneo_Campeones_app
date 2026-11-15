# Referencia de arquitectura para web apps

Usa estos criterios al decidir estructura o tecnología. No los conviertas en una lista de librerías obligatorias.

## Elegir stack

- **Proyecto existente:** conserva lenguaje, framework, gestor de paquetes, convenciones, despliegue y estructura cuando cumplen el propósito. Justifica cada dependencia nueva.
- **Nuevo sitio sencillo o herramienta interna:** HTML/CSS/JS modular puede ser suficiente; no impongas SPA por costumbre.
- **Producto con muchas vistas, estados y componentes compartidos:** un framework moderno puede reducir complejidad; selecciona uno solo si las necesidades lo justifican.
- **Backend:** separa endpoints, validación/autorización y persistencia. No dupliques reglas de negocio entre cliente y servidor como fuente de verdad.
- **Datos:** modela entidades, claves, restricciones, índices necesarios, transacciones y políticas de acceso según casos reales.
- **Despliegue:** adapta el diseño al proveedor elegido, sin dar por hecho que existe entorno de producción.

## Contratos y flujos

Para cada funcionalidad crítica, identifica: actor, evento, reglas, entradas válidas, respuestas correctas, errores previsibles, roles autorizados, persistencia y reversibilidad. Las API deben tener respuestas y códigos de error consistentes. Valida los datos en el límite de confianza.

## Vercel + Supabase + JavaScript

Si el proyecto ya usa ese stack:

- Respeta el flujo navegador -> `/api` serverless -> Supabase y verifica autenticación/autorización de cada operación sensible.
- Mantén claves privadas de Supabase únicamente en el servidor. Nunca uses `service_role` como prueba de que un usuario tiene permisos.
- Revisa consulta parametrizada o cliente oficial, integridad del esquema, operaciones idempotentes cuando corresponda y tratamiento de fallos parciales.
- Si el cliente accede directamente a Supabase, verifica RLS, políticas por operación y columnas expuestas. Si la API usa permisos elevados, las políticas RLS no sustituyen validación/autorización propia.
- Verifica compatibilidad con `vercel.json`, variables de entorno, rutas y contratos existentes antes de modificar configuración.

## Evolución del proyecto

- Evita migraciones tecnológicas sin mandato explícito.
- Aplica cambios pequeños por módulo, preserva compatibilidad o documenta ruptura.
- Antes de tocar esquema SQL o permisos, propone el plan de migración y solicita autorización si afecta datos compartidos o producción.
- Para refactorizaciones, prepara tests de comportamiento del flujo existente primero.
- No introduzcas microservicios, colas, cachés distribuidas ni otras infraestructuras complejas sin evidencia de necesidad.
