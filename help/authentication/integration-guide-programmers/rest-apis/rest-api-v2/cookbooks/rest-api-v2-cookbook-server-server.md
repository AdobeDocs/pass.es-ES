---
title: Guía de API de REST V2 (servidor a servidor)
description: Guía de API de REST V2 (servidor a servidor)
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# Guía de API de REST V2 (servidor a servidor) {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

El propósito de este documento de guía es detallar las prácticas recomendadas para implementar la autenticación de Adobe Pass mediante la API de REST V2 en una arquitectura de servidor a servidor. Proporciona requisitos básicos, implementación de flujo paso a paso y consideraciones generales para entornos y operaciones de producción.

## Componentes {#components}

En una solución de servidor a servidor en funcionamiento, están implicados los siguientes componentes:

| Tipo | Componente | Descripción |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dispositivo de streaming | Aplicación de streaming | La aplicación de programador que reside en el dispositivo de transmisión del usuario y reproduce vídeo autenticado. |
|                           | Módulo \[Optional\] AuthN | Si el dispositivo de streaming tiene un agente de usuario (es decir, un explorador web), el módulo AuthN es responsable de autenticar al usuario en el MVPD IdP. |
| \[Opcional\] Dispositivo AuthN | Aplicación AuthN | Si el dispositivo de streaming no tiene un agente de usuario (es decir, un explorador web), la aplicación AuthN es una aplicación web de programador a la que se accede desde un dispositivo de usuario independiente mediante un explorador web. |
| Infraestructura del programador | Servicio de programador | Servicio que vincula el dispositivo de streaming con el servicio de Adobe Pass para obtener decisiones de autenticación y autorización. |
| Infraestructura de Adobe | Servicio de Adobe Pass | Servicio que se integra con el servicio MVPD IdP y AuthZ y proporciona decisiones de autenticación y autorización. |
| Infraestructura de MVPD | ID de MVPD | Punto final de MVPD que proporciona un servicio de autenticación basado en credenciales para validar la identidad de su usuario. |
|                           | Servicio AuthZ de MVPD | Punto final de MVPD que proporciona decisiones de autorización basadas en las suscripciones del usuario, los controles parentales, etc. |

Los términos adicionales utilizados en el flujo se definen en el [Glosario](/help/authentication/kickstart/glossary.md).

El diagrama siguiente ilustra todo el flujo:

![Guía de API de REST V2 (de servidor a servidor)](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### Pasos para implementar la API de REST V2 en la arquitectura servidor a servidor {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

Para implementar la API de REST de Adobe Pass V2, debe seguir los pasos a continuación agrupados en fases.

## 0. Requisitos previos {#prerequisites}

En implementaciones de servidor a servidor, el servicio de programador y aplicación de streaming debe establecer un protocolo para que el servicio de programador pueda:

* Identifique de forma exclusiva la aplicación de streaming en el dispositivo.
* Actuar en nombre de la aplicación de streaming y comunicarse con el servicio de Adobe Pass.
* Recopile y almacene información sobre la aplicación de streaming y el dispositivo, como la dirección IP, el puerto de origen y la información del dispositivo, para pasarla a Adobe Pass.
* Devuelva las decisiones y las instrucciones a la aplicación de streaming.

Los parámetros como ID de dispositivo, ID de cliente y secreto de cliente (definidos a continuación) se pueden almacenar en la aplicación de streaming o en el servicio de programador.

## A. Fase de registro {#registration-phase}

### Paso 1: Registro de la aplicación {#step-1-register-your-application}

Para que la aplicación pueda llamar a la API de REST de Adobe Pass V2, necesita un token de acceso requerido por la capa de seguridad de la API.

Para implementaciones de servidor a servidor, el servicio de programador puede registrarse en nombre de una instancia de aplicación, pero es necesario obtener los valores de credenciales de cliente (ID de cliente y secreto de cliente) para cada dispositivo de flujo continuo.

Para obtener el token de acceso, el servicio de programación puede actuar en nombre de una aplicación de streaming y debe seguir los pasos descritos en la [documentación de registro dinámico de cliente](../../rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## B. Fase de autenticación {#authentication-phase}

### Paso 2: Compruebe si hay perfiles autenticados existentes {#step-2-check-for-existing-authenticated-profiles}

El servicio de programador comprueba, en nombre de la aplicación de streaming, los perfiles autenticados existentes: `/api/v2/{serviceProvider}/profiles` ([Recuperar perfiles autenticados](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)).

* Si no se encuentra ningún perfil, la aplicación de streaming implementa un flujo TempPass
   * Siga la documentación sobre cómo implementar [Flujos de acceso temporales](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Si no se encuentra ningún perfil, la aplicación de streaming implementa un flujo de autenticación
   * <b>Paso 2.a:</b> el Servicio de programador recupera la lista de MVPD disponibles para serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Recuperar lista de MVPD disponibles](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * El Servicio de programador puede implementar el filtrado en la lista de MVPD y mostrar solo las MVPD previstas mientras oculta otras (TempPass, prueba de MVPD, MVPD en desarrollo, etc.)
   * El servicio de programación debe devolver una lista de MVPD filtrada para que la aplicación de streaming muestre el selector, el usuario selecciona la MVPD
   * Con la MVPD seleccionada en la aplicación de streaming, el servicio de programador crea una sesión: <b>/api/v2/{serviceProvider}/session</b><br>
([Crear sesión de autenticación](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * Se devuelve un CÓDIGO y una URL que se utilizarán para la autenticación
      * Si se encuentra un perfil, el servicio de programador puede continuar con <a href="#preauthorization-phase">C. Fase de preautorización</a>
   * El servicio de programación debe devolver el CÓDIGO y la URL a la aplicación de streaming.

### Paso 3: Autenticar al usuario {#step-3-authenticate-the-user}

Uso de un explorador o una aplicación basada en Web de Second Screen:

* Opción 1. La aplicación de streaming puede abrir un navegador o una vista web, cargar la URL para autenticarse y el usuario aterriza en la página de inicio de sesión de MVPD donde deben enviarse las credenciales
   * El usuario introduce el inicio de sesión/contraseña, la redirección final muestra una página de éxito
* Opción 2. La aplicación de streaming no puede abrir un navegador y solo muestra el CÓDIGO. <b>Es necesario desarrollar una aplicación web independiente, AuthN_APP</b> para pedir al usuario que introduzca el CÓDIGO, cree y abra la URL: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * El usuario introduce el inicio de sesión/contraseña, la redirección final muestra una página de éxito

### Paso 4: Comprobación de perfiles autenticados {#step-4-check-for-authenticated-profiles}

El servicio de programación comprueba la autenticación con MVPD para completarla en el explorador o en la segunda pantalla

* Se recomienda sondear cada 15 segundos en <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Recuperar perfiles autenticados para MVPD específicos](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Si la selección de MVPD no se realiza en la aplicación de transmisión, ya que el selector de MVPD se presenta en la aplicación de segunda pantalla, el sondeo debe realizarse con CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Recuperar perfiles autenticados para CÓDIGO específico](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* El sondeo no debe exceder los 30 minutos, en caso de que se alcancen los 30 minutos y la aplicación de streaming siga activa, deberá iniciarse una nueva sesión y se devolverá un nuevo CÓDIGO y una URL
* Cuando se completa la autenticación, el resultado es 200 con un perfil autenticado
* El Servicio de programador puede continuar con <a href="#preauthorization-phase">C. Fase de preautorización</a>

## C. Fase previa a la autorización {#preauthorization-phase}

### Paso 5: buscar recursos autorizados previamente {#step-5-check-for-preauthorized-resources}

Con un perfil de autenticación válido para un usuario, el servicio de programador tiene la posibilidad de comprobar el acceso a los vídeos disponibles y pasar la lista a la aplicación de streaming para que la muestre.

* El paso es opcional y se ejecuta si la aplicación desea filtrar los recursos no disponibles en el paquete de usuario autenticado
* Llamada a <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
([Recuperar la decisión de preautorización utilizando MVPD específico](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))

## D. Fase de autorización {#authorization-phase}

### Paso 6: buscar recursos autorizados {#step-6-check-for-authorized-resources}

La aplicación de streaming se prepara para reproducir un vídeo, recurso o recurso seleccionado por el usuario.

* El paso es necesario para cada inicio de reproducción
* La aplicación de streaming pasa esta información al servicio de programación
* El servicio de programador en nombre de la aplicación de streaming, llama a <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Recuperar la decisión de autorización utilizando MVPD específico](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * decision = &#39;Permit&#39;, el servicio de programador ordena a la aplicación de streaming que inicie el streaming
   * decision = &#39;Deny&#39;, el servicio de programador ordena a la aplicación de streaming que informe al usuario de que no tiene acceso a ese vídeo
   * en el proceso, el Servicio de programador puede evaluar otras reglas comerciales y devolver la decisión adecuada a la Aplicación de streaming

## E. Fase de cierre {#logout-phase}

### Paso 7: Cerrar sesión {#step-7-logout}

Aplicación de streaming: el usuario quiere cerrar la sesión de la MVPD

* La aplicación de streaming informa al servicio de programación de que debe cerrar la sesión de la MVPD para esta aplicación específica.
* El Servicio de programador puede limpiar la información que almacena sobre el usuario autenticado
* La llamada del servicio de programador <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Iniciar el cierre de sesión de una MVPD específica](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* Si la respuesta actionType=&#39;interactive&#39; y la URL están presentes, el servicio de programador devolverá a la aplicación de streaming la URL
* En función de las capacidades existentes, la aplicación de streaming puede abrir la dirección URL en el explorador (normalmente la misma que se utiliza para la autenticación)
* Si la aplicación de streaming no tiene explorador, o si es una instancia diferente a la de la autenticación, el flujo se puede detener, ya que la sesión de MVPD no persistió en la caché del explorador.

## Entornos y requisitos funcionales{#environments}

Un programador debe crear al menos dos entornos: uno para la producción y uno o más para el ensayo.

### Producción

El entorno de producción debe tener una alta disponibilidad y escalarse adecuadamente para picos grandes o inesperados (por ejemplo, deportes en directo, noticias de última hora).

El servicio de Adobe Pass se ejecuta en varios centros de datos distribuidos geográficamente en Estados Unidos. Para lograr el mejor tiempo de respuesta (es decir, la menor latencia) desde el servicio de Adobe Pass, el programador también debe crear una infraestructura de servicio dispersa geográficamente similar.

El servicio de programación debe limitar la memoria caché DNS a un máximo de 30 segundos en caso de que el Adobe necesite redireccionar el tráfico. Esto puede ocurrir si un centro de datos deja de estar disponible.

El programador debe proporcionar el rango de IP pública del entorno de producción. Estas se incluirán en una lista de direcciones IP permitidas en la infraestructura de Adobe Pass para su acceso y se administrarán mediante las políticas de uso de API equitativas de Adobe.

### Ensayo

El entorno de ensayo puede ser mínimo, pero debe incluir todos los componentes del sistema y la lógica empresarial. Debe funcionar de manera similar a la producción y permitir versiones de prueba fuera de la producción. Lo ideal es que el entorno de ensayo se pueda conectar a los entornos de prueba de Adobe Pass para que el programador lo utilice y que el Adobe lo proporcione cuando sea necesario, de modo que podamos ayudarle en las pruebas y la resolución de problemas.

### Requisitos funcionales

El servicio de programación debe pasar información precisa de identificación del dispositivo para el que está ejecutando los flujos. Además, el servicio Programador debe pasar la IP del dispositivo para el que está ejecutando los flujos (en un encabezado x-forwarded-for) junto con el puerto de origen de la conexión (en el campo de información del dispositivo):

El servicio Programador debe enviar los datos y el formato requeridos por las MVPD individuales o aplicaciones integradas (por ejemplo, IP del dispositivo, puerto de origen, información del dispositivo, MRSS, datos opcionales como ECID). <!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->.

El servicio de programador debe respetar el perfil de autenticación y la validez de las decisiones al almacenar en caché e invalidar las autenticaciones o decisiones cuando se le notifique.

El servicio de programador debe mantener los certificados compartidos con el Adobe (para metadatos de usuario cifrados).

## Información relacionada {#related}

* [Referencia de la API de REST 2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
