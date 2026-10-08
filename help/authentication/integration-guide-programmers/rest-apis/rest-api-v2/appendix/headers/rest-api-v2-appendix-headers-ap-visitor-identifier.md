---
title: 'Encabezado: AP-Visitor-Identifier'
description: 'API de REST V2: encabezado: AP-Visitor-Identifier'
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 1%
---
# Encabezado: AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de la solicitud <b>AP-Visitor-Identifier</b> contiene `ECID`, requerido por la aplicación cliente para identificar de forma exclusiva a un visitante en las soluciones de Adobe Experience Cloud.

Para obtener más información sobre el uso de ECID en la autenticación de Adobe Pass, consulte [Uso del Experience Cloud ID en la autenticación de Adobe Pass](../../../../features-premium/analytics/exp-cloud-id-authn.md).

## Sintaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b>: &lt;visitor_identifier&gt;</td>
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

## Ejemplos {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
