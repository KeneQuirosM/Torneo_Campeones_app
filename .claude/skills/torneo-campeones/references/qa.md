# Matriz de aceptación y verificación

No marques como probado aquello que solo inspeccionaste estáticamente o asumiste.

| Área | Comprobaciones mínimas |
|---|---|
| Función principal | Caso feliz completo y persistencia real cuando aplica |
| Validación | Vacíos, formatos inválidos, límites y doble envío |
| Permisos | Sesión inexistente, rol sin privilegios, acceso a recurso ajeno |
| Estado de red | Carga, error 500, desconexión y recuperación cuando aplica |
| UI móvil | Navegación, botones, formularios, tablas, scroll horizontal |
| Accesibilidad | Teclado, foco, labels, contraste, mensajes de error |
| Regresión | Los módulos adyacentes siguen funcionando |
| Calidad técnica | Tests existentes, build, lint y typecheck disponibles |
| Secretos | Sin tokens/credenciales en frontend o repositorio |

## Estrategia de pruebas

1. Lee primero los scripts disponibles; no inventes comandos de prueba.
2. Prioriza tests unitarios para reglas complejas, integración para API y flujos E2E para el recorrido crítico.
3. Para problemas reportados, reproduce si es posible y añade una regresión cuando aporte valor.
4. Si hay navegador disponible, verifica realmente los controles en diferentes tamaños; no uses una captura estática como sustituto del funcionamiento.
5. No silencies, saltes ni reescribas tests solo para obtener verde; explica cualquier fallo preexistente.
6. Diferencia claramente **pasó**, **falló**, **no se ejecutó** y **bloqueado**.

## Formato de entrega recomendado

- Alcance implementado.
- Archivos y endpoints modificados.
- Evidencia: comando ejecutado + resultado, o prueba manual + pasos y resultado.
- Riesgos abiertos y supuestos.
- Próxima acción concreta.
