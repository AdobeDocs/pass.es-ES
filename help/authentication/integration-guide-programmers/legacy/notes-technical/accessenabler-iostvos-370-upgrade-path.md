---
title: Ruta de actualización de AccessEnabler iOS/tvOS 3.7.0
description: Ruta de actualización de AccessEnabler iOS/tvOS 3.7.0
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Ruta de actualización de AccessEnabler iOS/tvOS 3.7.0 (heredado) {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

</br>

Los cambios en el almacenamiento de llaveros de la [nueva versión 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md) de AccessEnabler son incompatibles con la implementación del almacenamiento de llaveros de la versión inferior a la 3.7.0 de AccessEnabler.

La ruta de actualización para una aplicación que adopte la nueva versión 3.7.0 de AccessEnabler migrará todos los tokens de las versiones anteriores del almacenamiento de llaveros. Por lo tanto, los usuarios finales **no deben experimentar la pérdida de sesiones de autenticación/autorización** durante el proceso de actualización del marco de AccessEnabler.

## Limitaciones conocidas

Los implementadores pueden encontrar algunas limitaciones, que se describen a continuación.


1. El SSO normal (Adobe) no funcionará entre una aplicación que utiliza AccessEnabler versión 3.7.0 y una aplicación que utiliza AccessEnabler versión/es inferior a 3.7.0, incluso para aplicaciones desarrolladas por el mismo proveedor.

   >[!IMPORTANT]
   >
   >* El SSO a nivel de sistema (Apple) no se verá afectado.
   >
   >* El SSO normal (Adobe) seguirá funcionando si ambas aplicaciones son desarrolladas por el mismo proveedor y usan versiones de AccessEnabler inferiores a la 3.7.0.
   >
   >* El SSO normal (Adobe) funcionará si ambas aplicaciones son desarrolladas por el mismo proveedor y utilizan la versión 3.7.0 de AccessEnabler.


1. En caso de que se desplace una aplicación con AccessEnabler versión 3.7.0 a una versión inferior de AccessEnabler, no se migrarán los nuevos tokens generados. Por lo tanto, los usuarios finales pueden experimentar la pérdida de sesiones de autenticación/autorización, sin esperarlo.

   >[!IMPORTANT]
   >
   >* Los usuarios finales autenticados a través de SSO de nivel de sistema (Apple) no se verán afectados.
   >* Los usuarios finales que ya se habían autenticado antes de actualizar a la nueva aplicación mediante AccessEnabler versión 3.7.0 no se verán afectados.

1. En caso de que se desplace una aplicación con AccessEnabler versión 3.7.0 a una versión inferior de AccessEnabler, no se reconocerán los tokens eliminados. Por lo tanto, los usuarios finales pueden experimentar la presencia de sesiones de autenticación/autorización, sin esperarlo.
