---
title: Vista previa gratuita para Pase temporal y Pase temporal promocional
description: Vista previa gratuita para Pase temporal y Pase temporal promocional
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Heredado) Vista previa gratuita para Pase temporal y Pase temporal promocional {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

Permite la creación de un token de autenticación para Pase temporal y Pase temporal promocional sin necesidad de una segunda pantalla.


| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | Servicio de programador </br></br>o</br></br>de aplicación de streaming | &#x200B;1. requestor_id (obligatorio)</br>    </br>2.  deviceId (obligatorio)</br>    </br>3.  mso_id (obligatorio)</br>    </br>4.  nombre_dominio (obligatorio)</br>    </br>5.  device_info/X-Device-Info (obligatorio)</br>6.  deviceType</br>    </br>7.  deviceUser (Obsoleto)</br>    </br>8.  appId (obsoleto)</br>    </br>9.  generic_data (opcional) | PUBLICAR | La respuesta correcta será un 204 Sin contenido, que indica que el token se creó correctamente y está listo para usarse en los flujos de autenticación. | 204 - Sin contenido   </br>400 - Solicitud incorrecta |

<div>


| Parámetro de entrada | Descripción |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| mso_id | ID de MVPD para el que es válida esta operación. |
| domain_name | Nombre de dominio para el que se otorgará un token. Esto se compara con los dominios del proveedor de servicios cuando se concede un token de autorización. |
| device_info/</br></br>X-Device-Info | Información del dispositivo de streaming.</br></br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br>Ver los detalles completos en [Pasar información de conexión y dispositivo](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br>Si este parámetro está configurado correctamente, ESM ofrece métricas que están [desglosadas por tipo de dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) al utilizar sin cliente, de modo que se puedan realizar diferentes tipos de análisis, por ejemplo, para Roku, AppleTV, Xbox, etc.</br></br>Vea [Ventajas de usar parámetros de tipo de dispositivo sin cliente &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info reemplazará este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo.</br></br>**Nota**: si se usa, deviceUser debería tener los mismos valores que en la solicitud [Crear código de registro](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | El nombre o ID de la aplicación. </br></br>**Nota**: device_info reemplaza este parámetro. Si se usa, `appId` debería tener los mismos valores que en la solicitud [Crear código de registro](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| generic_data | Se utiliza para limitar el ámbito del token para el pase temporal promocional. |


### [Volver a la referencia de API de REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
