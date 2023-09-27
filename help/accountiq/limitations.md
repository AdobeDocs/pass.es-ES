---
title: Limitaciones y problemas conocidos
description: Problemas conocidos del producto.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Problemas y limitaciones conocidos {#known-issues}

Adobe se esfuerza por ofrecer una funcionalidad sólida y experiencias de usuario sin problemas a través de sus ofertas. La versión actual (versión 1.0) de Account IQ proporciona análisis de uso compartido y suscripción a proveedores de streaming con un alto grado de confianza. Sin embargo, las siguientes limitaciones se abordarán en próximas versiones.

* Al definir cohortes en las páginas de informes o tableros, actualmente no hay opción para agregar métricas como **número de dispositivos** para refinar el segmento. Esta capacidad estará disponible en una versión futura.

* Al estimar las puntuaciones de uso compartido para cuentas individuales, Account IQ adopta un enfoque conservador que permite a las empresas actuar en el uso compartido con un alto grado de confianza. Sin embargo, este enfoque tiende a subestimar la cantidad total de uso compartido cuando se acumula en muchas cuentas.

* El [Puntuación general de uso compartido](/help/accountiq/dashboard.md#overall-sharing-score) actualmente solo hay factores en [Nivel de uso compartido](/help/accountiq/dashboard.md#sharing-level) y [Uso de cuentas compartidas](/help/accountiq/dashboard.md#usage-from-shared-accounts). Las versiones futuras tendrán en cuenta las métricas adicionales.

* Al definir cohortes en las páginas de informes o tableros, los selectores de MVPD y canales carecen del mecanismo de búsqueda, por ahora.

* Al definir cohortes en las páginas de informes o tableros, existe la limitación de seleccionar solo hasta 10 MVPD y programadores (o canales individuales).

* La opción de exportar estadísticas de cuenta se limita a exportar 1000 cuentas, a partir de ahora.

* La opción para seleccionar [Tipo de segmento](#segment-type) al definir Operations se limita a **Número fijo de cuentas**. El **Número variable de cuentas** La opción estará disponible en una próxima versión.

* Las secciones de Benchmark, Modelos de detección, Segmentos, Instantáneas y Reglas de la navegación izquierda están desactivadas actualmente y estarán disponibles en una próxima versión.

* Al crear [Operaciones](/help/accountiq/operation-affecting-user-segment.md), solo se pueden identificar dos tipos de [Acciones](/help/accountiq/operation-affecting-user-segment.md) a partir de ahora: reglas de supervisión de concurrencia y acciones externas.

* Actualmente, solo se pueden crear Operaciones y [programado](/help/accountiq/operation-affecting-user-segment.md#action). Las versiones futuras le permiten pausar, reanudar y administrar por completo.

* Debido al conjunto de datos más limitado que se utiliza, el modo de aislamiento no refleja realmente la cantidad de uso compartido. Por lo tanto, MVPD en el modo de aislamiento no se puede comparar con ninguna otra MVPD. <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* Al definir un nuevo [segmento](/help/accountiq/segments-timeframe.md) para una operación, puede añadir métricas. Sin embargo, si selecciona un segmento guardado, no podrá añadir más métricas para restringir el segmento.

* La granularidad y el selector de periodo de tiempo están limitados a una semana o un mes, lo que significa que los datos pueden evaluarse en una sola semana o solo en un mes.

* Los intervalos predefinidos están desactivados actualmente en el selector de granularidad y periodo de tiempo y estarán disponibles en una versión futura.
