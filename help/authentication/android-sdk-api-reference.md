---
title: Referencia de API de SDK para Android
description: Referencia de API de SDK para Android
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '4518'
ht-degree: 0%

---

# Referencia de API de SDK para Android {#android-sdk-api-reference}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#intro}

Este documento detalla los métodos y las llamadas de retorno expuestos por el SDK de Android para la autenticación de Adobe Pass, compatible con las versiones 1.7 y posteriores de la autenticación de Adobe Pass. Los métodos y las funciones de devolución de llamada que se describen aquí se definen en los archivos de encabezado AccessEnabler.h y EntitlementDelegate.h.

Consulte la [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) para el SDK de AccessEnabler de Android más reciente.


**Nota:** El equipo de autenticación de Adobe Pass le recomienda utilizar únicamente la autenticación de Adobe Pass *público* API:

- Las API públicas están disponibles *y completamente probado* en todos los tipos de cliente admitidos. Para cualquier función pública, nos aseguramos de que cada tipo de cliente tenga una versión correspondiente de los métodos asociados.</span>
- Las API públicas deben ser lo más estables posible, admitir la compatibilidad con versiones anteriores y garantizar que las integraciones de socios no se rompan. Sin embargo, para *non*-API públicas, nos reservamos el derecho de cambiar su firma en cualquier momento futuro. Si se encuentra con un flujo particular que no se puede admitir a través de una combinación de las llamadas públicas actuales a la API de autenticación de Adobe Pass, el mejor enfoque es hacérnoslo saber. Teniendo en cuenta sus necesidades, podemos modificar las API públicas y proporcionar una solución estable en el futuro.

## API de Android {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigationToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- preautorizar
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [cierre de sesión](#logout)
- [getSelectedProvider](#getSelectedProvider)
- [selectedProvider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setMetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

### Factory.getInstance {#getInstance}

**Descripción:** Crea una instancia del objeto Habilitador de acceso. Debe haber una sola instancia de Access Enabler por cada instancia de aplicación.

| Llamada de API: constructor |
| --- |
| **estática pública** AccessEnabler **getInstance**(Context appContext, String softwareStatement, String redirectUrl)<br>        **lanzamientos** AccessEnablerException <br><br>**estática pública** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**lanzamientos** AccessEnablerException |

**Disponibilidad:** v3.1.2+

**Parámetros:**

- *appContext*: contexto de aplicación de Android.
- env\_url: para realizar pruebas con el entorno de ensayo de Adobe, env\_url se puede establecer en &quot;sp.auth-staging.adobe.com&quot;

**Obsoleto:**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Descripción:** Establece la identidad del programador. A cada programador se le asigna un ID único al registrarse con el Adobe en el sistema de autenticación de Adobe Pass. Cuando se trata de SSO y tokens remotos, el estado de autenticación puede cambiar cuando la aplicación está en segundo plano. Se puede volver a llamar a setRequestor cuando la aplicación se pone en primer plano para sincronizar con el estado del sistema (recupere un token remoto si está habilitado el SSO o elimine el token local si se ha cerrado la sesión mientras tanto).

La respuesta del servidor contiene una lista de MVPD junto con información de configuración adjunta a la identidad del programador. El código Access Enabler utiliza internamente la respuesta del servidor. Solo el estado de la operación (es decir, SUCCESS/FAIL) se presenta a la aplicación a través de la llamada de retorno setRequestorComplete().

Si la variable *url* no se utiliza, la llamada de red resultante se dirige a la dirección URL del proveedor de servicios predeterminado: el entorno de producción/versión de Adobe.

Si se proporciona un valor para *url* , la llamada de red resultante se dirige a todas las direcciones URL proporcionadas en el parámetro *url* parámetro. Todas las solicitudes de configuración se activan simultáneamente en subprocesos independientes. El primer respondedor tiene prioridad al compilar la lista de MVPD. Para cada MVPD de la lista, el Habilitador de acceso recuerda la URL del proveedor de servicios asociado. Todas las solicitudes de derechos subsiguientes se dirigen a la URL asociada al proveedor de servicios emparejado con la MVPD de destino durante la fase de configuración.

| Llamada de API: configuración del solicitante |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Disponibilidad:** v3.0+

| Llamada de API: configuración del solicitante |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |
**Disponibilidad:** v3.0+


**Parámetros:**

- *requestorID*: ID único asociado con el programador. Pase el ID único asignado por el Adobe a su sitio cuando se registró por primera vez en el servicio de autenticación de Adobe Pass.

- *signedRequestorID*: una copia del ID del solicitante firmado digitalmente con su clave privada. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *url*: Parámetro opcional; de forma predeterminada, se utiliza el proveedor de servicios de Adobe (http://sp.auth.adobe.com/). Esta matriz permite especificar extremos para los servicios de autenticación y autorización proporcionados por el Adobe (se pueden utilizar diferentes instancias para la depuración). Puede utilizar esto para especificar varias instancias del proveedor de servicios de autenticación de Adobe Pass. Al hacerlo, la lista de MVPD se compone de los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD.

**Llamadas activadas:** `setRequestorComplete()`

Obsoleto:

    public void setRequestor(String requestorId, String signedRequestorId)
    
    public void setRequestor (String requestorId, String signedRequestorId, ArrayList)&lt;string> url)

[Volver a la API de Android...](#api)

### setRequestorComplete {#setRequestorComplete}

**Descripción:** Llamada de retorno activada por el Access Enabler que informa a la aplicación de que la fase de configuración ha finalizado. Esto indica que la aplicación puede empezar a emitir solicitudes de derechos. La aplicación no puede emitir solicitudes de asignación de derechos hasta que se complete la fase de configuración.

| Llamada de retorno: configuración del solicitante completa |
| --- |
| java public void setRequestorComplete(int status) |

**Disponibilidad:** v1.0+

**Parámetros:**

- *status*: Puede tomar uno de los siguientes valores:
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - la fase de configuración se completó correctamente
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - fase de configuración fallida
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - la fase de configuración se completó correctamente
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - fase de configuración fallida

**Activado por:** `setRequestor()`

[Volver a la API de Android...](#api)

### setOptions {#setOptions}

**Descripción:** Configura las opciones globales del SDK. Acepta un **Mapa\&lt;string string=&quot;&quot;>** como argumento. Los valores del mapa se pasan al servidor junto con cada llamada de red que realice el SDK.

Los valores se pasan al servidor independientemente del flujo actual (autenticación/autorización). Si desea cambiar los valores, puede llamar a este método en cualquier momento.

| Llamada de API: setOptions |
| --- |
| public void setOptions(HashMap)&lt;string string=&quot;&quot;> options) |

**Disponibilidad:** v1.9.2+

**Parámetros:**

- *opciones*: Un mapa&lt;string string=&quot;&quot;> que contiene las opciones globales de SDK. Actualmente están disponibles las siguientes opciones:
   - **applicationProfile** : se puede utilizar para realizar configuraciones de servidor basadas en este valor.
   - **ap_vi** : ID del Experience Cloud (visitorID). Este valor puede utilizarse posteriormente en informes de análisis avanzados.
   - **ap_ai** - El ID de publicidad
   - **device_info** - Información del cliente como se describe aquí: [Pasar la conexión y aplicación del dispositivo de información del cliente](/help/authentication/passing-client-information-device-connection-and-application.md).

[Volver al principio...](#apis)


### checkAuthentication {#checkAuthN}

**Descripción:** Comprueba el estado de autenticación. Para ello, busca un token de autenticación válido en el espacio de almacenamiento del token local. Llamar a este método no realiza llamadas de red. La aplicación la utiliza para consultar el estado de autenticación del usuario y actualizar la interfaz de usuario en consecuencia (es decir, actualizar la interfaz de usuario de inicio de sesión/cierre de sesión). El estado de autenticación se comunica a la aplicación a través de la [*setAuthenticationStatus()*](#setAuthNStatus) devolución de llamada.

Si una MVPD admite la función &quot;Autenticación por solicitante&quot;, se pueden almacenar varios tokens de autenticación en un dispositivo.  Para obtener más información sobre esta función, consulte la [Directrices de almacenamiento en caché](#$caching) de la Información general técnica de Android.

| Llamada de API: comprobar el estado de autenticación |
| --- |
| public void checkAuthentication() |

**Disponibilidad:** v1.0+

**Parámetros:** Ninguno

**Llamadas activadas:** `setAuthenticationStatus()`

[Volver a la API de Android...](#api)


### getAuthentication {#getAuthN}

**Descripción:** Inicia el flujo de trabajo de autenticación completo. Se inicia comprobando el estado de autenticación. Si no se ha autenticado ya, se inicia el flujo de autenticación estado-máquina:

- Si el último intento de autenticación se realizó correctamente, se omite la fase de selección de MVPD y la variable [*navigationToUrl()*](#navigagteToUrl) se activa la devolución de llamada. La aplicación utiliza esta llamada de retorno para crear una instancia del control WebView que presenta al usuario la página de inicio de sesión de la MVPD.
- Si el último intento de autenticación no se realizó correctamente o si el usuario cerró sesión explícitamente, el [*displayProviderDialog()*](#displayProviderDialog) se activa la devolución de llamada. La aplicación utiliza esta llamada de retorno para mostrar la interfaz de usuario de selección de MVPD. Además, la aplicación debe reanudar el flujo de autenticación informando a la biblioteca de habilitación de acceso de la selección de MVPD del usuario a través de la [setSelectedProvider()](#setSelectedProvider) método.

A medida que las credenciales del usuario se verifican en la página de inicio de sesión de MVPD, la aplicación debe supervisar las operaciones de redirección múltiples que se producen mientras el usuario se autentica en la página de inicio de sesión de MVPD. Cuando se especifican las credenciales correctas, el control WebView se redirige a una dirección URL personalizada definida por el *AccessEnabler.ADOBEPASS\_REDIRECT\_URL* constante. WebView no pretende cargar esta dirección URL. La aplicación debe interceptar esta URL e interpretar este evento como una señal de que la fase de inicio de sesión ha finalizado. Luego debe entregar el control al Habilitador de acceso para completar el flujo de autenticación (llamando al *getAuthenticationToken()* método).

Si una MVPD admite la función &quot;Autenticación por solicitante&quot;, se pueden almacenar varios tokens de autenticación en un dispositivo (uno por programador).  Para obtener más información sobre esta función, consulte la [Directrices de almacenamiento en caché](#$caching) de la Información general técnica de Android.

Finalmente, el estado de autenticación se comunica a la aplicación a través de la *setAuthenticationStatus()* devolución de llamada.



| Llamada de API: inicia el flujo de autenticación |
| --- |
| public void getAuthentication() |

**Disponibilidad:** v1.0+

| Llamada de API: inicia el flujo de autenticación |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;string object=&quot;&quot;> genericData) |

**Disponibilidad:** v1.8+

**Parámetros:**

- *forceAuthen*: un indicador que especifica si se debe iniciar el flujo de autenticación, independientemente de si el usuario ya se ha autenticado o no.
- *datos*: Mapa formado por pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar el SDK.

**Llamadas activadas:** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Volver a la API de Android...](#api)

### displayProviderDialog {#displayProviderDialog}

**Descripción** La llamada de retorno se activa mediante el Habilitador de acceso para informar a la aplicación de que es necesario crear una instancia de los elementos de la interfaz de usuario adecuados para permitir que el usuario seleccione la MVPD deseada. La llamada de retorno proporciona una lista de objetos MVPD con información adicional que puede ayudar a crear correctamente el panel de la interfaz de usuario de selección (como la URL que señala al logotipo de MVPD, un nombre para mostrar descriptivo, etc.)

Una vez que el usuario ha seleccionado la MVPD deseada, la aplicación de capa superior debe reanudar el flujo de autenticación llamando a *setSelectedProvider()* y pasándole el ID de la MVPD correspondiente a la selección del usuario.

>[!NOTE]
>
> Cancelación del flujo de autenticación
> </br></br>
> Tenga en cuenta que este es un punto en el que el usuario tiene la capacidad de pulsar el botón &quot;Atrás&quot;, lo que equivale a anular el flujo de autenticación. En este caso, la aplicación debe llamar a la función `setSelectedProvider()` método, pasar *null* como parámetro, para dar al Habilitador de acceso la oportunidad de restablecer su estado de autenticación-máquina.

| Llamada de retorno: mostrar la IU de selección de MVPD |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Disponibilidad:** v1.0+

**Parámetros**:

- *mvpds*: lista de objetos MVPD que contienen información relacionada con MVPD que la aplicación puede utilizar para crear los elementos de la interfaz de usuario de selección de MVPD.

**Activado por:** `getAuthentication(), getAuthorization()`

[Volver a la API de Android...](#api)


### setSelectedProvider {#setSelectedProvider}

**Descripción:** La aplicación llama a este método para informar al Habilitador de acceso de la selección de MVPD del usuario. La aplicación puede utilizar este método para seleccionar o cambiar el proveedor de servicios utilizado para la autenticación.

Si la MVPD seleccionada es una MVPD TempPass, se autenticará automáticamente con esa MVPD sin necesidad de llamar posteriormente a getAuthentication().

Tenga en cuenta que esto no es posible en el caso del pase temporal promocional, en el que se proporcionan parámetros adicionales en el método getAuthentication().

Al pasar *null* como parámetro, el Access Enabler supone que el usuario ha cancelado el flujo de autenticación (es decir, ha pulsado el botón &quot;Atrás&quot;), y responde restableciendo el estado de autenticación-máquina y llamando a *setAuthenticationStatus()* devolución de llamada con `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` código de error.

| Llamada de API: establezca el proveedor seleccionado actualmente |
| --- |
| public void setSelectedProvider(String mvpdId) |

**Disponibilidad:** v1.0+

**Parámetros:** Ninguno

**Llamadas activadas:** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Volver a la API de Android...](#api)


### navigationToUrl {#navigagteToUrl}

**Obsoleto:** A partir del SDK 3.0 de Android, navegarToUrl se utiliza solo si Chrome Custom Tab no está presente en el dispositivo

**Descripción:** Llamada de retorno activada por el Access Enabler, que informa a la aplicación de que el usuario debe tener la página de inicio de sesión de MVPD para poder introducir sus credenciales. El Access Enabler pasa como parámetro la dirección URL de la página de inicio de sesión de MVPD. La aplicación debe crear una instancia de un control WebView y dirigirlo a esta dirección URL. Además, la aplicación es necesaria para supervisar las direcciones URL cargadas por el control WebView e interceptar la operación de redirección dirigida a la dirección URL personalizada definida por el `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` constante. Tras este evento, la aplicación debe cerrar u ocultar el control WebView y devolver el control a la biblioteca del Habilitador de acceso llamando a la función *getAuthenticationToken()* método. Access Enabler completa el flujo de autenticación recuperando el token de autenticación del servidor back-end y almacenándolo localmente en el almacenamiento de tokens.

>[!WARNING]
>
> **Cancelación del flujo de autenticación**  <br>Tenga en cuenta que este es un punto en el que el usuario tiene la capacidad de pulsar el botón &quot;Atrás&quot;, lo que equivale a anular el flujo de autenticación. En este caso, la aplicación debe llamar a la función _setSelectedProvider()_ paso de método _null_ como parámetro y dando la oportunidad al Habilitador de acceso de restablecer su estado de autenticación-máquina.

| Callback: mostrar la página de inicio de sesión de MVPD |
| --- |
| public void navegarToUrl(String url) |

**Disponibilidad:** v1.0+

**Parámetros:**

- *url*: URL que señala a la página de inicio de sesión de la MVPD

**Activado por:** `getAuthentication(), setSelectedProvider()`

[Volver a la API de Android...](#api)


### getAuthenticationToken {#getAuthNToken}

**Obsoleto:** A partir del SDK 3.0 de Android, ya que Chrome Custom Tab se utiliza para la autenticación, este método ya no se utiliza desde la aplicación.

**Descripción:** Completa el flujo de autenticación solicitando el token de autenticación al servidor back-end. La aplicación debe llamar a este método únicamente en respuesta a un evento en el que el control WebView que aloja la página de inicio de sesión de MVPD se redirige a la dirección URL personalizada definida por la variable `AccessEnabler.ADOBEPASS_REDIRECT_URL` constante.

| Llamada de API: recuperar el token de autenticación |
| --- |
| public void getAuthenticationToken() |

**Disponibilidad:** v1.0+

**Parámetros:**

- *galletas*: cookies que se establecen en el dominio de destino (consulte la aplicación de demostración en el SDK para obtener una implementación de referencia).

**Llamadas activadas:** `setAuthenticationStatus()`, `sendTrackingData()`

[Volver a la API de Android...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Descripción:** Llamada de retorno activada por el Habilitador de acceso, que informa a la aplicación del estado del flujo de autenticación. Hay muchos lugares donde este flujo puede fallar, ya sea como resultado de la interacción del usuario o debido a otros escenarios imprevistos (es decir, problemas de conectividad de red, etc.). Esta llamada de retorno informa a la aplicación del estado de éxito/error del flujo de autenticación, a la vez que proporciona información adicional sobre el motivo del error, cuando es necesario.

| Callback: informar del estado del flujo de autenticación |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**Disponibilidad:** v1.0+

**Parámetros:**

- *status*: Puede tomar uno de los siguientes valores:
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - el flujo de autenticación se completó correctamente
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - error del flujo de autenticación
- *código*: Motivo del error. If *status* es `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`, entonces *código* es una cadena vacía (es decir, definida por la variable `AccessEnablerConstants.USER_AUTHENTICATED` constante). En caso de error, este parámetro puede tomar uno de los siguientes valores:
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - El usuario no está autenticado. En respuesta a la *checkAuthentication()* llamada al método cuando no hay ningún token de autenticación válido en la caché de token local.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler ha restablecido el estado de autenticación-máquina después de que se aprobara la aplicación de capa superior *null* hasta `setSelectedProvider()` para anular el flujo de autenticación.  Es de suponer que el usuario ha cancelado el flujo de autenticación (es decir, ha presionado el botón &quot;Atrás&quot;).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - El flujo de autenticación falló debido a razones como la no disponibilidad de la red o el usuario canceló el flujo de autenticación explícitamente.

**Activado por:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Volver a la API de Android...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**Obsoleto:** A partir del SDK 3.6 de Android, la API preautorizada sustituye a checkPreauthorizedResources, proporcionando códigos de error ampliados.

**Descripción:** La aplicación utiliza este método para determinar si el usuario ya está autorizado para ver recursos protegidos específicos. El propósito principal de este método es recuperar información para utilizarla en la decoración de la interfaz de usuario (por ejemplo, para indicar el estado de acceso con los iconos de bloqueo y desbloqueo).

| Llamada de API: establezca el proveedor seleccionado actualmente |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilidad:** v1.3+

**Parámetros:** El `resources` El parámetro es una matriz de recursos para los que se debe comprobar la autorización. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El ID de recurso está sujeto a las mismas limitaciones que el ID de recurso de la variable `getAuthorization()` llamada, es decir, debe ser un valor acordado establecido entre el Programador y el MVPD o un fragmento de medios RSS.

**Llamada de retorno activada:** `preauthorizedResources()`

[Volver a la API de Android...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**Obsoleto:** A partir del SDK 3.6 de Android, la API preautorizada sustituye a checkPreauthorizedResources, proporcionando códigos de error ampliados.

**Descripción:** La aplicación utiliza este método para determinar si el usuario ya está autorizado para ver recursos protegidos específicos. El propósito principal de este método es recuperar información para utilizarla en la decoración de la interfaz de usuario (por ejemplo, para indicar el estado de acceso con los iconos de bloqueo y desbloqueo).

| Llamada de API: establezca el proveedor seleccionado actualmente |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Disponibilidad:** v3.1+

**Parámetros:** El `resources` El parámetro es una matriz de recursos para los que se debe comprobar la autorización. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El ID de recurso está sujeto a las mismas limitaciones que el ID de recurso de la variable `getAuthorization()` llamada, es decir, debe ser un valor acordado establecido entre el Programador y el MVPD o un fragmento de medios RSS.

El `cache` el parámetro especifica si se puede utilizar o no la respuesta de preautorización en caché. De forma predeterminada, la caché es verdadera, el SDK devolverá una respuesta almacenada en caché anteriormente si está disponible.

**Llamada de retorno activada:** `preauthorizedResources()`

[Volver a la API de Android...](#api)

### preauthorizedResources {#preauthResources}

**Obsoleto:** A partir del SDK 3.6 de Android, la API preautorizada sustituye a checkPreauthorizedResources, proporcionando códigos de error ampliados. no se llamará a la devolución de llamada preauthorizedResources en la nueva API.


**Descripción:** Llamada de retorno activada por checkPreauthorizedResources(). Proporciona una lista de recursos que el usuario ya tiene autorización para ver.

| Llamada de API: establezca el proveedor seleccionado actualmente |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilidad:** v1.3+

**Parámetros:** El `resources` El parámetro es una matriz de recursos que el usuario ya tiene autorización para ver.

**Activado por:** `checkPreauthorizedResources()`

[Volver a la API de Android...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Descripción:** La aplicación utiliza este método para comprobar el estado de autorización. Se inicia comprobando primero el estado de autenticación. Si no se autentica, la variable *setTokenRequestFailed()* la llamada de retorno se activa y el método se cierra. Si el usuario está autenticado, también almacena en déclencheur el flujo de autorización. Consulte los detalles en la *getAuthorization()* método.

| Llamada de API: comprobar el estado de autorización |
| --- |
| public void checkAuthorization(String resourceId) |

**Disponibilidad:** v1.0+

| Llamada de API: comprobar el estado de autorización |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;string object=&quot;&quot;> genericData) |

**Disponibilidad:** v1.8+

**Parámetros:**

- *resourceId*: ID del recurso para el que el usuario solicita autorización.
- *datos*: Mapa formado por pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar el SDK.

**Llamadas activadas:** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Volver a la API de Android...](#api)


### <span id="getAuthZ"></span>getAuthorization

**Descripción:** La aplicación utiliza este método para iniciar el flujo de autorización. Si el usuario aún no se ha autenticado, también inicia el flujo de autenticación. Si el usuario se autentica, el Habilitador de acceso procede a emitir solicitudes para el token de autorización (si no hay ningún token de autorización válido en la caché de tokens local) y para el token de medios de corta duración. Una vez obtenido el token de medios corto, el flujo de autorización se considera completo. El *setToken()* la llamada de retorno se activa y el token de medios corto se entrega como parámetro a la aplicación. Si, por cualquier motivo, la autorización falla, la variable *tokenRequestFailed()* la llamada de retorno se activa y se proporcionan el código de error y los detalles.

| Llamada de API: iniciar el flujo de autorización |
| --- |
| public void getAuthorization(String resourceId) |

**Disponibilidad:** v1.0+

| Llamada de API: iniciar el flujo de autorización |
| --- |
| public void getAuthorization(String resourceId, Map&lt;string object=&quot;&quot;> genericData) |

**Disponibilidad:** v1.8+

**Parámetros:**

- *resourceId*: ID del recurso para el que el usuario solicita autorización.
- *datos*: Mapa formado por pares de clave-valor que se enviarán al servicio de pase de TV de pago. El Adobe de puede utilizar estos datos para habilitar futuras funciones sin cambiar el SDK.

**Llamadas activadas:** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Llamadas de retorno adicionales activadas**  <br> Este método también puede almacenar en déclencheur las siguientes llamadas de retorno (si también se inicia el flujo de autenticación): *setAuthenticationStatus()*, *displayProviderDialog()*, *navigationToUrl()*

**NOTA: Utilice checkAuthorization() en lugar de getAuthorization() siempre que sea posible. El método getAuthorization() iniciará un flujo de autenticación completo (si el usuario no está autenticado) y esto podría llevar a una implementación complicada por parte del programador.**

[Volver a la API de Android...](#api)


### setToken {#setToken}

**Descripción:** Llamada de retorno activada por el Habilitador de acceso que informa a la aplicación de que el flujo de autorización se completó correctamente. El token de medios de corta duración también se entrega como parámetro.

| Llamada de retorno: flujo de autorización completado correctamente |
| --- |
| public void setToken(String token, String resourceId) |

**Disponibilidad:** v1.0+

**Parámetros:**

- *token*: el token de medios de corta duración
- *resourceId*: El recurso para el que se obtuvo la autorización

**Activado por:** `checkAuthorization()`, `getAuthorization()`


[Volver a la API de Android...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Descripción:** Llamada de retorno activada por el Habilitador de acceso que informa a la aplicación de capa superior de que el flujo de autorización ha fallado.

| Callback: error del flujo de autorización |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        Cadena errorCode, Cadena errorDescription) |

**Disponibilidad:** v1.0+

**Parámetros:**

- *resourceId*: El recurso para el que se obtuvo la autorización
- *errorCode*: código de error asociado al escenario de error. Valores posibles:
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - El usuario no ha podido autorizar el recurso en cuestión
- *errorDescription*: Detalles adicionales sobre el escenario de error. Si esta cadena descriptiva no está disponible por ningún motivo, la autenticación de Adobe Pass envía una cadena vacía **(&quot;&quot;)**.

  Una MVPD puede utilizar esta cadena para pasar mensajes de error personalizados o mensajes relacionados con las ventas. Por ejemplo, si se deniega a un suscriptor la autorización para un recurso, la MVPD podría enviar un mensaje como: &quot;Actualmente no tiene acceso a este canal en su paquete. Si desea actualizar el paquete, haga clic aquí.&quot; El mensaje lo pasa la autenticación de Adobe Pass a través de esta llamada de retorno al programador, que tiene la opción de mostrarlo o ignorarlo. La autenticación de Adobe Pass también puede utilizar este parámetro para notificar la condición que podría haber provocado un error. Por ejemplo, &quot;Se produjo un error de red al comunicarse con el servicio de autorización del proveedor&quot;.

**Activado por:** `checkAuthorization(), getAuthorization()`

[Volver a la API de Android...](#api)

### cierre de sesión {#logout}

**Descripción:** Utilice este método para iniciar el flujo de cierre de sesión. El cierre de sesión es el resultado de una serie de operaciones de redirección HTTP debido al hecho de que el usuario debe cerrar la sesión tanto desde los servidores de autenticación de Adobe Pass como desde los servidores de MVPD. Como resultado, este flujo no se puede completar con una simple solicitud HTTP emitida por la biblioteca del Habilitador de acceso. El SDK utiliza unas pestañas personalizadas de Chrome para ejecutar las operaciones de redirección HTTP. Este flujo será visible para el usuario y se cerrará cuando se complete

| Llamada de API: iniciar el flujo de cierre de sesión |
| --- |
| public void logout() |

**Disponibilidad:** v1.0+

**Parámetros:** Ninguno

**Llamadas activadas:**

- `navigateToUrl()` para la versión de SDK anterior a 3.0
- `setAuthenticationStatus()` para la versión del SDK > 3.0


[Volver a la API de Android...](#api)


### getSelectedProvider {#getSelectedProvider}

**Descripción:** Utilice este método para determinar el proveedor seleccionado actualmente.

| Llamada de API: determina la MVPD seleccionada actualmente |
| --- |
| public void getSelectedProvider() |

**Disponibilidad:** v1.0+

**Parámetros:** Ninguno

**Llamadas activadas:** `selectedProvider()`

[Volver a la API de Android...](#api)


### <span id="selectedProvider"></span>selectedProvider

**Descripción:** Llamada de retorno activada por el Access Enabler que envía información sobre la MVPD seleccionada actualmente a la aplicación.

| Callback: información sobre la MVPD seleccionada actualmente. |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**Disponibilidad:** v1.0+

**Parámetros:**

- *mvpd*: objeto que contiene información sobre la MVPD seleccionada actualmente

**Activado por:** `getSelectedProvider()`

[Volver a la API de Android...](#api)


### getMetadata {#getMetadata}

**Descripción:** Utilice este método para recuperar la información expuesta como metadatos por la biblioteca del Habilitador de acceso. La aplicación puede tener acceso a esta información proporcionando un objeto MetadataKey compuesto.

| Llamada de API: consultar los metadatos en AccessEnabler |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Disponibilidad:** v1.0+

Hay dos tipos de metadatos disponibles para los programadores:

- Metadatos estáticos (TTL del token de autenticación, TTL del token de autorización e ID de dispositivo)
- Metadatos del usuario (información específica del usuario, como el ID de usuario y el código postal; pasados de una MVPD al dispositivo de un usuario durante los flujos de autenticación o autorización)

**Parámetros:**

- *metadataKey*: estructura de datos que encapsula una variable clave y args, con el siguiente significado:
   - Si la clave es `METADATA_KEY_USER_META` y args contiene un objeto SerializableNameValuePair denominado = `METADATA_ARG_USER_META` y valor = `[metadata_name]`A continuación, se realiza la consulta de los metadatos del usuario. La lista actual de tipos de metadatos de usuario disponibles:
      - `zip` - Código postal

      - `householdID` - Identificador del hogar. Si una MVPD no admite subcuentas, será idéntico a `userID`.

      - `maxRating` - Calificación parental máxima para el usuario

      - `userID` : el identificador de usuario. Si una MVPD admite subcuentas y el usuario no es la cuenta principal, `userID` será diferente a `householdID`.

      - `channelID` - Una lista de canales que el usuario puede ver
   - Si la clave es `METADATA_KEY_DEVICE_ID` a continuación, se realiza la consulta para obtener el id del dispositivo actual. Tenga en cuenta que esta función está desactivada de forma predeterminada y los programadores deben ponerse en contacto con el Adobe para obtener información sobre la habilitación y las tarifas.
   - Si la clave es `METADATA_KEY_TTL_AUTHZ` y args contiene un objeto SerializableNameValuePair denominado = `METADATA_ARG_RESOURCE_ID` y valor = `[resource_id]`, se realiza la consulta para obtener la hora de caducidad del token de autorización asociado al recurso especificado.
   - Si la clave es `METADATA_KEY_TTL_AUTHN` a continuación, se realiza la consulta para obtener el tiempo de caducidad del token de autenticación.



>[!NOTE]
>
>Para SDK 3.4.0, las constantes : `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` están disponibles en com.adobe.adobepass.accessenabler.api.profile.UserProfileService.



>[!NOTE]
>
>Los metadatos de usuario reales disponibles para un programador dependen de lo que una MVPD ponga a disposición.  Esta lista se ampliará a medida que haya nuevos metadatos disponibles y añadidos al sistema de autenticación de Adobe Pass.

**Llamadas activadas:** [`setMetadataStatus()`](#setMetadaStatus)

**Más información:** [Metadatos del usuario](/help/authentication/user-metadata-feature.md)

[Volver a la API de Android...](#api)

### setMetadataStatus {#setMetadaStatus}

**Descripción:** Llamada de retorno activada por el activador de acceso que entrega los metadatos solicitados mediante una *getMetadata()* llamada.

| Callback: resultado de la solicitud de recuperación de metadatos |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Disponibilidad:** v1.0+

**Parámetros:**

- *key*: el objeto MetadataKey que contiene la clave para la que se solicita el valor de los metadatos y los parámetros asociados (consulte la aplicación de demostración para una implementación de referencia).
- *resultado*: un objeto compuesto que contiene los metadatos solicitados. El objeto tiene los campos siguientes:
   - *simpleResult*: una cadena que representa el valor de los metadatos cuando se realizó la solicitud para el TTL de autenticación, el TTL de autorización o el ID de dispositivo. Este valor es nulo si se realizó la solicitud de metadatos del usuario.

   - *userMetadataResult*: un objeto que contiene la representación Java de una carga útil de metadatos de usuario JSON.\
     Por ejemplo:

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

se traduce a Java como:

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**La estructura real de los objetos de metadatos de usuario es similar a la siguiente:**

```json
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
          }
```

Este valor es nulo cuando se realizó la solicitud de metadatos simples (TTL de autenticación, TTL de autorización o ID de dispositivo).

- *cifrado*: Valor booleano que especifica si los metadatos recuperados están cifrados o no. Este parámetro solo es significativo para solicitudes de metadatos del usuario, no tiene significado para metadatos estáticos (por ejemplo, TTL de autenticación) que siempre se reciben sin cifrar. Si este parámetro se establece en True, depende del programador obtener el valor de metadatos del usuario sin cifrar realizando un descifrado RSA con la clave privada de la lista blanca (la misma clave privada que se utiliza para la firma del ID del solicitante en el [`setRequestor`](#setRequestor) llamada).

**Activado por:** [`getMetadata()`](#getMetadata)

**Más información:** [Metadatos del usuario](/help/authentication/user-metadata-feature.md)


[Volver a la API de Android...](#api)


### getVersion {#getVersion}

**Descripción:** Este método se puede utilizar para recuperar la versión de la biblioteca AccessEnabler.

| Llamada de API: obtener la versión de AccessEnabler |
| --- |
| ```public static String getVersion()``` |


[Volver a la API de Android...](#api)

</br>

## Seguimiento de eventos {#tracking}

El activador de acceso déclencheur una llamada de retorno adicional que no está necesariamente relacionada con los flujos de derechos. Implementación de la función de devolución de llamada de seguimiento de eventos denominada *sendTrackingData()* es opcional, pero permite a la aplicación realizar un seguimiento de eventos específicos y compilar estadísticas como el número de intentos de autenticación/autorización correctos/fallidos. A continuación se muestra la especificación para *sendTrackingData()* callback:


### sendTrackingData {#sendTrackingData}

**Descripción:** Llamada de retorno activada por el Habilitador de acceso que indica a la aplicación la ocurrencia de varios eventos, como la finalización/error de los flujos de autenticación/autorización. sendTrackingData() también informa del tipo de dispositivo, el tipo de cliente del Access Enabler y el sistema operativo.

>[!WARNING]
>
> El tipo de dispositivo y el sistema operativo se derivan del uso de una biblioteca Java pública ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) y la cadena del agente de usuario. Tenga en cuenta que esta información se proporciona solo como una forma grosera de desglosar las métricas operativas en categorías de dispositivos, pero que ese Adobe no puede responsabilizarse de los resultados incorrectos. Utilice la nueva funcionalidad en consecuencia.


- Valores posibles para el tipo de dispositivo:
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`


- Valores posibles para el tipo de cliente del Habilitador de acceso:
   - `flash`
   - `html5`
   - `ios`
   - `android`

</br>

| Callback: seguimiento de eventos |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Disponibilidad:** v1.0+

**Parámetros:**

- *evento*: el evento que se está rastreando. Existen tres tipos de eventos de seguimiento posibles:
   - **authorizationDetection:** cada vez que se devuelve una solicitud de token de autorización (el tipo de evento es `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** cada vez que se produce una comprobación de autenticación (el tipo de evento es `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** cuando el usuario selecciona una MVPD en el formulario de selección de MVPD (el tipo de evento es `EVENT_MVPD_SELECTION`)
- *datos*: datos adicionales asociados al evento del que se informa. Estos datos se presentan en forma de lista de valores.

A continuación se proporcionan instrucciones para interpretar los valores de la variable *datos*
matriz:

- Para el tipo de evento *`EVENT_AUTHN_DETECTION`:*
   - **0** - Si la solicitud de token se realizó correctamente (verdadero/falso) y si lo anterior es verdadero:
   - **1** - Cadena de ID de MVPD
   - **2** - GUID (MD5 con hash)
   - **3** - El token ya está en la caché (verdadero/falso)
   - **4** - Tipo de dispositivo
   - **5** - Tipo de cliente de Access Enabler
   - **6** - Tipo de sistema operativo

- Para el tipo de evento `EVENT_AUTHZ_DETECTION`
   - **0** - Si la solicitud de token se realizó correctamente (verdadero/falso) y, si se realizó correctamente:
   - **1** - ID de MVPD
   - **2** - GUID (MD5 con hash)
   - **3** - El token ya está en la caché (verdadero/falso)
   - **4** - Error
   - **5** - Detalles
   - **6** - Tipo de dispositivo
   - **7** - Tipo de cliente de Access Enabler
   - **8** - Tipo de sistema operativo

- Para el tipo de evento `EVENT_MVPD_SELECTION`
   - **0** - ID de la MVPD seleccionada actualmente
   - **1** - Tipo de dispositivo
   - **2** - Tipo de cliente de Access Enabler
   - **3** - Tipo de sistema operativo

**Activado por:** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Volver a la API de Android...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
