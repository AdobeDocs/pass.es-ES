---
title: Casos de uso
description: Casos de uso en Monitorización de concurrencia.
exl-id: 6cc30bb6-e985-4d9a-9f99-a7f04ae8deb7
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Casos de uso {#use-cases}

El caso de uso principal del servicio de recuento de transmisiones es contar el número de transmisiones de vídeo simultáneas que ve un usuario y tomar una decisión sobre su uso simultáneo para el mismo ID de cuenta.

Para monitorizar el uso por parte del suscriptor, es necesario un servicio centralizado que pueda acumular la actividad del usuario independientemente de si se produce en el sitio web del programador o en la aplicación, en el portal de contenido de MVPD o en una propiedad sindicada.

Los principales casos de uso admitidos por este servicio centralizado deben ser:

1. Tan pronto como un suscriptor comience a ver un vídeo, la aplicación puede **inicializar una sesión de streaming** e iniciar **datos de actividad de informes**.
1. En el mismo servicio central, otra instancia recibirá ***decisiones CM***; en caso de que la aplicación tenga una o más directivas registradas en el servicio CM, el servicio responderá con una decisión de acceso basada en la actividad actual.

## Casos de uso comunes {#common-use-cases}

### Limitación básica de secuencias

Limite el número de flujos simultáneos por suscriptor en todas las aplicaciones.

### Restricciones basadas en dispositivos

Permitir solo un determinado número de transmisiones por tipo de dispositivo (móvil, tableta, TV, etc.).

### Reglas específicas de contenido

Aplique diferentes límites para el contenido en directo frente al contenido de VOD.

### Políticas basadas en la ubicación

Restrinja la transmisión en función de la ubicación geográfica o el tipo de red.

## Creación de una sesión {#create-session}

Esta llamada de API permite al cliente crear una nueva sesión de CM cuando el usuario presiona el botón &quot;reproducir&quot; para ver cierto contenido. La respuesta del servidor contendrá la nueva URL de flujo (que contiene el ID de flujo) para mantenerla activa y el tiempo en que se agotará el tiempo de espera de la secuencia. Se espera que la aplicación cliente informe de la actividad a través de latidos. La llamada de inicialización de sesión debe incluir metadatos en forma de pares clave/valor enviados como datos de formulario (o parámetros de cadena de consulta). Además, la respuesta también incluirá un indicador para indicar si la reproducción es &quot;compatible con la política&quot;. Si no es así, la reproducción no está permitida.

## Actividad de informes {#reporting-activity}

Una vez que se crea una sesión, la aplicación debe enviar latidos regularmente para que ese flujo permanezca activo. Además, se recomienda que la aplicación del cliente detenga el flujo una vez que el usuario detenga la reproducción, de modo que el flujo no se cuente como activo hasta que se agote el tiempo de espera.

La respuesta de la llamada de Heartbeat puede permitir a la aplicación cliente continuar la reproducción de vídeo (cuando cumple la directiva) o puede indicarle que detenga la reproducción de vídeo. En caso de que el flujo de vídeo no sea compatible, la aplicación cliente tiene que detenerlo. La respuesta proporcionará información para que la aplicación cliente muestre un mensaje de error o las acciones disponibles para que el usuario continúe con la reproducción.
