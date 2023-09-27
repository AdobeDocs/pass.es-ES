---
title: Notas de la versión de Adobe Pass Concurrency Monitoring 2.6.0
description: Notas de la versión de Adobe Pass Concurrency Monitoring 2.6.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Notas de la versión de Adobe Pass Concurrency Monitoring 2.6.0 {#cm-260}


En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:



## Fecha de versión: 10/11/2016



## Nuevas funciones

Esta versión agrega la capacidad de terminar los flujos existentes para permitir que se inicie el flujo actual (también conocido como &quot;flujo mortal&quot;).



**Terminación remota**

* En una respuesta de conflicto 409, cada sesión enumerada dentro del campo &quot;conflictos&quot; del consejo llevará un atributo terminateCode.
* Se puede pedir al usuario la lista de sesiones en conflicto y se le puede permitir elegir las que desea eliminar
* Las sesiones remotas solo se pueden eliminar pasando un encabezado de solicitud X-Terminate (con los códigos de terminación seleccionados como valores) dentro de un intento de inicio de sesión.
* Se definió un nuevo tipo de &quot;consejo&quot; para la respuesta 410 Gone para indicar la sesión que ha matado a la actual.


Consulte la documentación actualizada para obtener más detalles.



>[!NOTE]
>
>La definición de sesión utilizada para enumerar sesiones activas se actualizó para incluir el nombre de la aplicación y el nombre del dispositivo (en lugar del ID de la aplicación).




## Corrección de errores {#bug-fixes}

Se han eliminado los encabezados duplicados en la respuesta del servidor (la corrección incluye ambos encabezados CORS y el de fecha).




## Problemas conocidos {#known-issues}

N/D
