---
title: Autorización de comprobación preliminar
description: Autorización de comprobación preliminar
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# Autorización de comprobación preliminar {#preflight-authorization}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

## Información general {#overview}

Esta función proporciona una comprobación de autorización ligera para varios recursos. El propósito de esta comprobación ligera es decorar la interfaz de usuario (por ejemplo, indicar el estado del acceso con iconos de bloqueo y desbloqueo). La autorización de comprobación preliminar es lo más ligera y eficiente posible, de modo que una sola llamada de API arroja el estado de autorización para una lista de recursos. Tenga en cuenta que esta función no es autoritativa con respecto a la autorización de un recurso.

Se debe realizar una llamada a `getAuthorization(resource)` o `checkAuthorization(resource)` antes de permitir la reproducción.

La autorización de comprobaciones también es compatible con un caso de uso diferente, en el que el programador debe solicitar autorización para varios ID de recurso para permitir la reproducción de un elemento de contenido multimedia. El programador puede realizar una comprobación preliminar inicial de los recursos necesarios y, según la respuesta, puede fallar antes de tiempo si no se cumplen las condiciones empresariales.

Para obtener una lista de las MVPD que admiten la autorización de comprobación preliminar, consulte la página [Autorización de comprobación preliminar de MVPD](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#preflight_support_list).

>[!NOTE]
>
> Tenga en cuenta que el uso de esta función para las MVPD que no tienen compatibilidad total con la autorización de comprobaciones debe acordarse de antemano con Adobe y MVPD. El uso de la autorización de comprobaciones en estas MVPD caerá en el &quot;peor escenario&quot; descrito [aquí](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#intro) y puede provocar problemas de rendimiento y tiempos de respuesta lentos. </br>
> Además, tenga en cuenta que el Adobe **debe aceptar explícitamente el uso de la autorización de comprobaciones con** más de 5 recursos.

## API de comprobaciones de AccessEnabler {#AE_pre_api}

AccessEnabler expone un par de funciones de API/devolución de llamada para implementar la autorización de comprobaciones. La llamada de API de toma un solo parámetro que consta de una lista de recursos. La función de llamada de retorno toma un único parámetro que representa los recursos autorizados reales. Los siguientes ejemplos están en ActionScript, pero las llamadas de están disponibles en todos los tipos de AccessEnabler.

### checkPreauthorizedResources(Matriz:recursos):void {#checkPreauthRes}

Llame a esta función en el objeto AccessEnabler para solicitar el estado de autorización de una lista de recursos.

El parámetro resources es la lista de recursos para los que se debe comprobar la autorización. Cada elemento de la lista debe ser una cadena que represente el ID de recurso. El id. de recurso está sujeto a las mismas limitaciones que el id. de recurso de la llamada `getAuthorization()`; es decir, se acuerda el valor establecido entre el programador y la MVPD o un fragmento de RSS multimedia. Tenga en cuenta que la autenticación de Adobe Pass no administra los recursos de ninguna manera, excepto por una capa de mediación fina que puede transformar los formatos de recursos según lo que admita realmente la MVPD.

### preauthorizedResources(Array:authorizedResources) {#preauthRes}

Esta es una función de llamada de retorno que debe implementarse en la aplicación de capa superior del programador. AccessEnabler llamará a esta función después de calcular la lista de recursos autorizados.


### Ejemplo de JS

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## Información de implementación {#details}

- [Comprobación preliminar mediante ChannelID](#preflight_using_channelID)
- [POST/Autorización previa](#post)
- [Almacenamiento](#storage)

La llamada de API intenta encontrar una lista en caché de recursos autorizados para el usuario actual en el almacenamiento local del cliente. Si no hay ninguna lista en caché, se realiza una llamada HTTPS a los servidores de AdobePass para recuperar la lista.

El mecanismo de almacenamiento en caché mejora los tiempos de rendimiento en llamadas subsiguientes al omitir por completo la llamada de red. Además, la lista en caché se puede rellenar de antemano como parte del proceso de autenticación.  (Para obtener información sobre la configuración de este escenario, consulte [Integración de autorización de comprobación preliminar](/help/authentication/integration-guide-mvpds/authz-usecase.md#preflight_authz_int) en la sección Autorización de la Guía de integración de MVPD).

Además, la lista almacenada en caché de recursos puede utilizarse potencialmente para optimizar el flujo de autorización, en el sentido de que si existe una lista almacenada en caché de recursos, `checkAuthorization()` puede comprobarla antes de realizar una llamada de red. Si el recurso no está en la lista de recursos autorizados previamente, la comprobación puede fallar sin necesidad de llamar a los servidores de autenticación de Adobe Pass.


### Comprobación preliminar mediante ChannelID {#preflight_using_channelID}

A partir de la versión 2.4.1 de autenticación de Adobe Pass, el flujo de comprobaciones funciona de la siguiente manera:

1. Durante la autenticación, la autenticación de Adobe Pass lee el elemento `channelIID` de la respuesta SAML de MVPD y utiliza este valor para establecer el elemento `authorizedResources` en el token de autenticación.
1. Dentro de la función de API `checkPreauthorizedResources()`, la autenticación de Adobe Pass comprueba si el elemento `authorizedResources` está establecido.
1. Si se establece el elemento `authorizedResources`, la autenticación de Adobe Pass lee ese valor y realiza una intersección entre la lista de recursos del elemento `authorizedResources` y la lista de recursos recibidos del parámetro `checkPreauthorizedResources()`.  El resultado de esta intersección es la lista final de recursos preautorizados.
1. Si no se establece el elemento `authorizedResources`, ejecute el flujo implementado anteriormente, en el que la lista de recursos recibidos del parámetro `checkPreauthorizedResources()` se pasa al PreAuthorizationServlet. Este servlet realiza las llamadas de autorización a los extremos de MVPD y devuelve la lista de recursos autorizados previamente.

### Ejemplo de comprobación preliminar con ID de canal

El ejemplo siguiente muestra una línea de canales de ejemplo. Tenga en cuenta que el nombre del atributo utilizado para enviar la lista de canales puede diferir de un MVPD a otro:

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


El elemento `authorizedResources` del elemento authentication aparece de la siguiente manera:

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

El programador ejecuta la llamada a la API `checkPreauthorizedResources()` y pasa la siguiente lista de parámetros:</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;
- &quot;fbc-fox&quot;

La implementación de comprobaciones actual realiza la intersección con la lista de recursos del elemento `authorizedResources` y devuelve esta lista:

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;



**Nota:** Tenga en cuenta que la intersección no distingue entre mayúsculas y minúsculas.



### POST/Autorización previa {#post}

AccessEnabler realizará automáticamente esta llamada cuando no exista una lista de recursos en caché, autorizada.


#### Solicitud {#req}

| Parámetro | Tipo | Requerido | Descripción |
| --- | --- | --- | --- |
| `authentication_token` | cadena | SÍ | El token de autenticación. |
| `resource_id` | cadena | SÍ | Un solo recurso. Esto se puede especificar varias veces, una vez para cada elemento de la matriz de recursos proporcionada en la llamada de API `checkPreauthorizedResources()`. |


**Nota:** El número máximo de recursos solicitados es configurable.
**El valor predeterminado máximo es 5.**


#### Respuesta {#resp}

La respuesta que devuelve el servlet preautorizado tiene el siguiente formato:

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### Almacenamiento {#storage}

Una lista de recursos autorizados previamente que AccessEnabler obtiene del proveedor de servicios. Esta lista de recursos:

- Se almacena junto con los tokens AuthN y AuthZ
- Es válido mientras el usuario esté en el mismo sitio web o hasta que el
El token de AuthN caduca
- Se recupera cada vez que el usuario aterriza en una nueva Adobe Pass
sitio web integrado de autenticación

Cada entrada contiene el ID de recurso para el que el usuario está autorizado previamente.

Por ejemplo:


| ID de recurso | Autorizado |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


Esta lista se denomina caché de preautorización.

#### Flujo {#flow}

1. El sitio o la aplicación del programador realiza una llamada de `checkPreauthorizedResources(resourceList)`.
1. AccessEnabler verifica el token de autenticación para los recursos autorizados:
   1. Si el token de autenticación contiene recursos autorizados, esta lista es autoritativa y no se debe realizar ninguna llamada para obtener esta información. Los recursos de resourceList se buscan dentro de la lista de recursos autorizados en el token de autenticación y la llamada de retorno `preauthorizedResources()` solo devuelve los que se encontraron.
   1. Si el token de autenticación NO contiene recursos autorizados - `resourceList` se compara con la lista de recursos de la caché de preautorización.
      1. Si la lista contiene los mismos recursos, significa que ya se realizó una llamada al servidor y la respuesta ya está en la caché de preautorización. La llamada de retorno `preauthorizedResources()` solo devolverá los recursos que estén autorizados.
      1. Si la lista NO contiene los mismos recursos, el cliente debe realizar una llamada al servidor para obtener el estado de autorización de los recursos en resourceList. La respuesta se recuperará y se almacenará en la caché de preautorización, sustituyendo completamente los recursos antiguos. La llamada de retorno `preauthorizedResources()` solo devolverá los recursos que estén autorizados.


#### Recuperación de lista {#listRetrieve}

Cada vez que se llama a `checkPreauthorizedResources()`, se comprueba la lista de recursos para comprobar si se ha concedido autorización con la caché de preautorización. Si la lista contiene el mismo conjunto de recursos, no se realiza ninguna llamada al proveedor de servicios, ya que todos los recursos necesarios para activar la devolución de llamada `preauthorizedResources()` ya están en la caché.


#### logout() {#logout}

La caché de preautorización se vacía al cerrar la sesión.


## Dependencias {#depends}

El rendimiento de la API de comprobaciones depende de implementaciones de MVPD específicas.  Para ver las opciones de implementación, consulte [Integración de autorización de comprobación preliminar](/help/authentication/integration-guide-mvpds/authz-usecase.md#preflight_authz_int) en la sección Autorización de la Guía de integración de MVPD.


## Seguridad {#security}

Las API de cliente están disponibles para todos los programadores.

La implementación utiliza HTTPS como transporte, pero para tener una llamada más ligera, no se emplean medidas de seguridad adicionales (sin firma, sin FAXES).

**Atención:** NO use esta API de manera autorizada para determinar si se debe otorgar acceso a un usuario a un recurso protegido. El propósito de esta API es la decoración de la interfaz de usuario y / o la verificación previa de las decisiones comerciales. Las llamadas `getAuthorization()` y `checkAuthorization()` siempre se deben realizar antes de permitir la reproducción.


## Compatibilidad {#compat}

Esta función es compatible con todos los tipos de AccessEnabler: AS, JS, AIR, iOS, Android, Xbox (en el flujo AuthN de la segunda pantalla).

La autorización de verificación previa no admite la autorización previa de recursos que contengan secciones de CDATA. El objetivo del sistema de comprobaciones actual es admitir el filtrado a nivel de canal. Es probable que los recursos con secciones CDATA sean recursos de nivel de recurso. La comprobación preliminar no admite recursos simples de `mrss` para la preautorización en el nivel de canal, siempre y cuando no contengan CDATA.

## Integración Con Otras Funciones {#integ_w_other_features}

Se puede producir una posible optimización en el flujo de autorización para que falle rápidamente si el recurso no está en la lista de recursos autorizados previamente.

En esta optimización, el punto final de comprobación preliminar del servidor se integra con el mecanismo de degradación.

Si existe la regla &quot;AuthN All&quot; para el MVPD y el solicitante, el punto final de comprobación preliminar simplemente reflejará todos los recursos recibidos en la solicitud.

Lo mismo ocurre si &quot;AuthZ All&quot; está habilitado para al menos uno de los recursos presentes en la solicitud.

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
