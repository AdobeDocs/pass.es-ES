---
title: Autorización de comprobación preliminar de MVPD
description: Autorización de comprobación preliminar de MVPD
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# Autorización de comprobación preliminar de MVPD

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#mvpd-preflight-authz-intro}

La &quot;autorización de comprobación preliminar&quot; es una comprobación de autorización ligera para varios recursos. Los programadores lo utilizan principalmente para decorar sus interfaces de usuario (por ejemplo, para indicar el estado de acceso con iconos de bloqueo y desbloqueo).

Actualmente, la autenticación de Adobe Pass admite la autorización de comprobaciones de dos formas para MVPD, ya sea mediante atributos de respuesta AuthN o a través de una solicitud AuthZ multicanal.  Los siguientes escenarios describen el costo/beneficio de las diferentes maneras de implementar la autorización de comprobaciones:

* **Mejor escenario posible**: la MVPD proporciona la lista de recursos autorizados previamente durante la fase de autorización (autenticación multicanal).
* **Peor escenario posible**: si una MVPD no admite ninguna forma de autorización de varios recursos, el servidor de autenticación de Adobe Pass realiza una llamada de autorización a la MVPD para cada recurso de la lista de recursos. Este escenario tiene un impacto (proporcional al número de recursos) en el tiempo de respuesta de la solicitud de autorización de verificación previa. Puede aumentar la carga en los servidores de Adobe y MVPD, lo que provoca problemas de rendimiento. Además, generará eventos de solicitudes de autorización / respuestas sin la necesidad real de una obra de teatro.
* **Obsoleto**: la MVPD proporciona la lista de recursos preautorizados durante la fase de autenticación, por lo que no se necesitarán llamadas de red, ni siquiera la solicitud de comprobación preliminar, ya que la lista se almacena en caché en el cliente.

Aunque las MVPD no tienen que admitir la autorización de comprobaciones, las secciones siguientes describen algunos métodos de autorización de comprobaciones que puede admitir la autenticación de Adobe Pass antes de volver al peor escenario anterior.

## Comprobación preliminar en AuthN {#preflight-authn}

Este escenario de comprobación preliminar es compatible con OLCA (cables). La sección Especificación 7.5.2 de la Interfaz de autenticación y autorización 1.0 titulada &quot;Declaración de atributo dentro de la aserción de autenticación&quot;, describe cómo una respuesta de autenticación SAML puede contener una lista de recursos preautorizados. Si un IdP admite esto, el servidor de autenticación de Adobe Pass podrá generar la lista de recursos comprobados previamente en el momento de la autenticación y almacenarla en la caché del cliente junto con el token de autenticación. Este método también logra el mejor escenario posible, y no se realizarán llamadas de red cuando el programador llame a checkPreauthorizedResources(), puesto que todo ya está en el cliente.

### Lista de recursos personalizada en instrucción de atributo SAML {#custom-res-saml-attr}

La respuesta de autenticación SAML del IdP incluirá una AttributeStatement que contiene nombres de recursos que AdobePass debe autorizar.  Algunas MVPD proporcionan esto en el siguiente formato:

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

La muestra anterior presenta una lista que contiene dos recursos preautorizados: &quot;MMOD&quot; y &quot;Olympic2012&quot;.

Esto logra efectivamente el mejor escenario posible, y no se realizarán llamadas de red cuando el programador llame a checkPreauthorizedResources(), ya que todo ya está en el cliente.

## Comprobación preliminar multicanal en AuthZ {#preflight-multich-authz}

Esta implementación de comprobaciones también es compatible con OLCA (cables).  La Especificación de la Interfaz de Autenticación y Autorización 1.0 (secciones 7.5.3 y 7.5.4) describe métodos para solicitar información de Autorización de una MVPD utilizando Afirmaciones SAML o XACML. Esta es la forma recomendada de consultar el estado de autorización de las MVPD que no admiten esto como parte del flujo de autenticación. La autenticación de Adobe Pass emite una sola llamada de red a MVPD para recuperar la lista de recursos autorizados.


La autenticación de Adobe Pass recibe la lista de recursos de la aplicación del programador. La integración de MVPD de la autenticación de Adobe Pass puede entonces hacer una llamada de AuthZ que incluya todos esos recursos y luego analizar la respuesta y extraer las múltiples decisiones de permiso/denegación.  El flujo para la comprobación preliminar con el escenario de AuthZ multicanal funciona de la siguiente manera:

1. La aplicación del programador envía una lista de recursos separados por comas a través de la API del cliente de comprobaciones, por ejemplo: &quot;TestChannel1,TestChannel2,TestChannel3&quot;.
1. La llamada de solicitud de AuthZ de comprobación preliminar de MVPD contiene varios recursos y tiene la siguiente estructura:

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## Autorización Personalizada Para Varios Recursos {#custom-authz}

Algunas MVPD tienen puntos finales de autorización que admiten la autorización de varios recursos en una solicitud, pero no entran dentro del escenario descrito en AuthZ multicanal. Estas MVPD específicas requieren un trabajo personalizado.

El Adobe también puede admitir la autorización de varios canales sin realizar cambios en la implementación existente.  Este enfoque debe revisarse entre el Adobe y el equipo técnico de MVPD para garantizar que funciona según lo esperado.

## MVPD compatibles con la autorización de comprobaciones {#mvpds-supp-preflight-authz}

En la tabla siguiente se enumeran las MVPD que admiten la autorización de comprobación preliminar, junto con el tipo de comprobación preliminar que admiten y las limitaciones conocidas:

| Método de comprobación preliminar | MVPD | Notas |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| AuthZ multicanal. | Comcast AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| Alineación de canales en metadatos de usuario | Suddenlink HTC | Todas las integraciones directas de Synacor también pueden admitir este enfoque. |
| Bifurcar y unir | Todos los demás no enumerados anteriormente | El número máximo predeterminado de recursos comprobados = 5. |

<!--
![RelatedInformation]
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Authorization](/help/authentication/authz-usecase.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
>* [MVPD User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Preflight Authorization - Programmer Integration Guide](/help/authentication/preflight-authz.md)
>* [AuthN and AuthZ Interface 1.0 Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank} 
-->
