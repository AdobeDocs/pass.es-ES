---
title: Perfiles
description: Perfiles
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Perfiles {#profiles}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Los perfiles se crean mediante la autenticación de Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) cuando un usuario se autentica correctamente con su proveedor de TV de pago (MVPD).

El tipo de perfiles varía en función del método de autenticación utilizado:

* **Normal**

  Creado mediante autenticación básica.

* **Apple SSO**

  Creado mediante inicio de sesión único (SSO) con el marco de trabajo de cuenta de suscriptor de vídeo de Apple.

* **SSO de plataforma**

  Creado mediante el inicio de sesión único (SSO) mediante una identidad de plataforma.

* **Token de servicio SSO**

  Creado mediante el inicio de sesión único (SSO) con un token de servicio.

Los perfiles almacenan datos clave que permiten a las aplicaciones cliente:

* Determine el estado de autenticación del usuario.
* Identifique el método de autenticación utilizado.
* Identifique al proveedor de identidad.
* Obtener acceso a [metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

Los perfiles se almacenan de forma segura en el backend de la autenticación de Adobe Pass y se vinculan a la aplicación, el dispositivo y el identificador del proveedor de servicios solicitante. Siguen siendo válidos durante un período de tiempo limitado, tal como se define en el [Tiempo de vida de autenticación (TTL)](#authentication-ttl-management).

## Administración del tiempo de vida de la autenticación (TTL) {#authentication-ttl-management}

Tiempo de vida de la autenticación (TTL) define cuánto tiempo permanece autenticado un usuario antes de tener que volver a autenticarse. Este periodo de tiempo es limitado y debe acordarse con los representantes de MVPD. Los valores TTL pueden variar en función de lo siguiente:

* Categoría de plataforma (por ejemplo, escritorio, móvil, dispositivos conectados a TV)
* Plataforma específica (por ejemplo, iOS, Android, tvOS, Roku, FireTV)

El TTL de autenticación (authN) se puede ver y cambiar a través del [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass por uno de los administradores de la organización o por un representante de autenticación de Adobe Pass que actúe en su nombre.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

## API DE REST V2 {#rest-api-v2}

Los perfiles se pueden recuperar mediante las siguientes API:

* [Recuperación de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Consulte las secciones **Respuesta** y **Muestras** de las API anteriores para comprender la estructura de los perfiles.

Para obtener más información acerca de cómo y cuándo integrar las API anteriores, consulte los siguientes documentos:

* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [Preguntas frecuentes sobre la fase de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
