---
title: Ver informes en modo de aislamiento
description: Ver informes en modo de aislamiento para Xfinity.
exl-id: e7cf24c5-9bfa-48f6-b5c8-20443a976891
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Ver informes de uso compartido en modo de aislamiento {#report-isolation-mode}

En el modo de aislamiento, las MVPD (como XFINITY) identifican de forma consistente a los suscriptores entre dispositivos, pero identifican a sus suscriptores de forma diferente en función de los programadores con los que interactúan. Mientras que en el modo estándar, las MVPD identifican de manera consistente a los suscriptores entre dispositivos, independientemente de los programadores.

Por ejemplo, en la siguiente imagen, si un suscriptor B de una MVPD en modo de aislamiento (como Xfinity) accede al contenido ofrecido por dos programadores diferentes que usan el mismo dispositivo, entonces la MVPD asociará identificadores diferentes con los dos intentos de acceso diferentes. Por lo tanto, para esos programadores (L y M en la figura) y para Account IQ, parece que hay dos suscriptores diferentes que acceden al contenido. Sin embargo, para MVPD estándar, si el suscriptor B accede al contenido ofrecido por dos programadores diferentes, entonces MVPD asociará un único identificador de acceso para ambos intentos de acceso. Las MVPD (como Xfinity) en el modo de aislamiento no identifican de manera consistente a un suscriptor aunque este esté usando el mismo dispositivo en diferentes programadores.

![](assets/isolation-diff-new.png)

*Figura: Modo de aislamiento MVPD identifica cuatro suscriptores diferentes en lugar de dos*

Para gestionar la distorsión de datos (debido a la identificación del mismo suscriptor como diferente en función del acceso a diferentes programadores), el modo de aislamiento limita la actividad informada sobre un programador a la actividad solo en las aplicaciones de ese programador. Por ejemplo, para el modo de aislamiento en la imagen anterior, el programador L ve los datos basados únicamente en la actividad de las identidades W e Y, ignorando las identidades X y Z.

>[!IMPORTANT]
>
> El inconveniente es que el Programador L se ve privado de compartir información recopilada sobre los Suscriptores A y B debido a la actividad con cualquier Programador que no sea L.

En el modo de aislamiento, todos los cálculos realizados para obtener las puntuaciones de uso compartido y todas las métricas asociadas se realizan utilizando únicamente la actividad de los dispositivos que transmiten desde aplicaciones que pertenecen al programador y a los canales seleccionados.
Las puntuaciones y probabilidades de uso compartido se calculan solo mediante el flujo que comienza desde los canales seleccionados actualmente.

Para ver las métricas en el modo de aislamiento:

1. Seleccionar **[!UICONTROL isolation mode]** desde el **[!UICONTROL MVPDs in segment]** y seleccione la opción de menú desplegable **[!UICONTROL Apply Selection]**.

   ![](assets/xfinity-in-segment.gif)

   *Figura: Selección de MVPD en modo de aislamiento*

1. Seleccione los canales que desee en **[!UICONTROL Channels in segment]** y seleccione la opción de menú desplegable **[!UICONTROL Apply Selection]**.

   Seleccione también una [lapso de tiempo](/help/accountiq/product-concepts.md#granularity-def).

   >[!IMPORTANT]
   >
   >Dado que el uso compartido de cuentas es más relevante cuando se mide para la transmisión en línea entre todas las aplicaciones de los programadores, verá puntuaciones de uso compartido más bajas y alguna variación en las métricas en el modo de aislamiento.

   ![](assets/aggregate-sharing-isolation.png)

   *Imagen: uso compartido de indicadores de probabilidad en el modo Aislamiento*

   Tenga en cuenta que los indicadores anteriores muestran que solo el 6% de todas las cuentas se comparten; y solo el 8% del contenido se consume por esos 8%. Así que los canales pueden comparar sus puntuaciones en el modo de aislamiento con las de los otros MVPD. Por lo tanto, la información obtenida mediante el Modo de aislamiento debe interpretarse de forma diferente a los demás datos.
