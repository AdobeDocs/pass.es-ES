---
title: Flujos de acceso temporales
description: 'API de REST V2: Flujos de acceso temporales'
exl-id: 387fcdb0-3a42-4893-ba83-e809426f92be
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '3223'
ht-degree: 0%

---

# Flujos de acceso temporales {#temporary-access-flows}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Asegúrese de visitar también las [Preguntas frecuentes sobre la API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

TempPass permite a los programadores proporcionar acceso temporal a su contenido protegido sin pedir a los usuarios que se autentiquen con una cuenta de MVPD válida.

Para obtener más información sobre la característica TempPass, consulte la documentación de [TempPass](../../../../features-premium/temporary-access/temp-pass-feature.md).

Los flujos de acceso temporales le permiten consultar los siguientes escenarios:

* [Recuperar decisiones de autorización utilizando TempPass básico](#retrieve-authorization-decisions-using-basic-temppass)
* [Recuperar decisiones de autorización mediante TempPass promocional](#retrieve-authorization-decisions-using-promotional-temppass)
* [Consumir el número máximo de recursos mediante TempPass promocional](#consume-maximum-number-of-resources-using-promotional-temppass)
* [Recuperar decisiones de autorización cuando caduca el TempPass básico o promocional](#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires)
* [Recuperar perfil para TempPass básico](#retrieve-profile-for-basic-temppass)
* [Recuperar perfil para TempPass promocional](#retrieve-profile-for-promotional-temppass)

## Recuperar decisiones de autorización utilizando TempPass básico {#retrieve-authorization-decisions-using-basic-temppass}

### Requisitos previos {#prerequisites-retrieve-authorization-decisions-using-basic-temppass}

Antes de recuperar las decisiones de autorización utilizando TempPass básico, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming quiere proporcionar acceso temporal para reproducir contenido sin pedir al usuario que se autentique.
* La aplicación de streaming debe recuperar una decisión de autorización antes de reproducir un recurso seleccionado por el usuario.

>[!IMPORTANT]
>
> Suposiciones
> 
> <br/>
> 
> * Debe haber una configuración válida de TempPass básico aplicado a la integración entre los `serviceProvider` y `mvpd` proporcionados.
> * El tiempo de vida (TTL) configurado para el TempPass básico no ha caducado.

### Flujo de trabajo {#workflow-retrieve-authorization-decisions-using-basic-temppass}

Siga los pasos dados para implementar el flujo de autorización utilizando TempPass básico, como se muestra en el diagrama siguiente.

![Recuperar decisiones de autorización utilizando TempPass básico](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-using-basic-temppass-flow.png)

*Recuperar decisiones de autorización utilizando TempPass básico*

1. **Recuperar decisión de autorización:** La aplicación de streaming recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   > 
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Validar TempPass básico:** El servidor de Adobe Pass comprueba si hay una configuración válida de TempPass básico aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Devuelve la decisión `Permit` con el token de medios:** La respuesta del extremo de autorización de decisiones contiene una decisión `Permit` y un token de medios.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar decisiones de autorización utilizando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de decisión.
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
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   > 
   > El extremo de autorización de decisiones utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso temporales:
   >
   > * El tiempo de vida (TTL) configurado para TempPass básico no debe caducar.
   >
   > <br/>
   > 
   > Si falla la validación temporal del acceso, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Iniciar flujo con token de medios:** La aplicación de flujo usa el token de medios para reproducir el contenido.

## Recuperar decisiones de autorización mediante TempPass promocional {#retrieve-authorization-decisions-using-promotional-temppass}

### Requisitos previos {#prerequisites-retrieve-authorization-decisions-using-promotional-temppass}

Antes de recuperar las decisiones de autorización mediante TempPass promocional, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming desea proporcionar acceso temporal para reproducir un número máximo de recursos sin solicitar al usuario que se autentique.
* La aplicación de streaming debe incluir información única sobre la identidad del usuario al recuperar una decisión de autorización.
* La aplicación de streaming debe recuperar una decisión de autorización antes de reproducir un recurso seleccionado por el usuario.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * Debe haber una configuración válida de TempPass promocional aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.
> * El tiempo de vida (TTL) configurado para el TempPass promocional no ha caducado.
> * No se ha consumido el número máximo de recursos configurados para el TempPass promocional.

### Flujo de trabajo {#workflow-retrieve-authorization-decisions-using-promotional-temppass}

Siga los pasos dados para implementar el flujo de autorización mediante TempPass promocional, como se muestra en el diagrama siguiente.

![Recuperar decisiones de autorización mediante TempPass promocional](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-using-promotional-temppass-flow.png)

*Recuperar decisiones de autorización mediante TempPass promocional*

1. **Recuperar decisión de autorización:** La aplicación de streaming recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   >
   > El extremo Decisions Authorize requiere la presencia del encabezado `AP-TempPass-Identity` al utilizar TempPass promocional. El encabezado incluye información única sobre la identidad del usuario que accede al contenido.
   > 
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AP-TempPass-Identity`, consulte la documentación de [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md).

1. **Validar TempPass promocional:** El servidor de Adobe Pass comprueba si hay una configuración válida de TempPass promocional aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Devuelve la decisión `Permit` con el token de medios:** La respuesta del extremo de autorización de decisiones contiene una decisión `Permit` y un token de medios.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar decisiones de autorización utilizando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de decisión.
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
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   > 
   > El extremo de autorización de decisiones utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso temporales:
   >
   > * El tiempo de vida (TTL) configurado para el TempPass promocional no debe caducar.
   > * No se debe consumir el número máximo de recursos configurados para el TempPass promocional.
   >
   > <br/>
   > 
   > Si falla la validación temporal del acceso, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Iniciar flujo con token de medios:** La aplicación de flujo usa el token de medios para reproducir el contenido.

## Consumir el número máximo de recursos mediante TempPass promocional {#consume-maximum-number-of-resources-using-promotional-temppass}

### Requisitos previos {#prerequisites-consume-maximum-number-of-resources-using-promotional-temppass}

Antes de consumir un número máximo de recursos con TempPass promocional, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming desea proporcionar acceso temporal para reproducir un número máximo de recursos sin solicitar al usuario que se autentique.
* La aplicación de streaming debe incluir información única sobre la identidad del usuario al recuperar una decisión de autorización.
* La aplicación de streaming debe recuperar una decisión de autorización antes de reproducir un recurso seleccionado por el usuario.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * Debe haber una configuración válida de TempPass promocional aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.
> * El tiempo de vida (TTL) configurado para el TempPass promocional no ha caducado.
> * El número máximo de recursos configurados para el TempPass promocional es 1.

### Flujo de trabajo {#workflow-consume-maximum-number-of-resources-using-promotional-temppass}

Siga los pasos dados para implementar el flujo de autorización al consumir un número máximo de recursos mediante TempPass promocional, como se muestra en el diagrama siguiente.

![Consumir el número máximo de recursos mediante TempPass promocional](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-consume-maximum-number-of-resources-using-promotional-temppass-flow.png)

*Consumir el número máximo de recursos mediante TempPass promocional*

1. **Recuperar perfil para TempPass promocional:** La aplicación de streaming recopila todos los datos necesarios para recuperar información de perfil para TempPass promocional enviando una solicitud al extremo de perfiles.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) específica para obtener más información sobre lo siguiente:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `mvpd`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   > 
   > La consulta de extremo de perfiles es opcional y se puede utilizar para determinar cuántos recursos se pueden seguir reproduciendo con el TempPass promocional.

1. **Validar TempPass promocional:** El servidor de Adobe Pass comprueba si hay una configuración válida de TempPass promocional aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Devuelve información sobre el perfil temporal:** La respuesta del extremo de perfiles contiene información sobre el perfil temporal, incluido el atributo `type` establecido como &quot;temporal&quot;.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de perfil.
   > 
   > <br/>
   > 
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
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
   > El extremo de perfiles utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso temporales:
   >
   > * El tiempo de vida (TTL) configurado para el TempPass promocional no debe caducar.
   > * No se debe consumir el número máximo de recursos configurados para el TempPass promocional.
   >
   > <br/>
   > 
   > Si falla la validación temporal del acceso, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Continúe con los flujos de decisiones:** Si la respuesta del extremo de perfiles contiene un perfil, la aplicación de transmisión utiliza la información de perfil temporal para continuar con los flujos de decisiones subsiguientes.

1. **Recuperar decisión de autorización:** La aplicación de streaming recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   >[!IMPORTANT]
   > 
   > Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   > 
   > El extremo Decisions Authorize requiere la presencia del encabezado `AP-TempPass-Identity` al utilizar TempPass promocional. El encabezado incluye información única sobre la identidad del usuario que accede al contenido.
   > 
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AP-TempPass-Identity`, consulte la documentación de [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md).

1. **Validar TempPass promocional:** El servidor de Adobe Pass comprueba si hay una configuración válida de TempPass promocional aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Devuelve la decisión `Permit` con el token de medios:** La respuesta del extremo de autorización de decisiones contiene una decisión `Permit` y un token de medios.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar decisiones de autorización utilizando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de decisión.
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
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   > 
   > <br/>
   > 
   > El extremo de autorización de decisiones utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso temporales:
   >
   > * El tiempo de vida (TTL) configurado para el TempPass promocional no debe caducar.
   > * No se debe consumir el número máximo de recursos configurados para el TempPass promocional.
   >
   > <br/>
   > 
   > Si falla la validación temporal del acceso, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Recuperar decisión de autorización:** La aplicación de streaming recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   > 
   > El extremo Decisions Authorize requiere la presencia del encabezado `AP-TempPass-Identity` al utilizar TempPass promocional. El encabezado incluye información única sobre la identidad del usuario que accede al contenido.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AP-TempPass-Identity`, consulte la documentación de [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md).

1. **Validar TempPass promocional:** El servidor de Adobe Pass comprueba si hay una configuración válida de TempPass promocional aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Devolver `Deny` decisión con detalles:** La respuesta de extremo de autorización de decisiones contiene una decisión `Deny` y una carga de error que se adhiere a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar decisiones de autorización utilizando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de decisión.
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
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   > 
   > El extremo de autorización de decisiones utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso temporales:
   >
   > * El tiempo de vida (TTL) configurado para el TempPass promocional no debe caducar.
   > * No se debe consumir el número máximo de recursos configurados para el TempPass promocional.
   >
   > <br/>
   > 
   > Si falla la validación temporal del acceso, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Controlar los detalles de la decisión `Deny`:** La aplicación de transmisión procesa la información de error de la respuesta y puede utilizarla para mostrar opcionalmente un mensaje específico en la interfaz de usuario.

   >[!TIP]
   >
   > La aplicación de streaming puede informar a los usuarios de que se ha superado el número máximo de recursos y aconsejarles que inicien un flujo de autenticación básico con un MVPD normal para seguir viendo.

## Recuperar decisiones de autorización cuando caduca el TempPass básico o promocional {#retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

### Requisitos previos {#prerequisites-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

Antes de recuperar las decisiones de autorización cuando caduque TempPass básico o promocional, asegúrese de que se cumplan los siguientes requisitos previos:

* [Requisitos previos para recuperar decisiones de autorización utilizando TempPass básico](#prerequisites-retrieve-authorization-decisions-using-basic-temppass).
* [Requisitos previos para recuperar decisiones de autorización con TempPass promocional](#prerequisites-retrieve-authorization-decisions-using-promotional-temppass).

>[!IMPORTANT]
>
> Suposiciones
> 
> <br/>
> 
> * Debe haber una configuración válida de TempPass básico o promocional aplicado a la integración entre el `serviceProvider` proporcionado y el `mvpd`.
> * El tiempo de vida (TTL) configurado para el básico o promocional: se ha superado el límite de duración de acceso temporal.

### Flujo de trabajo {#workflow-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires}

Siga los pasos dados para implementar el flujo de autorización cuando caduque TempPass básico o promocional, como se muestra en el diagrama siguiente.

![Recuperar decisiones de autorización cuando caduca TempPass básico o promocional](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-authorization-decisions-when-basic-or-promotional-temppass-expires-flow.png)

*Recuperar decisiones de autorización cuando caduca TempPass básico o promocional*

1. **Recuperar decisión de autorización:** La aplicación de streaming recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   > 
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   > 
   > El extremo Decisions Authorize requiere la presencia del encabezado `AP-TempPass-Identity` al utilizar TempPass promocional. El encabezado incluye información única sobre la identidad del usuario que accede al contenido.
   > 
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AP-TempPass-Identity`, consulte la documentación de [AP-TempPass-Identity](../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md).

1. **Validar TempPass básico o promocional:** El servidor de Adobe Pass comprueba si hay una configuración válida de TempPass básico o promocional aplicado a la integración entre `serviceProvider` y `mvpd` proporcionados.

1. **Devolver `Deny` decisión con detalles:** La respuesta de extremo de autorización de decisiones contiene una decisión `Deny` y una carga de error que se adhiere a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar decisiones de autorización utilizando mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de decisión.
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
   > Si la validación básica falla, se generará una respuesta de error que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   > 
   > El extremo de autorización de decisiones utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso temporales:
   >
   > * El tiempo de vida (TTL) configurado para el TempPass básico o promocional no debe caducar.
   > * No se debe consumir el número máximo de recursos configurados para el TempPass promocional.
   >
   > <br/>
   > 
   > Si falla la validación temporal del acceso, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Controlar los detalles de la decisión `Deny`:** La aplicación de transmisión procesa la información de error de la respuesta y puede utilizarla para mostrar opcionalmente un mensaje específico en la interfaz de usuario.

   >[!TIP]
   >
   > La aplicación de streaming puede informar a los usuarios de que el acceso temporal ha caducado y aconsejarles que inicien un flujo de autenticación básico con un MVPD normal para seguir viendo.

## Recuperar perfil para TempPass básico {#retrieve-profile-for-basic-temppass}

>[!IMPORTANT]
>
> La consulta de extremo de perfiles es opcional para TempPass básico.

### Requisitos previos {#prerequisites-retrieve-profile-for-basic-temppass}

Antes de recuperar el perfil para TempPass básico, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming desea recuperar el perfil temporal para garantizar que el acceso temporal no haya caducado.

>[!IMPORTANT]
>
> Suposiciones
> 
> <br/>
> 
> * Debe haber una configuración válida de TempPass básico aplicado a la integración entre los `serviceProvider` y `mvpd` proporcionados.
> * El tiempo de vida (TTL) configurado para TempPass básico no debe caducar.

### Flujo de trabajo {#workflow-retrieve-profile-information-for-basic-temppass}

Siga los pasos dados para implementar el flujo de recuperación de perfiles para TempPass básico, como se muestra en el siguiente diagrama.

![Recuperar perfil para TempPass básico](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-profile-for-basic-temppass-flow.png)

*Recuperar perfil para TempPass básico*

1. **Recuperar perfil para TempPass básico:** La aplicación de streaming recopila todos los datos necesarios para recuperar información de perfil para TempPass básico enviando una solicitud al extremo Profiles.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) específica para obtener más información sobre lo siguiente:
   > 
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `mvpd`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Validar TempPass básico:** El servidor de Adobe Pass comprueba si hay una configuración válida de TempPass básico aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Devuelve información sobre el perfil temporal:** La respuesta del extremo de perfiles contiene información sobre el perfil temporal, incluido el atributo `type` establecido como &quot;temporal&quot;.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de perfil.
   > 
   > <br/>
   > 
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
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
   > El extremo de perfiles utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso temporales:
   >
   > * El tiempo de vida (TTL) configurado para TempPass básico no debe caducar.
   >
   > <br/>
   > 
   > Si falla la validación temporal del acceso, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Continúe con los flujos de decisiones:** Si la respuesta del extremo de perfiles contiene un perfil, la aplicación de transmisión utiliza la información de perfil temporal para continuar con los flujos de decisiones subsiguientes.

## Recuperar perfil para TempPass promocional {#retrieve-profile-for-promotional-temppass}

>[!IMPORTANT]
>
> La consulta de extremo de perfiles es opcional para TempPass promocional.

### Requisitos previos {#prerequisites-retrieve-profile-for-promotional-temppass}

Antes de recuperar el perfil para TempPass promocional, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming desea recuperar el perfil temporal para asegurarse de que el acceso temporal no haya caducado o para determinar cuántos recursos se pueden reproducir.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * Debe haber una configuración válida de TempPass promocional aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.
> * El tiempo de vida (TTL) configurado para el TempPass promocional no ha caducado.
> * No se ha consumido el número máximo de recursos configurados para el TempPass promocional.

### Flujo de trabajo {#workflow-retrieve-profile-information-for-promotional-temppass}

Siga los pasos dados para implementar el flujo de recuperación de perfiles para TempPass promocional, como se muestra en el siguiente diagrama.

![Recuperar perfil para TempPass promocional](../../../../../assets/rest-api-v2/flows/temporary-access-flows/rest-api-v2-retrieve-profile-for-promotional-temppass-flow.png)

*Recuperar perfil para TempPass promocional*

1. **Recuperar perfil para TempPass promocional:** La aplicación de streaming recopila todos los datos necesarios para recuperar información de perfil para TempPass promocional enviando una solicitud al extremo de perfiles.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) específica para obtener más información sobre lo siguiente:
   > 
   > * Todos los _parámetros necesarios_, como `serviceProvider` y `mvpd`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Validar TempPass promocional:** El servidor de Adobe Pass comprueba si hay una configuración válida de TempPass promocional aplicada a la integración entre los `serviceProvider` y `mvpd` proporcionados.

1. **Devuelve información sobre el perfil temporal:** La respuesta del extremo de perfiles contiene información sobre el perfil temporal, incluido el atributo `type` establecido como &quot;temporal&quot;.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de perfil.
   > 
   > <br/>
   > 
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
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
   > El extremo de perfiles utiliza los datos de solicitud para comprobar si se cumplen las condiciones de acceso temporales:
   >
   > * El tiempo de vida (TTL) configurado para el TempPass promocional no debe caducar.
   > * No se debe consumir el número máximo de recursos configurados para el TempPass promocional.
   >
   > <br/>
   > 
   > Si falla la validación temporal del acceso, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Continúe con los flujos de decisiones:** Si la respuesta del extremo de perfiles contiene un perfil, la aplicación de transmisión utiliza la información de perfil temporal para continuar con los flujos de decisiones subsiguientes.
