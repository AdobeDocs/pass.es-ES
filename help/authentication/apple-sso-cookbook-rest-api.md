---
title: Guía de Apple SSO (API de REST)
description: Guía de Apple SSO (API de REST)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---

# Guía de Apple SSO (API de REST) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#Introduction}

La API de REST de autenticación de Adobe Pass puede admitir la autenticación de inicio de sesión único (SSO) de la plataforma para los usuarios finales de aplicaciones cliente que se ejecutan en iOS, iPadOS o tvOS a través de lo que llamamos flujo de trabajo de SSO de Apple.

Tenga en cuenta que este documento actúa como una extensión de la documentación de la API de REST existente, que se puede encontrar [aquí](/help/authentication/rest-api-reference.md).

## Libros {#Cookbooks}

Para beneficiarse de la experiencia del usuario de SSO de Apple, una aplicación necesitaría integrar la plataforma de [cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desarrollada por Apple, mientras que en cuanto a la comunicación de la API de REST de autenticación de Adobe Pass, tendría que seguir la secuencia de sugerencias presentadas a continuación.

### Autenticación {#Authentication}

- [¿Hay un token de autenticación de Adobe válido?](#Is_there_a_valid_Adobe_authentication_token)
- [¿El usuario ha iniciado sesión mediante el SSO de Platform?](#Is_the_user_logged_in_via_Platform_SSO)
- [Recuperar configuración de Adobe](#Fetch_Adobe_configuration)
- [Inicio del flujo de trabajo de Platform SSO con configuración de Adobe](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [¿El usuario ha iniciado sesión correctamente?](#Is_user_login_successful)
- [Obtener una solicitud de perfil del Adobe para la MVPD seleccionada](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [Reenviar la solicitud de Adobe a Platform SSO para obtener el perfil](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Intercambio del perfil SSO de Platform por un token de autenticación de Adobe](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [¿Se ha generado correctamente el token de Adobe?](#Is_Adobe_token_generated_successfully)
- [Iniciar flujo de trabajo de autenticación de segunda pantalla](#Initiate_second_screen_authentication_workflow)
- [Continuar con flujos de autorización](#Proceed_with_authorization_flows)


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)


#### Paso: &quot;¿Hay un token de autenticación de Adobe válido?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del servicio [Autenticación de Adobe Pass](/help/authentication/check-authentication-token.md).


#### Paso: &quot;¿El usuario ha iniciado sesión mediante el SSO de Platform?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del marco de [cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount).

- La aplicación tendría que comprobar si tiene [permiso para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario y continuar solo si el usuario lo permite.
- La aplicación tendría que enviar una [solicitud](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obtener información de la cuenta del suscriptor.
- La aplicación tendría que esperar y procesar la información de [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

#### Paso: &quot;Recuperar configuración de Adobe&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del servicio [Autenticación de Adobe Pass](/help/authentication/provide-mvpd-list.md).


>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Tenga en cuenta las propiedades de MVPD: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* y preste especial atención a los comentarios presentados en fragmentos de código de otros pasos.

#### Paso &quot;Inicio del flujo de trabajo de SSO de Platform con la configuración de Adobe&quot; {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del marco de [cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount).

- La aplicación tendría que comprobar si tiene [permiso para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario y continuar solo si el usuario lo permite.
- La aplicación tendría que proporcionar un [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para VSAccountManager.
- La aplicación tendría que enviar una [solicitud](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obtener información de la cuenta del suscriptor.
- La aplicación tendría que esperar y procesar la información de [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
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
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Paso: &quot;¿El inicio de sesión del usuario se ha realizado correctamente?&quot; {#Is_user_login_successful}

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Tenga en cuenta el fragmento de código del paso [&quot;Iniciar flujo de trabajo de SSO de Platform con configuración de Adobe&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config). El inicio de sesión del usuario se realiza correctamente en el caso de que *`vsaMetadata!.accountProviderIdentifier`* contenga un valor válido y la fecha actual no haya pasado el valor *`vsaMetadata!.authenticationExpirationDate`*.

#### Paso &quot;Obtener una solicitud de perfil del Adobe para la MVPD seleccionada&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del servicio de autenticación de Adobe Pass [Solicitud de perfil](/help/authentication/retrieve-profilerequest.md).

>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Tenga en cuenta que el identificador de proveedor obtenido del módulo de cuenta de suscriptor de vídeo representa a *`platformMappingId`* en términos de la configuración de autenticación de Adobe Pass. Por lo tanto, la aplicación debe determinar el valor de la propiedad MVPD id, usando el valor *`platformMappingId`*, a través del medio del servicio Autenticación de Adobe Pass [Proporcionar lista MVPD](/help/authentication/provide-mvpd-list.md).

#### Paso: &quot;Reenviar la solicitud de Adobe a Platform SSO para obtener el perfil&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del marco de [cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount).


- La aplicación tendría que comprobar si tiene [permiso para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario y continuar solo si el usuario lo permite.
- La aplicación tendría que enviar una [solicitud](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obtener información de la cuenta del suscriptor.
- La aplicación tendría que esperar y procesar la información de [metadata](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



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
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
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
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Paso: &quot;Intercambio del perfil SSO de Platform por un token de autenticación de Adobe&quot; {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del servicio de autenticación de Adobe Pass [Intercambio de tokens](/help/authentication/token-exchange.md).


>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Tenga en cuenta el fragmento de código de [&quot;Reenvíe la solicitud de Adobe a Platform SSO para obtener el paso del perfil&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile). Este(a) *`vsaMetadata!.samlAttributeQueryResponse!`* representa el(la) *`SAMLResponse`*, que necesita pasarse en [Intercambio de tokens](/help/authentication/token-exchange.md) y requiere manipulación de cadenas y codificación (*Codificado(a) Base64* y codificado(a) *URL* posteriormente) antes de realizar la llamada.

</br>

#### Paso: &quot;¿El token de Adobe se ha generado correctamente?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del medio de respuesta correcta de autenticación de Adobe Pass [Intercambio de tokens](/help/authentication/token-exchange.md), que será una respuesta de *`204 No Content`*, lo que indica que el token se creó correctamente y está listo para utilizarse en los flujos de autorización.

</br>

#### Paso: &quot;Iniciar flujo de trabajo de autenticación de segunda pantalla&quot; {#Initiate_second_screen_authentication_workflow}

**Importante:** La terminología &quot;Flujo de trabajo de autenticación de segunda pantalla&quot; es apropiada para AppleTV, mientras que la terminología &quot;Flujo de trabajo de autenticación de primera pantalla&quot; / &quot;Flujo de trabajo de autenticación regular&quot; sería más apropiada para iPhone y iPads.


>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del medio de autenticación de Adobe Pass

[Solicitud de código de registro](/help/authentication/registration-code-request.md), [Iniciar autenticación](/help/authentication/initiate-authentication.md) y [API REST Recuperar token de autenticación](/help/authentication/retrieve-authentication-token.md) o [Comprobar servicios de token de autenticación](/help/authentication/check-authentication-token.md).


>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Siga los pasos descritos a continuación para las implementaciones de tvOS.

- La aplicación tendría que [obtener un código de registro](/help/authentication/registration-code-request.md) y presentarlo al usuario final en el primer dispositivo (pantalla).
- La aplicación tendría que iniciar [sondeo para confirmar el estado de autenticación](/help/authentication/retrieve-authentication-token.md) en el primer dispositivo (pantalla) después de obtener el código de registro.
- Otra aplicación tendría que [iniciar autenticación](/help/authentication/initiate-authentication.md) en un segundo dispositivo (pantalla) cuando se use el código de registro.
- La aplicación tendría que detener [el sondeo](/help/authentication/retrieve-authentication-token.md) en el primer dispositivo (pantalla) cuando se genere el token de autenticación.



>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Siga los pasos descritos a continuación para las implementaciones de iOS/iPadOS.

- La aplicación tendría que [obtener un código de registro](/help/authentication/registration-code-request.md) que no debería presentarse al usuario final en el primer dispositivo (pantalla).
- La aplicación tendría que [iniciar la autenticación](/help/authentication/initiate-authentication.md) en el primer dispositivo (pantalla) usando el código de registro y un componente [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
- La aplicación tendría que iniciar [sondeo para conocer el estado de autenticación](/help/authentication/retrieve-authentication-token.md) en el primer dispositivo (pantalla) después de que se cierre [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o el componente [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
- La aplicación tendría que detener [el sondeo](/help/authentication/retrieve-authentication-token.md) en el primer dispositivo (pantalla) cuando se genere el token de autenticación.

</br>

#### Paso: &quot;Continuar con los flujos de autorización&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del medio de autenticación de Adobe Pass [Iniciar autorización](/help/authentication/initiate-authorization.md) y [Obtener servicios de token de medios cortos](/help/authentication/obtain-short-media-token.md).

</br>

### Cerrar sesión {#Logout}

El marco de trabajo de la cuenta de suscriptor de vídeo [Video Subscriber Account](https://developer.apple.com/documentation/videosubscriberaccount) no proporciona una API para cerrar la sesión mediante programación de las personas que han iniciado sesión en su cuenta de proveedor de TV en el nivel del sistema del dispositivo. Por lo tanto, para que el cierre de sesión surta efecto, el usuario final tendría que cerrar sesión explícitamente desde *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS. La otra opción que tendría el usuario es retirar el permiso para acceder a la información de suscripción del usuario desde la sección de configuración específica de la aplicación (acceso al proveedor de TV).

>[!TIP]
>
> **<u>Sugerencia:</u>** Implemente esto a través del medio de la autenticación de Adobe Pass [llamada de metadatos de usuario](/help/authentication/user-metadata.md) y los servicios [Cerrar sesión](/help/authentication/initiate-logout.md).


>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Siga los pasos descritos a continuación para las implementaciones de tvOS.


- La aplicación tendría que determinar si la autenticación se ha producido como resultado de un inicio de sesión a través del SSO de la plataforma o no, utilizando los &quot;*tokenSource&quot;* [metadatos de usuario](/help/authentication/user-metadata.md) del servicio de autenticación de Adobe Pass.
- La aplicación tendría que indicar o pedir al usuario que cierre sesión explícitamente desde *`Settings -> Accounts -> TV Provider`* en tvOS **solo** en caso de que el valor *&quot;tokenSource&quot;* sea igual a &quot;*Apple&quot;.*
- La aplicación tendría que [iniciar el cierre de sesión](/help/authentication/initiate-logout.md) desde el servicio de autenticación de Adobe Pass usando una llamada HTTP directa. Esto no facilitaría la limpieza de la sesión en el lado de MVPD.



>[!TIP]
>
> **<u>Sugerencia profesional:</u>** Siga los pasos descritos a continuación para las implementaciones de iOS/iPadOS.

- La aplicación tendría que determinar si la autenticación se ha producido como resultado de un inicio de sesión a través del SSO de la plataforma o no, utilizando los &quot;*tokenSource&quot;* [metadatos de usuario](/help/authentication/user-metadata.md) del servicio de autenticación de Adobe Pass.
- La aplicación tendría que indicar o pedir al usuario que cierre sesión explícitamente desde *`Settings -> TV Provider`* en iOS/iPadOS **solo** en caso de que el valor *&quot;tokenSource&quot;* sea igual a *&quot;Apple&quot;*.
- La aplicación tendría que [iniciar el cierre de sesión](/help/authentication/initiate-logout.md) desde el servicio de autenticación de Adobe Pass usando un [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) o un componente [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller). Esto facilitaría la limpieza de la sesión en el lado de MVPD.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
