---
title: Informes de cuentas compartidas
description: Informes de cuentas compartidas
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# Informes de cuentas compartidas {#shared-accounts-reports}

Los Informes de cuentas compartidas desglosan las métricas, como el número de dispositivos y los tipos de dispositivos, según el rango seleccionado de probabilidad de uso compartido, por ejemplo **[!UICONTROL Over Moderate Probability]** y **[!UICONTROL Over Low Probability]** para el segmento actual.

Estos intervalos pueden servir como umbrales definidos por el usuario y los gráficos se actualizan en función de los umbrales seleccionados.

Account IQ clasifica todas las cuentas de suscriptor del segmento definido en las cuentas con las siguientes cinco categorías en función de sus probabilidades de uso compartido:

* Muy alta (80%-100%)
* Alto (60-80%)
* Moderado (40 %-60 %)
* Baja (20%-40%)
* Muy bajo (0%-20%)

## Probabilidad de compartir cuentas {#accounts-sharing-probability}

El gráfico de anillo clasifica y muestra los porcentajes (y números absolutos) de las cuentas de suscriptor de varias categorías de probabilidad.

La línea roja marca el rango de umbral seleccionado por los usuarios en [Cuentas por encima del umbral en el segmento actual](#threshold-selector) panel.

![](assets/accounts-sharing-probability-pie.png)

El gráfico de barras representa el número de cuentas en el eje Y para varias categorías de probabilidades de uso compartido (trazadas en el eje X).

![](assets/accounts-sharing-probability-bar.png)

La línea roja marca el rango de umbral y se puede ajustar en el gráfico de barras. El umbral ajustado en el gráfico de barras se refleja en el intervalo de umbrales del gráfico de anillo.

<!--![](assets/shared-accounts-rep.gif)-->

### Cuentas por encima del umbral en el segmento actual{#threshold-selector}

Este panel le permite seleccionar un rango de los siguientes como umbral para cuentas de suscriptor (según sus probabilidades de uso compartido):

* Cuentas **muy bajo** uso compartido **probabilidad**

* Cuentas **demasiado bajo** uso compartido **probabilidad**

* Cuentas **demasiado moderado** uso compartido **probabilidad**

* Cuentas **demasiado alto** uso compartido **probabilidad**

![](assets/threshold-selector-shared-accounts.png)

Una vez seleccionado el umbral, el panel muestra el porcentaje (y el número) de cuentas de todas las cuentas de suscriptor del segmento seleccionado.

## Segmento: solicitudes de reproducción de un total {#play-request-out-total}

El gráfico de anillo muestra el porcentaje (y el número) de solicitudes de reproducción realizadas por los suscriptores del segmento y le permite comparar las solicitudes de reproducción realizadas por suscriptores que no están en el segmento definido.

![](assets/play-req-outof-total.png)

Cuando mueve el cursor sobre el gráfico de anillo, también muestra porcentajes de suscriptores y números de varios rangos de probabilidad.

<!--![](assets/play-request-total.gif)-->

## Número medio de dispositivos por cuenta según el segmento{#avg-devices-account}

El gráfico de barras muestra el número promedio de dispositivos de cada tipo de dispositivo que utilizan los suscriptores del segmento actual y los suscriptores que no están en el segmento actual.

![](assets/avg-devices-per-acc.png)

## Segmento: códigos postales por período y cuenta {#zip-codes-period-account}

Este gráfico informa sobre la cantidad de suscriptores que consumen contenido de diferentes ubicaciones en un lapso de tiempo.

![](assets/zip-period-account.png)

Puede acercarse para reducir y ver los detalles específicos de una barra en el gráfico que representa una serie de ubicaciones.

<!--![](assets/zip-code-period.gif)-->

## Segmento: intervalo geográfico/período/cuenta {#geo-span-period-account}

Este gráfico de barras representa el número de cuentas de suscriptor con respecto a diferentes rangos geográficos en millas. El rango se basa en la distancia máxima entre las ubicaciones desde las que un suscriptor ha transmitido en streaming durante el lapso de tiempo.

<!--Total number of users ...

How many accounts are within 99 miles of each other.....and how many are apart. 

Based on points on the map.-->

![](assets/geogr-span-account.png)

Al seleccionar una barra que representa un rango de distancia geográfica, se amplía el rango para mostrarle más detalles.

<!--![](assets/geo-span-period-acc.gif)-->
