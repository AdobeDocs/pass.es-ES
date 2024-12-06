---
title: Flujo de asignación de derechos del programador
description: Flujo de asignación de derechos del programador
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# Flujo de asignación de derechos del programador {#prog-entitlement-flow}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

En este documento se describe el flujo de derechos básico desde la perspectiva del programador.  Para obtener información sobre características y casos de uso que van más allá de la integración básica de TVE que se trata aquí, consulte [Casos de uso del programador](/help/authentication/integration-guide-programmers/programmer-use-cases.md).

La autenticación de Adobe Pass media el flujo de asignación de derechos entre los programadores y las MVPD al proporcionar interfaces seguras y coherentes para ambas partes.  Del lado del programador, la autenticación de Adobe Pass proporciona dos tipos generales de interfaz de asignación de derechos:

1. AccessEnabler: componente de cliente que proporciona una biblioteca de API para aplicaciones en dispositivos que pueden procesar páginas web (por ejemplo, aplicaciones web, aplicaciones para smartphones y tabletas).
2. API sin cliente: servicios web RESTful para dispositivos que no pueden procesar páginas web (por ejemplo, decodificadores, consolas de juegos, televisores inteligentes). El requisito para procesar páginas web proviene del requisito de la MVPD de que los usuarios se autentiquen en el sitio web de la MVPD.

Además de la descripción general neutra para la plataforma que se presenta aquí, hay una descripción general específica de la API sin cliente aquí: Documentación de la API sin cliente. AccessEnabler se ejecuta de forma nativa en plataformas compatibles (AS/JS en la web, Objective-C en iOS y Java en Android). Las API de AccessEnabler son coherentes en todas las plataformas admitidas. Todas las plataformas que no admiten AccessEnabler utilizan la misma API sin cliente.

Para ambos tipos de interfaz, la autenticación de Adobe Pass media de forma segura el flujo de asignación de derechos entre la aplicación del programador y la MVPD del usuario:

![](../assets/prog-entitlement-flow.png)


*Figura: Ecosistema de autenticación de Adobe Pass*

>[!IMPORTANT]
>
>Tenga en cuenta en el diagrama anterior que hay una parte del flujo de derechos que no pasa por los servidores de autenticación de Adobe Pass: el inicio de sesión de MVPD. Los usuarios deben iniciar sesión en la página de inicio de sesión de su MVPD. Debido a este requisito, en los dispositivos que no pueden procesar páginas web, la aplicación del programador debe indicar a los usuarios que cambien a un dispositivo compatible con la web para iniciar sesión con su MVPD, después de lo cual regresarán al dispositivo original durante el resto del flujo de asignación de derechos.

## Flujo de derecho {#entitlement-flow}

Hay cuatro subflujos distintos que constituyen la asignación básica de derechos
flujo:

1. [Flujo de inicio](/help/authentication/integration-guide-programmers/entitlement-flow.md#startup)
1. [Flujo de autenticación](/help/authentication/integration-guide-programmers/entitlement-flow.md#authentication)
1. [Flujo de autorización](/help/authentication/integration-guide-programmers/entitlement-flow.md#authorization)
1. [Cerrar sesión FLow](/help/authentication/integration-guide-programmers/entitlement-flow.md#logout)

En la visita inicial de un usuario al sitio de un programador, el flujo de derechos se ejecuta en el orden anterior. Sin embargo, en visitas posteriores, dependiendo de si los tokens de autenticación y autorización han caducado o no, o según las políticas de visualización, un usuario solo puede pasar por uno o dos de los subflujos.

### Flujo de inicio {#startup}

Establece la identidad del programador y el dispositivo, realiza tareas de inicialización. Esto es un requisito previo para todas las llamadas de derechos subsiguientes.

**Habilitador de acceso**

* **`setRequestor()`**: establece su identidad con AccessEnalber y, por extensión, con los servidores de autenticación de Adobe Pass. Esta llamada es un precursor del resto del flujo de derechos. Por ejemplo, en JavaScript:

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**API sin cliente**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`**: según la plataforma, puede haber tareas previas que completar antes de que la aplicación vuelva a codificar las llamadas. Consulte la **documentación de API sin cliente** para obtener más información. Por ejemplo, las plataformas Xbox requieren que complete los pasos de seguridad prescritos antes de llamar a regcode.

### Flujo de autenticación {#authentication}

La autenticación correcta genera un token de AuthN vinculado al dispositivo y al solicitante. La autenticación correcta es un requisito previo para la autorización.

**Habilitador de acceso**

* `checkAuthentication()`: comprueba si existe un token de autenticación en caché válido en la caché de tokens local, sin activar realmente el flujo de autenticación completo. Esto déclencheur la función de devolución de llamada `setAuthenticationStatus()`.
* `getAuthentication()`: inicia el flujo de autenticación completo. Si se realiza correctamente, la autenticación de Adobe Pass genera un token de AuthN y lo almacena en caché en el cliente. El usuario inicia sesión en el sitio de MVPD seleccionado, que se presenta en un iFrame, una ventana emergente o una vista web, según la plataforma. Esto déclencheur displayProviderDialog().

**API sin cliente**

* `<FQDN>/.../checkauthn` - La versión del servicio web de `checkAuthentication()` anterior.
* `<FQDN>/.../config`: devuelve la lista de MVPD a la aplicación de segunda pantalla.
* `<FQDN>/.../authenticate`: inicia el flujo de autenticación desde la aplicación de segunda pantalla, redireccionando a los usuarios a su MVPD seleccionada para el inicio de sesión. Si se realiza correctamente, la autenticación de Adobe Pass genera un token de AuthN y lo almacena en el servidor, y el usuario vuelve a su dispositivo original para completar el flujo de derechos.

Un token de AuthN se considera válido si se cumplen los dos puntos siguientes:

* El token de AuthN no ha caducado
* La MVPD asociada con el token de AuthN está en la lista de MVPD permitidas para el ID del solicitante actual

#### Flujo de trabajo de autenticación inicial de Generic AccessEnabler {#generic-ae-initial-authn-flow}

1. Su aplicación inicia el flujo de trabajo de autenticación con una llamada a `getAuthentication()`, que busca un token de autenticación en caché válido. Este método tiene un parámetro `redirectURL` opcional; si no proporciona un valor para `redirectURL`, después de una autenticación correcta, el usuario vuelve a la dirección URL desde la que se inicializó la autenticación.
1. AccessEnabler determina el estado de autenticación actual. Si el usuario está autenticado actualmente, AccessEnabler llama a la función de devolución de llamada `setAuthenticationStatus()` y pasa un estado de autenticación que indica que la función se ha realizado correctamente.
1. Si el usuario no está autenticado, AccessEnabler continúa el flujo de autenticación determinando si el último intento de autenticación del usuario se realizó correctamente con una MVPD determinada. Si se almacena en caché un ID de MVPD Y el indicador `canAuthenticate` es verdadero O si se seleccionó una MVPD con `setSelectedProvider()`, no se pide al usuario el cuadro de diálogo de selección de MVPD. El flujo de autenticación sigue utilizando el valor almacenado en caché de la MVPD (es decir, la misma MVPD que se utilizó durante la última autenticación correcta). Se realiza una llamada de red al servidor back-end y se redirige al usuario a la página de inicio de sesión de MVPD.

1. Si no se almacena en caché ningún ID de MVPD Y no se seleccionó ninguna MVPD con `setSelectedProvider()` O si el indicador `canAuthenticate` está establecido en falso, se llama a la devolución de llamada `displayProviderDialog()`. Esta llamada de retorno indica a la aplicación que cree la interfaz de usuario que presenta al usuario una lista de MVPD para elegir. Se proporciona una matriz de objetos MVPD, que contiene la información necesaria para crear el selector MVPD. Cada objeto de MVPD describe una entidad de MVPD y contiene información como el ID de la MVPD y la dirección URL donde se puede encontrar el logotipo de MVPD.

1. Una vez seleccionada una MVPD, la aplicación debe informar al AccessEnabler de la elección del usuario. Para los clientes que no son de Flash, una vez que el usuario selecciona la MVPD deseada, se informa al AccessEnabler de la selección del usuario mediante una llamada al método `setSelectedProvider()`. En su lugar, los clientes de Flash distribuyen un(a) `MVPDEvent` compartido(a) de tipo &quot;`mvpdSelection`&quot;, pasando el proveedor seleccionado.

1. Cuando se informa al AccessEnabler de la selección de MVPD por parte del usuario, se realiza una llamada de red al servidor back-end y se redirige al usuario a la página de inicio de sesión de MVPD.

1. Dentro del flujo de trabajo de autenticación, AccessEnabler se comunica con la autenticación de Adobe Pass y la MVPD seleccionada para solicitar las credenciales del usuario (ID de usuario y contraseña) y comprobar su identidad. Mientras que algunas MVPD redirigen a su propio sitio para el inicio de sesión, otras requieren que muestre su página de inicio de sesión dentro de un iFrame. Su página debe incluir la llamada de retorno que crea un iFrame, en caso de que el cliente elija una de esas MVPD.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->.

1. Una vez que el usuario ha iniciado sesión correctamente, AccessEnabler recupera el token de autenticación e informa a la aplicación de que el flujo de autenticación ha finalizado. AccessEnabler llama a la llamada de retorno `setAuthenticationStatus()` con un código de estado de 1, que indica que se ha realizado correctamente. Si hay un error durante la ejecución de estos pasos, la llamada de retorno `setAuthenticationStatus()` se activa con un código de estado de 0, lo que indica un error de autenticación, así como un código de error correspondiente.

>[!IMPORTANT]
>Comcast es el único MVPD en este momento que no proporciona una URL estática para el logotipo. Los programadores deben extraer los logotipos actualizados más recientes del [Portal para desarrolladores de XFINITY](https://developers.xfinity.com/products/tv-everywhere).
>

### Flujo de autorización {#authorization}

La autorización es un requisito previo para ver contenido protegido. La autorización correcta genera un token de AuthZ, junto con un token de medios de corta duración que se proporciona a la aplicación del programador por motivos de seguridad. Tenga en cuenta que, para admitir el flujo de trabajo de autorización, debe haber realizado previamente la configuración del solicitante necesaria y haber integrado el [Comprobador de token de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md). Una vez completados, puede iniciar la autorización.

La aplicación inicia la autorización cuando un usuario solicita acceso a un recurso protegido. Se pasa un ID de recurso que especifica el recurso solicitado (por ejemplo, un canal, un episodio, etc.). La aplicación comprueba primero si hay un token de autenticación almacenado. Si no se encuentra ninguna, se inicia el proceso de autenticación.

**Habilitador de acceso**

* `checkAuthorization()`: comprueba la autorización sin iniciar el flujo de autorización completo. Esto se utiliza a menudo para actualizar la información de estado que se muestra en la interfaz de usuario de la aplicación del programador.

* `getAuthorization()`: inicia el flujo de autorización completo.

Proporcione las siguientes funciones de llamada de retorno para controlar los resultados de
la llamada de autorización:

* `setToken()`: si la autenticación se realizó correctamente anteriormente y la autorización se realiza correctamente, AccessEnabler llama a la función de devolución de llamada `setToken()` y pasa el token de medios de corta duración, lo que indica que el flujo de derechos de autenticación de Adobe Pass ha finalizado correctamente. (Antes de permitir que el usuario vea contenido protegido, la aplicación del programador comprueba la validez del token de medios con el verificador de tokens de medios.

* `tokenRequestFailed()`: si el usuario no está autorizado para el recurso solicitado (o si la consulta falla por cualquier otro motivo), AccessEnabler llama a esta función de devolución de llamada (además de sus propias funciones de informe de errores) y pasa detalles sobre el error.

**API sin cliente**

* `\<FQDN\>/.../authorize`: inicia el flujo de autorización completo.

#### Flujo de trabajo de autorización de Generic AccessEnabler {#generic-ae-authr-wf}

1. Proporcione una función de devolución de llamada que registre el GUID de programador asignado con el Habilitador de acceso, usando `setReqestor()`. Se llama a esta función de devolución de llamada cuando se ha activado AccessEnabler
descargado correctamente.

1. Llame a `getAuthorization()` cuando un usuario solicite acceso a un recurso protegido. Con `getAuthorization()`, pase un ID de recurso que especifique el recurso solicitado (por ejemplo, un canal, un episodio, etc.). AccessEnabler busca un token de autenticación en caché para pasarlo con la solicitud de autorización. Si no se encuentra ninguna, inicia el flujo de autenticación.
1. Proporcione funciones de llamada de retorno para gestionar los resultados de la autorización:

   * `setToken()`: si la autorización se realiza correctamente o si el usuario ha sido autorizado anteriormente, el Habilitador de acceso continúa con el proceso de autorización llamando a la función de devolución de llamada `setToken()` y pasando el token de autorización de corta duración.

   * `tokenRequestFailed()`: si el usuario no está autorizado para el recurso solicitado (o si la consulta falla por cualquier otro motivo), AccessEnabler llama a cualquier función de informe de errores que haya registrado, además de la llamada de retorno `tokenRequestFailed()`, y pasa detalles sobre el error.

### Flujo de desconexión {#logout}

Borra los tokens y otros datos asociados con el flujo de derechos del usuario actual.

**Habilitador de acceso**

* `logout()`

**API sin cliente**

* `\<FQDN\>/.../logout`

## Comprender el comportamiento de AccessEnabler {#ae-behavior}

Todas las llamadas a la API de AccessEnabler son asincrónicas (con una excepción, indicada en las referencias de la API). Puede llamar a una API un número arbitrario de veces, pero no hay una garantía sólida de que las acciones se hayan activado
por las llamadas se completarán en el mismo orden en que se realizaron las llamadas. (Una excepción a esto es el tiempo de ejecución del Flash Player actual; al no ser de subprocesos múltiples, se asegurará de que las llamadas *do* se completen en el orden
se llaman.)

Para distinguir entre respuestas y poder emparejar respuestas con llamadas, todas las llamadas de retorno repiten sus parámetros de entrada. Esto incluye `setToken()` y `tokenRequestFailed()`, que se desencadenaron finalmente por `checkAuthorization()`. (Para las llamadas de retorno de `checkAuthorization()`, el recurso usado se devuelve). Aprovechando esta función, puede distinguir qué respuesta corresponde a cada llamada. Para utilizar esta función, puede codificar algo similar a lo siguiente:

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### Preguntas frecuentes sobre comportamiento de AEM {#ae-beh-faq}

**Pregunta. ¿Qué sucede si realizo una segunda llamada a AccessEnabler antes de que finalice la primera llamada?**

La primera llamada sigue ejecutándose mientras se ejecuta la segunda llamada (comunicaciones asincrónicas).

**Pregunta. ¿Hay un número máximo de llamadas simultáneas que AccessEnabler puede admitir?**

No se ha establecido ningún límite explícitamente en el código AccessEnabler, por lo que sólo está limitado por los recursos de sistema disponibles, así como por la capacidad de la MVPD.

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
