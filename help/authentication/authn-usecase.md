---
title: Autenticación de MVPD
description: Autenticación de MVPD
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# Autenticación de MVPD {#mvpd-authn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#mvpd-authn-overview}

La función de proveedor de servicios (SP) real la desempeña un programador, pero la autenticación de Adobe Pass sirve como proxy de SP para ese programador. El uso de la autenticación de Adobe Pass como intermediario permite a las MVPD y a los programadores evitar tener que personalizar sus procesos de asignación de derechos caso por caso.

Los pasos siguientes presentan la secuencia de eventos, usando la autenticación de Adobe Pass, cuando un programador solicita autenticación de una MVPD que admite SAML. Tenga en cuenta que el componente Habilitador de acceso a la autenticación de Adobe Pass está activo en el cliente del usuario/suscriptor. A partir de ahí, el Habilitador de acceso facilita todos los pasos del flujo de autenticación.

1. Cuando el usuario solicita acceso al contenido protegido, el Habilitador de acceso inicia la autenticación (AuthN) en nombre del Programador (SP).
1. La aplicación del SP presenta un &quot;Selector de MVPD&quot; al usuario con el fin de obtener su proveedor de TV de pago (MVPD). A continuación, redirige el explorador del usuario al servicio de proveedor de identidad (IdP) de MVPD seleccionado.  Este es &quot;**inicio de sesión iniciado por el programador**&quot;.  La MVPD envía la respuesta del IdP al servicio de consumidor de aserción de SAML del Adobe, donde se procesa.
1. Finalmente, el Habilitador de acceso redirige el explorador de nuevo al sitio del SP, informando al SP del estado (éxito / error) de la solicitud de AuthN.

## La solicitud de autenticación {#authn-req}

Como se presenta en los pasos anteriores, durante el flujo de AuthN una MVPD debe aceptar una solicitud de AuthN basada en SAML y enviar una respuesta de AuthN de SAML.

La [especificación de la interfaz de autenticación y autorización de acceso al contenido en línea (OLCA)](https://www.cablelabs.com/specifications/search?query=&amp;category=&amp;subcat=&amp;doctype=&amp;content=false&amp;archives=false){target=_blanck} presenta una solicitud y una respuesta AuthN estándar. Aunque la autenticación de Adobe Pass no requiere que las MVPD basen su mensajería de asignación de derechos en este estándar, mirar la especificación puede proporcionar perspectiva sobre los atributos clave necesarios para una transacción AuthN.

>[!NOTE]
>
>La solicitud de AuthN que un MVPD recibe con la autenticación de Adobe Pass no contiene una firma digital. Sin embargo, el ejemplo siguiente no muestra una firma por motivos de brevedad. Para ver un ejemplo que muestra una firma digital, vea el ejemplo de [la respuesta de autenticación](#authn-response) en las secciones siguientes.

Ejemplo de solicitud de autenticación SAML:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

La siguiente tabla explica los atributos y las etiquetas que deben estar en una solicitud de autenticación, con los valores predeterminados esperados.

**Detalles de solicitud de autenticación SAML**

| ejemplo:AuthnRequest | &lt;AuthnRequest> emitido por el proveedor de servicios al proveedor de identidad. |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | Este es el punto final de Adobe que se utilizará en la respuesta posterior. Valor predeterminado: **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| Destino | Una referencia de URI que indica la dirección a la que se ha enviado esta solicitud. Esto resulta útil para evitar el reenvío malintencionado de solicitudes a destinatarios no deseados, una protección que requieren algunos enlaces de protocolo. Si está presente, el destinatario real DEBE comprobar que la referencia URI identifica la ubicación en la que se recibió el mensaje. Si no es así, la solicitud DEBE descartarse. Algunos enlaces de protocolo pueden requerir el uso de este atributo. |
| ForceAuthn | El atributo ForceAuthen, si está presente con un valor true, obliga al proveedor de identidad a establecer recientemente esta identidad, en lugar de depender de una sesión existente que pueda tener con el principal. |
| ID | Un identificador para la solicitud. Consulte [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} sección 1.3.4 para obtener más información. |
| IsPassive | Un valor booleano. Si es &quot;true&quot;, el proveedor de identidad y el propio agente de usuario NO DEBEN tomar visiblemente el control de la interfaz de usuario del solicitante e interactuar con el moderador de forma destacable. Si no se proporciona un valor, el valor predeterminado es &quot;false&quot;. |
| IssueInstant | El instante en el que se emite la respuesta. El valor de tiempo está codificado en UTC como se describe en [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Sección 1.3.3. |
| ProtocolBinding | Referencia de URI que identifica un enlace de protocolo SAML que se utilizará al devolver el mensaje &lt;Response>. Consulte [SAMLBind] para obtener más información sobre los enlaces de protocolo y las referencias de URI definidas para ellos. Valor predeterminado: urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST |
| Versión | La versión de esta solicitud. |
| saml:Emisor | Identifica la entidad que generó el mensaje de respuesta. (Para obtener más información sobre este elemento, consulte SAML core 2.0-os Section 2.2.5) |
| ds:Firma | Una firma XML que protege la integridad de y autentica al emisor de la afirmación, tal como se describe en la sección 5 de SAML core 2.0-os |
| ejemplo:NameIDPolicy | Especifica restricciones en el identificador de nombre que se va a utilizar para representar el asunto solicitado. |
| AllowCreate | Valor booleano que se utiliza para indicar si, durante el cumplimiento de la solicitud, el proveedor de identidad puede crear un nuevo identificador para representar al principal. Predeterminado: true |
| Formato | Especifica la referencia URI correspondiente a un formato de identificador de nombre Predeterminado: urn:oasis:names:tc:SAML:2.0:nameid-format:Adobe transitorio recomienda: urn:oasis:names:tc:SAML:2.0:nameid-format:persistent |
| SPNameQualifier | Opcionalmente especifica que el identificador del sujeto de la afirmación se debe devolver (o crear) en el área de nombres de un proveedor de servicios que no sea el solicitante. Predeterminado: http://saml.sp.adobe.adobe.com |

## La respuesta de autenticación {#authn-response}

Una vez recibida y gestionada la solicitud de autenticación, el MVPD debe enviar una respuesta de autenticación.

**Respuesta de autenticación SAML de muestra**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


En el ejemplo anterior, el SP de Adobe espera recuperar el ID de usuario del Subject/NameId. El SP de Adobe se puede configurar para obtener el ID de usuario de un atributo definido personalizado; la respuesta debe contener un elemento como el siguiente:

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**Detalles de respuesta de autenticación SAML**
| ejemplo:Respuesta               | Respuesta recibida por la autenticación de Adobe Pass.                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Destino                  | Una referencia de URI que indica la dirección a la que se ha enviado esta solicitud. Esto resulta útil para evitar el reenvío malintencionado de solicitudes a destinatarios no deseados, una protección que requieren algunos enlaces de protocolo. Si está presente, el destinatario real DEBE comprobar que la referencia URI identifica la ubicación en la que se recibió el mensaje. Si no es así, la solicitud DEBE descartarse. Algunos enlaces de protocolo pueden requerir el uso de este atributo. |
| ID                           | Un identificador para la solicitud. Es de tipo xs:ID y DEBE seguir los requisitos especificados en la sección 1.3.4 de [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} para la exclusividad del identificador. Los valores del atributo ID en una solicitud y el atributo InResponseTo en la respuesta correspondiente DEBEN coincidir.                                                                                                                                                                                         |
| InResponseTo                 | El ID de un mensaje de protocolo SAML en respuesta al cual una entidad autenticadora puede presentar la afirmación. El valor debe ser igual al del atributo de ID enviado en la solicitud de autenticación. Consulte SAML core 2.0-os                                                                                                                                                                                                                                                      |
| IssueInstant                 | El instante en el que se emite la solicitud.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Versión                      | La versión de la solicitud.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| saml:Emisor                  | Identifica la entidad que generó el mensaje de solicitud. (Para más información sobre este elemento, véase la sección 2.2.5. SAML core 2.0-os )                                                                                                                                                                                                                                                                                                                                     |
| muestra:Estado                 | Un código que representa el estado de la solicitud correspondiente.                                                                                                                                                                                                                                                                                                                                                                                                               |
| ejemplo:StatusCode             | Un código que representa el estado de la actividad realizada en respuesta a la solicitud correspondiente.                                                                                                                                                                                                                                                                                                                                                                       |
| saml:Afirmación               | Este tipo especifica la información básica común a todas las aserciones.                                                                                                                                                                                                                                                                                                                                                                                                        |
| ID                           | El identificador de esta afirmación.                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Versión                      | La versión de esta afirmación.                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| IssueInstant                 | El instante en el que se emite la solicitud.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ds:Firma                 | Una firma XML que protege la integridad de y autentica al emisor de la afirmación, tal como se describe en la sección 5 de SAML core 2.0-os                                                                                                                                                                                                                                                                                                                                 |
| ds:SignedInfo                | La estructura de SignedInfo incluye el algoritmo de canonización, un algoritmo de firma y una o más referencias. El elemento SignedInfo puede contener un atributo ID opcional que permitirá que otras firmas y objetos hagan referencia a él. Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                                           |
| ds:CanonicalizationMethod    | CanonicalizationMethod es un elemento requerido que especifica el algoritmo de canonicalización aplicado al elemento SignedInfo antes de realizar los cálculos de firma. Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                                                                                                                           |
| ds:SignatureMethod           | SignatureMethod es un elemento requerido que especifica el algoritmo utilizado para la generación y validación de firmas. Este algoritmo identifica todas las funciones criptográficas involucradas en la operación de firma (por ejemplo, hash, algoritmos de clave pública, MAC, relleno, etc.) Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                              |
| ds:Referencia                 | La referencia es un elemento que puede producirse una o más veces. Especifica un algoritmo de resumen y un valor de resumen y, opcionalmente, un identificador del objeto que se firma, el tipo del objeto o una lista de transformaciones que se aplicarán antes de la digestión. Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                                         |
| ds:Transforms                | El elemento opcional Transforms contiene una lista ordenada de elementos Transform; estos describen cómo obtuvo el firmante el objeto de datos que se digirió. La salida de cada transformación sirve como entrada para la siguiente transformación. La entrada a la primera transformación es el resultado de anular la referencia al atributo URI del elemento Reference. El resultado de la última transformación es la entrada del algoritmo DigestMethod. Consulte Sintaxis y procesamiento de la firma XML          |
| ds:DigestMethod              | DigestMethod es un elemento requerido que identifica el algoritmo de resumen que se aplicará al objeto firmado. Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                                                                                                                                                                                        |
| ds:ValorDeResumen               | DigestValue es un elemento que contiene el valor codificado del compendio. El resumen siempre se codifica con base64. Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                                                                                                                                                                               |
| ds:SignatureValue            | El elemento SignatureValue contiene el valor real de la firma digital; siempre se codifica con base64. Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                                                                                                                                                                                 |
| ds:KeyInfo                   | KeyInfo es un elemento opcional que permite a los destinatarios obtener la clave necesaria para validar la firma. Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                                                                                                                                                                                   |
| ds:X509Datos                  | Un elemento de datos X509Data de KeyInfo contiene uno o varios identificadores de claves o certificados X509. Consulte Sintaxis y procesamiento de la firma XML                                                                                                                                                                                                                                                                                                                                 |
| ds: X509Certificate          | El elemento X509Certificate, que contiene un certificado [X509v3] con codificación base64                                                                                                                                                                                                                                                                                                                                                                                          |
| saml:Asunto                 | El asunto de las declaraciones de la afirmación.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| saml:NameID                  | El elemento &lt;NameID> es de tipo NameIDType (consulte la sección 2.2.2 en SAML core 2.0-os), y se utiliza en varias construcciones de aserción SAML como los elementos &lt;Subject> y &lt;SubjectConfirmation>, y en varios mensajes de protocolo.                                                                                                                                                                                                                                           |
| Formato                       | Una referencia de URI que representa la clasificación de la información de identificadores basados en cadenas.                                                                                                                                                                                                                                                                                                                                                                                    |
| SPNameQualifier              | Califica además un nombre con el nombre de un proveedor de servicios o afiliación de proveedores. Este atributo proporciona un medio adicional para federar los nombres sobre la base de la parte o partes que confían.                                                                                                                                                                                                                                                                      |
| saml:SubjectConfirmation     | Información que permite confirmar al sujeto. Si se proporciona más de una confirmación de sujeto, entonces satisfacer cualquiera de ellos es suficiente para confirmar al sujeto a los efectos de aplicar la afirmación.                                                                                                                                                                                                                                                    |
| saml:SubjectConfirmationData | Información de confirmación adicional que se utilizará en un método de confirmación específico. Por ejemplo, el contenido típico de este elemento podría ser un elemento <!--<ds:KeyInfo>--> tal como se define en la Sintaxis de la firma XML y la especificación de procesamiento                                                                                                                                                                                                                                         |
| NotOnOrAfter                 | Momento en el que el sujeto ya no puede confirmarse.                                                                                                                                                                                                                                                                                                                                                                                                            |
| Destinatario                    | URI que especifica la entidad o ubicación a la que una entidad autenticadora puede presentar la afirmación. Por ejemplo, este atributo podría indicar que la afirmación debe entregarse a un extremo de red concreto para evitar que un intermediario lo redirija a otro lugar.                                                                                                                                                                                   |
| saml:Condiciones              | El elemento &lt;Condition> sirve como punto de extensión para nuevas condiciones.                                                                                                                                                                                                                                                                                                                                                                                                   |
| NotBefore                    | El atributo NotBefore especifica el instante en que comienza el intervalo de validez.                                                                                                                                                                                                                                                                                                                                                                                  |
| saml:AudienceRestriction     | El elemento &lt;AudienceRestriction> especifica que la afirmación está dirigida a una o más audiencias específicas identificadas por elementos &lt;Audience>.                                                                                                                                                                                                                                                                                                                           |
| saml:Audiencia                | Referencia URI que identifica una audiencia a la que va dirigido.                                                                                                                                                                                                                                                                                                                                                                                                                      |
| saml:AuthnStatement          | Una instrucción de autenticación.                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| AuthnInstant                 | Especifica la hora a la que se realizó la autenticación.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| SessionIndex                 | Especifica el índice de una sesión concreta entre el principal identificado por el sujeto y la autoridad autenticadora.                                                                                                                                                                                                                                                                                                                                              |
| saml:AuthnContext            | El contexto utilizado por la autoridad autenticadora hasta el evento de autenticación que dio lugar a esta instrucción, inclusive.                                                                                                                                                                                                                                                                                                                                                 |
| saml:AuthnContextClassRef    | Referencia URI que identifica una clase de contexto de autenticación que describe la declaración de contexto de autenticación que sigue.                                                                                                                                                                                                                                                                                                                                            |
