---
title: Página de registro
description: Página de registro
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 3c44f1dfbbb5b9ec31f13e267dc691e14dd2ddfa
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# Página de registro {#registration-page}

## Extremos de API de REST {#clientless-endpoints}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/throttling-mechanism.md)

&lt;REGGIE_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## Descripción {#create-reg-code-svc}

Devuelve el código de registro generado aleatoriamente y el URI de la página de inicio de sesión.

| Extremo | Llamado <br> por | Entrada   <br>Parámetro | Método HTTP <br> | Respuesta | Respuesta HTTP <br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>Por ejemplo:<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Servicio de programador <br>o<br>de aplicación de streaming | 1. solicitante <br>    (Componente de ruta de acceso)<br>2.  deviceId (con hash)   <br>    (Obligatorio)<br>3.  device_info/X-Device-Info (obligatorio)<br>4.  mvpd (opcional)<br>5.  ttl (opcional)<br> | POST | XML o JSON que contienen un código de registro y detalles de información o error si no se consigue. Consulte los ejemplos siguientes. | 201 |

{style="table-layout:auto"}

| Parámetro de entrada | Tipo | Descripción |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autorización | Valor del encabezado <br>: portador &lt;access_token> | Token de acceso de DCR |
| Aceptar | Encabezado <br> Valor: application/json | indicar qué tipo de contenido debe comprender el cliente |
| solicitante | Parámetro de consulta | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | Parámetro de consulta | El ID de dispositivo bytes. |
| device_info/<br>X-Device-Info | device_info: Cuerpo <br> X-Device-Info: Header | Información del dispositivo de streaming.<br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. <br>Ver los detalles completos en [Pasar información de conexión y dispositivo](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | Parámetro de consulta | ID de MVPD para el que es válida esta operación. |
| ttl | Parámetro de consulta | El tiempo que debe permanecer este código de registro en segundos.<br>**Nota**: el valor máximo permitido para ttl es de 36000 segundos (10 horas). Los valores más altos dan como resultado una respuesta HTTP 400 (solicitud incorrecta). Si `ttl` se deja vacío, la autenticación de Adobe Pass establece un valor predeterminado de 30 minutos. |
| _deviceType_ | Parámetro de consulta | En desuso, no debe utilizarse. |
| _deviceUser_ | Parámetro de consulta | En desuso, no debe utilizarse. |
| _appId_ | Parámetro de consulta | En desuso, no debe utilizarse. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Dirección IP del dispositivo de transmisión**
><br>
>En implementaciones de cliente a servidor, la dirección IP del dispositivo de streaming se envía implícitamente con esta llamada.  En implementaciones de servidor a servidor, donde la llamada **regcode** se realiza como servicio de programador y no como dispositivo de transmisión, se requiere el siguiente encabezado para pasar la dirección IP del dispositivo de transmisión:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>donde `<streaming\_device\_ip>` es la dirección IP pública del dispositivo de streaming.
><br><br>
>Ejemplo :<br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### Respuesta JSON


#### Ejemplos de JSON de código de registro

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| Nombre de elemento | Descripción |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | UUID generado por el servicio de código de registro |
| código | Código de registro generado por el servicio de código de registro |
| solicitante | ID de solicitante |
| mvpd | ID de Mvpd |
| generado | Marca de tiempo de creación del código de registro (en milisegundos desde el 1 de enero de 1970 GMT) |
| caduca | Marca de tiempo cuando caduca el código de registro (en milisegundos desde el 1 de enero de 1970 GMT) |
| deviceId | ID de dispositivo único base64 |
| información:deviceId | Tipo de dispositivo Base64 |
| info:deviceInfo | La información normalizada de dispositivos de Base64 se basa en la información recibida de User-Agent, X-Device-Info o device_info |
| información:userAgent | Agente de usuario enviado por la aplicación |
| info:originalUserAgent | Agente de usuario enviado por la aplicación |
| info:authorizationType | OAUTH2 para llamadas que utilizan DCR |
| info:sourceApplicationInformation | Información de aplicación según se ha configurado en DCR |

{style="table-layout:auto"}


<br>

### Mensaje de error: muestra de respuesta JSON (#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

