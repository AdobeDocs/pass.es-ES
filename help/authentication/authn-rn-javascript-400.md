---
title: Notas de la versión JavaScript 4.0.0 de autenticación de Adobe Pass
description: Notas de la versión JavaScript 4.0.0 de autenticación de Adobe Pass
source-git-commit: 7057aeda34b4fe0d059912ab0a71ea856427654c
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Notas de la versión JavaScript 4.0.0 de autenticación de Adobe Pass {#javascript-sdk-400-release-notes}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Número de compilación {#build-no-javascript-sdk-400}

Autenticación de Adobe Pass: JavaScript 4.0.0

Fecha de versión: **05/07/2018**


## Información general de versión {#overview-javascript-sdk-400}

* Se ha implementado compatibilidad con el mecanismo de registro dinámico de clientes, un mecanismo de seguridad unificado en todas las plataformas. La gestión del acceso de seguridad en todas las plataformas se puede realizar a través del panel de TVE, por el propio programador.
* Elimina el uso de todas las cookies de terceros y casi todas las cookies de origen para todas las funcionalidades (excepto la individualización en la que se sigue utilizando una cookie de origen, que se eliminará en el futuro), lo que la hace más compatible y compatible con las políticas de explorador emergentes en torno a cookies de terceros o cookies en general (p. ej., ITP de Safari). Tenga en cuenta que la versión anterior de 3.x no funciona de la forma esperada para los flujos felices utilizando solo cookies de origen, pero esto puede cambiar con el lanzamiento de nuevas versiones del navegador.
* Compatibilidad con la desactivación del almacenamiento en caché para comprobaciones de autorización de comprobaciones.
* Esta versión NO es compatible con versiones anteriores y está alojada en una nueva ruta: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . Para beneficiarse de este nuevo SDK de JS, es necesario un proceso de migración de los programadores para registrar sus aplicaciones web con el nuevo mecanismo de registro dinámico de clientes y apuntar a la nueva URL del SDK de JS.


## Liberar paquete {#rel-pkg-javascript-sdk-400}

La URL de producción es: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

La URL de ensayo es: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
