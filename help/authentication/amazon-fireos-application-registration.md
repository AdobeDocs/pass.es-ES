---
title: Registro de aplicaciones de Amazon FireOS
description: Registro de aplicaciones de Amazon FireOS
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---

# Registro de aplicaciones de Amazon FireOS {#amazon-fireos-application-registration}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

## Introducción {#intro}

A partir de la versión 3.0 del SDK de FireOS AccessEnabler, estamos cambiando el mecanismo de autenticación con los servidores de Adobe. En lugar de utilizar una clave pública y un sistema secreto para firmar el ID de solicitante, presentamos el concepto de una cadena de declaración de software que se puede utilizar para obtener un token de acceso que luego se utiliza para todas las llamadas que el SDK realiza a nuestros servidores. Además de una Declaración de software, también deberá crear un vínculo profundo para su aplicación.

Para obtener más información, vea [Información general sobre el registro dinámico de clientes](./dcr-api/dynamic-client-registration-overview.md).

## ¿Qué es una declaración de software? {#what}

Una declaración de software es un token JWT que contiene información sobre su aplicación. Cada aplicación debe tener una Declaración de Software única que nuestros servidores utilizan para identificar la aplicación en el sistema de Adobe. La declaración de software debe pasarse al inicializar el SDK de AccessEnabler y se utilizará para registrar la aplicación con el Adobe. Tras el registro, el SDK recibirá un ID de cliente y un secreto de cliente que se utilizarán para obtener un token de acceso. Cualquier llamada que el SDK realice a nuestros servidores requerirá un token de acceso válido. El SDK es responsable de registrar la aplicación, obtener y actualizar el token de acceso.

**Nota:** las instrucciones de software son específicas de la aplicación y no se puede usar una instrucción de software individual para más de una aplicación. Tenga en cuenta que esto también se aplica a las aplicaciones que ofrecen acceso a varios canales.

## ¿Cómo obtener una declaración de software? {#how-to}

### Si tiene acceso al Tablero de TVE de Adobe:

1. Abra el explorador y vaya a `https://console.auth.adobe.com`.

1. Vaya a la sección **[!UICONTROL Channels]** y, a continuación, seleccione su canal.

1. Vaya a la ficha **[!UICONTROL Registered Applications]**.

1. Haga clic en **[!UICONTROL Add new application]**.

1. Proporcione un nombre y una versión para la aplicación y seleccione las plataformas en las que estará disponible (como Android).

1. Proporcione un **[!UICONTROL Domain Name]** eligiendo de una lista de dominios ya configurados para su programador.

1. Inserte los cambios en el servidor y vuelva a la pestaña **[!UICONTROL Registered Applications]** del canal.

   Debería ver una lista con todas las aplicaciones registradas.

1. Haga clic en **[!UICONTROL Download]** en la aplicación que acaba de crear.

   Es posible que tenga que esperar unos minutos antes de que su Declaración de software esté lista para su descarga.

   Se descarga un archivo de texto. Utilice su contenido como Declaración de software.

Para obtener más información, vea [Administración del registro de cliente dinámico](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Si no tiene acceso al Tablero de TVE de Adobe:

Envíe un ticket a [tve-support@adobe.com](mailto:tve-support@adobe.com). Incluya toda la información necesaria, incluidos el canal, el nombre de la aplicación, la versión y las plataformas. Alguien de nuestro equipo de asistencia creará una declaración de software para usted.

## Cómo utilizar la declaración de software {#use}

Después de obtener la instrucción de software, debe pasarla como parámetro en el constructor del Habilitador de acceso. Adobe recomienda alojar la Declaración de software en una ubicación remota. De este modo, puede revocar y cambiar fácilmente la Declaración de software sin lanzar una nueva versión de su aplicación.

## Cómo utilizar la declaración de software {#use-both}

Agregue el siguiente código en el archivo de recursos de su aplicación `strings.xml`:

```XML
<string name="software_statement">softwarestatement value</string>
```
