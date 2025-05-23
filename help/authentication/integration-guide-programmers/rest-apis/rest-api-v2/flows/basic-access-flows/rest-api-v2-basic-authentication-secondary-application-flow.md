---
title: Autenticación básica - Aplicación secundaria - Flujo
description: 'API de REST V2: autenticación básica: aplicación secundaria: flujo'
exl-id: 83bf592e-c679-4cfe-984d-710a9598c620
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2006'
ht-degree: 0%

---

# Flujo de autenticación básico realizado en la aplicación secundaria {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Asegúrese de visitar también las [Preguntas frecuentes sobre la API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

El **flujo de autenticación** dentro del derecho de autenticación de Adobe Pass permite que la aplicación de transmisión verifique que un usuario tenga una cuenta de MVPD válida. Este proceso requiere que el usuario tenga una cuenta de MVPD activa e introduzca credenciales de inicio de sesión válidas en la página de inicio de sesión de MVPD.

El flujo de autenticación es necesario en los siguientes casos:

* Cuando el usuario abre una aplicación por primera vez.
* Cuando la autenticación anterior del usuario ha caducado.
* Cuando el usuario cierra la sesión de la cuenta de MVPD.
* Cuando el usuario desea autenticarse con un MVPD diferente.

En todos estos casos, la aplicación que llama a cualquiera de los extremos de Profiles recibe una respuesta vacía para uno o más perfiles, pero para diferentes MVPD.

El **flujo de autenticación** requiere que un agente de usuario (explorador) complete una serie de llamadas desde la aplicación al servidor de Adobe Pass, luego a la página de inicio de sesión de MVPD y, finalmente, de nuevo a la aplicación. Este flujo puede incluir varias redirecciones a sistemas de MVPD y la administración de cookies o sesiones almacenadas para cada dominio, lo que puede resultar difícil de lograr y proteger sin un agente de usuario.

En función de las funciones de la aplicación principal (aplicación de streaming) para admitir la interacción del usuario con el fin de seleccionar un MVPD y autenticarse con el MVPD seleccionado en un agente de usuario, los escenarios de autenticación son los siguientes:

* [Realizar autenticación en la aplicación principal](rest-api-v2-basic-authentication-primary-application-flow.md)
* [Realizar autenticación en la aplicación secundaria con mvpd preseleccionado](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Realizar autenticación en la aplicación secundaria con mvpd preseleccionado {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### Requisitos previos {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

Antes de iniciar el flujo de autenticación dentro de una aplicación principal y finalizarlo mediante la interacción del usuario dentro de una aplicación secundaria, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming debe seleccionar un MVPD.
* La aplicación de streaming debe iniciar una sesión de autenticación para iniciar sesión con el MVPD seleccionado.
* La aplicación secundaria debe autenticarse con el MVPD seleccionado en un agente de usuario.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * La aplicación de streaming admite la interacción del usuario para seleccionar un MVPD.
> * La aplicación secundaria (normalmente en un dispositivo secundario) admite la interacción del usuario para autenticarse con el MVPD seleccionado en un agente de usuario.

### Flujo de trabajo {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

Siga los pasos dados para implementar el flujo de autenticación básico realizado dentro de una aplicación secundaria con una MVPD preseleccionada como se muestra en el diagrama siguiente.

![Realizar autenticación en la aplicación secundaria con mvpd preseleccionado](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*Realizar autenticación en la aplicación secundaria con mvpd preseleccionado*

1. **Crear sesión de autenticación:** La aplicación de flujo continuo recopila todos los datos necesarios para iniciar una sesión de autenticación llamando al extremo de sesiones.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Crear sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obtener detalles sobre:
   > 
   > * Todos los parámetros _requeridos_, como `serviceProvider`, `mvpd`, `domainName` y `redirectUrl`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   > 
   > La aplicación de streaming debe proporcionar todos los parámetros necesarios en una sola llamada al crear la sesión de autenticación.

1. **Indique la siguiente acción:** La respuesta del extremo de sesiones contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Crear sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obtener más información sobre la información proporcionada en una respuesta de sesión.
   > 
   > <br/>
   > 
   > El punto final de sesiones valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Continuar con flujos de decisiones:** La respuesta del extremo de sesiones contiene los siguientes datos:
   * El atributo `actionName` está establecido en &quot;authorize&quot;.
   * El atributo `actionType` está establecido en &quot;direct&quot;.

   Si el backend de Adobe Pass identifica un perfil válido, la aplicación de streaming no necesita volver a autenticarse con el MVPD seleccionado, ya que ya hay un perfil que se puede utilizar para flujos de decisiones posteriores.

1. **Mostrar código de autenticación:** La respuesta del extremo de sesiones contiene los siguientes datos:
   * `code` que se puede usar para reanudar la sesión de autenticación en una aplicación secundaria.
   * El atributo `actionName` está establecido en &quot;autenticar&quot;.
   * El atributo `actionType` está establecido en &quot;interactivo&quot;.

   Si el servidor de Adobe Pass no identifica un perfil válido, la aplicación de flujo continuo muestra `code` que se puede usar para reanudar la sesión de autenticación en una aplicación secundaria.

1. **Validar código de autenticación:** La aplicación secundaria valida el usuario proporcionado `code` para asegurarse de que puede continuar con la autenticación de MVPD en el agente de usuario.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar información de sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `code`
   > * Todos los _encabezados_ requeridos, como `Authorization`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Devolver información sobre la sesión de autenticación:** La respuesta del extremo de sesiones contiene los siguientes datos:
   * El atributo `existing` contiene los parámetros existentes que ya se proporcionaron.
   * El atributo `missing` contiene los parámetros que faltan y que deben proporcionarse para completar el flujo de autenticación.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar información de sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) para obtener más información sobre la información proporcionada en una respuesta de validación de sesión.
   >
   > <br/>
   >
   > El punto final de sesiones valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!TIP]
   >
   > La aplicación secundaria puede informar a los usuarios de que el `code` utilizado no es válido en caso de una respuesta de error que indique que falta una sesión de autenticación y aconsejarles que vuelvan a intentarlo con una nueva.

1. **Abrir URL en el agente de usuario:** La aplicación secundaria abre un agente de usuario para cargar el elemento autocalculado `url`, realizando una solicitud al extremo Authenticate. Este flujo puede incluir varias redirecciones, lo que finalmente lleva al usuario a la página de inicio de sesión de MVPD y proporciona credenciales válidas.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Realizar autenticación en el agente de usuario](../../apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `code`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Autenticación de MVPD completa:** Si el flujo de autenticación es correcto, la interacción del agente de usuario guarda un perfil normal en el servidor de Adobe Pass y alcanza el valor de `redirectUrl` proporcionado.

1. **Recuperar perfil para código específico:** La aplicación de flujo continuo recopila todos los datos necesarios para recuperar información de perfil enviando una solicitud al extremo de perfiles.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obtener detalles sobre:
   > 
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `code`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

   >[!TIP]
   >
   > La aplicación de streaming debe implementar un mecanismo de sondeo usando `code` para comprobar si el perfil regular se generó y guardó correctamente.

1. **Devuelve información sobre el perfil normal:** La respuesta del extremo de perfiles contiene información sobre el perfil normal asociado a los parámetros y encabezados recibidos.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obtener detalles sobre la información proporcionada en una respuesta de perfil.
   > 
   > <br/>
   > 
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

## Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### Requisitos previos {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

Antes de iniciar el flujo de autenticación dentro de una aplicación principal y finalizarlo mediante la interacción del usuario dentro de una aplicación secundaria, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming debe iniciar una sesión de autenticación cuando necesite iniciar sesión.
* La aplicación secundaria debe seleccionar un MVPD.
* La aplicación secundaria debe autenticarse con el MVPD seleccionado en un agente de usuario.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * La aplicación secundaria (normalmente en un dispositivo secundario) admite la interacción del usuario para seleccionar una MVPD.
> * La aplicación secundaria (normalmente en un dispositivo secundario) admite la interacción del usuario para autenticarse con el MVPD seleccionado en un agente de usuario.

### Flujo de trabajo {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

Siga los pasos dados para implementar el flujo de autenticación básico realizado dentro de una aplicación secundaria sin un MVPD preseleccionado, como se muestra en el diagrama siguiente.

![Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado*

1. **Crear sesión de autenticación:** La aplicación de flujo continuo recopila algunos de los datos necesarios para iniciar una sesión de autenticación llamando al extremo de sesiones.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Crear sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   > 
   > La aplicación de streaming no puede proporcionar todos los parámetros necesarios en una sola llamada al crear la sesión de autenticación.

1. **Indique la siguiente acción:** La respuesta del extremo de sesiones contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción:
   * `code` que se puede usar para reanudar la sesión de autenticación en una aplicación secundaria.
   * El atributo `actionName` está establecido en &quot;continuar&quot;.
   * El atributo `actionType` está establecido en &quot;direct&quot;.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Crear sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obtener más información sobre la información proporcionada en una respuesta de sesión.
   > 
   > <br/>
   > 
   > El punto final de sesiones valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Mostrar código de autenticación:** La aplicación de flujo continuo muestra `code` que se puede usar para reanudar la sesión de autenticación en una aplicación secundaria.

1. **Proporcione parámetros que faltan en la sesión de autenticación:** La aplicación secundaria recopila todos los datos que faltan para reanudar la sesión de autenticación y llama al extremo de sesiones.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Reanudar sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) para obtener detalles sobre:
   >
   > * Todos los parámetros _requeridos_, como `serviceProvider`, `mvpd`, `domainName` y `redirectUrl`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Indique la siguiente acción:** La respuesta del extremo de sesiones contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Reanudar sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) para obtener más información sobre la información proporcionada en una respuesta de sesión.
   > 
   > <br/>
   > 
   > El punto final de sesiones valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!TIP]
   >
   > La aplicación secundaria puede informar a los usuarios de que el `code` utilizado no es válido en caso de una respuesta de error que indique que falta una sesión de autenticación y aconsejarles que vuelvan a intentarlo con una nueva.

1. **Indique el perfil existente:** La respuesta del extremo de sesiones contiene los siguientes datos:
   * El atributo `actionName` está establecido en &quot;authorize&quot;.
   * El atributo `actionType` está establecido en &quot;direct&quot;.

   Si el backend de Adobe Pass identifica un perfil válido, la aplicación de streaming no necesita volver a autenticarse con el MVPD seleccionado, ya que ya hay un perfil que se puede utilizar para flujos de decisiones posteriores.

1. **Abrir URL en el agente de usuario:** La respuesta del extremo de sesiones contiene los siguientes datos:
   * `url` que se puede usar para iniciar la autenticación interactiva en la página de inicio de sesión de MVPD.
   * El atributo `actionName` está establecido en &quot;autenticar&quot;.
   * El atributo `actionType` está establecido en &quot;interactivo&quot;.

   Si el backend de Adobe Pass no identifica un perfil válido, la aplicación secundaria abre un agente de usuario para cargar el `url` proporcionado, realizando una solicitud al extremo Authenticate. Este flujo puede incluir varias redirecciones, lo que finalmente lleva al usuario a la página de inicio de sesión de MVPD y proporciona credenciales válidas.

1. **Autenticación de MVPD completa:** Si el flujo de autenticación es correcto, la interacción del agente de usuario guarda un perfil normal en el servidor de Adobe Pass y alcanza el valor de `redirectUrl` proporcionado.

1. **Recuperar perfil para código específico:** La aplicación de flujo continuo recopila todos los datos necesarios para recuperar información de perfil enviando una solicitud al extremo de perfiles.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `code`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

   >[!TIP]
   >
   > La aplicación de streaming debe implementar un mecanismo de sondeo usando `code` para comprobar si el perfil regular se generó y guardó correctamente.

1. **Devuelve información sobre el perfil normal:** La respuesta del extremo de perfiles contiene información sobre el perfil normal asociado a los parámetros y encabezados recibidos.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obtener detalles sobre la información proporcionada en una respuesta de perfil.
   > 
   > <br/>
   > 
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
