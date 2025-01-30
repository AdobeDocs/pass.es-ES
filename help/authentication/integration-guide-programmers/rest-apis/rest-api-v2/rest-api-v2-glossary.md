---
title: Glosario de la API de REST 2
description: Glosario de la API de REST 2
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# Glosario de la API de REST 2 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Este documento proporciona definiciones de los términos utilizados al integrar la API de REST de autenticación de Adobe Pass V2.

>[!MORELIKETHIS]
>
> * [Glosario de registro dinámico de clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## Términos del glosario {#glossary-terms}

### A {#a}

#### Autenticación {#authentication}

La autenticación es un proceso que permite a un usuario probar su identidad ante un [Programador](#programmer), para obtener acceso al contenido protegido ([recurso](#resource)), después de validar la suscripción del usuario con [MVPD](#mvpd).

#### Código de autenticación {#code}

El código de autenticación es un concepto de autenticación de Adobe Pass que almacena un valor único generado cuando un usuario inicia el proceso de [autenticación](#authentication) e identifica de forma exclusiva la [sesión de autenticación](#session) de un usuario hasta que se complete el proceso de autenticación.

El código de autenticación lo puede usar una aplicación [Primary (Programmer)](#primary-application) o una aplicación [Secondary (Programmer)](#secondary-application) para completar el proceso de [autenticación](#authentication), recuperar información sobre la [sesión de autenticación](#session) o para obtener acceso al usuario [perfil](#profile).

Sinónimo del código de registro usado anteriormente.

#### Sesión de autenticación {#session}

La sesión de autenticación es un concepto de autenticación de Adobe Pass que almacena información sobre el proceso de autenticación iniciado (o continuado) por el usuario desde una aplicación de [Programmer](#programmer), y que se identifica de forma única mediante un [código de autenticación](#code).

La sesión de autenticación también puede indicar que la aplicación [Programmer](#programmer) debe continuar con el proceso de [authorization](#authorization) como la siguiente fase del flujo de [derechos](#entitlement) en caso de que el usuario ya esté autenticado.

#### Autorización {#authorization}

La autorización es un proceso que permite a un usuario tener acceso al contenido protegido ([recurso](#resource)) desde un catálogo de [Programmer](#programmer) basado en la suscripción de [MVPD](#mvpd) de propiedad, después de validar los derechos de usuario con [MVPD](#mvpd).

### C {#c}

#### Configuración {#configuration}

La configuración es un concepto de autenticación de Adobe Pass que almacena información sobre la configuración de la integración de [Programmer](#programmer) y [MVPD](#mvpd) y que se puede usar durante el proceso de [autenticación](#authentication) al pedirle al usuario que seleccione su [proveedor de TV](#tv-provider) de una lista de integraciones activas.

### D {#d}

#### Decisión {#decision}

La decisión es un concepto de autenticación de Adobe Pass que almacena información sobre la consulta de proceso de [MVPD](#mvpd) [authorization](#authorization) o [preauthorization](#preauthorization) para permitir o denegar el acceso del usuario a contenido protegido por [Programmer](#programmer).

#### Degradación {#degradation}

La degradación es una función de autenticación de Adobe Pass que permite a un usuario acceder a contenido protegido incluso cuando su [MVPD](#mvpd) experimenta una interrupción del servicio.

Para obtener más información, consulte la [documentación sobre la característica de degradación](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md).

#### ID de dispositivo {#device-id}

La ID del dispositivo es un identificador único enlazado al dispositivo del usuario y la aplicación [Programmer](#programmer) debe proporcionarlo en todas las fases del flujo de [derechos](#entitlement).

### E {#e}

#### Derecho {#entitlement}

El derecho es un concepto de autenticación de Adobe Pass que incorpora los flujos y las características disponibles que ayudan a un usuario a pasar por diferentes fases para acceder al contenido protegido, desde la [autenticación](#authentication), la [preautorización](#preauthorization), la [autorización](#authorization) y, finalmente, el [cierre de sesión](#logout).

#### Código de error mejorado {#enhanced-error-code}

El código de error mejorado es un concepto de autenticación de Adobe Pass que proporciona información adicional sobre el error que se produjo al procesar una solicitud.

Para obtener más información, consulte la documentación de [Códigos de error mejorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

### H {#h}

#### HBA {#hba}

La autenticación basada en el hogar (HBA) es el proceso mediante el cual se concede automáticamente a un consumidor acceso al contenido de [TV Everywhere (TVE)](#tve) en determinados dispositivos que están conectados a su red doméstica, lo cual forma parte de la ubicación incluida en el contrato de suscripción.

### I {#i}

#### Proveedor de identidad {#identity-provider}

El proveedor de identidad es una compañía que proporciona servicios de identidad a los consumidores a través de cable, satélite o servicios basados en Internet en el contexto de [TV en todas partes (TVE)](#tve).

Sinónimo de [MVPD](#mvpd) y [Proveedor de TV](#tv-provider).

### L {#l}

#### Cerrar sesión {#logout}

El cierre de sesión es un proceso que permite a un usuario finalizar su [perfil](#profile) autenticado en la autenticación de Adobe Pass y actualizar la aplicación [Programmer](#programmer) para reflejar el estado del usuario.

### M {#m}

#### Token de medios {#media-token}

El token de medios es un token generado por la autenticación de Adobe Pass como resultado de una autorización [decisión](#decision) pensada para proporcionar acceso al contenido protegido.

El token multimedia se pasa a [Programmer](#programmer), que lo valida para garantizar la seguridad de acceso para ese [recurso](#resource).

Sinónimo de término anterior utilizado para el token de autorización corto.

#### Media Token Verifier {#media-token-verifier}

Media Token Verifier es una biblioteca distribuida por Adobe Pass Authentication que se encarga de verificar la autenticidad de un [token multimedia](#media-token).

Para obtener más información, consulte la documentación de [Comprobador de token de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

#### MVPD {#mvpd}

El distribuidor de programación de vídeo multicanal (MVPD) es una empresa que proporciona servicios de televisión a los consumidores a través de cable, satélite o servicios basados en Internet.

MVPD se identifica con un valor único definido durante el proceso de incorporación entre MVPD y Adobe.

Sinónimo de [Proveedor de TV](#tv-provider) y [Proveedor de identidad](#identity-provider).

### P {#p}

#### Socio {#partner}

El socio es una compañía que proporciona un servicio o marco a un [Programador](#programmer) para habilitar una experiencia de usuario de inicio de sesión único.

El socio se identifica con un valor único (por ejemplo, &quot;manzana&quot;) que se define durante el proceso de incorporación entre el socio y el Adobe.

#### Preautorización {#preauthorization}

La preautorización es un proceso que permite a un usuario obtener una vista previa de un subconjunto de [recursos](#resource) de un catálogo de [Programador](#programmer) al que tendría derecho después de validar los derechos de usuario con [MVPD](#mvpd).

Sinónimo de [Comprobación preliminar](#preflight).

#### Comprobación preliminar {#preflight}

La comprobación preliminar es un proceso que permite al usuario obtener una vista previa de un subconjunto de [recursos](#resource) de un catálogo de [Programador](#programmer) al que tendría derecho después de validar los derechos de usuario con [MVPD](#mvpd).

Sinónimo de [Preauthorization](#preauthorization).

#### Aplicación primaria (programador) {#primary-application}

La aplicación principal hace referencia a una aplicación de [Programmer](#programmer) que inicia la [autenticación](#authentication), pero que puede que no sea capaz de completar el proceso mediante un [agente de usuario](#user-agent) para navegar a la página de inicio de sesión de [MVPD](#mvpd).

#### Perfil {#profile}

El perfil es un concepto de autenticación de Adobe Pass que almacena información sobre la fecha de inicio y finalización de la autenticación del usuario, los [metadatos del usuario](#user-metadata) junto con otros campos que indican el método para obtener la autenticación (por ejemplo, &quot;normal&quot;, &quot;degradado&quot;, &quot;temporal&quot;, &quot;inicio de sesión único&quot;, etc.).

Sinónimo del término anterior utilizado para el token de autenticación.

#### Programador {#programmer}

El programador es una empresa que proporciona contenido a los consumidores a través de canales propios (marcas) en varias plataformas.

El programador agrupa varios canales propios (marcas) como [proveedores de servicios](#service-provider) en su integración con la autenticación de Adobe Pass.

#### Proxy MVPD {#proxy-mvpd}

El proxy MVPD es una empresa que proporciona servicios de identidad para otras MVPD e integra directamente con la autenticación de Adobe Pass.

#### MVPD de proxy {#proxied-mvpd}

MVPD con proxy es una empresa que no se integra directamente con la autenticación de Adobe Pass, pero que se integra mediante [MVPD con proxy](#proxy-mvpd).

#### Identidad de plataforma {#platform-identity}

La identidad de plataforma es una carga útil de identificador de plataforma única generada por un servicio o marco (biblioteca) enlazado al dispositivo del usuario y se proporciona al [Programador](#programmer) para habilitar una experiencia de usuario de inicio de sesión único.

Para obtener más información, consulte la documentación de [Inicio de sesión único mediante flujos de identidad de plataforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

### R {#r}

#### Recurso {#resource}

El recurso es un contenido protegido al que un usuario intenta obtener acceso desde un catálogo de [Programmer](#programmer).

El recurso se identifica con un valor único acordado entre el programador y los MVPD.

Para obtener más información, consulte la documentación de [Recursos protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

### S {#s}

#### SAML {#saml}

El lenguaje de marcado de afirmación de seguridad (SAML) es un estándar abierto para intercambiar datos de autenticación y autorización entre partes, en particular, entre un [proveedor de identidad](#identity-provider) y un [proveedor de servicios](#sp).

#### Aplicación secundaria (programador) {#secondary-application}

La aplicación secundaria hace referencia a una aplicación de [Programmer](#programmer) que puede completar el proceso de [autenticación](#authentication) con un [agente de usuario](#user-agent) para navegar a la página de inicio de sesión de [MVPD](#mvpd).

La aplicación secundaria puede ejecutarse en el mismo dispositivo que la aplicación principal o en un dispositivo diferente (secundario), en cuyo caso la experiencia de inicio de sesión se denomina experiencia del usuario &quot;segunda autenticación de pantalla&quot;.

#### Token de servicio {#service-token}

El token de servicio es un identificador de usuario único generado por un servicio o marco (biblioteca) enlazado al usuario y se proporciona a [Programador](#programmer) para habilitar una experiencia de usuario de inicio de sesión único.

Para obtener más información, consulte la documentación de [Inicio de sesión único mediante flujos de token de servicio](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

#### Proveedor de servicios {#service-provider}

El proveedor de servicios es un canal (marca) propiedad de [Programmer](#programmer).

El proveedor de servicios se identifica mediante un valor único definido durante el proceso de incorporación entre el programador y el Adobe.

Sinónimo del antiguo término utilizado ID de solicitante.

#### SLO {#slo}

El cierre de sesión único (SLO) es un proceso que permite a un usuario cerrar la sesión de todas las aplicaciones que formaban parte del [inicio de sesión único (SSO)](#sso).

#### SP {#sp}

El proveedor de servicios (SP) hace referencia a la función que desempeña la autenticación de Adobe Pass en nombre de un [programador](#programmer) en una integración con un [MVPD](#mvpd).

#### SSO {#sso}

El inicio de sesión único (SSO) es un proceso que permite a un usuario autenticarse una vez y obtener acceso a varias aplicaciones de [Programmer](#programmer) sin necesidad de autenticarse en cada una de ellas.

### T {#t}

#### TempPass Basic {#temp-pass-basic}

TempPass básico es una función de autenticación de Adobe Pass que permite a un usuario acceder al contenido protegido durante un tiempo limitado sin necesidad de autenticarse con un [MVPD](#mvpd).

Para obtener más información, consulte la [documentación básica de TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass).

#### Promocional de TempPass {#temp-pass-promotional}

El TempPass promocional es una función de autenticación de Adobe Pass que permite a un usuario acceder al contenido protegido durante un número máximo de recursos y un tiempo limitado sin necesidad de autenticarse con un [MVPD](#mvpd).

Para obtener más información, consulte la [Documentación promocional de TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass).

#### TTL {#ttl}

El tiempo de vida (TTL) es un valor que indica la cantidad de tiempo que una entidad subyacente es válida.

Se puede mencionar el TTL para un [token de acceso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token), un [perfil](#profile), una [decisión](#decision) de autorización o un [token multimedia](#media-token).

#### TVE {#tve}

La TV en todas partes (TVE) es un nicho en la industria que permite a los consumidores acceder a sus programas de televisión, películas y otros contenidos favoritos en múltiples dispositivos, como smartphones, tabletas, portátiles y muchos más.

#### Tablero de TVE {#tve-dashboard}

El Tablero de TV en todas partes (TVE) es una herramienta de autenticación de Adobe Pass que se proporciona a [Programadores](#programmer) para administrar su configuración y sus datos.

Para obtener más información, consulte la [Guía del usuario del panel de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

#### Proveedor de TV {#tv-provider}

El proveedor de TV es una compañía que proporciona servicios de televisión a los consumidores a través de servicios por cable, satélite o basados en Internet.

El proveedor de TV se identifica con un valor único definido durante el proceso de incorporación entre el proveedor de TV y el Adobe.

Sinónimo de [MVPD](#mvpd) y [proveedor de identidad](#identity-provider).

### U {#u}

#### Agente de usuario {#user-agent}

El agente de usuario hace referencia a un navegador o componente similar (específico de la plataforma) capaz de navegar por la web y procesar la página de inicio de sesión de [MVPD](#mvpd).

#### ID de usuario {#user-id}

El identificador de usuario es un identificador único enlazado al usuario y se origina a partir del proceso de autenticación de [MVPD](#mvpd).

#### Metadatos del usuario {#user-metadata}

Los metadatos del usuario hacen referencia a atributos específicos del usuario (por ejemplo, códigos postales, clasificaciones parentales, ID de usuario, etc.) que mantiene [MVPD](#mvpd) y que proporciona la autenticación de Adobe Pass como parte de un [perfil](#profile).

Para obtener más información, consulte la documentación de [Metadatos de usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

### V {#v}

#### VSA {#vsa}

La cuenta de suscriptor de vídeo (VSA) es un módulo desarrollado por Apple que se proporciona a [Programador](#programmer) para habilitar una experiencia de usuario de inicio de sesión único.

Para obtener más información, consulte la documentación de [Marco de la cuenta del suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) y [Inicio de sesión único mediante flujos de socios](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
