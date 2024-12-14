---
title: Depuración del SDK de AccessEnabler iOS/tvOS mediante los registros de aplicación de la consola
description: Depuración del SDK de AccessEnabler iOS/tvOS mediante los registros de aplicación de la consola
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# (Heredado) Depuración de AccessEnabler iOS/tvOS SDK mediante registros de aplicaciones de consola {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Información general

El ámbito de este documento es capturar y presentar la evolución del mecanismo de registro de iOS/tvOS SDK de AccessEnabler junto con algunos detalles útiles para depurar el marco de AccessEnabler mediante los registros de la aplicación de la consola.

## Estado del mecanismo de registro

El propósito del mecanismo de registro de AccessEnabler iOS/tvOS es emitir mensajes útiles para solucionar posibles problemas que una aplicación que utiliza el marco de AccessEnabler podría encontrar debido a él.

### AccessEnabler iOS/tvOS 3.5.0 y superior

A partir de la versión 3.5.0 de AccessEnabler iOS/tvOS, el mecanismo de registro presenta las siguientes mejoras como cambios:

* El marco de AccessEnabler usa la implementación [OSLog](https://developer.apple.com/documentation/os/oslog) recomendada por Apple.

* El marco de AccessEnabler introduce la capacidad de filtrar los registros de aplicaciones de la consola en función del subsistema: **com.adobe.pass.AccessEnabler**. Todos los mensajes emitidos por SDK forman parte de com.adobe.pass.AccessEnabler.

* El marco de AccessEnabler introduce la capacidad de filtrar los registros de la aplicación de la consola en función de Cualquiera (prefijo): **[AccessEnabler]**. Todos los mensajes emitidos por SDK tienen el prefijo [AccessEnabler].

* El marco de AccessEnabler introduce la capacidad de filtrar los registros de la aplicación de la consola en función de la categoría: **debug**, **error** junto con cualquiera de los dos criterios anteriores: Subsystem o Any (prefijo).

## Depuración mediante registros de aplicación de consola

Según los problemas que se investigan, es posible que desee incluir o excluir los mensajes de registro emitidos por el marco de AccessEnabler, por lo que puede encontrar a continuación algunos detalles útiles que pueden ayudarle durante las investigaciones y al utilizar los registros de la aplicación de la consola.


### AccessEnabler iOS/tvOS 3.5.0 y superior

#### Incluyendo {#including}

En primer lugar, para poder ver cualquiera de los mensajes de registro emitidos por el marco de AccessEnabler, **debe** seleccionar &quot;Incluir mensajes de información&quot; e &quot;Incluir mensajes de depuración&quot; en la sección Acción de la aplicación de la consola, tal como se muestra en la siguiente imagen.

![](../../../assets/include-info-debug-msg.png)


Para poder depurar la funcionalidad del SDK de AccessEnabler iOS/tvOS y **ver** los registros del módulo AccessEnabler, puede:

* Busque en la aplicación de consola usando la opción **Subsystem** que es igual al valor com.adobe.pass.AccessEnabler como se muestra en la siguiente imagen.

![](../../../assets/subsys-console-app.png)

* Busque en la aplicación de consola usando la opción **Any** que contiene la variable
  Valor de [AccessEnabler], como se muestra en la imagen siguiente.

![](../../../assets/any-optn-console-app.png)

Además de los dos criterios anteriores, también puede usar la opción **Category** junto con **Subsystem** o **Any (prefix)** para buscar explícitamente mensajes de nivel **debug** o **error** emitidos por el SDK AccessEnabler iOS/tvOS.

#### Exclusión

Para poder depurar mejor la funcionalidad de otros componentes y **excluir** los registros del marco de AccessEnabler, puede:

* Busque en la aplicación de consola usando la opción **Subsistema** que no es igual al valor com.adobe.pass.AccessEnabler.
* Busque en la aplicación de consola usando la opción **Any** que no contenga el valor [AccessEnabler].

## Informar de un problema

Cuando informe de un problema a la autenticación de Adobe Pass, tenga en cuenta las siguientes sugerencias:

* intente proporcionar los pasos de reproducción.
* intente proporcionar la versión del sistema operativo y los modelos de dispositivo en los que se produce el problema.
* intente proporcionar la versión del SDK de AccessEnabler de iOS/tvOS que experimenta el problema.
* intente capturar y adjuntar todos los mensajes de registro de iOS/tvOS SDK de AccessEnabler mediante cualquiera de las dos opciones presentadas en la sección [Incluyendo](#including).
