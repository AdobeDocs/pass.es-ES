---
title: Acerca de la autenticación de Adobe Pass y TV en todas partes
description: Acerca de la autenticación de Adobe Pass y TV en todas partes
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '6288'
ht-degree: 0%

---

# Acerca de la autenticación de Adobe Pass y TV en todas partes {#about-auth-tve}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Acerca de TV Everywhere {#about-tve}

Los televidentes de hoy en día pueden conectarse en cualquier momento y lugar, y esperan que su capacidad para acceder a contenido de televisión de pago esté ahí con ellos. Además, las audiencias están viendo contenido mediante una gama cada vez mayor de dispositivos compatibles con Internet, entre los que se incluyen:

* Portátiles
* Tablets
* Smartphones
* Sitios web
* Aplicaciones federadas
* Consolas de juegos
* Cuadros de definición superior
* Televisores inteligentes

TV Everywhere es el movimiento de la industria que apoya la capacidad de los suscriptores de Pay TV para acceder al mismo contenido que ya están pagando, a través de múltiples dispositivos, tanto dentro como fuera de sus hogares. Aunque la mayor parte de la visualización de televisión todavía se produce en la televisión convencional lineal, el crecimiento del consumo se da en el contenido desplazado en el tiempo, el vídeo en línea y las pantallas alternativas. Como resultado, el mercado de distribución de video hoy está en un estado de disrupción, y TV Everywhere ha surgido como la solución que alinea los intereses de los programadores, los proveedores de TV paga y los suscriptores de TV paga.


El objetivo técnico de TV Everywhere es permitir que los clientes de TV de pago accedan a contenido al que ya están suscritos en todos sus dispositivos y plataformas.


Los objetivos comerciales de TV Everywhere son:

* **Conservar las relaciones existentes con los clientes y habilitar las nuevas**
* Permita que los programadores y los propietarios de contenido lleguen a la audiencia más amplia y capturen más valor del contenido premium
* Amplíe las marcas mediante la interacción directa en línea con los espectadores

## Desafíos de TV Everywhere {#tve-challenges}

Junto con las oportunidades de la televisión en todas partes vienen desafíos. En el centro de esto está el derecho. Antes de que un usuario acceda al contenido de la suscripción, debe determinar si tiene derecho a ese acceso.

¿El usuario tiene una suscripción con un proveedor de TV de pago? En caso afirmativo, ¿incluye dicha suscripción el contenido que se solicita? El derecho es especialmente difícil de determinar directamente para los programadores y propietarios de contenido, ya que son los operadores de TV de pago los que tienen los datos de identificación de sus clientes, así como los privilegios de acceso de sus clientes.


Más allá de los derechos, hay una serie de desafíos técnicos y de integración relacionados, entre ellos:

* Desarrollar y promulgar una estrategia integral para varios dispositivos
* Coordinar las innumerables relaciones entre programadores y proveedores de TV de pago
* Prevención del acceso fraudulento o del abuso de los términos de servicio
* Proporciona una experiencia de autenticación coherente y sin frustraciones para los usuarios de distintos sitios web y aplicaciones
* Mantener un tiempo rápido de comercialización para mantenerse al día con las ofertas de afiliados
* Administración de los costes asociados a varias integraciones

Estos desafíos hacen que realizar y mantener integraciones complejas y directas entre los programadores y los sistemas de autenticación de múltiples proveedores de TV paga requiera una gran cantidad de recursos, lo que requiere tiempo y sofisticación técnica.


¿La solución? **Adobe ® autenticación de aprobación**.

## Introducción a la autenticación de Adobe Pass {#authentication-intro}

Con la autenticación de Adobe Pass, los programadores y los proveedores de TV de pago solo necesitan realizar una integración sencilla, mediante las API de autenticación de Adobe Pass, para obtener acceso a todo el ecosistema, lo que incluye:

* Programadores como Turner Broadcasting (TBS, TNT, CNN), Fox Broadcast Networks y Hulu

* Todos los principales proveedores de TV de pago en los Estados Unidos, que comprenden más del 90% de todos los hogares de TV de pago de los Estados Unidos

Además, la autenticación de Adobe Pass proporciona el marco que hace que la autenticación y autorización de usuarios sean sencillas y seguras.

![](../assets/programmers-connect-authn.png)

*Figura 1: Algunos de los programadores y proveedores de TV de pago que se conectan mediante la autenticación de Adobe Pass...*

Adobe Pass media de forma segura las transacciones de derechos entre los programadores y los proveedores de TV de pago, lo que facilita el acceso del visualizador al contenido de suscripción. O, en otras palabras...

La autenticación de **Adobe Pass facilita y agiliza el acceso de los clientes adecuados al contenido correcto.**

**¿Para quién es la autenticación de Adobe Pass?**

* **Programadores** que desean integrarse fácilmente con proveedores de TV de pago (también conocidos como &quot;MVPD&quot; o &quot;Distribuidores de Programación de Video Multicanal&quot;), llegando a la audiencia más amplia, para obtener ingresos óptimos. Con la autenticación de Adobe Pass, los programadores pueden autenticar a los visualizadores en todos los proveedores principales, independientemente de la plataforma del cliente.

* **Proveedores de TV de pago/MVPD** que buscan una conectividad sin problemas con varios Programadores y una mayor satisfacción del cliente al facilitar el acceso al contenido de suscripción en línea.

* **Clientes de TV de pago** que desean tener fácil acceso al contenido al que ya se han suscrito, estén donde estén, sin cargo adicional. El inicio de sesión único proporciona autenticación de visor segura en la web o en todas las aplicaciones móviles, sin necesidad de descargas de clientes ni inicios de sesión repetidos, así como una buena experiencia de usuario.

Para **programadores**, la autenticación de Adobe Pass proporciona:

* Fácil integración y conectividad instantánea con los principales proveedores de TV de pago, sin el dolor de múltiples integraciones directas
* Optimización de los ingresos por suscripción (licencias) y publicidad al admitir la mayor cantidad posible de audiencias de contenido
* Autenticación segura, con acceso al contenido premium concedido solo a usuarios/dispositivos autorizados
* Un marco de trabajo abierto y flexible, independiente del reproductor y de la plataforma DRM. La reproducción puede producirse en una amplia variedad de plataformas, como iOS, Android, Windows 8, consolas de juegos, decodificadores y mucho más.
* Compatibilidad con cualquier tecnología DRM, como Flash Access de Adobe ® o Play Ready®.
* Compatibilidad con la autenticación y autorización de inicio de sesión único (SSO), de modo que los suscriptores no tengan que iniciar sesión de nuevo después de su primera autenticación en su propio sistema.


Para **proveedores de TV de pago/MVPD**, la autenticación de Adobe Pass proporciona:

* Fácil integración con los propietarios de contenido, proporcionando conectividad instantánea con varios programadores con una sola integración
* Participación mejorada del cliente al admitir una experiencia de marca fluida a medida que ve el contenido en varias plataformas y dispositivos
* Autenticación segura que garantiza que solo los usuarios o dispositivos autorizados tengan acceso al contenido premium y (opcionalmente) limita el número de dispositivos y flujos simultáneos que se pueden conectar por cuenta doméstica.


Para **clientes de TV de pago**, la autenticación de Adobe Pass proporciona:

* **Televisión por todas partes!**

El resto de este documento proporciona una introducción técnica a la autenticación de Adobe Pass.  Aunque gran parte de lo siguiente se centra en la integración del programador, también hay información general y específica que se aplica a los proveedores de TV de pago. Este documento también destaca la seguridad y la integridad de cómo la autenticación de Adobe Pass funciona como una solución para TV en todas partes. Para obtener más información más allá de este documento, comuníquese con el representante del Adobe o complete el formulario de solicitud de información [aquí](https://www.adobe.com/).

## Bloques de creación arquitectónicos {#arch-building-blocks}

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/import-pc7mz3dfnv/check.gif) A continuación se describen las transacciones de derechos centrales de autenticación y autorización. La autenticación es el proceso de confirmar con un proveedor de TV de pago que un usuario determinado es un cliente conocido. La autorización es el proceso por el cual un proveedor de TV de pago confirma que un usuario autenticado tiene una suscripción válida a un recurso determinado.
La autenticación de Adobe Pass consta de los siguientes componentes básicos:

* Componente de cliente (uno de los siguientes):

   * El activador de acceso: una biblioteca específica de la plataforma que proporciona API fáciles de usar y ejemplos de código para implementar los flujos de derechos
   * La API de cliente: servicios web RESTful; proporciona puntos finales de flujo de asignación de derechos para plataformas sin capacidades de representación de páginas web (como consolas de juegos, descodificadores, etc.)

* Servidores back-end alojados en Adobe
* Media Token Verifier
* Un Medium de Exchange seguro y central (tokens)

En un nivel básico, la autenticación de Adobe Pass consta de tres componentes (el activador de acceso, los servidores back-end alojados en el Adobe y el verificador de tokens de medios) y un elemento central de intercambio (tokens).

### Componentes de cliente {#client-components}

* Habilitador de acceso
* API sin cliente

#### Habilitador de acceso {#access-enabler}

En plataformas totalmente compatibles (incluidas la web, iOS, Android y Windows 8), los programadores interactúan con la autenticación de Adobe Pass mediante el componente de cliente Access Enabler. Este componente facilita todas las interacciones de autenticación y autorización con el cliente.  El Habilitador de acceso se ejecuta localmente en su sistema. Cuando un usuario accede a un sitio o aplicación del programador y solicita contenido, el componente del activador de acceso, alojado o mantenido por el Adobe, se carga en segundo plano de forma silenciosa.

El Access Enabler gestiona los flujos de trabajo de asignación de derechos reales, mientras que el Programador es responsable de la página web de nivel superior o de la aplicación de reproducción que implementa la interfaz de usuario e interactúa con el Access Enabler. Estas interacciones se realizan a través de un sistema asincrónico de funciones y llamadas de retorno, definido por la API de Access Enabler.

Estos son los flujos de derechos básicos, fácilmente implementados mediante la API de Access Enabler:

* Configuración de la identidad del solicitante (programador)
* Comprobación/obtención de la autenticación de un usuario con un operador de TV de pago específico (el &quot;proveedor de identidad&quot;)
* Comprobación/obtención de la autorización de un usuario para un recurso concreto
* Cerrando sesión del usuario

El Habilitador de acceso también proporciona los siguientes servicios:

* Valida consultas del programador, incluido el estado de registro de clientes específicos, sus dominios y sus recursos/canales.
* Proporciona los datos que crean la lista de operadores de TV paga desde la que el usuario selecciona a su proveedor. Esta lista también se valida y define como adecuada para el programador desde el que se origina la solicitud.
* Inicia flujos de trabajo de autenticación y autorización específicos del operador de TV de pago.
* Almacena en caché las respuestas de autorización correctas por recurso/canal del programador para minimizar el tráfico de solicitudes innecesario.
* Se puede configurar para flujos de trabajo predefinidos específicos de cada operador de TV de pago, como el registro explícito del dispositivo.

Según el sitio web o la aplicación de reproducción, Access Enabler puede adoptar los siguientes formatos:

* Archivo de SWF que puede ejecutar el tiempo de ejecución de la Flash Player
* Un archivo JS ejecutado directamente por el explorador
* Un habilitador de acceso nativo para plataformas compatibles (incluidas iOS, Android y Windows 8)

#### API sin cliente {#clientless-api}

El método de la API sin cliente se aplica a los &quot;dispositivos inteligentes&quot; (consolas de juegos, descodificadores y televisores inteligentes) que no admiten exploradores web (necesarios para la autenticación con MVPD).  En el enfoque sin cliente, las aplicaciones de dispositivos inteligentes se comunican directamente con la autenticación de Adobe Pass a través de las API de servicios web RESTful para todo, excepto para la autenticación, que se realiza en una aplicación de segunda pantalla (explorador). En otras palabras, no se utiliza la biblioteca del lado del cliente de Access Enabler. En su lugar, los desarrolladores de aplicaciones para dispositivos inteligentes consumen directamente las API de servicios web de autenticación de Adobe Pass para implementar los flujos de derechos.

### Servidores back-end alojados en Adobe {#adobe-backend-servers}

Los servidores back-end de autenticación de Adobe Pass, alojados por Adobe:

* Aprovisionar los flujos de trabajo de autenticación y autorización con los proveedores de TV de pago que requieren una comunicación servidor a servidor entre la autenticación de Adobe Pass y el operador.
* Mantenga la configuración de los sitios y las aplicaciones de Programmer.
* Aloje los archivos de componente descargables del Habilitador de acceso.
* Proporcione los puntos finales del servicio web RESTful para la integración de API sin cliente.
* Generar (y, en algunos casos, almacenar) tokens de autenticación y autorización.

### Tokens y el Media Token Verifier {#tokens-media-token-verifier}

La solución del derecho de autenticación de Adobe Pass se centra en la generación de datos específicos que se obtienen al finalizar correctamente los flujos de trabajo de autenticación/autorización. Estos fragmentos de datos se denominan tokens. Tienen una duración limitada y se almacenan de forma segura, ya sea en ubicaciones dependientes de la plataforma en el cliente o en servidores de Adobe en el caso de la solución API sin cliente. Una vez caducados, los tokens deben volver a emitirse reiniciando los flujos de trabajo de autenticación o autorización.

Existen tres tipos de tokens que producen problemas de autenticación de Adobe Pass durante los flujos de trabajo de autenticación/autorización. Dos son &quot;de larga duración&quot;, lo que proporciona continuidad en la experiencia de visualización del usuario. El tercero, un token de corta duración, proporciona soporte para las prácticas recomendadas del sector para mitigar el fraude (donde el fraude incluye vulnerabilidades como la extracción de flujos, por ejemplo). Los valores de tiempo de vida (&quot;TTL&quot;) se establecen en función de los acuerdos entre los programadores y los proveedores de TV de pago, que acuerdan un valor que mejor sirva a todos los involucrados.

#### Token De Autenticación (De Larga Duración) {#long-lived-auth-token}

El éxito de la autenticación se produce una vez que un cliente utiliza la autenticación de Adobe Pass para iniciar sesión correctamente en su cuenta de Pay TV. A continuación, la autenticación de Adobe Pass produce un token de autenticación de larga duración (AuthN) vinculado al dispositivo solicitante y (según el proveedor de TV de pago) un identificador único global (&quot;GUID&quot;) que identifica de forma anónima al usuario.

* La autenticación de Adobe Pass almacena el token de AuthN de forma segura en una ubicación donde está disponible para todas las aplicaciones que utilizan la autenticación de Adobe Pass. Para las integraciones de Access Enabler, los tokens se almacenan de forma segura en el lado del cliente.  La autenticación de Adobe Pass utiliza el token de AuthN para realizar consultas de autorización posteriores en nombre del usuario.
* En un momento determinado solo se almacena un token de AuthN. Siempre que se emite un nuevo token de AuthN y ya existe uno antiguo, el nuevo token sobrescribe el valor almacenado existente.

#### Token De Autorización (De Larga Duración) {#long-lived-authriz-token}

Una vez realizada la autorización correctamente, la autenticación de Adobe Pass crea un token de autorización de larga duración (&quot;AuthZ&quot;). Este token no es portátil, ya que está vinculado al dispositivo solicitante y a un recurso protegido específico (por ejemplo, un canal, una serie o un episodio).

* La autenticación de Adobe Pass almacena el token de AuthZ de forma segura, junto con otros tokens de autorización para otros recursos.  De nuevo, como con los tokens de AuthN, en las plataformas que utilizan el Habilitador de acceso el token se almacena localmente en el cliente; en las plataformas que utilizan la API sin cliente, los tokens se almacenan en los servidores de autenticación de Adobe Pass.
* El tiempo de vida (TTL) del token de AuthZ de larga duración se define generalmente en el intervalo de días a semanas, según el acuerdo específico entre el proveedor de TV de pago y el programador.
* En cualquier momento dado, solo se almacena un token de AuthZ por recurso. Puede haber varios tokens de autorización almacenados, siempre y cuando estén asociados a distintos recursos. Siempre que se emite un nuevo token de autorización y ya existe uno antiguo para el mismo recurso, el nuevo token sobrescribe el valor almacenado en caché existente.
* La autenticación de Adobe Pass utiliza el token de AuthZ de larga duración para crear los tokens de medios de corta duración que se utilizan para el acceso de visualización real.

#### Token de medios de corta duración {#short-lived-media-token}

Una vez que Adobe Pass Authentication genera el token de AuthZ, lo utiliza para generar un token de medios de un solo uso y de corta duración que firma el Adobe y se cifra para evitar su alteración durante el intercambio:

* El TTL del token de corta duración (predeterminado: 5 minutos) está establecido para permitir problemas de sincronización de reloj entre el servidor que genera el token y el servidor que lo valida.
* El token de corta duración se expone al sitio de incrustación antes de proporcionar acceso al recurso protegido, por lo que el programador debe validar el token mediante el verificador de tokens de medios para integraciones del Habilitador de acceso o el servicio de verificador de tokens en el caso de integraciones de API sin cliente.

#### Media Token Verifier {#media-token-verifier}

Los programadores son responsables de integrar la Biblioteca de Comprobador de tokens de medios en su servidor de aplicaciones existente, de modo que el Verificador pueda realizar las validaciones finales del usuario antes de que se inicie realmente un flujo de vídeo. La biblioteca de Media Token Verifier define lo siguiente:

* Una API de verificación de tokens que recupera información del token, como si es válido, la hora en que se emitió y otros datos relevantes
* La clave pública de Adobe utilizada para comprobar que el token realmente proviene del Adobe
* Implementación de referencia que muestra cómo utilizar la API de verificador y cómo utilizar la clave pública de Adobe contenida en la biblioteca para comprobar su origen

![](../assets/high-level-architecture-nflows.png)

*Figura 2: Arquitectura de alto nivel del ecosistema de autenticación de Adobe Pass en una integración de Access Enabler*

## Integración con la autenticación de Adobe Pass {#integrate-auth}

Tanto si es un proveedor de TV de pago como si es un programador, el proceso de integración con la autenticación de Adobe Pass requiere cierta cantidad de su participación activa. A continuación se describe cada uno de estos procesos.

### El proceso del proveedor de TV de pago

La responsabilidad principal del proveedor de TV de pago con la autenticación de Adobe Pass es validar que un usuario solicitante es de hecho un suscriptor conocido que tiene derecho a acceder al contenido del programador. A un nivel alto
En este nivel, el proceso de autenticación de Adobe Pass para la integración con un nuevo proveedor de TV de pago requiere los siguientes pasos:

1. El proveedor firma el Acuerdo de no divulgación de autenticación de Adobe Pass (NDA).
1. El proveedor proporciona al Adobe las especificaciones de su sistema de autenticación y autorización. SOAP Para la integración más sencilla, se recomienda que los operadores de TV de pago cuenten con un proveedor de identidad (IdP) basado en SAML para la autenticación y la capacidad de comunicarse a través del protocolo de acceso a la para la autorización.
1. El proveedor establece la conectividad entre sus servidores y los servidores de autenticación de Adobe Pass. Esto incluye el suministro de puntos de conexión y la lista de direcciones IP.
1. Versión de precalificación y QE.
1. Versión de producción y QE.

Aunque la autenticación de Adobe Pass puede reemplazar las integraciones existentes para programadores, esto generalmente no es necesario para proveedores de TV de pago. Adobe trabaja con el equipo técnico del proveedor para configurar la autenticación de Adobe Pass de modo que satisfaga las necesidades de cualquier integración existente. La integración es gratuita para los proveedores de TV de pago, suponiendo una integración &quot;estándar&quot; y unos requisitos mínimos de soporte (documentación y asistencia básica por correo electrónico). Si un proveedor requiere soporte significativo o una escala de tiempo, se puede cobrar una tarifa de soporte o el proveedor puede querer trabajar con un tercero familiarizado con nuestra solución como Synacor.


La autenticación de Adobe Pass también admite la administración eficiente de la lógica empresarial del proveedor de TV de pago, como se indica a continuación:

* En el caso de la lógica empresarial independiente que el operador puede aplicar cuando recibe una solicitud de autorización, Adobe proporciona los datos necesarios para respaldar la aplicación de la lógica empresarial cuando el operador recibe una solicitud de autorización. Estos datos pueden incluir, entre otros, el ID único de dispositivo del usuario que realiza la solicitud y la dirección IP del dispositivo.
* Para la lógica empresarial que requiere la intervención del usuario o la gestión específica por parte de la solución de Adobe, el Adobe puede mantener algunas propiedades personalizadas para cada proveedor de TV de pago. Estas configuraciones/políticas específicas del operador incluyen la activación de flujos de trabajo predefinidos que se pueden iniciar en puntos específicos del flujo de trabajo de nivel superior. Para obtener más información sobre la compatibilidad con propiedades personalizadas, póngase en contacto con el representante del Adobe.

El Adobe también ofrece servicios de limitación de fraudes. Póngase en contacto con el representante del Adobe para obtener más información.

### El proceso del programador {#programmer-process}

Para integrar correctamente la autenticación de Adobe Pass, los programadores deben configurar su aplicación de reproductor de medios o página web para trabajar con la autenticación de Adobe Pass en la administración de los procesos principales de asignación de derechos: autenticación, autorización y cierre de sesión.


Antes de comenzar una integración con la autenticación de Adobe Pass, los programadores deben tener:

* Una plataforma de vídeo en línea existente, que incluye un reproductor de contenido, ya sea como parte de un sitio web o como aplicación independiente
* Un sistema de gestión de contenidos
* Un mecanismo de envío que puede incluir o no una red de envío de contenido de terceros (CDN)

Los programadores deben realizar algunas tareas de integración como parte del suministro de servicios de TV Everywhere con autenticación de Adobe Pass. Estas tareas incluyen:

* Integración de la biblioteca Access Enabler de autenticación de Adobe Pass en su página web o reproductor de medios, o implementación de la integración con el enfoque sin cliente para &quot;dispositivos inteligentes&quot; que no son compatibles con la web
* Trabajo del lado del servidor para integrar el componente Verificador de tokens de autenticación de Adobe Pass en su flujo de trabajo de flujo de vídeo
* Creación de una interfaz de usuario para el flujo de trabajo de acceso en el sitio web o la aplicación (algunos elementos de este flujo de trabajo, como el proceso de inicio de sesión real, los proporciona el operador de Pay TV y algunos elementos están disponibles de forma opcional como parte de la autenticación de Adobe Pass)

En este documento se ofrece una visión general del proceso del Programador y el Adobe proporciona directrices adicionales sobre el inicio formal de la integración.

#### Configuración del solicitante (programador) {#requester-prog-setup}

##### Registro en el Adobe {#registering}

Como primer paso, los programadores deben registrarse con Adobe o un socio autorizado por Adobe y especificar los dominios que desean utilizar con la autenticación de Adobe Pass. A continuación, los programadores reciben un ID de solicitante único, que se proporciona a la autenticación de Adobe Pass para cada sesión en la que el programador interactúa con el activador de acceso.

##### Configuración De La Integración Del Habilitador De Acceso Inicial {#access-enabler-int-setup}

Antes de que los clientes soliciten acceso al contenido, los programadores deben integrar el componente cliente Autenticación de Adobe Pass, el Habilitador de acceso, en su aplicación de reproductor de medios o página web existente. Hay varias opciones para hacer esto:

* Puede incrustar la versión de Flash, AccessEnabler.swf, en un reproductor de vídeo basado en Flash en una página web o directamente en HTML. Puede comunicarse con el SWF en ActionScript o JavaScript. La API base es de ActionScript, pero hay disponible una biblioteca completa de envoltorios de JavaScript.
* En los dispositivos que no son de Flash, puede:
   * Utilice la versión de HTML5/JavaScript, AccessEnabler.js, y comunique con él a través de la API de JavaScript, o
   * Utilice una biblioteca nativa de Access Enabler, como para iOS, Android o Windows 8

##### Configuración de la integración inicial de API sin cliente {#clientless-api-int-setup}

Antes de que los clientes soliciten acceso al contenido, los programadores deben implementar las llamadas de servicios web RESTful mediante la API sin cliente en su aplicación de reproductor de contenidos, así como configurar una aplicación de &quot;segunda pantalla&quot; para gestionar el inicio de sesión del usuario en su proveedor de TV por pago a través de la web.

#### Administrar autenticación y autorización {#auth-authr-handling}

Cuando un cliente solicita un recurso protegido de un programador por primera vez, el programador presenta al cliente una lista de proveedores de TV de pago entre los que elegir. Cuando se selecciona el proveedor, se redirige al usuario a ese operador para la autenticación inicial del usuario. Una vez que la autenticación se realiza correctamente, la autenticación de Adobe Pass se comunica con el proveedor de TV de pago seleccionado para autorizar el acceso al recurso especificado. A continuación se ofrecen detalles sobre estos procesos.

![](../assets/providr-selection-ui.png)


*Figura 3: Ejemplo de IU de selección de proveedores*

>[!NOTE]
>
>* La autenticación se produce como un intercambio SAML entre la autenticación de Adobe Pass como proveedor de servicios (o &quot;SP&quot;) y un proveedor de TV de pago como proveedor de identidad (o &quot;IdP&quot;).
>* La autorización utiliza un intercambio de servicio web de canal de retorno (servidor a servidor) entre la autenticación de Adobe Pass (el SP) y un proveedor de TV de pago (el IdP).


##### Comunicación del programador con el activador de acceso

El canal de comunicación bidireccional entre el activador de acceso y la página web o la aplicación del reproductor del programador sigue un patrón totalmente asincrónico. El programador envía mensajes al activador de acceso a través de los métodos expuestos por la API del activador de acceso. El activador de acceso responde mediante llamadas de retorno registradas con la biblioteca del activador de acceso.

* Cualquier solicitud de autorización solicita automáticamente primero la autenticación si no se encuentra un token de autenticación en el sistema local. Cuando la autenticación se realiza correctamente, el token del cliente se almacena localmente, de modo que no es necesario que vuelva a iniciar sesión durante un período de tiempo determinado. Si se ha autenticado correctamente a través de la solución de asignación de derechos de autenticación de Adobe Pass en cualquier otro contexto (por ejemplo, a través del sitio web del proveedor de TV paga o un programador diferente), el Access Enabler tiene acceso al token local y no requiere que se realice una autenticación adicional.
* Cuando un cliente solicita un recurso específico, el Programador solicita la autorización del proveedor de TV de pago a través del Habilitador de acceso. Después de verificar (o iniciar) la autenticación, el activador de acceso se pone en contacto con el proveedor de TV de pago (a través de la autenticación de Adobe Pass) para determinar si el cliente tiene derecho a ver el recurso. La autenticación de Adobe Pass gestiona la comunicación con el proveedor de TV de pago para obtener autorización. El programador solo necesita enviar la solicitud al activador de acceso y administrar la respuesta (éxito o fracaso de la autorización). Si la autorización se realiza correctamente, se almacena un token de autorización en el sistema cliente y la llamada de retorno recibe un token de medios de corta duración.

##### Comunicación del programador mediante la API sin cliente {#progr-comm-clientless-api}

La comunicación entre la aplicación del programador y la autenticación de Adobe Pass se realiza mediante los servicios web RESTful.  Existen protocolos de seguridad para todas las llamadas de API a los extremos de autenticación de Adobe Pass.  Los requisitos de seguridad se describen en la documentación de API sin cliente.

##### Flujo de trabajo de ejemplo con autenticación basada en SSO del explorador web SAML {#sample-wf}

1. El visor navega a un sitio (dummy1.com) e intenta acceder al contenido con derechos.
1. La página o el reproductor de vídeo carga el Access Enabler desde adobe.com y, cuando la acción del usuario lo solicita, solicita autorización para el contenido solicitado.
1. Access Enabler ejecuta y valida el solicitante y la solicitud.
1. Access Enabler comprueba si hay un token de autorización válido en el almacén local. Si se encuentra una autorización válida, el Habilitador de acceso produce un token multimedia de corta duración (consulte el paso 14).
1. Si no se encuentra ninguna autorización válida para el recurso solicitado pero existe un token de autenticación válido, el Habilitador de acceso inicia una solicitud de autorización con el proveedor de TV de pago con el que se autentica al usuario. El servidor de Adobe proporciona el intercambio de solicitud/respuesta de autorización con el proveedor de TV de pago.
1. Si no se encuentra ningún token de autenticación válido, el Habilitador de acceso pregunta al usuario cuál es su proveedor de TV de pago. (Al seleccionar un proveedor que admita la autenticación basada en SSO del explorador web de SAML, se crea un déclencheur de trabajo de autenticación basada en SAML. Para los proveedores que no son de SAML, el Adobe gestiona un flujo de trabajo personalizado similar).
1. Access Enabler navega el explorador al servicio SAML SP (Proveedor de servicios) de Adobe, pasando todos los parámetros adecuados.
1. El SP de SAML invoca el IdP de SAML (proveedor de identidad) apropiado en el proveedor de TV de pago del usuario, utilizando el perfil del explorador web de SAML como se indica en los metadatos de IdP. Esto efectivamente lleva al usuario al sitio del IdP (proveedor de TV de pago), donde el usuario se autentica.
1. Después de la autenticación correcta, el usuario se redirige de nuevo al SP de SAML de Adobe, pasándole un GUID de autenticación en la respuesta de SAML.
1. El SP de SAML de Adobe crea una sesión en el servidor donde se almacena el GUID de autenticación y redirige al usuario de nuevo a la página del programador original. (La sesión del servidor se elimina al recuperar Access Enabler del token authN).
1. Access Enabler recupera el GUID de autenticación del servidor del Adobe para incluirlo en el token con un ID de dispositivo mantenido por la autenticación de Adobe Pass. Cuando DRM de Flash está en el dispositivo, esto se realiza mediante API de Flash Access (componente DRM de Flash Player) que habilitan el enlace del GUID al ID del dispositivo y devuelven un token de autenticación. De lo contrario, esto se realiza a través de las API de JS a través de HTTPS mediante almacenamiento basado en HTML5 o a través de componentes nativos específicos.
1. Access Enabler utiliza el token de autenticación para realizar solicitudes de autorización al proveedor de TV de pago. En dispositivos con Flash Access habilitado, las solicitudes siempre se realizan mediante API de Flash Access para que el token de autorización resultante esté enlazado al dispositivo. En dispositivos que no son de Flash Access, se utiliza HTTPS para la comunicación segura entre el cliente y el servidor.
1. Una vez realizada la autorización correctamente, la autenticación de Adobe Pass crea un token de autorización de larga duración (&quot;authZ&quot;) y lo pasa al activador de acceso, que lo almacena en el sistema local.
1. Access Enabler utiliza el token authZ para crear tokens de medios de corta duración que se utilizan para el acceso de visualización real. Por motivos de seguridad, estos tokens de corta duración deben ser validados por otro componente de autenticación de Adobe Pass, el Media Token Verifier.

![](../assets/authn-authz-entitlmnt-flow.png)


*Figura 4: Flujo de trabajo del Habilitador de acceso para autenticación y autorización*

##### Proporción de una interfaz de usuario de derechos {#entitlement-ui}

Los programadores deben crear su propia interfaz de usuario para el flujo de trabajo de acceso a su sitio web o aplicación. Algunos elementos, como el proceso de inicio de sesión real, los proporciona el proveedor de TV de pago y algunos elementos están disponibles de forma opcional como parte de la autenticación de Adobe Pass. Como mínimo, el programador hace lo siguiente:

* **Implementa una interfaz de selección de proveedores** que permite a un nuevo usuario identificar su proveedor de TV de pago e iniciar sesión por primera vez. Para el desarrollo, Access Enabler proporciona una interfaz de usuario básica que ofrece al cliente la opción de elegir proveedores de TV de pago e inicia el proceso de inicio de sesión. Para un entorno de producción, los programadores deben implementar su propio cuadro de diálogo de selector de proveedores. Algunos proveedores de TV de pago redirigen a su propio sitio para el inicio de sesión y algunos requieren que sus páginas de inicio de sesión se muestren dentro de un iframe. Los programadores deben implementar la llamada de retorno que crea este iframe, en caso de que el cliente elija uno de esos proveedores.
* **Identifica recursos protegidos.** Los recursos protegidos son los que requieren autorización para tener acceso. Al ofrecer estos recursos, la interfaz de programador debe indicar la necesidad de autorización antes de la visualización. Una vez realizada la autorización correctamente, la interfaz debe mostrar que el recurso ya está autorizado.
* **Crea y mantiene una lista de proveedores de TV de pago** para controlar el acceso de los usuarios únicamente a los proveedores especificados.
* **Muestra que un usuario está autenticado.** El programador debe indicar el estado de autenticación del cliente como parte de cualquier medio utilizado para identificar los recursos protegidos. Los programadores pueden consultar el Habilitador de acceso para determinar si un cliente ya se ha autenticado.

#### Compatible con cierre de sesión único {#single-logout-support}

En la mayoría de los casos, el programador es responsable de administrar los cierres de sesión de los usuarios mediante una simple llamada de API. La llamada logout() indica a la autenticación de Adobe Pass que cierre la sesión del usuario actual mediante:

* Eliminación de todos los tokens de AuthN y AuthZ
* Borrando toda la información de autenticación y autorización de ese usuario
* Inicio de un flujo de trabajo específico del proveedor de TV paga para borrar la sesión de autenticación del usuario con el proveedor (por ejemplo, si la autenticación se realizó utilizando el protocolo de solicitud de autenticación SAML, el cierre de sesión se puede realizar utilizando el protocolo de cierre de sesión único SAML).

Si el usuario deja el equipo inactivo durante el tiempo suficiente para que los tokens caduquen, aún puede volver a su sesión e iniciar el cierre de sesión correctamente. La autenticación de Adobe Pass garantiza que todos los tokens se eliminen y notifica al proveedor de TV de pago que también elimine su sesión.


Cuando el cierre de sesión se inicia desde un sitio que no está integrado con la autenticación de Adobe Pass, el proveedor de TV de pago puede invocar el servicio de cierre de sesión único de autenticación de Adobe Pass mediante una redirección del explorador.

## Más allá de los flujos de derechos básicos: funciones adicionales {#beyond-basics}

Los flujos de derechos básicos son Inicio, Autenticación, Autorización y Cierre de sesión.  A medida que la autenticación de Adobe Pass madura y se desarrolla, se han agregado y se están agregando una serie de funciones adicionales a los flujos básicos.  Estos incluyen:

* **Metadatos de usuario**: según los acuerdos entre MVPD y Programadores, las MVPD pueden intercambiar metadatos de forma segura, como código postal, clasificación máxima, ID de canal y mucho más. Los metadatos permiten varios casos de uso, incluidos controles parentales, períodos de congelación regional para eventos deportivos, etc.
* **Acceso gratuito temporal**: permite a los programadores ofrecer acceso gratuito temporal a su contenido protegido (por ejemplo, ejemplos cortos de programación diaria o presentación gratuita de un evento de gran tamaño).
* **MVPD proxy**: una MVPD puede administrar su propia integración con la autenticación de Adobe Pass y también administrar el proceso de asignación de derechos en nombre de un grupo de &quot;ProxiedMVPD&quot; asociados.

## Seguridad {#security}

Esta sección resalta la seguridad y la integridad de la infraestructura de autenticación de Adobe Pass.

### Seguridad de token {#token-security}

Uno de los objetivos principales de la autenticación de Adobe Pass es garantizar que el sistema pueda resistir los ataques a los datos de asignación de contenido por parte de un usuario o agregador de contenido no fiable. Por lo tanto, el acceso a los datos está protegido en diferentes niveles del flujo de trabajo, con la protección de la generación y el uso de los datos del token de autorización que tienen la mayor importancia. La arquitectura de autenticación de Adobe Pass está diseñada para garantizar que el contenido del token se mantenga de forma segura y que el token permanezca en el dispositivo al que se emitió.

* **Seguridad de token AuthN y AuthZ de larga duración**: todos los tokens de larga duración están firmados digitalmente por el servidor de autenticación de Adobe Pass. Sin embargo, la firma digital difiere de una plataforma a otra, ya que utiliza un ID de dispositivo que difiere en la forma en que se genera, protege y valida. En todos los casos, una validación del lado del cliente garantiza que la firma digital esté intacta y que se preserve la integridad del token. Access Enabler almacena de forma segura los tokens validados en ubicaciones específicas del entorno en el que se está ejecutando. Si la validación del ID del dispositivo falla, la sesión de autenticación se invalida, los tokens se restablecen y se solicita al usuario que vuelva a iniciar sesión.
* **Seguridad de token de medios de corta duración**: los tokens de medios de corta duración, que se producen en el último paso antes del acceso al contenido, están firmados por el Adobe y cifrados para evitar su alteración durante el intercambio. Los tokens de medios de corta duración también requieren un paso de validación adicional a través de un componente de autenticación de Adobe Pass adicional, el verificador de tokens de medios. El TTL del token de corta duración está establecido en un valor predeterminado de 5 minutos y se puede acortar, si lo desea. El token de medios de corta duración nunca se almacena en caché; se recupera un nuevo token del servidor cada vez que se llama a una API de autorización.

### Seguridad de dispositivos específica de la plataforma {#platform-sp-security}

Las medidas de seguridad que utiliza la autenticación de Adobe Pass varían según la plataforma, pero todas son sólidas y vanguardistas.

* **dispositivos habilitados para Flash**: cuando Flash Player 10.1+ o AIR 2.5+ se encuentra en el dispositivo, la autenticación de Adobe Pass utiliza la funcionalidad DRM de Flash Player para la protección, también conocida como Flash Access. El Flash proporciona un nivel adicional de protección; la sólida garantía de enlace de dispositivos para tokens basados en el Flash significa que, en la mayoría de los casos, el tiempo de vida puede ser mayor, el usuario no tiene que iniciar sesión con tanta frecuencia y la experiencia del usuario es generalmente más fluida.
* **Experiencias en el navegador en dispositivos compatibles con HTML5**- En dispositivos que no son de Flash y que incluyen capacidad de navegador HTML5, la autenticación de Adobe Pass tiene un medio alternativo de protección limitada para integraciones basadas en el navegador. Sin embargo, dado que el enlace de dispositivo para HTML5 no es tan fuerte, el tiempo de vida (TTL) de los tokens en plataformas HTML5 suele ser más corto.
* **Compatibilidad con aplicaciones nativas para dispositivos internos y externos**: Adobe ofrece SDK nativos por sistema operativo (iOS, Android, Windows 8, etc.) que proporcionan una seguridad mejorada con respecto a la solución HTML5. Estos SDK utilizan API nativas para recuperar un ID de dispositivo y pasarlo de forma segura al servidor de autenticación de Adobe Pass.
* **Sin cliente**: la autenticación de Adobe Pass usa el protocolo HTTPS para la comunicación segura. Además, todas las llamadas de un dispositivo inteligente deben estar firmadas digitalmente.

## FAQ {#faqs}

**¿Qué es la TV en todas partes?**
El movimiento de la industria conocido como TV en Todas Partes permite a los clientes de televisión de pago acceder al contenido premium al que ya están suscritos en una variedad de dispositivos conectados a Internet, incluyendo computadoras personales, tabletas, smartphones, consolas de juegos, decodificadores y televisores &quot;inteligentes&quot;. El desafío de esta iniciativa es hacer que el proceso de autenticación sea lo más sencillo y sencillo posible, lo que permite a los clientes acceder sin problemas al contenido de su suscripción sin barreras prohibitivas y varios inicios de sesión.


**¿Qué es la autenticación de Adobe Pass y cómo se relaciona con TV en todas partes?**
La autenticación de Adobe Pass lleva a la TV en todas partes del concepto a la realidad, al verificar sin problemas el derecho de un usuario al contenido de una manera que sea simple y segura. La autenticación de Adobe Pass es un servicio alojado que permite una rápida integración del back-end en función de las reglas comerciales requeridas por los programadores y los proveedores de TV de pago. Esto significa un rápido momento para comercializar para todas las partes, un entorno más seguro para evitar el fraude y una experiencia de cliente superior, con más contenido de TV disponible para más personas en más plataformas.


**¿Cómo se ofrece/entrega la autenticación de Adobe Pass?**
La autenticación de Adobe Pass se ofrece mediante el modelo Software as a Service (SaaS). Esto permite que se produzca una comunicación más segura entre los usuarios finales, los programadores y los proveedores de TV de pago para validar el derecho al contenido. Los componentes principales del servicio incluyen el activador de acceso del lado del cliente (o la API sin cliente para algunos dispositivos) y el servidor de autenticación de Adobe Pass alojado. Access Enabler es un pequeño archivo que se carga en la página web o en la aplicación de reproducción de un programador. Se comunica con los servidores de autenticación de Adobe Pass, que a su vez tienen conexiones integradas en los sistemas de autenticación de varios proveedores de TV de pago. La autenticación de Adobe Pass también ofrece un enfoque de API sin cliente para la integración de algunos &quot;dispositivos inteligentes&quot; que no son compatibles con la web (televisores inteligentes, descodificadores, consolas de juegos, etc.). El método sin cliente proporciona servicios web RESTful con los que los desarrolladores pueden implementar los flujos de derechos de autenticación de Adobe Pass en estos dispositivos.


**¿En qué se diferencia la autenticación de Adobe Pass de otras soluciones de TV en todas partes?**
La autenticación de Adobe Pass tiene claras ventajas con respecto a las soluciones alternativas de TV en todas partes. Las integraciones directas con proveedores individuales no proporcionan la flexibilidad de un inicio de sesión único y persistente (SSO) a medida que los usuarios viajan de un sitio a otro a través de Internet. La autenticación de Adobe Pass también tiene una notable penetración en el mercado; una vez que un programador se integra con la autenticación de Adobe Pass, se conecta inmediatamente con los operadores de TV de pago que atienden a más del 90% de los hogares en los Estados Unidos. Además, la autenticación de Adobe Pass aprovecha las funciones de seguridad únicas integradas en el tiempo de ejecución de Flash (cuando están disponibles) para ayudar a mitigar el fraude, a la vez que proporciona SDK para que los programadores puedan tener la misma funcionalidad de TV en todas partes integrada en las aplicaciones nativas para dispositivos móviles o domésticos en los que el Flash no está disponible. Por último, aunque la autenticación de Adobe Pass está disponible como servicio independiente, también ofrecemos la opción de integrarla estrechamente con otros productos y servicios de Adobe (incluidos Adobe Pass y Adobe Analytics) relacionados con la entrega, la protección y la monetización de contenido de TV Everywhere.

**¿Cuán segura es la autenticación de Adobe Pass?**
La prioridad número uno de la arquitectura de autenticación de Adobe Pass es garantizar que solo los visualizadores autorizados estén autenticados y tengan acceso al contenido premium. La autenticación de Adobe Pass vincula estrechamente el acceso al dispositivo de visualización y puede ayudar a limitar los flujos, las sesiones o los dispositivos para un hogar determinado.


**¿Se requiere Flash Player?**
Se requiere el Flash Player de Adobe 11.x o posterior para la seguridad de enlace de dispositivos más estricta. Sin embargo, la autenticación de Adobe Pass para TV en todas partes no depende del reproductor ni de la plataforma, y se integra con cualquier aplicación de reproducción, incluidas Silverlight y HTML5. Además, la autenticación de Adobe Pass proporciona compatibilidad nativa con dispositivos como iOS, Android y Xbox donde el Flash Player no está disponible.  Por último, la autenticación de Adobe Pass proporciona un enfoque sin clientes para los dispositivos que no pueden procesar páginas web (consolas de juegos, televisores inteligentes, descodificadores).


**¿Qué dispositivos es compatible con la autenticación de Adobe Pass?**
La autenticación de Adobe Pass es compatible con prácticamente cualquier dispositivo con el kit web de HTML5 para experiencias de visualización en el explorador. Además, la autenticación de Adobe Pass sigue implementando kits de desarrollo de software (SDK) nativos para varias plataformas específicas de dispositivos, incluidas iOS, Android™ y Windows 8. La autenticación de Adobe Pass admite parcialmente algunos dispositivos no compatibles con la web (televisores inteligentes, descodificadores, consolas de juegos, etc.) a través de sus API de servicios web RESTful.

**¿La autenticación de Adobe Pass es compatible con los estándares emergentes para TV en todas partes?**
La autenticación de Adobe Pass es compatible con la **especificación CableLabs OLCA (acceso de contenido en línea)** [](https://www.cablelabs.com/specifications), que proporciona requisitos técnicos y arquitectura para la entrega de vídeo a un cliente de TV de pago desde fuentes en línea. En junio de 2011, el Adobe participó en el proyecto conjunto de pruebas de interopt de CableLabs y aprobó el proceso de prueba para la implementación de un proveedor de servicios. La autenticación de Adobe Pass se verifica (completa y prueba) según las especificaciones OLCA para la autenticación. El componente de autorización se ha completado, pero la verificación de las pruebas espera el lanzamiento del entorno de prueba de CableLabs (ETA, noviembre de 2011).

El Adobe también es miembro activo del **Consorcio Técnico de Autenticación Abierta (OATC)** y participa en varios de los proyectos de redacción de especificaciones de los subcomités como parte de ese organismo.

**¿Cómo gestiona la autenticación de Adobe Pass la administración de identidades federadas/el inicio de sesión único (SSO)?**
La autenticación de Adobe Pass le permite proporcionar a los clientes autenticación y autorización de inicio de sesión único (SSO), mediante la comunicación de canal de retorno (servidor a servidor) entre la autenticación de Adobe Pass y los operadores de TV de pago participantes. Por lo tanto, con la autenticación de Adobe Pass, no es necesario que los suscriptores vuelvan a iniciar sesión después de su primera autenticación, siempre y cuando la autenticación esté permitida por el operador de TV de pago para persistir. Normalmente, este límite se establece en 30 días. Para ello, la autenticación de Adobe Pass proporciona un dominio común para los tokens de autenticación de nuestros clientes. Esta información de estado de autenticación está disponible para todos los sitios participantes integrados con un operador de TV paga determinado.

Actualmente, la mayoría de las integraciones de autenticación de Adobe Pass con operadores de TV paga utilizan el protocolo SAML, uno de los estándares de autenticación principales. La autenticación de Adobe Pass actúa como un proveedor de servicios proxy en la arquitectura de SAML y mantiene la respuesta de autenticación de SAML como un token seguro en el dominio común de Adobe. La autenticación de Adobe Pass es compatible con SAML 2.0.

Aunque la autenticación de Adobe Pass se utiliza generalmente con soluciones de SSO de SAML en este punto, la arquitectura de autenticación de Adobe Pass abstrae cualquier protocolo específico de la integración del programador. Por lo tanto, con el tiempo se puede agregar compatibilidad con nuevos protocolos, como uno basado en OAuth 2.0 o protocolos personalizados.

**¿Cuánto cuesta la autenticación de Adobe Pass para TV en todas partes para los usuarios finales?**
El uso de la autenticación de Adobe Pass no supone ningún coste adicional para los usuarios finales.

>[!NOTE]
>
>**Pasos siguientes:** Para obtener más información, comuníquese con el representante del Adobe o complete el formulario de solicitud de información [aquí](https://www.adobe.com/cfusion/mmform/index.cfm?name=adobepass_rfi).
>
