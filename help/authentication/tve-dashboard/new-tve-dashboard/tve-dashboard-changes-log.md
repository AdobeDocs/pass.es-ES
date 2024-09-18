---
title: Registro de cambios
description: Saber cómo un administrador puede monitorizar los cambios de configuración en el Tablero de TVE.
exl-id: 9b53a61b-679f-491e-90f3-5d827e21b32c
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Registro de cambios {#changes-log}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La sección **Registro de cambios** del panel de TVE le permite ver los cambios de configuración insertados en el entorno de autenticación de Adobe Pass a través del panel de TVE. También puede comparar dos cambios de configuración diferentes.

La pestaña **Registro de cambios** del panel izquierdo muestra una lista de todos los cambios de configuración realizados a través de una cuenta específica del Tablero de TVE. Esta lista de cambios contiene los siguientes detalles:

* **Descripción del cambio**: Una breve descripción del ámbito del cambio de configuración.
* **Insertado por**: ID de correo electrónico del usuario responsable de realizar el cambio.
* **Fecha push**: La fecha del cambio de configuración.
* **Estado de inserción**: indica si la operación de inserción se realizó correctamente, está pendiente o no.

## Comparar cambios {#compare-changes}

Para comparar los cambios, siga estos pasos:

1. Seleccione dos cambios de configuración de la lista que desee comparar.

   ![Comparar cambios de configuración](../../assets/tve-dashboard/new-tve-dashboard/review/review-changes-compare-button.png)

   *Comparar cambios de configuración*

1. Seleccione **Comparar** en la esquina superior derecha de la pantalla.

   La sección **Cambios de configuración** muestra el tipo de entidad, el ID de entidad, la propiedad y el estado de la operación de cambio para cada cambio.

1. Pase el ratón sobre el cambio de configuración que desee ver.

1. Seleccione **Ver** para acceder a los valores cambiados.

   ![Ver cambios de configuración](../../assets/tve-dashboard/new-tve-dashboard/review/review-changes-view-button.png)

   *Ver cambios de configuración*

A continuación se muestra un ejemplo de un cambio realizado en la configuración seleccionada. Puede ver la diferencia entre los valores antiguos y los nuevos dentro del cambio.

![Valor antiguo y nuevo](../../assets/tve-dashboard/new-tve-dashboard/review/review-change-modal-view.png)

*Valor antiguo y nuevo*
