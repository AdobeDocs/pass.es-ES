---
title: Guía de iOS/tvOS
description: Guía de iOS/tvOS
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2402'
ht-degree: 0%

---

# Guía del SDK para iOS/tvOS {#iostvos-sdk-cookbook}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#intro}

En este documento se describen los flujos de trabajo de derechos que la aplicación de nivel superior de un programador puede implementar a través de las API expuestas por la biblioteca AccessEnabler de iOS/tvOS.

La solución de asignación de derechos de autenticación de Adobe Pass para iOS/tvOS se divide finalmente en dos dominios:

* Dominio de interfaz de usuario: es la capa de aplicación de nivel superior que implementa la interfaz de usuario y utiliza los servicios proporcionados por la biblioteca AccessEnabler para proporcionar acceso al contenido restringido.

* Dominio de AccessEnabler: aquí es donde se implementan los flujos de trabajo de asignación de derechos en forma de:

   * Llamadas de red realizadas a los servidores back-end de Adobe
   * Reglas de lógica empresarial relacionadas con los flujos de trabajo de autenticación y autorización
   * Administración de varios recursos y procesamiento del estado del flujo de trabajo (como la caché de símbolos)

El objetivo del dominio AccessEnabler es ocultar todas las complejidades de los flujos de trabajo de asignación de derechos y proporcionar a la aplicación de capa superior (a través de la biblioteca AccessEnabler) un conjunto de primitivas de asignación de derechos simples con las que implementar los flujos de trabajo de asignación de derechos:

1. Establecer la identidad del solicitante
1. Comprobación y obtención de autenticación con un proveedor de identidad concreto
1. Comprobación y obtención de autorización para un recurso concreto
1. Cerrar sesión
1. El SSO de Apple fluye mediante la indexación del marco VSA de Apple

La actividad de red de AccessEnabler tiene lugar en su propio subproceso, por lo que el subproceso de la interfaz de usuario nunca se bloquea. Como resultado, el canal de comunicación bidireccional entre los dos dominios de aplicación debe seguir un patrón totalmente asincrónico:

* La capa de aplicación de la interfaz de usuario envía mensajes al dominio AccessEnabler a través de las llamadas de API expuestas por la biblioteca AccessEnabler.
* AccessEnabler responde a la capa de interfaz de usuario mediante los métodos de devolución de llamada incluidos en el protocolo AccessEnabler que la capa de interfaz de usuario registra con la biblioteca AccessEnabler.

## Configuración del servicio de ID de Experience Cloud (ID de visitante) {#visitorIDSetup}

La configuración del valor [ID de Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/home.html) es importante desde el punto de vista de [!DNL Analytics]. Una vez establecido el valor `visitorID`, el SDK envía esta información junto con cada llamada de red y el servidor de autenticación [!DNL Adobe Pass] recopila esta información. Puede correlacionar los análisis del servicio de autenticación de Adobe Pass con cualquier otro informe de análisis que pueda tener de otras aplicaciones o sitios web. Encontrará información sobre cómo configurar visitorID [aquí](#setOptions).

## Flujos de derecho {#entitlement}

A. [Requisitos previos](#prereqs) </br>
B. [Flujo de inicio](#startup_flow) </br>
C. [Flujo de autenticación con Apple SSO](#authn_flow_wo_applesso) </br>
D. [Flujo de autenticación con Apple SSO en iOS](#authn_flow_with_applesso) </br>
E. [Flujo de autenticación con Apple SSO en tvOS](#authn_flow_with_applesso_tvOS) </br>
F. [Flujo de autorización](#authz_flow) </br>
G. [Flujo de medios de vista](#media_flow) </br>
H. [Flujo de cierre de sesión sin Apple SSO](#logout_flow_wo_AppleSSO) </br>
I. [Flujo de cierre de sesión con Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Requisitos previos {#prereqs}

1. Cree sus funciones de devolución de llamada:
   * `setRequestorComplete()` </br>
   * Activado por [setRequestor()](#$setReq), devuelve un resultado correcto o erróneo. </br>
   * El éxito indica que se puede continuar con las llamadas de asignación de derechos.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Activado por [`getAuthentication()`](#$getAuthN) solo si el usuario no ha seleccionado ningún proveedor (MVPD) y aún no se ha autenticado. </br>
      * El parámetro `mvpds` es una matriz de proveedores disponibles para el usuario.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Activado por `checkAuthentication()` cada vez. </br>
      * Activado por [`getAuthentication()`](#$getAuthN) solo si el usuario ya se ha autenticado y ha seleccionado un proveedor. </br>
      * El estado devuelto es éxito o error, el código de error describe el tipo de error.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Activado por [`getAuthentication()`](#$getAuthN) después de que el usuario seleccione una MVPD. El parámetro `url` proporciona la ubicación de la página de inicio de sesión de la MVPD.

   * `sendTrackingData(event, data)` </br>
      * Activado por `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * El parámetro `event` indica qué evento de asignación de derechos se produjo; el parámetro `data` es una lista de valores relacionados con el evento.

   * `setToken(token, resource)`

      * Activado por [checkAuthorization()](#checkAuthZ) y [getAuthorization()](#$getAuthZ) tras una autorización correcta para ver un recurso.
      * El parámetro `token` es el token de medios de corta duración; el parámetro `resource` es el contenido que el usuario tiene autorización para ver.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Activado por [checkAuthorization()](#checkAuthZ) y [getAuthorization()](#$getAuthZ) tras una autorización incorrecta.
      * El parámetro `resource` es el contenido que el usuario intentaba ver; el parámetro `code` es el código de error que indica qué tipo de error se produjo; el parámetro `description` describe el error asociado con el código de error.

   * `selectedProvider(mvpd)` </br>
      * Activado por [`getSelectedProvider()`](#getSelProv).
      * El parámetro `mvpd` proporciona información sobre el proveedor seleccionado por el usuario.

   * `setMetadataStatus(metadata, key, arguments)`
      * Activado por `getMetadata().`
      * El parámetro `metadata` proporciona los datos específicos solicitados; el parámetro `key` es la clave utilizada en la solicitud [getMetadata()](#getMeta); y el parámetro `arguments` es el mismo diccionario que se pasó a [getMetadata()](#getMeta).

   * [preauthorizedResources(authorizedResources)](#preauthResources)

      * Activado por [`checkPreauthorizedResources()`](#checkPreauth).

      * El parámetro `authorizedResources` presenta los recursos que el usuario
tiene autorización para ver.

   * [`presentTvProviderDialog(viewController)`](#presentTvDialog)

      * Se activa por [getAuthentication()](#getAuthN) cuando el solicitante actual admite al menos en MVPD compatibles con SSO.
      * El parámetro viewController es el cuadro de diálogo de SSO de Apple y debe presentarse en el controlador de vista principal.

   * [`dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Se activa mediante una acción del usuario (seleccionando &quot;Cancelar&quot; u &quot;Otros proveedores de TV&quot; en el cuadro de diálogo de SSO de Apple).
      * El parámetro viewController es el cuadro de diálogo de SSO de Apple y debe descartarse del controlador de vista principal.

![](../../../../assets/iOS-flows.png)

### B. Flujo de inicio {#startup_flow}

1. Iniciar la aplicación de nivel superior.</br>
1. Iniciar autenticación de Adobe Pass </br>

   a. Llame a [`init`](#$init) para crear una sola instancia del activador de acceso de autenticación de Adobe Pass.
   * **Dependencia:** Biblioteca iOS/tvOS nativa de autenticación de Adobe Pass (AccessEnabler)

   b. Llame a `setRequestor()` para establecer la identidad del programador; pase `requestorID` del programador y (opcionalmente) una matriz de extremos de autenticación de Adobe Pass. Para tvOS también deberá proporcionar la clave pública y el secreto. Consulte [Documentación sin cliente](#create_dev) para obtener más información.

   * **Dependencia:** ID de solicitante de autenticación de Adobe Pass válido (trabaje con su cuenta de autenticación de Adobe Pass)
Responsable para organizar esto).

   * **Déclencheur:**
     [setRequestorComplete()](#$setReqComplete) llamada de retorno.

   >[!NOTE]
   >
   >No se pueden completar solicitudes de asignación de derechos hasta que se haya establecido completamente la identidad del solicitante. Esto significa que mientras [`setRequestor()`](#$setReq) sigue ejecutándose, todas las solicitudes de derechos subsiguientes. Por ejemplo, [`checkAuthentication()`](#checkAuthN) están bloqueados.

   Tiene dos opciones de implementación: una vez que la información de identificación del solicitante se envía al servidor back-end, la capa de aplicación de la interfaz de usuario puede elegir uno de los dos enfoques siguientes: </br>

   1. Espere a que se active la llamada de retorno [`setRequestorComplete()`](#setReqComplete) (parte del delegado AccessEnabler). Esta opción proporciona la mayor certeza de que [`setRequestor()`](#$setReq) se completó, por lo que se recomienda para la mayoría de las implementaciones.

   1. Continúe sin esperar a que se active la llamada de retorno [`setRequestorComplete()`](#setReqComplete) y comience a emitir solicitudes de asignación de derechos. Estas llamadas (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) están en cola por la biblioteca AccessEnabler, que realizará las llamadas de red reales después de [`setRequestor()`](#$setReq). Esta opción se puede interrumpir ocasionalmente si, por ejemplo, la conexión de red es inestable.

1. Llame a `checkAuthentication()` para comprobar si hay una autenticación existente sin iniciar el flujo de autenticación completo.  Si esta llamada se realiza correctamente, puede continuar directamente al flujo de autorización. Si no es así, continúe con el flujo de autenticación.

   * **Dependencia:** Una llamada correcta a [setRequestor()](#$setReq) (esta dependencia también se aplica a todas las llamadas subsiguientes).

   * **Déclencheur:** [setAuthenticationStatus()](#$setAuthNStatus) devolución de llamada.


### C. Flujo de autenticación sin SSO de Apple {#authn_flow_wo_applesso}

1. Llame a [`getAuthentication()`](#$getAuthN) para iniciar el flujo de autenticación o para obtener confirmación de que el usuario ya está
autenticado.

   **Déclencheur:**

   * La llamada de retorno [setAuthenticationStatus()](#$setAuthNStatus), si el usuario ya está autenticado. En este caso, continúe directamente al [Flujo de autorización](#authz_flow).

   * La llamada de retorno [displayProviderDialog()](#$dispProvDialog), si el usuario aún no está autenticado.

1. Presentar al usuario la lista de proveedores enviados a
   [`displayProviderDialog()`](#dispProvDialog).

1. Una vez que el usuario seleccione un proveedor, obtenga la URL de la MVPD del usuario de la llamada de retorno `navigateToUrl:` o `navigateToUrl:useSVC:`, abra un controlador `UIWebView/WKWebView` o `SFSafariViewController` y dirija ese controlador a la URL.

1. A través de `UIWebView/WKWebView` o `SFSafariViewController` creados en el paso anterior, el usuario aterriza en la página de inicio de sesión de la MVPD e introduce credenciales de inicio de sesión. Se realizan varias operaciones de redirección en el controlador.</br>

>[!NOTE]
>
>En este punto, el usuario tiene la oportunidad de cancelar el flujo de autenticación. Si esto sucede, la capa de interfaz de usuario es responsable de informar a AccessEnabler sobre este evento llamando a [setSelectedProvider()](#setSelProv) con `null` como parámetro. Esto permite al AccessEnabler limpiar su estado interno y restablecer el flujo de autenticación.

1. Una vez que el usuario ha iniciado sesión correctamente, la capa de aplicación detecta la carga de una dirección URL personalizada específica. Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Solo su aplicación debe interpretarlo como una señal de que el flujo de autenticación se ha completado y de que es seguro cerrar el controlador `UIWebView/WKWebView` o `SFSafariViewController`. En caso de que se requiera el uso de un controlador `SFSafariViewController`, la dirección URL personalizada específica está definida por **`application's custom scheme`** (p. ej.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica está definida por la constante **`ADOBEPASS_REDIRECT_URL`** (es decir, `adobepass://ios.app`).

1. Cierre el controlador UIWebView/WKWebView o SFSafariViewController y llame al método de API `handleExternalURL:url` de AccessEnabler, que indica a AccessEnabler que recupere el token de autenticación del servidor back-end.

1. (Opcional) Llame a [`checkPreauthorizedResources(resources)`](#$checkPreauth) para comprobar qué recursos puede ver el usuario. El parámetro `resources` es una matriz de recursos protegidos asociados al token de autenticación del usuario. Un uso de la información de autorización obtenida del MVPD del usuario es decorar su interfaz de usuario (por ejemplo, símbolos bloqueados/desbloqueados junto al contenido protegido).

   * **Déclencheur:** [`preauthorizedResources()`](#preauthResources) devolución de llamada
   * **Punto de ejecución:** después del flujo de autenticación completado

1. Si la autenticación se ha realizado correctamente, continúe con el Flujo de autorización.

### D. Flujo de autenticación con Apple SSO en iOS {#authn_flow_with_applesso}

1. Llame a [`getAuthentication()`](#$getAuthN) para iniciar el flujo de autenticación o para obtener confirmación de que el usuario ya se ha autenticado.
   **Déclencheur:**

   * La llamada de retorno [presentTvProviderDialog()](#presentTvDialog), si el usuario no está autenticado y el solicitante actual tiene al menos en MVPD que admite SSO. Si ninguna MVPD admite SSO, se utilizará el flujo de autenticación clásico.

1. Una vez que el usuario selecciona un proveedor, la biblioteca AccessEnabler obtiene un token de autenticación con la información proporcionada por el marco de VSA de Apple.

1. Se desencadenará la devolución de llamada [setAuthenticationsStatus()](#setAuthNStatus). En este punto, el usuario debe autenticarse con Apple SSO.

1. [Opcional] Llame a [`checkPreauthorizedResources(resources)`](#$checkPreauth) para comprobar qué recursos puede ver el usuario. El parámetro `resources` es una matriz de recursos protegidos asociados al token de autenticación del usuario. Un uso de la información de autorización obtenida del MVPD del usuario es decorar su interfaz de usuario (por ejemplo, símbolos bloqueados / desbloqueados junto al contenido protegido).

   * **Déclencheur:** [`preauthorizedResources()`](#preauthResources) devolución de llamada
   * **Punto de ejecución:** después del flujo de autenticación completado

1. Si la autenticación se ha realizado correctamente, continúe con el Flujo de autorización.

### E. Flujo de autenticación con Apple SSO en tvOS {#authn_flow_with_applesso_tvOS}

1. Llamar a [`getAuthentication()`](#$getAuthN) para iniciar
flujo de autenticación o para obtener confirmación de que el usuario ya está
autenticado.
   **Déclencheur:**
   * La llamada de retorno [`presentTvProviderDialog()`](#presentTvDialog), si el usuario no está autenticado y el solicitante actual tiene al menos una MVPD que admita SSO. Si ninguna MVPD admite SSO, se utilizará el flujo de autenticación clásico.

1. Una vez que el usuario seleccione un proveedor, se llamará a la devolución de llamada [`status()`](#status_callback_implementation). Se proporcionará un código de registro y la biblioteca AccessEnabler comenzará a sondear el servidor para obtener una autenticación de segunda pantalla correcta.

1. Si el código de registro proporcionado se utilizó para autenticarse correctamente en la segunda pantalla, se activará la devolución de llamada [`setAuthenticatiosStatus()`](#setAuthNStatus). En este punto, el usuario debe autenticarse con Apple SSO.
1. [Opcional] Llame a [`checkPreauthorizedResources(resources)`](#$checkPreauth) para comprobar qué recursos puede ver el usuario. El parámetro `resources` es una matriz de recursos protegidos asociados al token de autenticación del usuario. Un uso de la información de autorización obtenida del MVPD del usuario es decorar su interfaz de usuario (por ejemplo, símbolos bloqueados / desbloqueados junto al contenido protegido).

   * **Déclencheur:** [`preauthorizedResources()`](#preauthResources) devolución de llamada

   * **Punto de ejecución:** después del flujo de autenticación completado
1. Si la autenticación se ha realizado correctamente, continúe con el Flujo de autorización.

### F. Flujo de autorización {#authz_flow}

1. Llame a [getAuthorization()](#$getAuthZ) para iniciar el flujo de autorización.

   * **Dependencia:** ResourceID válidos acordados con los MVPD.
   * Los ID de recurso deben ser los mismos que se usan en cualquier otro dispositivo o plataforma, y serán los mismos en todas las MVPD. Para obtener información sobre los identificadores de recursos, consulte [Identificación de recursos protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)

1. Valide la autenticación y la autorización.

   * Si la llamada a [getAuthorization()](#$getAuthZ) se realiza correctamente: El usuario tiene tokens AuthN y AuthZ válidos (el usuario está autenticado y autorizado para ver los medios solicitados).

   * Si [getAuthorization()](#$getAuthZ) falla: Examine la excepción lanzada para determinar su tipo (AuthN, AuthZ o algo más):
      * Si se ha producido un error de autenticación (AuthN), reinicie el flujo de autenticación.
      * Si se trata de un error de autorización (AuthZ), el usuario no tiene autorización para ver los medios solicitados y se le debe mostrar algún tipo de mensaje de error.
      * Si se ha producido algún otro tipo de error (error de conexión, error de red, etc.), muestre un mensaje de error apropiado al usuario.

1. Valide el token de medios corto.\
   Utilice la biblioteca del Comprobador de tokens de medios de autenticación de Adobe Pass para comprobar el token de medios de corta duración devuelto desde la llamada a [getAuthorization()](#$getAuthZ) anterior:

   * Si la validación se realiza correctamente: Reproduzca el medio solicitado para el usuario.
   * Si la validación falla: El token de AuthZ no era válido, la solicitud de medios debe rechazarse y se debe mostrar un mensaje de error al usuario.


1. Vuelva al flujo normal de aplicaciones.

### G. Ver flujo de medios {#media_flow}

1. El usuario selecciona los medios que desea ver.
1. ¿Están protegidos los medios? La aplicación comprueba si los medios seleccionados están protegidos:

   * Si el medio seleccionado está protegido, su aplicación inicia el [Flujo de autorización](#authz_flow) anterior.

   * Si el medio seleccionado no está protegido, reproduzca el medio durante
el usuario.

### H. Flujo de cierre de sesión sin Apple SSO {#logout_flow_wo_AppleSSO}

1. Llame a [`logout()`](#$logout) para cerrar la sesión del usuario. AccessEnabler borra todos los valores y tokens almacenados en caché. Después de borrar la caché, AccessEnabler realiza una llamada al servidor para limpiar las sesiones del lado del servidor. Tenga en cuenta que, dado que la llamada al servidor podría provocar un redireccionamiento de SAML al IdP (esto permite la limpieza de la sesión en el lado del IdP), esta llamada debe seguir todas las redirecciones. Por este motivo, esta llamada debe gestionarse dentro de un controlador UIWebView/WKWebView o SFSafariViewController.

   a. Siguiendo el mismo patrón que el flujo de trabajo de autenticación, el dominio AccessEnabler realiza una solicitud a la capa de aplicación de la interfaz de usuario, a través de la llamada de retorno `navigateToUrl:` o `navigateToUrl:useSVC:`, para crear un controlador UIWebView/WKWebView o SFSafariViewController e indicar a que cargue la dirección URL proporcionada en el parámetro `url` de la llamada de retorno. Dirección URL del extremo de cierre de sesión en el servidor back-end.

   b. La aplicación debe supervisar la actividad del controlador `UIWebView/WKWebView or SFSafariViewController` y detectar el momento en que carga una dirección URL personalizada específica, a medida que pasa por varias redirecciones. Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Solo su aplicación debe interpretarlo como una señal de que el flujo de cierre de sesión se ha completado y de que es seguro cerrar el controlador `UIWebView/WKWebView` o `SFSafariViewController`. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar el controlador `UIWebView/WKWebView or SFSafariViewController` y llamar al método de API `handleExternalURL:url`de AccessEnabler. En caso de que se requiera el uso de un controlador `SFSafariViewController`, la dirección URL personalizada específica está definida por **`application's custom scheme`** (por ejemplo, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica está definida por la constante **`ADOBEPASS_REDIRECT_URL`** (es decir, `adobepass://ios.app`).

   >[!NOTE]
   >
   >El flujo de cierre de sesión difiere del flujo de autenticación en que el usuario no tiene que interactuar con UIWebView/WKWebView o SFSafariViewController de ninguna manera. La capa de aplicación de la interfaz de usuario utiliza un UIWebView/WKWebView o SFSafariViewController para asegurarse de que se siguen todas las redirecciones. Por lo tanto, es posible (y recomendado) hacer que el controlador sea invisible (es decir, oculto) durante el proceso de cierre de sesión.


### I. Flujo de cierre de sesión con Apple SSO {#logout_flow_with_AppleSSO}

1. Llame a [`logout()`](#$logout) para cerrar la sesión del usuario.
1. Se llamará a la devolución de llamada [status()](#status_callback_implementation) con el ID VSA203.
1. También se debe indicar al usuario que inicie sesión desde la configuración del sistema. Si no se hace esto, se volverá a autenticar cuando se reinicie la aplicación.



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
