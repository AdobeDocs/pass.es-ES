---
title: Programadores
description: Obtenga información sobre los programadores y sus configuraciones en el tablero de TVE.
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Programadores {#programmers}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La sección **Programmers** del Tablero de TVE le permite ver y administrar la configuración de [programmers](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) vinculados a las autorizaciones de su cuenta. También puede [agregar un nuevo programador](#add-new-programmer) según sus necesidades.

La ficha **Programadores** del panel izquierdo muestra una lista de los programadores existentes con los siguientes detalles:

* **ID de programador**: Identificador de empresa de medios dentro del sistema.
* **Canales**: El número de canales asociados vinculados a un programador.

![Lista de programadores existentes](../assets/tve-dashboard/new-tve-dashboard/programmers/programmers-list-view.png)

*Lista de programadores existentes*

Escriba el nombre del programador en la barra **Buscar** situada encima de la lista para obtener más información sobre un programador.

## Administrar configuraciones del programador {#manage-programmer-conf}

Siga estos pasos para administrar varias configuraciones de un programador específico.

1. Seleccione la ficha **Programadores** en el panel izquierdo.
1. Seleccione un programador de la lista.
1. Seleccione una de las siguientes pestañas para ver y editar la configuración correspondiente del programador seleccionado:

   * [Canales](#channels)
   * [Certificados](#certificates)
   * [Aplicaciones registradas](#registered-applications)
   * [Esquemas personalizados](#custom-schemes)

   ![Configuración del programador](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-tabs-view.png)

   *Configuración del programador*

>[!IMPORTANT]
>
> Ver [cambios de revisión y inserción](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) para obtener más información sobre cómo activar los cambios de configuración.

### Canales {#channels}

Esta pestaña muestra una lista de canales vinculados a un programador actual. Seleccione un canal específico de esta lista para acceder a información detallada en la sección [Canales](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md).

Para agregar un canal nuevo para el programador seleccionado, seleccione **Agregar canal nuevo** en la esquina superior derecha de la sección **Canales disponibles**. Aprenda [cómo agregar un canal nuevo](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#add-new-channel).

![Agregar un canal nuevo](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-channel-button.png)

*Agregar un canal nuevo*

### Certificados {#certificates}

Esta ficha muestra una lista de [certificados disponibles](#available-certificates) utilizados en los flujos de cifrado de metadatos de usuario. Muestra detalles acerca de cada certificado, incluidos los siguientes:

* El estado (independientemente de si está habilitado para el uso de **cifrado de metadatos de usuario** o no)
* Número de serie
* Nombre de la organización emisora
* Nombre de la organización a la que pertenece
* Fecha de emisión
* Fecha de caducidad
* Menú desplegable para cifrar los metadatos del usuario (si selecciona **Sí**, el certificado cifrará la información confidencial del usuario, como los valores del código postal).

#### Certificados disponibles {#available-certificates}

Estos certificados sirven como claves privadas o públicas y se utilizan para el cifrado de metadatos de usuarios. Todos los canales asociados con la misma empresa de medios pueden utilizar estos certificados.

Puede realizar los siguientes cambios en los certificados disponibles:

* [Añadir nuevo certificado](#add-new-certificate)
* [Eliminar certificado](#delete-certificate)

##### Añadir nuevo certificado {#add-new-certificate}

Siga estos pasos para agregar un nuevo certificado.

1. Seleccione **Agregar nuevo certificado** en la esquina superior derecha de la sección **Certificados disponibles**.

   ![Agregar nuevo certificado](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-certificate-button.png)

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

   ![Quitar el certificado seleccionado](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-remove-certificate-button.png)

   *Quitar el certificado seleccionado*

1. Seleccione **Eliminar** en el cuadro de diálogo **Eliminar certificado**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. El certificado se eliminará de la sección **Certificados disponibles** solo después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

### Aplicaciones registradas {#registered-applications}

Esta pestaña muestra una lista de las aplicaciones registradas. Para obtener más información relacionada con el uso de aplicaciones registradas, consulte la [descripción general del registro dinámico de clientes](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

Puede realizar las siguientes acciones con las aplicaciones registradas:

* [Agregar una nueva aplicación registrada](#add-registered-applications)
* [Descargar una declaración de software](#download-software-statement)

#### Agregar nueva aplicación registrada {#add-registered-applications}

Siga estos pasos para agregar una nueva aplicación registrada.

1. Seleccione **Agregar nueva aplicación** en la esquina superior derecha de la sección **Aplicaciones registradas**.

   ![Agregar nueva aplicación](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-application-button.png)

   *Agregar nueva aplicación*

1. Seleccione **Asignado al canal** en el menú desplegable del cuadro de diálogo **Nueva aplicación**.

   >[!IMPORTANT]
   >
   > Se recomienda crear aplicaciones registradas con permisos más específicos y limitados para mejorar la seguridad y evitar el acceso no autorizado. Por lo tanto, cuando cree aplicaciones registradas, considere la posibilidad de utilizar opciones más reducidas para el `channel` asignado.

1. Seleccione **Plataformas** en el menú desplegable.

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

1. Pase el ratón sobre la aplicación registrada donde quiera descargar el extracto de software de la lista de **Aplicaciones registradas**.

1. Seleccione **Descargar**.

   ![Descargar una declaración de software](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-download-software-statement-button.png)

   *Descargar una declaración de software*


### Esquemas personalizados {#custom-schemes}

Esta pestaña muestra una lista de esquemas personalizados. Para obtener más información relacionada con el uso de esquemas personalizados, consulte [Registro de aplicaciones de iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

Puede realizar los siguientes cambios en las combinaciones personalizadas:

* [Generar un nuevo esquema personalizado](#generate-custom-schemes)

#### Generar nuevo esquema personalizado {#generate-custom-schemes}

Siga estos pasos para generar un nuevo esquema personalizado.

1. Seleccione **Generar nuevo esquema personalizado**.

   ![Generar un nuevo esquema personalizado](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-custom-scheme-button.png)

   *Generar un nuevo esquema personalizado*

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar el nuevo esquema personalizado que aparece en la sección **Esquemas personalizados**, continúe con el flujo [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

## Agregar nuevo programador {#add-new-programmer}

Siga estos pasos para agregar una nueva entidad de programador.

1. Seleccione la ficha **Programadores** en el panel izquierdo.

1. Seleccione **Agregar nuevo programador** en la esquina superior derecha de la sección **Programadores**.

   ![Agregar nuevo programador](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-programmer-button.png)

   *Agregar nuevo programador*

1. Escriba el identificador de la empresa multimedia en **ID de programador** en el cuadro de diálogo **Nuevo programador**.

1. Escriba un nombre de marca comercial que desee que se muestre en la consola bajo **Nombre para mostrar**.

1. Seleccione **Agregar programador**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar el nuevo programador que aparece en la sección **Programadores**, continúa con el flujo [revisar e insertar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).
