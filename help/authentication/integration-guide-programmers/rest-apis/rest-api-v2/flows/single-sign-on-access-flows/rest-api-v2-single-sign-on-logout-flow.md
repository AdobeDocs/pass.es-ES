---
title: 'Cierre de sesión único: flujo'
description: 'API de REST V2: cierre de sesión único: flujo'
exl-id: d7092ca7-ea7b-4e92-b45f-e373a6d673d6
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Flujo de cierre de sesión único {#single-logout-flow}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Asegúrese de visitar también las [Preguntas frecuentes sobre la API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

## Iniciar el cierre de sesión único de un mvpd específico {#initiate-single-logout-for-specific-mvpd}

### Requisitos previos {#prerequisites-initiate-single-logout-for-specific-mvpd}

Antes de iniciar el cierre de sesión único de un MVPD específico, asegúrese de que se cumplan los siguientes requisitos previos:

* La segunda aplicación de streaming debe tener un perfil de inicio de sesión único válido que se haya creado correctamente para MVPD mediante uno de los flujos de autenticación de inicio de sesión único:
   * [Realizar autenticación mediante el inicio de sesión único mediante la identidad de la plataforma](rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Realizar autenticación mediante el inicio de sesión único mediante el token de servicio](rest-api-v2-single-sign-on-service-token-flows.md)
* La segunda aplicación de streaming debe iniciar el flujo de cierre de sesión único cuando necesite cerrar la sesión de MVPD.

>[!IMPORTANT]
> 
> Suposiciones
>
> <br/>
> 
> * La primera y la segunda aplicaciones de streaming obtienen la misma carga de identificador de plataforma única que `JWS` o `JWE` o la misma carga de identificador de usuario único que `JWS`.

### Flujo de trabajo {#workflow-initiate-single-logout-for-specific-mvpd}

Realice los pasos dados para implementar el flujo de cierre de sesión único para una MVPD específica, como se muestra en el diagrama siguiente.

![Iniciar el cierre de sesión único de un mvpd específico](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*Iniciar el cierre de sesión único de un mvpd específico*

1. **Iniciar el cierre de sesión de Adobe Pass:** La aplicación de flujo continuo recopila todos los datos necesarios para iniciar el flujo de cierre de sesión llamando al extremo de cierre de sesión de Adobe Pass.

   >[!IMPORTANT]
   >
   > Consulte la [Iniciar cierre de sesión para obtener documentación específica de la API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) para obtener más información sobre lo siguiente:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `redirectUrl`
   > * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales
   >
   > <br/>
   >
   > La aplicación de streaming debe asegurarse de que incluye un valor válido para el identificador único de plataforma o el identificador único de usuario antes de realizar una solicitud.
   >
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `Adobe-Subject-Token`, consulte la [documentación de Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).
   > 
   > <br/>
   > 
   > Para obtener más información sobre el encabezado `AD-Service-Token`, consulte la documentación de [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Buscar perfiles de inicio de sesión único y normal:** El servidor de Adobe Pass identifica perfiles válidos de inicio de sesión único y regular en función de los parámetros y encabezados recibidos.

1. **Eliminar perfiles de inicio de sesión único y regular:** El servidor de Adobe Pass elimina los perfiles de inicio de sesión único y regular identificados del servidor de Adobe Pass.

1. **Indique la siguiente acción:** La respuesta del extremo de cierre de sesión de Adobe Pass contiene los datos necesarios para guiar a la aplicación de flujo continuo con respecto a la siguiente acción.

   >[!IMPORTANT]
   >
   > Consulte la [Iniciar cierre de sesión para obtener documentación específica de la API mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) para obtener más información sobre la información proporcionada en una respuesta de cierre de sesión.
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
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Indicar cierre de sesión completado:** Si MVPD no admite el flujo de cierre de sesión, la aplicación de flujo continuo procesa la respuesta y puede utilizarla para mostrar opcionalmente un mensaje específico en la interfaz de usuario.

1. **Iniciar el cierre de sesión de MVPD:** Si MVPD no admite el flujo de cierre de sesión, la aplicación de flujo continuo procesa la respuesta y utiliza un agente de usuario para iniciar el flujo de cierre de sesión con MVPD. El flujo puede incluir varias redirecciones a sistemas MVPD. Sin embargo, el resultado es que MVPD realiza su limpieza interna y envía la confirmación de cierre de sesión final de nuevo al back-end de Adobe Pass.

1. **Indicar cierre de sesión completado:** La aplicación de flujo continuo puede esperar a que el agente de usuario alcance el `redirectUrl` proporcionado y puede utilizarlo como señal para mostrar opcionalmente un mensaje específico en la interfaz de usuario.

>[!NOTE]
>
> Los pasos para el flujo de cierre de sesión único son los mismos que se describen arriba, si se inicia desde la primera aplicación de flujo continuo.
