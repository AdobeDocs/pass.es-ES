---
title: Procedimientos de escalación
description: Procedimientos de escalación
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# Procedimientos de escalación {#escalation-procedures}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
> 
>Llame a la línea directa: **+1-205-693-9813** y envíe un mensaje de correo electrónico a **tve-support@adobe.com**, incluido **URGENTE - INCIDENTE** en la línea de asunto.

## Introducción {#introduction}

En este documento se describen los procedimientos de soporte para incidentes importantes (nivel **SEVERITY 1**) que afecten a la autenticación de Adobe Pass, la supervisión de la concurrencia de Adobe Pass y sus asociados.


## Definición de un incidente de nivel GRAVEDAD 1 {#definition-of-a-severity-1-level-incident}

Un incidente de nivel **SEVERITY 1** es una situación de **LIVE**, **que ocurre en el entorno de producción**, que no permite la finalización de los flujos de autenticación y/o autorización para un canal y una MVPD, lo que afecta a un gran número de suscriptores de la MVPD que realizan el flujo.


## Ejemplos de incidentes de GRAVEDAD 1 {#examples-of-severity-1-incidentcs}

* El Habilitador de acceso a la producción hospedado en `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (o `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) no está disponible.

* Para una MVPD específica, el Adobe ya no redirige ni muestra la página de inicio de sesión, una vez que el usuario selecciona la MVPD (en cualquiera de los exploradores admitidos).

* El socio recibe un gran número de informes que los usuarios no pueden autenticar/autorizar con una MVPD específica.

* Durante el proceso de autenticación, el usuario se queda atascado en una página de error de Adobe sin la posibilidad de volver a iniciar el flujo de autenticación/autorización.


| Ejemplos de lo que es **NOT** un incidente de gravedad 1 |
|---|
| En el caso de problemas de este tipo, el Adobe prestará apoyo a las investigaciones, pero no se trata de incidentes de gravedad 1:<ul><li>Uno o unos pocos suscriptores no pueden realizar el flujo debido a un problema con la versión del Flash (falta Flash, bloqueadores de Flash, versión de Flash incorrecta).</li><li>Uno o algunos suscriptores no pueden autenticarse y permanecer en la página de inicio de sesión de MVPD.</li><li>Uno o varios suscriptores están autenticados, pero no pueden reproducir vídeos.</li><li>Uno o pocos suscriptores encuentran un error de JavaScript en el sitio del programador.</li></ul> |

## Flujos de escalación de gravedad 1 {#severity-1-escalation-flows}

Los incidentes de gravedad 1 pueden iniciarlos el Adobe o un socio de autenticación de Adobe Pass. A continuación se presentan los pasos de cada uno de ellos.

### Flujo iniciado por el socio {#partner-initiated-flow}

1. El socio identifica un incidente de gravedad 1 (como se ha descrito anteriormente) que requiere la atención inmediata del Adobe.
1. El socio envía un correo electrónico a **tve-support@adobe.com** que incluye **URGENTE - INCIDENTE** en la línea de asunto y agrega la siguiente información:
   * Título
   * Descripción y pasos a seguir
   * SO/Explorador
   * SDK y versión
   * Dispositivos afectados
   * % de usuarios afectados
   * Registros de dispositivo o seguimiento HTTP que muestran el problema
   * (opcional) Cualquier captura de pantalla o vídeo disponible que demuestre el problema
1. Si el Adobe no responde al ticket en 30 minutos, el socio llama al siguiente número:
   **1-205-693-9813**
   >[!IMPORTANT]
   >Si no incluyes &quot;URGENTE-INCIDENTE&quot; en el título del ticket, no será recogido por nuestro sistema de notificación**.

### Flujo iniciado por el Adobe {#adobe-initiated-flow}

#### ...por un problema de autenticación de Adobe Pass {#adobe-initiated-flow-authn-issue}

1. El Adobe identifica un problema interno y abre un ticket con nuestro sistema de seguimiento.

1. El Adobe notifica al administrador de programas y al contacto técnico del socio, especificando el número de ticket y el impacto estimado del problema.

1. El Adobe trabaja para resolver el incidente y mantiene informados a todos los socios afectados.

#### ...por un problema con un socio (Programador/MVPD) {#adobe-initiated-flow-partner-issue}

1. El Adobe identifica un problema relacionado con la integración con una MVPD o en uno de los sitios del programador.

1. El Adobe notifica al socio <u>afectado siguiendo los procedimientos de soporte establecidos con ese socio</u> y abre un ticket con la organización de soporte del socio.

1. Si, durante el análisis de impacto, el Adobe identifica que el problema pertenece a una de las decisiones preacordadas sobre escenarios de incidentes, consulte **Decisiones preacordadas sobre escenarios de incidentes**, actuará en consecuencia sin esperar la entrada del socio.

1. El Adobe esperará las actualizaciones del socio y una notificación del socio cuando se haya restaurado el servicio.

## Decisiones preacordadas sobre escenarios de incidentes {#pre-agreed-descn}

Hay algunas situaciones en las que se realizará una acción predeterminada en el caso de que se produzca ese escenario:

|   | Escenario | Descripción | Acciones |
|---|---|---|---|
| S1 | El Adobe identifica un problema con la integración de una MVPD durante las operaciones de producción normales. | Durante las operaciones de producción normales, el Adobe identifica un problema con una de las MVPD que hace que sea imposible realizar los flujos de autenticación/autorización (por ejemplo, certificados caducados, respuestas SAML caducadas, puertos cerrados, parámetros modificados, etc.) | - Adobe notificará a los MVPD y programadores afectados.  </br> </br>: el Adobe desactivará esta MVPD para todos los programadores afectados. </br> </br>: el Adobe abrirá un ticket con la MVPD siguiendo el procedimiento de soporte acordado con esa MVPD |
| S2 | Adobe activa un nuevo MVPD para un programador, y el programador permite el MVPD antes de la fecha de lanzamiento. | El Adobe está activando un nuevo MVPD para el sitio de un programador, y el sitio ya está mostrando el nuevo MVPD en el selector, incluso si no se suponía que lo hiciera. | - Adobe notificará al Programador sobre el nuevo MVPD que aparece en el selector antes de la fecha programada. </br> </br>: el programador tomará medidas para quitarlo del selector si es necesario. |
| S3 | El Adobe activa un nuevo MVPD para un programador incluso si el MVPD no está listo para entrar en producción | El Adobe está activando una nueva MVPD para un programador, pero la MVPD aún no ha implementado la compatibilidad para la integración, por lo que no se pueden realizar los flujos de autenticación/autorización | - El Adobe realizará la implementación solo si el programador </br> lo solicita </br>: el programador será responsable de garantizar el permiso de la MVPD una vez que se hayan realizado todas las pruebas. |

## Expectativas De Respuesta Para Incidentes De Gravedad 1 {#response-expectations-for-severity-one-incidents}

* Respuesta inicial: 30 minutos (24/7)
* Plan de acción: 1 hora (24/7)
* Resolución: ASAP (24/7)
