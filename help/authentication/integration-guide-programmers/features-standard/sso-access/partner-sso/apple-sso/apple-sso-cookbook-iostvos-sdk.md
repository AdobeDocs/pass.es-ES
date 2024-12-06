---
title: Guía de Apple SSO (SDK de iOS/tvOS)
description: Guía de Apple SSO (SDK de iOS/tvOS)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1811'
ht-degree: 0%

---

# Guía de Apple SSO (SDK de iOS/tvOS) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El SDK de Adobe Pass Authentication AccessEnabler de iOS/tvOS es compatible con el inicio de sesión único (SSO) de socio para los usuarios finales de aplicaciones cliente que se ejecutan en iOS, iPadOS o tvOS.

Este documento actúa como una extensión de la documentación existente del SDK de AccessEnabler para iOS/tvOS, que se puede encontrar [aquí](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md).

## Guía {#apple-sso-cookbook-iostvos-sdk-cookbook}

Para beneficiarse de la experiencia del usuario de SSO de Apple, la aplicación debe integrar el SDK de AccessEnabler para iOS/tvOS y seguir la secuencia de pasos que se presenta a continuación.

### Requisitos previos {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### Permiso {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** La aplicación de transmisión debe solicitar acceso a la información de suscripción del usuario guardada en el nivel de dispositivo, para lo cual el usuario debe dar permiso a la aplicación para continuar, de forma similar a proporcionar acceso a la cámara o al micrófono del dispositivo. Se debe solicitar este permiso por aplicación utilizando el [marco de trabajo de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) de Apple, y el dispositivo guardará la selección del usuario.

>[!TIP]
>
> **<u>Consejo profesional:</u>** Recomendamos incentivar a los usuarios que se nieguen a dar permiso para acceder a la información de la suscripción explicando los beneficios de la experiencia del usuario de inicio único de sesión de Apple, pero tenga en cuenta que el usuario puede cambiar su decisión yendo a la configuración de la aplicación (acceso de permiso del proveedor de TV) o a *`Settings -> TV Provider`* en iOS y iPadOS o a *`Settings -> Accounts -> TV Provider`* en tvOS.

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** La aplicación de transmisión puede solicitar el permiso del usuario cuando la aplicación entra en el estado de primer plano, ya que la aplicación puede comprobar el permiso de [para acceder a](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) la información de suscripción del usuario en cualquier momento antes de requerir la autenticación del usuario.

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Si el usuario no concede acceso a su información de suscripción o si la comunicación con el marco de trabajo de la cuenta del suscriptor de vídeo falla, el SDK de AccessEnabler para iOS/tvOS volverá al flujo de autenticación normal.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

#### Llamadas {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Implemente la siguiente lista de [devoluciones de llamadas](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) específicas del flujo de trabajo de SSO de Apple.

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog): la devolución de llamada se activó cuando se abre el selector de MVPD de Apple.
* [*dismissTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog): la devolución de llamada se activó cuando el selector de MVPD de Apple se va a cerrar.

#### Informes de errores {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Implemente la siguiente lista de [códigos de error avanzados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) específicos del flujo de trabajo de SSO de Apple.

* ***N003*** - El usuario seleccionó la opción &quot;Otro proveedor de TV&quot; del selector de MVPD de Apple.
* ***N004***: el usuario seleccionó un proveedor de TV del selector de MVPD de Apple, que no es compatible (integración o inicio de sesión único deshabilitado) por el solicitante actual.
* ***N005*** - El usuario decidió cancelar el selector MVPD normal o el selector MVPD de Apple.
* ***VSA403*** - Se ha denegado el permiso al proveedor de TV del usuario para la aplicación.
* ***VSA404***: el permiso del proveedor de TV del usuario no está determinado para la aplicación.
* ***VSA503***: error en la solicitud de metadatos de la cuenta del suscriptor de vídeo. Se proporciona más contexto en el campo *mensaje*.
* ***AAPL / APPL_ERROR***: error en la solicitud de metadatos de la cuenta del suscriptor de vídeo. Se proporciona más contexto en el campo *detalles*.

### Autenticación {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>Sugerencia:</u>** Siga los pasos descritos a continuación para las implementaciones de iOS/iPadOS/tvOS.

1. La aplicación tendría que [inicializar](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) el SDK de AccessEnabler para iOS/tvOS.


1. La aplicación tendría que [establecer el identificador del solicitante actual](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Importante:** Este segundo paso podría almacenar en déclencheur un [código de error avanzado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) que es específico del flujo de trabajo de SSO de Apple, en caso de que **una de las siguientes opciones sea verdadera**:

   * ***VSA403*** - Se ha denegado el permiso al proveedor de TV del usuario para la aplicación.
   * ***VSA404***: el permiso del proveedor de TV del usuario no está determinado para la aplicación.
   * ***APPL***: se produjo un error en la comunicación entre el SDK de AccessEnabler iOS/tvOS y el marco de trabajo de la cuenta del suscriptor de vídeo.

   Este segundo paso intentaría intercambiar silenciosamente el perfil SSO de Apple por un token de autenticación de Adobe, en caso de que **todo lo anterior sea falso** y **todo lo siguiente sea verdadero**:

   * El permiso Proveedor de TV del usuario se concede para la aplicación.
   * El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel del sistema del dispositivo.
   * El SDK de AccessEnabler para iOS/tvOS recibió el identificador del proveedor de TV del usuario desde el marco de trabajo de la cuenta del suscriptor de vídeo.
   * La integración del proveedor de TV del usuario con la aplicación se activa a través del panel de control de Adobe Primetime TVE.
   * El inicio de sesión único del proveedor de TV del usuario con la aplicación se habilita a través del panel de Adobe Primetime TVE.
   * El proveedor de TV del usuario no se degrada a través del panel de Adobe Primetime TVE.
   * El SDK de AccessEnabler para iOS/tvOS recibió la respuesta SAML del proveedor de TV del usuario desde el marco de cuentas del suscriptor de vídeo.

   **<u>Sugerencia profesional:</u>** Este segundo paso no almacenará en déclencheur ninguna otra devolución de llamada, aparte de la devolución de llamada [setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete), ya que la aplicación no inició explícitamente la autenticación.


1. La aplicación tendría que [comprobar el estado de autenticación](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Importante:** Este tercer paso podría almacenar en déclencheur un [código de error avanzado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) que es específico del flujo de trabajo de SSO de Apple, en caso de que **una de las siguientes opciones sea verdadera**:

   * ***VSA403** - El usuario inició sesión en su cuenta de proveedor de TV en
el nivel del sistema del dispositivo, pero el permiso del proveedor de TV del usuario es
denegado para la aplicación.
   * ***VSA404** - El usuario ha iniciado sesión en su cuenta de proveedor de TV en
el nivel del sistema del dispositivo, pero el permiso del proveedor de TV del usuario
no se ha determinado para la aplicación.
   * ***APPL\_ERROR**: el usuario ha iniciado sesión en su proveedor de TV
cuenta en el nivel del sistema del dispositivo, pero la comunicación entre
el SDK de AccessEnabler para iOS/tvOS y la cuenta de suscriptor de vídeo
framework ha encontrado un error.

   **Importante:** Este tercer paso almacenará en déclencheur la llamada de retorno [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) con *status* igual a 0, en caso de que **una de las siguientes opciones sea verdadera**:

   * El usuario no ha iniciado sesión en su cuenta de proveedor de TV en el nivel del sistema del dispositivo o a través de un flujo de autenticación regular.
   * El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo o a través de un flujo de autenticación normal, pero el TTL del token de autenticación de proveedor de TV del usuario ha pasado.
   * El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel del sistema del dispositivo o a través de un flujo de autenticación normal, pero la integración del proveedor de TV del usuario con la aplicación se desactiva a través del panel de control de Adobe Primetime TVE.
   * El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero el inicio de sesión único del proveedor de TV del usuario con la aplicación se deshabilita a través del panel de control de Adobe Primetime TVE.
   * El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero se ha denegado el permiso de proveedor de TV del usuario para la aplicación.
   * El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero el permiso de proveedor de TV del usuario no está determinado para la aplicación.
   * El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero se ha producido un error en la comunicación entre el SDK de iOS/tvOS de AccessEnabler y el marco de trabajo de la cuenta del suscriptor de vídeo.

   **Importante:** Este tercer paso almacenará en déclencheur la llamada de retorno [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) con *status* igual a 1, en caso de que **todo lo anterior sea falso.**


1. La aplicación tendría que [inicializar la autenticación](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) en caso de que la comprobación de estado de autenticación anterior activara la devolución de llamada [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) con *status* igual a 0.

   **<u>Sugerencia profesional:</u>** Implemente una de las siguientes API del SDK de AccessEnabler iOS/tvOS [getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) o [getAuthentication:filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Importante:** Este cuarto paso podría almacenar en déclencheur un [código de error avanzado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) que es específico del flujo de trabajo de SSO de Apple, en caso de que **una de las siguientes opciones sea verdadera**:

   * ***VSA403*** - Se ha denegado el permiso al proveedor de TV del usuario para la aplicación.
   * ***VSA404***: el permiso del proveedor de TV del usuario no está determinado para la aplicación.
   * ***VSA503***: se produjo un error en la comunicación entre el SDK de AccessEnabler iOS/tvOS y el marco de trabajo de la cuenta del suscriptor de vídeo.
   * ***N003*** - El usuario seleccionó la opción &quot;Otro proveedor de TV&quot; del selector de MVPD de Apple.
   * ***N004***: el usuario seleccionó un proveedor de TV del selector de MVPD de Apple, que no es compatible (integración o inicio de sesión único deshabilitado) por el solicitante actual.
   * ***N005*** - El usuario decidió cancelar el selector MVPD normal o el selector MVPD de Apple.

   **Importante:** Este cuarto paso volvería al flujo de autenticación normal, activando la devolución de llamada [displayProviderDialog](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog) y **uno** de los [códigos de error avanzados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) anteriores, en caso de que **uno de los anteriores sea verdadero**.

   **Importante:** Este cuarto paso volvería al flujo de autenticación normal, activando la llamada de retorno [navegarToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url) o [navegarToUrl:usarSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC) y **ninguno** de los [códigos de error avanzados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) anteriores, en caso de que el usuario seleccionara un proveedor de TV, que no admite SSO de Apple, pero que está presente en el selector de MVPD de Apple.

   **<u>Sugerencia profesional:</u>** El SDK de AccessEnabler para iOS/tvOS llama silenciosamente a la API [setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv), en caso de que el usuario haya seleccionado un proveedor de TV, que no admite SSO de Apple, pero está presente en el selector de MVPD de Apple.

   **Importante:** Este cuarto paso intentaría intercambiar silenciosamente el perfil SSO de Apple por un token de autenticación de Adobe, en caso de que **todo lo anterior sea falso** y **todo lo siguiente sea verdadero**:

   * El permiso Proveedor de TV del usuario se concede para la aplicación.
   * El usuario ha iniciado sesión o está iniciando sesión en su cuenta de proveedor de TV a nivel de sistema de dispositivo.
   * El SDK de AccessEnabler para iOS/tvOS recibió el identificador del proveedor de TV del usuario desde el marco de trabajo de la cuenta del suscriptor de vídeo.
   * La integración del proveedor de TV del usuario con la aplicación se activa a través del panel de control de Adobe Primetime TVE.
   * El inicio de sesión único del proveedor de TV del usuario con la aplicación se habilita a través del panel de Adobe Primetime TVE.
   * El proveedor de TV del usuario no se degrada a través del panel de Adobe Primetime TVE.
   * El SDK de AccessEnabler para iOS/tvOS recibió la respuesta SAML del proveedor de TV del usuario desde el marco de cuentas del suscriptor de vídeo.

   **<u>Sugerencia profesional:</u>** Este cuarto paso almacenará en déclencheur la llamada de retorno [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus), independientemente del resultado *status*, ya que la aplicación inició explícitamente la autenticación.

### Metadatos {#apple-sso-cookbook-iostvos-sdk-metadata}

La aplicación tiene la opción de determinar si la autenticación se ha producido como resultado de un inicio de sesión a través del SSO del socio o no, utilizando la API *tokenSource&quot;* [metadatos de usuario](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) del SDK de AccessEnabler iOS/tvOS.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### Cerrar sesión {#apple-sso-cookbook-iostvos-sdk-logout}

El [marco de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) no proporciona una API para cerrar la sesión mediante programación de las personas que han iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo. Por lo tanto, para que el cierre de sesión surta efecto, el usuario final tendría que cerrar sesión explícitamente desde *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS. La otra opción que tendría el usuario es retirar el permiso para acceder a la información de suscripción del usuario desde la sección de configuración específica de la aplicación (acceso al permiso del proveedor de TV).

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del medio de la API [logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) del SDK de AccessEnabler iOS/tvOS.

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Siga los pasos descritos a continuación para las implementaciones de tvOS.

* La aplicación tendría que [iniciar el cierre de sesión](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) desde el SDK de AccessEnabler para iOS/tvOS. Esto no facilitaría la limpieza de la sesión en el lado de MVPD.
* La aplicación tendría que indicar o pedir al usuario que cierre sesión explícitamente desde *`Settings -> Accounts -> TV Provider`* en tvOS solo en caso de que se active el código de estado [*VSA203*](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md).

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Siga los pasos descritos a continuación para las implementaciones de iOS/iPadOS.

* La aplicación tendría que [iniciar el cierre de sesión](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) desde el SDK de AccessEnabler para iOS/tvOS. Esto facilitaría la limpieza de la sesión en el lado de MVPD.
* La aplicación tendría que indicar o pedir al usuario que cierre sesión explícitamente desde *`Settings -> TV Provider`* en iOS/iPadOS solo en caso de que se active el código de estado [*VSA203*](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md).
