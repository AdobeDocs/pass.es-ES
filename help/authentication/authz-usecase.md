---
title: Autorización de MVPD
description: Autorización de MVPD
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# Autorización de MVPD

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#mvpd-authz-overview}

La autorización (AuthZ) se realiza mediante comunicaciones de canal de retorno (servidor a servidor) entre un servidor back-end alojado en el Adobe y el punto final de AuthZ de MVPD.

Para las solicitudes de AuthZ, el punto final de autorización debe poder procesar al menos los siguientes parámetros:

* **Uid**. El ID de usuario recibido desde el paso de autenticación.

* **ID de recurso**. Cadena que identifica un recurso de contenido determinado. El programador especifica este ID de recurso y la MVPD debe reforzar las reglas comerciales de estos recursos (por ejemplo, comprobando que el usuario esté suscrito a un canal determinado).

Además de determinar si el usuario está autorizado, la respuesta debe incluir el tiempo de vida (TTL) de esta autorización, es decir, cuando caduca la autorización. Si no se establece el TTL, la solicitud de AuthZ fallará.  Por esta razón, **el TTL es una configuración obligatoria en el lado de la autenticación de Adobe Pass**, para cubrir el caso en que un MVPD no incluya el TTL en su solicitud.

## La solicitud de autorización {#authz-req}

Una solicitud de AuthZ debe incluir un asunto en cuyo nombre se realiza la solicitud, los recursos a los que el asunto intenta acceder, la acción que el asunto intenta realizar en el recurso y el entorno en el que se va a llevar a cabo la operación. En el caso particular de la autenticación de Adobe Pass, estos elementos corresponden a:

| Elemento XACML | Corresponde a |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| Asunto | El principal identificado por la sesión autenticada, al que hace referencia el AttributeValue &quot;token de sujeto&quot; de la afirmación de SAML. |
| Recurso | URI del recurso protegido. |
| Acción | VER. |
| Entorno | Incluye la dirección IP del cliente solicitante, tal como la ve el SP. |



En este punto, el SP debe preparar una consulta de decisión de autorización XACML y enviarla (a través del POST HTTP) al punto de decisión de directiva (PDP) (previamente acordado) para el IdP. A continuación se muestra un ejemplo de una solicitud XACML simple (consulte las especificaciones principales de XACML):

```XML
POST https://authz.site.com/XACML_endpoint
<Request  xmlns="urn:oasis:names:tc:xacm:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os
http://docs.oasis-open.org/xacml/access_control-xacml-2.0-context-schema-os.xsd">
<Subject>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-token"
        DataType="http://www.w3.org/2001/XMLSchema#base64Binary">
      <AttributeValue>{Base64 Data}</AttributeValue>
   </Attribute>
</Subject>
<Resource>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
        DataType="http://www.w3.org/2001/XMLSchema#anyURI">
<AttributeValue>urn:tve:tms:1234</AttributeValue>
   </Attribute>
</Resource>
<Action>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
        DataType="http://www.w3.org/2001/XMLSchema#string">
       <AttributeValue>VIEW</AttributeValue>
   </Attribute>
</Action>
<Environment>
   <Attribute
       AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
       DataType="http://www.w3.org/2001/XMLSchema#string">
      <AttributeValue>1.2.3.4</AttributeValue>
   </Attribute>
</Environment>
</Request>
```


Después de recibir la solicitud de AuthZ, el PDP de la MVPD evalúa la solicitud y determina si se debe permitir al sujeto realizar la acción solicitada en el recurso. A continuación, la MVPD devuelve una respuesta con una decisión, un código de estado y un mensaje, tal como se describe en la respuesta de autorización que aparece a continuación.

## La respuesta de autorización {#authz-response}

La respuesta a la solicitud de AuthZ se produce después de que MVPD evalúe la solicitud y aplique las reglas de negocio solicitadas para determinar si el sujeto puede realizar la acción solicitada en el recurso La respuesta devuelta a la autenticación de Adobe Pass se vuelve a expresar siguiendo la especificación principal de XACML con una decisión, un código de estado, un mensaje y obligaciones que el SP tiene como punto de aplicación de políticas (PEP). El siguiente es un ejemplo de Respuesta:

```XML
<Response xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os">
  <Result>
  <Decision>Permit</Decision>
  <Status>
     <StatusCode Value="urn:oasis:names:tc:xacml:1.0:status:ok"/>
     <StatusMessage>ok</StatusMessage>
  </Status>
  <xacml:Obligations     
          xmlns:xacml="urn:oasis:names:tc:xacml:2.0:policy:schema:os">
     <xacml:Obligation    
              ObligationId="urn:cablelabs:olca:1.0:obligations:log"
              FulfillOn="Permit" />
  </xacml:Obligations>
 </Result>
</Response>
```

A continuación se muestra una lista de las obligaciones de DENEGACIÓN que admite la autenticación de Adobe Pass y que permite a los programadores cumplir:

* **urna:tve:xacml:2.0:obligations:restrict-pc** - El suscriptor no ha superado una comprobación de control parental y el SP debe tomar las medidas adecuadas para restringir el acceso a este contenido.

* **urna:tve:xacml:2.0:obligations:actualización** - El suscriptor no tiene un nivel de suscripción adecuado.  Debe actualizar la suscripción para acceder al contenido.

La autenticación de Adobe Pass admite lo siguiente **PERMITIR** Obligaciones y permite a los programadores cumplirlas:

* **urna:cablelabs:olca:1.0:obligations:registro** - Adobe Pass registra la transacción y puede ponerla a disposición a través del mecanismo de informes acordado.

* **urna:cablelabs:olca:1.0:obligations:volver a autorizar** : la autenticación de Adobe Pass actualiza la autorización de nuevo en n segundos (especificada como argumento de la obligación mediante una asignación de atributos XACML; consulte la especificación principal XACML, sección 5.46).

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
