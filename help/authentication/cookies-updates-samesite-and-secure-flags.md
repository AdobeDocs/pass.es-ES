---
title: 'Actualizaciones de cookies: indicadores SameSite y Secure'
description: 'Actualizaciones de cookies: indicadores SameSite y Secure'
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Actualizaciones de cookies: indicadores SameSite y Secure {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>


## Actualizaciones {#Updates}

En esta sección se destacan los cambios introducidos por el explorador Chrome y la autenticación de Adobe Pass para gestionar cookies de terceros.



### Actualizaciones de Chrome 80 {#Chrome}

A partir de la versión 80 de Chrome (excepto la versión 82), las cookies que no especifican un atributo *SameSite* se tratarán como si fueran *SameSite=Lax*. Por lo tanto, las cookies que necesitan enviarse en un contexto entre sitios deben especificar explícitamente *SameSite=None*, y también deben marcarse con el atributo *Secure* y entregarse a través de *HTTPS*. Se pueden leer más detalles sobre estas actualizaciones en la página oficial de chromium: <https://www.chromium.org/updates/same-site> y también en <https://web.dev/samesite-cookies-explained/>.


### Actualizaciones de autenticación de Adobe Pass {#Pass-Updates}

El servicio de autenticación de Adobe Pass depende actualmente de un par de cookies que se consideran cookies de terceros desde el punto de vista del explorador, incluido Chrome, para funcionar en combinación con algunas plataformas y versiones de los SDK de autenticación de Adobe Pass. Por lo tanto, para cumplir con los próximos cambios y continuar entregando estas cookies en un contexto entre sitios desde estos SDK más antiguos, el servicio de autenticación de Adobe Pass implementa los cambios necesarios en la versión *adobe-pass-2.55.1*.

Estos cambios de la versión *adobe-pass-2.55.1* implican la adición de los atributos *Secure* y *SameSite=None* para todas sus cookies devueltas a todos los SDK de autenticación de Adobe Pass al usar navegadores Chrome a partir de la versión 80 (excepto la versión 82).

La siguiente sección presenta algunos problemas potenciales para una lista de plataformas y versiones del SDK de autenticación de Adobe Pass en caso de que un usuario utilice el explorador Chrome 80 o posterior (excepto la versión 82).

## Resolución de problemas {#Troubleshooting}

Mientras explora esta sección, tenga en cuenta que todas las cookies del servicio de autenticación de Adobe Pass deben tener el atributo *Secure* establecido en la versión *adobe-pass-2.55.1* para todos los exploradores, mientras que el atributo *SameSite=None* debe establecerse solamente para los exploradores Chrome versión 80 y posteriores (excepto la versión 82).


### Solución de problemas general {#General}

1. Es importante tener en cuenta que algunos agentes de usuario son incompatibles con el atributo *SameSite=None*.

   - Versiones de Chrome de Chrome 51 a Chrome 66 (incluidas en ambos extremos). Estas versiones de Chrome rechazarán una cookie con *SameSite=None*. Esto también afecta a las versiones anteriores de los exploradores derivados de Chromium, así como a Android WebView. Este comportamiento era correcto según la versión de la especificación de la cookie en ese momento, pero con la adición del nuevo valor &quot;Ninguno&quot; a la especificación, este comportamiento se ha actualizado en Chrome 67 y versiones posteriores. (Antes de Chrome 51, el atributo SameSite se ignoraba por completo y todas las cookies se trataban como si fueran *SameSite=None*).
   - Versiones del explorador de comunicaciones unificadas en Android anteriores a la 12.13.2. Las versiones anteriores rechazarán una cookie con *SameSite=None*. Este comportamiento era correcto según la versión de la especificación de la cookie en ese momento, pero con la adición del nuevo valor &quot;Ninguno&quot; a la especificación, este comportamiento se ha actualizado en versiones más recientes del explorador de comunicaciones unificadas.
   - Versiones de Safari y de los exploradores incrustados en MacOS 10.14 y todos los exploradores en iOS 12. Estas versiones tratarán erróneamente las cookies marcadas con *SameSite=None* como si estuvieran marcadas como *SameSite=Strict*. Este error se ha corregido en versiones más recientes de iOS y MacOS.


1. Es importante tener en cuenta que las cookies que tienen el atributo *Secure* deben enviarse a través de *HTTPS*; de lo contrario, la cookie no llegará al servicio de autenticación de Adobe Pass.

   - SDK de JavaScript de AccessEnabler:
      - Es obligatorio que la comunicación con *sp.auth.adobe.com* use *HTTPS* para las versiones *2.35* y *3.5.0*, antes de presentar el registro de cliente dinámico.
   - SDK de AccessEnabler para iOS/tvOS:
      - Es obligatorio que la comunicación con *sp.auth.adobe.com* use *HTTPS* para las versiones anteriores a *3.0.0*, antes de introducir el registro de cliente dinámico.
   - SDK de Android de AccessEnabler:
      - Es obligatorio que la comunicación con *sp.auth.adobe.com* use *HTTPS* para las versiones anteriores a *3.0.0*, antes de introducir el registro de cliente dinámico.
   - SDK de AccessEnabler FireOS:
      - Es obligatorio que la comunicación con *sp.auth.adobe.com* use *HTTPS* para la versión *2.0.4*.

</br>

### Solución de problemas de AccessEnabler JavaScript SDK versión 2.35 {#235-Troubleshooting}

El flujo de autenticación del usuario puede verse afectado en Chrome 80 y versiones posteriores (excepto en la versión 82). Para asegurarse de que el usuario no tenga problemas para autenticarse debido a las actualizaciones anteriores, se puede:

- Compruebe que la cookie *JSESSIONID* esté establecida en el explorador y que los atributos *SameSite=None* y *Secure* estén establecidos.
- Compruebe que la cookie *JSESSIONID* de la solicitud de red *https://sp.auth.adobe.com/authenticate/saml* coincida con la cookie *JSESSIONID* de la solicitud de red *https://sp.auth.adobe.com/session*.


### Solución de problemas del SDK de JavaScript versión 3.5.0 de AccessEnabler {#350-Troubleshooting}

El flujo de autenticación del usuario puede verse afectado en Chrome 80 y versiones posteriores (excepto en la versión 82). Para asegurarse de que el usuario no tenga problemas para autenticarse debido a las actualizaciones anteriores, se puede:

- Compruebe que la cookie *JSESSIONID* esté establecida en el explorador y que los atributos *SameSite=None* y *Secure* estén establecidos.
- Compruebe que la cookie *JSESSIONID* de la solicitud de red *https://sp.auth.adobe.com/authenticate/saml* coincida con la cookie *JSESSIONID* de la solicitud de red *https://sp.auth.adobe.com/session*.
- Compruebe que la cookie *pass\_sfp* esté establecida en el explorador y que los atributos *SameSite=None* y *Secure* estén establecidos.
- Compruebe que la cookie *pass\_sfp* esté establecida en la solicitud de red *https://sp.auth.adobe.com/session*.


El flujo de autorización del usuario puede verse afectado en Chrome 80 y versiones posteriores (excepto en la versión 82). Para asegurarse de que el usuario no tiene problemas para ver un recurso protegido, después de autenticarse correctamente, debido a las actualizaciones anteriores, se puede:

- Compruebe que la cookie *pass\_sfp* esté establecida en el explorador y que los atributos *SameSite=None* y *Secure* estén establecidos.
- Compruebe que la cookie *pass\_sfp* esté establecida en la solicitud de red *https://sp.auth.adobe.com/adobe-services/authorize*.
- Compruebe que la cookie *pass\_sfp* esté establecida en la solicitud de red *https://sp.auth.adobe.com/adobe-services/shortAuthorize*.
