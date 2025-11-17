---
title: Tablero de ESM
description: Aprenda a utilizar el panel de ESM para monitorizar los datos de derechos y eventos de los socios de MVPD.
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# Tablero de ESM {#esm-dashboard}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El panel de ESM proporciona una vista unificada de los datos de derechos y eventos para ayudarle a monitorizar el rendimiento, identificar anomalías y comprender los patrones de acceso de los usuarios entre los socios de MVPD. En esta guía se explica cómo utilizar los filtros del panel, interpretar los informes y comprender las métricas clave a lo largo de intervalos de tiempo configurables.

![Vista del panel de ESM](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## Casos de uso {#use-cases}

- Visualización de tendencias por plataforma o MVPD
- Comparar el rendimiento de MVPD
- Comprenda el uso del cliente por aplicación

Encontrará más detalles sobre los datos y eventos de ESM en [Información general sobre la supervisión del servicio de derechos](https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview).

## Informes {#reports}

Están disponibles los siguientes informes:

### Solicitudes de reproducción {#play-requests}

Muestra el número de solicitudes de reproducción para el intervalo de tiempo seleccionado.

**Estilo de gráfico** - línea.

### Conversión de autenticación {#authentication-conversion}

Muestra la relación entre los eventos de autenticación correctos y el número total de intentos de autenticación.

**Estilo de gráfico** - barra horizontal

### Conversión de autorización {#authorization-conversion}

Muestra la relación entre los eventos de autenticación correctos y el número total de intentos de autenticación.

**Estilo de gráfico** - barra horizontal

### Latencia de autorización {#authorization-latency}

Muestra la latencia media (en milisegundos) de las respuestas de MVPD.

**Estilo de gráfico** - línea.

### Uso de MVPD {#mvpd-usage}

Muestra una comparación del número de usuarios únicos por MVPD.

**Estilo de gráfico** - área apilada.

### Autenticaciones correctas {#successful-authentications}

Muestra el número total de autenticaciones correctas para el intervalo de tiempo seleccionado.

**Estilo de gráfico** - línea.

### Autorizaciones correctas {#successful-authorizations}

Muestra el número total de autorizaciones correctas (respuestas &quot;Permitir&quot; de MVPD) para el intervalo de tiempo seleccionado.

**Estilo de gráfico** - línea.

## Acciones {#actions}

### Ver por {#view-by}

Para cada gráfico, hay una lista desplegable &quot;Ver por&quot; que le permite seleccionar exactamente qué datos mostrar.

- **Agregado**: muestra datos generales
- **MVPD / Canales / Plataformas**: describe los filtros específicos seleccionados

### Descargar {#download}

Puede descargar los datos sin procesar:

- **Descargar datos de gráfico como CSV**: descarga datos para un gráfico específico
- **Descargar todos los datos como CSV**: descarga todos los datos de ESM en todos los gráficos

## Filtros {#filters}

Utilice filtros para reducir el conjunto de datos y centrar el análisis. Los filtros disponibles son los siguientes:

- **Canal**: incluye todos los canales (marcas) disponibles
- **MVPD**: concéntrate en uno o más proveedores
- **Plataforma**: web, móvil, TV conectada o familia de dispositivos

Para añadir un nuevo filtro, seleccione el botón &quot;Añadir filtros&quot;.

En la página &quot;Filtros del conjunto de datos&quot;, puede arrastrar y soltar los filtros necesarios.

![Filtros del panel de ESM](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

Para cada sección puede eliminar filtros individualmente o borrar la selección completa.

### Sugerencias de filtro {#filter-tips}

- Combine varios filtros para aislar una cohorte (por ejemplo, un MVPD en una plataforma móvil para un canal).
- No agregue filtros para alejar y establecer una línea de base antes de profundizar.

## Intervalos de tiempo {#time-intervals}

Controle la ventana de análisis y la granularidad.

![Intervalos de tiempo del panel de ESM](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### Intervalo de fechas {#date-range}

**Ajustes preestablecidos**: hoy, semana actual, últimos 7 días, mes actual, últimos 30 días, últimos 3 meses, últimos 6 meses, últimos 12 meses

**Personalizado**: seleccione el intervalo de tiempo deseado

### Granularidad {#granularity}

Diario/mensual
