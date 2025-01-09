---
title: Preguntas frecuentes sobre el registro dinámico de clientes (DCR)
description: Preguntas frecuentes sobre el registro dinámico de clientes (DCR)
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Preguntas frecuentes sobre el registro dinámico de clientes (DCR) {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Este documento proporciona respuestas de información general elevadas para las preguntas más frecuentes acerca de la adopción de Autenticación de Adobe Pass y Registro dinámico de cliente (DCR).

Para obtener más información sobre el registro de cliente dinámico (DCR) en general, consulte la [Información general sobre el registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

>[!MORELIKETHIS]
>
> * [Preguntas frecuentes sobre la API REST v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)

## Preguntas frecuentes generales {#general-faqs}

Comience con esta sección si está trabajando en una aplicación que necesita integrar el Registro dinámico de clientes (DCR), ya sea una aplicación nueva o una existente que migre de uno de los mecanismos anteriores.

### Preguntas frecuentes sobre la fase de registro {#registration-phase-faqs-general}

+++Preguntas frecuentes sobre la fase de registro

#### 1. ¿Cuál es el propósito de la fase de registro? {#registration-phase-faq1}

El propósito de la fase de registro es registrar la aplicación cliente con la autenticación de Adobe Pass a través del proceso de [Registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr).

El proceso de registro dinámico de clientes (DCR) requiere que la aplicación cliente obtenga un par de credenciales de cliente y recupere un token de acceso como objetivo final de la fase de registro.

Para obtener más información, consulte la [Información general sobre el registro de clientes dinámicos](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 2. ¿Es obligatoria la fase de registro? {#registration-phase-faq2}

La fase de registro es obligatoria, pero la aplicación cliente puede omitir esta fase si tiene un par en caché de credenciales de cliente y un token de acceso que siguen siendo válidos.

#### 3. ¿Qué es una declaración de software y durante cuánto tiempo es válida? {#registration-phase-faq3}

La instrucción de software es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement).

La declaración de software consiste en un token web JSON (JWT) que uno de los administradores de su organización puede generar y descargar desde el [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass o un representante de autenticación de Adobe Pass que actúe en su nombre.

La declaración del software es válida durante un periodo de tiempo ilimitado, pero puede optar por pedirle a un representante de autenticación de Adobe Pass que la revoque en cualquier momento.

La aplicación cliente debe almacenar la instrucción de software y utilizarla cuando necesite recuperar las credenciales del cliente.

Para obtener más información, consulte la [Información general sobre el registro de clientes dinámicos](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 4. ¿Cómo generar y descargar una declaración de software? {#registration-phase-faq4}

Esta operación puede completarla uno de los administradores de su organización o un representante de autenticación de Adobe Pass que actúe en su nombre a través del [Panel de control de TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass.

Para obtener más información, consulte la [Guía del usuario de canales del panel de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) o la [Guía del usuario para programadores del panel de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

#### 5. ¿Qué sucede si se revoca una declaración de software? {#registration-phase-faq5}

Cuando se revoca la declaración del software, hay una consecuencia importante que considerar:

* Las aplicaciones cliente que usen la instrucción de software revocada ya no podrán pasar por los flujos de [derechos](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement), lo que significa que se bloqueará a los usuarios la reproducción de contenido.

#### 6. ¿Qué son las credenciales del cliente y durante cuánto tiempo son válidas? {#registration-phase-faq6}

Las credenciales del cliente son un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials).

Las credenciales del cliente constan de un identificador de cliente y un par de secreto de cliente que se pueden recuperar del extremo de registro de cliente.

Las credenciales del cliente son válidas para un periodo de tiempo ilimitado.

La aplicación cliente debe almacenar indefinidamente las credenciales del cliente y utilizarlas cuando necesite recuperar un token de acceso.

Para obtener más información, consulte la documentación de [Recuperar credenciales de cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

#### 7. ¿Cómo administrar las credenciales del cliente? {#registration-phase-faq7}

Recomendamos que la aplicación cliente administre un par único de credenciales de cliente para cada instancia de aplicación de usuario en caso de integraciones cliente a servidor y servidor a servidor con autenticación de Adobe Pass.

La aplicación cliente debe almacenar indefinidamente las credenciales del cliente y utilizarlas cuando necesite recuperar un token de acceso.

#### 8. ¿Qué sucede si se pierden las credenciales del cliente en caché? {#registration-phase-faq8}

Cuando se pierden las credenciales del cliente en caché, hay tres consecuencias importantes que hay que tener en cuenta:

* La aplicación cliente debe obtener un nuevo par de credenciales de cliente.
* La aplicación cliente debe obtener un nuevo token de acceso utilizando el nuevo par de credenciales de cliente.
* La aplicación cliente deberá solicitar al usuario que vuelva a autenticarse, ya que la aplicación cliente perderá el acceso a los perfiles autenticados obtenidos anteriormente.

#### 9. ¿Qué es un token de acceso y durante cuánto tiempo es válido? {#registration-phase-faq9}

El token de acceso es un término definido en la documentación de [Glosario](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token).

El token de acceso consiste en un [token portador](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) que se puede recuperar del extremo del token de cliente.

El token de acceso es válido durante un periodo de tiempo limitado y corto especificado en el momento de la emisión.

La aplicación cliente debe almacenar el token de acceso y utilizarlo hasta que caduque al segmentar la API de REST V2.

La aplicación cliente debe obtener un nuevo token de acceso antes de que caduque el actual para evitar solicitudes no autorizadas.

Para obtener más información, consulte la documentación de [Recuperar token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

#### 10. ¿Cómo puede la aplicación cliente actualizar un token de acceso? {#registration-phase-faq10}

La aplicación cliente debe actualizar un token de acceso de la misma manera que recuperar un nuevo token de acceso, pero utilizando credenciales de cliente en caché.

La aplicación cliente no debe volver a registrarse para actualizar un token de acceso; en su lugar, debe utilizar las credenciales de cliente almacenadas; de lo contrario, los usuarios deberán volver a autenticarse.

Para obtener más información, consulte la documentación de [Recuperar token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
