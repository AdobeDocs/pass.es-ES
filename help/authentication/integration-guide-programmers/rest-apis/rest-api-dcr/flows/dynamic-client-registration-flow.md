---
title: Flujo de registro de cliente dinámico
description: Flujo de registro de cliente dinámico
exl-id: d881cf0a-de09-4b1d-a094-d5490f944796
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Flujo de registro de cliente dinámico {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API de registro de cliente dinámico está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Acceso a API protegidas de Adobe Pass {#access-adobe-pass-protected-apis}

### Requisitos previos {#prerequisites-access-adobe-pass-protected-apis}

Antes de acceder a las API protegidas de Adobe Pass, asegúrese de que se cumplan los siguientes requisitos previos:

* Un representante del cliente debe crear una aplicación registrada como se describe en la sección [Administrar aplicaciones registradas](../dynamic-client-registration-overview.md#manage-registered-applications).
* Un representante del cliente debe descargar e incrustar una instrucción de software como se describe en la sección [Administrar instrucciones de software](../dynamic-client-registration-overview.md#manage-software-statements).

>[!IMPORTANT]
>
> Los SDK de autenticación de Adobe Pass son responsables de obtener y actualizar las credenciales del cliente y el token de acceso en nombre de la aplicación cliente.
> 
> Para todas las demás API protegidas por Adobe Pass, la aplicación cliente debe seguir el flujo de trabajo siguiente.

### Flujo de trabajo {#workflow-access-adobe-pass-protected-apis}

Siga los pasos dados para acceder a las API protegidas por Adobe Pass como se muestra en el diagrama siguiente.

![Acceso a API protegidas por Adobe Pass](../../../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Acceso a API protegidas por Adobe Pass*

1. **Recuperar credenciales de cliente:** La aplicación cliente recopila todos los datos necesarios para recuperar las credenciales de cliente llamando al extremo de registro de cliente.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar credenciales del cliente](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `software_statement`
   > * Todos los _encabezados_ necesarios, como `Content-Type`, `X-Device-Info`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Devolver credenciales de cliente:** La respuesta de extremo de registro de cliente contiene información sobre las credenciales de cliente asociadas con los parámetros y encabezados recibidos.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar credenciales del cliente](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) para obtener más información sobre la información proporcionada en una respuesta de credenciales de cliente.
   >
   > <br/>
   >
   > El registro de clientes valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de la API [Recuperar credenciales del cliente](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error).

   >[!TIP]
   >
   > Las credenciales del cliente deben almacenarse en caché y utilizarse indefinidamente.

1. **Recuperar token de acceso:** La aplicación cliente recopila todos los datos necesarios para recuperar el token de acceso llamando al extremo del token de cliente.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar token de acceso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request) para obtener detalles sobre:
   >
   > * Todos los _parámetros necesarios_, como `client_id`, `client_secret` y `grant_type`
   > * Todos los _encabezados_ necesarios, como `Content-Type`, `X-Device-Info`
   > * Todos los _parámetros y encabezados_ opcionales

1. **Devolver token de acceso:** La respuesta del extremo del token de cliente contiene información sobre el token de acceso asociado con los parámetros y encabezados recibidos.

   >[!IMPORTANT]
   >
   > Consulte la documentación de la API [Recuperar token de acceso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) para obtener detalles sobre la información proporcionada en una respuesta de token de acceso.
   >
   > <br/>
   >
   > El token de cliente valida los datos de solicitud para garantizar que se cumplen las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se adhiere a la documentación de la API [Recuperar token de acceso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error).

   >[!TIP]
   >
   > El token de acceso solo debe almacenarse en caché y utilizarse durante el tiempo especificado (por ejemplo, un período de vida de 24 horas). Una vez caducado, la aplicación cliente debe solicitar un nuevo token de acceso.

1. **Continúe con el acceso a las API protegidas:** La aplicación cliente utiliza el token de acceso para acceder a otras API protegidas por Adobe Pass. La aplicación cliente debe incluir el token de acceso en el encabezado de solicitud `Authorization` mediante el esquema de autenticación `Bearer` (es decir, `Authorization: Bearer <access_token>`).

   >[!IMPORTANT]
   >
   > Las API protegidas por Adobe Pass validan el token de acceso para garantizar que se cumplan las condiciones básicas:
   >
   > * El _token_de_acceso_ debe ser válido.
   > * El _token de acceso_ debe estar asociado con un _client_id_ y un _client_secret_ válidos.
   > * El _token_de_acceso_ debe estar asociado con un _extracto_de_software_ válido.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../features-standard/error-reporting/enhanced-error-codes.md).
