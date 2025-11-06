---
title: Notas de la versión de autenticación de Adobe Pass 3.1.0
description: Notas de la versión de autenticación de Adobe Pass 3.1.0
exl-id: cf9fc8e2-4b37-4b0a-a6ed-cda1b6738e76
source-git-commit: 0c6aec04ae9df410228730b5bce6ced1aeecd312
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 3.1.0 {#authn-310-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-310}

* [Número de compilación](#build-number-310)
* [Información general de versión](#release-overview-310)

### Número de compilación {#build-number-310}

Autenticación de Adobe Pass: adobe-pass-**3.1.0**

Fecha de versión: **25/02/2025 - 27/02/2025**

### Información general de versión {#release-overview-310}

#### API de REST v2

* Nuevo nombre de acción `partner_logout` y tipo de acción `partner_interactive` para diferenciar entre el cierre de sesión normal y el cierre de sesión único del socio en la respuesta de la API de REST v2 [Logout API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).
* Nuevo campo `reason` para proporcionar más información sobre el nombre de la acción en las respuestas de la API de REST v2 [API de sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) y la API de SSO de [sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md).

#### Corrección de errores

* Se ha corregido un problema que impedía a los suscriptores de Spectrum autenticarse a través de la API de REST v2 [Autenticar API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).
* Se ha corregido un problema que impedía que los eventos generados mediante la API de REST V2 [Autenticar API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) se agregaran correctamente en ESM.
* Se ha corregido un problema que provocaba un cálculo incorrecto de la marca de tiempo `notBefore` para los perfiles de usuario en la respuesta de la API de REST v2 [Perfiles API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).

#### JavaScript SDK

* Se eliminó la versión 3.5.0 de [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md).
