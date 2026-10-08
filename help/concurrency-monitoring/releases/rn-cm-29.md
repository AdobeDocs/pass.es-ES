---
title: Notas de la versión de Adobe Concurrency Monitoring 2.9
description: Notas de la versión de Adobe Concurrency Monitoring 2.9
exl-id: fd793b1f-b704-492b-850c-dae6478b575a
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 3%
---
# Notas de la versión de Concurrency Monitoring 2.9 {#rn-cm29}

Esta página describe las nuevas funciones, los cambios y los problemas conocidos de esta versión.

## Fecha de lanzamiento {#release-date}

03/14/2019


## Información general de versión {#release-overview}

* A partir de esta versión, hemos introducido un nuevo informe para comprender el uso simultáneo: un histograma para el nivel de concurrencia que muestra lo siguiente:

* el número de usuarios que han alcanzado cada nivel de concurrencia (es decir, cuántos usuarios han tenido 2 flujos simultáneos, 3 flujos simultáneos, etc.) durante cada intervalo de granularidad
* la duración total de cada nivel de concurrencia, en minutos (el valor promedio se puede calcular simplemente dividiendo este valor por el recuento anterior)
* el número total de veces que los usuarios se han encontrado con cada nivel de concurrencia para estimar el impacto de una regla determinada en términos tanto de usuarios afectados como de experiencia del usuario agregada
Encontrará más detalles en la página [Informes de uso](/help/concurrency-monitoring/reports/cm-usage-reports.md).

También mejoramos la protección contra inyecciones SQL y añadimos varias correcciones de errores.

## Problemas conocidos {#known-issues}

Ninguna.
