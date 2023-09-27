---
title: Creación de una operación en un segmento de usuario y seguimiento de efectos
description: Cómo crear una operación que afecte y rastree el efecto en un segmento definido de usuarios.
exl-id: ab74f857-e178-4120-8f9c-655ec921d096
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '970'
ht-degree: 0%

---

# Creación de una operación en un segmento de usuario {#operation-to-track-segment}

Cada página de informes de Account IQ tiene un **Crear nueva operación** opción que permite crear flujos de trabajo para automatizar (y simplificar) diversas acciones (masivas) en las cuentas de suscriptor; definir reglas para especificar un ejemplo, definir acciones y registrar y analizar los efectos de dichas acciones. En la página para crear operaciones, puede definir el ejemplo de grupos de usuarios en los que se realizarán las operaciones y programar la operación para que se ejecute en una fecha futura.

Para crear una operación:

1. Defina su segmento (cohorte) para su análisis en cualquiera de las páginas de informes o paneles, siguiendo los pasos indicados en [Definición de segmentos y periodo de tiempo](/help/accountiq/howto-select-segment-timeframe.md).

1. Seleccionar **[!UICONTROL Create new operation]** opción disponible en cualquiera de las páginas de informes o paneles.

   El **[!UICONTROL Create new operation]** se muestra la página.

   ![Página para crear una nueva operación](assets/create-new-operations.png)
   *Imagen: página para crear una nueva operación*

1. En el **[!UICONTROL Create new operation]** , rellene los detalles de los campos de formulario para:

   * [Nombre de operación](#operation-details) en Detalles de operación
   * Segmento en el que se ejecutará la operación [Segmento de destino](#segment) y perfeccione el segmento utilizando [Segmentación adicional](#additional-segmentation)
   * [Tipo de segmento](#segment-type) bajo [Segmento de destino](#segment)
   * [Acción](#action)
   * [Programar activación](#schedule)

1. [Guarde la operación](#save-operation).

## Detalles de operación {#operation-details}

+++Programador- detalles de operación

Asigne un nombre a la nueva operación en **[!UICONTROL Operation name]** en Detalles de la operación. Por ejemplo, &quot;*Pruebe el efecto de la autenticación multifactor en los suscriptores de MVPD X&quot; o &quot;Limite el número de flujos en la Monitorización de la Concurrencia&quot; o &quot;Limite los suscriptores de MVPD D D que ven el canal &#39;N&#39; desde más de 20 dispositivos*&quot;.

+++

+++MVPD- detalles de operación

Asigne un nombre a la nueva operación en **[!UICONTROL Operation name]** en Detalles de la operación. Por ejemplo, &quot;*Pruebe el efecto de la autenticación multifactor en los visualizadores del canal N&quot;, &quot;Limite el número de emisiones en la Monitorización de la concurrencia&quot; o &quot;Limite los suscriptores que visualizan el canal &#39;N&#39; desde 20 dispositivos más*&quot;.

+++

## Segmento de destino {#segment}

+++Programador- Segmento de destino

El **[!UICONTROL Segment]** aquí se definen los usuarios a los que se va a operar con esta operación o el grupo de muestra de la operación. El segmento predeterminado es **[!UICONTROL segment]** ha seleccionado utilizando [panel de segmento y periodo de tiempo](/help/accountiq/howto-select-segment-timeframe.md) en la página de informes principales o paneles del paso 1 anterior.

<!--* The first segment entry in the **Segment** section, by default, shows the **segment** you selected in the step 1.

* The **segment evaluation period** is the time period of analysis you selected in step 1 from **Granularity and Timeframe** option.
![](assets/operations-segment-selection.png)
*Figure: Segment and timeframe selection on the main page*-->

Este segmento define los suscriptores que se verán afectados por la operación que se está creando. Por ejemplo, el segmento seleccionado podría especificar *todas las cuentas de suscriptor de MVPD llamadas &quot;C&quot; que ven el canal &quot;N Sports&quot;*.

+++

+++MVPD- Segmento de destino

El **Segmento** aquí se definen los usuarios a los que se va a operar con esta operación o el grupo de muestra de la operación. El segmento predeterminado es **segmento** ha seleccionado utilizando [panel de segmento y periodo de tiempo](/help/accountiq/howto-select-segment-timeframe.md) en la página de informes principales o paneles del paso 1 anterior.

<!--* The first segment entry in the **Segment** section, by default, shows the **segment** you selected in the step 1.

* The **segment evaluation period** is the time period of analysis you selected in step 1 from **Granularity and Timeframe** option.
![](assets/operations-segment-selection.png)
*Figure: Segment and timeframe selection on the main page*-->

Este segmento define los suscriptores (que son visualizadores de canales específicos) que se verán afectados por la operación que se está creando. Por ejemplo, el segmento (predeterminado) incluye *todas las cuentas de suscriptor que ven el canal &quot;N Sports&quot;*.
+++

### Segmentación adicional {#additional-segmentation}

Además, puede restringir el segmento de destinatario agregando más métricas. Por ejemplo, puede agregar lo siguiente [!UICONTROL Sharing Probability] que sea superior al 90 % como otra métrica. Ahora la declaración del problema dice *&quot;crear una operación para cuentas de suscriptor de MVPD llamadas &#39;C&#39; que están viendo el canal &#39;N Sports&#39; que tienen una probabilidad de uso compartido mayor del 90%&quot;*.

![](assets/additional-segment.gif)

*Figura: Segmentación adicional*

Además, si refina la operación agregando otra métrica para el número de dispositivos, la instrucción de problema actualizada indica lo siguiente *&quot;crear una operación para cuentas de suscriptor de MVPD llamadas &#39;C&#39; que están viendo el canal &#39;N Sports&#39; que tienen una puntuación de uso compartido superior a 90 y están utilizando más de 5 dispositivos para ver contenido durante el periodo de evaluación&quot;*.

![](assets/refined-segment.png)

*Imagen: segmento de ejemplo refinado con puntuación de uso compartido general y métricas de número de dispositivos*

Al hacerlo, el grupo de usuarios se refina. Por lo tanto, al agregar más métricas y condiciones, está calificando aún más el segmento para definir las cuentas en las que operar.

### [!UICONTROL Segment Type] {#segment-type}

[!UICONTROL Segment Type] es la forma en que se trata un segmento durante todo el periodo de evaluación de la operación.

![](assets/segment-type.png)

*Imagen: refinamiento del número de segmentos en los que operar mediante el tipo de segmento*

<!--The segment type option allows you to further refine your segment based on the evaluation period (or time).

**Fixed number of accounts** 

When you select **Fixed number of accounts** segment type, then you need to specify an evaluation period as well.

By doing so, you are fixing the sample size for evaluation in terms of numbers. You are making Account IQ identify a specific set of users (that meet the criteria of defined evaluation period and segment metrics) to operate on. The analysis and graphs will be generated for this specific set of users only (identified initially) throughout the operation.

**Variable number of accounts**

When you select **Variable number of accounts** segment type, you do not limit the number of accounts in segment. The accounts which fall under the defined segment metrics are the part of the segment, and the number of accounts will change continuously during the course of operation.-->

>[!IMPORTANT]
>
>Solo puede utilizar **[!UICONTROL Fixed number of accounts]** opción, a partir de ahora. La opción para seleccionar **[!UICONTROL Variable number of accounts]** estará disponible en próximas versiones.

<!--

you tell Account IQ in the beginning of the operation which number of accounts to operate on.

Account IQ system only has a segment definition, and during the operation it looks into all the accounts that fit that segments.

the number of accounts in segment is not limited, the accounts that fall under defined segment metrics will be part of the segment, and the no of accounts will change continuously, as there are no specific limitations - like an evaluation period in the past.When the segment is defined (which in this example is, subscriber accounts of MVPD 'C' who are viewing the channel 'N Sports' that have a sharing score above 80 and are using 10 different IPs) and we also identified a time period to evaluate a segment. This identifies X number of accounts as sample (for example 5000). How many devices they are using?
It identifies x-number of accounts (5000)...a very specific set of users that meet this criteria.
for every period that we schedule (within that operation) during that operation) we will look at those 5K users that are originally identified and we will present graph about them. How are the sharing scores coming up?u We identified a period. Are their sharing scores going up? Are there fewer of them who are meeting this definition?
Fixed versus variable is the way the treated in fixed or variable way.

1. we identified a fixed set of accounts.
2. we evaluate those specific accounts on criteria throughout the operation.

General idea independent of graph is that we will evaluate a set of accounts identified initially, for no of periods during operation and generate graphs against that.
Those are the 5000 users for which I will create graphs for for every period of the operation.

**Variable number of accounts**
We do not identify any initial set of accounts, we just have a segment definition.
Each period during the operation, we go and look into all the accounts that fit that segments.
If it is not a fixed segment, I won't initially evaluate it. I won't have an initial set of 5000. Instead at every period during the evaluation I will evaluate the segment then, and then I will produce graph about the next 3000 users.
the......will vary from period to period.

if not fixed segment, then I won't initially evaluate or have initial set of 5000, instead at every period during an operation and the.-->

## [!UICONTROL Action] {#action}

El **[!UICONTROL Action]** define la operación que se realizará en el segmento definido.

Existen dos tipos de acciones que puede realizar:

* Acciones que utilizan sistemas integrados con Account IQ; como **[!UICONTROL Concurrency Monitoring]** <!--[Concurrency Monitoring](https://tve.helpdocsonline.com/concurrency-monitoring-introduction), or Adobe Target-->.

* Acciones para crear y procesar flujos de trabajo que sean externos a Account IQ y no estén integrados con el sistema Account IQ. Por ejemplo, una acción para que el programador de canal &quot;N&quot; envíe correos electrónicos masivos a todos los suscriptores de MVPD &quot;C&quot;.

>[!NOTE]
>
>Al crear operaciones, no solo se especifican acciones y se define su ámbito, sino que también se comienza a registrar el efecto de estas operaciones.

## Programación{#schedule}

Puede programar la activación de la operación estableciendo las fechas de inicio y finalización.

>[!NOTE]
>
>La fecha de inicio y la fecha de finalización tienen una granularidad igual a la granularidad seleccionada para la evaluación al definir segmentos mediante **[!UICONTROL segment and timeframe panel]**, en el paso 1.
>
>
>Por lo tanto, si seleccionó la granularidad como Semana, las fechas de inicio y finalización se expresan en términos de semana (por ejemplo, Semana 14); si selecciona la granularidad como Mes, las fechas de inicio y finalización se expresan en términos de meses.


>[!IMPORTANT]
>
>La fecha de inicio debe ser posterior al periodo de evaluación y también a la fecha actual. Del mismo modo, la fecha de finalización también debe ser posterior a la fecha de inicio y a la fecha actual.

### Guarde la operación {#save-operation}

Al guardar la operación, se muestra una pantalla de mensaje que le informa de que el segmento definido en esta operación también se guarda para futuras operaciones. Sin embargo, debe asignar un nombre a este segmento.

![](assets/save-operation.png)

*Imagen: operación de guardado y especificación del nombre del segmento*

>[!NOTE]
>
>Se recomienda asignar un nombre a la operación en función de la acción que esté realizando en combinación con el segmento sobre el que actuará.

<!--In future you can select this saved segment when defining a segment for your analysis on the main reports page. Moreover, the saved segment is also listed when you create an operation the next time.

![](assets/saved-segment-operations-page.png)

*Figure: Saved segments in segment selector on Create new operations page* 

>[!IMPORTANT]
>
>When creating an operation, if you select a segment that was previously created then you cannot add new metrics to it and refine it.
>
>Adding new metrics creates a new segment, but you cannot modify an existing segment.-->

Una vez creada una operación, se ejecutará desde la fecha de inicio hasta la fecha de finalización especificada.

Los detalles de la operación guardada se pueden ver en la página principal [Operaciones](/help/accountiq/operations.md) página.

![](assets/new-operation-created.png)

*Imagen: la operación recién creada aparece en la página principal de Operaciones*
