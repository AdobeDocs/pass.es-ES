---
title: 'Encabezado: AP-Device-Identifier'
description: 'API de REST V2: encabezado: AP-Device-Identifier'
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '77'
ht-degree: 1%

---


# Encabezado: AP-Device-Identifier {#header-ap-device-identifier}

## Información general {#overview}

El encabezado de la solicitud <b>AP-Device-Identifier</b> contiene el identificador del dispositivo de flujo continuo tal como lo creó la aplicación cliente.

## Sintaxis {#syntax}

<table>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Tipo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>huella digital</td>
      <td>El identificador del dispositivo consiste en un identificador opaco y lo crea la aplicación cliente.</td>
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
