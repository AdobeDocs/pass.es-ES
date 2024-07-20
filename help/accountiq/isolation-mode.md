---
title: MVPD en modo de aislamiento
description: Obtenga información sobre las MVPD de modo de aislamiento para programadores de TV Everywhere
exl-id: 7ffe4ce3-e9cb-4382-a421-bb57d1927b53
source-git-commit: 2bb570ab14a3295d46ee6dc0d38485697d63809c
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---

# MVPD en modo de aislamiento para programadores de TV Everywhere {#isolation-mode-tve}

>[!IMPORTANT]
>
> La limitación de MVPDs del modo de aislamiento solo es aplicable a los programadores de TV Everywhere.

En el modo de aislamiento, las MVPD (como Xfinity) identifican de manera consistente a los suscriptores entre dispositivos en función de sus interacciones con programadores específicos. En el modo estándar, las MVPD identifican de manera consistente a los suscriptores entre dispositivos independientemente de los programadores involucrados.

A continuación se muestra un ejemplo:

![](assets/isolation-diff-new.png)

*Las MVPD de modo de aislamiento identifican cuatro suscriptores diferentes en lugar de dos*

* Si un suscriptor B de una MVPD de modo de aislamiento (como Xfinity) accede al contenido ofrecido por dos programadores diferentes que usan el mismo dispositivo, entonces la MVPD asociará identificadores diferentes con los dos intentos de acceso diferentes. Parece ser que hay dos suscriptores diferentes que acceden al contenido para los programadores (L y M en la figura).

* En el caso de las MVPD estándar, si el suscriptor B accede al contenido ofrecido por dos programadores diferentes, entonces la MVPD asociará un único identificador de acceso para ambos intentos de acceso.

* Las MVPD (como Xfinity) en el modo de aislamiento no identifican de manera consistente a un suscriptor, aunque este utilice el mismo dispositivo en diferentes programadores.

Para evitar la distorsión de los datos causada por el recuento de un solo suscriptor como varios suscriptores debido al acceso a diferentes programadores, el modo de aislamiento restringe la actividad registrada sobre un programador únicamente a sus aplicaciones.

Por ejemplo, el programador L puede ver datos basados únicamente en la actividad de las identidades W e Y, omitiendo las identidades X y Z de la imagen anterior.

>[!IMPORTANT]
>
> El inconveniente es que el Programador L se ve privado de compartir información recopilada sobre los Suscriptores A y B debido a la actividad con cualquier Programador que no sea L.

En el modo de aislamiento, las puntuaciones de uso compartido y las métricas asociadas se calculan únicamente a partir de la actividad de los dispositivos que transmiten desde las aplicaciones del programador y del canal seleccionados. Las puntuaciones y probabilidades de uso compartido se calculan a partir de los inicios de flujo en los canales seleccionados actualmente.

El sistema funciona automáticamente en modo de aislamiento cuando el segmento seleccionado contiene una MVPD en modo de aislamiento que identifica a los suscriptores únicos como varios suscriptores al transmitir desde diferentes programadores. Todos los gráficos y diagramas de estos segmentos reflejarán los resultados de este comportamiento alterado.

>[!IMPORTANT]
>
> El comportamiento en el modo de aislamiento es incompatible con el modo estándar, el modo de aislamiento MVPD no se puede mezclar con otras MVPD y viceversa.

Para crear un segmento analizado en el modo de aislamiento, arrastre MVPD del modo de aislamiento, como **Xfinity**, a la sección MVPD de la definición del segmento.

>[!NOTE]
>
> Dado que las MVPD en modo de aislamiento no pueden mezclarse con otras MVPD, la sección de MVPD de la definición del segmento no permitirá que se arrastre allí otra MVPD.

![](assets/xfinity-in-segment.png)

*Selección de Xfinity en modo de aislamiento*

>[!IMPORTANT]
>
> El uso compartido de cuentas es más relevante cuando se mide para su transmisión por secuencias en todas las aplicaciones del programador. Se esperan **puntuaciones de uso compartido** más bajas y alguna variación en las métricas cuando se encuentre en modo de aislamiento.

![](assets/aggregate-sharing-isolation.png)

*Indicadores de probabilidad de uso compartido en modo de aislamiento*

Los indicadores anteriores muestran que solo el 9% de todas las cuentas se comparten, y entre ellas, solo se consume el 11% del contenido. Debido a las puntuaciones naturalmente más bajas, los resultados en el modo Aislamiento deben interpretarse de forma diferente a los resultados en el modo estándar.
