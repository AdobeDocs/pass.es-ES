---
title: Inicio y cierre de sesión sin actualización
description: Inicio y cierre de sesión sin actualización
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1772'
ht-degree: 0%

---

# Inicio y cierre de sesión sin actualización {#tefresh-less-login-and-logout}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

Para las aplicaciones web, debe tener en cuenta diferentes escenarios posibles para autenticar y cerrar la sesión de los usuarios.  Las MVPD requieren que los usuarios inicien sesión en la página web de las MVPD para autenticarse, y que entren en juego los siguientes factores adicionales:

- Algunas MVPD requieren un redireccionamiento completo desde el sitio a su página de inicio de sesión
- Algunas MVPD requieren que abra un iFrame en su sitio para mostrar la página de inicio de sesión de la MVPD
- Algunos exploradores no gestionan bien el escenario de iFrame, por lo que una mejor alternativa para estos exploradores es utilizar una ventana emergente en lugar del iFrame

Antes de la autenticación de Adobe Pass 2.7, todos estos escenarios para autenticar a un usuario implicaban una actualización de la página completa de la página del programador. En las versiones 2.7 y posteriores, el equipo de autenticación de Adobe Pass mejoró estos flujos para que el usuario no tenga que experimentar una actualización de la página en la aplicación durante el inicio de sesión y el cierre de sesión.


## Descripción detallada {#detailed_description}

Empecemos con un resumen de los flujos de autenticación y cierre de sesión originales, y luego continuemos con los flujos de autenticación y cierre de sesión mejorados. Tenga en cuenta que las primeras cuatro secciones tratan las MVPD normales (que no son TempPass), mientras que la última sección describe la implementación especial que debe aplicarse para TempPass:

- [Flujo de autenticación original](#orig_authn)
- [Flujo de cierre de sesión original](#orig_logout)
- [Flujo de autenticación mejorado](#improved_authn)
- [Flujo de cierre de sesión mejorado](#improved_logout)
- [Flujo TempPass](#improved_temppass)

</br>

## Flujos de autenticación/cierre de sesión originales {#orig_authn}

**Autenticación**

Los clientes web de autenticación de Adobe Pass tienen dos formas de autenticarse, según los requisitos de las MVPD:

1. **Redireccionamiento de página completa -** Una vez que el usuario selecciona un proveedor (configurado con redireccionamiento de página completa) desde el selector de MVPD del sitio web del programador, `setSelectedProvider(<mvpd>)` se invoca en AccessEnabler y se redirige al usuario a la página de inicio de sesión de la MVPD. Una vez que el usuario proporciona credenciales válidas, se le redirige de nuevo al sitio web del programador. AccessEnabler se inicializa y el token de autenticación se recupera de la autenticación de Adobe Pass durante `setRequestor`.
1. **iFrame / Ventana emergente -** Una vez que el usuario selecciona un proveedor (configurado con iFrame), `setSelectedProvider(<mvpd>)` se invoca en AccessEnabler. Esta acción almacenará en déclencheur la variable `createIFrame(width, height)` llamada de retorno, notificando al programador que cree un iFrame (o ventana emergente, según el navegador o las preferencias) con el nombre `"mvpdframe"` y las dimensiones proporcionadas. Una vez creado el iFrame/popup, AccessEnabler carga la página de inicio de sesión de la MVPD en el iFrame/popup. El usuario proporciona credenciales válidas y el iFrame/elemento emergente se redirige a Autenticación de Adobe Pass, que devuelve un fragmento de JS que cierra el iFrame/elemento emergente y vuelve a cargar la página principal (sitio web del programador). De forma similar al flujo 1, el token de autenticación se recupera durante `setRequestor`.

El `displayProviderDialog` devolución de llamada (desencadenada por `getAuthentication`/`getAuthorization`) devuelve una lista de MVPD y su configuración apropiada. El `iFrameRequired` La propiedad de un MVPD permite al Programador saber si debe activar el flujo 1 o el flujo 2. Tenga en cuenta que el programador debe realizar una acción adicional (crear un iFrame/popup) solo para el flujo 2.

**Cancelar autenticación**

También se da el caso de que el usuario cancela explícitamente el flujo de autenticación cerrando la página de inicio de sesión. Estos son los escenarios y la solución propuesta para los Programadores:

1. **Redireccionamiento de página completa -** Cuando se cierra la página de inicio de sesión, el usuario debe volver a navegar al sitio web del programador e iniciar todo el flujo desde el principio. En este escenario, no se requiere ninguna acción explícita por parte del programador.
1. **iFrame -** Se recomienda al programador que aloje el iFrame dentro de un `div` (o un componente de interfaz de usuario similar) que tenga un botón Cerrar adjunto. Cuando el usuario pulsa el botón Cerrar, el programador destruye el iFrame junto con la interfaz de usuario asociada y realiza `setSelectedProvider(null)`. Esta llamada permite al AccessEnabler borrar su estado interno y permite al usuario iniciar un flujo de autenticación posterior. `setAuthenticationStatus` y `sendTrackingData(AUTHENTICATION_DETECTION...)` se activará para indicar un flujo de autenticación fallido (ambos activados) `getAuthentication` y `getAuthorization`).
1. **Emergente -** Algunos exploradores no pueden detectar con precisión el evento de cierre de ventana, por lo que se debe realizar un enfoque diferente aquí (a diferencia del flujo de iFrame anterior). Adobe recomienda que el programador inicie un temporizador que compruebe periódicamente si existe la ventana emergente de inicio de sesión. Si la ventana no existe, el programador puede estar seguro de que el usuario ha cancelado manualmente el flujo de inicio de sesión y puede continuar llamando a `setSelectedProvider(null)`. Las llamadas de retorno activadas son las mismas que en el flujo 2 anterior.

</br>

## Flujo de cierre de sesión original {#orig_logout}

La API de cierre de sesión de AccessEnabler borra el estado local de la biblioteca y carga la URL de cierre de sesión de MVPD en la pestaña o ventana actual. El navegador navega hasta el punto final de cierre de sesión de la MVPD y, una vez completado el proceso, se redirige al usuario al sitio web del programador. La única acción que se requiere en nombre del usuario es presionar el botón/vínculo Cerrar sesión e iniciar el flujo; no se requiere la interacción del usuario en el punto final de cierre de sesión de la MVPD.

**Flujo De Autenticación/Cierre De Sesión Original Con Actualización De Página**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## Autenticación mejorada (sin actualización) {#improved_authn}

>[!NOTE]
>
>Los flujos de inicio y cierre de sesión mejorados sin actualización requieren que el explorador admita tecnologías modernas de HTML5, incluida la mensajería web.

Los flujos de autenticación (inicio de sesión) y cierre de sesión mencionados anteriormente proporcionan una experiencia de usuario similar al volver a cargar la página principal después de completar cada flujo.  La función actual pretende mejorar la experiencia del usuario al proporcionar un inicio y un cierre de sesión sin actualización (en segundo plano). El programador puede habilitar/deshabilitar el inicio y cierre de sesión en segundo plano pasando dos indicadores booleanos (`backgroundLogin` y `backgroundLogout`) a la `configInfo` parámetro del `setRequestor` API. De forma predeterminada, el inicio y cierre de sesión en segundo plano están desactivados (esto proporciona compatibilidad con la implementación anterior).

**Ejemplo:**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**Autenticación**

Los siguientes puntos describen la transición entre los flujos de autenticación originales y los flujos mejorados:

1. La redirección de página completa se reemplaza con una nueva pestaña del explorador en la que se realiza el inicio de sesión de MVPD. Se requiere el Programador para crear una nueva pestaña (a través de `window.open`) denominado `mvpdwindow` cuando el usuario selecciona una MVPD (con `iFrameRequired = false`). A continuación, el programador ejecuta `setSelectedProvider(<mvpd>)`, permitiendo que AccessEnabler cargue la URL de inicio de sesión de MVPD en la nueva pestaña. Una vez que el usuario proporciona credenciales válidas, la autenticación de Adobe Pass cierra la pestaña y envía un window.postMessage al sitio web del programador que indica al AccessEnabler que el flujo de autenticación ha finalizado. Se activan las siguientes llamadas de retorno:

   - Si el flujo lo inició `getAuthentication`: `setAuthenticationStatus` y `sendTrackingData(AUTHENTICATION_DETECTION...)` se activará para indicar una autenticación correcta o incorrecta.

   - Si el flujo lo inició `getAuthorization`: `setToken/tokenRequestFailed` y `sendTrackingData(AUTHORIZATION_DETECTION...)` se activará para indicar una autorización correcta o incorrecta.

1. El flujo de la ventana emergente/iFrame permanece prácticamente sin cambios, con la diferencia de que después de que el usuario proporcione credenciales válidas, la página principal no se vuelve a cargar. El iFrame/ventana emergente se cerrará automáticamente tras el inicio de sesión y `window.postMessage` se envía a la página principal, notificando a AccessEnabler que el flujo ha finalizado. Se activan las mismas llamadas de retorno que en el flujo anterior, **además de la siguiente llamada de retorno nueva:`destroyIFrame`**. El `destroyIFrame` La devolución de llamada permite al programador eliminar cualquier componente auxiliar o asociado a iFrame, como las decoraciones de la interfaz de usuario. La existencia de esta llamada de retorno no era necesaria en el flujo de autenticación antiguo porque, una vez completado el inicio de sesión, la autenticación de Adobe Pass volvía a cargar la página del programador, destruyendo así todos los componentes de la interfaz de usuario que contenía.

</br>

>[!IMPORTANT]
> 
>Debe cargar el iFrame de inicio de sesión de MVPD o la ventana emergente como elemento secundario directo de la página que contiene la instancia de AccessEnabler. Si el iFrame de inicio de sesión de MVPD o la ventana emergente están anidados dos o más niveles por debajo de la página que contiene la instancia de AccessEnabler, el flujo podría bloquearse. Por ejemplo, si tuviera un iFrame situado entre la página principal y el iFrame de MVPD (Página =\> iFrame =\> iFrame de MVPD), el flujo de inicio de sesión podría fallar.

</br>

**Cancelar autenticación**

Estos son los flujos para cancelar la autenticación:

1. **Pestaña Explorador -** Dado que la pestaña es básicamente una nueva ventana, la captura de su evento de cierre tiene las mismas limitaciones que se describen en el escenario 3 desde los flujos de autenticación antiguos. Además, el método del temporizador no es posible aquí, ya que no hay forma de distinguir entre una pestaña que el usuario cerró manualmente y una pestaña que se cerró automáticamente al final del flujo de inicio de sesión. La solución aquí es que AccessEnabler permanezca &quot;silencioso&quot; (no se activan las llamadas de retorno) cuando el usuario cancela el flujo. Además, el programador no está obligado a realizar ninguna acción específica. El usuario podrá iniciar otro flujo de autenticación sin recibir el error &quot;Error de varias solicitudes de autenticación&quot; (este error se ha deshabilitado en AccessEnabler para el inicio de sesión en segundo plano).

1. **iFrame -** El programador puede seguir el enfoque descrito en el escenario 2 desde los flujos de autenticación antiguos (crear una interfaz de usuario envolvente a partir del iFrame y el botón Cerrar asociado que crea un déclencheur) `setSelectedProvider(null)`. Aunque este método ya no es un requisito estricto (se permiten varios flujos de autenticación para el inicio de sesión en segundo plano, como se explica en el escenario 1 anterior), el Adobe sigue recomendándolo.

1. **Emergente -** Esto es idéntico al flujo de la pestaña del explorador anterior.

</br>

## Flujo de cierre de sesión mejorado {#improved_logout}

El nuevo flujo de cierre de sesión se realizará en un iFrame oculto, lo que elimina el redireccionamiento de página completa.  Esto es posible porque el usuario no necesita realizar ninguna acción específica en la página de cierre de sesión de la MVPD.

Una vez completado el flujo de cierre de sesión, redireccionará el iFrame a un punto final de autenticación de Adobe Pass personalizado. Esto servirá para un fragmento de JS que realice una `window.postMessage` al elemento principal, notificando a AccessEnabler que se ha completado el cierre de sesión. Se activan las siguientes llamadas de retorno: `setAuthenticationStatus()` y `sendTrackingData(AUTHENTICATION_DETECTION ...)`, indicando que el usuario ya no está autenticado.

La siguiente ilustración muestra el flujo sin actualización que permite a un usuario iniciar sesión en su MVPD sin actualizar la página principal de la aplicación:

**Flujo de autenticación/cierre de sesión mejorado (sin actualización)**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## Flujo TempPass {#improved_temppas}

El inicio de sesión sin actualización sigue un enfoque diferente para las MVPD de tipo TempPass.

Dado que el flujo TempPass requiere que se cree una ventana automáticamente y se cierre sin interacción explícita del usuario, puede suponer un problema para algunos exploradores (bloqueadores de ventanas emergentes). Por lo tanto, AccessEnabler implementa la fase de inicio de sesión entre bastidores, sin necesidad de un contenedor web creado por el programador.

Estos son los aspectos que el programador debe tener en cuenta al implementar TempPass para el inicio y cierre de sesión sin actualización:

- Antes de iniciar la autenticación, solo es necesario crear el iFrame o la ventana emergente para las MVPD que no sean TempPass. El programador puede detectar si una MVPD es TempPass o no leyendo el `tempPass` propiedad del objeto MVPD (devuelto por `setConfig()` / `displayProviderDialog()`).

- El `createIFrame()` La devolución de llamada debe contener una comprobación para TempPass y ejecutar su lógica sólo cuando la MVPD NO sea TempPass.

- El `destroyIFrame()` La devolución de llamada debe contener una comprobación para TempPass y ejecutar su lógica sólo cuando la MVPD NO sea TempPass.

- El `setAuthenticationStatus()` y `sendTrackingData()` las llamadas de retorno se invocan una vez finalizada la autenticación (exactamente como en el flujo sin actualización para las MVPD normales).

>[!NOTE]
>
>Este flujo solo está disponible para TempPass sin actualización. Para el flujo de actualización, TempPass debe gestionarse explícitamente (cuando TempPass requiera iFrame/ventana emergente)

</br>

El siguiente ejemplo de código muestra cómo controlar una ventana de MVPD en el sitio web de un programador (tanto para MVPD normales como para TempPass):

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
