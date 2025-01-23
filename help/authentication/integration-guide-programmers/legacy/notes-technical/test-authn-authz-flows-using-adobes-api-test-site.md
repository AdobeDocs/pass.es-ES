---
title: Prueba de los flujos de autenticación y autorización mediante el sitio de prueba de la API de Adobe
description: Prueba de los flujos de autenticación y autorización mediante el sitio de prueba de la API de Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 65475d6da7a1b25cb2d8ebd6229a7cb360c7ab4a
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# (Heredado) Prueba de los flujos de autenticación y autorización mediante el sitio de prueba de la API de Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

Para probar los flujos AuthN y AuthZ, hemos preparado un **sitio de prueba de API** que está a su disposición. Nuestro equipo de soporte estará encantado de proporcionarle las credenciales. Puede comunicarse con nosotros en **tve-support@adobe.com**.


## Parte I {#part-I}

Para realizar pruebas con el entorno de RELEASE, vaya directamente a la Parte II.  Para probar en el entorno de precalificación, consulte [Configuración de su entorno y pruebas en precalificación](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

## Parte II

Después de completar la parte I, realice los siguientes pasos:


1. Abrir página web: [Prueba de la API de ensayo](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Cargar el habilitador de acceso mediante:
   * Selección del menú desplegable desde el que desea acceder (ensayo o producción) y si debe estar en modo de depuración
   * Introducción de la instrucción de software con la que desea realizar la prueba
   * A continuación, haga clic en el botón &quot;**Cargar activador de acceso**&quot;.
1. Ahora establezca el valor del id del solicitante en &quot;**requestorID**&quot; y haga clic en el botón &quot;setRequestor&quot;.
1. Después, presione el botón &quot;getAuthentication&quot; y espere a que aparezca el selector de pantalla.
1. Seleccione el &quot;**MVPD**&quot; del selector.
1. Escriba sus credenciales en la página de inicio de sesión de &quot;**MVPD**&quot;.
1. Después de ser redirigido hacia atrás, rehaga los pasos del 1 al 3
1. Después de rehacer el paso 3 en &quot;setAuthenticationStatus&quot; debería ver el valor &quot;1&quot;. Si la autenticación no funcionó, se mostrará el cuadro de diálogo MVPD.
1. Para probar la autorización, en el campo de entrada a la derecha de los botones etiquetados como &quot;checkAuthorization&quot; y &quot;getAuthorization&quot;, introduzca el **recurso** para el que desea autorizar y haga clic en el botón &quot;getAuthorization&quot;.
1. Como resultado, en el cuadro de texto &quot;setToken&quot;-\>&quot;resource id&quot; se mostrará el recurso y en el cuadro de texto &quot;setToken&quot;-\>&quot;token&quot; se mostrará shortAuthorizationToken, lo que significa que authZ se realizó correctamente.
1. Ahora puede hacer clic en el botón &quot;Cerrar sesión&quot; para eliminar los tokens.
