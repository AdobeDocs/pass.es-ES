---
title: Preguntas frecuentes sobre la API de REST V2
description: Preguntas frecuentes sobre la API de REST V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '6460'
ht-degree: 0%

---

# Preguntas frecuentes sobre la API de REST V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Este documento proporciona respuestas generales para las preguntas frecuentes acerca de la adopción de la API de REST V2 de autenticación de Adobe Pass.

Para obtener más información sobre la API de REST V2 en general, consulte la [Información general sobre la API de REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

## Preguntas frecuentes generales {#general-faqs}

Empiece con esta sección si está trabajando en una aplicación que necesita integrar la API de REST V2, ya sea una aplicación nueva o una existente que migre de [API de REST V1](#migration-rest-api-v1-to-rest-api-v2) o [SDK](#migration-sdk-to-rest-api-v2).

Para obtener más información sobre los detalles y pasos de la migración, consulte las secciones siguientes también.

>[!MORELIKETHIS]
>
> * [Preguntas más frecuentes sobre el registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#general-faqs)

### Preguntas frecuentes sobre la fase de registro {#registration-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de registro

Consulte la [documentación de preguntas más frecuentes sobre el registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Preguntas frecuentes sobre la fase de configuración {#configuration-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de configuración

#### 1. ¿Cuál es el propósito de la fase de configuración? {#configuration-phase-faq1}

El propósito de la fase de configuración es proporcionar a la aplicación cliente la lista de MVPD con las que está integrada activamente junto con los detalles de configuración guardados por la autenticación de Adobe Pass para cada MVPD.

La fase de configuración actúa como un paso previo para la fase de autenticación cuando la aplicación cliente necesita pedir al usuario que seleccione su proveedor de TV.

#### 2. ¿Es obligatoria la fase de configuración? {#configuration-phase-faq2}

La fase de configuración no es obligatoria, la aplicación cliente puede omitir esta fase en los siguientes casos:

* El usuario ya se ha autenticado.
* Al usuario se le ofrece acceso temporal a través de la función básica o promocional TempPass.
* La autenticación del usuario ha caducado, pero la aplicación cliente ha almacenado en caché el MVPD seleccionado anteriormente como una opción motivada por la experiencia del usuario, y solo le pide que confirme que sigue siendo un suscriptor de ese MVPD.

#### 3. ¿Qué es una configuración y durante cuánto tiempo es válida? {#configuration-phase-faq3}

La configuración es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

La configuración consiste en una lista de MVPD que se pueden recuperar del punto de conexión de configuración.

La aplicación cliente puede utilizar la configuración para presentar un componente de interfaz de usuario denominado &quot;Selector&quot; cuando requiera al usuario que seleccione su MVPD.

La aplicación cliente debe actualizar la configuración antes de que el usuario pase por la fase de autenticación.

Para obtener más información, consulte la documentación de [Recuperar configuración](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### 4. ¿Puede la aplicación cliente gestionar su propia lista de MVPD? {#configuration-phase-faq4}

La aplicación cliente puede administrar su propia lista de MVPD, pero se recomienda utilizar la configuración proporcionada por la autenticación de Adobe Pass para garantizar que la lista esté actualizada y sea precisa.

La aplicación cliente recibiría un [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) de la API de REST de autenticación de Adobe Pass V2 si el usuario seleccionado MVPD no tiene una integración activa con el [proveedor de servicios](rest-api-v2-glossary.md#service-provider) especificado.

#### 5. ¿Puede la aplicación cliente filtrar la lista de MVPD? {#configuration-phase-faq5}

La aplicación cliente puede filtrar la lista de MVPD proporcionadas en la respuesta de configuración implementando un mecanismo personalizado basado en la propia lógica empresarial y en requisitos como la ubicación del usuario o el historial del usuario de selecciones anteriores.

#### 6. ¿Qué sucede si la integración con un MVPD está deshabilitada y marcada como inactiva? {#configuration-phase-faq6}

Cuando la integración con un MVPD está deshabilitada y marcada como inactiva, MVPD se elimina de la lista de MVPD proporcionadas en respuestas de configuración adicionales y hay dos consecuencias importantes que considerar:

* Los usuarios no autenticados de ese MVPD ya no podrán completar la fase de autenticación con ese MVPD.
* Los usuarios autenticados de ese MVPD ya no podrán completar las fases de preautorización, autorización o cierre de sesión con ese MVPD.

La aplicación cliente recibiría un [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) de la API de REST de autenticación de Adobe Pass V2 si el usuario seleccionado MVPD ya no tiene una integración activa con el [proveedor de servicios](rest-api-v2-glossary.md#service-provider) especificado.

#### 7. ¿Qué sucede si la integración con una MVPD se vuelve a habilitar y se marca como activa? {#configuration-phase-faq7}

Cuando la integración con un MVPD se vuelve a habilitar y se marca como activa, MVPD se vuelve a incluir en la lista de MVPD proporcionadas en respuestas de configuración adicionales y hay dos consecuencias importantes que se deben tener en cuenta:

* Los usuarios no autenticados de ese MVPD podrán completar de nuevo la fase de autenticación mediante ese MVPD.
* Los usuarios autenticados de ese MVPD podrán completar de nuevo las fases de preautorización, autorización o cierre de sesión con ese MVPD.

#### 8. ¿Cómo se habilita o deshabilita la integración con un MVPD? {#configuration-phase-faq8}

Esta operación puede completarla uno de los administradores de su organización o un representante de autenticación de Adobe Pass que actúe en su nombre a través del [Panel de control de TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Preguntas frecuentes sobre la fase de autenticación {#authentication-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de autenticación

#### 1. ¿Cuál es el propósito de la fase de autenticación? {#authentication-phase-faq1}

El propósito de la fase de autenticación es proporcionar a la aplicación cliente la capacidad de verificar la identidad del usuario y obtener información de metadatos del usuario.

La fase de autenticación actúa como un paso previo para la fase de preautorización o la fase de autorización cuando la aplicación cliente necesita reproducir contenido.

#### 2. ¿Cómo puede saber la aplicación cliente si el usuario ya está autenticado? {#authentication-phase-faq2}

La aplicación cliente puede consultar uno de los siguientes extremos capaces de comprobar si un usuario ya está autenticado y devolver información de perfil:

* [API de extremo de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Extremo de perfiles para API de MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Extremo de perfiles para API de código específica (autenticación)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Para obtener más información, consulte los siguientes documentos:

* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 3. ¿Cómo puede la aplicación cliente obtener la información de metadatos del usuario? {#authentication-phase-faq3}

La aplicación cliente puede consultar uno de los siguientes extremos capaces de devolver información de [metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) como parte de la información de perfil:

* [API de extremo de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Extremo de perfiles para API de MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Extremo de perfiles para API de código específica (autenticación)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Para obtener más información, consulte los siguientes documentos:

* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 4. ¿Qué es una sesión de autenticación y durante cuánto tiempo es válida? {#authentication-phase-faq4}

La sesión de autenticación es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

La sesión de autenticación almacena información sobre el proceso de autenticación iniciado que se puede recuperar del punto final de sesiones.

La sesión de autenticación es válida durante un periodo de tiempo limitado y corto especificado en el momento del problema, lo que indica la cantidad de tiempo que el usuario debe completar el proceso de autenticación antes de requerir el reinicio del flujo.

La aplicación cliente puede utilizar la respuesta de sesión de autenticación para saber cómo continuar con el proceso de autenticación. Tenga en cuenta que hay casos en los que no se requiere la autenticación del usuario, como proporcionar acceso temporal, acceso degradado o cuando el usuario ya está autenticado.

Para obtener más información, consulte los siguientes documentos:

* [Crear API de sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reanudar API de sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 5. ¿Qué es un código de autenticación y durante cuánto tiempo es válido? {#authentication-phase-faq5}

El código de autenticación es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

El código de autenticación almacena un valor único generado cuando un usuario inicia el proceso de autenticación e identifica de forma exclusiva la sesión de autenticación de un usuario hasta que el proceso se completa o hasta que caduca la sesión de autenticación.

El código de autenticación es válido durante un periodo de tiempo limitado y corto especificado en el momento de iniciar la sesión de autenticación, lo que indica la cantidad de tiempo que el usuario debe completar el proceso de autenticación antes de requerir el reinicio del flujo.

La aplicación cliente puede utilizar el código de autenticación para permitir al usuario completar o reanudar el proceso de autenticación en el mismo dispositivo o en un segundo dispositivo, teniendo en cuenta que la sesión de autenticación no caducó.

Para obtener más información, consulte los siguientes documentos:

* [Crear API de sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reanudar API de sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 6. ¿Cómo puede saber la aplicación cliente si el usuario ha escrito un código de autenticación válido y que la sesión de autenticación aún no ha caducado? {#authentication-phase-faq6}

La aplicación cliente puede validar el código de autenticación escrito por el usuario en una aplicación secundaria (pantalla) enviando una solicitud al extremo de sesiones responsable de recuperar la información de sesión de autenticación asociada al código de autenticación.

La aplicación cliente recibiría un [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) si el código de autenticación proporcionado estuviera escrito incorrectamente o en caso de que la sesión de autenticación expirara.

Para obtener más información, consulte la documentación de [Recuperar sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### 7. ¿Qué es un perfil y durante cuánto tiempo es válido? {#authentication-phase-faq7}

El perfil es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

El perfil almacena información sobre la validez de autenticación del usuario, información de metadatos y mucho más que se puede recuperar del extremo de perfiles.

La aplicación cliente puede utilizar el perfil para conocer el estado de autenticación del usuario, acceder a la información de metadatos del usuario o encontrar el método utilizado para la autenticación.

Para obtener más información, consulte los siguientes documentos:

* [API de extremo de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Extremo de perfiles para API de MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Extremo de perfiles para API de código específica (autenticación)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

El perfil es válido durante un periodo de tiempo limitado especificado cuando se consulta, lo que indica la cantidad de tiempo que el usuario tiene una autenticación válida antes de requerir volver a pasar por la fase de autenticación.

Este periodo de tiempo limitado conocido como autenticación (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) lo puede ver y cambiar uno de los administradores de su organización o un representante de autenticación de Adobe Pass que actúe en su nombre a través del [panel de control de TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

+++

### Preguntas frecuentes sobre la fase de preautorización {#preauthorization-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de preautorización

#### 1. ¿Cuál es el propósito de la fase de preautorización? {#preauthorization-phase-faq1}

El propósito de la fase de preautorización es proporcionar a la aplicación cliente la capacidad de presentar un subconjunto de recursos de su catálogo al que el usuario tendría derecho de acceso.

La fase de preautorización puede mejorar la experiencia del usuario cuando abre la aplicación cliente por primera vez o navega a una nueva sección.

#### 2. ¿Es obligatoria la fase de preautorización? {#preauthorization-phase-faq2}

La fase de preautorización no es obligatoria, la aplicación cliente puede omitir esta fase si desea presentar un catálogo de recursos sin filtrarlos primero según el derecho del usuario.

#### 3. ¿Qué es una decisión de preautorización? {#preauthorization-phase-faq3}

La preautorización es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization), mientras que el término de decisión también se puede encontrar en el [Glosario](rest-api-v2-glossary.md#decision).

La decisión de preautorización almacena información sobre el resultado de la consulta del proceso de preautorización de MVPD que se puede recuperar del extremo de preautorización de Decisions.

La aplicación cliente puede utilizar las decisiones de preautorización para presentar un subconjunto de recursos al que el proveedor de TV (informativo) permitiría acceder.

Para obtener más información, consulte los siguientes documentos:

* [Recuperar API de decisiones de preautorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4. ¿Por qué en la decisión de preautorización falta un token de medios? {#preauthorization-phase-faq4}

A la decisión de preautorización le falta un token de medios porque la fase de preautorización no debe utilizarse para reproducir recursos, ya que ese es el propósito de la fase de autorización.

#### 5. ¿Qué es un recurso y qué formatos se admiten? {#preauthorization-phase-faq5}

El medio es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

El recurso es un identificador único acordado con las MVPD y está asociado a un contenido que la aplicación cliente podría transmitir.

El identificador único del recurso puede tener dos formatos:

* Un formato de cadena simple, como un identificador único de un canal (marca).
* Un formato RSS (RSS) multimedia que contiene información adicional, como el título, las clasificaciones y los metadatos de control parental.

Para obtener más información, consulte la documentación de [Recursos protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. ¿Para cuántos recursos puede la aplicación cliente obtener una decisión de preautorización a la vez? {#preauthorization-phase-faq6}

La aplicación cliente puede obtener una decisión de preautorización para un número limitado de recursos en una sola solicitud de API, normalmente hasta 5, debido a condiciones impuestas por MVPD.

Este número máximo de recursos se puede ver y cambiar después de ponerse de acuerdo con las MVPD a través del [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass por uno de los administradores de la organización o por un representante de autenticación de Adobe Pass que actúe en su nombre.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### Preguntas frecuentes sobre la fase de autorización {#authorization-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de autorización

#### 1. ¿Cuál es el propósito de la fase de autorización? {#authorization-phase-faq1}

El propósito de la fase de autorización es proporcionar a la aplicación cliente la capacidad de reproducir recursos que el usuario solicita después de validar sus derechos con MVPD.

#### 2. ¿Es obligatoria la fase de autorización? {#authorization-phase-faq2}

La fase de autorización es obligatoria, la aplicación cliente no puede omitir esta fase si desea reproducir recursos que solicita el usuario, ya que requiere verificar con MVPD que el usuario tiene derecho antes de liberar el flujo.

#### 3. ¿Qué es una decisión de autorización y durante cuánto tiempo es válida? {#authorization-phase-faq3}

La autorización es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization), mientras que el término de decisión también se puede encontrar en el [Glosario](rest-api-v2-glossary.md#decision).

La decisión de autorización almacena información sobre el resultado de la consulta del proceso de autorización de MVPD que se puede recuperar del extremo de autorización de decisiones.

La aplicación cliente puede utilizar la decisión de autorización que contiene un token de medios para reproducir el flujo de recursos en caso de que la decisión del proveedor de TV (autoritativa) permita al usuario acceder a él.

Para obtener más información, consulte los siguientes documentos:

* [Recuperar API de decisiones de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

La decisión de autorización es válida durante un periodo de tiempo limitado y corto especificado en el momento de la emisión, lo que indica la cantidad de tiempo que la autenticación de Adobe Pass la almacenará en caché antes de solicitar consultar de nuevo MVPD.

Este periodo de tiempo limitado conocido como autorización (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) lo puede ver y modificar uno de los administradores de su organización o un representante de autenticación de Adobe Pass que actúe en su nombre a través del [panel de control de TVE](rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### 4. ¿Qué es un token de medios y durante cuánto tiempo es válido? {#authorization-phase-faq4}

El token multimedia es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

El token de medios consiste en una cadena firmada enviada en texto no cifrado que se puede recuperar del extremo de autorización de decisiones.

Para obtener más información, consulte la documentación de [Comprobador de token de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

El token de medios es válido durante un periodo de tiempo limitado y corto especificado en el momento del problema, lo que indica la cantidad de tiempo que debe utilizar la aplicación cliente antes de requerir volver a consultar el extremo de autorización de decisiones.

La aplicación cliente puede utilizar el token de medios para reproducir un flujo de recursos en caso de que la decisión del proveedor de TV (autoritativa) permita al usuario acceder a él.

Para obtener más información, consulte los siguientes documentos:

* [Recuperar API de decisiones de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 5. ¿Qué es un recurso y qué formatos se admiten? {#authorization-phase-faq5}

El medio es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

El recurso es un identificador único acordado con las MVPD y está asociado a un contenido que la aplicación cliente podría transmitir.

El identificador único del recurso puede tener dos formatos:

* Un formato de cadena simple, como un identificador único de un canal (marca).
* Un formato RSS (RSS) multimedia que contiene información adicional, como el título, las clasificaciones y los metadatos de control parental.

Para obtener más información, consulte la documentación de [Recursos protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. ¿Para cuántos recursos puede la aplicación cliente obtener una decisión de autorización a la vez? {#authorization-phase-faq6}

La aplicación cliente puede obtener una decisión de autorización para un número limitado de recursos en una sola solicitud de API, normalmente hasta 1, debido a condiciones impuestas por MVPD.

+++

### Preguntas frecuentes sobre la fase de cierre {#logout-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de cierre de sesión

#### 1. ¿Cuál es el propósito de la fase de cierre de sesión? {#logout-phase-faq1}

El propósito de la fase de cierre de sesión es proporcionar a la aplicación cliente la capacidad de finalizar el perfil autenticado del usuario dentro de la autenticación de Adobe Pass si el usuario lo solicita.

+++

### Preguntas frecuentes sobre encabezados {#headers-faqs-general}

+++Preguntas frecuentes sobre encabezados

#### 1. ¿Cómo se calcula el valor del encabezado Autorización? {#headers-faq1}

El encabezado de la solicitud [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contiene el token de acceso `Bearer` requerido por la aplicación cliente para acceder a las API protegidas por Adobe Pass.

El valor del encabezado Autorización debe obtenerse de la Autenticación de Adobe Pass durante la Fase de registro.

Para obtener más información, consulte los siguientes documentos:

* [Información general sobre el registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recuperar API de credenciales de cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar API de token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Si la aplicación cliente está migrando de la API de REST V1 a la API de REST V2, puede seguir utilizando el mismo método para obtener el token de acceso `Bearer` que antes.

#### 2. ¿Cómo calcular el valor del encabezado AP-Device-Identifier? {#headers-faq2}

El encabezado de la solicitud [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contiene el identificador del dispositivo de flujo continuo tal como lo creó la aplicación cliente.

La documentación del encabezado [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) proporciona algunos ejemplos de cómo calcular el valor para distintas plataformas, pero la aplicación cliente puede elegir utilizar un método diferente según su propia lógica y requisitos empresariales.

En caso de que la aplicación cliente esté migrando de la API de REST V1 a la API de REST V2, la aplicación cliente puede seguir utilizando el mismo método para calcular el identificador de dispositivo que antes.

#### 3. ¿Cómo calcular el valor del encabezado X-Device-Info? {#headers-faq3}

El encabezado de la solicitud [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contiene la información del cliente (dispositivo, conexión y aplicación) relacionada con el dispositivo de flujo continuo real.

La documentación del encabezado [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) proporciona algunos ejemplos de cómo calcular el valor para distintas plataformas, pero la aplicación cliente puede elegir utilizar un método diferente según su propia lógica y requisitos empresariales.

En caso de que la aplicación cliente esté migrando de la API de REST V1 a la API de REST V2, la aplicación cliente puede seguir utilizando el mismo método para calcular la información del dispositivo que antes.

+++

## Preguntas frecuentes sobre migración {#migration-faqs}

Continúe con esta sección si está trabajando en una aplicación que necesita migrar una aplicación existente a la API de REST V2.

>[!MORELIKETHIS]
>
> * [Preguntas más frecuentes sobre el registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Preguntas frecuentes generales sobre migración {#general-migration-faqs}

+++Preguntas frecuentes sobre la migración general

#### 1. ¿Debo implementar una nueva aplicación cliente migrada a la API de REST V2 para todos los usuarios a la vez? {#migration-faq1}

No.

La aplicación cliente no es necesaria para implementar una nueva versión que integre la API de REST V2 para todos los usuarios simultáneamente.

La autenticación de Adobe Pass seguirá siendo compatible con versiones de aplicaciones cliente anteriores que integren la API de REST V1 o SDK hasta finales de 2025.

#### 2. ¿Se me requiere para implementar una nueva aplicación cliente migrada a la API de REST V2 en todas las API y flujos a la vez? {#migration-faq2}

Sí.

La aplicación cliente debe implementar una nueva versión que integre la API de REST V2 en todas las API de autenticación de Adobe Pass y flujos simultáneamente.

En caso del flujo de &quot;autenticación de segunda pantalla&quot;, la aplicación cliente debe implementar una nueva versión que integre la API de REST V2 para las aplicaciones [primary](rest-api-v2-glossary.md#primary-application) y [secondary](rest-api-v2-glossary.md#secondary-application) simultáneamente.

La autenticación de Adobe Pass no admitirá implementaciones &quot;híbridas&quot; que integren tanto la API de REST V2 como la API de REST V1/SDK entre API y flujos.

#### 3. ¿Se conservará la autenticación del usuario al actualizar a una nueva aplicación cliente migrada a la API de REST V2? {#migration-faq3}

No.

No se conservará la autenticación de usuario obtenida en versiones anteriores de aplicaciones cliente que integran la API de REST V1 o SDK.

Por lo tanto, se solicitará al usuario que vuelva a autenticarse dentro de la nueva aplicación cliente migrada a la API de REST V2.

#### 4. ¿Los códigos de error mejorados están habilitados de forma predeterminada en la API de REST V2? {#migration-faq4}

Sí.

Las aplicaciones cliente que integran la API REST V2 se benefician de la función de códigos de error mejorados activada de forma predeterminada.

Para obtener más información, consulte la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

+++

### Migración de la API de REST V1 a la API de REST V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Continúe con esta subsección si está trabajando en una aplicación que necesita migrar de la API de REST V1 a la API de REST V2.

#### Preguntas frecuentes sobre la fase de registro {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de registro

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de registro? {#registration-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 no hay cambios de alto nivel con respecto a la fase de registro.

La aplicación cliente puede seguir utilizando el mismo flujo para registrarse con la autenticación de Adobe Pass a través del proceso de [registro dinámico de cliente (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) y obtener un token de acceso.

Para obtener más información, consulte los siguientes documentos:

* [Información general sobre el registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recuperar API de credenciales de cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar API de token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### Preguntas frecuentes sobre la fase de configuración {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de configuración

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de configuración? {#configuration-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPD con integración activa | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Preguntas frecuentes sobre la fase de autenticación {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de autenticación

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de autenticación? {#authentication-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticación) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar código de registro (código de autenticación) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reanudar la autenticación (MVPD) en la segunda pantalla (aplicación) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticación (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [GET <br/> /api/v1/checkauthn (primera pantalla)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (segunda pantalla)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar token de autenticación de usuario (perfil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de preautorización {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de preautorización

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de preautorización? {#preauthorization-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperación de recursos autorizados previamente (decisiones de preautorización) | [GET <br/> /api/v1/preauthorize (primera pantalla)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (segunda pantalla)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de autorización {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de autorización

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de autorización? {#authorization-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autorización de (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recuperar token de autorización (decisión de autorización) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recuperar token de autorización corto (token de medios) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de cierre {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de cierre de sesión

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de cierre de sesión? {#logout-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar cierre de sesión | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de cierre de sesión básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migración de SDK a la API de REST V2 {#migration-sdk-to-rest-api-v2}

Continúe con esta subsección si está trabajando en una aplicación que necesita migrar de SDK a la API de REST V2.

#### Preguntas frecuentes sobre la fase de registro {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de registro

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de registro? {#registration-phase-sdk-to-v2-faq1}

En la migración de los SDK a la API de REST V2 hay cambios de alto nivel que hay que tener en cuenta y que se presentan en las siguientes tablas:

###### AccessEnabler JavaScript SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registro dinámico de clientes (DCR) completo | Proporcionar instrucción de software al constructor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Información general sobre el registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registro dinámico de clientes (DCR) completo | Proporcionar instrucción de software al constructor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Información general sobre el registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registro dinámico de clientes (DCR) completo | Proporcionar instrucción de software al constructor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Información general sobre el registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Registro dinámico de clientes (DCR) completo | Proporcionar instrucción de software al constructor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Información general sobre el registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de configuración {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de configuración

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de configuración? {#configuration-phase-sdk-to-v2-faq1}

En la migración de los SDK a la API de REST V2 hay cambios de alto nivel que hay que tener en cuenta y que se presentan en las siguientes tablas:

###### AccessEnabler JavaScript SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPD con integración activa | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPD con integración activa | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPD con integración activa | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPD con integración activa | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Preguntas frecuentes sobre la fase de autenticación {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de autenticación

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de autenticación? {#authentication-phase-sdk-to-v2-faq1}

En la migración de los SDK a la API de REST V2 hay cambios de alto nivel que hay que tener en cuenta y que se presentan en las siguientes tablas:

###### AccessEnabler JavaScript SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticación) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar código de registro (código de autenticación) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reanudar la autenticación (MVPD) en la segunda pantalla (aplicación) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticación) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar código de registro (código de autenticación) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reanudar la autenticación (MVPD) en la segunda pantalla (aplicación) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/session](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de preautorización {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de preautorización

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de preautorización? {#preauthorization-phase-sdk-to-v2-faq1}

En la migración de los SDK a la API de REST V2 hay cambios de alto nivel que hay que tener en cuenta y que se presentan en las siguientes tablas:

###### AccessEnabler JavaScript SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperación de recursos autorizados previamente (decisiones de preautorización) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperación de recursos autorizados previamente (decisiones de preautorización) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperación de recursos autorizados previamente (decisiones de preautorización) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Ámbito | SDK | API DE REST V2 | Observaciones |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperación de recursos autorizados previamente (decisiones de preautorización) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de autorización {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de autorización

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de autorización? {#authorization-phase-sdk-to-v2-faq1}

En la migración de los SDK a la API de REST V2 hay cambios de alto nivel que hay que tener en cuenta y que se presentan en las siguientes tablas:

###### AccessEnabler JavaScript SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar token de autorización corto (token de medios) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar token de autorización corto (token de medios) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar token de autorización corto (token de medios) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar token de autorización corto (token de medios) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de cierre {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de cierre de sesión

##### 1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de cierre de sesión? {#logout-phase-sdk-to-v2-faq1}

En la migración de los SDK a la API de REST V2 hay cambios de alto nivel que hay que tener en cuenta y que se presentan en las siguientes tablas:

###### AccessEnabler JavaScript SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar cierre de sesión | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de cierre de sesión básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar cierre de sesión | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de cierre de sesión básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar cierre de sesión | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de cierre de sesión básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar cierre de sesión | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de cierre de sesión básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
