---
title: Segmentos e intervalo de tiempo
description: Defina cohortes o seleccione segmentos de suscriptores para medir las posibilidades de uso compartido de cuentas y los patrones de los visualizadores de canales a fin de utilizar herramientas gráficas e informes en Account IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# Segmentos e intervalo de tiempo {#segment-timeinterval}

Al iniciar sesión en Account IQ, el panel de segmento e intervalo de tiempo situado encima del panel le permite definir el suscriptor [segment](product-concepts.md#segmet-def). Este panel ayuda a filtrar los resultados y mostrar informes sobre el comportamiento y los patrones de uso compartido de los suscriptores. Un segmento con el nombre **TODAS LAS CUENTAS DE SUS PROPIEDADES** está seleccionado actualmente de forma predeterminada, donde puede ver las siguientes opciones:

![](assets/new-segment-selector-collapsed.png){align="left"}

*Panel de intervalo de tiempo y segmento con resumen de segmento contraído*

**A.** Nombre de segmento seleccionado actualmente **B.** Abrir lista de segmentos **C.** Editar segmento **D.** Crear nuevo segmento **E.** Selector de granularidad e intervalo de tiempo **F.** Icono para expandir resumen de segmento **G.** Resumen de segmento contraído **H.** Número de cuentas en el segmento para el intervalo seleccionado

>[!NOTE]
>
> El resumen del segmento contraído muestra las [categorías de vídeo](product-concepts.md#video-category-def) utilizadas en la versión de TV Everywhere de Account IQ. Si ha iniciado sesión como servicio D2C, estas etiquetas muestran las categorías de vídeo específicas de su empresa.

Obtenga más información sobre [cómo crear](work-with-segments.md#create-new-segment) y [administrar segmentos](work-with-segments.md#manage-segment) desde la pestaña **Segmentos** del panel izquierdo.

## Selección de segmentos {#segment-selection}

Para seleccionar un segmento específico, siga estos pasos:

1. Vaya a la opción **[!UICONTROL Open segment]** dentro del panel de segmento e intervalo de tiempo.
1. Seleccione el **Nombre del segmento** cuyos informes de uso compartido de cuentas desee ver.

   ![](assets/open-segment.png){align="left"}

   *Seleccionar nombre de segmento*

   >[!NOTE]
   >
   > Las categorías de vídeo mostradas en la imagen anterior, como **MVPD**, **Programadores** y **Canales** representan las etiquetas utilizadas en la versión de Account IQ de TV Everywhere. Si ha iniciado sesión como servicio D2C, estas etiquetas muestran las categorías de vídeo específicas de su empresa.

1. Seleccione **[!UICONTROL Open segment]**.


## Granularidad y selección del intervalo de tiempo {#granularity-timeinterval}

El selector **Granularidad e intervalo de tiempo** le permite especificar las fechas y la duración agregadas de forma semanal o mensual para observar el comportamiento de uso compartido del suscriptor. La selección predeterminada es la semana actual.

![Granularidad e intervalo de tiempo](assets/granularity-timeinterval-weekwise.png){align="left"}

*Cuadro de diálogo de granularidad e intervalo de tiempo*

**A.** Selector de granularidad e intervalo de tiempo **B.** Flecha derecha para ir al mes/semana siguiente **C.** Opción para elegir la granularidad por semana/mes **D.** Intervalo de tiempo seleccionado actualmente **E.** Flecha izquierda para ir al mes/semana anterior

Puede modificar la duración siguiendo estos pasos:

1. Seleccione **[!UICONTROL Granularity and Time Interval]** del selector de fechas.

1. Seleccione la opción **[!UICONTROL Week]** o **[!UICONTROL Month]** de **[!UICONTROL Aggregate By]** para establecer la granularidad de la evaluación.

1. Una vez seleccionada la granularidad, puede utilizar las flechas hacia delante y hacia atrás para navegar por el intervalo de tiempo.

1. Seleccione un período de tiempo específico para la evaluación.

1. Seleccione **[!UICONTROL Apply]** para asegurarse de que la selección surta efecto.

Esto le permite definir la declaración de problemas como &quot;Suscriptores de MVPD A que vieron los canales X, Y y Z durante la semana de diciembre elegida&quot;.

## Resumen de segmentos {#segment-summary}

El resumen de segmentos es similar para los servicios D2C y TV en todas partes. Las categorías de vídeo serán diferentes para cada versión respectiva de Account IQ.

Seleccionar Icono <img alt= "expandir resumen de segmentos" src="./assets/expand-segment-summary.svg" width="25"> para ver el resumen detallado del segmento. También presenta información sobre el número de cuentas de suscriptor y sus solicitudes de reproducción dentro del período de tiempo elegido.

+++ Servicios D2C

![](assets/segment-panel-d2c.png){align="left"}

*Resumen de segmentos para servicios D2C*

>[!NOTE]
>
>Las [categorías de vídeo](product-concepts.md#video-category-def) mostradas en la imagen anterior, como **región** y **tipos de contenido** del segmento, son solo ejemplos. Al iniciar sesión en Account IQ, estas etiquetas muestran las categorías de vídeo específicas de la empresa.

El **Resumen de segmentos** incluye las siguientes condiciones que definen un segmento:

**[Las regiones y los tipos de contenido](product-concepts.md#video-category-def) del segmento** hacen referencia a las etiquetas de metadatos asociadas con las secuencias de vídeo que ven las cuentas compartidas representadas en los informes de uso compartido de cuentas.

**[Las métricas](product-concepts.md#metric) del segmento** hacen referencia a atributos o criterios que los suscriptores deben cumplir para ser identificados en los informes de uso compartido de cuentas.

+++

+++ TV en todas partes

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*Resumen de segmentos para programadores/MVPD*

El **Resumen de segmentos** incluye las siguientes condiciones que definen un segmento:

**[Los programadores](product-concepts.md#programmer-def) del segmento** hacen referencia a proveedores de contenido cuyos flujos de vídeo fueron vistos por cuentas compartidas representadas en informes de uso compartido de cuentas.

**[Los canales](product-concepts.md#channel-def) del segmento** hacen referencia a canales cuyos flujos de vídeo fueron vistos por cuentas compartidas representadas en informes de uso compartido de cuentas.

**[MVPD](product-concepts.md#mvpd-def) en el segmento** se refieren a distribuidores de programación de vídeo múltiple a los que están asociados los suscriptores para ser identificados en informes de uso compartido de cuentas.

**[Las métricas](product-concepts.md#metric) del segmento** hacen referencia a atributos o criterios que los suscriptores deben cumplir para ser identificados en los informes de uso compartido de cuentas.

+++
