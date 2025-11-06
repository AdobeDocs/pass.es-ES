---
title: Guía de la API de REST (servidor a servidor)
description: Rest API cookbook server to server.
exl-id: 36ad4a64-dde8-4a5f-b0fe-64b6c0ddcbee
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1856'
ht-degree: 0%

---

# Guía de la API de REST (heredada) (de servidor a servidor) {#rest-api-cookbook-server-to-server}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Información general {#overview}

El propósito de este documento de guía es detallar las prácticas recomendadas para implementar la autenticación de Adobe Pass mediante las arquitecturas de servidor a servidor.  Proporciona requisitos básicos, implementación de flujo paso a paso y consideraciones generales para entornos y operaciones de producción.

### Mecanismo de limitación

La API de REST de autenticación de Adobe Pass se rige por un [mecanismo de restricción](/help/authentication/integration-guide-programmers/throttling-mechanism.md).


## Componentes {#components}

En una solución de servidor a servidor en funcionamiento están implicados los siguientes componentes:


| Tipo | Componente | Descripción |
| --- | --- | --- |
| Dispositivo de streaming | Aplicación de streaming | La aplicación de programador que reside en el dispositivo de transmisión del usuario y reproduce vídeo autenticado. |
| | Módulo \[Optional\] AuthN | si el dispositivo de streaming tiene un agente de usuario (es decir, un explorador web), el módulo AuthN es responsable de autenticar al usuario en el MVPD IdP. |
| \[Opcional\] Dispositivo AuthN | Aplicación AuthN | si el dispositivo de streaming no tiene un agente de usuario (es decir, un explorador web), la aplicación AuthN es una aplicación web de programador a la que se accede desde un dispositivo de un usuario independiente mediante un explorador web. |
| Infraestructura del programador | Servicio de programador | Servicio que vincula el dispositivo de streaming con el servicio de Adobe Pass para obtener decisiones de autenticación y autorización. |
| Infraestructura de Adobe | Servicio de Adobe Pass | Servicio que se integra con el servicio MVPD IdP y AuthZ y proporciona decisiones de autenticación y autorización. |
| Infraestructura de MVPD | MVPD IdP | Punto final de MVPD que proporciona un servicio de autenticación basado en credenciales para validar la identidad de su usuario. |
| | Servicio AuthZ de MVPD | Punto final de MVPD que proporciona decisiones de autorización basadas en las suscripciones del usuario, el control parental, etc. |

## Flujos {#flows}

### Registro dinámico de clientes (DCR)


Adobe Pass utiliza DCR para proteger las comunicaciones de cliente entre una aplicación o un servidor de programación y los servicios de Adobe Pass. El flujo de DCR es independiente y se describe en la [documentación de información general sobre el registro dinámico de clientes](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


### Autenticación (authN)

El flujo de autenticación se utiliza para permitir que un usuario se identifique
a su MVPD para determinar si el usuario tiene una cuenta válida.

1. El usuario inicia la aplicación Dispositivo de streaming e intenta iniciar sesión o ver contenido protegido.
2. La aplicación Dispositivo de streaming realiza una solicitud al servicio Programador para determinar si el dispositivo ya está autenticado.
3. El servicio Programador registra la aplicación mediante DCR.
4. El servicio de programación comprueba el estado de authN del dispositivo de streaming llamando a la API del servicio de Adobe Pass **checkauthn**.
5. En el caso de que la llamada **checkauthn** devuelva el estado de autenticación del dispositivo del usuario, la aplicación podrá continuar con el flujo de autorización.
6. En el caso de que la llamada **checkauthn** devuelva el estado de que el dispositivo de usuario NO está autenticado, la aplicación debería esperar a que un usuario solicite iniciar sesión.
7. Cuando el usuario solicita iniciar sesión directamente (por ejemplo, selecciona el botón de inicio de sesión) o indirectamente (por ejemplo, selecciona el contenido protegido cuando aún no se ha autenticado), la aplicación del Dispositivo de streaming realiza una solicitud al Servicio de programador para iniciar la autenticación de usuario. El servicio de programación solicita y recibe un código de registro único (regcode) llamando a la API del servicio de Adobe Pass **regcode**.
8. El servicio Programador también recupera la lista de MVPD y atributos actuales llamando a la API **config** del servicio Adobe Pass. Nota: También se puede llamar a esta API antes en el flujo y almacenarla en la caché.
9. El servicio Programador devuelve el regcode a la aplicación Streaming Device y la lista de MVPD procesados solicitada en el paso \#7. Nota: El programador especifica el formato de lista de MVPD procesada y se puede filtrar para permitir o bloquear explícitamente MVPD específicos (es decir, listas de permitidos o de bloqueados).
10. Si el es diferente del dispositivo AuthN (es decir, &quot;segunda pantalla&quot;), ya sea por elección o por necesidad (es decir, el dispositivo de streaming no admite un agente de usuario), el dispositivo de streaming debe mostrar el regcode y un URI para que el usuario acceda a la aplicación AuthN. El usuario escribe el URI en el agente de usuario del dispositivo AuthN para iniciar la aplicación AuthN y, a continuación, escribe el código de referencia en esa aplicación. Si el dispositivo de flujo continuo es el mismo que el dispositivo AuthN, el regcode se puede pasar mediante programación al módulo AuthN.
11. El módulo AuthN inicia la autenticación del usuario con MVPD mostrando un selector de MVPD. Una vez que el usuario selecciona MVPD, el módulo AuthN llama a **authenticate** con el regcode, lo cual redirige el agente de usuario al MVPD IdP. Cuando el usuario se autentica correctamente con MVPD, el agente de usuario se redirige de nuevo a través del servicio de Adobe Pass, donde la autenticación correcta se registra con el regcode y, a continuación, se redirige de nuevo al módulo AuthN.
12. Si el dispositivo de streaming es diferente del dispositivo AuthN, este debe mostrar un mensaje de autenticación al usuario y los pasos para continuar (p. ej., &quot;Success\! Ahora puede volver a la consola de juegos para continuar \[...\]&quot;). Si el dispositivo de streaming es el mismo que el dispositivo AuthN, el dispositivo de streaming puede detectar mediante programación la finalización de la autenticación.



El diagrama siguiente ilustra el flujo de autenticación:

![](../../../../assets/authn-flow.png)

### Autorización (authZ)

El flujo de autorización se utiliza para determinar si un usuario tiene derecho a acceder al contenido solicitado.

1. Cada vez que el usuario intenta ver contenido protegido en la aplicación del Dispositivo de streaming, la aplicación del Dispositivo de streaming llama al Servicio de programador para identificar el contenido y solicitar el permiso y la información necesarios para iniciar el flujo.
1. El servicio de programación llama a la API **authorize** de Adobe Pass y pasa el identificador de recurso junto con otros parámetros necesarios. El servicio de Adobe llama al servicio MVPD AuthZ con el ID de recurso y recibe una decisión de autorización que, a continuación, se devuelve al servicio de programador. El servicio de Adobe Pass almacenará en caché esta decisión de autorización durante un periodo configurable. En las llamadas subsiguientes **authorize** del servicio de programador al servicio de Adobe Pass, el valor almacenado en caché se devolverá siempre y cuando sea válido.
1. Si se concede la autorización, el servicio de programación debe llamar a la API de Adobe Pass **/tokens/media**, que devolverá un token de medios firmado. El servicio de programación debe validar el token de medios mediante la biblioteca de comprobador de token de medios (JAR). Si es válido, el servicio de programador debe devolver el permiso y el permiso necesarios para iniciar la secuencia (por ejemplo, la URL de la secuencia) solicitada en el paso \#1.
1. Si se deniega la autorización, la llamada a **authorize** devolverá un código de error y una descripción al servicio de programador. El servicio de programación debe devolver el código de error y la descripción (o un mensaje modificado por el programador) a la solicitud en el paso \#1.

El diagrama siguiente ilustra el flujo de autorización:

![](../../../../assets/authz-flow.png)

### Cerrar sesión

El flujo de cierre de sesión permite al usuario eliminar la identidad actual
asociadas a la aplicación.

1. Cuando el usuario solicita cerrar la sesión (es decir, quitar del dispositivo la cuenta actual de MVPD asociada con la aplicación), la aplicación de dispositivos de streaming llama al servicio de programador para indicarle que cierre la sesión del dispositivo.
1. El servicio de programación debe llamar a la API de Adobe Pass **logout**.

El diagrama siguiente ilustra el flujo de cierre de sesión:

![](../../../../assets/logout-flow.png)

### \[Opcional\] Autorización previa (también conocida como Pre-flight)

La preautorización se puede utilizar para determinar rápidamente, a partir de un conjunto de recursos, a qué recursos puede tener acceso un usuario.  El resultado de esta llamada se utiliza generalmente para personalizar la interfaz de usuario de un usuario individual.

1. Una vez autenticado el usuario, el Dispositivo de streaming puede llamar al Servicio de programador para solicitar el contenido al que el usuario tiene derecho a transmitir.

1. El servicio de programación debe llamar a la API de Adobe Pass **preauthorize** con una lista de ID de recursos, que son una cadena simple que normalmente representa un canal al que un usuario podría tener derecho de flujo. *Nota: actualmente, la llamada a* ***preauthorize*** *está configurada para limitar la lista a cinco (5) ID de recursos. Cuando se necesitan más de cinco recursos, se pueden realizar varias* llamadas ****** preautorizadas *, o bien se puede configurar la llamada para aceptar más de cinco recursos con un acuerdo de las MVPD. Los implementadores deben tener en cuenta el costo de una llamada de* ***preauthorize*** *a los recursos de MVPD, así como el tiempo de respuesta al programador y estructurar su uso de la llamada con prudencia.*

1. La llamada **preauthorize** responderá al servicio de programador con un objeto JSON que contiene un valor TRUE o FALSE para cada ID de recurso en la solicitud que indica si el usuario tiene derecho al canal asociado o no. *Nota: Si un MVPD no proporciona una respuesta para un Id. de recurso determinado (por ejemplo, debido a errores de red o tiempos de espera), el valor predeterminado será FALSO.*

1. El servicio de programador debe usar la respuesta de llamada **preauthorize** para crear una respuesta personalizada definida por el programador al dispositivo de flujo, normalmente para personalizar la presentación al usuario en función de sus derechos.

El diagrama siguiente ilustra el flujo de preautorización:

![](../../../../assets/preauthz-flow.png)


### \[Opcional\] Metadatos

Los metadatos se pueden utilizar para recuperar la información de usuario que comparte MVPD.
Algunos ejemplos de esto son el ID de usuario, el código postal, etc.

1. Una vez autenticado el usuario, el servicio de programador puede llamar a la API de Adobe Pass **usermetadata** para solicitar información sobre el usuario autenticado.

1. La respuesta incluirá todos los metadatos disponibles para el usuario determinado. Los campos específicos se configuran por separado para cada integración de Programador/MVPD.

El diagrama siguiente ilustra el flujo de preautorización:



![](../../../../assets/user-metadata-api-preauthz.png)



## Entornos y requisitos funcionales{#environments}



Un programador debe crear al menos dos entornos: uno para la producción y uno o más para el ensayo.


### Producción

El entorno de producción debe tener una alta disponibilidad y escalarse adecuadamente para picos grandes o inesperados (por ejemplo, deportes en directo, roturas)
noticias).



El servicio Adobe Pass se ejecuta en varios centros de datos distribuidos geográficamente en todo Estados Unidos.  Para lograr el mejor tiempo de respuesta (es decir, la menor latencia) desde el servicio de Adobe Pass, el Programador también debe crear un servicio similar disperso geográficamente
infraestructura.


El servicio Programador debe limitar la caché DNS a un máximo de 30 segundos en caso de que Adobe necesite redireccionar el tráfico. Esto puede ocurrir si un centro de datos deja de estar disponible.


El programador debe proporcionar el rango de IP pública del entorno de producción. Estas se incluirán en una lista de direcciones IP permitidas en la infraestructura de Adobe Pass para su acceso y se administrarán mediante las políticas de uso de API equitativas de Adobe.

### Ensayo

El entorno de ensayo puede ser mínimo, pero debe incluir todos los componentes del sistema y la lógica empresarial. Debe funcionar de manera similar a la producción y permitir versiones de prueba fuera de la producción. Lo ideal es que el entorno de ensayo se pueda conectar a los entornos de prueba de Adobe Pass para que el programador lo use, y Adobe cuando sea necesario, para que podamos ayudarle en las pruebas y la resolución de problemas.

### Requisitos funcionales

El servicio Programador debe pasar información precisa de identificación del dispositivo para el que está ejecutando los flujos. Además, el servicio Programador debe pasar la IP del dispositivo para el que está ejecutando los flujos (en un encabezado x-forwarded-for) junto con el puerto de origen de la conexión (en el campo de información del dispositivo):

    **X-Forwarded-For : \&lt;client\_ip\>**
    
    donde \&lt;client\_ip\> es la dirección IP pública del cliente
    
    
    
    Es necesario agregar el encabezado en **regcode** y **authorize** llamadas
    
    Ejemplos :
    
    POST /reggie/v1/{req\_id}/regcode HTTP/1.1
    
    X-Forwarded-For:203.45.101.20
    
    
    
    GET /api/v1/authorize HTTP/1.1
    
    X-Forwarded-For:203.45.101.20



El servicio Programador debe enviar los datos y el formato requeridos por las MVPD individuales o aplicaciones integradas (por ejemplo, IP del dispositivo, puerto de origen, información del dispositivo, MRSS, datos opcionales como ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.


El servicio Programador debe respetar los TTL authN y authZ al almacenar en caché e invalidar las sesiones authN o authZ cuando se le notifique.

El programador debe mantener los certificados compartidos con Adobe.
