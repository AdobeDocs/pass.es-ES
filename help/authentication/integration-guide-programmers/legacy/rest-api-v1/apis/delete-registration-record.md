---
title: Eliminar registro
description: Eliminar registro de registro
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 1%

---

# (Heredado) Eliminar registro {#delete-registration-record}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Extremos de API de REST {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Descripción {#delete-record}

Elimina el registro de código reg y libera el código reg para su reutilización.

| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Por ejemplo:</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Servicio de programador </br></br>o</br></br>de aplicación de streaming | &#x200B;1. Id. de solicitante </br>    (Componente de ruta de acceso)</br>2.  Código de registro </br>    (Componente Ruta) | DELETE | Ninguno | 204 |

{style="table-layout:auto"}

</br>

| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| código de registro | El valor del código de registro que se mostrará en el dispositivo de streaming (para introducirlo en el flujo de autenticación). |

{style="table-layout:auto"}

</br>

### [Volver a la referencia de API de REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
