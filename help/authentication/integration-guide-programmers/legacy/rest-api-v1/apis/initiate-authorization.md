---
title: Iniciar autorización
description: Iniciar autorización
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# (Heredado) Iniciar autorización {#initiate-authorization}

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

&lt;REGGIE_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Obtiene una respuesta de autorización.

| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | Servicio de programador </br></br>o</br></br>de aplicación de streaming | &#x200B;1. solicitante (obligatorio)</br>2.  deviceId (obligatorio)</br>3.  recurso (obligatorio)</br>4.  device_info/X-Device-Info (obligatorio)</br>5.  _deviceType_</br> 6.  _deviceUser_ (obsoleto)</br>7.  _appId_ (obsoleto)</br>8.  parámetros adicionales (opcional) | GET | XML o JSON que contienen detalles de autorización o detalles de error si no se ha realizado correctamente. Consulte los ejemplos siguientes. | 200 - Correcto </br>403 - Sin éxito |

{style="table-layout:auto"}

</br>


| Parámetro de entrada | Descripción |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| resource | Cadena que contiene un resourceId (o fragmento MRSS), identifica el contenido solicitado por un usuario y es reconocida por los extremos de autorización de MVPD. |
| device_info/</br></br>X-Device-Info | Información del dispositivo de streaming.</br></br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br>Ver los detalles completos en [Pasar información de conexión y dispositivo](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br>Si este parámetro está configurado correctamente, ESM ofrece métricas que están [desglosadas por tipo de dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) al utilizar sin cliente, de modo que se puedan realizar diferentes tipos de análisis, por ejemplo, para Roku, AppleTV, Xbox, etc.</br></br>Vea [Ventajas del parámetro de tipo de dispositivo sin cliente en las métricas de pase &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info reemplazará este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo. |
| _appId_ | El nombre o ID de la aplicación. </br></br>**Nota**: device_info reemplaza este parámetro. |
| parámetros adicionales | La llamada también puede contener parámetros opcionales que habilitan otras funcionalidades como:</br></br>* generic_data - habilita el uso de [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br>Ejemplo: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Dirección IP del dispositivo de transmisión**</br>
>En implementaciones de cliente a servidor, la dirección IP del dispositivo de streaming se envía implícitamente con esta llamada.  En implementaciones de servidor a servidor, donde el servicio Programador realiza la llamada **regcode** y no el dispositivo de transmisión, se requiere el siguiente encabezado para pasar la dirección IP del dispositivo de transmisión:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>donde `<streaming\_device\_ip>` es la dirección IP pública del dispositivo de transmisión.</br></br>
>Ejemplo :</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Respuesta de ejemplo {#sample-response}

* **Caso 1: Correcto**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>Cuando la respuesta proviene de un MVPD Proxy, puede incluir un elemento adicional denominado `proxyMvpd`.



* **Caso 2: autorización denegada**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
