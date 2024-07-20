---
title: Programadores
description: Obtenga información sobre los programadores y sus configuraciones en el tablero de TVE.
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: c2dcea9e4170a3e10654bcd3f8d2f5cdb82c9603
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Programadores {#programmers}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La sección **Programmers** del Tablero de TVE le permite ver y administrar la configuración de [programmers](/help/authentication/glossary.md#programmer) vinculados a las autorizaciones de su cuenta. También puede [agregar un nuevo programador](#add-new-programmer) según sus necesidades.

La ficha **Programadores** del panel izquierdo muestra una lista de los programadores existentes con los siguientes detalles:

* **ID de programador**: Identificador de empresa de medios dentro del sistema.
* **Canales**: El número de canales asociados vinculados a un programador.

![Lista de programadores existentes](assets/programmers-list.png)

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

   ![Configuración del programador](assets/programmer-settings.png)

   *Configuración del programador*

>[!IMPORTANT]
>
> Ver [cambios de revisión y inserción](/help/authentication/tve-dashboard-review-push-changes.md) para obtener más información sobre cómo activar los cambios de configuración.

### Canales {#channels}

Esta pestaña muestra una lista de canales vinculados a un programador actual. Seleccione un canal específico de esta lista para acceder a información detallada en la sección [Canales](/help/authentication/tve-dashboard-channels.md).

Para agregar un canal nuevo para el programador seleccionado, seleccione **Agregar canal nuevo** en la esquina superior derecha de la sección **Canales disponibles**. Aprenda [cómo agregar un canal nuevo](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Agregar un canal nuevo](assets/programmers-channels.png)

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

   ![Agregar nuevo certificado](assets/programmer-add-new-certificate.png)

   *Agregar nuevo certificado*

1. Pegue la clave pública del certificado en el cuadro de diálogo **Nuevo certificado**.
1. Seleccione **Agregar certificado**.

   Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar el nuevo certificado que aparece en la sección **Certificados disponibles**, continúe con el flujo [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

1. Busque el nuevo certificado en la lista de **Certificados disponibles**.

   >[!IMPORTANT]
   >
   > Asegúrese de que los sistemas estén actualizados y listos para utilizar el nuevo certificado.

1. Seleccione **Sí** de **Se usó para cifrar los metadatos del usuario** en el menú desplegable para activar un nuevo certificado.

##### Eliminar certificado {#delete-certificate}

Siga estos pasos para eliminar un certificado.

1. Pase el ratón sobre el certificado que quiera eliminar de la lista de **certificados disponibles**.
1. Seleccione **Quitar**.

   ![Quitar el certificado seleccionado](assets/programmer-remove-certificate.png)

   *Quitar el certificado seleccionado*

1. Seleccione **Eliminar** en el cuadro de diálogo **Eliminar certificado**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. El certificado se eliminará de la sección **Certificados disponibles** solo después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

### Aplicaciones registradas {#registered-applications}

Esta pestaña proporciona una lista de los registros de aplicaciones. Vea [Dynamic client registration management](/help/authentication/dynamic-client-registration-management.md) para obtener más información.

### Esquemas personalizados {#custom-schemes}

Esta pestaña muestra una lista de esquemas personalizados. Vea [Registro de aplicaciones de iOS/tvOS](/help/authentication/iostvos-application-registration.md) y [Administración dinámica de registro de clientes](/help/authentication/dynamic-client-registration-management.md) para obtener más información.

## Agregar nuevo programador {#add-new-programmer}

Siga estos pasos para agregar una nueva entidad de programador.

1. Seleccione la ficha **Programadores** en el panel izquierdo.
1. Seleccione **Agregar nuevo programador** en la esquina superior derecha de la sección **Programadores**.

   ![Agregar nuevo programador](assets/add-new-programmer.png)

   *Agregar nuevo programador*

1. Escriba el identificador de la empresa multimedia en **ID de programador** en el cuadro de diálogo **Nuevo programador**.
1. Escriba un nombre de marca comercial que desee que se muestre en la consola bajo **Nombre para mostrar**.
1. Seleccione **Agregar programador**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para usar el nuevo programador que aparece en la sección **Programadores**, continúa con el flujo [revisar e insertar cambios](/help/authentication/tve-dashboard-review-push-changes.md).
