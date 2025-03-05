---
title: 'Encabezado: AP-Device-Identifier'
description: 'API de REST V2: encabezado: AP-Device-Identifier'
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# Encabezado: AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de la solicitud <b>AP-Device-Identifier</b> contiene el identificador del dispositivo de flujo continuo tal como lo creó la aplicación cliente.

## Sintaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>: &lt;type&gt; &lt;identifier&gt;</td>
   </tr>
   <tr>
      <td>Tipo de encabezado</td>
      <td>Encabezado de solicitud</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>No</td>
   </tr>
</table>

## Directivas {#directives}

<b>&lt;tipo></b>

El tipo de identificador del dispositivo.

Solo hay un tipo compatible como se muestra a continuación.

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Tipo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>huella digital</td>
      <td>
            El identificador de dispositivo consiste en un identificador estable y único creado y administrado por la aplicación cliente para cada dispositivo.
            <br/>
            La aplicación cliente debe almacenar en caché el identificador del dispositivo en el almacenamiento persistente, ya que perderlo o modificarlo invalidará la autenticación. La aplicación cliente debe evitar los cambios de valor causados por las acciones del usuario, como la desinstalación, reinstalación o actualizaciones de la aplicación.
      </td>
   </tr>
</table>


<b>&lt;identificador></b>

El valor `Base64-encoded` del identificador del dispositivo.

## Ejemplo {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## Libros {#cookbooks}

>[!IMPORTANT]
>
> Los recursos de documentación se proporcionan con fines de referencia.
>
> Los recursos de documentación no son exhaustivos y pueden requerir modificaciones adicionales para trabajar en el proyecto.
> 
> Independientemente de su implementación real, el encabezado `AP-Device-Identifier` debe contener un valor con el formato descrito en la sección [Directivas](#directives).

### Navegadores {#browsers}

Para generar el encabezado `AP-Device-Identifier` para los dispositivos que se ejecutan en un explorador, la aplicación cliente necesita calcular un identificador estable y único basado en los datos disponibles, como el explorador, el dispositivo o datos específicos del usuario.

_(*) Se recomienda integrar una biblioteca o servicio que proporcione un mecanismo de huella digital de explorador o dispositivo._

### Dispositivos móviles {#mobile-devices}

#### iOS y iPadOS {#ios-ipados}

Para generar el encabezado `AP-Device-Identifier` para los dispositivos que ejecutan [iOS o iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes), puede consultar los siguientes documentos:

* Documentación para desarrolladores de Apple para [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Se recomienda aplicar una función hash SHA-256 sobre el valor proporcionado por el sistema operativo._

#### Android {#android}

Para generar el encabezado `AP-Device-Identifier` para los dispositivos que ejecutan [Android](https://developer.android.com/about/versions), puede consultar los siguientes documentos:

* Documentación para desarrolladores de Android para [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Se recomienda aplicar una función hash SHA-256 sobre el valor proporcionado por el sistema operativo._

### Dispositivos conectados a TV {#tv-connected-devices}

#### tvOS {#tvos}

Para generar el encabezado `AP-Device-Identifier` para los dispositivos que ejecutan [tvOS](https://developer.apple.com/documentation/tvos-release-notes), puede consultar los siguientes documentos:

* Documentación para desarrolladores de Apple para [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Se recomienda aplicar una función hash SHA-256 sobre el valor proporcionado por el sistema operativo._

#### Fire OS {#fireos}

Para generar el encabezado `AP-Device-Identifier` para los dispositivos que ejecutan [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html), puede consultar los siguientes documentos:

* Documentación para desarrolladores de Android para [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Se recomienda aplicar una función hash SHA-256 sobre el valor proporcionado por el sistema operativo._

#### Roku OS {#rokuos}

Para generar el encabezado `AP-Device-Identifier` para los dispositivos que ejecutan [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md), puede consultar los siguientes documentos:

* Documentación para desarrolladores de Roku para [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string).

_(*) Se recomienda aplicar una función hash SHA-256 sobre el valor proporcionado por el sistema operativo._

### Otros {#others}

En el caso de las plataformas de dispositivo no incluidas en la documentación, el identificador de dispositivo debe vincularse a cualquier identificación de hardware disponible, normalmente especificada en el manual de hardware del dispositivo.

Si no hay identificadores de hardware disponibles, se debe utilizar y almacenar en caché en el almacenamiento persistente un identificador generado de forma única basado en atributos de la aplicación cliente.
