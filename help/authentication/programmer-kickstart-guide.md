---
title: Guía de KickStart del programador
description: Guía de KickStart del programador
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 0%

---

# Guía de KickStart del programador {#programmer-kickstart-guide}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#prog-kickstart-guide-intro}

Bienvenido a la autenticación de Adobe Pass para TV en todas partes. Esperamos poder trabajar con usted.

>[!NOTE]
>
>Esta es la Guía de KickStart para programadores (proveedores de contenido). Si trabaja con un Distribuidor de programación de vídeo multicanal (MVPD), asegúrese de consultar la [guía de inicio de MVPD](/help/authentication/mvpd-kickstart-guide.md).


Contactos de autenticación de Adobe Pass:

* Soporte: para todas las preguntas, incidentes o solicitudes de características, `tve-support@adobe.com`
* Se asignará un contacto de Habilitación al proyecto cuando este comience.

En la siguiente información se describen algunos primeros pasos importantes para comenzar de forma sólida y eficaz. El objetivo es proporcionar una explicación y expectativas sobre cómo trabajaremos con los socios para lograr integraciones. Tenga en cuenta las secciones &quot;proporcionará&quot; / &quot;El Adobe proporcionará&quot; a continuación para cada artículo. Estos se enumeran a través de una lista de comprobación o guía a medida que trabajamos en el proyecto.

Este documento supone que los programadores están registrados para trabajar con un socio de MVPD elegido.

## Programación de versiones {#release-schedule}

El ciclo de desarrollo de sprint de Adobe está planificado para que pueda ver cuándo se programan nuestras versiones y cómo se promociona cada versión a través de nuestro sistema de desarrollo.

Una vez completado cada sprint, está disponible para QE o para ver nuevas implementaciones en nuestro servidor UAT.

Aproximadamente, cada 6 semanas se realiza una versión en el servidor de precalificación de Adobe. (Este es el servidor en el que realizamos la próxima versión propuesta mientras realizamos el QE final). Estas compilaciones incluirán todo el trabajo completado en sprints desde la última gota. En este momento hay disponible una ventana de QE de dos semanas para que los socios prueben esta versión.

Si no se producen problemas críticos durante las dos semanas anteriores a la fase de prueba, la versión se promocionará para su producción en directo. Esto significa que la integración estará disponible en el entorno de la versión de Adobe, pero los socios elegirán cuándo harán público el lanzamiento.

<!--For the latest release schedule information, see the Release Calendar.-->

## Documentación de asistencia {#supp-doc}

El Adobe proporcionará:

* Guía de implementación: **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Acceso a nuestro sistema de atención al cliente de Zendesk. Aquí también puede encontrar ejemplos, información y tutoriales de vídeo sobre algunos de los procesos. Para acceder a este documento en Zendesk, junto con otros documentos publicados allí, tendrá que registrarse y crear una cuenta en `https://tve.zendesk.com/home`. No hay límite en la cantidad de usuarios que puede registrar.  Puede ver y compartir comentarios en cualquier ticket archivado. Todas las preguntas de soporte técnico deben dirigirse a `tve-support@adobe.com`.
* [Guía de integración del programador](/help/authentication/programmer-integration-guide-overview.md)
* Biblioteca de comprobador de tokens de medios: `https://tve.zendesk.com/entries/471323-media-token-validator-library`.

## Configuración del entorno de prueba {#test-env-setup}

Adobe le configurará primero con el sitio de prueba de Adobe, donde Adobe actúa como MVPD para fines de prueba. A continuación, su equipo puede configurar un sitio web de prueba que llame a la API de Adobe. Utilice el selector predeterminado de MVPD y seleccione &quot;Adobe&quot; como idP.

Proporcionará lo siguiente:

1. ID de solicitante. Se trata de una cadena que identifica de forma exclusiva la marca del sitio web o la aplicación que realiza solicitudes de autenticación de Adobe Pass. La cadena en sí es arbitraria, pero debe acordarse entre el Adobe y el programador
1. Información del canal. Es un conjunto de cadenas que identifica los canales de contenido que solicita el ID del solicitante. En muchos casos, el canal y el ID del solicitante son el mismo. Sin embargo, es posible que tenga varios canales de contenido que se puedan solicitar por el mismo ID. Las cadenas del nombre del canal deben corresponder a los canales de TV por cable. Algunas MVPD validan este valor sobre el protocolo AuthN y/o AuthZ.
1. Nombres de dominio (permitidos para ese ID de solicitante). Esta será una lista de nombres de dominio reales que se enumerarán por Adobe para aceptar el ID del solicitante. Esto garantiza que solo los dominios aprobados tengan acceso a la autenticación de Adobe Pass con los metadatos. NOTA: los nombres de dominio válidos para la producción pueden ser diferentes para las pruebas y el ensayo, y ambos deben proporcionarse e identificarse.

El Adobe configurará la cuenta y el Adobe proporcionará:

* Inicio de sesión y contraseña para acceder al sitio de prueba

## Configuración con MVPD {#setup-mvpd}

En esta sección se describe lo que necesita al migrar del sitio de prueba de Adobe para trabajar con una MVPD.

Proporcionará (a través de MVPD):

* **Dos conjuntos de credenciales**:
   * AuthN + AuthZ : nombre de usuario y contraseña para un usuario autenticado y autorizado
   * AuthN + Non-AuthZ : nombre de usuario y contraseña para un usuario autenticado pero no autorizado
* **Id. de recurso**. Este es un identificador de contenido específico que se validará con una MVPD a través del protocolo AuthZ. Esto puede ser a nivel de canal, programa, episodio o recurso; debe acordarse con su MVPD.

La autenticación de Adobe Pass admite un esquema de metadatos basado en MRSS, lo que significa que los ID de recurso pueden ser tan específicos como sea necesario y pueden incluir identificadores que pueden ser únicos para una MVPD específica.

**NUEVA integración de MVPD**: Es importante recordar que la MVPD elegida juega un papel integral en completar cualquier integración. El Adobe debe escribir el código de cada MVPD según sus especificaciones. Hasta que se completen estos pasos, no podrá seleccionar ese MVPD en el cuadro de diálogo ni completar la prueba del producto. El Adobe debe programar este trabajo con antelación para que se ajuste al siguiente sprint disponible. (Para obtener información sobre la programación actual, consulte el Calendario de versiones).

**Integraciones de MVPD existentes**: Si la MVPD elegida ya está configurada con Adobe, los pasos de conectividad deberían ser mucho más sencillos (más rápidos) y, a menudo, la conectividad se puede lograr mediante cambios de configuración.

>[!NOTE]
>
>El MVPD TODAVÍA tendrá que habilitar al Programador, y firmar cualquier acuerdo comercial relevante.

**QE con MVPD**: Todas las integraciones implicarán QE conjunto y, dado que el usuario final es en última instancia cliente de MVPD, muchos han establecido ciclos de prueba antes de activar el servicio. Dado que esto implica la programación de los recursos de MVPD, esta es una área potencial de demora.

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
