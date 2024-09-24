---
title: Información general sobre el registro dinámico de clientes
description: Información general sobre el registro dinámico de clientes
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: 7107d4a915113fb237602143aafc350b776c55d6
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Información general sobre el registro dinámico de clientes {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El registro de cliente dinámico representa un mecanismo de autorización definido por [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) y se basa en el marco de autorización OAuth 2.0 descrito en [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

Adobe Pass proporciona un servicio de registro de cliente dinámico que permite acceder a las siguientes API protegidas:

* API de administración de autenticación de Adobe Pass:
   * [Restablecer API de pase temporal](../reset-temp-pass.md)
   * [API de degradación](../degradation-api-overview.md)
   * [API de MVPD proxy](../proxy-mvpd-webserv.md)
   * [API de supervisión del servicio de derechos](../entitlement-service-monitoring-api.md)
* API de REST de autenticación de Adobe Pass:
   * [API DE REST V1](../rest-api-reference.md)
   * [API DE REST V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
* SDK de autenticación de Adobe Pass:
   * [SDK de JavaScript](../javascript-sdk-api-reference.md)
   * [SDK de iOS/tvOS](../iostvos-sdk-api-reference.md)
   * [SDK de Android](../android-sdk-api-reference.md)
   * [FireOS SDK](../amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> El mecanismo de autorización de registro de cliente dinámico reemplaza las soluciones de autenticación de Adobe Pass más antiguas, que están sujetas a descatalogación:
>
> * El mecanismo de ID de solicitante firmado.
> * El mecanismo de lista de dominios.
> * El mecanismo de clave de API.

Con la adopción del registro dinámico de clientes, las principales ventajas son:

* Seguridad mejorada.
* Modelo unificado en todas las plataformas.
* Control preciso del ciclo de vida de la aplicación.

Para obtener más información sobre cómo administrar y utilizar el registro de cliente dinámico, consulte las siguientes secciones.

## Dynamic Client Registration Management {#dynamic-client-registration-management}

El proceso de administración dinámica de registro de clientes permite que las aplicaciones cliente que se ejecutan en plataformas específicas y necesitan acceso a API específicas de autenticación de Adobe Pass se registren a través del [Tablero de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

El Tablero de Adobe Pass TVE es una herramienta para que los clientes (programadores) de autenticación de Adobe Pass administren su configuración y sus datos. Este tablero de autoservicio habilita una serie de funcionalidades que se describen en la [Guía del usuario del tablero de Adobe Pass TVE](../tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md).

Si tiene acceso al [Panel de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication), siga los pasos de las secciones siguientes para crear una aplicación registrada y descargar la instrucción del software.

### Administración de aplicaciones registradas {#manage-registered-applications}

>[!IMPORTANT]
>
> Si no tienes acceso al panel de Adobe Pass TVE, crea un ticket a través de nuestro [Zendesk](https://adobeprimetime.zendesk.com) y pídele a tu administrador técnico de cuentas (TAM) que cree una aplicación registrada y comparta el extracto de software contigo.

Hay dos formas disponibles de crear una aplicación registrada:

* **Nivel de programador**

  El proceso de registro a nivel de programador permite crear una aplicación registrada vinculada a todos los canales disponibles o a un subconjunto seleccionado de canales. Para obtener más información, consulte la documentación de [TVE Dashboard User Guide for Programmers](../tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md).


* **Nivel de canal**

  El proceso de registro a nivel de canal permite crear una aplicación registrada vinculada únicamente al canal seleccionado actualmente. Para obtener más información, consulte la [Guía del usuario del panel de TVE para canales](../tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md).

>[!IMPORTANT]
>
> Se recomienda crear aplicaciones registradas con permisos más específicos y limitados para mejorar la seguridad y evitar el acceso no autorizado. Por lo tanto, cuando cree aplicaciones registradas, considere la posibilidad de utilizar opciones más reducidas para las `channels`, `platforms` y `scopes` asignadas.
>
> Se recomienda crear una nueva aplicación registrada para cada actualización principal de la aplicación cliente para administrar su ciclo de vida y uso. Si es necesario, cree un ticket a través de [Zendesk](https://adobeprimetime.zendesk.com) y pídale al administrador de cuentas técnico (TAM) que revoque una aplicación registrada para bloquear la funcionalidad de una versión específica de la aplicación cliente.

### Administrar instrucciones de software {#manage-software-statements}

>[!IMPORTANT]
>
> Si no tienes acceso al panel de Adobe Pass TVE, crea un ticket a través de nuestro [Zendesk](https://adobeprimetime.zendesk.com) y pídele a tu administrador técnico de cuentas (TAM) que cree una aplicación registrada y comparta el extracto de software contigo.

Antes de descargar un extracto de software, asegúrese de que ha creado una aplicación registrada como se describe en la sección [Administrar aplicaciones registradas](#manage-registered-applications) que cumple los requisitos de la aplicación cliente.

Hay dos formas disponibles de descargar una declaración de software basada en el nivel en el que se creó la aplicación registrada:

* **Nivel de programador**

  Para obtener más información, consulte la documentación de [TVE Dashboard User Guide for Programmers](../tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md).

* **Nivel de canal**

  Para obtener más información, consulte la [Guía del usuario del panel de TVE para canales](../tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md).

La instrucción de software es un token web JSON (`JWT`) que contiene información acerca del software de la aplicación cliente como paquete. Cuando se presenta a la API [Recuperar credenciales del cliente](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md), la instrucción del software se firma digitalmente mediante la firma web JSON (`JWS`).

Para obtener una explicación más detallada sobre qué son las instrucciones de software y cómo funcionan, consulte la [documentación de RFC 7591](https://tools.ietf.org/html/rfc7591).

## Flujo de registro de cliente dinámico  {#dynamic-client-registration-flow}

En resumen, el mecanismo de autorización del registro dinámico de clientes incluye varios pasos:

**Administración**

* Un representante del cliente debe crear una aplicación registrada como se describe en la sección [Administrar aplicaciones registradas](#manage-registered-applications).
* Un representante del cliente debe descargar e incrustar una instrucción de software como se describe en la sección [Administrar instrucciones de software](#manage-software-statements).

**Flujo**

* La aplicación cliente debe obtener las credenciales del cliente como se describe en la [Documentación de la API Retrieve client credentials](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
* La aplicación cliente debe obtener el token de acceso como se describe en la documentación de la API [Recuperar token de acceso](./apis/dynamic-client-registration-apis-retrieve-access-token.md).

Consulte la documentación de [Flujo de registro de cliente dinámico](./flows/dynamic-client-registration-flow.md) para comprender mejor cómo acceder a las API protegidas de Adobe Pass. Además, también puede ver esta grabación de [seminario web](https://my.adobeconnect.com/pzkp8ujrigg1/), que proporciona más contexto e incluye una demostración.
