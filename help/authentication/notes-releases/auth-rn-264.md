---
title: Notas de la versión de autenticación de Adobe Pass 2.64
description: Notas de la versión de autenticación de Adobe Pass 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.64 {#authn-264-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-264}

* [Número de compilación](#build-number-264)
* [Información general de versión](#release-overview-264)

### Número de compilación {#build-number-264}

Autenticación de Adobe Pass: adobe-pass-**2.64**

Fecha de versión: **11/08/2022 - 10/11/2022**

### Información general de versión {#release-overview-264}

* Actualizaciones de la infraestructura, destinadas a reducir los tiempos de respuesta del servidor y mejorar el rendimiento general del sistema.
* Mejoras en el nuevo mecanismo de identificación de plataformas.
* Capacidad para bloquear respuestas de autenticación no solicitadas de MVPD donde el parámetro &quot;in_response_to&quot; no aparece en la afirmación de SAML.

#### Corrección de errores

* Se ha corregido un formato incorrecto de algunos tokens de TempPass heredados.
* Se han corregido problemas menores en el flujo de autenticación de segunda pantalla.
