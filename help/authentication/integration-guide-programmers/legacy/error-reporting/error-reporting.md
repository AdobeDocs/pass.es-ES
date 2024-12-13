---
title: Informes de errores
description: Informes de errores
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '2990'
ht-degree: 0%

---

# Informes de errores (heredados) {#error-reporting}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


## Información general {#overview}

Los informes de errores en la autenticación de Adobe Pass se implementan actualmente de dos formas diferentes:

* **Informes de errores avanzados** El implementador registra una llamada de retorno de error en el caso de [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk) o implementa un método de interfaz denominado &quot;`status`&quot; en el caso de [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk) y [AccessEnabler Android SDK](#accessenabler-android-sdk), para recibir informes de errores avanzados. Los errores se clasifican en **Información**, **Advertencia** y **Errores**. Este sistema de informes es **asincrónico**, en el sentido de que **no hay garantía del orden en que se desencadenarán varios errores**.  Para obtener más información sobre el sistema de informes de errores avanzado, consulte la sección [Informes de errores avanzados](#advanced-error-reporting).

* **Informe de errores original -** Un sistema de informes estático en el que los mensajes de error se pasan a funciones de devolución de llamada específicas cuando fallan solicitudes específicas. Los errores se agrupan en tipos genéricos, de autenticación y de autorización. Para obtener la lista de errores notificados en el sistema original, consulte la sección [Informes de errores originales](#original-error-reporting).


## Informes de errores avanzados {#advanced-error-reporting}

* [AccessEnabler JavaScript SDK](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [AccessEnabler Android SDK](#accessenabler-android-sdk)
* [AccessEnabler FireOS SDK](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>La API antigua de [Informes de errores originales](#original-error-reporting) seguirá funcionando como antes, los informes de errores avanzados no rompen la funcionalidad, pero los informes de errores originales YA NO recibirán ninguna actualización. Todos los errores y actualizaciones nuevos se producirán en el sistema de informes de errores avanzado.

### AccessEnabler JavaScript SDK {#accessenabler-javascript-sdk}

El nuevo sistema de informes de errores se deja opcional, por lo que el implementador puede registrar explícitamente una llamada de retorno del controlador de errores para recibir informes de errores avanzados. El sistema incluye la capacidad de registrar y anular el registro de varias llamadas de retorno de error de forma dinámica. Además, puede registrar cualquier llamada de retorno de error nueva en cuanto se cargue AccessEnabler JavaScript SDK, sin necesidad de realizar ninguna otra inicialización (antes de llamar a `setRequestor()`), lo que significa que puede recibir informes avanzados sobre los errores de inicialización y configuración.


#### Implementación {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


La función de devolución de llamada del controlador de errores recibirá un único objeto (un mapa) con la siguiente estructura:

```JavaScript
    {
      errorId: "CFG410",
      level: "error",
      message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

### 1. Vincular {#bind}

**`.bind(eventType:String, handlerName:String):void`**

Adjunta un controlador para un evento.

**`eventType`**: SOLO el valor &quot;`errorEvent`&quot; provoca que AccessEnabler JavaScript SDK active las llamadas de retorno de informes de errores avanzados.

**`handlerName`**: una cadena que especifica el nombre de la función de controlador de errores.


Ambos parámetros de enlace deben utilizar únicamente caracteres del siguiente conjunto: `[0-9a-zA-Z][-._a-zA-Z0-9]`; es decir, los parámetros deben comenzar con un número o una letra y, a continuación, pueden incluir sólo guiones, puntos, guiones bajos y caracteres alfanuméricos.  Además, los parámetros no pueden superar los 1024 caracteres.

**Ejemplo** de controladores de error de enlace:

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

Debido a limitaciones técnicas, no se puede enlazar un cierre ni una función anónima. Debe especificar el nombre del método en el segundo parámetro.


### 2. Desenlazar {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

Quita un controlador de eventos previamente adjunto.

**`eventType`**: SOLO el valor &#39;`errorEvent`&#39; provoca que AccessEnabler JavaScript SDK active las llamadas de retorno de informes de error avanzadas.

**`handlerName`**: se quitará una cadena que especifica el nombre de la función de controlador de errores, si es nula o falta, todos los controladores adjuntos para el `eventType` especificado.

Ambos parámetros de enlace deben utilizar únicamente caracteres del siguiente conjunto: `[0-9a-zA-Z][-._a-zA-Z0-9]`; es decir, los parámetros deben comenzar con un número o una letra y, a continuación, pueden incluir sólo guiones, puntos, guiones bajos y caracteres alfanuméricos.  Además, los parámetros no pueden superar los 1024 caracteres.

**Ejemplo** de quitar un solo controlador de error:

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**Ejemplo** eliminando todos los controladores de error:

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

El nuevo sistema de notificación de errores es obligatorio, por lo que el implementador debe ajustarse explícitamente al nuevo protocolo Objective C &quot;EntitlementStatus&quot;. Este nuevo enfoque permite a los programadores recibir informes de errores avanzados.

#### Implementación {#accessenab-ios-tvossdk-imp}

Un implementador debe cumplir con el siguiente protocolo **EntitlementStatus**:

**EntitlementStatus.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

Su función **status** recibirá un solo objeto (un `NSDictionary`) con la siguiente estructura:

```OBJ-C
    {
          errorId: "CFG410",
          level: "error",
          message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

**1. Declaración**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2. Implementación**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### AccessEnabler Android SDK {#accessenabler-android-sdk}

El nuevo sistema de informe de errores es obligatorio, ya que el implementador debe cumplir explícitamente con el protocolo definido en la interfaz `IAccessEnablerDelegate`. Este nuevo enfoque permite a los programadores recibir informes de errores avanzados.

#### Implementación {#access-enablr-androidsdk-imp}

Un implementador debe controlar el nuevo método `status` desde la interfaz `IAccessEnablerDelegate`. La función **`status`** recibirá un único objeto **`AdvancedStatus`** con el siguiente modelo:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Muestra**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

### AccessEnabler FireOS SDK {#accessenabler-fireos-sdk}


El nuevo sistema de informe de errores es obligatorio, ya que el implementador debe cumplir explícitamente con el protocolo definido en la interfaz `IAccessEnablerDelegate`. Este nuevo enfoque permite a los programadores recibir informes de errores avanzados.

#### Implementación {#access-enab-fireos-sdk-}

Un implementador necesita controlar el nuevo método `status`desde la interfaz`IAccessEnablerDelegate`. La función **`status`** recibirá un único objeto **`AdvancedStatus`** con el siguiente modelo:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Muestra**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

## Referencia de códigos de error avanzados {#advanced-error-codes-reference}

En la tabla siguiente se enumeran y describen los códigos de error expuestos por la API de error más reciente, junto con las acciones sugeridas para corregirlos:

| ID | Nivel | Descripción | Acciones de desarrollador | Acciones de usuario | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| AAPL&amp; AAPL_ERROR | Error | Error genérico de SSO de Apple | El error contiene un campo de detalles con el error de VSA original. | n/a | n/a | Sí | n/a |
| VSA203 | Información | El usuario decidió cerrar la sesión de la aplicación mientras la autenticación se producía como resultado del inicio de sesión a través del SSO de la plataforma. | Indique/pida al usuario que cierre sesión explícitamente desde Configuración -> Cuentas -> Proveedor de TV en tvOS. <br><br> Indica o solicita al usuario que cierre sesión explícitamente desde Configuración -> Proveedor de TV en iOS/iPadOS. | Cerrar sesión explícitamente desde Configuración -> Cuentas -> Proveedor de TV en tvOS. <br> <br> Cerrar sesión explícitamente desde Configuración -> Proveedor de TV en iOS/iPadOS | n/a | Sí | n/a |
| VSA404 | Información | El permiso de cuenta de suscriptor de vídeo de aplicación es indeterminado. | Incentivar a los usuarios que se nieguen a dar permiso para acceder a la información de suscripción explicando las ventajas de la experiencia del usuario de inicio de sesión único (SSO). | El usuario puede cambiar su decisión accediendo a la configuración de la aplicación (acceso al proveedor de TV) o a la sección desde Configuración -> Proveedor de TV en iOS/iPadOS o Configuración -> Cuentas -> Proveedor de TV en tvOS. | n/a | Sí | n/a |
| VSA503 | Información | Error de solicitud de metadatos de cuenta de suscriptor de vídeo de aplicación. | El extremo de MVPD no responde. La aplicación podría volver al flujo de autenticación normal. | n/a | n/a | Sí | n/a |
| 500 | Error | Error interno | Utilice AccessEnablerDebug e inspeccione los registros de depuración (salida de console.log) para determinar qué ha fallado. | n/a | Sí | Sí | n/a |
| SEC403 | Error | Error de seguridad de dominio. El solicitante está utilizando un dominio no válido. Todos los dominios utilizados por un ID de solicitante concreto deben incluirse en la lista blanca por Adobe. | : cargue AccessEnabler solo desde la lista de dominios permitidos <br> <br>: Adobe de contacto para administrar la lista blanca de dominios para el ID de solicitante utilizado <br> <br> - iOS: compruebe que está utilizando el certificado correcto y que la firma se ha creado correctamente | n/a | n/a | Sí | n/a |
| SEC412 | Advertencia | [Disponible en la versión 2.5], el ID del dispositivo no coincide. Esto puede suceder siempre que la plataforma subyacente cambie su ID de dispositivo. En este caso, los tokens existentes se borrarán y el usuario ya no se autenticará. Tenga en cuenta que esto sucede de forma legítima cuando el usuario utiliza JS SDK y está en itinerancia (en JS, la IP del cliente forma parte del ID del dispositivo). De lo contrario, esto podría indicar un intento de fraude: un intento de copiar tokens de un dispositivo diferente. | - Monitorizar el número de advertencias. Si se disparan sin razón aparente (sin actualizaciones recientes del navegador; nuevos sistemas operativos) que podría ser un indicador de intentos de fraude.  <br> <br>: si lo desea, informe al usuario de que debe iniciar sesión de nuevo. | Inicie sesión de nuevo. | Sí | Sí | Sí desde la versión 3.2 |
| SEC420 | Error | Error de seguridad HTTP al comunicarse con los servidores de autenticación de Adobe Pass. Este error suele producirse cuando se utilizan servidores proxy o suplantación. | - Cargue `[https://]{SP_FQDN\}` en el explorador y acepte manualmente los certificados SSL, por ejemplo, **https://api.auth.adobe.com** o **https://api.auth-staging.adobe.com** <br> <br>- Marcar los certificados de proxy como de confianza | Si esto ocurre para un usuario normal, es una indicación de un posible ataque de hombre en medio. | Sí | Sí | Sí desde la versión 3.2 |
| CFG100 | Advertencia | La fecha/hora/zona horaria del equipo cliente no está configurada correctamente. Esto probablemente dará lugar a errores de autenticación/autorización. | - Informar al usuario para establecer la hora correcta. <br> <br> Realice acciones para evitar flujos de derechos, ya que es probable que fallen. | Establezca la fecha y la hora correctas. | Sí | Sí | Sí desde la versión 3.2 |
| CFG400 | Error | Se ha proporcionado un ID de solicitante no válido. | El desarrollador DEBE especificar un ID de solicitante válido. | n/a | Sí | Sí | Sí desde la versión 3.2 |
| CFG404 | Error | No se han encontrado los servidores de autenticación de Adobe Pass. Esto puede ocurrir en 3 instancias: <br><br> - El desarrollador tiene una suplantación no válida. <br><br>: el usuario tiene problemas de red y no puede acceder a los dominios de autenticación de Adobe Pass. <br><br>: los servidores de autenticación de Adobe Pass no están configurados correctamente. <br><br>  **Nota:** En Firefox, aparecerá CFG400 en lugar de CFG404 (limitación del navegador) | - Compruebe la suplantación. <br><br>: comprobar la configuración de red/DNS. <br><br>: informar al Adobe. | Compruebe la configuración de red/DNS. | Sí | Sí | Sí desde la versión 3.2 |
| CFG410 | Error | AccessEnabler es demasiado antiguo. | Informe al usuario de que borre las cachés. | Borre la memoria caché del explorador. | Sí | n/a | Sí desde la versión 3.2 |
| CFG5xx | Error | Los servidores de autenticación de Adobe Pass están experimentando errores internos. xx puede ser cualquier número. | - Informar al usuario de la no disponibilidad de la autenticación de Adobe Pass. <br><br> - Omitir autenticación de Adobe Pass. <br> <br> - Informar al Adobe. | Inténtelo más tarde. | Sí | Sí | Sí desde la versión 3.2 |
| N000 | Información | El usuario no está autenticado. | n/a | Inicie sesión. | Sí | Sí | Sí desde la versión 3.2 |
| N001 | Información | Se inició un intento de autenticación pasivo en segundo plano. Esto ocurrirá en el caso de las MVPD configuradas con &quot;Autenticación por solicitante&quot;. Aunque se espera que el usuario se autentique automáticamente, esto supone una penalización del rendimiento durante la inicialización. | Si lo desea, informe al usuario de que el trabajo está en curso o a la interfaz de usuario actual que le alerta. | Espera. | Sí | Sí | Sí desde la versión 3.2 |
| N003 | Información | El usuario selecciona la opción &quot;Otro proveedor de TV&quot; del selector de MVPD de Apple. | Se llamará a la devolución de llamada *displayProviderDialog* y la aplicación podría volver al flujo de autenticación normal. | Seleccione MVPD normal y continúe con la pantalla de inicio de sesión. | n/a | Sí | n/a |
| N004 | Información | El usuario selecciona un proveedor de TV no admitido por el solicitante actual. | Se llamará a la devolución de llamada *displayProviderDialog* y la aplicación podría volver al flujo de autenticación normal. | Seleccione MVPD normal y continúe con la pantalla de inicio de sesión. | n/a | Sí | n/a |
| N005 | Información | Se canceló el selector de MVPD. | n/a | n/a | Sí | Sí | Sí desde la versión 3.2 |
| N010 | Advertencia | Se autenticó al usuario mientras se aplicaba la regla de degradación autenticar todo para el MVPD seleccionado. | Si lo desea, informe al usuario de que está recibiendo acceso gratuito debido a problemas con MVPD. | n/a | Sí | Sí | Sí desde la versión 3.2 |
| N011 | Información | Se autenticó al usuario mediante TempPass. | - Informar al usuario. <br> <br>: si lo desea, puede presentar una lista de MVPD normales. | Si lo desea, puede iniciar sesión con su MVPD habitual. | Sí | Sí | Sí desde la versión 3.2 |
| N111 | Advertencia | Pase temporal caducado. | - Informar al usuario. <br> <br> - Presentar una lista de MVPD regulares. <br> <br> - Ocultar la opción TempPass. | Inicie sesión con su MVPD habitual. | Sí | Sí | Sí desde la versión 3.2 |
| N130 | Error | **No se encontró el token de autenticación en la sesión.** Esto puede deberse a una de las siguientes razones: <br> <br> 1. El explorador tiene deshabilitadas las cookies (de terceros) (no aplicable a AccessEnabler JavaScript SDK versión 4.x) <br> <br> 2. El explorador tiene habilitada la opción Impedir el seguimiento entre sitios (Safari 11+) <br> <br> 3. La sesión ha caducado <br> <br> 4. El programador llama a las API de autenticación en una sucesión incorrecta <br> <br> NOTA: Este código de error no está disponible para flujos de autenticación de redireccionamiento de página completa. | 1. Solicitar al usuario que habilite las cookies (de terceros) <br> <br> 2. Pedir al usuario que deshabilite el seguimiento entre sitios <br> <br> 3. Pedir al usuario que vuelva a autenticar <br> <br> 4. Llamar a las API en el orden correcto | 1. Habilitar las cookies de (terceros) <br> <br> 2. Deshabilitar seguimiento entre sitios <br> <br> 3. Volver a autenticar <br> <br> 4. N/D | Sí | Sí | Sí desde la versión 3.2 |
| N500 | Error | Error interno. <br> <br> Nota: Este es el &quot;Error de autenticación genérica&quot; y &quot;Error de autenticación interna&quot; del sistema de errores original. Este error se eliminará gradualmente. | Utilice AccessEnablerDebug e inspeccione los registros de depuración (salida de console.log) para determinar qué ha fallado. | n/a | Sí | Sí | n/a |
| R401 | Error | Se ha producido un error al tratar de obtener un token de acceso. <br> <br> Nota: Se trata de un error irrecuperable. Informar al usuario de que la aplicación no está disponible. | - iOS: Compruebe la declaración de software y los esquemas personalizados en su aplicación. <br> <br> - JavaScript: compruebe el extracto de software en la aplicación del sitio web. <br> <br> Abra un ticket usando Zendesk e informe al usuario de que el sistema no está disponible temporalmente | n/a | Sí desde la versión 4.0 | Sí desde la versión 3.0 | Sí desde la versión 3.2 |
| R400 | Error | La aplicación no está registrada. La instrucción del software no es válida o se ha revocado. <br> <br> Nota: Se trata de un error irrecuperable. Informar al usuario de que la aplicación no está disponible. | - iOS: Compruebe la declaración de software y los esquemas personalizados en su aplicación. <br> <br> - JavaScript: compruebe el extracto de software en la aplicación del sitio web. <br> <br> Abra un ticket usando Zendesk e informe al usuario de que el sistema no está disponible temporalmente | n/a | Sí desde la versión 4.0 | Sí desde la versión 3.0 | Sí desde la versión 3.2 |
| REG500 | Error | No se ha podido obtener el código de registro del servidor. <br> <br> Nota: Se trata de un error irrecuperable. Informar al usuario de que la aplicación no está disponible. | Abra un ticket usando Zendesk e informe al usuario que el sistema no está disponible temporalmente. | n/a | Sí desde la versión 4.0 | Sí desde la versión 3.0 | Sí desde la versión 3.2 |
| REGCODE | Correcto | La aplicación denominada API setSelectedProvider en la plataforma tvOS. | Indique o pida al usuario que utilice un segundo dispositivo (pantalla) para iniciar sesión con el código de registro proporcionado. | Utilice el regcode en un segundo dispositivo (pantalla) para iniciar la autenticación. | n/a | Sí solo para tvOS | n/a |
| Z010 | Advertencia | Se autorizó al usuario mientras la regla de degradación authentication-all o authorize-all estaba activa para el MVPD seleccionado. | Si lo desea, informe al usuario de que está recibiendo acceso gratuito debido a problemas con MVPD. | n/a | Sí | Sí | Sí desde la versión 3.2 |
| Z011 | Información | Se autorizó al usuario mediante TempPass | Si lo desea, informe al usuario | n/a | Sí | Sí | Sí desde la versión 3.2 |
| Z100 | Error | Error de autorización porque el usuario no tiene ninguna suscripción para el recurso solicitado o por otros motivos que se originan en la MVPD, por ejemplo, el vídeo no coincide con la configuración de Control parental de la cuenta de usuario | - No permitir la reproducción. <br> <br> - Informar al usuario. <br> <br>: la clave &quot;message&quot; del mensaje de error PUEDE contener un mensaje más detallado proporcionado por MVPD. | n/a | Sí | Sí | Sí desde la versión 3.2 |
| Z110 | Error | Autorización denegada debido a denegaciones repetidas de MVPD. Posible intento de fraude o DOS. | - No permitir la reproducción. <br> <br> - Informar al usuario. | n/a | Sí | Sí | Sí desde la versión 3.2 |
| Z120 | Error | Autorización denegada por motivos técnicos para comunicarse con MVPD. Posible error de red. | - No permitir la reproducción. <br> <br>: informe al usuario de que MVPD ha tenido problemas y que debe intentarlo más tarde. | Inténtelo más tarde. | Sí | Sí | Sí desde la versión 3.2 |
| Z130 | Error | Autorización denegada porque se ha utilizado un recurso no válido o con un formato incorrecto. | Compruebe la cadena de recurso y corríjala. Por lo general, este error se debe a un MRSS mal formado o a que se utiliza una cadena sin formato en lugar de MRSS. | n/a | Sí | Sí | Sí desde la versión 3.2 |
| Z169 | Error | Autorización denegada porque se ha aplicado la regla de degradación authzNone al recurso especificado. | Informar al usuario | n/a | Sí | Sí | Sí desde la versión 3.2 |
| Z500 | Error | Error interno. <br> <br> Nota: este es el &quot;Error de autenticación genérico&quot; y &quot;Error de autenticación interna&quot; heredados. Este error se eliminará gradualmente. | Utilice AccessEnablerDebug e inspeccione los registros de depuración (salida de console.log) para determinar qué ha fallado. | n/a | Sí | Sí | Sí desde la versión 3.2 |
| P100 | Error | Error de preautorización. Lo más probable es que se deba a que se ha solicitado autorización para demasiados recursos. | - NO utilice más del número máximo de recursos permitidos. <br> <br>: póngase en contacto con el soporte de autenticación de Adobe Pass para buscar o configurar el número máximo de recursos permitidos. | n/a | Sí desde la versión 3.0 | Sí | Sí desde la versión 3.2 |
| IS2XX | Error | Estos códigos de error se devuelven cuando los datos de respuesta del extremo del servidor de individualización tienen un formato no válido o no contienen la información de individualización necesaria. | Abra un ticket usando Zendesk e informe al usuario que el sistema no está disponible temporalmente | n/a | Sí desde la versión 3.0 | n/a | n/a |
| IS4XX | Error | Estos códigos de error se devuelven en caso de fallo del extremo del servidor de individualización 4XX - es el código de estado HTTP de la respuesta. | Abra un ticket usando Zendesk e informe al usuario que el sistema no está disponible temporalmente | n/a | Sí desde la versión 3.0 | n/a | n/a |
| IS5XX | Error | Estos códigos de error se devuelven en caso de fallo del extremo del servidor de individualización 5XX - es el código de estado HTTP de la respuesta. | Abra un ticket usando Zendesk e informe al usuario que el sistema no está disponible temporalmente | n/a | Sí desde la versión 3.0 | n/a | n/a |
| IS0 | Error | Este código se devuelve cuando el extremo del servidor de individualización no respondió en absoluto y, por lo tanto, se ha agotado el tiempo de espera de la conexión | Abra un ticket usando Zendesk e informe al usuario que el sistema no está disponible temporalmente | n/a | Sí desde la versión 3.0 | n/a | n/a |
| LS011 | Advertencia | AccessEnabler está utilizando un almacenamiento volátil debido a problemas de LSO / LocalStorage y problemas de WebStorage (o no disponibilidad). <br> La autenticación/autorización de <br> no se mantiene más allá de la página actual. Cada carga de página obliga al usuario a autenticarse. Los TTL configurados no se aplican en todas las recargas de página. | - Informar al usuario de las limitaciones. <br> <br>: informe al usuario sobre cómo aumentar el espacio de almacenamiento disponible. <br> <br>: como alternativa, cierre la sesión para borrar el almacenamiento. | - Aumentar el almacenamiento. <br> <br>: cierre la sesión para borrar el almacenamiento. | Sí | n/a | n/a |

<br>

## Informe de errores original {#original-error-reporting}

En esta sección se describe el sistema original de informe de errores, junto con los códigos de error originales. En el sistema original de informes de errores, AccessEnabler pasa errores a estas dos funciones de devolución de llamada: `setAuthenticationStatus()` después de una llamada a `checkAuthentication()`; `tokenRequestFailed()`, después del error de una llamada a `checkAuthorization()` o `getAuthorization()`.

Los informes de errores y las API de estado originales siguen funcionando exactamente como antes. Sin embargo, en adelante, las API de informes de errores originales no se actualizarán. Todos los informes de errores nuevos y las actualizaciones de los errores antiguos se reflejarán SOLAMENTE en el nuevo [sistema de informes de errores avanzados](#advanced-error-reporting).


Para ver ejemplos del uso del sistema de informes de errores original, consulte las funciones [JavaScript API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) y [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#token-request-failed-error-msg), [iOS/tvOS API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setAuthNStatus)y [tokentRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#tokenReqFailed), [Android API Reference](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus) y [tokenRequestFailed()](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### Códigos de error de devolución de llamada original {#original-callback-error-codes}

| **Errores genéricos** | |
|---|---|
| Error interno | Se ha producido un error del sistema al intentar procesar la solicitud. |
| Error de proveedor no seleccionado | Se produce cuando el cliente cancela en el cuadro de diálogo de selección de proveedores. |
| Error de proveedor no disponible | Se produce cuando no hay proveedores disponibles. |
|  |  |
| **Errores de autenticación** | |
| Error de autenticación genérica | Se devuelve cuando el motivo no se conoce o no puede publicarse. |
| Error de autenticación interna | Se ha producido un error del sistema al intentar autenticarse. |
| Error de usuario no autenticado | El usuario no está autenticado. |
|  |  |
| **Errores de autorización** |  |
| Error de autorización genérica | Se devuelve cuando el motivo no se conoce o no puede publicarse. |
| Error de autorización interna | Se ha producido un error del sistema al intentar autorizar. |
| Error de usuario no autorizado | El cliente no tiene autorización para ver el contenido solicitado. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
