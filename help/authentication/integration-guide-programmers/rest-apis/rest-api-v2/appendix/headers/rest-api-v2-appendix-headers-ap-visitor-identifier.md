---
title: 'Encabezado: AP-Visitor-Identifier'
description: 'API de REST V2: encabezado: AP-Visitor-Identifier'
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: 7d19319d83af0122624ab3b80b1f31fc0cbbf182
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---

# Encabezado: AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de la solicitud <b>AP-Visitor-Identifier</b> contiene `ECID`, requerido por la aplicación cliente para identificar un visitante de forma exclusiva en todas las soluciones de Adobe Experience Cloud.

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
