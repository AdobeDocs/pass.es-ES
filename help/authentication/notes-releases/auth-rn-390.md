---
title: Notas de la versión de autenticación de Adobe Pass 3.9.0
description: Notas de la versión de autenticación de Adobe Pass 3.9.0
hold: true
source-git-commit: 5ca8f29764a07ddb68abb36accb12cfb3b68b72d
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 3.9.0 {#authn-390-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-390}

* [Número de compilación](#build-number-390)
* [Información general de versión](#release-overview-390)

### Número de compilación {#build-number-390}

Autenticación de Adobe Pass: adobe-pass-**3.9.0.1**\
Fecha de versión: **09/08/2026 - 10/09/2026**

### Información general de versión {#release-overview-390}

Esta versión se centra en las mejoras de las métricas de REST API V2 y ESM.

#### Mejoras

* Se ha mejorado el inicio de sesión único del socio de la API de REST 2 para garantizar que se devuelva una solicitud de autenticación válida para las MVPD configuradas con OAuth2.
* Se han mejorado las decisiones de la API REST 2.0 para devolver una respuesta de error clara cuando falla la autorización, en lugar de una respuesta vacía.
* Se ha mejorado la generación del código de registro para evitar caracteres visualmente ambiguos, lo que facilita la lectura y la introducción correcta de códigos.
* Mejoras en el tablero de ESM con compatibilidad con métricas de AuthZ de comprobación preliminar.
