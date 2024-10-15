---
title: 'Encabezado: autorización'
description: API de REST V2 - Encabezado - Autorización
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---


# Encabezado: autorización {#header-authorization}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de la solicitud <b>Authorization</b> contiene el token de acceso `Bearer` requerido por la aplicación cliente para acceder a las API protegidas por Adobe Pass.

Para obtener más información sobre el mecanismo para acceder a las API protegidas por Adobe Pass, consulte la [Información general sobre el registro de clientes dinámicos](../../../dcr-api/dynamic-client-registration-overview.md).

## Sintaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Autorización</b>: Portador &lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>Tipo de encabezado</td>
      <td>Encabezado de solicitud</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>Sí</td>
   </tr>
</table>

## Directivas {#directives}

<b>&lt;token_de_acceso></b>

El valor del token de acceso es un valor opaco que tiene un tiempo de vida limitado (por ejemplo, 24 horas) que debe obtenerse de Adobe Pass tal como se describe en la [Recuperación del token de acceso](../../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) de la documentación de la API.

## Ejemplos {#examples}

```JSON
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
