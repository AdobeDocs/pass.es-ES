---
title: Decisiones
description: Decisiones
exl-id: 1efd70af-8c1d-43c4-87fc-14488d42b23d
source-git-commit: a19f4fd40c9cd851a00f05f82adbabb85edd8422
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 0%

---

# Decisiones {#decisions}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Las decisiones las genera la autenticación de Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) en función de las consultas de autorización o preautorización de MVPD del usuario, lo que determina si se concede o deniega el acceso al [contenido protegido](#protected-resources).

Existen dos tipos de decisiones que se proporcionan, según la API invocada:

* [Decisiones de preautorización](#preauthorization-decisions) que son decisiones informativas.
* [Decisiones de autorización](#authorization-decisions) que son decisiones autorizadas.

## Decisiones de preautorización {#preauthorization-decisions}

La decisión de preautorización es una decisión informativa que permite informar a la aplicación cliente de si MVPD puede permitir o denegar el acceso del usuario a un [recurso protegido](#protected-resources).

El propósito de la preautorización (autorización de comprobación preliminar) es permitir que la aplicación muestre información precisa sobre el contenido que el usuario puede tener derecho a ver. Esto se logra mejorando la interfaz de usuario con indicadores, como los iconos bloqueados o desbloqueados, para reflejar el estado del acceso.

>[!IMPORTANT]
>
> La decisión de preautorización no debe utilizarse de manera autorizada para reproducir recursos, ya que ese es el propósito de una [decisión de autorización](#authorization-decisions).

El uso de la API de preautorización no es obligatorio, la aplicación cliente puede omitirlo si desea presentar un catálogo de recursos sin ningún filtro.

Si la aplicación cliente pretende utilizar esta función, es importante tener en cuenta que las decisiones de preautorización solo se pueden obtener para un número limitado de recursos por solicitud de API, normalmente hasta 5.

>[!IMPORTANT]
> 
> El número máximo de recursos solo se puede aumentar después de alcanzar un acuerdo con los representantes de MVPD y autenticación de Adobe Pass. Una vez acordados, un administrador de su organización o un representante de autenticación de Adobe Pass que actúe en su nombre puede implementar los cambios a través del panel de control de Adobe Pass TVE.
> 
> Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

Las MVPD pueden admitir la preautorización a través de varios mecanismos, cada uno con implicaciones distintas para el rendimiento y el número máximo de recursos que se pueden administrar en una sola solicitud de API.

Para obtener más información sobre los mecanismos existentes que admiten la preautorización, consulte la [Autorización de comprobaciones de MVPD](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md).

>[!IMPORTANT]
>
> En el caso de las MVPD sin compatibilidad total con la autorización de verificación previa, el uso de la autorización previa debe acordarse de antemano con los representantes de las MVPD y los representantes de autenticación de Adobe Pass, ya que puede provocar problemas de rendimiento y tiempos de respuesta más lentos.

## Decisiones de autorización {#authorization-decisions}

La decisión de autorización es una decisión autorizada que permite que la aplicación cliente cumpla con la decisión de MVPD de permitir o denegar el acceso del usuario a un [recurso protegido](#protected-resources).

El propósito de la autorización es permitir que la aplicación reproduzca los recursos solicitados por el usuario, después de la validación de los derechos con el MVPD y de recibir un token de medios de la autenticación de Adobe Pass.

>[!IMPORTANT]
> 
> La autenticación de Adobe Pass recomienda que los programadores utilicen la biblioteca de Media Token Verifier para validar el token de medios incluido en una decisión de autorización, lo que garantiza un acceso seguro antes de iniciar el flujo de vídeo.
> 
> Para obtener más información, consulte la documentación de [Tokens de medios](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md).

El uso de la API de autorización es obligatorio, la aplicación cliente no puede omitir esta fase si desea reproducir recursos que el usuario solicita, ya que requiere verificar con la MVPD que el usuario tiene derecho antes de liberar el flujo.

Es importante tener en cuenta que las decisiones de autorización solo se pueden obtener para un número limitado de recursos por solicitud de API, normalmente 1.

>[!IMPORTANT]
>
> El número máximo de recursos solo se puede aumentar después de alcanzar un acuerdo con los representantes de MVPD y autenticación de Adobe Pass.

## Administración del tiempo de vida de la autorización (TTL) {#authorization-ttl-management}

Tiempo de vida de autorización (TTL) define cuánto tiempo permanece autorizado un recurso antes de tener que volver a autorizarlo. Este periodo de tiempo es limitado y debe acordarse con los representantes de MVPD. Los valores TTL pueden variar en función de lo siguiente:

* Categoría de plataforma (por ejemplo, escritorio, móvil, dispositivos conectados a TV)
* Plataforma específica (por ejemplo, iOS, Android, tvOS, Roku, FireTV)

El TTL de autorización (authZ) se puede ver y cambiar a través del [panel de TVE de Adobe Pass](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) por uno de los administradores de la organización o por un representante de autenticación de Adobe Pass que actúe en su nombre.

Para obtener más información, consulte la [Guía del usuario sobre integraciones de paneles de TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

## Recursos protegidos {#protected-resources}

Los recursos protegidos se refieren a contenido transmisible, identificado por valores únicos definidos a través de acuerdos entre MVPD y los programadores participantes.

Los recursos protegidos siguen una estructura de árbol jerárquica, y cada nivel proporciona una mayor granularidad para la autorización de contenido:

* Red
   * Canal
      * Mostrar
         * Episodio
            * Recurso

>[!IMPORTANT]
>
> La preautorización (autorización de comprobación preliminar) se centra en recursos de nivel de canal con identificadores que tienen una cadena simple o un formato MRSS.
> 
> No se recomienda utilizar recursos con identificadores que incluyan secciones de `CDATA` en caso de preautorización, ya que se utilizan principalmente para recursos de nivel de recurso definidos por un MRSS.

### Identificador de recurso {#resource-identifier}

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

Los identificadores únicos son principalmente opacos para la autenticación de Adobe Pass, sin embargo, los transformadores pueden aplicarse en función de las capacidades y los requisitos de MVPD. Si MVPD no puede reconocer o analizar un identificador de recurso, devuelve un error a Autenticación de Adobe Pass, que posteriormente retransmite el error a la aplicación cliente mediante un [Código de error mejorado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

## API DE REST V2 {#rest-api-v2}

Las decisiones de preautorización se pueden recuperar mediante la siguiente API:

* [Recuperar decisiones de preautorización utilizando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

Las decisiones de autorización se pueden recuperar mediante la siguiente API:

* [Recuperar decisiones de autorización utilizando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulte las secciones **Respuesta** y **Ejemplos** de las API anteriores para comprender la estructura de las decisiones de autorización y preautorización.

Para obtener más información acerca de cómo y cuándo integrar las API anteriores, consulte los siguientes documentos:

* [Flujo de preautorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [Preguntas frecuentes sobre la fase de preautorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)
> [Preguntas frecuentes sobre la fase de autorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
