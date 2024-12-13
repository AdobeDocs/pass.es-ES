---
title: Supervisión de la autenticación de Adobe Pass
description: Supervisión de la autenticación de Adobe Pass
exl-id: fb000e9d-b5aa-45b1-a914-9e419ec8a4d9
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 0%

---

# (Heredado) Supervisión de la autenticación de Adobe Pass {#monitoring-adobe-primetime-authentication}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#intro}

Los clientes pueden usar [Nagios](http://www.nagios.org) u otras herramientas para comprobar si la autenticación de Adobe Pass está activa o desactivada.

## Monitorización de extremos {#monitoring-endpoints}

### Puntos finales que puede supervisar {#endpoints-to-monitor}

* Punto final de configuración para todas las plataformas: `https://sp.auth.adobe.com/adobe-services/config/[your-config-ID]`- Está disponible en HTTP o HTTPS (según la opción realizada por el desarrollador del proveedor de contenido). Si falta este punto de conexión, significa que el contenido no estará disponible en todas las plataformas y todas las MVPD. Para la API de REST sin cliente también tenemos el siguiente extremo: `https://api.auth.adobe.com/adobe-services/config your-config-ID]`.

* Los siguientes extremos son parte del SDK web de autenticación de Adobe Pass.  Si falta, significa que pay-TVpass está desactivado para todos los programadores y todas las propiedades web:

   * `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`
   * `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`


### Puntos finales que no debe monitorizar {#endpoints-not-monitor}

* `https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer`

  Siempre obtendrá un error 503, ya que este extremo necesita una respuesta SAML de MVPD.

* Otros extremos de derecho: `adobe-services/1.0/authenticate/`, `adobe-services/1.0/deviceShortAuthorize`, `adobe-services/1.0/authorize`

No puede monitorizar estos extremos porque necesitan una carga útil para una respuesta relevante.
