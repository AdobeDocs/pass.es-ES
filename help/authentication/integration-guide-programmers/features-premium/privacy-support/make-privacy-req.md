---
title: Cómo realizar una solicitud de privacidad
description: Cómo realizar una solicitud de privacidad
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# Cómo realizar una solicitud de privacidad {#howto-make-privacy-request}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Identificadores y áreas de nombres {#identifier-namespace}

Al enviar una solicitud de privacidad de acceso o eliminación, la aplicación del cliente debe incluir los siguientes identificadores:

* **mvpdID** - Identificador único de MVPD.
* **userID**: identifica de forma exclusiva al usuario de la aplicación de un programador, pero se origina desde MVPD. Consulte Explicación de los ID de usuario en la Información general del programador.
* **IMSOrgID**: el ID de organización del servicio Adobe Experience Cloud Identity Management que identifica de forma exclusiva al cliente en Adobe Experience Cloud.


Compruebe el ejemplo siguiente:

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Los usuarios deben autenticarse para poder generar solicitudes de privacidad para la autenticación de Adobe Pass. De lo contrario, los programadores deben encontrar otros medios para extraer el ID de usuario de MVPD.

## Tipos de solicitudes {#req-type}

La autenticación de Adobe Pass admite solicitudes de acceso y eliminación.

### Acceso {#access-req}

Para una solicitud de acceso:

proporcionaremos un archivo JSON que contenga un resumen del número total de solicitudes de autenticación y autorización creadas para ese sujeto de datos.
todos estos eventos se filtran por cliente.


**Solicitar muestra**

Debe cargar un JSON con los identificadores de autenticación de Adobe Pass para los que envía la solicitud de acceso a datos. Para ver el aspecto de un JSON bien formado, consulte este ejemplo:

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Ejemplo de respuesta**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### Eliminar {#delete-req}

Debe cargar un JSON con los identificadores de autenticación de Adobe Pass para los que envía la solicitud de eliminación de datos. Para ver el aspecto de un JSON bien formado, consulte este ejemplo:

**Solicitar muestra**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Ejemplo de respuesta**

Para una solicitud de eliminación:

* solo compartimos un recibo de que se eliminaron los datos, no un archivo agregado con todos los datos eliminados.
* el recibo incluido en la respuesta contiene un resumen del número total de tokens de autenticación y autorización encontrados para ese sujeto de datos.

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## Cómo almacenar en déclencheur una solicitud {#trigger-req}

Hay dos opciones para que los clientes envíen solicitudes de privacidad a Adobe:

* **manualmente** - usando la [interfaz de usuario de Privacy Service](#privacy-service-ui)
* **automáticamente** - usando [API de Privacy Service](#privacy-service-api)

### Mediante la IU de Privacy Service {#privacy-service-ui}

Hay disponible un [tutorial completo](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) sobre cómo acceder y utilizar la interfaz de usuario de Privacy Service en línea a través de los servicios de Adobe I/O. Además, los clientes pueden utilizar este vínculo para acceder a la biblioteca de vídeos y artículos sobre normas de privacidad. Haga clic en el menú Adobe Experience Cloud y RGPD. Se abrirán varios vídeos: &quot;Cómo... la IU del RGPD&quot; explica cómo utilizarla.

En la interfaz de usuario de, los clientes deben cargar su propio IMSOrgID y un JSON que contenga solicitudes de RGPD para cada producto.

### Mediante la API de Privacy Service {#privacy-service-api}

Adobe Experience Platform Privacy Service proporciona una facilitación común y centralizada de las solicitudes de acceso/eliminación y las solicitudes de exclusión de la venta de datos privados.

La **documentación de la API de Privacy Service** explica en detalle cómo un cliente de Adobe puede integrarse con la API de Adobe.

**Visualizar llamadas API con Postman (software gratuito de terceros):**

* [Colección de Postman de la API de Privacy Service en GitHub](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Guía de vídeo para crear el entorno de Postman](https://video.tv.adobe.com/v/28832)
* [Pasos para importar entornos y colecciones en Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**Rutas de API:**

* URL de puerta de enlace de PLATFORM: `https://platform.adobe.io/`
* Ruta de acceso base para esta API: `/data/core/privacy/jobs`
* Ejemplo de ruta de acceso completa: `https://platform.adobe.io/data/core/privacy/jobs/ping`


**Encabezados requeridos:**

* Todas las llamadas requieren los encabezados `Authorization`, `x-gw-ims-org-id` y `x-api-key`. Para obtener más información sobre cómo obtener estos valores, consulte el **tutorial de autenticación**.
* Todas las solicitudes con una carga útil en el cuerpo de la solicitud (como llamadas POST, PUT y PATCH) deben incluir el encabezado `Content-Type` con un valor de `application/json`.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=en#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
