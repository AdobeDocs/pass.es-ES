---
title: Iniciar cierre de sesión
description: Iniciar cierre de sesión
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Iniciar cierre de sesión {#initiate-logout}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/throttling-mechanism.md)

## Extremos de API de REST {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Quite los tokens AuthN y AuthZ del almacenamiento.


| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logout | Servicio de programador </br></br>o</br></br>de aplicación de streaming | 1. solicitante</br>2.  deviceId (obligatorio)</br>3.  device_info/X-Device-Info (obligatorio)</br>4.  _deviceType_</br> 5.  _deviceUser_ (obsoleto)</br>6.  _appId_ (obsoleto) | DELETE | Ninguno | 204 |


| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| device_info/</br></br>X-Device-Info | Información del dispositivo de streaming.</br></br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br>Ver los detalles completos en [Pasar información de conexión y dispositivo](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br>Si este parámetro está configurado correctamente, ESM ofrece métricas que están [desglosadas por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) al utilizar sin cliente, de modo que se puedan realizar diferentes tipos de análisis, por ejemplo, para Roku, AppleTV, Xbox, etc.</br></br>Ver [Ventajas de usar el parámetro de tipo de dispositivo sin cliente en las métricas de pase ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info reemplazará este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo.</br></br>**Nota**: si se usa, deviceUser debería tener los mismos valores que en la solicitud [Crear código de registro](/help/authentication/registration-code-request.md). |
| _appId_ | El nombre o ID de la aplicación. </br></br>**Nota**: device_info reemplaza este parámetro. Si se usa, `appId` debería tener los mismos valores que en la solicitud [Crear código de registro](/help/authentication/registration-code-request.md). |

>[!IMPORTANT]
> 
>Actualmente, la llamada de cierre de sesión tiene la siguiente limitación: borra los tokens AuthN y AuthZ del almacenamiento (es decir, del lado del programador/autenticación de Adobe Pass), pero **no** llama al extremo de cierre de sesión de MVPD.
