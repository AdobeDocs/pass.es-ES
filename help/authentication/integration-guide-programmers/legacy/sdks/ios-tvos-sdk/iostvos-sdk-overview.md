---
title: Información general sobre iOS/tvOS SDK
description: Información general sobre iOS/tvOS SDK
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '3732'
ht-degree: 0%

---

# Información general sobre iOS/tvOS SDK (heredado) {#iostvos-sdk-overview}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


</br>


## Introducción {#intro}

iOS AccessEnabler es una biblioteca de Objective-C iOS/tvOS que permite a las aplicaciones móviles utilizar la autenticación de Adobe Pass para los servicios de asignación de derechos de TV Everywhere. La implementación consiste en la interfaz *AccessEnabler* que define la API de derechos, y los protocolos *EntitlementDelegate* y *[EntitlementStatus](#ios%20entitlement%20status)* que describen las llamadas de retorno que la biblioteca déclencheur. Se hace referencia a la interfaz junto con el protocolo con un nombre común: la biblioteca AccessEnabler.

## Requisitos de iOS y tvOS {#reqs}

Para conocer los requisitos técnicos actuales relacionados con la plataforma iOS y tvOS y la autenticación de Adobe Pass, consulte [Requisitos de plataforma/dispositivo/herramienta](#ios) y consulte las notas de la versión incluidas con la descarga de SDK. En el resto de esta página, verá secciones que indican cambios que se aplican a versiones de SDK específicas y posteriores. Por ejemplo, la siguiente es una nota legítima sobre SDK 1.7.5:

## Explicación de los flujos de trabajo de cliente nativos {#flows}

Los flujos de trabajo de cliente nativos suelen ser iguales o muy similares a los de los clientes de autenticación de Adobe Pass basados en explorador. Sin embargo, hay algunas excepciones, como se describe a continuación.

- [Flujo de trabajo posterior a la inicialización](#post-init)
- [Flujo de trabajo de autenticación inicial genérica](#generic)
- [Flujo de trabajo de cierre](#logout)


### Flujo de trabajo posterior a la inicialización {#post-init}

Todos los flujos de trabajo de derechos admitidos por AccessEnabler suponen que ha llamado anteriormente a [`setRequestor()`](#setReq) para establecer su identidad. Esta llamada se realiza para proporcionar el ID de solicitante solo una vez, normalmente durante la fase de inicialización/configuración de la aplicación.


Con un cliente nativo de iOS, después de la llamada inicial a [`setRequestor()`](#setReq), tiene la opción de continuar:

- Puede empezar a realizar llamadas de asignación de derechos inmediatamente y permitir que se pongan en cola silenciosamente, si es necesario.

- Puede recibir una confirmación del éxito/error de [`setRequestor()`](#setReq) implementando la devolución de llamada [`setRequestorComplete()`](#setReqComplete).

- Puede realizar las dos acciones anteriores.

Usted elige que su aplicación espere una notificación del éxito de [`setRequestor()`](#setReq) o que confíe en el mecanismo de cola de llamadas de AccessEnabler. Dado que todas las solicitudes de autorización y autenticación subsiguientes necesitan el ID del solicitante y la información de configuración asociada, el método [`setRequestor()`](#setReq) bloquea efectivamente todas las llamadas de API de autenticación y autorización hasta que se complete la inicialización.



### Flujo de trabajo de autenticación inicial genérica {#generic}

El propósito de este flujo de trabajo es iniciar sesión en un usuario con su MVPD. Después de un inicio de sesión correcto, el servidor back-end emite al usuario un token de autenticación. Aunque la autenticación se suele realizar como parte del proceso de autorización, a continuación se describe cómo puede funcionar la autenticación de forma aislada y no se incluye ningún paso de autorización.

Tenga en cuenta que, aunque este flujo de trabajo difiere para los clientes nativos del flujo de trabajo de autenticación basado en explorador habitual, los pasos del 1 al 5 son los mismos para los clientes nativos y los basados en explorador.

1. La aplicación inicia el flujo de trabajo de autenticación con una llamada al método de API `getAuthentication() `de AccessEnabler, que comprueba si hay un token de autenticación en caché válido.
1. Si el usuario está autenticado actualmente, AccessEnabler llama a la función de devolución de llamada [`setAuthenticationStatus()`](#setAuthNStatus), pasando un estado de autenticación que indica que se ha realizado correctamente y finalizando el flujo.
1. Si el usuario no está autenticado actualmente, AccessEnabler continúa el flujo de autenticación determinando si el último intento de autenticación del usuario se realizó correctamente con un MVPD determinado. Si se almacena en caché un MVPD ID Y el indicador `canAuthenticate` es verdadero O si se seleccionó un MVPD mediante [`setSelectedProvider()`](#setSelProv), no se solicitará al usuario el cuadro de diálogo de selección de MVPD. El flujo de autenticación sigue utilizando el valor almacenado en caché de MVPD (es decir, el mismo MVPD que se utilizó durante la última autenticación correcta). Se realiza una llamada de red al servidor back-end y se redirige al usuario a la página de inicio de sesión de MVPD (paso 6 a continuación).
1. Si no se almacena en caché ningún MVPD ID Y no se seleccionó ningún MVPD con [`setSelectedProvider()`](#setSelProv) O si el indicador `canAuthenticate` está establecido en falso, se llama a la devolución de llamada [`displayProviderDialog()`](#dispProvDialog). Esta llamada de retorno indica a la aplicación que cree la interfaz de usuario que presenta al usuario una lista de MVPD para elegir. Se proporciona una matriz de objetos de MVPD que contiene la información necesaria para crear el selector de MVPD. Cada objeto de MVPD describe una entidad de MVPD y contiene información como el ID de MVPD (por ejemplo, XFINITY, AT\&amp;T, etc.) y la URL donde se puede encontrar el logotipo de MVPD.
1. Una vez seleccionada una MVPD determinada, la aplicación debe informar al AccessEnabler de la elección del usuario. Una vez que el usuario selecciona el MVPD deseado, se informa al AccessEnabler de la selección del usuario mediante una llamada al método [`setSelectedProvider()`](#setSelProv).
1. IOS AccessEnabler llama a su llamada de retorno `navigateToUrl:` o a su llamada de retorno `navigateToUrl:useSVC:` para redirigir al usuario a la página de inicio de sesión de MVPD. Al activar cualquiera de ellas, AccessEnabler realiza una solicitud a la aplicación para crear un controlador `UIWebView/WKWebView or SFSafariViewController` y cargar la dirección URL proporcionada en el parámetro `url` de la devolución de llamada. Dirección URL del extremo de autenticación en el servidor back-end. Para tvOS AccessEnabler, se llama a la devolución de llamada [status()](#status_callback_implementation) con un parámetro `statusDictionary` y se inicia inmediatamente el sondeo para la segunda autenticación de pantalla. El `statusDictionary` contiene el `registration code` que debe usarse para la segunda autenticación de pantalla.
1. En el caso del AccessEnabler de iOS, el usuario aterriza en la página de inicio de sesión de MVPD para escribir sus credenciales a través del medio del controlador `UIWebView/WKWebView or SFSafariViewController ` de la aplicación. Tenga en cuenta que durante esta transferencia se producen varias operaciones de redireccionamiento y la aplicación debe supervisar las direcciones URL que carga el controlador durante las diversas operaciones de redireccionamiento.
1. En el caso del AccessEnabler de iOS, cuando el controlador `UIWebView/WKWebView or SFSafariViewController` carga una dirección URL personalizada específica, la aplicación debe cerrar el controlador y llamar al método de API `handleExternalURL:url `de AccessEnabler. Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de autenticación se ha completado y de que es seguro cerrar el controlador `UIWebView/WKWebView or SFSafariViewController`. En caso de que la aplicación necesite utilizar un controlador `SFSafariViewController `la dirección URL personalizada específica está definida por `application's custom scheme` (p. ej.: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica está definida por la constante `ADOBEPASS_REDIRECT_URL` (p. ej. `adobepass://ios.app`).
1. Una vez que la aplicación cierra el controlador `UIWebView/WKWebView or SFSafariViewController` y llama al método de API `handleExternalURL:url `de AccessEnabler, AccessEnabler recupera el token de autenticación del servidor back-end e informa a la aplicación de que el flujo de autenticación ha finalizado. AccessEnabler llama a la llamada de retorno [`setAuthenticationStatus()`](#setAuthNStatus) con un código de estado de 1, que indica que se ha realizado correctamente. Si hay un error durante la ejecución de estos pasos, la llamada de retorno [`setAuthenticationStatus()`](#setAuthNStatus) se activa con un código de estado de 0, lo que indica un error de autenticación, así como un código de error correspondiente.


>[!WARNING]
>
> Durante los pasos en los que AccessEnabler cede el control a la aplicación (es decir, cuando se muestra el cuadro de diálogo de selección del proveedor o cuando se muestra UIWebView/WKWebView o SFSafariViewController), el usuario tiene la oportunidad de cancelar el flujo de autenticación. En estas situaciones, la aplicación es responsable de informar a AccessEnabler sobre este evento y de llamar al método de API [`setSelectedProvider()`](#setSelProv), pasando el valor nulo como parámetro. Esto ofrece al AccessEnabler la oportunidad de limpiar su estado interno y restablecer el flujo de autenticación. En tvOS, puede utilizar el mismo método para cancelar el sondeo de autenticación.


### Flujo de trabajo de cierre {#logout}

Para los clientes nativos, el cierre de sesión se gestiona de manera similar al proceso de autenticación descrito anteriormente.

1. La aplicación inicia el flujo de trabajo de cierre de sesión con una llamada al método de API `logout() `de AccessEnabler. El cierre de sesión es el resultado de una serie de operaciones de redirección HTTP debido al hecho de que el usuario debe cerrar la sesión tanto desde los servidores de autenticación de Adobe Pass como desde los servidores de MVPD. Dado que este flujo no se puede completar con una simple solicitud HTTP emitida por la biblioteca AccessEnabler, es necesario crear una instancia de un controlador `UIWebView/WKWebView or SFSafariViewController` para poder seguir las operaciones de redirección HTTP.

1. Se utiliza un patrón similar al flujo de autenticación. El AccessEnabler de iOS déclencheur la llamada de retorno `navigateToUrl:` o `navigateToUrl:useSVC:` para crear un controlador `UIWebView/WKWebView or SFSafariViewController` y cargar la URL proporcionada en el parámetro `url` de la llamada de retorno. Dirección URL del extremo de cierre de sesión en el servidor back-end. Para el AccessEnabler de tvOS, no se llama ni a la devolución de llamada `navigateToUrl:` ni a la devolución de llamada `navigateToUrl:useSVC:`.

1. A medida que pasa por varias redirecciones, la aplicación debe supervisar la actividad del controlador `UIWebView/WKWebView or SFSafariViewController `y detectar el momento en que carga una dirección URL personalizada específica. Tenga en cuenta que esta dirección URL personalizada específica no es válida y no está pensada para que el controlador la cargue. Su aplicación debe interpretarlo únicamente como una señal de que el flujo de cierre de sesión se ha completado y de que es seguro cerrar el controlador. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar el controlador y llamar al método de API `handleExternalURL:url `de AccessEnabler. Si la aplicación necesita usar un controlador `SFSafariViewController `la dirección URL personalizada específica está definida por `application's custom scheme` (p. ej.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); de lo contrario, esta dirección URL personalizada específica está definida por la constante ` ADOBEPASS_REDIRECT_URL  `es decir, `adobepass://ios.app`.

1. Al final, AccessEnabler llamará a la llamada de retorno [`setAuthenticationStatus()`](#setAuthNStatus) con un código de estado de 0, lo que indica que el flujo de cierre de sesión se ha realizado correctamente.

El flujo de cierre de sesión difiere del flujo de autenticación en que el usuario no tiene que interactuar con el controlador `UIWebView/WKWebView or SFSafariViewController` de ninguna manera. Por lo tanto, Adobe recomienda hacer que el control sea invisible (es decir, oculto) durante el proceso de cierre de sesión.

## Tokens {#tokens}

- [Definiciones y uso](#definitions)
- [Directrices de almacenamiento en caché](#caching)
- [Persistencia](#persistence)
- [Formato](#format)
- [Enlace de dispositivo](#device_binding)


### Definiciones y uso {#definitions}

La solución de asignación de derechos de autenticación de Adobe Pass gira en torno a la generación de datos específicos (tokens) que la autenticación de Adobe Pass genera al finalizar correctamente los flujos de trabajo de autenticación y autorización. Estos tokens se almacenan localmente en el dispositivo iOS del cliente.



Los tokens tienen una duración limitada; tras la caducidad, los tokens deben volver a emitirse reiniciando los flujos de trabajo de autenticación o autorización.



Existen tres tipos de tokens emitidos durante los flujos de trabajo de asignación de derechos:

- **Token de autenticación:** El resultado final del flujo de trabajo de autenticación del usuario será un GUID de autenticación que AccessEnabler puede utilizar para realizar consultas de autorización en nombre del usuario. Este GUID de autenticación tendrá asociado un valor de tiempo de vida (TTL) que puede diferir de la sesión de autenticación del usuario. Se generará un token de autenticación enlazando el GUID de autenticación al dispositivo que inicia las solicitudes de autenticación.
- **Token de autorización:** otorga acceso a un recurso protegido específico identificado por un identificador de recurso único. Consiste en una concesión de autorización emitida por la parte que autoriza junto con el ID de recurso original. Esta información está enlazada al dispositivo que inicia la solicitud.
- **Token multimedia de corta duración:** AccessEnabler concede acceso a la aplicación de alojamiento de un recurso determinado devolviendo un token multimedia de corta duración. Este token se genera en función del token de autorización adquirido anteriormente para ese recurso en particular. Además, este token no está enlazado al dispositivo y la duración asociada es considerablemente más corta (valor predeterminado: 5 minutos).

Una vez que la autenticación y la autorización se hayan realizado correctamente, la autenticación de Adobe Pass emitirá tokens de autenticación, autorización y medios de corta duración. Estos tokens deben almacenarse en caché en el dispositivo del usuario y utilizarse durante la duración de sus CICLOS de vida asociados.



### Directrices de almacenamiento en caché {#caching}

- Testigo de autenticación
- Token de autorización
- Token de medios de corta duración


#### Testigo de autenticación

- **AccessEnabler 1.7:** Este SDK presenta un nuevo método de almacenamiento de tokens, que permite varios contenedores de Programmer-MVPD y, por lo tanto, varios tokens de autenticación. Ahora se utiliza el mismo diseño de almacenamiento tanto para el escenario &quot;Autenticación por solicitante&quot; como para el flujo de autenticación normal. La única diferencia entre los dos es la forma en que se realiza la autenticación: &quot;Autenticación por solicitante&quot; contiene una nueva mejora (Autenticación pasiva) que permite que AccessEnabler realice la autenticación de canal de retorno, basada en la existencia de un token de autenticación en el almacenamiento (para un programador diferente). El usuario solo tiene que autenticarse una vez, y esta sesión se utilizará para obtener tokens de autenticación en aplicaciones adicionales. Este flujo de canal de retorno tiene lugar durante la llamada de [`setRequestor()`](#setReq) y es mayormente transparente para el programador. **Sin embargo, hay un requisito importante aquí: el programador DEBE llamar a setRequestor() desde el subproceso de la interfaz de usuario principal.**
- **AccessEnabler 1.6 y versiones anteriores:** La forma en que se almacenan en caché los tokens de autenticación en el dispositivo depende del indicador &quot;**Autenticación por solicitante&quot;** asociado con el MVPD actual:

<!-- end list -->

1. Si la función &quot;Autenticación por solicitante&quot; está deshabilitada, se almacenará un solo token de autenticación localmente en la mesa de trabajo global; este token se compartirá entre todas las aplicaciones integradas con la MVPD actual.
1. Si la función &quot;Autenticación por solicitante&quot; está habilitada, se asociará explícitamente un token con el programador que realizó el flujo de autenticación (el token no se almacenará en la mesa de trabajo global, sino en un archivo privado visible solo para la aplicación de ese programador). Más específicamente, se deshabilitará el inicio de sesión único (SSO) entre diferentes aplicaciones; el usuario deberá realizar el flujo de autenticación explícitamente al cambiar a una nueva aplicación (siempre que el programador de la segunda aplicación esté integrado con la MVPD actual y que no exista ningún token de autenticación para ese programador en la caché local).



#### Token de autorización

En cualquier momento dado, AccessEnabler almacena en caché solo UN token de autorización por recurso. Puede haber varios tokens de autorización en la caché, pero están asociados a diferentes recursos. Siempre que se emite un nuevo token de autorización y ya existe uno antiguo para *el mismo recurso*, el nuevo token sobrescribe el valor almacenado en caché existente.



#### Token de medios

El token de medios de corta duración NO debe almacenarse en caché. El token de medios debe recuperarse del servidor cada vez que se llama a una API de autorización, ya que está restringido a un solo uso.



### Persistencia {#persistence}

Los tokens deben ser persistentes en ejecuciones consecutivas de la misma aplicación. Esto significa que una vez que se han adquirido los tokens de autenticación y autorización y el usuario cierra la aplicación, los mismos tokens están disponibles para la aplicación cuando el usuario vuelve a abrir la aplicación. Además, es deseable que estos tokens sean persistentes en varias aplicaciones. En otras palabras, después de que un usuario utilice una aplicación para iniciar sesión con un proveedor de identidad específico (obteniendo correctamente tokens de autenticación y autorización), los mismos tokens se pueden utilizar a través de una aplicación diferente y ya no se le piden credenciales al usuario al iniciar sesión a través del mismo proveedor de identidad. Este tipo de flujo de trabajo de autenticación/autorización sin problemas es lo que hace que la solución de autenticación de Adobe Pass sea una verdadera TV en todas partes
implementación.



## iOS

La biblioteca iOS AccessEnabler soluciona los problemas que plantea el uso compartido de datos entre aplicaciones almacenando los datos de tokens en una estructura de datos &quot;similar a un portapapeles&quot; denominada *tablero de pegado*. Este recurso compartido de nivel de sistema proporciona los ingredientes clave que permiten la implementación del caso de uso de tokens persistentes deseado:

- **Compatibilidad con almacenamiento estructurado**: la placa de pegado no es sólo una estructura de memoria lineal simple similar a un búfer. Proporciona un mecanismo de almacenamiento similar a un diccionario que permite la indexación de datos en función de los valores de clave especificados por el usuario.
- **Compatibilidad con la persistencia de datos mediante el sistema de archivos subyacente**. El contenido de la estructura del panel de pegado se puede marcar como persistente, en cuyo caso los datos se guardan en la memoria interna del dispositivo.



Una vez que un token particular se coloca en la caché de tokens, la biblioteca AccessEnabler comprueba su validez en diferentes momentos.  Un token válido se define como:

- El TTL del token no ha caducado.
- El emisor del token se incluye en la lista de proveedores de identidad permitidos.



## tvOS

Como en tvOS la mesa de trabajo no está disponible, la biblioteca de tvOS AccessEnabler utiliza NsUserDefaults como opción de almacenamiento. Esto soluciona el problema de la autenticación persistente y los tokens de autorización, pero la información almacenada no se puede compartir entre distintas aplicaciones.



**Cambios en la mesa de trabajo de iOS 10 -** Al actualizar desde una versión anterior de iOS, se borrará la mesa de trabajo. Esto significa que todas las aplicaciones deben volver a autenticarse.



**Cambios en la mesa de trabajo de iOS 7 -** Debido a los cambios en el funcionamiento de las mesas de trabajo en iOS 7, habrá un SSO cruzado limitado entre las aplicaciones que se ejecuten en iOS 7. Las aplicaciones que tengan el mismo `<Bundle Seed ID>` (también conocido como `<Team ID>`) compartirán tokens, lo que significa que las aplicaciones A1 y A2 del mismo Programmer X compartirán tokens, mientras que la aplicación A1 (Programmer X) y la aplicación A3 (Programmer Y) no compartirán tokens.

- El ID de raíz del paquete/ID del equipo es el mismo entre dos aplicaciones si las genera el mismo perfil de aprovisionamiento. Encontrará más información en este enlace:
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Esta limitación de &quot;SSO cruzado&quot; estará presente en iOS 7 independientemente de la autenticación de Adobe Pass que se use en SDK.

Lea esta nota técnica para obtener más información sobre la configuración de SSO en iOS 7 y versiones posteriores (la nota técnica se aplica a Access Enabler v1.8 y versiones posteriores): <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Almacenamiento de tokens (AccessEnabler 1.7)

A partir de AccessEnabler 1.7, el almacenamiento de tokens puede admitir varias combinaciones de programador-MVPD, basándose en una estructura de mapas anidada de varios niveles que puede contener varios tokens de autenticación. Este nuevo almacenamiento no afecta a la API pública de AccessEnabler de ninguna manera y no requiere cambios por parte del programador. Este es un ejemplo de lo siguiente
ilustra esta nueva funcionalidad:

1. Abra App1 (desarrollado por Programmer1).
1. Autenticar con MVPD1 (que está integrado con Programmer1).
1. Suspenda/cierre la aplicación actual y abra App2 (desarrollada por Programmer2).
1. Supongamos que Programmer2 no está integrado con MVPD2; por lo tanto, el usuario NO se autenticará en App2.
1. Autentique con MVPD2 (que está integrado con Programmer2) en App2.
1. Cambie a App1; el usuario se autenticará con Programmer1.

En versiones anteriores de AccessEnabler, el paso 6 hacía que el usuario no estuviera autenticado, ya que el almacenamiento de tokens anteriormente solo admitía un token de autenticación.



Si se cierra la sesión de un programador/MVPD, se borrará todo el almacenamiento subyacente, incluidos todos los demás tokens de autenticación de programador/MVPD del dispositivo. Por otro lado, cancelar el flujo de autenticación (invocando [`setSelectedProvider(null)`](#setSelProv)) NO borrará el almacenamiento subyacente, pero solo afectará el intento de autenticación actual del Programador/MVPD (borrando MVPD para el Programador actual).



### Importador de tokens (AccessEnabler 1.7)

Otra función relacionada con el almacenamiento que se incluye en AccessEnabler 1.7 permite importar tokens de autenticación de áreas de almacenamiento antiguas. Este &quot;Importador de tokens&quot; ayuda a lograr la compatibilidad entre versiones consecutivas de AccessEnabler, manteniendo el estado de SSO incluso cuando se actualiza la versión de almacenamiento. El importador se ejecuta durante el flujo [`setRequestor()`](#setReq) y se ejecuta en los dos casos siguientes (suponiendo que no hay ningún token de autenticación válido para el programador actual en el almacenamiento actual):

- La primera instalación de una aplicación 1.7 desarrollada por un programador específico
- Ruta de actualización a un futuro AccessEnabler que utilice un nuevo almacenamiento

La operación de importación es transparente para el programador y no requiere ningún cambio de código en la aplicación cliente.



### Eliminador de tokens (AccessEnabler 1.7.5)

A partir de AccessEnabler 1.7.5, este servicio se ejecuta en [`setRequestor()`](#setReq)`. `Se desarrolló como resultado del cambio de iOS 7 de la dirección de WiFi MAC al IDFA para seguimiento. El desinfectante garantiza que el almacenamiento actual contenga únicamente tokens de autenticación válidos (válidos en términos del ID de dispositivo, anteriormente calculados mediante la dirección MAC, antes de iOS7). El eliminador de tokens elimina todos los tokens no válidos.



El servicio Token Sanitizer es más útil cuando se utiliza una aplicación AccessEnabler 1.7.5 en iOS 6 y el usuario actualiza a iOS 7. Cuando esto sucede, todos los tokens de autenticación obtenidos en iOS 6 ahora no son válidos (debido al cambio del algoritmo de ID de dispositivo de usar la dirección de MAC a IDFA). El desinfectador borrará todos los tokens no válidos y el usuario no se autenticará.



Sin el eliminador de tokens que elimine los tokens no válidos, AccessEnabler no obtendría autorizaciones debido a la presencia de tokens de autenticación no válidos. Esto resultaría difícil de depurar para el usuario final, ya que los mensajes de error de autorización pueden ser crípticos y no demasiado útiles para determinar la causa del problema.



### Formato {#format}

- [Token de AuthN](#authn_token)
- [Token de AuthZ](#authz_token)
- [Token de medios corto](#short_token)
- [Enlace de dispositivo](#device_binding)


Tenga en cuenta que el formato de los tokens AuthN y AuthZ se incluye aquí solo para obtener información general. La estructura de estos tokens se puede cambiar mediante la autenticación de Adobe Pass en cualquier momento. Los programadores no necesitan conocer la estructura exacta de los tokens AuthN y AuthZ para implementar sus aplicaciones, ya que los tokens AuthN y AuthZ no se exponen en el dispositivo local. El token de medios cortos *is* expuesto a la aplicación del programador.



#### Testigo de autenticación {#authn_token}

La siguiente lista presenta el formato del token de autenticación:

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthenticationToken>
      <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
      <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>   
  </simpleAuthenticationToken>
```


#### Token de autorización {#authz_token}

La siguiente lista presenta el formato del token de autorización:

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthorizationToken>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
      <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>
  </simpleAuthorizationToken>
```


#### Token de medios corto {#short_token}

La siguiente lista presenta el formato del token de medios corto. Este token se expone a la aplicación del programador. Se pasa a la aplicación del programador al final de un proceso de asignación de derechos correcto:

```
  <signatureInfo>signature<signatureInfo>
  <shortAuthorizationToken>
    <sessionGUID>session_guid</sessionGUID>
    <requestorID>requestor_id</requestorID>
    <resourceID>resource_id</resourceID>
    <ttl>ttl_in_ms</ttl>
    <issueTime>issue_time</issueTime>
    <mvpdId>mvpd_id</mvpdId>
    <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
  </shortAuthorizationToken>
```


### Enlace de dispositivo {#device_binding}

En los listados XML anteriores, observe la etiqueta `simpleTokenFingerprint`. El propósito de esta etiqueta es contener información de individualización de ID de dispositivo nativo. La biblioteca AccessEnabler puede obtener dicha información de individualización y ponerla a disposición de los servicios de autenticación de Adobe Pass durante las llamadas de asignación de derechos. El servicio utilizará esta información e la incrustará en los tokens reales, enlazando así de forma eficaz los tokens a un dispositivo específico. El objetivo final de esto es hacer que los tokens sean intransferibles entre dispositivos.



Dado que esta es, obviamente, una función relacionada con la seguridad, esta información es inherentemente &quot;confidencial&quot; desde el punto de vista de la seguridad. Como resultado, esta información debe protegerse contra la manipulación y la escucha. El problema de la escucha se resuelve enviando las solicitudes de autenticación/autorización a través del protocolo HTTPS. La protección contra la manipulación se controla mediante la firma digital de la información de identificación del dispositivo. La biblioteca AccessEnabler calcula un ID de dispositivo a partir de la información proporcionada por el dispositivo y, a continuación, envía el ID de dispositivo &quot;sin cifrar&quot; a los servidores de autenticación de Adobe Pass como parámetro de solicitud. Los servidores de autenticación de Adobe Pass firman digitalmente el ID del dispositivo con la clave privada de Adobe y lo agregan al token de autenticación que se devuelve al AccessEnabler. Por lo tanto, el ID del dispositivo está enlazado con el token de autenticación. Durante el flujo de autorización, AccessEnabler vuelve a enviar el ID del dispositivo de forma clara, junto con el token de autenticación. Un error en el proceso de validación provocará automáticamente un error en los flujos de trabajo de autenticación/autorización. Los servidores de autenticación de Adobe Pass aplican la clave privada al ID de dispositivo y la comparan con el valor del token de autenticación. Si no coinciden, se produce un error en el flujo de derechos.



**Nota sobre el enlace de dispositivos en AccessEnabler 1.7.5:** A partir de AccessEnabler 1.7.5, hay un cambio en la forma en que se calcula el ID del dispositivo para proporcionar información de individualización para un dispositivo iOS. Este cambio refleja un cambio en iOS 7: a partir de iOS 7, Apple ya no proporciona la dirección de WiFi MAC como opción de seguimiento, a favor de su identificador para anunciantes (IDFA). Dado que la información de individualización de una aplicación que se ejecuta en iOS 7 se basa en el IDFA y que esa información está incrustada en los tokens de flujo de derechos, esto significa que hay una serie de posibles efectos diferentes en la experiencia del usuario que resultan de este cambio. Los diferentes efectos posibles se basan en la versión de iOS desde la que el usuario está actualizando, junto con la versión del AccessEnabler desde la que el programador está actualizando. Para obtener más información sobre este cambio, consulte las Notas de la versión incluidas con AccessEnabler SDK 1.7.5.

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
