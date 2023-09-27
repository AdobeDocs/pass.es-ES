---
title: Guía de Apple SSO (SDK de iOS/tvOS)
description: Guía de Apple SSO (SDK de iOS/tvOS)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Guía de Apple SSO (SDK de iOS/tvOS) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#Introduction}

El SDK de Adobe Primetime Authentication AccessEnabler de iOS/tvOS puede admitir la autenticación de inicio de sesión único (SSO) de la plataforma para los usuarios finales de aplicaciones cliente que se ejecuten en iOS, iPadOS o tvOS a través de lo que llamamos flujo de trabajo de SSO de Apple.

Tenga en cuenta que este documento actúa como una extensión de la documentación existente del SDK de AccessEnabler para iOS/tvOS, que se puede encontrar [aquí](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Guía {#Cookbook}

Para beneficiarse de la experiencia del usuario de SSO de Apple, una aplicación tendría que integrar el SDK de AccessEnabler para iOS/tvOS y seguir la secuencia de sugerencias que se presenta a continuación.

</br>

### Requisitos previos {#Prerequisites}

</br>

#### Permiso

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Para tener acceso a la información de suscripción del usuario, el usuario debe dar permiso a la aplicación para continuar, de forma similar a proporcionar acceso a la cámara o al micrófono del dispositivo. Este permiso debe solicitarse por aplicación y el dispositivo guardará la selección del usuario. Tenga en cuenta que el usuario puede cambiar su decisión en la configuración de la aplicación (acceso al permiso del proveedor de TV) o en la sección de *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS.

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Se recomienda solicitar el permiso del usuario cuando la aplicación entre en el estado en primer plano, pero es solo una sugerencia, ya que la aplicación puede buscar [permiso para acceder a](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) la información de suscripción del usuario en cualquier momento antes de requerir la autenticación del usuario. Además, las API del SDK de iOS/tvOS de AccessEnabler solicitarán automáticamente el permiso del usuario cuando lo necesite.

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** En caso de que el usuario no conceda acceso a su información de suscripción o en caso de que falle la comunicación con la plataforma de la cuenta del suscriptor de vídeo, el SDK de AccessEnabler para iOS/tvOS volverá al flujo de autenticación normal.

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Recomendamos incentivar a los usuarios que se nieguen a dar permiso para acceder a la información de suscripción explicando las ventajas de la experiencia del usuario de inicio de sesión único (SSO). Tenga en cuenta que el usuario puede cambiar su decisión en la configuración de la aplicación (acceso al permiso del proveedor de TV) o en la sección de *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS.


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

</br>

#### Llamadas

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Implemente la siguiente lista de [callbacks](/help/authentication/iostvos-sdk-api-reference.md) que son específicos del flujo de trabajo de SSO de Apple.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - La llamada de retorno se activa cuando se abre el selector de MVPD de Apple.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - La llamada de retorno se activa cuando el selector de MVPD de Apple se va a cerrar.

</br>

#### Informes de errores

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Implemente la siguiente lista de [códigos de error avanzados](/help/authentication/error-reporting.md) que son específicos del flujo de trabajo de SSO de Apple.

- ***N003*** - El usuario seleccionó la opción &quot;Otro proveedor de TV&quot; del selector de MVPD de Apple.
- ***N004*** : el usuario seleccionó un proveedor de TV del selector de MVPD de Apple que no es compatible (integración o inicio de sesión único deshabilitado) con el solicitante actual.
- ***N005*** - El usuario decidió cancelar el selector de MVPD normal o el selector de MVPD de Apple.
- ***VSA403*** - Se ha denegado el permiso al proveedor de TV del usuario para la aplicación.
- ***VSA404*** - El permiso del proveedor de TV del usuario es indeterminado para la aplicación.
- ***VSA503*** - Error en la solicitud de metadatos de la cuenta del suscriptor de vídeo. Se proporciona más contexto en la *message* field.
- ***APL / APPL_ERROR*** - Error en la solicitud de metadatos de la cuenta del suscriptor de vídeo. Se proporciona más contexto en la *detalles* field.

</br>

### Autenticación {#Authentication}

>[!TIP]
>
> **<u>Sugerencia:</u>** Siga los pasos a continuación para las implementaciones de iOS/iPadOS/tvOS.

1. La aplicación tendría que [initialize](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) el SDK de AccessEnabler para iOS/tvOS.
1. La aplicación tendría que [establecer el identificador del solicitante actual](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Importante:** Este segundo paso podría almacenar en déclencheur un [código de error avanzado](/help/authentication/error-reporting.md) que es específico del flujo de trabajo de SSO de Apple, en caso de que **una de las siguientes opciones es verdadera**:

   - ***VSA403*** - Se ha denegado el permiso al proveedor de TV del usuario para la aplicación.
   - ***VSA404*** - El permiso del proveedor de TV del usuario es indeterminado para la aplicación.
   - ***APPL*** : Se ha producido un error en la comunicación entre el SDK de iOS/tvOS de AccessEnabler y la estructura de la cuenta del suscriptor de vídeo.

   Este segundo paso intentaría intercambiar de forma silenciosa el perfil SSO de Apple por un token de autenticación de Adobe, en caso de que **todo lo anterior es falso** y **todo lo siguiente es verdadero**:

   - El permiso Proveedor de TV del usuario se concede para la aplicación.
   - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel del sistema del dispositivo.
   - El SDK de AccessEnabler para iOS/tvOS recibió el identificador del proveedor de TV del usuario desde el marco de la cuenta del suscriptor de vídeo.
   - La integración del proveedor de TV del usuario con la aplicación se activa a través del panel de control de Adobe Primetime TVE.
   - El inicio de sesión único del proveedor de TV del usuario con la aplicación se habilita a través del panel de Adobe Primetime TVE.
   - El proveedor de TV del usuario no se degrada a través del panel de Adobe Primetime TVE.
   - El SDK de AccessEnabler para iOS/tvOS recibió la respuesta SAML del proveedor de TV del usuario desde el marco de trabajo de la cuenta del suscriptor de vídeo.

   **<u>Sugerencia profesional:</u>** Este segundo paso no almacenará en déclencheur ninguna otra llamada de retorno, aparte de [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) llamada de retorno, ya que la aplicación no inició explícitamente la autenticación.

1. La aplicación tendría que [comprobar el estado de autenticación](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Importante:** Este tercer paso podría almacenar en déclencheur un [código de error avanzado](/help/authentication/error-reporting.md) que es específico del flujo de trabajo de SSO de Apple, en caso de que **una de las siguientes opciones es verdadera**:

   - ***VSA403** - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero se ha denegado el permiso de proveedor de TV del usuario para la aplicación.
   - ***VSA404** - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero el permiso de proveedor de TV del usuario no se ha determinado para la aplicación.
   - ***APPL\_ERROR** : el usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero la comunicación entre el SDK de iOS/tvOS de AccessEnabler y la estructura de la cuenta del suscriptor de vídeo ha detectado un error.

   **Importante:** Este tercer paso almacenará en déclencheur la variable [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) devolución de llamada con *status* igual a 0, en caso de que **una de las siguientes opciones es verdadera**:

   - El usuario no ha iniciado sesión en su cuenta de proveedor de TV en el nivel del sistema del dispositivo o a través de un flujo de autenticación regular.
   - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo o a través de un flujo de autenticación normal, pero el TTL del token de autenticación de proveedor de TV del usuario ha pasado.
   - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel del sistema del dispositivo o a través de un flujo de autenticación normal, pero la integración del proveedor de TV del usuario con la aplicación se desactiva a través del panel de control de Adobe Primetime TVE.
   - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero el inicio de sesión único del proveedor de TV del usuario con la aplicación se deshabilita a través del panel de control de Adobe Primetime TVE.
   - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero se ha denegado el permiso de proveedor de TV del usuario para la aplicación.
   - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero el permiso de proveedor de TV del usuario no está determinado para la aplicación.
   - El usuario ha iniciado sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, pero se ha producido un error en la comunicación entre el SDK de iOS/tvOS de AccessEnabler y la estructura de la cuenta del suscriptor de vídeo.

   **Importante:** Este tercer paso almacenará en déclencheur la variable [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) devolución de llamada con *status* igual a 1, en caso de **todo lo anterior es falso.**


1. La aplicación tendría que [inicializar la autenticación](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) en caso de que la comprobación de estado de autenticación anterior activara la [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) devolución de llamada con *status* igual a 0.

   **<u>Sugerencia profesional:</u>** Implemente una de las siguientes API de SDK de AccessEnabler para iOS/tvOS [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) o [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Importante:** Este cuarto paso podría almacenar en déclencheur un [código de error avanzado](/help/authentication/error-reporting.md) que es específico del flujo de trabajo de SSO de Apple, en caso de que **una de las siguientes opciones es verdadera**:

   - ***VSA403*** - Se ha denegado el permiso al proveedor de TV del usuario para la aplicación.
   - ***VSA404*** - El permiso del proveedor de TV del usuario es indeterminado para la aplicación.
   - ***VSA503*** : Se ha producido un error en la comunicación entre el SDK de iOS/tvOS de AccessEnabler y la estructura de la cuenta del suscriptor de vídeo.
   - ***N003*** - El usuario seleccionó la opción &quot;Otro proveedor de TV&quot; del selector de MVPD de Apple.
   - ***N004*** : el usuario seleccionó un proveedor de TV del selector de MVPD de Apple que no es compatible (integración o inicio de sesión único deshabilitado) con el solicitante actual.
   - ***N005*** - El usuario decidió cancelar el selector de MVPD normal o el selector de MVPD de Apple.

   **Importante:** Este cuarto paso volvería al flujo de autenticación normal, activando el [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) callback y **uno** de lo anterior [códigos de error avanzados](/help/authentication/error-reporting.md), en caso de **una de las opciones anteriores es verdadera**.

   **Importante:** Este cuarto paso volvería al flujo de autenticación normal, activando el [navigationToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) o [navegarAurl:usarSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) callback y **ninguno** de lo anterior [códigos de error avanzados](/help/authentication/error-reporting.md), en caso de que el usuario haya seleccionado un proveedor de TV, que no admite SSO de Apple, pero está presente en el selector de MVPD de Apple.

   **<u>Sugerencia profesional:</u>** El SDK de AccessEnabler para iOS/tvOS llama silenciosamente a [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API, en caso de que el usuario haya seleccionado un proveedor de TV, que no admite SSO de Apple, pero está presente en el selector de MVPD de Apple.

   **Importante:** Este cuarto paso intentaría intercambiar de forma silenciosa el perfil SSO de Apple por un token de autenticación de Adobe, en caso de que **todo lo anterior es falso** y **todo lo siguiente es verdadero**:

   - El permiso Proveedor de TV del usuario se concede para la aplicación.
   - El usuario ha iniciado sesión o está iniciando sesión en su cuenta de proveedor de TV a nivel de sistema de dispositivo.
   - El SDK de AccessEnabler para iOS/tvOS recibió el identificador del proveedor de TV del usuario desde el marco de la cuenta del suscriptor de vídeo.
   - La integración del proveedor de TV del usuario con la aplicación se activa a través del panel de control de Adobe Primetime TVE.
   - El inicio de sesión único del proveedor de TV del usuario con la aplicación se habilita a través del panel de Adobe Primetime TVE.
   - El proveedor de TV del usuario no se degrada a través del panel de Adobe Primetime TVE.
   - El SDK de AccessEnabler para iOS/tvOS recibió la respuesta SAML del proveedor de TV del usuario desde el marco de trabajo de la cuenta del suscriptor de vídeo.



>**<u>Sugerencia profesional:</u>** Este cuarto paso almacenará en déclencheur la variable [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) devolución de llamada, independientemente de *status* como resultado, ya que la aplicación inició explícitamente la autenticación.


</br>

### Metadatos {#Metadata}

La aplicación tiene la opción de determinar si la autenticación se ha producido como resultado de un inicio de sesión a través del SSO de plataforma o no, utilizando el &quot;*tokenSource&quot;* [metadatos de usuario](/help/authentication/iostvos-sdk-api-reference.md#getMeta) API del SDK de iOS/tvOS de AccessEnabler.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Cerrar sesión {#Logout}

El [Cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) Este marco de trabajo no proporciona una API para cerrar la sesión de las personas que han iniciado sesión en su cuenta de proveedor de TV a nivel de sistema de dispositivo mediante programación. Por lo tanto, para que el cierre de sesión surta efecto, el usuario final tendría que cerrar sesión explícitamente desde *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS. La otra opción que tendría el usuario es retirar el permiso para acceder a la información de suscripción del usuario desde la sección de configuración específica de la aplicación (acceso al permiso del proveedor de TV).

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del SDK de AccessEnabler para iOS/tvOS [cierre de sesión](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Siga los pasos a continuación para las implementaciones de tvOS.

- La aplicación tendría que [iniciar el cierre de sesión](/help/authentication/iostvos-sdk-api-reference.md#logout) del SDK de AccessEnabler para iOS/tvOS. Esto no facilitaría la limpieza de la sesión en el lado de MVPD.
- La aplicación tendría que indicar o pedir al usuario que cierre sesión explícitamente desde *`Settings -> Accounts -> TV Provider`* en tvOS solo por si acaso [*VSA203* código de estado activado](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Siga los pasos a continuación para las implementaciones de iOS/iPadOS.

- La aplicación tendría que [iniciar el cierre de sesión](/help/authentication/iostvos-sdk-api-reference.md#logout) del SDK de AccessEnabler para iOS/tvOS. Esto facilitaría la limpieza de la sesión en el lado de MVPD.
- La aplicación tendría que indicar o pedir al usuario que cierre sesión explícitamente desde *`Settings -> TV Provider`* en iOS/iPadOS solo en caso de [*VSA203* código de estado activado](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
