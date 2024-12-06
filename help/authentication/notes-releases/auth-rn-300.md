---
title: Notas de la versión de Adobe Pass Authentication 3.0
description: Notas de la versión de Adobe Pass Authentication 3.0
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Notas de la versión de Adobe Pass Authentication 3.0 {#pt-authn-300-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-300}

* [Número de compilación](#build-number-300)
* [Información general de versión](#release-overview-300)

### Número de compilación {#build-number-2651}

Autenticación de Adobe Pass: adobe-pass-**3.0**
Fecha de versión: **10/09/2024 - 12/09/2024**

### Nuevas funciones {#new-features-300}

#### API de REST v2 {#rest-apis}

##### Código

* A partir de la versión adobe-pass-**3.0**, las aplicaciones cliente nuevas y existentes se pueden integrar o migrar a la nueva API de REST v2 para beneficiarse de las funcionalidades de Adobe Pass más recientes.

##### Documentación

* Para empezar con la nueva API de REST v2, consulte los siguientes documentos:
   * [API de REST v2: API: información general](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [API de REST v2: Flujos: información general](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* Las direcciones URL de los documentos públicos de la API de REST v1 han cambiado, consulte los siguientes documentos:
   * [API de REST v1: API: información general](../integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
   * [API de REST v1: API: referencia](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### Herramientas

* Para probar la nueva API de REST v2, consulte la nueva página Autenticación de Adobe Pass del sitio web de [Adobe Developer](https://developer.adobe.com/adobe-pass).

### Corrección de errores {#bug-fixes-300}

* Se ha corregido un problema con el parámetro de URL de redireccionamiento que no se usaba cuando estaba presente en la solicitud de cierre de sesión.
