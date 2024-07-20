---
title: Información general sobre Apple SSO
description: Información general sobre Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Información general sobre Apple SSO {#apple-sso-overview}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#Introduction}

Apple proporciona una API que permite a las personas iniciar sesión en su cuenta de proveedor de TV en el nivel de sistema del dispositivo, lo que elimina la necesidad de autenticarse aplicación por aplicación.

Por lo tanto, Apple y Autenticación de Adobe Pass se asociaron para crear la experiencia de usuario de inicio de sesión único (SSO) de plataforma en el ecosistema de TV en todas partes para propietarios de iPhone, iPad y Apple TV.

Para beneficiarse de la experiencia de usuario de inicio de sesión único (SSO) en un dispositivo Apple, existe una lista de requisitos previos que deben completarse.

</br>

## Requisitos previos {#Prerequisites}

Los requisitos previos pueden aplicarse a una o varias entidades involucradas en el negocio de TVE, como programadores, MVPD, autenticación de Adobe Pass o Apple.

</br>

### Programador {#Programmer}

Para beneficiarse de la experiencia de usuario de inicio de sesión único (SSO), un programador debe:

1. Utilice al menos la versión 8 de Xcode y la versión 10 de iOS/tvOS.

1. Tenga el [derecho de inicio de sesión único del suscriptor de vídeo](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) configurado en su cuenta de desarrollador de Apple. Póngase en contacto con Apple para habilitar [módulo de cuenta de suscriptor de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para el identificador de equipo de Apple.

1. Habilite el inicio de sesión único (SÍ) para cada integración deseada (Canal x MVPD) y plataforma deseada (iOS / tvOS) a través del [Panel de Adobe Primetime TVE](https://console.auth.adobe.com/).

1. Integre los flujos de trabajo de SSO de Apple con una de las dos soluciones siguientes que ofrece el equipo de autenticación de Adobe Pass:

   - La API de REST de autenticación de Adobe Pass puede admitir la autenticación de inicio de sesión único (SSO) de plataforma para los usuarios finales de aplicaciones cliente que se ejecuten en iOS, iPadOS o tvOS. Consulte también [Guía de SSO de Apple (API de REST)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - El SDK de Adobe Pass Authentication AccessEnabler de iOS/tvOS puede admitir la autenticación de inicio de sesión único (SSO) de la plataforma para los usuarios finales de aplicaciones cliente que se ejecuten en iOS, iPadOS o tvOS. Consulte también [Guía de Apple SSO (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>Sugerencia profesional:</u>** Para tener acceso a la información de suscripción del usuario, el usuario debe dar permiso a la aplicación para continuar, de forma similar a proporcionar acceso a la cámara o al micrófono del dispositivo. Este permiso debe solicitarse por aplicación y el dispositivo guardará la selección del usuario. Tenga en cuenta que el usuario puede cambiar su decisión en la configuración de la aplicación (acceso al permiso del proveedor de TV) o en la sección de *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS.

   - **<u>Sugerencia profesional:</u>** Se recomienda solicitar el permiso del usuario cuando la aplicación entre en el estado en primer plano, pero es solo una sugerencia, ya que la aplicación puede comprobar el permiso de [para acceder](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) a la información de suscripción del usuario en cualquier momento antes de requerir la autenticación del usuario. Además, las API del SDK de iOS/tvOS de AccessEnabler solicitarán automáticamente el permiso del usuario cuando lo necesite.

   - **<u>Sugerencia profesional:</u>** Recomendamos incentivar a los usuarios que se nieguen a dar permiso para acceder a la información de suscripción explicando las ventajas del inicio de sesión único (SSO). Tenga en cuenta que el usuario puede cambiar su decisión en la configuración de la aplicación (acceso al permiso del proveedor de TV) o en la sección de *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS.

El resultado debe crear una experiencia en línea con los siguientes flujos de usuarios, que le sugerimos consultar antes de comenzar a desarrollar su aplicación o aplicaciones:

- [Flujos de usuario de iPhone / iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)
- [Apple TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) flujos de usuarios


>[!IMPORTANT]
>
> Cuando la función de inicio de sesión único está **habilitada** para iOS/tvOS **y** en el caso de MVPD de Apple **integradas (admitidas) o seleccionadoras**, los flujos de autenticación/cierre de sesión de los flujos de trabajo de SSO de Apple implicarán soluciones de autenticación de Apple y Adobe Pass, mientras que todos los demás flujos (autorización, preautorización, metadatos, etc.) será atendido únicamente por Autenticación de Adobe Pass.


>[!IMPORTANT]
>
> Cuando la función de inicio de sesión único está **deshabilitada** para iOS/tvOS **o** en el caso de MVPD de Apple **no integradas (no admitidas)**, los flujos de autenticación/cierre de sesión se volverán de los flujos de trabajo SSO de Apple a los normales a los que solo sirve la autenticación de Adobe Pass.


>[!IMPORTANT]
>
> Una ganancia principal del flujo de trabajo de SSO de Apple está representada por el flujo de usuario de autenticación de una pantalla, que también se puede entregar en Apple TV cuando la función de inicio de sesión único está **habilitada** para tvOS **y** en el caso de MVPD **incorporadas (admitidas)** de Apple.


### MVPD {#MVPD}

Para beneficiarse de la experiencia de usuario de inicio de sesión único (SSO), debe
MVPD debe:



1. Incorpórese al flujo de trabajo de SSO de Apple del lado de Apple. Póngase en contacto con Apple para facilitar el proceso de incorporación.
1. Proporcionar una aplicación TVML de JavaScript capaz de gestionar el formulario de inicio de sesión del usuario. Póngase en contacto con Apple para recibir la documentación adecuada.
1. Proporcione un valor de cadena que represente el identificador de proveedor asignado por Apple durante el proceso de incorporación. Póngase en contacto con Autenticación de Adobe Pass para realizar cambios en la configuración.

</br>

## FAQ {#FAQ}

1. En caso de que algo salga mal con el flujo de trabajo de SSO de Apple, ¿puede la aplicación que utiliza el SDK de AccessEnabler para iOS/tvOS tener la capacidad de volver al flujo de autenticación normal?
   - Esto es posible, pero requiere que se realice un cambio de configuración en el [Panel de Adobe Primetime TVE](https://console.auth.adobe.com/). *Habilitar el inicio de sesión único* debe estar establecido en *NO* para la integración deseada (canal x MVPD) y la plataforma deseada (iOS/tvOS).
   - La aplicación reconocería el cambio de configuración solo después de llamar a la API [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) en caso de que utilice el SDK iOS/tvOS de AccessEnabler.
1. ¿Sabrá la aplicación cuándo se ha producido una autenticación como resultado de un inicio de sesión a través del SSO de la plataforma en otro dispositivo u otra aplicación?
   - Esta información no estará disponible.
1. ¿Sabrá la aplicación cuándo se ha producido una autenticación como resultado de un inicio de sesión a través del SSO de plataforma en el mismo dispositivo?
   - Esta información está disponible como parte de la clave de metadatos del usuario: *tokenSource*, que debería devolver el valor de cadena: &quot;Apple&quot; en este caso.
1. ¿Qué sucede si un usuario inicia sesión en la sección *`Settings -> TV Provider`* en iOS/iPadOS o *`Settings -> Accounts -> TV Provider`* en tvOS usando una MVPD que no está integrada con la aplicación?
   - Cuando el usuario inicia la aplicación, no se autentica mediante el flujo de trabajo de SSO de Apple. Por lo tanto, la aplicación tendría que volver al flujo de autenticación normal y presentar su propio selector de MVPD.
1. ¿Qué sucede si un usuario inicia sesión yendo a *`Settings -> TV Provider`* en iOS/iPadOS o a *`Settings -> Accounts -> TV Provider`* en la sección de tvOS usando una MVPD que tiene *Activar inicio de sesión único* establecido en *NO* en el [Panel de Adobe Primetime TVE](https://console.auth.adobe.com/) para la plataforma iOS/tvOS?
   - Cuando el usuario inicia la aplicación, no se autentica mediante el flujo de trabajo de SSO de Apple. Por lo tanto, la aplicación tendría que volver al flujo de autenticación normal y presentar su propio selector de MVPD.
1. ¿Qué sucede si un usuario tiene una MVPD que no está integrada (no es compatible) en Apple, pero está presente en el selector de Apple?
   - Cuando el usuario inicia la aplicación, solo selecciona la MVPD mediante el flujo de trabajo de SSO de Apple sin completar el flujo de autenticación. Por lo tanto, la aplicación tendría que volver al flujo de autenticación normal, pero podría utilizar la MVPD ya seleccionada.
1. ¿Qué sucede si un usuario tiene una MVPD que no está integrada (no es compatible) en Apple?
   - Cuando el usuario inicia la aplicación, selecciona la opción de selector &quot;Otros proveedores de TV&quot; mediante el flujo de trabajo de SSO de Apple. Por lo tanto, la aplicación tendría que volver al flujo de autenticación normal y presentar su propio selector de MVPD.
1. ¿Qué sucede si un usuario tiene una MVPD que se degrada a través del [Tablero de TVE de Adobe Primetime](https://console.auth.adobe.com/)?
   - Cuando el usuario inicia la aplicación, se autentica mediante el mecanismo de degradación y no mediante el flujo de trabajo de SSO de Apple.
   - La experiencia debería ser perfecta para el usuario, mientras que a la aplicación se le informará a través del código de advertencia *N010* en caso de que esté usando el SDK iOS/tvOS de AccessEnabler.
1. ¿Cambiará el ID de usuario de MVPD entre el SSO de Apple y el flujo de autenticación SSO que no es de Apple?
   - Se espera que el ID de usuario no cambie, pero debe verificarse para cada proveedor seleccionado.
1. ¿Habrá algún cambio en los TTL de autenticación?
   - La autenticación de Adobe Pass seguirá respetando los TTL requeridos por los programadores para su integración con cada MVPD.
   - Al navegar de una aplicación de Programador a otra aplicación de Programador a través de Apple SSO, la segunda aplicación tendrá el TTL de su integración correspondiente de Programador x MVPD (no compartirá el TTL de la primera aplicación que se autentica)

|                                      | TTL de autenticación de Adobe Pass caducado | TTL de autenticación de Adobe Pass válido |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **El TTL del token del dispositivo de Apple caducó** | el usuario NO está autenticado (debería aparecer el selector de MVPD) | El usuario se autentica y el TTL es el tiempo restante de su token de autenticación de Adobe Pass |
| **TTL de token de dispositivo de Apple válido** | El usuario de se autentica de forma silenciosa y obtiene otro token de autenticación de Adobe Pass con el TTL especificado en el Tablero de TVE | El usuario se autentica y el TTL es el tiempo restante de su token de autenticación de Adobe Pass |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
