---
title: Integraciones de tableros de TVE
description: Conozca las integraciones entre sus canales y MVPD y cómo administrar las integraciones.
exl-id: 0add340b-120c-4e82-8e3c-6c190d77cf7e
source-git-commit: c2dcea9e4170a3e10654bcd3f8d2f5cdb82c9603
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
* Nombre para mostrar de MVPD e ID de MVPD

![Lista de integraciones existentes](assets/integrations-list.png)

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
> Ver [cambios de revisión y inserción](/help/authentication/tve-dashboard-review-push-changes.md) para obtener más información sobre cómo activar los cambios de configuración.

### Selección de extremo {#endpoint-selection}

Esta sección le permite elegir los puntos finales de la MVPD utilizada para los flujos de autenticación, autorización y cierre de sesión en los menús desplegables respectivos.

![Puntos finales para flujos de autenticación, autorización y cierre de sesión](assets/endpoint-selection.png)

*Puntos finales para flujos de autenticación, autorización y cierre de sesión*

>[!NOTE]
>
>Las MVPD pueden proporcionar uno o varios extremos para cada flujo. Al integrar un nuevo canal, la MVPD debe especificar su punto final preferido para cada flujo.

>[!IMPORTANT]
>
>Cualquier cambio en los extremos afectará al comportamiento general de una integración. Estos cambios solo deben implementarse después de recibir la confirmación de la MVPD.

### Configuración de plataforma {#platform-settings}

Esta sección le permite ver y editar la configuración de la integración en todas las [plataformas](/help/authentication/tve-dashboard-reports.md#platforms). Puede cambiar esta configuración en función de plataformas individuales. Por ejemplo, puede ajustar la duración del TTL de autorización en Android mientras mantiene un valor predeterminado para otra plataforma.

Cada propiedad en la configuración de la plataforma hereda un valor predeterminado establecido por la MVPD, pero se puede ajustar si es necesario.

>[!IMPORTANT]
>
>Se requiere un acuerdo con la MVPD para determinar los valores establecidos para cada propiedad en la configuración de la plataforma.

>[!IMPORTANT]
>
> La herencia de la configuración sigue una cadena que comienza desde la configuración de MVPD (que es la más general), luego el punto final de MVPD, la integración, la categoría de plataforma y la plataforma (que contiene el valor más específico).

**Configuración de plataforma** se usa para anular la configuración de cada nivel en la cadena de herencia. Los niveles disponibles en la cadena se agrupan de la siguiente manera:

* **Valor predeterminado para todos**: establezca valores para propiedades aplicables universalmente en todas las plataformas si no se definen valores de plataforma específicos, independientemente de las implementaciones del programador.

* **Dispositivos de escritorio**: establezca valores para las propiedades aplicables a todos los equipos de escritorio y portátiles, independientemente del método de programación (SDK de JS o API de REST).

* **Dispositivos móviles**: establezca valores para las propiedades aplicables a todos los dispositivos móviles, incluidos **iOS**, **Android** y otros, independientemente del método de programación (SDK o API REST).

* **Dispositivos conectados a TV**: establezca valores para las propiedades aplicables a todos los dispositivos conectados a TV, incluidos **tvOS**, **Roku**, **FireTV** y otros, independientemente del método de programación (SDK o API REST).

* **Dispositivos no identificados**: establezca valores para las propiedades aplicables a todos los dispositivos en los que el mecanismo actual no pueda identificar la plataforma con precisión. En estos casos, aplique las reglas más restrictivas definidas por la MVPD.

  ![Categoría de plataformas y sus dispositivos](assets/platform-settings.png)

  *Categoría de plataformas y sus dispositivos*

Seleccionar Icono <img alt= "icono de cadena de herencia" src="./assets/multiple-icon.svg" width="25"> ubicado a la derecha de cada propiedad para explorar las propiedades utilizadas para cada nivel de herencia descrito anteriormente.

#### Flujos empresariales más utilizados {#most-used-flows}

La sección **Configuración de plataforma** ofrece una serie de propiedades utilizadas en diferentes flujos comerciales. Las propiedades reales pueden variar según las MVPD seleccionadas en la integración específica. A continuación, se muestran los flujos más utilizados:

**AuthN TTL y AuthZ TTL en todas las plataformas**

>[!IMPORTANT]
>
>Los valores TTL de autenticación (AuthN) y TTL de autorización (AuthZ) deben alinearse de manera consistente con la configuración de MVPD.

Siga estos pasos para cambiar el TTL de autenticación y autorización en todas las plataformas para una integración específica.

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.
1. Seleccione la integración para la que desea cambiar los valores de AuthN TTL y AuthZ TTL.
1. Vaya a la sección **Configuración de plataforma**.

1. Seleccione la ficha **Opción predeterminada para todos** en **Configuración de la plataforma**.

   >[!NOTE]
   >
   >Si desea cambiar la duración de **AuthN TTL** y **AuthZ TTL** para una categoría de plataforma o una plataforma específica, seleccione la plataforma según corresponda.

   ![Cambiar la duración del TTL de AuthN TTL en todas las plataformas](assets/authn-ttl-authz-ttl-for-all-platform.png)

   *Cambiar la duración del TTL de AuthN TTL en todas las plataformas*

   **A.** Propiedad TTL AuthN **B.** Propiedad TTL AuthZ

1. Seleccione las flechas arriba y abajo para ajustar la duración del número de días, horas, minutos y segundos en las propiedades **AuthN TTL** y **AuthZ TTL**.

La duración de **AuthN TTL** y **AuthZ TTL** en todas las plataformas se actualizará solo después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

**Habilitar SSO de plataforma**

>[!IMPORTANT]
>
>La propiedad **Habilitar inicio de sesión único** se admite exclusivamente en las plataformas *iOS, tvOS, Roku y FireTV*. Solo es aplicable a integraciones con MVPD que admiten el inicio de sesión único para estas plataformas.

Siga estos pasos para habilitar o deshabilitar el SSO para una integración y plataforma específicas.

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.
1. Seleccione la integración para la que desea habilitar o deshabilitar el inicio de sesión único.

1. Vaya a la sección **Configuración de plataforma**.

1. Seleccione una plataforma o categoría de plataformas específica para la que desee habilitar el inicio de sesión único en **Configuración de plataforma**.

   ![Habilitar el inicio de sesión único para una plataforma específica](assets/single-sign-on.png)

   *Habilitar el inicio de sesión único para una plataforma específica*

   **A.** Propiedad de inicio de sesión único **B.** Aplicar propiedad de permisos de plataforma

1. Seleccione **Sí** para habilitar o **No** para deshabilitar desde el menú desplegable **Habilitar inicio de sesión único**.

1. Seleccione **Sí** para habilitar o **No** para deshabilitar en el menú desplegable **Aplicar permisos de plataforma**.

   La propiedad **Aplicar permiso de plataforma** controla si se respeta la decisión del usuario de **Permitir** o **Denegar** el acceso de plataforma a su suscripción al proveedor de TV.

   Por ejemplo, si están habilitados **Habilitar el inicio de sesión único** y **Aplicar permisos de plataforma**, y el usuario opta por denegar el acceso de plataforma a su suscripción al proveedor de TV, la aplicación (canal) correspondiente no podrá usar el token de autenticación de Adobe Pass obtenido por otra aplicación (canal).

La propiedad **Inicio de sesión único** de una plataforma seleccionada se habilitará o deshabilitará solo después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

**Habilitar la autenticación basada en el inicio**

Siga estos pasos para habilitar o deshabilitar la autenticación basada en el hogar para MVPD basadas en OAuth2.

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.
1. Seleccione la integración para la que desea habilitar o deshabilitar la autenticación basada en el inicio.
1. Vaya a la sección **Configuración de plataforma**.
1. Seleccione una plataforma o categoría de plataformas específica para la cual desee habilitar la autenticación basada en el inicio en **Configuración de la plataforma**.

   ![Habilitar la autenticación basada en el inicio para una plataforma específica](assets/attempt-hba.png)

   *Habilitar la autenticación basada en el inicio para una plataforma específica*

   **A.** Intento de propiedad HBA **B.** Propiedad HBA AuthN TTL

1. Seleccione **Sí** para habilitar y **No** para deshabilitar en el menú desplegable **Intentar HBA**.

>[!IMPORTANT]
>
>Se debe evitar cambiar la duración de la propiedad **HBA AuthN TTL**. Podría provocar errores inesperados en el proceso de autorización.

La propiedad **Intentar HBA** para una MVPD específica se habilitará o deshabilitará solo después de [revisar e insertar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

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

**Recursos máximos de comprobación preliminar** agregará un nuevo atributo en el que se puede especificar el límite acordado con la MVPD.

![Agregar propiedad de recursos máximos de comprobación preliminar](assets/preflight-max-resources.png)

*Agregar propiedad de recursos máximos de comprobación preliminar*

La propiedad **Recursos máximos de comprobación preliminar** se agregará solo después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

**Cambiar el nombre para mostrar de MVPD o la URL del logotipo**

Para las aplicaciones de programador que no desean crear su selector de MVPD y que, en su lugar, dependen de las configuraciones proporcionadas, puede navegar hasta **Agregar más propiedades** y seleccionar **Nombre para mostrar** o **URL del logotipo** para agregar el nombre para mostrar o las URL del logotipo necesarios para cada MVPD desde el menú de opciones.

Se pueden utilizar valores diferentes para estas propiedades para la misma MVPD según la plataforma del dispositivo y la experiencia de usuario deseada.

![Agregar propiedad de nombre para mostrar o URL de logotipo](assets/displayname-logourl.png)

*Agregar propiedad de nombre para mostrar o URL de logotipo*

La propiedad **Display Name** o **Logo URL** solo se agregará después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

**Solicitar un nuevo flujo de autenticación al cambiar de aplicación (canal)**

Si desea forzar una nueva autenticación cuando los usuarios cambien entre aplicaciones. En ese caso, puede navegar hasta **Agregar más propiedades** y seleccionar la propiedad **Auth per Aggregator**.

Al agregar **Autenticación por agregador**, se interrumpe de forma efectiva el inicio de sesión único en el canal correspondiente.

![Agregar autenticación por propiedad de agregador](assets/auth-per-aggregator.png)

*Agregar autenticación por propiedad de agregador*

La propiedad **Auth per Aggregator** solo se agregará después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

Una vez agregado, seleccione **Yes** para habilitar la propiedad **Auth per Aggregator** para una integración seleccionada.

#### Eliminar propiedades {#delete-properties}

Seleccionar Icono <img alt= "botón eliminar propiedad" src="./assets/delete-icon.svg" width="25"> ubicado a la derecha de cada propiedad para eliminar las propiedades que ya no son necesarias.

>[!NOTE]
>
>Algunas propiedades no se pueden eliminar porque son requisitos obligatorios para la MVPD seleccionada.

La propiedad se eliminará de la sección **Configuración de plataforma** solo después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

### Metadatos del usuario {#user-metadata}

Esta sección le permite actualizar la configuración de cada parámetro de metadatos de usuario compartido por MVPD.

>[!NOTE]
>
>Cada MVPD puede compartir parámetros diferentes. Para obtener más información sobre los parámetros que un MVPD específico puede compartir, póngase en contacto con su representante de Adobe.

La sección de metadatos del usuario muestra las siguientes columnas:

**Clave**: Representa los parámetros de metadatos de usuario reales que se utilizarán en la API para extraer valores.

**Descripción**: proporciona una breve descripción de cada parámetro de metadatos de usuario.

**Cifrado**: Esta columna le permite habilitar o deshabilitar parámetros en la API al seleccionar **Sí** o **No** en el menú desplegable, respectivamente. Si se elige **Yes**, el valor del parámetro se cifrará en la API. El cifrado se realiza mediante un certificado definido por un ámbito de **metadatos de usuario**.

>[!TIP]
>
>
> Asegúrese siempre de que el parámetro **ZIP** esté cifrado.

Obtenga más información acerca de los certificados disponibles en las secciones [Programadores](/help/authentication/tve-dashboard-programmers.md#available-certificates) y [Canales](/help/authentication/tve-dashboard-channels.md#available-certificates).

**Habilitado**: esta columna le permite habilitar o deshabilitar los parámetros de la API al seleccionar **Sí** o **No** respectivamente en el menú desplegable.

![Parámetros disponibles para los metadatos del usuario](assets/user-metadata.png)

*Parámetros disponibles para los metadatos del usuario*

## Crear nueva integración {#create-new-integration}

Para crear una nueva integración con una nueva MVPD en la configuración actual, siga estos pasos:

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.
1. Seleccione **Crear nueva integración** en la parte superior derecha de la sección **Integraciones**.

   ![Crear una nueva integración](assets/create-new-integration.png)

   *Crear una nueva integración*

   Se muestran las siguientes secciones:

   **Seleccionar canal y MVPD**

   Seleccione un **canal** del menú desplegable **Seleccionar canal** para agregar una nueva integración. Una vez que haya seleccionado el canal, seleccione el **MVPD** necesario del menú desplegable **Seleccionar MVPD** que se integrará con el canal seleccionado.

   ![Seleccionar canal y MVPD](assets/select-channel-mvpd.png)

   *Seleccionar canal y MVPD*

   **Seleccionar extremos**

   Después de seleccionar la MVPD requerida, la sección **Seleccionar extremo** se rellenará previamente con los extremos predeterminados configurados para esa MVPD en particular.

   >[!IMPORTANT]
   >
   >No cambie los extremos predeterminados en ningún flujo a menos que lo indique específicamente la MVPD.

   ![Seleccionar extremos ](assets/select-endpoints.png)

   *Seleccionar extremos*

   **Información adicional**

   Esta sección incluye varias propiedades que deben configurarse para la MVPD seleccionada en la sección **Seleccionar canal y MVPD**.

   >[!NOTE]
   >
   > Las propiedades reales pueden diferir según las MVPD seleccionadas en la sección **Seleccionar canal y MVPD**.

   Por ejemplo, puede editar **AuthN TTL** o **ID de socio** (ID de canal) con fines de promoción conjunta de marca en la página de inicio de sesión de MVPD en la siguiente imagen.

   ![Editar información adicional](assets/additional-information.png)

   *Editar información adicional*

   Seleccione **Guardar integración** en la parte superior derecha de la sección **Crear nueva integración**.

Se creará una nueva integración solamente después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).


## Deshabilitar integración {#disable-integratgion}

Para desactivar una integración, siga estos pasos:

1. Seleccione la pestaña **Integraciones** en el panel izquierdo.
1. Seleccione la integración que desee desactivar.
1. Desactive la opción disponible en la parte superior derecha de la integración seleccionada.

   ![Deshabilitar integración](assets/disable-integration.png)

   *Deshabilitar integración*

La integración se deshabilitará solo después de [revisar y enviar cambios](/help/authentication/tve-dashboard-review-push-changes.md).

Una vez deshabilitada la integración, los usuarios finales perderán la capacidad de autenticarse o autorizar con la MVPD específica.
