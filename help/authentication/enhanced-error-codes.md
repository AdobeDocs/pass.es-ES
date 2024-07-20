---
title: Códigos de error mejorados
description: Códigos de error mejorados
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 87639ad93d8749ae7b1751cd13a099ccfc2636ac
workflow-type: tm+mt
source-wordcount: '2207'
ht-degree: 2%

---

# Códigos de error mejorados {#enhanced-error-codes}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

Este documento describe la lista de códigos de error de API y la información de error adicional devuelta a las aplicaciones.

Para utilizar códigos de error mejorados en la aplicación de programadores, se debe realizar una solicitud al equipo de asistencia para habilitarla con un cambio de configuración.

## Gestión de errores de respuesta {#response-error-handling}

En la mayoría de los casos, la API de autenticación de Adobe Pass incluye información de error adicional en el cuerpo de respuesta para proporcionar **contexto significativo** para saber por qué se ha producido un error determinado o posibles soluciones para solucionar automáticamente el problema.  *Sin embargo, en algunos casos específicos que implican flujos de autenticación o cierre de sesión, los servicios de autenticación de Adobe Pass podrían devolver una respuesta del HTML o un cuerpo vacío. Consulte la documentación de la API para obtener más información.*

Aunque algunos tipos de errores se pueden gestionar automáticamente (como reintentar una solicitud de autorización en caso de tiempo de espera de red o requerir que el usuario se vuelva a autenticar si su sesión ha caducado), otros tipos pueden requerir cambios de configuración o la interacción del equipo de atención al cliente. Es importante que los programadores recopilen y proporcionen información completa sobre los errores en estos casos.

La API de autenticación de Adobe Pass devuelve códigos de estado HTTP en el rango 400-500 para indicar errores. Cada código de estado HTTP tiene ciertas implicaciones:

- Los códigos de error 4xx implican que el cliente genera el error y que el cliente necesita hacer un trabajo adicional para corregirlo (por ejemplo, obtener un token de acceso antes de invocar las API protegidas o proporcionar cualquier parámetro requerido)

- Los códigos de error 5xx implican que el servidor genera el error y que el servidor necesita hacer un trabajo adicional para corregirlo.

La información de error adicional se incluye en el campo &quot;error&quot; dentro del cuerpo de la respuesta.

<table>
<thead>
  <tr>
    <th>Nombre</th>
    <th>Tipo</th>
    <th>Ejemplo</th>
    <th>Descripción</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>error</td>
    <td><i>objeto</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>Se refiere a objetos de colección o error recopilados al intentar completar la solicitud.</td>
  </tr>
</tbody>
</table>

</br>

Las API de Adobe Pass que administran varios elementos (API de preautorización, etc.) pueden indicar si el procesamiento ha fallado para un elemento en particular y si se ha realizado correctamente para otros elementos mediante información de error de nivel de elemento. En este caso, el objeto ***&quot;error&quot;*** se encuentra en el nivel de elemento y el cuerpo de respuesta puede contener varios objetos ***&quot;errores&quot;***; consulte la documentación de la API.

</br>

**Ejemplo con éxito parcial y error de elemento**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

Cada objeto de error tiene los parámetros siguientes:

| Nombre | Tipo | Ejemplo | Restringido | Descripción |
|---|---|----|:---:|---|
| *estado* | *entero* | *403* | &amp;check; | El código de estado HTTP de respuesta tal como se documenta en RFC 7231 (<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400 Solicitud incorrecta</li><li>401 No autorizado</li><li>403 Prohibido</li><li>404 No encontrado</li><li>405 Método no permitido</li><li>Conflicto 409</li><li>410 desaparecido</li><li>Error de condición previa 412</li><li>Demasiadas solicitudes</li><li>500 Error de servidor de intervalo</li><li>503 Servicio no disponible</li></ul> |
| *código* | *cadena* | *error_conexión_red* | &amp;check; | El código de error estándar de autenticación de Adobe Pass. A continuación se incluye la lista completa de códigos de error. |
| *mensaje* | *cadena* | *No se puede contactar con los servicios de tu proveedor de TV* | | Mensaje legible en lenguaje natural que se puede mostrar al usuario final. |
| *detalles* | *cadena* | *El paquete de suscripción no incluye el canal &quot;Activo&quot;* | | En algunos casos, los puntos finales de autorización de MVPD o el programador proporcionan un mensaje detallado mediante reglas de degradación. <p> Tenga en cuenta que si no se recibe ningún mensaje personalizado de los servicios de socio, es posible que este campo no esté presente en los campos de error. |
| *helpUrl* | *url* | &quot;`http://`&quot; | | Una dirección URL que se vincula a más información sobre el motivo por el que se produjo este error y las posibles soluciones. <p>El URI representa una dirección URL absoluta y no debe inferirse del código de error. Según el contexto de error, se puede proporcionar una dirección URL diferente. Por ejemplo, el mismo código de error bad_request producirá direcciones URL diferentes para los servicios de autenticación y autorización. |
| *seguimiento* | *cadena* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | Un identificador único para esta respuesta que se puede utilizar al ponerse en contacto con el servicio de asistencia para identificar problemas específicos en situaciones más complejas. |
| *acción* | *cadena* | *reintentar* | &amp;check; | Medidas recomendadas para remediar la situación: <ul><li> *ninguno* - Lamentablemente no hay ninguna acción predefinida para remediar este problema. Esto podría indicar una invocación incorrecta de la API pública</li><li>*configuración*: se necesita un cambio de configuración a través del panel de TVE o poniéndose en contacto con el servicio de asistencia. </li><li>*application-registration*: la aplicación debe registrarse a sí misma de nuevo. </li><li>*autenticación*: el usuario debe autenticarse o volver a autenticarse. </li><li>*autorización*: el usuario debe obtener autorización para el recurso específico. </li><li>*degradación*: se debe aplicar alguna forma de degradación. </li><li>*reintentar*: si se reintenta la solicitud, el problema podría resolverse</li><li>*reintentar después*: si se reintenta la solicitud después del período de tiempo indicado, el problema podría resolverse.</li></ul> |

</br>

**Notas:**

- ***Restringido*** la columna *indica si el valor de campo respectivo representa un conjunto finito* (por ejemplo, los códigos de estado HTTP existentes para el campo &quot;*status*&quot;). Las futuras actualizaciones de esta especificación podrían añadir valores a la lista restringida, pero no eliminarán ni cambiarán los existentes. Los campos sin restricciones generalmente pueden contener datos, pero puede haber limitaciones para garantizar un tamaño razonable.

- Cada respuesta de Adobe contendrá un &quot;Adobe-Request-Id&quot; que identifica la solicitud del cliente a través de nuestros servicios HTTP. El campo &quot;**trace**&quot; complementa eso y deberían notificarse juntos.

## Códigos de estado HTTP y códigos de error {#http-status-codes-and-error-codes}

Las incoherencias entre los distintos códigos de error y sus códigos de estado HTTP asociados se deben a los requisitos de compatibilidad con versiones anteriores de los SDK y las aplicaciones más antiguas ( para
ejemplo *unknown\_application* genera 400 solicitudes incorrectas, mientras que *unknown\_software\_statement* genera 401 (no autorizado). La solución de estas incoherencias se abordará en futuras iteraciones.

## Acciones y códigos de error {#actions-and-error-codes}

Para la mayoría de los códigos de error, podrían elegirse varias acciones como rutas para solucionar el problema o incluso podrían ser necesarias varias acciones para solucionarlo automáticamente. Optamos por indicar el que tenía la mayor probabilidad de corregir el error. Las **acciones** se pueden dividir en tres categorías:

1. aquéllos que intentan corregir el contexto de la solicitud (reintentar, reintentar después)
1. que intentan corregir el contexto de usuario dentro de la aplicación
(solicitud-registro, autenticación, autorización)
1. que intentan corregir el contexto de integración entre una aplicación
y un proveedor de identidad (configuración, degradación)

Para la primera categoría (reintento y reintento después), reintentar la misma solicitud podría ser suficiente para resolver el problema. En casos de API que administran varios elementos, la aplicación debe repetir la solicitud e incluir solo los elementos con la acción &quot;reintentar&quot; o &quot;reintentar después&quot;. Para la acción &quot;*retry-after*&quot;, un encabezado &quot;<u>Retry-After</u>&quot; indicará cuántos segundos debe esperar la aplicación antes de repetir la solicitud.

Para la segunda y tercera categoría, la implementación de la acción real depende en gran medida de las funciones de la aplicación. Por ejemplo, la &quot;*degradación*&quot; se puede implementar como &quot;cambiar a pases temporales de 15 minutos para permitir que los usuarios reproduzcan el contenido&quot; o como &quot;herramienta automática para aplicar la degradación AUTHN-ALL o AUTHZ-ALL para su integración con la MVPD especificada&quot;. Una acción similar a &quot;*authentication*&quot; puede almacenar en déclencheur una autenticación pasiva (autenticación de canal secundario) en una tableta y un flujo de autenticación de pantalla secundaria completo en televisores conectados. Por este motivo, optamos por proporcionar direcciones URL completas con esquema y todos los parámetros.

## Códigos de error {#error-codes}

La siguiente tabla de errores enumera los posibles códigos de error, mensajes asociados y acciones posibles.

| Acción | Código de error | Código de estado HTTP | Descripción |
|---|---|---|---|
| **ninguno** | *authorization_denied_by_mvpd* | 403 | La MVPD ha devuelto una decisión &quot;Denegar&quot; al solicitar autorización para el recurso especificado. |
|  | *authorization_denied_by_parent_controls* | 403 | La MVPD ha devuelto una decisión &quot;Denegar&quot; debido a la configuración del control parental del recurso especificado. |
|  | *autorizacion_denegada_por_programador* | 403 | La regla de degradación aplicada por el programador fuerza una decisión &quot;Denegar&quot; para el usuario actual. |
|  | *bad_request* | 400 | La solicitud de API no es válida o está formada incorrectamente. Revise la documentación de la API para determinar los requisitos de la solicitud. |
|  | *individualization_service_unavailable* | 503 | Error en la solicitud debido a que el servicio de individualización no está disponible. |
|  | *error_interno* | 500 | La solicitud falló debido a un error interno del servidor. |
|  | *tiempo_cliente_no_válido* | 400 | La fecha/hora/zona horaria del equipo cliente no está configurada correctamente. Esto probablemente dará lugar a errores de autenticación/autorización. |
|  | *esquema_personalizado_no válido* | 400 | No se reconoce el esquema personalizado especificado utilizado en el registro de la aplicación. Compruebe la configuración del tablero de TVE para ver los valores de esquema personalizados adecuados. |
|  | *dominio_inválido* | 400 | El solicitante está utilizando un dominio no válido. Todos los dominios utilizados por un ID de solicitante concreto deben incluirse en la lista blanca por Adobe. |
|  | *encabezado_no_válido* | 400 | La solicitud falló porque contenía un encabezado no válido. Revise la documentación de la API para determinar qué encabezados son válidos para su solicitud y si hay alguna limitación para su valor. |
|  | *método_http_no válido* | 405 | No se admite el método HTTP asociado con la solicitud. Consulte la documentación de la API para determinar los métodos HTTP admitidos para su solicitud. |
|  | *valor_parámetro_no_válido* | 400 | Error en la solicitud porque contenía un parámetro o valor de parámetro no válido. Revise la documentación de la API para determinar qué parámetros son válidos para su solicitud y si hay limitaciones para su valor. |
|  | *valor_recurso_no_válido* | 400 | Error en la solicitud porque se ha utilizado un recurso no válido o con un formato incorrecto. Revise la documentación de la API para determinar cómo se deben codificar los recursos complejos para su solicitud y si hay limitaciones para su valor y/o tamaño. |
|  | *código_registro_no válido* | 404 | El código de registro especificado ya no es válido o ha caducado. |
|  | *configuración_servicio_no_válida* | 500 | Error en la solicitud debido a una configuración de servicio incorrecta. |
|  | *missing_authentication_header* | 400 | La solicitud falló porque no contiene el encabezado de autenticación necesario para la API específica. |
|  | *asignación_recursos_ausentes* | 400 | No hay ninguna asignación correspondiente para el recurso especificado. Póngase en contacto con el servicio de asistencia para corregir la asignación requerida. |
|  | *preauthorization_denied_by_mvpd* | 403 | La MVPD ha devuelto una decisión &quot;Denegar&quot; al solicitar la preautorización del recurso especificado. |
|  | *preauthorization_denied_by_programmer* | 403 | Las reglas de degradación aplicadas por el programador aplican una decisión &quot;Denegar&quot; para el usuario actual. |
|  | *registration_code_service_unavailable* | 503 | Error en la solicitud porque el servicio de código de registro no está disponible. |
|  | *service_unavailable* | 503 | Error en la solicitud debido a que el servicio de autenticación o autorización no está disponible. |
|  | *access_token_unavailable* | 400 | Error inesperado en la solicitud al recuperar el token de acceso. Compruebe la configuración del tablero de TVE para ver las instrucciones de software disponibles y los esquemas personalizados registrados. |
|  | *versión_cliente_no_admitida* | 400 | Esta versión del SDK de autenticación de Adobe Pass es demasiado antigua y ya no es compatible. Consulte la documentación de la API para ver los pasos necesarios para actualizar a la versión más reciente. |
| **configuración** | *network_required_ssl* | 403 | Hay un problema de conexión SSL para el servicio de socio de destino. Póngase en contacto con el equipo de asistencia. |
|  | *demasiados_recursos* | 403 | Error en la solicitud de autorización o preautorización porque se consultaron demasiados recursos. Póngase en contacto con el equipo de soporte para configurar correctamente las limitaciones de autorización y preautorización. |
|  | *programador_desconocido* | 400 | No se reconoce el programador o el proveedor de servicios. Utilice el Tablero de TVE para registrar el programador especificado. |
|  | *aplicación_desconocida* | 400 | La aplicación no se reconoce. Utilice el Tablero de TVE para registrar la aplicación especificada. |
|  | *integración_desconocida* | 400 | La integración entre el programador especificado y el proveedor de identidad no existe. Utilice el Tablero de TVE para crear la integración necesaria. |
|  | *instrucción_de_software_desconocida* | 401 | No se reconoce la instrucción de software asociada al token de acceso. Póngase en contacto con el equipo de soporte técnico para aclarar el estado de la instrucción del software. |
| **registro de aplicación** | *token_de_acceso_caducado* | 401 | El token de acceso ha caducado. La aplicación debe actualizar el token de acceso como se indica en la documentación de la API. |
|  | *firma_token_acceso_no válida* | 401 | La firma del token de acceso ya no es válida. La aplicación debe actualizar el token de acceso como se indica en la documentación de la API. |
|  | *id_cliente_no_válido* | 401 | No se reconoce el identificador de cliente asociado. La aplicación debe seguir el proceso de registro de la aplicación como se indica en la documentación de la API. |
| **autenticación** | *authentication_session_expire* | 410 | La sesión de autenticación actual ha caducado. Para continuar, el usuario debe volver a autenticarse con una MVPD compatible. |
|  | *authentication_session_missing* | 401 | No se pudo recuperar la sesión de autenticación asociada con esta solicitud. Para continuar, el usuario debe volver a autenticarse con una MVPD compatible. |
|  | *authentication_session_invalidated* | 401 | El proveedor de identidad invalidó la sesión de autenticación. Para continuar, el usuario debe volver a autenticarse con una MVPD compatible. |
|  | *authentication_session_Issuer_mismatch* | 400 | La solicitud de autorización falló debido al hecho de que la MVPD indicada para el flujo de autorización es diferente de la que emitió la sesión de autenticación. El usuario debe volver a autenticarse con la MVPD deseada para continuar. |
|  | *autorización_denegada_por_hba_policies* | 403 | La MVPD ha devuelto una decisión &quot;Denegar&quot; debido a políticas de autenticación basadas en el hogar. La autenticación actual se obtuvo mediante un flujo de autenticación basado en el hogar (HBA), pero el dispositivo ya no está en casa al solicitar autorización para el recurso especificado. Para continuar, el usuario debe volver a autenticarse con una MVPD compatible. |
|  | *identidad_no_reconocida_por_mvpd* | 403 | Error en la solicitud de autorización porque MVPD no reconoció la identidad del usuario. |
| **autorización** | *authorization_expire* | 410 | La autorización previa para el recurso especificado ha caducado. El usuario debe obtener una nueva autorización para continuar. |
|  | *authorization_not_found* | 404 | No se ha encontrado ninguna autorización para el recurso especificado. El usuario debe obtener una nueva autorización para continuar. |
|  | *device_identifier_mismatch* | 403 | El identificador de dispositivo especificado no coincide con la identificación del dispositivo de autorización. El usuario debe obtener una nueva autorización para continuar. |
| **reintentar** | *error_conexión_red* | 403 | Error de conexión con el servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse. |
|  | *tiempo de espera_de_conexión_de_red* | 403 | Se agotó el tiempo de espera de conexión con el servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse. |
|  | *error_recibido_de_red* | 403 | Error de lectura al recuperar la respuesta del servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse. |
|  | *tiempo_de_ejecución_máximo_excedido* | 403 | La solicitud no se completó en el tiempo máximo permitido. Si vuelve a intentar la solicitud, el problema podría resolverse. |
| **reintentar después** | *demasiadas_solicitudes* | 429 | Se han enviado demasiadas solicitudes en un intervalo determinado. La aplicación puede reintentar la solicitud después del período de tiempo sugerido. |
|  | *user_rate_limit_expanded* | 429 | Un usuario en particular ha emitido demasiadas solicitudes en un intervalo determinado. La aplicación puede reintentar la solicitud después del período de tiempo sugerido. |
