---
title: Cierre de sesión básico - Aplicación principal - Flujo
description: 'API de REST V2: cierre de sesión básico, aplicación principal, flujo'
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---


# Flujo de cierre de sesión básico realizado en la aplicación principal {#basic-logout-flow-performed-within-primary-application}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El **flujo de cierre de sesión** dentro del derecho de autenticación de Adobe Pass permite que la aplicación de flujo continuo realice dos pasos principales:

* Elimine los perfiles normales guardados en el backend de Adobe Pass.
* Utilice un agente de usuario (explorador) para navegar hasta el punto final de cierre de sesión de MVPD, lo que activa una limpieza en el servidor de MVPD.

El flujo de cierre de sesión básico le permite consultar los siguientes escenarios:

* [Iniciar el cierre de sesión de un mvpd específico con el extremo de cierre de sesión](#initiate-logout-for-specific-mvpd-with-logout-endpoint)
* [Iniciar el cierre de sesión de un mvpd específico sin extremo de cierre de sesión](#initiate-logout-for-specific-mvpd-without-logout-endpoint)

## Iniciar el cierre de sesión para un mvpd específico que tenga un extremo de cierre de sesión {#initiate-logout-for-specific-mvpd-with-logout-endpoint}

### Requisitos previos {#prerequisites-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Antes de iniciar el cierre de sesión de una MVPD específica con un punto final de cierre de sesión, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming debe tener un perfil regular válido que se haya creado correctamente para la MVPD mediante uno de los flujos de autenticación básicos:
   * [Realizar autenticación en la aplicación principal](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria con mvpd preseleccionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* La aplicación de streaming debe iniciar el flujo de cierre de sesión cuando necesite cerrar la sesión de la MVPD.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * La MVPD admite el flujo de cierre de sesión y tiene un punto final de cierre de sesión.

### Flujo de trabajo {#workflow-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Siga los pasos dados para implementar el flujo de cierre de sesión básico para una MVPD específica con un punto final de cierre de sesión realizado dentro de una aplicación principal, como se muestra en el diagrama siguiente.

![Iniciar el cierre de sesión de un mvpd específico con el extremo de cierre de sesión](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-with-logout-endpoint.png)

*Iniciar el cierre de sesión de un mvpd específico con el extremo de cierre de sesión*

1. **Iniciar el cierre de sesión de Adobe Pass:** La aplicación de flujo continuo recopila todos los datos necesarios para iniciar el flujo de cierre de sesión llamando al extremo de cierre de sesión de Adobe Pass.

   Consulte la [Iniciar cierre de sesión para obtener documentación específica de la API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) para obtener más información sobre lo siguiente:
   * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `redirectUrl`
   * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

1. **Buscar perfil normal:** El servidor de Adobe Pass identifica un perfil válido en función de los parámetros y encabezados recibidos.

1. **Eliminar perfil regular:** El servidor de Adobe Pass elimina el perfil regular identificado del servidor de Adobe Pass.

1. **Indique la siguiente acción:** La respuesta del extremo de cierre de sesión de Adobe Pass contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción:
   * El atributo `url` está presente porque la MVPD admite el flujo de cierre de sesión.
   * El atributo `actionName` se ha establecido en &quot;cerrar sesión&quot;.
   * El atributo `actionType` está establecido en &quot;interactivo&quot;.

   Consulte la [Iniciar cierre de sesión para obtener documentación específica de la API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) para obtener más información sobre la información proporcionada en una respuesta de cierre de sesión.

   >[!IMPORTANT]
   >
   > El extremo de cierre de sesión de Adobe Pass valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Iniciar el cierre de sesión de MVPD:** La aplicación de streaming lee `url` y usa un agente de usuario para iniciar el flujo de cierre de sesión con MVPD. El flujo puede incluir varias redirecciones a sistemas de MVPD. Sin embargo, el resultado es que la MVPD realiza su limpieza interna y envía la confirmación de cierre de sesión final de nuevo al servidor de Adobe Pass.

1. **Indicar cierre de sesión completado:** La aplicación de flujo continuo puede esperar a que el agente de usuario alcance el `redirectUrl` proporcionado y puede utilizarlo como señal para mostrar opcionalmente un mensaje específico en la interfaz de usuario.

## Iniciar el cierre de sesión de un mvpd específico sin extremo de cierre de sesión {#initiate-logout-for-specific-mvpd-without-logout-endpoint}

### Requisitos previos {#prerequisites-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Antes de iniciar el cierre de sesión de una MVPD específica sin un punto final de cierre de sesión, asegúrese de que se cumplan los siguientes requisitos previos:

* La aplicación de streaming debe tener un perfil regular válido que se haya creado correctamente para la MVPD mediante uno de los flujos de autenticación básicos:
   * [Realizar autenticación en la aplicación principal](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria con mvpd preseleccionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* La aplicación de streaming debe iniciar el flujo de cierre de sesión cuando necesite cerrar la sesión de la MVPD.

>[!IMPORTANT]
>
> Suposiciones
>
> <br/>
> 
> * La MVPD no admite el flujo de cierre de sesión y no tiene un punto final de cierre de sesión.

### Flujo de trabajo {#workflow-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Siga los pasos dados para implementar el flujo de cierre de sesión básico para una MVPD específica sin un punto final de cierre de sesión realizado dentro de una aplicación principal, como se muestra en el diagrama siguiente.

![Iniciar el cierre de sesión de un mvpd específico sin el extremo de cierre de sesión](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-without-logout-endpoint.png)

*Iniciar el cierre de sesión de un mvpd específico sin el extremo de cierre de sesión*

1. **Iniciar el cierre de sesión de Adobe Pass:** La aplicación de flujo continuo recopila todos los datos necesarios para iniciar el flujo de cierre de sesión llamando al extremo de cierre de sesión de Adobe Pass.

   Consulte la [Iniciar cierre de sesión para obtener documentación específica de la API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) para obtener más información sobre lo siguiente:
   * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `redirectUrl`
   * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

1. **Buscar perfil normal:** El servidor de Adobe Pass identifica un perfil válido en función de los parámetros y encabezados recibidos.

1. **Eliminar perfil regular:** El servidor de Adobe Pass elimina el perfil regular identificado.

1. **Indique la siguiente acción:** La respuesta del extremo de cierre de sesión de Adobe Pass contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción:
   * Falta el atributo `url` porque MVPD no admite el flujo de cierre de sesión.
   * El atributo `actionName` está establecido en &quot;completo&quot;.
   * El atributo `actionType` está establecido en &quot;ninguno&quot;.

   Consulte la [Iniciar cierre de sesión para obtener documentación específica de la API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) para obtener más información sobre la información proporcionada en una respuesta de cierre de sesión.

   >[!IMPORTANT]
   >
   > El extremo de cierre de sesión de Adobe Pass valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Indicar cierre de sesión completado:** La aplicación de flujo continuo procesa la respuesta y puede utilizarla para mostrar opcionalmente un mensaje específico en la interfaz de usuario.
