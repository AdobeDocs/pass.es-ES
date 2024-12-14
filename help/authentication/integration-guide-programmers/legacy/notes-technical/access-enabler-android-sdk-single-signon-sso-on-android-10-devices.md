---
title: Acceso Habilitar el inicio de sesión único (SSO) de Android SDK en aplicaciones de Android 10
description: Acceso Habilitar el inicio de sesión único (SSO) de Android SDK en aplicaciones de Android 10
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# (Heredado) Habilitador de acceso Inicio de sesión único (SSO) de Android SDK en aplicaciones de Android 10 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Información general

El inicio de sesión único (SSO) entre aplicaciones con autenticación de Adobe Pass está disponible en dispositivos que usan el sistema operativo Android a través del Habilitador de acceso Android SDK. Para ofrecer el inicio de sesión único (SSO) en los dispositivos Android, la versión 3.2.1 (más reciente) y las versiones anteriores de Access Enabler Android SDK usan un archivo de base de datos compartido guardado en una implementación de almacenamiento de Android, al que pueden acceder todas las aplicaciones que utilizan autenticación de Adobe Pass.

Sin embargo, en la última versión de Android 10, Google produjo algunos cambios &quot;para dar a los usuarios más control sobre sus archivos y limitar el desorden de archivos, las aplicaciones dirigidas a Android 10 (nivel de API 29) y superiores reciben acceso con ámbito en un dispositivo de almacenamiento externo, o almacenamiento con ámbito, de forma predeterminada. Estas aplicaciones solo pueden ver el directorio específico de la aplicación `\[...\]`&quot;. Encontrará más detalles relacionados con estos cambios en el almacenamiento de Android 10 en [Documentación sobre el almacenamiento de datos y archivos de Android](https://developer.android.com/training/data-storage/files/external-scoped).

Como resultado de estos cambios, el inicio de sesión único (SSO) ofrecido por el Access Enabler Android versión **3.2.1 SDK (última versión)** y las versiones anteriores se pueden ver afectadas en los dispositivos Android 10, como se explica en la siguiente sección.

## Comportamiento

Según la aplicación **[!UICONTROL target SDK level]** o el uso del atributo de manifiesto **android:requestLegacyExternalStorage**, el inicio de sesión único (SSO) ofrecido por el Access Enabler Android versión 3.2.1 SDK (más reciente) y las versiones anteriores se comportarán de la siguiente manera:

- Su aplicación se dirige a **Android 9 (nivel de API 28)** o inferior **-\>** El inicio de sesión único (SSO) **funcionará**
- La aplicación se dirige a **Android 10** **(nivel de API 29)** y **establece** el valor de **requestLegacyExternalStorage en true** en el archivo de manifiesto de la aplicación **-\>** El inicio de sesión único (SSO) **funcionará**
- La aplicación se dirige a **Android 10** **(nivel de API 29)** y **no establece** el valor de **requestLegacyExternalStorage en true** en el archivo de manifiesto de la aplicación **-\>** El inicio de sesión único (SSO) **no funcionará**

>[!TIP]
>
> Antes de que el Habilitador de acceso a autenticación de Adobe Pass Android SDK sea totalmente compatible con el almacenamiento con ámbito, puede optar por excluirse temporalmente según el nivel de SDK de destino de la aplicación o el atributo de manifiesto requestLegacyExternalStorage, tal como se explica en la [documentación de Android](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage) pública.
