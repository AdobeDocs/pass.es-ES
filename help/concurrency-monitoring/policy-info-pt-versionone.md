---
title: Punto de información de política
description: Punto de información de política
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 0%

---



# Punto de información de política {#pip}

>[!NOTE]
>
>Esta página está obsoleta, ya que se aplica a la versión anterior de la API, que ya no se recomienda para las nuevas integraciones

El diagrama siguiente muestra el flujo en caso de que el cliente elija el **Punto de información de política**, en cuyo caso, CM se utiliza únicamente para consultar la actividad y toda la lógica de acceso está incrustada en la aplicación cliente):

![](assets/pip-workflow.png)



El diagrama siguiente ilustra cómo funciona el recuento de secuencias para un usuario que ve contenido desde dos dispositivos.

![](assets/pip-sequence.png)

En pocas palabras, el flujo de mensajes habitual es el siguiente:

1. Inicialmente, para un usuario que nunca ha utilizado el servicio, se carga una página web / se abre una aplicación, donde una aplicación instrumentada del Servicio de supervisión de concurrencia realiza una llamada de inicialización de sesión.
1. El Servicio de monitorización de concurrencia devuelve el nuevo recurso de flujo para los latidos, así como la actividad del usuario actual.
1. Durante la reproducción de vídeo, la aplicación instrumentada realiza llamadas de Heartbeat al servicio de monitorización de concurrencia, lo que muestra que el usuario está consumiendo un vídeo en este momento.
1. En cualquier otro momento, otras aplicaciones instrumentadas pueden realizar llamadas de consulta de estado al servicio de supervisión de simultaneidad, que devolverá la actividad del usuario actual.
1. Al final de la reproducción de vídeo, la aplicación instrumentada puede realizar una llamada de Heartbeat con &quot;event=stop&quot;, lo que significa que el vídeo se ha detenido y que el flujo actual ya no debe contarse como un flujo activo.

