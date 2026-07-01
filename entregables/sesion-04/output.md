# User Stories — FlowSync MVP

**Documento**: User Stories derivadas del PRD.md  
**Versión**: 1.0  
**Fecha**: Q3 2026  
**Audiencia**: Equipo de ingeniería, Product Owner, QA

---

## 📋 Estructura de historias

Cada historia sigue el formato:
```
Como [rol], quiero [acción], para [beneficio].
```

Los criterios de aceptación están en formato **Given/When/Then** y son verificables.

---

## 🔐 ÉPICA 1: Autenticación y Gestión de Cuenta

### Historia 1.1: Registro de usuario

**Como** visitante, **quiero** crear una cuenta con email y contraseña, **para** acceder a FlowSync.

#### Criterios de aceptación

- **Given** que estoy en la pantalla de registro, **When** ingreso un email válido y una contraseña de al menos 8 caracteres, **Then** se crea mi cuenta y soy redirigido a la pantalla de onboarding.

- **Given** que ingreso un email que ya está registrado, **When** intento crear la cuenta, **Then** veo un mensaje indicando que el email ya existe y se me invita a ir al inicio de sesión.

- **Given** que ingreso una contraseña con menos de 8 caracteres, **When** intento registrarme, **Then** recibo un mensaje de error indicando el requisito mínimo de caracteres.

- **Given** que completo el registro exitosamente, **When** accedo a la pantalla de onboarding, **Then** veo una explicación breve de qué es FlowSync y una invitación a crear mi primera tarea.

- **Given** que soy un usuario registrado exitosamente, **When** intento crear otra cuenta con el mismo email, **Then** el sistema me indica que ese email ya existe.

---

### Historia 1.2: Inicio de sesión

**Como** usuario registrado, **quiero** iniciar sesión con mi email y contraseña, **para** acceder a mi cuenta y mis tareas.

#### Criterios de aceptación

- **Given** que tengo una cuenta registrada, **When** ingreso mi email y contraseña correctos, **Then** accedo a mi lista de tareas.

- **Given** que ingreso un email correcto pero contraseña incorrecta, **When** intento iniciar sesión, **Then** veo un mensaje de error indicando que las credenciales son inválidas.

- **Given** que ingreso un email no registrado, **When** intento iniciar sesión, **Then** veo un mensaje indicando que ese email no está registrado.

- **Given** que he iniciado sesión exitosamente, **When** cierro el navegador y regreso, **Then** mi sesión se mantiene mediante tokens de acceso (asumo: cookies/localStorage seguro).

---

### Historia 1.3: Cierre de sesión

**Como** usuario autenticado, **quiero** cerrar sesión, **para** salir de forma segura de mi cuenta.

#### Criterios de aceptación

- **Given** que estoy autenticado, **When** hago clic en "Cerrar sesión", **Then** se finaliza mi sesión y soy redirigido a la pantalla de login.

- **Given** que acabo de cerrar sesión, **When** intento acceder directamente a una ruta protegida (ej: /tasks), **Then** soy redirigido a la pantalla de login.

- **Given** que mi sesión se ha cerrado, **When** intento usar tokens antiguos de acceso, **Then** recibo un error de autenticación y se me solicita iniciar sesión nuevamente.

---

## ✅ ÉPICA 2: Gestión de Tareas (CRUD)

### Historia 2.1: Crear tarea

**Como** usuario autenticado, **quiero** crear una tarea con al menos un título, **para** agregar pendientes a mi lista.

#### Criterios de aceptación

- **Given** que estoy en la pantalla de tareas, **When** hago clic en "Nueva tarea" e ingreso un título, **Then** la tarea se crea y aparece en mi listado con estado `pending`.

- **Given** que estoy creando una tarea, **When** ingreso un título, una descripción y una fecha límite, **Then** la tarea se crea con todos esos datos.

- **Given** que intento crear una tarea sin título, **When** hago clic en guardar, **Then** veo un mensaje de error indicando que el título es obligatorio.

- **Given** que creo una tarea exitosamente, **When** miro el listado, **Then** la nueva tarea aparece inmediatamente sin necesidad de recargar.

- **Given** que creo una tarea sin especificar descripción ni fecha límite, **When** se guarda, **Then** la tarea se crea con esos campos vacíos y eso es válido.

---

### Historia 2.2: Visualizar listado de tareas

**Como** usuario autenticado, **quiero** ver todas mis tareas en un listado, **para** revisar mis pendientes.

#### Criterios de aceptación

- **Given** que tengo varias tareas creadas, **When** accedo a la sección de tareas, **Then** veo un listado con todas mis tareas mostrando al menos título, estado y fecha límite (si existe).

- **Given** que no tengo ninguna tarea, **When** accedo al listado, **Then** veo un estado vacío con una invitación a crear mi primera tarea.

- **Given** que tengo tareas en el listado, **When** el listado carga, **Then** se muestra en menos de 1 segundo (asumo: medido en condiciones estándar para un usuario con hasta 200 tareas).

- **Given** que accedo al listado de tareas, **When** las tareas se cargan, **Then** están ordenadas priorizando las más relevantes para "hoy" (asumo: ordenadas por fecha límite próxima, luego por estado).

---

### Historia 2.3: Editar tarea

**Como** usuario autenticado, **quiero** editar cualquier campo de una tarea existente, **para** actualizar mi información.

#### Criterios de aceptación

- **Given** que tengo una tarea creada, **When** hago clic en editar y cambio el título, **Then** la tarea se actualiza con el nuevo título.

- **Given** que edito una tarea, **When** cambio su descripción o fecha límite, **Then** esos cambios se guardan correctamente.

- **Given** que una tarea tiene una fecha límite, **When** la edito para cambiar esa fecha, **Then** la tarea actualiza su fecha límite y se reordena en el listado según corresponda.

- **Given** que tengo la tarea en edición, **When** hago clic en cancelar sin guardar cambios, **Then** la tarea mantiene sus valores anteriores.

---

### Historia 2.4: Eliminar tarea

**Como** usuario autenticado, **quiero** borrar una tarea, **para** remover un pendiente que ya no necesito.

#### Criterios de aceptación

- **Given** que tengo una tarea en mi listado, **When** hago clic en eliminar, **Then** me pide confirmación antes de borrar.

- **Given** que confirmo la eliminación de una tarea, **When** se procesa la acción, **Then** la tarea desaparece del listado.

- **Given** que acabo de eliminar una tarea, **When** intento acceder a ella por ID, **Then** obtengo un error 404 o "no encontrada".

- **Given** que tenía una tarea eliminada, **When** realizo la acción, **Then** no hay opción de deshacerla en el MVP (asumo: la eliminación es permanente).

---

### Historia 2.5: Cambiar estado de tarea

**Como** usuario autenticado, **quiero** cambiar el estado de una tarea (pendiente, completada, archivada), **para** organizar mis pendientes.

#### Criterios de aceptación

- **Given** que tengo una tarea con estado `pending`, **When** la marco como completada, **Then** su estado cambia a `completed` y se refleja visualmente en el listado.

- **Given** que tengo una tarea completada, **When** la cambio a archivada, **Then** su estado cambia a `archived`.

- **Given** que una tarea está en estado `archived`, **When** la cambio a `pending`, **Then** vuelve a estar disponible como pendiente.

- **Given** que completo una tarea que tiene una fecha límite y está sincronizada con Google Calendar, **When** cambio su estado a `completed`, **Then** el evento en Google Calendar se elimina o se marca como completado (asumo: se elimina como comportamiento por defecto del MVP).

---

## 🔍 ÉPICA 3: Organización y Filtrado

### Historia 3.1: Filtrar tareas por estado

**Como** usuario autenticado, **quiero** filtrar mis tareas por estado (pendientes, completadas, archivadas), **para** ver solo las que me interesan en cada momento.

#### Criterios de aceptación

- **Given** que tengo tareas en diferentes estados, **When** selecciono el filtro "Pendientes", **Then** solo veo tareas con estado `pending`.

- **Given** que aplico el filtro "Completadas", **When** se actualiza el listado, **Then** solo veo tareas con estado `completed`.

- **Given** que aplico el filtro "Archivadas", **When** se actualiza el listado, **Then** solo veo tareas con estado `archived`.

- **Given** que aplico un filtro, **When** cambio a otro, **Then** el listado se actualiza sin necesidad de recargar la página.

- **Given** que aplico un filtro y no hay tareas que coincidan, **When** veo el resultado, **Then** se muestra un mensaje indicando que no hay tareas en esa categoría.

---

## 📥 ÉPICA 4: Exportación

### Historia 4.1: Exportar tareas a CSV

**Como** usuario autenticado, **quiero** exportar mis tareas a un archivo CSV, **para** poder llevarme mis datos fuera de FlowSync.

#### Criterios de aceptación

- **Given** que tengo tareas en mi cuenta, **When** hago clic en "Exportar a CSV", **Then** se descarga un archivo CSV con mis tareas.

- **Given** que exporto mis tareas, **When** abro el archivo CSV, **Then** contiene al menos las columnas: título, descripción, estado y fecha límite.

- **Given** que tengo tareas sin descripción o fecha límite, **When** las exporto, **Then** esas columnas aparecen vacías en el CSV pero están presentes.

- **Given** que tengo varias tareas, **When** exporto el CSV, **Then** incluye todas mis tareas sin importar su estado o fecha límite.

- **Given** que no tengo ninguna tarea, **When** intento exportar, **Then** se descarga un archivo CSV con headers pero sin registros, o veo un mensaje indicando que no hay datos para exportar (asumo: comportamiento a definir durante refinamiento).

---

## 🔄 ÉPICA 5: Sincronización con Google Calendar

### Historia 5.1: Conectar Google Calendar

**Como** usuario autenticado, **quiero** conectar mi cuenta de Google a FlowSync, **para** sincronizar mis tareas con mi calendario.

#### Criterios de aceptación

- **Given** que estoy autenticado en FlowSync, **When** hago clic en "Conectar Google Calendar", **Then** soy redirigido al flujo de autorización OAuth de Google.

- **Given** que completo la autorización OAuth, **When** regreso a FlowSync, **Then** veo una confirmación de que mi cuenta de Google está conectada.

- **Given** que he conectado exitosamente Google Calendar, **When** voy a mis configuraciones, **Then** veo que mi cuenta de Google está vinculada y tengo la opción de desconectar.

- **Given** que me autorizo pero luego revoco permisos en Google, **When** intento sincronizar en FlowSync, **Then** recibo un error claro indicando que necesito reconectar mi cuenta.

---

### Historia 5.2: Tareas con fecha límite aparecen como eventos en Google Calendar

**Como** usuario autenticado con Google conectado, **quiero** que mis tareas con fecha límite aparezcan automáticamente como eventos en mi Google Calendar, **para** tener toda mi información en un solo lugar.

#### Criterios de aceptación

- **Given** que tengo Google Calendar conectado, **When** creo una tarea con una fecha límite, **Then** aparece automáticamente como un evento en mi Google Calendar.

- **Given** que creo una tarea con fecha límite, **When** reviso mi Google Calendar, **Then** veo el evento con el título de la tarea y la fecha correcta.

- **Given** que creo una tarea sin fecha límite, **When** Google Calendar se sincroniza, **Then** no se crea un evento (solo tareas con fecha límite se sincronizan).

- **Given** que me encuentro en una situación donde Google Calendar API falla, **When** intento crear una tarea con fecha límite, **Then** la tarea se guarda en FlowSync pero la sincronización se reintenta más tarde (asumo: se registra en logs para diagnosticar).

---

### Historia 5.3: Cambios en fecha límite se actualizan en Google Calendar

**Como** usuario autenticado con Google conectado, **quiero** que cuando cambio la fecha límite de una tarea, el evento en Google Calendar se actualice automáticamente, **para** mantener ambos sincronizados.

#### Criterios de aceptación

- **Given** que tengo una tarea sincronizada con una fecha límite original, **When** edito la tarea y cambio su fecha límite, **Then** el evento en Google Calendar se actualiza a la nueva fecha.

- **Given** que cambio la fecha límite de una tarea, **When** voy a Google Calendar, **Then** veo el evento movido a la nueva fecha sin duplicados.

- **Given** que edito una tarea sincronizada pero no cambio la fecha límite, **When** guardo la tarea, **Then** solo se actualizan los cambios en FlowSync (asumo: no se fuerza una sincronización innecesaria con Google).

---

### Historia 5.4: Eliminar o completar tarea elimina evento en Google Calendar

**Como** usuario autenticado con Google conectado, **quiero** que cuando elimino o completo una tarea, el evento correspondiente en Google Calendar se elimine o se marque como completado, **para** mantener la coherencia.

#### Criterios de aceptación

- **Given** que tengo una tarea sincronizada, **When** cambio su estado a `completed`, **Then** el evento en Google Calendar se elimina o se marca como completado (asumo: se elimina como comportamiento por defecto).

- **Given** que tengo una tarea sincronizada, **When** la elimino del listado de FlowSync, **Then** el evento correspondiente se elimina de Google Calendar.

- **Given** que elimino una tarea con una fecha límite, **When** Google Calendar se sincroniza, **Then** no aparece duplicada ni quedada en el calendario.

- **Given** que la API de Google falla al eliminar un evento, **When** intento eliminar una tarea, **Then** la tarea se elimina en FlowSync pero se registra el error para reintentarlo.

---

### Historia 5.5: Desconectar Google Calendar

**Como** usuario autenticado, **quiero** desconectar mi cuenta de Google de FlowSync, **para** dejar de sincronizar mis tareas con mi calendario.

#### Criterios de aceptación

- **Given** que tengo Google Calendar conectado, **When** hago clic en "Desconectar", **Then** veo un mensaje de confirmación.

- **Given** que confirmo la desconexión, **When** se procesa, **Then** mi cuenta de Google se desvincula y FlowSync deja de sincronizar tareas.

- **Given** que he desconectado Google Calendar, **When** creo una nueva tarea con fecha límite, **Then** no se crea un evento en Google Calendar.

- **Given** que desconecto Google Calendar, **When** reviso mis tareas en FlowSync, **Then** todas mis tareas previas se mantienen (la desconexión no borra tareas, solo detiene la sincronización).

- **Given** que desconecté Google hace una semana, **When** reconecto, **Then** vuelvo a sincronizar pero no se duplican eventos de tareas anteriores (asumo: existe control de idempotencia en la sincronización).

---

## 📝 Notas de asunción

Las siguientes asunciones se han marcado en los AC y basadas en lecturas del PRD o mejores prácticas estándar:

1. **Asumo: Ordenación por defecto** — El PRD menciona que el listado se ordena "de forma que las más relevantes para hoy sean visibles primero" pero no especifica el criterio exacto. Asumo: ordenadas por fecha límite próxima, luego por estado.

2. **Asumo: Manejo de errores de sincronización** — El PRD menciona que si Google Calendar API falla, se reintenta más tarde. Asumo: existe un sistema de cola/reintentos con logs para observabilidad.

3. **Asumo: Comportamiento al completar/eliminar** — El PRD dice "se elimina o se marca según corresponda" para eventos. Asumo comportamiento por defecto: el evento se elimina cuando la tarea se completa o se borra.

4. **Asumo: Sesiones y tokens** — El PRD menciona "sesión se mantiene mediante tokens de acceso". Asumo: implementación segura usando cookies httpOnly o localStorage con rotación de tokens.

5. **Asumo: Exportación vacía** — No se especifica qué ocurre si no hay tareas. Asumo: se descarga CSV con headers o mensaje indicando sin datos.

6. **Asumo: Control de idempotencia** — Para evitar duplicados al reconectar Google, asumo: existe un ID único por tarea que vincula a eventos en Google.

---

## 📊 Resumen de historias

| Épica | Historias | Total |
|-------|-----------|-------|
| Autenticación y Gestión de Cuenta | 1.1, 1.2, 1.3 | 3 |
| Gestión de Tareas (CRUD) | 2.1, 2.2, 2.3, 2.4, 2.5 | 5 |
| Organización y Filtrado | 3.1 | 1 |
| Exportación | 4.1 | 1 |
| Sincronización con Google Calendar | 5.1, 5.2, 5.3, 5.4, 5.5 | 5 |
| **TOTAL** | — | **15** |

---

**Versión**: 1.0  
**Generado**: Q3 2026  
**Próximo paso**: Refinamiento con el equipo y estimación de puntos de historia.
