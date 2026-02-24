---
title: Notas de la versión de autenticación de Adobe Pass 3.5.0
description: Obtenga información acerca de las nuevas funciones, los cambios y los problemas conocidos de esta versión.
exl-id: b196f636-26a5-4974-903e-40b5f8b93a24
source-git-commit: 1cbddf081fc7d57a187c9701e4ade8593baf8759
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 3.5.0

Última actualización: Mar, 09 de diciembre de 2025, 00:00:00 GMT+0000 (hora universal coordinada)

* Temas:
* Autenticación

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-350}

* [Número de compilación](#build-number-350)
* [Información general de versión](#release-overview-350)

### Número de compilación {#build-number-350}

Autenticación de Adobe Pass: adobe-pass-**3.5.0**\
Fecha de versión: **12/09/2025 - 11/12/2025**

### Información general de versión {#release-overview-350}

#### API de REST v2

* Se ha añadido una nueva dimensión en ESM (&quot;api&quot;) para ayudar a los clientes a crear informes que distingan los eventos en función del tipo de implementación: SDK, REST V1 o REST V2.

#### Corrección de errores

* Se ha corregido un problema por el cual los mensajes de degradación personalizados configurados en el Tablero de TVE no aparecían en los detalles del error de la API de REST V2.
* Se ha corregido un problema en la API REST V2 por el cual no se devolvía el código de error `authenticated_profile_expired` cuando los perfiles autenticados caducaban.
* Se ha corregido un problema en el cual los cálculos de latencia de autorización y los valores TTL de comprobación preliminar eran incorrectos en la API de REST V2.
* Se ha corregido un problema en el cual se devolvía un formato de respuesta incoherente cuando caduca el token de DCR.

## Actualización de mantenimiento: febrero de 2026 {#maintenance-update-february-2026}

Autenticación de Adobe Pass: adobe-pass-**3.5.0.5**\
Fecha de versión: **24/02/2026 - 26/02/2026**

Esta versión de actualización de mantenimiento incluye mejoras importantes para mejorar la fiabilidad y seguridad del sistema:

### Mejoras

* Se ha mejorado la administración de la degradación de la autenticación para las configuraciones de MVPD proxy en la API de REST V2, lo que garantiza un comportamiento más coherente durante las interrupciones del servicio de MVPD.
* Validación mejorada de parámetros de URL y gestión de redirecciones para reforzar los controles de seguridad y mejorar la integridad general del sistema.
