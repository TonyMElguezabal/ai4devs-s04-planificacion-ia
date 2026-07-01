# Objetivo

Eres un Product Owner senior con experiencia en aplicaciones SaaS. Crea User Stories apartir del documento PRD.md que esta en el folder docs sobre la aplicacion web FlowSync.

## Contexto:

* FlowSync es el nombre del sistema que debe construirse como sitio web.
* FlowSync es un sistema de tareas que está sincronizado con Google Calendar
* La explicación detallada del sistema FlowSync se encuentra en el archivo PRD.md que está en el directorio docs.

## Formato exacto:

las stories deben venir en el formato: Como \[rol], quiero \[acción], para \[beneficio].

## Alcance acotado:

solo funcionalidades del MVP (la sección correspondiente del PRD). sin inventar features que no estén en el documento.&#x20;

## AC en Given/When/Then:

cada story con 3-5 criterios de aceptación verificables, no genéricos.

## Agrupación:

las stories agrupadas por módulo, caso de uso o épica, según tenga más sentido para FlowSync.


## Ejemplo formato de historia de usuario:

### Historia de usuario

Como administrador, quiero asignar roles a los usuarios, para controlar sus permisos dentro de la aplicación.

### Criterios de aceptación (Given/When/Then)

* Given que soy administrador, When asigno el rol “Editor” a un usuario, Then ese usuario puede crear y modificar contenido.
* Given que soy administrador, When asigno el rol “Viewer” a un usuario, Then ese usuario solo puede visualizar contenido sin editarlo.
* Given que retiro un rol previamente asignado, When el usuario intenta acceder a funcionalidades restringidas, Then recibe un mensaje de “Acceso denegado”.

## Nota importante para tener en cuenta cuando se hacen las User Stories  

Marca como asumido lo que infieras y no este literal en el PRD.
