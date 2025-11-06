---
title: Notas de la versión de Adobe Pass Authentication Android 3.8.0
description: Notas de la versión de Adobe Pass Authentication Android 3.8.0
exl-id: ad020b9a-61ad-492f-9522-d0e7a668196a
source-git-commit: c9958a17ad9dfb518bab1d24087c85fdcb6fd057
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
