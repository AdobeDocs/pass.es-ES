---
title: Notas de la versión de autenticación de Adobe Pass 3.10.0
description: Notas de la versión de autenticación de Adobe Pass 3.10.0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: d60b5256f5809b52122d70d04eb873018707fcaf
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Notas de la versión de autenticación de Adobe Pass 3.10.0 {#authn-3100-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-3100}

* [Número de compilación](#build-number-3100)
* [Información general de versión](#release-overview-3100)

### Número de compilación {#build-number-3100}

Autenticación de Adobe Pass: adobe-pass-**3.10.0**\
Fecha de versión: **27/10/2026 - 29/10/2026**

### Información general de versión {#release-overview-3100}

Esta versión de se centra en los controles de inicio de sesión único, las mejoras en los metadatos de los usuarios y las mejoras de fiabilidad para la API de REST V2 y la administración de la configuración de MVPD.

#### Mejoras

* Se ha agregado control a nivel de integración para el inicio de sesión único (SSO) de token de servicio. El SSO del token de servicio solo se utiliza cuando se habilita explícitamente para la integración.
* Se agregó compatibilidad con los metadatos de usuario personalizados (por ejemplo, `packageType`) durante la autenticación, la autorización y la comprobación preliminar. Las decisiones de autorización y comprobación preliminar de la API de REST V2 ahora incluyen metadatos de usuario configurados, incluidas las respuestas en caché, y respetan el acceso a los metadatos y la configuración de cifrado.

#### Corrección de errores

* Se ha corregido un problema que provocaba que las solicitudes de perfil SSO de Apple de la API de REST V2 fallaran con un error de respuesta SAML no válido para integraciones que usaban MVPD de OAuth2 a través de SAML.
* Se ha corregido un problema en el cual las decisiones de comprobación preliminar y autorización para el mismo recurso podían sobrescribir los tiempos de caducidad en caché y los metadatos de usuario de los demás.
* Se ha corregido un problema en el cual al eliminar MVPD proxy se dejaba una configuración de plataforma sin referencia que impedía guardar configuraciones posteriores.
