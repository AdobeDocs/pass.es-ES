---
title: 'API de REST V2: Guía - Pasos de implementación de cliente/servidor'
description: 'API de REST V2: Guía - Pasos de implementación de cliente/servidor'
source-git-commit: 5ba538bdb13d121ba27005df82d4ae604f912241
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# API de REST V2: Guía - Pasos de implementación de cliente/servidor {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/throttling-mechanism.md).

## Pasos para implementar la API de REST V2 en aplicaciones del lado del cliente {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Para implementar la API de REST de Adobe Pass V2, debe seguir los pasos a continuación agrupados en fases.

## A. Fase de registro {#registration-phase}

### Paso 1: Registro de la aplicación {#step-1-register-your-application}

Para que la aplicación pueda llamar a la API de REST de Adobe Pass V2, necesita un token de acceso requerido por la capa de seguridad de la API.
Para obtener el token de acceso, la aplicación debe seguir los pasos descritos:
[Registro dinámico de clientes](./dynamic-client-registration.md)

## B. Fase de autenticación {#authentication-phase}

### Paso 2: Compruebe si hay perfiles autenticados existentes {#step-2-check-for-existing-authenticated-profiles}

La aplicación de streaming comprueba los perfiles autenticados existentes: <b>/api/v2/{serviceProvider}/profiles</b><br>
([Recuperar perfiles autenticados](./apis/profiles-apis/rest-api-v2-retrieve-authenticated-profiles.md))

* si no se encuentra ningún perfil y la aplicación de streaming implementa un flujo TempPass
   * siga la documentación sobre cómo implementar [flujos de acceso temporales](./temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* si no se encuentra ningún perfil, la aplicación de streaming implementará un flujo de autenticación
   * La aplicación de streaming recupera la lista de MVPD disponibles para serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Recuperar lista de MVPD disponibles](./apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md))
   * La aplicación de streaming puede implementar el filtrado en la lista de MVPD y mostrar solo las MVPD previstas mientras oculta otras (TempPass, prueba de MVPD, MVPD en desarrollo, etc.)
   * Selector de visualización de la aplicación de streaming, el usuario selecciona la MVPD
   * Aplicación de streaming para crear una sesión: <b>/api/v2/{serviceProvider}/session</b><br>
([Crear sesión de autenticación](./apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * se devuelve un CÓDIGO y una URL que se utilizarán para la autenticación
      * si se encuentra un perfil, la aplicación de transmisión puede continuar a <a href="#preauthorization-phase">C. Fase de preautorización</a> o <a href="#authorization-phase">D. Fase de autorización</a>

### Paso 3: Autenticar al usuario {#step-3-authenticate-the-user}

Uso de un explorador o una aplicación basada en Web de Second Screen:

* Opción 1. La aplicación de streaming puede abrir un navegador o una vista web, cargar la URL para autenticarse y el usuario aterriza en la página de inicio de sesión de MVPD donde deben enviarse las credenciales
   * usuario: introducir nombre de usuario y contraseña, redirección final: mostrar una página de éxito
* Opción 2. La aplicación de streaming no puede abrir un explorador y solo muestra el CÓDIGO. <b>Es necesario desarrollar una aplicación web independiente</b> para pedir al usuario que introduzca el CÓDIGO, cree y abra la URL : <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * usuario: introducir nombre de usuario y contraseña, redirección final: mostrar una página de éxito

### Paso 4: Comprobación de perfiles autenticados {#step-4-check-for-authenticated-profiles}

La aplicación de streaming comprueba la autenticación con MVPD para completarla en el explorador o en la segunda pantalla

* se recomienda sondear cada 15 segundos en <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Recuperar perfiles autenticados para MVPD específicos](.apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * si la selección de MVPD no se realiza en la aplicación de transmisión, ya que el selector de MVPD se presenta en la aplicación de segunda pantalla, el sondeo debe realizarse con CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Recuperar perfiles autenticados para CÓDIGO específico](./apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* sondeo no debe exceder los 30 minutos, en caso de que se alcancen los 30 minutos y la Aplicación de streaming aún esté activa, debe iniciarse una nueva sesión y se devolverá un nuevo CÓDIGO y URL
* cuando se completa la autenticación, la devolución es de 200 con perfil autenticado
* la aplicación de transmisión por secuencias puede continuar en <a href="#preauthorization-phase">C. Fase de preautorización</a> o <a href="#authorization-phase">D. Fase de autorización</a>

## C. Fase previa a la autorización {#preauthorization-phase}

### Paso 5: buscar recursos autorizados previamente {#step-5-check-for-preauthorized-resources}

La aplicación de streaming se prepara para mostrar los vídeos disponibles para el usuario autenticado y tiene la posibilidad de comprobar los
acceso a estos recursos.

* Este paso es opcional y se ejecuta si la aplicación desea filtrar los recursos no disponibles en el paquete de usuario autenticado
* llamada a <b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
([Recuperar la decisión de preautorización utilizando MVPD específico](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md))


## D. Fase de autorización {#authorization-phase}

### Paso 6: buscar recursos autorizados {#step-6-check-for-authorized-resources}

La aplicación de streaming se prepara para reproducir un vídeo, recurso o recurso seleccionado por el usuario.

* Este paso es necesario para cada inicio de reproducción
* llamar a <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Recuperar la decisión de autorización utilizando MVPD específico](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md))
   * decision = &#39;Permit&#39; , El dispositivo de streaming inicia el streaming
   * decision = &#39;Denegar&#39;, el dispositivo de streaming informa al usuario de que no tiene acceso a ese vídeo

## E. Fase de cierre {#logout-phase}

### Paso 7: Cerrar sesión {#step-7-logout}

Dispositivo de streaming: el usuario quiere cerrar la sesión de la MVPD

* llamar a <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Iniciar el cierre de sesión de una MVPD específica](.apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md))
* si la respuesta actionType=&#39;interactive&#39; y la URL están presentes, abra la URL en un explorador o segunda pantalla para completar el cierre de sesión con MVPD

