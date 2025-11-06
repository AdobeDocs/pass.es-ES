---
title: Función de degradación
description: Función de degradación
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Función de degradación {#degradation-feature}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En el dinámico mundo de los deportes en directo y los grandes eventos, es esencial garantizar una experiencia de espectador perfecta. El alto tráfico durante estos eventos puede saturar los puntos finales de autenticación y autorización del Distribuidor de programación de vídeo multicanal (MVPD), lo que provoca retrasos o interrupciones.

La autenticación de Adobe Pass resuelve estos desafíos con su **característica de degradación**, una solución que permite omitir temporalmente determinados extremos de autorización y autenticación de MVPD. Esta función es especialmente valiosa durante los eventos de tráfico máximo, en los que los tiempos de respuesta pueden degradarse debido a una carga pesada en los sistemas MVPD.

La característica de degradación **1} puede ser una protección vital para los programadores, ya que garantiza la continuidad del servicio.** Aunque su audiencia principal incluye deportes en vivo y canales de noticias, su utilidad se extiende a cualquier Programador que busque mitigar el riesgo de interrupciones causadas por puntos finales de MVPD.

>[!IMPORTANT]
>
> La API de degradación es una función Premium y requiere una licencia actual de Adobe.

Al aplicar una regla de degradación, los programadores pueden habilitar temporalmente la autenticación automática o la autorización automática, lo que garantiza el acceso ininterrumpido al contenido durante el período en que se aplica la degradación. Las acciones de degradación siempre las inicia un Programador basado en acuerdos preestablecidos con MVPD. Aunque Adobe actualmente no almacena la degradación del déclencheur directamente, las funciones futuras pueden incluir la administración proactiva si se establecen acuerdos de nivel de servicio (SLA).

Esta característica está diseñada para utilizarse junto con una [API de supervisión de uso](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) y basada en los acuerdos previos con MVPD, la autenticación de Adobe Pass ofrece una potente herramienta para equilibrar la experiencia del usuario, la fiabilidad y el control operativo durante los momentos críticos.

>[!IMPORTANT]
>
> Esta función no permite omitir el propio servicio de autenticación de Adobe Pass. Si la autenticación de Adobe Pass no está disponible, no hay ningún mecanismo integrado dentro de este servicio para facilitar el acceso de los usuarios. En estos casos, los sitios o las aplicaciones pueden elegir implementar sus propias soluciones de enrutamiento alternativas para mantener la entrega de contenido.

## Acceso a API de degradación {#degradation-api-access}

Antes de acceder a la [API de degradación](#degradation-api), debe completar los pasos necesarios en el proceso de registro de cliente dinámico (DCR). Este proceso obligatorio garantiza que dispone del token de acceso necesario para interactuar con la API de degradación.

Para obtener instrucciones detalladas, consulte la [Documentación general sobre el registro de clientes dinámicos](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## API de degradación {#degradation-api}

La API de degradación es una API de RESTful que permite a los programadores administrar reglas de degradación para MVPD específicas. La API proporciona los medios para activar, eliminar y recuperar el estado de las reglas de degradación activas.

Para obtener más información sobre la API de degradación, consulte el siguiente documento de Zendesk [Autenticación de Adobe Pass | API de degradación v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3) y busque el archivo PDF que desea descargar.

## API DE REST V2 {#rest-api-v2}

El uso de la característica Degradación requiere la implementación de actualizaciones de código para modificar la forma en que la aplicación TV Everywhere (TVE) interactúa con la autenticación de Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

Para obtener una guía completa sobre estas actualizaciones y los flujos de trabajo asociados, consulte la documentación de [Flujos de acceso degradados](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).
