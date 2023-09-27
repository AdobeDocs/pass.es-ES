---
title: Ventajas de utilizar el parámetro deviceType sin cliente en las métricas de autenticación de Adobe Pass
description: Ventajas de utilizar el parámetro deviceType sin cliente en las métricas de autenticación de Adobe Pass
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# Ventajas de utilizar el parámetro deviceType sin cliente en las métricas de autenticación de Adobe Pass {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

## Contexto

Aunque es opcional, el parámetro `deviceType` de la API sin cliente, cuando está presente, se utiliza en las métricas de autenticación de Adobe Pass que se exponen a través de [Supervisión del servicio de derechos](/help/authentication/entitlement-service-monitoring-overview.md).

Teniendo en cuenta que la conexión entre `deviceType` y su **ventajas** Cuando no se indicaba inicialmente ninguna métrica de autenticación de Adobe Pass, el ámbito de esta nota técnica es añadir más información sobre ellas.

## Explicación

El `deviceType` El parámetro estaba presente en la API sin cliente desde la primera versión, pero sus implicaciones en las métricas de autenticación de Adobe Pass se añadieron en una versión más reciente.



>[!IMPORTANT]
>
>Si el parámetro `deviceType` está configurado correctamente, tiene lo siguiente **provecho** en la Monitorización del servicio de derecho: ofrece métricas que son [desglosado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) cuando se utiliza sin cliente, de modo que se puedan realizar distintos tipos de análisis para, por ejemplo, Roku, AppleTV, Xbox, etc.


Para obtener más información sobre la API de supervisión del servicio de derechos, consulte la [explorar en profundidad el árbol,](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) que ilustra el [dimensiones](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) (recursos) disponibles en ESM 2.0.

>[!NOTE]
>
>El contenido de esta nota técnica también se ha añadido al [API sin cliente](#clientless_device_type).




## Implementación

Para beneficiarse completamente de las métricas de autenticación de Adobe Pass, existen dos tipos de [API sin cliente](#web_srvs_summary) que se están utilizando actualmente y que necesitan tener los `deviceType` set:

1. API que tienen `regcode` como parámetro obligatorio y utilizará la variable `deviceType` parámetro que se estableció al crear el `regcode`, con la siguiente llamada de API:
   - [\&lt;reggie _fqdn=&quot;&quot;>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. API que tienen el `deviceType` como parámetro opcional:
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/authorize](#init_authz)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/media](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/mediatoken](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/logout](#init_logout)

Nuestra recomendación es utilizar el `deviceType` y pase el tipo de dispositivo sin cliente correcto para todas las API.
