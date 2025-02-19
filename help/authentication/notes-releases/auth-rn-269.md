---
title: Notas de la versión de autenticación de Adobe Pass 2.69
description: Notas de la versión de autenticación de Adobe Pass 2.69
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.69 {#authn-269-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-269}

* [Número de compilación](#build-number-269)
* [Información general de versión](#release-overview-269)

### Número de compilación {#build-number-269}

Autenticación de Adobe Pass: adobe-pass-**2.69**

Fecha de versión: **27/02/2024 - 29/02/2024**

### Información general de versión {#release-overview-269}

#### Varios

* Vulnerabilidades de seguridad parcheadas.
* Mejoras para restablecer la capa de seguridad de Temp Pass con el registro dinámico de clientes (DCR).
   * Puede encontrar más detalles aquí: [Función TempPass](../integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
* Mejoras en la creación de informes de Platform Identification.

#### API de REST

* Desarrollo en curso de nuevas API de REST.
   * Una próxima versión dedicada introducirá nuevos puntos de conexión y flujos, que se anunciarán en una notificación independiente.
   * Está en curso la actualización de la documentación para el uso de estas nuevas API.

#### Tablero de TVE

* Desarrollo en curso en el nuevo Tablero de TVE.
   * Una próxima versión dedicada presentará el nuevo Tablero de TVE, que se anunciará en una notificación separada.
   * Se está actualizando la documentación para el uso de este nuevo tablero de TVE.

#### JavaScript SDK 4.7.0

* Se ha eliminado la versión obsoleta 2.0.1 del Access Enabler JavaScript SDK debido a vulnerabilidades de seguridad.
   * Siga el enlace para obtener más detalles: [Notas de la versión de Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
