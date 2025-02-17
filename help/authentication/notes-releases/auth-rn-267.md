---
title: Notas de la versión de autenticación de Adobe Pass 2.67
description: Notas de la versión de autenticación de Adobe Pass 2.67
exl-id: d899fe96-a273-4681-90a5-bde54cc2f3b3
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.67 {#authn-267-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-267}

* [Número de compilación](#build-number-267)
* [Información general de versión](#release-overview-267)

### Número de compilación {#build-number-267}

Autenticación de Adobe Pass: adobe-pass-**2.67.0.1**

Fecha de versión: **12/09/2023 - 14/09/2023**

### Información general de versión {#release-overview-267}

* Actualizaciones internas continuas para la nueva API de REST.
* Mejoras continuas de la arquitectura interna.

#### Actualizaciones de MVPD

* Actualizaciones para la integración de **DirecTV Puerto Rico** con Adobe. Póngase en contacto con su TAM para obtener más detalles.

#### Corrección de errores

* Se ha corregido un problema que provocaba la interrupción del SSO, obtenido mediante el encabezado Adobe-Subject-Token, entre aplicaciones que utilizan la API de REST y FireTV SDK.
