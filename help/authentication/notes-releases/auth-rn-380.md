---
title: Notas de la versión de autenticación de Adobe Pass 3.8.0
description: Notas de la versión de autenticación de Adobe Pass 3.8.0
source-git-commit: 7d3f430ccfa158c3da32512e6c6d3b6f189ee63c
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 3.8.0 {#authn-380-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-380}

* [Número de compilación](#build-number-380)
* [Información general de versión](#release-overview-380)

### Número de compilación {#build-number-380}

Autenticación de Adobe Pass: adobe-pass-**3.8.0**\
Fecha de versión: **11/08/2026 - 13/08/2026**

### Información general de versión {#release-overview-380}

Esta versión de se centra en la estabilidad, las mejoras y las actualizaciones de seguridad en todos los servicios de autenticación de Adobe Pass.

#### Corrección de errores

* Se ha corregido un problema que provocaba errores HTTP 500 en las API V2 debido a ciertos caracteres no válidos en deviceId.

#### Mejoras

* Se ha mejorado la administración de tokens de actualización para admitir la renovación de tokens móviles.
* Se ha mejorado el reconocimiento visitorId en dispositivos secundarios para Analytics.
* Validación de parámetros de URL mejorada para reforzar los controles de seguridad y mejorar la integridad general del sistema.
* Panel de TVE versión 1.5.2 con mejoras menores en la IU.
