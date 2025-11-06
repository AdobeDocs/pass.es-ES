---
title: Notas de la versión de autenticación de Adobe Pass 3.2.0
description: Notas de la versión de autenticación de Adobe Pass 3.2.0
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
source-git-commit: fcdf50b2caad20deef15fceeb3e23f4195c0078d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 3.2.0 {#authn-320-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-320}

* [Número de compilación](#build-number-320)
* [Información general de versión](#release-overview-320)

### Número de compilación {#build-number-320}

Autenticación de Adobe Pass: adobe-pass-**3.2.0**

Fecha de versión: **10/06/2025 - 12/06/2025**

### Información general de versión {#release-overview-320}

#### API de REST v2

* Se ha agregado un nuevo motivo `missing_parameters_fallback` para los casos en los que faltan parámetros en la respuesta [API de sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
* Se ha agregado un nuevo campo &quot;dispositivo&quot; a la respuesta [API de sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### Nuevas funciones

* Expanda la API de degradación para añadir la capacidad de aplicar reglas de degradación para varias MVPD en una sola llamada

#### Corrección de errores

* Se ha corregido un problema que impedía que el flujo de autenticación finalizara correctamente en casos en los que se producía un error en el procesamiento de metadatos de usuario.
* Se corrigió un problema en el cual el motivo de autorización no se calculaba correctamente.
* Se ha corregido un problema en el cual upstreamUserID no estaba presente en la respuesta del perfil.
* Se ha corregido un problema que hacía que la solicitud de autenticación no incluyera información de ámbito para las solicitudes de autenticación de inicio de la API de REST V2.
