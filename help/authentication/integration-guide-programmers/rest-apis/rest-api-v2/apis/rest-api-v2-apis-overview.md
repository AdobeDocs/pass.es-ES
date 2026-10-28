---
title: Información general sobre las API de REST V2
description: Información general sobre las API de REST V2
exl-id: 62b48bf0-d200-4949-b268-8f8ea2daabfa
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 35c6ed6491cd47e6f85ee8974d72236c19fb8a6d
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%
---
# Información general sobre las API de REST V2 {#rest-api-v2-apis-overview}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

Para empezar, visite la documentación pública de las API que le interesan en la lista siguiente.

## Direcciones URL base {#rest-api-v2-apis-base-urls}

Utilice la siguiente URL base específica del entorno para las solicitudes de API de REST V2. Añadir
el `path` relativo documentado para cada API en esta dirección URL base.

Algunas respuestas de API devuelven direcciones URL interactivas que el dispositivo de flujo continuo debe abrir
en un navegador o agente de usuario. Algunos ejemplos son el atributo `url` devuelto por
[Iniciar el cierre de sesión de un mvpd específico](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
y el redireccionamiento `Location` devuelto por
[Realizar autenticación en el agente de usuario](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

Estas direcciones URL interactivas utilizan el mismo host de Adobe Pass Service Provider (SP).
Abra una dirección URL interactiva absoluta exactamente como se devuelve. Si Adobe Pass devuelve un
dirección URL interactiva relativa que comienza con `/adobe-services/`, resolverla con
Seleccione el host SP para el entorno de destino:

| Entorno | API de REST V2 y URL base de URL interactiva |
| --- | --- |
| Producción | `https://sp.auth.adobe.com` |
| Ensayo | `https://sp.auth-staging.adobe.com` |

## Configuración {#rest-api-v2-apis-configuration}

* [Recuperar la configuración de un proveedor de servicios específico](configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

## Sesiones {#rest-api-v2-apis-sessions}

* [Crear sesión de autenticación](sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reanudar sesión de autenticación](sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Recuperar sesión de autenticación](sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Realizar autenticación en el agente de usuario](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)

## Perfiles {#rest-api-v2-apis-profiles}

* [Recuperación de perfiles](profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

## Decisiones {#rest-api-v2-apis-decisions}

* [Recuperar decisiones de autorización utilizando mvpd específico](decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Recuperar decisiones de preautorización utilizando mvpd específico](decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## Cerrar sesión {#rest-api-v2-apis-logout}

* [Iniciar el cierre de sesión de un mvpd específico](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

## Inicio de sesión único de socio {#rest-api-v2-apis-partner-single-sign-on}

* [Recuperar solicitud de autenticación de socio](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Crear y recuperar perfiles mediante una respuesta de autenticación de socio](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

## ¿Quiere probar la API de REST V2?

Ahora puede explorar la API REST V2 a través de nuestra página dedicada al producto desde el sitio web [Adobe Developer](https://developer.adobe.com/adobe-pass/).
