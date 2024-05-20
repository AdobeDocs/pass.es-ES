---
title: Programadores
description: Obtenga información sobre los programadores y sus configuraciones en el tablero de TVE.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Programadores {#programmers}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El **Programadores** del panel de TVE permite ver y gestionar la configuración de [programadores](/help/authentication/glossary.md#programmer) vinculado a sus derechos de cuenta. También puede [agregar un nuevo programador](#add-new-programmer) según sus necesidades.

El **Programadores** en el panel izquierdo se muestra una lista de los programadores existentes con los siguientes detalles:

* **ID de programador**: identificador de empresa de medios dentro del sistema.
* **Canales**: el número de canales asociados vinculados a un programador.

![Lista de programadores existentes](assets/programmers-list.png)

*Lista de programadores existentes*

Escriba el nombre del programador en la **Buscar** situado encima de la lista para obtener más información sobre un programador.

## Administrar configuraciones del programador {#manage-programmer-conf}

Siga estos pasos para administrar varias configuraciones de un programador específico.

1. Seleccione el **Programadores** en el panel izquierdo.
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
> Ver [Revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md) para obtener más información sobre cómo activar los cambios de configuración.

### Canales {#channels}

Esta pestaña muestra una lista de canales vinculados a un programador actual. Seleccione un canal específico de esta lista para acceder a información detallada en la [Canales](/help/authentication/tve-dashboard-channels.md) sección.

Para añadir un canal nuevo para el programador seleccionado, seleccione **Añadir nuevo canal** desde la esquina superior derecha de **Canales disponibles** sección. Aprender [cómo añadir un canal nuevo](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Añadir un canal nuevo](assets/programmers-channels.png)

*Añadir un canal nuevo*

### Certificados {#certificates}

Esta pestaña muestra una lista de [certificados disponibles](#available-certificates) se utiliza en los flujos de cifrado de metadatos de usuario. Muestra detalles acerca de cada certificado, incluidos los siguientes:

* El estado (si está habilitado para **cifrado de metadatos de usuario** uso o no)
* Número de serie
* Nombre de la organización emisora
* Nombre de la organización a la que pertenece
* Fecha de emisión
* Fecha de caducidad
* Un menú desplegable para cifrar los metadatos del usuario (si selecciona **Sí**, el certificado cifrará la información confidencial del usuario, como los valores del código postal).

#### Certificados disponibles {#available-certificates}

Estos certificados sirven como claves privadas o públicas y se utilizan para el cifrado de metadatos de usuarios. Todos los canales asociados con la misma empresa de medios pueden utilizar estos certificados.

Puede realizar los siguientes cambios en los certificados disponibles:

* [Añadir nuevo certificado](#add-new-certificate)
* [Eliminar certificado](#delete-certificate)

##### Añadir nuevo certificado {#add-new-certificate}

Siga estos pasos para agregar un nuevo certificado.

1. Seleccionar **Añadir nuevo certificado** en la esquina superior derecha de la **Certificados disponibles** sección.

   ![Añadir un nuevo certificado](assets/programmer-add-new-certificate.png)

   *Añadir un nuevo certificado*

1. Pegue la clave pública del certificado en la **Nuevo certificado** Cuadro de diálogo.
1. Seleccionar **Añadir certificado**.

   Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para utilizar el nuevo certificado que aparece en la **Certificados disponibles** , continúe con la sección [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md) Flujo.

1. Busque el nuevo certificado en la lista de **Certificados disponibles**.

   >[!IMPORTANT]
   >
   > Asegúrese de que los sistemas estén actualizados y listos para utilizar el nuevo certificado.

1. Seleccionar **Sí** de **Se utiliza para cifrar los metadatos de usuario** menú desplegable para activar un nuevo certificado.

##### Eliminar certificado {#delete-certificate}

Siga estos pasos para eliminar un certificado.

1. Pase el ratón sobre el certificado que desea eliminar de la lista de **Certificados disponibles**.
1. Seleccionar **Eliminar**.

   ![Quitar el certificado seleccionado](assets/programmer-remove-certificate.png)

   *Quitar el certificado seleccionado*

1. Seleccionar **Eliminar** en el **Eliminar certificado** Cuadro de diálogo.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. El certificado se eliminará del **Certificados disponibles** solo después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

### Aplicaciones registradas {#registered-applications}

Esta pestaña proporciona una lista de los registros de aplicaciones. Ver [Dynamic client registration management](/help/authentication/dynamic-client-registration-management.md), para obtener más información.

### Esquemas personalizados {#custom-schemes}

Esta pestaña muestra una lista de esquemas personalizados. Ver [Registro de aplicaciones de iOS/tvOS](/help/authentication/iostvos-application-registration.md) y [Dynamic client registration management](/help/authentication/dynamic-client-registration-management.md), para obtener más información.

## Agregar nuevo programador {#add-new-programmer}

Siga estos pasos para agregar una nueva entidad de programador.

1. Seleccione el **Programadores** en el panel izquierdo.
1. Seleccionar **Agregar nuevo programador** en la esquina superior derecha de la **Programadores** sección.

   ![Añadir un nuevo programador](assets/add-new-programmer.png)

   *Añadir un nuevo programador*

1. Escriba el identificador de la empresa de medios en **ID de programador** en el **Nuevo programador** Cuadro de diálogo.
1. Escriba un nombre de marca comercial que desee que se muestre en la consola en **Nombre para mostrar**.
1. Seleccionar **Añadir programador**.

Se ha creado un nuevo cambio de configuración y está listo para la actualización del servidor. Para utilizar el nuevo programador que aparece en la **Programadores** , continúe con la sección [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md) Flujo.

