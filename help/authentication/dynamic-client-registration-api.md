---
title: API de registro de cliente dinámico
description: API de registro de cliente dinámico
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# API de registro de cliente dinámico {#dynamic-client-registration-api}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

Actualmente, la autenticación de Adobe Pass identifica y registra las aplicaciones de dos formas:

* los clientes basados en el explorador se registran mediante permitido [lista de dominios](/help/authentication/programmer-overview.md)
* los clientes de aplicaciones nativas, como las aplicaciones de iOS y Android, se registran mediante el mecanismo de solicitante firmado.

La autenticación de Adobe Pass propone un nuevo mecanismo para registrar aplicaciones. Este mecanismo se describe en los párrafos siguientes.

## El mecanismo de registro de solicitudes {#appRegistrationMechanism}

### Razones técnicas {#reasons}

El mecanismo de autenticación de la autenticación de Adobe Pass dependía de las cookies de sesión, pero debido a [Fichas personalizadas de Android Chrome](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari View Controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}Sin embargo, este objetivo ya no se puede lograr.

Dadas estas limitaciones, el Adobe introduce un nuevo mecanismo de registro para todos sus clientes. Se basa en el documento RFC de OAuth 2.0 y consta de los siguientes pasos:

1. Recuperación de la declaración del software desde el panel de TVE
1. Obteniendo credenciales del cliente
1. Obtención de token de acceso

### Recuperación de la Declaración de Software del Tablero de TVE {#softwareStatement}

Para cada aplicación que publique, deberá obtener una declaración de software. Todas las instrucciones de software se proporcionan a través del Tablero de TVE, una vez creadas las aplicaciones. La instrucción de software debe implementarse junto con la aplicación en el dispositivo del usuario.

>[!IMPORTANT]
>
>Al utilizar una instrucción de software, el mecanismo de ID de solicitante firmado ya no será necesario.

Para obtener más información sobre cómo crear instrucciones de software, visite [Registro de cliente en el panel de TVE](/help/authentication/dynamic-client-registration.md).

### Obtención de credenciales de cliente {#clientCredentials}

Después de recuperar una declaración de software del Tablero de TVE, debe registrar la aplicación con el servidor de autorización de Adobe Pass. Para ello, realice una llamada /register y recupere el identificador de cliente único.

**Solicitud**

| llamada HTTP |                    |
|-----------|--------------------|
| ruta | /o/client/register |
| método | POST |

| campos |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | La declaración de software creada en TVE Dashboard. | obligatorio |
| redirect_uri | URI que utiliza la aplicación para completar el flujo de autenticación. | opcional |

| encabezados de solicitud |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Content-Type | application/json | obligatorio |
| X-Device-Info | La información del dispositivo tal como se define en Pasar información de dispositivo y conexión | obligatorio |
| User-Agent | El agente de usuario | obligatorio |

**Respuesta**

| encabezados de respuesta |                  |           |
|------------------|------------------|-----------|
| Content-Type | application/json | obligatorio |

| campos de respuesta |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | Cadena | obligatorio |
| client_secret | Cadena | obligatorio |
| client_id_Issued_at | largo | obligatorio |
| redirect_uris | lista de cadenas | obligatorio |
| grant_types | lista de cadenas<br/> **valor aceptado**<br/> `client_credentials`: lo utilizan clientes no seguros, como el SDK de Android. | obligatorio |
| error | **valores aceptados**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unapproved_software_statement</li></ul> | obligatorio en un flujo de error |


#### Respuesta de error {#error-response}

En caso de error, el servidor de registro responde con un código de estado HTTP 400 (Solicitud incorrecta) e incluye los siguientes parámetros en la respuesta:

| código de estado | cuerpo respuesta | description |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | A la solicitud le falta un parámetro requerido, incluye un valor de parámetro no admitido, repite un parámetro o su formato no es correcto. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_redirect_uri&quot;} | Redirect_uri no está permitido para este cliente en función de su aplicación registrada. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | La instrucción del software no es válida. |
| HTTP 400 | {&quot;error&quot;: &quot;unapproved_software_statement&quot;} | No se encuentra software_id en la configuración. |

#### Ejemplo de credenciales del cliente {#client-credentials-example}

**Solicitud:**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
 {
        "software_statement": "eyJhbGciOiJSUzI1NiJ9.
    eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
    bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
    aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
    GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
    zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
    5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
    fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
    U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
    IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
  "redirect_uri": "adobepass://com.programmer"  }
```

**Respuesta:**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**Respuesta de error:**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### Obtención de token de acceso {#accessToken}

Después de recuperar el identificador de cliente único (ID de cliente y secreto de cliente) para la aplicación, debe obtener un token de acceso. El token de acceso es un token obligatorio de OAuth 2.0 que se utiliza para llamar a las API de autenticación de Adobe Pass.

>[!NOTE]
>
>Actualmente, los tokens de acceso tienen una duración de 24 horas.

**Solicitud**


| **llamada HTTP** | |
| --- | --- |
| ruta | `/o/client/token` |
| método | POST |

| **parámetros de solicitud** | |
| --- | --- |
| `grant_type` | Recibido en el proceso de registro de cliente.<br/> **Valor aceptado**<br/>`client_credentials`: se utiliza para clientes no seguros, como el SDK para Android. |
| `client_id` | Identificador de cliente obtenido en el proceso de registro de cliente. |
| `client_secret` | Identificador de cliente obtenido en el proceso de registro de cliente. |

**Respuesta**

| campos de respuesta | | |
| --- | --- | --- |
| `access_token` | El valor del token de acceso que debe utilizar para llamar a las API de Adobe Pass. | obligatorio |
| `expires_in` | Tiempo en segundos hasta que caduca access_token | obligatorio |
| `token_type` | El tipo de token **portador** | obligatorio |
| `created_at` | La hora de emisión del token | obligatorio |
| **encabezados de respuesta** | | |
| `Content-Type` | application/json | obligatorio |

**Respuesta de error**

En caso de error, el servidor de autorización responde con un código de estado HTTP 400 (Solicitud incorrecta) e incluye los siguientes parámetros en la respuesta:

| código de estado | cuerpo respuesta | description |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | A la solicitud le falta un parámetro requerido, incluye un valor de parámetro no admitido (que no sea el tipo de concesión), repite un parámetro, incluye varias credenciales, utiliza más de un mecanismo para autenticar al cliente o tiene un formato incorrecto. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | Error de autenticación del cliente porque se desconocía el cliente. El SDK DEBE volver a registrarse en el servidor de autorización. |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorized_client&quot;} | El cliente autenticado no tiene autorización para utilizar este tipo de concesión de autorización. |

#### Ejemplo de obtención del token de acceso: {#obt-access-token}

**Solicitud:**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**Respuesta:**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**Respuesta de error:**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## Realización de solicitudes de autenticación {#autheticationRequests}

Utilice el token de acceso para realizar Adobe Pass [Llamadas de API de autenticación](/help/authentication/initiate-authentication.md). Para ello, es necesario añadir el token de acceso a la solicitud de API de una de las siguientes maneras:

* agregando un nuevo parámetro de consulta a la solicitud. Ese nuevo parámetro se llama **access_token**.

* añadiendo un nuevo encabezado HTTP a la solicitud: Autorización: Portador. Se recomienda utilizar el encabezado HTTP, ya que las cadenas de consulta tienden a ser visibles en los registros del servidor.

En caso de error, se podrían devolver las siguientes respuestas de error:

| Respuestas de error |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | La solicitud tiene un formato incorrecto. |
| invalid_client | 403 | El ID de cliente ya no tiene permiso para realizar solicitudes. Deben generarse nuevas credenciales de cliente. |
| access_denied | 401 | El access_token no es válido (caducado o no válido). El cliente DEBE solicitar un nuevo access_token. |

### Ejemplos de solicitudes de autenticación:

**Envío del token de acceso como parámetro de solicitud:**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**Envío del token de acceso como encabezado HTTP:**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**Respuestas de error como cuerpo de respuesta:**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**Respuestas de error como parámetros de URL:**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
