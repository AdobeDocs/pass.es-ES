---
title: Guía de Apple SSO (API REST V2)
description: Guía de Apple SSO (API REST V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: 63ffde4a32f003d7232d2c79ed6878ca59748f74
workflow-type: tm+mt
source-wordcount: '3857'
ht-degree: 0%

---

# Guía de Apple SSO (API REST V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La API de REST de autenticación de Adobe Pass V2 es compatible con el inicio de sesión único (SSO) de socio para usuarios finales de aplicaciones cliente que se ejecutan en iOS, iPadOS o tvOS.

Este documento actúa como una extensión de la [Información general de la API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existente que proporciona una vista de alto nivel y el documento que describe cómo implementar el [inicio de sesión único mediante flujos de socios](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

## Inicio de sesión único de Apple con flujos de socios {#cookbook}

### Requisitos previos {#prerequisites}

Antes de continuar con el inicio de sesión único de Apple mediante flujos de socios, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming debe recopilar todos los datos necesarios requeridos por los encabezados `X-Device-Info` o `User-Agent` de modo que el backend de autenticación de Adobe Pass pueda identificar la plataforma del dispositivo y sus funcionalidades. Para obtener más información sobre el encabezado `X-Device-Info`, consulte la documentación de [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md).

* La aplicación de streaming debe solicitar acceso a la información de suscripción del usuario guardada en el nivel de dispositivo, para lo cual el usuario debe dar permiso a la aplicación para continuar, de forma similar a proporcionar acceso a la cámara o al micrófono del dispositivo. Se debe solicitar este permiso por aplicación utilizando el [marco de trabajo de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) de Apple, y el dispositivo guardará la selección del usuario.

  Se recomienda incentivar a los usuarios que se nieguen a dar permiso para acceder a la información de suscripción explicando las ventajas de la experiencia de usuario de inicio de sesión único de Apple, pero tenga en cuenta que el usuario puede cambiar su decisión yendo a la configuración de la aplicación (acceso de permiso del proveedor de TV) o a *`Settings -> TV Provider`* en iOS y iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS.

  La aplicación de transmisión por secuencias puede solicitar el permiso del usuario cuando la aplicación entra en el estado de primer plano, porque la aplicación puede comprobar el permiso [para tener acceso](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario en cualquier momento antes de requerir la autenticación del usuario.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
>
> * La aplicación de streaming ha completado los [requisitos previos de incorporación](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer) que se aplican a un programador y que son necesarios para habilitar la experiencia del usuario de inicio de sesión único de Apple.

### Flujo de trabajo {#workflow}

Realice los pasos dados para implementar el inicio de sesión único de Apple mediante flujos de socios como se muestra en el diagrama siguiente.

![Inicio de sesión único de Apple con flujos de socios](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*Inicio de sesión único de Apple con flujos de socios*

+++A. Fase de registro

1. **Recuperar credenciales de cliente:** La aplicación de flujo continuo recopila todos los datos necesarios para recuperar las credenciales de cliente llamando al extremo de registro de cliente.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar credenciales del cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `software_statement`
   > * Todos los _encabezados_ necesarios, como `Content-Type`, `X-Device-Info`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Devolver credenciales de cliente:** La respuesta de extremo de registro de cliente contiene información sobre las credenciales de cliente asociadas con los parámetros y encabezados recibidos.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar credenciales del cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) para obtener más información sobre la información proporcionada en una respuesta de credenciales de cliente.
   >
   > <br/>
   >
   > El registro de clientes valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de la API [Recuperar credenciales del cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error).

   >[!TIP]
   >
   > Las credenciales del cliente deben almacenarse en caché y utilizarse indefinidamente.

1. **Recuperar token de acceso:** La aplicación de streaming recopila todos los datos necesarios para recuperar el token de acceso llamando al extremo del token de cliente.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `client_id`, `client_secret` y `grant_type`
   > * Todos los _encabezados_ necesarios, como `Content-Type`, `X-Device-Info`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Devolver token de acceso:** La respuesta del extremo del token de cliente contiene información sobre el token de acceso asociado con los parámetros y encabezados recibidos.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) para obtener detalles sobre la información proporcionada en una respuesta de token de acceso.
   >
   > <br/>
   >
   > El token de cliente valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se adhiere a la documentación de la API [Recuperar token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error).

   >[!TIP]
   >
   > El token de acceso solo debe almacenarse en caché y utilizarse durante el tiempo especificado (por ejemplo, un período de vida de 24 horas). Una vez caducado, la aplicación de streaming debe solicitar un nuevo token de acceso.

+++

+++B. Comprobar fase de autenticación

1. **Recuperar el estado del marco de trabajo del socio:** La aplicación de transmisión llama al [marco de trabajo de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desarrollado por Apple para obtener permisos de usuario e información del proveedor.

   >[!IMPORTANT]
   >
   > Consulte la documentación de [Marco de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obtener detalles sobre:
   >
   > <br/>
   >
   > * La aplicación de streaming debe comprobar el permiso de [para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario y continuar solo si el usuario lo permite.
   > * La aplicación de streaming debe proporcionar un [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para `VSAccountManager`.
   > * La aplicación de streaming debe enviar una [solicitud](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obtener información de la cuenta del suscriptor.
   > * La aplicación de streaming debe esperar y procesar la información de [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > La aplicación de flujo continuo debe asegurarse de que especifica un valor booleano igual a `false` para la propiedad [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) en el objeto `VSAccountMetadataRequest`, para indicar que el usuario no se puede interrumpir en esta fase.

   >[!TIP]
   >
   > **<u>Sugerencia profesional:</u>** Siga el fragmento de código y preste especial atención a los comentarios.

   ```swift
   ...
   let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
   
   videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
   
                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
   
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
   
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
   
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
   
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Retrieve profiles" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Retrieve profiles" step.
                            ...
                        }
                    }
   
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Retrieve profiles" step.
                ...
            }
   }
   ...
   ```

1. **Devolver información de estado del marco de trabajo del socio:** La aplicación de flujo continuo valida los datos de respuesta para garantizar que se cumplan las condiciones básicas:
   * Se concede el estado de acceso al permiso de usuario.
   * El identificador de asignación del proveedor de usuarios está presente y es válido.
   * La fecha de caducidad del perfil del proveedor de usuarios (si está disponible) es válida.

1. **Recuperar perfiles:** La aplicación de transmisión recopila todos los datos necesarios para recuperar toda la información de perfiles enviando una solicitud al extremo de perfiles.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) para obtener más información sobre lo siguiente:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier` y `AP-Partner-Framework-Status`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   >
   > La aplicación de streaming debe asegurarse de que incluya un valor válido para el estado del marco de trabajo del socio, de modo que la respuesta recuperada pueda incluir un perfil de tipo &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Devolver información sobre los perfiles encontrados:** La respuesta del extremo de perfiles contiene información sobre los perfiles encontrados asociados con los parámetros y encabezados recibidos.

1. **Elija un perfil y continúe con los flujos de decisiones:** Si la respuesta del extremo de perfiles contiene perfiles, la aplicación de transmisión utiliza su lógica interna (finalmente interactuando con el usuario final) para elegir uno de los perfiles disponibles y continuar con los flujos de decisiones posteriores.

1. **Continúe con el flujo de autenticación del asociado:** Si la respuesta del extremo de perfiles no contiene un perfil, la aplicación de transmisión continúa con el flujo de autenticación del asociado.

+++

+++C. Fase de autenticación de socio

1. **Recuperar configuración:** La aplicación de streaming recopila todos los datos necesarios para recuperar la lista de MVPD que tienen una integración activa enviando una solicitud al extremo de configuración.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar configuración de un proveedor de servicios específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier` y `X-Device-Info`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Devolver configuración:** La respuesta del extremo de configuración contiene información sobre las MVPD que tienen una integración activa con el proveedor de servicios.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar la configuración de un proveedor de servicios específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) para obtener más información sobre la información proporcionada en una respuesta de configuración.
   >
   > <br/>
   >
   > El punto de conexión de configuración valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > La aplicación de streaming debe garantizar que procesa los siguientes detalles proporcionados para cada MVPD al continuar:
   >
   > * `enablePlatformServices`: indica si MVPD admite actualmente el inicio de sesión único de Apple.
   > * `displayInPlatformPicker`: indica si MVPD se puede mostrar en el selector de Apple.
   > * `boardingStatus`: indica si MVPD está incorporado en el inicio de sesión único de Apple.

1. **Recuperar el estado del marco de trabajo del socio:** La aplicación de transmisión llama al [marco de trabajo de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desarrollado por Apple para obtener permisos de usuario e información del proveedor.

   >[!IMPORTANT]
   >
   > Consulte la documentación de [Marco de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obtener detalles sobre:
   >
   > <br/>
   >
   > * La aplicación de streaming debe comprobar el permiso de [para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario y continuar solo si el usuario lo permite.
   > * La aplicación de streaming debe proporcionar un [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para `VSAccountManager`.
   > * La aplicación de streaming debe enviar una [solicitud](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obtener información de la cuenta del suscriptor.
   > * La aplicación de streaming debe esperar y procesar la información de [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > La aplicación de transmisión por secuencias debe asegurarse de que especifica un valor booleano igual a `true` para la propiedad [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) en el objeto `VSAccountMetadataRequest`, para indicar que el usuario puede ser interrumpido para seleccionar un proveedor de TV en esta fase.

   >[!TIP]
   >
   > **<u>Sugerencia profesional:</u>** Siga el fragmento de código y preste especial atención a los comentarios.

   ```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
   
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
   
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
   
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
   
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
   
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
   
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
   
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
   
                        // This can be computed from the Configuration service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
   
                        // This can be computed from the Configuration service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
   
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Retrieve partner authentication request" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
   
                                        // The requestor.mvpds must be computed during the "Return configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
   
                                        if mvpd != nil {
                                            // Continue with the "Proceed with basic authentication flow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Proceed with basic authentication flow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Proceed with basic authentication flow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Proceed with basic authentication flow" step.
                                    ...
                                }
                            }
                        }
   
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Proceed with basic authentication flow" step.
                    ...
                }
    }
    ...
   ```

1. **Devolver información de estado del marco de trabajo del socio:** La aplicación de flujo continuo valida los datos de respuesta para garantizar que se cumplan las condiciones básicas:
   * Se concede el estado de acceso al permiso de usuario.
   * El identificador de asignación del proveedor de usuarios está presente y es válido.
   * La fecha de caducidad del perfil del proveedor de usuarios (si está disponible) es válida.

1. **Recuperar solicitud de autenticación de socio:** La aplicación de streaming recopila todos los datos necesarios para iniciar una sesión de autenticación llamando al extremo de socio de sesiones.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar solicitud de autenticación de socio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `partner`
   > * Todos los _encabezados_ necesarios como `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` y `AP-Partner-Framework-Status`
   > * Todos los encabezados y parámetros _optional_
   >
   > <br/>
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el estado del marco de socio, de modo que la respuesta recuperada pueda incluir una solicitud de autenticación de socio (solicitud SAML).
   >
   > <br/>
   >
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Indique la siguiente acción:** La respuesta del extremo del asociado de sesiones contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar solicitud de autenticación de socio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) para obtener detalles sobre la información proporcionada en una respuesta de sesión.
   >
   > <br/>
   >
   > El punto de conexión del socio de sesiones valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   >
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > El punto de conexión del socio de sesiones valida los datos de solicitud para garantizar que se cumplen las condiciones de inicio de sesión único del socio:
   >
   >  * La configuración de inicio de sesión único de socio del servidor de Adobe Pass debe ser válida y estar habilitada.
   >  * La carga del estado del marco de trabajo del socio recibida a través del encabezado [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) debe ser válida.
   >
   > <br/>
   >
   > Si falla la validación del inicio de sesión único del socio, la respuesta se establecerá de forma predeterminada en el flujo de autenticación básico.

1. **Continúe con los flujos de decisiones:** La respuesta del extremo del asociado de sesiones contiene los siguientes datos:
   * El atributo `actionName` está establecido en &quot;authorize&quot;.
   * El atributo `actionType` está establecido en &quot;direct&quot;.

   Si el backend de Adobe Pass identifica un perfil válido, la aplicación de streaming no necesita volver a autenticarse con el MVPD seleccionado, ya que ya hay un perfil que se puede utilizar para flujos de decisiones posteriores.

1. **Continúe con el flujo de autenticación básico:** La respuesta del extremo del asociado de sesiones contiene los siguientes datos:
   * El atributo `actionName` se ha establecido en &quot;autenticar&quot; o &quot;reanudar&quot;.
   * El atributo `actionType` se ha establecido como &quot;interactivo&quot; o &quot;directo&quot;.

   Si el servidor de Adobe Pass no identifica un perfil válido y falla la validación del inicio de sesión único del socio, el servidor de Adobe Pass vuelve al flujo de autenticación básico.

   Para obtener más información sobre el flujo de autenticación básico, consulte los siguientes documentos:
   * [Realizar autenticación en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria con mvpd preseleccionado](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Continúe con la creación y recuperación de un perfil mediante el flujo de respuesta de autenticación del socio:** La respuesta del extremo del socio de sesiones contiene los siguientes datos:
   * El atributo `actionName` está establecido en &quot;partner_profile&quot;.
   * El atributo `actionType` está establecido en &quot;direct&quot;.
   * El atributo `authenticationRequest - type` incluye el protocolo de seguridad utilizado por el marco de trabajo del socio para el inicio de sesión de MVPD (actualmente establecido como solo SAML).
   * El atributo `authenticationRequest - request` incluye la solicitud SAML que se pasa al marco de trabajo del socio.
   * El atributo `authenticationRequest - attributesNames` incluye los atributos SAML que se pasan al marco de trabajo del socio.

   Si el backend de Adobe Pass no identifica un perfil válido y la validación del inicio de sesión único del socio pasa, la aplicación de streaming recibe una respuesta con acciones y datos para pasar al marco del socio para iniciar el flujo de autenticación con MVPD.

1. **Completar la autenticación de MVPD con el marco del socio:** Reenviar la solicitud de autenticación del socio (solicitud SAML) obtenida en el paso anterior al [marco de la cuenta del suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount). Si el flujo de autenticación es correcto, la interacción de [Marco de trabajo de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) con MVPD produce una respuesta de autenticación de socio (respuesta SAML) que se devuelve junto con la información de estado del marco de trabajo de socio.

   >[!IMPORTANT]
   >
   > Consulte la documentación de [Marco de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obtener detalles sobre:
   >
   > <br/>
   >
   > * La aplicación de streaming debe comprobar el permiso de [para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario y continuar solo si el usuario lo permite.
   > * La aplicación de streaming debe proporcionar un [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para `VSAccountManager`.
   > * La aplicación de streaming debe enviar una [solicitud](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) de información de cuenta de suscriptor y debe incluir la solicitud de autenticación de socio (solicitud SAML) obtenida en el paso anterior.
   > * La aplicación de streaming debe esperar y procesar la información de [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > La aplicación de transmisión por secuencias debe asegurarse de que especifica un valor booleano igual a `true` para la propiedad [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) en el objeto `VSAccountMetadataRequest`, para indicar que se puede interrumpir la autenticación del usuario con el proveedor de TV seleccionado en esta fase.

   >[!TIP]
   >
   > **<u>Sugerencia profesional:</u>** Siga el fragmento de código y preste especial atención a los comentarios.

   ```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
   
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
   
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
   
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
   
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
   
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
   
                        // This are the user metadata fields expected to be available on a successful login and are determined from the Sessions SSO service. Look for the authenticationRequest > attributesNames associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = attributesNames;
   
                        // This is the authenticationRequest > request field from Sessions SSO service.
                        vsaMetadataRequest.verificationToken = authenticationRequestPayload;
   
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
   
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
   
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
   
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
   
                                // Continue with the "Create and retrieve profile using partner authentication response" step.
                                ...
                            } else {
                                // Continue with the "Proceed with basic authentication flow" step.
                                ...
                            }
                        }
   
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Proceed with basic authentication flow" step.
                    ...
                }
    }
    ...
   ```

1. **Devolver respuesta de autenticación de socio:** La aplicación de transmisión valida los datos de respuesta para garantizar que se cumplan las condiciones básicas:
   * Se concede el estado de acceso al permiso de usuario.
   * El identificador de asignación del proveedor de usuarios está presente y es válido.
   * La fecha de caducidad del perfil del proveedor de usuarios (si está disponible) es válida.
   * La respuesta de autenticación del socio (respuesta SAML) está presente y es válida.

1. **Cree y recupere un perfil mediante la respuesta de autenticación del socio:** La aplicación de flujo continuo recopila todos los datos necesarios para crear y recuperar un perfil llamando al extremo del socio de perfiles.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Crear y recuperar perfiles mediante la respuesta de autenticación de socio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `partner` y `SAMLResponse`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` y `AP-Partner-Framework-Status`
   > * Todos los encabezados y parámetros _optional_
   >
   > <br/>
   >
   > La aplicación de streaming debe asegurarse de que incluya valores válidos para el estado del marco de trabajo del socio y la respuesta de autenticación del socio (respuesta SAML) de modo que la respuesta recuperada pueda incluir un perfil de tipo &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Devuelve información sobre el perfil del socio:** La respuesta del extremo de perfiles contiene información sobre el perfil del socio, incluido el atributo `type` establecido en &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Crear y recuperar perfiles mediante la respuesta de autenticación de socio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) para obtener más información sobre la información proporcionada en una respuesta de perfil.
   >
   > <br/>
   >
   > El punto de conexión del socio de perfiles valida los datos de la solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > El extremo de socio de perfiles valida los datos de solicitud para garantizar que se cumplen las condiciones de inicio de sesión único del socio:
   >
   >  * La configuración de inicio de sesión único de socio del servidor de Adobe Pass debe ser válida y estar habilitada.
   >  * La carga del estado del marco de trabajo del socio recibida a través del encabezado [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) debe ser válida.
   >
   > <br/>
   >
   > Si falla la validación del inicio de sesión único del socio, la respuesta se establecerá de forma predeterminada en el flujo básico de recuperación de perfiles.

1. **Continúe con los flujos de decisiones:** La aplicación de transmisión puede continuar con los flujos de decisiones subsiguientes.

+++

+++ D. Fase de decisión

1. **Recuperar el estado del marco de trabajo del socio:** La aplicación de transmisión llama al [marco de trabajo de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desarrollado por Apple para obtener permisos de usuario e información del proveedor.

   >[!IMPORTANT]
   > 
   > La aplicación de streaming puede omitir este paso si el tipo de perfil de usuario seleccionado no es &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte la documentación de [Marco de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obtener detalles sobre:
   >
   > <br/>
   >
   > * La aplicación de streaming debe comprobar el permiso de [para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario y continuar solo si el usuario lo permite.
   > * La aplicación de streaming debe proporcionar un [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para `VSAccountManager`.
   > * La aplicación de streaming debe enviar una [solicitud](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obtener información de la cuenta del suscriptor.
   > * La aplicación de streaming debe esperar y procesar la información de [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > La aplicación de flujo continuo debe asegurarse de que especifica un valor booleano igual a `false` para la propiedad [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) en el objeto `VSAccountMetadataRequest`, para indicar que el usuario no se puede interrumpir en esta fase.

   >[!TIP]
   >
   > La aplicación de streaming puede utilizar en su lugar un valor almacenado en caché para la información de estado del marco de trabajo del socio, que se recomienda actualizar cuando la aplicación pase del estado en segundo plano al estado en primer plano. En ese caso, la aplicación de streaming debe asegurarse de que almacena en caché y utiliza solo valores válidos para el estado del marco de trabajo de socio tal como se describe en el paso Devolver información de estado del marco de trabajo de socio.

1. **Devolver información de estado del marco de trabajo del socio:** La aplicación de flujo continuo valida los datos de respuesta para garantizar que se cumplan las condiciones básicas:
   * Se concede el estado de acceso al permiso de usuario.
   * El identificador de asignación del proveedor de usuarios está presente y es válido.
   * La fecha de caducidad del perfil del proveedor de usuarios es válida.

   >[!IMPORTANT]
   >
   > La aplicación de streaming puede omitir este paso si el tipo de perfil de usuario seleccionado no es &quot;appleSSO&quot;.

1. **Recuperar decisiones de preautorización:** La aplicación de streaming recopila todos los datos necesarios para obtener decisiones de preautorización para una lista de recursos llamando al extremo Decisions Preauthorize.

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de preautorización utilizando la documentación específica de la API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el estado de la estructura del socio antes de realizar una solicitud más adelante, cuando el perfil elegido es un perfil de tipo &quot;appleSSO&quot;. Sin embargo, este paso se puede omitir si el tipo de perfil de usuario seleccionado no es &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Devolver decisiones de preautorización:** La respuesta de extremo de preautorización de Decisions contiene una decisión `Permit` o `Deny` para cada recurso:
   * Una decisión `Permit` significa que el recurso se puede reproducir. La respuesta no incluye un token de medios, ya que el flujo de preautorización no debe utilizarse para reproducir recursos.
   * Una decisión `Deny` significa que el recurso no se puede reproducir. La respuesta incluye una carga de error que se adhiere a la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de preautorización utilizando la documentación específica de la API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) para obtener más información sobre la información proporcionada en una respuesta de decisión.
   >
   > <br/>
   >
   > El extremo de preautorización de Decisions valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

1. **Recuperar el estado del marco de trabajo del socio:** La aplicación de transmisión llama al [marco de trabajo de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desarrollado por Apple para obtener permisos de usuario e información del proveedor.

   >[!IMPORTANT]
   >
   > La aplicación de streaming puede omitir este paso si el tipo de perfil de usuario seleccionado no es &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte la documentación de [Marco de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obtener detalles sobre:
   >
   > <br/>
   >
   > * La aplicación de streaming debe comprobar el permiso de [para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario y continuar solo si el usuario lo permite.
   > * La aplicación de streaming debe proporcionar un [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para `VSAccountManager`.
   > * La aplicación de streaming debe enviar una [solicitud](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obtener información de la cuenta del suscriptor.
   > * La aplicación de streaming debe esperar y procesar la información de [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > La aplicación de flujo continuo debe asegurarse de que especifica un valor booleano igual a `false` para la propiedad [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) en el objeto `VSAccountMetadataRequest`, para indicar que el usuario no se puede interrumpir en esta fase.

   >[!TIP]
   >
   > La aplicación de streaming puede utilizar en su lugar un valor almacenado en caché para la información de estado del marco de trabajo del socio, que se recomienda actualizar cuando la aplicación pase del estado en segundo plano al estado en primer plano. En ese caso, la aplicación de streaming debe asegurarse de que almacena en caché y utiliza solo valores válidos para el estado del marco de trabajo de socio tal como se describe en el paso Devolver información de estado del marco de trabajo de socio.

1. **Devolver información de estado del marco de trabajo del socio:** La aplicación de flujo continuo valida los datos de respuesta para garantizar que se cumplan las condiciones básicas:
   * Se concede el estado de acceso al permiso de usuario.
   * El identificador de asignación del proveedor de usuarios está presente y es válido.
   * La fecha de caducidad del perfil del proveedor de usuarios es válida.

   >[!IMPORTANT]
   >
   > La aplicación de streaming puede omitir este paso si el tipo de perfil de usuario seleccionado no es &quot;appleSSO&quot;.

1. **Recuperar decisión de autorización:** La aplicación de streaming recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el estado de la estructura del socio antes de realizar una solicitud más adelante, cuando el perfil elegido es un perfil de tipo &quot;appleSSO&quot;. Sin embargo, este paso se puede omitir si el tipo de perfil de usuario seleccionado no es &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Decisión de autorización de devolución:** La respuesta de extremo de autorización de decisiones contiene una decisión `Permit` o `Deny` para el recurso específico:
   * Una decisión `Permit` significa que el recurso se puede reproducir. La respuesta incluye un token de medios.
   * Una decisión `Deny` significa que el recurso no se puede reproducir. La respuesta incluye una carga de error que se adhiere a la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar decisiones de autorización utilizando mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) específica para obtener más información sobre la información proporcionada en una respuesta de decisión.
   >
   > <br/>
   >
   > El punto de conexión de autorización de decisiones valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

+++

+++ D. Fase de cierre

1. **Iniciar el cierre de sesión de Adobe Pass:** La aplicación de flujo continuo recopila todos los datos necesarios para iniciar el flujo de cierre de sesión llamando al extremo de cierre de sesión de Adobe Pass.

   >[!IMPORTANT]
   >
   > Consulte la [Iniciar cierre de sesión para obtener documentación específica de la API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) para obtener más información sobre lo siguiente:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `redirectUrl`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Indique la siguiente acción:** La respuesta del extremo de cierre de sesión de Adobe Pass contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción:
   * Falta el atributo `url` porque el usuario debe interactuar con el nivel de socio (sistema) para completar el flujo de cierre de sesión.
   * El atributo `actionName` está establecido en &quot;partner_logout&quot;.
   * El atributo `actionType` está establecido en &quot;partner_interactive&quot;.

   >[!IMPORTANT]
   >
   > La aplicación de flujo continuo debe pedir al usuario que complete el proceso de cierre de sesión en el nivel de socio, tal como especifican los atributos `actionName` y `actionType`, cuando el tipo de perfil de usuario eliminado sea &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte la [Iniciar cierre de sesión para obtener documentación específica de la API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) para obtener más información sobre la información proporcionada en una respuesta de cierre de sesión.
   >
   > <br/>
   >
   > El extremo de cierre de sesión de Adobe Pass valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

+++
