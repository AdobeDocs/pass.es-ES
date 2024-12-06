---
title: Ventajas de utilizar el parámetro deviceType sin cliente en las métricas de autenticación de Adobe Pass
description: Ventajas de utilizar el parámetro deviceType sin cliente en las métricas de autenticación de Adobe Pass
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Ventajas de utilizar el parámetro deviceType sin cliente en las métricas de autenticación de Adobe Pass {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

## Contexto

Aunque es opcional, el parámetro `deviceType` de la API sin cliente, cuando está presente, se utiliza en las métricas de autenticación de Adobe Pass que se exponen a través de [Supervisión del servicio de derechos](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md).

Teniendo en cuenta que la conexión entre el parámetro `deviceType` y sus **beneficios** en las métricas de autenticación de Adobe Pass no se indicó inicialmente, el ámbito de esta nota técnica es agregar más información sobre ellos.

## Explicación

El parámetro `deviceType` estaba presente en la API sin cliente desde la primera versión, pero sus implicaciones en las métricas de autenticación de Adobe Pass se agregaron en una versión más reciente.



>[!IMPORTANT]
>
>Si el parámetro `deviceType` está configurado correctamente, tiene el siguiente **beneficio** en la supervisión del servicio de derechos: ofrece métricas que se [desglosan por tipo de dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) al usar sin clientes, de modo que se puedan realizar diferentes tipos de análisis, por ejemplo, para Roku, AppleTV, Xbox, etc.


Para obtener más información sobre la API de supervisión del servicio de derechos, consulte el [árbol detallado,](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree) que ilustra las [dimensiones](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions) (recursos) disponibles en ESM 2.0.

>[!NOTE]
>
>El contenido de esta nota técnica también se agregó a la [API sin cliente](#clientless_device_type).




## Implementación

Para beneficiarse completamente de las métricas de autenticación de Adobe Pass, hay dos tipos de [API sin cliente](#web_srvs_summary) que se están usando actualmente y que necesitan tener `deviceType` establecido correctamente:

1. API que tienen `regcode` como parámetro obligatorio y usarán el parámetro `deviceType` establecido al crear `regcode`, con la siguiente llamada de API:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. API que tienen `deviceType` como parámetro opcional:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

Se recomienda usar el parámetro `deviceType` y pasar el tipo de dispositivo sin cliente correcto para todas las API.
