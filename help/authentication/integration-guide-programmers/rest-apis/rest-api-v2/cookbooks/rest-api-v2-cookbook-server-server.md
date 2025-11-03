---
title: Guía de API de REST V2 (servidor a servidor)
description: Guía de API de REST V2 (servidor a servidor)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '2497'
ht-degree: 0%

---

# Guía de API de REST V2 (servidor a servidor) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

El documento está dirigido a desarrolladores que están integrando la [API de REST de autenticación de Adobe Pass V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) en sus aplicaciones de streaming que tengan una arquitectura de servidor a servidor (S2S).

## Requisitos previos {#prerequisites}

Para ver los términos y definiciones, consulte la documentación de [Glosario de la API de REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md).

Para conocer los requisitos obligatorios y las prácticas recomendadas, consulte la documentación de [Lista de comprobación de la API de REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md).

Para ver las preguntas más frecuentes, consulte la [documentación de preguntas frecuentes sobre la API de REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md).

### Componentes {#components}

Antes de empezar, asegúrese de comprender los siguientes componentes y términos utilizados en el flujo de trabajo:

| Tipo | Componente | Descripción |
|---------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Infraestructura de Adobe | Servicio de Adobe Pass | Se integra con MVPD IdP y el servicio AuthZ para proporcionar decisiones de autenticación y autorización. |
| Infraestructura del programador | Servicio de programador | Conecta el dispositivo de streaming con el servicio de Adobe Pass para obtener perfiles autenticados y decisiones de autorización. |
| Infraestructura de MVPD | Servicio de MVPD IdP | Extremo de MVPD responsable de la autenticación basada en credenciales, que valida la identidad del usuario. |
|                           | Servicio AuthZ de MVPD | Punto final de MVPD que determina las decisiones de autorización en función de las suscripciones del usuario, los controles parentales y otras reglas de asignación de derechos. |
| Dispositivo de streaming | Aplicación de streaming | La aplicación del programador que se ejecuta en el dispositivo de flujo continuo del usuario y reproduce contenido de vídeo autenticado. |
|                           | (Opcional) Módulo AuthN | Si el dispositivo de streaming tiene un user-agent (por ejemplo, explorador), el módulo AuthN administra la autenticación del usuario en el MVPD IdP. |
| (Opcional) Dispositivo AuthN | Aplicación AuthN | Si el dispositivo de streaming carece de user-agent (por ejemplo, explorador), la aplicación AuthN es una aplicación web de Programmer a la que se accede desde un dispositivo independiente a través de un explorador web. |

### Requisitos {#requirements}

En implementaciones de servidor a servidor (S2S), la aplicación de streaming y el servicio de programador deben establecer un protocolo que permita al servicio de programador:

* Comuníquese con el servicio de Adobe Pass en nombre de la aplicación de streaming.

* Recopile y pase un identificador de dispositivo único para el dispositivo de transmisión según lo requerido por el encabezado [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md).

* Recopile y pase información precisa del dispositivo de transmisión, incluido el puerto de origen y los detalles específicos del dispositivo, según lo requerido por el encabezado [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md).

* Recopile y pase la dirección IP del dispositivo de streaming según lo requerido por el encabezado X-Forwarded-For.

* Almacene de forma segura parámetros como ID de dispositivo, ID de cliente y secreto de cliente en la aplicación de streaming o en el servicio de programador.

* Formatee y envíe datos de conformidad con MVPD y aplicaciones integradas, incluidas la IP del dispositivo, el puerto de origen, la información específica del dispositivo, los MRSS y los identificadores opcionales como ECID.

* Mantenga y administre de forma segura los certificados compartidos con Adobe para la transmisión cifrada de metadatos de usuarios.

* Respete la validez del perfil de autenticación y de la decisión de autorización al almacenar en caché, asegurándose de que los estados de autenticación y autorización se invaliden cuando se notifiquen.

* Devuelva las decisiones de autorización e instrucciones relevantes a la aplicación de streaming.

### Entornos {#environments}

Antes de entrar en el flujo de trabajo, asegúrese de mantener al menos dos entornos: Producción y Ensayo.

**Producción**

El entorno de producción debe tener una alta disponibilidad y una escala adecuadas para gestionar picos de tráfico grandes o inesperados, como los causados por eventos deportivos en directo o por noticias de última hora.

* El servicio Adobe Pass funciona en varios centros de datos distribuidos geográficamente en todo Estados Unidos para optimizar el rendimiento y minimizar la latencia.

   * El servicio de programación debe adoptar una estrategia de infraestructura similar, lo que garantiza tiempos de respuesta de baja latencia de Adobe Pass.

* El programador debe proporcionar el rango de IP pública de su entorno de producción.

   * Estas IP se añadirán a una lista de permitidos dentro de la infraestructura de Adobe Pass.

* El servicio de programador debe limitar el almacenamiento en caché de DNS a un máximo de 30 segundos para permitir el redireccionamiento dinámico en caso de que Adobe necesite redirigir el tráfico debido a que un centro de datos deja de estar disponible.

**Ensayo**

El entorno de ensayo puede ser mínimo, pero debe reflejar la producción incluyendo todos los componentes críticos del sistema y la lógica empresarial.

* Debe permitir versiones de prueba antes de la implementación en producción.

* Desde el punto de vista operativo, debería ser similar a la producción, lo que permitiría realizar pruebas realistas.

* Lo ideal es que el entorno de ensayo se conecte a los entornos de prueba de Adobe Pass para lo siguiente:

   * Permita que los programadores realicen pruebas con la infraestructura de Adobe.

   * Habilite Adobe para ayudarle con las pruebas y la resolución de problemas cuando sea necesario.

## Flujo de trabajo {#workflow}

Siga estos pasos, tal como se muestra en el diagrama siguiente.

![Guía de API de REST V2 (de servidor a servidor)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

*Guía de API de REST V2 (de servidor a servidor)*

## A. Fase de registro {#registration-phase}

El propósito de la fase de registro es registrar la aplicación de flujo continuo con la autenticación de Adobe Pass a través del proceso de registro de cliente dinámico (DCR).

El proceso de registro dinámico de clientes (DCR) requiere que la aplicación de streaming obtenga un par de credenciales de cliente y recupere un token de acceso como objetivo final de la fase de registro.

La fase de registro es obligatoria, pero la aplicación de streaming puede omitir esta fase si tiene un par en caché de credenciales de cliente y un token de acceso que aún son válidos.

+++Artículos relacionados

API:

* [Recuperar credenciales de cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

Flujos:

* [Flujo de registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Preguntas frecuentes:

* [Preguntas frecuentes sobre la fase de registro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Paso 1: Registro de la aplicación {#step-1-register-your-application}

* Recuperar credenciales de cliente: el servicio de programación recupera las credenciales de cliente llamando al extremo [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

   * El servicio de programador o la aplicación de programador deben almacenar las credenciales del cliente y utilizarlas indefinidamente cuando necesite recuperar un token de acceso.


* Recuperar token de acceso: el servicio de programador recupera el token de acceso llamando al extremo [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

   * El servicio de programador o la aplicación de programador deben almacenar y utilizar el token de acceso hasta que caduque y, a continuación, descartarlo y obtener uno nuevo.

## B. Fase de autenticación {#authentication-phase}

El propósito de la fase de autenticación es proporcionar a la aplicación de transmisión la capacidad de verificar la identidad del usuario y obtener información de metadatos del usuario.

La fase de autenticación actúa como un paso previo para la fase de preautorización o de autorización cuando la aplicación de streaming necesita reproducir contenido.

+++Artículos relacionados

API

* [Crear sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Reanudar sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Recuperar sesión de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Realizar autenticación en el agente de usuario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Recuperación de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Flujos

* [Flujo de autenticación básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Flujo de autenticación básico realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Preguntas frecuentes

* [Preguntas frecuentes sobre la fase de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Paso 2: Compruebe si hay perfiles autenticados existentes {#step-2-check-for-existing-authenticated-profiles}

* **Recuperar perfiles:** El servicio de programación comprueba los perfiles existentes en nombre de la aplicación de streaming llamando al extremo [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).


* **Escenario 1:** Hay perfiles existentes, el Servicio de programador puede continuar con la [Fase de preautorización](#preauthorization-phase) o [Fase de autorización](#authorization-phase).


* **Escenario 2:** No existen perfiles, el Servicio de programador puede continuar con el siguiente paso para [Autenticar al usuario](#step-3-authenticate-the-user).


* **Escenario 3:** No existen perfiles, el Servicio de programador puede proceder a proporcionar al usuario acceso temporal a través de la característica [TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md).

   * Este escenario está fuera del ámbito de este documento. Consulte la documentación de [Flujos de acceso temporales](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) para obtener más información.

### Paso 3: Autenticar al usuario {#step-3-authenticate-the-user}

* **Recuperar configuración:** El servicio de programación recupera la lista de MVPD disponibles llamando al extremo [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

   * El servicio de programación puede implementar un mecanismo de filtrado personalizado para refinar la lista de MVPD de la respuesta de configuración, de modo que la aplicación de streaming muestre solo los proveedores deseados mientras oculta otros (por ejemplo, MVPD en desarrollo, MVPD de prueba, TempPass). Esto garantiza que los usuarios tengan una selección revisada al elegir su proveedor de TV.


* **Crear sesión de autenticación:** El servicio Programador inicia una sesión de autenticación llamando al extremo [**/api/v2/{serviceProvider}/session**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

   * El servicio de programador debe devolver `code` y `url` a la aplicación de streaming.


* **Escenario 1:** La aplicación de streaming puede abrir un explorador o una vista web, por lo que debe cargar la autenticación `url`.

   * El usuario envía su nombre de usuario y contraseña en la página de inicio de sesión de MVPD. Tras la autenticación correcta, la redirección final muestra una página de éxito.


* **Escenario 2:** La aplicación de streaming no puede abrir un explorador, por lo que debe mostrar la autenticación `code`. Se requiere una aplicación web independiente para solicitar al usuario que introduzca `code`, construya la autenticación `url` y abra: [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * El usuario envía su nombre de usuario y contraseña en la página de inicio de sesión de MVPD. Tras la autenticación correcta, la redirección final muestra una página de éxito.

### Paso 4: Comprobación de perfiles autenticados {#step-4-check-for-authenticated-profiles}

* **Recuperar perfil para código específico:** El servicio de programador debe implementar un mecanismo de sondeo usando `code` para comprobar si el perfil se generó correctamente y se guardó llamando al extremo [**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).

   * El servicio de programador debe **iniciar el mecanismo de sondeo** en las siguientes condiciones:

      * **Autenticación realizada en la aplicación principal (pantalla):** El servicio de programador debe comenzar a sondear cuando el usuario llegue a la página de destino final, después de que el componente del explorador cargue la dirección URL especificada para el parámetro `redirectUrl` en la solicitud de extremo de [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

      * **Autenticación realizada en una aplicación secundaria (pantalla):** La aplicación del servicio de programador debe comenzar a sondear en cuanto el usuario inicie el proceso de autenticación, justo después de recibir la respuesta del extremo [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) y mostrar el código de autenticación al usuario.

   * El servicio de programador debe **detener el mecanismo de sondeo** en las siguientes condiciones:

      * **Autenticación correcta:** La información de perfil del usuario se ha recuperado correctamente, lo que confirma su estado de autenticación. En este punto, ya no es necesario el sondeo.

      * **Sesión de autenticación y caducidad del código:** La sesión de autenticación y el código caducan, tal como indica la marca de tiempo `notAfter` (por ejemplo, 30 minutos) en la respuesta de extremo [Sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Si esto sucede, el usuario debe reiniciar el proceso de autenticación y el sondeo con el código de autenticación anterior debe detenerse inmediatamente.

      * **Nuevo código de autenticación generado:** Si el usuario solicita un nuevo código de autenticación en el dispositivo principal (pantalla), la sesión existente ya no es válida y el sondeo con el código de autenticación anterior debe detenerse inmediatamente.

   * El servicio de programación debe **configurar la frecuencia del mecanismo de sondeo** en las siguientes condiciones:

      * **Autenticación realizada en la aplicación principal (pantalla):** El servicio de programador debe sondear cada 3-5 segundos o más.

      * **Autenticación realizada en una aplicación secundaria (pantalla):** El servicio de programador debe sondear cada 3-5 segundos o más.

   * El servicio Programador debe almacenar en caché partes de la información de perfil del usuario en un almacenamiento persistente para evitar solicitudes innecesarias y mejorar la experiencia del usuario.

## C. Fase de preautorización (opcional) {#preauthorization-phase}

El propósito de la fase de preautorización es proporcionar a la aplicación de transmisión la capacidad de presentar un subconjunto de recursos de su catálogo al que el usuario tendría derecho de acceso.

La fase de preautorización puede mejorar la experiencia del usuario cuando abre la aplicación de flujo continuo por primera vez o navega a una nueva sección.

La fase de preautorización no es obligatoria, la aplicación de streaming puede omitir esta fase si desea presentar un catálogo de recursos sin filtrarlos primero según el derecho del usuario.

+++Artículos relacionados

API

* [Recuperar decisiones de preautorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Flujos

* [Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

Preguntas frecuentes

* [Preguntas frecuentes sobre la fase de preautorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Paso 5: buscar recursos autorizados previamente {#step-5-check-for-preauthorized-resources}

* **Recuperar decisiones de preautorización:** El servicio Programador recupera decisiones de preautorización para una lista de recursos llamando al extremo [**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md).

   * El servicio de programador debe pasar la lista de decisiones de permiso y denegar la preautorización a la aplicación de streaming.

   * El servicio Programador no es necesario para almacenar las decisiones de preautorización en el almacenamiento persistente. Sin embargo, se recomienda almacenar en caché las decisiones de permiso en la memoria para mejorar la experiencia del usuario. Esto ayuda a evitar llamadas innecesarias a recursos que ya se han preautorizado, lo que reduce la latencia y mejora el rendimiento.

   * El servicio de programación puede determinar el motivo de una decisión de autorización previa denegada inspeccionando el [código de error y el mensaje](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) incluidos en la respuesta del extremo de autorización previa de Decisions. Estos detalles proporcionan a insight el motivo específico por el que se denegó la solicitud de preautorización, lo que ayuda a informar a la experiencia del usuario o al déclencheur de cualquier tratamiento necesario en la aplicación. Asegúrese de que cualquier mecanismo de reintento implementado para recuperar decisiones de preautorización no genere un bucle interminable si se deniega la decisión de preautorización. Considere la posibilidad de limitar los reintentos a un número razonable y administrar las denegaciones correctamente mostrando comentarios claros al usuario.

   * El servicio de programación puede obtener una decisión de preautorización para un número limitado de recursos en una sola solicitud de API, normalmente hasta 5, debido a las condiciones impuestas por las MVPD. Este número máximo de recursos se puede ver y cambiar después de ponerse de acuerdo con las MVPD a través del [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass por uno de los administradores de la organización o por un representante de autenticación de Adobe Pass que actúe en su nombre.

## D. Fase de autorización {#authorization-phase}

El propósito de la fase de autorización es proporcionar a la aplicación de streaming la capacidad de reproducir recursos que el usuario solicita después de validar sus derechos con MVPD.

La fase de autorización es obligatoria, la aplicación de streaming no puede omitir esta fase si desea reproducir recursos que el usuario solicita, ya que requiere verificar con la MVPD que el usuario tiene derecho antes de liberar el flujo.

+++Artículos relacionados

API

* [Recuperar decisiones de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Flujos

* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

Preguntas frecuentes

* [Preguntas frecuentes sobre la fase de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Paso 6: buscar recursos autorizados {#step-6-check-for-authorized-resources}

* **Recuperar decisión de autorización:** El servicio de programador recupera la decisión de autorización para un recurso específico pasado por la aplicación de transmisión llamando al extremo [**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).

   * El servicio Programador no es necesario para almacenar las decisiones de autorización en almacenamiento persistente.

   * El servicio de programador puede determinar el motivo de una decisión de autorización denegada inspeccionando el [código de error y el mensaje](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) incluidos en la respuesta del extremo de autorización de decisiones. Estos detalles proporcionan a insight la razón específica por la que se denegó la solicitud de autorización, lo que ayuda a informar a la experiencia del usuario o al déclencheur de cualquier manejo necesario en la aplicación de streaming. Asegúrese de que cualquier mecanismo de reintento implementado para recuperar decisiones de autorización no genere un bucle interminable si se deniega la decisión de autorización. Considere la posibilidad de limitar los reintentos a un número razonable y administrar las denegaciones correctamente mostrando comentarios claros al usuario.

   * El Servicio de programador puede evaluar otras reglas comerciales y devolver una decisión de autorización adecuada a la Aplicación de streaming.

   * El servicio Programador no es necesario para actualizar un token de medios caducado mientras el flujo se está reproduciendo activamente. Si el token de medios caduca durante la reproducción, se debe permitir que el flujo continúe ininterrumpidamente. Sin embargo, el cliente debe solicitar una nueva decisión de autorización y obtener un nuevo token de medios la próxima vez que el usuario intente reproducir un recurso.

   * El servicio de programación puede obtener una decisión de autorización para un número limitado de recursos en una sola solicitud de API, normalmente hasta 1, debido a condiciones impuestas por MVPD.

## E. Fase de cierre {#logout-phase}

El propósito de la fase de cierre de sesión es proporcionar a la aplicación de streaming la capacidad de finalizar el perfil autenticado del usuario dentro de la autenticación de Adobe Pass si el usuario lo solicita.

La fase de cierre de sesión es obligatoria, la aplicación de streaming debe proporcionar al usuario la capacidad de cerrar la sesión.

+++Artículos relacionados

API

* [Iniciar el cierre de sesión de un mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

Flujos

* [Flujo de cierre de sesión básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

Preguntas frecuentes

* [Preguntas frecuentes sobre la fase de cierre](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Paso 7: Cerrar sesión {#step-7-logout}

* Iniciar el cierre de sesión de Adobe Pass: el servicio de programación inicia el flujo de cierre de sesión como lo solicita la aplicación de streaming llamando al extremo [/api/v2/{serviceProvider}/logout/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).

   * El Servicio de programador puede limpiar cualquier información que almacene sobre el usuario autenticado.

   * El servicio de programador debe seguir las instrucciones proporcionadas en los atributos `actionName` y `actionType` de la respuesta de extremo de cierre de sesión para garantizar que el proceso de cierre de sesión se complete correctamente.

      * Si el atributo `actionType` de la respuesta se establece en &quot;interactivo&quot;, el servicio de programador debe devolver el valor del atributo `url` a la aplicación de streaming.

         * **Escenario 1:** La aplicación de streaming puede abrir un explorador o una vista web, por lo que debe cargar el cierre de sesión `url`.

         * **Escenario 2:** La aplicación de streaming no puede abrir un explorador, por lo que el proceso de cierre de sesión se puede detener porque la sesión de MVPD no persistió en la memoria caché de un explorador de Dispositivo de streaming.
