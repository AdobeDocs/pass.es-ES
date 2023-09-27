---
title: Guía de integración de Amazon FireOS
description: Guía de integración de Amazon FireOS
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# Guía de integración de Amazon FireOS {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>


## Introducción {#intro}

En este documento se describen los flujos de trabajo de asignación de derechos que la aplicación de nivel superior de un programador puede implementar mediante las API expuestas por Amazon FireOS `AccessEnabler` biblioteca.

La solución de asignación de derechos de autenticación de Adobe Pass para Amazon FireOS se divide finalmente en dos dominios:

- El dominio de la interfaz de usuario: es la capa de aplicación de nivel superior que implementa la interfaz de usuario y utiliza los servicios proporcionados por el `AccessEnabler` para proporcionar acceso al contenido restringido.
- El `AccessEnabler` dominio: aquí es donde se implementan los flujos de trabajo de derechos en forma de:
   - Llamadas de red realizadas a los servidores back-end de Adobe
   - Reglas de lógica empresarial relacionadas con los flujos de trabajo de autenticación y autorización
   - Administración de varios recursos y procesamiento del estado del flujo de trabajo (como la caché de símbolos)

El objetivo del `AccessEnabler` Este dominio es para ocultar todas las complejidades de los flujos de trabajo de asignación de derechos y para proporcionar a la aplicación de capa superior (a través de `AccessEnabler` library) un conjunto de primitivas de derechos simples. Este proceso permite implementar los flujos de trabajo de derechos:

1. Establezca la identidad del solicitante.
1. Comprobación y obtención de autenticación con un proveedor de identidad concreto.
1. Compruebe y obtenga autorización para un recurso en particular.
1. Cerrar sesión.

El `AccessEnabler`La actividad de red de se lleva a cabo en un subproceso diferente, por lo que el subproceso de interfaz de usuario nunca se bloquea. Como resultado, el canal de comunicación bidireccional entre los dos dominios de aplicación debe seguir un patrón totalmente asincrónico:

- La capa de aplicación de la interfaz de usuario envía mensajes a `AccessEnabler` dominio a través de las llamadas API expuestas por el `AccessEnabler` biblioteca.
- El `AccessEnabler` responde a la capa de interfaz de usuario mediante los métodos de llamada de retorno incluidos en la variable `AccessEnabler` protocolo que registra la capa de interfaz de usuario en `AccessEnabler` biblioteca.

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

      - Activado por `setRequestor()`, devuelve éxito o error.     El éxito indica que se puede continuar con las llamadas de asignación de derechos.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Activado por `getAuthentication()` solo si el usuario no ha seleccionado un proveedor (MVPD) y aún no está autenticado. El `mvpds` El parámetro es una matriz de proveedores disponibles para el usuario.

   - [&quot;setAuthenticationStatus(status, reason)&quot;](#$setAuthNStatus)

      - Activado por `checkAuthentication()` cada vez. Activado por `getAuthentication()` solo si el usuario ya se ha autenticado y ha seleccionado un proveedor.

      - El estado devuelto está autenticado o no autenticado, el motivo describe un error de autenticación o una acción de cierre de sesión.

   - [navigationToUrl(url)](#$navigateToUrl)

      - Se ignora en el SDK de AmazonFireOS y se utiliza el método en plataformas Android donde se activa mediante `getAuthentication()` después de que el usuario seleccione una MVPD.  El `url` proporciona la ubicación de la página de inicio de sesión de la MVPD.

   - [sendTrackingData(event, data)](#$sendTrackingData)

      - Activado por `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
El `event` parámetro indica qué evento de derecho se produjo;el parámetro `data` parámetro es una lista de valores relacionados con el evento.

   - [setToken(token, recurso)](#$setToken)

      - Activado por `checkAuthorization()` y `getAuthorization()` después de una autorización correcta para ver un recurso.
      - El `token` parámetro es el token de medios de corta duración; el `resource` parámetro es el contenido que el usuario tiene autorización para ver.

   - [&quot;tokenRequestFailed(resource, code, description)&quot;](#$tokenRequestFailed)

      - Activado por `checkAuthorization()` y `getAuthorization()` después de una autorización fallida.
      - El `resource` parámetro es el contenido que el usuario intentaba ver; la variable `code` parámetro es el código de error que indica qué tipo de error se produjo; la variable `description` describe el error asociado con el código de error.

   - [selectedProvider(mvpd)](#$selectedProvider)

      - Activado por `getSelectedProvider()`.
      - El `mvpd` proporciona información sobre el proveedor seleccionado por el usuario.

   - [&quot;setMetadataStatus(metadata, key, arguments)&quot;](#$setMetadataStatus)

      - Activado por `getMetadata().`
      - El `metadata` proporciona los datos específicos solicitados; el parámetro `key` parámetro es la clave utilizada en el `getMetadata()` solicitud; y la `arguments` El parámetro es el mismo diccionario que se pasó a `getMetadata()`.

   - [preauthorizedResources(resources)](#$preauthResources)

      - Activado por `checkPreauthorizedResources()`.
      - El `authorizedResources` presenta los recursos que el usuario está autorizado a ver.


![](assets/android-entitlement-flows.png)


### B. Flujo de inicio {#startup_flow}

1. Inicie la aplicación de nivel superior.
1. Inicie la autenticación de Adobe Pass.

   1. Llamada [`getInstance`](#$getInstance) para crear una única instancia de autenticación de Adobe Pass `AccessEnabler`.

      - **Dependencia:** Biblioteca Adobe Pass Authentication Native Amazon FireOS (`AccessEnabler`)

   1. Llamada` setRequestor()` para establecer la identificación del Programador; pase en el `requestorID` y (opcionalmente) una matriz de puntos finales de autenticación de Adobe Pass.

      - **Dependencia:** ID de solicitante de autenticación de Adobe Pass válido (póngase en contacto con el administrador de cuentas de autenticación de Adobe Pass para arreglarlo).

      - **Déclencheur:** llamada de retorno setRequestorComplete()

   No se pueden completar solicitudes de asignación de derechos hasta que se haya establecido completamente la identidad del solicitante. Esto significa que mientras setRequestor() sigue ejecutándose, todas las solicitudes de derechos subsiguientes (por ejemplo,`checkAuthentication()`) están bloqueados.

   Tiene dos opciones de implementación: Una vez que la información de identificación del solicitante se envía al servidor back-end, la capa de aplicación de la interfaz de usuario puede elegir uno de los dos enfoques siguientes:</p>

   1. Espere a que se active la `setRequestorComplete()` callback (parte de `AccessEnabler` delegate).  Esta opción proporciona la mayor certeza de que `setRequestor()` completado, por lo que se recomienda para la mayoría de las implementaciones.
   1. Continúe sin esperar a que se active el `setRequestorComplete()` llamada de retorno y empiece a emitir solicitudes de derechos. El pone en cola estas llamadas (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) `AccessEnabler` , que realizará las llamadas de red reales después de que `setRequestor()`. Esta opción se puede interrumpir ocasionalmente si, por ejemplo, la conexión de red es inestable.

1. Llamada [checkAuthentication()](#$checkAuthN) para comprobar si hay una autenticación existente sin iniciar el flujo de autenticación completo.  Si esta llamada se realiza correctamente, puede continuar directamente al flujo de autorización.  Si no es así, continúe con el flujo de autenticación.

- **Dependencia:** Una llamada a correcta a `setRequestor()` (esta dependencia también se aplica a todas las llamadas subsiguientes).

- **Déclencheur:** llamada de retorno setAuthenticationStatus()

### C. Flujo de autenticación {#authn_flow}

1. Llamada [`getAuthentication()`](#$getAuthN) para iniciar el flujo de autenticación o para obtener confirmación de que el usuario ya está autenticado.

   **Déclencheur:**

   - La llamada de retorno setAuthenticationStatus(), si el usuario ya está autenticado.  En este caso, vaya directamente a [Flujo de autorización](#authz_flow).
   - La llamada de retorno displayProviderDialog(), si el usuario aún no se ha autenticado.

1. Presentar al usuario la lista de proveedores enviados a `displayProviderDialog()`.
1. Una vez que el usuario selecciona un proveedor, se abre una página WebView para que el usuario inicie sesión

   >[!NOTE]
   >
   >En este punto, el usuario tiene la oportunidad de cancelar el flujo de autenticación. Si esto sucede, el `AccessEnabler` limpiará su estado interno y restablecerá el flujo de autenticación.

1. Cuando el usuario inicie sesión correctamente, WebView se cerrará.
1. llamada `getAuthenticationToken(),` que ordena al `AccessEnabler` para recuperar el token de autenticación del servidor back-end.
1. [Opcional] Llamada [`checkPreauthorizedResources(resources)`](#$checkPreauth) para comprobar qué recursos puede ver el usuario. El `resources` El parámetro es una matriz de recursos protegidos asociados al token de autenticación del usuario.

   **Déclencheur:** `preAuthorizedResources()` callback\
   **Punto de ejecución:** Después del flujo de autenticación completado

1. Si la autenticación se ha realizado correctamente, continúe con el Flujo de autorización.


### D. Flujo de autorización {#authz_flow}

1. Llamada [`getAuthorization()`](#$getAuthZ) para iniciar el flujo de autorización.

   Dependencia: ID de recurso válidos acordados con los MVPD.

   **Nota:** Los ID de recurso deben ser los mismos que se usan en cualquier otro dispositivo o plataforma, y serán los mismos en todas las MVPD.

1. Valide la autenticación y la autorización.

   - Si la variable `getAuthorization()` La llamada se ha realizado correctamente: el usuario tiene tokens de AuthN y AuthZ válidos (el usuario está autenticado y autorizado para ver los medios solicitados).
   - If `getAuthorization()` failed: Examine la excepción producida para determinar su tipo (AuthN, AuthZ o algo más):
      - Si se ha producido un error de autenticación (AuthN), reinicie el flujo de autenticación.
      - Si se trata de un error de autorización (AuthZ), el usuario no tiene autorización para ver los medios solicitados y se le debe mostrar algún tipo de mensaje de error.
      - Si hubo algún otro tipo de error (error de conexión, error de red, etc.) a continuación, mostrar un mensaje de error apropiado al usuario.

1. Valide el token de medios corto.

   Utilice la biblioteca del Comprobador de tokens de medios de autenticación de Adobe Pass para comprobar el token de medios de corta duración devuelto desde el `getAuthorization()` llamada anterior:

   - Si la validación se realiza correctamente: Reproduzca el medio solicitado para el usuario.
   - Si la validación falla: El token de AuthZ no era válido, la solicitud de medios debe rechazarse y se debe mostrar un mensaje de error al usuario.

1. Vuelva al flujo normal de aplicaciones.

### E. Ver flujo de medios {#media_flow}

1. El usuario selecciona los medios que desea ver.
1. ¿Están protegidos los medios?  La aplicación comprueba si los medios seleccionados están protegidos:
   - Si los medios seleccionados están protegidos, la aplicación inicia el [Flujo de autorización](#authz_flow) arriba.
   - Si el medio seleccionado no está protegido, reproduzca el medio para el usuario.

### F. Flujo de cierre de sesión {#logout_flow}

1. Llamada [`logout()`](#$logout) para cerrar la sesión del usuario. El `AccessEnabler` borra todos los valores en caché y tokens obtenidos por el usuario para la MVPD actual en todos los solicitantes que comparten el inicio de sesión mediante el inicio de sesión único. Después de borrar la caché, el `AccessEnabler` realiza una llamada al servidor para limpiar las sesiones del lado del servidor.  Tenga en cuenta que, dado que la llamada al servidor podría provocar un redireccionamiento de SAML al IdP (esto permite la limpieza de la sesión en el lado del IdP), esta llamada debe seguir todas las redirecciones. Por este motivo, esta llamada se controlará dentro de un control WebView, invisible para el usuario.

   **Nota:** El flujo de cierre de sesión difiere del flujo de autenticación en que el usuario no tiene que interactuar con WebView de ninguna manera. Por lo tanto, es posible (y recomendado) hacer que el control WebView sea invisible (es decir, oculto) durante el proceso de cierre de sesión.
