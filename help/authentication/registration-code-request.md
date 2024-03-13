---
title: Página de registro
description: Página de registro
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Página de registro {#registration-page}

## Extremos de API de REST {#clientless-endpoints}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/throttling-mechanism.md)

&lt;reggie_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#create-reg-code-svc}

Devuelve el código de registro generado aleatoriamente y el URI de la página de inicio de sesión.

| Extremo | Llamado  </br>Por | Entrada   </br>Parámetro | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestor}/regcode</br>Por ejemplo:</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Aplicación de streaming</br>o</br>Servicio de programador | 1. solicitante  </br>    (Componente Ruta)</br>2.  deviceId (con hash)   </br>    (Obligatorio)</br>3.  device_info/X-Device-Info (obligatorio)</br>4.  mvpd (opcional)</br>5.  ttl (opcional)</br>6.  _deviceType_</br> 7.  _deviceUser_ (Obsoleto)</br>8.  _appId_ (Obsoleto) | POST | XML o JSON que contienen un código de registro y detalles de información o error si no se consigue. Consulte esquemas y ejemplos a continuación. | 201 |

{style="table-layout:auto"}

| Parámetro de entrada | Descripción |
| --- | --- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| deviceId | El ID de dispositivo bytes. |
| device_info/</br>X-Device-Info | Información del dispositivo de streaming.</br>**Nota**: Esto PUEDE pasarse a device_info como parámetro de URL, pero debido al tamaño potencial de este parámetro y a las limitaciones en la longitud de una URL de GET, DEBE pasarse como X-Device-Info en el encabezado http. </br>Consulte todos los detalles en [Pasar la información de dispositivo y conexión](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | ID de MVPD para el que es válida esta operación. |
| ttl | El tiempo que debe permanecer este código de registro en segundos.</br>**Nota**: el valor máximo permitido para ttl es de 36000 segundos (10 horas). Los valores más altos dan como resultado una respuesta HTTP 400 (solicitud incorrecta). If `ttl` Si se deja vacío, la autenticación de Adobe Pass establece un valor predeterminado de 30 minutos. |
| _deviceType_ | El tipo de dispositivo (por ejemplo, Roku, PC).</br>Si este parámetro está configurado correctamente, ESM ofrece métricas que [desglosado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) cuando se utiliza sin cliente, de modo que se puedan realizar diferentes tipos de análisis, por ejemplo, Roku, Apple TV y Xbox.</br>Consulte. [Ventajas de utilizar el parámetro de tipo de dispositivo sin cliente en las métricas de pase ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Nota**: device_info reemplaza este parámetro. |
| _deviceUser_ | El identificador de usuario del dispositivo. |
| _appId_ | El nombre o ID de la aplicación. </br>**Nota**: device_info reemplaza este parámetro. |

{style="table-layout:auto"}


>[!CAUTION]
>
>**Dirección IP del dispositivo de streaming**
></br>
>En implementaciones de cliente a servidor, la dirección IP del dispositivo de streaming se envía implícitamente con esta llamada.  En implementaciones de servidor a servidor, donde la variable **regcode** La llamada se realiza mediante el servicio del programador y no mediante el dispositivo de streaming. Se requiere el siguiente encabezado para pasar la dirección IP del dispositivo de streaming:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>donde `<streaming\_device\_ip>` es la dirección IP pública del dispositivo de streaming.
></br></br>
>Ejemplo :</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### Esquema XML de respuesta {#xml-schema}


#### Código de registro XSD {#registration-code-xsd}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

</br>

| Nombre de elemento | Descripción |
| --------------- | ------------------------------------------------------------------------------------ |
| id | UUID generado por el servicio de código de registro |
| código | Código de registro generado por el servicio de código de registro |
| solicitante | ID de solicitante |
| mvpd | ID de Mvpd |
| generado | Marca de tiempo de creación del código de registro (en milisegundos desde el 1 de enero de 1970 GMT) |
| caduca | Marca de tiempo cuando caduca el código de registro (en milisegundos desde el 1 de enero de 1970 GMT) |
| deviceId | ID de dispositivo único (o token XSTS) |
| deviceType | Tipo de dispositivo |
| deviceUser | El usuario ha iniciado sesión en el dispositivo |
| appId | ID de aplicación |
| appVersion | Versión de aplicación |
| registrationURL | URL de la aplicación web de inicio de sesión que se mostrará al usuario final |

{style="table-layout:auto"}


</br>

### Mensaje de error XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### Respuesta de ejemplo {#sample-response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```

**JSON:**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
