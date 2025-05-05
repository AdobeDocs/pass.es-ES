---
title: Manual de inicio de sesión único de Roku (API de REST V2)
description: Manual de inicio de sesión único de Roku (API de REST V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Manual de inicio de sesión único de Roku (API de REST V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia vigente de Adobe Systems. No se permite ningún uso no autorizado.

El Adobe Pass Authentication REST API V2 es compatible con Platform inicio de sesión único (SSO) para los usuarios finales de las aplicaciones cliente que se ejecutan en RokuOS.

Este documento actúa como una extensión de la información general[&#128279;](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existente de la API de REST V2 que proporciona un vista de alto nivel y el documento que describe cómo implementar [Inicio de sesión único mediante flujos de identidad de plataforma.](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)

## Roku inicio de sesión único usar flujos de identidad de plataforma {#cookbook}

Adobe Pass Authentication colabora con Roku para mejorar el experiencia del usuario de inicio de sesión y facilitar el inicio de sesión único (SSO) en las aplicaciones de TV Everywhere para suscriptores TV.

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

* [Encabezado - X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [Encabezado - AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

Para obtener detalles específicos sobre el formato de los encabezados necesarios, póngase en contacto con su representante de Adobe.

### Preguntas más frecuentes {#faqs}

* **¿Cómo funcionará el SSO?**

  SSO funcionará en todas las aplicaciones del programador impulsadas por Adobe Pass Authentication en todos los dispositivos de Roku asociados con el mismo usuario Roku. No todos los MVPD permitirán el inicio de sesión único de Roku.


* **¿Habrá algún cambio en los TTL de autenticación?**

  El primer token de autenticación válido se usará para realizar SSO y, en este caso, todas las demás aplicaciones que se autenticarán a través de SSO usarán el mismo TTL hasta que caduque. Así, al navegar de un aplicación a otro, el segundo aplicación compartirá el TTL del primer aplicación que se autentique.


* **¿Funcionarán otros funcionalidad de Adobe Systems como antes?**

  Todo Adobe Pass Authentication funcionalidad funcionará como antes.


* **¿Hay un proceso de inclusión/exclusión del programador que se beneficia de SSO en la plataforma Roku?**

  Este será un cambio de configuración en el Tablero de TVE de Adobe. Cada programador puede activar o desactivar el SSO en la plataforma Roku para integraciones específicas.


* **¿Cuáles son algunos problemas comunes?**

  Los programadores deben comprobar que sus implementaciones actuales basadas en la API REST de Adobe no impiden el inicio de sesión único de la plataforma de Roku.

  Consulte a continuación una lista de posibles problemas y cómo deben resolverse.

| Problema | Posible causa | Posibles soluciones |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| No se ha enviado ningún encabezado SSO de Roku a Adobe | Usar HTTP en lugar de HTTPS para llamadas a dominios de autenticación de Adobe Pass | Utilizar HTTPS |
| El logotipo de MVPD no se muestra o no se actualiza para los tokens de SSO | IU depende de la almacenamiento local | Las aplicaciones deben actualizar IU (y almacenamiento local, si es necesario) después de comprobar la autenticación. |
| Cierre de sesión activado al no AuthZ | Diseño de aplicación | La aplicación debe actualizarse para no cerrar la sesión nunca entre bastidores |
