---
title: Vista previa gratuita para Pase temporal y Pase temporal promocional
description: Vista previa gratuita para Pase temporal y Pase temporal promocional
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 1%

---

# Vista previa gratuita para Pase temporal y Pase temporal promocional {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

Permite la creación de un token de autenticación para Pase temporal y Pase temporal promocional sin necesidad de una segunda pantalla.


| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate/freepreview | Aplicación de streaming</br></br>o</br></br>Servicio de programador | 1. requestor_id (obligatorio)</br>    </br>2.  deviceId (obligatorio)</br>    </br>3.  mso_id (obligatorio)</br>    </br>4.  domain_name (obligatorio)</br>    </br>5.  device_info/X-Device-Info (obligatorio)</br>6.  deviceType</br>    </br>7.  deviceUser (Obsoleto)</br>    </br>8.  appId (obsoleto)</br>    </br>9.  generic_data (opcional) | POST | La respuesta correcta será un 204 Sin contenido, que indica que el token se creó correctamente y está listo para usarse en los flujos de autenticación. | 204 - Sin contenido   </br>400: Solicitud incorrecta |

<div>


| Parámetro de entrada | Descripción |
| --- | --- |
| requestor_id | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| mso_id | Id de MVPD para el que es válida esta operación. |
| domain_name | Nombre de dominio para el que se otorgará un token. Esto se compara con los dominios del proveedor de servicios cuando se concede un token de autorización. |
| device_info/</br></br>X-Device-Info | Información del dispositivo de streaming.</br></br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br></br>Si este parámetro está configurado correctamente, ESM ofrece métricas que [desglosado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) cuando se utiliza sin cliente, de modo que se puedan realizar distintos tipos de análisis para, por ejemplo, Roku, AppleTV, Xbox, etc.</br></br>Consulte [Ventajas de utilizar parámetros de tipo de dispositivo sin cliente ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info reemplaza este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo.</br></br>**Nota**: si se utiliza, deviceUser debe tener los mismos valores que en la variable [Crear código de registro](/help/authentication/registration-code-request.md) solicitud. |
| _appId_ | El nombre o ID de la aplicación. </br></br>**Nota**: device_info reemplaza este parámetro. Si se usa, `appId` debe tener los mismos valores que en la variable [Crear código de registro](/help/authentication/registration-code-request.md) solicitud. |
| generic_data | Se utiliza para limitar el ámbito del token para el pase temporal promocional. |


### [Volver a la referencia de la API de REST](/help/authentication/rest-api-reference.md)
