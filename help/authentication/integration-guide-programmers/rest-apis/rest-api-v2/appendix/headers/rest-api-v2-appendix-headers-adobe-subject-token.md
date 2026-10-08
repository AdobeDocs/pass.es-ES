---
title: 'Encabezado: Adobe-Subject-Token'
description: 'API de REST V2: encabezado: Adobe-Subject-Token'
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 1%
---
# Encabezado: Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de solicitud <b>Adobe-Subject-Token</b> contiene el identificador de plataforma único como `JWS` o `JWE` obtenido de un servicio de identidad o biblioteca que se ejecuta fuera de los sistemas de autenticación de Adobe Pass.

Este encabezado está diseñado para utilizarse en flujos habilitados para el inicio de sesión único (SSO) que aprovechan el método de identidad de plataforma.

Para obtener más información sobre los flujos habilitados para el inicio de sesión único (SSO) que aprovechan el método de identidad de plataforma, consulte la documentación de [Inicio de sesión único mediante flujos de identidad de plataforma](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Sintaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

<b>identificador_plataforma_única</b>

La firma web JSON (`JWS`) o el cifrado web JSON (`JWE`) que es un token web JSON (`JWT`) firmado o cifrado que contiene información de identificador de plataforma única.

Esta opción está disponible para las siguientes plataformas:

* [Guía de Amazon SSO (API REST V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## Ejemplos {#examples}

Consulte los ejemplos tal como se describe para las siguientes plataformas:

* [Guía de Amazon SSO (API REST V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
