---
title: Registro dinámico de clientes
description: Registro dinámico de clientes
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Registro dinámico de clientes {#dynamic-client-registration}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Contexto {#context}

Para cumplir con las prácticas de seguridad modernas, los requisitos mejorados de UX y de los propietarios de plataformas, el SDK de Android de autenticación de Adobe Pass y el SDK de iOS avanzan hacia la adopción de [Fichas personalizadas de Android Chrome](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} and [Apple Safari view controller](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}.

La implementación actual de AdobePass utiliza vistas web específicas de la plataforma, a fin de proporcionar el entorno web para mostrar la página de inicio de sesión de MVPD. Estas vistas web no comparten la administración de credenciales con los exploradores de la plataforma, por lo que el usuario no puede utilizar una contraseña guardada del explorador al utilizar una aplicación de autenticación de Adobe Pass. Además, por motivos de seguridad, algunas plataformas se están moviendo para dejar de utilizar los controladores WebView para las tareas de autenticación. Tanto Google como Apple ofrecen opciones alternativas como &quot;Chrome Custom Tabs&quot; y &quot;Safari View Controller&quot;. Básicamente son pestañas de un solo uso de sus respectivos navegadores. La autenticación de Adobe Pass adoptará estos nuevos componentes en 2018.

## Detalles {#details}

Actualmente, la autenticación de Adobe Pass identifica y registra las aplicaciones de dos formas:

* los clientes basados en el explorador se registran mediante la lista de dominios permitidos
* los clientes de aplicaciones nativas, como las aplicaciones de iOS y Android, se registran mediante el mecanismo de solicitante firmado

Para abordar los nuevos flujos para Chrome Custom Tabs y Safari View Controller, Adobe Pass propone un nuevo mecanismo de registro de clientes para registrar nuevas aplicaciones. Este mecanismo permitirá un control más seguro y granular de sus aplicaciones y se puede utilizar para registrar aplicaciones en todas las plataformas.

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
