---
title: Operaciones en Account IQ
description: Las operaciones en Account IQ implican realizar acciones para realizar automatizaciones y operaciones masivas en las cuentas de los suscriptores y rastrear sus efectos.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 0%

---

# Operaciones {#operations-tab-next-steps}

Una vez que haya comprendido los patrones de uso de sus suscriptores y haya identificado el uso compartido de contraseñas para el segmento seleccionado (mediante [!DNL Analytics] en Account IQ), puede realizar acciones dirigidas hacia un objetivo para mitigar el uso compartido de contraseñas.

La funcionalidad Operaciones de Account IQ le ayuda a abordar y administrar de forma eficaz el uso compartido de credenciales mediante procedimientos centrados denominados operaciones. Le ofrece opciones para diseñar un objetivo, adaptar las acciones segmentadas (según el objetivo) para un grupo específico de cuentas de suscriptor y automatizar su ejecución durante un tiempo futuro. Mediante la funcionalidad Operaciones, no solo puede crear y ejecutar operaciones, sino también medir su impacto. Por lo tanto, al medir los impactos, puede ajustar su estrategia para optimizar el efecto, ya sea convirtiendo a los prestatarios o mitigando el uso compartido de credenciales.

Para ver **Operaciones** selección de página **Operaciones** opción en **Acciones** en la navegación izquierda de la aplicación Account IQ. La página Operaciones enumera todas las operaciones existentes en el sistema de Account IQ junto con sus detalles.

![](assets/operations-page.png)

*Figura: lista y detalles de operaciones existentes en Account IQ*

En la página Operaciones, puede:

* Ver una lista de operaciones ya existentes en Account IQ

* Ver detalles de la operación, como:

   * Estado (programado, en ejecución, finalizado, error o detenido)

   * progreso (en porcentaje completado)

   * audiencia de destino (segmento en el que ejecutar la operación)

   * programación (fecha de inicio y finalización de la operación)

   * fecha de creación y finalización de la operación

* [Crear nueva operación](/help/accountiq/operation-affecting-user-segment.md)

* [Ver informes de operaciones](#operation-reports)

<!--* Search from the list of operations using Search field

* Stop an operation.

* Create a duplicate operation.

* [Configure columns of Operations details page](#configure-columns)-->

## Ver informes de operaciones {#operation-reports}

Puede analizar el impacto de una operación consultando su informe. Para ver el informe de una operación:

1. Seleccione el nombre de la operación en la página Operaciones principal.

   El informe se muestra en forma de gráfico de columnas apiladas.

   ![](assets/operation-impact-report.png)

   *Imagen: informe de operaciones para ver los impactos de las operaciones*

   El eje X representa el periodo de evaluación y el eje Y representa el impacto de la operación (en términos de número de cuentas en un segmento durante el periodo de evaluación). Cada barra está dividida en tres partes.

   * Una parte representa el número de cuentas que aún cumplen los criterios del segmento de operación.

   * Otra parte representa el número de cuentas activas para ese período que originalmente estaban en el segmento, pero que ya no cumplen los criterios del segmento de operación.

   * La tercera parte representa las cuentas que no estaban activas en ese período.

   >[!NOTE]
   >
   >La primera barra representa el número de cuentas que cumplen las condiciones del segmento de operación al comienzo del período de evaluación.

   Con el tiempo, el gráfico muestra el efecto de la acción (a través de la operación ) indicando el número de cuentas que han cambiado su comportamiento en relación con los criterios originales (por ejemplo, si tienen una probabilidad de uso compartido superior a 90 y usan más de 5 dispositivos) o que han quedado inactivas.

<!--For example, in the above image the variable on the y-axis is number of accounts. Looking at the graph you can compare the number of accounts that are in the operations' segment versus the number of accounts that are outside the operations segment at a particular time (such as week 2nd of the operations evaluation period). Therefore, you can analyze how over the evaluation period do number of accounts vary within the operation segment and outside the segment.

So, if your operation was to send out warning emails to suspecting accounts, and accounts in operations segment were those with sharing probability more than 90 and using more than 5 devices to stream content, then in the beginning of the evaluation period accounts in segment are more than 17 thousand. This number changes over the evaluation period as shown in the graph, thereby indicating the impact of operation. Based on the evaluation, you can take remedial measures on suspecting accounts, or continue with the operation, or adjust your strategy for better outcomes to curb credential sharing.-->

1. Para cerrar el informe y volver a la página principal de Operaciones, seleccione **Operaciones** opción en **Acciones** en la navegación izquierda.

<!--

![](assets/operations-details.png)

*Figure: Operation details*
## Configure columns {#configure-columns}

You can select the icon to **Configure columns** on the top of the operations table.

![](assets/config-columns.png)

*Figure: Configure columns of Operations details page*-->