---
title: Preguntas frecuentes sobre la API de REST V2
description: Preguntas frecuentes sobre la API de REST V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: ebe0a53e3ba54c2effdef45c1143deea0e6e57d3
workflow-type: tm+mt
source-wordcount: '9566'
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

### Preguntas frecuentes sobre la fase de registro {#registration-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de registro

Consulte la [documentación de preguntas más frecuentes sobre el registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Preguntas frecuentes sobre la fase de configuración {#configuration-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de configuración

#### &#x200B;1. ¿Cuál es el propósito de la fase de configuración? {#configuration-phase-faq1}

El propósito de la fase de configuración es proporcionar a la aplicación cliente la lista de MVPD con las que está integrada activamente junto con los detalles de configuración (por ejemplo, `id`, `displayName`, `logoUrl`, etc.) guardados por la autenticación de Adobe Pass para cada MVPD.

La fase de configuración actúa como un paso previo para la fase de autenticación cuando la aplicación cliente necesita pedir al usuario que seleccione su proveedor de TV.

#### &#x200B;2. ¿Es obligatoria la fase de configuración? {#configuration-phase-faq2}

La fase de configuración no es obligatoria, la aplicación cliente debe recuperar la configuración solo cuando el usuario necesite seleccionar su MVPD para autenticarse o volver a autenticarse.

La aplicación cliente puede omitir esta fase en los siguientes casos:

* El usuario ya se ha autenticado.
* Se ofrece acceso temporal al usuario a través de la característica [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) básica o promocional.
* La autenticación del usuario ha caducado, pero la aplicación cliente ha almacenado en caché el MVPD seleccionado anteriormente como una opción motivada por la experiencia del usuario, y solo le pide que confirme que sigue siendo un suscriptor de ese MVPD.

#### &#x200B;3. ¿Qué es una configuración y durante cuánto tiempo es válida? {#configuration-phase-faq3}

La configuración es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

La configuración contiene una lista de MVPD definidas por los siguientes atributos `id`, `displayName`, `logoUrl`, etc., que se pueden recuperar del extremo [Configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

La aplicación cliente solo debe recuperar la configuración cuando el usuario necesite seleccionar su MVPD para autenticarse o volver a autenticarse.

La aplicación cliente puede utilizar la respuesta de configuración para presentar un selector de IU con opciones de MVPD disponibles cada vez que el usuario necesite seleccionar su proveedor de TV.

La aplicación cliente debe almacenar el identificador MVPD seleccionado del usuario, tal como lo especifica el atributo de nivel de configuración `id` de MVPD, para continuar con las fases Autenticación, Preautorización, Autorización o Cierre de sesión.

Para obtener más información, consulte la documentación de [Recuperar configuración](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### &#x200B;4. ¿Debe la aplicación cliente almacenar en caché la información de respuesta de configuración en un almacenamiento persistente? {#configuration-phase-faq4}

La aplicación cliente solo debe recuperar la configuración cuando el usuario necesite seleccionar su MVPD para autenticarse o volver a autenticarse.

La aplicación cliente debe almacenar en caché la información de respuesta de configuración en un almacenamiento de memoria para evitar solicitudes innecesarias y mejorar la experiencia del usuario cuando:

* El usuario ya se ha autenticado.
* Se ofrece acceso temporal al usuario a través de la característica [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) básica o promocional.
* La autenticación del usuario ha caducado, pero la aplicación cliente ha almacenado en caché el MVPD seleccionado anteriormente como una opción motivada por la experiencia del usuario, y solo le pide que confirme que sigue siendo un suscriptor de ese MVPD.

#### &#x200B;5. ¿Puede la aplicación cliente gestionar su propia lista de MVPD? {#configuration-phase-faq5}

La aplicación cliente puede administrar su propia lista de MVPD, pero necesitaría mantener los identificadores de MVPD sincronizados con la autenticación de Adobe Pass. Por lo tanto, se recomienda utilizar la configuración proporcionada por la autenticación de Adobe Pass para garantizar que la lista esté actualizada y sea precisa.

La aplicación cliente recibirá un [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) de la API de REST de autenticación de Adobe Pass V2 si el identificador de MVPD proporcionado no es válido o si no tiene una integración activa con el [proveedor de servicios](rest-api-v2-glossary.md#service-provider) especificado.

#### &#x200B;6. ¿Puede la aplicación cliente filtrar la lista de MVPD? {#configuration-phase-faq6}

La aplicación cliente puede filtrar la lista de MVPD proporcionadas en la respuesta de configuración implementando un mecanismo personalizado basado en su propia lógica empresarial y en requisitos como la ubicación del usuario o el historial del usuario de selecciones anteriores.

La aplicación cliente puede filtrar la lista de [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) MVPD o las MVPD que tienen su integración aún en desarrollo o prueba.

#### &#x200B;7. ¿Qué sucede si la integración con un MVPD está deshabilitada y marcada como inactiva? {#configuration-phase-faq7}

Cuando la integración con un MVPD está deshabilitada y marcada como inactiva, MVPD se elimina de la lista de MVPD proporcionadas en respuestas de configuración adicionales y hay dos consecuencias importantes que considerar:

* Los usuarios no autenticados de ese MVPD ya no podrán completar la fase de autenticación con ese MVPD.
* Los usuarios autenticados de ese MVPD ya no podrán completar las fases de preautorización, autorización o cierre de sesión con ese MVPD.

La aplicación cliente recibiría un [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) de la API de REST de autenticación de Adobe Pass V2 si el usuario seleccionado MVPD ya no tiene una integración activa con el [proveedor de servicios](rest-api-v2-glossary.md#service-provider) especificado.

#### &#x200B;8. ¿Qué sucede si la integración con una MVPD se vuelve a habilitar y se marca como activa? {#configuration-phase-faq8}

Cuando la integración con un MVPD se vuelve a habilitar y se marca como activa, MVPD se vuelve a incluir en la lista de MVPD proporcionadas en respuestas de configuración adicionales y hay dos consecuencias importantes que se deben tener en cuenta:

* Los usuarios no autenticados de ese MVPD podrán completar de nuevo la fase de autenticación mediante ese MVPD.
* Los usuarios autenticados de ese MVPD podrán completar de nuevo las fases de preautorización, autorización o cierre de sesión con ese MVPD.

#### &#x200B;9. ¿Cómo se habilita o deshabilita la integración con un MVPD? {#configuration-phase-faq9}

Esta operación puede completarla uno de los administradores de su organización o un representante de autenticación de Adobe Pass que actúe en su nombre a través del [Panel de control de TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Preguntas frecuentes sobre la fase de autenticación {#authentication-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de autenticación

#### &#x200B;1. ¿Cuál es el propósito de la fase de autenticación? {#authentication-phase-faq1}

El propósito de la fase de autenticación es proporcionar a la aplicación cliente la capacidad de verificar la identidad del usuario y obtener información de metadatos del usuario.

La fase de autenticación actúa como un paso previo para la fase de preautorización o la fase de autorización cuando la aplicación cliente necesita reproducir contenido.

#### &#x200B;2. ¿Es obligatoria la fase de autenticación? {#authentication-phase-faq2}

La fase de autenticación es obligatoria, la aplicación cliente debe autenticar al usuario cuando no tiene un perfil válido dentro de la autenticación de Adobe Pass.

La aplicación cliente puede omitir esta fase en los siguientes casos:

* El usuario ya se ha autenticado y el perfil sigue siendo válido.
* Se ofrece acceso temporal al usuario a través de la característica [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) básica o promocional.

La administración de errores de la aplicación cliente requiere que se administren los códigos [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) (por ejemplo, `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated`, etc.), que indican que la aplicación cliente requiere que el usuario se autentique.

#### &#x200B;3. ¿Qué es una sesión de autenticación y durante cuánto tiempo es válida? {#authentication-phase-faq3}

La sesión de autenticación es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

La sesión de autenticación almacena información sobre el proceso de autenticación iniciado que se puede recuperar del punto final de sesiones.

La sesión de autenticación es válida durante un período de tiempo limitado y corto especificado en el momento de la emisión por la marca de tiempo `notAfter`, lo que indica la cantidad de tiempo que el usuario debe completar el proceso de autenticación antes de requerir el reinicio del flujo.

La aplicación cliente puede utilizar la respuesta de sesión de autenticación para saber cómo continuar con el proceso de autenticación. Tenga en cuenta que hay casos en los que no se requiere la autenticación del usuario, como proporcionar acceso temporal, acceso degradado o cuando el usuario ya está autenticado.

Para obtener más información, consulte los siguientes documentos:

* [Crear API de sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reanudar API de sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;4. ¿Qué es un código de autenticación y durante cuánto tiempo es válido? {#authentication-phase-faq4}

El código de autenticación es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

El código de autenticación almacena un valor único generado cuando un usuario inicia el proceso de autenticación e identifica de forma exclusiva la sesión de autenticación de un usuario hasta que el proceso se completa o hasta que caduca la sesión de autenticación.

El código de autenticación es válido para un período de tiempo limitado y corto especificado en el momento de iniciar la sesión de autenticación por la marca de tiempo `notAfter`, lo que indica la cantidad de tiempo que el usuario debe completar el proceso de autenticación antes de requerir reiniciar el flujo.

La aplicación cliente puede utilizar el código de autenticación para comprobar si el usuario ha completado correctamente la autenticación y recuperar la información de perfil del usuario, normalmente a través de un mecanismo de sondeo.

La aplicación cliente también puede utilizar el código de autenticación para permitir al usuario completar o reanudar el proceso de autenticación en el mismo dispositivo o en uno segundo (pantalla), teniendo en cuenta que la sesión de autenticación no caducó.

Para obtener más información, consulte los siguientes documentos:

* [Crear API de sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reanudar API de sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;5. ¿Cómo puede saber la aplicación cliente si el usuario ha escrito un código de autenticación válido y que la sesión de autenticación aún no ha caducado? {#authentication-phase-faq5}

La aplicación cliente puede validar el código de autenticación escrito por el usuario en una aplicación secundaria (pantalla) enviando una solicitud a uno de los extremos de sesiones responsables de reanudar la sesión de autenticación o recuperar la información de sesión de autenticación asociada con el código de autenticación.

La aplicación cliente recibiría un [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) si el código de autenticación proporcionado no se escribiera correctamente o en caso de que la sesión de autenticación expirara.

Para obtener más información, consulte los documentos [Reanudar sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) y [Recuperar sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### &#x200B;6. ¿Cómo puede saber la aplicación cliente si el usuario ya está autenticado? {#authentication-phase-faq6}

La aplicación cliente puede consultar uno de los siguientes extremos capaces de comprobar si un usuario ya está autenticado y devolver información de perfil:

* [API de extremo de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Extremo de perfiles para API de MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Extremo de perfiles para API de código específica (autenticación)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Para obtener más información, consulte los siguientes documentos:

* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### &#x200B;7. ¿Qué es un perfil y durante cuánto tiempo es válido? {#authentication-phase-faq7}

El perfil es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

El perfil almacena información sobre la validez de autenticación del usuario, información de metadatos y mucho más que se puede recuperar del extremo de perfiles.

La aplicación cliente puede utilizar el perfil para conocer el estado de autenticación del usuario, acceder a la información de metadatos del usuario, encontrar el método utilizado para autenticarse o la entidad utilizada para proporcionar identidad.

Para obtener más información, consulte los siguientes documentos:

* [API de extremo de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Extremo de perfiles para API de MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Extremo de perfiles para API de código específica (autenticación)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

El perfil es válido para un período de tiempo limitado especificado cuando se consulta mediante la marca de tiempo `notAfter`, lo que indica la cantidad de tiempo que el usuario tiene una autenticación válida antes de requerir pasar de nuevo por la fase de autenticación.

Este periodo de tiempo limitado conocido como autenticación (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) lo puede ver y cambiar uno de los administradores de su organización o un representante de autenticación de Adobe Pass que actúe en su nombre a través del [panel de control de TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;8. ¿Debe la aplicación cliente almacenar en caché la información de perfil del usuario en un almacenamiento persistente? {#authentication-phase-faq8}

La aplicación cliente debe almacenar en caché partes de la información de perfil del usuario en un almacenamiento persistente para evitar solicitudes innecesarias y mejorar la experiencia del usuario teniendo en cuenta los siguientes aspectos:

| Atributo | Experiencia del usuario |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | La aplicación cliente puede utilizar esto para realizar un seguimiento del proveedor de TV seleccionado del usuario y seguir utilizándolo durante las fases de preautorización o autorización.<br/><br/>Cuando caduca el perfil de usuario actual, la aplicación cliente puede utilizar la selección de MVPD recordada y solo pedirle al usuario que la confirme. |
| `attributes` | La aplicación cliente puede usar esto para personalizar la experiencia del usuario en función de diferentes claves de [metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) (por ejemplo, `zip`, `maxRating`, etc.).<br/><br/>Los metadatos de usuario están disponibles una vez finalizado el flujo de autenticación, por lo que la aplicación cliente no necesita consultar un extremo independiente para recuperar la información de [metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), ya que ya está incluida en la información de perfil.<br/><br/>Algunos atributos de metadatos se pueden actualizar durante el flujo de autorización, según el MVPD y el atributo de metadatos específico. Como resultado, es posible que la aplicación cliente necesite volver a consultar las API de perfiles para recuperar los metadatos de usuario más recientes. |
| `notAfter` | La aplicación cliente puede utilizar esto para realizar un seguimiento de la fecha de caducidad del perfil del usuario.<br/><br/>La administración de errores de la aplicación cliente requiere que se administren los códigos [error](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) (por ejemplo, `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated`, etc.), lo que indica que la aplicación cliente requiere que el usuario se autentique. |

#### &#x200B;9. ¿Puede la aplicación cliente ampliar el perfil del usuario sin requerir una nueva autenticación? {#authentication-phase-faq9}

No.

El perfil de usuario no puede extenderse más allá de su validez sin la interacción del usuario, ya que su caducidad está determinada por el TTL de autenticación establecido con MVPD.

Por lo tanto, la aplicación cliente debe solicitar al usuario que vuelva a autenticarse e interactúe con la página de inicio de sesión de MVPD para actualizar su perfil en el sistema.

Sin embargo, para las MVPD que admiten [autenticación basada en el hogar](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) (HBA), el usuario no tendrá que escribir credenciales.

#### &#x200B;10. ¿Cuáles son los casos de uso de cada extremo de perfiles disponible? {#authentication-phase-faq10}

Los extremos básicos de perfiles están diseñados para proporcionar a la aplicación cliente la capacidad de conocer el estado de autenticación del usuario, acceder a la información de metadatos del usuario, encontrar el método utilizado para autenticarse o la entidad utilizada para proporcionar identidad.

Cada extremo se adapta a un caso de uso específico, como se indica a continuación:

| API | Descripción | Caso de uso |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [API de extremo de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | Recupere todos los perfiles de usuario. | **El usuario abre la aplicación cliente por primera vez**<br/><br/> En esta situación, la aplicación cliente no tiene el identificador de MVPD seleccionado del usuario en la caché en el almacenamiento persistente.<br/><br/>Como resultado, enviará una sola solicitud para recuperar todos los perfiles de usuario disponibles. |
| [Extremo de perfiles para la API específica de MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | Recupere el perfil de usuario asociado a un MVPD específico. | **El usuario vuelve a la aplicación cliente después de autenticarse en una visita anterior**<br/><br/> En este caso, la aplicación cliente debe tener el identificador de MVPD seleccionado anteriormente en la caché en el almacenamiento persistente.<br/><br/>Como resultado, enviará una única solicitud para recuperar el perfil del usuario para ese MVPD específico. |
| [Extremo de perfiles para la API de código específica (autenticación)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Recupere el perfil de usuario asociado a un código de autenticación específico. | **El usuario inicia el proceso de autenticación**<br/><br/> En este escenario, la aplicación cliente debe determinar si el usuario ha completado correctamente la autenticación y recuperar su información de perfil.<br/><br/>Como resultado, se iniciará un mecanismo de sondeo para recuperar el perfil del usuario asociado al código de autenticación. |

Para obtener más información, consulte el [flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) y [flujo de perfiles básicos realizado en documentos de la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md).

El extremo SSO de perfiles tiene un propósito diferente y proporciona a la aplicación cliente la capacidad de crear un perfil de usuario utilizando la respuesta de autenticación del socio y recuperarlo en una sola operación única.

| API | Descripción | Caso de uso |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Perfiles de API de extremo SSO](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | Cree y recupere un perfil de usuario mediante la respuesta de autenticación de socio. | **El usuario permite que la aplicación use el inicio de sesión único del socio para autenticarse**<br/><br/> En este caso, la aplicación cliente debe crear un perfil de usuario después de recibir la respuesta de autenticación del socio y recuperarlo en una única operación única. |

Para cualquier consulta posterior, se deben usar los extremos básicos de perfiles para determinar el estado de autenticación del usuario, acceder a la información de metadatos del usuario, encontrar el método utilizado para autenticarse o la entidad utilizada para proporcionar identidad.

Para obtener más información, consulte los documentos [Inicio de sesión único mediante flujos de socios](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) y [Guía de Apple SSO (API REST V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md).

#### &#x200B;11. ¿Qué debe hacer la aplicación cliente si el usuario tiene varios perfiles de MVPD? {#authentication-phase-faq11}

La decisión de admitir varios perfiles depende de los requisitos empresariales de la aplicación cliente.

La mayoría de los usuarios solo tendrán un perfil. Sin embargo, en los casos en los que existen varios perfiles (como se detalla a continuación), la aplicación cliente es la responsable de determinar la mejor experiencia del usuario para la selección de perfiles.

La aplicación cliente puede solicitar al usuario que seleccione el perfil de MVPD deseado o que realice la selección automáticamente, como elegir el primer perfil de usuario de la respuesta o el que tenga el periodo de validez más largo.

La API de REST v2 admite varios perfiles para admitir:

* Los usuarios que tengan que elegir entre un perfil de MVPD normal y uno obtenido mediante inicio de sesión único (SSO).
* A los usuarios se les ofrece acceso temporal sin necesidad de cerrar la sesión de su MVPD normal.
* Usuarios con suscripción a MVPD combinada con servicios directo al consumidor (DTC).
* Usuarios con varias suscripciones a MVPD.

#### &#x200B;12. ¿Qué sucede cuando los perfiles de usuario caducan? {#authentication-phase-faq12}

Cuando los perfiles de usuario caducan, ya no se incluyen en la respuesta del extremo de perfiles.

Si el extremo Profiles devuelve una respuesta de asignación de perfiles vacía, la aplicación cliente debe crear una nueva sesión de autenticación y solicitar al usuario que vuelva a autenticarse.

Para obtener más información, consulte la [Documentación de la API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) Crear sesión de autenticación.

#### &#x200B;13. ¿Cuándo los perfiles de usuario dejan de ser válidos? {#authentication-phase-faq13}

Los perfiles de usuario dejan de ser válidos en los siguientes casos:

* Cuando caduca el TTL de autenticación, tal como indica la marca de tiempo `notAfter` en la respuesta de extremo de perfiles.
* Cuando la aplicación cliente cambia el valor del encabezado [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md).
* Cuando la aplicación cliente actualiza las credenciales del cliente utilizadas para recuperar el valor del encabezado [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md).
* Cuando la aplicación cliente revoca o actualiza la instrucción de software utilizada para obtener las credenciales del cliente.

#### &#x200B;14. ¿Cuándo debe iniciar la aplicación cliente el mecanismo de sondeo? {#authentication-phase-faq14}

Para garantizar la eficacia y evitar solicitudes innecesarias, la aplicación cliente debe iniciar el mecanismo de sondeo en las siguientes condiciones:

**Autenticación realizada en la aplicación principal (pantalla)**

La aplicación principal (streaming) debe comenzar a sondear cuando el usuario llegue a la página de destino final, después de que el componente del explorador cargue la dirección URL especificada para el parámetro `redirectUrl` en la solicitud de extremo [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

**Autenticación realizada en una aplicación secundaria (pantalla)**

La aplicación principal (streaming) debe comenzar a sondear en cuanto el usuario inicie el proceso de autenticación, justo después de recibir la respuesta del extremo [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) y mostrar el código de autenticación al usuario.

#### &#x200B;15. ¿Cuándo debe detener la aplicación cliente el mecanismo de sondeo? {#authentication-phase-faq15}

Para garantizar la eficacia y evitar solicitudes innecesarias, la aplicación cliente debe detener el mecanismo de sondeo en las siguientes condiciones:

**Autenticación correcta**

La información de perfil del usuario se ha recuperado correctamente, lo que confirma su estado de autenticación. En este punto, ya no es necesario el sondeo.

**Sesión de autenticación y caducidad del código**

La sesión de autenticación y el código caducan, tal como indica la marca de tiempo `notAfter` (por ejemplo, 30 minutos) en la respuesta de extremo [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Si esto sucede, el usuario debe reiniciar el proceso de autenticación y el sondeo con el código de autenticación anterior debe detenerse inmediatamente.

**Nuevo código de autenticación generado**

Si el usuario solicita un nuevo código de autenticación en el dispositivo principal (pantalla), la sesión existente ya no es válida y el sondeo con el código de autenticación anterior debe detenerse inmediatamente.

#### &#x200B;16. ¿Qué intervalo entre llamadas debe utilizar la aplicación cliente para el mecanismo de sondeo? {#authentication-phase-faq16}

Para garantizar la eficacia y evitar solicitudes innecesarias, la aplicación cliente debe configurar la frecuencia del mecanismo de sondeo en las siguientes condiciones:

| **Autenticación realizada en la aplicación principal (pantalla)** | **Autenticación realizada en una aplicación secundaria (pantalla)** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| La aplicación principal (streaming) debe sondear cada 3-5 segundos o más. | La aplicación principal (streaming) debe sondear cada 3-5 segundos o más. |

#### &#x200B;17. ¿Cuál es el número máximo de solicitudes de sondeo que puede enviar la aplicación cliente? {#authentication-phase-faq17}

La aplicación cliente debe cumplir los límites actuales definidos por el [mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits) de autenticación de Adobe Pass.

La administración de errores de la aplicación cliente debe poder controlar el código de error [429 Demasiadas solicitudes](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response), que indica que la aplicación cliente ha superado el número máximo de solicitudes permitidas.

Para obtener más información, consulte la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

#### &#x200B;18. ¿Cómo puede la aplicación cliente obtener la información de metadatos del usuario? {#authentication-phase-faq18}

La aplicación cliente puede consultar uno de los siguientes extremos capaces de devolver información de [metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) como parte de la información de perfil:

* [API de extremo de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Extremo de perfiles para API de MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Extremo de perfiles para API de código específica (autenticación)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Los metadatos del usuario están disponibles una vez finalizado el flujo de autenticación, por lo que la aplicación cliente no necesita consultar un extremo independiente para recuperar la información de [metadatos del usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), ya que ya está incluida en la información del perfil.

Para obtener más información, consulte los siguientes documentos:

* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Algunos atributos de metadatos se pueden actualizar durante el flujo de autorización, según la MVPD y el atributo de metadatos específico. Como resultado, es posible que la aplicación cliente tenga que volver a consultar las API anteriores para recuperar los metadatos de usuario más recientes.

#### &#x200B;19. ¿Cómo debe administrar la aplicación cliente el acceso degradado? {#authentication-phase-faq19}

La [característica de degradación](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) permite que la aplicación cliente mantenga una experiencia de transmisión perfecta para los usuarios, incluso cuando los servicios de autenticación o autorización de MVPD encuentren problemas.

En resumen, esto puede garantizar el acceso ininterrumpido al contenido a pesar de las interrupciones temporales del servicio de MVPD.

Dado que su organización tiene intención de utilizar la función de degradación Premium, la aplicación cliente debe gestionar los flujos de acceso degradados, que describen cómo se comportan los extremos de la API de REST v2 en estos casos.

Para obtener más información, consulte la documentación de [Flujos de acceso degradados](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).

#### &#x200B;20. ¿Cómo debe administrar la aplicación cliente el acceso temporal? {#authentication-phase-faq20}

La característica [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) permite que la aplicación cliente proporcione acceso temporal al usuario.

En resumen, esto puede atraer a los usuarios al proporcionar acceso limitado al contenido o a un número predefinido de títulos de VOD durante un período de tiempo especificado.

Dado que su organización tiene intención de utilizar la función TempPass Premium, la aplicación cliente debe administrar los flujos de acceso temporales, que describen cómo se comportan los extremos de la API de REST v2 en estos casos.

En versiones anteriores de la API, la aplicación cliente tenía que cerrar la sesión de un usuario autenticado con su MVPD normal para ofrecer acceso temporal.

Con la API de REST v2, la aplicación cliente puede alternar sin problemas entre una MVPD normal y una MVPD TempPass al autorizar un flujo, lo que elimina la necesidad de cerrar la sesión del usuario.

Para obtener más información, consulte la documentación de [Flujos de acceso temporales](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).

#### &#x200B;21. ¿Cómo debe administrar la aplicación cliente el acceso de inicio de sesión único entre dispositivos? {#authentication-phase-faq21}

La API de REST v2 puede habilitar el inicio de sesión único entre dispositivos si la aplicación cliente proporciona un identificador de usuario único y coherente entre dispositivos.

La aplicación cliente debe generar este identificador, conocido como token de servicio, mediante la implementación o integración de un servicio de identidad externo de su elección.

Para obtener más información, consulte la documentación de [Inicio de sesión único mediante flujos de token de servicio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

+++

### Preguntas frecuentes sobre la fase de preautorización {#preauthorization-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de preautorización

#### &#x200B;1. ¿Cuál es el propósito de la fase de preautorización? {#preauthorization-phase-faq1}

El propósito de la fase de preautorización es proporcionar a la aplicación cliente la capacidad de presentar un subconjunto de recursos de su catálogo al que el usuario tendría derecho de acceso.

La fase de preautorización puede mejorar la experiencia del usuario cuando abre la aplicación cliente por primera vez o navega a una nueva sección.

#### &#x200B;2. ¿Es obligatoria la fase de preautorización? {#preauthorization-phase-faq2}

La fase de preautorización no es obligatoria, la aplicación cliente puede omitir esta fase si desea presentar un catálogo de recursos sin filtrarlos primero según el derecho del usuario.

#### &#x200B;3. ¿Qué es una decisión de preautorización? {#preauthorization-phase-faq3}

La preautorización es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization), mientras que el término de decisión también se puede encontrar en el [Glosario](rest-api-v2-glossary.md#decision).

La decisión de preautorización almacena información sobre el resultado de la consulta del proceso de preautorización de MVPD que se puede recuperar del extremo de preautorización de Decisions.

La aplicación cliente puede utilizar las decisiones de preautorización para presentar un subconjunto de recursos al que el proveedor de TV (informativo) permitiría acceder.

Para obtener más información, consulte los siguientes documentos:

* [Recuperar API de decisiones de preautorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### &#x200B;4. ¿Debe la aplicación cliente almacenar en caché las decisiones de preautorización en un almacenamiento persistente? {#preauthorization-phase-faq4}

La aplicación cliente no es necesaria para almacenar decisiones de preautorización en almacenamiento persistente. Sin embargo, se recomienda almacenar en caché las decisiones de permiso en la memoria para mejorar la experiencia del usuario. Esto ayuda a evitar llamadas innecesarias al extremo de preautorización de Decisions para recursos que ya se han preautorizado, lo que reduce la latencia y mejora el rendimiento.

#### &#x200B;5. ¿Cómo puede determinar la aplicación cliente por qué se denegó una decisión de preautorización? {#preauthorization-phase-faq5}

La aplicación cliente puede determinar el motivo de una decisión de preautorización denegada inspeccionando el [código de error y el mensaje](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) incluidos en la respuesta del extremo de preautorización de Decisions. Estos detalles proporcionan a insight el motivo específico por el que se denegó la solicitud de preautorización, lo que ayuda a informar a la experiencia del usuario o al déclencheur de cualquier tratamiento necesario en la aplicación.

Asegúrese de que cualquier mecanismo de reintento implementado para recuperar decisiones de preautorización no genere un bucle interminable si se deniega la decisión de preautorización.

Considere la posibilidad de limitar los reintentos a un número razonable y administrar las denegaciones correctamente mostrando comentarios claros al usuario.

#### &#x200B;6. ¿Por qué en la decisión de preautorización falta un token de medios? {#preauthorization-phase-faq6}

A la decisión de preautorización le falta un token de medios porque la fase de preautorización no debe utilizarse para reproducir recursos, ya que ese es el propósito de la fase de autorización.

#### &#x200B;7. ¿Se puede omitir la fase de autorización si ya existe una decisión de preautorización? {#preauthorization-phase-faq7}

No.

La fase de autorización no se puede omitir aunque haya una decisión de preautorización disponible. Las decisiones de preautorización son solo informativas y no otorgan derechos de reproducción reales. La fase de preautorización pretende proporcionar directrices tempranas, pero la fase de autorización sigue siendo necesaria antes de reproducir cualquier contenido.

#### &#x200B;8. ¿Qué es un recurso y qué formatos se admiten? {#preauthorization-phase-faq8}

El medio es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

El recurso es un identificador único acordado con las MVPD y está asociado a un contenido que la aplicación cliente podría transmitir.

El identificador único del recurso puede tener dos formatos:

* Un formato de cadena simple, como un identificador único de un canal (marca).
* Un formato RSS (RSS) multimedia que contiene información adicional, como el título, las clasificaciones y los metadatos de control parental.

Para obtener más información, consulte la documentación de [Recursos protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;9. ¿Para cuántos recursos puede la aplicación cliente obtener una decisión de preautorización a la vez? {#preauthorization-phase-faq9}

La aplicación cliente puede obtener una decisión de preautorización para un número limitado de recursos en una sola solicitud de API, normalmente hasta 5, debido a condiciones impuestas por MVPD.

Este número máximo de recursos se puede ver y cambiar después de ponerse de acuerdo con las MVPD a través del [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass por uno de los administradores de la organización o por un representante de autenticación de Adobe Pass que actúe en su nombre.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### Preguntas frecuentes sobre la fase de autorización {#authorization-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de autorización

#### &#x200B;1. ¿Cuál es el propósito de la fase de autorización? {#authorization-phase-faq1}

El propósito de la fase de autorización es proporcionar a la aplicación cliente la capacidad de reproducir recursos que el usuario solicita después de validar sus derechos con MVPD.

#### &#x200B;2. ¿Es obligatoria la fase de autorización? {#authorization-phase-faq2}

La fase de autorización es obligatoria, la aplicación cliente no puede omitir esta fase si desea reproducir recursos que solicita el usuario, ya que requiere verificar con MVPD que el usuario tiene derecho antes de liberar el flujo.

#### &#x200B;3. ¿Qué es una decisión de autorización y durante cuánto tiempo es válida? {#authorization-phase-faq3}

La autorización es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization), mientras que el término de decisión también se puede encontrar en el [Glosario](rest-api-v2-glossary.md#decision).

La decisión de autorización almacena información sobre el resultado de la consulta del proceso de autorización de MVPD que se puede recuperar del extremo de autorización de decisiones.

La aplicación cliente puede utilizar la decisión de autorización que contiene un token de medios para reproducir el flujo de recursos en caso de que la decisión del proveedor de TV (autoritativa) permita al usuario acceder a él.

Para obtener más información, consulte los siguientes documentos:

* [Recuperar API de decisiones de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

La decisión de autorización es válida durante un periodo de tiempo limitado y corto especificado en el momento de la emisión, lo que indica la cantidad de tiempo que la autenticación de Adobe Pass la almacenará en caché antes de solicitar consultar de nuevo MVPD.

Este periodo de tiempo limitado conocido como autorización (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl) lo puede ver y modificar uno de los administradores de su organización o un representante de autenticación de Adobe Pass que actúe en su nombre a través del [panel de control de TVE](rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;4. ¿Debe la aplicación cliente almacenar en caché las decisiones de autorización en un almacenamiento persistente? {#authorization-phase-faq4}

La aplicación cliente no es necesaria para almacenar decisiones de autorización en almacenamiento persistente.

#### &#x200B;5. ¿Cómo puede determinar la aplicación cliente por qué se denegó una decisión de autorización? {#authorization-phase-faq5}

La aplicación cliente puede determinar el motivo de una decisión de autorización denegada inspeccionando el [código de error y el mensaje](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) incluidos en la respuesta del extremo de autorización de decisiones. Estos detalles proporcionan a insight el motivo específico por el que se denegó la solicitud de autorización, lo que ayuda a informar a la experiencia del usuario o al déclencheur de cualquier tratamiento necesario en la aplicación.

Asegúrese de que cualquier mecanismo de reintento implementado para recuperar decisiones de autorización no genere un bucle interminable si se deniega la decisión de autorización.

Considere la posibilidad de limitar los reintentos a un número razonable y administrar las denegaciones correctamente mostrando comentarios claros al usuario.

#### &#x200B;6. ¿Qué es un token de medios y durante cuánto tiempo es válido? {#authorization-phase-faq6}

El token multimedia es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

El token de medios consiste en una cadena firmada enviada en texto no cifrado que se puede recuperar del extremo de autorización de decisiones.

Para obtener más información, consulte la documentación de [Comprobador de token de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

El token de medios es válido durante un periodo de tiempo limitado y corto especificado en el momento del problema, lo que indica el límite de tiempo antes de que la aplicación cliente deba verificarlo y utilizarlo.

La aplicación cliente puede utilizar el token de medios para reproducir un flujo de recursos en caso de que la decisión del proveedor de TV (autoritativa) permita al usuario acceder a él.

Para obtener más información, consulte los siguientes documentos:

* [Recuperar API de decisiones de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### &#x200B;7. ¿Debe la aplicación cliente validar el token de medios antes de reproducir el flujo de recursos? {#authorization-phase-faq7}

Sí.

La aplicación cliente debe validar el token de medios antes de iniciar la reproducción del flujo de recursos. Esta validación debe realizarse con el [Comprobador de token de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). Al comprobar `serializedToken` desde el objeto `token` devuelto, la aplicación cliente ayuda a evitar el acceso no autorizado, como la copia desde secuencias, y garantiza que solo los usuarios autorizados correctamente puedan reproducir el contenido.

#### &#x200B;8. ¿Debe la aplicación cliente actualizar un token de medios caducado durante la reproducción? {#authorization-phase-faq8}

No.

La aplicación cliente no es necesaria para actualizar un token de medios caducado mientras el flujo se está reproduciendo activamente. Si el token de medios caduca durante la reproducción, se debe permitir que el flujo continúe ininterrumpidamente. Sin embargo, el cliente debe solicitar una nueva decisión de autorización y obtener un nuevo token de medios la próxima vez que el usuario intente reproducir un recurso.

#### &#x200B;9. ¿Cuál es la finalidad de cada atributo de marca de tiempo en la decisión de autorización? {#authorization-phase-faq9}

La decisión de autorización incluye varios atributos de marca de tiempo que proporcionan un contexto esencial sobre el periodo de validez de la propia autorización y el token de medios asociado. Estas marcas de tiempo tienen diferentes propósitos, dependiendo de si están relacionadas con la decisión de autorización o el token de medios.

**Marcas de hora de nivel de decisión**

Estas marcas de tiempo describen el periodo de validez de la decisión de autorización general:

| Atributo | Descripción | Notas |
|-------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Tiempo en milisegundos en el que se emitió la decisión de autorización. | Esto marca el inicio de la ventana de validez de la autorización. |
| `notAfter` | El tiempo en milisegundos cuando caduca la decisión de autorización. | El [tiempo de vida de la autorización (TTL)](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management) determina cuánto tiempo la autorización sigue siendo válida antes de requerir una nueva autorización. Este TTL se negocia con representantes de MVPD. |

**Marcas de hora a nivel de token**

Estas marcas de tiempo describen el periodo de validez del token de medios asociado a la decisión de autorización:

| Atributo | Descripción | Notas |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | Tiempo en milisegundos durante el que se emitió el token de medios. | Esto marca cuándo el token se vuelve válido para la reproducción. |
| `notAfter` | Tiempo en milisegundos cuando caduca el token de medios. | Los tokens de medios tienen una duración deliberadamente corta (normalmente de 7 minutos) para minimizar los riesgos de uso incorrecto y tener en cuenta las posibles diferencias de reloj entre el servidor que genera tokens y el servidor que verifica tokens. |

#### &#x200B;10. ¿Qué es un recurso y qué formatos se admiten? {#authorization-phase-faq10}

El medio es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

El recurso es un identificador único acordado con las MVPD y está asociado a un contenido que la aplicación cliente podría transmitir.

El identificador único del recurso puede tener dos formatos:

* Un formato de cadena simple, como un identificador único de un canal (marca).
* Un formato RSS (RSS) multimedia que contiene información adicional, como el título, las clasificaciones y los metadatos de control parental.

Para obtener más información, consulte la documentación de [Recursos protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;11. ¿Para cuántos recursos puede obtener la aplicación cliente una decisión de autorización a la vez? {#authorization-phase-faq11}

La aplicación cliente puede obtener una decisión de autorización para un número limitado de recursos en una sola solicitud de API, normalmente hasta 1, debido a condiciones impuestas por MVPD.

+++

### Preguntas frecuentes sobre la fase de cierre {#logout-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de cierre de sesión

#### &#x200B;1. ¿Cuál es el propósito de la fase de cierre de sesión? {#logout-phase-faq1}

El propósito de la fase de cierre de sesión es proporcionar a la aplicación cliente la capacidad de finalizar el perfil autenticado del usuario dentro de la autenticación de Adobe Pass si el usuario lo solicita.

#### &#x200B;2. ¿Es obligatoria la fase de cierre de sesión? {#logout-phase-faq2}

La fase de cierre de sesión es obligatoria, la aplicación cliente debe proporcionar al usuario la capacidad de cerrar la sesión.

+++

### Preguntas frecuentes sobre encabezados {#headers-faqs-general}

+++Preguntas frecuentes sobre encabezados

#### &#x200B;1. ¿Cómo se calcula el valor del encabezado Autorización? {#headers-faq1}

>[!IMPORTANT]
>
> Si la aplicación cliente está migrando de la API de REST V1 a la API de REST V2, puede seguir utilizando el mismo método para obtener el valor del token de acceso `Bearer` que antes.

El encabezado de la solicitud [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contiene el token de acceso `Bearer` requerido por la aplicación cliente para acceder a las API protegidas por Adobe Pass.

El valor del encabezado Autorización debe obtenerse de la Autenticación de Adobe Pass durante la Fase de registro.

Para obtener más información, consulte los siguientes documentos:

* [Información general sobre el registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recuperar API de credenciales de cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar API de token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### &#x200B;2. ¿Cómo calcular el valor del encabezado AP-Device-Identifier? {#headers-faq2}

>[!IMPORTANT]
>
> En caso de que la aplicación cliente esté migrando de la API de REST V1 a la API de REST V2, la aplicación cliente puede seguir utilizando el mismo método para calcular el valor del identificador de dispositivo que antes.

El encabezado de la solicitud [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contiene el identificador del dispositivo de flujo continuo tal como lo creó la aplicación cliente.

La documentación del encabezado [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) proporciona ejemplos para las plataformas principales sobre cómo calcular el valor, pero la aplicación cliente puede elegir utilizar un método diferente según su propia lógica y requisitos empresariales.

#### &#x200B;3. ¿Cómo calcular el valor del encabezado X-Device-Info? {#headers-faq3}

>[!IMPORTANT]
>
> En caso de que la aplicación cliente esté migrando de la API de REST V1 a la API de REST V2, la aplicación cliente puede seguir utilizando el mismo método para calcular el valor de información del dispositivo que antes.

El encabezado de la solicitud [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contiene la información del cliente (dispositivo, conexión y aplicación) relacionada con el dispositivo de flujo continuo real y se usa para determinar las reglas específicas de la plataforma que las MVPD pueden aplicar.

La documentación del encabezado [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) proporciona ejemplos para las plataformas principales sobre cómo calcular el valor, pero la aplicación cliente puede elegir utilizar un método diferente según su propia lógica y requisitos empresariales.

Si falta el encabezado [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) o contiene valores incorrectos, la solicitud puede clasificarse como originada en una plataforma `unknown`.

Esto puede hacer que la solicitud se trate como no segura y sujeta a reglas más restrictivas, como TTL de autenticación más cortos. Además, algunos campos, como el dispositivo de transmisión por secuencias `connectionIp` y `connectionPort`, son obligatorios para características como la [autenticación de base principal](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) de Spectrum.

Incluso cuando la solicitud se origina desde un servidor en nombre de un dispositivo, el valor del encabezado [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) debe reflejar la información real del dispositivo de flujo continuo.

+++

### Preguntas frecuentes varias {#misc-faqs-general}

+++Preguntas frecuentes varias

#### &#x200B;1. ¿Puedo explorar las solicitudes y respuestas de la API de REST V2 y probar la API? {#misc-faq1}

Sí.

Puede explorar la API REST V2 a través de nuestro sitio web [Adobe Developer](https://developer.adobe.com/adobe-pass/). El sitio web de Adobe Developer proporciona acceso sin restricciones a:

* [API de DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [API DE REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Para interactuar con la [API de REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), debe incluir el encabezado [Autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) con un token de acceso de `Bearer` obtenido a través de la [API de DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Para usar la [API de DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), se requiere una instrucción de software con el ámbito de API de REST V2. Para obtener más información, consulte el documento [Preguntas frecuentes sobre el registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

#### &#x200B;2. ¿Puedo explorar las solicitudes y respuestas de la API de REST V2 mediante una herramienta de desarrollo de API compatible con OpenAPI? {#misc-faq2}

Sí.

Puede descargar archivos de especificación OpenAPI para la [API de DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) y la [API de REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) desde el sitio web de [Adobe Developer](https://developer.adobe.com/adobe-pass/).

Para descargar los archivos de especificación de OpenAPI, haga clic en los botones de descarga para guardar los siguientes archivos en el equipo local:

* [JSON DE API DE DCR](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [API DE REST V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

A continuación, puede importar estos archivos en la herramienta de desarrollo de API que prefiera para explorar las solicitudes y respuestas de la API de REST V2 y probar la API.

#### &#x200B;3. ¿Puedo seguir utilizando la herramienta de prueba de API existente alojada en https://sp.auth-staging.adobe.com/apitest/api.html? {#misc-faq3}

No.

Las aplicaciones cliente que migran a la API de REST V2 deben utilizar la nueva herramienta de prueba alojada en https://developer.adobe.com/adobe-pass/. El sitio web de Adobe Developer proporciona acceso sin restricciones a:

* [API de DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [API DE REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Para interactuar con la [API de REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), debe incluir el encabezado [Autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) con un token de acceso de `Bearer` obtenido a través de la [API de DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Para usar la [API de DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), se requiere una instrucción de software con el ámbito de API de REST V2. Para obtener más información, consulte el documento [Preguntas frecuentes sobre el registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

+++

## Preguntas frecuentes sobre migración {#migration-faqs}

Continúe con esta sección si está trabajando en una aplicación que necesita migrar una aplicación existente a la API de REST V2.

>[!MORELIKETHIS]
>
> * [Preguntas más frecuentes sobre el registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Preguntas frecuentes generales sobre migración {#general-migration-faqs}

+++Preguntas frecuentes sobre la migración general

#### &#x200B;1. ¿Debo implementar una nueva aplicación cliente migrada a la API de REST V2 para todos los usuarios a la vez? {#migration-faq1}

No.

La aplicación cliente no es necesaria para implementar una nueva versión que integre la API de REST V2 para todos los usuarios simultáneamente.

La autenticación de Adobe Pass seguirá siendo compatible con versiones de aplicaciones cliente anteriores que integren la API de REST V1 o SDK hasta finales de 2025.

#### &#x200B;2. ¿Se me requiere para implementar una nueva aplicación cliente migrada a la API de REST V2 en todas las API y flujos a la vez? {#migration-faq2}

Sí.

La aplicación cliente debe implementar una nueva versión que integre la API de REST V2 en todas las API de autenticación de Adobe Pass y flujos simultáneamente.

En caso del flujo de &quot;autenticación de segunda pantalla&quot;, la aplicación cliente debe implementar una nueva versión que integre la API de REST V2 para las aplicaciones [primary](rest-api-v2-glossary.md#primary-application) y [secondary](rest-api-v2-glossary.md#secondary-application) simultáneamente.

La autenticación de Adobe Pass no admitirá implementaciones &quot;híbridas&quot; que integren tanto la API de REST V2 como la API de REST V1/SDK entre API y flujos.

#### &#x200B;3. ¿Se conservará la autenticación del usuario al actualizar a una nueva aplicación cliente migrada a la API de REST V2? {#migration-faq3}

No.

No se conservará la autenticación de usuario obtenida en versiones anteriores de aplicaciones cliente que integran la API de REST V1 o SDK.

Por lo tanto, se solicitará al usuario que vuelva a autenticarse dentro de la nueva aplicación cliente migrada a la API de REST V2.

#### &#x200B;4. ¿Los códigos de error mejorados están habilitados de forma predeterminada en la API de REST V2? {#migration-faq4}

Sí.

Las aplicaciones cliente que migran a la API de REST V2 se benefician automáticamente de esta función de forma predeterminada, lo que proporciona información de error más detallada y precisa.

Para obtener más información, consulte la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

#### &#x200B;5. ¿Las integraciones existentes requieren cambios de configuración al migrar a la API de REST V2? {#migration-faq5}

No.

Las aplicaciones cliente que migran a la API de REST V2 no requieren ningún cambio de configuración para las integraciones de MVPD existentes. Además, seguirán usando los mismos identificadores para los [proveedores de servicios](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) y [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd) existentes.

Además, los protocolos utilizados por la autenticación de Adobe Pass para comunicarse con los extremos de MVPD permanecen inalterados.

+++

### Migración de la API de REST V1 a la API de REST V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Continúe con esta subsección si está trabajando en una aplicación que necesita migrar de la API de REST V1 a la API de REST V2.

#### Preguntas frecuentes sobre la fase de registro {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de registro

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de registro? {#registration-phase-v1-to-v2-faq1}

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

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de configuración? {#configuration-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPD con integración activa | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Preguntas frecuentes sobre la fase de autenticación {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de autenticación

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de autenticación? {#authentication-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticación) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar código de registro (código de autenticación) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reanudar la autenticación (MVPD) en la segunda pantalla (aplicación) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticación (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [GET <br/> /api/v1/checkauthn (primera pantalla)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (segunda pantalla)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar token de autenticación de usuario (perfil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de preautorización {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de preautorización

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de preautorización? {#preauthorization-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperación de recursos autorizados previamente (decisiones de preautorización) | [GET <br/> /api/v1/preauthorize (primera pantalla)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (segunda pantalla)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de autorización {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de autorización

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de autorización? {#authorization-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autorización de (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recuperar token de autorización (decisión de autorización) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recuperar token de autorización corto (token de medios) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | La aplicación cliente puede utilizar la respuesta de esta API para varios fines a la vez: <br/> <ul><li>Iniciar autorización de (MVPD)</li><li>Recuperar decisión de autorización</li><li>Recuperar token de medios corto</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de cierre {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de cierre de sesión

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de cierre de sesión? {#logout-phase-v1-to-v2-faq1}

En la migración de la API de REST V1 a la API de REST V2 hay cambios de alto nivel que se presentan en la siguiente tabla:

| Ámbito | API DE REST V1 | API DE REST V2 | Observaciones |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar cierre de sesión | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de cierre de sesión básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migración de SDK a la API de REST V2 {#migration-sdk-to-rest-api-v2}

Continúe con esta subsección si está trabajando en una aplicación que necesita migrar de SDK a la API de REST V2.

#### Preguntas frecuentes sobre la fase de registro {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de registro

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de registro? {#registration-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de configuración? {#configuration-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de autenticación? {#authentication-phase-sdk-to-v2-faq1}

En la migración de los SDK a la API de REST V2 hay cambios de alto nivel que hay que tener en cuenta y que se presentan en las siguientes tablas:

###### AccessEnabler JavaScript SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticación) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar código de registro (código de autenticación) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reanudar la autenticación (MVPD) en la segunda pantalla (aplicación) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Ámbito | SDK | API DE REST V2 | Observaciones |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticación) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar código de registro (código de autenticación) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/session/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Reanudar la autenticación (MVPD) en la segunda pantalla (aplicación) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticación (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [PUBLICACIÓN <br/> /api/v2/{serviceProvider}/sesiones](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Comprobar estado de autenticación del usuario | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar información de metadatos de usuario | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Utilice uno de los siguientes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | La aplicación cliente puede utilizar la respuesta de estas API para varios fines a la vez: <br/> <ul><li>Comprobar estado de autenticación del usuario</li><li>Recuperar perfil de usuario</li><li>Recuperar información de metadatos de usuario</li></ul> <br/> Para obtener más información, consulte los siguientes documentos: <br/> <ul><li>[Flujo de perfiles básicos realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Preguntas frecuentes sobre la fase de preautorización {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Preguntas frecuentes sobre la fase de preautorización

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de preautorización? {#preauthorization-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de autorización? {#authorization-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. ¿Cuáles son las migraciones de API de alto nivel necesarias para la fase de cierre de sesión? {#logout-phase-sdk-to-v2-faq1}

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
