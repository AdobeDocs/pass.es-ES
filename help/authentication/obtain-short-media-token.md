---
title: Obtener token de medios corto
description: obtener token de medios corto
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Obtener token de medios corto {#obtain-short-media-token}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Extremos de API de REST {#clientless-endpoints}

&lt;reggie_fqdn>:

* Producción - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Ensayo - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Producción - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Ensayo - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Obtiene El Token De Medios Cortos.

| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/mediatoken</br></br>  o</br></br>&lt;sp_fqdn>/api/v1/tokens/media</br></br>Por ejemplo:</br></br>&lt;sp_fqdn>/api/v1/tokens/media | Aplicación de streaming</br></br>o</br></br>Servicio de programador | 1. solicitante (obligatorio)</br>2.  deviceId (obligatorio)</br>3.  recurso (obligatorio)</br>4.  device_info/X-Device-Info (obligatorio)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Obsoleto)</br>7.  _appId_ (Obsoleto) | GET | XML o JSON que contienen un token de medios codificado Base64 o detalles de error si no se consigue. | 200 - Éxito  </br>403 - Sin éxito |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| resource | Cadena que contiene un resourceId (o fragmento MRSS), identifica el contenido solicitado por un usuario y es reconocida por los extremos de autorización de MVPD. |
| device_info/</br></br>X-Device-Info | Información del dispositivo de streaming.</br></br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br>Consulte todos los detalles en [Pasar la información de dispositivo y conexión](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br>Si este parámetro está configurado correctamente, ESM ofrece métricas que [desglosado por tipo de dispositivo]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) cuando se utiliza sin cliente, de modo que se puedan realizar diferentes tipos de análisis para. Por ejemplo, Roku, AppleTV y Xbox.</br></br>Consulte [Ventajas de utilizar el parámetro devicetype sin cliente ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info reemplaza este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo.</br></br>**Nota**: si se utiliza, deviceUser debe tener los mismos valores que en la variable [Crear código de registro](/help/authentication/registration-code-request.md) solicitud. |
| _appId_ | El nombre o ID de la aplicación. </br></br>**Nota**: device_info reemplaza este parámetro. Si se usa, `appId` debe tener los mismos valores que en la variable [Crear código de registro](/help/authentication/registration-code-request.md) solicitud. |

{style="table-layout:auto"}

### Respuesta de ejemplo {#response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON:**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### Compatibilidad con Media Verification Library

El campo `serializedToken` de la llamada &quot;Obtener token de medio corto&quot; es un token codificado en Base64 que se puede comprobar con la biblioteca de verificación de Adobes Medium.
