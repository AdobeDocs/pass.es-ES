---
title: Guía de integración del programador
description: Guía de integración del programador
exl-id: 51461caf-08ef-459e-b284-8f317f45e7b1
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '2119'
ht-degree: 0%

---

# Guía de integración del programador {#programmer-integration-guide}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Esta guía de integración está dirigida a los proveedores de contenido (programadores) que planean integrarse con la autenticación de Adobe® Pass.

En el panorama digital actual, los espectadores pueden acceder a Internet en cualquier momento y en cualquier lugar, y solicitar acceso a su contenido protegido. Es posible que busquen ver un evento único o que busquen los derechos para transmitir una serie completa de televisión que esté transmitiendo.

Antes de conceder acceso al contenido protegido, debe determinar si el visualizador tiene derecho a él. Las preguntas clave incluyen:

* **¿Tiene el visor una suscripción activa con un Distribuidor de programación de vídeo multicanal (MVPD)?**
* **¿Esa suscripción incluye su programación?**

## Autenticación de Adobe Pass para TV en todas partes {#adobe-pass-authentication-for-tv-everywhere}

Para los programadores, la determinación de la asignación de derechos no siempre es sencilla. Las MVPD custodian los datos de identificación de sus clientes y sus privilegios de acceso. Para complicar aún más las cosas, los programadores y los espectadores pueden suscribirse a una amplia variedad de MVPD, cada una de las cuales funciona con sistemas únicos. Estas complejidades hacen que la verificación de los derechos sea técnicamente difícil y consuma muchos recursos.

![Derecho De Usuario Determinado Directamente Por El Programador](../assets/user-ent-by-progr.png){align="center"}

*Derecho De Usuario Determinado Directamente Por El Programador*

La autenticación de Adobe Pass facilita de forma segura las transacciones de derechos entre programadores y MVPD, lo que facilita, simplifica y asegura la provisión de contenido protegido a los visualizadores aptos.

![Derecho de usuario mediado por la autenticación de Adobe Pass](../assets/user-ent-mediatedby-authn.png){align="center"}

*Derecho de usuario mediado por la autenticación de Adobe Pass*

La autenticación de Adobe Pass actúa como un proxy y facilita el flujo de asignación de derechos entre programadores y MVPD al ofrecer interfaces seguras y coherentes para ambas partes.

Para los programadores, la autenticación de Adobe Pass proporciona API como parte de un nivel **Standard** o **Premium**:

* API de autenticación estándar de Adobe Pass:
   * [DCR de API de REST](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [API DE REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* API de autenticación de Adobe Pass Premium:
   * [Restablecer API de pase temporal](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [Función TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [API de degradación](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [Función de degradación](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [API de supervisión del servicio de derechos](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

### Casos de uso {#use-cases}

En esta sección se describen más adelante los casos de uso de integración de programador admitidos por la autenticación de Adobe Pass:

* Programador (TVE) con una red de un solo canal

  Esto permite al programador proporcionar a los visualizadores acceso al contenido desde una red de canal de una sola marca dentro de una aplicación de TVE.

* Aplicación de programador (TVE) con redes multicanal

  Esto permite al programador proporcionar a los visualizadores acceso al contenido de varias redes de canales dentro de una sola aplicación de TVE.

* Programador (TVE) aplicación para eventos especiales

  Esto permite al programador proporcionar a los visualizadores acceso al contenido de eventos especiales que pueden no ser recursos que se encuentran en la base de datos de derechos de MVPD como en los canales normales.

| **Fase** | **Prioridad** | **Caso de uso** | **Documentos** |
|----------------------|--------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Autenticación** | **Alta** | Autenticación | Para obtener más información, consulte los documentos agregados en la sección [Fase de autenticación](#authentication-phase). |
|                      | **Alta** | Autenticación basada en el hogar (HBA) | Para obtener más información, consulte [Autenticación basada en el hogar](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md). |
|                      | **Alta** | Inicio de sesión único (SSO) | Para obtener más información, consulte los documentos agregados en la sección [Inicio de sesión único (SSO)](#sso). |
|                      | **Alta** | Seleccionar MVPD | Para obtener más información, consulte los documentos agregados en la sección [Fase de configuración](#configuration-phase). |
|                      | **Medium** | Página de inicio de sesión de MVPD | Permite a las MVPD proporcionar a las páginas de inicio de sesión una personalización de marca específica para el programador o el proveedor de servicios, incluida la compatibilidad con las preferencias de idioma predeterminadas. |
|                      | **Alta** | Configuración de valores de tiempo de vida (TTL) por plataforma | Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows). |
| **Autorización Previa** | **Baja** | Autorización previa (Autorización de verificación previa) | Para obtener más información, consulte los documentos agregados en la sección [Fase de preautorización](#preauthorization-phase). |
|                      | **Medium** | Códigos de error mejorados | Para obtener más información, consulte [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
| **Autorización** | **Alta** | Autorización | Para obtener más información, consulte los documentos agregados en la sección [Fase de autorización](#authorization-phase). |
|                      | **Alta** | Autorización de canal distinto | Permite a los usuarios acceder al contenido desde varias redes de canales dentro de una única aplicación de TVE. Los programadores pueden hacer llamadas de autorización específicas al canal para verificar el derecho. |
|                      | **Baja** | Autorización de nivel de recurso | Permite a las MVPD recopilar análisis detallados de los activos de contenido individuales durante la autorización. |
|                      | **Medium** | Códigos de error mejorados | Para obtener más información, consulte [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md). |
|                      | **Alta** | Reproductor Federado De Programador: Con Autorización A Nivel De Página | Para obtener más información, consulte [Tokens de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Medium** | Reproductor Federado De Programador: Con Autorización Interna Del Reproductor | Para obtener más información, consulte [Tokens de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Alta** | Syndicated Player: alojado en el portal de MVPD con autorización de nivel de página | Para obtener más información, consulte [Tokens de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md). |
|                      | **Baja** | Control parental: clasificaciones de contenido en solicitudes de autorización | Permite al programador incluir clasificaciones de contenido como parte de la solicitud de autorización a MVPD que son útiles para la autorización de nivel de recurso. |
|                      | **Baja** | Control parental: filtrado de contenido basado en atributos del usuario | Permite al programador comprobar la clasificación de contenido máxima permitida para un usuario y filtrar el contenido disponible en consecuencia. |
| **Cerrar sesión** | **Medium** | Cerrar sesión | Para obtener más información, consulte los documentos agregados en la sección [Fase de cierre de sesión](#logout-phase). |

## Flujo de derecho {#entitlement-flow}

El flujo de derechos es una serie de pasos que una aplicación de Programador (TVE) debe completar para transmitir contenido protegido. El flujo consta de las siguientes fases:

* [Fase de registro](#registration-phase)
* [Fase de configuración](#configuration-phase)
* [Fase de autenticación](#authentication-phase)
* [(Opcional) Fase de preautorización](#preauthorization-phase)
* [Fase de autorización](#authorization-phase)
* [Fase de cierre de sesión](#logout-phase)

En la visita inicial de un usuario a una aplicación de Programador (TVE), el flujo de derechos sigue la secuencia descrita. Sin embargo, en visitas posteriores, la aplicación puede omitir ciertos pasos según el estado del registro o la autenticación y las políticas de visualización aplicables.

Para obtener una exploración detallada del flujo de derechos y sus fases, continúe leyendo este documento y, después, consulte las guías del libro de cocina que lo acompañan para obtener información adicional:

* [Guía de API de REST V2 (de cliente a servidor)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
* [Guía de API de REST V2 (servidor a servidor)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)

>[!NOTE]
>
> La aplicación Programador (TVE) se utiliza en este documento para hacer referencia de forma colectiva a los tipos de aplicaciones que se ejecutan en diferentes plataformas (navegadores, dispositivos móviles, dispositivos conectados a TV, etc.) compatibles con la autenticación de Adobe Pass.

### Fase de registro {#registration-phase}

El propósito de la fase de registro es registrar la aplicación cliente con la autenticación de Adobe Pass a través del proceso de [Registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

El proceso de registro dinámico de clientes (DCR) requiere que la aplicación cliente obtenga un par de credenciales de cliente y recupere un token de acceso como objetivo final de la fase de registro.

**API**

* [Recuperar credenciales de cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Flujos**

* [Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**preguntas más frecuentes**

* [Preguntas frecuentes sobre la fase de registro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#registration-phase-faqs-general).

### Fase de configuración {#configuration-phase}

El propósito de la fase de configuración es proporcionar a la aplicación cliente la lista de MVPD con las que está integrada activamente junto con los detalles de configuración guardados por la autenticación de Adobe Pass para cada MVPD.

La fase de configuración actúa como un paso previo para la fase de autenticación cuando la aplicación cliente necesita pedir al usuario que seleccione su proveedor de TV.

**API**

* [Recuperar la configuración de un proveedor de servicios específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

**preguntas más frecuentes**

* [Preguntas frecuentes sobre la fase de configuración](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general).

>[!TIP]
>
> La aplicación TVE debe incluir una interfaz de selección de MVPD que permita a los usuarios identificar y seleccionar fácilmente a su proveedor de TV.

### Fase de autenticación {#authentication-phase}

El propósito de la fase de autenticación es proporcionar a la aplicación cliente la capacidad de comprobar la identidad del usuario con MVPD y obtener información de metadatos del usuario.

La fase de autenticación actúa como un paso previo para la fase de preautorización o la fase de autorización cuando la aplicación cliente necesita reproducir contenido.

La autenticación correcta genera un perfil vinculado a la aplicación, el dispositivo y el proveedor de servicios, que también contiene información de metadatos del usuario.

**Pasos de alto nivel**

Los siguientes pasos describen los pasos de alto nivel en caso de integración de SAML:

1. **Carga de aplicación del programador (sitio web)**\
   El usuario navega a la aplicación del programador (sitio web), que integra la autenticación de Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Solicitud de contenido protegido**\
   Cuando el usuario intenta acceder a contenido protegido, la aplicación del programador muestra una lista de MVPD para que el usuario las seleccione.

1. **Inicialización de solicitud de autenticación**\
   Al seleccionar MVPD, se redirige al usuario a un servidor de autenticación de Adobe Pass. En este caso, se genera una solicitud de autenticación SAML cifrada para el MVPD seleccionado, en caso de integración con SAML. Esta solicitud se envía en nombre del programador a MVPD. Según el sistema de MVPD, el explorador del usuario se redirige a la página de inicio de sesión de MVPD o se incrusta un iFrame de inicio de sesión en la aplicación del programador.

1. **Inicio de sesión en MVPD**\
   MVPD acepta la solicitud y presenta su interfaz de inicio de sesión, ya sea mediante redireccionamiento o iFrame.

1. **Inicio de sesión y validación de usuario**\
   El usuario inicia sesión con sus credenciales de MVPD. MVPD valida el estado de suscripción del usuario y establece su propia sesión HTTP.

1. **Respuesta de MVPD a la autenticación de Adobe Pass**\
   Una vez finalizada la validación, MVPD genera una respuesta SAML (cifrada) y la devuelve a la autenticación de Adobe Pass.

1. **Generación de perfiles**\
   La autenticación de Adobe Pass verifica la respuesta de SAML, genera un perfil de usuario que se almacena en caché y redirige al usuario de nuevo a la aplicación del programador (sitio web).

**API**

* [Crear sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reanudar sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Recuperar sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Realizar autenticación en el agente de usuario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Recuperación de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**Flujos**

* [Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**preguntas más frecuentes**

* [Preguntas frecuentes sobre la fase de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

>[!TIP]
>
> La aplicación de TVE debe transmitir claramente el estado de autenticación del usuario, por ejemplo, mostrando su logotipo de MVPD junto a iconos &quot;bloqueados&quot; o &quot;desbloqueados&quot; para indicar la accesibilidad de los contenidos protegidos.

#### Inicio de sesión único (SSO) {#single-sign-on}

**API**

* [Recuperar solicitud de autenticación de socio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Crear y recuperar perfiles mediante una respuesta de autenticación de socio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**Flujos**

* [Inicio de sesión único con flujos de socios](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
* [Inicio de sesión único con flujos de identidad de plataforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
* [Inicio de sesión único mediante flujos de token de servicio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)

### (Opcional) Fase de preautorización {#preauthorization-phase}

El propósito de la fase de preautorización es proporcionar a la aplicación cliente la capacidad de presentar un subconjunto de recursos de su catálogo al que el usuario tendría derecho de acceso.

La fase de preautorización puede mejorar la experiencia del usuario cuando abre la aplicación cliente por primera vez o navega a una nueva sección.

**API**

* [Recuperar decisiones de preautorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Flujos**

* [Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**preguntas más frecuentes**

* [Preguntas frecuentes sobre la fase de preautorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general).

>[!TIP]
>
> La aplicación de TVE debe diferenciar claramente el contenido restringido del contenido autorizado mediante indicadores visuales, como un icono &quot;bloqueado&quot; para el contenido restringido y un icono &quot;desbloqueado&quot; para el contenido autorizado.

### Fase de autorización {#authorization-phase}

El propósito de la fase de autorización es proporcionar a la aplicación cliente la capacidad de reproducir recursos que el usuario solicita después de validar sus derechos con MVPD.

La autorización correcta genera una decisión que contiene también un token de medios que se proporciona a la aplicación Programador (TVE) por motivos de seguridad.

**Pasos de alto nivel**

Los pasos siguientes describen los pasos de alto nivel:

1. **Control de identificador de recurso**\
   El contenido protegido se identifica con un [identificador de recurso](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier), que puede ser una cadena simple o una estructura más compleja. Este identificador está predefinido y es acordado por el programador y MVPD. La aplicación del programador envía el identificador de recurso a la API de autenticación de Adobe Pass [REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

1. **Comprobación de autorización de MVPD**\
   El servidor de autenticación de Adobe Pass se comunica con el extremo de autorización de MVPD mediante protocolos estandarizados.

1. **Respuesta de MVPD a la autenticación de Adobe Pass**\
   Una vez finalizada la validación, MVPD confirma que el usuario tiene derecho (o no) a acceder al contenido y envía una respuesta de vuelta a Autenticación de Adobe Pass.

1. **Generación de token de medios y decisión**\
   La autenticación de Adobe Pass comprueba la respuesta, genera una [decisión](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) que se almacena en caché y devuelve la decisión que contiene un token multimedia de nuevo a la aplicación del programador (sitio web).

1. **Verificación de acceso al contenido**\
   La aplicación del programador usa el [Comprobador de token multimedia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) para confirmar que el usuario correcto tiene acceso al contenido correcto. Una vez validado, se concede al usuario acceso para ver el contenido protegido.

**API**

* [Recuperar decisiones de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Flujos**

* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**preguntas más frecuentes**

* [Preguntas frecuentes sobre la fase de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general).

>[!TIP]
>
> La aplicación de TVE debe diferenciar claramente el contenido restringido del contenido autorizado mediante indicadores visuales, como un icono &quot;bloqueado&quot; para el contenido restringido y un icono &quot;desbloqueado&quot; para el contenido autorizado.

### Fase de cierre de sesión {#logout-phase}

El propósito de la fase de cierre de sesión es proporcionar a la aplicación cliente la capacidad de finalizar el perfil autenticado del usuario dentro de la autenticación de Adobe Pass si el usuario lo solicita.

**API**

* [Iniciar el cierre de sesión de un mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Flujos**

* [Flujo de cierre de sesión básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**preguntas más frecuentes**

* [Preguntas frecuentes sobre la fase de cierre de sesión](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general).

#### Cierre de sesión único (SLO) {#single-logout}

**Flujos**

* [Flujo de cierre de sesión único](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)

## Explicación de derechos {#understanding-entitlements}

La solución de autenticación de Adobe Pass gira en torno a la creación de datos específicos de derechos generados al completar correctamente los flujos de trabajo de autenticación y autorización. Estos derechos otorgan acceso a contenido protegido, pero tienen una duración limitada. Una vez que caduca un derecho, debe renovarse reiniciando los procesos de autenticación o autorización.

Para obtener más información sobre los derechos, consulte los siguientes documentos:

* **Perfiles**

  Una vez autenticada correctamente, la autenticación de Adobe Pass crea un perfil autenticado (&quot;de larga duración&quot;) asociado a la aplicación, el dispositivo y el identificador del proveedor de servicios solicitante (identificador del solicitante).

* **[Metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Tras la autenticación correcta (y, en algunos casos, después de la autorización), la autenticación de Adobe Pass recibe metadatos de usuario de MVPD que pueden exponerlos a la aplicación solicitante.

* **[Decisiones](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Una vez concedida la autorización, la autenticación de Adobe Pass crea una decisión de autorización (&quot;larga duración&quot;) asociada a la aplicación, el dispositivo, el identificador del proveedor de servicios (identificador del solicitante) y un recurso protegido específico (identificador de recurso) solicitantes.

* **[Tokens de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Una vez que la autorización se ha realizado correctamente, la autenticación de Adobe Pass crea un token de medios (&quot;de corta duración&quot;) que se asocia a una solicitud de reproducción correcta y proporciona compatibilidad con las prácticas recomendadas del sector para mitigar el fraude (por ejemplo, copiar secuencias).

Los valores de tiempo de vida (&quot;TTL&quot;) para perfiles y decisiones se establecen en función de los acuerdos entre programadores y proveedores de TV de pago, que acuerdan un valor que mejor sirva a todos los involucrados.
