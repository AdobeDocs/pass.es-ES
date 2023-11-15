---
title: Notas de la versión de autenticación de Adobe Pass 2.68
description: Notas de la versión de autenticación de Adobe Pass 2.68
source-git-commit: 47e663b9bc2044a182abede390aa15c5bf7d2e87
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.68 {#authn-268-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-268}

* [Número de compilación](#build-number-268)
* [Nuevas funciones](#new-features-268)
* [Actualizaciones de MVPD](#mvpd-updates-268)
* [Corrección de errores](#bug-fixes-268)

### Número de compilación {#build-number-268}

Autenticación de Adobe Pass: adobe-pass-end **2.68.0.5**
Fecha de versión: **21/11/2023 - 23/11/2023**

### Nuevas funciones {#new-features-268}

* Actualizaciones para nuestras nuevas API de REST.  Los nuevos puntos de conexión aún no están disponibles, pero estamos trabajando en la actualización de la documentación que detalla el uso de estas nuevas API.
* Mejoras continuas de la arquitectura interna.
* Se ha actualizado la biblioteca Device Atlas para mejorar la identificación de dispositivos.

#### Corrección de errores {#bug-fixes-268}

* Se ha corregido un problema con Vidgo MVPD para que no se puedan devolver varias decisiones para el mismo recurso.
