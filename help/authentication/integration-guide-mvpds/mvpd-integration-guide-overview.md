---
title: Guía de integración de MVPD
description: Guía de integración de MVPD
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# Guía de integración de MVPD {#mvpd-integration-guide}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Esta guía de integración está dirigida a los distribuidores de programación de vídeo multicanal (MVPD) que planean integrarse con la autenticación Adobe® Pass.

TV Everywhere (TVE) es una iniciativa transformadora en la industria de la TV de pago, que permite a los suscriptores acceder a los contenidos que ya pagan a través de múltiples dispositivos, ya sea en casa o fuera de casa. Para los proveedores de TV de pago, TVE ofrece oportunidades significativas entre las que se encuentran el fortalecimiento de las relaciones existentes con los clientes y la apertura de puertas a nuevas relaciones. Sin embargo, estas oportunidades conllevan desafíos.

En el ecosistema de TVE, **Programmers** proporciona el contenido, mientras que **MVPD** (Multichannel Video Programming Distributors) administra los datos de clientes necesarios para verificar si los espectadores son suscriptores elegibles. Aunque la coordinación de la autenticación y la autorización con un solo Programador puede ser manejable, hacerlo con docenas o incluso cientos de Programadores presenta una complejidad considerable.

Aquí es donde **Autenticación mediante Adobe® Pass** simplifica el proceso. Las MVPD solo necesitan implementar una integración única y optimizada con Adobe Pass para tener acceso a todo el ecosistema de TVE. El marco de integración proporcionado acelera el tiempo de salida al mercado, proporciona un entorno seguro para mitigar el fraude y mejora la experiencia del cliente al ofrecer más contenido de TV en varias plataformas.

## Autenticación de Adobe Pass para TV en todas partes {#adobe-pass-authentication-for-tv-everywhere}

La autenticación de Adobe Pass funciona como una solución SaaS (Software as a Service) diseñada para permitir la integración rápida entre servidores, cumpliendo con las reglas comerciales tanto de los distribuidores de programación de vídeo multicanal (MVPD) como de los programadores.

### Estándares y protocolos {#standards-protocols}

La autenticación de Adobe Pass está totalmente alineada con los estándares y protocolos emergentes para TV en todas partes (TVE), lo que permite una integración y conformidad sin problemas en todo el ecosistema.

* Especificación de **CableLabs OLCA (acceso de contenido en línea)**\
  La autenticación de Adobe Pass se adhiere a la especificación OLCA de CableLabs, que define los requisitos técnicos y la arquitectura para ofrecer contenido de vídeo desde fuentes en línea a clientes de TV de pago. Adobe participó activamente en el proyecto de pruebas de interoperabilidad de CableLabs en junio de 2011, superando con éxito el proceso de prueba de una implementación de Service Provider.

La autenticación de Adobe Pass está diseñada para admitir varios protocolos (por ejemplo, SAML, OAuth 2.0, etc.), lo que permite realizar futuras expansiones, incluidos protocolos personalizados, según las necesidades en evolución.

La mayoría de las integraciones utilizan el protocolo SAML (Security Assertion Markup Language), un estándar principal para la autenticación. La autenticación de Adobe Pass actúa como un proveedor de servicios proxy dentro del marco de SAML, conservando la respuesta de autenticación de SAML como un token seguro en el dominio común de Adobe.

En resumen, la autenticación de Adobe Pass no depende de protocolos, y está diseñada para cumplir estrechamente con los estándares OLCA. Estos estándares establecen un marco común para programadores, MVPD y proveedores de servicios, lo que garantiza un enfoque cohesivo para la implementación de funciones de TVE.

### Integración y asistencia {#integration-support}

La autenticación de Adobe Pass colabora con los equipos técnicos de MVPD para configurar integraciones adaptadas a sus requisitos específicos, de la siguiente manera:

* **Integraciones estándar**\
  Se ofrece de forma gratuita, con documentación y asistencia básica por correo electrónico.

* **Soporte mejorado**\
  Para las personalizaciones significativas o los plazos acelerados, puede aplicarse una tarifa de soporte.

La autenticación de Adobe Pass admite la administración eficiente de la lógica empresarial de MVPD, como se indica a continuación:

* **Lógica empresarial independiente**\
  Para la lógica empresarial aplicada por completo por MVPD durante las solicitudes de autorización, Adobe proporciona los datos necesarios. Estos datos pueden incluir, entre otros, el ID de dispositivo único y la dirección IP del dispositivo del usuario que realiza la solicitud.

* **Compatibilidad con propiedades personalizadas**\
  Para la lógica empresarial que requiere la intervención del usuario o la administración específica de Adobe, Adobe puede mantener configuraciones personalizadas para cada MVPD. Estas políticas permiten flujos de trabajo predefinidos que se pueden activar en puntos específicos del flujo de trabajo de derechos. Para obtener más información, póngase en contacto con su representante de Adobe.

## Flujo de derecho {#entitlement-flow}

El flujo de derechos es una serie de pasos que una aplicación de Programador (TVE) debe completar para transmitir contenido protegido. El flujo incluye varias fases que implican interacciones con MVPD:

* [Fase de autenticación](#authentication-phase)
* (Opcional) Fase de preautorización
* [Fase de autorización](#authorization-phase)
* Fase de cierre de sesión

En la visita inicial de un usuario a una aplicación de Programador (TVE), el flujo de derechos sigue la secuencia descrita. Sin embargo, en visitas posteriores, la aplicación puede omitir ciertos pasos según el estado de la autenticación y las políticas de visualización aplicables.

>[!NOTE]
>
> La aplicación Programador (TVE) se utiliza en este documento para hacer referencia de forma colectiva a los tipos de aplicaciones que se ejecutan en diferentes plataformas (navegadores, dispositivos móviles, dispositivos conectados a TV, etc.) compatibles con la autenticación de Adobe Pass.

### Fase de autenticación {#authentication-phase}

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

### Fase de autorización {#authorization-phase}

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
