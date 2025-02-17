---
title: Notas de la versión de Adobe Pass Authentication JavaScript 4.4.0
description: Notas de la versión de Adobe Pass Authentication JavaScript 4.4.0
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Notas de la versión de Adobe Pass Authentication JavaScript 4.4.0 {#javascript-sdk-440-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Número de compilación {#build-number-440}

Autenticación de Adobe Pass: JavaScript 4.4.0

Fecha de versión: **22/06/2021**

## Información general de versión {#release-overview-440}

### Nuevas funciones

Autorización de verificación previa

* Nueva API preautorizada: esta es la nueva llamada de API que se utilizará para la función de autorización de comprobaciones, que también introduce un sistema mejorado de informes de errores para el flujo de comprobaciones.
* Esta función está disponible bajo solicitud, ya que debe habilitarse en la configuración de autenticación de Primetime. Póngase en contacto con su equipo de administración para obtener más información sobre cómo habilitar esta función.
* Obsoleta la API checkPreauthorizedResources.

Identificación de plataforma

* Agrega el encabezado AP-SDK-Identifier en todas las llamadas de SDK para identificar mejor el tipo y la versión de SDK.

Otros

* Mejoras arquitectónicas internas.

### Corrección de errores

* Se corrigió una condición de carrera que se producía cuando se llama simultáneamente a setRequestor y getAuthentication.
* Se ha corregido un problema que impedía que los derechos se cargaran correctamente en entornos de ensayo.
* Se ha corregido un problema que impedía que el flujo de cierre de sesión en segundo plano se completara en los exploradores Safari, por lo que el usuario seguía pareciendo autenticado hasta que se producía una actualización de página. Se introdujo un tiempo de espera, establecido actualmente en 30 segundos, por lo que si no hay respuesta del servidor de autenticación de Primetime durante este período, SDK invocará la llamada de retorno setAuthenticationStatus.

## Liberar paquete {#release-package-440}

La URL de producción es: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

La URL de ensayo es: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
