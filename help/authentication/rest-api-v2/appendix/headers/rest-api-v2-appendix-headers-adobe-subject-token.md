---
title: 'Encabezado: Adobe-Subject-Token'
description: 'API de REST V2: encabezado: Adobe-Subject-Token'
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 1%

---


# Encabezado: Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de solicitud <b>Adobe-Subject-Token</b> contiene el identificador de plataforma único `JWS` o `JWE` obtenido de un servicio de identidad o biblioteca que se ejecuta fuera de los sistemas de autenticación de Adobe Pass.

Este encabezado está diseñado para utilizarse en flujos habilitados para el inicio de sesión único (SSO) que aprovechan el método de identidad de plataforma.

Para obtener más información sobre los flujos habilitados para el inicio de sesión único (SSO) que aprovechan el método de identidad de plataforma, consulte la documentación de [Inicio de sesión único mediante flujos de identidad de plataforma](../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Sintaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Token de asunto de Adobe</b>: &lt;unique_platform_identifier&gt;</td>
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

* [Amazon](../../../amazon-fireos-sso-using-clientless-api-cookbook.md)

## Ejemplos {#examples}

Consulte los ejemplos tal como se describe para las siguientes plataformas:

* [Amazon](../../../amazon-fireos-sso-using-clientless-api-cookbook.md)
