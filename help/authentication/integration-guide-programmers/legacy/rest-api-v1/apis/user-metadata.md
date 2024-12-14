---
title: Metadatos del usuario
description: Metadatos del usuario
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# Metadatos de usuario (heredados) {#user-metadata}

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

`<REGGIE_FQDN>`:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Recupere metadatos compartidos por MVPD sobre el usuario autenticado.


| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Servicio de programador </br></br>o</br></br>de aplicación de streaming | 1. solicitante</br>2.  deviceId (obligatorio)</br>3.  device_info/X-Device-Info (obligatorio)</br>4.  deviceType</br>5.  deviceUser (Obsoleto)</br>6.  appId (obsoleto) | GET | XML o JSON que contienen metadatos de usuario o detalles del error si no se ha realizado correctamente. | 200 - Éxito<p>404 - No se han encontrado metadatos<p>412 - Token de AuthN no válido (por ejemplo, token caducado) |


| Parámetro de entrada | Descripción |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| device_info/<p>X-Device-Info | Información del dispositivo de transmisión.</br></br> **Nota:** Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br> Ver los detalles completos en [Pasar información de conexión y dispositivo](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br> Si este parámetro está configurado correctamente, ESM ofrece métricas que están [desglosadas por tipo de dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics) al utilizar sin cliente, de modo que se puedan realizar diferentes tipos de análisis, por ejemplo, para Roku, AppleTV, Xbox, etc.</br></br> Ver [Ventajas de usar el parámetro de tipo de dispositivo sin cliente en Pasar métricas](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Nota:** `device_info` reemplaza este parámetro. |
| _deviceUser_ | Identificador de usuario del dispositivo.</br></br> **Nota:** Si se usa, `deviceUser` debería tener los mismos valores que en la solicitud [Crear código de registro](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | El nombre o ID de la aplicación. </br></br> **Nota:** `device_info` reemplaza este parámetro. Si se usa, `appId` debería tener los mismos valores que en la solicitud [Crear código de registro](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

>[!NOTE]
> 
>La información de metadatos del usuario debe estar disponible una vez completado el flujo de autenticación, pero se puede actualizar en el flujo de autorización, según la MVPD y el tipo de metadatos.




## Respuesta de ejemplo {#sample-response}

Después de una llamada correcta, el servidor responderá con un objeto XML (predeterminado) o JSON con una estructura similar a la presentada a continuación:


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

En la raíz del objeto habrá tres nodos:

* *updated*: especifica una marca de tiempo UNIX que representa la última vez que se actualizaron los metadatos. El servidor establecerá esta propiedad inicialmente al generar los metadatos durante la fase de autenticación. Las llamadas posteriores (después de actualizar los metadatos) producirán un incremento en la marca de tiempo.
* *data*: contiene los valores de metadatos reales.
* *cifrado*: una matriz con las propiedades cifradas. Para descifrar un valor de metadatos específico, el programador debe realizar una descodificación Base64 de los metadatos y, a continuación, aplicar una descodificación RSA en el valor resultante, utilizando su propia clave privada (el Adobe cifra los metadatos en el servidor utilizando el certificado público del programador).

En caso de error, el servidor devolverá un objeto XML o JSON que especifica un mensaje de error detallado.

Para obtener más información, consulte [Metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md).

[Volver a la referencia de la API de REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
