---
title: Información general del SDK para Android
description: Información general del SDK para Android
exl-id: a1d98325-32a1-4881-8635-9a3c38169422
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2720'
ht-degree: 0%

---

# Información general del SDK para Android {#android-sdk-overview}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>


## Introducción {#intro}

Android AccessEnabler es una biblioteca Java Android que permite a las aplicaciones móviles utilizar la autenticación de Adobe Pass para los servicios de derechos de TV Everywhere. Una implementación de Android consiste en la interfaz AccessEnabler que define la API de asignación de derechos y un protocolo EntitlementDelegate que describe las llamadas de retorno que déclencheur la biblioteca. Se hace referencia a la interfaz junto con el protocolo con un nombre común: la biblioteca de Android AccessEnabler.

## Requisitos de Android {#reqs}

Para conocer los requisitos técnicos actuales relacionados con la plataforma Android y la autenticación de Adobe Pass, consulte [Requisitos de plataforma/dispositivo/herramienta](#android), o consulte las notas de la versión incluidas con la descarga del SDK para Android.

## Explicación de los flujos de trabajo de cliente nativos {#native_client_workflows}

Los flujos de trabajo de cliente nativos suelen ser los mismos o muy similares a los de los clientes de autenticación de Adobe Pass basados en el explorador. Sin embargo, hay algunas excepciones, como se describe a continuación.

- [Flujo de trabajo posterior a la inicialización](#post-init)
- [Flujo de trabajo de autenticación inicial genérica](#generic)
- [Flujo de trabajo de cierre](#logout)



### Flujo de trabajo posterior a la inicialización {#post-init}

Todos los flujos de trabajo de derechos admitidos por AccessEnabler suponen que ha llamado anteriormente a [`setRequestor()`](#setRequestor) para establecer su identidad. Esta llamada se realiza para proporcionar el ID de solicitante solo una vez, normalmente durante la fase de inicialización/configuración de la aplicación.


Con los clientes nativos (por ejemplo, Android), después de la llamada inicial a [`setRequestor()`](#setRequestor), tiene la opción de cómo proceder:

- Puede empezar a realizar llamadas de asignación de derechos inmediatamente y permitir que se pongan en cola silenciosamente, si es necesario.

- O bien, puede recibir una confirmación del éxito/fracaso de [`setRequestor()`](#setRequestor) implementando la llamada de retorno setRequestorComplete().

- O bien, haga ambas cosas.

Depende de usted si desea esperar la notificación del éxito de [`setRequestor()`](#setRequestor) o confiar en el mecanismo de cola de llamadas de AccessEnabler. Dado que todas las solicitudes de autorización y autenticación subsiguientes necesitan el ID del solicitante y la información de configuración asociada, la variable [`setRequestor()`](#setRequestor) bloquea de forma efectiva todas las llamadas de API de autenticación y autorización hasta que se complete la inicialización.



### Flujo de trabajo de autenticación inicial genérica {#generic}

El propósito de este flujo de trabajo es iniciar sesión en un usuario con su MVPD.  Después de un inicio de sesión correcto, el servidor back-end emite al usuario un token de autenticación. Aunque la autenticación se suele realizar como parte del proceso de autorización, a continuación se describe cómo puede funcionar la autenticación de forma aislada y no se incluye ningún paso de autorización.

Tenga en cuenta que, aunque el siguiente flujo de trabajo de cliente nativo difiere del flujo de trabajo de autenticación típico basado en explorador, los pasos 1-5 son los mismos para los clientes nativos y los basados en explorador:

1. La página o el reproductor inicia el flujo de trabajo de autenticación con una llamada a [getAuthentication()](#getAuthN), que comprueba si hay un token de autenticación en caché válido. Este método tiene un `redirectURL` parámetro; si no proporciona un valor para `redirectURL`, después de una autenticación correcta, el usuario vuelve a la dirección URL desde la que se inicializó la autenticación.
1. AccessEnabler determina el estado de autenticación actual. Si el usuario está autenticado actualmente, AccessEnabler llama a su `setAuthenticationStatus()` función de llamada de retorno, pasando un estado de autenticación que indica éxito (Paso 7 a continuación).
1. Si el usuario no está autenticado, AccessEnabler continúa el flujo de autenticación determinando si el último intento de autenticación del usuario se realizó correctamente con una MVPD determinada. Si se almacena en caché un ID de MVPD Y la variable `canAuthenticate` el indicador es verdadero O se seleccionó una MVPD usando [`setSelectedProvider()`](#setSelectedProvider)Sin embargo, no se le pide al usuario el cuadro de diálogo de selección de MVPD. El flujo de autenticación sigue utilizando el valor almacenado en caché de la MVPD (es decir, la misma MVPD que se utilizó durante la última autenticación correcta). Se realiza una llamada de red al servidor back-end y se redirige al usuario a la página de inicio de sesión de MVPD (paso 6 a continuación).
1. Si no se almacena en caché ningún ID de MVPD Y no se seleccionó ninguna MVPD usando [`setSelectedProvider()`](#setSelectedProvider) O el `canAuthenticate` Si el indicador se establece en false, la variable [`displayProviderDialog()`](#displayProviderDialog) se llama a la devolución de llamada. Esta llamada de retorno indica a su página o reproductor que cree la interfaz de usuario que presenta al usuario una lista de MVPD para elegir. Se proporciona una matriz de objetos MVPD, que contiene la información necesaria para crear el selector MVPD. Cada objeto MVPD describe una entidad MVPD y contiene información como el ID de la MVPD (por ejemplo, XFINITY, AT\&amp;T, etc.) y la URL donde se puede encontrar el logotipo de MVPD.
1. Una vez que se selecciona una MVPD en particular, su página o reproductor debe informar al AccessEnabler de la elección del usuario. Para los clientes que no son de Flash, una vez que el usuario selecciona la MVPD deseada, se informa al AccessEnabler de la selección del usuario mediante una llamada a [`setSelectedProvider()`](#setSelectedProvider) método. En su lugar, los clientes de Flash distribuyen un `MVPDEvent` del tipo &quot;`mvpdSelection`&quot;, pasando el proveedor seleccionado.
1. Para aplicaciones Android, si com.android.chrome está disponible, la URL de autenticación se cargará en fichas personalizadas de Chrome.
1. A través de las fichas personalizadas de Chrome, el usuario llega a la página de inicio de sesión de la MVPD e introduce sus credenciales. Tenga en cuenta que durante esta transferencia se producen varias operaciones de redirección.
1. Cuando las fichas personalizadas de Chrome detectan que una dirección URL coincide con el esquema (adobepass://) y el vínculo profundo del recurso &quot;redirect\_uri&quot; (es decir, adobepass://com.adobepass ), AccessEnabler recupera el token de autenticación real de los servidores back-end. Tenga en cuenta que las direcciones URL de redireccionamiento finales no son válidas y no están pensadas para que las pestañas personalizadas de Chrome las carguen. Solo el SDK debe interpretarlos como una señal de que el flujo de autenticación se ha completado.
1. AccessEnabler informa a la aplicación de que se ha completado el flujo de autenticación. AccessEnabler llama al método [`setAuthenticationStatus()`](#setAuthNStatus) llamada de retorno con un código de estado de 1, que indica que se ha realizado correctamente. Si se produce un error durante la ejecución de estos pasos, la variable [`setAuthenticationStatus()`](#setAuthNStatus) la llamada de retorno se activa con un código de estado de 0, junto con un código de error correspondiente, que indica un error de autenticación.

### Flujo de trabajo de cierre {#logout}

Para los clientes nativos, los cierres de sesión se gestionan de forma similar al proceso de autenticación descrito anteriormente. Siguiendo este patrón, el AccessEnabler abre las fichas personalizadas de Chrome y carga la URL del punto final de cierre de sesión en el servidor back-end.



**Nota:** Si se cierra la sesión de un programador/MVPD, se borrará el almacenamiento subyacente de ese MVPD específico, incluidos todos los demás tokens de autenticación del programador obtenidos mediante SSO en ese dispositivo. No se eliminarán los tokens obtenidos para otras MVPD o no mediante SSO.


## Tokens {#tokens}

- [Definiciones y uso](#definitions)
- [Directrices de almacenamiento en caché](#caching)
- [Persistencia](#persistence)
- [Formato](#format)
- [Enlace de dispositivo](#device_binding)



### Definiciones y uso {#definitions}

La solución de asignación de derechos de autenticación de Adobe Pass gira en torno a la generación de datos específicos (tokens) que la autenticación de Adobe Pass genera al finalizar correctamente los flujos de trabajo de autenticación y autorización. Estos tokens se almacenan localmente en el dispositivo Android del cliente.

Los tokens tienen una duración limitada; tras la caducidad, los tokens deben volver a emitirse reiniciando los flujos de trabajo de autenticación o autorización.

Existen tres tipos de tokens emitidos durante los flujos de trabajo de asignación de derechos:

- **Token de autenticación** : el resultado final del flujo de trabajo de autenticación de usuarios será un GUID de autenticación que AccessEnabler puede utilizar para realizar consultas de autorización en nombre del usuario. Este GUID de autenticación tendrá asociado un valor de tiempo de vida (TTL) que puede diferir de la sesión de autenticación del usuario. La autenticación de Adobe Pass genera un token de autenticación al enlazar el GUID de autenticación al dispositivo que inicia las solicitudes de autenticación.
- **Token de autorización** - Concede acceso a un recurso protegido específico identificado por un único `resourceID`. Consiste en una concesión de autorización expedida por la parte que autoriza junto con el original `resourceID`. Esta información está enlazada al dispositivo que inicia la solicitud.
- **Token de medios de corta duración** : AccessEnabler concede acceso a la aplicación de alojamiento para un recurso determinado devolviendo un token de medios de corta duración. Este token se genera en función del token de autorización adquirido anteriormente para ese recurso en particular. Además, este token no está enlazado al dispositivo y la duración asociada es considerablemente más corta (valor predeterminado: 5 minutos).

Una vez que la autenticación y la autorización se hayan realizado correctamente, la autenticación de Adobe Pass emitirá tokens de autenticación, autorización y medios de corta duración. Estos tokens deben almacenarse en caché en el dispositivo del usuario y utilizarse durante la duración de sus CICLOS de vida asociados.



### Directrices de almacenamiento en caché {#caching}

- Testigo de autenticación
- Token de autorización
- Token de medios


#### Testigo de autenticación

- **AccessEnabler 1.6 y versiones posteriores** - La forma en que se almacenan en caché los tokens de autenticación en el dispositivo depende de la &quot;**Autenticación por solicitante&quot;** Indicador asociado con la MVPD actual:


1. Si la función &quot;Autenticación por solicitante&quot; está *inhabilitado*, se almacenará un único token de autenticación localmente en la mesa de trabajo global. Este token se compartirá entre todas las aplicaciones que estén integradas con la MVPD actual.
1. Si la función &quot;Autenticación por solicitante&quot; está *activado*, se asociará explícitamente un token con el programador que realizó el flujo de autenticación (el token no se almacenará en la mesa de trabajo global, sino en un archivo privado visible solo para la aplicación de ese programador). Más específicamente, se deshabilitará el inicio de sesión único (SSO) entre diferentes aplicaciones; el usuario deberá realizar el flujo de autenticación explícitamente al cambiar a una nueva aplicación (siempre que el Programador de la segunda aplicación esté integrado con la MVPD actual y que no exista ningún token de autenticación para ese Programador en la caché local).

   **Nota:** Nota técnica de AEM 1.6 Google GSON: [Cómo resolver dependencias Gson](https://tve.zendesk.com/entries/22902516-Android-AccessEnabler-1-6-How-to-resolve-Gson-dependencies)

- **AccessEnabler 1.7** : Este SDK introduce un nuevo método de almacenamiento de tokens, que permite varios contenedores de MVPD de programador y, por lo tanto, varios tokens de autenticación. A partir de AEM 1.7, se utiliza el mismo diseño de almacenamiento tanto para el escenario &quot;Autenticación por solicitante&quot; como para el flujo de autenticación normal. La única diferencia entre los dos es la forma en que se realiza la autenticación: &quot;Autenticación por solicitante&quot; contiene una nueva mejora (autenticación pasiva) que hace posible que AccessEnabler realice una autenticación de canal de retorno, basada en la existencia de un token de autenticación en el almacenamiento (para un programador diferente). El usuario solo tiene que autenticarse una vez, y esta sesión se utilizará para obtener tokens de autenticación en aplicaciones posteriores. Este flujo de canal posterior tiene lugar durante el [`setRequestor()`](#setRequestor) llama a y es mayormente transparente para el programador. Sin embargo, hay un requisito importante aquí: el programador DEBE llamar a [`setRequestor()`](#setRequestor) desde el hilo de la IU principal y desde una actividad.


#### Token de autorización

En cualquier momento dado, AccessEnabler almacena en caché ÚNICAMENTE UN token de autorización por recurso. Puede haber varios tokens de autorización en la caché, pero están asociados a diferentes recursos. Siempre que se emite un nuevo token de autorización y ya existe uno antiguo para el mismo recurso, el nuevo token sobrescribe el valor almacenado en caché existente.



#### Token de medios

El token de medios de corta duración NO debe almacenarse en caché. El token de medios debe recuperarse del servidor cada vez que se llama a una API de autorización, ya que está restringido a un solo uso.



### Persistencia {#persistence}

Los tokens deben ser persistentes en ejecuciones consecutivas de la misma aplicación. Esto significa que una vez que se han adquirido los tokens de autenticación y autorización y el usuario cierra la aplicación, los mismos tokens están disponibles para la aplicación cuando el usuario vuelve a abrir la aplicación. Además, es deseable que estos tokens sean persistentes en varias aplicaciones. En otras palabras, después de que un usuario utilice una aplicación para iniciar sesión con un proveedor de identidad específico (obteniendo correctamente tokens de autenticación y autorización), los mismos tokens se pueden utilizar a través de una aplicación diferente y ya no se le piden credenciales al usuario al iniciar sesión a través del mismo proveedor de identidad.



Este tipo de flujo de trabajo de autenticación/autorización sin problemas es lo que hace que la solución de autenticación de Adobe Pass sea una verdadera implementación de TV en todas partes. Desde el punto de vista de la ingeniería, la biblioteca Android AccessEnabler soluciona los problemas del uso compartido de datos entre aplicaciones almacenando los datos de tokens en un archivo de base de datos ubicado en un almacenamiento externo. Estos recursos compartidos de nivel de sistema proporcionan los ingredientes clave que permiten la implementación del caso de uso de tokens persistentes deseado:

- Compatibilidad con almacenamiento estructurado: el almacenamiento de tokens de autenticación de Adobe Pass no es solo una estructura de memoria lineal simple similar a un búfer. Proporciona un mecanismo de almacenamiento similar a un diccionario que permite la indexación de datos en función de los valores de clave especificados por el usuario.
- Compatibilidad con la persistencia de datos mediante el sistema de archivos subyacente: el contenido del archivo de base de datos se mantiene de forma predeterminada y los datos se guardan en la memoria externa del dispositivo.



Una vez que un token particular se coloca en la caché de tokens, la biblioteca AccessEnabler comprueba su validez en diferentes momentos.  Un token válido se define como:

- El TTL del token no ha caducado
- El emisor del token se incluye en la lista de proveedores de identidad permitidos



A partir de AccessEnabler 1.7, el almacenamiento de tokens puede admitir varias combinaciones de programador-MVPD, basándose en una estructura de mapas anidada de varios niveles que puede contener varios tokens de autenticación. Este nuevo almacenamiento no afecta a la API pública de AccessEnabler de ninguna manera y no requiere cambios por parte del programador. Este es un ejemplo que ilustra esta funcionalidad más reciente:

1. Abra App1 (desarrollado por Programmer1).
1. Autenticar con MVPD1 (que está integrado con Programmer1).
1. Suspenda/cierre la aplicación actual y abra App2 (desarrollada por Programmer2).
1. Supongamos que Programmer2 no está integrado con MVPD2; por lo tanto, el usuario NO será autenticado en App2.
1. Autentique con MVPD2 (que está integrado con Programmer2) en App2.
1. Cambie a App1; el usuario se autenticará con Programmer1.

En versiones anteriores de AccessEnabler, el paso 6 hacía que el usuario no estuviera autenticado, ya que el almacenamiento de tokens anteriormente solo admitía un token de autenticación.


**NOTA:** Si se cierra la sesión de un programador/MVPD, se borrará el almacenamiento subyacente, incluidos todos los demás tokens de autenticación de programador del dispositivo con SSO. No se eliminarán los tokens obtenidos para otras MVPD o no mediante SSO. Cancelación del flujo de autenticación (invocación) [`setSelectedProvider(null)`](#setSelectedProvider)) NO borrará el almacenamiento subyacente, sino que solo afectará al intento de autenticación actual del Programador / MVPD (borrando la MVPD para el Programador actual).


Otra función relacionada con el almacenamiento que se incluye en AccessEnabler 1.7 permite importar tokens de autenticación de áreas de almacenamiento antiguas. Este &quot;Importador de tokens&quot; ayuda a lograr la compatibilidad entre versiones consecutivas de AccessEnabler, manteniendo el estado de SSO incluso cuando se actualiza la versión de almacenamiento.

El importador se ejecuta durante el [`setRequestor()`](#setRequestor) y se ejecuta en los dos casos siguientes (suponiendo que no hay ningún token de autenticación válido para el programador actual en el almacenamiento actual):

- La primera instalación de una aplicación 1.7 desarrollada por un programador específico
- Ruta de actualización a un futuro AccessEnabler que utilice un nuevo almacenamiento

La operación de importación es transparente para el programador y no requiere ningún cambio de código en la aplicación cliente.



### Formato {#format}

- [Testigo de autenticación](#authn_token)
- [Token de autorización](#authz_token)
- [Token de medios corto](#short_media_token)
- [Enlace de dispositivo](#device_binding)

#### Testigo de autenticación {#authn_token}

La siguiente lista presenta el formato del token de autenticación:

```XML
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthenticationToken>
        <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
        <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>   
    </simpleAuthenticationToken>
```


#### Token de autorización {#authz_token}

La siguiente lista presenta el formato del token de autorización:

```XML
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthorizationToken>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
        <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>
    </simpleAuthorizationToken>
```


#### Token de medios corto {#short_media_token}

La siguiente lista presenta el formato del token de medios corto.  Este token se expone a la aplicación del programador.  Se pasa a la aplicación del programador al final de un proceso de asignación de derechos correcto:

```XML
    <signatureInfo>signature<signatureInfo>
    <shortAuthorizationToken>
      <sessionGUID>session_guid</sessionGUID>
      <requestorID>requestor_id</requestorID>
      <resourceID>resource_id</resourceID>
      <ttl>ttl_in_ms</ttl>
      <issueTime>issue_time</issueTime>
      <mvpdId>mvpd_id</mvpdId>
      <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
    </shortAuthorizationToken>
```


#### Enlace de dispositivo {#device_binding}

En los listados XML anteriores, observe la etiqueta titulada `simpleTokenFingerprint`. El propósito de esta etiqueta es contener información de individualización de ID de dispositivo nativo. La biblioteca AccessEnabler puede obtener dicha información de individualización y ponerla a disposición de los servicios de autenticación de Adobe Pass durante las llamadas de asignación de derechos. El servicio utilizará esta información e la incrustará en los tokens reales, enlazando así de forma eficaz los tokens a un dispositivo específico. El objetivo final de esto es hacer que los tokens sean intransferibles entre dispositivos.



En los listados XML anteriores, observe la etiqueta titulada simpleTokenFingerprint. El propósito de esta etiqueta es contener información de individualización de ID de dispositivo nativo. La biblioteca AccessEnabler puede obtener dicha información de individualización y ponerla a disposición de los servicios de autenticación de Adobe Pass durante las llamadas de asignación de derechos. El servicio utilizará esta información e la incrustará en los tokens reales, enlazando así de forma eficaz los tokens a un dispositivo específico. El objetivo final de esto es hacer que los tokens sean intransferibles entre dispositivos.



Dado que esta es, obviamente, una función relacionada con la seguridad, esta información es inherentemente &quot;confidencial&quot; desde el punto de vista de la seguridad. Como resultado, esta información debe protegerse contra la manipulación y la escucha. El problema de la escucha se resuelve enviando las solicitudes de autenticación/autorización a través del protocolo HTTPS. La protección contra la manipulación se controla mediante la firma digital de la información de identificación del dispositivo. La biblioteca AccessEnabler calcula un ID de dispositivo a partir de la información proporcionada por el dispositivo y, a continuación, envía el ID de dispositivo &quot;sin cifrar&quot; a los servidores de autenticación de Adobe Pass como parámetro de solicitud.  Los servidores de autenticación de Adobe Pass firman digitalmente el ID del dispositivo con la clave privada de Adobe y lo agregan al token de autenticación que se devuelve al AccessEnabler. Por lo tanto, el ID del dispositivo está enlazado con el token de autenticación.  Durante el flujo de autorización, AccessEnabler vuelve a enviar el ID del dispositivo de forma clara, junto con el token de autenticación.  Un error en el proceso de validación provocará automáticamente un error en los flujos de trabajo de autenticación/autorización.  Los servidores de autenticación de Adobe Pass aplican la clave privada al ID de dispositivo y la comparan con el valor del token de autenticación.  Si no coinciden, se produce un error en el flujo de derechos.


<!--
## Related Information {#related}

- [Android Integration Cookbook](#)
- [Android API](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass Authentication native SDK (Tech Note)](#)
- [AE 1.6: How to resolve Gson dependencies (Tech Note)](#)
-->
