---
title: Notas de la versión de Adobe Concurrency Monitoring 2.9
description: Notas de la versión de Adobe Concurrency Monitoring 2.9
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Notas de la versión de Concurrency Monitoring 2.9 {#rn-cm29}

Esta página describe las nuevas funciones, los cambios y los problemas conocidos de esta versión.

## Fecha de lanzamiento {#release-date}

03/14/2019


## Información general de versión {#release-overview}

* A partir de esta versión, hemos introducido un nuevo informe para comprender el uso simultáneo: un histograma para el nivel de concurrencia que muestra lo siguiente:

* el número de usuarios que han alcanzado cada nivel de concurrencia (es decir, cuántos usuarios han tenido 2 flujos simultáneos, 3 flujos simultáneos, etc.) durante cada intervalo de granularidad
* la duración total de cada nivel de concurrencia, en minutos (el valor promedio se puede calcular simplemente dividiendo este valor por el recuento anterior)
* el número total de veces que los usuarios se han encontrado con cada nivel de concurrencia, para estimar el impacto de una regla determinada en términos tanto de usuarios afectados como de experiencia del usuario agregada. Más información en la [Informes de uso](/help/concurrency-monitoring/cm-usage-reports.md) página.

También mejoramos la protección contra inyecciones SQL y añadimos varias correcciones de errores.

## Problemas conocidos {#known-issues}

Ninguna.
