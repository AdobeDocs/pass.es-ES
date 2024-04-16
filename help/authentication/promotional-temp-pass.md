---
title: Pase temporal promocional
description: Pase temporal promocional
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: c88fba76b1805dd88fccb68b13bde11b1b8ea181
workflow-type: tm+mt
source-wordcount: '1510'
ht-degree: 0%

---

# Pase temporal promocional {#promotional-temp-pass}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Resumen de funciones {#feature-summary}

El pase temporal promocional permite a los programadores ofrecer acceso temporal a su contenido protegido para usuarios que no tengan credenciales de cuenta con una MVPD.

Promotional Temp Pass está diseñado para utilizarse para ejecutar campañas promocionales en las que un usuario, después de proporcionar información de identificación válida (por ejemplo, una dirección de correo electrónico) al Programador, podrá consumir un **número predefinido de títulos de VOD diferentes durante un período de tiempo predefinido**.

>[!IMPORTANT]
>
>El Adobe no almacena ninguna información de identificación personal (PII). Por lo tanto, el programador debe establecer un hash con la información proporcionada por el usuario único en las API de autenticación de Adobe Pass.

El pase temporal promocional se crea sobre el [Pase temporal](/help/authentication/temp-pass.md) función, lo que significa que incluye toda la funcionalidad Pase temporal.

Una vez que se supera el número máximo predefinido de títulos de VOD o el período de tiempo predefinido, ese usuario no podrá acceder al contenido en el mismo dispositivo ni utilizar la misma información de identificador de usuario (por ejemplo, la dirección de correo electrónico) hasta que se borren los tokens de autorización del servidor de autenticación de Adobe Pass.

>[!NOTE]
>
>Pase temporal forma parte del paquete Flujo de trabajo Premium. Póngase en contacto con su representante de ventas de Adobe Pass si está interesado en utilizar esta funcionalidad.

## Comparación de Pase temporal y Pase temporal promocional {#tp-ptp-comparison}

| Pase temporal | Pase temporal promocional |
|----------------------------------|----------------------------------------------------------------------------------------|
| Acceso al contenido <ul><li>basado en el tiempo</li></ul> | Acceso al contenido <ul><li>basado en el tiempo</li><li>en función del número de recursos</li></ul> |
| Seguridad de acceso basada en <ul><li>ID de dispositivo</li></ul> | Seguridad basada en <ul><li>ID de dispositivo</li><li>hash over proporcionó información sobre el identificador de usuario (por ejemplo, correo electrónico)</li></ul> |
| API de error de cliente disponible | API de error de cliente disponible |
| Restablecimiento/Depuración disponible | Restablecimiento/Depuración disponible |

## Detalles de funciones {#feature-details}

Esta función permite a los usuarios acceder al contenido promocional desde un dispositivo específico (teléfono y tableta) después de haber proporcionado información única, como la dirección de correo electrónico, en la aplicación del programador.

El programador proporciona un hash sobre la PII del usuario en las API de autenticación y autorización. Este hash se utilizará junto con el ID del dispositivo para generar una clave única para identificar al usuario y al dispositivo.

En función del ID del dispositivo y de la información que proporcionó el usuario, y siguiendo la siguiente lógica, la autenticación de Adobe Pass determinará si el usuario está en una versión de prueba nueva o en una existente:

* nuevo hash para la información proporcionada por el usuario (por ejemplo, correo electrónico), nuevo ID de dispositivo => nueva versión de prueba
* hash existente sobre la información proporcionada por el usuario (por ejemplo, correo electrónico), nuevo id. de dispositivo => prueba existente (con información proporcionada por el usuario de hash sobre existente (por ejemplo, correo electrónico))
* nuevo hash con información proporcionada por el usuario (por ejemplo, correo electrónico), ID de dispositivo existente => versión de prueba existente (con ID de dispositivo existente)
* hash existente sobre la información proporcionada por el usuario (por ejemplo, correo electrónico), ID de dispositivo existente => versión de prueba existente

>[!NOTE]
>La validación y el hash de la información proporcionada por el usuario la gestiona el programador, no el Adobe.

**La función Promotional Temp Pass se puede configurar en función de las siguientes propiedades:**

* Clave de información proporcionada por el usuario (por ejemplo, correo electrónico)
* Número de recursos que el usuario tiene derecho a consumir
* TTL: intervalo de tiempo en el que el usuario tiene derecho a consumir el número configurado de recursos

### Metadatos del usuario {#user-metadata}

Para facilitar la implementación de la aplicación del Programador, lo siguiente **se expone la información de metadatos del usuario** en el Promotional Temp Pass, con las claves correspondientes (para activar las claves, póngase en contacto con tve-support@adobe.com):

* **remaining_recursos**: el número de recursos restantes que el usuario actual tiene derecho a consumir
* **used_assets**: la lista de recursos que el usuario actual ya ha consumido
* **expiration_date**: la fecha de caducidad del usuario actual

### ¿Cómo se calcula el tiempo de visualización? {#compute-viewing-time}

La cantidad de tiempo que un pase temporal sigue siendo válido no se correlaciona con la cantidad de tiempo que un usuario emplea para ver contenido en la aplicación del programador. Tras la solicitud inicial del usuario para obtener autorización a través de Promotional Temp Pass, se calcula un tiempo de caducidad añadiendo el tiempo de solicitud actual inicial al TTL (intervalo de tiempo de duración) especificado por el Programador.

### Autenticación y autorización {#authn-authz}

Para los flujos Promotional Temp Pass, la autenticación y la autorización no se comunican con una MVPD real, **por lo tanto, todas las solicitudes de autorización se procesarán correctamente** siempre que se cumplan todas estas condiciones:

* los tokens de autorización son válidos para los recursos especificados
* número de **used_assets** es inferior al límite establecido por el programador
* **expiration_date** el valor es posterior a la fecha actual.

### Comportamiento de comprobación preliminar {#preflight-beh}

Cuando se realiza una solicitud de comprobación preliminar o de autorización previa para una MVPD de pase temporal promocional, la respuesta de comprobación preliminar correspondiente devuelta contendrá toda la lista de recursos de la solicitud de comprobación preliminar como comprobación preliminar correcta.

La lógica detrás de esto es: las condiciones de autorización de la Promoción Temp Pass se basan en el límite de tiempo y número de recursos en lugar de en recursos específicos. Más específicamente, siempre y cuando se cumpla la restricción de tiempo y no se supere el límite de recursos, se autorizarán los recursos llamados.

### SSO {#sso}

SSO no está habilitado para instancias de Promotional Temp Pass, que está configurado con &quot;Autenticación por solicitante&quot; habilitado. Esto significa que cuando el usuario cambia de la aplicación A a otra aplicación B que están integradas con el mismo pase temporal promocional, el usuario no iniciará sesión automáticamente.

### Cerrar sesión {#logout}

Todos los tokens de un dispositivo se eliminan al cerrar la sesión. Por lo tanto, cambiar del pase temporal promocional a una MVPD regular seleccionada por el usuario no debería depender de esta implementación. La recomendación es utilizar el `setSelectedProvider(null)` para borrar el estado de la aplicación y reiniciar el flujo de autenticación, que ofrece una mejor experiencia de usuario.

### Diagrama de flujo de pase temporal promocional {#promo-tempass-flowdia}

![Diagrama de flujo de pase temporal promocional](assets/promo-temp-pass-flow.png)

*Figura: Flujo de pase temporal promocional*

## Implementar pase temporal promocional {#impl-promo-tempass}

El pase temporal promocional requiere las siguientes funcionalidades del lado del cliente:

* **Información de identificador de usuario, por ejemplo, propagación de direcciones de correo electrónico** (envío de la dirección de correo electrónico del usuario en los flujos de autenticación y autorización). La autenticación de Adobe Pass requiere el correo electrónico para enlazar los tokens de autenticación y autorización (como en el caso del `device_ID`, requerido en todas las llamadas).
* **Forzar autenticación** - permitiendo al programador forzar un flujo de autenticación cuando el usuario ya está autenticado. Esta funcionalidad es necesaria para forzar una actualización de los metadatos del usuario (la clave de metadatos del usuario) **used_assets** contiene el número de recursos disponibles) cada vez que se inicia la aplicación. Dado que el usuario puede iniciar sesión en varios dispositivos, los metadatos de usuario presentes en el dispositivo durante el inicio de la aplicación no son fiables y es necesario actualizarlos para reflejar el estado actual de ese usuario específico (identificado por dirección de correo electrónico).


>[!IMPORTANT]
>La autenticación forzada solo es posible en iOS y Android.
>La autenticación de Adobe Pass no tiene un mecanismo integrado para detener la transmisión gratuita después de que hayan pasado los minutos X. La autenticación de Adobe Pass dejará de emitirse **autorización** y **medios cortos** tokens una vez que el usuario consume los recursos libres de Y. Depende de los programadores restringir el acceso una vez que caduque el pase temporal promocional.

## Seguridad {#security}

>[!IMPORTANT]
>El Adobe no almacena ninguna información de identificación personal (PII). Por lo tanto, el programador debe establecer un hash con la información proporcionada por el usuario único en las API de autenticación de Adobe Pass.

**Hash de la información del identificador de usuario**

El Adobe recomienda utilizar la variable **SHA-2** familia o sus características específicas **SHA-256**, **SHA-512** funciona en los datos antes de enviarse al Adobe.

Por ejemplo, **SHA-256** sobre **&quot;user@domain.com&quot;** es **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**.

## Restablecer o purgar pase temporal promocional {#reset-promo-tempass}

Ciertas reglas comerciales requieren una depuración regular de Promotional Temp Pass. Para ello, la autenticación de Adobe Pass proporciona a los programadores una *público* API web, que se describe a continuación:

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protocolo: **https**</li><li>Host:<ul><li>Versión: **mgmt.auth.adobe.com**</li><li>Precuación: **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Ruta: **/reset-tempass/v2/reset**</li><li>Parámetros de consulta: **device_id=all&amp;requestor_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>encabezados: ApiKey: **1232293681726481**</li> <li>Respuesta:<ul><li>Correcto: **HTTP 204**</li><li>Error: **HTTP 400** para solicitudes incorrectas, **HTTP 401** si no se especifica ApiKey, **HTTP 403** si ApiKey no es válida</li></ul></li></ul> |

Además de los requisitos para purgar el pase temporal, el pase temporal promocional utiliza el hash para transmitir la información del identificador de usuario enviada como **generic_data** sobre la autenticación y la autorización para la depuración.

Se envía el hash, no el JSON completo:

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Clientes admitidos {#supported-clients}

| Clientes de autenticación de Adobe Pass | Pase temporal promocional | Herramienta Restablecer | Admite código de respuesta dedicado/error de cliente |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| Habilitador de acceso JS | SÍ | SÍ | SÍ (a partir de la versión 3.0.0) |
| IOS de cliente nativo | SÍ | SÍ | SÍ (a partir de la versión 1.10) |
| Android de cliente nativo | SÍ | SÍ | SÍ |
| API sin cliente | SÍ | SÍ | NO |


## Limitaciones {#limitations}

Esta sección describe las limitaciones que se aplican a la implementación actual de Promotional Temp Pass.

### Sin Cliente {#lim-clientless}

**Dispositivos inteligentes sin un ID de dispositivo único**

No todas las aplicaciones de dispositivos inteligentes pueden proporcionar un ID de dispositivo único. A falta de uno, la autenticación de Adobe Pass puede utilizar el UUID generado por el servicio de código de registro de Adobe como ID de dispositivo único. Esto significa que, cuando el usuario cierra la sesión, se eliminan los tokens de autenticación y autorización. Una vez que el usuario intente autenticarse de nuevo, esta vez con información de usuario diferente (por ejemplo, correo electrónico), el usuario podrá volver a autorizar. El Adobe recomienda añadir un flujo de interfaz de usuario que no permita a los usuarios &quot;engañar&quot; al sistema y añadir lógica para determinar si se trata de un nuevo usuario que solicita una prueba o una prueba existente.

**Restablecimiento/depuración de Pase temporal**

Reset Temp Pass for Clientless no está disponible en los casos específicos de Xbox360 y Xbox One, porque estas plataformas requieren un análisis de ID de dispositivo adicional, no es posible en la herramienta Reset Temp Pass.

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
