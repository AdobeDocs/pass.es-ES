---
title: Notas de la versión de autenticación de Adobe Pass 2.65.1
description: Notas de la versión de autenticación de Adobe Pass 2.65.1
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# Notas de la versión de autenticación de Adobe Pass 2.65.1 {#authn-2651-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-2651}

* [Número de compilación](#build-number-2651)
* [Información general de versión](#release-overview-2651)

### Número de compilación {#build-number-2651}

Autenticación de Adobe Pass: adobe-pass-**2.65.1**

Fecha de versión: **20/06/2023 - 22/06/2023**

### Información general de versión {#release-overview-2651}

Esta versión añade lo siguiente:

A partir de esta versión, introduciremos la asistencia técnica para añadir reglas de limitación de velocidad en todas nuestras API, con el fin de proteger el ecosistema general contra un uso incorrecto.

El objetivo inicial será monitorizar el comportamiento de las aplicaciones para establecer los valores límite adecuados y no se aplicarán límites como parte de esta versión. Para los casos en los que el tráfico generado por una aplicación específica supera ciertos límites, trabajaremos con cada cliente para validar la implementación.

Las reglas de limitación de velocidad se aplicarán a finales de este año, después de validar el tráfico generado a partir de todas las aplicaciones de los clientes.
