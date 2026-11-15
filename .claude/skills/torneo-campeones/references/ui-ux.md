# Estándar UI/UX

## Identidad y consistencia

Define tokens reutilizables para colores, tipografía, espaciado, radios, bordes y elevación. Usa jerarquía visual clara, grupos funcionales, densidad apropiada y microcopy breve. Los botones primarios deben representar acciones primarias reales. Mantén consistencia entre módulos.

Evita: interfaces clonadas de plantillas genéricas, muchas tarjetas sin utilidad, degradados decorativos indiscriminados, tipografías diminutas, iconos ambiguos y tablas imposibles de usar en celular.

## Flujos y estados

Toda pantalla de datos debe prever: carga inicial, lista vacía, resultados, error recuperable, error de permisos, sesión vencida y actualización. Los formularios requieren etiqueta, ayuda cuando proceda, validación comprensible, prevención de doble envío y feedback de éxito/fallo.

## Responsividad

- **~375 px:** sin scroll horizontal de página; prioriza contenido/acciones esenciales, navegación y tablas transformadas o desplazamiento contenido.
- **~820 px:** adapta columnas, menús y formularios a tablet.
- **~1440 px:** aprovecha el espacio sin estirar formularios o líneas de texto excesivamente.
- No dependas de hover para acciones críticas; considera controles táctiles de tamaño razonable.

## Accesibilidad

Apunta a WCAG 2.2 AA cuando sea viable: navegación teclado, foco visible, semántica de controles, labels, contraste suficiente, textos alternativos pertinentes, mensajes de error anunciables y estados no comunicados únicamente por color. Evalúa con herramienta automática cuando esté disponible, pero no confundas un resultado automático limpio con accesibilidad garantizada.

## Verificación visual

Cuando haya navegador o herramienta de capturas disponible, compara al menos un flujo completo en móvil y escritorio; revisa menú, formularios, tablas, modal, mensajes y estados de carga. Si no es posible inspeccionar visualmente, no afirmes que el diseño fue probado en pantalla.


Para dirección de arte, antipatrones generativos y proceso de revisión visual, leer también `direccion-visual-profesional.md`. UI UX Pro Max es opcional y subordinada a la aprobación del proyecto.
