---
title: Atributos de metadatos estándar
description: Atributos de metadatos estándar
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# Atributos de metadatos estándar {#std-metadata-attributes}

Esta página pretende proporcionar una lista exhaustiva de atributos de metadatos que el servicio de Monitorización de concurrencia puede procesar y que pueden utilizarse como base para las políticas que se pueden implementar. Los atributos de metadatos estándar se pueden clasificar de la siguiente manera:

* Atributos incluidos por diseño (enviados en cada llamada de inicialización de sesión, ya que son necesarios en la ruta URL). No se puede realizar ninguna llamada válida sin estos valores.
* Atributos de metadatos: valores que deben pasarse como datos de formulario durante la llamada de inicialización de la sesión (en caso de que las políticas back-end requieran sus valores).

## Atributos requeridos por el diseño {#attr-req-by-design}

La API de supervisión de simultaneidad obliga a los clientes a enviar los siguientes valores como parte de cualquier llamada de inicialización válida: [llamadas de inicio de sesión](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Nombre de campo | Valor de ejemplo | Dónde se usa | Obtenido de |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | Encabezado de autorización | Billete de Zendesk en integración |
| mvpdName | Sample_MVPD | Ruta de URI | Autenticación de Adobe Pass desde el punto final de configuración cuando el usuario selecciona MVPD |
| accountId | 12345 | Ruta de URI | Metadatos de upstreamUserID de autenticación de Adobe Pass después de que el usuario inicie sesión [Metadatos de usuario upstreamUserID - Autenticación de Adobe Pass](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## Atributos de metadatos {#metadata-attr}

Los programadores y las MVPD pueden utilizar los campos de la tabla siguiente para crear políticas que se implementarán en la Monitorización de concurrencia.

Con [API v2.0](http://docs.adobeptime.io/cm-api-v2/), si cualquiera de estos atributos es requerido por las directivas definidas, un intento de inicio de sesión sin ese atributo resultará en una solicitud 400 incorrecta.


| Entidad | Nombre de atributo | Tipo de datos | Descripción | Referencia externa (por ejemplo: EIDR, OATC) | Valor de ejemplo | Reglas de validación |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Empresa de medios | programmerName | cadena | El nombre del programador |                                                   | ProgrammerX |                                                                                   |
| Recurso | canal | cadena | El canal de televisión |                                                   | CanalY |                                                                                   |
|                 | assetId | cadena | El título &quot;descriptivo&quot; o legible para el consumidor que se presentará para este contenido | [Referencia de campos de datos EIDR 2.0](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | type | enumeración | Valor que describe el tipo general de contenido representado por TveItem. Los valores enumerados incluyen: emisión de películaEpisodio noEmisiónEpisodio músicaPremios de vídeoMostrar clip concierto noticias de conferenciaEvento sportingEvent trailer | [Práctica recomendada de fuente de metadatos OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | El campo debe corresponder a uno de los elementos de la enumeración |
|                 | contentType | cadena | Este campo determina si el contenido solicitado está activo o es de VOD | N/D | en directo, vod | live o vod |
|                 | género | cadena | El género del contenido que se transmite. Describe el tipo de programación general | [Fuente de metadatos OATC recomendada](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} Práctica | Comedia | Tipo de género válido |
|                 | duration | número | La duración del elemento de medios en segundos | [Práctica recomendada de fuente de metadatos OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&amp;X-Amz-Algorithm=AWS4-HMAC-SHA256&amp;X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&amp;X-Amz-Date=20230803T144225Z&amp;X-Amz-SignedHeaders=host&amp;X-Amz-Expires=1200&amp;X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | secuencia numérica |
| Dispositivo/Explorador | deviceId | cadena | El identificador único del dispositivo. | [Propiedades de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | cadena | Nombre descriptivo de este dispositivo. |                                                   | Joe&#39;s iPad |                                                                                   |
|                 | marketingName | cadena | El nombre de marketing (o el nombre descriptivo) de un dispositivo | [Propiedades de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | nombre de marketing válido |
|                 | mobileDevice | booleano | El valor es True si el dispositivo está pensado para utilizarse mientras se está desplazando | [Propiedades de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | true, false | true, false |
|                 | deviceModel | cadena | El nombre del modelo del dispositivo, explorador u otro componente | [Propiedades de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | tableta, teléfono, xbox. decodificador | nombre de modelo de dispositivo válido |
|                 | osName | cadena | El sistema operativo que está ejecutando el dispositivo | [Device Atlas: valores de propiedad predefinidos del SO](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Otros Nota: Debe iniciar sesión con un nombre de usuario y una contraseña en Device Atlas para ver los valores de las propiedades | el valor esperado es uno de los valores de las propiedades predefinidas de Device Atlas |
|                 | browserName | cadena | Nombre o tipo de explorador del dispositivo | [Device Atlas: valores de propiedad predefinidos del explorador](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | Nombre o tipo del explorador del dispositivo.  Nota: Debe iniciar sesión con un nombre de usuario y una contraseña en Device Atlas para ver los valores de las propiedades | el valor esperado es uno de los valores de las propiedades predefinidas de Device Atlas |
|                 | browserVersion | cadena | La versión del explorador del dispositivo | [Propiedades de Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | La versión del explorador del dispositivo |                                                                                   |
| Aplicación | applicationName | cadena | El nombre &quot;fácil de usar&quot; o legible para el consumidor de la aplicación | N/D | Sample_Application |                                                                                   |
|                 | applicationId | cadena | Identificador de aplicación que identifica de forma exclusiva una aplicación cliente. | N/D | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | cadena | La plataforma nativa de la aplicación | N/D | ios, android |                                                                                   |
|                 | applicationVersion | cadena | Este valor puede utilizarse con fines de análisis | N/D | 1.0, 2.0 |                                                                                   |
| Asunto | accountId | cadena | El ID de cuenta del sujeto de supervisión de simultaneidad (en el ámbito de MVPD) | N/D | test-account |                                                                                   |
|                 | ContractType | cadena | premium, básico. Los clientes pueden añadir esto como metadatos personalizados y utilizarlos dentro de sus propios dominios | N/D | premium, básico |                                                                                   |
| Usuario | name | cadena | Algunas MVPD proporcionan información relacionada con el usuario específico que reproduce el contenido. | N/D |                                                                                                                                                         |                                                                                   |
|                 | hba | booleano | Identifica si el usuario intenta iniciar el flujo desde su ubicación local | N/D | true, false | true o false |
| Ubicación | continente | cadena | Continente desde el que se origina el ID del dispositivo que envía la solicitud de reproducción | N/D | América del Norte | nombre de continente válido |
|                 | país | cadena | El país desde donde se origina el ID del dispositivo que envía la solicitud de reproducción | N/D | EE. UU. | nombre de país válido |
|                 | state | cadena | El estado desde el que se origina el ID del dispositivo que envía la solicitud de reproducción | N/D | CA | nombre de estado válido |
|                 | ciudad | cadena | Ciudad de la que procede el ID del dispositivo que envía la solicitud de reproducción | N/D | Cupertino | nombre de ciudad válido |
|                 | código postal | número | El código postal desde el que se origina el ID del dispositivo que envía la solicitud de reproducción | N/D | 95014 | código postal válido |
| Transmitir | streamId | cadena | Generado por el servicio CM, no bajo control de cliente. Se utiliza implícitamente cuando se definen reglas de tipo maxstreams. | N/D | N/D | N/D |
|                 | streamCDN | cadena | indica la CDN desde la que se obtuvo el flujo | N/D | N/D | N/D |

## Ejemplos de uso de atributos de metadatos para crear directivas {#examples-metadata-attr}

Los campos de metadatos estándar se pueden utilizar para definir directivas del lado del servidor basadas en sus valores de campo:

* Puede configurar una directiva para que solo se aplique a valores de campo específicos (por ejemplo, una directiva de iOS específica: donde `osType` es `iOS`)
* Puede limitar el número de valores distintos para un campo determinado. Algunos ejemplos son los siguientes:
   * no más de X dispositivos distintos: `HAVING DISTINCT COUNT(deviceId) <= 2`
   * no más de X códigos postales distintos: `HAVING DISTINCT COUNT(zipcode) <= 3`
* Puede limitar el número de flujos activos por valor de campo. Algunos ejemplos son los siguientes:
   * no más de X flujos activos para un solo tipo de dispositivo: `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * no más de X flujos activos para flujos de contenido en vivo: `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Póngase en contacto con el equipo de Supervisión de concurrencia [creando un ticket en Zendesk](mailto:tve-support@adobe.com) e indique qué políticas desea haber implementado.

Puede encontrar más ejemplos de directivas y libros de cocina de integración en los siguientes enlaces:

* [Punto de decisión de política](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [Consola de API: supervisión de concurrencia de Adobe](http://docs.adobeptime.io/cm-api-v2/)
