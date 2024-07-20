---
title: Restablecer pase temporal
description: Restablecer pase temporal
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 28d432891b7d7855e83830f775164973e81241fc
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# Restablecer pase temporal {#reset-temp-pass}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.
>
>Para utilizar la API Restablecer pase temporal, deberá hacer lo siguiente:
>- solicitar al equipo de soporte técnico una declaración de software para la aplicación registrada
>- obtenga un token de acceso basado en [Registro dinámico de clientes](dynamic-client-registration.md)
> 

Para **restablecer un pase temporal específico**, la autenticación de Adobe Pass proporciona a los programadores una API web *public*:

- **Entorno:** especifica el extremo del servidor de pase de TV de pago de Adobe que recibirá la llamada de red de Pase temporal restablecida. Valores posibles: **Prequal** (*mgmt-prequal.auth.adobe.com*), **Versión** (*mgmt.auth.adobe.com*) o **Personalizado** (reservado para pruebas internas de Adobe).
- **Token de acceso de OAuth2:** el token de OAuth2 es necesario para autorizar al programador para la autenticación de Adobe de TV paga. Este token se puede obtener de [Registro dinámico de clientes](dynamic-client-registration.md).
- **Id. de pase temporal:** el Id. único para que se restablezca la MVPD de pase temporal.(un programador puede usar varias MVPD de Temp Pass y quiere restablecer una específica)
- **Clave genérica:** algunas MVPD de pase temporal (por ejemplo: [pase temporal promocional](promotional-temp-pass.md)).

Todos los parámetros anteriores (excepto la *clave genérica*) son obligatorios. A continuación, se muestra un ejemplo de parámetros y la llamada de red asociada (el ejemplo tiene la forma de un comando *curl *):

- **Entorno:** versión (*mgmt.auth.adobe.com*)
- **Token de acceso de OAuth2:** &lt;access_token> de [Registro dinámico de cliente](dynamic-client-registration.md)
- **Id. de programador:** REF
- **Id. de pase temporal:** TempPassREF
- **Clave genérica:** nula (no se proporcionó ningún valor)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

se realizará una solicitud HTTP del DELETE al extremo **/reset**, pasando el *token de acceso OAuth2* en el encabezado Autorización y el *ID de dispositivo*, *ID de solicitante* y *ID de paso temporal (ID de MVPD)* como parámetros.

Si el programador proporciona un valor para la *clave genérica*, se realizará otra llamada HTTP (esta vez al extremo **/reset/generic**), pasando la *clave genérica* dentro del parámetro de solicitud *key*.

Por ejemplo, si establece la *clave genérica* en un hash de dirección de correo electrónico (para
Las MVPD de pase temporal (que admiten este tipo de funcionalidad) producirán el
siguiente llamada HTTP (el correo electrónico es `user@domain.com` su SHA-256)
hash es `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Para **restablecer un pase temporal específico para todos los dispositivos**, la autenticación de Adobe Pass proporciona a los programadores una API web *pública*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>La URL anterior reemplaza a la API de restablecimiento anterior. La API de restablecimiento antigua (v1) ya no es compatible.

- **Protocolo:** HTTPS
- **Host:**
   - Versión: mgmt.auth.adobe.com
   - Precual: mgmt-prequal.auth.adobe.com
- **Ruta:** /reset-tempass/v3/reset
- **Parámetros de consulta:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **Encabezados:** Autorización: Portador &lt;access_token_here>
- **Respuesta:**
   - Correcto: HTTP 204
   - Error:
      - HTTP 400 para una solicitud incorrecta
      - HTTP 401 si se deniega el acceso. El cliente DEBE solicitar un nuevo access_token.
      - HTTP 403 si el ID de cliente ya no tiene permiso para realizar solicitudes. Deben generarse nuevas credenciales de cliente.


Por ejemplo:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
