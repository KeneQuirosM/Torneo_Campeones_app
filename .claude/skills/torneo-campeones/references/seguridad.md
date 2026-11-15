# Controles mínimos de seguridad por riesgo

Esta guía no reemplaza una auditoría de seguridad formal. Adapta cada control a arquitectura y exposición real.

## Identidad y autorización

- Distingue autenticación (quién es) y autorización (qué puede hacer).
- Valida permisos del lado servidor o base de datos en cada operación sensible, también cuando el frontend oculte una acción.
- Aplica mínimo privilegio y evita depender de identificadores enviados por el cliente como prueba de propiedad.
- Gestiona expiración de sesión, 401 y 403 de manera diferenciada.

## Datos y entradas

- Valida tipo, longitud, rango y formato en límites externos. Escapa contenido según contexto y evita inyección de SQL/HTML.
- Usa consultas parametrizadas o SDKs confiables; nunca concatenes SQL con entrada no confiable.
- Evita retornar errores internos, stack traces, secretos y datos privados a clientes o logs públicos.
- Protege uploads si hay archivos: tipo, tamaño, autorización y almacenamiento.

## Credenciales y configuración

- Guarda secrets en variables de entorno del servidor; usa `.env.example` con claves de ejemplo vacías.
- `service_role` de Supabase debe vivir solo del lado servidor; una clave publicable/anon no sustituye RLS.
- Configura CORS de acuerdo con los orígenes legítimos; no lo abras sin necesidad.
- Revisa CSRF si la aplicación usa cookies autenticadas y operaciones con efectos.

## Integridad y operaciones

- Usa restricciones de base de datos e idealmente transacciones para operaciones que deben ser atómicas.
- Considera límites de frecuencia para autenticación o endpoints abusables.
- Nunca hagas migraciones destructivas en producción, ni cambies acceso a datos compartidos, sin autorización y plan de reversión.

## Comprobación

Prueba al menos: acceso no autenticado, rol insuficiente, recurso de otro usuario, datos inválidos, credenciales ausentes, error de servicio y respuestas que no filtren datos internos. Reporta lo que no se pudo probar.
