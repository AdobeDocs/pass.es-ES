---
title: Registro de aplicaciones Android
description: Registro de aplicaciones Android
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Registro de aplicaciones de Android (heredadas) {#android-application-registration}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#intro}

A partir de la versión 3.0 de Android AccessEnabler SDK, estamos cambiando el mecanismo de autenticación con los servidores de Adobe. En lugar de utilizar una clave pública y un sistema secreto para firmar el ID de solicitante, presentamos el concepto de una cadena de declaración de software que se puede utilizar para obtener un token de acceso que luego se utiliza para todas las llamadas que SDK realiza a nuestros servidores. Además de una Declaración de software, también deberá crear un vínculo profundo para su aplicación.

Para obtener más información, vea [Información general sobre el registro dinámico de clientes](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## ¿Qué es una declaración de software? {#what}

Una declaración de software es un token JWT que contiene información sobre su aplicación. Cada aplicación debe tener una declaración de software única que nuestros servidores utilicen para identificar la aplicación en el sistema de Adobe.

Se debe pasar la instrucción de software al inicializar el SDK `AccessEnabler`. Se utiliza para registrar la aplicación con el Adobe. Tras el registro, SDK recibe un ID de cliente y un secreto de cliente, que se utilizan para obtener un token de acceso. Cualquier llamada que SDK realice a los servidores de Adobe requiere un token de acceso válido. SDK es responsable de registrar la aplicación, obtener y actualizar el token de acceso.

>[!NOTE]
>
>Las declaraciones de software son específicas de la aplicación y una declaración de software individual no se puede utilizar para más de una aplicación. Tenga en cuenta que las instrucciones de software a nivel de programador tienen la misma restricción, solo se pueden utilizar para una sola aplicación, ya sea de un solo canal o multicanal.

## Cómo obtener una declaración de software {#how-to-get-ss}

A continuación se indican formas de obtener una Declaración de software.

### Si tiene acceso al Tablero de TVE de Adobe

1. Abra el explorador y vaya al [Panel de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

1. Vaya a la sección **[!UICONTROL Channels]** y, a continuación, seleccione su canal.

1. Vaya a la ficha **[!UICONTROL Registered Applications]**.

1. Haga clic en **[!UICONTROL Add new application]**.

1. Asigne un nombre a la aplicación y especifique una versión.

1. Seleccione las plataformas en las que la aplicación estará disponible (Android en este caso).

1. Proporcione un **[!UICONTROL Domain Name]** eligiendo de una lista de dominios ya configurados para su programador.

1. Inserte los cambios en el servidor y vuelva a la pestaña **[!UICONTROL Registered Applications]** del canal.

   Debería ver una lista con todas las aplicaciones registradas. Seleccione **[!UICONTROL Download]** en la aplicación que creó. Es posible que tenga que esperar unos minutos antes de que su declaración de software esté lista para su descarga.

   Se descarga un archivo de texto. Utilice su contenido como Declaración de software.

Para obtener más información, consulte [Dynamic Client Registration Management](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

### Si no tiene acceso al Tablero de TVE de Adobe

Enviar un ticket a `tve-support@adobe.com`. Incluya la información necesaria como canal, nombre de aplicación, versión y plataformas. Alguien de nuestro equipo de soporte creará una declaración de software para usted.

## Cómo utilizar la declaración de software {#how-to-use-ss}

Después de obtener la instrucción de software, debe pasarla como parámetro en el constructor del Habilitador de acceso. Se recomienda alojar la Declaración de software en una ubicación remota. De este modo, puede revocar y cambiar fácilmente la Declaración de software sin lanzar una nueva versión de su aplicación.

## Cree y utilice un vínculo profundo para su aplicación {#create}

En Android, utilice como valor de vínculo profundo el inverso del nombre de dominio seleccionado al crear la Declaración de software

El vínculo profundo creado debe tener un valor único en el dispositivo Android. Cuando varias aplicaciones utilizan el mismo valor de vínculo profundo, los flujos de autenticación y cierre de sesión interferirán.

## Cómo utilizar la Declaración de software y el vínculo profundo {#use-both}

Agregue el siguiente código en el archivo de recursos de su aplicación `strings.xml`:

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
