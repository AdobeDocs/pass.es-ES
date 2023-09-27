---
title: Notas de la versión de autenticación de Adobe Pass 2.67
description: Notas de la versión de autenticación de Adobe Pass 2.67
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '125'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.67 {#authn-267-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-267}

* [Número de compilación](#build-number-267)
* [Nuevas funciones](#new-features-267)
* [Actualizaciones de MVPD](#mvpd-updates-267)
* [Corrección de errores](#bug-fixes-267)

### Número de compilación {#build-number-267}

Autenticación de Adobe Pass: adobe-pass-end **2.67.0.1**
Fecha de versión: **12/09/2023 - 14/09/2023**

### Nuevas funciones {#new-features-267}

* Actualizaciones internas continuas para la nueva API de REST.
* Mejoras continuas de la arquitectura interna.

### Actualizaciones de MVPD {#mvpd-updates-267}

* Actualizaciones para **DirecTV Puerto Rico** integración con Adobe. Póngase en contacto con su TAM para obtener más detalles.

#### Corrección de errores {#bug-fixes-267}

* Se ha corregido un problema que provocaba la interrupción del SSO, obtenido mediante el encabezado Adobe-Subject-Token, entre aplicaciones que utilizan la API de REST y el SDK de FireTV.
