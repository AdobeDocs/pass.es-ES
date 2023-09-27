---
title: Prueba de los flujos de autenticación y autorización mediante el sitio de prueba de la API de Adobe
description: Prueba de los flujos de autenticación y autorización mediante el sitio de prueba de la API de Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Prueba de los flujos de autenticación y autorización mediante el sitio de prueba de la API de Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Para probar los flujos AuthN y AuthZ, hemos preparado una **Sitio de prueba de API** que está a su disposición. Nuestro equipo de soporte estará encantado de proporcionarle las credenciales. Puede contactarnos en **support@tve.zendesk.com**.


## Parte I {#part-I}

Para realizar pruebas con el entorno de RELEASE, vaya directamente a la Parte II.  Para realizar pruebas en el entorno de precalificación, consulte [Configurar el entorno y realizar pruebas en la calidad previa](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## Parte II

Después de completar la parte I, realice los siguientes pasos:


1. Abrir página web: [Prueba de API de ensayo](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Cargue el activador de acceso desde la siguiente URL:
   * [Acceso al javascript del habilitador para ensayo](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * O
   * [Acceso al javascript habilitado para producción](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * A CONTINUACIÓN, haga clic en &quot;**Habilitador de acceso a carga** Botón &quot;.
1. Ahora establezca el valor de ID del solicitante en &quot;**requestorID**&quot; y haga clic en el botón &quot;setRequestor&quot;.
1. Después, presione el botón &quot;getAuthentication&quot; y espere a que aparezca el selector de pantalla.
1. Seleccione el &quot;**MVPD**&quot; del selector.
1. Introduzca sus credenciales en la &quot;**MVPD**&quot; página de inicio de sesión.
1. Después de ser redirigido hacia atrás, rehaga los pasos del 1 al 3
1. Después de rehacer el paso 3 en &quot;setAuthenticationStatus&quot; debería ver el valor &quot;1&quot;. Si la autenticación no funcionó, aparecerá el cuadro de diálogo MVPD.
1. Para probar la autorización, en el campo de entrada de recurso introduzca &quot;**requestorID**&quot; y haga clic en el botón &quot;getAuthorization&quot;.
1. Como resultado, en el cuadro de texto &quot;setToken&quot;-\>&quot;resource id&quot; se mostrará el recurso y en el cuadro de texto &quot;setToken&quot;-\>&quot;token&quot; se mostrará shortAuthorizationToken, lo que significa que authZ se realizó correctamente.
1. Ahora puede hacer clic en el botón &quot;Cerrar sesión&quot; para eliminar los tokens.
