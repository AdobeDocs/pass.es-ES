---
title: Intercambio de metadatos de contenido MVPD
description: Intercambio de metadatos de contenido MVPD
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# Intercambio de metadatos de contenido MVPD

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#content-metadat-exchange-overview}

En esta página se describen dos implementaciones estándar que utiliza la autenticación de Adobe Pass para enviar datos estructurados a MVPD en la solicitud de autorización.  Los datos estructurados representan el recurso (el Programador) que realiza la solicitud y, posiblemente, datos adicionales como la clasificación de contenido.

En el lado del programador, la autenticación de Adobe Pass admite recursos de datos MRSS estructurados de la siguiente manera:

1. El programador envía el recurso como una cadena MRSS. La autenticación de Adobe Pass no la codifica en el lado del cliente para dispositivos web o nativos. El MRSS se envía como una cadena normal al servidor de autenticación de Adobe Pass.
1. En el servidor, el MRSS se valida con el esquema predefinido (http://search.yahoo.com/mrss/).  Si la validación se realiza correctamente, la autenticación de Adobe Pass extrae la información de los campos MRSS, incluidos los siguientes:
   * título de canal
   * título de elemento
   * identificador de recurso
   * tipo y valor de clasificación
1. Los valores extraídos de MRSS se utilizan para crear la solicitud de autorización que se pasa a MVPD.

La autenticación de Adobe Pass admite dos métodos para traducir los MRSS a los formatos admitidos por las MVPD:

* **XACML**.  El primer enfoque se ajusta al estándar OLCA.  Utiliza XACML, en el que los valores MRSS se extraen para generar un recurso XACMLR con atributos que se asignan a los elementos MRSS.  A continuación, esto se pasa a MVPD.
* **REST**.  El segundo enfoque se basa en REST.  El MRSS está codificado en Base64 y se pasa como parámetro de URL en la llamada REST.

En ambos métodos, MVPD procesa la solicitud de autorización incluyendo los valores extraídos dentro de su propio flujo lógico y devolviendo una respuesta de autorización.

## Detalles de integración {#integration-details}

* Recurso estructurado XACML basado en OLCA
* Recurso estructurado basado en REST

### Recurso estructurado XACML basado en OLCA {#olca-based-xacml-struc-resource}

La mayoría de las MVPD orientadas a cables utilizan el enfoque basado en XACML, pero aún no admiten el enfoque de datos estructurados completos.  Otras MVPD que admiten XACML toman el Título del canal y lo aceptan para el atributo ResourceID. El ejemplo siguiente muestra el enfoque completo estructurado basado en XACML. El equipo de autenticación de Adobe Pass recomienda que, en el caso de las MVPD que utilizan XACML, pero que aún no admiten funciones como el control parental, adapten su integración XACML al siguiente ejemplo:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11=">
    <soap11:Header/>
    <soap11:Body>
        <xacml-samlp:XACMLAuthzDecisionQuery
                xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
                Destination="
                ID="_f1dd34469c5aeac016760e51dbba007d" IssueInstant="2012-06-26T16:30:24.879Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
                https://saml.sp.auth.adobe.com/
            </saml:Issuer>
            <ds:Signature xmlns:ds=">.......</ds:Signature>
            <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                [....info skipped for brevity....]
                <xacml-context:Resource>
 
        // The MRSS item GUID is passed as the XACML Resource resource-id
                    <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
                        <xacml-context:AttributeValue>DISNEY_GUID_12345</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
        // The MRSS channel title is passed as the XACML Resource tv-network
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:tv-network">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // Adobe doesn't yet support an explicit namespace for the GUID, so we reuse the channel title as the GUID.  
        // We expect to add an explicit namespace later next year pulling it from the GUID scheme attribute.
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:id:namespace">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS item title is passed as the XACML Resource content title
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:title">
                        <xacml-context:AttributeValue>Disney Program X</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS media rating is passed as the XACML Resource content rating 
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:rating:vchip">
                        <xacml-context:AttributeValue>TV-Y</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
                </xacml-context:Resource>
 
                <xacml-context:Action>
                    <xacml-context:Attribute>
                        <xacml-context:AttributeValue>VIEW</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
                </xacml-context:Action>
 
                [.....info skipped for brevity....]
            </xacml-context:Request>
        </xacml-samlp:XACMLAuthzDecisionQuery>
    </soap11:Body>
</soap11:Envelope>
 
//formatted for readability
```

### Recurso estructurado basado en REST {#rest-based-struct-resource}

Algunos MVPD han estandarizado el siguiente protocolo basado en REST para la autorización. Este enfoque tiene todas las características que el enfoque XACML, pero proporciona una implementación &quot;más ligera&quot;.

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
