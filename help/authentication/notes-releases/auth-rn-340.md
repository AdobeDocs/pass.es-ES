---
title: Notas de la versión de autenticación de Adobe Pass 3.4.0
description: Notas de la versión de autenticación de Adobe Pass 3.4.0
exl-id: 7a9b8c6d-4e5f-4a3b-8c7d-9e0f1a2b3c4d
source-git-commit: 3a275f64f7f8cffa3bdc0d546c8e2db517840069
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 3.4.0 {#authn-340-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-340}

* [Número de compilación](#build-number-340)
* [Información general de versión](#release-overview-340)

### Número de compilación {#build-number-340}

Autenticación de Adobe Pass: adobe-pass-**3.4.0**
Fecha de versión: **09/09/2025 - 11/09/2025**

### Información general de versión {#release-overview-340}

#### API de REST v2

* Se agregó compatibilidad con [Experience Cloud ID (ECID)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md) para mejorar la identificación del usuario y las capacidades de seguimiento.
* Se han cambiado los encabezados AP-Device-Identifier y X-Device-Info a opcionales para la API de REST V2 [API de configuración](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### Corrección de errores

* Se ha corregido un problema por el cual el ID de MVPD y el ID de MVPD proxy se invertían en el token de la respuesta de la API de REST V2 [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).
* Se ha corregido un problema en el cual el ID del solicitante no estaba presente en el token de la respuesta de la API de REST V2 [Decisions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).
* Se ha corregido un problema por el cual al pasar demasiados recursos a la API REST API V2 [Decisiones de autorización previa](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md), se producía una lista de decisiones vacía en la respuesta en lugar de [Código de error mejorado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **demasiados_recursos**.

#### Varios

* Vulnerabilidades de seguridad parcheadas.