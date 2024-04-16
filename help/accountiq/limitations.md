---
title: Limitaciones
description: Conozca las limitaciones y el modo de aislamiento MVPD para programadores en Account IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Limitaciones {#limitations}

Las versiones D2C y TV Everywhere de Account IQ proporcionan análisis de uso compartido y suscripción a proveedores de streaming. Sin embargo, la versión actual 1.3 tiene ciertas limitaciones, que se abordarán en futuras versiones.

* El [Puntuación de uso compartido general](/help/accountiq/data-panels.md#overall-sharing-score) actualmente incluye [Nivel de uso compartido](/help/accountiq/data-panels.md#sharing-level) y [Uso de cuentas compartidas](/help/accountiq/data-panels.md#usage-from-shared-accounts). Las futuras versiones incluirán más métricas.

* Al definir segmentos en el panel o patrones de uso, la variable [Categorías de vídeo en el segmento](/help/accountiq/data-panels.md#video-categories-segment), [Puntuación de uso compartido por canales y MVPD](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds), y [Distribución del patrón de uso para categorías de vídeo](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) los informes solo pueden mostrar datos de hasta 20 [categorías de vídeo](product-concepts.md#video-category-def). Los segmentos que contengan más de 20 categorías de vídeo no mostrarán datos en estos informes.

* Actualmente, la opción para exportar estadísticas de cuentas está restringida a la exportación de 1000 cuentas.

* Al definir Operaciones, la opción para seleccionar [tipo de segmento](/help/accountiq/operations.md#segment) se limita a **Número fijo de cuentas**. El **Número variable de cuentas** estará disponible en futuras versiones de.

* El **Referencia**, **Modelos de detección**, **Acciones**, y **Configuración** Las secciones de la navegación izquierda están desactivadas actualmente y estarán disponibles en una versión futura.

* Al crear operaciones, sólo se pueden aplicar dos tipos de [acciones](/help/accountiq/operations.md#action) — Reglas de supervisión de concurrencia y acciones externas.

* Actualmente, solo puede [crear](/help/accountiq/operations.md#create-new-operation) y [programación](/help/accountiq/operations.md#schedule) Operaciones. Las futuras versiones le permiten pausar, reanudar y administrar por completo.

* Solo se pueden analizar los datos de una sola semana o mes a la vez al seleccionar Granularidad e Intervalo de tiempo.

* No puede añadir MVPD de modo de aislamiento a la definición del segmento con ninguna otra MVPD. Algunas MVPD no identifican exclusivamente a los suscriptores en varios canales de programación. Por lo tanto, esos MVPDs son tratados por separado para los programadores de TV Everywhere.



