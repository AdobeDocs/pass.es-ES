---
title: 'Encabezado: X-Roku-Reserved-Roku-Connect-Token'
description: 'API de REST V2: encabezado: X-Roku-Reserved-Roku-Connect-Token'
exl-id: 21016d5b-4d10-4018-a82c-f2797b2d9fb9
source-git-commit: 2afe9ea2a814817757f1ab28484a84466da68d62
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# Encabezado: X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de solicitud <b>X-Roku-Reserved-Roku-Connect-Token</b> contiene el identificador de plataforma único como `JWS` o `JWE` obtenido de un servicio de identidad o biblioteca que se ejecuta fuera de los sistemas de autenticación de Adobe Pass.

Este encabezado está diseñado para utilizarse en flujos habilitados para el inicio de sesión único (SSO) que aprovechan el método de identidad de plataforma.

Para obtener más información sobre los flujos habilitados para el inicio de sesión único (SSO) que aprovechan el método de identidad de plataforma, consulte la documentación de [Inicio de sesión único mediante flujos de identidad de plataforma](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Sintaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>: &lt;unique_platform_identifier&gt;</td>
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

* [Guía de Roku SSO (API de REST V2)](/help/premium-workflow/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
