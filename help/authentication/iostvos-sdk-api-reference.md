---
title: Referencia de la API de iOS/tvOS
description: Referencia de la API de iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '6933'
ht-degree: 0%

---

# Referencia de la API del SDK para iOS/tvOS {#iostvos-sdk-api-reference}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#intro}

En esta página se describen los métodos y las funciones de llamada de retorno expuestos por el cliente nativo de iOS/tvOS para la autenticación de Adobe Pass. Los métodos y las funciones de llamada de retorno descritos aquí se definen en la variable `AccessEnabler.h` y `EntitlementDelegate.h` archivos de encabezado; puede encontrarlos en el SDK de iOS AccessEnabler aquí: `[SDK directory]/AccessEnabler/headers/api/`


Documentación asociada:

* Para obtener una descripción del flujo de derechos de autenticación básico de Adobe Pass, consulte [Flujo de derecho](/help/authentication/entitlement-flow.md).
* Para obtener una guía paso a paso sobre cómo implementar el flujo de derechos de autenticación de Adobe Pass mediante esta API, consulte la [Guía de integración de iOS](/help/authentication/iostvos-sdk-cookbook.md).
* Para obtener el SDK de iOS AccessEnabler más reciente, consulte [Biblioteca del habilitador de acceso nativo de iOS](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>El Adobe de le recomienda utilizar únicamente la autenticación de Adobe Pass *público* API:
>
>* Las API públicas están disponibles y se han probado completamente en todos los tipos de cliente admitidos. Para cualquier función pública, nos aseguramos de que cada tipo de cliente tenga una versión correspondiente de los métodos asociados.
>* Las API públicas deben ser lo más estables posible, admitir la compatibilidad con versiones anteriores y garantizar que las integraciones de socios no se rompan. Sin embargo, para las API no públicas, nos reservamos el derecho de cambiar su firma en cualquier momento futuro. Si se encuentra con un flujo particular que no se puede admitir a través de una combinación de las llamadas públicas actuales a la API de autenticación de Adobe Pass, el mejor enfoque es hacérnoslo saber. Teniendo en cuenta sus necesidades, podemos modificar las API públicas y proporcionar una solución estable en el futuro.

</br>

## Referencia de API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement: crea una instancia del objeto AccessEnabler.

* **[OBSOLETO]** [init](#init) : crea una instancia del objeto AccessEnabler.

* [`setOptions:options:`](#setOptions) : configura opciones globales del SDK como profile o visitorID.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Establece la identidad del Programador.

* **[OBSOLETO]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Establece la identidad del Programador.

* **[OBSOLETO]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Establece la identidad del Programador.

* [`setRequestorComplete:`](#setReqComplete) - Informa a la aplicación de que la fase de configuración ha finalizado.

* [`checkAuthentication`](#checkAuthN) - Comprueba el estado de autenticación del usuario actual.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) : inicia el flujo de trabajo de autenticación completo.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter) : inicia el flujo de trabajo de autenticación completo.

* [`displayProviderDialog:`](#dispProvDialog) : informa a la aplicación para que cree una instancia de los elementos de interfaz de usuario adecuados para que el usuario seleccione una MVPD.

* [`setSelectedProvider:`](#setSelProv) - Informa al AccessEnabler de la selección de MVPD del usuario.

* [`navigateToUrl:`](#nav2url) : informa a la aplicación de que el usuario debe tener la página de inicio de sesión de MVPD.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Informa a la aplicación de que el usuario debe tener la página de inicio de sesión de MVPD, utilizando SFSafariViewController

* [`handleExternalURL:url`](#handleExternalURL) - Completa el flujo de autenticación/cierre de sesión.

* **[OBSOLETO]** [`getAuthenticationToken`](#getAuthNToken) - Solicita el token de autenticación del servidor back-end.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) : informa a la aplicación del estado del flujo de autenticación.

* [`checkPreauthorizedResources:`](#checkPreauth) : Determina si el usuario ya está autorizado para ver recursos protegidos específicos.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) : Determina si el usuario ya está autorizado para ver recursos protegidos específicos.

* [`preauthorizedResources:`](#preauthResources) : Proporciona una lista de recursos que el usuario ya está autorizado a ver.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Comprueba el estado de autorización del usuario actual.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Inicia el flujo de autorización.

* [`setToken:forResource:`](#setToken) - Informa a la aplicación de que el flujo de autorización se completó correctamente.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Informa a la aplicación de que el flujo de autorización ha fallado.

* [`logout`](#logout) - Inicia el flujo de cierre de sesión.

* [`getSelectedProvider`](#getSelProv) - Determina el proveedor seleccionado actualmente.

* [`selectedProvider:`](#selProv) - Entrega información sobre la MVPD seleccionada actualmente a su aplicación.

* [`getMetadata:`](#getMeta) : recupera información expuesta como metadatos por la biblioteca AccessEnabler.

* [`presentTvProviderDialog:`](#presentTvDialog) : Informa a la aplicación para que muestre el cuadro de diálogo SSO de Apple.

* [`dismissTvProviderDialog:`](#dismissTvDialog) : Informa a la aplicación para ocultar el cuadro de diálogo SSO de Apple.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Envía los metadatos solicitados por un [`getMetadata:`](#getMeta) llamada.

* [`sendTrackingData:forEventType:`](#sendTracking) : Proporciona información de datos de seguimiento.

* [`MVPD`](#mvpd) - La clase MVPD. [Contiene información sobre MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Crea una instancia del objeto AccessEnabler. Debe haber una sola instancia de AccessEnabler por cada instancia de aplicación.

| **Llamada de API: constructor de iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilidad:** v3.0+

**Parámetros:**

* **softwareStatement:** Cadena que identifica la aplicación en el sistema de Adobe. Consulte cómo obtener una declaración de software.

[Volver al principio...](#apis)



### init - [OBSOLETO]{#init}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Crea una instancia del objeto AccessEnabler. Debe haber una sola instancia de AccessEnabler por cada instancia de aplicación.

| Llamada de API: constructor de iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Disponibilidad:** v1.0+ **Hasta:** Versión 3.0

**Parámetros:** Ninguno

[Volver al principio...](#apis)

</br>

### setOptions:opciones {#setOptions}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Configura las opciones globales del SDK. Acepta un NSDictionary como argumento. Los valores del diccionario se pasarán al servidor junto con cada llamada de red que realice el SDK.

**Nota:** Los valores se pasan al servidor independientemente del flujo actual (autenticación/autorización). Si desea cambiar los valores, puede llamar a este método en cualquier momento.

| Llamada de API: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilidad:** v2.3.0+

**Parámetros:**

* *opciones*: NSDictionary que contiene las opciones globales del SDK. Actualmente están disponibles las siguientes opciones:
   * **applicationProfile** : se puede utilizar para realizar configuraciones de servidor basadas en este valor.
   * **visitorID** : El servicio de ID de Experience Cloud. Este valor puede utilizarse posteriormente en informes de análisis avanzados.
   * **handleSVC** - Valor booleano que indica si el programador controlará SFSafariViewControllers. Consulte lo siguiente [Compatibilidad con SFSafariViewController en el SDK 3.2+ de iOS](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) para obtener más información.
      * Si se establece en **false,** el SDK presentará automáticamente al usuario final un SFSafariViewController. El SDK se desplazará aún más a la URL de la página de inicio de sesión de MVPD.
      * Si se establece en **true,** el SDK **NO** presentar automáticamente al usuario final un SFSafariViewController. El SDK proporcionará más déclencheur **navegar(toUrl:{url}, useSVC:SÍ)**.
* **device\_info** - Información del cliente como se describe en [Pasar información del cliente](/help/authentication/passing-client-information-device-connection-and-application.md).

[Volver al principio...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Establece la identidad del programador. A cada programador se le asigna un ID único al registrarse con el Adobe en el sistema de autenticación de Adobe Pass. Cuando se trata de SSO y tokens remotos, el estado de autenticación puede cambiar cuando la aplicación está en segundo plano, se puede volver a llamar a setRequestor cuando la aplicación se pone en primer plano para sincronizar con el estado del sistema (recupere un token remoto si el SSO está habilitado o elimine el token local si se ha cerrado la sesión mientras tanto).

La respuesta del servidor contiene una lista de MVPD junto con información de configuración adjunta a la identidad del programador. El código AccessEnabler utiliza internamente la respuesta del servidor. Solo el estado de la operación (es decir, SUCCESS/FAIL) se presenta a la aplicación a través de la variable `setRequestorComplete:` devolución de llamada.

Si la variable `urls` no se utiliza, la llamada de red resultante se dirige a la URL del proveedor de servicios predeterminada: el entorno de Adobe RELEASE/production.


Si se proporciona un valor para `urls` , la llamada de red resultante se dirige a todas las direcciones URL proporcionadas en el parámetro `urls` parámetro. Todas las solicitudes de configuración se activan simultáneamente en subprocesos independientes. El primer respondedor tiene prioridad al compilar la lista de MVPD. Para cada MVPD de la lista, AccessEnabler recuerda la dirección URL del proveedor de servicios asociado. Todas las solicitudes de derechos subsiguientes se dirigen a la URL asociada al proveedor de servicios emparejado con la MVPD de destino durante la fase de configuración.

| Llamada de API: configuración del solicitante |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Disponibilidad:** v3.0+

| Llamada de API: configuración del solicitante |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Disponibilidad:** v3.0+

**Parámetros:**

* *requestorID*: ID único asociado con el programador. Pase el ID único asignado por el Adobe a su sitio cuando se registre por primera vez en el servicio de autenticación de Adobe Pass.
* *url*: Parámetro opcional; de forma predeterminada, se utiliza el proveedor de servicios de Adobe (http://sp.auth.adobe.com/). Esta matriz permite especificar extremos para los servicios de autenticación y autorización proporcionados por el Adobe (se pueden utilizar diferentes instancias para la depuración). Puede utilizar esto para especificar varias instancias del proveedor de servicios de autenticación de Adobe Pass. Al hacerlo, la lista de MVPD se compone de los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD.

>[!NOTE]
>
>Si se llama sin el `serviceProviders` , la biblioteca recuperará la configuración del proveedor de servicios predeterminado (es decir, `https://sp.auth.adobe.com` para el perfil de producción o `https://sp.auth-staging.adobe.com` para el perfil de ensayo). Si la variable `serviceProviders` Cuando se proporciona un parámetro, debe ser una matriz de direcciones URL. La información de configuración se recupera de todos los extremos especificados y se combina. Si existe información duplicada en diferentes respuestas del proveedor de servicios, el conflicto se resuelve en favor del servidor que responde más rápido (es decir, el servidor con el tiempo de respuesta más corto tiene prioridad).

**Llamadas activadas:** [`setRequestorComplete:`](#setReqComplete)

[Volver al principio...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [OBSOLETO] {#setReq}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Establece la identidad del programador. A cada programador se le asigna un ID único al registrarse con el Adobe en el sistema de autenticación de Adobe Pass. Cuando se trata de SSO y tokens remotos, el estado de autenticación puede cambiar cuando la aplicación está en segundo plano. Se puede volver a llamar a setRequestor cuando la aplicación se pone en primer plano para sincronizar con el estado del sistema (recupere un token remoto si está habilitado el SSO o elimine el token local si se ha cerrado la sesión mientras tanto).

La respuesta del servidor contiene una lista de MVPD junto con información de configuración adjunta a la identidad del programador. El código AccessEnabler utiliza internamente la respuesta del servidor. Solo el estado de la operación (es decir, SUCCESS/FAIL) se presenta a la aplicación a través de la variable `setRequestorComplete:` devolución de llamada.

Si la variable `urls` no se utiliza, la llamada de red resultante se dirige a la URL del proveedor de servicios predeterminada: el entorno de Adobe RELEASE/production.

Si se proporciona un valor para `urls` , la llamada de red resultante se dirige a todas las direcciones URL proporcionadas en el parámetro `urls` parámetro. Todas las solicitudes de configuración se activan simultáneamente en subprocesos independientes. El primer respondedor tiene prioridad al compilar la lista de MVPD. Para cada MVPD de la lista, AccessEnabler recuerda la dirección URL del proveedor de servicios asociado. Todas las solicitudes de derechos subsiguientes se dirigen a la URL asociada al proveedor de servicios emparejado con la MVPD de destino durante la fase de configuración.

| Llamada de API: configuración del solicitante |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilidad:** v1.0+ **Hasta:** Versión 3.0

| Llamada de API: configuración del solicitante |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilidad:** v1.0+ **Hasta:** Versión 3.0

**Parámetros:**

* *requestorID*: ID único asociado con el programador. Pase el ID único asignado por el Adobe a su sitio cuando se registró por primera vez en el servicio de autenticación de Adobe Pass.
* *signedRequestorID*: **Este parámetro existe en las versiones 1.2 y posteriores de iOS AccessEnabler.** Una copia del ID del solicitante firmado digitalmente con su clave privada. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: Parámetro opcional; de forma predeterminada, se utiliza el proveedor de servicios de Adobe (http://sp.auth.adobe.com/). Esta matriz permite especificar extremos para los servicios de autenticación y autorización proporcionados por el Adobe (se pueden utilizar diferentes instancias para la depuración). Puede utilizar esto para especificar varias instancias del proveedor de servicios de autenticación de Adobe Pass. Al hacerlo, la lista de MVPD se compone de los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD.

**Notas:** Si se llama sin el `serviceProviders` , la biblioteca recuperará la configuración del proveedor de servicios predeterminado (es decir,`https://sp.auth.adobe.com` para el perfil de producción o `https://sp.auth-staging.adobe.com` para el perfil de ensayo). Si la variable `serviceProviders` Cuando se proporciona un parámetro, debe ser una matriz de direcciones URL. La información de configuración se recupera de todos los extremos especificados y se combina. Si existe información duplicada en diferentes respuestas del proveedor de servicios, el conflicto se resuelve en favor del servidor que responde más rápido (es decir, el servidor con el tiempo de respuesta más corto tiene prioridad).

**Llamadas activadas:** [`setRequestorComplete:`](#setReqComplete)


[Volver al principio...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [OBSOLETO] {#setReq_tvos}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Establece la identidad del programador. A cada programador se le asigna un ID único al registrarse con el Adobe en el sistema de autenticación de Adobe Pass. Esta configuración solo debe realizarse una vez durante el ciclo de vida de la aplicación.

La respuesta del servidor contiene una lista de MVPD junto con información de configuración adjunta a la identidad del programador. El código AccessEnabler utiliza internamente la respuesta del servidor. Solo el estado de la operación (es decir, SUCCESS/FAIL) se presenta a la aplicación a través de la variable `setRequestorComplete:` devolución de llamada.

Si la variable `urls` no se utiliza, la llamada de red resultante se dirige a la URL del proveedor de servicios predeterminada: el entorno de Adobe RELEASE/production.

Si se proporciona un valor para `urls` , la llamada de red resultante se dirige a todas las direcciones URL proporcionadas en el parámetro `urls` parámetro. Todas las solicitudes de configuración se activan simultáneamente en subprocesos independientes. El primer respondedor tiene prioridad al compilar la lista de MVPD. Para cada MVPD de la lista, AccessEnabler recuerda la dirección URL del proveedor de servicios asociado. Todas las solicitudes de derechos subsiguientes se dirigen a la URL asociada al proveedor de servicios emparejado con la MVPD de destino durante la fase de configuración.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: configuración del solicitante</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilidad:** v2.0+ **Hasta:** Versión 3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: configuración del solicitante</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v2.0+ **Hasta:** Versión 3.0

**Parámetros:**

* *requestorID*: ID único asociado con el programador. Pase el ID único asignado por el Adobe a su sitio cuando se registró por primera vez en el servicio de autenticación de Adobe Pass.
* *signedRequestorID*: **Este parámetro existe en las versiones 1.2 y posteriores de iOS AccessEnabler.** Una copia del ID del solicitante firmado digitalmente con su clave privada. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*: Parámetro opcional; de forma predeterminada, se utiliza el proveedor de servicios de Adobe (http://sp.auth.adobe.com/). Esta matriz permite especificar extremos para los servicios de autenticación y autorización proporcionados por el Adobe (se pueden utilizar diferentes instancias para la depuración). Puede utilizar esto para especificar varias instancias del proveedor de servicios de autenticación de Adobe Pass. Al hacerlo, la lista de MVPD se compone de los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD.
* secret y publicKey: La clave secreta y pública utilizada para firmar las segundas llamadas de pantalla. Para obtener más información, consulte [Documentación sin cliente](#create_dev).

Si se llama sin el `serviceProviders` , la biblioteca recuperará la configuración del proveedor de servicios predeterminado (es decir, `https://sp.auth.adobe.com` para el perfil de producción o https://sp.auth-staging.adobe.com para el perfil de ensayo). Si la variable `serviceProviders` Cuando se proporciona un parámetro, debe ser una matriz de direcciones URL. La información de configuración se recupera de todos los extremos especificados y se combina. Si existe información duplicada en diferentes respuestas del proveedor de servicios, el conflicto se resuelve en favor del servidor que responde más rápido (es decir, el servidor con el tiempo de respuesta más corto tiene prioridad).

**Llamadas activadas:** [`setRequestorComplete:`](#setReqComplete)

[Volver al principio...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler que informa a la aplicación de que la fase de configuración ha finalizado. Esto indica que la aplicación puede empezar a emitir solicitudes de derechos. La aplicación no puede emitir solicitudes de asignación de derechos hasta que se complete la fase de configuración.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de retorno: configuración del solicitante completa</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilidad:** v1.0+

**Parámetros**:

* *status*: puede tomar uno de los siguientes valores:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - la fase de configuración se completó correctamente
   * `ACCESS_ENABLER_STATUS_ERROR` - fase de configuración fallida

**Activado por:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Volver al principio...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Comprueba el estado de autenticación del usuario actual.
Para ello, busca un token de autenticación válido en el espacio de almacenamiento del token local. Este método no realiza llamadas de red y se recomienda llamarlo en el subproceso principal.
La aplicación la utiliza para consultar el estado de autenticación del usuario y actualizar la interfaz de usuario en consecuencia (es decir, actualizar la interfaz de usuario de inicio de sesión/cierre de sesión). El estado de autenticación se comunica a la aplicación a través de la [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) devolución de llamada.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: comprobar el estado de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros:** Ninguno

**Llamadas activadas:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Volver al principio...](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Inicia el flujo de trabajo de autenticación completo. Se inicia comprobando el estado de autenticación. Si no se ha autenticado ya, se inicia el flujo de autenticación estado-máquina:

* si el último intento de autenticación se realizó correctamente, se omite la fase de selección de MVPD y la variable [`navigateToUrl:`](#nav2url) se activa la devolución de llamada. La aplicación utiliza esta llamada de retorno para crear una instancia del control WebView que presenta al usuario la página de inicio de sesión de la MVPD. **[NOTA: A partir de Access Enabler 1.5, esta funcionalidad no está disponible debido a una limitación del SDK].**
* si el último intento de autenticación no se realizó correctamente o si el usuario cerró sesión explícitamente, la variable [`displayProviderDialog:`](#dispProvDialog) se activa la devolución de llamada. La aplicación utiliza esta llamada de retorno para mostrar la interfaz de usuario de selección de MVPD. Además, la aplicación debe reanudar el flujo de autenticación informando a la biblioteca AccessEnabler sobre la selección de MVPD del usuario a través de la [`setSelectedProvider:`](#setSelProv) método.

A medida que las credenciales del usuario se verifican en la página de inicio de sesión de MVPD, la aplicación debe supervisar las operaciones de redirección múltiples que se producen mientras el usuario se autentica en la página de inicio de sesión de MVPD. Cuando se especifican las credenciales correctas, el control WebView se redirige a una dirección URL personalizada definida por el `ADOBEPASS_REDIRECT_URL` constante. WebView no pretende cargar esta dirección URL. La aplicación debe interceptar esta URL e interpretar este evento como una señal de que la fase de inicio de sesión ha finalizado. Luego debe entregar el control al AccessEnabler para completar el flujo de autenticación (llamando a la función [handleExternalURL](#handleExternalURL) método).

Finalmente, el estado de autenticación se comunica a la aplicación a través de la [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) devolución de llamada.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: inicia el flujo de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: inicia el flujo de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Disponibilidad:** v1.9+

**Parámetros:**

* *forceAuthen*: un indicador que especifica si se debe iniciar el flujo de autenticación, independientemente de si el usuario ya se ha autenticado o no.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar el SDK.

**Llamadas activadas:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Volver al principio...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Inicia el flujo de trabajo de autenticación completo. Se inicia comprobando el estado de autenticación. Si no se ha autenticado ya, se inicia el flujo de autenticación estado-máquina:

* [presentTvProviderDialog()](#presentTvDialog) se llamará si el solicitante actual tiene al menos una MVPD que admita SSO. Si ninguna MVPD admite SSO, comenzará el flujo de autenticación clásico y se omitirá el parámetro de filtro.
* Una vez que el usuario completa el flujo de SSO de Apple [`dismissTvProviderDialog()`](#dismissTvDialog) se activará y el proceso de autenticación finalizará.

Finalmente, el estado de autenticación se comunica a la aplicación a través de la [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) devolución de llamada.

**Disponibilidad:** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: inicia el flujo de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: inicia el flujo de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>
 </div></td>
</tr>
</tbody>
</table>



**Parámetros:**

* *forceAuthen*: un indicador que especifica si se debe iniciar el flujo de autenticación, independientemente de si el usuario ya se ha autenticado o no.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar el SDK.
* filtro: diccionario con dos listas de ID de MVPD que deben aparecer en el cuadro de diálogo SSO de Apple. Cualquier MVPD que no admita SSO será ignorada, pero se respetará el orden. El diccionario debe tener dos claves:
   * TV\_PROVIDERS: Una lista con todas las MVPD que deben aparecer en el selector
   * FEATURED\_TV\_PROVIDERS: Una lista con todas las MVPD que deben marcarse como destacadas en el selector. Las MVPD de esta lista también deben especificarse en la lista TV\_PROVIDERS.

**Disponibilidad:** Versión 2.0: versión 2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: inicia el flujo de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: inicia el flujo de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Parámetros:**

* *forceAuthen*: un indicador que especifica si se debe iniciar el flujo de autenticación, independientemente de si el usuario ya se ha autenticado o no.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar el SDK.
* filtro: una lista de los ID de MVPD que deben aparecer en el cuadro de diálogo SSO de Apple. Cualquier MVPD que no admita SSO será ignorada, pero se respetará el orden.

**Llamadas activadas:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Volver al principio...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La llamada de retorno activada por AccessEnabler para informar a la aplicación de que es necesario crear una instancia de los elementos de la interfaz de usuario adecuados para permitir que el usuario seleccione la MVPD deseada. La llamada de retorno proporciona una lista de objetos MVPD con información adicional que puede ayudar a crear correctamente el panel de la interfaz de usuario de selección (como la URL que señala al logotipo de MVPD, un nombre para mostrar descriptivo, etc.)

Una vez que el usuario ha seleccionado la MVPD deseada, la aplicación de capa superior debe reanudar el flujo de autenticación llamando a `setSelectedProvider:` y pasándole el ID de la MVPD correspondiente a la selección del usuario.

**Cancelación del flujo de autenticación** - Este es un punto en el que el usuario tiene la posibilidad de pulsar el botón &quot;Atrás&quot;, lo que equivale a anular el flujo de autenticación. En ese caso, la aplicación debe llamar a la función [setSelectedProvider:](#setSelProv) , pasando null como parámetro, para dar al AccessEnabler la oportunidad de restablecer su estado de autenticación-máquina.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de retorno: mostrar la IU de selección de MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilidad:** v1.0+

**Parámetros**:

* *mvpds*: lista de objetos MVPD que contienen información relacionada con MVPD que la aplicación puede utilizar para crear los elementos de la IU de selección de MVPD.

**Activado por:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Volver al principio...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación llama a este método para informar al Habilitador de acceso de la selección de MVPD del usuario. La aplicación puede utilizar este método para seleccionar o cambiar el proveedor de servicios utilizado para la autenticación.

Si la MVPD seleccionada es una MVPD TempPass, se autenticará automáticamente con esa MVPD sin necesidad de llamar posteriormente a getAuthentication().

Tenga en cuenta que esto no es posible en el caso del pase temporal promocional, en el que se proporcionan parámetros adicionales en el método getAuthentication().

Al pasar *null* como parámetro, el Access Enabler supone que el usuario ha cancelado el flujo de autenticación (es decir, ha pulsado el botón &quot;Atrás&quot;), y responde restableciendo el estado de autenticación-máquina y llamando a [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) devolución de llamada con `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` código de error.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: establezca el proveedor seleccionado actualmente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros:** Ninguno

**Llamadas activadas:** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`,  [`navigateToUrl:`](#nav2url)

[Volver al principio...](#apis)

</br>

#### navigationToUrl: {#nav2url}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción:** Llamada de retorno activada por AccessEnabler para solicitar a su aplicación que cree una instancia de un controlador UIWebView/WKWebView y que cargue la URL proporcionada en la llamada de retorno **`url`** parámetro. La llamada de retorno pasa el **`url`** parámetro que representa la dirección URL del extremo de autenticación o la dirección URL del extremo de cierre de sesión.

Como UIWebView/WKWebView` `El controlador pasa por varias redirecciones, la aplicación debe supervisar la actividad del controlador y detectar el momento en que carga una dirección URL personalizada específica definida por `ADOBEPASS_REDIRECT_URL `constante (es decir, `adobepass://ios.app`). Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de autenticación o cierre de sesión se ha completado y de que es seguro cerrar el controlador. Cuando el controlador carga esta URL personalizada específica, la aplicación debe cerrar UIWebView/WKWebView y llamar al servicio AccessEnabler `handleExternalURL:url `Método de API.

**Nota:** Tenga en cuenta que en el caso del flujo de autenticación, este es un punto en el que el usuario tiene la capacidad de pulsar el botón &quot;Atrás&quot;, lo que equivale a anular el flujo de autenticación. En este caso, la aplicación debe llamar a la función [setSelectedProvider:](#setSelProv) paso de método **`nil`** como el parámetro y dando la oportunidad al AccessEnabler de restablecer su estado de autenticación-máquina.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: mostrar la página de inicio de sesión de MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros**:

* *url*: la dirección URL que señala a la página de inicio de sesión de la MVPD

**Activado por:** [setSelectedProvider:](#setSelProv)



[Volver al principio...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción:** La llamada de retorno se activa mediante AccessEnabler en lugar de con el `navigateToUrl:` devolución de llamada en caso de que la aplicación haya habilitado anteriormente la gestión manual de Safari View Controller (SVC) mediante el [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) y solo en el caso de las MVPD que requieran el controlador de vista de Safari (SVC). Para todas las demás MVPD, la variable `navigateToUrl:` se llamará a la devolución de llamada. Consulte lo siguiente[Compatibilidad con SFSafariViewController en el SDK 3.2+ de iOS](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) para obtener más información sobre cómo se debe administrar Safari View Controller (SVC).

Similar a la `navigateToUrl:` devolución de llamada de `navigateToUrl:useSVC:` se activa mediante AccessEnabler para solicitar a la aplicación que cree una instancia de `SFSafariViewController` y para cargar la dirección URL proporcionada en el **`url`** parámetro. La llamada de retorno pasa el **`url`** que representa la URL del extremo de autenticación o la URL del extremo de cierre de sesión, y el parámetro **`useSVC`** parámetro que especifica que la aplicación debe utilizar un `SFSafariViewController`.

Como el `SFSafariViewController` El controlador pasa por varias redirecciones, la aplicación debe supervisar la actividad del controlador y detectar el momento en que carga una dirección URL personalizada específica definida por el `application's custom scheme` (p. ej.** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de autenticación o cierre de sesión se ha completado y de que es seguro cerrar el controlador. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar el `SFSafariViewController` y llame al servicio AccessEnabler `handleExternalURL:url `Método de API.

**Nota:** Tenga en cuenta que en el caso del flujo de autenticación, este es un punto en el que el usuario tiene la capacidad de pulsar el botón &quot;Atrás&quot;, lo que equivale a anular el flujo de autenticación. En este caso, la aplicación debe llamar a la función [setSelectedProvider:](#setSelProv) paso de método **`nil`** como el parámetro y dando la oportunidad al AccessEnabler de restablecer su estado de autenticación-máquina.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: mostrar la página de inicio de sesión de MVPD en SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:**v 3.2+

**Parámetros**:

* *url:* la URL que señala a la página de inicio de sesión de la MVPD
* *useSVC:* si la dirección URL debe cargarse en SFSafariViewController.

**Activado por:**[ setOptions:](#setOptions) antes [setSelectedProvider:](#setSelProv)

[Volver al principio...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación llama a este método para completar el flujo de autenticación o cierre de sesión. Se debe llamar a este método justo después de que la aplicación detecte el momento en que se produce el error `UIWebView/WKWebView or SFSafariViewController` se redirige a una dirección URL personalizada específica. En caso de que la aplicación deba utilizar un `SFSafariViewController `la dirección URL personalizada específica está definida por su `application's custom scheme` (p. ej.,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica se define mediante `ADOBEPASS_REDIRECT_URL `constante (es decir, `adobepass://ios.app`).

En el caso del flujo de autenticación, AccessEnabler completa el flujo recuperando el token de autenticación del servidor back-end y almacenándolo localmente en el almacenamiento de tokens. AccessEnabler informará a su aplicación de que el flujo de autenticación se ha completado llamando a la función `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> llamada de retorno con un código de estado de 1, que indica que se ha realizado correctamente. Si se produce un error durante la ejecución de estos pasos, la variable `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> la llamada de retorno se activa con un código de estado de 0, que indica un error de autenticación, así como un código de error correspondiente.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: complete el flujo de autenticación o cierre de sesión</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v3.0+

**Parámetros:**

* *url*: la dirección URL interceptada del ` UIWebView/WKWebView or SFSafariViewController ` control como cadena.


**Llamadas activadas:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Volver al principio...](#apis)

</br>

#### getAuthenticationToken - [OBSOLETO] {#getAuthNToken}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Completa el flujo de autenticación solicitando el token de autenticación al servidor back-end. La aplicación debe llamar a este método únicamente en respuesta a un evento en el que el control WebView que aloja la página de inicio de sesión de MVPD se redirige a la dirección URL personalizada definida por la variable `ADOBEPASS_REDIRECT_URL` constante.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: recuperar el token de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+ **Hasta:** Versión 3.0

**Parámetros:** Ninguno

**Llamadas activadas:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Volver al principio...](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler que informa a la aplicación del estado del flujo de autenticación. Hay muchos lugares donde este flujo puede fallar, ya sea como resultado de la interacción del usuario o debido a otros escenarios imprevistos (es decir, problemas de conectividad de red, etc.). Esta llamada de retorno informa a la aplicación del estado de éxito/error del flujo de autenticación, a la vez que proporciona información adicional sobre el motivo del error, cuando es necesario.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: informar del estado del flujo de autenticación</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**Disponibilidad:** v1.0+

**Parámetros**:

* *status*: puede tomar uno de los siguientes valores:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - el flujo de autenticación se completó correctamente
   * `ACCESS_ENABLER_STATUS_ERROR` - error del flujo de autenticación
* *código*: motivo del error. If *status* es `ACCESS_ENABLER_STATUS_SUCCESS`, entonces *código* es una cadena vacía (es decir, definida por la variable `USER_AUTHENTICATED` constante). En caso de error, este parámetro puede tomar uno de los siguientes valores:
   * `USER_NOT_AUTHENTICATED_ERROR` - El usuario no está autenticado. En respuesta a la [checkAuthentication:](#checkAuthN) llamada al método cuando no hay ningún token de autenticación válido en la caché de token local.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler ha restablecido el estado de autenticación-máquina después de que se aprobara la aplicación de capa superior *null* hasta [`setSelectedProvider:`](#setSelProv) para anular el flujo de autenticación.  Es de suponer que el usuario ha cancelado el flujo de autenticación (es decir, ha presionado el botón &quot;Atrás&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - El flujo de autenticación falló debido a razones como la no disponibilidad de la red o el usuario canceló el flujo de autenticación explícitamente.

**Activado por:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Volver al principio...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación utiliza este método para determinar si el usuario ya está autorizado para ver recursos protegidos específicos. El propósito principal de este método es recuperar información para utilizarla en la decoración de la interfaz de usuario **(por ejemplo, para indicar el estado del acceso con los iconos de bloqueo y desbloqueo).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: establezca el proveedor seleccionado actualmente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.3+

**Parámetros:**

* *recursos:* matriz de recursos cuya autorización debe comprobarse. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El ID de recurso está sujeto a las mismas limitaciones que el ID de recurso de la llamada, es decir, debe ser un valor acordado entre el programador y la MVPD o un fragmento de RSS de medios.

**Llamada de retorno activada:** [`preauthorizedResources:`](#preauthResources)

[Volver al principio...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación utiliza este método para determinar si el usuario ya está autorizado para ver recursos protegidos específicos. El propósito principal de este método es recuperar información para utilizarla en la decoración de la interfaz de usuario (por ejemplo, para indicar el estado de acceso con los iconos de bloqueo y desbloqueo). El **escondrijo** controla si la caché interna se utiliza para resolver recursos.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: establezca el proveedor seleccionado actualmente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**Disponibilidad:** v3.1+



**Parámetros:**

* *recursos:* matriz de recursos cuya autorización debe comprobarse. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El ID de recurso está sujeto a las mismas limitaciones que el ID de recurso de la variable `getAuthorization:` llamada, es decir, debe ser un valor acordado establecido entre el Programador y el MVPD o un fragmento de medios RSS.
* *caché:* Valor booleano que especifica si se debe utilizar la caché interna para resolver recursos. Si es false, se omitirá la caché, lo que dará como resultado llamadas al servidor cada vez que se realice una llamada a esta API.

**Llamada de retorno activada:** [`preauthorizedResources:`](#preauthResources)

[Volver al principio...](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción:** Llamada de retorno activada por `checkPreauthorizedResources:`. Proporciona una lista de recursos que el usuario ya tiene autorización para ver.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: establezca el proveedor seleccionado actualmente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.3+

**Parámetros:**

* `resources`: matriz de recursos para la que el usuario ya tiene autorización para ver.

**Activado por:** [`checkPreauthorizedResources:`](#checkPreauth)



[Volver al principio...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación utiliza este método para comprobar el estado de autorización. Se inicia comprobando primero el estado de autenticación. Si no se autentica, la variable [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) la llamada de retorno se activa y el método se cierra. Si el usuario está autenticado, también almacena en déclencheur el flujo de autorización. Consulte los detalles en la [`getAuthorization:`](#getAuthZ) método.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: comprobar el estado de autorización</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: comprobar el estado de autorización</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.9+

**Parámetros:**

* *resource*: el ID del recurso para el que el usuario solicita autorización.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar el SDK.

**Llamadas activadas:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Volver al principio...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación utiliza este método para iniciar el flujo de autorización. Si el usuario aún no se ha autenticado, también inicia el flujo de autenticación. Si el usuario se autentica, AccessEnabler procede a emitir solicitudes para el token de autorización (si no hay ningún token de autorización válido en la caché de token local) y para el token de medios de corta duración. Una vez obtenido el token de medios corto, el flujo de autorización se considera completo. El [`setToken:forResource:`](#setToken) la llamada de retorno se activa y el token de medios corto se entrega como parámetro a la aplicación. Si, por cualquier motivo, la autorización falla, la variable [`tokenRequestFailed:forEventType:`](#tokenReqFailed) la llamada de retorno se activa y se proporcionan el código o los detalles del error.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: iniciar el flujo de autorización</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: iniciar el flujo de autorización</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**Disponibilidad:** v1.9+

**Parámetros:**

* *resource*: el ID del recurso para el que el usuario solicita autorización.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar el SDK.

**Llamadas activadas:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Llamadas de retorno adicionales activadas:**\
Este método también puede almacenar en déclencheur las siguientes llamadas de retorno (si también se inicia el flujo de autenticación): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Utilice `checkAuthorization:` / `checkAuthorization:withData:` en lugar de `getAuthorization:` / `getAuthorization:withData:` siempre que sea posible. El `getAuthorization:` / `getAuthorization:withData:` iniciará un flujo de autenticación completo (si el usuario no está autenticado), lo que podría provocar una implementación complicada por parte del programador.

[Volver al principio...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler que informa a la aplicación de que el flujo de autorización se completó correctamente. El token de medios de corta duración también se entrega como parámetro.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de retorno: flujo de autorización completado correctamente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros**:

* *token*: el token de medios de corta duración
* *resource*: el recurso para el que se obtuvo la autorización

**Activado por:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Volver al principio...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler que informa a la aplicación de capa superior de que el flujo de autorización ha fallado.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: error del flujo de autorización</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros**:

* *resource*: El recurso para el que se obtuvo la autorización.
* *código*: el código de error asociado con el escenario de error. Valores posibles:
   * `USER_NOT_AUTHORIZED_ERROR` - el usuario no ha podido autorizar el recurso en cuestión
* *description*: Detalles adicionales sobre el escenario de error. Si esta cadena descriptiva no está disponible por ningún motivo, la autenticación de Adobe Pass envía una cadena vacía **(&quot;&quot;)**.\
  Una MVPD puede utilizar esta cadena para pasar mensajes de error personalizados o mensajes relacionados con las ventas. Por ejemplo, si se deniega a un suscriptor la autorización para un recurso, la MVPD podría enviar un mensaje como: &quot;Actualmente no tiene acceso a este canal en su paquete. Si desea actualizar el paquete, haga clic en **aquí**.&quot; El mensaje lo pasa la autenticación de Adobe Pass a través de esta llamada de retorno al programador, que tiene la opción de mostrarlo o ignorarlo. La autenticación de Adobe Pass también puede utilizar este parámetro para notificar la condición que podría haber provocado un error. Por ejemplo, &quot;Se produjo un error de red al comunicarse con el servicio de autorización del proveedor&quot;.

**Activado por:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Volver al principio...](#apis)

</br>

### cierre de sesión {#logout}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación llama a este método para iniciar el flujo de cierre de sesión. El cierre de sesión es el resultado de una serie de operaciones de redirección HTTP debido al hecho de que el usuario debe cerrar la sesión tanto desde los servidores de autenticación de Adobe Pass como desde los servidores de MVPD. Dado que este flujo no se puede completar con una simple solicitud HTTP emitida por la biblioteca AccessEnabler, `UIWebView/WKWebView or SFSafariViewController` debe crearse una instancia del controlador para poder seguir las operaciones de redirección HTTP.

El flujo de cierre de sesión difiere del flujo de autenticación en que el usuario no tiene que interactuar con el `UIWebView/WKWebView or SFSafariViewController`  controlador de cualquier manera. Por lo tanto, Adobe recomienda hacer que el control sea invisible (es decir, oculto) durante el proceso de cierre de sesión.

Se utiliza un patrón similar al flujo de autenticación. El iOS AccessEnabler almacena en déclencheur `navigateToUrl:` devolución de llamada o `navigateToUrl:useSVC:` para crear un `UIWebView/WKWebView or SFSafariViewController` y para cargar la dirección URL proporcionada en el `url` parámetro. Dirección URL del extremo de cierre de sesión en el servidor back-end. Para el AccessEnabler de tvOS, ni la variable `navigateToUrl:` devolución de llamada o `navigateToUrl:useSVC:` se llama a la devolución de llamada.

A medida que pasa por varias redirecciones, la aplicación debe monitorizar la actividad del `UIWebView/WKWebView or SFSafariViewController `y detectar el momento en que carga una dirección URL personalizada específica. Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de cierre de sesión se ha completado y de que es seguro cerrar el controlador. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar el controlador y llamar al servicio AccessEnabler `handleExternalURL:url `Método de API. En caso de que la aplicación deba utilizar un `SFSafariViewController `la dirección URL personalizada específica está definida por su `application's custom scheme` (p. ej.,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica se define mediante `ADOBEPASS_REDIRECT_URL `constante (es decir, `adobepass://ios.app`).

Al final, AccessEnabler llama al método [`setAuthenticationStatus()`](#setAuthNStatus) llamada de retorno con un código de estado de 0, que indica que el flujo de cierre de sesión se ha realizado correctamente.

**Nota:** Si el usuario ha iniciado sesión con Apple SSO, se activará el estado de VSA203. En este caso, se debe indicar al usuario que también cierre la sesión desde la configuración del sistema. Si no se hace esto, se volverá a autenticar cuando se reinicie la aplicación.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: iniciar el flujo de cierre de sesión</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros:** Ninguno

**Llamadas activadas:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Volver al principio...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Utilice este método para determinar el proveedor seleccionado actualmente.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: determina la MVPD seleccionada actualmente</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros:** Ninguno

**Llamadas activadas:** [`selectedProvider:`](#selProv)

[Volver al principio...](#apis)

</br>

### selectedProvider {#selProv}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler que envía información sobre la MVPD seleccionada actualmente a la aplicación.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: información sobre la MVPD seleccionada actualmente.</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros**:

* *mvpd*: objeto que contiene información sobre la MVPD seleccionada actualmente

**Activado por:** [`getSelectedProvider`](#getSelProv)

[Volver al principio...](#apis)

</br>

### getMetadata: {#getMeta}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Utilice este método para recuperar información expuesta como metadatos por la biblioteca AccessEnabler. La aplicación puede acceder a estos datos mediante un diccionario *key* parámetro de entrada.

Hay dos tipos de metadatos disponibles para los programadores:

* Metadatos estáticos (TTL del token de autenticación, TTL del token de autorización e ID de dispositivo)
* Los metadatos del usuario (información específica del usuario, como el ID de usuario y el código postal; se pueden pasar de un MVPD al dispositivo de un usuario durante los flujos de autenticación y autorización)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de API: consultar los metadatos en AccessEnabler</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros:**

* *keyDictionary*: una estructura de datos de diccionario, con el siguiente formato:
   * Si la clave es `METADATA_OPCODE_KEY` y el valor es `METADATA_AUTHENTICATION`, se realiza la consulta para obtener el tiempo de caducidad del token de autenticación.
   * Si la clave es `METADATA_OPCODE_KEY` y el valor es `METADATA_AUTHORIZATION` **y**\
     la clave es `METADATA_RESOURCE_ID_KEY` y el valor es un ID de recurso determinado, se realiza la consulta para obtener la hora de caducidad del token de autorización asociado al recurso especificado.
   * Si la clave es `METADATA_OPCODE_KEY` y el valor es `METADATA_DEVICE_ID`, se realiza la consulta para obtener el id del dispositivo actual. Tenga en cuenta que esta función está desactivada de forma predeterminada y los programadores deben ponerse en contacto con el Adobe para obtener información sobre la habilitación y las tarifas.
   * Si la clave es `METADATA_OPCODE_KEY` y el valor es `METADATA_USER_META` **y** la clave es `METADATA_USER_META_KEY` y value es el nombre de los metadatos, y se realiza la consulta de los metadatos del usuario. La lista de tipos de metadatos de usuario disponibles:
      * `zip` - Lista de códigos postales
      * `householdID` - Identificador del hogar. En el caso de que una MVPD no admita subcuentas, esto será idéntico a `userID`.
      * `maxRating` - Una colección de calificaciones paternas máximas para el usuario
      * `userID` : el identificador de usuario. Si una MVPD admite subcuentas y el usuario no es la cuenta principal, `userID` será diferente a `householdID.`
      * `channelID` - Una lista de canales que un usuario tiene derecho a ver.

  >[!NOTE]
  >
  >Los metadatos de usuario reales disponibles para un programador dependen de lo que una MVPD ponga a disposición. Esta lista se ampliará a medida que haya nuevos metadatos disponibles y añadidos al sistema de autenticación de Adobe Pass.

**Llamadas activadas:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Más información:** [Metadatos del usuario](/help/authentication/user-metadata.md)

[Volver al principio...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler después de llamar a[getAuthentication()](#getAuthN) si el solicitante actual admite al menos una MVPD compatible con SSO.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de retorno: resultado de flujos de SSO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v2.0+

**Parámetros**:

* viewController: representa el cuadro de diálogo de SSO de Apple. Este viewController debe mostrarse en la pantalla.

**Activado por:** [`getAuthentication`](#getAuthN)

**Más información:** [Inicio de sesión único de iOS/tvOS](#presentTvDialog)

[Volver al principio...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler después de que el usuario cierre el cuadro de diálogo de SSO de Apple.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de retorno: resultado de flujos de SSO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v2.0+

**Parámetros**:

* viewController: representa el cuadro de diálogo de SSO de Apple. Este viewController debe eliminarse de la pantalla.

**Activado por:** Acción del usuario

**Más información:** [Inicio de sesión único de iOS/tvOS](#presentTvDialog)

[Volver al principio...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler que envía los metadatos solicitados mediante una llamada de retorno [`getMetadata:`](#getMeta) llamada.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: resultado de la solicitud de recuperación de metadatos</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidad:** v1.0+

**Parámetros**:

* *metadatos*: los metadatos solicitados. Este valor es un `NSString` en el caso de los metadatos estáticos (TTL de autenticación, TTL de autorización, ID de dispositivo).  Es un objeto complejo al solicitar metadatos específicos del usuario. Este objeto complejo suele ser la representación Objective-C de una carga útil JSON (por ejemplo &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;edificios&quot;: [&quot;150&quot;, &quot;320&quot;]}&#39; se traduce en Objective-C como NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;edificios&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;))).   Objeto JSON de metadatos de muestra:

```JSON
        {
            updated: 1334243471,
            encrypted: ["encryptedProp"],
            data: {
                zip: ["12345", "34567"],
                maxRating: { 
                    "MPAA": "PG-13",
                    "VCHIP": "TV-Y", 
                    "URL": "http://exam.pl/e/manage/ratings"
                },
                householdID: "3456",
                userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                channelID: ["channel-1", "channel-2"]
            }
```

* *cifrado*: Valor booleano que especifica si los metadatos recuperados están cifrados o no. Este parámetro solo es significativo para solicitudes de metadatos del usuario, no tiene significado para metadatos estáticos (por ejemplo, TTL de autenticación) que siempre se reciben sin cifrar. Si este parámetro se establece en true, depende del programador obtener el valor de metadatos del usuario sin cifrar realizando un descifrado RSA con la clave privada de la lista blanca (la misma clave privada que se utiliza para la firma del ID del solicitante en el [`setRequestor:setSignedRequestorId:`](#setReq) y `setRequestor:setSignedRequestorId:serviceProviders: `llamadas).

* *key*: clave utilizada para formular la solicitud de recuperación de metadatos.

* *argumentos*: El mismo diccionario que se pasó al [`getMetadata:`](#getMeta) llamada. Esto se proporciona para permitir que la aplicación combine las solicitudes con las respuestas.

**Activado por:** [`getMetadata:`](#getMeta)

**Más información:** [Metadatos del usuario](/help/authentication/user-metadata.md)


[Volver al principio...](#apis)

</br>

### MVPD {#mvpd}

**Archivo:** AccessEnabler/headers/model/MVPD.h

**Descripción** Describe el objeto MVPD. Se puede utilizar para obtener información sobre las propiedades de la MVPD.

**Disponibilidad:** v1.0+ [la propiedad boardingStatus está disponible en la versión 2.2]

**Propiedades**:

* ID de (NSString): el ID de MVPD.
* (NSString) displayName: el nombre de MVPD. [Debe utilizarse para mostrar en el selector]
* (NSString) logoURL: la dirección del logotipo de MVPD.
* (BOOL) enablePlatformServices: si es true, MVPD admite servicios SSO como [APPLE SSO](#presentTvDialog).
* (NSString) boardingStatus: puede tener 3 valores:
   * nil: la MVPD no es compatible con el SSO de Apple.
   * SELECTOR: la MVPD puede aparecer en el selector de Apple, pero el flujo de autenticación se realiza por Adobe.
   * COMPATIBLE: Apple admite completamente MVPD y utilizará el token SSO de Apple.

[Volver al principio...](#apis)

</br>

## Seguimiento de eventos {#tracking}

AccessEnabler almacena en déclencheur una llamada de retorno adicional que no está necesariamente relacionada con los flujos de derechos. Implementación de [`sendTrackingData()`](#sendTracking) callback es opcional, pero permite a la aplicación realizar un seguimiento de eventos específicos y compilar estadísticas como el número de intentos de autenticación/autorización correctos/fallidos.

### sendTrackingData:forEventType: {#sendTracking}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno activada por AccessEnabler que indica a la aplicación la ocurrencia de varios eventos, como la finalización o el error de los flujos de autenticación o autorización. Con la autenticación de Adobe Pass 1.6, el tipo de dispositivo, el tipo de cliente de AccessEnabler y el sistema operativo se informan mediante [`sendTrackingData()`](#sendTracking). El [`sendTrackingData()`](#sendTracking) la devolución de llamada sigue siendo compatible con versiones anteriores.

**Callback: seguimiento de eventos**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilidad:** v1.0+

**Nota:** El tipo de dispositivo y el sistema operativo se derivan del uso de una biblioteca Java pública (<http://java.net/projects/user-agent-utils>) y la cadena del agente de usuario. Tenga en cuenta que esta información se proporciona solo como una forma grosera de desglosar las métricas operativas en categorías de dispositivos, pero que ese Adobe no puede responsabilizarse de los resultados incorrectos. Utilice la nueva funcionalidad en consecuencia.

* Valores posibles para el tipo de dispositivo:
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* Valores posibles para el tipo de cliente AccessEnabler:
   * `flash`
   * `html5`
   * `ios`
   * `android`


**Parámetros**:

* *evento*: el código del evento del que se está realizando un seguimiento. Existen tres tipos de eventos de seguimiento posibles:
   * **authorizationDetection:** cada vez que se devuelve una solicitud de token de autorización (el evento es `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** cada vez que se produce una comprobación de autenticación (el evento es `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** cuando el usuario selecciona una MVPD en el formulario de selección de MVPD (el evento es `TRACKING_GET_SELECTED_PROVIDER`)
* *datos*: datos adicionales asociados al evento del que se informa. Estos datos se presentan en forma de lista de valores.

**Activado por:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Instrucciones para interpretar los valores de *datos* matriz:

* Para trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Si la solicitud de token se realizó correctamente (verdadero/falso) y, si se realizó correctamente:
   * **1** - Cadena de ID de MVPD
   * **2** - GUID (MD5 con hash)
   * **3** - El token ya está en la caché (verdadero/falso)
   * **4** - Tipo de dispositivo
   * **5** - Tipo de cliente de AccessEnabler
   * **6** - Tipo de sistema operativo

* Para trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Si la solicitud de token se realizó correctamente (verdadero/falso) y, si se realizó correctamente:
   * **1** - ID de MVPD
   * **2** - GUID (MD5 con hash)
   * **3** - El token ya está en la caché (verdadero/falso)
   * **4** - Error
   * **5** - Detalles
   * **6** - Tipo de dispositivo
   * **7** - Tipo de cliente de AccessEnabler
   * **8** - Tipo de sistema operativo
* Para trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID de la MVPD seleccionada actualmente
   * **1** - Tipo de dispositivo
   * **2** - Tipo de cliente de AccessEnabler
   * **3** - Tipo de sistema operativo

</br>
