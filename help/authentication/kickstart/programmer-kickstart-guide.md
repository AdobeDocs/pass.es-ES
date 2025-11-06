---
title: Guía de KickStart del programador
description: Guía de KickStart del programador
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 37858fa83aecbdf443a4a6058c78e4f9246eee42
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Guía de KickStart del programador {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Esta guía de KickStart está dirigida a los proveedores de contenido (programadores) que planean integrar la autenticación de Adobe® Pass en sus sitios web o aplicaciones.

Este documento describe los pasos iniciales clave para garantizar un inicio fluido y eficiente del proceso de integración. Su objetivo es aclarar expectativas y proporcionar orientación sobre cómo colaboraremos con los socios para lograr integraciones exitosas.

Adobe proporciona una serie de recursos para ayudarle a integrar la autenticación de Adobe Pass en su sitio web o aplicación. Consulte las menciones **&quot;Proporcionará&quot;** y **&quot;Adobe proporcionará&quot;** de cada sección a continuación.

## Proceso de configuración {#setup-process}

El proceso de configuración incluye, entre otros, los siguientes pasos:

![Proceso de integración de autenticación de Adobe® Pass](../assets/progr-flow-int-lifecycle.png)

*Proceso de integración de autenticación de Adobe® Pass*

**Proporcionará** durante la fase de inicio:

* **Proveedor de servicios (identificador de solicitante)**

  Se trata de una cadena que identifica de forma exclusiva la marca del sitio web o la aplicación que realiza solicitudes de autenticación de Adobe Pass. La cadena en sí es arbitraria, pero debe acordarse entre Adobe y el programador

* **Información de canal**

  Es un conjunto de cadenas que se utiliza para identificar los canales de contenido solicitados por el proveedor de servicios. En muchos casos, el canal y el proveedor de servicios son los mismos. Sin embargo, un solo identificador puede representar varios canales de contenido. Estas cadenas de nombre de canal deben alinearse con los canales de TV por cable correspondientes. Tenga en cuenta que algunas MVPD pueden validar este valor durante el proceso de autenticación o autorización.

* **Nombres de dominio**

  Esta lista incluirá los nombres de dominio reales enumerados a Adobe para representar al proveedor de servicios. Garantiza que solo los dominios autorizados puedan acceder a la autenticación de Adobe Pass con los metadatos. Asegúrese de proporcionar e identificar claramente los nombres de dominio para los entornos de producción y ensayo (prueba), ya que pueden diferir.

**Proporcionará** a través de MVPD:

* **Conjuntos de credenciales**

  Son credenciales utilizadas para autenticar y autorizar, o autenticar únicamente, al usuario con MVPD. Normalmente, estas credenciales constan de un nombre de usuario y una contraseña que MVPD le proporcionará para ambos perfiles (ensayo y producción).

* **Identificadores de recursos**

  Son identificadores únicos para los canales de contenido, programas, episodios o recursos que el proveedor de servicios desea proteger. Estos identificadores se utilizan para solicitar decisiones de autorización y deben acordarse con MVPD.

>[!IMPORTANT]
>
> El programador es responsable de la coordinación con MVPD para finalizar cualquier acuerdo comercial necesario. Mientras tanto, la autenticación de Adobe Pass colaborará con MVPD para garantizar que la integración técnica esté correctamente establecida:
>
> * **Nuevo MVPD**
>
>     Si MVPD no está integrado con Adobe, se debe desarrollar un código personalizado basado en los requisitos específicos de MVPD. Hasta que se complete este desarrollo, MVPD no estará disponible y las pruebas de producto con MVPD no podrán continuar.
>
> * **MVPD existente**
>
>     Si MVPD ya está integrado con Adobe, el proceso de conectividad se optimiza considerablemente. En la mayoría de los casos, la conectividad se puede establecer rápidamente mediante ajustes de configuración en lugar de un desarrollo exhaustivo.
>
> Todas las integraciones requieren esfuerzos conjuntos de garantía de calidad (QA), incluidas pruebas realizadas por MVPD, ya que el usuario final es en última instancia cliente de MVPD. La coordinación de los ciclos de prueba suele depender de la disponibilidad de recursos de MVPD, lo que puede provocar posibles retrasos.

## Acceso a asistencia al cliente {#access-customer-support}

**Adobe proporcionará acceso a nuestro sistema de atención al cliente a través de** Zendesk[. &#x200B;](https://tve.zendesk.com/home) Para acceder a Zendesk, debe registrarse y crear una cuenta en https://tve.zendesk.com/home. No hay límite en el número de usuarios que puede registrar. Una vez registrado, puede ver y compartir comentarios en cualquier ticket enviado.

El equipo de autenticación de Adobe Pass está disponible para ayudarle con cualquier pregunta o problema técnico que pueda encontrar durante el proceso de integración. Póngase en contacto con nosotros en [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Acceso a la documentación {#access-documentation}

**Adobe proporcionará acceso a nuestra documentación pública a través de** Adobe Experience League[.](https://experienceleague.adobe.com/es/docs/pass/authentication/home)

El equipo de autenticación de Adobe Pass proporciona documentación completa sobre las funciones y API disponibles en la sección [Guía de integración para programadores](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md). Consulte la tabla de contenido de esta sección para ver vínculos a información detallada sobre cada tema.

## Acceso a la herramienta de prueba {#access-testing-tool}

**Adobe proporcionará acceso a** nuestra herramienta de exploración de API a través del sitio web [Adobe Developer](https://developer.adobe.com/adobe-pass/).

## Acceso a la herramienta de administración de configuración {#access-configuration-management-tool}

**Adobe proporcionará acceso a** una herramienta de autoservicio para administrar tu configuración y tus datos a través de [Adobe Pass TVE Dashboard](https://experience.adobe.com/pass/authentication).

El equipo de autenticación de Adobe Pass proporciona documentación completa sobre el uso del tablero de TVE en la sección [Guía del usuario del tablero de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md). Consulte la tabla de contenido de esta sección para ver vínculos a información detallada sobre cada tema.
