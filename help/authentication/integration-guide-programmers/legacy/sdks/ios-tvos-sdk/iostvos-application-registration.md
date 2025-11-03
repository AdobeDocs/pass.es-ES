---
title: Registro de aplicaciones de iOS/tvOS
description: Registro de aplicaciones de iOS/tvOS
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---


# Registro de aplicaciones de iOS/tvOS (heredadas) {#iostvos-application-registration}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Introducción {#Intro}

A partir de la versión 3.0 de iOS/tvOS AccessEnabler SDK, estamos cambiando el mecanismo de autenticación con los servidores de Adobe. En lugar de utilizar una clave pública y un sistema secreto para firmar el ID de solicitante, presentamos el concepto de una cadena de declaración de software que se puede utilizar para obtener un token de acceso que luego se utiliza para todas las llamadas que SDK realiza a nuestros servidores. Además de una declaración de software, también necesitará un esquema de URL personalizado para su aplicación.

Para obtener más información, vea [Información general sobre el registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## ¿Qué es una declaración de software? {#Soft_state}

Una declaración de software es un token JWT que contiene información sobre su aplicación. Cada aplicación debe tener una declaración de software única que nuestros servidores utilicen para identificar la aplicación en el sistema de Adobe. La Declaración de software debe pasarse al inicializar AccessEnabler SDK y se utilizará para registrar la aplicación en Adobe. Tras el registro, SDK recibirá un ID de cliente y un secreto de cliente que se utilizarán para obtener un token de acceso. Cualquier llamada que SDK realice a nuestros servidores requerirá un token de acceso válido. SDK es responsable de registrar la aplicación, obtener y actualizar el token de acceso.

**Nota:** Una instrucción de software es específica de la aplicación y no se puede usar la misma instrucción de software en más de una aplicación. Tenga en cuenta que las instrucciones de software a nivel de programador también siguen el mismo procedimiento, es decir, solo se pueden utilizar para una sola aplicación, ya sea de un solo canal o multicanal. Esta limitación también se aplica al esquema personalizado.

## ¿Cómo obtener una declaración de software? {#obtain}

### Si tiene acceso al Tablero de TVE de Adobe:

- Abra el explorador y vaya a <https://experience.adobe.com/#/pass/authentication>
- Vaya a la sección `Channels` y seleccione su canal.
- Vaya a la ficha `Registered Applications`.
- Haga clic en `Add new application`.
- Proporcione un nombre y una versión para la aplicación y seleccione   plataformas en las que estará disponible. iOS/tvOS en nuestro caso.
- Inserte los cambios en el servidor y, a continuación, vuelva a la pestaña Aplicaciones registradas del canal.
- Debería ver una lista con todas las aplicaciones registradas. Haga clic en   `Download` botón en la aplicación que acaba de crear. Es posible que tenga que esperar unos minutos antes de que su declaración de software esté lista para su descarga.
- Se descargará un archivo de texto. Utilice su contenido como Declaración de software.

Para obtener más información, vea [Administración del registro de cliente dinámico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Si no tiene acceso al Tablero de TVE de Adobe:

Enviar un ticket a <tve-support@adobe.com>. Incluya toda la información necesaria, como el canal, el nombre de la aplicación, la versión y las plataformas. Alguien de nuestro equipo de asistencia creará una declaración de software para usted.

## ¿Cómo utilizar la Declaración de software? {#use}

Después de obtener la Declaración de software, debe pasarla como un parámetro en el constructor del Habilitador de acceso. Se recomienda alojar la Declaración de software en una ubicación remota. De este modo, puede revocar y cambiar fácilmente la Declaración de software sin lanzar una nueva versión de su aplicación.

## Generar un esquema de URL personalizado para la aplicación {#generating}

### Si tiene acceso al Tablero de TVE de Adobe:

- Abra el explorador y vaya a <https://experience.adobe.com/#/pass/authentication>
- Vaya a la sección `Channels` y seleccione su canal.
- Vaya a la ficha `Custom Schemes`.
- Haga clic en `Generate a new custom scheme`.
- Se generará un nuevo esquema personalizado para la aplicación. Ejemplo: `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- Inserte los cambios en el servidor.

### Si no tiene acceso al Tablero de TVE de Adobe:

Enviar un ticket a <tve-support@adobe.com>. Incluya el ID de canal y alguien de nuestro equipo de asistencia creará un esquema personalizado para usted.

## Cómo utilizar el esquema personalizado {#use_custom}

En el archivo `info.plist` de su aplicación, agregue el siguiente código:

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
