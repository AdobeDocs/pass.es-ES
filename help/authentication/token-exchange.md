---
title: Intercambio de un token SSO de Platform por un token de Adobe
description: Intercambio de un token SSO de Platform por un token de Adobe
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Intercambio de un token SSO de Platform por un token de Adobe {#exchange-a-platform-sso-token-for-an-adobe-token}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/throttling-mechanism.md)

## Extremos de API de REST {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Permite &quot;intercambiar&quot; un perfil SSO de Platform por un token de Adobe.

| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | Servicio de programador </br></br>o</br></br>de aplicación de streaming | 1. solicitante (obligatorio)</br>    </br>2.  deviceId (obligatorio)</br>    </br>3.  mvpd (obligatorio)</br>    </br>4.  deviceType (obligatorio)</br>    </br>5.  SAMLResponse (obligatorio)</br>    </br>6.  deviceUser (Obsoleto)</br>    </br>7.  appId (obsoleto) | POST | La respuesta correcta será un 204 Sin contenido, que indica que el token se creó correctamente y está listo para usarse en los flujos de autenticación. | 204 - Sin contenido   </br>400 - Solicitud incorrecta |


| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| mvpd | Id de MVPD para el que es válida esta operación. |
| deviceType | La plataforma de Apple para la que intentamos obtener una solicitud de perfil.  **iOS** o **tvOS**. |
| SAMLResponse | El perfil real devuelto por el SSO de Platform. |
| _deviceUser_ | El identificador de usuario del dispositivo. |
| _appId_ | El nombre o ID de la aplicación. |
