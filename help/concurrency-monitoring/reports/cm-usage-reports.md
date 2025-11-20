---
title: Informes de uso de supervisión de concurrencia
description: Informes de uso de supervisión de concurrencia
exl-id: 20220436-e748-4b22-8e7c-e074e0bfe242
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# Informes de uso de supervisión de concurrencia {#cm-usage-reports}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.



## Información general {#usage-rep-overview}

El servicio **Informes de uso de supervisión de concurrencia** está disponible a través de una API de REST que proporciona insight en el uso simultáneo según lo informado por las aplicaciones del cliente.

## Requisitos previos {#usage-rep-prerequisites}

Para tener acceso al producto de informes de uso de supervisión de concurrencia, un cliente debe ponerse en contacto primero con el [equipo de soporte](mailto:tve-support@adobe.com) de supervisión de concurrencia y realizará los pasos necesarios para permitirle tener acceso al producto de API. Más detalles sobre [Acceso a la API de CMU](/help/concurrency-monitoring/reports/cmu-api-access.md).

## Métricas y desgloses generales de informes {#general-rep-metrics-breakdown}

### Los usuarios de informes de uso pueden monitorizar las siguientes métricas:{#monitor-metrics}

| Nombre | Descripción |
|:---|:---|
| active-users | número de cuentas de usuario distintas que han iniciado al menos una sesión de flujo continuo durante el intervalo, desglosado por granularidad de tiempo |
| active-session | número de sesiones de flujo continuo que han informado de actividad durante el intervalo (no incluye intentos fallidos, sesiones que recibieron una respuesta de denegación para la llamada de inicialización) |
| started-session | número de sesiones de flujo continuo que se iniciaron dentro del intervalo |
| completed-session | número de sesiones de flujo continuo que finalizaron durante el intervalo (explícitamente o debido al tiempo de espera) |
| failed-tries | número de intentos de inicialización de sesión que recibieron una respuesta de denegación |
| sesiones descartadas | número de sesiones de flujo continuo que finalizaron durante la reproducción (en latidos) debido a una infracción de directiva. Esta métrica solo es significativa cuando se aprueba una política FIFO o last_one_wins. |
| sesiones inactivas | número de sesiones de flujo continuo que se cancelaron explícitamente al iniciar una nueva sesión (a través del encabezado X-Terminate) |
| duration_0-15 | número de flujos con una duración entre 0 y 15 minutos |
| duration_15-30 | número de emisiones con una duración entre 15 y 30 minutos |
| duration_30-60 | número de emisiones con una duración entre 30 y 60 minutos |
| duration_60-120 | número de emisiones con una duración entre 60 y 120 minutos |
| duration_2h-4h | número de flujos con una duración entre 2 horas (120 minutos) y 4 horas |
| duration_4h-8h | número de flujos con una duración entre 4 horas y 8 horas |
| duration_8h-16h | número de flujos con una duración entre 8 horas y 16 horas |
| duration_16h-1d | número de flujos con una duración entre 16 horas y 1 día (24 horas) |
| duration_1d-3d | número de flujos con una duración entre 1 día y 3 días |
| duration_3d-7d | número de flujos con una duración entre 3 días y 7 días |
| duration_1w-1m | número de flujos con una duración entre 1 semana (7 días) y 1 mes (30 días) |
| duration_over-1m | número de flujos con una duración superior a 1 mes (30 días) |

### Los usuarios de los informes de uso pueden filtrar las métricas enumeradas anteriormente según las siguientes dimensiones: {#dimensions-2-filter-metrics}

| Nombre de Dimension | Descripción |
|:---------------|:------------------------------------------------------------------------------------------------------------------|
| año | El año expresado con 4 dígitos |
| mes | El mes del año (1-12) |
| día | El día del mes (1-31) |
| hora | La hora del día |
| minuto | El minuto de la hora[^1] |
| aplicación | El nombre de la aplicación registrada en la Monitorización de concurrencia que se utiliza para administrar sesiones |
| application-id | El ID de aplicación registrado en la Monitorización de concurrencia utilizado para administrar sesiones |
| canal | Los metadatos de canal enviados durante la inicialización de la sesión (marcados como Desconocido si no se envían metadatos). |
| mvpd | El MVPD proporcionado en la administración de sesiones |
| plataforma | Los metadatos de plataforma proporcionados al inicializar la sesión o predefinidos para una aplicación en el nivel de configuración |

## Métricas y desgloses de informes de simultaneidad {#concurrency-reports-metrics-breakdown}

A partir de la versión 2.9.0 de supervisión de concurrencia, presentamos un nuevo informe para comprender el uso simultáneo: un histograma para **nivel de concurrencia** y **nivel de actividad**.

El objetivo principal de este informe es ayudarle a comprender el impacto de establecer una directiva con un determinado límite de concurrencia y proporcionarle suficiente información para decidir si debe aumentar el límite.

### Los usuarios de informes de uso pueden monitorizar las siguientes métricas: {#metrics-usage-rep-users}

| Nombre de Dimension | Descripción |
|:---|:---|
| usuarios | número de usuarios que han alcanzado cada nivel de concurrencia/actividad |

### Los usuarios de los informes de uso pueden filtrar las métricas enumeradas anteriormente según las siguientes dimensiones: {#dimensions-to-filter-metrics}

| Nombre de Dimension | Descripción |
|:---|:---|
| año | El año expresado con 4 dígitos |
| mes | El mes del año (1-12) |
| día | El día del mes (1-31) |
| nivel de concurrencia | Representa cualquier actividad de flujo **distinta que se aprobó en la fase de inicialización de la sesión** para que un usuario pueda observar cuántos flujos simultáneos **abrió** un usuario y comprender el impacto de aplicar un determinado límite de concurrencia |
| nivel de actividad | Representa cualquier actividad de flujo **distinta (independientemente de su estado: iniciada, activa, detenida, rechazada)** para un usuario con el fin de poder observar cuántas secuencias simultáneas intentó abrir un usuario y comprender el impacto de aplicar un determinado límite de concurrencia |
| mvpd | El MVPD proporcionado en la administración de sesiones |

### Ejemplos de informes

Para obtener la mejor precisión de los datos, recomendamos los informes presentados en esta página [Ejemplos de informes de CMU](/help/concurrency-monitoring/reports/cm-usage-reports-examples.md)

[^1]: Los informes por minuto no están disponibles de forma predeterminada. Póngase en contacto con el [Equipo de soporte](mailto:tve-support@adobe.com) de supervisión de concurrencia para solicitarlas.