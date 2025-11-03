---
title: Guía de Roku SSO (API de REST V2)
description: Guía de Roku SSO (API de REST V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Guía de Roku SSO (API de REST V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La API de REST de autenticación de Adobe Pass V2 es compatible con el inicio de sesión único (SSO) de Platform para usuarios finales de aplicaciones cliente que se ejecutan en RokuOS.

Este documento actúa como una extensión de la [Información general de la API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existente que proporciona una vista de alto nivel y el documento que describe cómo implementar el [inicio de sesión único mediante flujos de identidad de la plataforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Inicio de sesión único de Roku mediante flujos de identidad de plataforma {#cookbook}

La autenticación de Adobe Pass colabora con Roku para mejorar la experiencia de inicio de sesión del usuario y para facilitar el inicio de sesión único (SSO) en las aplicaciones de TV Everywhere para suscriptores de TV.

### Requisitos previos {#prerequisites}

Antes de continuar con el inicio de sesión único de Roku mediante flujos de identidad de plataforma, asegúrese de que el SSO de Roku esté habilitado. Roku SSO está habilitado de forma predeterminada a menos que el Programador o MVPD soliciten que se deshabilite SSO.

Cada programador puede habilitar o deshabilitar el inicio de sesión único (SSO) en la plataforma Roku para integraciones específicas a través del [panel de Adobe Pass TVE](https://experience.adobe.com/pass/authentication).

### Flujo de trabajo {#workflow}

**Cliente a servidor**

Para aplicaciones de programador que utilizan una arquitectura de cliente a servidor para integrar la API de REST V2, Roku SSO funciona sin problemas sin ninguna modificación.

RokuOS anexa automáticamente dos encabezados HTTP a todas las solicitudes enviadas a los extremos de autenticación de Adobe Pass.

**Servidor a servidor**

Para aplicaciones de programador que utilizan una arquitectura de servidor a servidor para integrar la API de REST V2, el programador debe coordinarse con el equipo de Roku para configurar estos encabezados para que se incluyan en todos los flujos de API dirigidos a su dominio.

Para habilitar el SSO entre aplicaciones y dispositivos, se debe usar el ID de suscriptor proporcionado por Roku en lugar del ID de dispositivo cuando la aplicación lo pase.

Para obtener más información, consulte la siguiente documentación:

* [Encabezado: X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [Encabezado: AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

Para obtener más información sobre el formato de los encabezados necesarios, póngase en contacto con su representante de Adobe.

### Preguntas frecuentes {#faqs}

* **¿Cómo funcionará el SSO?**

  SSO funcionará en todas las aplicaciones de programador con autenticación de Adobe Pass en todos los dispositivos Roku asociados con el mismo usuario de Roku. No todas las MVPD permitirán el SSO de Roku.


* **¿Habrá algún cambio en los TTL de autenticación?**

  El primer token de autenticación válido se utilizará para realizar el SSO y, en este caso, todas las demás aplicaciones que se autenticarán mediante SSO utilizarán el mismo TTL hasta que caduque. Por lo tanto, al navegar de una aplicación a otra, la segunda aplicación compartirá el TTL de la primera aplicación que se autentica.


* **¿Funcionarán otras funciones de Adobe como antes?**

  Toda la funcionalidad de autenticación de Adobe Pass funcionará como antes.


* **¿Hay un proceso de inclusión/exclusión del programador que se beneficia de SSO en la plataforma Roku?**

  Este será un cambio de configuración en el Tablero de TVE de Adobe. Cada programador puede activar o desactivar el SSO en la plataforma Roku para integraciones específicas.


* **¿Cuáles son algunos problemas comunes?**

  Los programadores deben comprobar que sus implementaciones actuales basadas en la API REST de Adobe no impiden el inicio de sesión único de la plataforma de Roku.

  Consulte a continuación una lista de posibles problemas y cómo deben resolverse.

| Problema | Posible causa | Posibles soluciones |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| No se ha enviado ningún encabezado SSO de Roku a Adobe | Usar HTTP en lugar de HTTPS para llamadas a dominios de autenticación de Adobe Pass | Utilizar HTTPS |
| No se muestra el logotipo de MVPD o no se actualiza para los tokens SSO | La IU depende del almacenamiento local | Las aplicaciones deben actualizar la interfaz de usuario (y el almacenamiento local, si es necesario) después de comprobar la autenticación |
| Cierre de sesión activado sin AuthZ | Diseño de aplicación | La aplicación debe actualizarse para no cerrar la sesión nunca entre bastidores |
