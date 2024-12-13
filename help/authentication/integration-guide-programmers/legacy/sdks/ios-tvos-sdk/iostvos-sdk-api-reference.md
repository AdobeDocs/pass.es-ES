---
title: Referencia de la API de iOS/tvOS
description: Referencia de la API de iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '6934'
ht-degree: 0%

---

# Referencia de la API de SDK de iOS/tvOS (heredada) {#iostvos-sdk-api-reference}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#intro}

En esta página se describen los métodos y las funciones de llamada de retorno expuestos por el cliente nativo de iOS/tvOS para la autenticación de Adobe Pass. Los métodos y las funciones de devolución de llamada que se describen aquí se definen en los archivos de encabezado `AccessEnabler.h` y `EntitlementDelegate.h`; puede encontrarlos en el SDK de iOS AccessEnabler aquí: `[SDK directory]/AccessEnabler/headers/api/`


Documentación asociada:

* Para obtener una descripción del flujo de derechos básicos de autenticación de Adobe Pass, consulte [Flujo de derechos](/help/authentication/integration-guide-programmers/entitlement-flow.md).
* Para obtener una guía paso a paso sobre cómo implementar Adobe Pass
flujo de derechos de autenticación mediante esta API, consulte [Guía de integración de iOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md).
* Para obtener la última versión de iOS AccessEnabler SDK, consulte la [Biblioteca del habilitador de acceso nativo de iOS](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>El Adobe recomienda usar únicamente las API *public* de autenticación de Adobe Pass:
>
>* Las API públicas están disponibles y se han probado completamente en todos los tipos de cliente admitidos. Para cualquier función pública, nos aseguramos de que cada tipo de cliente tenga una versión correspondiente de los métodos asociados.
>* Las API públicas deben ser lo más estables posible, admitir la compatibilidad con versiones anteriores y garantizar que las integraciones de socios no se rompan. Sin embargo, para las API no públicas, nos reservamos el derecho de cambiar su firma en cualquier momento futuro. Si se encuentra con un flujo particular que no se puede admitir a través de una combinación de las llamadas públicas actuales a la API de autenticación de Adobe Pass, el mejor enfoque es hacérnoslo saber. Teniendo en cuenta sus necesidades, podemos modificar las API públicas y proporcionar una solución estable en el futuro.

</br>

## Referencia de API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement: crea una instancia del objeto AccessEnabler.

* **[OBSOLETO]** [init](#init): crea una instancia del objeto AccessEnabler.

* [`setOptions:options:`](#setOptions): configura opciones globales de SDK como profile o visitorID.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Establece la identidad del programador.

* **[OBSOLETO]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq): establece la identidad del programador.

* **[OBSOLETO]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos): establece la identidad del programador.

* [`setRequestorComplete:`](#setReqComplete): informa a la aplicación de que se ha completado la fase de configuración.

* [`checkAuthentication`](#checkAuthN) - Comprueba el estado de autenticación del usuario actual.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN): inicia el flujo de trabajo de autenticación completo.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter): inicia el flujo de trabajo de autenticación completo.

* [`displayProviderDialog:`](#dispProvDialog): informa a la aplicación de que debe crear una instancia de los elementos de la interfaz de usuario adecuados para que el usuario seleccione una MVPD.

* [`setSelectedProvider:`](#setSelProv): informa al AccessEnabler de la selección de MVPD del usuario.

* [`navigateToUrl:`](#nav2url): informa a la aplicación de que el usuario debe recibir la página de inicio de sesión de MVPD.

* [`navigateToUrl:useSVC:`](#nav2urlSVC): informa a la aplicación de que el usuario debe recibir la página de inicio de sesión de MVPD mediante SFSafariViewController

* [`handleExternalURL:url`](#handleExternalURL): completa el flujo de autenticación/cierre de sesión.

* **[OBSOLETO]** [`getAuthenticationToken`](#getAuthNToken): solicita el token de autenticación del servidor back-end.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus): informa a su aplicación del estado del flujo de autenticación.

* [`checkPreauthorizedResources:`](#checkPreauth): determina si el usuario ya está autorizado para ver recursos protegidos específicos.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache): determina si el usuario ya está autorizado para ver recursos protegidos específicos.

* [`preauthorizedResources:`](#preauthResources): proporciona una lista de recursos que el usuario ya tiene autorización para ver.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Comprueba el estado de autorización del usuario actual.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Inicia el flujo de autorización.

* [`setToken:forResource:`](#setToken): informa a la aplicación de que el flujo de autorización se completó correctamente.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed): informa a la aplicación de que el flujo de autorización ha fallado.

* [`logout`](#logout): inicia el flujo de cierre de sesión.

* [`getSelectedProvider`](#getSelProv): determina el proveedor seleccionado actualmente.

* [`selectedProvider:`](#selProv): envía información sobre el MVPD seleccionado actualmente a su aplicación.

* [`getMetadata:`](#getMeta): recupera información expuesta como metadatos por la biblioteca AccessEnabler.

* [`presentTvProviderDialog:`](#presentTvDialog): informa a la aplicación para que muestre el cuadro de diálogo de SSO de Apple.

* [`dismissTvProviderDialog:`](#dismissTvDialog): informa a la aplicación que oculte el cuadro de diálogo de SSO de Apple.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Envía los metadatos solicitados por una llamada a [`getMetadata:`](#getMeta).

* [`sendTrackingData:forEventType:`](#sendTracking): entrega información de datos de seguimiento.

* [`MVPD`](#mvpd): la clase MVPD. [Contiene información sobre MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** crea una instancia del objeto AccessEnabler. Debe haber una sola instancia de AccessEnabler por cada instancia de aplicación.

| **Llamada de API: constructor de iOS AccessEnabler** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilidad:** v3.0+

**Parámetros:**

* **softwareStatement:** Una cadena que identifica la aplicación en el sistema de Adobe. Consulte cómo obtener una declaración de software.

[Volver al principio...](#apis)



### init - [OBSOLETO]{#init}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** crea una instancia del objeto AccessEnabler. Debe haber una sola instancia de AccessEnabler por cada instancia de aplicación.

| Llamada de API: constructor de iOS AccessEnabler |
| --- |
| ```- (id) init;``` |

**Disponibilidad:** v1.0+ **Hasta:** v3.0

**Parámetros:** Ninguno

[Volver al principio...](#apis)

</br>

### setOptions:opciones {#setOptions}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** configura opciones globales de SDK. Acepta un NSDictionary como argumento. Los valores del diccionario se pasarán al servidor junto con cada llamada de red que realice SDK.

**Nota:** Los valores se pasarán al servidor independientemente del flujo actual (autenticación/autorización). Si desea cambiar los valores, puede llamar a este método en cualquier momento.

| Llamada de API: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilidad:** v2.3.0+

**Parámetros:**

* *options*: un NSDictionary que contiene opciones globales de SDK. Actualmente están disponibles las siguientes opciones:
   * **applicationProfile**: se puede usar para realizar configuraciones de servidor basadas en este valor.
   * **visitorID** - Servicio de ID de Experience Cloud. Este valor puede utilizarse posteriormente en informes de análisis avanzados.
   * **handleSVC**: valor booleano que indica si el programador controlará SFSafariViewControllers. Para obtener más información, consulte la [compatibilidad con SFSafariViewController en iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md).
      * Si se establece en **false,** el SDK presentará automáticamente al usuario final un SFSafariViewController. SDK irá más allá a la URL de la página de inicio de sesión de MVPD.
      * Si se establece en **true,**, el SDK **NO** presentará automáticamente al usuario final un SFSafariViewController. El SDK realizará más déclencheur en **navegar(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Información del cliente como se describe en [Pasar información del cliente](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

[Volver al principio...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** establece la identidad del programador. A cada programador se le asigna un ID único al registrarse con el Adobe en el sistema de autenticación de Adobe Pass. Cuando se trata de SSO y tokens remotos, el estado de autenticación puede cambiar cuando la aplicación está en segundo plano, se puede volver a llamar a setRequestor cuando la aplicación se pone en primer plano para sincronizar con el estado del sistema (recupere un token remoto si el SSO está habilitado o elimine el token local si se ha cerrado la sesión mientras tanto).

La respuesta del servidor contiene una lista de MVPD junto con información de configuración adjunta a la identidad del programador. El código AccessEnabler utiliza internamente la respuesta del servidor. Solo el estado de la operación (es decir, SUCCESS/FAIL) se presenta a su aplicación a través de la llamada de retorno `setRequestorComplete:`.

Si no se usa el parámetro `urls`, la llamada de red resultante se dirigirá a la dirección URL del proveedor de servicios predeterminado: el entorno de producción/VERSIÓN de Adobe.


Si se proporciona un valor para el parámetro `urls`, la llamada de red resultante se dirigirá a todas las direcciones URL proporcionadas en el parámetro `urls`. Todas las solicitudes de configuración se activan simultáneamente en subprocesos independientes. El primer respondedor tiene prioridad al compilar la lista de MVPD. Para cada MVPD de la lista, AccessEnabler recuerda la dirección URL del proveedor de servicios asociado. Todas las solicitudes de derechos subsiguientes se dirigen a la URL asociada al proveedor de servicios emparejado con el MVPD de destino durante la fase de configuración.

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
* *urls*: Parámetro opcional; de forma predeterminada, se utiliza el proveedor de servicios de Adobe (http://sp.auth.adobe.com/). Esta matriz permite especificar extremos para los servicios de autenticación y autorización proporcionados por el Adobe (se pueden utilizar diferentes instancias para la depuración). Puede utilizar esto para especificar varias instancias del proveedor de servicios de autenticación de Adobe Pass. Al hacerlo, la lista MVPD está compuesta por los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD.

>[!NOTE]
>
>Si se llama sin el parámetro `serviceProviders`, la biblioteca recuperará la configuración del proveedor de servicios predeterminado (es decir, `https://sp.auth.adobe.com` para el perfil de producción o `https://sp.auth-staging.adobe.com` para el perfil de ensayo). Si se proporciona el parámetro `serviceProviders`, debe ser una matriz de direcciones URL. La información de configuración se recupera de todos los extremos especificados y se combina. Si existe información duplicada en diferentes respuestas del proveedor de servicios, el conflicto se resuelve en favor del servidor que responde más rápido (es decir, el servidor con el tiempo de respuesta más corto tiene prioridad).

**Llamadas de retorno activadas:** [`setRequestorComplete:`](#setReqComplete)

[Volver al principio...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [OBSOLETO] {#setReq}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** establece la identidad del programador. A cada programador se le asigna un ID único al registrarse con el Adobe en el sistema de autenticación de Adobe Pass. Cuando se trata de SSO y tokens remotos, el estado de autenticación puede cambiar cuando la aplicación está en segundo plano. Se puede volver a llamar a setRequestor cuando la aplicación se pone en primer plano para sincronizar con el estado del sistema (recupere un token remoto si está habilitado el SSO o elimine el token local si se ha cerrado la sesión mientras tanto).

La respuesta del servidor contiene una lista de MVPD junto con información de configuración adjunta a la identidad del programador. El código AccessEnabler utiliza internamente la respuesta del servidor. Solo el estado de la operación (es decir, SUCCESS/FAIL) se presenta a su aplicación a través de la llamada de retorno `setRequestorComplete:`.

Si no se usa el parámetro `urls`, la llamada de red resultante se dirigirá a la dirección URL del proveedor de servicios predeterminado: el entorno de producción/VERSIÓN de Adobe.

Si se proporciona un valor para el parámetro `urls`, la llamada de red resultante se dirigirá a todas las direcciones URL proporcionadas en el parámetro `urls`. Todas las solicitudes de configuración se activan simultáneamente en subprocesos independientes. El primer respondedor tiene prioridad al compilar la lista de MVPD. Para cada MVPD de la lista, AccessEnabler recuerda la dirección URL del proveedor de servicios asociado. Todas las solicitudes de derechos subsiguientes se dirigen a la URL asociada al proveedor de servicios emparejado con el MVPD de destino durante la fase de configuración.

| Llamada de API: configuración del solicitante |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilidad:** v1.0+ **Hasta:** v3.0

| Llamada de API: configuración del solicitante |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilidad:** v1.0+ **Hasta:** v3.0

**Parámetros:**

* *requestorID*: ID único asociado con el programador. Pase el ID único asignado por el Adobe a su sitio cuando se registró por primera vez en el servicio de autenticación de Adobe Pass.
* *signedRequestorID*: **Este parámetro existe en iOS AccessEnabler versiones 1.2 y posteriores.** Una copia del ID del solicitante firmado digitalmente con su clave privada. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Parámetro opcional; de forma predeterminada, se utiliza el proveedor de servicios de Adobe (http://sp.auth.adobe.com/). Esta matriz permite especificar extremos para los servicios de autenticación y autorización proporcionados por el Adobe (se pueden utilizar diferentes instancias para la depuración). Puede utilizar esto para especificar varias instancias del proveedor de servicios de autenticación de Adobe Pass. Al hacerlo, la lista MVPD está compuesta por los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD.

**Notas:** Si se llama sin el parámetro `serviceProviders`, la biblioteca recuperará la configuración del proveedor de servicios predeterminado (es decir, `https://sp.auth.adobe.com` para el perfil de producción o `https://sp.auth-staging.adobe.com` para el perfil de ensayo). Si se proporciona el parámetro `serviceProviders`, debe ser una matriz de direcciones URL. La información de configuración se recupera de todos los extremos especificados y se combina. Si existe información duplicada en diferentes respuestas del proveedor de servicios, el conflicto se resuelve en favor del servidor que responde más rápido (es decir, el servidor con el tiempo de respuesta más corto tiene prioridad).

**Llamadas de retorno activadas:** [`setRequestorComplete:`](#setReqComplete)


[Volver al principio...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [OBSOLETO] {#setReq_tvos}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** establece la identidad del programador. A cada programador se le asigna un ID único al registrarse con el Adobe en el sistema de autenticación de Adobe Pass. Esta configuración solo debe realizarse una vez durante el ciclo de vida de la aplicación.

La respuesta del servidor contiene una lista de MVPD junto con información de configuración adjunta a la identidad del programador. El código AccessEnabler utiliza internamente la respuesta del servidor. Solo el estado de la operación (es decir, SUCCESS/FAIL) se presenta a su aplicación a través de la llamada de retorno `setRequestorComplete:`.

Si no se usa el parámetro `urls`, la llamada de red resultante se dirigirá a la dirección URL del proveedor de servicios predeterminado: el entorno de producción/VERSIÓN de Adobe.

Si se proporciona un valor para el parámetro `urls`, la llamada de red resultante se dirigirá a todas las direcciones URL proporcionadas en el parámetro `urls`. Todas las solicitudes de configuración se activan simultáneamente en subprocesos independientes. El primer respondedor tiene prioridad al compilar la lista de MVPD. Para cada MVPD de la lista, AccessEnabler recuerda la dirección URL del proveedor de servicios asociado. Todas las solicitudes de derechos subsiguientes se dirigen a la URL asociada al proveedor de servicios emparejado con el MVPD de destino durante la fase de configuración.



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


**Disponibilidad:** v2.0+ **Hasta:** v3.0

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

**Disponibilidad:** v2.0+ **Hasta:** v3.0

**Parámetros:**

* *requestorID*: ID único asociado con el programador. Pase el ID único asignado por el Adobe a su sitio la primera vez que   esté registrado con el servicio de autenticación de Adobe Pass.
* *signedRequestorID*: **Este parámetro existe en iOS AccessEnabler   versiones 1.2 y posteriores.** Una copia del ID del solicitante firmado digitalmente con su clave privada. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: Parámetro opcional; de forma predeterminada, el proveedor de servicios de Adobe   (http://sp.auth.adobe.com/). Esta matriz permite especificar extremos para los servicios de autenticación y autorización proporcionados por el Adobe (se pueden utilizar diferentes instancias para la depuración). Puede utilizar esto para especificar varias instancias del proveedor de servicios de autenticación de Adobe Pass. Al hacerlo, la lista MVPD está compuesta por los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD.
* secret y publicKey: La clave secreta y pública utilizada para firmar las segundas llamadas de pantalla. Para obtener más información, consulte [Documentación sin cliente](#create_dev).

Si se llama sin el parámetro `serviceProviders`, la biblioteca recuperará la configuración del proveedor de servicios predeterminado (es decir, `https://sp.auth.adobe.com` para el perfil de producción o https://sp.auth-staging.adobe.com para el perfil de ensayo). Si se proporciona el parámetro `serviceProviders`, debe ser una matriz de direcciones URL. La información de configuración se recupera de todos los extremos especificados y se combina. Si existe información duplicada en diferentes respuestas del proveedor de servicios, el conflicto se resuelve en favor del servidor que responde más rápido (es decir, el servidor con el tiempo de respuesta más corto tiene prioridad).

**Llamadas de retorno activadas:** [`setRequestorComplete:`](#setReqComplete)

[Volver al principio...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La devolución de llamada desencadenada por AccessEnabler informa a la aplicación de que la fase de configuración ha finalizado. Esto indica que la aplicación puede empezar a emitir solicitudes de derechos. La aplicación no puede emitir solicitudes de asignación de derechos hasta que se complete la fase de configuración.

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
   * `ACCESS_ENABLER_STATUS_SUCCESS`: la fase de configuración se completó correctamente
   * `ACCESS_ENABLER_STATUS_ERROR`: error en la fase de configuración

**Activado por:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Volver al principio...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** comprueba el estado de autenticación del usuario actual.
Para ello, busca un token de autenticación válido en la biblioteca local de
espacio de almacenamiento de token. Este método no realiza llamadas de red y se recomienda llamarlo en el subproceso principal.
La aplicación la utiliza para consultar el estado de autenticación del usuario y
actualice la interfaz de usuario en consecuencia (es decir, actualice la interfaz de usuario de inicio de sesión/cierre de sesión). El
el estado de autenticación se comunica a la aplicación a través de
la llamada de retorno [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).


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

**Llamadas de retorno activadas:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Volver al principio...](#apis)

</br>

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** inicia el flujo de trabajo de autenticación completo. Se inicia comprobando el estado de autenticación. Si no se ha autenticado ya, se inicia el flujo de autenticación estado-máquina:

* si el último intento de autenticación se realizó correctamente, la MVPD   se omite la fase de selección y   se activa la devolución de llamada [`navigateToUrl:`](#nav2url). El   La aplicación utiliza esta llamada de retorno para crear una instancia del control WebView que presenta al usuario la página de inicio de sesión de MVPD. **[NOTA: a partir de Access Enabler 1.5, esta funcionalidad no está disponible debido a una limitación en SDK].**
* si el último intento de autenticación no se realizó correctamente o si el usuario cerró la sesión explícitamente, la llamada de retorno [`displayProviderDialog:`](#dispProvDialog) es   activado. La aplicación utiliza esta llamada de retorno para mostrar la interfaz de usuario de selección de MVPD. Además, la aplicación debe reanudar el flujo de autenticación informando a la biblioteca AccessEnabler sobre la selección de MVPD del usuario mediante el método [`setSelectedProvider:`](#setSelProv).

Dado que las credenciales del usuario se verifican en la página de inicio de sesión de MVPD, la aplicación debe monitorizar las diversas operaciones de redirección que se producen mientras el usuario se autentica en la página de inicio de sesión de MVPD. Cuando se especifican las credenciales correctas, el control WebView se redirige a una dirección URL personalizada definida por la constante `ADOBEPASS_REDIRECT_URL`. WebView no pretende cargar esta dirección URL. La aplicación debe interceptar esta URL e interpretar este evento como una señal de que la fase de inicio de sesión ha finalizado. Luego debe entregar el control a AccessEnabler para completar el flujo de autenticación (llamando al método [handleExternalURL](#handleExternalURL)).

Por último, el estado de autenticación se comunica a la aplicación a través de la llamada de retorno [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

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

* *forceAuthn*: Un indicador que especifica si el flujo de autenticación debe iniciarse, independientemente de si el usuario ya se ha autenticado o no.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar SDK.

**Llamadas de retorno activadas:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Volver al principio...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** inicia el flujo de trabajo de autenticación completo. Se inicia comprobando el estado de autenticación. Si no se ha autenticado ya, se inicia el flujo de autenticación estado-máquina:

* Se llamará a [presentTvProviderDialog()](#presentTvDialog) si el solicitante actual tiene al menos un MVPD que admita SSO. Si ningún MVPD admite SSO, se iniciará el flujo de autenticación clásico y se ignorará el parámetro de filtro.
* Una vez que el usuario finalice, el flujo de SSO de Apple [`dismissTvProviderDialog()`](#dismissTvDialog) se activará y el proceso de autenticación finalizará.

Por último, el estado de autenticación se comunica a la aplicación a través de la llamada de retorno [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

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

* *forceAuthn*: Un indicador que especifica si el flujo de autenticación debe iniciarse, independientemente de si el usuario ya se ha autenticado o no.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar SDK.
* filter: Un diccionario con dos listas de MVPD ids que deben aparecer en el cuadro de diálogo SSO de Apple. Se ignorará cualquier MVPD que no admita SSO, pero se respetará el orden. El diccionario debe tener dos claves:
   * TV\_PROVIDERS: Una lista con todas las MVPD que deben aparecer en el selector
   * FEATURED\_TV\_PROVIDERS: Una lista con todas las MVPD que deben marcarse como destacadas en el selector. Las MVPD de esta lista también deben especificarse en la lista TV\_PROVIDERS.

**Disponibilidad:** v2.0 - v2.3.1


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

* *forceAuthn*: Un indicador que especifica si el flujo de autenticación debe iniciarse, independientemente de si el usuario ya se ha autenticado o no.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar SDK.
* filter: Una lista de MVPD ids que deben aparecer en el Diálogo de SSO de Apple. Se ignorará cualquier MVPD que no admita SSO, pero se respetará el orden.

**Llamadas de retorno activadas:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Volver al principio...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La llamada de retorno desencadenada por AccessEnabler para informar a la aplicación de que es necesario crear una instancia de los elementos de la interfaz de usuario apropiados para permitir que el usuario seleccione el MVPD deseado. La llamada de retorno proporciona una lista de objetos de MVPD con información adicional que puede ayudar a crear correctamente el panel de la interfaz de usuario de selección (como la URL que señala al logotipo de MVPD, un nombre para mostrar descriptivo, etc.)

Una vez que el usuario ha seleccionado el MVPD deseado, la aplicación de nivel superior debe reanudar el flujo de autenticación llamando a `setSelectedProvider:` y pasando el ID del MVPD correspondiente a la selección del usuario.

**Anulando el flujo de autenticación**. En este punto, el usuario tiene la posibilidad de presionar el botón &quot;Atrás&quot;, lo que equivale a anular el flujo de autenticación. En ese caso, la aplicación debe llamar al método [setSelectedProvider:](#setSelProv), pasando null como parámetro, para dar al AccessEnabler la oportunidad de restablecer su equipo de estado de autenticación.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Llamada de retorno: muestra la IU de selección de MVPD</th>
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

* *mvpds*: lista de objetos de MVPD que contienen información relacionada con MVPD que la aplicación puede utilizar para generar los elementos de la interfaz de usuario de selección de MVPD.

**Activado por:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Volver al principio...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Su aplicación llama a este método para informar al Habilitador de acceso de la selección de MVPD del usuario. La aplicación puede utilizar este método para seleccionar o cambiar el proveedor de servicios utilizado para la autenticación.

Si la MVPD seleccionada es una MVPD TempPass, se autenticará automáticamente con esa MVPD sin necesidad de llamar posteriormente a getAuthentication().

Tenga en cuenta que esto no es posible en el caso del pase temporal promocional, en el que se proporcionan parámetros adicionales en el método getAuthentication().

Al pasar *null* como parámetro, Access Enabler supone que el usuario ha cancelado el flujo de autenticación (es decir, ha presionado el botón &quot;Atrás&quot;), y responde restableciendo el estado-máquina de autenticación y llamando a la llamada de retorno [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) con el código de error `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR`.

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

**Llamadas de retorno activadas:** `setAuthenticationStatus:errorCode:`, `sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Volver al principio...](#apis)

</br>

#### navigationToUrl: {#nav2url}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción:** La devolución de llamada desencadenada por AccessEnabler para solicitar a su aplicación que cree una instancia de un controlador UIWebView/WKWebView y que cargue la dirección URL proporcionada en el parámetro **`url`** de la devolución de llamada. La devolución de llamada pasa el parámetro **`url`** que representa la dirección URL del extremo de autenticación o la dirección URL del extremo de cierre de sesión.

A medida que el controlador UIWebView/WKWebView` `pasa por varias redirecciones, su aplicación debe supervisar la actividad del controlador y detectar el momento en que carga una dirección URL personalizada específica definida por la constante `ADOBEPASS_REDIRECT_URL `i.e. `adobepass://ios.app`). Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de autenticación o cierre de sesión se ha completado y de que es seguro cerrar el controlador. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar UIWebView/WKWebView y llamar al método de API `handleExternalURL:url `de AccessEnabler.

**Nota:** Tenga en cuenta que en el caso del flujo de autenticación, este es un punto en el que el usuario tiene la capacidad de presionar el botón &quot;Atrás&quot;, lo que equivale a anular el flujo de autenticación. En este caso, la aplicación debe llamar al método [setSelectedProvider:](#setSelProv) pasando **`nil`** como parámetro y dando la oportunidad al AccessEnabler de restablecer su equipo de estado de autenticación.

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

* *url*: la dirección URL que señala a la página de inicio de sesión de MVPD

**Activado por:** [setSelectedProvider:](#setSelProv)



[Volver al principio...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción:** La devolución de llamada se activó mediante AccessEnabler en lugar de la devolución de llamada `navigateToUrl:` en el caso de que la aplicación habilitara previamente la administración manual de Safari View Controller (SVC) mediante la llamada [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions), y solo en el caso de las MVPD que requieran Safari View Controller (SVC). Para todas las demás MVPD se llamará a la devolución de llamada `navigateToUrl:`. Consulte la [compatibilidad con SFSafariViewController en iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md) para obtener detalles sobre cómo se debe administrar Safari View Controller (SVC).

De forma similar a la llamada de retorno `navigateToUrl:`, AccessEnabler activa la llamada `navigateToUrl:useSVC:` para solicitar a su aplicación que cree una instancia de un controlador `SFSafariViewController` y que cargue la URL proporcionada en el parámetro **`url`** de la llamada de retorno. La devolución de llamada pasa el parámetro **`url`** que representa la dirección URL del extremo de autenticación o la dirección URL del extremo de cierre de sesión, y el parámetro **`useSVC`** que especifica que la aplicación debe usar un `SFSafariViewController`.

A medida que el controlador `SFSafariViewController` pasa por varias redirecciones, su aplicación debe supervisar la actividad del controlador y detectar el momento en que carga una dirección URL personalizada específica definida por su `application's custom scheme` (por ejemplo** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de autenticación o cierre de sesión se ha completado y de que es seguro cerrar el controlador. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar `SFSafariViewController` y llamar al método de API `handleExternalURL:url `de AccessEnabler.

**Nota:** Tenga en cuenta que en el caso del flujo de autenticación, este es un punto en el que el usuario tiene la capacidad de presionar el botón &quot;Atrás&quot;, lo que equivale a anular el flujo de autenticación. En este caso, la aplicación debe llamar al método [setSelectedProvider:](#setSelProv) pasando **`nil`** como parámetro y dando la oportunidad al AccessEnabler de restablecer su equipo de estado de autenticación.

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

* *url:* URL que señala a la página de inicio de sesión de MVPD
* *useSVC:* indica si la dirección URL debe cargarse en SFSafariViewController.

**Activado por:**[ setOptions:](#setOptions) antes de [setSelectedProvider:](#setSelProv)

[Volver al principio...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** su aplicación llama a este método para completar el flujo de autenticación o cierre de sesión. Se debe llamar a este método justo después de que la aplicación detecte el momento en que se redirige el controlador `UIWebView/WKWebView or SFSafariViewController` a una dirección URL personalizada específica. Si la aplicación necesita usar un controlador `SFSafariViewController `la dirección URL personalizada específica está definida por su `application's custom scheme` (p. ej.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica está definida por la constante `ADOBEPASS_REDIRECT_URL `es decir, `adobepass://ios.app`.

En el caso del flujo de autenticación, AccessEnabler completa el flujo recuperando el token de autenticación del servidor back-end y almacenándolo localmente en el almacenamiento de tokens. AccessEnabler informará a su aplicación de que el flujo de autenticación se ha completado llamando a la devolución de llamada `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> con un código de estado de 1, lo que indica que se ha realizado correctamente. Si hay un error durante la ejecución de estos pasos, la llamada de retorno `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> se activa con un código de estado de 0, lo que indica un error de autenticación, así como un código de error correspondiente.

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

* *url*: La dirección URL interceptada del control ` UIWebView/WKWebView or SFSafariViewController ` como cadena.


**Llamadas de retorno activadas:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Volver al principio...](#apis)

</br>

#### getAuthenticationToken - [OBSOLETO] {#getAuthNToken}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** completa el flujo de autenticación al solicitar el token de autenticación del servidor back-end. Su aplicación debería llamar a este método únicamente en respuesta a un evento en el que el control WebView que aloja la página de inicio de sesión de MVPD se redirija a la dirección URL personalizada definida por la constante `ADOBEPASS_REDIRECT_URL`.


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

**Disponibilidad:** v1.0+ **Hasta:** v3.0

**Parámetros:** Ninguno

**Llamadas de retorno activadas:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Volver al principio...](#apis)

&lt;/br

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La llamada de retorno desencadenada por AccessEnabler informa a la aplicación del estado del flujo de autenticación. Hay muchos lugares donde este flujo puede fallar, ya sea como resultado de la interacción del usuario o debido a otros escenarios imprevistos (es decir, problemas de conectividad de red, etc.). Esta llamada de retorno informa a la aplicación del estado de éxito/error del flujo de autenticación, a la vez que proporciona información adicional sobre el motivo del error, cuando es necesario.

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
   * `ACCESS_ENABLER_STATUS_SUCCESS`: el flujo de autenticación se completó correctamente
   * `ACCESS_ENABLER_STATUS_ERROR` - error en el flujo de autenticación
* *código*: motivo del error. Si *status* es `ACCESS_ENABLER_STATUS_SUCCESS`, entonces *code* es una cadena vacía (es decir, definida por la constante `USER_AUTHENTICATED`). En caso de error, este parámetro puede tomar uno de los siguientes valores:
   * `USER_NOT_AUTHENTICATED_ERROR` - El usuario no está autenticado. En respuesta a la llamada al método [checkAuthentication:](#checkAuthN) cuando no hay un token de autenticación válido en la caché de token local.
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler ha restablecido el       estado de autenticación-máquina después de la aplicación de capa superior       pasó *null* a [`setSelectedProvider:`](#setSelProv) para anular el flujo de autenticación.  Es de suponer que el usuario ha cancelado el flujo de autenticación (es decir, ha presionado el botón &quot;Atrás&quot;).
   * `GENERIC_AUTHENTICATION_ERROR`: error en el flujo de autenticación debido a motivos como la no disponibilidad de la red o que el usuario canceló explícitamente el flujo de autenticación.

**Activado por:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Volver al principio...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación utiliza este método para determinar si el usuario ya tiene autorización para ver recursos protegidos específicos. El propósito principal de este método es recuperar información para utilizarla en la decoración de la interfaz de usuario **(por ejemplo, para indicar el estado de acceso con los iconos de bloqueo y desbloqueo).**

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

* *recursos:* matriz de recursos para los que se debe comprobar la autorización. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El     El ID de recurso está sujeto a las mismas limitaciones que el ID de recurso de la llamada, es decir, debe ser un valor acordado entre el programador y MVPD o un fragmento de RSS de medios.

**Devolución de llamada desencadenada:** [`preauthorizedResources:`](#preauthResources)

[Volver al principio...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación utiliza este método para determinar si el usuario ya tiene autorización para ver recursos protegidos específicos. El propósito principal de este método es recuperar información para utilizarla en la decoración de la interfaz de usuario (por ejemplo, para indicar el estado de acceso con los iconos de bloqueo y desbloqueo). El parámetro **cache** controla si la caché interna se usa para resolver recursos.

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

* *recursos:* matriz de recursos para los que se debe comprobar la autorización. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El id. de recurso está sujeto a las mismas limitaciones que el id. de recurso de la llamada `getAuthorization:`; es decir, debe ser un valor acordado entre el programador y MVPD o un fragmento de RSS multimedia.
* *caché:* booleano que especifica si se debe usar la caché interna para resolver recursos. Si es false, se omitirá la caché, lo que dará como resultado llamadas al servidor cada vez que se realice una llamada a esta API.

**Devolución de llamada desencadenada:** [`preauthorizedResources:`](#preauthResources)

[Volver al principio...](#apis)

</br>

### preauthorizedResources: {#preauthResources}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción:** Llamada de retorno desencadenada por `checkPreauthorizedResources:`. Proporciona una lista de recursos que el usuario ya tiene autorización para ver.

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

* `resources`: matriz de recursos para los que el usuario ya tiene autorización para ver.

**Activado por:** [`checkPreauthorizedResources:`](#checkPreauth)



[Volver al principio...](#apis)

</br>

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación utiliza este método para comprobar el estado de autorización. Se inicia comprobando primero el estado de autenticación. Si no se autentica, se activa la devolución de llamada [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) y se cierra el método. Si el usuario está autenticado, también almacena en déclencheur el flujo de autorización. Ver detalles del método [`getAuthorization:`](#getAuthZ).


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

* *recurso*: el identificador del recurso para el cual el usuario solicita autorización.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar SDK.

**Llamadas de retorno activadas:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Volver al principio...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** La aplicación utiliza este método para iniciar el flujo de autorización. Si el usuario aún no se ha autenticado, también inicia el flujo de autenticación. Si el usuario se autentica, AccessEnabler procede a emitir solicitudes para el token de autorización (si no hay ningún token de autorización válido en la caché de token local) y para el token de medios de corta duración. Una vez obtenido el token de medios corto, el flujo de autorización se considera completo. La llamada de retorno [`setToken:forResource:`](#setToken) se activa y el token de medios corto se entrega como parámetro a la aplicación. Si, por cualquier motivo, la autorización falla, la llamada de retorno de [`tokenRequestFailed:forEventType:`](#tokenReqFailed) se activa y se proporcionan el código o los detalles del error.

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

* *recurso*: el identificador del recurso para el cual el usuario solicita autorización.
* *datos*: Un diccionario que consta de pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar SDK.

**Llamadas de retorno activadas:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Se desencadenaron llamadas de retorno adicionales:**\
Este método también puede almacenar en déclencheur las siguientes llamadas de retorno (si también se inicia el flujo de autenticación): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Use `checkAuthorization:` / `checkAuthorization:withData:` en lugar de `getAuthorization:` / `getAuthorization:withData:` siempre que sea posible. El método `getAuthorization:` / `getAuthorization:withData:` iniciará un flujo de autenticación completo (si el usuario no está autenticado) y esto podría provocar una implementación complicada por parte del programador.

[Volver al principio...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La llamada de retorno desencadenada por AccessEnabler informa a la aplicación de que el flujo de autorización se completó correctamente. El token de medios de corta duración también se entrega como parámetro.


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
* *recurso*: recurso para el que se obtuvo la autorización

**Activado por:** [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Volver al principio...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** Llamada de retorno desencadenada por AccessEnabler que informa a la aplicación de capa superior de que el flujo de autorización ha fallado.

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

* *recurso*: El recurso para el que se obtuvo la autorización.
* *código*: El código de error asociado con el escenario de error. Valores posibles:
   * `USER_NOT_AUTHORIZED_ERROR` - el usuario no pudo autorizar
para el recurso determinado
* *description*: Detalles adicionales acerca del escenario de error. Si esta cadena descriptiva no está disponible por algún motivo, la autenticación de Adobe Pass enviará una cadena vacía **(&quot;&quot;)**.\
  MVPD puede utilizar esta cadena para pasar mensajes de error personalizados o mensajes relacionados con ventas. Por ejemplo, si se deniega a un suscriptor la autorización de un recurso, MVPD podría enviar un mensaje como: &quot;Actualmente no tiene acceso a este canal en su paquete. Si desea actualizar el paquete, haga clic **aquí**.&quot; El mensaje lo pasa la autenticación de Adobe Pass a través de esta llamada de retorno al programador, que tiene la opción de mostrarlo o ignorarlo. La autenticación de Adobe Pass también puede utilizar este parámetro para notificar la condición que podría haber provocado un error. Por ejemplo, &quot;Se produjo un error de red al comunicarse con el servicio de autorización del proveedor&quot;.

**Activado por:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Volver al principio...](#apis)

</br>

### cierre de sesión {#logout}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** su aplicación llama a este método para iniciar el flujo de cierre de sesión. El cierre de sesión es el resultado de una serie de operaciones de redirección HTTP debido al hecho de que el usuario debe cerrar la sesión tanto desde los servidores de autenticación de Adobe Pass como desde los servidores de MVPD. Dado que este flujo no se puede completar con una simple solicitud HTTP emitida por la biblioteca AccessEnabler, es necesario crear una instancia de un controlador `UIWebView/WKWebView or SFSafariViewController` para poder seguir las operaciones de redirección HTTP.

El flujo de cierre de sesión difiere del flujo de autenticación en que el usuario no tiene que interactuar con el controlador `UIWebView/WKWebView or SFSafariViewController` de ninguna manera. Por lo tanto, Adobe recomienda hacer que el control sea invisible (es decir, oculto) durante el proceso de cierre de sesión.

Se utiliza un patrón similar al flujo de autenticación. El AccessEnabler de iOS déclencheur la llamada de retorno `navigateToUrl:` o `navigateToUrl:useSVC:` para crear un controlador `UIWebView/WKWebView or SFSafariViewController` y cargar la URL proporcionada en el parámetro `url` de la llamada de retorno. Dirección URL del extremo de cierre de sesión en el servidor back-end. Para el AccessEnabler de tvOS, no se llama ni a la devolución de llamada `navigateToUrl:` ni a la devolución de llamada `navigateToUrl:useSVC:`.

A medida que pasa por varias redirecciones, la aplicación debe supervisar la actividad del controlador `UIWebView/WKWebView or SFSafariViewController `y detectar el momento en que carga una dirección URL personalizada específica. Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de cierre de sesión se ha completado y de que es seguro cerrar el controlador. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar el controlador y llamar al método de API `handleExternalURL:url `de AccessEnabler. Si la aplicación necesita usar un controlador `SFSafariViewController `la dirección URL personalizada específica está definida por su `application's custom scheme` (p. ej.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica está definida por la constante `ADOBEPASS_REDIRECT_URL `es decir, `adobepass://ios.app`.

Al final, AccessEnabler llamará a la llamada de retorno [`setAuthenticationStatus()`](#setAuthNStatus) con un código de estado de 0, lo que indica que el flujo de cierre de sesión se ha realizado correctamente.

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

**Llamadas de retorno activadas:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



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
<th>Llamada de API: determine la MVPD seleccionada actualmente</th>
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

**Llamadas de retorno activadas:** [`selectedProvider:`](#selProv)

[Volver al principio...](#apis)

</br>

### selectedProvider {#selProv}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La llamada de retorno desencadenada por AccessEnabler que envía información sobre el MVPD seleccionado actualmente a la aplicación.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: información sobre el MVPD seleccionado actualmente.</th>
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

* *mvpd*: objeto que contiene información sobre el MVPD seleccionado actualmente

**Activado por:** [`getSelectedProvider`](#getSelProv)

[Volver al principio...](#apis)

</br>

### getMetadata: {#getMeta}

**Archivo:** AccessEnabler/headers/AccessEnabler.h

**Descripción:** Utilice este método para recuperar información expuesta como metadatos por la biblioteca AccessEnabler. La aplicación puede obtener acceso a estos datos proporcionando un parámetro de entrada *key* basado en diccionario.

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

* *keyDictionary*: una estructura de datos de diccionario, con lo siguiente
formato:
   * Si la clave es `METADATA_OPCODE_KEY` y el valor es `METADATA_AUTHENTICATION`, se realiza la consulta para obtener la hora de caducidad del token de autenticación.
   * Si la clave es `METADATA_OPCODE_KEY` y el valor es `METADATA_AUTHORIZATION` **y**\
     la clave es `METADATA_RESOURCE_ID_KEY` y el valor es un ID de recurso en particular; a continuación, se realiza la consulta para obtener la hora de caducidad del token de autorización asociado al recurso especificado.
   * Si la clave es `METADATA_OPCODE_KEY` y el valor es `METADATA_DEVICE_ID`, se realiza la consulta para obtener el ID del dispositivo actual. Tenga en cuenta que esta función está desactivada de forma predeterminada y los programadores deben ponerse en contacto con el Adobe para obtener información sobre la habilitación y las tarifas.
   * Si la clave es `METADATA_OPCODE_KEY` y el valor es `METADATA_USER_META` **y la clave** es `METADATA_USER_META_KEY` y el valor es el nombre de los metadatos, se realiza la consulta de los metadatos del usuario. La lista de tipos de metadatos de usuario disponibles:
      * `zip` - Lista de códigos postales
      * `householdID` - Identificador del hogar. En caso de que un MVPD no admita subcuentas, será idéntico a `userID`.
      * `maxRating`: una colección de clasificaciones paternas máximas para el usuario
      * `userID`: el identificador de usuario. Si un MVPD admite subcuentas y el usuario no es la cuenta principal, `userID` será diferente de `householdID.`
      * `channelID`: lista de canales que un usuario tiene derecho a ver.

  >[!NOTE]
  >
  >Los metadatos de usuario reales disponibles para un programador dependen de lo que MVPD ponga a disposición. Esta lista se ampliará a medida que haya nuevos metadatos disponibles y añadidos al sistema de autenticación de Adobe Pass.

**Llamadas de retorno activadas:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Más información:** [Metadatos de usuario](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

[Volver al principio...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La llamada de retorno desencadenada por AccessEnabler después de llamar a[getAuthentication()](#getAuthN) si el solicitante actual admite al menos un MVPD compatible con SSO.

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

**Descripción** La llamada de retorno desencadenada por AccessEnabler después de que el usuario cierre el cuadro de diálogo de SSO de Apple.

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

**Desencadenado por:** acción del usuario

**Más información:** [Inicio de sesión único de iOS/tvOS](#presentTvDialog)

[Volver al principio...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La llamada de retorno desencadenada por AccessEnabler que entrega los metadatos solicitados mediante una llamada de [`getMetadata:`](#getMeta).

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

* *metadatos*: Los metadatos solicitados. Este valor es un `NSString` en el caso de los metadatos estáticos (TTL de autenticación, TTL de autorización, ID de dispositivo).  Es un objeto complejo al solicitar metadatos específicos del usuario. Este objeto complejo suele ser la representación Objective-C de una carga útil JSON (por ejemplo, &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;edificios&quot;: [&quot;150&quot;, &quot;320&quot;]}&#39; se traduce en Objective-C como NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;edificios&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;))).   Objeto JSON de metadatos de muestra:

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

* *cifrado*: valor booleano que especifica si los metadatos recuperados están cifrados o no. Este parámetro solo es significativo para solicitudes de metadatos del usuario, no tiene significado para metadatos estáticos (por ejemplo, TTL de autenticación) que siempre se reciben sin cifrar. Si este parámetro se establece en true, depende del programador obtener el valor de metadatos de usuario sin cifrar realizando un descifrado RSA con la clave privada incluida en la lista de elementos permitidos (la misma clave privada que se utiliza para la firma del ID de solicitante en las llamadas [`setRequestor:setSignedRequestorId:`](#setReq) y `setRequestor:setSignedRequestorId:serviceProviders: `s).

* *key*: La clave utilizada para formular la solicitud de recuperación de metadatos.

* *argumentos*: El mismo diccionario que se pasó a la llamada [`getMetadata:`](#getMeta). Esto se proporciona para permitir que la aplicación combine las solicitudes con las respuestas.

**Activado por:** [`getMetadata:`](#getMeta)

**Más información:** [Metadatos de usuario](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)


[Volver al principio...](#apis)

</br>

### MVPD {#mvpd}

**Archivo:** AccessEnabler/headers/model/MVPD.h

**Descripción** Describe el objeto MVPD. Se puede utilizar para obtener información sobre las propiedades de MVPD.

**Disponibilidad:** v1.0+ [La propiedad boardingStatus está disponible en v2.2]

**Propiedades**:

* ID (NSString): El ID de MVPD.
* (NSString) displayName: el nombre de MVPD. [Se debe usar para mostrar en el selector]
* (NSString) logoURL: la dirección del logotipo de MVPD.
* (BOOL) enablePlatformServices: si es true, MVPD admite servicios SSO como [Apple SSO](#presentTvDialog).
* (NSString) boardingStatus: puede tener 3 valores:
   * nil: MVPD no es compatible con Apple SSO.
   * SELECTOR: MVPD puede aparecer en el selector de Apple, pero el flujo de autenticación se realiza por Adobe.
   * COMPATIBLE: MVPD es totalmente compatible con Apple y utilizará el token SSO de Apple.

[Volver al principio...](#apis)

</br>

## Seguimiento de eventos {#tracking}

AccessEnabler almacena en déclencheur una llamada de retorno adicional que no está necesariamente relacionada con los flujos de derechos. La implementación de la función de devolución de llamada [`sendTrackingData()`](#sendTracking) es opcional, pero permite a la aplicación realizar un seguimiento de eventos específicos y compilar estadísticas como el número de intentos de autenticación/autorización correctos/fallidos.

### sendTrackingData:forEventType: {#sendTracking}

**Archivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descripción** La llamada de retorno desencadenada por AccessEnabler indica a la aplicación la ocurrencia de varios eventos, como la finalización o el error de los flujos de autenticación o autorización. Con la autenticación de Adobe Pass 1.6, el tipo de dispositivo, el tipo de cliente de AccessEnabler y el sistema operativo se registran en [`sendTrackingData()`](#sendTracking). La llamada de retorno [`sendTrackingData()`](#sendTracking) sigue siendo compatible con versiones anteriores.

**Llamada de retorno: seguimiento de eventos**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilidad:** v1.0+

**Nota:** El tipo de dispositivo y el sistema operativo se derivan mediante el uso de una biblioteca Java pública (<http://java.net/projects/user-agent-utils>) y la cadena del agente de usuario. Tenga en cuenta que esta información se proporciona solo como una forma grosera de desglosar las métricas operativas en categorías de dispositivos, pero que ese Adobe no puede responsabilizarse de los resultados incorrectos. Utilice la nueva funcionalidad en consecuencia.

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

* *event*: el código del evento que se está rastreando. Existen tres tipos de eventos de seguimiento posibles:
   * **authorizationDetection:** cada vez que se devuelve una solicitud de token de autorización (el evento es `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** cada vez que se produce una comprobación de autenticación (el evento es `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** cuando el usuario selecciona un MVPD en el formulario de selección de MVPD (el evento es `TRACKING_GET_SELECTED_PROVIDER`)
* *datos*: datos adicionales asociados al evento del que se informó. Estos datos se presentan en forma de lista de valores.

**Activado por:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Instrucciones para interpretar los valores de la matriz *data*:

* Para trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Si la solicitud de token se realizó correctamente (verdadero/falso) y si se realizó correctamente:
   * **1** - cadena de ID de MVPD
   * **2** - GUID (md5 con hash)
   * **3**: el token ya está en la caché (verdadero/falso)
   * **4** - Tipo de dispositivo
   * **5** - Tipo de cliente de AccessEnabler
   * **6** - Tipo de sistema operativo

* Para trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Si la solicitud de token se realizó correctamente (verdadero/falso) y si se realizó correctamente:
   * **1** - MVPD ID
   * **2** - GUID (md5 con hash)
   * **3**: el token ya está en la caché (verdadero/falso)
   * **4** - Error
   * **5** - Detalles
   * **6** - Tipo de dispositivo
   * **7** - Tipo de cliente de AccessEnabler
   * **8** - Tipo de sistema operativo
* Para trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID del MVPD seleccionado actualmente
   * **1** - Tipo de dispositivo
   * **2** - tipo de cliente AccessEnabler
   * **3** - Tipo de sistema operativo

</br>
