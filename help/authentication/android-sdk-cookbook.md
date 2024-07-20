---
title: Guía del SDK para Android
description: Guía del SDK para Android
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 0%

---

# Guía del SDK para Android {#android-sdk-cookbook}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

## Introducción {#intro}

En este documento se describen los flujos de trabajo de asignación de derechos que la aplicación de nivel superior de un programador puede implementar a través de las API expuestas por la biblioteca Android AccessEnabler.


La solución de asignación de derechos de autenticación de Adobe Pass para Android se divide finalmente en dos dominios:

- Dominio de interfaz de usuario: es la capa de aplicación de nivel superior que implementa la interfaz de usuario y utiliza los servicios proporcionados por la biblioteca AccessEnabler para proporcionar acceso al contenido restringido.
- Dominio de AccessEnabler: aquí es donde se implementan los flujos de trabajo de asignación de derechos en forma de:
   - Llamadas de red realizadas a los servidores back-end de Adobe
   - Reglas de lógica empresarial relacionadas con los flujos de trabajo de autenticación y autorización
   - Administración de varios recursos y procesamiento del estado del flujo de trabajo (como la caché de símbolos)

El objetivo del dominio AccessEnabler es ocultar todas las complejidades de los flujos de trabajo de asignación de derechos y proporcionar a la aplicación de capa superior (a través de la biblioteca AccessEnabler) un conjunto de primitivas de asignación de derechos simples con las que implementar los flujos de trabajo de asignación de derechos:

1. Establezca la identidad del solicitante.

1. Comprobación y obtención de autenticación con un proveedor de identidad concreto.

1. Compruebe y obtenga autorización para un recurso en particular.

1. Cerrar sesión.

La actividad de red de AccessEnabler tiene lugar en un subproceso diferente, por lo que el subproceso de la interfaz de usuario nunca se bloquea. Como resultado, el canal de comunicación bidireccional entre los dos dominios de aplicación debe seguir un patrón totalmente asincrónico:

- La capa de aplicación de la interfaz de usuario envía mensajes al dominio AccessEnabler a través de las llamadas de API expuestas por la biblioteca AccessEnabler.
- AccessEnabler responde a la capa de interfaz de usuario mediante los métodos de devolución de llamada incluidos en el protocolo AccessEnabler que la capa de interfaz de usuario registra con la biblioteca AccessEnabler.

## Flujos de derecho {#entitlement}

1. [Requisitos previos](#prereqs)
1. [Flujo de inicio](#startup_flow)
1. [Flujo de autenticación](#authn_flow)
1. [Flujo de autorización](#authz_flow)
1. [Ver flujo de medios](#media_flow)
1. [Flujo de cierre de sesión](#logout_flow)

### A. Requisitos previos {#prereqs}

1. Cree sus funciones de devolución de llamada:
   - [`setRequestorComplete()`](#$setRequestorComplete)

     Activado por `setRequestor()`, devuelve éxito o error.\
     El éxito indica que se puede continuar con las llamadas de asignación de derechos.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

     Activado por `getAuthentication()` solo si el usuario no ha seleccionado ningún proveedor (MVPD) y aún no se ha autenticado.\
     El parámetro `mvpds` es una matriz de proveedores disponibles para el usuario.

   - [&quot;setAuthenticationStatus(status, errorcode)&quot;](#$setAuthNStatus)

     Activado por `checkAuthentication()` cada vez.\
     Activado por `getAuthentication()` solo si el usuario ya se ha autenticado y ha seleccionado un proveedor.

     El estado devuelto es éxito o error, el código de error describe el tipo de error.

   - [navigationToUrl(url)](#$navigateToUrl)

     Activado por `getAuthentication()` después de que el usuario seleccione una MVPD. El parámetro `url` proporciona la ubicación de la página de inicio de sesión de la MVPD.

   - [sendTrackingData(event, data)](#$sendTrackingData)

     Activado por `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.\
     El parámetro `event` indica qué evento de asignación de derechos se produjo; el parámetro `data` es una lista de valores relacionados con el evento.

   - [setToken(token, recurso)](#$setToken)

     Activado por `checkAuthorization()` y `getAuthorization()` tras una autorización correcta para ver un recurso.\
     El parámetro `token` es el token de medios de corta duración; el parámetro `resource` es el contenido que el usuario tiene autorización para ver.

   - [&quot;tokenRequestFailed(resource, code, description)&quot;](#$tokenRequestFailed)

     Activado por `checkAuthorization()` y `getAuthorization()` tras una autorización incorrecta.\
     El parámetro `resource` es el contenido que el usuario intentaba ver; el parámetro `code` es el código de error que indica qué tipo de error se produjo; el parámetro `description` describe el error asociado con el código de error.

   - [selectedProvider(mvpd)](#$selectedProvider)

     Activado por `getSelectedProvider()`.\
     El parámetro `mvpd` proporciona información sobre el proveedor seleccionado por el usuario.

   - [&quot;setMetadataStatus(metadata, key, arguments)&quot;](#$setMetadataStatus)

     Activado por `getMetadata().`\
     El parámetro `metadata` proporciona los datos específicos solicitados; el parámetro `key` es la clave utilizada en la solicitud `getMetadata()`; y el parámetro `arguments` es el mismo diccionario que se pasó a `getMetadata()`.

   - [preauthorizedResources(resources)](#$preauthResources)

     Activado por `checkPreauthorizedResources()`.\
     El parámetro `authorizedResources` presenta los recursos que el usuario tiene autorización para ver.


![](assets/android-entitlement-flows.png)


### B. Flujo de inicio {#startup_flow}

1. Inicie la aplicación de nivel superior.
1. Iniciar autenticación de Adobe Pass

   a. Llame a [`getInstance`](#$getInstance) para crear una sola instancia del activador de acceso de autenticación de Adobe Pass.

   - **Dependencia:** autenticación de Adobe Pass nativa
Biblioteca de Android (AccessEnabler)

   b. Llame a ` setRequestor()` para establecer la identificación del programador; pase `requestorID` del programador y (opcionalmente) una matriz de extremos de autenticación de Adobe Pass.

   - **Dependencia:** ID de solicitante de autenticación de Adobe Pass válido\
     (Póngase en contacto con el administrador de cuentas de autenticación de Adobe Pass para arreglarlo).

   - **Déclencheur:** setRequestorComplete() callback

   | NOTA |     |
   | --- | --- |  
   | ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) | No se pueden completar solicitudes de asignación de derechos hasta que se haya establecido completamente la identidad del solicitante. Esto significa que mientras setRequestor() sigue ejecutándose, todas las solicitudes de derechos subsiguientes (por ejemplo, `checkAuthentication()`) se bloquean.<br><br>Tiene dos opciones de implementación: una vez que la información de identificación del solicitante se envía al servidor back-end, la capa de aplicación de la interfaz de usuario puede elegir uno de los dos enfoques siguientes:<br><br>1.  Espere a que se active la llamada de retorno `setRequestorComplete()` (parte del delegado AccessEnabler).  Esta opción proporciona la mayor certeza de que `setRequestor()` se completó, por lo que se recomienda para la mayoría de las implementaciones.<br>2.  Continúe sin esperar a que se active la llamada de retorno `setRequestorComplete()` y comience a emitir solicitudes de asignación de derechos. Estas llamadas (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) están en cola por la biblioteca AccessEnabler, que realizará las llamadas de red reales después de `setRequestor(). `Esta opción puede interrumpirse ocasionalmente si, por ejemplo, la conexión de red es inestable. |

1. Llame a [checkAuthentication()](#$checkAuthN) para comprobar si hay una autenticación existente sin iniciar el flujo de autenticación completo.   Si esta llamada se realiza correctamente, puede continuar directamente al flujo de autorización.  Si no es así, continúe con el flujo de autenticación.

   - **Dependencia:** Una llamada correcta a `setRequestor()` (esta dependencia también se aplica a todas las llamadas subsiguientes).

   - **Déclencheur:** setAuthenticationStatus() callback

### C. Flujo de autenticación {#authn_flow}

1. Llame a [`getAuthentication()`](#$getAuthN) para iniciar el flujo de autenticación o para obtener confirmación de que el usuario ya se ha autenticado.\
   **Déclencheur:**
   - La llamada de retorno setAuthenticationStatus(), si el usuario ya está autenticado.  En este caso, continúe directamente al [Flujo de autorización](#authz_flow).
   - La llamada de retorno displayProviderDialog(), si el usuario aún no se ha autenticado.

1. Presente al usuario la lista de proveedores enviados a `displayProviderDialog()`.

1. Una vez que el usuario seleccione un proveedor, obtenga la URL de la MVPD del usuario de la llamada de retorno `navigateToUrl()`.  Abra un control WebView y dirija ese control WebView a la dirección URL.

1. A través del WebView creado en el paso anterior, el usuario aterriza en la página de inicio de sesión de la MVPD e introduce credenciales de inicio de sesión. Varias operaciones de redirección tienen lugar dentro de WebView.

   **Nota:** En este momento, el usuario tiene la oportunidad de cancelar el flujo de autenticación. Si esto sucede, la capa de la interfaz de usuario es responsable de informar a AccessEnabler sobre este evento llamando a `setSelectedProvider()` con `null` como parámetro. Esto permite al AccessEnabler limpiar su estado interno y restablecer el flujo de autenticación.

1. Una vez que el usuario ha iniciado sesión correctamente, la capa de aplicación detecta la carga de una &quot;URL de redireccionamiento personalizada&quot; (por ejemplo: `http://adobepass.android.app`). Esta dirección URL personalizada es en realidad una dirección URL no válida que no está destinada a que WebView se cargue. Es una señal de que el flujo de autenticación se ha completado y de que WebView debe cerrarse.

1. Cierre el control WebView y llame a `getAuthenticationToken()`, que indica a AccessEnabler que recupere el token de autenticación del servidor back-end.

1. [Opcional] Llame a [`checkPreauthorizedResources(resources)`](#$checkPreauth) para comprobar qué recursos puede ver el usuario. El parámetro `resources` es una matriz de recursos protegidos asociados al token de autenticación del usuario.\
   **Déclencheur:** `preAuthorizedResources()` devolución de llamada\
   **Punto de ejecución:** después del flujo de autenticación completado

1. Si la autenticación se ha realizado correctamente, continúe con el Flujo de autorización.



### D. Flujo de autorización {#authz_flow}

1. Llame a [getAuthorization()](#$getAuthZ) para iniciar la autorización
Flujo.

   Dependencia: ID de recurso válidos acordados con los MVPD.

   **Nota:** Los ResourceID deben ser los mismos que se usan en cualquier otro dispositivo o plataforma, y serán los mismos en todas las MVPD.

1. Valide la autenticación y la autorización.

   - Si la llamada `getAuthorization()` se realiza correctamente: el usuario tiene tokens AuthN y AuthZ válidos (el usuario está autenticado y autorizado para ver los medios solicitados).
   - Si `getAuthorization()` produce un error: examine la excepción generada para determinar su tipo (AuthN, AuthZ o algo más):
      - Si se ha producido un error de autenticación (AuthN), reinicie el flujo de autenticación.
      - Si se trata de un error de autorización (AuthZ), el usuario no tiene autorización para ver los medios solicitados y se le debe mostrar algún tipo de mensaje de error.
      - Si hubo algún otro tipo de error (error de conexión, error de red, etc.) a continuación, mostrar un mensaje de error apropiado al usuario.

1. Valide el token de medios corto.\
   Utilice la biblioteca del Comprobador de tokens de medios de autenticación de Adobe Pass para comprobar el token de medios de corta duración devuelto desde la llamada de `getAuthorization()` anterior:

   - Si la validación se realiza correctamente: Reproduzca el medio solicitado para el usuario.
   - Si la validación falla: El token de AuthZ no era válido, la solicitud de medios debe rechazarse y se debe mostrar un mensaje de error al usuario.

1. Vuelva al flujo normal de aplicaciones.

### E. Ver flujo de medios {#media_flow}

1. El usuario selecciona los medios que desea ver.
2. ¿Están protegidos los medios?  La aplicación comprueba si los medios seleccionados están protegidos:
- Si el medio seleccionado está protegido, su aplicación inicia el [Flujo de autorización](#authz_flow) anterior.
- Si el medio seleccionado no está protegido, reproduzca el medio para el usuario.



### F. Flujo de cierre de sesión {#logout_flow}

1. Llame a [`logout()`](#$logout) para cerrar la sesión del usuario.\
   AccessEnabler borra todos los valores en caché y tokens de la MVPD actual para el solicitante actual y también para los solicitantes con inicio de sesión único. Después de borrar la caché, AccessEnabler realiza una llamada al servidor para limpiar las sesiones del lado del servidor.  Tenga en cuenta que, dado que la llamada al servidor podría provocar un redireccionamiento de SAML al IdP (esto permite la limpieza de la sesión en el lado del IdP), esta llamada debe seguir todas las redirecciones. Por este motivo, esta llamada debe controlarse dentro de un control WebView.

   a. Siguiendo el mismo patrón que el flujo de trabajo de autenticación, el dominio AccessEnabler realiza una solicitud a la capa de aplicación de la interfaz de usuario (a través de la llamada de retorno `navigateToUrl()`) para crear un control WebView e indicar a ese control que cargue la dirección URL del extremo de cierre de sesión en el servidor back-end.

   b. De nuevo, la interfaz de usuario debe supervisar la actividad del control WebView y detectar el momento en que el control, a medida que pasa por varias redirecciones, carga la dirección URL personalizada de la aplicación (es decir: `http://adobepass.android.app/`). Una vez que se produce este evento, la capa de aplicación de la interfaz de usuario cierra WebView y se completa el proceso de cierre de sesión.

   **Nota:** El flujo de cierre de sesión difiere del flujo de autenticación en que el usuario no tiene que interactuar con WebView de ninguna manera. La capa de aplicación de la interfaz de usuario utiliza un WebView para asegurarse de que se siguen todas las redirecciones. Por lo tanto, es posible (y recomendado) hacer que el control WebView sea invisible (es decir, oculto) durante el proceso de cierre de sesión.



### Flujos de usuario para iniciar sesión con varias MVPD y cerrar la sesión {#user_flows}

[Aquí](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf) tiene un documento que describe el comportamiento al usar varias MVPD y lo que sucede cuando el usuario cierra la sesión de una aplicación.

El comportamiento descrito está disponible al utilizar la versión >= 2.0.0 del SDK para Android.
