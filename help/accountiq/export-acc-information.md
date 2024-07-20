---
title: Exportar información para cuentas con una puntuación de uso compartido alta
description: Exportar información para cuentas con una puntuación de uso compartido alta.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# Exportar información para cuentas con una puntuación de uso compartido alta {#export-account-info-high-score}

[!UICONTROL Account IQ] le permite exportar los detalles de uso compartido de cuentas de los 1000 suscriptores principales en función de sus [probabilidades de uso compartido](/help/accountiq/product-concepts.md#account-sharing-probability-def). Puede exportar la información de uso compartido de cuentas para el [segmento](/help/accountiq/product-concepts.md#segment-def) actual y el [intervalo de tiempo especificado](/help/accountiq/product-concepts.md#time-interval-def) en la página [Informes de cuentas compartidas](/help/accountiq/shared-acc-reports.md).

Siga los pasos para exportar la información de uso compartido de cuentas de las cuentas de suscriptor de un segmento específico.

1. Inicie sesión con sus credenciales.
1. Vaya a la pestaña **Cuentas compartidas** en la sección **Informes**.
1. Seleccione el segmento y el intervalo de tiempo necesarios en el panel Segmento e intervalo de tiempo. Aprenda [cómo seleccionar un segmento y un intervalo de tiempo](segments-timeinterval.md).

   Si es necesario, consulte las instrucciones para [crear un segmento](work-with-segments.md#create-new-segment) o [editar un segmento](work-with-segments.md#edit-segment).

1. Seleccione **[!UICONTROL Export top 1000 accounts]** ubicado en la esquina superior derecha del panel segmento e intervalo de tiempo.

   ![Exportar las 1000 cuentas principales](assets/export-top-1000-accounts.png)

   *Seleccionar opción Exportar las 1000 cuentas principales*

El archivo se descargará automáticamente en el equipo local como un .csv.

Este archivo contiene los datos de las 1000 cuentas principales en función de las probabilidades de uso compartido de las cuentas de suscriptor del segmento actual en orden decreciente.

A continuación se muestra un ejemplo del archivo .csv exportado.

![datos exportados en archivo .csv](assets/exported-csv.png)

*Datos exportados en archivo .csv*

## Columnas del informe exportado {#columns-in-export}

**Semana/Mes**

La semana o el mes seleccionados dentro de la opción **[!UICONTROL Granularity and Time Interval]** en el selector de segmentos.

**MVPD**

Si es programador, la columna muestra el distribuidor con el que está suscrita la cuenta.

>[!NOTE]
>
> La columna **MVPD** solo está disponible para las versiones de TV en todas partes.

**ID de suscriptor**

El identificador único de la cuenta específica.

**Número mínimo de dispositivos**

Número mínimo de dispositivos desde los que los usuarios transmiten contenido de forma activa.

>[!NOTE]
>
>El número real de dispositivos que transmiten contenido es mayor que el número mínimo de dispositivos especificado para una cuenta en particular.

**Número mínimo de personas**

El número mínimo de personas que retransmitieron contenido de forma activa mediante esos dispositivos.

>[!NOTE]
>
>El número real de personas que transmiten contenido es mayor que el número mínimo de personas asignadas a una cuenta determinada.

**[!UICONTROL # IPs]**

El número de direcciones IP desde las que se transmite el contenido.

**[!UICONTROL # Locations]**

El número de ubicaciones (según el código postal) desde las que se transmite el contenido.

**[!UICONTROL # Cities]**

El número de ciudades en las que se ha producido la actividad de streaming.

**[!UICONTROL # States]**

El número de estados en los que se ha producido la actividad de flujo continuo.

**[!UICONTROL # Clusters]**

Número de [clústeres](/help/accountiq/product-concepts.md#cluster-def) distintos en los que se ha producido la transmisión.

**[!UICONTROL Geographic span (miles)]**

La distancia máxima entre las ubicaciones de streaming asociadas con la cuenta.

**[!UICONTROL # AuthN OK]**

El número de inicios de sesión que los usuarios realizan durante el período especificado con esa cuenta.

>[!NOTE]
>
> Es posible que algunos servicios D2C no vean **[!UICONTROL # AuthN OK]** datos, ya que es posible que no se incluyan en los datos de su compañía.

**[!UICONTROL # AuthZ OK]**

El número de veces que un MVPD ha autorizado un flujo o ha concedido acceso al contenido de esa cuenta.

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]** no está disponible para los servicios D2C.

>[!NOTE]
>
>Para TV en todas partes, **[!UICONTROL # AuthZ OK]** se correlaciona con la cantidad de **[# solicitudes de reproducción](/help/accountiq/product-concepts.md##play-requests-def)**. Siempre será menor que **[!UICONTROL # Play Requests]** porque el Adobe generalmente almacena en caché las autorizaciones de las MVPD durante aproximadamente 24 horas.


**[!UICONTROL # Play Requests]**

El número real de flujos se produjeron durante un período de tiempo especificado.

>[!NOTE]
>
>La columna [# Solicitudes de reproducción](/help/accountiq/product-concepts.md##play-requests-def) no está disponible en la versión de MVPD de TV Everywhere.

**[!UICONTROL # Channels]**

El número total de canales que la cuenta ha visto durante un periodo especificado.

>[!NOTE]
>
> Para los servicios D2C **[!UICONTROL # Channels]** equivale al número de **[!UICONTROL # Video categories]**.

>[!NOTE]
>
>Para TV Everywhere, incluyen los canales que pueden no pertenecer al programador que ha iniciado sesión. Este número de la cuenta incluye el canal y otros canales a los que se accedió durante el periodo especificado.


**Patrón de uso**

Los valores dentro de estas columnas sirven como identificadores correspondientes a uno de los 14 patrones que utilizamos para categorizar todas las cuentas de usuario.

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">Patrones de uso</th>
      </tr>
      <tr>
        <td>1</td>
        <td>Usuario normal</td>
      </tr>
      <tr>
        <td>2</td>
        <td>Viajero o viajero</td>
      </tr>
      <tr>
        <td>3</td>
        <td>Familia numerosa</td>
      </tr>
      <tr>
        <td>4</td>
        <td>Familias y amigos cercanos</td>
      </tr>
      </tr>
         <td>5 y 8</td>
         <td>Uso compartido de grupos sociales</td>
      </tr>
      </tr>
         <td>6</td>
         <td>Gran grupo de amigos</td>
      </tr>
      </tr>
         <td>7</td>
         <td>Flujo simultáneo</td>
      </tr>
      </tr>
         <td>9</td>
         <td>Uso compartido de comunidades</td>
      </tr>
      </tr>
         <td>10 y 11</td>
         <td>Comportamiento incierto</td>
      </tr>
      </tr>
         <td>12</td>
         <td>Familia pequeña</td>
      </tr>
      </tr>
         <td>13</td>
         <td>Segunda casa </td>
      </tr>
      </tr>
         <td>14</td>
         <td>Uso anormal</td>
      </tr>
    </tbody>
  </table>

*Identificadores de patrones de uso en la asignación .csv exportada con patrones de uso*

**Probabilidad de uso compartido**

La probabilidad de que una cuenta específica comparta sus credenciales.

>[!NOTE]
>
> El promedio de la probabilidad de compartir de todas las cuentas del segmento seleccionado se usa para calcular el [nivel de uso compartido](/help/accountiq/data-panels.md#sharing-level) de la [puntuación de uso compartido promedio](/help/accountiq/data-panels.md#aggregated-sharing).
