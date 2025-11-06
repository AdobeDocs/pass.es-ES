---
title: Servicio web de MVPD proxy
description: Servicio web de MVPD proxy
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---


# Servicio web Proxy MVPD {#proxy-mvpd-wbservice}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Antes de usar el servicio web Proxy MVPD, asegúrese de que se cumplan los siguientes requisitos previos:
>
> * Obtenga las credenciales del cliente como se describe en la [Documentación de la API Retrieve client credentials](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenga el token de acceso como se describe en la [Documentación de la API Recuperar token de acceso](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Consulte la documentación de [Información general sobre el registro dinámico de clientes](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) para obtener más información sobre cómo crear una aplicación registrada y descargar la instrucción de software.

## Información general {#overview-proxy-mvpd-webserv}

Una &quot;MVPD proxy&quot; es una MVPD que, además de gestionar su propia integración con la autenticación de Adobe Pass, también gestiona el proceso de asignación de derechos en nombre de un grupo de &quot;MVPD proxy&quot; asociadas. Esta disposición es transparente para los programadores.

Para implementar la función ProxyMVPD, la autenticación de Adobe Pass proporciona servicios web RESTful, con los cuales ProxyMVPD puede enviar y recuperar listas de ProxiedMVPD. El protocolo utilizado para esta API pública es REST HTTP, con las siguientes suposiciones:

: el MVPD proxy utiliza el método HTTP GET para recuperar la lista de las MVPD integradas actuales.
: el MVPD proxy utiliza el método HTTP POST para actualizar la lista de las MVPD admitidas.

## Servicios de Proxy MVPD {#proxy-mvpd-services}

- [Recuperar MVPD proxy](#retriev-proxied-mvpds)
- [Enviar MVPD proxy](#submit-proxied-mvpds)

### Recuperar MVPD proxy {#retriev-proxied-mvpds}

Recupera la lista actual de MVPD proxy integradas con el MVPD proxy identificado.

| Extremo | Llamado por | Parámetros de solicitud | Encabezados de solicitud | Método HTTP | Respuesta HTTP |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorización (obligatoria) | GET | <ul><li> 200 (ok): la solicitud se procesó correctamente y la respuesta contiene una lista de ProxiedMVPD en formato XML</li><li>401 (sin autorización): indica una de las siguientes opciones:<ul><li>El cliente DEBE solicitar un nuevo access_token</li><li>La solicitud se origina desde una dirección IP que no está presente en la lista de permitidos</li><li>El token no es válido</li></ul></li><li>403 (prohibido): indica que la operación no es compatible con los parámetros proporcionados o que el MVPD proxy no está configurado como proxy o que falta</li><li>405 (método no permitido): se ha utilizado un método HTTP distinto de GET o POST. El método HTTP no es compatible en general o no es compatible con este extremo específico.</li><li>500 (error interno del servidor): se ha producido un error en el lado del servidor durante el proceso de solicitud.</li></ul> |

Ejemplo de curl:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


Ejemplo de respuesta XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### Envío de MVPD proxy {#submit-proxied-mvpds}

Inserta una matriz de MVPD integradas con el MVPD Proxy identificado.

| Extremo | Llamado por | Parámetros de solicitud | Encabezados de solicitud | Método HTTP | Respuesta HTTP |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorización (obligatoria) proxy-mvpds (obligatoria) | PUBLICAR | <ul><li>201 (creado): la inserción se procesó correctamente</li><li>400 (solicitud incorrecta): El servidor no sabe cómo procesar la solicitud:<ul><li>El XML entrante no cumple el esquema publicado en esta especificación</li><li>Los mvpd proxy no tienen ID únicos</li><li>Los requestorIds insertados no existen como la razón del contenedor de otro servlet para el código de respuesta 400</li></ul><li>401 (sin autorización): indica una de las siguientes opciones:<ul><li>El cliente DEBE solicitar un nuevo access_token</li><li>La solicitud se origina desde una dirección IP que no está presente en la lista de permitidos</li><li>El token no es válido</li></ul></li><li>403 (prohibido): indica que la operación no es compatible con los parámetros proporcionados o que el MVPD proxy no está configurado como proxy o que falta</li><li>405 (método no permitido): se ha utilizado un método HTTP distinto de GET o POST. El método HTTP no es compatible en general o no es compatible con este extremo específico.</li><li>500 (error interno del servidor): se ha producido un error en el lado del servidor durante el proceso de solicitud.</li></ul> |

Ejemplo de curl:

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



Ejemplo XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### Frecuencia de contabilización {#posting-frequency}

La autenticación de Adobe Pass recomienda que las MVPD de proxy inserten su lista de MVPD de proxy solo cuando haya un cambio con respecto a la inserción anterior.

### Eliminación de MVPD proxy {#delete-proxied-freqency}

Si ProxyMVPD inserta un registro XML con una lista de ProxiedMVPD vacía, esa lista vacía se almacenará en el sistema como cualquier lista, con lo que se eliminará de forma efectiva la lista anterior.



## Formato XSD {#xsd-format}

Adobe ha definido el siguiente formato aceptado para publicar/recuperar MVPD proxy desde/hacia nuestro servicio web público:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**Notas sobre los elementos:**

-   `id` (obligatorio): el ID de MVPD de proxy debe ser una cadena relevante para el nombre de MVPD, con cualquiera de los siguientes caracteres (ya que se expondrá a los programadores con fines de seguimiento):
-   Cualquier carácter alfanumérico, guion bajo (&quot;_&quot;) y guion (&quot;-&quot;).
-   El idID debe ajustarse a la siguiente expresión regular:
`(a-zA-Z0-9((-)|_)*)`

    Por lo tanto, debe tener al menos un carácter, comenzar con una letra y continuar con cualquier letra, dígito, guión o guion bajo.

-   `iframeSize` (opcional): el elemento iframeSize es opcional y define el tamaño del iFrame si la página de autenticación de MVPD debe estar en un iFrame. De lo contrario, si el elemento iframeSize no está presente, la autenticación se producirá en una página de redirección de explorador completa.
-   `requestorIds` (opcional): Adobe proporcionará los valores de requestorIds. Un requisito es que una MVPD proxy debe integrarse con al menos un requestorId. Si la etiqueta &quot;requestorIds&quot; no está presente en el elemento MVPD proxy, ese MVPD proxy se integrará con todos los solicitantes disponibles integrados en el MVPD proxy.
-   `ProviderID` (opcional): cuando el atributo ProviderID está presente en el elemento de ID, el valor de ProviderID se enviará en la solicitud de autenticación de SAML a la MVPD proxy como ID de Proxied MVPD/SubMVPD (en lugar del valor de ID). En este caso, el valor de id solo se utilizará en el selector de MVPD presentado en la página Programador y, de forma interna, mediante Autenticación de Adobe Pass. La longitud del atributo ProviderID debe estar entre 1 y 128 caracteres.

## Seguridad {#security}

Para que una solicitud se considere válida, debe respetar las siguientes reglas:

: el encabezado de la solicitud debe contener el token de acceso de seguridad Oauth2 obtenido tal como se describe en la documentación de la API [Recuperar token de acceso](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
: la solicitud debe provenir de una dirección IP específica que se haya permitido.
: la solicitud debe enviarse a través del protocolo SSL.

Se ignorará cualquier parámetro presente en el encabezado de la solicitud que no esté enumerado anteriormente.

Ejemplo de curl:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/<proxy-mvpd-identifier>/mvpds"`

## Puntos finales de servicio web de MVPD proxy para los entornos de autenticación de Adobe Pass {#proxy-mvpd-wevserv-endpoints}

- **URL de producción:** https://mgmt.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL de ensayo:** https://mgmt.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL de preproducción de calidad:** https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
- **URL de ensayo previo a la calidad:** https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
