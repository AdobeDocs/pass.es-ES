---
title: Cómo distinguir entre VOD y contenido en directo en la monitorización de concurrencia
description: Cómo distinguir entre VOD y contenido en directo en la monitorización de concurrencia
exl-id: 51ba686a-7c1f-4403-9e8e-cd247bf9e345
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Cómo distinguir entre VOD y contenido en directo en la monitorización de concurrencia {#dist-vod-live}

**Q:** ¿Puede el servicio de monitoreo de concurrencia distinguir entre el tipo de contenido que se está reproduciendo (contenido en vivo vs. Vídeo bajo demanda)?



**A:** La supervisión de simultaneidad no puede distinguir directamente entre contenido en directo y Vídeo bajo demanda (VOD). El reproductor de vídeo debe saber el tipo de contenido que está reproduciendo y enviar esta información durante la [llamada de inicialización de la sesión](/help/concurrency-monitoring/api/cm-api-overview.md#session-initial) (necesaria para la supervisión de la concurrencia). El flujo de trabajo normal tiene este aspecto:

1. Los clientes de Monitorización de simultaneidad definen un conjunto de metadatos en el que les gustaría que se implementaran las reglas (por ejemplo, content-type=live|vod, device-type=mobile|console|desktop).
1. El equipo de Monitorización de concurrencia implementa la política deseada. Ejemplo:
   1. si content-type=live, flujos máximos=3, últimas victorias
   1. si content-type=vod, flujos máximos=1, últimas victorias

1. Cuando el usuario final reproduce el contenido, el reproductor de vídeo debe enviar valores para los campos de metadatos que se establecieron como parte de una directiva.

1. El servicio de supervisión de concurrencia, según la política definida y los valores recibidos, emitirá una decisión (reproducir/detener).

1. El reproductor de vídeo debe obedecer esta decisión para que el sistema funcione.



## Información relacionada {#related-info-vod-live-dist}

* [Atributos de metadatos estándar de supervisión de concurrencia](/help/concurrency-monitoring/technical/standard-metadata-attributes.md)
* [Resumen de API de supervisión de concurrencia](/help/concurrency-monitoring/api/cm-api-overview.md)
