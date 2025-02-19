---
title: Notas de la versión de Adobe Pass Authentication JavaScript 4.0.0
description: Notas de la versión de Adobe Pass Authentication JavaScript 4.0.0
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Notas de la versión de Adobe Pass Authentication JavaScript 4.0.0 {#javascript-sdk-400-rn}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Número de compilación {#build-number-400}

Autenticación de Adobe Pass: JavaScript 4.0.0

Fecha de versión: **07/05/2018**

## Información general de versión {#release-overview-400}

* Se ha implementado compatibilidad con el mecanismo de registro dinámico de clientes, un mecanismo de seguridad unificado en todas las plataformas. La gestión del acceso de seguridad en todas las plataformas se puede realizar a través del panel de TVE, por el propio programador.
* Elimina el uso de todas las cookies de terceros y casi todas las cookies de origen para todas las funcionalidades (excepto la individualización en la que se sigue utilizando una cookie de origen, que se eliminará en el futuro), lo que la hace más compatible y compatible con las políticas de explorador emergentes en torno a cookies de terceros o cookies en general (p. ej., ITP de Safari). Tenga en cuenta que la versión anterior de 3.x no funciona de la forma esperada para los flujos felices utilizando solo cookies de origen, pero esto puede cambiar con el lanzamiento de nuevas versiones del navegador.
* Compatibilidad con la desactivación del almacenamiento en caché para comprobaciones de autorización de comprobaciones.
* Esta versión NO es compatible con versiones anteriores y está alojada en una nueva ruta: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . Para beneficiarse de esta nueva SDK de JS, es necesario un proceso de migración de los programadores para registrar sus aplicaciones web con el nuevo mecanismo de registro dinámico de clientes y apuntar a la nueva URL de JS SDK.

## Liberar paquete {#release-package-400}

La URL de producción es: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

La URL de ensayo es: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
