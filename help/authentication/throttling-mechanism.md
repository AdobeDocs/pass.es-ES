---
title: Mecanismo de limitación
description: Obtenga información acerca del mecanismo de limitación utilizado en la autenticación de Adobe Pass. Explore una descripción general de este mecanismo en esta página.
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---

# Mecanismo de limitación {#throttling-mechanism}

Todos los clientes de autenticación Pass deben poder acceder a la API de autenticación Pass para cada uno de sus usuarios, según las instrucciones y el caso empresarial.

La autenticación de paso introduce un mecanismo de limitación para garantizar una distribución equitativa de los recursos entre los usuarios de nuestros clientes.

Este mecanismo es importante por varias razones:

- Una de las reglas de oro de la arquitectura de varios inquilinos es que el comportamiento de un usuario no debe afectar a otra persona.
- La limitación de velocidad es importante para las API, ya que es fácil cometer errores al integrarse con una API. Como las API son programáticas, es relativamente fácil enviar accidentalmente más solicitudes de las deseadas.

## Resumen del mecanismo {#mechanism-overview}

### Implementaciones de cliente

La autenticación de paso proporciona directrices y SDK para interactuar con la API, pero no controla cómo la utiliza el cliente. Algunas implementaciones pueden tener una implementación rudimentaria y podrían inundar el servicio con solicitudes de API innecesarias, ya sea de forma accidental o no, lo que podría significar que otros usuarios experimenten ralentizaciones o problemas de capacidad.

El propio servicio debe poder gestionar cualquier capacidad razonable. Pero no importa lo escalable o el rendimiento de un servicio, siempre hay límites. Como tal, el servicio debe tener límites configurados para el número de llamadas aceptadas dentro de un intervalo de tiempo específico.

### Introducción de la restricción

La autenticación de paso se basa en la identificación del usuario y en un algoritmo de limitación de la velocidad en bloque de tokens con valores predefinidos para controlar el acceso de cada usuario al dispositivo a nuestra API.

### Mecanismo de identificación del dispositivo

El mecanismo de regulación propuesto utiliza los dispositivos identificados individualmente, con la ayuda del encabezado &quot;X-Forwarded-For&quot;. Los límites se aplicarán de la misma manera para cada dispositivo.

### Actualizaciones requeridas

Las implementaciones de servidor a servidor deben reenviar las direcciones IP de sus clientes mediante el mecanismo de encabezado &quot;X-Forwarded-For&quot;.

Puede encontrar más detalles sobre cómo pasar el encabezado X-Forwarded-For [aquí](rest-api-cookbook-servertoserver.md).

### Límites y extremos reales

Actualmente, el límite predeterminado permite un máximo de 1 solicitud por segundo., con una ráfaga inicial de 3 solicitudes (asignación única en la primera interacción del cliente identificado, que debería permitir que la inicialización finalice correctamente). Esto no debería afectar a ningún caso comercial normal en todos nuestros clientes.

El mecanismo de restricción se habilitará en los siguientes extremos:

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/.+/profile-requests/.+
- /api/v1/identities
- /reggie/v1/.+/regcode
- /reggie/v1/.+/regcode/.+

### Desambiguación de implementación de SDK

Dado que los clientes que utilizan los SDK proporcionados de autenticación de Adobe Pass no interactúan explícitamente con ningún extremo, esta sección presentará las funciones conocidas, cómo se comportan al encontrar una respuesta de acelerador y las acciones que deben realizarse.

#### setRequestor

Al alcanzar el límite de aceleración utilizando la función `setRequestor` del SDK, el SDK devolverá un código de error CFG429 mediante la llamada de retorno `errorHandler`.

#### getAuthorization

Al alcanzar el límite de aceleración utilizando la función `getAuthorization` del SDK, el SDK devolverá un código de error Z100 mediante la llamada de retorno `errorHandler`.

#### checkPreauthorizedResources

Al alcanzar el límite de aceleración utilizando la función `checkPreauthorizedResources` del SDK, el SDK devolverá un código de error P100 mediante la llamada de retorno `errorHandler`.

#### getMetadata

Al alcanzar el límite de aceleración utilizando la función `getMetadata` del SDK, el SDK devolverá una respuesta vacía mediante la llamada de retorno `setMetadataStatus`.

Para cada detalle de implementación específico, consulte la documentación específica del SDK.

- [Referencia de API de SDK para JavaScript](javascript-sdk-api-reference.md)
- [Referencia de API de SDK para Android](android-sdk-api-reference.md)
- [Referencia de la API de iOS/tvOS](iostvos-sdk-api-reference.md)

### Cambios y respuestas de API

Cuando identificamos que se ha superado el límite, marcamos esta solicitud con un estado de respuesta específico (HTTP 429 Too Many Requests), indicando que ha consumido todos los tokens asignados al dispositivo del usuario (dirección IP) durante el intervalo de tiempo.

La restricción caduca después de un segundo de las primeras 429 respuestas. Cada aplicación que recibe una respuesta 429 debe esperar al menos 1 segundo antes de generar una nueva solicitud.

Todas las aplicaciones de clientes deben gestionar correctamente la respuesta &quot;429 demasiadas solicitudes&quot;.

Este es un ejemplo de mensaje de respuesta 429:

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## Impacto y cambios necesarios

### Pasar el encabezado X-Forwarded-For

Los clientes que utilizan una implementación personalizada (incluidas las de servidor a servidor) para interactuar con la API de autenticación Pass deben asegurarse de que pueden capturar su dirección IP de usuario y reenviarla correctamente, utilizando el encabezado X-Forwarded-For más allá de la API de autenticación Pass.

Ver [aquí](rest-api-cookbook-servertoserver.md) para obtener más detalles.

### Reacción al nuevo código de respuesta

Los clientes que utilizan una implementación personalizada (incluidas las de servidor a servidor) para interactuar con la API de autenticación de paso deben asegurarse de que cualquier llamada posterior realizada después de recibir una 429 demasiadas solicitudes incluya un periodo de espera mínimo de 1 segundo. Este periodo de espera garantiza la oportunidad de cambiar este mecanismo y obtener una respuesta empresarial válida.

## Ejemplo de escenario para regulación

| Tiempo desde la primera solicitud | Respuesta recibida | Explicación |
|--------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| Segundo 0 | La llamada recibe el código de estado correcto | 1 llamadas consumidas desde el límite |
| Segundo 0,3 | La llamada recibe el código de estado correcto | 1 llamada consumida desde el límite y 1 llamada marcada como ráfaga |
| Segundo 0,6 | La llamada recibe el código de estado correcto | 1 llamada consumida desde el límite y 2 llamadas marcadas como ráfaga |
| Segundo 0,9 | La llamada recibe el código de estado correcto | 1 llamada consumida desde el límite y 3 llamadas marcadas como ráfaga |
| Second 1.2 | La llamada recibe el código de estado correcto | 2 llamadas consumidas desde el límite y 3 llamadas marcadas como ráfaga |
| Second 1.4 | La llamada recibe el código de estado 429 | 2 llamadas consumidas desde el límite y 3 llamadas marcadas como ráfaga y 1 llamada recibe &quot;429 Too many requests&quot; (Demasiadas solicitudes) |
| Second 1.6 | La llamada recibe el código de estado 429 | 2 llamadas consumidas desde el límite y 3 llamadas marcadas como ráfaga y 2 llamadas reciben &quot;429 Demasiadas solicitudes&quot; |
| Second 1.8 | La llamada recibe el código de estado 429 | 2 llamadas consumidas desde el límite y 3 llamadas marcadas como ráfaga y 3 llamadas reciben &quot;429 Too many requests&quot; (Demasiadas solicitudes) |
| Segundo 2.1 | La llamada recibe el código de estado correcto | 3 llamadas consumidas desde el límite y 3 llamadas marcadas como ráfaga y 3 llamadas reciben &quot;429 Too many requests&quot; (Demasiadas solicitudes) |
