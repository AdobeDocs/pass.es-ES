---
title: Notas de la versión de Adobe Pass Concurrency Monitoring 2.3.2
description: Notas de la versión de Adobe Pass Concurrency Monitoring 2.3.2
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Notas de la versión de Adobe Pass Concurrency Monitoring 2.3.2 {#cm-232}

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

Fecha de versión: 11/12/2015

## Nuevas funciones y mejoras {#new-features}

* Nuevos desgloses disponibles en los informes de uso. Los nuevos desgloses están disponibles si la aplicación integrada con la Monitorización de concurrencia envía metadatos personalizados.
   * application: el ID de aplicación indicado en la URL de llamada.
   * mvpd: el MVPD indicado en la dirección URL de la llamada
   * canal: el canal de metadatos personalizado
   * platform: la aplicación de metadatos personalizada
* Nuevas métricas relacionadas con **duración del flujo** disponible en los informes de uso. Las nuevas métricas se pueden utilizar para crear un histograma de duración del flujo. Actualmente están disponibles los siguientes intervalos en minutos:
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## Corrección de errores {#bug-fixes}

N/D

## Problemas conocidos {#known-issues}

N/D
