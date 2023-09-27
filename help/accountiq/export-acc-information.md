---
title: Exportar información para cuentas con una puntuación de uso compartido alta
description: Exportar información para cuentas con una puntuación de uso compartido alta.
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# Exportar información para cuentas con una puntuación de uso compartido alta {#export-account-info-high-score}

[!UICONTROL Account IQ] le da la opción de exportar los detalles de uso compartido de cuentas para las 1000 cuentas de suscriptores principales en función de su [compartir probabilidades](/help/accountiq/product-concepts.md#account-sharing-probability-def). Los datos del archivo CSV exportado se ordenan en orden decreciente de las probabilidades de uso compartido de las cuentas de suscriptor, de las MVPD seleccionadas en la [segmento](/help/accountiq/product-concepts.md#segment-def), para a [lapso de tiempo especificado](/help/accountiq/product-concepts.md#time-frame-def).

La opción para exportar la información de uso compartido de cuentas está disponible en [Informes de uso generales](/help/accountiq/general-usage-reports.md) y [Informes de cuentas compartidas](/help/accountiq/shared-acc-reports.md) páginas.

>[!NOTE]
>
>Los números del archivo CSV descargado son diferentes para las páginas de informes Uso general y Cuentas compartidas. Esto se debe a que la página Informes de uso generales tiene filtros adicionales para que los programadores seleccionen Umbral para el número de dispositivos, IP y códigos postales. Por lo tanto, los datos exportados desde Informes de uso general se basan en el filtro de umbral adicional aplicado.

![Opción Exportar en Uso general](assets/export.png)

Para exportar la información de uso compartido de cuentas de los suscriptores:

1. Defina el segmento que desee siguiendo los pasos de [Cómo definir el segmento y seleccionar el periodo de tiempo](/help/accountiq/howto-select-segment-timeframe.md) para evaluación desde [segmento y periodo de tiempo](/help/accountiq/segments-timeframe.md) panel.

1. Seleccione el **[!UICONTROL Export top 1000 accounts]** opción para exportar la información de la cuenta para 1000 suscriptores con la mayor probabilidad de uso compartido.

Al utilizar la opción de exportación, las estadísticas de 1000 cuentas con las mayores probabilidades de compartir (para un lapso de tiempo definido) se descargan en la carpeta Descargas del equipo local.

>[!NOTE]
>
>El archivo CSV descargado se puede abrir con cualquier aplicación que lea un archivo CSV, por ejemplo, Microsoft Excel.

![datos exportados en formato csv](assets/exported-csv.png)

*Imagen: datos de cuenta compartida exportados en formato CSV*

## Columnas del informe exportado {#columns-in-export}

**Semana/ Mes**

La semana o el mes que seleccionó en la variable **[!UICONTROL Granularity and Time Frame]** en el selector de segmentos, para el cual se buscan las estadísticas de uso compartido.

**MVPD**

Si es un usuario programador, la columna muestra a qué MVPD pertenece la cuenta del suscriptor.

**ID de suscriptor**

Cuenta específica de la que estamos hablando en una fila.

**Cantidad mínima de dispositivos**

El número real de dispositivos (que transmiten contenido) es casi con certeza mayor que el número mínimo de dispositivos, especificado para una cuenta en particular.

>[!NOTE]
>
>El número real de dispositivos (que transmiten contenido) es ciertamente mayor que el número mínimo de dispositivos, especificado para una cuenta en particular.

**Cantidad mínima de personas**

La cantidad mínima absoluta de personas que estaban activas en el contenido de streaming mediante esos dispositivos.

>[!NOTE]
>
>El número real de personas (que transmiten contenido) es casi con certeza mucho mayor que el número mínimo de personas, especificado para una cuenta en particular.

**[!UICONTROL # IPs]**

Número de direcciones IP desde las que se transmite el contenido.

**[!UICONTROL # Locations]**

Número de ubicaciones (según el código postal) desde las que se transmite el contenido.

**[!UICONTROL # Cities]**

Número de ciudades en las que se ha producido la transmisión.

**[!UICONTROL # States]**

Número de estados en los que se ha producido la transmisión.

**[!UICONTROL # Clusters]**

El número de diferentes [clústeres](/help/accountiq/product-concepts.md#cluster-def) donde se ha producido la transmisión.

**[!UICONTROL Geographic span (miles)]**

La distancia máxima entre las ubicaciones de streaming asociadas con la cuenta.

**[!UICONTROL # AuthN OK]**

El número de veces que los usuarios han iniciado sesión durante el período, utilizando esa cuenta.

**[!UICONTROL # AuthZ OK]**

Número de veces que un MVPD ha autorizado un flujo o ha concedido acceso (al contenido) a esa cuenta.

>[!NOTE]
>
>El **[!UICONTROL # AuthZ OK]** está relacionado con el **[!UICONTROL # Play Requests]**; es más pequeña que la **[!UICONTROL # Play Requests]** porque el Adobe almacena en caché las autorizaciones que vienen para MVPD por lo general durante 24 horas.

**[!UICONTROL # Play Requests]**

El número real de flujos durante el período de tiempo.

**[!UICONTROL # Channels]**

Número total de canales diferentes que la cuenta ha visto durante el período de tiempo.

>[!NOTE]
>
>**[!UICONTROL # Channels]** incluye los canales que no pertenecían necesariamente al programador que ha iniciado sesión.
>
>Este número de la cuenta se mostró porque la cuenta vio el canal, pero también accedió a otros canales durante ese período de tiempo.

**Patrón de uso**

Los números de esta columna son identificadores que se asignan a uno de los 14 patrones con los que identificamos todas las cuentas de usuario.

*Tabla: Identificadores de patrones de uso en la asignación CSV exportada con patrones de uso*

| ID | 1 | 2 | 3 | 4 | 5 y 8 | 6 | 7 | 9 | 10 y 11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Patrones de uso | Usuario normal | Viajero o viajero | Familia numerosa | Familias y amigos cercanos | Uso compartido de grupos sociales | Gran grupo de amigos | Flujo simultáneo | Uso compartido de comunidades | Comportamiento incierto | Familia pequeña | Segunda casa | Uso anormal |

{style="table-layout:auto"}

**Probabilidad de uso compartido**

La probabilidad de compartir es la probabilidad de que la cuenta específica comparta sus credenciales.

>[!NOTE]
>
> El promedio de la probabilidad de compartir de todas las cuentas (en el segmento seleccionado) se utiliza para calcular la [nivel de uso compartido](/help/accountiq/dashboard.md#sharing-level) de la [Puntuación de uso compartido agregado](/help/accountiq/dashboard.md#aggregated-sharing).
