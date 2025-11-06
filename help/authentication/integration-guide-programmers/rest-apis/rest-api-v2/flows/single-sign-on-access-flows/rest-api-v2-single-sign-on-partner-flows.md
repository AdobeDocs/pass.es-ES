---
title: Inicio de sesión único - Socio - Flujos
description: API de REST V2 - Inicio de sesión único - Socio - Flujos
exl-id: 5735d67f-a311-4d03-ad48-93c0fcbcace5
source-git-commit: d8097b8419aa36140e6ff550714730059555fd14
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# Inicio de sesión único con flujos de socios {#single-sign-on-partner-flows}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Asegúrese de visitar también las [Preguntas frecuentes sobre la API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

El método Partner permite que varias aplicaciones utilicen una carga útil de estado de marco de socio para lograr el inicio de sesión único (SSO) en el nivel de dispositivo al utilizar los servicios de Adobe Pass.

Las aplicaciones son responsables de recuperar la carga útil del estado del marco de trabajo de socio mediante marcos o bibliotecas específicos del socio fuera de los sistemas de Adobe Pass.

Las aplicaciones son responsables de incluir esta carga útil de estado de marco de socio como parte del encabezado `AP-Partner-Framework-Status` para todas las solicitudes que lo especifican.

Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

La API de REST de autenticación de Adobe Pass V2 es compatible con el inicio de sesión único (SSO) de socio para usuarios finales de aplicaciones cliente que se ejecutan en iOS, iPadOS o tvOS.

Para obtener más información sobre el inicio de sesión único (SSO) para Apple Platform, consulte la [Guía de Apple SSO (API de REST V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md).

## Recuperar solicitud de autenticación de socio {#retrieve-partner-authentication-request}

### Requisitos previos {#prerequisites-retrieve-partner-authentication-request}

Antes de recuperar la solicitud de autenticación del socio, asegúrese de que se cumplan los siguientes requisitos previos:

* El marco de socios debe seleccionar un MVPD.
* La aplicación de streaming debe obtener la información del estado de la estructura del socio desde la estructura del socio y pasarla al servidor de Adobe Pass.
* La aplicación de streaming debe obtener la solicitud de autenticación del socio del servidor de Adobe Pass y pasarla al marco del socio.

>[!IMPORTANT]
>
> Suposiciones
> 
> <br/>
> 
> * El marco de trabajo de socios admite la interacción del usuario para seleccionar un MVPD.
> * El marco de socios admite la interacción del usuario para autenticarse con el MVPD seleccionado.
> * El marco de socios proporciona permisos de usuario e información del proveedor.

### Flujo de trabajo {#workflow-retrieve-partner-authentication-request}

Realice los pasos dados para recuperar la solicitud de autenticación de socio como se muestra en el diagrama siguiente.

![Recuperar solicitud de autenticación de socio](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-partner-authentication-request-flow.png)

*Recuperar solicitud de autenticación de socio*

1. **Recuperar el estado del marco del socio:** La aplicación de transmisión llama al marco del socio, fuera de los sistemas Adobe Pass, para obtener permisos de usuario e información del proveedor.

1. **Devolver información de estado del marco de trabajo del socio:** La aplicación de flujo continuo valida los datos de respuesta para garantizar que se cumplan las condiciones básicas:
   * Se concede el estado de acceso al permiso de usuario.
   * El identificador de asignación del proveedor de usuarios está presente y es válido.
   * La fecha de caducidad del perfil del proveedor de usuarios (si está disponible) es válida.

1. **Recuperar solicitud de autenticación de socio:** La aplicación de streaming recopila todos los datos necesarios para iniciar una sesión de autenticación llamando al extremo de socio de sesiones.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar solicitud de autenticación de socio](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `partner`
   > * Todos los _encabezados_ necesarios como `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` y `AP-Partner-Framework-Status`
   > * Todos los encabezados y parámetros _optional_
   >
   > <br/>
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el estado del marco de trabajo del socio antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Indique la siguiente acción:** La respuesta del extremo del asociado de sesiones contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar solicitud de autenticación de socio](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) para obtener detalles sobre la información proporcionada en una respuesta de sesión.
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
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > El punto de conexión del socio de sesiones valida los datos de solicitud para garantizar que se cumplen las condiciones de inicio de sesión único del socio:
   >
   >  * La configuración de inicio de sesión único de socio del servidor de Adobe Pass debe ser válida y estar habilitada.
   >  * La carga del estado del marco de trabajo del socio recibida a través del encabezado [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) debe ser válida.
   >
   > <br/>
   >
   > Si falla la validación del inicio de sesión único del socio, la respuesta se establecerá de forma predeterminada en el flujo de autenticación básico.

1. **Continúe con el flujo de recuperación de perfiles mediante la respuesta de autenticación del socio:** La respuesta del extremo del socio de sesiones contiene los siguientes datos:
   * El atributo `actionName` está establecido en &quot;partner_profile&quot;.
   * El atributo `actionType` está establecido en &quot;direct&quot;.
   * El atributo `authenticationRequest - type` incluye el protocolo de seguridad utilizado por el marco de trabajo del socio para el inicio de sesión de MVPD (actualmente establecido como solo SAML).
   * El atributo `authenticationRequest - request` incluye la solicitud SAML que se pasa al marco de trabajo del socio.
   * El atributo `authenticationRequest - attributesNames` incluye los atributos SAML que se pasan al marco de trabajo del socio.

   Si el backend de Adobe Pass no identifica un perfil válido y la validación del inicio de sesión único del socio pasa, la aplicación de streaming recibe una respuesta con acciones y datos para pasar al marco del socio para iniciar el flujo de autenticación con MVPD.

   Para obtener más información sobre el flujo de recuperación de perfiles mediante una respuesta de autenticación de socio, consulte la sección [Crear y recuperar perfiles mediante una respuesta de autenticación de socio](#create-and-retrieve-profile-using-partner-authentication-response).

1. **Continúe con el flujo de autenticación básico:** La respuesta del extremo del asociado de sesiones contiene los siguientes datos:
   * El atributo `actionName` se ha establecido en &quot;autenticar&quot; o &quot;reanudar&quot;.
   * El atributo `actionType` se ha establecido como &quot;interactivo&quot; o &quot;directo&quot;.

   Si el servidor de Adobe Pass no identifica un perfil válido y falla la validación del inicio de sesión único del socio, el servidor de Adobe Pass vuelve al flujo de autenticación básico.

   Para obtener más información sobre el flujo de autenticación básico, consulte los siguientes documentos:
   * [Realizar autenticación en la aplicación principal](../basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria con mvpd preseleccionado](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Continúe con los flujos de decisiones:** La respuesta del extremo del asociado de sesiones contiene los siguientes datos:
   * El atributo `actionName` está establecido en &quot;authorize&quot;.
   * El atributo `actionType` está establecido en &quot;direct&quot;.

   Si el backend de Adobe Pass identifica un perfil válido, la aplicación de streaming no necesita volver a autenticarse con el MVPD seleccionado, ya que ya hay un perfil que se puede utilizar para flujos de decisiones posteriores.

   >[!IMPORTANT]
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el estado del marco de trabajo del socio antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

## Crear y recuperar perfiles mediante una respuesta de autenticación de socio {#create-and-retrieve-profile-using-partner-authentication-response}

### Requisitos previos {#prerequisites-create-and-retrieve-profile-using-partner-authentication-response}

Antes de recuperar el perfil mediante una respuesta de autenticación de socio, asegúrese de que se cumplan los siguientes requisitos previos:

* El marco de socio debe realizar la autenticación con el MVPD seleccionado.
* La aplicación de streaming debe obtener la respuesta de autenticación del socio junto con la información de estado de la estructura del socio desde la estructura del socio y pasarla al servidor de Adobe Pass.

>[!IMPORTANT]
>
> Suposición
>
> * El marco de trabajo de socios admite la interacción del usuario para seleccionar un MVPD.
> * El marco de socios admite la interacción del usuario para autenticarse con el MVPD seleccionado.
> * El marco de socios proporciona permisos de usuario e información del proveedor.

### Flujo de trabajo {#workflow-create-and-retrieve-profile-using-partner-authentication-response}

Realice los pasos dados para implementar el flujo de recuperación de perfiles mediante una respuesta de autenticación de socio como se muestra en el diagrama siguiente.

![Crear y recuperar perfiles mediante la respuesta de autenticación del socio](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-profile-using-partner-authentication-response-flow.png)

*Crear y recuperar un perfil autenticado mediante la respuesta de autenticación del socio*

1. **Autenticación de MVPD completa con el marco del socio:** Si el flujo de autenticación es correcto, la interacción del marco del socio con MVPD produce una respuesta de autenticación del socio (respuesta SAML) que se devuelve junto con la información de estado del marco del socio.

1. **Devolver respuesta de autenticación de socio:** La aplicación de transmisión valida los datos de respuesta para garantizar que se cumplan las condiciones básicas:
   * Se concede el estado de acceso al permiso de usuario.
   * El identificador de asignación del proveedor de usuarios está presente y es válido.
   * La fecha de caducidad del perfil del proveedor de usuarios (si está disponible) es válida.

1. **Cree y recupere un perfil mediante la respuesta de autenticación del socio:** La aplicación de flujo continuo recopila todos los datos necesarios para crear y recuperar un perfil llamando al extremo del socio de perfiles.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Crear y recuperar perfiles mediante la respuesta de autenticación de socio](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `partner` y `SAMLResponse`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` y `AP-Partner-Framework-Status`
   > * Todos los encabezados y parámetros _optional_
   >
   > <br/>
   > 
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el estado del marco de trabajo del socio antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Cree y guarde un perfil de socio:** El servidor de Adobe Pass crea y guarda un perfil de socio después de asegurarse de que se cumplen todas las condiciones.

1. **Devuelve información sobre el perfil del socio:** La respuesta del extremo de perfiles contiene información sobre el perfil del socio, incluido el atributo `type` establecido en &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Crear y recuperar perfiles mediante la respuesta de autenticación de socio](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) para obtener más información sobre la información proporcionada en una respuesta de perfil.
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
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > El extremo de socio de perfiles valida los datos de solicitud para garantizar que se cumplen las condiciones de inicio de sesión único del socio:
   >
   >  * La configuración de inicio de sesión único de socio del servidor de Adobe Pass debe ser válida y estar habilitada.
   >  * La carga del estado del marco de trabajo del socio recibida a través del encabezado [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) debe ser válida.
   >
   > <br/>
   >
   > Si falla la validación del inicio de sesión único del socio, la respuesta se establecerá de forma predeterminada en el flujo básico de recuperación de perfiles.

1. **Continúe con los flujos de decisiones:** La aplicación de transmisión puede continuar con los flujos de decisiones subsiguientes.

   >[!IMPORTANT]
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el estado del marco de trabajo del socio antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AP-Partner-Framework-Status`, consulte la documentación de [AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).
