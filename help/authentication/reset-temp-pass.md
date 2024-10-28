---
title: Restablecer pase temporal
description: Restablecer pase temporal
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 32060798501abe2bf7314474d55cf844efb3f7b1
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# Restablecer pase temporal {#reset-temp-pass}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Antes de usar la API Restablecer pase temporal, asegúrese de que se cumplan los siguientes requisitos previos:
>
> * Obtenga las credenciales del cliente como se describe en la [Documentación de la API Retrieve client credentials](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenga el token de acceso como se describe en la [Documentación de la API Recuperar token de acceso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Consulte la documentación de [Información general sobre el registro dinámico de clientes](./dcr-api/dynamic-client-registration-overview.md) para obtener más información sobre cómo crear una aplicación registrada y descargar la instrucción de software.

Para **restablecer un pase temporal específico para un dispositivo o para todos los dispositivos**, la autenticación de Adobe Pass proporciona a los programadores una API web *pública*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **Protocolo:** HTTPS
* **Host:**
   * Versión: mgmt.auth.adobe.com
   * Precual: mgmt-prequal.auth.adobe.com
* **Ruta:** /reset-tempass/v3/reset
* **Parámetros de consulta:** device_id (cuando no se proporciona ningún valor, el valor predeterminado es todo), requestor_id (obligatorio), mvpd_id (obligatorio), appId, deviceUser, entorno
* **Encabezados:** Autorización: Portador &lt;access_token_here>
* **Respuesta:**
   * Correcto: HTTP 204
   * Error:xw
      * HTTP 400 para una solicitud incorrecta
      * HTTP 401 si se deniega el acceso. El cliente DEBE solicitar un nuevo access_token.
      * HTTP 403 si el ID de cliente ya no tiene permiso para realizar solicitudes. Deben generarse nuevas credenciales de cliente.


Por ejemplo:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

Para **restablecer el pase temporal flexible para una clave genérica o todas las claves**, la autenticación de Adobe Pass proporciona a los programadores una API web *pública*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **Protocolo:** HTTPS
* **Host:**
   * Versión: mgmt.auth.adobe.com
   * Precual: mgmt-prequal.auth.adobe.com
* **Ruta de acceso:** /reset-tempass/v3/reset/generic
* **Parámetros de consulta:** clave (cuando no se proporciona ningún valor, el valor predeterminado es todo), requestor_id (obligatorio), mvpd_id (obligatorio), entorno
* **Encabezados:** Autorización: Portador &lt;access_token_here>
* **Respuesta:**
   * Correcto: HTTP 204
   * Error:
      * HTTP 400 para una solicitud incorrecta
      * HTTP 401 si se deniega el acceso. El cliente DEBE solicitar un nuevo access_token.
      * HTTP 403 si el ID de cliente ya no tiene permiso para realizar solicitudes. Deben generarse nuevas credenciales de cliente.


Por ejemplo, si establece la *clave genérica* en un hash de dirección de correo electrónico (para
Las MVPD de pase temporal (que admiten este tipo de funcionalidad) producirán el
siguiente llamada HTTP (el correo electrónico es `user@domain.com` su SHA-256)
hash es `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
