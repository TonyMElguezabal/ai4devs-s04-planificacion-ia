Aquí tienes esta user story con sus criterios de aceptación:
  Historia 3.1: Filtrar tareas por estado


  Como usuario autenticado, quiero filtrar mis tareas por estado (pendientes, completadas, archivadas), para ver solo las que me interesan en cada momento.


  Criterios de aceptación


  Given que tengo tareas en diferentes estados, When selecciono el filtro "Pendientes", Then solo veo tareas con estado  pending .



  Given que aplico el filtro "Completadas", When se actualiza el listado, Then solo veo tareas con estado  completed .



  Given que aplico el filtro "Archivadas", When se actualiza el listado, Then solo veo tareas con estado  archived .



  Given que aplico un filtro, When cambio a otro, Then el listado se actualiza sin necesidad de recargar la página.



  Given que aplico un filtro y no hay tareas que coincidan, When veo el resultado, Then se muestra un mensaje indicando que no hay tareas en esa categoría.


  Tu tarea: identifica edge cases, supuestos implícitos,
  escenarios faltantes, y dependencias o riesgos no
  mencionados. No reescribas la story, solo lista lo
  que falta o lo que asumiste.
  ~

Análisis de Historia 3.1: Filtrar tareas por estado

🔍 Edge Cases No Cubiertos

1. Acción sobre tareas en estado filtrado
  - ¿Qué pasa si cambio el estado de una tarea mientras el filtro está activo en ese estado? (ej: cambio pending → completed con filtro "Pendientes" activo)
  - ¿Se quita automáticamente del listado o queda ahí?
2. Creación de tareas con filtro activo
  - Si creo una tarea nueva (que nace en pending) mientras el filtro activo es "Completadas", ¿aparece en el listado?
  - ¿O solo se actualiza el listado si la nueva tarea coincide con el filtro?
3. Eliminación de tareas con filtro activo
  - Si elimino una tarea mientras tengo filtro "Pendientes" activo, ¿el listado se actualiza automáticamente o debo recargar?
4. Grandes volúmenes de datos
  - ¿Qué pasa si un usuario tiene 150 tareas pendientes? ¿Hay paginación o scroll infinito?
  - El PRD menciona "menos de 1 segundo" para cargar hasta 200 tareas, pero ¿se refiere al listado completo o filtrado?
5. Filtro por defecto en primer load
  - ¿Al entrar a la aplicación, ¿qué se muestra por defecto? ¿Todas las tareas o solo pendientes?
  - No hay AC que cubra el estado inicial.
6. Ausencia de "Ver todas"
  - ¿Hay un botón/opción para "Limpiar filtro" o "Ver todas las tareas"?
  - Los AC solo mencionan los 3 filtros específicos.

🤔 Supuestos Implícitos

1. Filtros mutuamente excluyentes
  - Se asume que los filtros son de selección única (no se pueden combinar: "Ver solo pendientes Y archivadas a la vez")
2. Actualización instantánea del UI
  - Se asume que el cambio de filtro es inmediato sin latencia de red (¿o hay un loading state?)
3. El filtro no afecta al ordenamiento
  - Se asume que "las más relevantes para hoy" sigue siendo el criterio de ordenamiento incluso dentro de un filtro
4. Mensajes genéricos
  - AC #5 dice "no hay tareas en esa categoría" pero ¿cuál es el texto exacto del mensaje? ¿Es localizable?
5. Persistencia de la sesión
  - ¿Se recuerda qué filtro tenía activo si cierro sesión y vuelvo?
6. Indicación visual del filtro activo
  - Se asume que el usuario sabe cuál filtro está activo, pero no hay AC que requiera una indicación clara (ej: botón destacado, ícono activo)

⚠️ Escenarios Faltantes

1. Filtrado + Búsqueda por texto
  - No se menciona si se puede filtrar por estado Y buscar por título simultáneamente
  - ¿Qué pasa si busco "proyecto A" dentro del filtro "Completadas"?
2. Comportamiento en dispositivos móviles
  - ¿Cómo se accede a los filtros en pantalla pequeña? (¿Dropdown, tabs, botones?)
  - ¿Hay espacio para mostrar los 3 filtros lado a lado?
3. Accesibilidad
  - No hay AC para navegación por keyboard (¿puedo cambiar filtros con Tab + Enter?)
  - No hay mención de screen readers (¿se anuncia el filtro activo?)
  - ¿Se comunica el estado de "sin tareas" de forma accesible?
4. Tiempo real / Múltiples pestañas
  - Si edito una tarea en otra pestaña del navegador, ¿se sincroniza el filtro en tiempo real?
  - Riesgo: desincronización entre pestañas
5. URL / Compartir estado del filtro
  - ¿Puede un usuario compartir un enlace con un filtro específico? (ej: ?state=completed)
  - ¿O el filtro es solo temporal en la sesión?
6. Performance con filtrado repetido
  - Si aplico filtros constantemente (pending → completed → archived → pending), ¿hay throttling o debouncing?

🔗 Dependencias

┌───────────────────────────────────┬──────────────────────────────────────────────────────────────────────────┐
│            Dependencia            │                                  Razón                                   │
├───────────────────────────────────┼──────────────────────────────────────────────────────────────────────────┤
│ Historia 2.2 (Visualizar listado) │ El filtro depende de que exista un listado base para filtrar             │
├───────────────────────────────────┼──────────────────────────────────────────────────────────────────────────┤
│ Historia 2.5 (Cambiar estado)     │ Cambios de estado deben reflejarse inmediatamente en los filtros activos │
├───────────────────────────────────┼──────────────────────────────────────────────────────────────────────────┤
│ Historia 2.1 (Crear tarea)        │ Nueva tarea debe afectar los filtros si coincide con el filtrado         │
├───────────────────────────────────┼──────────────────────────────────────────────────────────────────────────┤
│ Historia 2.4 (Eliminar tarea)     │ Eliminación debe actualizar el listado filtrado                          │
├───────────────────────────────────┼──────────────────────────────────────────────────────────────────────────┤
│ Historia 1.2 (Login)              │ Usuario debe estar autenticado para ver sus tareas filtradas             │
└───────────────────────────────────┴──────────────────────────────────────────────────────────────────────────┘

⚡ Riesgos Técnicos

1. Rendimiento con re-renders
  - Cada cambio de filtro dispara un re-render; con 200 tareas esto podría ser lento
  - Riesgo: UI lag o jank si no se optimiza (virtualización, memoización)
2. Consistencia de datos
  - Si hay actualización en tiempo real (ej: sync con Google Calendar), el filtro podría desincronizarse
  - Riesgo: el usuario ve "tareas no encontradas" pero luego aparecen tareas nuevas
3. Estado del filtro vs Base de datos
  - ¿Se guarda el filtro en el cliente o servidor?
  - Riesgo: si el servidor cambia el estado de una tarea, el cliente local puede estar desactualizado
4. Errores silenciosos
  - Si la API de filtrado falla, ¿qué ve el usuario?
  - Riesgo: pantalla vacía sin contexto de por qué
5. Compliance / Privacidad
  - El filtrado ocurre en cliente o servidor?
  - Si es en cliente, se asume que todos los datos ya se cargaron (riesgo si hay 200 tareas)

📋 Verificaciones Faltantes en los AC

- [ ] ¿Hay un indicador visual (badge, highlight) mostrando cuántas tareas hay en cada categoría?
- [ ] ¿Qué ocurre si cambio el estado de una tarea mientras el filtro está activo en ese estado?
- [ ] ¿Se puede "limpiar" o resetear el filtro a "Ver todas"?
- [ ] ¿El filtro se mantiene si la página se recarga accidentalmente?
- [ ] ¿Se puede cambiar de filtro usando keyboard (accesibilidad)?
