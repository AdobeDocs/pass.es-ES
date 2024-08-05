---
title: Flujos de acceso degradados
description: 'API REST V2: Flujos de acceso degradados'
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '1594'
ht-degree: 0%

---


# Flujos de acceso degradados {#degraded-access-flows}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La degradación proporciona la omisión temporal de extremos de autenticación y autorización de MVPD específicos. Normalmente, el programador inicia esta acción, pero independientemente de quién déclencheur un evento de degradación, la acción depende de los acuerdos previos realizados con las MVPD afectadas.

Para obtener más información sobre la característica de degradación, consulte la documentación de [Degradación](../../../degradation-api-overview.md).

Los flujos de acceso degradados le permiten consultar los siguientes escenarios:

* [Realizar autenticación mientras se aplica la degradación](#perform-authentication-while-degradation-is-applied)
* [Recuperar decisiones de autorización mientras se aplica la degradación](#retrieve-authorization-decisions-while-degradation-is-applied)
* [Recuperar decisiones de preautorización mientras se aplica la degradación](#retrieve-preauthorization-decisions-while-degradation-is-applied)
* [Recuperar perfil mientras se aplica degradación](#retrieve-profile-while-degradation-is-applied)

## Realizar autenticación mientras se aplica la degradación {#perform-authentication-while-degradation-is-applied}

### Requisitos previos {#prerequisites-perform-authentication-while-degradation-is-applied}

Antes de realizar el flujo de autenticación mientras se aplica la degradación, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming debe iniciar una sesión de autenticación cuando necesite iniciar sesión con la MVPD.

>[!IMPORTANT]
> 
> Suposiciones
> 
> <br/>
> 
> * La aplicación de streaming no tiene un perfil válido para ese MVPD específico guardado en el backend de Adobe Pass.
> * Hay una regla de degradación AuthNAll aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

### Flujo de trabajo {#workflow-perform-authentication-while-degradation-is-applied}

Siga los pasos dados para implementar el flujo de autenticación mientras se aplica la degradación como se muestra en el diagrama siguiente.

![Realizar autenticación mientras se aplica la degradación](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-perform-authentication-while-degradation-is-applied-flow.png)

*Realizar autenticación mientras se aplica la degradación*

1. **Crear sesión de autenticación:** La aplicación de flujo continuo recopila todos los datos necesarios para iniciar una sesión de autenticación llamando al extremo de sesiones.

   Consulte la documentación de la API [Crear sesión de autenticación](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obtener detalles sobre:
   * Todos los parámetros _requeridos_, como `serviceProvider`, `mvpd`, `domainName` y `redirectUrl`
   * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

1. **Comprobar reglas de degradación:** El servidor de Adobe Pass comprueba si hay una regla de degradación AuthNAll aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Indique la siguiente acción:** La respuesta del extremo de sesiones contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción:
   * El atributo `actionName` está establecido en &quot;authorize&quot;.
   * El atributo `actionType` está establecido en &quot;direct&quot;.

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
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > El extremo de sesiones utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso degradado:
   >
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe tener aplicada una regla de degradación AuthNAll.
   >
   > <br/>
   > 
   > Si la validación del acceso degradado falla, la respuesta se establecerá de forma predeterminada en el flujo de autenticación básico.

1. **Continúe con los flujos de decisiones:** La aplicación de transmisión puede continuar con los flujos de decisiones subsiguientes.

## Recuperar decisiones de autorización mientras se aplica la degradación {#retrieve-authorization-decisions-while-degradation-is-applied}

### Requisitos previos {#prerequisites-retrieve-authorization-decisions-while-degradation-is-applied}

Antes de recuperar las decisiones de autorización mientras se aplica la degradación, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación de streaming debe recuperar una decisión de autorización antes de reproducir un recurso seleccionado por el usuario.

>[!IMPORTANT]
>
> Suposiciones
> 
> <br/>
> 
> * La aplicación de streaming no tiene un perfil válido para ese MVPD específico.
> * Hay una regla de degradación AuthZAll o AuthNAll aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

### Flujo de trabajo {#workflow-retrieve-authorization-decisions-while-degradation-is-applied}

Siga los pasos dados para implementar el flujo de autorización mientras se aplica la degradación como se muestra en el diagrama siguiente.

![Recuperar decisiones de autorización mientras se aplica la degradación](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-authorization-decisions-while-degradation-is-applied-flow.png)

*Recuperar decisiones de autorización mientras se aplica la degradación*

1. **Recuperar decisión de autorización:** La aplicación de streaming recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

1. **Comprobar reglas de degradación:** El servidor de Adobe Pass comprueba si hay una regla de degradación AuthZAll o AuthNAll aplicada a la integración entre `serviceProvider` y `mvpd` proporcionados.

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
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).
   >
   > <br/>
   >
   > El extremo de autorización de decisiones utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso degradado:
   >
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe tener aplicada una regla de degradación AuthZAll o AuthNAll.
   >
   > <br/>
   > 
   > Si la validación del acceso degradado falla, la respuesta se establecerá de forma predeterminada en el flujo de autorización básico.

1. **Iniciar flujo con token de medios:** La aplicación de flujo usa el token de medios para reproducir el contenido.

## Recuperar decisiones de preautorización mientras se aplica la degradación {#retrieve-preauthorization-decisions-while-degradation-is-applied}

### Requisitos previos {#prerequisites-retrieve-preauthorization-decisions-while-degradation-is-applied}

Antes de recuperar las decisiones de preautorización mientras se aplica la degradación, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación de streaming desea recuperar las decisiones de preautorización para mostrar una lista de recursos junto con sus estados asociados.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * La aplicación de streaming no tiene un perfil válido para ese MVPD específico.
> * Hay una regla de degradación AuthZAll o AuthNAll aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

### Flujo de trabajo {#workflow-retrieve-preauthorization-decisions-while-degradation-is-applied}

Siga los pasos dados para implementar el flujo de preautorización mientras se aplica la degradación como se muestra en el diagrama siguiente.

![Recuperar decisiones de preautorización mientras se aplica la degradación](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-preauthorization-decisions-while-degradation-is-applied-flow.png)

*Recuperar decisiones de preautorización mientras se aplica la degradación*

1. **Recuperar decisiones de preautorización:** La aplicación de streaming recopila todos los datos necesarios para obtener decisiones de preautorización para una lista de recursos llamando al extremo Decisions Preauthorize.

   Consulte [Recuperar decisiones de preautorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

1. **Comprobar reglas de degradación:** El servidor de Adobe Pass comprueba si hay una regla de degradación AuthZAll o AuthNAll aplicada a la integración entre `serviceProvider` y `mvpd` proporcionados.

1. **Devolver decisiones de preautorización:** La respuesta de extremo de preautorización de Decisions contiene una decisión `Permit` para cada recurso.

   Consulte [Recuperar decisiones de preautorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) para obtener más información sobre la información proporcionada en una respuesta de decisión.

   >[!IMPORTANT]
   >
   > El extremo de preautorización de Decisions valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).
   >
   > <br/>
   >
   > El extremo de preautorización de Decisions utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso degradado:
   >
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe tener aplicada una regla de degradación AuthZAll o AuthNAll.
   >
   > <br/>
   > 
   > Si la validación del acceso degradado falla, la respuesta se establecerá de forma predeterminada en el flujo de preautorización básico.

1. **Controlar decisiones de preautorización:** La aplicación de transmisión procesa la respuesta y puede utilizarla para mostrar opcionalmente el estado apropiado para cada recurso en la interfaz de usuario.

## Recuperar perfil mientras se aplica degradación {#retrieve-profile-while-degradation-is-applied}

>[!IMPORTANT]
>
> La consulta del extremo de perfiles es opcional mientras se aplica la degradación.
>
> <br/>
> 
> La respuesta de extremo de sesiones indica a la aplicación que continúe con los flujos de decisiones mientras se aplica la degradación. Para obtener más información, consulte la sección [Realizar autenticación mientras se aplica la degradación](#perform-authentication-while-degradation-is-applied).

### Requisitos previos {#prerequisites-retrieve-profile-while-degradation-is-applied}

Antes de recuperar el perfil de una MVPD específica mientras se aplica la degradación, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación de streaming, que tiene un identificador `mvpd` seleccionado o almacenado en caché, desea recuperar el perfil de una MVPD específica.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * La aplicación de streaming no tiene un perfil válido para ese MVPD específico.
> * Hay una regla de degradación AuthNAll aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

### Flujo de trabajo {#workflow-retrieve-profile-while-degradation-is-applied}

Siga los pasos dados para implementar el flujo de recuperación de perfiles para una MVPD específica mientras se aplica la degradación como se muestra en el diagrama siguiente.

![Recuperar perfil mientras se aplica la degradación](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-profile-while-degradation-is-applied-flow.png)

*Recuperar perfil mientras se aplica la degradación*

1. **Recuperar perfil para mvpd específico:** La aplicación de streaming recopila todos los datos necesarios para recuperar información de perfil para esa MVPD específica enviando una solicitud al extremo de perfiles.

   Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) específica para obtener más información sobre lo siguiente:
   * Todos los _parámetros necesarios_, como `serviceProvider` y `mvpd`
   * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

1. **Comprobar reglas de degradación:** El servidor de Adobe Pass comprueba si hay una regla de degradación AuthNAll aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Devolver información sobre el perfil degradado:** La respuesta del extremo de perfiles contiene información sobre el perfil degradado, incluido el atributo `type` establecido como &quot;degradado&quot;.

   Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de perfil.

   >[!IMPORTANT]
   >
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > El extremo de perfiles utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso degradado:
   >
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe tener aplicada una regla de degradación AuthNAll.
   >
   > <br/>
   > 
   > Si la validación del acceso degradado falla, la respuesta se establecerá de forma predeterminada en el flujo básico de recuperación de perfiles.

1. **Continúe con los flujos de decisiones:** Si la respuesta del extremo de perfiles contiene un perfil, la aplicación de transmisión utiliza la información del perfil degradado para continuar con los flujos de decisiones subsiguientes.

1. **Indique un nuevo flujo de autenticación básico:** Si la respuesta del extremo de perfiles no contiene un perfil, la aplicación de transmisión indica al usuario que inicie un nuevo flujo de autenticación básico.

>[!NOTE]
>
> Los pasos del flujo de recuperación de perfiles para un código de autenticación específico son los mismos que los indicados arriba, excepto que el punto final utilizado es el descrito en la documentación de [Recuperar perfil para un código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md).
