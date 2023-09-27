---
title: Guía de iOS/tvOS
description: Guía de iOS/tvOS
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2404'
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

Configuración de la [ID de Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/home.html) es importante desde el [!DNL Analytics] punto de vista. Una vez al `visitorID` cuando se establece, el SDK envía esta información junto con cada llamada de red y la variable [!DNL Adobe Pass] El servidor de autenticación recopila esta información. Puede correlacionar los análisis del servicio de autenticación de Adobe Pass con cualquier otro informe de análisis que pueda tener de otras aplicaciones o sitios web. Se puede encontrar información sobre cómo configurar visitorID [aquí](#setOptions).

## Flujos de derecho {#entitlement}

A.  [Requisitos previos](#prereqs) </br>
B.  [Flujo de inicio](#startup_flow) </br>
C.  [Flujo de autenticación con Apple SSO](#authn_flow_wo_applesso)  </br>
D.  [Flujo de autenticación con Apple SSO en iOS](#authn_flow_with_applesso) </br>
E.  [Flujo de autenticación con Apple SSO en tvOS](#authn_flow_with_applesso_tvOS) </br>
F..  [Flujo de autorización](#authz_flow) </br>
G.  [Ver flujo de medios](#media_flow) </br>
H..  [Flujo de cierre de sesión sin Apple SSO](#logout_flow_wo_AppleSSO) </br>
YO.  [Flujo de cierre de sesión con Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Requisitos previos {#prereqs}

1. Cree sus funciones de devolución de llamada:
   * `setRequestorComplete()` </br>
   * Activado por [setRequestor()](#$setReq), devuelve éxito o error. </br>
   * El éxito indica que se puede continuar con las llamadas de asignación de derechos.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Activado por [`getAuthentication()`](#$getAuthN) solo si el usuario no ha seleccionado un proveedor (MVPD) y aún no está autenticado. </br>
      * El `mvpds` El parámetro es una matriz de proveedores disponibles para el usuario.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Activado por `checkAuthentication()` cada vez. </br>
      * Activado por [`getAuthentication()`](#$getAuthN) solo si el usuario ya se ha autenticado y ha seleccionado un proveedor. </br>
      * El estado devuelto es éxito o error, el código de error describe el tipo de error.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Activado por [`getAuthentication()`](#$getAuthN) después de que el usuario seleccione una MVPD. El `url` proporciona la ubicación de la página de inicio de sesión de la MVPD.

   * `sendTrackingData(event, data)` </br>
      * Activado por `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * El `event` indica qué evento de asignación de derechos se produjo; el parámetro `data` parámetro es una lista de valores relacionados con el evento.

   * `setToken(token, resource)`

      * Activado por [checkAuthorization()](#checkAuthZ) y [getAuthorization()](#$getAuthZ) después de una autorización correcta para ver un recurso.
      * El `token` parámetro es el token de medios de corta duración; el `resource` parámetro es el contenido que el usuario tiene autorización para ver.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Activado por [checkAuthorization()](#checkAuthZ) y [getAuthorization()](#$getAuthZ) después de una autorización fallida.
      * El `resource` parámetro es el contenido que el usuario intentaba ver; la variable `code` parámetro es el código de error que indica qué tipo de error se produjo; la variable `description` describe el error asociado con el código de error.

   * `selectedProvider(mvpd)` </br>
      * Activado por [`getSelectedProvider()`](#getSelProv).
      * El `mvpd` proporciona información sobre el proveedor seleccionado por el usuario.

   * `setMetadataStatus(metadata, key, arguments)`
      * Activado por `getMetadata().`
      * El `metadata` proporciona los datos específicos solicitados; el parámetro `key` parámetro es la clave utilizada en el [getMetadata()](#getMeta) solicitud; y la `arguments` El parámetro es el mismo diccionario que se pasó a [getMetadata()](#getMeta).

   * [preauthorizedResources(authorizedResources)](#preauthResources)

      * Activado por [`checkPreauthorizedResources()`](#checkPreauth).

      * El `authorizedResources` presenta los recursos que el usuario está autorizado a ver.

   * [`presentTvProviderDialog(viewController)`](#presentTvDialog)

      * Activado por [getAuthentication()](#getAuthN) cuando el solicitante actual admite al menos una MVPD compatible con SSO.
      * El parámetro viewController es el cuadro de diálogo de SSO de Apple y debe presentarse en el controlador de vista principal.

   * [`dismissTvProviderDialog(viewController)`](#dismissTvDialog)

      * Se activa mediante una acción del usuario (seleccionando &quot;Cancelar&quot; u &quot;Otros proveedores de TV&quot; en el cuadro de diálogo de SSO de Apple).
      * El parámetro viewController es el cuadro de diálogo de SSO de Apple y debe descartarse del controlador de vista principal.

![](assets/iOS-flows.png)

### B. Flujo de inicio {#startup_flow}

1. Inicie la aplicación de nivel superior.</br>
1. Iniciar autenticación de Adobe Pass </br>

   a. Llamada [`init`](#$init) para crear una instancia única del Adobe Pass Authentication AccessEnabler.
   * **Dependencia:** Biblioteca iOS/tvOS nativa de autenticación de Adobe Pass (AccessEnabler)

   b. Llamada `setRequestor()` para establecer la identidad del Programador; pase en el `requestorID` y (opcionalmente) una matriz de puntos finales de autenticación de Adobe Pass. Para tvOS también deberá proporcionar la clave pública y el secreto. Consulte [Documentación sin cliente](#create_dev) para obtener más información.

   * **Dependencia:** ID de solicitante de autenticación de Adobe Pass válido (póngase en contacto con el administrador de cuentas de autenticación de Adobe Pass para arreglarlo).

   * **Déclencheur:**
     [setRequestorComplete()](#$setReqComplete) devolución de llamada.

   >[!NOTE]
   >
   >No se pueden completar solicitudes de asignación de derechos hasta que se haya establecido completamente la identidad del solicitante. Esto significa que mientras [`setRequestor()`](#$setReq)  sigue ejecutándose, todas las solicitudes de derechos subsiguientes. Por ejemplo, [`checkAuthentication()`](#checkAuthN) están bloqueados.

   Tiene dos opciones de implementación: Una vez que la información de identificación del solicitante se envía al servidor back-end, la capa de aplicación de la interfaz de usuario puede elegir uno de los dos enfoques siguientes: </br>

   1. Espere a que se active la [`setRequestorComplete()`](#setReqComplete) callback (parte del delegado AccessEnabler). Esta opción proporciona la mayor certeza de que [`setRequestor()`](#$setReq) completado, por lo que se recomienda para la mayoría de las implementaciones.

   1. Continúe sin esperar a que se active el [`setRequestorComplete()`](#setReqComplete) llamada de retorno y empiece a emitir solicitudes de derechos. Estas llamadas (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) se ponen en cola mediante la biblioteca AccessEnabler, que realizará las llamadas de red reales después de la [`setRequestor()`](#$setReq). Esta opción se puede interrumpir ocasionalmente si, por ejemplo, la conexión de red es inestable.

1. Llamada `checkAuthentication()` para comprobar si hay una autenticación existente sin iniciar el flujo de autenticación completo.  Si esta llamada se realiza correctamente, puede continuar directamente al flujo de autorización. Si no es así, continúe con el flujo de autenticación.

   * **Dependencia:** Una llamada a correcta a [setRequestor()](#$setReq) (esta dependencia también se aplica a todas las llamadas subsiguientes).

   * **Déclencheur:** [setAuthenticationStatus()](#$setAuthNStatus) devolución de llamada.


### C. Flujo de autenticación sin SSO de Apple {#authn_flow_wo_applesso}

1. Llamada [`getAuthentication()`](#$getAuthN) para iniciar el flujo de autenticación o para obtener confirmación de que el usuario ya está autenticado.

   **Déclencheur:**

   * El [setAuthenticationStatus()](#$setAuthNStatus) llamada de retorno, si el usuario ya está autenticado. En este caso, vaya directamente a [Flujo de autorización](#authz_flow).

   * El [displayProviderDialog()](#$dispProvDialog) llamada de retorno, si el usuario aún no se ha autenticado.

1. Presentar al usuario la lista de proveedores enviados a
   [`displayProviderDialog()`](#dispProvDialog).

1. Una vez que el usuario haya seleccionado un proveedor, obtenga la URL de la MVPD del usuario en el `navigateToUrl:` o `navigateToUrl:useSVC:` devolución de llamada y abrir un `UIWebView/WKWebView` o `SFSafariViewController` y dirija ese controlador a la dirección URL.

1. A través de `UIWebView/WKWebView` o `SFSafariViewController` Cuando se crea una instancia en el paso anterior, el usuario aterriza en la página de inicio de sesión de la MVPD e introduce credenciales de inicio de sesión. Se realizan varias operaciones de redirección dentro del controlador.</br>

>[!NOTE]
>
>En este punto, el usuario tiene la oportunidad de cancelar el flujo de autenticación. Si esto sucede, el nivel de la interfaz de usuario es responsable de informar a AccessEnabler sobre este evento llamando a [setSelectedProvider()](#setSelProv) con `null` como parámetro. Esto permite al AccessEnabler limpiar su estado interno y restablecer el flujo de autenticación.

1. Una vez que el usuario ha iniciado sesión correctamente, la capa de aplicación detecta la carga de una dirección URL personalizada específica. Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de autenticación se ha completado y de que es seguro cerrar el `UIWebView/WKWebView` o `SFSafariViewController` controlador. En el caso a `SFSafariViewController`necesita que se use el controlador, la dirección URL personalizada específica está definida por el **`application's custom scheme`** (p. ej.,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica se define mediante **`ADOBEPASS_REDIRECT_URL`** constante (es decir, `adobepass://ios.app`).

1. Cierre el controlador UIWebView/WKWebView o SFSafariViewController y llame al método AccessEnabler `handleExternalURL:url` método de API, que indica al AccessEnabler que recupere el token de autenticación del servidor back-end.

1. (Opcional) Llamada [`checkPreauthorizedResources(resources)`](#$checkPreauth) para comprobar qué recursos puede ver el usuario. El `resources` El parámetro es una matriz de recursos protegidos asociados al token de autenticación del usuario. Un uso de la información de autorización obtenida del MVPD del usuario es decorar su interfaz de usuario (por ejemplo, símbolos bloqueados/desbloqueados junto al contenido protegido).

   * **Déclencheur:** [`preauthorizedResources()`](#preauthResources) callback
   * **Punto de ejecución:** Después del flujo de autenticación completado

1. Si la autenticación se ha realizado correctamente, continúe con el Flujo de autorización.

### D. Flujo de autenticación con Apple SSO en iOS {#authn_flow_with_applesso}

1. Llamada [`getAuthentication()`](#$getAuthN) para iniciar el flujo de autenticación o para obtener confirmación de que el usuario ya está autenticado.
   **Déclencheur:**

   * El [presentTvProviderDialog()](#presentTvDialog) llamada de retorno, si el usuario no está autenticado y el solicitante actual tiene al menos una MVPD que admita SSO. Si ninguna MVPD admite SSO, se utilizará el flujo de autenticación clásico.

1. Una vez que el usuario selecciona un proveedor, la biblioteca AccessEnabler obtiene un token de autenticación con la información proporcionada por el marco de VSA de Apple.

1. El [setAuthenticationsStatus()](#setAuthNStatus) se activará la devolución de llamada. En este punto, el usuario debe autenticarse con Apple SSO.

1. [Opcional] Llamada [`checkPreauthorizedResources(resources)`](#$checkPreauth) para comprobar qué recursos puede ver el usuario. El `resources` El parámetro es una matriz de recursos protegidos asociados al token de autenticación del usuario. Un uso de la información de autorización obtenida del MVPD del usuario es decorar su interfaz de usuario (por ejemplo, símbolos bloqueados / desbloqueados junto al contenido protegido).

   * **Déclencheur:** [`preauthorizedResources()`](#preauthResources) callback
   * **Punto de ejecución:** Después del flujo de autenticación completado

1. Si la autenticación se ha realizado correctamente, continúe con el Flujo de autorización.

### E. Flujo de autenticación con Apple SSO en tvOS {#authn_flow_with_applesso_tvOS}

1. Llamada [`getAuthentication()`](#$getAuthN) para iniciar el flujo de autenticación o para obtener confirmación de que el usuario ya está autenticado.
   **Déclencheur:**
   * El [`presentTvProviderDialog()`](#presentTvDialog) llamada de retorno, si el usuario no está autenticado y el solicitante actual tiene al menos una MVPD que admita SSO. Si ninguna MVPD admite SSO, se utilizará el flujo de autenticación clásico.

1. Una vez que el usuario selecciona un proveedor, la variable [`status()`](#status_callback_implementation) se llamará a la devolución de llamada. Se proporcionará un código de registro y la biblioteca AccessEnabler comenzará a sondear el servidor para obtener una autenticación de segunda pantalla correcta.

1. Si el código de registro proporcionado se utilizó para autenticarse correctamente en la segunda pantalla, la variable [`setAuthenticatiosStatus()`](#setAuthNStatus) se activará la devolución de llamada. En este punto, el usuario debe autenticarse con Apple SSO.
1. [Opcional] Llamada [`checkPreauthorizedResources(resources)`](#$checkPreauth) para comprobar qué recursos puede ver el usuario. El `resources` El parámetro es una matriz de recursos protegidos asociados al token de autenticación del usuario. Un uso de la información de autorización obtenida del MVPD del usuario es decorar su interfaz de usuario (por ejemplo, símbolos bloqueados / desbloqueados junto al contenido protegido).

   * **Déclencheur:** [`preauthorizedResources()`](#preauthResources) callback

   * **Punto de ejecución:** Después del flujo de autenticación completado
1. Si la autenticación se ha realizado correctamente, continúe con el Flujo de autorización.

### F. Flujo de autorización {#authz_flow}

1. Llamada [getAuthorization()](#$getAuthZ) para iniciar el flujo de autorización.

   * **Dependencia:** ResourceID(s) válidos acordados con los MVPD.
   * Los ID de recurso deben ser los mismos que se usan en cualquier otro dispositivo o plataforma, y serán los mismos en todas las MVPD. Para obtener información sobre los ID de recurso, consulte [Identificación de recursos protegidos](/help/authentication/identify-protected-resources.md)

1. Valide la autenticación y la autorización.

   * Si la variable [getAuthorization()](#$getAuthZ) La llamada se ha realizado correctamente: el usuario tiene tokens de AuthN y AuthZ válidos (el usuario está autenticado y autorizado para ver los medios solicitados).

   * If [getAuthorization()](#$getAuthZ) failed: Examine la excepción producida para determinar su tipo (AuthN, AuthZ o algo más):
      * Si se ha producido un error de autenticación (AuthN), reinicie el flujo de autenticación.
      * Si se trata de un error de autorización (AuthZ), el usuario no tiene autorización para ver los medios solicitados y se le debe mostrar algún tipo de mensaje de error.
      * Si hubo algún otro tipo de error (error de conexión, error de red, etc.) a continuación, mostrar un mensaje de error apropiado al usuario.

1. Valide el token de medios corto.\
   Utilice la biblioteca del Comprobador de tokens de medios de autenticación de Adobe Pass para comprobar el token de medios de corta duración devuelto desde el [getAuthorization()](#$getAuthZ) llamada anterior:

   * Si la validación se realiza correctamente: Reproduzca el medio solicitado para el usuario.
   * Si la validación falla: El token de AuthZ no era válido, la solicitud de medios debe rechazarse y se debe mostrar un mensaje de error al usuario.


1. Vuelva al flujo normal de aplicaciones.

### G. Ver flujo de medios {#media_flow}

1. El usuario selecciona los medios que desea ver.
1. ¿Están protegidos los medios? La aplicación comprueba si los medios seleccionados están protegidos:

   * Si los medios seleccionados están protegidos, la aplicación inicia el [Flujo de autorización](#authz_flow) arriba.

   * Si el medio seleccionado no está protegido, reproduzca el medio para el usuario.

### H. Flujo de cierre de sesión sin Apple SSO {#logout_flow_wo_AppleSSO}

1. Llamada [`logout()`](#$logout) para cerrar la sesión del usuario. AccessEnabler borra todos los valores y tokens almacenados en caché. Después de borrar la caché, AccessEnabler realiza una llamada al servidor para limpiar las sesiones del lado del servidor. Tenga en cuenta que, dado que la llamada al servidor podría provocar un redireccionamiento de SAML al IdP (esto permite la limpieza de la sesión en el lado del IdP), esta llamada debe seguir todas las redirecciones. Por este motivo, esta llamada debe gestionarse dentro de un controlador UIWebView/WKWebView o SFSafariViewController.

   a. Siguiendo el mismo patrón que el flujo de trabajo de autenticación, el dominio AccessEnabler realiza una solicitud a la capa de aplicación de la interfaz de usuario, a través de `navigateToUrl:` o `navigateToUrl:useSVC:` callback, para crear un controlador UIWebView/WKWebView o SFSafariViewController e indicar a que cargue la URL proporcionada en el `url` parámetro. Dirección URL del extremo de cierre de sesión en el servidor back-end.

   b. La aplicación debe supervisar la actividad del `UIWebView/WKWebView or SFSafariViewController` y detectar el momento en que carga una dirección URL personalizada específica, a medida que pasa por varias redirecciones. Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de cierre de sesión se ha completado y de que es seguro cerrar el `UIWebView/WKWebView` o `SFSafariViewController` controlador. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar el `UIWebView/WKWebView or SFSafariViewController` controlador y llamar a AccessEnabler `handleExternalURL:url`Método de API. En el caso a `SFSafariViewController`necesita que se use el controlador, la dirección URL personalizada específica está definida por el **`application's custom scheme`** (por ejemplo, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica se define mediante **`ADOBEPASS_REDIRECT_URL`**  constante (es decir, `adobepass://ios.app`).

   >[!NOTE]
   >
   >El flujo de cierre de sesión difiere del flujo de autenticación en que el usuario no tiene que interactuar con UIWebView/WKWebView o SFSafariViewController de ninguna manera. La capa de aplicación de la interfaz de usuario utiliza un UIWebView/WKWebView o SFSafariViewController para asegurarse de que se siguen todas las redirecciones. Por lo tanto, es posible (y recomendado) hacer que el controlador sea invisible (es decir, oculto) durante el proceso de cierre de sesión.


### I. Flujo de cierre de sesión con Apple SSO {#logout_flow_with_AppleSSO}

1. Llamada [`logout()`](#$logout) para cerrar la sesión del usuario.
1. El [status()](#status_callback_implementation) se llamará a la devolución de llamada con el ID VSA203.
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
