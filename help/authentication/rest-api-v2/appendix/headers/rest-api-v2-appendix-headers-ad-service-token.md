---
title: 'Encabezado: AD-Service-Token'
description: 'API de REST V2: encabezado: AD-Service-Token'
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '226'
ht-degree: 1%

---


# Encabezado: AD-Service-Token {#header-ad-service-token}

## Información general {#overview}

El encabezado de la solicitud <b>AD-Service-Token</b> contiene el identificador de usuario único como `JWS` obtenido de un servicio de identidad que se ejecuta fuera de los sistemas de autenticación de Adobe Pass.

Este encabezado está diseñado para utilizarse en flujos habilitados para el inicio de sesión único (SSO) que aprovechan el método del token de servicio.

Para obtener más información sobre los flujos habilitados para el inicio de sesión único (SSO) que aprovechan el método del token de servicio, consulte la documentación de [Inicio de sesión único mediante flujos de token de servicio](../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md).

## Sintaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Token de servicio de AD</b>: &lt;unique_user_identifier&gt;</td>
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

<b>identificador_usuario_único</b>

La firma web JSON (`JWS`) que es un token web JSON (`JWT`) firmado que contiene información de identificador de usuario único.

`JWT` tiene los atributos siguientes:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
      <th style="background-color: #EFF2F7;">Descripción</th>
   </tr>
   <tr>
      <td>is</td>
      <td>Identificador único asociado a la entidad que ofrece a la aplicación un servicio de identidad externo para lograr el inicio de sesión único (SSO).</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>El identificador único del usuario devuelto por el servicio de identidad externo.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>La audiencia, que debería ser "Adobe".</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>El emitido a la marca de tiempo para el JWT actual.</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>La marca de tiempo de caducidad para el JWT actual.</td>
   </tr>
</table>

El `JWT` debe estar firmado usando el algoritmo `SHA256withRSA`.

El `JWT` debe estar firmado con una clave privada, que forma parte de un par de clave privada RSA - clave pública administrada por el servicio de identidad externo.

La clave pública de ese par debe entregarse a la autenticación de Adobe Pass para poder reconocer `JWT` tokens firmados con la clave privada mencionada.

## Ejemplos {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
