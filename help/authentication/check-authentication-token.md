---
title: Comprobar token de autenticación
description: Comprobar token de autenticación
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Comprobar token de autenticación {#check-authentication-token}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/throttling-mechanism.md)

## Extremos de API de REST {#clientless-endpoints}

&lt;reggie_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Indica si el dispositivo tiene un token de autenticación no caducado.

| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/checkauthn | Aplicación de streaming</br></br>o</br></br>Servicio de programador | 1. solicitante (obligatorio)</br>2.  deviceId (obligatorio)</br>3.  device_info/X-Device-Info (obligatorio)</br>4.  _deviceType_ </br>5.  _deviceUser_ (Obsoleto)</br>6.  _appId_ (Obsoleto) | GET | XML o JSON con detalles de error si no se ha realizado correctamente. | 200 - Éxito   </br>403 - Sin éxito |

{style="table-layout:auto"}


| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| device_info/</br></br>X-Device-Info | Información del dispositivo de streaming.</br></br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br>Si este parámetro está configurado correctamente, ESM ofrece métricas que [desglosado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) cuando se utiliza sin cliente, de modo que se puedan realizar distintos tipos de análisis para, por ejemplo, Roku, AppleTV, Xbox, etc.</br></br>Para obtener más información, consulte [Ventajas de utilizar el parámetro deviceType sin cliente en las métricas de autenticación de Adobe Pass ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Nota**: device_info reemplaza este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo. |
| _appId_ | El nombre o ID de la aplicación.</br>**Nota**: device_info reemplaza este parámetro. |

{style="table-layout:auto"}


## Respuesta (si no es correcta) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [Volver a la referencia de la API de REST](/help/authentication/rest-api-reference.md)
