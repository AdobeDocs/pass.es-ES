---
title: Integraciones de tableros de TVE
description: Conozca las integraciones entre sus canales y MVPD y cómo administrar las integraciones.
exl-id: 0add340b-120c-4e82-8e3c-6c190d77cf7e
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '2093'
ht-degree: 0%

---

# Integraciones

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La sección **Integraciones** del panel de TVE le permite ver y administrar la configuración de las integraciones entre sus canales y MVPD. También puede [crear una nueva integración](#create-new-integration) según sus necesidades.

La pestaña **Integraciones** del panel izquierdo muestra una lista de integraciones existentes con los siguientes detalles:

* Estado que indica si la integración está activa o inactiva en este momento
* Integración que vincula canales específicos con MVPD respectivos
* Nombre del canal con ID de canal
* Nombre para mostrar y MVPD ID de MVPD

![Lista de integraciones existentes](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integrations-list.png)

*Lista de integraciones existentes*

Escriba el nombre del canal o MVPD en la barra **Buscar** situada encima de la lista para obtener más información acerca de la integración.

## Administrar configuraciones de integración {#manage-integration-conf}

Siga estos pasos para administrar una integración específica.

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.
1. Seleccione una integración de la lista proporcionada para ver y editar varias configuraciones en las secciones siguientes:

   * [Selección de extremo](#endpoint-selection)
   * [Configuración de plataforma](#platform-settings)
   * [Metadatos del usuario](#user-metadata)

>[!IMPORTANT]
>
> Ver [cambios de revisión y inserción](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) para obtener más información sobre cómo activar los cambios de configuración.

### Selección de extremo {#endpoint-selection}

Esta sección le permite elegir los puntos finales de MVPD utilizados para los flujos de autenticación, autorización y cierre de sesión en los menús desplegables respectivos.

![Puntos finales para flujos de autenticación, autorización y cierre de sesión](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-endpoint-selection-panel-view.png)

*Puntos finales para flujos de autenticación, autorización y cierre de sesión*

>[!NOTE]
>
>Las MVPD pueden proporcionar uno o varios extremos para cada flujo. Al integrar un nuevo canal, MVPD debe especificar su punto final preferido para cada flujo.

>[!IMPORTANT]
>
>Cualquier cambio en los extremos afectará al comportamiento general de una integración. Estos cambios solo deben implementarse después de recibir la confirmación de MVPD.

### Configuración de plataforma {#platform-settings}

Esta sección le permite ver y editar la configuración de la integración en todas las [plataformas](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md#platforms). Puede cambiar esta configuración en función de plataformas individuales. Por ejemplo, puede ajustar la duración del TTL de autorización en Android mientras mantiene un valor predeterminado para otra plataforma.

Cada propiedad de la configuración de la plataforma hereda un valor predeterminado establecido por MVPD, pero se puede ajustar si es necesario.

>[!IMPORTANT]
>
>Se requiere un acuerdo con MVPD para determinar los valores establecidos para cada propiedad en la configuración de la plataforma.

>[!IMPORTANT]
>
> La herencia de la configuración sigue una cadena que comienza desde la configuración de MVPD (que es la más general), luego el punto final de MVPD, la integración, la categoría de plataforma y la plataforma (que contiene el valor más específico).

**Configuración de plataforma** se usa para anular la configuración de cada nivel en la cadena de herencia. Los niveles disponibles en la cadena se agrupan de la siguiente manera:

* **Valor predeterminado para todos**: establezca valores para propiedades aplicables universalmente en todas las plataformas si no se definen valores de plataforma específicos, independientemente de las implementaciones del programador.

* **Dispositivos de escritorio**: establezca valores para las propiedades aplicables a todos los equipos de escritorio y portátiles, independientemente del método de programación (SDK JS o API REST).

* **Dispositivos móviles**: establezca valores para las propiedades aplicables a todos los dispositivos móviles, incluidos **iOS**, **Android** y otros, independientemente del método de programación (API de SDK o REST).

* **Dispositivos conectados a TV**: establezca valores para las propiedades aplicables a todos los dispositivos conectados a TV, incluidos **tvOS**, **Roku**, **FireTV** y otros, independientemente del método de programación (API de SDK o REST).

* **Dispositivos no identificados**: establezca valores para las propiedades aplicables a todos los dispositivos en los que el mecanismo actual no pueda identificar la plataforma con precisión. En estos casos, aplique las reglas más restrictivas definidas por MVPD.

  ![Categoría de plataformas y sus dispositivos](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-menu.png)

  *Categoría de plataformas y sus dispositivos*

Seleccionar Icono <img alt= "icono de cadena de herencia" src="/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-inheritance-chain-icon.svg" width="25"> ubicado a la derecha de cada propiedad para explorar las propiedades utilizadas para cada nivel de herencia descrito anteriormente.

#### Flujos empresariales más utilizados {#most-used-flows}

La sección **Configuración de plataforma** ofrece una serie de propiedades utilizadas en diferentes flujos comerciales. Las propiedades reales pueden variar según las MVPD seleccionadas en la integración específica. A continuación, se muestran los flujos más utilizados:

**AuthN TTL y AuthZ TTL en todas las plataformas**

>[!IMPORTANT]
>
>Los valores TTL de autenticación (AuthN) y TTL de autorización (AuthZ) deben alinearse de forma coherente con la configuración de MVPD.

Siga estos pasos para cambiar el TTL de autenticación y autorización en todas las plataformas para una integración específica.

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.

1. Seleccione la integración para la que desea cambiar los valores de AuthN TTL y AuthZ TTL.

1. Vaya a la sección **Configuración de plataforma**.

1. Seleccione la ficha **Opción predeterminada para todos** en **Configuración de la plataforma**.

   >[!NOTE]
   >
   >Si desea cambiar la duración de **AuthN TTL** y **AuthZ TTL** para una categoría de plataforma o una plataforma específica, seleccione la plataforma según corresponda.

   ![Cambiar la duración del TTL de AuthN TTL en todas las plataformas](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-authn-ttl-authz-ttl-properties.png)

   *Cambiar la duración del TTL de AuthN TTL en todas las plataformas*

   **A.** Propiedad TTL AuthN **B.** Propiedad TTL AuthZ

1. Seleccione las flechas arriba y abajo para ajustar la duración del número de días, horas, minutos y segundos en las propiedades **AuthN TTL** y **AuthZ TTL**.

La duración de **AuthN TTL** y **AuthZ TTL** en todas las plataformas se actualizará solo después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**Habilitar SSO de plataforma**

>[!IMPORTANT]
>
>La propiedad **Habilitar inicio de sesión único** se admite exclusivamente en las plataformas *iOS, tvOS, Roku y FireTV*. Solo es aplicable a integraciones con MVPD que admiten el inicio de sesión único para estas plataformas.

Siga estos pasos para habilitar o deshabilitar el SSO para una integración y plataforma específicas.

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.

1. Seleccione la integración para la que desea habilitar o deshabilitar el inicio de sesión único.

1. Vaya a la sección **Configuración de plataforma**.

1. Seleccione una plataforma o categoría de plataformas específica para la que desee habilitar el inicio de sesión único en **Configuración de plataforma**.

   ![Habilitar el inicio de sesión único para una plataforma específica](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-single-sign-on-properties.png)

   *Habilitar el inicio de sesión único para una plataforma específica*

   **A.** Propiedad de inicio de sesión único **B.** Aplicar propiedad de permisos de plataforma

1. Seleccione **Sí** para habilitar o **No** para deshabilitar desde el menú desplegable **Habilitar inicio de sesión único**.

1. Seleccione **Sí** para habilitar o **No** para deshabilitar en el menú desplegable **Aplicar permisos de plataforma**.

   La propiedad **Aplicar permiso de plataforma** controla si se respeta la decisión del usuario de **Permitir** o **Denegar** el acceso de plataforma a su suscripción al proveedor de TV.

   Por ejemplo, si están habilitados **Habilitar el inicio de sesión único** y **Aplicar permisos de plataforma**, y el usuario opta por denegar el acceso de plataforma a su suscripción al proveedor de TV, la aplicación (canal) correspondiente no podrá usar el token de autenticación de Adobe Pass obtenido por otra aplicación (canal).

La propiedad **Inicio de sesión único** de una plataforma seleccionada se habilitará o deshabilitará solo después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**Habilitar la autenticación basada en el inicio**

Siga estos pasos para habilitar o deshabilitar la autenticación basada en el hogar para MVPD basadas en OAuth2.

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.

1. Seleccione la integración para la que desea habilitar o deshabilitar la autenticación basada en el inicio.

1. Vaya a la sección **Configuración de plataforma**.

1. Seleccione una plataforma o categoría de plataformas específica para la cual desee habilitar la autenticación basada en el inicio en **Configuración de la plataforma**.

   ![Habilitar la autenticación basada en el inicio para una plataforma específica](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-attempt-hba-properties.png)

   *Habilitar la autenticación basada en el inicio para una plataforma específica*

   **A.** Intento de propiedad HBA **B.** Propiedad HBA AuthN TTL

1. Seleccione **Sí** para habilitar y **No** para deshabilitar en el menú desplegable **Intentar HBA**.

>[!IMPORTANT]
>
>Se debe evitar cambiar la duración de la propiedad **HBA AuthN TTL**. Podría provocar errores inesperados en el proceso de autorización.

La propiedad **Intentar HBA** para un MVPD específico solo se habilitará o deshabilitará después de [revisar e insertar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Añadir más propiedades {#add-more-properties}

**Agregar más propiedades** permite la flexibilidad de incluir propiedades específicas adicionales para las integraciones, especialmente para los flujos menos comunes.

Puede añadir estas propiedades:

* Para todas las plataformas, seleccione **Predeterminado para todas** en la ficha de la izquierda.
* Para una categoría de plataforma, seleccione la ficha **Dispositivos de escritorio**, **Dispositivos móviles** o **Dispositivos conectados a TV** a la izquierda.
* Para un dispositivo específico, selecciona las pestañas **iOS**, **Android**, **tvOS**, **Roku** o **FireTV** a la izquierda.

A continuación, se muestran algunos ejemplos de diferentes flujos que se pueden habilitar añadiendo estas propiedades:

**Cambiar el número de recursos preautorizados**

La mayoría de las MVPD admiten una llamada authZ de comprobación preliminar que usa hasta 5 ID de recurso de forma predeterminada.
Sin embargo, en los casos en que las MVPD acepten aumentar este límite, puede navegar hasta **Agregar más propiedades** y seleccionar **Recursos máximos de comprobación preliminar** en el menú de opciones.

**Recursos máximos de comprobación preliminar** agregará un nuevo atributo donde se puede especificar el límite acordado con MVPD.

![Agregar propiedad de recursos máximos de comprobación preliminar](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-preflight-max-resources-properties.png)

*Agregar propiedad de recursos máximos de comprobación preliminar*

La propiedad **Recursos máximos de comprobación preliminar** se agregará solo después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**Cambiar el nombre para mostrar o la URL del logotipo de MVPD**

Para las aplicaciones de programador que no deseen crear su selector de MVPD y que, en su lugar, utilicen las configuraciones proporcionadas, puede navegar hasta **Agregar más propiedades** y seleccionar **Nombre para mostrar** o **URL del logotipo** para agregar el nombre para mostrar o las URL del logotipo necesarios para cada MVPD desde el menú de opciones.

Se pueden utilizar valores diferentes para estas propiedades para la misma MVPD según la plataforma del dispositivo y la experiencia del usuario deseada.

![Agregar propiedad de nombre para mostrar o URL de logotipo](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-display-name-logo-url-properties.png)

*Agregar propiedad de nombre para mostrar o URL de logotipo*

La propiedad **Display Name** o **Logo URL** solo se agregará después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**Solicitar un nuevo flujo de autenticación al cambiar de aplicación (canal)**

Si desea forzar una nueva autenticación cuando los usuarios cambien entre aplicaciones. En ese caso, puede navegar hasta **Agregar más propiedades** y seleccionar la propiedad **Auth per Aggregator**.

Al agregar **Autenticación por agregador**, se interrumpe de forma efectiva el inicio de sesión único en el canal correspondiente.

![Agregar autenticación por propiedad de agregador](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-auth-per-aggregator-properties.png)

*Agregar autenticación por propiedad de agregador*

La propiedad **Auth per Aggregator** solo se agregará después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

Una vez agregado, seleccione **Yes** para habilitar la propiedad **Auth per Aggregator** para una integración seleccionada.

#### Eliminar propiedades {#delete-properties}

Seleccionar Icono <img alt= "botón eliminar propiedad" src="/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-delete-property-icon.svg" width="25"> ubicado a la derecha de cada propiedad para eliminar las propiedades que ya no son necesarias.

>[!NOTE]
>
>Algunas propiedades no se pueden eliminar, ya que son requisitos obligatorios para la MVPD seleccionada.

La propiedad se eliminará de la sección **Configuración de plataforma** solo después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

### Metadatos del usuario {#user-metadata}

Esta sección le permite actualizar la configuración de cada parámetro de metadatos de usuario compartido por MVPD.

>[!NOTE]
>
>Cada MVPD puede compartir diferentes parámetros. Para obtener más información sobre los parámetros que un MVPD específico puede compartir, póngase en contacto con su representante de Adobe.

La sección de metadatos del usuario muestra las siguientes columnas:

**Clave**: Representa los parámetros de metadatos de usuario reales que se utilizarán en la API para extraer valores.

**Descripción**: proporciona una breve descripción de cada parámetro de metadatos de usuario.

**Cifrado**: Esta columna le permite habilitar o deshabilitar parámetros en la API al seleccionar **Sí** o **No** en el menú desplegable, respectivamente. Si se elige **Yes**, el valor del parámetro se cifrará en la API. El cifrado se realiza mediante un certificado definido por un ámbito de **metadatos de usuario**.

>[!TIP]
>
>
> Asegúrese siempre de que el parámetro **ZIP** esté cifrado.

Obtenga más información acerca de los certificados disponibles en las secciones [Programadores](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#available-certificates) y [Canales](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#available-certificates).

**Habilitado**: esta columna le permite habilitar o deshabilitar los parámetros de la API al seleccionar **Sí** o **No** respectivamente en el menú desplegable.

![Parámetros disponibles para los metadatos del usuario](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-user-metadata-panel-view.png)

*Parámetros disponibles para los metadatos del usuario*

## Crear nueva integración {#create-new-integration}

Para crear una nueva integración con un nuevo MVPD en la configuración actual, siga estos pasos:

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.

1. Seleccione **Crear nueva integración** en la parte superior derecha de la sección **Integraciones**.

   ![Crear una nueva integración](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-create-new-integration-button.png)

   *Crear una nueva integración*

   Se muestran las siguientes secciones:

   **Seleccionar canal y MVPD**

   Seleccione un **canal** del menú desplegable **Seleccionar canal** para agregar una nueva integración. Una vez que hayas seleccionado el canal, selecciona el **MVPD** necesario del menú desplegable **Seleccionar MVPD** que se integrará con el canal seleccionado.

   ![Seleccionar canal y MVPD](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-select-channel-and-mvpd-panel-view.png)

   *Seleccionar canal y MVPD*

   **Seleccionar extremos**

   Después de seleccionar el MVPD requerido, la sección **Seleccionar extremo** se rellenará previamente con los extremos predeterminados configurados para ese MVPD en particular.

   >[!IMPORTANT]
   >
   >No cambie los extremos predeterminados en ningún flujo a menos que MVPD lo indique específicamente.

   ![Seleccionar extremos ](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-select-endpoints-panel-view.png)

   *Seleccionar extremos*

   **Información adicional**

   Esta sección incluye varias propiedades que deben configurarse para el MVPD seleccionado en la sección **Seleccionar canal y MVPD**.

   >[!NOTE]
   >
   > Las propiedades reales pueden diferir según las MVPD seleccionadas en la sección **Seleccionar canal y MVPD**.

   Por ejemplo, puede editar **AuthN TTL** o **ID de socio** (ID de canal) con fines de promoción conjunta de marca en la página de inicio de sesión de MVPD en la siguiente imagen.

   ![Editar información adicional](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-additional-information-panel-view.png)

   *Editar información adicional*

   Seleccione **Guardar integración** en la parte superior derecha de la sección **Crear nueva integración**.

Se creará una nueva integración solamente después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).


## Deshabilitar integración {#disable-integration}

Para desactivar una integración, siga estos pasos:

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.

1. Seleccione la integración que desee desactivar.

1. Desactive la opción disponible en la parte superior derecha de la integración seleccionada.

   ![Deshabilitar integración](/help/authentication/assets/tve-dashboard/new-tve-dashboard/integrations/integration-enabled-disabled-button.png)

   *Deshabilitar integración*

La integración se deshabilitará solo después de [revisar y enviar cambios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

Una vez deshabilitada la integración, los usuarios finales perderán la capacidad de autenticarse o autorizar con el MVPD específico.
