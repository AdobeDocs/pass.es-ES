---
title: Recuperar solicitud de perfil SSO de Platform
description: Recuperar solicitud de perfil SSO de Platform
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# (Heredado) Recuperar la solicitud de perfil SSO de Platform {#retrieve-platform-sso-profile-request}

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

## Descripción {#description}

Este recurso produce solicitudes de perfil para un ID de solicitante y una tupla de MVPD.


| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | Servicio de programador </br></br>o</br></br>de aplicación de streaming | &#x200B;1. solicitante (parámetro de ruta)</br>2. mvpd (parámetro de ruta)</br>3. deviceType (obligatorio) | GET | El Content-Type de respuesta será application/octet-stream, ya que la carga útil real es opaca para la aplicación cliente.</br></br>La aplicación debe reenviar la respuesta al motor de SSO de Platform</br></br>para obtener un SSO de perfil. | 200 - Éxito   </br>400 - Solicitud incorrecta |


| Parámetro de entrada | Descripción |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| mvpd | ID de MVPD para el que es válida esta operación. |
| deviceType | La plataforma de Apple para la que intentamos obtener una solicitud de perfil.  **iOS** o **tvOS**. |
