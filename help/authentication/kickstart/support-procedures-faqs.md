---
title: Preguntas frecuentes sobre procedimientos de soporte
description: Preguntas frecuentes sobre procedimientos de soporte
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 0ab1fc212752dd4a4d6e12a4ab1287ef74e4a282
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Preguntas frecuentes sobre procedimientos de soporte {#support-procedures-faqs}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Este documento describe las preguntas más frecuentes (FAQ) sobre los procedimientos de soporte para incidentes importantes (nivel SEVERITY 1) que afectan a la autenticación de Adobe Pass y a sus socios.

## Preguntas frecuentes {#faqs}

### ¿Qué es un incidente de nivel 1 de GRAVEDAD? {#support-procedures-faqs-1}

Un incidente de nivel GRAVEDAD 1 es una situación activa en el entorno de producción que impide la finalización de los flujos de autenticación o autorización para un canal y una MVPD, lo que afecta a un gran número de suscriptores.

Ejemplos de incidentes de GRAVEDAD 1

* Durante el proceso de autenticación, no se redirige al usuario a la página de inicio de sesión después de que este haya seleccionado MVPD en cualquier explorador compatible.

* Durante el proceso de autenticación, el usuario se queda atascado en una página de error de Adobe sin la capacidad de volver a iniciar el flujo de autenticación.

* El socio recibe numerosos informes que los usuarios no pueden autenticar ni autorizar con un MVPD específico.

* El activador de acceso a la producción alojado en https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js no está disponible.

### ¿Qué es un incidente de nivel no GRAVE 1?

Adobe apoyará las investigaciones de estos problemas, pero no se consideran incidentes de nivel 1 GRAVEDAD:

* Uno o unos pocos suscriptores no pueden autenticarse y permanecer en la página de inicio de sesión de MVPD.

* Uno o unos pocos suscriptores están autenticados, pero no pueden reproducir vídeos.

* Algunos o todos los suscriptores encuentran un error de JavaScript en el sitio del programador.

### ¿Cómo se gestionan los incidentes de nivel SEVERITY 1?

Una incidencia de nivel GRAVEDAD 1 puede iniciarla Adobe o un socio de autenticación de Adobe Pass. A continuación se describen los pasos de cada uno de ellos.

**Flujo iniciado por el socio**

1. El socio identifica un incidente de nivel de gravedad 1 que requiere la atención inmediata de Adobe.

1. El socio envía un correo electrónico a **tve-support@adobe.com** que incluye **URGENTE - INCIDENTE** en la línea de asunto y agrega la siguiente información:
   * Título
   * Descripción y pasos a seguir
   * SO/Explorador
   * SDK y versión
   * Dispositivos afectados
   * % de usuarios afectados
   * Registros de dispositivo o seguimiento HTTP que muestran el problema
   * (opcional) Cualquier captura de pantalla o vídeo disponible que demuestre el problema

1. Si Adobe no responde al ticket dentro de un período, el socio puede llamar al siguiente número: **1-657-312-4623**.

>[!IMPORTANT]
>
> Si no incluyes &quot;URGENTE-INCIDENTE&quot; en el título del ticket, no será recogido por nuestro sistema de notificación.

**Flujo iniciado por Adobe**

Para un problema de autenticación de Adobe Pass:

1. Adobe identifica un problema interno y abre un ticket en nuestro sistema de seguimiento.

1. Adobe notifica al administrador de programas y al contacto técnico del socio, especificando el número de ticket y el impacto estimado del problema.

1. Adobe trabaja para resolver el incidente y mantiene informados a todos los socios afectados.

Para un problema con un socio (Programador/MVPD):

1. Adobe identifica un problema relacionado con la integración con un MVPD o en uno de los sitios del programador.

1. Adobe notifica al socio afectado siguiendo los procedimientos de asistencia establecidos con dicho socio y abre un ticket con la organización de asistencia del socio.

1. Si, durante el análisis de impacto, Adobe identifica que el problema entra dentro de una de las decisiones preacordadas sobre escenarios de incidentes, actuará en consecuencia sin esperar la contribución del socio.

1. Adobe esperará actualizaciones del socio y una notificación cuando se haya restaurado el servicio.

### ¿Qué son las decisiones preacordadas sobre escenarios de incidentes?

Algunas situaciones con acciones predeterminadas que se realizan si se produce el escenario:

|    | Escenario | Descripción | Acciones |
|----|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | Adobe identifica un problema con la integración de MVPD durante las operaciones de producción normales. | Durante las operaciones de producción normales, Adobe identifica un problema con una de las MVPD que hace que sea imposible realizar los flujos de autenticación/autorización (por ejemplo, certificados caducados, respuestas SAML caducadas, puertos cerrados, parámetros modificados, etc.) | Adobe notificará a los programadores y MVPD afectados.  </br></br> Adobe desactivará esta MVPD para todos los programadores afectados. </br></br> Adobe abrirá un ticket con MVPD siguiendo el procedimiento de soporte acordado con MVPD |
| S2 | Adobe activa un nuevo MVPD para un programador y el programador permite el MVPD antes de la fecha de inicio. | Adobe está activando un nuevo MVPD para el sitio de un programador y el sitio ya está mostrando el nuevo MVPD en el selector, aunque no se suponía que lo hiciera. | Adobe notificará al programador que el nuevo MVPD aparece en el selector antes de la fecha programada. El programador de </br></br> tomará medidas para quitarlo del selector si es necesario. |
| S3 | Adobe activa un nuevo MVPD para un programador incluso si el MVPD no está listo para su producción | Adobe está activando un nuevo MVPD para un programador, pero MVPD aún no ha implementado la compatibilidad con la integración, por lo que no se pueden realizar los flujos de autenticación/autorización | Adobe realizará la implementación solo si el programador </br></br> se lo solicita. El programador será responsable de garantizar la autorización de MVPD una vez que se hayan realizado todas las pruebas. |
