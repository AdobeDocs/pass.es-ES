---
title: Amazon fireTV SSO - Guía de inicio del programador
description: Amazon fireTV SSO - Guía de inicio del programador
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# (Heredado) Amazon fireTV SSO - Guía de inicio del programador {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

</br>

## Introducción {#intro}

Este documento describe la información necesaria para integrar el nuevo SDK de **Adobe Pass Authentication fireTV** en tu aplicación fireTV. Este nuevo SDK aprovecha la integración a nivel del SO en la plataforma fireTV de Amazon y ofrece compatibilidad con el inicio de sesión único **Single Sign On**. Para beneficiarse del inicio de sesión único se requiere un pequeño esfuerzo por su parte para migrar su aplicación de la API sin cliente al nuevo SDK fireTV. A continuación se detallan algunos cambios en los flujos de autenticación.

## Arquitectura de alto nivel e integración a nivel del sistema operativo {#high}

Con el fin de lograr el inicio de sesión único entre las aplicaciones de TV Everywhere en Amazon fireTV platform y para mejorar la experiencia general en esta plataforma, decidimos integrar nuestro SDK principal a nivel del sistema operativo fireTV. Los programadores deberán compilar con una biblioteca de código auxiliar proporcionada por Adobe. La funcionalidad real la proporcionará la biblioteca de Adobe presente en el sistema operativo fireTV de Amazon.

Hasta que Amazon proporcione un simulador fireTV que incorpore nuestra biblioteca a nivel del sistema operativo, el desarrollo solo sería posible usando dispositivos fireTV reales.

## Ventajas {#bene}

* Inicio de sesión único entre todas las aplicaciones de Adobe powered TV Everywhere en la plataforma Amazon fireTV con todas las MVPD integradas.
* Posibilidad de beneficiarse de HBA (con MVPD compatibles).
* Capacidad de usar la última versión de FireTV SDK sin necesidad de actualizar las aplicaciones cada vez que se lanza una nueva versión de SDK.
* Todas las aplicaciones de TVE se benefician del uso de la biblioteca del sistema compartido al eliminar la necesidad de tener una copia local de la biblioteca AccessEnabler. Esto también garantiza que todas las aplicaciones utilicen la misma versión de SDK.
* Autenticación de pantalla única: sin necesidad de código de registro y flujos de trabajo de segunda pantalla.

## Migración de la aplicación basada en API sin cliente a la aplicación basada en SDK fireTV {#migra1}

Para migrar de la API sin cliente a fireTV SDK, debe eliminar el código base relacionado con la API sin cliente e integrar el nuevo SDK fireTV.

En comparación con la aplicación basada en la API sin cliente, con el nuevo FireTV SDK la autenticación se mueve a la primera pantalla, ya no hay necesidad de una segunda autenticación de pantalla.

Esto requiere que los programadores agreguen un selector de MVPD a sus aplicaciones para que los usuarios puedan elegir su proveedor de TV directamente en el dispositivo fireTV. Una vez seleccionado MVPD, se mostrará al usuario la página de inicio de sesión de MVPD en el dispositivo fireTV.

Puede encontrar mallas metálicas de los flujos de usuario que describen los escenarios normales, HBA y SSO en fireTV en [Flujo de usuario de inicio de sesión de Amazon Fire TV - MVVPD](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migración de la aplicación basada en Android SDK a la aplicación basada en SDK fireTV {#migra2}

Esta nueva SDK de fireTV es muy similar a nuestra SDK de Android existente y la documentación actual que tenemos para **integrar nuestra SDK de Android** <!--http://tve.helpdocsonline.com/android-technical-overview-->se puede usar hasta que tengamos listos los documentos de SDK de fireTV. Si ya tiene aplicaciones de Android que usan nuestro SDK de Android, entonces la integración de fireTV SDK en su aplicación de fireTV debería ser sencilla.

En comparación con Android SDK existente, en fireTV SDK el proceso de autenticación será más fácil de desarrollar, ya que las tareas de administrar/presentar la página de inicio de sesión de MVPD y recuperar el token de AuthN las realizará internamente la biblioteca AccessEnabler.

## Preguntas frecuentes {#faq}

1. ¿Cómo funcionará el **SSO**?

   * SSO funcionará en todas las aplicaciones de Programador con autenticación de Adobe Pass que estén usando el nuevo SDK fireTV en el mismo dispositivo Amazon fireTV
   * No se admitirá SSO entre aplicaciones de programador implementadas en la API de REST sin cliente y aplicaciones implementadas en fireTV SDK ****

1. ¿Cuál es la cobertura de MVPD de FireTV SSO?

   * **Todas las MVPD** integradas por la autenticación de Adobe Pass serán técnicamente compatibles con SSO en fireTV SDK.

1. Además de usar el nuevo SDK, ¿qué otros **cambios en el flujo de trabajo** deben tener en cuenta los programadores?

   * Los programadores deben implementar un selector de MVPD para FireTV.

1. ¿Habrá algún cambio en la autenticación **TTL**?

   * No hay cambios en el comportamiento con respecto a los TTL de autenticación.
   * El primer token de autenticación válido se utilizará para realizar el SSO y, en este caso, todas las demás aplicaciones que se autenticarán mediante SSO utilizarán el mismo TTL hasta que caduque. Por lo tanto, al navegar de una aplicación a otra, la segunda aplicación compartirá el TTL de la primera aplicación que se autentica.

1. ¿Cómo funciona la **API de degradación**?

   * No se necesitan cambios para la API de degradación; la experiencia del usuario será la misma que en los dispositivos Android.

1. ¿Cómo se ven afectados los flujos **TempPass**?

   * Los flujos TempPass son de una sola pantalla y se comportan como en cualquier otro dispositivo nativo.

1. ¿Funcionará otra funcionalidad de Adobe como antes?

   * Toda la funcionalidad de autenticación de Adobe Pass funcionará en fireTV como en dispositivos Android.
