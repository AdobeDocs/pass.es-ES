---
title: Ruta de actualización de AccessEnabler iOS/tvOS 3.7.0
description: Ruta de actualización de AccessEnabler iOS/tvOS 3.7.0
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 0%

---

# Ruta de actualización de AccessEnabler iOS/tvOS 3.7.0 {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

Cambios en el almacenamiento de llaveros de [nuevo AccessEnabler versión 3.7.0](/help/authentication/authn-rn-ios-tvos-370.md) no son compatibles con la implementación del almacenamiento de llavero de la versión de AccessEnabler anterior a la 3.7.0.

La ruta de actualización para una aplicación que adopte la nueva versión 3.7.0 de AccessEnabler migrará todos los tokens de las versiones anteriores del almacenamiento de llaveros. Por lo tanto, usuarios finales **no debería experimentar pérdida de sesiones de autenticación/autorización** durante el proceso de actualización del marco de AccessEnabler.

## Limitaciones conocidas

Los implementadores pueden encontrar algunas limitaciones, que se describen a continuación.


1. El SSO normal (Adobe) no funcionará entre una aplicación que utiliza AccessEnabler versión 3.7.0 y una aplicación que utiliza AccessEnabler versión/es inferior a 3.7.0, incluso para aplicaciones desarrolladas por el mismo proveedor.

   - **Importante:**
      - El SSO a nivel de sistema (Apple) no se verá afectado.
      - El SSO normal (de Adobe) seguirá funcionando si ambas aplicaciones son desarrolladas por el mismo proveedor y usan versiones de AccessEnabler inferiores a la 3.7.0.
      - El SSO normal (de Adobe) funcionará si ambas aplicaciones son desarrolladas por el mismo proveedor y usan la versión 3.7.0 de AccessEnabler.

1. En caso de que se desplace una aplicación con AccessEnabler versión 3.7.0 a una versión inferior de AccessEnabler, no se migrarán los nuevos tokens generados. Por lo tanto, los usuarios finales pueden experimentar la pérdida de sesiones de autenticación/autorización, sin esperarlo.

   - **Importante:**
      - Los usuarios finales autenticados a través de SSO de nivel de sistema (Apple) no se verán afectados.
      - Los usuarios finales que ya se habían autenticado antes de actualizar a la nueva aplicación mediante AccessEnabler versión 3.7.0 no se verán afectados.

1. En caso de que se desplace una aplicación con AccessEnabler versión 3.7.0 a una versión inferior de AccessEnabler, no se reconocerán los tokens eliminados. Por lo tanto, los usuarios finales pueden experimentar la presencia de sesiones de autenticación/autorización, sin esperarlo.
