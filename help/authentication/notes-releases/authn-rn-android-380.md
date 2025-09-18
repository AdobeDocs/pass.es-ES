---
title: Notas de la versión de Adobe Pass Authentication Android 3.8.0
description: Notas de la versión de Adobe Pass Authentication Android 3.8.0
exl-id: 7f8a9b2c-3d4e-5f6g-7h8i-9j0k1l2m3n4o
source-git-commit: 2276066d453701dc5e034da29cb971b090688afe
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Notas de la versión de Adobe Pass Authentication Android 3.8.0 {#android-sdk-380-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Número de compilación {#build-number-380}

Autenticación de Adobe Pass: Android 3.8.0

Fecha de versión: **18/09/2025**

## Información general de versión {#release-overview-380}

* Corrija una vulnerabilidad con el receptor de difusión de almacenamiento de SDK. Una aplicación malintencionada puede presentar un vínculo falso para buscar en el almacenamiento compartido los tokens de Adobe.
Sin embargo, no se almacena ninguna información crítica y la probabilidad de que algún usuario se haya visto afectado por esta vulnerabilidad es muy baja.
   * Nota: Como resultado del cambio, se cerrará la sesión de los usuarios.

## Liberar paquete {#release-package-380}

Puede descargar Android SDK v3.8.0 desde [aquí](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
