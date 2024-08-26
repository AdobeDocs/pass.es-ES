---
title: Referencia de API de REST
description: Referencia de API de REST
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 2%

---

# Referencia de API de REST {#rest-api-reference}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Mecanismo de limitación

La API de REST de autenticación de Adobe Pass se rige por un [mecanismo de restricción](/help/authentication/throttling-mechanism.md).

## Formatos de respuesta {#response-formats}


>[!NOTE]
>
> Las API proporcionadas en estos servicios pueden devolver respuestas en XML o JSON (para API que devuelven una respuesta). Existen tres formas diferentes de especificar el formato de respuesta en la solicitud:
>
>* Establezca el encabezado Aceptar HTTP en `application/xml` o `application/json`.
>* En la carga útil de la solicitud, especifique el parámetro `format=xml` o `format=json`.
>* Llame al extremo del servicio web con la extensión `.xml` o `.json`. Por ejemplo, `/regcode.xml` o `/regcode.json`
>
>Puede especificar cualquiera de los métodos anteriores. La especificación de varios métodos con formatos conflictivos puede dar lugar a errores o a resultados no deseados.

## Extremos de API de REST {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Resumen de servicios web {#web_srvs_summary}

En la tabla siguiente se enumeran los servicios web disponibles para el enfoque sin cliente. Haga clic en los extremos de los servicios web para obtener más información (solicitud y respuesta de ejemplo, parámetros de entrada, métodos HTTP, etc.).


| Sr | Punto final de servicio web | Descripción | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | Alojado en | Llamado por |
| --- | --- | --- | --- | --- | --- |
| 1. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode](/help/authentication/registration-code-request.md) | Devuelve el código de registro generado aleatoriamente y el URI de la página de inicio de sesión | 2 | Adobe </br>Servicio de código regular | Smart Device |
| 2. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/return-registration-record.md) | Devuelve el registro del código de registro que contiene el UUID del código de registro, el código de registro y el ID del dispositivo con hash | 8 | Adobe </br>Servicio de código regular | Adobe Pass Authentication |
| 3. | [&lt;SP_FQDN>/api/v1/config/ </br> {requestorId}](/help/authentication/provide-mvpd-list.md) | Devuelve la lista de MVPD configuradas para el solicitante | 5 | Adobe </br>Servicio de autenticación </br>Adobe Pass </br>de | Iniciar sesión en la aplicación </br>Web </br> |
| 4. | [&lt;SP_FQDN>/api/v1/authentication](/help/authentication/initiate-authentication.md) | Inicia el proceso AuthN al informar al evento de selección de MVPD. Crea un registro en la base de datos de autenticación, que se concilia cuando se recibe una respuesta correcta de MVPD (Paso 13) | 7 | Adobe </br>Servicio de autenticación </br>Adobe Pass </br>de | Iniciar sesión en la aplicación </br>Web </br> |
| 5. | Consumidor de afirmación de SAML | Flujo de trabajo SAML existente entre Adobe Pass Authentication y MVPD | 13 | Servicio </br>de autenticación </br>de Adobe Pass | Adobe Pass Authentication |
| 6. | [&lt;SP_FQDN>/api/v1/checkauthn/ </br> {registrationCode}](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | La aplicación web de inicio de sesión puede comprobar si el flujo de inicio de sesión intentado se ha realizado correctamente |     | Autenticación de Adobe Pass </br>   </br>Servicio | Iniciar sesión   </br>Web   </br>Aplicación |
| 7. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Obtiene metadatos relacionados con el token de AuthN | 15 | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |
| 8. | [&lt;REGGIE_FQDN>/reggie/v1/ </br> {requestorId}/regcode/ </br> {registrationCode}](/help/authentication/delete-registration-record.md) | Elimina el registro de código reg y libera el código reg para su reutilización | 16 | Adobe </br>Servicio de código regular | Adobe Pass Authentication |
| 9. | [&lt;SP_FQDN>/api/v1/authorize](/help/authentication/initiate-authorization.md) | Obtiene una respuesta de autorización. | 17 | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |
| 10. | [&lt;SP_FQDN>/api/v1/checkauthn](/help/authentication/check-authentication-token.md) | Indica si el dispositivo tiene un token de AuthN no caducado. |     | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |
| 11. | [&lt;SP_FQDN>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | Devuelve el token de AuthN si se encuentra. |     | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |
| 12. | [&lt;SP_FQDN>/api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | Devuelve el token de AuthZ si se encuentra. |     | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |
| 13. | [&lt;SP_FQDN>/api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | Devuelve el token de medio corto si se encuentra, igual que /api/v1/mediatoken |     | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |
| 14. | [&lt;SP_FQDN>/api/v1/mediatoken](/help/authentication/obtain-short-media-token.md) | Obtiene el token de medios corto |     | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |
| 15. | [&lt;SP_FQDN>/api/v1/preauthorize](/help/authentication/retrieve-list-of-preauthorized-resources.md) | Recupera la lista de recursos preautorizados |     | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |
| 16. | [&lt;SP_FQDN>/api/v1/preauthorize/{code}](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | Recupera la lista de recursos preautorizados |     | Servicio </br>de autenticación </br>de Adobe Pass | Iniciar sesión en aplicación web |
| 17. | [&lt;SP_FQDN>/api/v1/logout](/help/authentication/initiate-logout.md) | Quitar los tokens AuthN y AuthZ del almacenamiento |     | Autenticación de Adobe Pass </br>   </br>Servicio | Smart Device |
| 18. | [&lt;SP_FQDN>/api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | Obtiene metadatos de usuario una vez completado el flujo de autenticación | N/D | N/D | Smart Device |
| 19. | [&lt;SP_FQDN>/api/v1/authenticate/freepreview](/help/authentication/free-preview-for-temp-pass-and-promotional-temp-pass.md) | Crear un token de autenticación para Pase temporal o Pase temporal promocional | N/D | Servicio </br>de autenticación </br>de Adobe Pass | Smart Device |


## Seguridad de API de REST {#security}

Todas las API de REST de autenticación de Adobe Pass deben llamarse usando el protocolo HTTPS para una comunicación segura. Además, la mayoría de las API a las que se llama deben contener un token de acceso obtenido tal como se describe en la documentación de la API [Recuperar token de acceso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
