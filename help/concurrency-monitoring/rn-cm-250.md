---
title: Notas de la versión de Adobe Pass Concurrency Monitoring 2.5.0
description: Notas de la versión de Adobe Pass Concurrency Monitoring 2.5.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---


# Notas de la versión de Adobe Pass Concurrency Monitoring 2.5.0 {#cm-250}

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

Fecha de versión: 21/04/2016

## Nuevas funciones {#new-features}

La API de monitorización de concurrencia v2.0 ya está disponible en producción.

La versión 2 unifica las llamadas de consulta y de latido y simplifica en gran medida la implementación cuando ambas API se utilizan simultáneamente.



### Ciclo de sesión {#session-lifecycle}

* API de solo escritura para la creación, la conexión persistente y la finalización de una sesión; es decir, no más consultas, la decisión se devuelve tanto en las llamadas de inicio de sesión como en las llamadas de Heartbeat
* El servidor gestiona ahora el intervalo de latido mediante los encabezados Expires establecidos en las llamadas init y keep-alive de la sesión. Esto permite la configuración del lado del servidor de los intervalos de latido y un valor codificado menos en las aplicaciones.
* La ruta feliz (comportamiento compatible tanto del usuario como de la aplicación) está &quot;pavimentada&quot; con códigos de estado de 2XX

### Control de errores {#error-handling}

* Las decisiones de denegación, la falta de metadatos o el mal comportamiento de la aplicación se marcan como respuestas 4XX (conflicto, solicitud incorrecta, no autorizada, desaparecida, etc.)

* Siempre que tenga sentido, la respuesta incluye:

   * asesoramiento asociado: explicación o explicaciones detalladas del error que se solicitarán al usuario.

   * obligaciones: acciones obligatorias que debe realizar la aplicación (por ejemplo, actualizar metadatos, cerrar sesión desde Adobe Pass).

### Metadatos {#metadata}

* Estándar en lugar de metadatos personalizados: esto permite a la API identificar si se están enviando metadatos incorrectos o si faltan los metadatos requeridos por las reglas.
* Los atributos requeridos para cada aplicación ahora los proporciona una llamada de API y los clientes deben almacenarlos en caché.
Notas

>[!NOTE]
>
>La versión V1 sigue disponible. Si los clientes necesitan utilizar consultas fuera de Heartbeat, se puede utilizar la versión V1.




### Corrección de errores {#bug-fixes}

N/D

### Problemas conocidos {#known-issues}

N/D
