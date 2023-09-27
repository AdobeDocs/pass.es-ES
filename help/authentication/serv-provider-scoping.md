---
title: Ámbito del proveedor de servicios
description: Ámbito del proveedor de servicios
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 0%

---

# Ámbito del proveedor de servicios {#service-provoider-scoping}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

La implementación predeterminada de una integración de autenticación de Adobe Pass con una MVPD se basa en **Especificación de OLCA**. La sección Requisitos de autenticación de la especificación OLCA (6.5, Identificador del sujeto), indica que es posible indicar el ámbito del proveedor de servicios (SP) para el identificador del sujeto. (El identificador del sujeto es el ID de usuario ofuscado que la MVPD devuelve al SP).  En una integración de autenticación de Adobe Pass, es necesario que las MVPD habiliten el ámbito de las solicitudes de autenticación del SP.

Con la autenticación de Adobe Pass asumiendo la función de SP para el programador, es necesario implementar una personalización que permita el ámbito de SP de la solicitud de autenticación.  Esto debe hacerse para que la MVPD pueda identificar la marca de red que se pasa en la afirmación de SAML al proveedor de identidad (IdP) de la MVPD.  El ámbito se puede implementar de una de las dos maneras descritas en la siguiente sección.

## Ámbito del proveedor de servicios {#service-provider-scoping}

La autenticación de Adobe Pass admite las dos formas siguientes de habilitar el ámbito de SP de las solicitudes de autenticación:

* **El enfoque del emisor de SAML.**  En este enfoque, el &quot;ID de solicitante&quot; se anexa a la cadena del emisor de SAML en la solicitud de autenticación de SAML.

* **Enfoque de propiedad de ámbito personalizado.**  En este enfoque, el &quot;ID de solicitante&quot; se incluye explícitamente como propiedad &quot;Scoping&quot; personalizada en la solicitud de autenticación SAML.

>[!NOTE]
>
>El &quot;ID de solicitante&quot; es el modo en que la autenticación de Adobe Pass hace referencia a la marca de red del programador (por ejemplo, &quot;CNN&quot; es una de las marcas de la red de Turner).

### Enfoque del emisor de SAML {#saml-issuer-approach}

Este enfoque utiliza el SAML `<Issuer>` en la solicitud de autenticación SAML, como se muestra en este fragmento:

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Enfoque de propiedad de ámbito personalizado {#custom-scoping-property-approach}

Este método utiliza una propiedad personalizada denominada &quot;Ámbito&quot;, como se muestra en este fragmento de una solicitud de autenticación SAML:

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
