---
title: Preautorización básica - Aplicación principal - Flujo
description: 'API de REST V2: preautorización básica: aplicación principal: flujo'
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---


# Flujo de preautorización básico realizado en la aplicación principal {#basic-preauthorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/throttling-mechanism.md).

El **flujo de preautorización** dentro del derecho de autenticación de Adobe Pass permite que la aplicación de streaming determine si una MVPD puede permitir o denegar el acceso del usuario a una lista de recursos. Esta verificación garantiza que la aplicación pueda presentar información precisa al usuario sobre el contenido que podría poder ver.

## Recuperar decisiones de preautorización utilizando mvpd específico {#retrieve-preauthorization-decisions-using-specific-mvpd}

### Requisitos previos {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

Antes de recuperar las decisiones de preautorización utilizando una MVPD específica, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación de streaming debe tener un perfil regular válido que se haya creado correctamente para la MVPD mediante uno de los flujos de autenticación básicos:
   * [Realizar autenticación en la aplicación principal](./rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria con mvpd preseleccionado](./rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* La aplicación de streaming desea recuperar las decisiones de preautorización para mostrar una lista de recursos junto con sus estados asociados.

### Flujo de trabajo {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

Siga los pasos dados para implementar el flujo básico de preautorización utilizando una MVPD específica realizada dentro de una aplicación principal como se muestra en el diagrama siguiente.

![Recuperar decisiones de preautorización utilizando mvpd específico](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*Recuperar decisiones de preautorización utilizando mvpd específico*

1. **Recuperar decisiones de preautorización:** La aplicación de streaming recopila todos los datos necesarios para obtener decisiones de preautorización para una lista de recursos llamando al extremo Decisions Preauthorize.

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de preautorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Buscar perfil normal:** El servidor de Adobe Pass identifica un perfil válido en función de los parámetros y encabezados recibidos.

1. **Recuperar decisiones de MVPD para recursos solicitados:** El servidor de Adobe Pass llama al extremo de preautorización de MVPD para obtener una decisión `Permit` o `Deny` para cada recurso recibido de la aplicación de flujo continuo.

1. **Devolver decisiones de preautorización:** La respuesta de extremo de preautorización de Decisions contiene una decisión `Permit` o `Deny` para cada recurso:
   * Una decisión `Permit` significa que el recurso se puede reproducir. La respuesta no incluye un token de medios, ya que el flujo de preautorización no debe utilizarse para reproducir recursos.
   * Una decisión `Deny` significa que el recurso no se puede reproducir. La respuesta incluye una carga de error que se adhiere a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de preautorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) para obtener más información sobre la información proporcionada en una respuesta de decisión.
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
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Controlar decisiones de preautorización:** La aplicación de transmisión procesa la respuesta y puede utilizarla para mostrar opcionalmente el estado apropiado para cada recurso en la interfaz de usuario.
