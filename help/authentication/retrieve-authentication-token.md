---
title: Recuperar token de autenticación
description: Recuperar token de autenticación
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# Recuperar token de autenticación {#retrieve-authentication-token}

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

## Descripción {#description}

Recupera el token de autenticación (AuthN).

| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn</br></br>Por ejemplo:</br></br>&lt;sp_fqdn>/api/v1/tokens/authn | Aplicación de streaming</br></br>o</br></br>Servicio de programador | 1. solicitante (obligatorio)</br>2.  deviceId (obligatorio)</br>3.  device_info/X-Device-Info (obligatorio)</br>4.  _deviceType_ (Obsoleto)</br>5.  _deviceUser_ (Obsoleto)</br>6.  _appId_ (Obsoleto) | GET | XML o JSON que contienen información de autenticación o detalles de error si no se han realizado correctamente. | 200 - Éxito.  </br>404 - Token no encontrado  </br>410: Token caducado |

{style="table-layout:auto"}


| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| device_info/</br></br>X-Device-Info | Información del dispositivo de streaming.</br></br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br>**Nota**: device_info reemplaza este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo.</br></br>**Nota**: si se utiliza, deviceUser debe tener los mismos valores que en la variable [Crear código de registro](/help/authentication/registration-code-request.md) solicitud. |
| _appId_ | El nombre o ID de la aplicación. </br></br>**Nota**: device_info reemplaza este parámetro. Si se usa, `appId` debe tener los mismos valores que en la variable [Crear código de registro](/help/authentication/registration-code-request.md) solicitud. |

{style="table-layout:auto"}

</br>

### Respuesta de ejemplo {#response}



#### Correcto

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON:**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





#### Testigo de autenticación no encontrado:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```


**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found"
    }
```
