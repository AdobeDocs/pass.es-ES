---
title: Notas de la versión de autenticación de Adobe Pass 3.0.3
description: Notas de la versión de autenticación de Adobe Pass 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 3.0.3 {#authn-303-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-303}

* [Número de compilación](#build-number-303)
* [Información general de versión](#release-overview-303)

### Número de compilación {#build-number-303}

Autenticación de Adobe Pass: adobe-pass-**3.0.3**

Fecha de versión: **29/10/2024 - 31/10/2024**

### Información general de versión {#release-overview-303}

#### API de REST v2

##### Código

* Mejoras en la API de REST V2 (desde la versión principal de Adobe Pass 3.0 que entregó [API de REST V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Se agregaron `notBefore` y `notAfter` campos en `/sessions/{code}` para devolver información sobre la validez del código de autenticación.
* Identificación de la plataforma mejorada.

##### Documentación

* Para empezar con la nueva API de REST v2, consulte el documento [Información general sobre la API de REST v2](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

##### Herramientas

* Para probar la nueva API de REST v2, consulte la nueva página Autenticación de Adobe Pass del sitio web de [Adobe Developer](https://developer.adobe.com/adobe-pass).
