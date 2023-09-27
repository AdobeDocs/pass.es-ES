---
title: Resumen de API
description: Información general de API de Monitorización de concurrencia
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1425'
ht-degree: 0%

---


# Resumen de API {#api-overview}

Ver el [documentación de API en línea](http://docs.adobeptime.io/cm-api-v2/) para obtener más información.

## Objetivo y requisitos previos {#purpose-prerequisites}

Este documento ayuda a los desarrolladores de aplicaciones a utilizar nuestra especificación de API de Swagger al implementar una integración con la Monitorización de concurrencia. Es muy recomendable que el lector tenga una comprensión previa de los conceptos definidos por el servicio antes de seguir esta guía. Para tener este conocimiento, es necesario tener una visión general de la [documentación del producto](/help/concurrency-monitoring/cm-home.md) y el [Especificación de API de Swagger](http://docs.adobeptime.io/cm-api-v2/).


## Introducción {#api-overview-intro}

Durante el proceso de desarrollo, la documentación pública de Swagger representa la guía de referencia para comprender y probar los flujos de API. Este es un buen punto de partida para tener un enfoque práctico y familiarizarse con la forma en que las aplicaciones del mundo real se comportarían en diferentes escenarios de interacción de usuarios.

Envío de un ticket en [Zendesk](mailto:tve-support@adobe.com) para registrar su empresa y aplicaciones en Monitoreo de Concurrencia. El Adobe asignará un ID de aplicación a cada entidad. En esta guía utilizaremos dos aplicaciones de referencia con ID **demo-app** y **demo-app-2** que estará bajo el Adobe de inquilino.


## Casos de uso {#api-use-case}

El primer paso para probar un flujo con Swagger es introducir el ID de aplicación en la parte superior derecha de la página de esta manera:

![](assets/setting-app-id.png)

Después de esto, presionamos **Explorar** para establecer el id que se utilizará en el encabezado Autorización para todas las llamadas realizadas a la API de REST.  Cada llamada a la API espera que el ID de aplicación se pase a través de la autenticación básica HTTP. El nombre de usuario es el ID de aplicación y la contraseña está vacía.


### Primera aplicación {#first-app-use-cases}

Aplicación con ID **demo-app** el equipo de Adobe ha asignado una directiva con una regla que restringe el número de flujos simultáneos a 3. Se asigna una póliza a una aplicación específica en función de la solicitud presentada en Zendesk.


#### Recuperando metadatos {#retrieve-metadata-use-case}

La primera llamada que realizamos es para el recurso de metadatos para obtener la lista de atributos de metadatos necesarios que se deben pasar como datos de formulario durante la inicialización de la sesión. Estos metadatos se utilizarán para evaluar las directivas asignadas a esta aplicación.

![](assets/retrieving-metadata.png)

Después de pulsar &quot;Probar&quot;, para la aplicación con ID **demo-app** obtendremos el siguiente resultado:

![](assets/empty-metadata-call.png)

Como podemos ver en el campo del cuerpo de respuesta, la lista de atributos de metadatos está vacía. Esto significa que los atributos requeridos por el diseño son suficientes para evaluar la directiva de 3 flujos asignada a esta aplicación. Consulte también la [Documentación de campos de metadatos estándar](/help/concurrency-monitoring/standard-metadata-attributes.md). Después de esta llamada, podemos continuar y crear una nueva sesión en el recurso REST de sesiones.


#### Inicialización de sesión {#session-initial}

Una aplicación realiza la llamada de inicialización de sesión después de adquirir toda la información necesaria para realizarla.

![](assets/session-init.png)

No es necesario proporcionar ningún código de terminación en la primera llamada, ya que no tenemos ningún otro flujo activo. Y no hay atributo de metadatos, porque no se devolvió ninguno desde la llamada de recuperación de metadatos.

El **sujeto** y el **idp** Los parámetros son obligatorios y se especificarán como variables de ruta de URI. Puede obtener la **sujeto** y **idp** parámetros realizando una llamada para el **mvpd** y **upstreamUserID** campos de metadatos de la autenticación de Adobe Pass. Consulte también la [información general sobre las API de metadatos](https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/user-metadat/user-metadata-feature.html?lang=en#). Para este ejemplo, proporcionaremos el valor &quot;12345&quot; como asunto y &quot;adobe&quot; como idp.


![](assets/session-init-params-frstapp.png)

Realice la llamada de inicialización de la sesión. Recibirá la siguiente respuesta:


![](assets/session-init-result-first-app.png)


Todos los datos que necesitamos están en los encabezados de respuesta. El **Ubicación** encabezado representa el id de la nueva sesión creada y el **Fecha** y **Caduca** Los encabezados de representan los valores utilizados para programar la aplicación y realizar el siguiente latido para mantener la sesión activa.

#### Heartbeat {#heartbeat}

Haz una llamada de Heartbeat. Proporcione el **session id** obtenido en la llamada de inicialización de sesión, junto con la variable **sujeto** y **idp** parámetros utilizados.

![](assets/heartbeat.png)


Si la sesión sigue siendo válida (no ha caducado o se ha eliminado manualmente), recibirá un resultado satisfactorio:

![](assets/heartbeat-succesfull-result.png)

Como en el primer caso, utilizaremos la variable **Fecha** y **Caduca** encabezados para programar otro latido para esta sesión en particular. Si la sesión ya no es válida, esta llamada generará un error con el código de estado HTTP 410 GONE.

Puede utilizar la opción &quot;Mantener el flujo activo&quot; disponible en la interfaz de usuario de Swagger para ejecutar latidos automáticos en una sesión específica, lo que puede ayudarle a probar una regla sin tener que preocuparse por la plantilla necesaria para realizar latidos de sesión oportunos. Este botón se coloca junto al botón &quot;Probar&quot; en la pestaña Swagger Heartbeat. Para establecer un latido automático para todas las sesiones creadas, debe programarlas cada una en una interfaz de usuario de Swagger independiente abierta en una pestaña del explorador web.

![](assets/keep-stream-alive.png)

#### Finalización de sesión {#session-termination}

El caso empresarial de su empresa puede requerir que la Monitorización de concurrencia termine una sesión específica cuando, por ejemplo, un usuario deje de ver un vídeo. Esto se puede hacer realizando una llamada al DELETE en el recurso de sesiones.

![](assets/delete-session.png)

Utilice los mismos parámetros para la llamada que para el latido de la sesión. Los códigos de estado HTTP de respuesta son:

* 202 ACEPTADO para obtener una respuesta correcta
* 410 SE HA IDO si la sesión ya se ha detenido.

#### Infracción de la directiva {#breaking-policy-app-first}


Para simular el comportamiento de nuestra aplicación cuando se rompe la política de 3 flujos asignada a ella, necesitamos hacer 3 llamadas para la inicialización de la sesión. Para que la directiva surta efecto, las llamadas deben realizarse antes de que caduque una de las sesiones debido a la falta de latidos. Veremos que todas estas llamadas tienen éxito, pero si hacemos una cuarta, fallará con el siguiente error:

![](assets/breaking-policy-frstapp.png)


Obtenemos una respuesta 409 CONFLICT junto con un objeto de resultado de evaluación en la carga útil. Lea una descripción completa del resultado de la evaluación en la [Especificación de API de Swagger](http://docs.adobeptime.io/cm-api-v2/#evaluation-result).

La aplicación puede utilizar la información del resultado de la evaluación para mostrar un determinado mensaje al usuario al detener el vídeo y para realizar más acciones si es necesario. Un caso de uso puede ser detener otros flujos existentes para iniciar uno nuevo. Esto se realiza mediante el uso de **completionCode** valor presente en el **conflictos** para un atributo en conflicto específico. El valor se proporcionará como encabezado HTTP X-Terminate en la llamada para una nueva inicialización de sesión.

![](assets/session-init-termination-code.png)

Al proporcionar uno o más códigos de finalización en la inicialización de la sesión, la llamada se realizará correctamente y se generará una nueva sesión. Entonces, si intentamos hacer un latido con una de las sesiones que se han detenido remotamente, obtendremos una respuesta 410 GONE con una carga útil de resultado de evaluación que describe el hecho de que la sesión se ha terminado remotamente, como en el ejemplo:

![](assets/remote-termination.png)

### Segunda aplicación {#second-application}

La otra aplicación de ejemplo que utilizaremos es la que tiene ID **demo-app-2**. A este se le ha asignado una directiva con una regla que limita el número de flujos disponibles para un canal a un máximo de 2.   Debe proporcionar la variable channel para evaluar esta directiva.

#### Recuperando metadatos {#retrieving-metadata}

Establezca el nuevo ID de aplicación en la esquina superior derecha de la página y realice una llamada al recurso de metadatos. Recibirá la siguiente respuesta:

![](assets/non-empty-metadata-secndapp.png)

Esta vez, el cuerpo de respuesta ya no es una lista vacía, como en el ejemplo de la primera aplicación. Ahora, el Servicio de supervisión de concurrencia indica en el cuerpo de respuesta que la variable **canal** se requieren metadatos en la inicialización de la sesión para evaluar la directiva.

Si realiza una llamada a sin proporcionar un valor para **canal** , obtendrá lo siguiente:

* Código de respuesta: 400 SOLICITUD INCORRECTA
* Cuerpo de respuesta: una carga útil de resultado de evaluación que describe en la variable **obligaciones** especifique lo que se espera en la solicitud de inicialización de sesión para que la operación se realice correctamente.

![](assets/metadata-request-secndapp.png)


#### Inicialización de sesión {#session-init}

Asigne un valor a la clave de metadatos requerida y configúrela como parámetro de formulario en la solicitud de inicialización de sesión, como se muestra a continuación:

![](assets/session-init-params-secndapp.png)

Ahora la llamada se realizará correctamente y se generará una nueva sesión.


#### Infracción de la directiva {#breaking-policy-second-app}

Para romper la regla que tenemos en la política asignada a esta aplicación, necesitamos hacer 2 llamadas con el mismo valor de canal. Al igual que en el primer ejemplo, la segunda llamada debe realizarse mientras la primera sesión generada sigue siendo válida.

![](assets/breaking-policy-secnd-app.png)

Si utilizamos valores diferentes para los metadatos de canal cada vez que creamos una nueva sesión, todas las llamadas se realizarán correctamente porque el umbral de 2 se establece para cada valor de forma individual.

Al igual que en el primer ejemplo, podemos usar el código de terminación para detener de forma remota los flujos en conflicto o podemos esperar a que uno de los flujos caduque, suponiendo que no se opere ningún latido en ellos.

