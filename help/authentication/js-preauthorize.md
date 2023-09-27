---
title: Preautorizar
description: Autorización previa de JavaScript
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1499'
ht-degree: 0%

---

# Preautorizar {#js-preauthorize}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#preauth-overview}

Las aplicaciones deben utilizar el método de API de preautorización para obtener decisiones de preautorización para uno o más recursos. La solicitud de API de preautorización debe utilizarse para sugerencias de interfaz de usuario o filtrado de contenido. Se debe realizar una solicitud de API de autorización real antes de permitir que el usuario tenga acceso a los recursos especificados.

En caso de que se produzca un error inesperado (por ejemplo, un problema de red y un punto final de autorización de MVPD no disponible) cuando los servicios de autenticación de Adobe Pass procesen una solicitud de API preautorizada, se incluirá una o varias informaciones de error independientes para los recursos afectados como parte del resultado de la respuesta de API preautorizada.

### public preauthorize(solicitud: PreauthorizeRequest, devolución de llamada: AccessEnablerCallback&lt;any>): void {#preauth-method}

**Descripción:** Las aplicaciones deben utilizar este método para obtener las decisiones de autorización previa (informativas) del usuario autenticado del servicio de autenticación de Adobe Pass para ver recursos protegidos específicos, con el propósito principal de decorar la interfaz de usuario de la aplicación (por ejemplo, indicar el estado de acceso con iconos de bloqueo y desbloqueo).

**Disponibilidad:** v4.4.0+

**Parámetros:**

* `PreauthorizeRequest`: objeto de creación utilizado para definir la solicitud
* `AccessEnablerCallback`: llamada de retorno utilizada para devolver la respuesta de API
* `PreauthorizeResponse`: objeto utilizado para devolver el contenido de respuesta de API

### class PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string)[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* Establece la lista de recursos para los que desea obtener decisiones de preautorización.
* Es obligatorio establecerlo para el uso de la API preautorizada.
* Cada elemento de la lista debe ser una cadena que represente el valor del ID de recurso o el fragmento RSS de medios que debe acordarse con MVPD.
* Este método establece la información solo en el contexto de la actual `PreauthorizeRequestBuilder` instancia de objeto, que es el receptor de esta llamada al método.

* Para generar una `PreauthorizeRequest` puede echar un vistazo a `PreauthorizeRequestBuilder`Método de:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` recursos. La lista de recursos para los que desea obtener decisiones de preautorización.
* `@returns {PreauthorizeRequestBuilder}` La referencia a lo mismo `PreauthorizeRequestBuilder` instancia de objeto, que es el receptor de la llamada al método.
* Esto se hace para permitir la creación del encadenamiento de métodos.

#### disableFeatures(...features: string[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Establece las funciones que desea que se deshabiliten al obtener decisiones de preautorización.
* Esta función establece la información solo en el contexto de la actual `PreauthorizeRequestBuilder` instancia de objeto, que es el receptor de esta llamada de función.
* Para generar una `PreauthorizeRequest` puede echar un vistazo a `PreauthorizeRequestBuilder`Función de:

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` funciones. El conjunto de funciones que desea que se deshabiliten.
* `@returns` La referencia a lo mismo `PreauthorizeRequestBuilder` instancia de objeto, que es el receptor de la llamada a la función.
* Lo hace para permitir la creación de cadenas de funciones.

#### build(): PreauthorizeRequest {#preauth-req}

* Crea y recupera la referencia de un nuevo `PreauthorizeRequest` instancia de objeto.
* Este método crea una instancia nueva `PreauthorizeRequest` cada vez que se llama a.
* Este método utiliza los valores establecidos de antemano en el contexto de la actual `PreauthorizeRequestBuilder` instancia de objeto, que es el receptor de esta llamada al método.
* Tenga en cuenta que este método no produce ningún efecto secundario,
* por lo tanto, no altera el estado del SDK ni el estado del `PreauthorizeRequestBuilder` instancia de objeto, que es el receptor de esta llamada al método.
* Significa que las llamadas sucesivas de este método para el mismo receptor crearán nuevas llamadas diferentes `PreauthorizeRequest` instancias de objeto, pero que tengan la misma información, en caso de que los valores se establezcan en `PreauthorizeRequestBuilder` no se ha modificado entre las llamadas de.
* Si no necesita actualizar ninguna de la información proporcionada (recursos y almacenamiento en caché), puede reutilizar la instancia PreauthorizeRequest para varios usos de la API preautorizada.
* `@returns {PreauthorizeRequest}`

### interfaz AccessEnablerCallback&lt;t> {#interface-access-enablr-callback}

#### onResponse(resultado: T); {#on-response-result}

* Llamada de retorno de respuesta invocada por el SDK cuando se completó la solicitud de API preautorizada.
* El resultado es un resultado correcto o un resultado de error que contiene un estado.
* `@param {T} result`

#### onFailure(resultado: T); {#on-failure-result}

* Llamada de retorno de error invocada por el SDK cuando no se pudo atender la solicitud de API preautorizada.
* El resultado es un resultado de error que contiene un estado.
* `@param {T} result`

### class PreauthorizeResponse {#preauth-response-class}

#### estado público: estado; {#public-status}

* Devuelve: información adicional de estado (estado) en caso de error.
* Podría contener un `null` valor.

#### decisiones públicas: decisión[]; {#public-decisions}

* Devuelve: la lista de decisiones de preautorización. Una decisión para cada recurso.
* La lista puede estar vacía en caso de error.

### Estado de clase {#class-status}

#### estatus público: número; {#public-status-numbr}

* El código de estado de respuesta HTTP tal como se documenta en RFC 7231.
* Podría ser 0 en caso de que la variable `Status` proviene del SDK en lugar de los servicios de autenticación de Adobe Pass.

#### código público: número; {#public-code-numbr}

* El código de error estándar de los servicios de autenticación de Adobe Pass.
* Puede contener una cadena vacía o un `null` valor.

#### public message: string; {#public-msg-string}

* El mensaje detallado que, en algunos casos, proporcionan los puntos finales de autorización de MVPD o las reglas de degradación del Programador.
* Puede contener una cadena vacía o un `null` valor.

#### public details: string; {#public-details-strng}

* Contiene un mensaje detallado que, en algunos casos, lo proporcionan los puntos finales de autorización de MVPD o las reglas de degradación del programador.
* Puede contener una cadena vacía o un `null` valor.


#### public helpUrl: string; {#public-help-url-string}

* La dirección URL que vincula a más información sobre por qué se produjo este estado/error y posibles soluciones.
* Puede contener una cadena vacía o un `null` valor.

#### public trace: string; {#public-trace-string}

* El identificador único de esta respuesta, que se puede utilizar al ponerse en contacto con el servicio de asistencia para identificar problemas específicos en situaciones más complejas.
* Puede contener una cadena vacía o un `null` valor.

#### public action: string; {#public-action-string}

* La acción recomendada para remediar la situación.
   * **ninguno**: Lamentablemente, no hay ninguna acción predefinida para solucionar este problema. Esto podría indicar una invocación incorrecta de la API pública
   * **configuración**: Es necesario realizar un cambio de configuración a través del panel de TVE o poniéndose en contacto con el servicio de asistencia.
   * **application-registration**: la aplicación debe registrarse de nuevo.
   * **authentication**: el usuario debe autenticarse o volver a autenticarse.
   * **autorización**: el usuario debe obtener autorización para el recurso específico.
   * **degradación**: Se debe aplicar alguna forma de degradación.
   * **volver a intentar**: Si se vuelve a intentar la solicitud, el problema podría resolverse
   * **retry-after**: Si se reintenta la solicitud después del período de tiempo indicado, el problema podría resolverse.
* Puede contener una cadena vacía o un `null` valor.

### decisión colectiva {#class-decision}

#### id pública: string; {#public-id-string}

* El ID de recurso para el que se obtuvo la decisión.

#### public authorized: boolean; {#public-auth-boolean}

* El valor del indicador que indica si la decisión se ha realizado correctamente o no.

#### error público: Estado; {#public-error-status}

* Información adicional de estado (estado) en caso de que se haya producido algún error. Podría contener un `null` valor.

## Ejemplo de implementación de cliente {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## Ejemplos de escenarios {#scenario-examples}

### Escenario 1: se autorizaron todos los recursos solicitados {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicador de código de error mejorado</th>
    <th>Respuesta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Desactivado</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### Escenario 2: se autorizaron algunos recursos solicitados. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicador de código de error mejorado</th>
    <th>Respuesta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Desactivado</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisiones&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Habilitado</td>
    <td>

    &quot;JavaScript
    {
    &quot;decisiones&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: true
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;El MVPD ha devuelto una decisión \&quot;Deny\&quot; al solicitar la preautorización del recurso especificado.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: true
    },
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Escenario 3: no se autorizó ninguno de los recursos solicitados. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Indicador de código de error mejorado</th>
    <th>Respuesta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Desactivado</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisiones&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false
    }
    ]
    }
    
    &quot;

</td>
  </tr>

<tr>
    <td>Habilitado</td>
    <td>

    &quot;JavaScript
    
    {
    &quot;decisiones&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;El MVPD ha devuelto una decisión \&quot;Deny\&quot; al solicitar la preautorización del recurso especificado.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;preauthorization_denied_by_mvpd&quot;,
    &quot;message&quot;: &quot;El MVPD ha devuelto una decisión \&quot;Deny\&quot; al solicitar la preautorización del recurso especificado.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES03&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;maximum_execution_time_expanded&quot;,
    &quot;message&quot;: &quot;La solicitud no se completó en el tiempo máximo permitido. Si vuelve a intentar la solicitud, el problema podría resolverse.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;acción&quot;: &quot;reintentar&quot;
    }
    }
    ]
    }
    
    &quot;

</td>
  </tr>
</tbody>


### Escenario 4: solicitud de cliente incorrecta; no se especificaron recursos. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Indicador de código de error mejorado</th>
    <th>Respuesta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Desactivado/Habilitado</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 400,
    &quot;code&quot;: &quot;internal_error&quot;,
    &quot;message&quot;: &quot;Error interno en la solicitud.&quot;,
    &quot;details&quot;: &quot;El parámetro &quot;resource&quot; de String[] requerido no está presente&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decisiones&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Escenario 5: solicitud de cliente incorrecta; recursos vacíos especificados. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Indicador de código de error mejorado</th>
    <th>Respuesta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Desactivado/Habilitado</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 412,
    &quot;code&quot;: &quot;missing_resource&quot;,
    &quot;message&quot;: &quot;Falta el parámetro resource&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;action&quot;: &quot;none&quot;
    },
    &quot;decisiones&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Escenario 6: error de red. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>Indicador de código de error mejorado</th>
    <th>Respuesta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Habilitado</td>
    <td>

    &quot;JavaScript
    {
    &quot;decisiones&quot;: [
    {
    &quot;id&quot;: &quot;RES01&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;Se ha producido un error de lectura al recuperar la respuesta del servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;acción&quot;: &quot;reintentar&quot;
    }
    },
    {
    &quot;id&quot;: &quot;RES02&quot;,
    &quot;authorized&quot;: false,
    &quot;error&quot;: {
    &quot;status&quot;: 403,
    &quot;code&quot;: &quot;network_received_error&quot;,
    &quot;message&quot;: &quot;Se ha producido un error de lectura al recuperar la respuesta del servicio de socio asociado. Si vuelve a intentar la solicitud, el problema podría resolverse.&quot;,
    &quot;helpUrl&quot;: &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;,
    &quot;acción&quot;: &quot;reintentar&quot;
    }
    }
    ]
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### Escenario 7: se invocó un flujo de preautorización sin una sesión de AuthN válida.

<table>
<thead>
  <tr>
    <th>Indicador de código de error mejorado</th>
    <th>Respuesta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Desactivado/Habilitado</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;authentication_session_missing&quot;,
    &quot;message&quot;: &quot;No se pudo recuperar la sesión de autenticación asociada con esta solicitud. El usuario debe volver a autenticarse con una MVPD compatible para continuar.&quot;,
    &quot;action&quot;: &quot;authentication&quot;
    },
    &quot;decisiones&quot;: []
    }
    
    &quot;

</td>
  </tr>
</tbody>
</table>



### Escenario 8: se invocó el flujo de preautorización antes de completar la llamada a setRequestor

<table>
<thead>
  <tr>
    <th>Indicador de código de error mejorado</th>
    <th>Respuesta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Desactivado/Habilitado</td>
    <td>

    &quot;JavaScript
    {
    &quot;status&quot;: {
    &quot;status&quot;: 0,
    &quot;code&quot;: &quot;requestor_not_configured&quot;,
    &quot;message&quot;: &quot;El solicitante aún no está configurado, lo que es un requisito previo para utilizar cualquier API aparte de la API setRequestor.&quot;,
    &quot;acción&quot;: &quot;reintentar&quot;
    },
    &quot;decisiones&quot;: []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>
