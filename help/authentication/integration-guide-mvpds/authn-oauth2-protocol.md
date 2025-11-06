---
title: Autenticación mediante el protocolo OAuth 2.0
description: Autenticación mediante el protocolo OAuth 2.0
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 0%

---

# Autenticación mediante el protocolo OAuth 2.0

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

Aunque SAML sigue siendo el principal protocolo utilizado para la autenticación por las MVPD de EE. UU. y las empresas en general, existe una clara tendencia a pasar a OAuth 2.0 como protocolo de autenticación principal. El protocolo OAuth 2.0 (https://tools.ietf.org/html/rfc6749) se desarrolló principalmente para sitios de consumidores y fue adoptado rápidamente por gigantes de Internet como Facebook, Google y Twitter.

OAuth 2.0 tiene un enorme éxito y esto ha llevado a las empresas a actualizar lentamente su infraestructura para apoyarla.



## Ventajas de pasar a OAuth 2.0 {#adv-oauth2}

En un nivel superior, el protocolo OAuth 2.0 ofrece la misma funcionalidad que el protocolo SAML, pero hay algunas distinciones importantes.

Uno de ellos es el hecho de que el flujo del token de actualización puede utilizarse como una forma de actualizar la autenticación entre bastidores. Esto permite que el IdP (las MVPD en este caso) mantenga el control y al mismo tiempo ofrezca una buena experiencia de usuario, ya que el usuario ya no tiene que iniciar sesión con frecuencia debido a problemas de seguridad.

El protocolo también ofrece más flexibilidad en términos de los datos expuestos, ya que un proveedor de servicios ahora puede utilizar los tokens para acceder a otras API y obtener información adicional. Esto, a su vez, resulta en un protocolo &quot;chattier&quot; para casos de uso de TVE, pero permite la flexibilidad necesaria para flujos de trabajo complejos.





## Requisitos para cambiar a OAuth 2.0 {#oauth-req}

Para admitir la autenticación con OAuth 2.0, un MVPD debe cumplir los siguientes requisitos previos:

En primer lugar, MVPD debe asegurarse de que admite el flujo [Concesión de código de autorización](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html).

Después de confirmar que admite el flujo, MVPD debe proporcionarnos la siguiente información:

* el punto final de autenticación
   * el punto final proporcionará el código de autorización que se utilizará más adelante a cambio del token de actualización y acceso
* el punto final /token
   * proporcionará el token de actualización y el token de acceso
   * el token de actualización debe ser estable (no debe cambiar cada vez que solicitamos un nuevo token de acceso)
   * MVPD necesita permitir varios tokens de acceso activos para cada token de actualización
   * este extremo también intercambiará un token de actualización por un token de acceso
* necesitamos un **punto final para el perfil de usuario**
   * este extremo proporcionará el ID de usuario, que debe ser único para una cuenta y no debe contener información de identificación personal
* el punto final **/logout** (opcional)
   * La autenticación de Adobe Pass redireccionará a este punto final, proporciona al MVPD un URI de redireccionamiento; en este punto final, el MVPD puede borrar las cookies en el equipo cliente o aplicar la lógica deseada para el cierre de sesión
* es muy recomendable tener compatibilidad con clientes autorizados (aplicaciones de cliente que no almacenan en déclencheur una página de autorización de usuario)
* también necesitaremos lo siguiente:
   * **clientID** y **clientsecret** para las configuraciones de integración
   * **tiempo de vida** valores (TTL) para el token de actualización y el token de acceso
   * Podemos proporcionar a MVPD un URI de devolución de llamada de autorización y devolución de llamada de cierre de sesión. Además, si es necesario, podemos proporcionar a las MVPD una lista de las IP que se incluirán en la lista de direcciones permitidas en la configuración del cortafuegos.


## Flujo de autenticación {#authn-flow}

En el flujo de autenticación, la autenticación de Adobe Pass se comunicará con MVPD en el protocolo seleccionado en la configuración. El flujo de OAuth 2.0 se muestra en la siguiente imagen:



![Diagrama para mostrar el flujo de autenticación en la autenticación de Adobe que se comunica con MVPD en el protocolo seleccionado en la configuración.](../assets/authn-flow.png)

**Figura 1: Flujo de autenticación de OAuth 2.0**



## Solicitud y respuesta de autenticación {#authn-req-response}

En pocas palabras, el flujo de autenticación para las MVPD que admiten el protocolo OAuth 2.0 sigue estos pasos:

1. El usuario final navega al sitio del programador y selecciona para iniciar sesión con sus credenciales de MVPD
1. El AccessEnabler instalado del lado del programador con envía una solicitud de autenticación en forma de solicitud HTTP al extremo de autenticación de Adobe Pass, que el extremo de autenticación de Adobe Pass redirige al extremo de autorización de MVPD.
1. El extremo de autorización de MVPD envía un código de autorización al extremo de autenticación de Adobe Pass
1. La autenticación de Adobe Pass utiliza el código de autorización recibido para solicitar un token de actualización y un token de acceso del extremo de token de MVPD
1. Se puede enviar una llamada para recuperar información y metadatos de usuario al extremo del perfil de usuario en caso de que la información del usuario no se incluya en el token
1. El token de autenticación se pasa al usuario final, que ahora puede examinar correctamente el sitio del programador

   >[!NOTE]
   >
   >El token de actualización se utiliza para obtener un nuevo token de acceso después de que el token de acceso actual se vuelva no válido o caduque.


>[!IMPORTANT]
>
>El token de actualización no debe cambiar cuando se intercambia por un token de acceso.

Esta limitación se deriva de los flujos de cliente que no permiten al servidor actualizar el AuthNToken, que, para el protocolo OAuth 2.0, también contiene el token de actualización.

Un flujo de autorización típico realiza un intercambio del token de actualización guardado en el AuthNToken por un token de acceso que se utiliza posteriormente para realizar la llamada de autorización en el nombre del usuario que se autenticó en primer lugar. Si el servidor de autorización (MVPD) cambiara el token de actualización e invalidara el anterior, no se podría actualizar el AuthNToken válido. Por este motivo, las MVPD deben admitir tokens de actualización estables para poder configurar integraciones de OAuth 2.0 para ellos.


## Migración de SAML a OAuth 2.0 {#saml-auth2-migr}

Adobe y MVPD realizarán la migración de integraciones de SAML a OAuth 2.0. No es necesario realizar ningún cambio técnico en el lado del programador, aunque es posible que este desee comprobar o probar la promoción conjunta de marca en la página de inicio de sesión de MVPD. Desde el punto de vista de MVPD, los puntos de conexión y otra información solicitada en los requisitos de Oauth 2.0 son obligatorios.

Para **conservar SSO**, los usuarios que ya tengan un token de autenticación obtenido a través de SAML se seguirán considerando autenticados y sus solicitudes se enrutarán a través de la antigua integración de SAML.

Desde una perspectiva técnica:

1. Adobe habilitará una integración de OAuth 2.0 entre el programador y MVPD, SIN eliminar la integración de SAML.
1. Después de la activación, todos los usuarios nuevos utilizarán flujos de OAuth 2.0.
1. Los usuarios ya autenticados, que ya tienen un token de AuthN local que contiene el ID de asunto de SAML, Adobe los enrutará automáticamente a través de la integración de SAML.
1. Para los usuarios en el paso número 3, una vez que caduque su token de AuthN generado por SAML, Adobe los tratará como nuevos usuarios y se comportará como los usuarios en el paso número 2.
1. Adobe revisará los patrones de uso para determinar en qué momento se puede desactivar de forma segura la integración de SAML.
