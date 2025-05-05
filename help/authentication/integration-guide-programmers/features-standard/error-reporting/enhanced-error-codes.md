---
title: Códigos de error mejorados
description: Códigos de error mejorados
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 27aaa0d3351577e60970a4035b02d814f0a17e2f
workflow-type: tm+mt
source-wordcount: '2649'
ht-degree: 3%

---

# Códigos de error mejorados {#enhanced-error-codes}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Los códigos de error mejorados representan una función de autenticación de Adobe Pass que proporciona información de error adicional a las aplicaciones cliente integradas con:

* API de REST de autenticación de Adobe Pass:
   * [API de REST v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [API de REST (heredada) v1](../../legacy/rest-api-v1/rest-api-overview.md)
* API de preautorización de SDK de autenticación de Adobe Pass:
   * [(Heredado) JavaScript SDK (API de preautorización)](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [(Heredado) iOS/tvOS SDK (API de preautorización)](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [(Heredado) Android SDK (API de preautorización)](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _(*) La API de preautorización es la única API de SDK de autenticación de Adobe Pass que admite códigos de error mejorados._

>[!IMPORTANT]
>
> Las aplicaciones que integran la API de REST de autenticación de Adobe Pass v2 se beneficiarán de los códigos de error mejorados de forma predeterminada sin requerir una configuración adicional.
>
> <br/>
>
> Las aplicaciones que integran la API de REST de autenticación de Adobe Pass v1 o la API de preautorización de SDK solo pueden beneficiarse de los códigos de error mejorados si la función está habilitada explícitamente.
>
> <br/>
>
> Para habilitar explícitamente esta función, crea un ticket a través de nuestro [Zendesk](https://adobeprimetime.zendesk.com) y pídele ayuda a tu administrador técnico de cuentas (TAM).

## Representación {#enhanced-error-codes-representation}

Los códigos de error mejorados se pueden representar en formato `JSON` o `XML` según la API de autenticación de Adobe Pass integrada y el valor de encabezado &quot;Accept&quot; utilizado (es decir, `application/json` o `application/xml`):

| API de autenticación de Adobe Pass | JSON | XML |
|-------------------------------|---------|---------|
| API de REST v2 | &check; |         |
| API de REST v1 | &check; | &check; |
| API de preautorización de SDK | &check; |         |

>[!IMPORTANT]
>
> La autenticación de Adobe Pass puede comunicar códigos de error mejorados a las aplicaciones cliente de dos formas:
>
> <br/>
>
> * **Información de error de nivel superior**: En este caso, el objeto ***&quot;error&quot;*** se encuentra en el nivel superior, por lo que el cuerpo de la respuesta solo puede contener el objeto ***&quot;error&quot;***.
> * **Información de error en el nivel de elemento**: En este caso, el objeto ***&quot;error&quot;*** se encuentra en el nivel de elemento, por lo que el cuerpo de la respuesta puede contener un objeto ***&quot;error&quot;*** para todos los elementos que experimentaron un error durante el servicio.
>
> <br/>
>
> Consulte la documentación pública de cada API de autenticación de Adobe Pass integrada para determinar los detalles específicos de la representación de los códigos de error mejorados.

**API de REST v2**

Consulte las siguientes respuestas HTTP que contienen ejemplos de códigos de error mejorados representados como `JSON` aplicables a la API de REST v2.

>[!BEGINTABS]

>[!TAB API de REST v2 - Información de error de nivel de elemento (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB API de REST v2 - Información de error de nivel superior (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!ENDTABS]

**API de REST v1**

Consulte las siguientes respuestas HTTP que contienen ejemplos de códigos de error mejorados representados como `JSON` o `XML` aplicables para la API de REST v1.

>[!BEGINTABS]

>[!TAB API de REST v1 - Información de error de nivel de elemento (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB API de REST v1 - Información de error de nivel superior (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB API de REST v1 - Información de error de nivel superior (XML)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!ENDTABS]

### Estructura {#enhanced-error-codes-representation-structure}

Los códigos de error mejorados incluyen los siguientes `JSON` campos o `XML` atributos con ejemplos:

| Nombre | Tipo | Ejemplo | Restringido | Descripción |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *acción* | *cadena* | *ninguno* | &check; | La autenticación de Adobe Pass recomendó una acción que podría solucionar la situación tal como se define en este documento. <br/><br/> Para obtener más información, consulte la sección [Acción](#enhanced-error-codes-action). |
| *estado* | *entero* | *403* | &check; | El código de estado de respuesta HTTP tal como se define en el documento [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6). <br/><br/> Para obtener más información, consulte la sección [Estado](#enhanced-error-codes-status). |
| *código* | *cadena* | *authorization_denied_by_mvpd* | &check; | El código de identificador único de autenticación de Adobe Pass asociado con el error tal como se define en este documento. <br/><br/> Para obtener más información, consulte la sección [Código](#enhanced-error-codes-code). |
| *mensaje* | *cadena* | *El MVPD ha devuelto una decisión &quot;Denegar&quot; al solicitar autorización para el recurso especificado* |            | El mensaje en lenguaje natural que se puede mostrar al usuario final en algunos casos. <br/><br/> Para obtener más información, consulte la sección [Gestión de respuestas](#enhanced-error-codes-response-handling). |
| *detalles* | *cadena* | *El paquete de suscripción no incluye el canal &quot;Activo&quot;* |            | El mensaje detallado que un socio de servicios podría proporcionar en algunos casos, <br/><br/> Este campo podría no estar presente en caso de que el socio de servicios no proporcione ningún mensaje personalizado. |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | La URL de documentación pública de autenticación de Adobe Pass que se vincula a más información sobre por qué se produjo este error y posibles soluciones. <br/><br/> Este campo contiene una dirección URL absoluta y no debería inferirse del código de error, dependiendo del contexto de error en que se pueda proporcionar una dirección URL diferente. |
| *seguimiento* | *cadena* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | El identificador único de la respuesta que se puede utilizar al ponerse en contacto con el soporte de autenticación de Adobe Pass para solucionar problemas específicos. |

>[!IMPORTANT]
>
> La columna **Restringido** indica si el campo respectivo contiene un valor de un conjunto finito, mientras que los campos no restringidos pueden contener datos.
>
> <br/>
>
> Las futuras actualizaciones de este documento podrían agregar valores a los conjuntos finitos, pero no quitarán ni cambiarán los existentes.

### Acción {#enhanced-error-codes-representation-action}

Los códigos de error mejorados incluyen un campo &quot;acción&quot; que proporciona una acción recomendada que podría remediar la situación.

Los valores posibles del campo &quot;acción&quot; incluyen:

| Acción | Descripción | Categoría |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| ninguno | No hay ninguna acción predefinida para solucionar este problema, pero en algunos casos esto puede indicar una invocación incorrecta de la API. | Corrija el contexto de la solicitud. |
| configuración | La aplicación cliente requiere un cambio de configuración, la mayoría de las veces realizado a través de Adobe Pass TVE Dashboard. | Corrija el contexto de configuración de la integración. |
| application-registration | La aplicación cliente requiere registrarse de nuevo. | Corrija el contexto de la aplicación cliente. |
| authentication | La aplicación cliente requiere autenticar o volver a autenticar al usuario. | Corrija el contexto de la aplicación cliente. |
| autorización | La aplicación cliente requiere obtener autorización para el recurso especificado. | Corrija el contexto de la aplicación cliente. |
| volver a intentar | La aplicación cliente requiere que vuelva a intentar la solicitud. | Corrija el contexto de la solicitud. |

_(*) Para algunos errores, varias acciones podrían ser posibles soluciones, pero el campo &quot;acción&quot; indica la que tiene la mayor probabilidad de corregir el error._

### Estado {#enhanced-error-codes-representation-status}

Los códigos de error mejorados incluyen un campo &quot;estado&quot; que indica el código de estado HTTP asociado al error.

Los valores posibles del campo &quot;estado&quot; incluyen:

| Código | Motivo-Frase |
|------|-----------------------|
| 400 | Solicitud incorrecta |
| 401 | No autorizado |
| 403 | Prohibido |
| 404 | No encontrado |
| 405 | Método no permitido |
| 410 | Gone |
| 412 | Error de condición previa |
| 500 | Error interno del servidor |

Los códigos de error mejorados con un &quot;estado&quot; 4xx generalmente aparecen cuando el cliente genera el error y la mayoría de las veces implica que el cliente requiere trabajo adicional para corregirlo.

Los códigos de error mejorados con un &quot;estado&quot; 5xx generalmente aparecen cuando el servidor genera el error y la mayoría de las veces implica que el servidor requiere trabajo adicional para corregirlo.

>[!IMPORTANT]
>
> Hay casos en los que el código de estado de respuesta HTTP es diferente del campo &quot;estado&quot; del código de error mejorado, especialmente al interactuar con una API de autenticación de Adobe Pass que comunica los códigos de error mejorados como información de error de nivel de elemento.

### Código {#enhanced-error-codes-representation-code}

Los códigos de error mejorados incluyen un campo &quot;código&quot; que proporciona un identificador único de autenticación de Adobe Pass asociado con el error.

Los valores posibles para el campo &quot;código&quot; se agregan [debajo de](#enhanced-error-codes-list) en dos listas basadas en la API de autenticación de Adobe Pass integrada.

## Listas {#enhanced-error-codes-lists}

### API de REST v2 {#enhanced-error-codes-lists-rest-api-v2}

En la tabla siguiente se enumeran los posibles códigos de error mejorados que una aplicación cliente podría encontrar al integrarse con la API de REST de autenticación de Adobe Pass v2.

| Acción | Código | Estado | Mensaje |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ninguno** | *proveedor_servicio_parámetros_no_válido* | 400 | Falta el valor del parámetro del proveedor de servicios o no es válido. |
|                              | *parámetro_mvpd_no válido* | 400 | Falta el valor del parámetro mvpd o no es válido. |
|                              | *código_parámetro_no_válido* | 400 | Falta el valor del parámetro de código o no es válido. |
|                              | *recursos_parámetro_no_válidos* | 400 | Falta el valor del parámetro de recursos o no es válido. |
|                              | *url_redirect_parameter_invalid_parameter* | 400 | Falta el valor del parámetro de URL de redireccionamiento o no es válido. |
|                              | *socio_parámetro_no_válido* | 400 | Falta el valor del parámetro del socio o no es válido. |
|                              | *invalid_parameter_saml_response* | 400 | Falta el valor del parámetro de respuesta SAML o no es válido. |
|                              | *información_dispositivo_encabezado_no válida* | 400 | Falta el valor del encabezado de información del dispositivo o no es válido. |
|                              | *identificador_dispositivo_encabezado_no válido* | 400 | Falta el valor del encabezado del identificador del dispositivo o no es válido. |
|                              | *identidad_de_encabezado_no_válida_para_acceso_temporal* | 400 | Falta la identidad del valor del encabezado de acceso temporal o no es válida. |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | El valor de estado de acceso de permiso del encabezado de estado del marco de trabajo del socio no está presente. |
|                              | *acceso_permiso_pfs_header_invalid_not_defined* | 400 | El valor del estado de acceso de permiso del encabezado de estado del marco de trabajo del socio es indeterminado. |
|                              | *no válido_header_pfs_permission_access_not_granted* | 400 | No se ha concedido el valor de estado de acceso de permiso del encabezado de estado del marco de trabajo del socio. |
|                              | *id_de_proveedor_pfs_header_invalid_not_defined* | 400 | El valor de ID del proveedor del encabezado de estado del marco de socios no está asociado a un mvpd conocido. |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | El valor de id del proveedor del encabezado de estado del marco de socios no coincide con el mvpd enviado como parámetro. |
|                              | *invalid_header_pfs_provider_info_expire* | 400 | La información del proveedor del encabezado de estado del marco de trabajo del socio ha caducado. |
|                              | *integración_no_válida* | 400 | La integración entre el proveedor de servicios especificado y mvpd no existe o está deshabilitada. |
|                              | *sesión_autenticación_no_válida* | 400 | Falta la sesión de autenticación asociada con esta solicitud o no es válida. |
|                              | *preauthorization_denied_by_mvpd* | 403 | MVPD ha devuelto una decisión &quot;Denegar&quot; al solicitar la preautorización del recurso especificado. |
|                              | *authorization_denied_by_mvpd* | 403 | MVPD ha devuelto una decisión &quot;Denegar&quot; al solicitar autorización para el recurso especificado. |
|                              | *authorization_denied_by_parent_controls* | 403 | La MVPD ha devuelto una decisión &quot;Denegar&quot; debido a la configuración del control parental del recurso especificado. |
|                              | *regla_de_degradación_denegada_de_autorización* | 403 | La integración entre el proveedor de servicios especificado y mvpd tiene aplicada una regla de degradación que deniega la autorización de los recursos solicitados. |
|                              | *error_de_servidor_interno* | 500 | La solicitud falló debido a un error interno del servidor. |
| **configuración** | *demasiados_recursos* | 403 | Error en la solicitud de autorización o preautorización porque se consultaron demasiados recursos. Póngase en contacto con el equipo de soporte para configurar correctamente las limitaciones de autorización y preautorización. |
|                              | *certificado_metadatos_usuario_configuración_no_válido* | 500 | Falta la configuración del certificado de metadatos de usuario o no es válida. |
|                              | *acceso_temporal_configuración_no válido* | 500 | La configuración de acceso temporal no es válida. |
|                              | *plataforma_configuración_no_válida* | 500 | Falta la configuración de la plataforma o no es válida para la integración. |
|                              | *id_plataforma_configuración_no_válido* | 500 | Falta la configuración del ID de plataforma o no es válida. |
|                              | *rasgo_plataforma_configuración_no_válido* | 500 | Falta la configuración de la característica de plataforma o no es válida. |
|                              | *rasgo_category_platform_configuration_invalid* | 500 | Falta la configuración de rasgos de la categoría de plataforma o no es válida. |
|                              | *invalid_configuration_platform_services* | 500 | Falta la configuración de los servicios de plataforma o no es válida para la integración. |
|                              | *plataforma_mvpd_configuration_invalid* | 500 | Falta la configuración de la plataforma mvpd o no es válida para mvpd y platform. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | Falta la configuración del estado de carga de la plataforma mvpd o no es válida para mvpd y platform. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | Falta la configuración de intercambio de perfiles de plataforma de mvpd o no es válida para mvpd y platform. |
| **registro de aplicación** | *proveedor_servicio_token_acceso_no_válido* | 401 | El token de acceso no es válido debido a un proveedor de servicios no válido. |
|                              | *aplicación_cliente_token_acceso_no válida* | 401 | El token de acceso no es válido debido a una aplicación de cliente no válida. |
| **autenticación** | *perfil_autenticado_ausente* | 403 | Falta el perfil autenticado asociado a esta solicitud. |
|                              | *perfil_autenticado_caducado* | 403 | El perfil autenticado asociado con esta solicitud ha caducado. |
|                              | *perfil_autenticado_invalidado* | 403 | El perfil autenticado asociado con esta solicitud se ha invalidado. |
|                              | *se ha excedido el límite de duración_acceso_temporal* | 403 | Se ha superado el límite temporal de duración del acceso. |
|                              | *recursos_acceso_temporal_excedido_límite* | 403 | Se ha superado el límite temporal de recursos de acceso. |
|                              | *autorización_denegada_por_hba_policies* | 403 | MVPD ha devuelto una decisión &quot;Denegar&quot; debido a las políticas de autenticación basadas en el hogar. La autenticación actual se obtuvo mediante un flujo de autenticación basado en Inicio y, pero el dispositivo ya no está en Inicio al solicitar autorización para el recurso especificado. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
|                              | *autorización_denegada_por_sesión_invalidada* | 403 | El proveedor de identidad invalidó la sesión de autenticación. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
|                              | *identidad_no_reconocida_por_mvpd* | 403 | Error en la solicitud de autorización debido a que MVPD no reconoció la identidad del usuario. |
| **reintentar** | *error_recibido_de_red* | 403 | Error de lectura al recuperar la respuesta del servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse. |
|                              | *tiempo de espera_de_conexión_de_red* | 403 | Se agotó el tiempo de espera de conexión con el servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse. |
|                              | *tiempo_de_ejecución_máximo_excedido* | 403 | La solicitud no se completó en el tiempo máximo permitido. Si vuelve a intentar la solicitud, el problema podría resolverse. |

### API de REST v1 {#enhanced-error-codes-lists-rest-api-v1}

En la tabla siguiente se enumeran los posibles códigos de error mejorados que una aplicación cliente podría encontrar al integrarse con la API de REST de autenticación de Adobe Pass v1.

| Acción | Código | Estado | Mensaje |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ninguno** | *solicitante no válido* | 400 | Falta el parámetro del solicitante o no es válido. |
|                    | *información_de_dispositivo_no válida* | 400 | Falta la información del dispositivo o no es válida. |
|                    | *id_dispositivo_no válido* | 400 | Falta el identificador del dispositivo o no es válido. |
|                    | *falta_recurso* | 400, 412 | Falta el parámetro de recurso. |
|                    | *solicitud_Authz_incorrecta* | 400, 412 | La solicitud de autorización es nula o no es válida. |
|                    | *preauthorization_denied_by_mvpd* | 403 | MVPD ha devuelto una decisión &quot;Denegar&quot; al solicitar la preautorización del recurso especificado. |
|                    | *authorization_denied_by_mvpd* | 403 | MVPD ha devuelto una decisión &quot;Denegar&quot; al solicitar autorización para el recurso especificado. |
|                    | *authorization_denied_by_parent_controls* | 403 | La MVPD ha devuelto una decisión &quot;Denegar&quot; debido a la configuración del control parental del recurso especificado. |
|                    | *error_interno* | 400, 405, 500 | La solicitud falló debido a un error interno del servidor. |
| **configuración** | *integración_desconocida* | 400, 412 | La integración entre el programador especificado y el proveedor de identidad no existe. Utilice el Tablero de TVE para crear la integración necesaria. |
|                    | *demasiados_recursos* | 403 | Error en la solicitud de autorización o preautorización porque se consultaron demasiados recursos. Póngase en contacto con el equipo de soporte para configurar correctamente las limitaciones de autorización y preautorización. |
| **autenticación** | *authentication_session_Issuer_mismatch* | 400 | Error en la solicitud de autorización debido a que el MVPD indicado para el flujo de autorización es diferente del que emitió la sesión de autenticación. El usuario debe volver a autenticarse con el MVPD deseado para continuar. |
|                    | *autorización_denegada_por_hba_policies* | 403 | MVPD ha devuelto una decisión &quot;Denegar&quot; debido a las políticas de autenticación basadas en el hogar. La autenticación actual se obtuvo mediante un flujo de autenticación basado en el hogar (HBA), pero el dispositivo ya no está en casa al solicitar autorización para el recurso especificado. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
|                    | *autorización_denegada_por_sesión_invalidada* | 403 | El proveedor de identidad invalidó la sesión de autenticación. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
|                    | *identidad_no_reconocida_por_mvpd* | 403 | Error en la solicitud de autorización debido a que MVPD no reconoció la identidad del usuario. |
|                    | *authentication_session_invalidated* | 403 | El proveedor de identidad invalidó la sesión de autenticación. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
|                    | *authentication_session_missing* | 403, 412 | No se pudo recuperar la sesión de autenticación asociada con esta solicitud. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
|                    | *authentication_session_expire* | 403, 412 | La sesión de autenticación actual ha caducado. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
|                    | *falta_sesión_autenticación_autorización_previa* | 412 | No se pudo recuperar la sesión de autenticación asociada con esta solicitud. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
|                    | *preauthorization_authentication_session_expire* | 412 | La sesión de autenticación actual ha caducado. Para continuar, el usuario debe volver a autenticarse con un MVPD compatible. |
| **autorización** | *authorization_not_found* | 403, 404 | No se ha encontrado ninguna autorización para el recurso especificado. El usuario debe obtener una nueva autorización para continuar. |
|                    | *authorization_expire* | 410 | La autorización previa para el recurso especificado ha caducado. El usuario debe obtener una nueva autorización para continuar. |
| **reintentar** | *error_recibido_de_red* | 403 | Error de lectura al recuperar la respuesta del servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse. |
|                    | *tiempo de espera_de_conexión_de_red* | 403 | Se agotó el tiempo de espera de conexión con el servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse. |
|                    | *tiempo_de_ejecución_máximo_excedido* | 403 | La solicitud no se completó en el tiempo máximo permitido. Si vuelve a intentar la solicitud, el problema podría resolverse. |

### API de preautorización de SDK {#enhanced-error-codes-lists-sdks-preauthorize-api}

Consulte la [sección](#enhanced-error-codes-list-rest-api-v1) anterior para ver los posibles códigos de error mejorados que podría encontrar una aplicación cliente al integrarse con la API de autorización previa del SDK de autenticación de Adobe Pass.

## Gestión de respuestas {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Existen códigos de error mejorados que se pueden gestionar automáticamente en el código de la aplicación cliente, como reintentar una solicitud de autorización en caso de tiempo de espera de red o requerir que el usuario se vuelva a autenticar cuando su sesión haya caducado, pero otros tipos pueden requerir cambios de configuración o la interacción del equipo de atención al cliente de autenticación de Adobe Pass.
>
> <br/>
>
> Por lo tanto, es importante recopilar y proporcionar información de error completa al crear un ticket a través de nuestro [Zendesk](https://adobeprimetime.zendesk.com), para asegurarnos de que se realicen los cambios necesarios antes de iniciar la nueva aplicación o función.

En resumen, al administrar respuestas que contengan códigos de error mejorados, debe tener en cuenta lo siguiente:

1. **Comprobar ambos valores de estado**: Compruebe siempre el código de estado de respuesta HTTP y el campo &quot;estado&quot; del código de error mejorado. Pueden diferir y ambos proporcionan información valiosa.

1. **Información de error de nivel superior frente a nivel de elemento**: administre la información de error de nivel superior y de nivel de elemento independientemente de la forma en que se comunique, asegúrese de que puede gestionar ambas formas de transmisión de códigos de error mejorados.

1. **Lógica de reintento**: En el caso de errores que requieran un reintento, asegúrese de que los reintentos se realicen con retroceso exponencial para evitar sobrecargar el servidor. Además, en el caso de las API de autenticación de Adobe Pass que administran varios elementos a la vez (por ejemplo, la API de preautorización), debe incluir en la solicitud repetida solo los elementos marcados con &quot;reintentar&quot; y no la lista completa.

1. **Cambios de configuración**: en el caso de errores que requieran cambios de configuración, asegúrese de realizar los cambios necesarios antes de iniciar la nueva aplicación o característica.

1. **Autenticación y autorización**: Para los errores relacionados con la autenticación y la autorización, debe pedir al usuario que vuelva a autenticarse u obtenga la nueva autorización según sea necesario.

1. **Comentarios del usuario**: como opción, use los campos de &quot;mensaje&quot; y &quot;detalles&quot; (potenciales) legibles en lenguaje natural para informar al usuario sobre el problema. El mensaje de texto &quot;detalles&quot; puede pasarse desde los extremos de preautorización o autorización de MVPD o desde el Programador al aplicar reglas de degradación.
