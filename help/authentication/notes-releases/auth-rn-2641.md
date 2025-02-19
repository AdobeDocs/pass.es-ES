---
title: Notas de la versión de autenticación de Adobe Pass 2.64.1
description: Notas de la versión de autenticación de Adobe Pass 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.64.1 {#authn-264-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-2641}

* [Número de compilación](#build-number-2641)
* [Información general de versión](#release-overview-2641)

### Número de compilación {#build-number-2641}

Autenticación de Adobe Pass: adobe-pass-**2.64.1**

Fecha de versión: **31/01/2023 - 02/02/2023**

### Información general de versión {#release-overview-2641}

Esta versión añade lo siguiente:

* La capacidad de bloquear respuestas de autenticación no solicitadas de MVPD donde el parámetro &quot;in_response_to&quot; no aparece en la afirmación de SAML.
* Se ha mejorado la validación sobre el parámetro redirect_url para cumplir con los requisitos de seguridad.
* Registro mejorado de la información del dispositivo para solicitudes de autenticación de segunda pantalla, con información proporcionada desde el dispositivo inicial.
