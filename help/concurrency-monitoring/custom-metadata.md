---
title: Metadatos personalizados
description: Metadatos personalizados
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---



# Metadatos personalizados {#cm}

>[!NOTE]
>
> Esta página está obsoleta, ya que se aplica a la versión anterior de la API, que ya no se recomienda para las nuevas integraciones

El servicio permite a los clientes utilizar campos estándar y personalizados tanto en las consultas como en la toma de decisiones. Los campos estándar siempre están disponibles para cualquier flujo (ya que son necesarios para la creación del flujo o los genera el servidor):

* aplicación
* mvpd
* accountId
* streamId
* streamStart
* iniciador


Los campos personalizados son cualquier par clave/valor que se pasa durante la inicialización del flujo, como parámetros de formulario o de cadena de consulta. Los campos estándar y personalizados se pueden utilizar en los siguientes casos (para obtener más información, consulte la documentación real de los recursos de API implicados a continuación):

* Solicitud de campos adicionales (a través del parámetro de cadena de consulta fields) al recuperar la lista de flujo para una cuenta (el recurso /streams)
* Desglose de la actividad de la cuenta especificando las dimensiones por las que agrupar (el recurso /activity)
* Definición de políticas del lado del servidor basadas en valores de campo o cardinalidades (los ejemplos utilizan pseudo-SQL solo para una mayor claridad):
* Configure una directiva para que se aplique únicamente a valores de campo específicos (por ejemplo, una directiva específica de iOS: WHERE osType IS &#39;iOS&#39;)
* Limite el número de valores distintos para un campo determinado (por ejemplo, no más de X dispositivos distintos: HAVING DISTINCT COUNT(deviceId) >= 2)
* Limite el número de flujos activos por valor de campo (por ejemplo, no más de X flujos activos para un solo tipo de dispositivo: GROUP BY deviceType HAVING COUNT(streamId) >= 3)


En función de estas claves o valores que se envían, se pueden establecer diferentes reglas. Esto podría ser algo en este sentido :

1. El cliente decide que desea enviar el grupo de parámetros, que tendrá como valores &quot;SPORTS&quot; y &quot;KIDS&quot;.
1. A continuación, la aplicación tendría que hacer esto:
   * Para los canales deportivos, en la inicialización del flujo la aplicación estaría enviando ***type=SPORTS*** como parámetro de consulta
   * Para los canales con contenido relacionado con niños, en la inicialización del flujo la aplicación estaría enviando ***type=KIDS*** como parámetro de consulta
1. Luego se puede definir una política como esta:
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. Esto básicamente significaría que cuando un usuario está viendo deportes, no puede hacerlo en más de 1 dispositivo, sin embargo, cuando el usuario ve contenido para niños, la visualización está permitida en un máximo de 3 dispositivos.

