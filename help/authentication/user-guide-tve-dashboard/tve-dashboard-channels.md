---
title: Canales
description: Obtenga información sobre los canales y sus distintas configuraciones dentro del tablero de TVE.
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# Canales {#channels}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La sección **Canales** del Tablero de TVE le permite ver y administrar la configuración de los canales asociados con un programador específico. También puede [agregar un canal nuevo](#add-new-channel) según sus necesidades.

La ficha **Canales** del panel izquierdo muestra una lista de canales vinculados con los siguientes detalles:

* **Nombre para mostrar**: El nombre de marca del canal usado con fines comerciales.
* **ID de canal**: Un identificador único, también denominado ID de solicitante.
* **Integraciones**: El número de conexiones establecidas con [MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd).

![Lista de canales existentes](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channels-list-view.png)

*Lista de canales existentes*

Escriba el nombre del canal en la barra **Buscar** situada encima de la lista para obtener más información sobre el canal.

## Administrar configuraciones de canal {#manage-channel-conf}

Siga los pasos para administrar varias configuraciones de un canal específico.

1. Seleccione la ficha **Canales** en el panel izquierdo.

1. Seleccione el canal de la lista disponible.

1. Seleccione una de las siguientes pestañas para ver y editar la configuración correspondiente del canal seleccionado:

   * [Configuración general](#general-settings)
   * [Integraciones](#integrations)
   * [Certificados](#certificates)
   * [Domains](#domains)
   * [Aplicaciones registradas](#registered-applications)
   * [Esquemas personalizados](#custom-schemes)

   ![Configuración de canal](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-tabs-view.png)

   *Configuración de canal*

>[!IMPORTANT]
>
> Ver [cambios de revisión y inserción](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) para obtener más información sobre cómo activar los cambios de configuración.

### Configuración general {#general-settings}

Esta ficha presenta **Información del canal** y **Configuración de Analytics**.

#### Información del canal {#channel-information}

En esta sección, puede editar los siguientes detalles:

* **Nombre para mostrar**: El nombre de marca del canal usado con fines comerciales.

* **URL de redireccionamiento predeterminada**: La URL de redireccionamiento de copia de seguridad para la autenticación y el cierre de sesión.

* **Informes de errores**: al seleccionar **Sí**, los SDK de Adobe Pass envían informes de errores al servidor de Adobe Pass para Analytics.

![Editar información de canal](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-general-settings-tab-view.png)

*Editar información de canal*

#### Configuración de Analytics {#analytics-configuration}

Esta sección le permite configurar el reenvío de eventos de autenticación de Adobe Pass a Adobe Analytics.

Para habilitar la **configuración de Analytics**, póngase en contacto con el administrador técnico de cuentas (TAM) para obtener más información sobre cómo configurar el ID del grupo de informes (RSID).

![Habilitar configuraciones de Analytics](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-analytics-configuration-button.png)

*Habilitar configuraciones de Analytics*

Seleccione **Agregar nueva configuración de análisis** para agregar varias configuraciones.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar la nueva configuración de Analytics de la sección **Configuración de Analytics**, continúe con el flujo [revisar e insertar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

### Integraciones {#integrations}

Esta pestaña muestra una lista de las integraciones disponibles entre el canal seleccionado actualmente y las MVPD. La lista presenta cada integración junto con su estado, indicando si está habilitada o no. Seleccione una integración específica de esta lista para acceder a información detallada en la sección [Integraciones](tve-dashboard-integrations.md).

![Lista de integraciones disponibles](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-integrations-tab-view.png)

*Lista de integraciones disponibles*

### Certificados {#certificates}

Esta ficha muestra una lista de [certificados disponibles](#available-certificates) y [certificados disponibles heredados](#inherited-avail-certificates) utilizados en los flujos de cifrado de metadatos de usuario. Muestra detalles acerca de cada certificado, incluidos los siguientes:

* El estado (independientemente de si está habilitado para el uso de **cifrado de metadatos de usuario** o no)
* Número de serie
* Nombre de la organización emisora
* Nombre de la organización a la que pertenece
* Fecha de emisión
* Fecha de caducidad
* Menú desplegable para cifrar los metadatos del usuario (si selecciona **Sí**, el certificado cifrará la información confidencial del usuario, como los valores del código postal).

#### Certificados disponibles {#available-certificates}

Estos certificados sirven como claves privadas o públicas y se utilizan para el cifrado de metadatos de usuarios.
Puede realizar los siguientes cambios en la sección certificados disponibles:

* [Añadir nuevo certificado](#add-new-certificate)
* [Eliminar certificado](#delete-certificate)

##### Añadir nuevo certificado {#add-new-certificate}

Para añadir un nuevo certificado, siga estos pasos:

1. Seleccione **Agregar nuevo certificado** en la parte superior de la sección **Certificados disponibles**.

   ![Agregar nuevo certificado](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-certificate-button.png)

   *Agregar nuevo certificado*

1. Pegue la clave pública del certificado en el cuadro de diálogo **Nuevo certificado**.

1. Seleccione **Agregar certificado**.

1. Busque el nuevo certificado en la lista de **Certificados disponibles**.

   >[!IMPORTANT]
   >
   > Asegúrese de que los sistemas estén actualizados y listos para utilizar el nuevo certificado.

1. Seleccione **Sí** de **Se usó para cifrar los metadatos del usuario** en el menú desplegable para activar un nuevo certificado.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar el nuevo certificado que aparece en la sección **Certificados disponibles**, continúe con el flujo [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

##### Eliminar certificado {#delete-certificate}

Siga estos pasos para eliminar un certificado.

1. Pase el ratón sobre el certificado que quiera eliminar de la lista de **certificados disponibles**.

1. Seleccione **Quitar**.

   ![Quitar el certificado seleccionado](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-delete-certificate-button.png)

   *Quitar el certificado seleccionado*

1. Seleccione **Eliminar** del cuadro de diálogo **Eliminar certificado activo**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. El certificado se eliminará de la sección **Certificados disponibles** solo después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Certificados disponibles heredados {#inherited-avail-certificates}

Las empresas de medios definen estos certificados en su propio nivel. Todos los canales asociados con la misma empresa de medios pueden utilizar estos certificados.

![Certificados disponibles heredados](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-available-certificates-panel-view.png)

*Certificados disponibles heredados*

### Domains {#domains}

Esta pestaña muestra una lista de los dominios disponibles a través de los cuales el canal respectivo se comunica con la autenticación de Adobe Pass.

Puede realizar los siguientes cambios en los dominios:

* [Añadir un nuevo dominio](#add-domains)
* [Eliminar dominio](#delete-domain)

>[!TIP]
>
> Evite agregar un nuevo subdominio si existe un dominio más general en la lista.

#### Añadir nuevo dominio {#add-domains}

Siga estos pasos para agregar un dominio.

1. Seleccione **Agregar nuevo dominio** en la esquina superior derecha de la sección **Dominios disponibles**.

   ![Agregar nuevo dominio](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-domain-button.png)

   *Agregar nuevo dominio*

1. Escriba el nombre de su dominio en el cuadro de diálogo **Nuevo dominio**.

1. Seleccione **Agregar dominio** para agregar un nuevo dominio para el canal seleccionado.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar el nuevo dominio que aparece en la sección **Dominios disponibles**, continúa con el flujo [revisar e insertar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Eliminar dominio {#delete-domain}

Siga estos pasos para eliminar un dominio.

1. Pase el ratón sobre el dominio que quiera eliminar de la lista de **dominios disponibles**.

1. Seleccione **Quitar**.

   ![Quitar el dominio seleccionado](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-remove-domain-button.png)

   *Quitar el dominio seleccionado*

1. Seleccione **Eliminar** en el cuadro de diálogo **Eliminar dominio**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. El dominio se eliminará de la sección **Dominios disponibles** solo después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

El dominio seleccionado ya no está disponible para su uso. Como resultado, la aplicación asociada con este dominio pierde el acceso a los servicios de autenticación de Adobe Pass.

### Aplicaciones registradas {#registered-applications}

Esta pestaña muestra una lista de las aplicaciones registradas. Para obtener más información relacionada con el uso de aplicaciones registradas, consulte la [descripción general del registro dinámico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Puede realizar las siguientes acciones con las aplicaciones registradas:

* [Agregar una nueva aplicación registrada](#add-registered-applications)
* [Descargar una declaración de software](#download-software-statement)

#### Agregar nueva aplicación registrada {#add-registered-applications}

Siga estos pasos para agregar una nueva aplicación registrada.

1. Seleccione **Agregar nueva aplicación** en la esquina superior derecha de la sección **Aplicaciones registradas**.

   ![Agregar nueva aplicación](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-application-button.png)

   *Agregar nueva aplicación*

1. Seleccione **Plataformas** en el menú desplegable del cuadro de diálogo **Nueva aplicación**.

   >[!IMPORTANT]
   >
   > Se recomienda crear aplicaciones registradas con permisos más específicos y limitados para mejorar la seguridad y evitar el acceso no autorizado. Por lo tanto, cuando cree aplicaciones registradas, considere la posibilidad de utilizar opciones más reducidas para el `platforms` asignado.

1. Seleccione **Dominios** en el menú desplegable.

   >[!IMPORTANT]
   >
   > En el proceso de registro del cliente, la aplicación cliente puede solicitar que se le permita utilizar una URL de redireccionamiento para finalizar el flujo de autenticación. Cuando una aplicación cliente utiliza una dirección URL de redireccionamiento específica, se valida con el `domains` seleccionado en esta selección.

1. Escriba **Name** de la aplicación.

1. Escriba **Version** de la aplicación.

   >[!IMPORTANT]
   >
   > Se recomienda crear una nueva aplicación registrada para cada actualización principal de la aplicación cliente para administrar su ciclo de vida y uso. Si es necesario, cree un ticket a través de [Zendesk](https://adobeprimetime.zendesk.com) y pídale al administrador de cuentas técnico (TAM) que revoque una aplicación registrada para bloquear la funcionalidad de una versión específica de la aplicación cliente.

1. Seleccione el valor &quot;DIRECT&quot; de **Type** en el menú desplegable.

1. Seleccione **Agregar aplicación**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar la nueva aplicación registrada que aparece en la sección **Aplicaciones registradas**, continúe con el flujo [revisar e insertar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Descargar declaración de software {#download-software-statement}

Siga estos pasos para descargar una declaración de software.

1. Pase el ratón sobre la aplicación registrada de la que quiera descargar el extracto de software de la lista de **Aplicaciones registradas**.

1. Seleccione **Descargar**.

   ![Descargar una declaración de software](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-download-software-statement-button.png)

   *Descargar una declaración de software*

### Esquemas personalizados {#custom-schemes}

Esta pestaña muestra una lista de esquemas personalizados. Para obtener más información relacionada con el uso de esquemas personalizados, consulte [Registro de aplicaciones de iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

Puede realizar los siguientes cambios en las combinaciones personalizadas:

* [Generar un nuevo esquema personalizado](#generate-custom-schemes)

#### Generar nuevo esquema personalizado {#generate-custom-schemes}

Siga estos pasos para generar un nuevo esquema personalizado.

1. Seleccione **Generar nuevo esquema personalizado**.

   ![Generar un nuevo esquema personalizado](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-custom-scheme-button.png)

   *Generar un nuevo esquema personalizado*

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar el nuevo esquema personalizado que aparece en la sección **Esquemas personalizados**, continúe con el flujo [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Esquemas personalizados heredados {#inherited-custom-schemes}

Las empresas de medios definen estos esquemas personalizados en su propio nivel. Todos los canales asociados con la misma empresa de medios pueden utilizar estos esquemas personalizados.

![Esquemas personalizados heredados](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-custom-schemes-panel-view.png)

*Esquemas personalizados heredados*

## Añadir nuevo canal {#add-new-channel}

Siga estos pasos para agregar un canal nuevo.

1. Seleccione la ficha **Canales** en el panel izquierdo.

1. Seleccione **Agregar nuevo canal** en la esquina superior derecha de la sección **Canales**.

   ![Agregar un canal nuevo](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-channel-button.png)

   *Agregar un canal nuevo*

1. Seleccione **ID de programador** en el menú desplegable del cuadro de diálogo **Nuevo canal**.

1. Escriba un identificador único en **ID de canal**.

1. Escriba el nombre de marca del canal usado con fines comerciales en **Nombre para mostrar**.

1. Seleccione **Agregar canal**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar el nuevo canal enumerado en la sección **Canales**, continúa con el flujo [revisar e insertar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).
