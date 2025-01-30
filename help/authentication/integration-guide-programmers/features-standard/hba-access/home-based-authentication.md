---
title: Autenticación basada en el hogar (HBA)
description: Autenticación basada en el hogar (HBA)
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# Autenticación basada en el hogar (HBA) {#home-based-authentication}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La autenticación basada en el hogar (HBA) es una función de TV en todas partes que permite a los suscriptores de TV de pago acceder al contenido de TV en línea sin introducir credenciales de MVPD cuando se conectan a su red doméstica, lo que mejora en gran medida la experiencia de autenticación.

Según el Comité de Tecnología de Autenticación Abierta (OATC):

> &quot;La autenticación automática en el hogar es el proceso mediante el cual un MVPD/OVD utiliza características de la red doméstica (o identificadores accesibles automáticamente entre dispositivos en la red doméstica) para autenticar qué cuenta de suscriptor está asociada con esa red doméstica, lo que elimina la necesidad de que los usuarios introduzcan manualmente credenciales al iniciar una sesión de TVE para acceder a contenido protegido&quot;.

Para obtener más información sobre la autenticación basada en el hogar (HBA) y los estándares relevantes del sector, consulte los siguientes recursos:

>[!MORELIKETHIS]
>
> * [Acceso instantáneo (HBA) por CTAM](http://www.ctamtve.com/instantaccess)
> * [Casos de uso y requisitos de autenticación basada en el hogar por OATC](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf)
> * [Infografía de autenticación basada en el hogar por Adobe](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640)
> * [Autenticación con MVPD habilitadas para OAuth2](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
> * [Autenticación con MVPD habilitadas para SAML](/help/authentication/integration-guide-mvpds/authn-usecase.md)
> * [Metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

## Ventajas de HBA {#hba-benefits}

La autenticación basada en el hogar (HBA) es una función clave que elimina la barrera de inicio de sesión para los espectadores en casa con una suscripción de cable activa. Esta barrera es un desafío significativo para los servicios de TV en todas partes, con casi la mitad de todos los intentos de inicio de sesión resultando en un fracaso.

HBA puede mejorar en gran medida la participación del espectador, proporcionando una experiencia de usuario perfecta y superior para acceder al contenido de TV Everywhere.

## Compatibilidad con HBA {#hba-support}

>[!IMPORTANT]
>
> Si está interesado en aprovechar la funcionalidad HBA, póngase en contacto con su representante de ventas de autenticación de Adobe Pass para obtener más información, ya que ciertos flujos HBA están incluidos en el paquete de flujo de trabajo Premium.

HBA es compatible con varias MVPD integradas con la autenticación de Adobe Pass, pero para beneficiarse de HBA, es posible que tenga que realizar algunos pasos adicionales.

**MVPD de SAML**

Para las MVPD de SAML, el HBA se activa solo en el lado de MVPD.

**MVPD de OAuth2**

Para las MVPD de OAuth2, el HBA se puede activar o desactivar a través del [Tablero de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) siguiendo los pasos de la [Guía del usuario sobre integraciones de tableros de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

### MVPD {#hba-support-mvpds}

**MVPD de SAML**

En la tabla siguiente se ofrece una descripción general de las MVPD habilitadas para SAML que admiten HBA:

| MVPD | ¿Funcionalidad básica disponible? | ¿Envía el indicador en la respuesta de autenticación? |
|--------------|--------------------------------|----------------------------------------|
| DirecTV | Sí | No |
| Plato | Sí | No |
| Espectro | Sí | Sí |
| Cox | Sí | No |
| AT&amp;T | Sí | No |
| Verizon | Sí | Sí |
| Cablevision | Sí | No |
| Mediacom | Sí | No |
| Medio continente | Sí | No |
| Massilon | Sí | No |
| AlticeOne | Sí | Sí |

**MVPD de OAuth2**

En la tabla siguiente se proporciona una descripción general de las MVPD habilitadas para OAuth2 que admiten HBA:

| MVPD | ¿Funcionalidad básica disponible? | ¿Envía el indicador en la respuesta de autenticación? |
|---------|--------------------------------|----------------------------------------|
| Comcast | Sí | Sí |

### Adobe Pass Authentication {#hba-support-adobe-pass-authentication}

En esta sección se describe la experiencia con HBA habilitado y se detalla la compatibilidad que ofrece la autenticación de Adobe Pass, destacando las siguientes características clave:

* **Identificación de HBA:** Capacidad para indicar a los programadores si la autenticación era HBA o no HBA (requiere compatibilidad con MVPD).
* **TTL de autenticación configurables:** Capacidad para establecer diferentes valores de tiempo de vida (TTL) de autenticación para las autenticaciones HBA en comparación con las no HBA (requiere compatibilidad con MVPD).

La siguiente tabla proporciona información general de alto nivel sobre la experiencia del usuario en un HBA y el flujo de autenticación normal (no HBA):

| Tipo de autenticación | Experiencia del usuario |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HBA | Los usuarios seleccionan su MVPD y se autentican automáticamente cuando se conectan a su red doméstica. Una vez que caduca la autenticación, los usuarios deben volver a seleccionar su MVPD, tras lo cual se vuelven a autenticar automáticamente. |
| Normal (no HBA) | Los usuarios seleccionan su MVPD y se les pide que escriban sus credenciales, independientemente de su conexión a una red doméstica. Una vez que caduque la autenticación, los usuarios deben volver a seleccionar su MVPD y volver a introducir sus credenciales. |

**MVPD de SAML**

En la tabla siguiente se ofrece una descripción general de la implementación de HBA en el caso de MVPD habilitadas para SAML:

| Acciones de usuario | Acciones del sistema |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| El usuario intenta reproducir un vídeo.<br/><br/>Se muestra el selector de MVPD.<br/><br/>El usuario selecciona su MVPD y continúa iniciando sesión. | Se realiza una comprobación de antecedentes cuando MVPD aplica sus reglas para la identificación del usuario. Por ejemplo, esto puede implicar la asignación de la dirección IP del usuario a la dirección MAC de módems aprovisionados por el distribuidor o decodificadores conectados a banda ancha. |
| Se muestra una pantalla que persiste durante aproximadamente 3 segundos.<br/><br/>Una página intersticial puede informar al usuario de que ha iniciado sesión automáticamente con su cuenta de MVPD. | La aplicación abre la URL de autenticación en un agente de usuario, iniciando una solicitud HTTP al extremo de autenticación de Adobe Pass.<br/><br/>El extremo de autenticación de Adobe Pass reenvía la solicitud al extremo de autenticación de MVPD a través de una redirección de agente de usuario.<br/><br/>Se espera que MVPD envíe una decisión de autenticación en forma de respuesta SAML que incluya el indicador HBA (`hba_status`) con un valor &quot;true&quot; o &quot;false&quot;.<br/><br/>El servidor de autenticación de Adobe Pass realiza una solicitud al extremo del perfil de usuario de MVPD para exponer el indicador `hba_status` como parte de los [metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md). |
| El usuario está autenticado y ahora puede navegar por el contenido titulado TV en todas partes. | La aplicación recupera el perfil de usuario y puede continuar con el flujo de decisiones para reproducir contenido. |

**MVPD de OAuth2**

La siguiente tabla proporciona una descripción general de la implementación de HBA en el caso de MVPD habilitadas para OAuth2:

| Acciones de usuario | Acciones del sistema |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| El usuario intenta reproducir un vídeo.<br/><br/>Se muestra el selector de MVPD.<br/><br/>El usuario selecciona su MVPD y continúa iniciando sesión. | Se realiza una comprobación de antecedentes cuando MVPD aplica sus reglas para la identificación del usuario. Por ejemplo, esto puede implicar la asignación de la dirección IP del usuario a la dirección MAC de módems aprovisionados por el distribuidor o decodificadores conectados a banda ancha. |
| Se muestra una pantalla que persiste durante aproximadamente 3 segundos.<br/><br/>Una página intersticial puede informar al usuario de que ha iniciado sesión automáticamente con su cuenta de MVPD. | La aplicación abre la URL de autenticación en un agente de usuario, iniciando una solicitud HTTP al extremo de autenticación de Adobe Pass.<br/><br/>El extremo de autenticación de Adobe Pass reenvía la solicitud al extremo de autenticación de MVPD a través de una redirección de agente de usuario.<br/><br/>El extremo de autenticación MVPD envía un código de autorización al extremo de autenticación Adobe Pass.<br/><br/>La autenticación de Adobe Pass usa el código de autorización para solicitar un token de actualización y un token de acceso del extremo de token de MVPD.<br/><br/>Se espera que MVPD envíe una decisión de autenticación que incluya el indicador HBA (`hba_status`) con un valor &quot;true&quot; o &quot;false&quot; como parte de `id_token`.<br/><br/>El servidor de autenticación de Adobe Pass realiza una solicitud al extremo del perfil de usuario de MVPD para exponer el indicador `hba_status` como parte de los [metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).<br/><br/>MVPD establece el TTL del token de actualización en un valor acordado por MVPD y el Adobe establece el TTL de autenticación en un valor menor o igual que el valor del token de actualización. |
| El usuario está autenticado y ahora puede navegar por el contenido titulado TV en todas partes. | La aplicación recupera el perfil de usuario y puede continuar con el flujo de decisiones para reproducir contenido. |

>[!IMPORTANT]
>
> En el flujo HBA para MVPD que utilizan el protocolo de autenticación OAuth 2.0, MVPD emite un token de actualización con un TTL definido por sus requisitos empresariales, mientras que el Adobe emite un token de autenticación HBA con un TTL que no debe exceder el TTL del token de actualización.

## Preguntas frecuentes {#faqs}

1. ¿Por qué existe una distinción entre la implementación de HBA para los protocolos SAML y OAuth2?

   La separación entre la autenticación basada en el hogar (HBA) para los protocolos SAML y OAuth2 existe porque estos protocolos funcionan de forma diferente en términos de mecanismos de autenticación, configuración y flexibilidad de implementación. Para las MVPD de SAML, no se requiere ninguna acción por parte del programador para habilitar HBA, mientras que para las MVPD de OAuth2, HBA se puede activar o desactivar a través del [Tablero de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

1. Cuando HBA está habilitado, ¿siguen necesitando los usuarios introducir su nombre de usuario y contraseña durante su autenticación inicial?

   No, el nombre de usuario y la contraseña no son obligatorios.

1. ¿Cómo se puede hacer cumplir el control parental?

   La autenticación de Adobe Pass puede deshabilitar HBA para integraciones con canales que necesitan la aprobación del control parental. Además, estamos trabajando con OATC en un documento UX que recomienda cómo configurar la experiencia HBA con controles parentales.

1. ¿Los proveedores que admiten HBA utilizan ventanas TTL más cortas para HBA en comparación con la autenticación normal?

   La configuración del TTL es configurable. Se recomienda configurar un TTL más corto para las integraciones de MVPD con HBA a fin de evitar un mal manejo.
