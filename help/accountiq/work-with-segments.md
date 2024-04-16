---
title: Trabajo con segmentos
description: Explicación y uso de los segmentos. Obtenga información sobre cómo crear y administrar un segmento.
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1970'
ht-degree: 0%

---

# Trabajo con segmentos {#work-with-segments}

[Segmentos](product-concepts.md#segmet-def) son una colección de cuentas de suscriptor que permiten analizar el uso compartido de credenciales bajo condiciones definidas por el usuario. Puede utilizar segmentos para examinar diferentes conjuntos de cuentas de suscriptor y generar los informes de datos correspondientes en tablas y gráficos. Existen dos tipos de segmentos en Account IQ:

1. **Segmento predeterminado**: **Todas las cuentas de sus propiedades** es un segmento predeterminado del sistema que incluye todas las cuentas de suscriptor activas sin condiciones específicas aplicadas.

   >[!NOTE]
   >
   >El uso del segmento predeterminado puede impedir la visualización de ciertas tablas como [Categorías de vídeo en el segmento](data-panels.md#video-categories-segment), [Puntuación de uso compartido por canales y MVPD](data-panels.md#sharin-score-by-channels-and-mvpds), y [Distribución del patrón de uso para categorías de vídeo](usage-patterns.md#usage-pattern-dis-video-categories). Estas tablas solo pueden admitir y mostrar datos de hasta 20 filas a la vez. El resto de tablas, gráficos e informes son los mismos para los segmentos predeterminados y personalizados.

1. **Segmentos personalizados**: Son segmentos personalizados que permiten agrupar cuentas de suscriptor de categorías específicas, como tipos de contenido D2C, programadores, canales y MVPD para analizar el uso compartido de credenciales en condiciones definidas por el usuario. Obtenga más información sobre cómo [crear un segmento personalizado](#create-new-segment).

   >[!IMPORTANT]
   >
   >Todos los procedimientos descritos en esta guía se basan en segmentos personalizados. Sin embargo, los conceptos siguen siendo los mismos para los segmentos predeterminados y personalizados.

Cuando vaya a la **Acciones** y seleccione la **[!UICONTROL Segments]** en el panel izquierdo, se muestra una lista de los segmentos disponibles en el sistema. La página de segmentos le permite evaluar rápidamente los detalles clave sobre cada segmento en formato de tabla. Los detalles incluyen el nombre del segmento, el número de [categorías de vídeo](product-concepts.md#video-category-def), métricas, [operaciones](product-concepts.md#operation-def) uso del segmento actual, fecha y hora de la última modificación, así como el nombre del creador del segmento.

Puede realizar las siguientes funciones con segmentos:

* [Crear un nuevo segmento](#create-new-segment)
* [Administración de segmentos](#manage-segments)


## Crear un nuevo segmento {#create-new-segment}

El proceso de creación de un nuevo segmento es similar para los servicios D2C y TV en todas partes. Las categorías de vídeo serán diferentes para cada versión respectiva de Account IQ.

Servicios +++D2C

Para crear un segmento y analizar el comportamiento compartido del suscriptor, seleccione **[!UICONTROL Create new segment]** en la parte superior derecha.

![Seleccione Crear nuevo segmento](assets/d2c-create-new-segment.png)

*Seleccione Crear nuevo segmento*

>[!NOTE]
>
>Las categorías de vídeo mostradas en la imagen anterior, como **Regiones** y **Tipos de contenido** son solo ejemplos. Al iniciar sesión en Account IQ, estas etiquetas muestran las categorías de vídeo específicas de su empresa.

Se abre una **Nuevo segmento** , que incluye los siguientes elementos:

![Nueva página de segmento](assets/d2c-new-segment-dialog.png)

*Nueva página de segmento*

**A.** Componentes de segmento **B.** Definición del segmento **C.** Resumen de segmentos

* **Componentes de segmento**: un inventario de [categorías de vídeo](product-concepts.md##video-category-def) y métricas calculadas utilizadas para definir un segmento.

  >[!NOTE]
  >
  >Uso **[!UICONTROL Show all]** para expandir la lista de componentes de segmentos. Para buscar un componente rápidamente, busque su nombre en **buscar componentes de segmento** en lugar de desplazarse por toda la lista.

* **Definición del segmento**: Un lienzo en el que puede arrastrar y soltar varios componentes de segmento para crear un segmento.

* **Resumen de segmentos**: Resumen que calcula las cuentas cualificadas en función de los componentes de la definición del segmento y proporciona una breve descripción del segmento durante el periodo de evaluación.

Siga estos pasos para crear un segmento:

1. Escriba el nombre del segmento en **Nombre del segmento** que se pueden ver en la lista de segmentos y durante la selección de segmentos.
1. Escriba una descripción detallada del segmento en **Descripción del segmento**.
1. Por ejemplo, arrastre **Regiones y tipos de contenido** de los componentes del segmento en el panel izquierdo y suéltelos en el **Regiones/Tipos de contenido** dentro de la sección **Definición del segmento**.

   >[!NOTE]
   >
   >Puede crear un segmento basado en regiones o tipos de contenido. Ver los tipos de contenido asociados de una región desde un menú desplegable.

   Si empieza por agregar un **tipo de contenido** en el **Regiones/Tipos de contenido** , solo puede añadir tipos de contenido como componentes subsiguientes.

   Si empieza por agregar un **Región** en el **Regiones/Tipos de contenido** , se muestra un cuadro de diálogo de decisión.

   ![Agregar un componente de segmento como área o sus tipos de contenido ](assets/d2c-segment-basis-selector.png){width="550" align="left"}

   *Agregar componente de segmento como área o cuadro de diálogo de tipos de contenido*

   Decida si desea comparar regiones específicas o un segmento en función de los tipos de contenido asociados a una región.

   Seleccionar **[!UICONTROL As a region]** para agregar regiones a **Regiones/Tipos de contenido** sección.

   Seleccionar **[!UICONTROL As its content types]** para agregar tipos de contenido de una región.

1. Arrastrar **Métricas** de los componentes del segmento en el panel izquierdo y suéltelos en el **Métricas** dentro de la sección **Definición del segmento**.

   ![Seleccione un operador y defina un valor para la métrica añadida](assets/component-metrics.png)

   *Seleccione un operador y asigne un valor para la métrica añadida*

   Después de añadir métricas en la definición del segmento, elija un operador de **[!UICONTROL Select an operator]** menú desplegable y asigne un valor mediante **[!UICONTROL Select an option]**.

   Ajuste los valores de determinadas métricas utilizando la flecha hacia arriba para aumentar y la flecha hacia abajo para disminuir.

1. Arrastrar **Métricas calculadas** de los componentes del segmento en el panel izquierdo y suéltelos en el **Métricas calculadas** dentro de la sección **Definición del segmento**.

   ![Seleccione un operador y defina un valor para la métrica calculada añadida](assets/component-calculated-metrics.png)

   *Seleccione un operador y asigne un valor para la métrica calculada añadida*

   Después de agregar métricas calculadas en la definición del segmento, **[!UICONTROL Select an operator]** en el menú desplegable y asigne un valor utilizando **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Todas las métricas y métricas calculadas que suelte en la definición del segmento irán acompañadas de los operadores adecuados para asignar valores a las métricas y métricas calculadas correspondientes.

1. Revise los detalles del segmento en la **Resumen de segmentos** para decidir los cambios que desea implementar en todo el segmento.
1. Seleccionar **[!UICONTROL Last week]** o **[!UICONTROL Last month]** desde el **Período de evaluación** menú desplegable para estimar los valores de resumen de la semana o el mes pasados.
1. Seleccionar **[!UICONTROL Update estimation]** para calcular el número de cuentas cualificadas estimadas en el segmento actual basándose en el período de evaluación seleccionado.
1. Seleccionar **[!UICONTROL Save segment]**.

El segmento que ha creado ya está disponible en la lista de segmentos.

+++

+++TV en todas partes

Para crear un segmento y analizar el comportamiento compartido del suscriptor, seleccione **[!UICONTROL Create new segment]** en la parte superior derecha.

![Seleccione Crear nuevo segmento](assets/create-new-segment.png)

*Seleccione Crear nuevo segmento*

Se abre una **Nuevo segmento** , que incluye los siguientes elementos:

![Nueva página de segmento](assets/new-segment-dialog.png)

*Nueva página de segmento*

**A.** Componentes de segmento **B.** Definición del segmento **C.** Resumen de segmentos

* **Componentes de segmento**: inventario de programadores y canales, MVPD, métricas y métricas calculadas utilizadas para definir un segmento.

  >[!NOTE]
  >
  >Uso **[!UICONTROL Show all]** para expandir la lista de componentes de segmentos. Para buscar un componente rápidamente, busque su nombre en **buscar componentes de segmento** en lugar de desplazarse por toda la lista.

* **Definición del segmento**: Un lienzo en el que puede arrastrar y soltar varios componentes de segmento para crear un segmento.

* **Resumen de segmentos**: Resumen que calcula las cuentas cualificadas en función de los componentes de la definición del segmento y proporciona una breve descripción del segmento durante el periodo de evaluación.

Siga estos pasos para crear un segmento:

1. Escriba el nombre del segmento en **Nombre del segmento** que se pueden ver en la lista de segmentos y durante la selección de segmentos.
1. Escriba una descripción detallada del segmento en **Descripción del segmento**.
1. Arrastrar **Programadores y canales** de los componentes del segmento en el panel izquierdo y suéltelos en el **Programadores/Canales** dentro de la sección **Definición del segmento**.

   >[!NOTE]
   >
   >Puede crear un segmento basado en programadores o canales. Vea los canales asociados con un programador desde un menú desplegable.

   Si empieza por agregar un **Canal** en el **Programadores/Canales** , solo puede añadir canales como componentes subsiguientes.

   Si empieza por agregar un **Programador** en el **Programadores/Canales** , se muestra un cuadro de diálogo de decisión.

   ![Añadir un componente de segmento como programador o sus canales ](assets/segment-basis-selector.png){width="550" align="left"}


   *Añadir un componente de segmento como programador o su cuadro de diálogo de canales*

   Decida si desea comparar programadores específicos o un segmento basado en los canales asociados a un programador.

   Seleccionar **[!UICONTROL As a programmer]** para agregar programadores a **Programadores/Canales** sección.

   Seleccionar **[!UICONTROL As its channels]** para agregar todos los canales de un programador.

1. Arrastrar **MVPD** de los componentes del segmento en el panel izquierdo y suéltelos en el **MVPD** dentro de la sección **Definición del segmento**.

   >[!NOTE]
   >
   >Cuando inicia sesión como programador, una MVPD denominada **xfinity** aparece como una opción independiente en la **MVPD** sección. No se puede combinar con ningún otro MVPD.

1. Arrastrar **Métricas** de los componentes del segmento en el panel izquierdo y suéltelos en el **Métricas** dentro de la sección **Definición del segmento**.

   ![Seleccione un operador y defina un valor para la métrica añadida](assets/component-metrics.png)

   *Seleccione un operador y asigne un valor para la métrica añadida*

   Después de añadir métricas en la definición del segmento, elija un operador de **[!UICONTROL Select an operator]** menú desplegable y asigne un valor mediante **[!UICONTROL Select an option]**.

   Ajuste los valores de determinadas métricas utilizando la flecha hacia arriba para aumentar y la flecha hacia abajo para disminuir.

1. Arrastrar **Métricas calculadas** de los componentes del segmento en el panel izquierdo y suéltelos en el **Métricas calculadas** dentro de la sección **Definición del segmento**.

   ![Seleccione un operador y defina un valor para la métrica calculada añadida](assets/component-calculated-metrics.png)

   *Seleccione un operador y asigne un valor para la métrica calculada añadida*

   Después de agregar métricas calculadas en la definición del segmento, **[!UICONTROL Select an operator]** en el menú desplegable y asigne un valor utilizando **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Todas las métricas y métricas calculadas que suelte en la definición del segmento irán acompañadas de los operadores adecuados para asignar valores a las métricas y métricas calculadas correspondientes.

1. Revise los detalles del segmento en la **Resumen de segmentos** para decidir los cambios que desea implementar en todo el segmento.
1. Seleccionar **[!UICONTROL Last week]** o **[!UICONTROL Last month]** desde el **Período de evaluación** menú desplegable para estimar los valores de resumen de la semana o el mes pasados.
1. Seleccionar **[!UICONTROL Update estimation]** para calcular el número de cuentas cualificadas estimadas en el segmento actual basándose en el período de evaluación seleccionado.
1. Seleccionar **[!UICONTROL Save segment]**.

El segmento que ha creado ya está disponible en la lista de segmentos.
+++

## Administración de segmentos {#manage-segments}

Puede seleccionar un segmento de la lista de segmentos y, a continuación, realizar las siguientes acciones:

* [Edición de segmentos](#edit-segment)
* [Duplicación de segmentos](#duplicate-segment)
* [Eliminar un segmento](#delete-segment)

![Editar, duplicar o eliminar un segmento](assets/manage-segments-list.png)

*Seleccione un segmento para editar, duplicar o eliminar*

**A.** [Categorías de vídeo](product-concepts.md#video-category-def) **B.** [Segmento predeterminado](#work-with-segments)

>[!NOTE]
>
>Las categorías de vídeo que se muestran en esta sección, como **MVPD**, **Programadores**, y **Canales** representan las etiquetas utilizadas en la versión de TV Everywhere de Account IQ. Si ha iniciado sesión como servicio D2C, estas etiquetas muestran las categorías de vídeo específicas de su empresa.

No puede editar, duplicar ni eliminar el segmento predeterminado denominado **Todas las cuentas de sus propiedades**.

### Edición de segmentos {#edit-segment}

1. Vaya a **[!UICONTROL Segments]** pestaña debajo de **Acciones** en el panel izquierdo para ver una lista de segmentos.
1. Seleccione el segmento que desee editar.
1. Seleccionar **[!UICONTROL Edit]**.
1. Modifique los detalles del segmento, como el nombre, la descripción o los componentes del segmento en **Definición del segmento**.

   >[!TIP]
   >
   >Uso **[!UICONTROL Clear all]** para eliminar todos los componentes de segmento de cada sección en definición de segmento a la vez. También puede seleccionar el botón cruzado para eliminar elementos individuales.

   ![Borre todos los componentes del segmento en cada sección de la definición del segmento ](assets/clear-all-components.png)

   *Seleccione Borrar todo para eliminar todos los componentes del segmento a la vez*

1. Seleccione una de las opciones **[!UICONTROL Update segment]** para actualizar el segmento existente o **[!UICONTROL Save as new segment]** para crear un nuevo segmento con los cambios.

   >[!NOTE]
   >
   >No se permite actualizar segmentos que estén en funcionamiento en este momento. Guardar cambios como un nuevo segmento es la única opción para segmentos con operaciones en curso.

### Duplicación de segmentos {#duplicate-segment}

1. Vaya a **[!UICONTROL Segments]** pestaña debajo de **Acciones** en el panel izquierdo para ver una lista de segmentos.
1. Seleccione el segmento que desee duplicar.
1. Seleccionar **[!UICONTROL Duplicate]**.

Se genera una copia del segmento seleccionado y se coloca al final de la lista de segmentos. Puede editar los detalles necesarios en el segmento duplicado y, a continuación, actualizar el segmento duplicado o guardarlo como un nuevo segmento.

### Eliminar un segmento {#delete-segment}

1. Vaya a **[!UICONTROL Segments]** pestaña debajo de **Acciones** en el panel izquierdo para ver una lista de segmentos.
1. Seleccione el segmento que desee eliminar.

   Seleccione varios segmentos para eliminarlos en una sola operación. También puede seleccionar una casilla de verificación a la izquierda del **Nombre del segmento** para eliminar todos los segmentos a la vez.

   >[!NOTE]
   >
   > Solo puede eliminar más de un segmento o todos los segmentos si ninguna de las operaciones utiliza ninguno. Además, elimina el segmento predeterminado denominado **Todas las cuentas de sus propiedades** no está permitido. Permanecerá sin seleccionar cuando intente eliminar todos los segmentos a la vez.

   ![Eliminar más de un segmento](assets/delete-more-than-one-segment.png)

   *Seleccione varios segmentos para eliminar más de un segmento*

1. Seleccionar **[!UICONTROL Delete]**.
1. Confirmar en **[!UICONTROL Delete]** en el cuadro de diálogo para eliminar el segmento de forma permanente.

   >[!NOTE]
   >
   >El segmento se elimina permanentemente del sistema y no se puede deshacer esta acción.


