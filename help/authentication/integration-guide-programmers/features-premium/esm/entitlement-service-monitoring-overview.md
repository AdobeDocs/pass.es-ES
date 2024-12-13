---
title: Información general sobre supervisión del servicio de derechos
description: Información general sobre supervisión del servicio de derechos
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 0%

---

# Información general sobre supervisión del servicio de derechos {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#introduction}

Los sitios y las aplicaciones de TVE deben estar disponibles las 24 horas del día, los 7 días de la semana, de modo que los clientes necesiten conocer los eventos de asignación de derechos en tiempo real para detectar y corregir los problemas lo antes posible. También deben analizar los datos mensuales para determinar qué plataformas proporcionan la mayor parte del tráfico y qué plataformas podrían tener una implementación incorrecta y tasas de conversión deficientes.

La Monitorización del servicio de derecho (ESM) proporciona a los programadores y a las MVPD una fuente de datos que ofrece visibilidad en tiempo real de sus eventos de autenticación y autorización. Los datos se recopilan de los sistemas de autenticación de Adobe Pass y se proporcionan a través de una API RESTful.  Los clientes pueden consumir los datos de forma directa o desde sus propios paneles operativos personalizados.

Los elementos principales del sistema de gestión ambientalmente racional son sus métricas y dimensiones. ESM genera informes que contienen métricas agregadas según la selección de dimensiones. Como los eventos de Adobe Pass se registran en la zona horaria PST, los informes de ESM también están disponibles en la zona horaria PST.

La API de ESM no está disponible de forma general.  Póngase en contacto con el representante del Adobe para preguntas sobre disponibilidad.

## ESM para programadores {#esm-for-programmers}

### Los programadores pueden monitorizar las siguientes métricas: {#programmers-monitor-metrics}


| *Nombre de métrica* | *Descripción* |
|-------------------------|--------------------------|
| authn-tries | Número de flujos de autenticación iniciados |
| autocorrecto | Número de tokens de autenticación obtenidos correctamente por los clientes |
| Authn-pending | Número de tokens de autenticación generados correctamente (sin tener en cuenta si el cliente los obtuvo o no) |
| authn-failed | Número de autenticación fallida realizada a través de un sistema externo. |
| clientless-tokens | Número de tokens de Clientless emitidos correctamente |
| clientless-failures | Número de intentos fallidos para recibir tokens de la API sin cliente |
| authz-tries | Número de intentos de autorización |
| auténtico | Número de autorizaciones correctas |
| authz-failed | Número de autorizaciones denegadas por MVPD en el nivel de aplicación |
| Authz-rejected | Número de intentos de autorización considerados maliciosos por el proveedor de servicios de Adobe y rechazados como resultado de una prevención de ataques DoS |
| authz-latency | Número total de milisegundos empleados en el extremo de MVPD |
| media-tokens | Número de tokens de medios cortos generados (que se asimilan al número de solicitudes de reproducción) |
| unique-accounts | Número de usuarios únicos que realizaron acciones de asignación de derechos (AuthN/AuthZ) en el intervalo de tiempo seleccionado. (Esta métrica solo se mostrará si se solicitan valores diarios). </br> Esto se calcula para cada centro de datos individual. Cuando no se solicita la dimensión &quot;dc&quot;, esta métrica no se muestra. |
| unique-session | Número de sesiones únicas que realizaron llamadas de flujo de autenticación al servicio de autenticación de Adobe Pass dentro del intervalo de tiempo seleccionado. (Esta métrica solo se mostrará si se solicitan valores diarios). </br> Esto se calcula para cada centro de datos individual. Cuando no se solicita la dimensión &quot;dc&quot;, esta métrica no se muestra. |
| count | Un contador simple utilizado en los informes orientados a eventos |

</br>

### Los programadores pueden filtrar las métricas enumeradas anteriormente según las siguientes dimensiones: {#progr-filter-metrics}


| *Nombre de Dimension* | *Descripción* |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| año | El año expresado con 4 dígitos |
| mes | El mes del año (1-12) |
| día | El día del mes (1-31) |
| hora | La hora del día |
| minuto | El minuto de la hora |
| media-company | Compañía de medios propietaria del sitio web que inició el proceso de asignación de derechos para el usuario |
| dc | (Centro de datos) La región de origen en la que se recibió la solicitud. |
| proxy | El MVPD proxy (que será &quot;Directo&quot; para integraciones directas) |
| mvpd | MVPD responsable de conceder el derecho al usuario |
| requestor-id | ID del solicitante utilizado para realizar la solicitud de asignación de derechos |
| canal | El sitio web del canal, extraído del campo de recurso (extraído de la carga útil MRSS como canal/título si se proporciona, o asignado al valor del recurso si no está en formato RSS). |
| resource-id | El título real del recurso implicado en la solicitud de autorización (extraído de la carga útil MRSS como el artículo/título si se proporciona) |
| dispositivo | La plataforma del dispositivo (PC, móvil, consola, etc.) |
| bisiesto | El proveedor de autenticación externa cuando el flujo de autenticación se realiza a través de un sistema externo. </br> Los valores pueden ser: </br> - N/D - la autenticación fue proporcionada por la autenticación de Adobe Pass </br> - Apple - el sistema externo que proporcionó la autenticación es Apple |
| os-family | Sistema operativo en ejecución en el dispositivo |
| browser-family | Agente de usuario utilizado para acceder a la autenticación de Adobe Pass |
| cdt | La plataforma del dispositivo (alternativa), utilizada actualmente para Clientes sin conexión. </br> Los valores pueden ser: </br> - N/D - el evento no se originó desde una SDK sin cliente </br> - Desconocido - Dado que el parámetro deviceType de una API sin cliente es opcional, hay llamadas que no contienen ningún valor. </br>: cualquier otro valor enviado a través de la API sin cliente, como xbox, appletv, roku, etc. </br> |
| platform-version | La versión de SDK sin cliente |
| os-type | Sistema operativo en ejecución en el dispositivo, alternativo (no utilizado actualmente) |
| browser-version | Versión del agente de usuario |
| nsdk | El SDK de cliente utilizado (android, fireTV, js, iOS, tvOS, que no es sdk) |
| nsdk-version | La versión de SDK del cliente de autenticación de Adobe Pass |
| evento | El nombre del evento de autenticación de Adobe Pass |
| razonar | El motivo de los errores, según la información de Autenticación de Adobe Pass |
| sso-type | El mecanismo SSO subyacente: platform/pasive/adobe. Indica que el token de autorización se emitió reutilizando AuthN en una aplicación diferente |
| plataforma | El dispositivo identificó la plataforma. Valores posibles: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc |
| application-name | El nombre de la aplicación configurada, en Tablero de TVE, para la aplicación registrada DCR configurada para utilizarse. |
| application-version | La versión de la aplicación configurada, en Tablero de TVE, para la aplicación registrada de DCR configurada para utilizarse. |
| customer-app | Id. de aplicación personalizada que se pasó a través de [Información del dispositivo](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| content-category | La categoría del contenido solicitado por la aplicación. |

## ESM para MVPD {#esm-for-mvpds}

### Las MVPD pueden monitorizar las siguientes métricas:

| *Nombre de métrica* | *Descripción* |
|---|---|
| authn-tries | Número de flujos de autenticación iniciados |
| autocorrecto | Número de tokens de autenticación obtenidos correctamente por los clientes |
| Authn-pending | Número de tokens de autenticación generados correctamente (sin tener en cuenta si el cliente los obtuvo o no) |
| authn-failed | Número de autenticación fallida realizada a través de un sistema externo. |
| authz-tries | Número de intentos de autorización |
| auténtico | Número de autorizaciones correctas |
| authz-failed | Número de autorizaciones denegadas por MVPD en el nivel de aplicación |
| Authz-rejected | Número de intentos de autorización considerados maliciosos por el proveedor de servicios de Adobe y rechazados como resultado de una prevención de ataques DoS |
| authz-latency | Número total de milisegundos empleados en el extremo de MVPD |

### Las MVPD pueden filtrar las métricas enumeradas anteriormente según las siguientes dimensiones:

| *Nombre de Dimension* | *Descripción* |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| año | El año expresado con 4 dígitos |
| mes | El mes del año (1-12) |
| día | El día del mes (1-31) |
| hora | La hora del día |
| minuto | El minuto de la hora |
| mvpd | ID de mvpd utilizado para realizar la solicitud de asignación de derechos |
| requestor-id | ID del solicitante utilizado para realizar la solicitud de asignación de derechos |
| bisiesto | El proveedor de autenticación externa cuando el flujo de autenticación se realiza a través de un sistema externo. </br> Los valores pueden ser: </br> - N/D - la autenticación fue proporcionada por la autenticación de Adobe Pass </br> - Apple - el sistema externo que proporcionó la autenticación es Apple |
| cdt | La plataforma del dispositivo (alternativa), utilizada actualmente para Clientes sin conexión. </br> Los valores pueden ser: </br> - N/D - el evento no se originó desde una SDK sin cliente </br> - Desconocido - Dado que el parámetro deviceType de una API sin cliente es opcional, hay llamadas que no contienen ningún valor. </br>: cualquier otro valor enviado a través de la API sin cliente, como xbox, appletv, roku, etc. </br> |
| sdk-type | El SDK de cliente utilizado (Flash, HTML 5, Android nativo, iOS, sin cliente, etc.) |
| plataforma | El dispositivo identificó la plataforma. Valores posibles: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc |
| nsdk | El SDK de cliente utilizado (android, fireTV, js, iOS, tvOS, que no es sdk) |
| nsdk-version | La versión de SDK del cliente de autenticación de Adobe Pass |

## Casos de uso {#use-cases}

Puede utilizar los datos de ESM para los siguientes casos de uso:

- **Supervisión**: los equipos de operaciones o supervisión pueden crear un tablero o gráfico que llame a la API cada minuto. Con la información mostrada, pueden detectar un problema (con la autenticación de Adobe Pass o con una MVPD) en el minuto en que aparece.

- **Depuración/Prueba de calidad**: como los datos también se desglosan por plataforma, dispositivo, explorador y sistema operativo, el análisis de los patrones de uso puede identificar problemas en combinaciones específicas (por ejemplo, Safari en OSX).

- **Analytics**: los datos proporcionados se pueden usar para complementar o auditar los datos del lado del cliente que se recopilan mediante Adobe Analytics u otra herramienta de análisis.

<!--
## Related Information {#related-information}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
