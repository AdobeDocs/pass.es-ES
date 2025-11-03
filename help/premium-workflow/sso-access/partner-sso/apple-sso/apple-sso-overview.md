---
title: Información general sobre Apple SSO
description: Información general sobre Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 2afe9ea2a814817757f1ab28484a84466da68d62
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Información general sobre Apple SSO {#apple-sso-overview}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Apple permite a los usuarios iniciar sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, lo que elimina la necesidad de autenticarse aplicación por aplicación.

La autenticación de Adobe Pass se asoció con Apple para crear la experiencia de usuario de inicio de sesión único (SSO) de socio en el ecosistema de TV en todas partes para propietarios de iPhone, iPad y Apple TV.

Para beneficiarse de la experiencia del usuario de inicio de sesión único (SSO) en un dispositivo Apple, hay una lista de requisitos previos documentados a continuación que deben completarse.

El resultado final debe crear una experiencia en línea con los siguientes flujos de usuarios, que le recomendamos que consulte antes de comenzar a desarrollar la aplicación:

* Flujos de usuario [de inicio de sesión único (SSO) para dispositivos iPhone y iPad](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf).
* Flujos de usuario [de inicio de sesión único (SSO) para dispositivos Apple TV](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf).

## Requisitos previos {#apple-sso-prerequisites}

Los requisitos previos de incorporación pueden aplicarse a una o varias entidades involucradas en el negocio de TVE, como programadores, MVPD, autenticación de Adobe Pass o Apple.

### Programador {#apple-sso-prerequisites-programmer}

Para beneficiarse de la experiencia de usuario de inicio de sesión único (SSO), un programador debe:

* Póngase en contacto con Apple para habilitar [Marco de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) como parte de su ID de equipo de Apple y configurar el [derecho de inicio de sesión único de suscriptor de vídeo](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) como parte de su cuenta de desarrollador de Apple.

   * Utilice Xcode versión 8 o posterior y iOS/tvOS versión 10 o posterior.

* Habilite el inicio de sesión único (SSO) para cada integración y plataforma deseadas (iOS/tvOS) a través del [Panel de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication), para lo cual debe establecer la propiedad `Enable Single Sign On` en `Yes`.

| Habilitar inicio de sesión único de Adobe | Apple **MVPD incorporadas (admitidas)** | **Selector** MVPD de Apple | Apple **No Incorporado (No Compatible)** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Sí (habilitado) | Los flujos de autenticación y cierre de sesión implicarán soluciones de autenticación de Apple y Adobe Pass, mientras que el resto de flujos (autorización, preautorización, metadatos, etc.) se gestionarán únicamente mediante la autenticación de Adobe Pass. | Los flujos de autenticación y cierre de sesión volverán a los flujos normales a los que solo sirve la autenticación de Adobe Pass. | Los flujos de autenticación y cierre de sesión volverán a los flujos normales a los que solo sirve la autenticación de Adobe Pass. |
| No (deshabilitado) | Los flujos de autenticación y cierre de sesión volverán a los flujos normales a los que solo sirve la autenticación de Adobe Pass. | Los flujos de autenticación y cierre de sesión volverán a los flujos normales a los que solo sirve la autenticación de Adobe Pass. | Los flujos de autenticación y cierre de sesión volverán a los flujos normales a los que solo sirve la autenticación de Adobe Pass. |

* Integre los flujos de usuario de inicio de sesión único (SSO) mediante una de las siguientes soluciones que ofrece la autenticación de Adobe Pass para los usuarios finales de aplicaciones cliente que se ejecutan en iOS, iPadOS o tvOS.

   * La API de REST de autenticación de Adobe Pass V2 es compatible con el inicio de sesión único (SSO) de socio.

     Consulte la [Guía de Apple SSO (API REST V2)](apple-sso-cookbook-rest-api-v2.md).

   * La API de REST de autenticación de Adobe Pass V1 heredada es compatible con el inicio de sesión único (SSO) de socio.

     Consulte la documentación de [ (heredado) Apple SSO Cookbook (REST API V1)](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md).

   * El Adobe Pass Authentication AccessEnabler iOS/tvOS SDK heredado es compatible con el inicio de sesión único (SSO) de socio.

     Consulte la documentación de [ (heredado) Apple SSO (iOS/tvOS SDK)](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md).

### MVPD {#apple-sso-prerequisites-mvpd}

Para beneficiarse de la experiencia de usuario de inicio de sesión único (SSO), un MVPD debe:

* Póngase en contacto con Apple para iniciar el proceso de incorporación en el lado de Apple.

   * Solicite la documentación técnica sobre cómo integrar y desarrollar una aplicación TVML de JavaScript capaz de gestionar el formulario de inicio de sesión del usuario.

* Póngase en contacto con la autenticación de Adobe Pass para iniciar el proceso de incorporación en el lado de Adobe.

   * Proporcione el valor de cadena que representa el identificador del proveedor de TV asignado por Apple durante el proceso de incorporación.

## FAQ {#FAQ}

* En caso de que algo salga mal con el flujo de trabajo de SSO de Apple, ¿puede la aplicación que utiliza el Adobe Pass Authentication AccessEnabler iOS/tvOS SDK tener la capacidad de volver al flujo de autenticación normal?

  Esto es posible, pero requiere que se realice un cambio de configuración a través del [Panel de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) para establecer el **Habilitar el inicio de sesión único** en **NO** para la integración y plataforma deseadas (iOS/tvOS). Tenga en cuenta que la aplicación cliente reconocerá el cambio de configuración solo después de llamar a la API [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3).


* ¿Sabrá la aplicación cuándo se ha producido una autenticación como resultado del inicio de sesión a través de Apple SSO?

  Esta información está disponible como parte de la clave de metadatos del usuario: *tokenSource*, que debería devolver el valor de cadena: &quot;Apple&quot; en este caso.


* ¿Sabrá la aplicación cuándo se ha producido una autenticación como resultado de un inicio de sesión a través de Apple SSO en otra aplicación?

  Esta información no está disponible.


* ¿Qué sucede si un usuario inicia sesión en la sección *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS con un MVPD que no está integrado con la aplicación?

  Cuando el usuario inicia la aplicación, no se autentica mediante el flujo de trabajo de SSO de Apple. Por lo tanto, la aplicación tendría que volver al flujo de autenticación normal y presentar su propio selector de MVPD.


* ¿Qué sucede si un usuario inicia sesión en *`Settings -> TV Provider`* en iOS/iPadOS o en *`Settings -> Accounts -> TV Provider`* en la sección de tvOS usando un MVPD que tiene **Habilitar inicio de sesión único** establecido en **NO** a través del [Panel de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) para la plataforma iOS/tvOS?

  Cuando el usuario inicia la aplicación, no se autentica mediante el flujo de trabajo de SSO de Apple. Por lo tanto, la aplicación tendría que volver al flujo de autenticación normal y presentar su propio selector de MVPD.


* ¿Qué sucede si un usuario tiene un MVPD que no está incorporado (no es compatible) en Apple, pero que está presente en el selector de Apple?

  Cuando el usuario inicia la aplicación, solo selecciona el MVPD mediante el flujo de trabajo de SSO de Apple sin completar el flujo de autenticación. Por lo tanto, la aplicación tendría que volver al flujo de autenticación normal, pero podría utilizar el MVPD ya seleccionado.


* ¿Qué sucede si un usuario tiene un MVPD que no está incorporado (no es compatible) en Apple?

  Cuando el usuario inicia la aplicación, selecciona la opción de selector &quot;Otros proveedores de TV&quot; mediante el flujo de trabajo de SSO de Apple. Por lo tanto, la aplicación tendría que volver al flujo de autenticación normal y presentar su propio selector de MVPD.


* ¿Qué sucede si un usuario tiene un MVPD que se ha degradado a través de [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)?

  Cuando el usuario inicia la aplicación, se autentica mediante el mecanismo de degradación y no mediante el flujo de trabajo de SSO de Apple. La experiencia debería ser perfecta para el usuario, mientras que a la aplicación se le informará a través del código de advertencia *N010* en caso de que esté usando el SDK iOS/tvOS Adobe Pass Authentication AccessEnabler.


* ¿Cambiará el ID de usuario de MVPD entre el SSO de Apple y los flujos de autenticación SSO que no son de Apple?

  Se espera que el ID de usuario no cambie, pero debe verificarse para cada proveedor seleccionado.


* ¿Habrá algún cambio en los TTL de autenticación?

  La autenticación de Adobe Pass seguirá respetando los TTL requeridos por los programadores para su integración con cada MVPD. Al navegar de una aplicación de Programador a otra aplicación de Programador a través de Apple SSO, la segunda aplicación tendrá el TTL de su integración correspondiente de Programador x MVPD (no compartirá el TTL de la primera aplicación que se autentica)

|                                      | TTL de autenticación de Adobe Pass caducado | TTL de autenticación de Adobe Pass válido |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **El TTL del token del dispositivo de Apple caducó** | el usuario NO está autenticado (debería aparecer el selector de MVPD) | El usuario se autentica y el TTL es el tiempo restante de su token/perfil de autenticación de Adobe Pass |
| **TTL de token de dispositivo de Apple válido** | El usuario de se autentica de forma silenciosa y obtiene otro token/perfil de autenticación de Adobe Pass con el TTL especificado en el Tablero de TVE | El usuario se autentica y el TTL es el tiempo restante de su token/perfil de autenticación de Adobe Pass |
