---
title: Notas de la versión de autenticación de Adobe Pass 3.7.0
description: Notas de la versión de autenticación de Adobe Pass 3.7.0
source-git-commit: 89b5fbd8e8510cbf84ce7908e8cf86551e7a0cb9
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 3.7.0 {#authn-370-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-370}

* [Número de compilación](#build-number-370)
* [Información general de versión](#release-overview-370)

### Número de compilación {#build-number-370}

Autenticación de Adobe Pass: adobe-pass-**3.7.0.2**\
Fecha de versión: **12/05/2026 - 14/05/2026**

### Información general de versión {#release-overview-370}

Esta versión de se centra en las actualizaciones de la integración de MVPD, las correcciones de errores y las mejoras del tablero de TVE.

#### Integraciones de MVPD

* Se ha agregado compatibilidad con PKCE para la autenticación de MVPD basada en OAuth2.

#### Mejoras

* TVE Dashboard versión 1.5.1.

#### Corrección de errores

* Se ha corregido un problema que provocaba que Apple SSO pasara por alto ciertas discrepancias de configuración de MVPD.
* Se ha corregido un problema que provocaba errores HTTP 500 en la denegación de autorización debido a caracteres no válidos en el encabezado de respuesta.
