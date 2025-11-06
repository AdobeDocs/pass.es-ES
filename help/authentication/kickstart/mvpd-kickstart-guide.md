---
title: Guía de KickStart de MVPD
description: Guía de KickStart de MVPD
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# Guía de KickStart de MVPD {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Esta guía de KickStart está dirigida a los distribuidores de programación de vídeo multicanal (MVPD) que planean integrarse con la autenticación Adobe® Pass.

Este documento describe los pasos iniciales clave para garantizar un inicio fluido y eficiente del proceso de integración. Su objetivo es aclarar expectativas y proporcionar orientación sobre cómo colaboraremos con los socios para lograr integraciones exitosas.

Adobe proporciona una serie de recursos para ayudarle a integrar con la autenticación de Adobe Pass. Consulte las menciones **&quot;Proporcionará&quot;** y **&quot;Adobe proporcionará&quot;** de cada sección a continuación.

>[!CAUTION]
>
> Cada vez que un usuario inicia un flujo de derechos, se le asigna un único ID de usuario opaco y único. Este ID, originado en MVPD, se utiliza para identificar al usuario dentro de la aplicación de un programador.
>
> <br/>
>
> El ID de usuario no debe contener información de identificación personal (PII) ni datos que se puedan usar, solos o en combinación con otros, para identificar o localizar al usuario.

## Proceso de configuración {#setup-process}

El proceso de configuración incluye, entre otros, los siguientes pasos:

![Proceso de integración de autenticación de Adobe® Pass](../assets/mvpd-int-lifecycle.png)

*Proceso de integración de autenticación de Adobe® Pass*

### Kickoff {#kickoff}

**Proporcionará** durante la fase de inicio:

* **Nombre para mostrar**

  Es una cadena que se muestra en los sitios web o aplicaciones del programador cuando se solicita a los usuarios que seleccionen su proveedor de TV de pago.

* **URL del logotipo**

  Se trata de un archivo de 112 x 33 píxeles que contiene el logotipo mostrado en las páginas web o aplicaciones del programador cuando se solicita a los usuarios que seleccionen su proveedor de TV de pago.

* **Tiempo de vida (TTL)**

  El TTL es un valor que suele establecer MVPD como parte de los procesos de autenticación o autorización. Sin embargo, Adobe puede anular esos valores TTL y proporcionar valores diferentes según lo que acuerden el Programador y MVPD.

* **Conjuntos de credenciales**

  Son credenciales utilizadas para autenticar y autorizar, o autenticar únicamente, al usuario con MVPD. Normalmente, estas credenciales constan de un nombre de usuario y una contraseña, que deben proporcionarse para ambos perfiles (ensayo y producción).

### Intercambio de metadatos (SAML) {#metadata-exchange-saml}

**Adobe proporcionará** durante la fase de intercambio de metadatos:

* **Metadatos del entorno de ensayo**

  Los metadatos de SP de Adobe se pueden recuperar de https://sp.auth-staging.adobe.com/sp/metadata.

* **Metadatos del entorno de producción**

  Los metadatos de SP de Adobe se pueden recuperar de https://sp.auth.adobe.com/sp/metadata.

**Proporcionará** durante la fase de intercambio de metadatos:

* **Metadatos de ensayo**

  Los metadatos de MVPD para el entorno de ensayo.

* **Metadatos de producción**

  Los metadatos de MVPD para el entorno de producción.

### Conectividad {#connectivity}

**Proporcionará** una forma de lista de permitidos de direcciones IP desde Adobe, ya que la autenticación de Adobe Pass requiere que los servidores de seguridad permitan el tráfico a través de los puertos 80 y 443 para habilitar el acceso a los recursos restringidos durante los procesos de autenticación y autorización.

**Proporcionará** una implementación en el perfil de ensayo para probar la conectividad.

### Desarrollo {#development}

**Adobe proporcionará tiempo de ingeniería de** para trabajar en estrecha colaboración con MVPD a fin de garantizar que la integración técnica se establezca correctamente. Este proceso implica el desarrollo de código personalizado adaptado a los requisitos específicos de MVPD.

### Implementación en ensayo {#deployment-staging}

**Adobe proporcionará** una compilación con las actualizaciones de código necesarias que se implementarán primero en el entorno de ensayo PRE-QUAL. Durante esta fase, también se implementarán los cambios de configuración necesarios para integrar MVPD con el proveedor de servicios `TestDistributors` con fines de prueba.

**Usted y Adobe proporcionarán** tiempo de control de calidad (QA) para garantizar que la integración se pruebe correctamente en el entorno de ensayo PRE-QUAL. Después de esta fase, MVPD se trasladará al entorno de ensayo RELEASE para realizar más pruebas con un programador real.

### Implementación en producción {#deployment-production}

**Proporcionará** una implementación en el perfil de producción para probar la conectividad.

**Adobe proporcionará** una compilación con las actualizaciones de código necesarias que se implementarán en el entorno de producción PRE-QUAL.

**Usted y Adobe proporcionarán** tiempo de control de calidad (QA) para garantizar que la integración se pruebe correctamente usando el perfil de producción. Si todo funciona correctamente en este momento, Adobe puede trasladar la integración al entorno de producción de RELEASE (&quot;live&quot;), que está disponible para todos los usuarios.

>[!IMPORTANT]
>
> Una vez que la integración esté activa en el entorno de producción de RELEASE, es fundamental mantener una experiencia del cliente óptima. Para abordar los escenarios de caída del servidor de forma eficaz, las MVPD deben proporcionar documentación detallada sobre los procedimientos de escalación a Adobe para gestionar estos problemas.
>
> A cambio, Adobe garantiza que las MVPD reciban la última versión del proceso de escalación de autenticación de Adobe Pass para optimizar la resolución de problemas.

## Acceso a entornos {#access-environments}

**Adobe proporcionará acceso a entornos para diferentes etapas del proceso de desarrollo:**

* **Calificación previa (PRE-QUAL)**

  El entorno PRE-QUAL aloja al candidato a la próxima versión y sirve como plataforma de integración inicial para nuevos socios. Antes de pasar al entorno de RELEASE, los socios tienen tiempo de probar su integración en PRE-QUAL.

* **Versión (VERSIÓN)**

  El entorno de RELEASE aloja la versión de producción actual (estable).

Para obtener más información sobre cómo usar estos entornos, consulte la [Explicación de los entornos de Adobe](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md).

>[!IMPORTANT]
> 
> Los cambios de configuración a estos entornos deben solicitarse explícitamente a través de su representante de Adobe, siguiendo el proceso de solicitud de cambio establecido.

## Acceso a asistencia al cliente {#access-customer-support}

**Adobe proporcionará acceso a nuestro sistema de atención al cliente a través de** Zendesk[. &#x200B;](https://tve.zendesk.com/home) Para acceder a Zendesk, debe registrarse y crear una cuenta en https://tve.zendesk.com/home.

El equipo de autenticación de Adobe Pass está disponible para resolver cualquier pregunta o problema técnico que pueda surgir durante el proceso de integración. Póngase en contacto con nosotros en [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Acceso a la documentación {#access-documentation}

**Adobe proporcionará acceso a nuestra documentación pública a través de** Adobe Experience League[.](https://experienceleague.adobe.com/en/docs/pass/authentication/home)

El equipo de autenticación de Adobe Pass proporciona documentación completa sobre las funciones y flujos de trabajo disponibles en la sección [Guía de integración para MVPD](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md). Consulte la tabla de contenido de esta sección para ver vínculos a información detallada sobre cada tema.

## Acceso a la herramienta de prueba {#access-testing-tool}

**Adobe proporcionará acceso a** nuestra herramienta de exploración de API a través del sitio web [Adobe Developer](https://developer.adobe.com/adobe-pass/).
