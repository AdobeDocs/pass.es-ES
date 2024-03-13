---
title: Información general de API REST
description: Información general sobre las API de REST
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '1612'
ht-degree: 0%

---

# Información general de API REST {#rest-api-overview}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


## Información general {#over}

La API de REST de autenticación de Adobe Pass proporciona acceso directo a los servicios de autenticación y autorización de TV Everywhere (TVE). Esta API admite dos arquitecturas principales: servidor a servidor o dispositivos conectados (por ejemplo, consolas de juegos, televisores inteligentes, decodificadores, etc.) aplicaciones que no tienen capacidades de navegación web.

### Mecanismo de limitación

La API de REST de autenticación de Adobe Pass se rige por un [Mecanismo de limitación](/help/authentication/throttling-mechanism.md).


### De servidor a servidor

Las soluciones de servidor a servidor implican aplicaciones cliente de Programmer que se integran con los servicios de Programmer que se conectan con los servicios de autenticación de Adobe Pass para flujos de TVE. Este enfoque traslada la mayor parte de la implementación de TVE del cliente al servidor, donde se puede crear y mantener un módulo de autorización único y unificado. La principal responsabilidad restante de las aplicaciones cliente es la administración de una vista web para la autenticación de usuarios.



### Dispositivos conectados

Las aplicaciones de Dispositivos conectados se comunican directamente con la autenticación de Adobe Pass a través de las API de REST para realizar la configuración, el registro, las comprobaciones del estado de autenticación y los flujos de autorización, mientras que se requiere una segunda aplicación de pantalla (explorador) para el flujo de autenticación. Como tal, no se utilizan SDK nativos.



### Otras arquitecturas

Además de las dos arquitecturas principales basadas en la API de REST, las soluciones de cliente de servidor a servidor y las soluciones de cliente directo para dispositivos inteligentes, existen otras arquitecturas.  Principal entre ellos es la arquitectura del SDK, que utiliza un componente de cliente denominado Access Enabler que la autenticación de Adobe Pass proporciona a los programadores.  La aplicación utiliza las API del Habilitador de acceso para gestionar el inicio, la autenticación, la autorización y el cierre de sesión.  Toda la comunicación entre la aplicación del programador y los servidores de autenticación de Adobe Pass se produce a través del Habilitador de acceso.  Hay disponible un tipo diferente de Access Enabler para las siguientes plataformas: JavaScript, iOS, tvOS, Android y FireTV.

Aunque es posible utilizar la API de REST directamente en plataformas cliente que admitan SDK nativos fuera de una solución servidor a servidor, no se recomienda este método.



## Ventajas e inconvenientes de la API de REST {#ProsAndCons}

La API de REST de autenticación de Adobe Pass se creó para proporcionar una solución de TV en todas partes (TVE) para dispositivos que no tienen capacidades de navegación web o almacenamiento persistente. La API de REST es compatible con todos los flujos de autenticación y autorización, pero porque carece de un componente SDK nativo. Los SDK proporcionados y mantenidos por la autenticación de Adobe Pass incluyen funcionalidades integradas que implementan reglas empresariales que, en el caso de la API de REST, los programadores deben implementar y mantener. En la tabla Responsabilidades del programador que aparece a continuación, describimos las limitaciones de la API de REST actual que deben abordar los programadores.



### Ventajas e inconvenientes de servidor a servidor frente a clientes

Una arquitectura de servidor a servidor proporciona una forma de consolidar la mayor parte de la lógica relacionada con la autenticación y la autorización en una sola unidad lógica o implementación.  Este enfoque tiene pros y contras.  Las ventajas incluyen:

* Implementación única para la lógica empresarial de autenticación y autorización.
* Evite la necesidad de implementar esa lógica en cada plataforma admitida mediante las herramientas nativas de esas plataformas.
* La capacidad de actualizar las funcionalidades sin tener que actualizar a los clientes con todos sus requisitos asociados (por ejemplo, actualizaciones de la tienda de aplicaciones).
* **Más fácilmente** ampliar y personalizar las capacidades de authN y authZ (por ejemplo, agregar D2C).
* Gestión directa del tráfico asociado para un mayor control, calidad y monitorización.



De nuevo, los inconvenientes se enumeran en las responsabilidades del Programador, pero incluyen lo siguiente:

* SSO debe implementarse para cada cliente para plataformas sin SSO de Platform.
* Los programadores deben implementar una lógica específica de MVPD si es necesario.
* Todas las plataformas que utilizan la API de REST comparten una sola configuración que rige las propiedades como los TTL de autenticación.



### Dispositivos conectados

Para la mayoría de los dispositivos conectados, la API de REST debe utilizarse de una forma u otra, ya que un SDK no está disponible. El dispositivo conectado utilizará la API de REST directamente o se integrará con una solución de servidor a servidor que utilice la API de REST.

## Responsabilidades del programador {#programmer-responsibilities}

Lo siguiente se aplica tanto a las aplicaciones servidor a servidor como a las de dispositivos conectados.

| **Funcionalidad** | **Responsable de gestionar la funcionalidad** | **Descripción de las limitaciones de la API actual sin cliente y las diferencias con los SDK nativos** |
| --- | --- | --- |
| Ajustes de configuración aplicados por plataforma | Adobe | Uno **limitación importante** Al usar la API de REST en todas las plataformas (incluidos dispositivos móviles como iOS y Android), los ajustes de configuración correspondientes a la API de REST en nuestra herramienta de configuración del TVE Dashboard se aplican a todos los dispositivos (incluso si hay un dispositivo iOS que ejecuta una aplicación nativa implementada sobre nuestra API de REST). Esta limitación **se puede romper** los TTL acordados y los ajustes de plataforma acordados con los MVPD, si estos difieren según cada plataforma. [1](#1) |
| Inicio de sesión único | Programadores | Mediante la API de REST, el SSO solo está disponible en plataformas compatibles con el SSO de Platform (por ejemplo, Apple, Roku, Amazon), mientras que el SSO no se puede garantizar para otras plataformas al utilizar la API de REST. Los SDK almacenan en caché los datos de forma multisitio/aplicación. Esto significa que el usuario inicia sesión una vez en un sitio o aplicación y ya ha iniciado sesión en los sitios participantes, sin que sea necesaria la interacción del usuario. [2](#2) |
| Cierre de sesión único | Programadores | En un escenario de SSO de SDK nativo, al cerrar la sesión desde una aplicación participante, se cerrará la sesión del usuario desde cualquier lugar. En la API de REST actual no se admite el SLO, al cerrar la sesión desde una aplicación, se cerrará la sesión del usuario solo para esa aplicación en particular. |
| Almacenamiento en caché | Programadores | Las implementaciones de la API de REST tendrán que implementar su propio mecanismo de almacenamiento en caché para los elementos de datos acordados por las empresas. Los SDK almacenan automáticamente en caché varios elementos de datos, teniendo en cuenta al mismo tiempo diversas reglas empresariales. Por ejemplo, los metadatos del usuario se almacenan en caché con el mismo TTL que el token de autenticación, mientras que algunos elementos se pueden excluir mediante programación del almacenamiento en caché (comprobaciones). |
| Mecanismo detallado de informes de errores | Programadores | La API de REST se basa principalmente en códigos de error HTTP para informar de errores de aplicaciones, mientras que los SDK tienen un mecanismo de informe de errores detallado que ayuda a los desarrolladores de aplicaciones a comprender mejor lo que está ocurriendo. |
| Recuperación de errores de la aplicación (reintento por error, detección de bucle, etc.) | Programadores | Las implementaciones de API de REST deben crear sus propios sistemas de recuperación de aplicaciones, mientras que las implementaciones de SDK se benefician además del sistema de recuperación de errores de SDK: recuperación de errores de red transitorios, reintentando determinadas llamadas de red con lógica in situ para evitar &quot;bucles&quot;. |
| Autenticación por solicitante | Programadores | Algunas MVPD requieren autenticación para cada sitio/aplicación, ya sea por motivos comerciales o debido a desafíos técnicos. Los SDK lo aplican automáticamente en función de la configuración de TVE Dashboard. Los implementadores de la API de REST deben implementarla ellos mismos, para no violar los acuerdos comerciales o para poder completar la autorización de las MVPD que abarcan sus datos de autenticación por aplicación. |
| Autenticación pasiva | Programadores | Algunas MVPD admiten la autenticación &quot;pasiva&quot;, en la que no se requiere que el usuario introduzca las credenciales, y se intenta autenticar al usuario automáticamente. Esto resulta especialmente útil en las MVPD que también tienen como requisito la &quot;autenticación por solicitante&quot;. En tal caso, la experiencia de usuario habilitada por los SDK es perfecta, donde el usuario se autentica solo una vez en una aplicación y el SDK realiza una autenticación &quot;pasiva&quot; para otras aplicaciones del ecosistema. El usuario no ve las llamadas pasivas adicionales, simplemente ya estará autenticado, mientras que la MVPD logra el objetivo de tener sesiones de autenticación independientes para cada aplicación. |
| Detección de dispositivos implícita y uniforme | Programadores | Los SDK detectan automáticamente el tipo de dispositivo y envían esa información de forma estándar. Las implementaciones de API de REST son propensas a enviar información diferente, según el implementador, lo que distorsiona o rompe las reglas y estadísticas empresariales en los sitios. **Los programadores deben asegurarse de que nos han enviado la información correcta del dispositivo** en cada una de sus aplicaciones. Para implementaciones de servidor a servidor, la API de REST no identifica correctamente la dirección IP del usuario final, a menos que se realicen pasos adicionales. La dirección IP es importante en determinados casos de uso, como la prevención de fraudes o HBA. |
| Prueba de manipulación TempPass | Programadores | La aplicación de TempPass se basa en un ID de dispositivo estable. Los SDK utilizan técnicas de información y huellas digitales de hardware para identificar el dispositivo, y este mecanismo no es público, y por lo tanto es más seguro y libre de colisiones. Para implementaciones de API de REST, los programadores deben implementar sus propios algoritmos para la identificación/huella digital del dispositivo. |
| Enlace de dispositivo | Programadores | Los tokens generados en un dispositivo con una aplicación a través del SDK no son portátiles, lo que dificulta que un usuario malintencionado comparta sus tokens y proporcione acceso a otros usuarios. Esto se basa en el mismo mecanismo de ID de dispositivo que TempPass a prueba de manipulaciones. Para la API de REST, los tokens permanecen en la nube, lo que significa que dos dispositivos con los mismos ID de dispositivo que realizan las mismas llamadas obtendrán las mismas respuestas/acceso. Los programadores deben asegurarse de que tienen un mecanismo robusto, seguro y libre de colisiones para generar/asignar ID de dispositivo. |
| Conjunto de características predecibles y certificadas | Programadores | El conjunto de funciones existente en cada SDK es coherente, predecible y está completamente certificado y probado. Algunos requisitos comerciales se aplican mediante los SDK, lo que hace que sea arriesgado para el programador incumplir los contratos al utilizar la API de REST. Aunque la API de REST puede ser más flexible, el Adobe garantiza que las implementaciones del SDK apliquen las decisiones y configuraciones comerciales desde el panel de TVE. En el caso de los usuarios sin cliente, cada programador implementa su propio subconjunto de reglas empresariales que agrupa sus aplicaciones en un momento determinado. |


1. Como parte de nuestra nueva iniciativa One API, planeamos corregir esta limitación y poder aplicar reglas por plataforma en función de la identificación del dispositivo.

2. Adobe sigue trabajando con todas las plataformas principales para implementar Platform SSO que se puede utilizar con nuestra API de REST. Nuestra iniciativa Una API ofrecerá compatibilidad con SSO entre aplicaciones implementadas con SDK nativos y aplicaciones implementadas con la API de REST.

## Requisitos mínimos del dispositivo {#min_reqs}

Para utilizar la API de REST de autenticación de Adobe Pass, los dispositivos deben cumplir o superar los requisitos técnicos mínimos enumerados en la sección API de REST de la [Documento de requisitos de la plataforma de autenticación de Adobe Pass/dispositivo/herramientas](#general_clientless_reqs).
