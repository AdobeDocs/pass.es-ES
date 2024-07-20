---
title: Seguimiento de evaluación de prevención Google Chrome
description: Seguimiento de evaluación de prevención Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---

# Evaluación de la prevención de seguimiento: Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general

Este documento agrega recursos útiles y evalúa los próximos cambios planificados por Google Chrome como parte de su iniciativa de eliminar gradualmente las cookies de terceros.

La evaluación se realiza para aplicaciones de TV en todas partes (TVE) que se ejecutan en el explorador Google Chrome y que utilizan el SDK v4 de JavaScript del habilitador de acceso a Adobe Pass para integrarse con los servicios back-end de autenticación de Adobe Pass.

## Recursos públicos

Vea a continuación una lista de los recursos agregados desde el sitio web para desarrolladores de Google y también desde su blog oficial que recomendamos a nuestros clientes que consulten:

* [El siguiente paso para eliminar gradualmente las cookies de terceros en Chrome](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [Documentación para desarrolladores para espacio aislado de privacidad](https://developers.google.com/privacy-sandbox)
* [Prepararse para restricciones de cookies de terceros](https://developers.google.com/privacy-sandbox/3pcd)
* [Prepararse para la eliminación gradual de cookies de terceros](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [Preparándose para el fin de las cookies de terceros](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [Las cookies de terceros están restringidas de forma predeterminada para el 1% de los usuarios de Chrome](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## Cronología

A modo de breve resumen, Google Chrome comenzó a probar [Tracking Protection](https://privacysandbox.com/), una nueva característica que limita el seguimiento entre sitios y que afecta a todas las cookies de terceros.

Inicialmente, esto comenzó a principios de 2024 y afecta a alrededor del 1 % de sus usuarios y su plan (tentativo) es ampliar esto hasta el 100 % de los usuarios a partir del tercer trimestre de 2024.

## Evaluación

Google ha publicado un documento en el que agrega su manual de implementación recomendado para prepararse para la eliminación gradual de las cookies de terceros en el siguiente vínculo: https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout.

Nos adherimos a este manual para la evaluación de aplicaciones de TV en todas partes (TVE) que se ejecutan en el navegador Google Chrome y que utilizan el SDK v4 de JavaScript para habilitar el acceso a Adobe Pass para integrarse con los servicios back-end de autenticación de Adobe Pass.

### Conclusiones

En función de nuestras pruebas, simulando las próximas actualizaciones de Google Chrome, los flujos comerciales de TVE principales **seguirán funcionando según lo esperado**.

Sin embargo, es crucial reconocer la estrategia más amplia de Google, que implica no solo suspender las cookies de terceros, sino también dividir el almacenamiento de terceros.

En consecuencia, los usuarios de Chrome experimentarán interrupciones con el inicio de sesión único (SSO), el cierre de sesión único (SLO) y las funciones de autenticación pasiva, lo que requerirá acciones de inicio de sesión/cierre de sesión independientes para cada aplicación de TVE que utilicen (alineada con la experiencia actual en Safari).

## Llamar a la autoevaluación

Instamos a nuestros clientes a realizar evaluaciones similares de forma proactiva para identificar posibles problemas con mucha antelación y familiarizarse con la experiencia revisada del usuario de Google Chrome.

Esta evaluación debe abarcar tanto los servicios de origen como los servicios de terceros, especialmente en relación con la integración del SDK v4 de JavaScript del activador de acceso a Adobe Pass.

En caso de que encuentre dificultades relacionadas con los flujos comerciales de TVE, como autenticación, preautorización, autorización, metadatos de usuario o cierre de sesión, le invitamos a presentar un informe a través de un ticket de Zendesk con nuestro equipo de atención al cliente.

Para obtener asistencia en el desarrollo de su plan de autoevaluación, consulte las secciones siguientes.

### Auditar el uso de cookies

A partir de Chrome 118, la ficha [Problemas de DevTools](https://developer.chrome.com/docs/devtools/issues/) resalta las cookies potencialmente afectadas con el siguiente mensaje: `Cookie sent in cross-site context will be blocked in future Chrome versions`.

Las cookies marcadas para uso de terceros se pueden identificar mediante su valor de atributo `SameSite=None`.

Siga este enlace para obtener más información: https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### Prueba de rotura

Para comprobar si hay algún error, inicie Chrome con el marcador de línea de comandos `--test-third-party-cookie-phaseout` o desde Chrome 118, habilite `#test-third-party-cookie-phaseout` en `chrome://flags/`.

Esto configurará Google Chrome para bloquear las cookies de terceros y garantizar que la futura funcionalidad esté activa para simular mejor el estado después de la eliminación.

Vale la pena profundizar en las especificaciones técnicas de los siguientes indicadores Chrome:

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

Siga este enlace para obtener más información: https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## Otros exploradores

### Firefox

Firefox implementó su mecanismo llamado: `Enhanced Tracking Protection` hace un par de años.

Vea a continuación algunos recursos útiles de Firefox:

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari implementó su mecanismo llamado: `Intelligent Tracking Prevention` hace un par de años.

Consulte a continuación algunos recursos útiles de Safari:

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

Consulte a continuación algunos recursos útiles de Adobe Pass:

* [Evaluación de la prevención del seguimiento: Apple Safari](tracking-prevention-assessment-apple-safari.md)
