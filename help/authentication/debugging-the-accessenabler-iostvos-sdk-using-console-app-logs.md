---
title: Depuración del SDK de AccessEnabler para iOS/tvOS mediante los registros de aplicación de la consola
description: Depuración del SDK de AccessEnabler para iOS/tvOS mediante los registros de aplicación de la consola
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 0%

---

# Depuración del SDK de AccessEnabler para iOS/tvOS mediante los registros de aplicación de la consola {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


## Información general

El ámbito de este documento es capturar y presentar la evolución del mecanismo de registro del SDK de iOS/tvOS de AccessEnabler junto con algunos detalles útiles para depurar el marco de AccessEnabler mediante los registros de la aplicación de la consola.

## Estado del mecanismo de registro

El propósito del mecanismo de registro de AccessEnabler iOS/tvOS es emitir mensajes útiles para solucionar posibles problemas que una aplicación que utiliza el marco de AccessEnabler podría encontrar debido a él.

### AccessEnabler iOS/tvOS 3.5.0 y superior

A partir de la versión 3.5.0 de AccessEnabler iOS/tvOS, el mecanismo de registro presenta las siguientes mejoras como cambios:

* Apple El marco de AccessEnabler utiliza los [OSLog](https://developer.apple.com/documentation/os/oslog) implementación.

* El marco de AccessEnabler introduce la capacidad de filtrar los registros de aplicaciones de la consola en función del subsistema: **com.adobe.pass.AccessEnabler**. Todos los mensajes emitidos por el SDK forman parte de com.adobe.pass.AccessEnabler.

* El marco de AccessEnabler introduce la capacidad de filtrar los registros de aplicación de la consola en función de Cualquiera (prefijo): **[AccessEnabler]**. Todos los mensajes emitidos por el SDK llevan el prefijo [AccessEnabler].

* El marco de AccessEnabler introduce la capacidad de filtrar los registros de aplicaciones de la consola en función de la categoría: **depurar**, **error** junto con cualquiera de los dos criterios anteriores: Subsystem o Any (prefijo).

## Depuración mediante registros de aplicación de consola

Según los problemas que se investigan, es posible que desee incluir o excluir los mensajes de registro emitidos por el marco de AccessEnabler, por lo que puede encontrar a continuación algunos detalles útiles que pueden ayudarle durante las investigaciones y al utilizar los registros de la aplicación de la consola.


### AccessEnabler iOS/tvOS 3.5.0 y superior

#### Incluyendo {#including}

En primer lugar, para poder ver cualquiera de los mensajes de registro emitidos por el marco de AccessEnabler, **debe** seleccione &quot;Incluir mensajes de información&quot; e &quot;Incluir mensajes de depuración&quot; en la sección Acción de la aplicación de la consola, como se muestra en la siguiente imagen.

![](assets/include-info-debug-msg.png)


Para poder depurar la funcionalidad del SDK de AccessEnabler para iOS/tvOS y **consulte** Los registros del marco de AccessEnabler le permiten:

* Buscar en la aplicación de consola mediante **Subsistema** Opción igual al valor com.adobe.pass.AccessEnabler, como se muestra en la imagen siguiente.

![](assets/subsys-console-app.png)

* Buscar en la aplicación de consola mediante **Cualquiera** que contiene la opción
  [AccessEnabler] como en la imagen siguiente.

![](assets/any-optn-console-app.png)

Junto con los dos criterios anteriores, también puede utilizar el **Categoría** opción junto con **Subsistema** o **Cualquiera (prefijo)** para buscar explícitamente **depurar** o **error** mensajes de nivel emitidos por el SDK de iOS/tvOS de AccessEnabler.

#### Exclusión

Para poder depurar mejor la funcionalidad de otros componentes y **excluir** Los registros del marco de AccessEnabler le permiten:

* Buscar en la aplicación de consola mediante **Subsistema** que no es igual al valor com.adobe.pass.AccessEnabler.
* Buscar en la aplicación de consola mediante **Cualquiera** que no contiene la opción [AccessEnabler] valor.

## Informar de un problema

Cuando informe de un problema a la autenticación de Adobe Pass, tenga en cuenta las siguientes sugerencias:

* intente proporcionar los pasos de reproducción.
* intente proporcionar la versión del sistema operativo y los modelos de dispositivo en los que se produce el problema.
* intente proporcionar la versión del SDK de AccessEnabler de iOS/tvOS que experimenta el problema.
* intente capturar y adjuntar todos los mensajes de registro del SDK de AccessEnabler para iOS/tvOS mediante cualquiera de las dos opciones presentadas en la [Incluyendo](#including) sección.
