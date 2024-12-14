---
title: Cómo migrar la página de inicio de sesión de MVPD de iFrame a emergente
description: Cómo migrar la página de inicio de sesión de MVPD de iFrame a emergente
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '709'
ht-degree: 0%

---

# (Heredado) Migración de la página de inicio de sesión de MVPD de iFrame a emergente {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Emergente frente a iFrame {#popup-vs-iframe}

Algunos usuarios han encontrado problemas de cookies de terceros con la implementación de iFrame de una página de inicio de sesión de MVPD.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

El equipo de autenticación de Adobe Pass **recomienda implementar la página emergente/nueva ventana de inicio de sesión** en lugar de la versión de iFrame en Firefox y Safari.  Sin embargo, si va a implementar una página de inicio de sesión para Internet Explorer, puede encontrar problemas con la implementación emergente. Los problemas de IE se deben al hecho de que, después de que el usuario se autentique con su MVPD en la ventana emergente, la autenticación de Adobe Pass fuerza un redireccionamiento de página principal, que Internet Explorer ve como un bloqueador de ventanas emergentes. El equipo de autenticación de Adobe Pass **recomienda implementar el inicio de sesión de iFrame para Internet Explorer**.

El código de ejemplo presentado en esta nota técnica utiliza una implementación híbrida de iFrame y popup, lo que abre un iFrame en Internet Explorer y una ventana emergente en los demás navegadores.

Teniendo en cuenta que ya existe una implementación de iFrame, la primera parte de la nota técnica presenta el código para la implementación de iFrame y la segunda parte presenta los cambios para dar cabida a la implementación emergente como predeterminada.


## Selector de MVPD con página de inicio de sesión en un iFrame {#mvpd-pickr-iframe}

Los ejemplos de código anteriores mostraban una página de HTML que contiene la etiqueta &lt;div> en la que se va a crear el iFrame junto con el botón cerrar iFrame:

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

Este es el código **JavaScript** asociado:

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## Selector de MVPD con página de inicio de sesión en una ventana emergente {#mvpd-pickr-popup}

Como ya no usaremos **iFrame**, el código del HTML no contendrá el iFrame ni el botón para cerrar el iFrame. El div que anteriormente contenía el iFrame - **mvpddiv** - se conservará y utilizará para lo siguiente:

* para notificar al usuario de que la página de inicio de sesión de MVPD ya está abierta si se pierde el enfoque emergente
* para proporcionar un vínculo que le permita volver a centrarse en la ventana emergente

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

La lista de MVPD se mostrará en el div llamado **selector** como **-mvpdList** seleccionado.

Se usará una nueva llamada de retorno de API: **setConfig(configXML)**. La llamada de retorno se activa después de llamar a la función setRequestor(requestorID). Esta llamada de retorno devuelve la lista de MVPD que están integradas con el ID del solicitante establecido anteriormente. En el método de devolución de llamada, se analizará el XML entrante y se almacenará en caché la lista de MVPD. El selector de MVPD también se crea, pero no se muestra.

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

Después de llamar a las funciones getAuthentication() o getAuthorization(), se activa la llamada de retorno displayProviderDialog(). Normalmente, dentro de esta llamada de retorno, se habría creado y mostrado la lista de MVPD. Dado que el selector de MVPD ya está creado, lo único que queda por hacer es mostrarlo al usuario.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

Una vez que el usuario ha seleccionado una MVPD del selector, es necesario crear la ventana emergente. Algunos exploradores pueden bloquear la ventana emergente si se crea con about:blank o con una página que se encuentra en otro dominio; por lo tanto, se recomienda abrirla con el nombre de host desde el que se carga AccessEnabler.

En la implementación de iFrame, el restablecimiento del flujo de autenticación se realizaba mediante el botón btnCloseIframe y la función de JavaScript closeIframeAction(), pero ahora la decoración del iFrame ya no es posible. Por lo tanto, se logra el mismo comportamiento observando cuándo se cierra la ventana emergente (ya sea por el usuario o al finalizar el flujo de autenticación). Se ha agregado un fragmento de código que también ayuda en caso de que el usuario pierda el enfoque de la ventana emergente:

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

En la llamada de retorno createIFrame() se mostrará el div **mvpddiv**.

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* El código de ejemplo contiene una variable codificada para el ID de solicitante utilizado: &quot;REF&quot;, que debe reemplazarse por un ID de solicitante de programador real.
>* El código de ejemplo solo se ejecutará correctamente desde un dominio de la lista blanca asociado al ID de solicitante utilizado.
>* Dado que todo el código está disponible para su descarga, el código presentado en esta nota técnica se ha truncado. Para ver una muestra completa, vea **JS iFrame vs Popup Sample**.
>* Las bibliotecas de JavaScript externas se vincularon desde [Google Hosted Services](https://developers.google.com/speed/libraries/).
