---
title: Compatibilidad con WKWebView en iOS SDK 3.1+
description: Compatibilidad con WKWebView en iOS SDK 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Compatibilidad con WKWebView (heredado) en iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

</br>

**Debido a que Apple ha desaprobado UIWebView en iOS, hemos actualizado iOS SDK 3.1 con compatibilidad con WKWebView.**

## Compatibilidad {#compatibility}

A partir de iOS SDK versión 3.1, los implementadores pueden utilizar ahora WKWebView o UIWebView de forma intercambiable. Dado que UIWebView está obsoleto en Apple, las aplicaciones deben migrar a WKWebView para evitar problemas con futuras versiones de iOS.

Tenga en cuenta que la migración implicaría simplemente cambiar la clase UIWebView con WKWebView, no hay ningún trabajo específico por hacer en relación con AccessEnabler de Adobe.

## Problemas conocidos {#known-issues}

AccessEnabler de Adobe utilizó una instancia de UIWebView interna oculta para realizar &quot;[autenticación pasiva](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)&quot; en determinadas MVPD. El flujo &quot;pasivo&quot; resultó útil para las MVPD que requieren autenticación para cada ID de solicitante y, a partir de este flujo, se beneficiaron los programadores que utilizaron el mismo ID de equipo en varias aplicaciones de iOS para simular una experiencia SSO (Adobe SSO). Actualmente, esta función la utilizan un número limitado de MVPD.

La función utilizaba un comportamiento de UIWebView que permitía a Adobe capturar las cookies de autenticación y reproducirlas durante el flujo &quot;pasivo&quot;. WKWebView introduce una seguridad más sólida que impide que Adobe capture las cookies configuradas al iniciar sesión y las reproduzca utilizando una instancia oculta de WKWebView. Debido a esta mejora de la seguridad y teniendo en cuenta que el flujo &quot;pasivo&quot; solo beneficiaba a un conjunto muy limitado de MVPD en un escenario de implementación muy específico (varias aplicaciones que utilizan el mismo ID de equipo), Adobe eliminó la función de &quot;autenticación pasiva&quot; para MVPD que utilizan vistas web para autenticarse.

La función sigue presente para las MVPD configuradas para utilizar SFSafariViewController, pero tenga en cuenta que en este caso la autenticación &quot;pasiva&quot; será visible para el usuario, ya que SFSafariViewController no se puede utilizar de forma &quot;oculta&quot;.
