---
title: Metadatos del usuario
description: Metadatos del usuario
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# Metadatos del usuario {#user-metadata}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Extremos de API de REST {#clientless-endpoints}

`<REGGIE_FQDN>`:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Recuperar metadatos que MVPD compartió sobre el usuario autenticado.


| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | Aplicación de streaming</br></br>o</br></br>Servicio de programador | 1. solicitante</br>2.  deviceId (obligatorio)</br>3.  device_info/X-Device-Info (obligatorio)</br>4.  deviceType</br>5.  deviceUser (Obsoleto)</br>6.  appId (obsoleto) | GET | XML o JSON que contienen metadatos de usuario o detalles del error si no se ha realizado correctamente. | 200 - Éxito<p>404 - No se han encontrado metadatos<p>412 - Token de AuthN no válido (por ejemplo, token caducado) |


| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| device_info/<p>X-Device-Info | Información del dispositivo de streaming.</br></br> **Nota:** Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br> Consulte todos los detalles en [Pasar la información de dispositivo y conexión](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br> Si este parámetro está configurado correctamente, ESM ofrece métricas que [desglosado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) cuando se utiliza sin cliente, de modo que se puedan realizar distintos tipos de análisis para, por ejemplo, Roku, AppleTV, Xbox, etc.</br></br> Consulte [Ventajas de utilizar el parámetro de tipo de dispositivo sin cliente en las métricas de pase](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **Nota:** El `device_info` reemplaza este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo.</br></br> **Nota:** Si se usa, `deviceUser` debe tener los mismos valores que en la variable [Crear código de registro](/help/authentication/registration-code-request.md) solicitud. |
| _appId_ | El nombre o ID de la aplicación. </br></br> **Nota:** El `device_info` reemplaza este parámetro. Si se usa, `appId` debe tener los mismos valores que en la variable [Crear código de registro](/help/authentication/registration-code-request.md) solicitud. |

>[!NOTE]
> 
>La información de metadatos del usuario debe estar disponible una vez completado el flujo de autenticación, pero puede actualizarse en el flujo de autorización, según la MVPD y el tipo de metadatos.




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

* *actualizado*: especifica una marca de tiempo UNIX que representa la última vez que se actualizaron los metadatos. El servidor establecerá esta propiedad inicialmente al generar los metadatos durante la fase de autenticación. Las llamadas posteriores (después de actualizar los metadatos) producirán un incremento en la marca de tiempo.
* *datos*: contiene los valores de metadatos reales.
* *cifrado*: una matriz que enumera las propiedades cifradas. Para descifrar un valor de metadatos específico, el programador debe realizar una descodificación Base64 de los metadatos y, a continuación, aplicar una descodificación RSA en el valor resultante, utilizando su propia clave privada (el Adobe cifra los metadatos en el servidor utilizando el certificado público del programador).

En caso de error, el servidor devolverá un objeto XML o JSON que especifica un mensaje de error detallado.

Para obtener más información, consulte [Metadatos del usuario](/help/authentication/user-metadata-feature.md).

[Volver a la referencia de la API de REST](/help/authentication/rest-api-reference.md)
