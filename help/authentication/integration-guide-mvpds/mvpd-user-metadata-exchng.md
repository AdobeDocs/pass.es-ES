---
title: Intercambio de metadatos de usuario de MVPD
description: Intercambio de metadatos de usuario de MVPD
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# Intercambio de metadatos de usuario de MVPD

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#intro-user-metadata-exchange}

Las MVPD mantienen metadatos específicos del usuario sobre sus clientes que, en algunos casos, se comparten con los programadores. El objetivo de la autenticación de Adobe Pass es intermediar en el intercambio de estos &quot;metadatos de usuario&quot;, pero no aplicar ningún tipo de reglas con respecto al intercambio. Las reglas de intercambio son para que los MVPDs trabajen con sus socios programadores.

Los tipos de metadatos de usuario disponibles para Exchange actualmente incluyen los siguientes:

* Código postal
* Calificación máxima (VChip o MPAA)
* ID de usuario
* Identificador del hogar
* ID de canal

Con esta función, las MVPD y los programadores pueden implementar casos de uso especiales, como el control parental. Por ejemplo, un MVPD puede pasar datos de clasificación parental a un programador, que luego utiliza esos datos para filtrar las opciones de visualización disponibles para un usuario.

Puntos clave de metadatos de usuario:

* La MVPD pasa los metadatos del usuario a la aplicación del programador durante los flujos de autenticación y autorización
* La autenticación de Adobe Pass guarda los valores de los metadatos en los tokens AuthN y AuthZ
* La autenticación de Adobe Pass puede normalizar los valores de las MVPD que proporcionan metadatos de usuario en diferentes formatos
* Algunos parámetros se pueden cifrar con la clave del programador
* Los valores específicos están disponibles por Adobe, a través de un cambio de configuración

>[!NOTE]
>
>Los metadatos de usuario son una extensión de los metadatos estáticos (TTL del token de autenticación, TTL del token de autorización e ID de dispositivo) disponibles anteriormente en la autenticación de Adobe Pass.

## Ejemplos {#example-mvpd-user-metadata-exch}

### Control parental {#example-parental-control}

Este ejemplo muestra el intercambio de lo siguiente:

* [Intercambio de metadatos de programador a MVPD](#progr-mvpd-metadata-exch)

* [Flujo de intercambio de metadatos de MVPD a programador](#mvpd-progr-exchange-flow)

### Intercambio de metadatos de programador a MVPD {#progr-mvpd-metadata-exch}

Actualmente, la API de programador, la autenticación de Adobe Pass y los autorizadores de MVPD solo admiten la autorización a nivel de canal. El canal se especifica como una cadena de texto sin formato en la llamada de API getAuthorization() del programador. Esta cadena se propaga hasta el backend de autorización de MVPD:

Desde la aplicación o el sitio del programador, el usuario elige una MVPD compatible con XACML (en este ejemplo, &quot;TNT&quot;). Para obtener información sobre XACML, consulte [Lenguaje de marcado de control de acceso extensible](https://en.wikipedia.org/wiki/XACML){target=_blank}.
La aplicación del programador forma una solicitud de AuthZ que incluye el recurso y sus metadatos.  Este ejemplo incluye una clasificación MPAA de &quot;pg&quot; en el atributo media del elemento channel:

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

La autenticación de Adobe Pass admite realmente una autorización más granular, hasta el nivel de recurso, cuando la admiten tanto la MVPD como el programador. El recurso y sus metadatos son opacos para el Adobe; la intención es establecer un formato estándar para especificar el ID de recurso y los metadatos de una manera normalizada, para enviar ID de recurso a diferentes MVPD.

>[!NOTE]
>
>Si el usuario elige una MVPD compatible solo con el canal, la autenticación de Adobe Pass extrae SOLAMENTE el título del canal (&quot;TNT&quot; en el ejemplo anterior) y pasa solo el título a la MVPD.

### Flujo de intercambio de metadatos de MVPD a programador {#mvpd-progr-exchange-flow}

La autenticación de Adobe Pass realiza las siguientes suposiciones:

* La MVPD envía la clasificación máxima como parte de la respuesta de SAML
* Esta información se guarda como parte del token de autenticación
* La autenticación de Adobe Pass proporciona una API para permitir a los programadores recuperar esta información
* Los programadores implementan esta función en su sitio o aplicación (por ejemplo, para ocultar vídeos que superen la clasificación máxima del usuario)

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### Notas {#notes-mvpd-progr-metadata-exch-flow}

**Normalización y validación de recursos.** identificadores de recursos se pueden pasar como una cadena sin formato o una cadena MRSS. Un programador puede decidir utilizar el formato de cadena sin formato o el MRSS, pero necesitará un acuerdo previo con el MVPD para que el MVPD sepa cómo tratar ese recurso.

**ID de recurso y especificación de metadatos.La autenticación de Adobe Pass** utiliza el estándar RSS con la extensión Media RSS para especificar un recurso y sus metadatos. Junto con la extensión RSS de medios, la autenticación de Adobe Pass admite una amplia variedad de metadatos, como controles parentales (a través de `<media:rating>`) o geolocalización (`<media:location>`).

La autenticación de Adobe Pass también puede admitir la conversión transparente de la cadena de canal heredada al recurso RSS correspondiente para MVPD que requieren RSS. En la otra dirección, la autenticación de Adobe Pass admite la conversión de RSS+MRSS a título de canal sin formato, para MVPD solo de canal.

La autenticación de **Adobe Pass garantiza la compatibilidad total con versiones anteriores de las integraciones existentes.** Es decir, para los programadores que utilizan autenticación de nivel de canal, la autenticación de Adobe Pass se encarga de empaquetar el ID de canal en el formato necesario antes de enviarlo a una MVPD que comprenda ese formato. Lo contrario también se aplica: si un programador especifica todos sus recursos en un nuevo formato, la autenticación de Adobe Pass traduce el nuevo formato a una cadena de canal simple si autoriza con una MVPD que solo realiza autorización de nivel de canal.

## Casos de uso de metadatos de usuario {#user-metadata-use-cases}

Los casos de uso cambian y se expanden constantemente a medida que más MVPD hacen arreglos legales y agregan funcionalidad. A continuación se muestran ejemplos de para qué se pueden utilizar los metadatos de usuario.

* [ID de usuario de MVPD](#mvpd-user-id)
* [Identificador del hogar](#household-user-id)
* [Código postal](#zip-code)
* [Clasificación máxima (Control parental)](#max-rating-parental-control)
* [Alineación de canales](#channel-line-up)

### ID de usuario de MVPD {#mvpd-user-id}

* Según lo dispuesto por la MVPD
* No es la información de inicio de sesión real del usuario, ya que está cifrado en hash por la MVPD
* Se puede utilizar para indicar problemas con o para usuarios específicos
* Cifrado
* Compatibilidad con MVPD: Todas las MVPD

### ID de usuario doméstico {#household-user-id}

* Permite una buena información de las métricas
* Cifrado
* Compatibilidad con MVPD: Algunas MVPD

### Código postal {#zip-code}

* El código postal de facturación del usuario
* Se utiliza principalmente para aplicar reglas de período de congelación de eventos deportivos
* Se puede proporcionar con la respuesta AuthZ para actualizaciones rápidas
* Compatibilidad con MVPD: Algunas MVPD

### Clasificación máxima (Control parental) {#max-rating-parental-control}

* AuthN inicialmente, más actualización de AuthZ
* Filtrado de contenido fuera de la IU
* Clasificaciones de MPAA o VChip
* Compatibilidad con MVPD: Algunas MVPD

### Alineación de canales {#channel-line-up}

* Las MVPD pueden proporcionar una lista de canales que el usuario tiene derecho a ver
* Permite pintar rápidamente la IU
* La especificación OLCA lo permite como AttributeStatement en la respuesta AuthN
* Compatibilidad con MVPD: Algunas MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
