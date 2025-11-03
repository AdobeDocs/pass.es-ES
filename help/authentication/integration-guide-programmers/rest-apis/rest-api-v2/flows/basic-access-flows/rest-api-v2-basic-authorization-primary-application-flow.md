---
title: Autorización básica - Aplicación principal - Flujo
description: REST API V2 - Autorización básica - Aplicación principal - Flujo
exl-id: 46bc9326-966e-44fc-8546-2f58be01b7bc
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# Flujo de autorización básico realizado en la aplicación principal {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

El **flujo de autorización** dentro del derecho de autenticación de Adobe Pass permite que la aplicación de streaming determine si un MVPD permite o deniega la solicitud del usuario para transmitir contenido. Si la decisión es `Permit`, la respuesta incluye un token multimedia. El servidor de Adobe Pass firma el token de medios y permite a la aplicación de streaming utilizar la biblioteca de verificador de tokens de medios para comprobar su autenticidad antes de que se libere el flujo.

La verificación con la biblioteca de verificador de tokens de medios debe realizarse en el servicio back-end de la aplicación de streaming vinculado en la cadena de permisos para liberar un flujo desde la CDN.

## Recuperar decisiones de autorización utilizando mvpd específico {#retrieve-authorization-decisions-using-specific-mvpd}

### Requisitos previos {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

Antes de recuperar decisiones de autorización utilizando un MVPD específico, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación de streaming debe tener un perfil regular válido que se haya creado correctamente para MVPD mediante uno de los flujos de autenticación básicos:
   * [Realizar autenticación en la aplicación principal](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria con mvpd preseleccionado](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Realizar autenticación en la aplicación secundaria sin mvpd preseleccionado](rest-api-v2-basic-authentication-secondary-application-flow.md)
* La aplicación de streaming debe recuperar una decisión de autorización antes de reproducir un recurso seleccionado por el usuario.

### Flujo de trabajo {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

Siga los pasos dados para implementar el flujo de autorización básico utilizando una MVPD específica realizada dentro de una aplicación principal como se muestra en el diagrama siguiente.

![Recuperar decisiones de autorización utilizando mvpd específico](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*Recuperar decisiones de autorización utilizando mvpd específico*

1. **Recuperar decisión de autorización:** La aplicación de streaming recopila todos los datos necesarios para obtener una decisión de autorización para un recurso específico llamando al extremo Decisions Authorize.

   >[!IMPORTANT]
   >
   > Consulte [Recuperar decisiones de autorización utilizando la documentación específica de la API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obtener más información sobre:
   >
   > * Todos los _parámetros necesarios_, como `serviceProvider`, `mvpd` y `resources`
   > * Todos los _encabezados_ necesarios, como `Authorization` y `AP-Device-Identifier`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Buscar perfil normal:** El servidor de Adobe Pass identifica un perfil válido en función de los parámetros y encabezados recibidos.

1. **Recuperar la decisión de MVPD para el recurso solicitado:** El servidor de Adobe Pass llama al extremo de autorización de MVPD para obtener una decisión `Permit` o `Deny` para el recurso específico recibido de la aplicación de flujo continuo.

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
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Iniciar flujo con token de medios:** La aplicación de flujo usa el token de medios para reproducir el contenido.

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
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Controlar los detalles de la decisión `Deny`:** La aplicación de transmisión procesa la información de error de la respuesta y puede utilizarla para mostrar opcionalmente un mensaje específico en la interfaz de usuario.
