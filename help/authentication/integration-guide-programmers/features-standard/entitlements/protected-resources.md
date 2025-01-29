---
title: Recursos protegidos
description: Recursos protegidos
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Recursos protegidos {#protected-resources}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Los recursos protegidos representan el contenido que los usuarios pueden transmitir y se identifican con valores únicos establecidos a través de acuerdos entre MVPD y los programadores participantes.

Los recursos protegidos siguen una estructura de árbol jerárquica, y cada nivel proporciona una mayor granularidad para la autorización de contenido:

* Red
   * Canal
      * Mostrar
         * Episodio
            * Recurso

## Identificadores de recursos protegidos {#identifiers}

El identificador único del recurso puede tener dos formatos:

* Un formato de cadena simple, como un identificador único de un canal (marca).
* Un formato RSS (RSS) multimedia que contiene información adicional, como el título, las clasificaciones y los metadatos de control parental.

En el caso de un identificador de recurso simple, como &quot;REF30&quot; (se supone que representa un canal), se puede traducir en un identificador de recurso RSS de la siguiente manera:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

En el caso de un identificador de recurso más complejo, el identificador de recurso RSS puede incluir información de clasificación adicional de la siguiente manera:

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

## API DE REST V2 {#rest-api-v2}

Las API de autenticación de Adobe Pass que recuperan las decisiones de autorización y preautorización requieren que la aplicación cliente incluya los identificadores de recursos protegidos como parámetros.

Las decisiones de autorización previa y autorización se pueden recuperar mediante las siguientes API:

* [Recuperar decisiones de preautorización utilizando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Recuperar decisiones de autorización utilizando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulte las secciones **Solicitud** y **Ejemplos** de las API anteriores para comprender el formato requerido para proporcionar los identificadores únicos de los recursos protegidos.

Los identificadores únicos son opacos para la autenticación de Adobe Pass, ya que se pasan directamente a MVPD. Si MVPD no puede reconocer o analizar un identificador de recurso, devuelve un error a Autenticación de Adobe Pass, que vuelve a transmitir el error a la aplicación cliente mediante un [Código de error mejorado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

Para obtener más información acerca de cómo y cuándo integrar las API anteriores, consulte los siguientes documentos:

* [Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
