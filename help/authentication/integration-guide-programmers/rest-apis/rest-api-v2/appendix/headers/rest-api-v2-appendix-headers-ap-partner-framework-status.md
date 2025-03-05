---
title: 'Encabezado: AP-Partner-Framework-Status'
description: 'API de REST V2: encabezado - AP-Partner-Framework-Status'
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# Encabezado: AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de la solicitud <b>AP-Partner-Framework-Status</b> contiene información de estado obtenida de un marco de trabajo de socio para lograr el inicio de sesión único (SSO).

## Sintaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;partner_framework_status_information&gt;</td>
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

<b>&lt;partner_framework_status_information></b>

El valor `Base64-encoded` del elemento JSON que contiene los atributos siguientes:

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Este es un atributo obligatorio.
         <br/><br/>
         La información de estado de permisos de usuario devuelta por el marco de trabajo del socio y procesada por la aplicación.
         <br/><br/>
         Es un elemento JSON con los atributos siguientes:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  Este es un atributo obligatorio.
                  <br/><br/>
                  Esta es una enumeración con los siguientes valores posibles:
                  <br/>
                  <ul>
                     <li><b>concedido</b><br/>El usuario permitió que la aplicación accediera a la información de suscripción.</li>
                     <li><b>denegado</b><br/>El usuario denegó la aplicación para tener acceso a la información de suscripción.</li>
                     <li><b>pendiente</b><br/>El usuario aún no ha elegido permitir que la aplicación acceda a la información de suscripción.</li>
                     <li><b>notDetermined</b><br/>La aplicación no tiene permiso para obtener acceso a la información de suscripción.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>error</td>
               <td>
                  Es un atributo opcional.
                  <br/><br/>
                  Esto se puede utilizar para pasar el error de marco de socio en caso de que se active uno al consultar la información de estado de permisos de usuario.
                  <br/><br/>
                  Es un elemento JSON con los atributos siguientes:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>código</td>
                        <td>Una cadena que identifica de forma exclusiva el error tal como lo define el marco de trabajo del socio.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>Una cadena que contiene la descripción del error tal como lo define el marco de trabajo del socio.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         Este es un atributo obligatorio.
         <br/><br/>
         La información de estado de inicio de sesión del proveedor devuelta por el marco de trabajo del socio y procesada por la aplicación.
         <br/><br/>
         Es un elemento JSON con los atributos siguientes:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  Este es un atributo obligatorio.
                  <br/><br/>
                  mappingId que identifica el MVPD utilizado durante el flujo de autenticación en el nivel de marco de trabajo del socio.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Este es un atributo obligatorio.
                  <br/><br/>
                  Esta es la fecha de caducidad del perfil de usuario autenticado, en caso de que el usuario haya iniciado sesión correctamente utilizando un MVPD compatible en el nivel de marco de trabajo del socio.
               </td>
            </tr>
            <tr>
               <td>error</td>
               <td>
                  Es un atributo opcional.
                  <br/><br/>
                  Esto se puede utilizar para pasar el error de marco de socio en caso de que se active uno al consultar la información de estado de inicio de sesión del proveedor.
                  <br/><br/>
                  Es un elemento JSON con los atributos siguientes:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>código</td>
                        <td>Una cadena que identifica de forma exclusiva el error tal como lo define el marco de trabajo del socio.</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>Una cadena que contiene la descripción del error tal como lo define el marco de trabajo del socio.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## Ejemplos {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
