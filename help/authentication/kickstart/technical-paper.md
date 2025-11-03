---
title: Acerca de la autenticación de Adobe Pass
description: Acerca de la autenticación de Adobe Pass
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 7ca9d8996756086a6b963c0b6d5b0bb64608ecbc
workflow-type: tm+mt
source-wordcount: '1828'
ht-degree: 0%

---

# Acerca de la autenticación Adobe® Pass {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Acerca de TV Everywhere {#about-tv-everywhere}

Los televidentes de hoy en día esperan un acceso sin problemas a los contenidos de TV de pago en cualquier momento y en cualquier lugar. Con el auge de los dispositivos conectados a Internet, las audiencias consumen contenido en una gama cada vez más amplia de plataformas, entre las que se incluyen:

* Portátiles
* Tablets
* Smartphones
* Sitios web
* Aplicaciones federadas
* Consolas de juegos
* Cuadros de definición superior
* Televisores inteligentes

TV Everywhere es una iniciativa de la industria que garantiza que los suscriptores de Pay TV puedan acceder al contenido que ya pagan a través de múltiples dispositivos, tanto dentro como fuera de sus hogares.

Aunque la TV tradicional (lineal) sigue siendo fuerte, los segmentos de consumo de vídeo que crecen más rápidamente son el contenido con desplazamiento de tiempo, el streaming en línea y las pantallas alternativas. Este cambio ha perturbado el mercado de la distribución de video, convirtiendo a TV Everywhere en una solución crucial que alinea los intereses de **Programadores, proveedores de TV de pago y suscriptores.**

### Metas de la TV en todas partes {#goals-tv-everywhere}

**Objetivo técnico**

* Permite a los clientes de TV de pago acceder fácilmente al contenido al que se han suscrito en todos los dispositivos y plataformas.

**Objetivos empresariales**

* Preservar y fortalecer las relaciones con los clientes existentes, al tiempo que se ofrecen nuevas oportunidades.
* Habilite a los programadores y propietarios de contenido para que lleguen a audiencias más amplias y maximicen el valor del contenido Premium.
* Amplíe las marcas mediante la participación directa en línea con los espectadores.

### Desafíos de la TV en todas partes {#challenges-tv-everywhere}

Con las oportunidades de TV Everywhere vienen desafíos importantes, el derecho es el más crítico. Para que un usuario pueda acceder al contenido de la suscripción, un sistema debe verificar sus derechos.

Las preguntas clave incluyen:

* ¿El usuario tiene una suscripción activa con un proveedor de TV de pago?
* ¿Su suscripción incluye el contenido solicitado?

La determinación de los derechos es especialmente difícil para los programadores y los propietarios de contenido, ya que los operadores de TV de pago controlan los datos de los clientes y los privilegios de acceso.

Más allá de los derechos, surgen varios desafíos técnicos y de integración, entre ellos:

* Desarrollo de una estrategia multidispositivo que garantice un acceso sin problemas.
* Gestión de relaciones complejas entre programadores y proveedores de TV de pago.
* Prevención del acceso fraudulento y aplicación de los términos de servicio.
* Ofrecer una experiencia de autenticación coherente y fácil de usar en sitios web y aplicaciones.
* Mantener un tiempo de salida al mercado rápido para alinearse con los acuerdos de afiliados.
* Control de los costes asociados a varias integraciones.

Estos desafíos hacen que las integraciones directas entre los programadores y varios sistemas de autenticación de TV paga requieran muchos recursos, tanto tiempo como experiencia técnica.

Para superar estos obstáculos, la **autenticación mediante Adobe® Pass** simplifica y optimiza la verificación de derechos, lo que permite un acceso seguro y sin problemas al contenido de TV Everywhere.

## Introducción a la autenticación de Adobe Pass {#introduction-adobe-pass-authentication}

La autenticación de Adobe Pass media de forma segura las transacciones de derechos entre los programadores y los proveedores de TV de pago, lo que garantiza que los clientes adecuados puedan acceder al contenido correcto sin esfuerzo.

![](/help/authentication/assets/programmers-connect-authn.png)

*Algunos de los programadores y proveedores de TV de pago que se conectan mediante la autenticación de Adobe Pass*

**Quién se beneficia de la autenticación de Adobe Pass**

* **Programadores**

  Que desean integrarse fácilmente con los proveedores de TV de pago, también conocidos como MVPD (Multichannel Video Programming Distributors), para llegar a la audiencia más amplia y optimizar los ingresos.

* **Proveedores de TV de pago (MVPD)**

  Que buscan conectarse con varios propietarios de contenido (programadores) a través de una sola integración, mejorando la satisfacción del cliente al facilitar el acceso al contenido de suscripción en línea.

* **Clientes de TV de pago**

  Que quieren ver el contenido por el que ya pagan en cualquier momento, en cualquier lugar, en cualquier dispositivo.

**Programadores**

Los programadores que buscan maximizar el alcance y los ingresos de la audiencia pueden:

* Integre fácilmente con los principales proveedores de TV de pago sin administrar varias conexiones directas.
* Amplíe su audiencia asegurando la autenticación en todos los proveedores y plataformas principales.
* Proteja el contenido Premium con autenticación segura, restringiendo el acceso a los usuarios y dispositivos autorizados.
* Habilite el inicio de sesión único (SSO) para mejorar la experiencia del usuario en todas las aplicaciones y sitios web.

**Proveedores de TV de pago (MVPD)**

Los proveedores de TV de pago pueden mejorar la experiencia del cliente y optimizar las operaciones al:

* Conexión con varios propietarios de contenido a través de una sola integración.
* Ofrece una experiencia de visualización de marca sin problemas en todos los dispositivos y plataformas.
* Garantizar la autenticación segura para evitar el acceso no autorizado y administrar los flujos simultáneos por hogar.

**Clientes de TV de pago**

Los suscriptores se benefician de la televisión en todas partes, lo que les permite ver el contenido que ya pagan en cualquier momento, en cualquier lugar, en cualquier dispositivo.

### Integración con la autenticación de Adobe Pass {#integrating-adobe-pass-authentication}

Tanto si es un proveedor de TV de pago como si es un programador, la integración con la autenticación de Adobe Pass requiere una participación activa. A continuación se ofrece una amplia descripción general del proceso para ambas funciones.

#### Proceso de integración del programador {#programmer-integration-process}

Una vez que la integración se ha iniciado formalmente, encontrará disponibles más directrices, pero el proceso suele incluir los siguientes pasos:

**Requisitos previos**

* Un sistema de administración de contenido (CMS).
* Un mecanismo de entrega de contenido, que puede incluir una red de entrega de contenido de terceros (CDN).
* Una plataforma de vídeo en línea existente con un reproductor de contenidos integrado en un sitio web o una aplicación independiente.

**Tareas de integración**

* Integre la autenticación de Adobe Pass [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).
* Integre la autenticación de Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md).
* Integrar la autenticación de Adobe Pass [Verificador de token de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).
* Desarrollar una interfaz de usuario para el flujo de trabajo de autenticación, autorización y cierre de sesión.

Para obtener más información sobre el proceso de integración del programador, consulte los documentos de [Guía de KickStart del programador](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md).

#### Proceso de integración de proveedor de TV paga {#pay-tv-provider-integration-process}

Una vez que la integración se ha iniciado formalmente, encontrará disponibles más directrices, pero el proceso suele incluir los siguientes pasos:

**Requisitos previos**

* Firme el Acuerdo de no divulgación de autenticación de Adobe Pass (NDA).
* Proporcione especificaciones al equipo de ingeniería de autenticación de Adobe Pass para el sistema de autenticación y autorización. Se recomienda un proveedor de identidad (IdP) basado en SAML para la autenticación y un sistema de autorización basado en SOAP para la integración más sencilla.

**Tareas de integración**

* Establezca la conectividad con los servidores de autenticación de Adobe Pass.
* Complete la versión de ensayo y garantice la calidad.
* Complete el lanzamiento de la producción y garantice la calidad.

La integración estándar es gratuita para los proveedores de TV de pago, incluida la documentación y la asistencia básica por correo electrónico. Sin embargo, los proveedores que requieren una asistencia extensa o plazos acelerados pueden incurrir en una tarifa de soporte o elegir trabajar con un socio de terceros con experiencia, como Synacor.

La autenticación de Adobe Pass puede admitir de forma eficaz la lógica empresarial específica del proveedor de TV de pago de dos formas clave:

* Para una lógica empresarial independiente y aplicada por el proveedor de TV de pago cuando se recibe una solicitud de autorización, Adobe proporciona los datos necesarios (por ejemplo, ID de dispositivo único, dirección IP) para admitir la aplicación.
* Para la lógica empresarial que requiere la intervención del usuario o la administración específica por parte de Adobe, se pueden mantener las propiedades personalizadas para cada proveedor de TV de pago. Estas configuraciones pueden incluir flujos de trabajo predefinidos activados en puntos específicos del proceso de autenticación.

Para obtener más información sobre el proceso de integración del proveedor de TV de pago, consulte los documentos [Guía de KickStart de MVPD](/help/authentication/kickstart/mvpd-kickstart-guide.md) y [Guía de integración de MVPD](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md).

### Flujo de derecho {#entitlement-flow}

La autenticación de Adobe Pass actúa como un proxy y facilita el flujo de asignación de derechos entre programadores y MVPD al ofrecer interfaces seguras y coherentes para ambas partes.

Para los programadores, la autenticación de Adobe Pass proporciona API como parte de un nivel **Standard** o **Premium**:

* API de autenticación estándar de Adobe Pass:
   * [DCR de API de REST](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [API DE REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* API de autenticación de Adobe Pass Premium:
   * [Restablecer API de pase temporal](/help/premium-workflow/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [Función TempPass](/help/premium-workflow/temporary-access/temp-pass-feature.md)
   * [API de degradación](/help/premium-workflow/degraded-access/degradation-feature.md#degradation-api-access)
      * [Función de degradación](/help/premium-workflow/degraded-access/degradation-feature.md)
   * [API de supervisión del servicio de derechos](/help/premium-workflow/esm/entitlement-service-monitoring-api.md)

Para obtener más información sobre el flujo de derechos, consulte la documentación de [Programmer Integration Guide](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow).

#### Explicación de derechos {#understanding-entitlements}

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

#### Creación de la interfaz de usuario {#building-user-interface}

Los programadores son responsables de diseñar e implementar la interfaz de usuario (IU) para el flujo de trabajo de asignación de derechos en su sitio web o aplicación, mientras que ciertos elementos, como el proceso de inicio de sesión, los gestiona el proveedor de TV de pago.

Como mínimo, los programadores deben:

* **Implementar una interfaz de selección de proveedores**
   * Permitir que los nuevos usuarios identifiquen a su proveedor de TV de pago e inicien sesión por primera vez.
   * Algunos proveedores de TV de pago redirigen a los usuarios a una página de inicio de sesión externa, mientras que otros requieren el inicio de sesión dentro de un iframe. Los programadores deben implementar una función de llamada de retorno para generar el iframe cuando sea necesario.

* **Administrar una lista de proveedores de TV de pago admitidos**
   * Asegúrese de que los usuarios solo puedan acceder al contenido a través de proveedores aprobados.

* **Indicar estado de autenticación**
   * Mostrar cuándo se autentica un usuario dentro de la aplicación o del sitio web.

* **Identificar recursos protegidos**
   * Indique claramente qué contenido requiere autorización antes de visualizarlo.
   * Actualice la IU para reflejar la autorización correcta una vez concedido el acceso.

## FAQ {#faqs}

**¿Qué es la TV en todas partes?**

TV Everywhere es una iniciativa del sector que permite a los clientes de TV de pago acceder al contenido premium al que ya están suscritos a través de varios dispositivos conectados a Internet. Esto incluye computadoras personales, tabletas, smartphones, consolas de juegos, decodificadores y televisores inteligentes. El principal desafío es garantizar un proceso de autenticación fluido y fácil de usar, que permita a los clientes acceder a su contenido de suscripción sin varios inicios de sesión o barreras técnicas.

**¿Qué es la autenticación de Adobe Pass y cómo admite TV en todas partes?**

La autenticación de Adobe Pass hace que la TV en todas partes cobre vida al verificar de manera segura el derecho de un usuario al contenido de una manera simple y eficiente. Es un servicio alojado que facilita una rápida integración back-end basada en reglas comerciales establecidas por los programadores y los proveedores de TV de pago. Esto se traduce en un tiempo de comercialización más rápido para todas las partes interesadas, un entorno más seguro que minimiza el fraude y una mejor experiencia de usuario, con más contenido de TV accesible en varias plataformas.

**¿Cómo se entrega la autenticación de Adobe Pass?**

La autenticación de Adobe Pass se proporciona como una solución de software como servicio (SaaS). Este método garantiza una comunicación segura entre los usuarios finales, los programadores y los proveedores de TV de pago para la validación de derechos de contenido.

**¿Qué diferencia la autenticación de Adobe Pass de otras soluciones de TV en todas partes?**

La autenticación de Adobe Pass ofrece varias ventajas con respecto a las soluciones alternativas:

* **Inicio de sesión único (SSO) fluido**: a diferencia de las integraciones directas con proveedores individuales, la autenticación de Adobe Pass permite una experiencia de inicio de sesión persistente a medida que los usuarios se mueven entre diferentes sitios web y aplicaciones.
* **Penetración amplia en el mercado**: Una vez que un programador se integra con la autenticación de Adobe Pass, obtiene acceso de inmediato a los operadores de TV de pago que cubren más del 90% de los hogares de los Estados Unidos.
* **Integración con el ecosistema de Adobe**: funciona a la perfección con otras soluciones de Adobe para la entrega de contenido, la protección y la monetización, incluido Adobe Analytics.

**¿Cuán segura es la autenticación de Adobe Pass?**

La seguridad es una prioridad. La autenticación de Adobe Pass garantiza que solo los usuarios autorizados puedan acceder al contenido premium enlazando el acceso al dispositivo del usuario. También proporciona opciones para limitar el número de flujos, sesiones o dispositivos simultáneos por hogar.

**¿Qué dispositivos admite la autenticación de Adobe Pass?**

La autenticación de Adobe Pass está diseñada para funcionar en una amplia gama de dispositivos, como dispositivos basados en la web, dispositivos móviles o dispositivos conectados a TV.

**¿La autenticación de Adobe Pass cuesta algo para los usuarios finales?**

No. La autenticación de Adobe Pass es gratuita para los usuarios finales.
