---
title: Definición de un segmento y un lapso de tiempo
description: Definición de un segmento y un lapso de tiempo
exl-id: 86fe010d-3202-4ce2-b803-ff44f5538d7e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---

# Definición de un segmento y un lapso de tiempo {#define-segment}

Todos los informes de análisis o visualización en [!UICONTROL Account IQ] comience por definir el segmento y seleccionar el lapso de tiempo para la evaluación. [Segmento](/help/accountiq/product-concepts.md#segmet-def) hace referencia a todos los suscriptores o visualizadores que cumplen con sus criterios de evaluación (suscripción a una MVPD y visualización de canales específicos).

![](assets/segment-panel.png)

*Figura: Selección de segmentos y lapsos de tiempo*

En la parte superior de todas las páginas de informes de [!UICONTROL Account IQ]Además, hay un panel para definir segmentos seleccionando MVPD, programadores de canales y granularidad y lapso de tiempo.

## Selección de segmentos {#select-segment}

### Seleccionar MVPD en el segmento {#select-segment-mvpds}

Para seleccionar MVPD de **[!UICONTROL MVPDs in segment]** opción:

1. Toque o haga clic en **[!UICONTROL MVPDs in segment]** opción desplegable.

   >[!NOTE]
   >
   >**Todo** las MVPD del sector se seleccionan de forma predeterminada. Desde aquí, puede seleccionar cualquiera de las siguientes opciones **Las 10 principales MVPD por puntuación de uso compartido**, **Principales 10 MVPD por uso**, **Principales 10 MVPD por cuentas**, o MVPD individuales. Sin embargo, para seleccionar MVPD individuales debe anular la selección **Todo**.

1. Toque o haga clic en las MVPD que desee.

   Puede eliminar una MVPD de la selección anulando su selección.

1. Haga clic o toque **[!UICONTROL Apply selection]** para que la selección surta efecto. De lo contrario, perderá la selección que haya realizado.

   >[!NOTE]
   >
   >Si selecciona el modo de aislamiento, no se puede seleccionar ninguna de las otras MVPD.

### Seleccionar canales en el segmento {#select-segment-channels}

Para seleccionar los canales de programador deseados de la **[!UICONTROL Channels in segment]** opción:

1. Toque o haga clic en **[!UICONTROL Channels in segment]** opción desplegable.

   >[!NOTE]
   >
   >**Todo** los canales del programador de su empresa están seleccionados de forma predeterminada. Para seleccionar canales o programadores individuales, primero debe anular la selección **Todo**.

1. Toque o haga clic en los canales o programadores que desee.

   Los elementos de lista de nivel superior de la **[!UICONTROL Channels in segment]** son [programador](/help/accountiq/product-concepts.md#programmer-def) compañías y los elementos de la lista bajo nombres de programador son sus [canales](/help/accountiq/product-concepts.md#channel-def). Puede seleccionar canales individuales en programadores o seleccionar programadores, y todas las actividades de los canales bajo ese programador se incluyen en los resultados de informes y gráficos.

   ![](assets/programmer-channels.png)


   *Imagen: programadores y canales enumerados en el selector de canales*

   >[!IMPORTANT]
   >
   >Los resultados de seleccionar canales individuales bajo un programador no son los mismos que los de seleccionar el programador.
   >
   >
   >Al seleccionar canales individuales, las actividades de esos canales se desglosan individualmente en algunos informes. Sin embargo, al seleccionar el programador principal de todos esos canales, se incluye toda la actividad de esos canales, pero no se desglosa de forma individual en los informes.

1. Haga clic o toque **[!UICONTROL Apply selection]** para que la selección surta efecto.

>[!NOTE]
>
>No se pueden seleccionar más de 10 elementos en los menús desplegables de MVPD o programador.

### Anular la selección de MVPD y canales {#deselect-segment-mvpds-channels}

Además de cambiar su selección en la **[!UICONTROL MVPDs in segment]** y **[!UICONTROL Channels in segment]** selectores de segmentos, puede anular la selección de las MVPD y los canales seleccionados anteriormente mediante:

* Selección de la **[!UICONTROL Remove]** icono (![quitar icono](assets/remove-icon.png)) en los nombres de estas MVPD y canales seleccionados que se muestran debajo del selector de segmentos.

* También puede utilizar **[!UICONTROL Clear Selection]** para eliminar todos los canales o MVPD seleccionados previamente.

![](assets/segment-panel-selection.png)

*Imagen: MVPD y canales seleccionados en el panel de segmentos y periodo de tiempo*

## Granularidad y selección de lapsos de tiempo {#granularity-timeframe}

Para seleccionar un período de tiempo de evaluación:

1. Seleccione el **[!UICONTROL Granularity and time frame]** selector de fechas.

1. Seleccione una de las opciones **[!UICONTROL Week]** o **[!UICONTROL Month]** de **[!UICONTROL Aggregate By]** para definir la granularidad de la evaluación.

   ![](assets/granularity-timeframe-weekwise.png)


   *Figura: Selector de fecha para seleccionar la granularidad y el lapso de tiempo*

1. Una vez seleccionada la granularidad, puede utilizar flechas hacia delante o hacia atrás para avanzar o retroceder en el tiempo.

1. Especifique un período de tiempo en el pasado (en mes o semana según la granularidad seleccionada) para la evaluación.

1. Seleccionar **[!UICONTROL Apply Selection]** para asegurarse de que la selección surta efecto.
