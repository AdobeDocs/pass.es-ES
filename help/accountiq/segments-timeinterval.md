---
title: Segmentos e intervalo de tiempo
description: Defina cohortes o seleccione segmentos de suscriptores para medir las posibilidades de uso compartido de cuentas y los patrones de sus visualizadores de canales para utilizar herramientas gráficas e informes en Account IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# Segmentos e intervalo de tiempo {#segment-timeinterval}

Al iniciar sesión en Account IQ, el panel de segmentos e intervalos de tiempo situado encima del panel le permite definir el suscriptor [segmento](product-concepts.md#segmet-def). Este panel ayuda a filtrar los resultados y mostrar informes sobre el comportamiento y los patrones de uso compartido de los suscriptores. Un segmento llamado **TODAS LAS CUENTAS DE SUS PROPIEDADES** está seleccionado de forma predeterminada, donde puede ver las siguientes opciones:

![](assets/new-segment-selector-collapsed.png){align="left"}

*Panel de intervalos de tiempo y segmento con resumen de segmento contraído*

**A.** Nombre del segmento seleccionado actualmente **B.** Abrir lista de segmentos **C.** Editar segmento **D.** Crear nuevo segmento **E.** Selector de granularidad e intervalo de tiempo **F..** Icono para expandir el resumen del segmento **G.** Resumen de segmento contraído **H..** Número de cuentas en el segmento para el intervalo seleccionado

>[!NOTE]
>
> El resumen del segmento contraído muestra el [Categorías de vídeo](product-concepts.md#video-category-def) se utiliza en la versión de TV Everywhere de Account IQ. Si ha iniciado sesión como servicio D2C, estas etiquetas muestran las categorías de vídeo específicas de su empresa.

Más información sobre [cómo crear](work-with-segments.md#create-new-segment) y [administrar segmentos](work-with-segments.md#manage-segment) desde el **Segmentos** en el panel izquierdo.

## Selección de segmentos {#segment-selection}

Para seleccionar un segmento específico, siga estos pasos:

1. Vaya a **[!UICONTROL Open segment]** dentro del panel de segmentos e intervalos de tiempo.
1. Seleccione el **Nombre del segmento** para la que desea ver los informes de uso compartido de cuentas.

   ![](assets/open-segment.png){align="left"}

   *Seleccionar nombre del segmento*

   >[!NOTE]
   >
   > Las categorías de vídeo mostradas en la imagen anterior, como **MVPD**, **Programadores**, y **Canales** representan las etiquetas utilizadas en la versión de TV Everywhere de Account IQ. Si ha iniciado sesión como servicio D2C, estas etiquetas muestran las categorías de vídeo específicas de su empresa.

1. Seleccionar **[!UICONTROL Open segment]**.


## Granularidad y selección del intervalo de tiempo {#granularity-timeinterval}

El **Granularidad e intervalo de tiempo** El selector de permite especificar las fechas y la duración agregadas de forma semanal o mensual para observar el comportamiento de uso compartido del suscriptor. La selección predeterminada es la semana actual.

![Granularidad e intervalo de tiempo](assets/granularity-timeinterval-weekwise.png){align="left"}

*Cuadro de diálogo Granularidad e intervalo de tiempo*

**A.** Selector de granularidad e intervalo de tiempo **B.** Flecha derecha para ir al mes o semana siguiente **C.** Opción para elegir la granularidad por semana/mes **D.** Intervalo de tiempo seleccionado actualmente **E.** Flecha izquierda para ir al mes o semana anterior

Puede modificar la duración siguiendo estos pasos:

1. Seleccione el **[!UICONTROL Granularity and Time Interval]** del selector de fechas.

1. Seleccione una de las opciones **[!UICONTROL Week]** o **[!UICONTROL Month]** de **[!UICONTROL Aggregate By]** para definir la granularidad de la evaluación.

1. Una vez seleccionada la granularidad, puede utilizar las flechas hacia delante y hacia atrás para navegar por el intervalo de tiempo.

1. Seleccione un período de tiempo específico para la evaluación.

1. Seleccionar **[!UICONTROL Apply]** para asegurarse de que la selección surta efecto.

Esto le permite definir la declaración de problemas como &quot;Suscriptores de MVPD A que vieron los canales X, Y y Z durante la semana de diciembre elegida&quot;.

## Resumen de segmentos {#segment-summary}

El resumen de segmentos es similar para los servicios D2C y TV en todas partes. Las categorías de vídeo serán diferentes para cada versión respectiva de Account IQ.

Seleccionar <img alt= "expandir resumen de segmentos" src="./assets/expand-segment-summary.svg" width="25"> para ver el resumen detallado del segmento. También presenta información sobre el número de cuentas de suscriptor y sus solicitudes de reproducción dentro del período de tiempo elegido.

+++ Servicios D2C

![](assets/segment-panel-d2c.png){align="left"}

*Resumen de segmentos para servicios D2C*

>[!NOTE]
>
>El [categorías de vídeo](product-concepts.md#video-category-def) se muestra en la imagen anterior, como **región** y **tipos de contenido** en el segmento son solo ejemplos. Al iniciar sesión en Account IQ, estas etiquetas muestran las categorías de vídeo específicas de su empresa.

El **Resumen de segmentos** incluye las siguientes condiciones que definen un segmento:

**[Regiones y tipos de contenido](product-concepts.md#video-category-def) en el segmento** consulte las etiquetas de metadatos asociadas con los flujos de vídeo vistos por cuentas compartidas representadas en informes de uso compartido de cuentas.

**[Métricas](product-concepts.md#metric) en el segmento** consulte los atributos o criterios que los suscriptores deben haber cumplido para ser identificados en los informes de uso compartido de cuentas.

+++

+++ TV en todas partes

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*Resumen de segmentos para programadores/MVPD*

El **Resumen de segmentos** incluye las siguientes condiciones que definen un segmento:

**[Programadores](product-concepts.md#programmer-def) en el segmento**  consulte proveedores de contenido cuyos flujos de vídeo fueron vistos por cuentas compartidas representadas en informes de uso compartido de cuentas.

**[Canales](product-concepts.md#channel-def) en el segmento** consulte canales cuyos flujos de vídeo han sido vistos por cuentas compartidas representadas en informes de uso compartido de cuentas.

**[MVPD](product-concepts.md#mvpd-def) en el segmento** consulte distribuidores de programación de vídeo múltiple a los que están asociados los suscriptores para poder identificarlos en los informes de uso compartido de cuentas.

**[Métricas](product-concepts.md#metric) en el segmento** consulte los atributos o criterios que los suscriptores deben haber cumplido para ser identificados en los informes de uso compartido de cuentas.

+++
