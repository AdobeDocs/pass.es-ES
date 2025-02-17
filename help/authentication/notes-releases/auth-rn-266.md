---
title: Notas de la versión de autenticación de Adobe Pass 2.66
description: Notas de la versión de autenticación de Adobe Pass 2.66
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.66 {#authn-266-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-266}

* [Número de compilación](#build-number-266)
* [Información general de versión](#release-overview-266)

### Número de compilación {#build-number-266}

Autenticación de Adobe Pass: adobe-pass-**2.66.0.1**

Fecha de versión: **11/07/2023 - 13/07/2023**

### Información general de versión {#release-overview-266}

Con esta versión continuamos con las actualizaciones internas de la nueva API de REST.

#### Corrección de errores

* Se ha corregido el flujo de cierre de sesión de las MVPD basadas en SAML, en las que faltaba el parámetro RelayState en la solicitud de cierre de sesión. Nos centraremos en las actualizaciones de configuración después de la versión para restaurar el flujo de cierre de sesión de las MVPD afectadas.
* Se ha añadido la capacidad de actualizar certificados SSL en la configuración de los extremos de autorización de SOAP.
* Se ha corregido un caso límite en el que se registraban datos incorrectos en el campo Programador en algunos informes de ESM.
