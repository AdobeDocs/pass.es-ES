---
title: Android SDK con registro dinámico de clientes
description: Android SDK con registro dinámico de clientes
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---

# (Heredado) Android SDK con registro de cliente dinámico {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Introducción {#Intro}

Android AccessEnabler SDK para Android se ha modificado para habilitar la autenticación sin usar cookies de sesión. Dado que cada vez más exploradores restringen el acceso a las cookies, es necesario utilizar otro método para permitir la autenticación.

Para Android, el uso de fichas personalizadas de Chrome restringe el acceso a las cookies de otras aplicaciones.

>**Android SDK 3.0.0** presenta:

- el registro dinámico de clientes sustituye al mecanismo de registro de aplicaciones actual en función del ID de solicitante firmado y la autenticación de cookies de sesión
- Pestañas personalizadas de Chrome para flujos de autenticación

>[!NOTE]
>
>Para las versiones de Android anteriores sin compatibilidad con Chrome Custom Tabs, se utilizará la autenticación de WebView similar a la de las versiones de SDK de AccessEnabler anteriores.


## Registro dinámico de clientes {#DCR}

Android SDK v3.0+ utilizará el procedimiento de registro de cliente dinámico como se define en [Información general sobre el registro de cliente dinámico](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


## Demostración de funciones {#Demo}

Vea [este seminario web](https://my.adobeconnect.com/pzkp8ujrigg1/) que proporciona más contexto sobre la funcionalidad y contiene una demostración sobre cómo administrar las instrucciones de software mediante el panel de TVE y cómo probar las generadas mediante una aplicación de demostración proporcionada por Adobe como parte de Android SDK.

## Cambios de API {#API}


### Factory.getInstance

**Descripción:** crea una instancia del objeto de habilitación de acceso. Debe haber una sola instancia de Access Enabler por cada instancia de aplicación.

| Llamada de API: constructor |
| --- |
| public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        inicia AccessEnablerException |


**Disponibilidad:** v3.0+

**Parámetros:**

- *appContext*: contexto de aplicación de Android
- softwareStatement: valor obtenido del Tablero de TVE o *null* si &quot;software\_statement&quot; está establecido en strings.xml
- redirectUrl : url única, uno de los dominios en orden inverso que se agregó explícitamente en el Tablero de TVE o *null* si &quot;redirect\_uri&quot; está establecido en strings.xml

Nota: si softwareStatement o redirectUrl no son válidos, la aplicación no se inicializará AccessEnabler o no se registrará para la autenticación y autorización de Adobe Pass
</br>
Nota : el parámetro redirectUrl o redirect\_uri en strings.xml debe ser el valor del dominio agregado en el Tablero de TVE para la aplicación en orden inverso ( por ejemplo: para el dominio &quot;adobe.com&quot; agregado en el Tablero de TVE, redirectUrl debe ser &quot;com.adobe&quot;.


### setRequestor

**Descripción:** establece la identidad del canal. A cada canal se le asigna un ID único al registrarse en Adobe para el sistema de autenticación de Adobe Pass. Cuando se trata de SSO y tokens remotos, el estado de autenticación puede cambiar cuando la aplicación está en segundo plano. Se puede volver a llamar a setRequestor cuando la aplicación se pone en primer plano para sincronizar con el estado del sistema (recupere un token remoto si está habilitado el SSO o elimine el token local si se ha cerrado la sesión mientras tanto).

La respuesta del servidor contiene una lista de MVPD junto con información de configuración adjunta a la identidad del canal. El código Access Enabler utiliza internamente la respuesta del servidor. Solo el estado de la operación (es decir, SUCCESS/FAIL) se presenta a la aplicación a través de la llamada de retorno setRequestorComplete().

Si no se usa el parámetro *urls*, la llamada de red resultante se dirigirá a la dirección URL del proveedor de servicios predeterminado: el entorno de producción/versión de Adobe.

Si se proporciona un valor para el parámetro *urls*, la llamada de red resultante se dirigirá a todas las direcciones URL proporcionadas en el parámetro *urls*. Todas las solicitudes de configuración se activan simultáneamente en subprocesos independientes. El primer respondedor tiene prioridad al compilar la lista de MVPD. Para cada MVPD de la lista, el Habilitador de acceso recuerda la URL del proveedor de servicios asociado. Todas las solicitudes de derechos subsiguientes se dirigen a la URL asociada al proveedor de servicios emparejado con el MVPD de destino durante la fase de configuración.

| Llamada de API: configuración del solicitante |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Disponibilidad:** v3.0+

| Llamada de API: configuración del solicitante |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilidad:** v3.0+

**Parámetros:**

- *requestorID*: El ID único asociado con el canal. Pase el ID único asignado por Adobe a su sitio la primera vez que se registró con el servicio de autenticación de Adobe Pass.
- *urls*: Parámetro opcional; de forma predeterminada, el proveedor de servicios de Adobe se usa [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/). Esta matriz permite especificar extremos para los servicios de autenticación y autorización proporcionados por Adobe (se pueden utilizar diferentes instancias para la depuración). Puede utilizar esto para especificar varias instancias del proveedor de servicios de autenticación de Adobe Pass. Al hacerlo, la lista MVPD está compuesta por los extremos de todos los proveedores de servicios. Cada MVPD está asociado con el proveedor de servicios más rápido; es decir, el proveedor que respondió primero y que admite ese MVPD.

Obsoleto:

- *signedRequestorID*: Una copia del identificador del solicitante firmado digitalmente con su clave privada. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Llamadas de retorno activadas:** `setRequestorComplete()`

### cierre de sesión

**Descripción:** Utilice este método para iniciar el flujo de cierre de sesión. El cierre de sesión es el resultado de una serie de operaciones de redirección HTTP debido al hecho de que el usuario debe cerrar la sesión tanto desde los servidores de autenticación de Adobe Pass como desde los servidores de MVPD. Como resultado, este flujo abrirá una ventana de Chrome CustomTab para ejecutar el cierre de sesión.

| Llamada de API: iniciar el flujo de cierre de sesión |
| --- |
| public void logout() |

**Disponibilidad:** v3.0+

**Parámetros:** Ninguno

**Llamadas de retorno activadas:** `setAuthenticationStatus()`
</br></br>

## Flujo de implementación del programador {#Progr}

### **1. Registrar aplicación**

a. Obtenga software\_statement y redirect\_uri de Adobe Pass ( Tablero de TVE )

b. Existen dos opciones para pasar estos valores a Adobe Pass SDK:

En strings.xml, agregue:

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

Llamar a AccessEnabler.getInstance(appContext,softwareStatement,
redirectUrl)


### &#x200B;2. Configurar la aplicación

a. setRequestor(solicitante\_id)

SDK realizará las siguientes operaciones:

- solicitud de registro: con **software\_statement**, SDK obtendrá un **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. Esta información se almacena en el almacenamiento interno de la aplicación.

- obtenga un **access\_token** usando client\_id, client\_secret y grant\_type=&quot;client\_credentials&quot; . Este access\_token se usará en cada llamada realizada por SDK a los servidores de Adobe Pass

**Respuestas de error de token :**

| Respuestas de error | | |
| --- | --- | --- |
| HTTP 400 (solicitud incorrecta) | {&quot;error&quot;: &quot;invalid\_request&quot;} | A la solicitud le falta un parámetro requerido, incluye un valor de parámetro no admitido (que no sea el tipo de concesión), repite un parámetro, incluye varias credenciales, utiliza más de un mecanismo para autenticar al cliente o tiene un formato incorrecto. |
| HTTP 400 (solicitud incorrecta) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Error de autenticación del cliente porque se desconocía el cliente. SDK DEBE volver a registrarse en el servidor de autorización. |
| HTTP 400 (solicitud incorrecta) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | El cliente autenticado no tiene autorización para utilizar este tipo de concesión de autorización. |

- en caso de que una MVPD requiera autenticación pasiva, se abrirá una pestaña personalizada de Chrome para ejecutar la autenticación pasiva con esa MVPD y se cerrará cuando se complete

b. checkAuthentication()

- true : vaya a Autorización
- false : vaya a Seleccionar MVPD

c. getAuthentication : SDK incluirá **access_token** en los parámetros de llamada

- mvpd recordó : ir a setSelectedProvider(mvpd_id)
- mvpd no seleccionado: displayProviderDialog
- mvpd seleccionado: ir a setSelectedProvider(mvpd_id)

d. setSelectedProvider

- La URL de autenticación mvpd\_id se carga en ChromeCustomTabs
- inicio de sesión correcto : delegate.setAuthenticationStatus ( SUCCESS )
- inicio de sesión cancelado : restablecer selección de MVPD
- El esquema URL se establece como &quot;adobepass://redirect_uri&quot; para capturar cuándo se completa la autenticación

e. get/checkAuthorization : SDK incluirá **access_token** en el encabezado como Autorización: Portador **access_token**

- si la autorización es correcta, se realizará una llamada para obtener la
token de medios

f. cierre de sesión:

- SDK eliminará el token válido para el solicitante actual (las autenticaciones obtenidas por otras aplicaciones y no a través de SSO seguirán siendo válidas)
- SDK abrirá las fichas personalizadas de Chrome para llegar al extremo de cierre de sesión de mvpd_id. Una vez finalizado, se cerrarán las fichas personalizadas de Chrome
- El esquema de URL se establece como &quot;adobepass://logout&quot; para capturar el momento en que se completa el cierre de sesión
- el cierre de sesión almacenará en déclencheur sendTrackingData(new Event(EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR) y una llamada de retorno : setAuthenticationStatus(0,&quot;Logout&quot;)

**Nota:**, ya que cada llamada requiere un **token de acceso,** los posibles códigos de error que se indican a continuación se gestionan en SDK.


| Respuestas de error | | |
| --- | ---|--- |
| invalid_request | 400 | La solicitud tiene un formato incorrecto. SDK debe dejar de realizar llamadas al servidor. |
| invalid_client | 403 | El ID de cliente ya no tiene permiso para realizar solicitudes. El SDK DEBE volver a realizar el registro de cliente. |
| access_denied | 401 | El access\_token no es válido. El SDK DEBE solicitar un nuevo access_token. |
