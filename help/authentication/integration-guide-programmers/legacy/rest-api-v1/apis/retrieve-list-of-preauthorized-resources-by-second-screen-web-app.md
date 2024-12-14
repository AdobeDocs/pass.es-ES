---
title: Recuperar lista de recursos preautorizados por aplicación web de segunda pantalla
description: Recuperar lista de recursos preautorizados por aplicación web de segunda pantalla
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---

# (Heredado) Recuperar lista de recursos autorizados previamente por la aplicación web de segunda pantalla {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Extremos de API de REST {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Una solicitud a la autenticación de Adobe Pass para obtener la lista de recursos autorizados previamente.

Existen dos conjuntos de API: un conjunto para la aplicación de streaming o el servicio de programador y otro para la aplicación web de segunda pantalla. Esta página describe la API para la aplicación AuthN.


| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize/{código de registro} | Módulo AuthN | 1. código de registro </br>    (Componente de ruta de acceso)</br>2.  solicitante (obligatorio)</br>3.  lista de recursos (obligatorio) | GET | XML o JSON que contienen decisiones individuales de preautorización o detalles de error. Consulte los ejemplos siguientes. | 200 - Correcto</br></br>400 - Solicitud incorrecta</br></br>401 - No autorizado</br></br>405 - Método no permitido </br></br>412 - Error de condición previa</br></br>500 - Error interno del servidor |



| Parámetro de entrada | Descripción |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| código de registro | El valor del código de registro proporcionado por el usuario al principio del flujo de autenticación. |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| lista de recursos | Cadena que contiene una lista delimitada por comas de resourceIds que identifica el contenido al que un usuario podría tener acceso y que los extremos de autorización de MVPD reconocen. |


### Respuesta de ejemplo {#sample-response}

**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4`
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
    <resource>
        <id>TestStream1</id>
        <authorized>true</authorized>
    </resource>
    <resource>
        <id>TestStream2</id>
        <authorized>false</authorized>  
        <error>
            <status>403</status>
            <code>authorization_denied_by_mvpd</code>
            <message>User not authorized</message>
            <details>Your subscription package does not include the "TestStream3" channel.</details>
            <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
            <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
            <action>none</action>
        </error>
    <resource>
</resources>
```

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
