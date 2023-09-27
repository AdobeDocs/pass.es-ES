---
title: Eliminar registro
description: Eliminar registro de registro
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Eliminar registro {#delete-registration-record}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Extremos de API de REST {#clientless-endpoints}

&lt;reggie_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Descripción {#delete-record}

Elimina el registro de código reg y libera el código reg para su reutilización.

| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Por ejemplo:</br></br>&lt;reggie_fqdn>/reggie/v1/regcode/ER45RTY | Aplicación de streaming</br></br>o</br></br>Servicio de programador | 1. ID del solicitante  </br>    (Componente Ruta)</br>2.  Código de registro  </br>    (Componente Ruta) | DELETE | Ninguno | 204 |

{style="table-layout:auto"}

</br>

| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| código de registro | El valor del código de registro que se mostrará en el dispositivo de streaming (para introducirlo en el flujo de autenticación). |

{style="table-layout:auto"}

</br>

### [Volver a la referencia de la API de REST](/help/authentication/rest-api-reference.md)
