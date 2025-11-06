---
title: Referencia de la API de JavaScript SDK
description: Referencia de la API de JavaScript SDK
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '2883'
ht-degree: 0%

---

# Referencia de la API de JavaScript SDK (heredada) {#javascript-sdk-api-reference}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Referencia de API {#api-reference}

Estas funciones inician solicitudes de interacción con un MVPD. Todas las llamadas son asincrónicas; debe implementar [callbacks](#callbacks) para gestionar las respuestas:

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestorID, extremos, opciones){#setrequestor(inRequestorID,endpoints,options)}

**Descripción:** identifica el sitio desde el que se originan las solicitudes.  Debe realizar esta llamada antes que cualquier otra llamada de API en una sesión de comunicación.

**Parámetros:**

- *inRequestorID*: el identificador único que Adobe asignó al sitio de origen durante el registro.

- *extremos*: este parámetro es opcional. Puede tener uno de los siguientes valores:

   - Matriz que permite especificar extremos para los servicios de autenticación y autorización proporcionados por Adobe (se pueden utilizar distintas instancias para la depuración). En caso de que se proporcionen varias direcciones URL, la lista de MVPD estará compuesta por los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD. De forma predeterminada (si no se especifica ningún valor), se utiliza el proveedor de servicios de Adobe (<http://sp.auth.adobe.com/>).

  Ejemplo:
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options*: un objeto JSON que contiene el valor ID de aplicación, el valor ID de visitante, la configuración sin actualización (cierre de sesión en segundo plano) y la configuración de MVPD (iFrame). Todos los valores son opcionales.
   1. Si se especifica, se informará del ID de visitante de Experience Cloud en todas las llamadas de red realizadas por la biblioteca. El valor se puede utilizar posteriormente en informes de análisis avanzados.
   2. Si se especifica el identificador único de la aplicación -`applicationId` - el valor se agregará a todas las llamadas subsiguientes realizadas por la aplicación como parte del encabezado HTTP X-Device-Info. Este valor se puede obtener posteriormente de [ESM](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) informes con la consulta adecuada.

  **Nota:** Todas las claves JSON distinguen entre mayúsculas y minúsculas.

  Ejemplo:

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- El programador puede anular la configuración de MVPD establecida en la autenticación de Adobe Pass especificando si se requiere o no un iFrame para el inicio de sesión (clave *iFrameRequired*) y las dimensiones del iFrame (claves *iFrameWidth* y *iFrameHeight*). El objeto JSON tiene la siguiente plantilla:

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


Todas las claves de nivel superior de la plantilla anterior son opcionales y tienen valores predeterminados (*backgroundLogin*, *backgroundLogut* son false de forma predeterminada y mvpdConfig es null, lo que significa que no se invalida ninguna configuración de MVPD).


- **Nota**: si se especifican valores o tipos no válidos para los parámetros anteriores, se producirá un comportamiento no definido.



Este es un ejemplo de configuración para el siguiente escenario: activación del inicio de sesión y cierre de sesión sin actualización, cambio de MVPD1 al inicio de sesión de redirección de páginas completo (que no sea iFrame) y inicio de sesión de MVPD2 a iFrame con anchura=500 y altura=300:

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**Llamadas de retorno activadas:** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[Volver al principio](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Descripción:** solicita autorización para el recurso especificado. Cada vez que un cliente intente acceder a un recurso autorizable, llame a esta función para obtener un token de autorización de corta duración del Habilitador de acceso. Los ID de recurso se acuerdan con la autorización de MVPD.

Utiliza el token de autenticación en caché para el cliente actual. Si no se encuentra ese token, inicia primero el proceso de autenticación y, a continuación, continúa con la autorización.

**Parámetros:**

- `inResourceID`: ID del recurso para el cual el usuario solicita autorización.
- `redirect_url`: proporcione opcionalmente una URL de redireccionamiento, de modo que el proceso de autorización de MVPD devuelva al usuario a esa página en lugar de a la página desde la cual se inició la autorización.


Se activaron **devoluciones de llamadas:** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) con éxito, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) con error

>[!CAUTION]
>
>Utilice checkAuthorization() en lugar de getAuthorization() siempre que sea posible. El método getAuthorization() iniciará un flujo de autenticación completo (si el usuario no está autenticado) y esto podría llevar a una implementación complicada por parte del programador.

</b>

[Volver al principio](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Descripción:** solicita autenticación para el cliente actual. Normalmente se llama en respuesta a un clic en un botón de inicio de sesión. Busca un token de autenticación en caché para el cliente actual. Si no se encuentra ese token, inicia el proceso de autenticación. Esto invoca el cuadro de diálogo de selección de proveedores predeterminado o personalizado y, a continuación, utiliza el proveedor seleccionado para redirigir a la interfaz de inicio de sesión de MVPD.

Si se realiza correctamente, crea y almacena un token de autenticación para el usuario. Si la autenticación falla, el proveedor devuelve un mensaje de error apropiado a la llamada de retorno [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode).

**Parámetros:**

- redirect_url: Opcionalmente, proporcione una URL de redireccionamiento para que el proceso de autenticación de MVPD devuelva al usuario a esa página en lugar de a la página desde la que se inició la autenticación.

Se desencadenaron **devoluciones de llamadas:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Volver al principio](#top)

</br>

## checkAuthN {#checkauthn}

**Descripción:** comprueba el estado de autenticación actual del cliente actual.  No asociado a ninguna interfaz de usuario.

**Llamadas de retorno activadas:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Volver al principio](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Descripción:** La aplicación utiliza este método para comprobar el estado de autorización del cliente actual y del recurso dado. Se inicia comprobando primero el estado de autenticación. Si no se autentica, se activa la llamada de retorno tokenRequestFailed() y se cierra el método. Si el usuario está autenticado, también almacena en déclencheur el flujo de autorización. Ver detalles del método [getAuthorization()]&#x200B;(#getAuthZ.

>[!TIP]
>
> **Usar funciones de estado de comprobación** No es necesario comprobar el estado de autenticación o autorización antes de solicitar la autorización. Puede llamar a estas funciones, por ejemplo, para actualizar su propia visualización de estado. No los utilice cuando requiera la interacción del usuario.

**Parámetros:**

- `inResourceID`: ID del recurso para el cual el usuario solicita autorización.


**Llamadas de retorno activadas:**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**Descripción:** solicita el estado de autorización de &quot;comprobación preliminar&quot; para una lista de
recursos.

**Parámetros:**

- *resources*: el parámetro resources es una matriz de recursos cuya autorización se debe comprobar. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El id. de recurso está sujeto a las mismas limitaciones que el id. de recurso de la llamada `getAuthorization()`; es decir, se trata de un valor acordado entre el programador y MVPD, o un fragmento de RSS multimedia.

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Esta variante de la API está disponible a partir de la versión 4.0 de JS SDK


**Parámetros:**

- *resources*: el parámetro resources es una matriz de recursos cuya autorización se debe comprobar. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El id. de recurso está sujeto a las mismas limitaciones que el id. de recurso de la llamada `getAuthorization()`; es decir, se trata de un valor acordado entre el programador y MVPD, o un fragmento de RSS multimedia.

- *cache*: Indica si se debe usar la caché interna cuando se comprueban recursos preautorizados. Este es un parámetro opcional, con el valor predeterminado **true**. Si es true, el comportamiento es idéntico al de la API anterior, lo que significa que las llamadas posteriores a esta función utilizarán una caché interna para resolver el recurso autorizado previamente. Si se pasa **false** para este parámetro, se deshabilitará la caché interna, lo que dará como resultado una llamada al servidor cada vez que se llame a la API **checkPreauthorizedResources**.

**Llamadas de retorno activadas:** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[Volver Al Principio](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Descripción:** recupera información expuesta como metadatos por la biblioteca del Habilitador de acceso.

Existen dos tipos de metadatos:

- **Estático** (TTL de token de autenticación, TTL de token de autorización e ID de dispositivo)
- **Metadatos de usuario** (Esto incluye información específica del usuario que se pasa desde MVPD al dispositivo del usuario durante los flujos de autenticación o autorización)

**Más información:** [Metadatos de usuario](#UserMetadata)

**Parámetros:**

- *key*: ID que especifica los metadatos solicitados:
   - Si la clave es `"TTL_AUTHN",`, se realiza la consulta para obtener el tiempo de caducidad del token de autenticación.

   - Si la clave es `"TTL_AUTHZ"` y params es una matriz que contiene el ID de recurso como una cadena, se realiza la consulta para obtener la hora de caducidad del token de autorización asociado al recurso especificado.

   - Si la clave es `"DEVICEID"`, se realiza la consulta para obtener el ID del dispositivo actual. Tenga en cuenta que esta función está desactivada de forma predeterminada y los programadores deben ponerse en contacto con Adobe para obtener información sobre la habilitación y las tarifas.

   - Si la clave procede de la siguiente lista de tipos de metadatos de usuario, se envía a la función de devolución de llamada [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) un objeto JSON que contiene los metadatos de usuario correspondientes:

   - `"zip"` - Código postal

   - `"encryptedZip"` - Código postal cifrado

   - `"householdID"` - Identificador del hogar. En caso de que una MVPD no admita subcuentas, será idéntico a userID.

   - `"maxRating"` - Clasificación parental máxima para el usuario

   - `"userID"`: el identificador de usuario. En el caso de que un MVPD admita subcuentas y el usuario no sea la cuenta principal, userID será diferente a householdID.

   - `"channelID"`: lista de canales que el usuario puede ver

   - `"is_hoh"` - Indicador que identifica si un usuario es cabeza de familia

   - `"encryptedZip"` - Código postal cifrado

   - `"typeID"`: indicador que identifica si la cuenta de usuario es la cuenta principal/secundaria

   - `"primaryOID"` - Identificador del hogar

   - `"postalCode"` - Similar al código postal

   - `"acctID"` - ID de cuenta

   - `"acctParentID"` - Identificador principal de la cuenta

  **Nota**: los metadatos de usuario reales disponibles para un programador dependen de lo que un MVPD ponga a disposición.  Consulte [Metadatos de usuario](#UserMetadata) para ver la lista actual de metadatos de usuario disponibles.


Por ejemplo:

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**Llamadas de retorno activadas:** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[Volver al principio](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**Descripción:** Llame a esta función cuando el usuario haya seleccionado un MVPD en la interfaz de usuario de selección de proveedores para enviar la selección de proveedores al Habilitador de acceso o llame a esta función con un parámetro nulo en caso de que el usuario descarte la interfaz de usuario de selección de proveedores sin seleccionar un proveedor.

**Llamadas de retorno
desencadenó:**[&#x200B; setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Volver al principio](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Descripción:** Recupera los resultados de la selección del cliente en el cuadro de diálogo de selección de proveedor. Esto se puede utilizar en cualquier momento después de la comprobación de autenticación inicial.

Esta función es asincrónica y devuelve su resultado a la función de devolución de llamada `selectedProvider()`.

- **MVPD** MVPD seleccionado actualmente, o nulo si no se ha seleccionado ningún MVPD.
- **AE_State**: el resultado de la autenticación para el cliente actual es &quot;Nuevo usuario&quot;, &quot;Usuario no autenticado&quot; o &quot;Usuario autenticado&quot;

**Llamadas de retorno activadas:** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[Volver al principio](#top)

</br>

## cierre de sesión {#logout}

**Descripción:** cierra la sesión del cliente actual y borra toda la información de autenticación y autorización de ese usuario. Elimina todos los tokens authN y authZ del sistema del cliente.

**Llamadas de retorno activadas:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[Volver al principio](#top)

</br>

## Definición de llamada {#calllback-definitions}

Debe implementar estas llamadas de retorno para gestionar las respuestas a sus llamadas de solicitud asincrónicas:

- [rightLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider()](#selectedproviderresult-selectedprovider)

</br>

## rightLoaded() {#entitlementLoaded}

**Descripción:** se activó cuando el Habilitador de acceso ha completado la inicialización y está listo para recibir solicitudes. Implemente esta llamada de retorno para saber cuándo puede iniciar la comunicación con la API del Habilitador de acceso.
</br>

[Volver al principio](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Descripción:** Implemente esta devolución de llamada para recibir la información de configuración y la lista de MVPD.

**Parámetros:**

- *configXML*: objeto xml que contiene la configuración del SOLICITANTE actual, incluida la lista de MVPD.


**Activado por:** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Volver al principio](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**Descripción:** Implemente esta devolución de llamada para invocar su propia interfaz de usuario de selección de proveedor personalizada. El cuadro de diálogo debe utilizar el nombre para mostrar (y el logotipo opcional) para proporcionar las opciones del cliente. Cuando el cliente haya elegido y haya cerrado el cuadro de diálogo, envíe el ID asociado del proveedor elegido en la llamada a *setSelectedProvider()*.

**Parámetros:**

- *proveedores*: una matriz de objetos que representan las MVPD solicitadas:

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**Activado por:** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[Volver al principio](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Descripción:** Implemente esta llamada de retorno si el usuario seleccionó un MVPD que requiera un iFrame en el que mostrar su interfaz de usuario de la página de inicio de sesión de autenticación.

**Activado por:**&#x200B;[&#x200B; setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [Volver al principio](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Descripción:** Implemente esta devolución de llamada para recibir el estado de autenticación (1=autenticado o 0=no autenticado) y un mensaje de error descriptivo si se produce algún error al intentar determinar el estado de autenticación (cadena vacía al finalizar correctamente la comprobación).

>[!NOTE]
> 
>Si está usando el sistema actual [Informes de errores avanzados](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md), puede ignorar el parámetro errorCode enviado a esta función.  Sin embargo, el indicador isAuthenticated sigue siendo útil para rastrear el estado de autenticación de un usuario en el flujo de derechos


**Parámetros:**

- *isAuthenticated* - Proporciona el estado de autenticación: 1 (autenticado) o 0 (no autenticado).
- *errorCode*: cualquier error que se produjo al determinar el estado de autenticación. Una cadena vacía si no hay ninguna.


**Activado por:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Volver al principio](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>El tipo de dispositivo y el sistema operativo se derivan del uso de una biblioteca Java pública (<http://java.net/projects/user-agent-utils>) y de la cadena del agente de usuario. Tenga en cuenta que esta información se proporciona únicamente como una forma grosera de desglosar las métricas operativas en categorías de dispositivos, pero que Adobe no puede responsabilizarse de los resultados incorrectos. Utilice la nueva funcionalidad en consecuencia.

**Descripción:** Implemente esta devolución de llamada para recibir datos de seguimiento cuando se produzcan eventos específicos. Puede utilizar esto, por ejemplo, para realizar un seguimiento de cuántos usuarios han iniciado sesión con las mismas credenciales. El seguimiento no se puede configurar actualmente. Con la autenticación de Adobe Pass 1.6, `sendTrackingData()` también genera informes sobre el dispositivo, el cliente del habilitador de acceso y el tipo de sistema operativo. La llamada de retorno `sendTrackingData()` sigue siendo compatible con versiones anteriores.

- Valores posibles para el tipo de dispositivo:
   - ordenador
   - tableta
   - mobile
   - consola de juegos
   - desconocido

- Valores posibles para el tipo de cliente del Habilitador de acceso:
   - html5
   - ios
   - androide


Pasa el tipo de evento y una matriz de información asociada. Los tipos de eventos son:

| mvpdSelection | El usuario seleccionó un MVPD en un cuadro de diálogo de selección de proveedores. |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | Se ha completado una comprobación de autenticación. |
| authorizationDetection | Se completó una solicitud de autorización. |

</br>
Los datos son específicos para cada tipo de evento:
</br>

| Tipo de evento (cadena) | Datos (matriz) |
|:--- | :--- |
| mvpdSelection | 0: MVPD seleccionado |
|  | 1: Tipo de dispositivo |
|  | 2: Tipo de cliente del Habilitador de acceso |
|  | 3: SO |
| authenticationDetection | 0: Si la solicitud de token se realizó correctamente (verdadero/falso) |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: El token ya está en la caché (verdadero/falso) |
|  | 4: Tipo de dispositivo |
|  | &#x200B;5. Tipo de cliente del Habilitador de acceso |
|  | 6: SO |
| authorizationDetection | 0: Si la solicitud de token se realizó correctamente (verdadero/falso) |
|  | 1: MVPD ID |
|  | 2: GUID |
|  | 3: El token ya está en la caché (verdadero/falso) |
|  | 4: Error |
|  | 5: Detalles |
|  | 6: Tipo de dispositivo |
|  | 7: Tipo de cliente del Habilitador de acceso |
|  | 8: SO |


**Activado por:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Volver al principio](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Descripción:** Implemente esta llamada de retorno para recibir el token de medios de corta duración (inToken) y el identificador del recurso (inRequestedResourceID) para el que se realizó una solicitud de autorización o una solicitud de autorización de comprobación y que se completó correctamente.

**Activado por:** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[Volver al principio](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**Descripción:** Implemente esta llamada de retorno para que se señale cuando haya fallado una autorización o una solicitud de autorización de comprobación. Opcionalmente, un MVPD puede utilizarlo para proporcionar un mensaje personalizado que mostrará el programador.

>[!IMPORTANT]
>
>Esta función de llamada de retorno forma parte del sistema de informes de errores de autenticación de Adobe Pass original más antiguo. Se conserva para la compatibilidad con versiones anteriores, pero no es necesario utilizar esta función en absoluto si ha implementado sus propias devoluciones de llamada mediante el sistema actual de informes de errores avanzados. El nuevo sistema de informes de errores proporciona información más detallada sobre por qué ha fallado una autorización (u otra operación), junto con cursos de acción sugeridos para cada tipo de error o advertencia.

**Parámetros:**

- *inRequestedResourceID*: cadena que proporciona el identificador de recurso que se utilizó en la solicitud de autorización.
- *inRequestErrorCode*: cadena que muestra el código de error de autenticación de Adobe Pass, indicando el motivo del error; los valores posibles son &quot;Error de usuario no autenticado&quot; y &quot;Error de usuario no autorizado&quot;; para obtener más detalles, vea &quot;Códigos de error de devolución de llamada&quot; a continuación.
- *inRequestDetailedErrorMessage*: una cadena descriptiva adicional adecuada para mostrar. Si esta cadena descriptiva no está disponible por algún motivo, la autenticación de Adobe Pass enviará una cadena vacía **(&quot;&quot;)**.  MVPD puede utilizarlo para pasar mensajes de error personalizados o mensajes relacionados con ventas. Por ejemplo, si se deniega a un suscriptor la autorización para un recurso, MVPD podría responder con un `*inRequestDetailedErrorMessage*` como: **&quot;Actualmente no tiene acceso a este canal en su paquete. Si desea actualizar el paquete, haga clic \*aquí\*.&quot;** Autenticación de Adobe Pass pasa el mensaje a través de esta devolución de llamada al sitio del programador. A continuación, el programador tiene la opción de mostrarlo u omitirlo. La autenticación de Adobe Pass también puede utilizar `*inRequestDetailedErrorMessage*` para notificar al programador la condición que podría haber provocado un error. Por ejemplo, **&quot;Se produjo un error de red al comunicarse con el servicio de autorización del proveedor&quot;.**



**Activado por:** [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Volver al principio](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**Descripción:** La llamada de retorno desencadenada por el Habilitador de acceso que entrega la lista de recursos autorizados devuelta después de una llamada a `checkPreauthorizedResources()`.

**Parámetros:**

- *authorizedResources*: La lista de recursos autorizados.

**Activado por:** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[Volver al principio](#top)

</br>

## setMetadataStatus(clave, cifrado, datos) {#setMetadataStatus(key,encrypted,data)}

**Descripción:** La llamada de retorno desencadenada por el Habilitador de acceso que entrega los metadatos solicitados mediante una llamada de `getMetadata()`.

**Más información:** [Metadatos de usuario](#userMetadata)

**Parámetros:**

- *key (String)*: Clave de los metadatos para los que se realizó la solicitud.
- *cifrado (booleano)*: Un indicador que indica si el &quot;valor&quot; está cifrado o no. Si es &quot;true&quot;, &quot;value&quot; será en realidad una representación cifrada con web JSON del valor real.
- *data (objeto JSON)*: un objeto JSON con la representación de los metadatos. Para las solicitudes simples (&#39;`TTL_AUTHN`&#39;, &#39;`TTL_AUTHZ`&#39;, &#39;`DEVICEID`&#39;), el resultado es una cadena (que representa el TTL de autenticación, el TTL de autorización o el ID de dispositivo). En el caso de una solicitud de metadatos de usuario, el resultado puede ser un objeto primitivo o JSON que represente la carga útil de metadatos. La estructura real de los objetos de metadatos de usuario de JSON es similar a la siguiente:

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


Por ejemplo:

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**Activado por:** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[Volver al principio](#top)

</br>

## selectedProvider(resultado) {#selectedProvider(result)}

**Descripción:** Implemente esta devolución de llamada para recibir el MVPD seleccionado actualmente y el resultado de la autenticación del usuario actual ajustado en el parámetro `result`. El parámetro `result` es un objeto con las siguientes propiedades:

- **MVPD** MVPD seleccionado actualmente, o nulo si no se ha seleccionado ningún MVPD.
- **AE\_State** Resultado de la autenticación para el usuario actual, uno de &quot;Nuevo usuario&quot;, &quot;Usuario no autenticado&quot; o &quot;Usuario autenticado&quot;

**Activado por:** [getSelectedProvider()](#getSelProv)

</br>

[Volver al principio](#top)

</br>

### Códigos de error de devolución {#callback-error-codes}

| Errores genéricos | |
|:--- | :--- | 
| Error interno | Se ha producido un error del sistema al intentar procesar la solicitud. |
| Error de proveedor no seleccionado | Se produce cuando el cliente cancela en el cuadro de diálogo de selección de proveedores |
| Error de proveedor no disponible | Se produce cuando no hay proveedores disponibles. |

| Errores de autenticación | |
|:--- | :--- | 
| Error de autenticación genérica | Se devuelve cuando el motivo no se conoce o no puede publicarse. |
| Error de autenticación interna | Se ha producido un error del sistema al intentar autenticarse. |
| Error de usuario no autenticado | El usuario no está autenticado. |
| Error de varias solicitudes de autenticación | Se recibieron solicitudes de autenticación adicionales antes de que se completara la primera. |

| Errores de autorización | |
|:--- | :--- | 
| Error de autorización genérica | Se devuelve cuando el motivo no se conoce o no puede publicarse. |
| Error de autorización interna | Se ha producido un error del sistema al intentar autorizar. |
| Error de usuario no autorizado | El cliente no tiene autorización para ver el contenido solicitado. |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[Volver al principio](#top)
