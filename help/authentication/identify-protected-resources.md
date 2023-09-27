---
title: Identificación de recursos protegidos
description: Identificación de recursos protegidos
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Identificación de recursos protegidos {#identifying-protected-resources}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

Cada solicitud de autorización (o solicitud de comprobación de autorización) debe contener un identificador único para el recurso protegido para el que el usuario solicita acceso. Un recurso protegido puede ser cualquier nivel de contenido autorizado, según lo acordado entre un MVPD y los programadores participantes. Los recursos protegidos potenciales deben encajar en esta estructura de árbol de granularidad cada vez más específica:

- Red
   - Canal
      - Mostrar
         - Episodio
            - Recurso

</br>

## Formato RSS de medios {#media_rss}

Los recursos se pueden identificar mediante una cadena simple (un identificador único para un canal) o se pueden representar en formato RSS de medios (MRSS), tal como se acuerde entre Adobe (o un socio autorizado de autenticación de Adobe Pass) y las MVPD y programadores participantes. La cadena RSS utilizada como especificador de recursos puede incluir información adicional, como clasificaciones y metadatos de control parental.


Si utiliza un identificador de recurso simple, como &quot;TNT&quot;, se supone que representa un canal y se traduce en este especificador de recurso RSS:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


Un especificador más complejo podría incluir, por ejemplo, información de clasificación adicional. Puede pasar toda la cadena RSS a las funciones del Habilitador de acceso que requieren un ID de recurso, como [`getAuthorization()`](/help/authentication/rest-api-reference.md):

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

Los especificadores de recursos son opacos para la autenticación de Adobe Pass; simplemente se pasan a la MVPD. Si el MVPD no reconoce o no puede analizar el especificador de recursos, devuelve un error a la autenticación de Adobe Pass, que devuelve el error a su `tokenRequestFailed()` devolución de llamada.

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
