---
title: Resumen de API de degradación
description: Resumen de API de degradación
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---


# Resumen de API de degradación {#degradation-api-overview}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Antes de usar la API de degradación, asegúrese de que se cumplan los siguientes requisitos previos:
>
> * Obtenga las credenciales del cliente como se describe en la [Documentación de la API Retrieve client credentials](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenga el token de acceso como se describe en la [Documentación de la API Recuperar token de acceso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Consulte la documentación de [Información general sobre el registro dinámico de clientes](./dcr-api/dynamic-client-registration-overview.md) para obtener más información sobre cómo crear una aplicación registrada y descargar la instrucción de software.

## Información general {#general_info}

>[!NOTE]
>
>Esta API no está disponible de forma general. Póngase en contacto con el representante del Adobe para ver las actualizaciones de disponibilidad.

Esta función proporciona a cualquiera de las tres partes principales de una integración (programadores, MVPD y Adobe) la capacidad de omitir temporalmente extremos específicos de autenticación y autorización de MVPD. Por lo general, es el Programador el que inicia dicha acción, pero independientemente de quién déclencheur un evento de degradación, la acción depende de los acuerdos previamente acordados con los MVPD afectados.

El caso de uso principal de esta función se produce durante deportes en directo o grandes eventos. En estos escenarios de alto tráfico, es posible que la carga en un punto final de MVPD específico sea demasiado alta, lo que resulta en tiempos de respuesta muy largos para los usuarios. Para conservar una buena experiencia de usuario durante un escenario de este tipo, el programador puede decidir almacenar en déclencheur una regla de degradación que pueda autenticar/autorizar usuarios automáticamente de forma temporal, o deshabilitar una MVPD eliminándola de la lista de MVPD disponibles.

Una regla de degradación solo se aplica durante un periodo de tiempo fijo. Aunque los clientes principales de esta función son los canales deportivos y los canales de noticias en directo, es posible que cualquier programador desee tener acceso a esta función, ya que los servicios de MVPD no funcionan de vez en cuando.

Notas de degradación:

: Esta función está diseñada para utilizarse junto con la API de monitorización de uso, que proporciona información en tiempo real sobre el número de autenticaciones y autorizaciones por MVPD, la latencia de autorización promedio y otras métricas necesarias para una descripción general completa del servicio.
: Esta función no permite omitir el servicio de autenticación de Adobe Primetime. Si la autenticación de Adobe Pass está caída, no hay ningún mecanismo dentro del servicio que se pueda usar para permitir que los usuarios vean el contenido. Sin embargo, los sitios o las aplicaciones podrían enrutar la autenticación de Adobe Pass por sí mismos.
- El Adobe no va a degradar el déclencheur directamente actualmente, la decisión siempre debe estar en manos de un programador específico que haya aceptado tales condiciones con MVPD. En el futuro, la autenticación de Adobe Pass podría activar reglas de degradación de forma proactiva si se pueden alcanzar acuerdos (protección de SLA) con MVPD.

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
