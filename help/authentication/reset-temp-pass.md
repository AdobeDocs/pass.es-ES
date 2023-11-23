---
title: Restablecer pase temporal
description: Restablecer pase temporal
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
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

Con el fin de **restablecer un pase temporal específico**, la autenticación de Adobe Pass proporciona a los programadores una *público* API web:

- **Entorno:** especifica el punto final del servidor de pase de TV de pago de Adobe que recibirá la llamada de red de Pase temporal restablecida. Valores posibles: **Preigualar** (*mgmt-prequal.auth.adobe.com*), **Versión** (*mgmt.auth.adobe.com*) o **Personalizado** (reservado para pruebas internas de Adobe).
- **Token de acceso de OAuth2:** el token de OAuth2 es necesario para autorizar al programador para la autenticación de Adobe de TV paga. Este token se puede obtener de [Registro dinámico de clientes](dynamic-client-registration.md).
- **Identificador de pase temporal:** el ID único para que se restablezca la MVPD de Temp Pass.(un programador puede usar varias MVPD de Temp Pass y quiere restablecer una específica)
- **Clave genérica:** algunas MVPD de Temp Pass (es decir, [Pase temporal promocional](promotional-temp-pass.md)).

Todos los parámetros anteriores (excepto el *Clave genérica*) son obligatorios. A continuación, se muestra un ejemplo de parámetros y la llamada de red asociada (el ejemplo tiene la forma de un comando *curl *):

- **Entorno:** Versión (*mgmt.auth.adobe.com*)
- **Token de acceso de OAuth2:** &lt;access_token> de [Registro dinámico de clientes](dynamic-client-registration.md)
- **ID de programador:** REF
- **Identificador de pase temporal:** TempPassREF
- **Clave genérica:** null (sin valor proporcionado)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

se realizará una solicitud HTTP del DELETE al **/reset** punto final, pasar el *Token de acceso de OAuth2* en el encabezado Autorización y *ID de dispositivo*, *ID de solicitante* y *Identificador de pase temporal (ID de MVPD)* como parámetros.

Si el programador proporciona un valor para *Clave genérica*, se realizará otra llamada HTTP (esta vez al **/reset/generic** extremo), pasando el *Clave genérica* dentro de *key* parámetro de solicitud.

Por ejemplo, configurar la variable *Clave genérica* a un hash de dirección de correo electrónico (para las MVPD de paso temporal que admiten este tipo de funcionalidad) dará como resultado la siguiente llamada HTTP (el correo electrónico es `user@domain.com` su hash SHA-256 es `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Con el fin de **restablecer un pase temporal específico para todos los dispositivos**, la autenticación de Adobe Pass proporciona a los programadores una *público* API web:

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
