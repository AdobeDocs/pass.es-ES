---
title: Notas de la versión de Adobe Pass Authentication Android 3.7.3
description: Notas de la versión de Adobe Pass Authentication Android 3.7.3
exl-id: f335357e-c209-428d-af2a-2181551447d4
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 0%
---
# Notas de la versión de Adobe Pass Authentication Android 3.7.3 {#android-sdk-373-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Número de compilación {#build-number-373}

Autenticación de Adobe Pass: Android 3.7.3

Fecha de versión: **19/09/2023**

## Información general de versión {#release-overview-373}

* Cambios para admitir Android 14 y aplicaciones dirigidas al nivel de API 34
  * Agregar indicador requerido por [receptores de difusiones registradas en tiempo de ejecución de Android 14](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* Corregir que ChromeCustomTabs no se abra para el inicio de sesión de MVPD en la API del emulador 32+
  * Nota: Una solución para este problema en SDK &lt;3.7.3 es abrir la aplicación de Chrome en el emulador y terminar de configurarla antes de intentar iniciar sesión en MVPD

## Liberar paquete {#release-package-373}

Puede descargar Android SDK v3.7.3 desde [aquí](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
