---
title: Cómo distinguir entre VOD y contenido en directo en la monitorización de concurrencia
description: Cómo distinguir entre VOD y contenido en directo en la monitorización de concurrencia
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# Cómo: Distinguir entre VOD y contenido en directo en la supervisión de concurrencia {#dist-vod-live}

**P:** ¿Puede el servicio de monitorización de concurrencia distinguir entre el tipo de contenido que se está reproduciendo (contenido en directo o vídeo bajo demanda)?



**A:** El monitorado de concurrencia no puede distinguir directamente entre contenido en directo y vídeo bajo demanda (VOD). El reproductor de vídeo debe conocer el tipo de contenido que está reproduciendo y enviar esta información durante la [llamada de inicialización de sesión](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (necesario para la Monitorización de concurrencia). El flujo de trabajo normal tiene este aspecto:

1. Los clientes de Monitorización de simultaneidad definen un conjunto de metadatos en el que les gustaría que se implementaran las reglas (por ejemplo, content-type=live|vod, device-type=mobile|console|desktop).
1. El equipo de Monitorización de concurrencia implementa la política deseada. Ejemplo:
   1. si content-type=live, flujos máximos=3, últimas victorias
   1. si content-type=vod, flujos máximos=1, últimas victorias

1. Cuando el usuario final reproduce el contenido, el reproductor de vídeo debe enviar valores para los campos de metadatos que se establecieron como parte de una directiva.

1. El servicio de supervisión de concurrencia, según la política definida y los valores recibidos, emitirá una decisión (reproducir/detener).

1. El reproductor de vídeo debe obedecer esta decisión para que el sistema funcione.



## Información relacionada {#related-info-vod-live-dist}

* [Atributos de metadatos estándar de supervisión de concurrencia](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [Resumen de API de supervisión de concurrencia](/help/concurrency-monitoring/cm-api-overview.md)
