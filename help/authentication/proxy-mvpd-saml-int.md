---
title: Integración de SAML de MVPD proxy
description: Integración de SAML de MVPD proxy
exl-id: 6c83e703-d8cd-476b-8514-05b8230902be
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 1%

---

# Integración de SAML de MVPD proxy

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview-proxy-mvpd-saml-int}

Este documento describe el flujo de autenticación SAML para integraciones de proxy.  Estos flujos dependen de que los datos de configuración proxy estén presentes en la configuración del servidor de autenticación de Adobe Pass. La MVPD proxy envía sus datos de configuración proxy al servidor de autenticación de Adobe Pass a través del servicio web proxy de autenticación de Adobe Pass.

## Datos de configuración de proxy {#proxy-config-data}

Cada proxy MVPD proporciona datos de configuración proxy para sus MVPD proxy al servicio web del proxy de autenticación de Adobe Pass.  Los detalles de que se tratan en la documentación del servicio web proxy.   Para que funcione el flujo SAML AuthN, los datos de configuración de proxy deben incluir las siguientes propiedades:

| Propiedad | Descripción |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID de MVPD | Cadena que representa el MVPD proxy internamente a la autenticación de Adobe Pass.  El Adobe debe confirmarlo como único en el contexto de la autenticación de Adobe Pass. |
| URL de logotipo predeterminado de MVPD | URL de un logotipo que se puede mostrar en una experiencia de Selector de MVPD para el usuario.  Debe utilizar un fondo transparente. |
| Nombre para mostrar de MVPD | Cadena que se utilizará como texto del nombre para mostrar que podría mostrarse con el logotipo, posiblemente como texto alternativo. |



## Flujos de integración de SAML {#saml-int-flows}

Cuando un suscriptor de MVPD visita el sitio o la aplicación de un programador, la autenticación de Adobe Pass responde a una llamada de API desde el sitio o la aplicación con una lista de MVPD activadas para ese programador.  La integración puede ser directa o proxy; no hay distinción entre ellos y el Programador. Esto permite a los programadores presentar la lista de MVPD activas de la manera que consideren adecuada. El suscriptor elige su MVPD y la autenticación de Adobe Pass redirige al suscriptor al proveedor de identidad específico de la MVPD.

En el caso de un proxy MVPD integrado, la integración se realiza entre la autenticación de Adobe Pass y el proxy MVPD. La autenticación de Adobe Pass envía la solicitud de autenticación del usuario al proxy MVPD, y este se encarga de la redirección. Para que el proxy MVPD sepa dónde redirigir la solicitud de autenticación del usuario, la autenticación de Adobe Pass envía un identificador MVPD en la solicitud de autenticación SAML.  Este identificador es el ID de MVPD especificado por el proveedor de proxy mediante el servicio web de proxy como se ha especificado anteriormente.

### Autenticación {#authn-saml-int}

Para que la autenticación de Adobe Pass se integre con una MVPD proxy se necesitará lo siguiente:

* Una MVPD proxy proporcionó una lista de MVPD proxy, insertadas en el servicio web proxy de Adobe

* Metadatos SAML para el proxy MVPD principal

* (Recomendado): la MVPD proxy gestiona la redirección adicional a la URL de la página de inicio de sesión de la MVPD proxy

* El proxy MVPD debe abrir los puertos 443 y 80 para las siguientes IP:
   * 192.150.4.5
   * 192.150.10.200
   * 192.150.11.4
   * 4.53.93.130
   * 193.105.140.131
   * 193.105.140.132
   * 76.74.170.204
   * 63.140.39.4
   * 66.235.132.38
   * 66.235.139.38
   * 66.235.139.168


#### Solicitud y respuesta de SAML de autenticación {#authn-saml-req-resp}

En la solicitud de AUTHn de SAML, las integraciones de proxy incluyen la siguiente propiedad adicional que debe gestionar el proxy MVPD.  Esta propiedad es necesaria para procesar correctamente el solicitante en nombre de la MVPD de proxy y para procesar la experiencia de inicio de sesión correcta. (Esta propiedad se resalta en la solicitud de ejemplo que aparece a continuación).

**Propiedad Scoping** : incluye un elemento IDPEntry que incluye el MVPD_ID y el nombre de MVPD específicos.  Representa el MVPD que el usuario seleccionó realmente del selector del programador y coincide con el MVPD_ID especificado en el servicio web proxy.

Hay una propiedad de ámbito adicional para RequestorID que se puede utilizar para personalizar el inicio de sesión a la marca concreta del programador (si es necesario). O bien, se puede utilizar simplemente para análisis sobre el origen de la solicitud.

En la respuesta de SAML AuthN, la MVPD proxy debe especificar la MVPD proxy como la entidad IdP en las siguientes propiedades:

* Emisor de SAML
* Calificador de nombres


**Solicitud AuthN de muestra**

```XML
<samlp:AuthnRequest
  AssertionConsumerServiceURL="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer"
  Destination="DESTIONATION_URL"
  ForceAuthn="false"
  ID="_4cb70308-b445-462e-b044-f7d0323dde0c"
  IsPassive="false"
  IssueInstant="2012-04-03T15:41:25.884Z"
  ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
  Version="2.0"
  xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
  xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
      https://saml.sp.auth-staging.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        ...........
    </ds:Signature>
    <samlp:NameIDPolicy AllowCreate="true"
                        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                        SPNameQualifier="https://saml.sp.auth-staging.adobe.com"
                        xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" />
    <samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
        <samlp:IDPList xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
            <samlp:IDPEntry Name="MVPD NAME" ProviderID="MVPD_ID"/>
        </samlp:IDPList>
        <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
          RequestorID-Value
        </samlp:RequesterID>
    </samlp:Scoping>
</samlp:AuthnRequest>
```


**Respuesta AuthN de muestra**

```XML
<samlp:Response Destination="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_1d39be60-66de-012f-bfd5-0030488a31a4"
                InResponseTo="_4cb70308-b445-462e-b044-f7d0323dde0c"
                IssueInstant="2012-04-12T15:00:06Z"
                Version="2.0"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" >
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER_VALUE</saml:Issuer>
    <samlp:Status>
        <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="_1d39c280-66de-012f-bfd6-0030488a31a4"
                    IssueInstant="2012-04-12T15:00:06Z"
                    Version="2.0"
                    xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" >
        <saml:Issuer>ISSUER_VALUE</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            ...........
        </ds:Signature>
        <saml:Subject>
            <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                         NameQualifier="IDP_NameQualifier"
                         SPNameQualifier="https://saml.sp.auth-staging.adobe.com">
                oRD6ALr5jlzkofNR1OaSCDbC6GaXV1cq8gF7Eotf
            </saml:NameID>
            <saml:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
                <saml:SubjectConfirmationData
                  InResponseTo="_4cb70308-b445-462e-b044-f7d0323dde0c"
                  NotOnOrAfter="2012-04-12T15:10:06Z"
                  Recipient="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer" />
            </saml:SubjectConfirmation>
        </saml:Subject>
        <saml:Conditions NotBefore="2012-04-12T15:00:06Z"
                         NotOnOrAfter="2012-04-12T15:10:06Z">
            <saml:AudienceRestriction>
                <saml:Audience>https://saml.sp.auth-staging.adobe.com</saml:Audience>
            </saml:AudienceRestriction>
        </saml:Conditions>
        <saml:AuthnStatement AuthnInstant="2012-04-12T15:00:06Z"
                             SessionIndex="f6d15540cf27966115028d35c94eefb9" >
            <saml:AuthnContext>
                <saml:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
                </saml:AuthnContextClassRef>
            </saml:AuthnContext>
        </saml:AuthnStatement>
    </saml:Assertion>
</samlp:Response>
```

### Autorización {#authz-proxy-mvpd-saml-int}

Para la parte de autorización, el MVPD tendría que aceptar para autorización el recurso especificado por el Programador.  En la mayoría de los casos, se trata de un identificador de cadena para la red de canal, como TBS o TNT.

#### Solicitud y respuesta de SAML de autorización {#authz-saml-req-resp}

En la respuesta AuthZ, el EMISOR debe coincidir con el EMISOR de la respuesta SAML, que debe ser el identificador de MVPD por proxy.

**Solicitud XACML de AuthZ de muestra**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/">
<soap11:Header/>
<soap11:Body>
    <xacml-samlp:XACMLAuthzDecisionQuery
            xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
            ID="_c2346a8f2c9cfb205b6b8bf12c2db4d0" IssueInstant="2012-04-12T15:07:51.280Z" Version="2.0">
        <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com
        </saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
                <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                <ds:Reference URI="#_c2346a8f2c9cfb205b6b8bf12c2db4d0">
                    <ds:Transforms>
                        <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                        <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#">
                            <ec:InclusiveNamespaces xmlns:ec="http://www.w3.org/2001/10/xml-exc-c14n#"
                                                    PrefixList="ds saml xacml-context xacml-samlp"/>
                        </ds:Transform>
                    </ds:Transforms>
                    <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                    <ds:DigestValue>GmEkSZI+SDS1i4vV2ApGh0mx1X4=</ds:DigestValue>
                </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>..........</ds:SignatureValue>
        </ds:Signature>
        <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">          
            <xacml-context:Subject
              SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject">
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">
                        oRD6ALr5jlzkofNR1OaSCDbC6GaXV1cq8gF7Eotf
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Subject>
            <xacml-context:Resource>
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">TBS
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Resource>
            <xacml-context:Action>
                <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
                                         DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                                  xsi:type="xacml-context:AttributeValueType">VIEW
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Action>
            <xacml-context:Environment>
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
                  DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">127.0.0.1
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Environment>
        </xacml-context:Request>
    </xacml-samlp:XACMLAuthzDecisionQuery>
</soap11:Body>
</soap11:Envelope>
```

**Respuesta XACML de AuthZ de muestra (autorización concedida)**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap-env:Envelope xmlns:soap-env="http://schemas.xmlsoap.org/soap/envelope/">
    <soap-env:Body>
        <samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_311fa030-66df-012f-bfd7-0030488a31a4"
                        IssueInstant="2012-04-12T15:07:49Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER</saml:Issuer>
            <samlp:Status>
                <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
            </samlp:Status>
            <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
                <ds:SignedInfo>
                    <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                    <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                    <ds:Reference URI="#_311fa030-66df-012f-bfd7-0030488a31a4">
                        <ds:Transforms>
                            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                        </ds:Transforms>
                        <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                        <ds:DigestValue>2+fDBPYnOT1w5dufJZoVsgckRkM=</ds:DigestValue>
                    </ds:Reference>
                </ds:SignedInfo>
                <ds:SignatureValue>..........</ds:SignatureValue>
                <ds:KeyInfo>
                    <ds:X509Data>
                        <ds:X509Certificate>.........</ds:X509Certificate>
                    </ds:X509Data>
                </ds:KeyInfo>
            </ds:Signature>
            <xacml-samlp:Assertion xmlns:xacml-samlp="urn:oasis:names:tc:SAML:2.0:assertion"
                                   ID="_311fa5a0-66df-012f-bfd8-0030488a31a4" IssueInstant="2012-04-12T15:07:49Z"
                                   Version="2.0">
                <xacml-samlp:Issuer>ISSUER</xacml-samlp:Issuer>
                <xacml-samlp:Conditions NotBefore="2012-04-12T15:07:49Z" NotOnOrAfter="2012-04-13T15:07:49Z">
                    <xacml-samlp:AudienceRestriction>
                        <xacml-samlp:Audience>https://saml.sp.auth-staging.adobe.com</xacml-samlp:Audience>
                    </xacml-samlp:AudienceRestriction>
                </xacml-samlp:Conditions>
                <xacml-saml:XACMLAuthzDecisionStatement
                        xmlns:xacml-saml="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:assertion"
                        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                    <xacml-context:Response xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                        <xacml-context:Result ResourceId="TBS">
                            <xacml-context:Decision>Permit</xacml-context:Decision>
                        </xacml-context:Result>
                    </xacml-context:Response>
                </xacml-saml:XACMLAuthzDecisionStatement>
            </xacml-samlp:Assertion>
        </samlp:Response>
    </soap-env:Body>
</soap-env:Envelope>
```

**Respuesta XACML de AuthZ de muestra (autorización denegada)**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap-env:Envelope xmlns:soap-env="http://schemas.xmlsoap.org/soap/envelope/">
<soap-env:Body>
    <samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_69ed8d80-66df-012f-bfda-0030488a31a4"
                    IssueInstant="2012-04-12T15:09:24Z" Version="2.0">
        <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER</saml:Issuer>
        <samlp:Status>
            <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
        </samlp:Status>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
                <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                <ds:Reference URI="#_69ed8d80-66df-012f-bfda-0030488a31a4">
                    <ds:Transforms>
                        <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                        <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                    </ds:Transforms>
                    <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                    <ds:DigestValue>2SXNFA4pb/283wq5FVQdp4Ms5SQ=</ds:DigestValue>
                </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>........</ds:SignatureValue>
            <ds:KeyInfo>
                <ds:X509Data>
                    <ds:X509Certificate>........</ds:X509Certificate>
                </ds:X509Data>
            </ds:KeyInfo>
        </ds:Signature>
        <xacml-samlp:Assertion xmlns:xacml-samlp="urn:oasis:names:tc:SAML:2.0:assertion"
                               ID="_69ed91e0-66df-012f-bfdb-0030488a31a4" IssueInstant="2012-04-12T15:09:24Z"
                               Version="2.0">
            <xacml-samlp:Issuer>ISSUER</xacml-samlp:Issuer>
            <xacml-samlp:Conditions NotBefore="2012-04-12T15:09:24Z" NotOnOrAfter="2012-04-13T15:09:24Z">
                <xacml-samlp:AudienceRestriction>
                    <xacml-samlp:Audience>https://saml.sp.auth-staging.adobe.com</xacml-samlp:Audience>
                </xacml-samlp:AudienceRestriction>
            </xacml-samlp:Conditions>
            <xacml-saml:XACMLAuthzDecisionStatement
                    xmlns:xacml-saml="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:assertion"
                    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                <xacml-context:Response xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                    <xacml-context:Result ResourceId="NOT_Authorized_Resouce">
                        <xacml-context:Decision>Deny</xacml-context:Decision>
                    </xacml-context:Result>
                </xacml-context:Response>
            </xacml-saml:XACMLAuthzDecisionStatement>
        </xacml-samlp:Assertion>
    </samlp:Response>
</soap-env:Body>
</soap-env:Envelope>
```

<!--
>![RelatedInformation]
>* [ProxyMVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Service Provider Scoping](/help/authentication/serv-provider-scoping.md)
-->
