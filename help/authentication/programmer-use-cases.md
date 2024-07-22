---
title: Casos de uso del programador
description: Casos de uso del programador
exl-id: 51ca7e4f-b0d8-4e35-8398-2efb4879de2a
source-git-commit: 189d0e5c83d1bc171c6ff3057f22dbf4055ea6b1
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# Casos de uso del programador {#programmer-use-cases}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

Este documento resume los casos de uso de integración del programador admitidos por la autenticación de Adobe Pass. Puede consultar esta página antes de comenzar un proyecto de integración para ver qué funciones se admiten actualmente.

## Casos de uso {#use-cases}


### Integración básica: autenticación federada y autorización para una red de un solo canal {#basic-integration}

**Prioridad** - Alta

**Desglose**: aplicación TVE de marca de programador único con una red de canal alojada dentro de la experiencia

Esto permite a los programadores ofrecer contenido premium, en su propia aplicación TVE de marca*, con una comprobación de derechos federada para la MVPD. El ID del solicitante debe alinearse para que coincida con la marca de la aplicación que sirve el contenido al visualizador. En este escenario, existe una relación 1 a 1 entre el ID del solicitante de autenticación de Adobe Pass y el ID de recurso que se verifica para el derecho.

>[!NOTE]
>
>La aplicación TVE se utiliza en este documento para hacer referencia de forma colectiva a los diferentes tipos de aplicaciones (aplicaciones web, aplicaciones móviles, etc.) compatible con la autenticación de Adobe Pass. La columna Plataformas que aparece a continuación puede contener detalles sobre plataformas compatibles para casos de uso específicos.

#### Casos de uso específicos (comunes a la mayoría de las integraciones) {#sp-use-cases-basic-int}

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|:--------:|:-----------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------:|:-----------------------------------------:|
| Alta | Descubrimiento de MVPD desde la aplicación TVE del programador | El usuario comienza con la aplicación TVE de marca del programador y se le pide que seleccione su proveedor de MVPD. | Web (SWF/JS)                    Móvil (iOS/Android)                   API sin cliente (para la segunda pantalla) |                                           |
| Alta | Autenticación federada desde la aplicación de TVE del programador | El usuario comienza con la aplicación TVE de marca del programador y después de seleccionar su proveedor de MVPD, el usuario pasa a la página de inicio de sesión de la MVPD para introducir sus credenciales. | Web (SWF/JS)                    Móvil (iOS/Android) |                                           |
| Alta | Autorización del programador de la aplicación TVE | Una vez autenticado el usuario, la aplicación TVE del programador puede realizar solicitudes de autorización de canal de retorno a la MVPD para comprobar el derecho del usuario. Normalmente esto es solo comprobar si la red del canal está en el paquete de suscripción de MVPD de los usuarios.                                  En este caso, el ID de solicitante y el ID de recurso coincidirán 1:1. | Todas las plataformas |                                           |
| Medium | Cerrar sesión desde la aplicación TVE del programador | Permite al usuario cerrar la sesión y borrar los tokens AuthN/AuthZ de autenticación de Adobe Pass. En muchos casos, esto también desconecta al usuario de la MVPD. Sin embargo, las MVPD varían en cuanto a si esto es compatible. Siempre borra la sesión y los tokens de autenticación de Adobe Pass. | Todas las plataformas excepto XBox Native | Varias MVPD no son compatibles con esto. |
| Alta | Inicio de sesión único en sitios y aplicaciones | Permite al usuario compartir la sesión de inicio de sesión en sitios y aplicaciones sin necesidad de volver a iniciar sesión. | Todas las plataformas excepto API sin cliente | Requiere al menos SDK 1.7 para algunas MVPD. |

### Aplicación TVE única que aloja redes de varios canales {#single-app-multi-channel}

**Prioridad** - Alta

Permite al programador agregar varias redes de canales de contenido en el mismo destino de marca para sus visualizadores.

#### Casos de uso específicos {#sp-use-cases-singl-tve-app}

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|--------|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alta | Autorización de canal distinto | El usuario puede ver contenido de varias redes de canales dentro de la misma aplicación de TVE. El programador puede realizar llamadas de autorización específicas a cada red de canal para confirmar el derecho de los usuarios. | Todas las plataformas | Todas las MVPD ahora admiten esto de alguna forma. |
| Baja | Consulta de autorización de comprobación preliminar | Esto permite al programador comprobar qué canales tiene el usuario en su paquete en una sola llamada de API. Esto se realiza antes de las llamadas reales de AuthZ para filtrar contenido fuera de la interfaz de usuario a la que el usuario no tiene acceso. |               | La mayoría de las MVPD aún no exponen estos datos como Atributos de usuario, por lo que Adobe realmente hace llamadas AuthZ para obtenerlos. Además, la mayoría de las MVPD están limitadas a 5 a la vez, porque no admiten varios canales en una sola llamada.                             Es muy importante verificar cuántos canales necesita el programador para comprobar previamente. No importa cuál sea el número, tendremos que comprobar que está bien con las MVPD. La mayoría de las MVPD no admiten actualmente (tercer trimestre de 2013) más de cinco canales. |

### Autorización de nivel de recurso {#asset-level-authz}

**Prioridad** - Baja

**Desglose** - Pasar Un Identificador De Recurso En Solicitud De Autorización

**Plataformas** - Todas las plataformas

#### Casos de uso específicos {#sp-use-cases-asset-lvl-authz}

Permite que la MVPD obtenga análisis de nivel de recurso en cada llamada de AuthZ. Esto tiene la desventaja de anular la caché de autenticación AuthZ de Adobe Pass.

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|--------|-------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|-------------|--------------------------------------|
| Baja | Pasar un identificador de recurso en una solicitud de autorización | Permite que la MVPD obtenga análisis de nivel de recurso en cada llamada de AuthZ.  Tiene el inconveniente de anular la caché de autenticación AuthZ de Adobe Pass. | Todas las plataformas | Actualmente, solo una MVPD admite esto. |




### Control parental {#parental-controls}

**Prioridad** - Baja

Permite aplicar restricciones de cuenta de usuario de MVPD en la aplicación TVE del programador.

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|--------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|-----------------------------------|
| Baja | Filtrar contenido en función de los atributos del usuario | Permite al programador comprobar la clasificación máxima permitida para un usuario antes de procesar la lista de contenido disponible para el usuario. | Web (Flash/JS)                    Móvil (iOS/Android) | Actualmente solo funciona con un MVPD. |
| Baja | Pasar clasificaciones de contenido en la solicitud de AuthZ | Permite al programador transmitir a la MVPD la clasificación específica del contenido que el usuario desea ver como parte de la solicitud de AuthZ                             En relación con #3, ya que las calificaciones se encuentran normalmente en el nivel de los activos. | Todas las plataformas | Actualmente solo funciona con un MVPD. |

#### Personalización de la integración de MVPD por marca de programador {#mvpd-int-cust-prog-brand}

**Prioridad** - Medium

Habilita la experiencia personalizada durante AuthN o para mensajes de error de AuthZ.

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|--------|------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------------------------------------|
| Medium | Pase el identificador del proveedor de servicios en la solicitud de AuthN. | Habilite la personalización de marca específica en la página de inicio de sesión de MVPD específica para el proveedor de servicios. También habilite la selección automática del valor predeterminado para que coincida con la audiencia, como Español para Univision. | Todas las plataformas | Varía según MVPD. Algunos no lo admiten. |
| Medium | Mensajes de error personalizados en respuesta de AuthZ | Activa los mensajes de error específicos del programador o de la marca procedentes de la MVPD que pueden incluir un mensaje específico para mejorar la venta con un vínculo que actualice el paquete. | Web, Android, iOS | Varía según MVPD. Algunos no lo admiten. |


### Casos de uso de dispositivos conectados {#connected-devices}

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|--------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Medium | XBox LiveID SSO en aplicaciones y consolas | Permite al usuario compartir una sesión de AuthN entre aplicaciones y entre distintas consolas de juegos, vinculadas a su cuenta de LiveID. | SDK nativo de XBox | A la mayoría de las MVPD no les gusta esto porque el modelo típico es enlazar el token al dispositivo, no al usuario.                             No recomendamos este enfoque más si es posible. |
| Alta | Dispositivo conectado con tokens enlazados al appID en el dispositivo | Permite al programador enlazar el derecho de MVPD en el token al appID en el dispositivo al que se emitió. | API sin cliente | Esto alinea el Dispositivo conectado más estrechamente con la implementación de Pase estándar para tokens.                             Sigue siendo necesario mejorar para que sea un ID para todo el dispositivo. |

### Longitud TTL de AuthN específica del dispositivo {#authn-ttl-length}

Habilite el derecho de TVE para eventos especiales que puedan no ser recursos que estén en la base de datos de derechos de MVPD como los canales normales.

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|--------|------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|--------------------------------------------------------------------------------------------------------------------------|
| Alta | Establecer diferentes valores TTL por plataforma | Permite al programador establecer una longitud TTL diferente para dispositivos web, móviles y conectados. Actualmente, la autenticación de Adobe Pass admite la capacidad de tener 3 valores TTL independientes:                                Web (Flash)                    Móvil/HTML 5                    Sin cliente: dispositivos conectados |           | Algunas MVPD establecen el TTL dinámicamente. El Adobe puede anular esta configuración dinámica si es necesario mediante los ajustes de configuración. |

### Aplicaciones especiales basadas en eventos {#special-event}

**Prioridad** - Baja

Habilite el derecho de TVE para eventos especiales que puedan no ser recursos que estén en la base de datos de derechos de MVPD como los canales normales.

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|--------|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|------------------------------------------|
| Baja | Varios canales como proxy para un evento | Esto se hizo para las Olimpiadas, donde el suscriptor necesitaba tener dos canales diferentes en su paquete para tener acceso. En este caso, la autenticación de Adobe Pass creó un nuevo ID de recurso e hizo que todas las MVPD hicieran la asignación a los canales específicos en su extremo.  Eso funcionó bien con suficiente aviso avanzado. Esto era importante porque la mayoría de las MVPD no admiten múltiples llamadas de recursos. | Todas las plataformas | Compatible con todas las MVPD con el aviso adecuado. |
| Baja | Aplicación De Evento Especial Nuevo, Uso De Recursos De Canal Existentes | Esto se hizo para la locura de marzo. El proveedor de contenido creó una nueva aplicación con un nuevo ID de solicitante. Todas las MVPD necesarias para agregar compatibilidad con el nuevo ID de solicitante en su sistema. Los resourceID eran canales normales.  Algunas MVPD también necesitaban asignar los canales como válidos bajo el nuevo solicitante, por lo que se necesitaba más tiempo para esos casos. | Todas las plataformas | Compatible con todas las MVPD con el aviso adecuado. |
| Baja | ID del solicitante existente, resourceID | Esto se hizo para el torneo de fin de semana de golf Masters. Fue solo un pequeño evento durante un par de días, y los Masters tenían su propia aplicación móvil que estaba bien para mostrar el contenido. El programador tenía previsto pagar el tráfico de autenticación de Adobe Pass y utilizar únicamente su ID de solicitante y el ID de recurso estándar. El único truco era que el programador compartiera un certificado móvil para la firma del ID de solicitante con los maestros, y que lo agregaran a su configuración como su certificado de copia de seguridad para ese fin de semana. | Todas las plataformas | Sin impacto para las MVPD |

### Integración del servidor de contenido {#content-server-integration}

**Prioridad**- Medium

Habilitar la validación de tokens de medios antes de lanzar el flujo de vídeo al reproductor cliente.

| Prioridad | Caso de uso | Descripción | Plataformas | Notas de MVPD |
|---------|-----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------|
| Alta | Reproductor Federado De Programador: Con Autorización A Nivel De Página | Las API de autenticación de Adobe Pass se realizan en JavaScript en la página y el token se pasa al reproductor. El token se puede pasar al servicio de validación de un par de maneras:                                 Obtener parámetro en la URL del servicio de validación                    Se ha pasado un parámetro de URL en la cadena de consulta de la URL de flujo                    API de interfaz externa                    FlashVars |           |            |
| Medium | Reproductor Federado De Programador: Con Autorización Interna Del Reproductor | Las API de autenticación de Adobe Pass se realizan en ActionScript en el SWF del reproductor, por lo que el token está disponible para el reproductor desde la llamada de retorno. |           |            |
| Alta | Reproductor sindicado: alojado en el portal MVPD con autorización a nivel de página mediante un iFrame para envolver el reproductor | Similar al reproductor con autorización de nivel de página, pero con el contenedor de página del reproductor iFramed en el portal de MVPD. La autenticación debe realizarse por separado en el portal de MVPD. |           |                        |


<!--
>[!RELATEDINFORMATION]
>
>* MVPD Integration Features
>* Entitlement Flow
>* Platform / Device Requirements
-->
