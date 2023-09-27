---
title: Iniciar autorización
description: Iniciar autorización
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# Iniciar autorización {#initiate-authorization}

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

Obtiene una respuesta de autorización.

| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authorize | Aplicación de streaming</br></br>o</br></br>Servicio de programador | 1. solicitante (obligatorio)</br>2.  deviceId (obligatorio)</br>3.  recurso (obligatorio)</br>4.  device_info/X-Device-Info (obligatorio)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Obsoleto)</br>7.  _appId_ (Obsoleto)</br>8.  parámetros adicionales (opcional) | GET | XML o JSON que contienen detalles de autorización o detalles de error si no se ha realizado correctamente. Consulte los ejemplos siguientes. | 200 - Éxito  </br>403 - Sin éxito |

{style="table-layout:auto"}

</br>


| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| resource | Cadena que contiene un resourceId (o fragmento MRSS), identifica el contenido solicitado por un usuario y es reconocida por los extremos de autorización de MVPD. |
| device_info/</br></br>X-Device-Info | Información del dispositivo de streaming.</br></br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br>Si este parámetro está configurado correctamente, ESM ofrece métricas que [desglosado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) cuando se utiliza sin cliente, de modo que se puedan realizar distintos tipos de análisis para, por ejemplo, Roku, AppleTV, Xbox, etc.</br></br>Consulte [Ventajas del parámetro de tipo de dispositivo sin cliente en las métricas de pase ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info reemplaza este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo. |
| _appId_ | El nombre o ID de la aplicación. </br></br>**Nota**: device_info reemplaza este parámetro. |
| parámetros adicionales | La llamada también puede contener parámetros opcionales que habilitan otras funcionalidades como:</br></br>* generic_data: permite el uso de [TempPass promocional](/help/authentication/promotional-temp-pass.md)</br></br>Ejemplo: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Dirección IP del dispositivo de streaming**</br>
>En implementaciones de cliente a servidor, la dirección IP del dispositivo de streaming se envía implícitamente con esta llamada.  En implementaciones de servidor a servidor, donde la variable **regcode** La llamada se realiza mediante el servicio de programador y no mediante el dispositivo de streaming. Se requiere el siguiente encabezado para pasar la dirección IP del dispositivo de streaming:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>donde `<streaming\_device\_ip>` es la dirección IP pública del dispositivo de streaming.</br></br>
>Ejemplo :</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Respuesta de ejemplo {#sample-response}

* **Caso 1: Éxito**
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
>Cuando la respuesta procede de una MVPD proxy, puede incluir un elemento adicional denominado `proxyMvpd`.



* **Caso 2: Autorización denegada**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
