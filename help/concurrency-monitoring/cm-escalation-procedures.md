---
title: Procedimientos de escalación de supervisión de concurrencia
description: Procedimientos de escalación de supervisión de concurrencia
exl-id: eb110465-3a74-489e-a521-0e17f5aeecb8
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Procedimientos de escalación de supervisión de concurrencia {#esc-procedures}

>[!NOTE]
>
>Llame a la línea directa: +1-205-693-9813 y envíe un mensaje de correo electrónico a `tve-support@adobe.com`, incluido &quot;URGENTE - INCIDENTE&quot; en la línea de asunto.


## Introducción {#cm-escalation-intro}

En este documento se describen los procedimientos de soporte para incidentes importantes (nivel **SEVERITY 1**) que afecten a la autenticación de Adobe Pass, la supervisión de la concurrencia de Adobe Pass y sus asociados.

## Definición de Nivel de Gravedad de Escalación 1 {#defn-escl-sevrityone-level}

Un incidente de nivel **SEVERITY 1** es una situación de **LIVE**, **que ocurre en el entorno de producción**, que no permite la finalización de los flujos de autenticación o autorización para un canal y un MVPD, lo que afecta a un gran número de suscriptores del MVPD que realizan el flujo.

## Ejemplos de incidentes de Gravedad 1 {#exampl-sevone-incident}

* El Habilitador de acceso a la producción hospedado en <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> no está disponible.

* Para un MVPD específico, Adobe ya no redirige ni muestra la página de inicio de sesión, una vez que el usuario selecciona el MVPD (en cualquiera de los exploradores admitidos).

* El socio recibe un gran número de informes que los usuarios no pueden autenticar/autorizar con un MVPD específico.

* Durante el proceso de autenticación, el usuario se queda atascado en una página de error de Adobe sin la posibilidad de volver a iniciar el flujo de autenticación/autorización.


## Ejemplos de lo que es *NOT* un incidente de gravedad 1 {#exampl-not-sev1}

*En el caso de problemas de este tipo, Adobe proporcionará soporte para las investigaciones, pero no son incidentes de gravedad 1:*

* Uno o algunos suscriptores no pueden realizar el flujo debido a un problema con la versión de Flash (falta Flash, bloqueadores Flash, versión de Flash incorrecta).
* Uno o varios suscriptores no pueden autenticarse y permanecer en la página de inicio de sesión de MVPD.
* Uno o varios suscriptores están autenticados, pero no pueden reproducir vídeos.
* Uno o pocos o todos los suscriptores encuentran un error de JavaScript en el sitio del programador.

## Flujos de escalación de gravedad 1 {#sevone-escalation-flows}

Los incidentes de gravedad 1 pueden iniciarlos Adobe o un socio de autenticación de Adobe Pass. A continuación se presentan los pasos de cada uno de ellos.

### Flujo iniciado por el socio {#partner-initiated-flow}

1. El socio identifica un incidente de gravedad 1 (como se ha descrito anteriormente) que requiere la atención inmediata de Adobe.

1. El socio envía un correo electrónico a tve-support@adobe.com que incluye &quot;URGENTE - INCIDENTE&quot; en la línea de asunto y añade la siguiente información:

   * Título
   * Descripción y pasos a seguir
   * SO
   * Explorador
   * Versión de Flash
   * (opcional) Cualquier captura de pantalla o vídeo disponible que demuestre el problema

1. Si Adobe no responde al ticket en un plazo de 30 minutos, el socio llama al siguiente número:

   * **1-205-693-9813**


**Si no incluye &quot;URGENT-INCIDENT&quot; en el título del ticket, nuestro sistema de notificación no lo recogerá.**

### Flujo iniciado por Adobe {#adobe-initiated-flow}

**...por un problema de autenticación de Adobe Pass**

1. Adobe identifica un problema interno y abre un ticket con nuestro sistema de seguimiento.

1. Adobe notifica al administrador de programas y al contacto técnico del socio, especificando el número de ticket y el impacto estimado del problema.

1. Adobe trabaja para resolver el incidente y mantiene informados a todos los socios afectados.


**...por un problema con un socio (Programador/MVPD)**

1. Adobe identifica un problema relacionado con la integración con un MVPD o en uno de los sitios del programador.

1. Adobe notifica al socio **afectado siguiendo los procedimientos de asistencia establecidos con ese socio** y abre un ticket con la organización de asistencia del socio.

1. Si, durante el análisis de impacto, Adobe identifica que el problema pertenece a una de las decisiones preacordadas sobre escenarios de incidente (consulte la sección &quot;Decisiones preacordadas sobre escenarios de incidente&quot; a continuación), actuará en consecuencia sin esperar al socio1. La entrada de.

1. Adobe esperará actualizaciones del socio y una notificación del socio cuando se haya restaurado el servicio.

### Decisiones preacordadas sobre escenarios de incidentes {#pre-agreed-decisions}

Hay algunas situaciones en las que se realizará una acción predeterminada en el caso de que se produzca ese escenario:

|    | Escenario | Descripción | Acciones |
|:---:|:---|:---|:---|
| S1 | Adobe identifica un problema con la integración de MVPD durante las operaciones de producción normales. | Durante las operaciones de producción normales, Adobe identifica un problema con una de las MVPD que hace que sea imposible realizar los flujos de autenticación/autorización (por ejemplo, certificados caducados, respuestas SAML caducadas, puertos cerrados, parámetros modificados, etc.) | Adobe notificará a los programadores y MVPD afectados. Adobe desactivará este MVPD para todos los programadores afectados. Adobe abrirá un ticket con MVPD siguiendo el procedimiento de asistencia acordado con ese MVPD |
| S2 | Adobe activa un nuevo MVPD para un programador y el programador incluye el MVPD en la lista de elementos permitidos antes de la fecha de inicio. | Adobe está activando un nuevo MVPD para el sitio de un programador y el sitio ya está mostrando el nuevo MVPD en el selector, aunque no se suponía que lo hiciera. | Adobe notificará al programador que el nuevo MVPD aparece en el selector antes de la fecha programada. El programador tomará medidas para quitarlo del selector si es necesario. |
| S3 | Adobe activa un nuevo MVPD para un programador incluso si el MVPD no está listo para su producción | Adobe está activando un nuevo MVPD para un programador, pero MVPD aún no ha implementado la compatibilidad con la integración, por lo que no se pueden realizar los flujos de autenticación/autorización | Adobe realizará la implementación solo si el programador lo solicita. El programador será responsable de garantizar la inclusión de MVPD en la lista de admitidos una vez que se hayan realizado todas las pruebas. |

### Expectativas De Respuesta Para Incidentes De Gravedad 1 {#response-expectations}

* Respuesta inicial: 30 minutos (24/7)
* Plan de acción: 1 hora (24/7)
* Resolución: ASAP (24/7)
