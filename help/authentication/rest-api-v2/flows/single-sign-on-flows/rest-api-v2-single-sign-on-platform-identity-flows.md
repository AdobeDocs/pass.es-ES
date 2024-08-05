---
title: 'Inicio de sesión único: identidad de plataforma: flujos'
description: 'API de REST V2: inicio de sesión único, identidad de plataforma, flujos'
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 0%

---


# Inicio de sesión único con flujos de identidad de plataforma {#single-sign-on-platform-identity-full-flows}

El método de identidad de plataforma permite que varias aplicaciones utilicen un identificador de plataforma único para lograr el inicio de sesión único (SSO) en el nivel de dispositivo o plataforma al utilizar los servicios de Adobe Pass.

Las aplicaciones son responsables de recuperar la carga útil del identificador de plataforma único mediante servicios de identidad o bibliotecas específicos del dispositivo fuera de los sistemas de Adobe Pass.

Las aplicaciones son responsables de incluir esta carga útil de identificador de plataforma única como parte del encabezado `Adobe-Subject-Token` para todas las solicitudes que lo especifican.

Para obtener más información sobre el encabezado `Adobe-Subject-Token`, consulte la documentación de [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

## Realizar autenticación mediante el inicio de sesión único mediante la identidad de la plataforma {#perform-authentication-through-single-sign-on-using-platform-identity}

### Requisitos previos {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

Antes de realizar el flujo de autenticación mediante el inicio de sesión único mediante una identidad de plataforma, asegúrese de que se cumplan los siguientes requisitos previos:

* La plataforma debe proporcionar un servicio de identidad o una biblioteca que devuelva información coherente como `JWS` o `JWE` carga útil en todas las aplicaciones del mismo dispositivo o plataforma.
* La primera aplicación de streaming debe recuperar el identificador de plataforma único e incluir la carga útil `JWS` o `JWE` como parte del encabezado [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) para todas las solicitudes que lo especifiquen.
* La primera aplicación de streaming debe seleccionar una MVPD.
* La primera aplicación de streaming debe iniciar una sesión de autenticación para iniciar sesión con la MVPD seleccionada.
* La primera aplicación de streaming debe autenticarse con la MVPD seleccionada en un agente de usuario.
* La segunda aplicación de streaming debe recuperar el identificador de plataforma único e incluir la carga útil `JWS` o `JWE` como parte del encabezado [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) para todas las solicitudes que lo especifiquen.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * La primera aplicación de streaming admite la interacción del usuario para seleccionar una MVPD.
> * La primera aplicación de streaming admite la interacción del usuario para autenticarse con la MVPD seleccionada en un agente de usuario.

### Flujo de trabajo {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

Realice los pasos dados para implementar el flujo de autenticación mediante el inicio de sesión único utilizando una identidad de plataforma como se muestra en el diagrama siguiente.

![Realizar autenticación mediante el inicio de sesión único mediante la identidad de la plataforma](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*Realizar autenticación mediante el inicio de sesión único mediante la identidad de la plataforma*

1. **Recuperar identificador de plataforma:** La primera aplicación de transmisión llama al servicio o biblioteca de identidad, fuera de los sistemas de Adobe Pass, para obtener la carga `JWS` o `JWE` asociada al identificador de plataforma único.

1. **Devuelve un identificador de plataforma único como JWS o JWE:** La primera aplicación de streaming valida los datos de respuesta para garantizar que se cumplan las condiciones básicas de seguridad:
   * La carga útil no ha caducado.
   * La carga útil está firmada o cifrada.

1. **Crear sesión de autenticación:** La primera aplicación de flujo continuo recopila todos los datos necesarios para iniciar una sesión de autenticación llamando al extremo de sesiones.

   Consulte la documentación de la API [Crear sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obtener detalles sobre:
   * Todos los parámetros _requeridos_, como `serviceProvider`, `mvpd`, `domainName` y `redirectUrl`
   * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

   >[!IMPORTANT]
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el identificador de plataforma único antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `Adobe-Subject-Token`, consulte la documentación de [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Indique la siguiente acción:** La respuesta del extremo de sesiones contiene los datos necesarios para guiar la primera aplicación de flujo continuo con respecto a la siguiente acción.

   Consulte la documentación de la API [Crear sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obtener más información sobre la información proporcionada en una respuesta de sesión.

   >[!IMPORTANT]
   >
   > El punto final de sesiones valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Abrir URL en el agente de usuario:** La respuesta del extremo de sesiones contiene los siguientes datos:
   * `url` que se puede usar para iniciar la autenticación interactiva en la página de inicio de sesión de MVPD.
   * El atributo `actionName` está establecido en &quot;autenticar&quot;.
   * El atributo `actionType` está establecido en &quot;interactivo&quot;.

   Si el servidor de Adobe Pass no identifica un perfil válido, la primera aplicación de flujo continuo abre un agente de usuario para cargar el `url` proporcionado, realizando una solicitud al extremo Authenticate. Este flujo puede incluir varias redirecciones, lo que finalmente lleva al usuario a la página de inicio de sesión de MVPD y proporciona credenciales válidas.

1. **Autenticación MVPD completa:** Si el flujo de autenticación es correcto, la interacción del agente de usuario guarda un perfil normal en el servidor de Adobe Pass y alcanza el valor de `redirectUrl` proporcionado.

1. **Recuperar perfil para código específico:** La primera aplicación de flujo continuo recopila todos los datos necesarios para recuperar información de perfil enviando una solicitud al extremo de perfiles.

   Consulte la documentación de la API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) para obtener detalles sobre:
   * Todos los _parámetros necesarios_, como `serviceProvider`, `code`
   * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

   >[!NOTE]
   >
   > Sugerencia: la aplicación de streaming puede esperar a que el agente de usuario alcance el `redirectUrl` proporcionado para comprobar si el perfil normal se generó y guardó correctamente.

1. **Buscar perfil normal:** El servidor de Adobe Pass identifica un perfil válido en función de los parámetros y encabezados recibidos.

1. **Devuelve información sobre el perfil normal:** La respuesta del extremo de perfiles contiene información sobre el perfil encontrado asociado a los parámetros y encabezados recibidos.

   Consulte la documentación de la API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) para obtener detalles sobre la información proporcionada en una respuesta de perfil.

   >[!IMPORTANT]
   >
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Continúe con los flujos de decisiones:** La primera aplicación de flujo continuo puede continuar con los flujos de decisiones subsiguientes.

   >[!IMPORTANT]
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el identificador de plataforma único antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `Adobe-Subject-Token`, consulte la documentación de [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Recuperar identificador de plataforma:** La segunda aplicación de transmisión llama al servicio o biblioteca de identidad, fuera de los sistemas de Adobe Pass, para obtener la carga `JWS` o `JWE` asociada al identificador de plataforma único.

1. **Devuelve un identificador de plataforma único como JWS o JWE:** La segunda aplicación de streaming valida los datos de respuesta para garantizar que se cumplan las condiciones básicas de seguridad:
   * La carga útil no ha caducado.
   * La carga útil está firmada o cifrada.

1. **Recuperar perfiles:** La segunda aplicación de transmisión recopila todos los datos necesarios para recuperar toda la información de perfiles enviando una solicitud al extremo de perfiles.

   Consulte la documentación de la API [Recuperar perfiles](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) para obtener más información sobre lo siguiente:
   * Todos los _parámetros necesarios_, como `serviceProvider`
   * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

   >[!IMPORTANT]
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el identificador de plataforma único antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `Adobe-Subject-Token`, consulte la documentación de [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Buscar perfil de inicio de sesión único:** El servidor de Adobe Pass identifica un perfil de inicio de sesión único válido basado en los parámetros y encabezados recibidos.

1. **Devolver información sobre el perfil de inicio de sesión único:** La respuesta del extremo de perfiles contiene información sobre el perfil encontrado asociado a los parámetros y encabezados recibidos.

   Consulte la documentación de la API [Recuperar perfiles](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) para obtener más información sobre la información proporcionada en una respuesta de perfil.

   >[!IMPORTANT]
   >
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Continúe con los flujos de decisiones:** La segunda aplicación de transmisión puede continuar con los flujos de decisiones subsiguientes.

   >[!IMPORTANT]
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el identificador de plataforma único antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `Adobe-Subject-Token`, consulte la documentación de [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

## Recuperar decisiones de autorización mediante el inicio de sesión único mediante la identidad de la plataforma{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### Requisitos previos {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Antes de realizar el flujo de autorización mediante el inicio de sesión único mediante una identidad de plataforma, asegúrese de que se cumplan los siguientes requisitos previos:

* La plataforma debe proporcionar un servicio de identidad o una biblioteca que devuelva información coherente como `JWS` o `JWE` carga útil en todas las aplicaciones del mismo dispositivo o plataforma.
* La segunda aplicación de streaming debe recuperar el identificador de plataforma único e incluir la carga útil `JWS` o `JWE` como parte del encabezado [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) para todas las solicitudes que lo especifiquen.
* La segunda aplicación de streaming debe recuperar una decisión de autorización antes de reproducir un recurso seleccionado por el usuario.

>[!IMPORTANT]
>
> Suposiciones
> 
> <br/>
> 
> * La primera aplicación de flujo continuo ha realizado la autenticación y ha incluido un valor válido para el encabezado de solicitud [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

### Flujo de trabajo {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Realice los pasos dados para implementar el flujo de autorización a través del inicio de sesión único utilizando una identidad de plataforma como se muestra en el diagrama siguiente.

![Recuperar decisiones de autorización mediante el inicio de sesión único mediante la identidad de la plataforma](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*Recuperar decisiones de autorización mediante el inicio de sesión único mediante la identidad de la plataforma*

1. **Recuperar identificador de plataforma:** La segunda aplicación de transmisión llama al servicio o biblioteca de identidad, fuera de los sistemas de Adobe Pass, para obtener la carga `JWS` o `JWE` asociada al identificador de plataforma único.

1. **Devuelve un identificador de plataforma único como JWS o JWE:** La segunda aplicación de streaming valida los datos de respuesta para garantizar que se cumplan las condiciones básicas de seguridad:
   * La carga útil no ha caducado.
   * La carga útil está firmada o cifrada.

1. **Recuperar decisión de autorización:** La segunda aplicación de flujo continuo recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

   >[!IMPORTANT]
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el identificador de plataforma único antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `Adobe-Subject-Token`, consulte la documentación de [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Buscar perfil de inicio de sesión único:** El servidor de Adobe Pass identifica un perfil de inicio de sesión único válido basado en los parámetros y encabezados recibidos.

1. **Recuperar la decisión de MVPD para el recurso solicitado:** El servidor de Adobe Pass llama al extremo de autorización de MVPD para obtener una decisión `Permit` o `Deny` para el recurso específico recibido de la aplicación de flujo continuo.

1. **Devuelve la decisión `Permit` con el token de medios:** La respuesta del extremo de autorización de decisiones contiene una decisión `Permit` y un token de medios.

   Consulte la documentación de la API [Recuperar decisiones de autorización utilizando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de decisión.

   >[!IMPORTANT]
   >
   > El punto de conexión de autorización de decisiones valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Iniciar flujo con token de medios:** La segunda aplicación de flujo usa el token de medios para reproducir el contenido.

1. **Devolver `Deny` decisión con detalles:** La respuesta de extremo de autorización de decisiones contiene una decisión `Deny` y una carga de error que se adhiere a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

   Consulte la documentación de la API [Recuperar decisiones de autorización utilizando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de decisión.

   >[!IMPORTANT]
   >
   > El punto de conexión de autorización de decisiones valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Controlar los detalles de la decisión `Deny`:** La segunda aplicación de transmisión procesa la información de error de la respuesta y puede utilizarla para mostrar opcionalmente un mensaje específico en la interfaz de usuario.

>[!NOTE]
>
> Los pasos para el flujo de preautorización son los mismos que para el flujo de autorización, excepto que el punto de conexión utilizado es el descrito en la [Documentación de Recuperar decisiones de preautorización utilizando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) específico.
