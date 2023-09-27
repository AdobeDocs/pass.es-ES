---
title: Informes de uso general
description: Informes de uso general
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# [!UICONTROL General Usage] informes {#general-usage-reports}

[!UICONTROL Account IQ] los informes son herramientas de análisis básicas y son informes que le permiten explorar en profundidad los datos para aislarlos [cohortes](/help/accountiq/product-concepts.md#segmet-def), identifique anomalías y comprenda las características de la cuenta.

[!UICONTROL General Usage] La página Informes proporciona herramientas para crear subgrupos de métricas en función del número de dispositivos de cuenta en uso, las direcciones IP detectadas y los códigos postales correspondientes.

<!--Divide the content in cohorts.

Content filters
device filters

segment and definition replicate to cohorts. Number of people and number of account that ......
content consumption.....-->

Todos los informes se basan en el segmento actual seleccionado usando [Segmentos y lapso de tiempo](/help/accountiq/howto-select-segment-timeframe.md) panel. Puede ajustar la selección y reducirla aún más especificando umbrales (número de dispositivos, número de direcciones IP y número de códigos postales) en [Resumen de Instantáneas: Cuentas por encima de los umbrales](#snapshot-overview) panel.

<!--To view General Usage Reports:

1. Select the desired MVPDs from the **MVPDs in Segment** option.

2. Select the desired programmer channels from the **Channels in Segment** Option.

3. Select an appropriate time frame from the **Granularity and time frame** option.

   Using the above options you have defined segments for your analysis. Based on your segment selection, following graphs and reports are displayed.

4. You can fine tune your selection and further narrow it down by specifying (number of devices, number of IPs, and number of zip codes) thresholds in [Snapshot Overview - Accounts above thresholds](#snapshot-overview) widget/panel.-->

## AuthN OK / AuthZ OK / Solicitudes de reproducción / Suscriptores únicos {#authn-authz-playreq-uniquesubs}

Los gráficos de líneas aquí le proporcionan una vista de los cambios a lo largo del tiempo en los valores de AuthN OK, AuthZ OK, Solicitudes de reproducción y Suscriptores únicos en un lapso de tiempo seleccionado para el segmento definido.

+++Programador- **AuthN OK / AuthZ OK / Solicitudes de reproducción / Suscriptores únicos**

![](assets/progr-line-graph-gu.png)


*Figura: AuthN OK / AuthZ OK / Solicitudes de reproducción / Suscriptores únicos para usuario programador*


+++


+++MVPD- **AuthN OK / AuthZ OK / Suscriptores únicos**

![](assets/mvpd-line-graph-gu.png)


*Figura: AuthN OK / AuthZ OK / Suscriptores únicos para usuario de MVPD*


+++

El eje x presenta las unidades dentro del lapso de tiempo actual y el eje y representa las métricas básicas de actividad del suscriptor durante ese período. Los gráficos de líneas le permiten comparar los siguientes valores para los suscriptores de MVPD y canales que seleccionó en el panel de selección de segmentos:

* **AuthN OK**

  AuthN OK es el número de autenticaciones correctas. Para obtener más información y definición, consulte [Conceptos de producto: AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK**

  AuthZ OK es el número de autorizaciones correctas. Para obtener más información y definición, consulte [Conceptos de producto: AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Solicitudes de reproducción**

  Las solicitudes de reproducción son el número de solicitudes de reproducción. Para obtener más información y definición, consulte [Conceptos de producto: solicitudes de reproducción](/help/accountiq/product-concepts.md#play-requests-def)

  >[!NOTE]
  >
  >El gráfico de líneas de solicitudes de reproducción no está disponible para los usuarios de MVPD.


* **Suscriptores únicos**

  Los suscriptores únicos son el número de suscriptores únicos correctos. Para obtener más información y definición, consulte [Conceptos de producto: suscriptores únicos](/help/accountiq/product-concepts.md#unique-subscriber-def)

  >[!NOTE]
  >
  >El número total de suscriptores únicos también incluye el número de dispositivos únicos si el uso de TempPass de Adobe por parte de un programador (es decir, vista previa gratuita) es parte del segmento.

## Resumen de Instantáneas: Cuentas por encima de los umbrales {#snapshot-overview}

Ajuste los análisis e informes con este filtro adicional para establecer varios umbrales de uso. Una vez que haya definido el segmento (o cohorte) para su análisis seleccionando los canales y las MVPD deseados, también puede utilizar los siguientes filtros para analizar el comportamiento de los suscriptores:

* Umbral de número de dispositivos

* Umbral de número de direcciones IP

* Umbral de número de códigos postales

Cuando se actualizan los valores de umbral en [Segmento de cuentas: según los umbrales seleccionados](#account-segments-basedon-segments) , puede ver el efecto en:

* [Dispositivos por semana (o mes) por cuenta](#devices-week-account)

* [Ubicaciones por semana (o mes) por cuenta](#locations-week-account)

* [IP por semana (o mes) por cuenta](#ip-week-account)

* [Vista histórica del segmento de cuentas](#account-segment-historical-view)

>[!NOTE]
>
>El valor predeterminado para cada uno de los umbrales es 4. Lo que significa que la página Uso general muestra un análisis de MVPD con suscriptores que usan cuatro (y más de cuatro) dispositivos y consumen contenido de cuatro (y más) ubicaciones geográficas diferentes y cuatro (y más) códigos postales diferentes.

### Segmento de cuentas: según los umbrales seleccionados {#account-segments-basedon-segments}

El **Segmento de cuentas: según los umbrales seleccionados** El panel le ofrece opciones para establecer umbrales (entre 1 y 10) para el número de dispositivos, el número de direcciones IP y el número de códigos postales.

El gráfico muestra lo siguiente:

* número absoluto de cuentas de suscriptor y

* porcentaje del total de cuentas de suscriptor en ese segmento,

  que utilizan un número X de dispositivos, un número Y de direcciones IP y un número Z de códigos postales para consumir contenido de su canal para las MVPD (segmento definido de), durante un periodo de tiempo.

![](assets/select-thresholds.png)

## Dispositivos por semana (o mes) por cuenta {#devices-week-account}

El **gráfico de barras** proporciona perspectivas sobre el comportamiento de uso en términos de cómo los suscriptores utilizan sus dispositivos para acceder al contenido.

El eje x representa el número de cuentas y el eje y el número de dispositivos. En función del umbral que establezca para el número de dispositivos por cuenta, marca el número absoluto de cuentas de suscriptores que consumen contenido de un número específico de dispositivos en una semana.

![](assets/bar-gr-devices-w-acc.png)

Al pasar el ratón por encima de una barra (específica del número de dispositivos), aparece una etiqueta que proporciona información sobre el número de cuentas de suscriptor (y el porcentaje del total de cuentas de suscriptor en el segmento) que están transmitiendo contenido de canal mediante esos dispositivos en una semana.

El gráfico también marca lo siguiente:

* Una línea roja para marcar el umbral establecido.

* Una línea verde para marcar el promedio del número de dispositivos diferentes utilizados por una cuenta de suscriptor por semana (o mes).

Puede comparar el nivel de umbral con la media semanal del número de dispositivos diferentes utilizados por una cuenta para valorar el nivel de uso compartido.

El gráfico también proporciona un vistazo al porcentaje de cuentas de suscriptor que utilizan más dispositivos que el umbral establecido.

El gráfico de anillo le ayuda a juzgar la magnitud de las cuentas de suscriptores que consumen contenido del canal mediante dispositivos más que el umbral establecido (en un periodo de tiempo) de un vistazo.

![](assets/donut-devices-w-acc.png)

## Ubicaciones por semana (o mes) por cuenta {#locations-week-account}

Like [Dispositivos por semana (o mes) por cuenta](#devices-week-account), la métrica Ubicaciones por semana (o mes) y por cuenta le ayuda a analizar el uso de la cuenta del suscriptor desde diferentes ubicaciones para identificar mejor el uso compartido de contraseñas. El eje x representa el número de cuentas y el eje y el número de ubicaciones.

Resultados de esta métrica combinados con el número de [Dispositivos por semana (o mes) por cuenta](#devices-week-account) y número de [IP por semana (o mes) por cuenta](#ip-week-account) le ayuda a valorar con mayor precisión las instancias de uso compartido de contraseñas, de modo que los usuarios auténticos no se cuentan en.

![](assets/graph-loc-week-acc.png)

Una vez que haya definido un segmento y establecido el umbral para el número de ubicaciones, puede identificar desde el gráfico:

* Número (y porcentaje) de suscriptores que consumen contenido de (una ubicación específica) x número de ubicaciones en una semana.

* Porcentaje del total de cuentas de suscriptor que están viendo contenido desde más ubicaciones que el umbral.

* Comparar el promedio semanal (número de ubicaciones diferentes para una cuenta) con el umbral.

## IP por semana (o mes) por cuenta {#ip-week-account}

Similar a [Dispositivos por semana (o mes) por cuenta](#devices-week-account) y [Ubicaciones por semana (o mes) por cuenta](#locations-week-account), el **Número de direcciones IP por semana por cuenta** Esta métrica le permite analizar el uso compartido de contraseñas de forma más precisa y con mayor granularidad.

El eje x representa el número de cuentas y el eje y el número de direcciones IP.

![](assets/graph-ip-week-acc.png)

Una vez que haya definido un segmento (seleccionando MVPD y canales) y establecido el umbral para el número de IP, puede identificar desde el gráfico:

* Número (y porcentaje) de suscriptores que consumen contenido de (una cantidad específica) x número de IP en una semana.

* Porcentaje del total de cuentas de suscriptor que están viendo contenido desde más direcciones IP que el umbral.

* Comparar el promedio semanal (número de direcciones IP diferentes para una cuenta) con el umbral.

## Segmento de cuentas: vista histórica {#account-segment-historical-view}

El gráfico de barras Vista histórica le ayuda a comparar las métricas de uso en diferentes lapsos de tiempo. Además, traza colectivamente las distintas métricas de uso, como [Dispositivos por semana (o mes) por cuenta](#devices-week-account), [Ubicaciones por semana (o mes) por cuenta](#locations-week-account), y [IP por semana (o mes) por cuenta](#ip-week-account).

* El eje x representa el lapso de tiempo y el eje y representa el número de cuentas de suscriptor, dispositivos, ubicaciones e IP.

* Las barras de color naranja significan segmentos en varios lapsos de tiempo.

* El gráfico de líneas representa los cambios en [Dispositivos por semana (o mes) por cuenta](#devices-week-account), [Ubicaciones por semana (o mes) por cuenta](#locations-week-account), y [IP por semana (o mes) por cuenta](#ip-week-account) valores a lo largo del lapso de tiempo en función del umbral.

![](assets/historical-view.png)

* Las barras azules indican el número total de suscriptores activos en el sector durante un periodo de tiempo.

* Puede seleccionar leyendas específicas, que le ayudarán a escalar el gráfico.

![](assets/historical-view-total.png)

>[!MORELIKETHIS]
>
>* Obtenga información sobre cómo exportar informes para los 1000 suscriptores principales del segmento seleccionado mediante filtros en Informe de uso general mediante [Exportar las 1000 cuentas principales](/help/accountiq/export-acc-information.md) opción.
