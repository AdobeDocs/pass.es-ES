---
title: Notas de la versión de autenticación de Adobe Pass 2.69
description: Notas de la versión de autenticación de Adobe Pass 2.69
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.69 {#pt-authn-269-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-269}

* [Número de compilación](#build-number-269)
* [Nuevas funciones](#new-features-269)

### Número de compilación {#build-number-269}

Autenticación de Adobe Pass: adobe-pass-**2.69**
Fecha de versión: **27/02/2024 - 29/02/2024**

### Nuevas funciones {#new-features-269}

#### Varios {#misc}

* Vulnerabilidades de seguridad parcheadas.
* Mejoras para restablecer la capa de seguridad de Temp Pass con el registro dinámico de clientes (DCR).
   * Puede encontrar más detalles aquí: [Restablecer pase temporal](reset-temp-pass.md)
* Mejoras en la creación de informes de Platform Identification.

#### API de REST {#rest-apis}

* Desarrollo en curso de nuevas API de REST.
   * Una próxima versión dedicada introducirá nuevos puntos de conexión y flujos, que se anunciarán en una notificación independiente.
   * Está en curso la actualización de la documentación para el uso de estas nuevas API.

#### Tablero de TVE {#tve-dashboard}

* Desarrollo en curso en el nuevo Tablero de TVE.
   * Una próxima versión dedicada presentará el nuevo Tablero de TVE, que se anunciará en una notificación separada.
   * Se está actualizando la documentación para el uso de este nuevo tablero de TVE.

#### SDK para JavaScript 4.7.0 {#js-sdk}

* Se ha eliminado la versión 2.0.1 obsoleta del SDK de JavaScript del activador de acceso debido a vulnerabilidades de seguridad.
   * Siga el enlace para obtener más detalles: [Notas de la versión de Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
