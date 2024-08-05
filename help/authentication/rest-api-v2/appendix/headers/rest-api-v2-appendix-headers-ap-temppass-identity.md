---
title: 'Encabezado: AP-TempPass-Identity'
description: 'API de REST V2: encabezado: AP-TempPass-Identity'
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 3%

---


# Encabezado: AP-TempPass-Identity {#header-ap-temppass-identity}

## Información general {#overview}

El encabezado de la solicitud <b>AP-TempPass-Identity</b> contiene la información de identidad del usuario que se utilizó para obtener TempPass promocional.

## Sintaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>: &lt;user_identity_information&gt;</td>
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

<b>&lt;user_identity_information></b>

El valor `Base64-encoded` sobre la información de identidad del usuario asociado con el usuario final al que se debe otorgar un acceso temporal promocional.

## Ejemplos {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
