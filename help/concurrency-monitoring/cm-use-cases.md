---
title: Casos de uso
description: Casos de uso en Monitorización de concurrencia.
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 0%

---


# Casos de uso {#use-cases}

El caso de uso principal del servicio de recuento de transmisiones es contar el número de transmisiones de vídeo simultáneas que ve un usuario y tomar una decisión sobre su uso simultáneo para el mismo ID de cuenta.

Para monitorizar el uso por parte del suscriptor, es necesario un servicio centralizado que pueda acumular la actividad del usuario independientemente de si ocurre en el sitio web o la aplicación del programador, en el portal de contenido de MVPD o en una propiedad sindicada.

Los principales casos de uso admitidos por este servicio centralizado deben ser:

1. En cuanto un suscriptor comienza a ver un vídeo, la aplicación puede **Inicialización de una sesión de flujo continuo** y comenzar **actividad de informe** datos.
1. En el mismo servicio central, otra instancia recibirá ***Decisiones de CM*** - en caso de que la aplicación tenga una o más políticas registradas en el servicio CM, el servicio responderá con una decisión de acceso basada en la actividad actual.


## Creación de una sesión {#create-session}

Esta llamada de API permite al cliente crear una nueva sesión de CM cuando el usuario presiona el botón &quot;reproducir&quot; para ver cierto contenido. La respuesta del servidor contendrá la nueva URL de flujo (que contiene el ID de flujo) para mantenerla activa y el tiempo en que se agotará el tiempo de espera de la secuencia. Se espera que la aplicación cliente informe de la actividad a través de latidos. La llamada de inicialización de sesión debe incluir metadatos en forma de pares clave/valor enviados como datos de formulario (o parámetros de cadena de consulta). Además, la respuesta también incluirá un indicador para indicar si la reproducción es &quot;compatible con la política&quot;. Si no es así, la reproducción no está permitida.

## Actividad de informes {#reporting-activity}

Una vez que se crea una sesión, la aplicación debe enviar latidos regularmente para que ese flujo permanezca activo. Además, se recomienda que la aplicación del cliente detenga el flujo una vez que el usuario detenga la reproducción, de modo que el flujo no se cuente como activo hasta que se agote el tiempo de espera.

La respuesta de la llamada de Heartbeat puede permitir a la aplicación cliente continuar la reproducción de vídeo (cuando cumple la directiva) o puede indicarle que detenga la reproducción de vídeo. En caso de que el flujo de vídeo no sea compatible, la aplicación cliente tiene que detenerlo. La respuesta proporcionará información para que la aplicación cliente muestre un mensaje de error o las acciones disponibles para que el usuario continúe con la reproducción.
