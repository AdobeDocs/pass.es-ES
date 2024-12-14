---
title: Guía del usuario del panel de TVE Primetime
description: Guía del usuario del panel de TVE Primetime
exl-id: 6f7f7901-db3a-4c68-ac6a-27082db9240a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '5527'
ht-degree: 0%

---

# Guía del usuario del panel de TVE de Primetime (heredado) {#tve-db-user-guide}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Introducción {#tve-db-intro}

[[!DNL Adobe] Tablero de TVE (Tablero de TVE)](https://console.auth.adobe.com/) es un tablero de autoservicio dirigido a usuarios que trabajan para empresas de medios (programadores) que tienen una relación comercial con el equipo de producto de autenticación de Adobe Pass.

Póngase en contacto con el administrador de cuentas técnico (TAM) para obtener acceso. Para obtener acceso, necesitará que se configuren dos nuevos grupos de usuarios en su organización de Adobe Marketing Cloud:

* Panel de TVE de lectura-escritura: los miembros de este grupo tienen derechos completos en todas las secciones editables del panel
* Panel de TVE de solo lectura: los miembros de este grupo solo tienen derechos de visualización en todo el panel


Antes de profundizar en esta guía del usuario, le recomendamos que revise los siguientes recursos para comprender bien los flujos y las funciones que proporciona el equipo de producto de autenticación de Adobe Pass y familiarizarse con los términos utilizados en el presente documento:

* [Documento técnico de TVE](/help/authentication/kickstart/technical-paper.md)
* [Guía de KickStart del programador](/help/authentication/kickstart/programmer-kickstart-guide.md)
* [Flujo de derecho](/help/authentication/integration-guide-programmers/entitlement-flow.md)
* [Glosario](/help/authentication/kickstart/glossary.md)


Continuando con las siguientes secciones de esta guía del usuario, descubrirá formas de administrar diferentes configuraciones para los canales de su empresa, programadores o las integraciones entre canales y MVPD (Multichannel Video Program Distributors).

>[!IMPORTANT]
>TVE Dashboard ofrece la opción de cambiar entre un Workspace Básico y uno Avanzado. Para ello, alterne el icono en la esquina superior derecha. El Workspace avanzado está dirigido a usuarios con notables conocimientos técnicos, así como con conocimientos avanzados de las funciones que ofrece el equipo de producto de autenticación de Adobe Pass.

![Espacios de trabajo del tablero de TVE](../../../assets/tve-dashboard/old-tve-dashboard/tve-basic-advanced-workspace.png)

*Figura 1: La lista desplegable &quot;Workspace básico/avanzado&quot; del panel de Adobe Primetime TVE*

## Entornos {#authn-environments}

Según las tareas que un usuario deba realizar, es posible que tenga que cambiar entre entornos de autenticación de Adobe Pass. Para obtener información detallada sobre los entornos de autenticación de Adobe Pass, consulte el siguiente documento: [Comprender los entornos de autenticación de Adobe Pass](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md).

El Tablero de TVE proporciona dos entornos llamados Prequal (Precalificación) y Release, cada uno con dos perfiles llamados Staging y Production, como se muestra a continuación:

* [Ensayo de precuación](https://console-prequal.auth-staging.adobe.com/)
* [Producción de precuación](https://console-prequal.auth.adobe.com/)
* [Ensayo de lanzamiento](https://console.auth-staging.adobe.com/)
* [Producción de versiones](https://console.auth.adobe.com/)

Para cambiar entre entornos, el usuario puede hacer clic en el entorno deseado representado por la entrada del elemento desplegable que se muestra a continuación:

![Menú desplegable de entornos del panel de TVE](../../../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

*Figura 2: La lista desplegable Entornos del panel de Adobe Pass TVE*

>[!IMPORTANT]
>
>Es muy importante tener en cuenta que, cuando realice cambios administrativos en la configuración de autenticación de Adobe Pass a través del panel de TVE, le recomendamos encarecidamente que siga la secuencia que se muestra a continuación para garantizar la funcionalidad adecuada.

Para realizar cambios administrativos en la configuración de la autenticación de Adobe Pass a través del panel de TVE:

* Realice los cambios en [Ensayo de lanzamiento y valide](http://sp.auth-staging.adobe.com/apitest/api.html).
* Realice los cambios en [Producción de preculación y valide](http://sp.auth-staging.adobe.com/apitest/api.html).
* Realice los cambios en [Liberar producción y validarlos](http://sp.auth-staging.adobe.com/apitest/api.html).

>[!IMPORTANT]
>
>Para que los cambios administrativos se activen, los usuarios deben navegar a la sección &quot;Revisar y pulsar cambios&quot; seleccionando el botón, que se mostrará en la parte inferior izquierda de la barra lateral para revisar los cambios, añadir una descripción para los cambios recién creados y confirmar la actualización de la configuración seleccionando la &quot;Configuración push&quot;.

![El panel de Tve revisó una notificación push](../../../assets/tve-dashboard/old-tve-dashboard/tve-review-push-notifications.png)

*Figura 3: Revisión del panel de TVE de Adobe Primetime y notificación de cambios push*

## Secciones {#sections}

Los usuarios que trabajan para empresas de comunicación (programadores) pueden acceder a las siguientes secciones del Tablero de TVE desde la barra lateral:

* **Canales**: contiene configuración relacionada con proveedores de contenido
* **Programadores**: contiene configuración relacionada con la organización principal que agrega uno o varios **canales**
* **Integraciones**: contiene configuraciones relacionadas con la integración entre **Canales** y **MVPD**
* **MVPD**: contiene configuraciones relacionadas con las **MVPD** disponibles
* **Informes** - Contiene datos agregados para tres tipos de informes: AuthN TTL, AuthZ TTL, SSO
* **Registro de cambios**: contiene las últimas modificaciones aplicadas a la configuración del Tablero de TVE

![Secciones del tablero de TVE](../../../assets/tve-dashboard/old-tve-dashboard/tve-dashboard-sections.png)

*Figura 4: Secciones del panel de Adobe Primetime TVE*

### Canales {#tve-db-channels-section}

Esta sección permite ver y editar la configuración de los canales disponibles o crear uno nuevo. Al hacer clic en uno de los canales disponibles, aparecerá una pantalla con las siguientes pestañas:

* **Datos de canal**
   * **ID de canal**: el ID único del canal que se usa en nuestro sistema, también conocido como &quot;ID de solicitante&quot;.
   * **Nombre para mostrar** - El nombre comercial del canal.
* **Configuración general**
   * **Configuración de Analytics**: configure los eventos de autenticación de Adobe Pass para que se reenvíen a Adobe Analytics. Póngase en contacto con el Adobe para obtener más información sobre cómo se debe configurar el ID del grupo de informes (RSID) antes de habilitar esta función.
* **Certificados**

  Contiene la lista de certificados utilizados en el flujo de autenticación junto con su organización emisora, fecha de emisión y fecha de caducidad. Estos certificados sirven como claves privadas/públicas y se utilizan para fines de validación.
* **Dominios**

  Contiene la lista de dominios desde los que el canal respectivo se comunicará con la autenticación de Adobe Pass.
* **Integraciones**

  Contiene la lista de integraciones con MVPD disponibles, junto con el estado de cada integración que puede estar activada o no. Para navegar a la página Integración, haga clic en una entrada específica.
* **Aplicaciones registradas**

  Contiene la lista de registros de aplicaciones. Para obtener más información, revise el documento [Dynamic Client Registration Management](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

* **Esquemas personalizados**

  Contiene la lista de esquemas personalizados. Para obtener más información, consulte [Registro de aplicaciones de iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md) y [Dynamic Client Registration Management](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)

#### Agregar o eliminar dominios {#add-delete-domains}

Para iniciar el proceso de añadir un nuevo dominio para el canal seleccionado, debe hacer clic en el botón &quot;Añadir nuevo dominio&quot; debajo de la lista Dominios. Esto creará una nueva entrada de dominio donde puede especificar el nombre de dominio. Si ya existe un dominio más genérico en la lista de dominios, no debe agregar un nuevo subdominio.

![Agregar un nuevo dominio a la sección de un canal seleccionado](../../../assets/tve-dashboard/old-tve-dashboard/add-domain-to-channel-sec.png)

*Figura: ficha Dominios en los canales*

#### Crear una aplicación registrada en el nivel de canal {#create-registered-application-channel-level}

Para crear una aplicación registrada a nivel de canal, vaya al menú &quot;Canales&quot; y elija para el que desea crear una aplicación. Luego, después de navegar a la pestaña &quot;Aplicaciones registradas&quot;, haga clic en el botón &quot;Agregar nueva aplicación&quot;.

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-new-app-channel-level.png)

Como se ve en la siguiente imagen, los campos que debe rellenar son los siguientes:

* **Nombre de aplicación** - el nombre de la aplicación

* **Asignado al canal**: como se muestra a continuación, lo que es ligeramente diferente aquí, en comparación con la misma acción realizada en el nivel de programador, es la lista desplegable &quot;Canales asignados&quot; que no está habilitada, por lo que no hay opción de enlazar la aplicación registrada a una ubicación que no sea el canal actual.

* **Versión de la aplicación**: de forma predeterminada, se establece en &quot;1.0.0&quot;, pero le recomendamos encarecidamente que la modifique con su propia versión de la aplicación. Si decide cambiar la versión de la aplicación, se recomienda reflejarla creando una nueva aplicación registrada para ella.

* **Plataformas de aplicación**: las plataformas con las que se vinculará la aplicación. Tiene la opción de seleccionarlos todos o varios valores.

* **Nombres de dominio**: los dominios con los que se vinculará la aplicación. Los dominios de la lista desplegable son una selección unificada de todos los dominios de todos los canales. Tiene la opción de seleccionar varios dominios en la lista. El significado de los dominios es URL de redireccionamiento [RFC6749](https://tools.ietf.org/html/rfc6749). En el proceso de registro del cliente, la aplicación cliente puede solicitar que se le permita utilizar una URL de redireccionamiento para finalizar el flujo de autenticación. Cuando una aplicación cliente solicita una URL de redireccionamiento específica, se valida con los dominios incluidos en la lista blanca de esta aplicación registrada asociada a la declaración de software.

![](../../../assets/tve-dashboard/old-tve-dashboard/new-reg-app-channel.png)

Después de rellenar los campos con los valores adecuados, debe hacer clic en &quot;Listo&quot; para que la aplicación se guarde en la configuración.

Tenga en cuenta que no hay **ninguna opción para modificar una aplicación ya creada**. En caso de que se descubra que algo creado ya no cumple los requisitos, se deberá crear y utilizar una nueva aplicación registrada con la aplicación cliente cuyos requisitos cumpla.

##### Descargar una declaración de software {#download-software-statement-channel-level}

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-list.png)

Hacer clic en el botón &quot;Descargar&quot; en la entrada de la lista para la que se necesita una declaración de software generará un archivo de texto. Este archivo contendrá algo similar a la siguiente salida de ejemplo.

![](../../../assets/download-software-statement.png)

El nombre del archivo se identifica de forma exclusiva añadiendo como prefijo &quot;software_statement&quot; y la marca de tiempo actual.

Tenga en cuenta que, para la misma aplicación registrada, se recibirán diferentes declaraciones de software cada vez que se haga clic en el botón de descarga, pero esto no invalida las declaraciones de software obtenidas anteriormente para esta aplicación. Esto sucede porque se generan en el momento, por cada solicitud de acción.

Hay una **limitación** con respecto a la acción de descarga. Si se solicita una declaración de software haciendo clic en el botón &quot;Descargar&quot; poco después de crear la aplicación registrada y esto aún no se ha guardado y el json de configuración no se ha sincronizado, aparecerá el siguiente mensaje de error en la parte inferior de la página.

![](../../../assets/tve-dashboard/old-tve-dashboard/error-sw-statement-notready.png)

Esto ajusta un código de error HTTP 404 Not Found recibido del núcleo, ya que el ID de la aplicación registrada aún no se ha propagado y el núcleo no lo conoce.

Después de crear la aplicación registrada, la solución consiste en esperar 2 minutos como máximo para sincronizar la configuración. Cuando esto sucede, el mensaje de error deja de recibirse y el archivo de texto con la declaración del software queda disponible para su descarga.

### Programadores {#tve-db-programmers-section}

Esta sección permite ver y editar la configuración de los programadores disponibles o crear una nueva. Al hacer clic en uno de los programadores disponibles, se muestra una pantalla con las siguientes pestañas:

* **Datos del programador**
   * **Id. de programador** - El id. único del programador usado en nuestro sistema.
   * **Nombre para mostrar** - El nombre comercial del programador.
   * **URL del logotipo**: el localizador uniforme de recursos (URL) del logotipo comercial del programador.
   * **Vista previa del logotipo**: la vista previa del logotipo comercial del programador descargándolo desde el localizador uniforme de recursos (URL) anterior.

* **Certificados**

  Contiene la lista de certificados utilizados en el flujo de autenticación junto con su organización emisora, fecha de emisión y fecha de caducidad. Estos certificados sirven como claves privadas/públicas y se utilizan para fines de validación.

* **Canales**

  Contiene la lista de canales que pertenecen a este programador específico. Para ir a la sección Canales, haga clic en una entrada específica.

* **Aplicaciones registradas**

  Contiene la lista de registros de aplicaciones. Para obtener más información, consulte [Dynamic Client Registration Management](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management).

* **Esquemas personalizados**

  Contiene la lista de esquemas personalizados. Para obtener más información, consulte [Registro de aplicaciones de iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md).

#### Crear una aplicación registrada en el nivel de programador {#create-registered-application-programmer-level}

Vaya a la ficha **Programadores** > **Aplicaciones registradas**.

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-progr-level.png)

En la ficha Aplicaciones registradas, haga clic en **Agregar nueva aplicación**. Rellene los campos obligatorios en la nueva ventana.

Como se ve en la siguiente imagen, los campos que debe rellenar son los siguientes:

* **Nombre de aplicación** - el nombre de la aplicación

* **Asignado al canal**: el nombre del canal al que está vinculada esta aplicación. </span> La configuración predeterminada de la máscara desplegable es **Todos los canales.** La interfaz le permite seleccionar uno o todos los canales.

* **Versión de la aplicación**: de forma predeterminada, se establece en &quot;1.0.0&quot;, pero le recomendamos encarecidamente que la modifique con su propia versión de la aplicación. Si decide cambiar la versión de la aplicación, se recomienda reflejarla creando una nueva aplicación registrada para ella.

* **Plataformas de aplicación**: las plataformas con las que se vinculará la aplicación. Tiene la opción de seleccionarlos todos o varios valores.

* **Nombres de dominio**: los dominios con los que se vinculará la aplicación. Los dominios de la lista desplegable son una selección unificada de todos los dominios de todos los canales. Tiene la opción de seleccionar varios dominios en la lista. El significado de los dominios es URL de redireccionamiento [RFC6749](https://tools.ietf.org/html/rfc6749). En el proceso de registro del cliente, la aplicación cliente puede solicitar que se le permita utilizar una URL de redireccionamiento para finalizar el flujo de autenticación. Cuando una aplicación cliente solicita una URL de redireccionamiento específica, se valida con los dominios incluidos en la lista blanca de esta aplicación registrada asociada a la declaración de software.

![](../../../assets/tve-dashboard/old-tve-dashboard/new-reg-app.png)

Después de rellenar los campos con los valores adecuados, debe hacer clic en &quot;Listo&quot; para que la aplicación se guarde en la configuración.

Tenga en cuenta que no hay **ninguna opción para modificar una aplicación ya creada**. En caso de que se descubra que algo creado ya no cumple los requisitos, se deberá crear y utilizar una nueva aplicación registrada con la aplicación cliente cuyos requisitos cumpla.

##### Descargar una declaración de software {#download-software-statement-programmer-level}

![](../../../assets/tve-dashboard/old-tve-dashboard/reg-app-list.png)

Hacer clic en el botón &quot;Descargar&quot; en la entrada de la lista para la que se necesita una declaración de software generará un archivo de texto. Este archivo contendrá algo similar a la siguiente salida de ejemplo.

![](../../../assets/download-software-statement.png)

El nombre del archivo se identifica de forma exclusiva añadiendo como prefijo &quot;software_statement&quot; y la marca de tiempo actual.

Tenga en cuenta que, para la misma aplicación registrada, se recibirán diferentes declaraciones de software cada vez que se haga clic en el botón de descarga, pero esto no invalida las declaraciones de software obtenidas anteriormente para esta aplicación. Esto sucede porque se generan en el momento, por cada solicitud de acción.

Hay una **limitación** con respecto a la acción de descarga. Si se solicita una declaración de software haciendo clic en el botón &quot;Descargar&quot; poco después de crear la aplicación registrada y esto aún no se ha guardado y el json de configuración no se ha sincronizado, aparecerá el siguiente mensaje de error en la parte inferior de la página.

![](../../../assets/tve-dashboard/old-tve-dashboard/error-sw-statement-notready.png)

Esto ajusta un código de error HTTP 404 Not Found recibido del núcleo, ya que el ID de la aplicación registrada aún no se ha propagado y el núcleo no lo conoce.

Después de crear la aplicación registrada, la solución consiste en esperar 2 minutos como máximo para sincronizar la configuración. Cuando esto sucede, el mensaje de error deja de recibirse y el archivo de texto con la declaración del software queda disponible para su descarga.

### Integraciones {#tve-db-integrations-sec}

Esta sección permite ver y editar la configuración de las integraciones entre canales y MVPD disponibles, o crear una nueva. Al hacer clic en una de las integraciones disponibles, se devolverá una sola página al utilizar la Workspace básica o una pantalla con las siguientes pestañas al utilizar la Workspace avanzada:

* **Datos de integración**
   * **Id. de integración**: el resultado de anexar el ID único de MVPD al ID único del canal separado por el carácter &quot;_&quot;.
   * **Nombre para mostrar canal** - El nombre comercial del canal.
   * **ID de canal**: el ID único del canal que se usa en nuestro sistema, también conocido como &quot;ID de solicitante&quot;.
   * **Nombre para mostrar de MVPD** - El nombre comercial de MVPD.
   * **ID de MVPD**: el ID único de MVPD que se usa en nuestro sistema.
* **Configuración general**
   * **Claves de metadatos de usuario**: configure las claves de metadatos disponibles para la integración específica.
   * **Configuración específica de la plataforma**: configure diferentes configuraciones en una plataforma específica (por ejemplo, TTL, SSO e IFrames).

* **Configuración de autenticación**
   * Contiene la configuración relacionada con la función de autenticación de Adobe Pass.
* **Configuración de autorización**
   * Contiene la configuración relacionada con la función de autorización de autenticación de Adobe Pass.
* **Configuración de cierre de sesión**
   * Contiene la configuración relacionada con la función de cierre de sesión de autenticación de Adobe Pass.

#### Crear integración {#create-integration}

Para crear una nueva integración, siga los pasos a continuación:

* haga clic en el botón &quot;Añadir nueva integración&quot;.
* buscar y seleccionar un canal
* busque y seleccione un MVPD
* Espere a que el Tablero de TVE calcule &quot;Integration Id&quot; y muestre los extremos de MVPD disponibles
* seleccione los extremos de autenticación, autorización y cierre de sesión o utilice los valores predeterminados
* Haga clic en el botón &quot;Crear integración&quot;.
* según la configuración de MVPD, puede aparecer una ventana emergente y solicitar propiedades adicionales, que MVPD debería haber proporcionado previamente; de lo contrario, se redirigirá a la página de integración recién creada

![](../../../assets/tve-dashboard/old-tve-dashboard/new-integration-window.png)



*Figura 5. Ventana Nueva integración del panel de Adobe Primetime TVE*


#### Actualizar integración {#update-integration}

Para actualizar una integración existente, haga clic en la entrada de tabla de esa integración específica desde la sección Integraciones o desde la sección Canales, que contiene una pestaña Integraciones.

Al utilizar el modo Básico de Workspace, esta sección permite ver y editar la configuración actualizada con más frecuencia, como los TTL de token de autenticación y autorización (tiempo de vida), así como la configuración de iFrame. Tenga en cuenta que puede faltar la configuración de TTL para las integraciones con MVPD que admiten TTL de persistencia de token definido dinámicamente (consulte la entrada 1.19 de [Requisitos de integración de MVPD](/help/authentication/integration-guide-mvpds/mvpd-integr-features.md)).



Al utilizar el modo Advanced Workspace, esta sección permite ver y editar configuraciones menos comunes.



En el caso de los modos Básico y Avanzado de Workspace, esta configuración se puede cambiar a nivel de plataforma (por ejemplo, seleccione un valor personalizado para el token TTL de autorización en Android, de forma predeterminada en todas las plataformas).



>[!IMPORTANT]
>Es importante comprender la cadena de herencia de configuración: MVPD -> Extremo de MVPD -> Integración -> Platform, donde Platform tiene el valor más específico y MVPD el valor predeterminado más genérico.

![](../../../assets/tve-dashboard/old-tve-dashboard/inheritance-chain-component.png)


*Figura 6. Componente de cadena de herencia de propiedades del panel de Adobe Primetime TVE*


#### Configuración específica de la plataforma {#platform-sp-settings}

Esta subsección se puede utilizar para anular la configuración de plataformas específicas. Las plataformas disponibles son:

* **Todas las plataformas**: establezca valores que se aplicarán a todas las plataformas independientemente de las implementaciones del programador en caso de que no haya otros valores establecidos para una plataforma específica.
* **Android**: establezca los valores que se aplicarán a las implementaciones del programador mediante la autenticación de Adobe Pass en Android SDK.
* **API de REST sin cliente**: establezca valores que se aplicarán a las implementaciones del programador a través de la API de REST de autenticación de Adobe Pass.
* **Fire TV**: establezca valores que se aplicarán a las implementaciones de Programmer a través de Adobe Pass Authentication FireTV SDK.
* **Flash SDK**: esta plataforma está en desuso. **obsoleto**
* **JavaScript SDK**: establezca valores que se aplicarán a las implementaciones del programador a través de Adobe Pass Authentication JavaScript SDK.
* **Roku**: establezca valores que se aplicarán a las implementaciones del programador a través de la API de REST de autenticación de Adobe Pass y que enviarán &quot;Roku&quot; como tipo de dispositivo. Esto tiene prioridad sobre los valores establecidos para la plataforma de API de REST sin cliente en el caso de los dispositivos Roku.
* **SDK nativo de Xbox**: esta plataforma está en desuso. **obsoleto**
* **API de REST de Xbox 360**: establece los valores que se aplicarán a las implementaciones del programador a través de la API de REST de autenticación de Adobe Pass y que enviarán &quot;xbox&quot; como tipo de dispositivo. Esto tiene prioridad sobre los valores establecidos para la plataforma de API REST sin cliente en el caso de dispositivos Xbox 360.
* **API de REST de Xbox One**: establece los valores que se aplicarán a las implementaciones del programador a través de la API de REST de autenticación de Adobe Pass y que enviarán &quot;xboxOne&quot; como tipo de dispositivo. Esto tiene prioridad sobre los valores establecidos para la plataforma de Api REST sin cliente en el caso de dispositivos XboxOne.
* **iOS**: establezca los valores que se aplicarán a las implementaciones del programador mediante la autenticación de Adobe Pass en iOS SDK.
* **tvOS**: establezca los valores que se aplicarán a las implementaciones del programador a través de la autenticación de Adobe Pass tvOS SDK.


![](../../../assets/tve-dashboard/old-tve-dashboard/platform-sp-settings.png)

*Figura 7. Configuración específica de la plataforma de paneles de Adobe Primetime TVE*


#### Habilitar inicio de sesión único de Platform {#enable-platform-sso}

Siga los pasos a continuación para habilitar o deshabilitar el inicio de sesión único para una integración y plataforma específicas:

* asegúrese de que está utilizando el modo de Workspace avanzado
* vaya a la integración que desee
* vaya a la ficha **Configuración general**
* seleccione la plataforma en la que desea habilitar o deshabilitar el inicio de sesión único
* alternar el indicador **Habilitar inicio de sesión único** al valor deseado (Sí / No)

  >[!IMPORTANT]
  >Es importante tener en cuenta que el indicador **Habilitar el inicio de sesión único** solo está disponible para las plataformas iOS, tvOS, Roku y FireTV, y solo para las integraciones con MVPD que admiten el inicio de sesión único en dichas plataformas.

* alternar el indicador **Aplicar permiso de plataforma** al valor deseado (Sí / No)

  >[!IMPORTANT]
  >Es importante tener en cuenta que el indicador **Aplicar permisos de plataforma** controla si se aplicará o no la decisión del usuario de permitir o denegar el acceso de plataforma a su suscripción al proveedor de TV. Teniendo en cuenta el escenario en el que el indicador **Habilitar el inicio de sesión único** se establece en &quot;Sí&quot;, el indicador **Aplicar permisos de plataforma** también se establece en &quot;Sí&quot; y el usuario elige Denegar el acceso de plataforma a su suscripción de proveedor de TV, la aplicación (canal) correspondiente no podrá utilizar el token de autenticación de Adobe Pass obtenido por otra aplicación (canal).


#### Habilitar autenticación basada en el hogar {#enable-hba}

Siga los pasos a continuación para habilitar o deshabilitar la autenticación basada en inicio para las MVPD basadas en **OAuth2**:

* asegúrese de que está utilizando el modo de Workspace avanzado
* vaya a la integración que desee
* vaya a la ficha **Configuración de autenticación**
* vaya a la subpestaña **Reglas dinámicas de AuthN**
* alternar el indicador **Intento de HBA** al valor deseado (Sí / No)


>[!IMPORTANT]
>Tenga en cuenta que el valor &quot;HBA AuthN TTL&quot; nunca debe anularse; de lo contrario, el flujo de autorización podría fallar inesperadamente.

Póngase en contacto con **tve-support@adobe.com** para obtener información sobre cómo habilitar la autenticación de base de inicio para MVPD basadas en SAML.

### MVPD {#tve-db-mvpds-sec}

Esta sección permite ver la configuración de las MVPD disponibles. Al hacer clic en una de las MVPD disponibles, aparece una pantalla con las siguientes pestañas:

* **Datos de MVPD**
   * **ID de MVPD**: el ID único de MVPD que se usa en nuestro sistema.
   * **Nombre para mostrar**: el nombre comercial de MVPD que podría usarse en el selector del usuario.
   * **URL del logotipo**: el localizador uniforme de recursos (URL) del logotipo comercial de MVPD.
   * **Vista previa del logotipo**: para obtener una vista previa del logotipo comercial de MVPD, descárguelo desde el localizador uniforme de recursos (URL) anterior.
* **Configuración general**
   * **Claves de metadatos de usuario**
      * Claves de metadatos disponibles para el MVPD específico.
   * **Propiedades de datos de cliente**
      * **Auth / Aggregator**: si se establece en &quot;Yes&quot;, se necesita un nuevo token de autenticación para cada nuevo canal al que el usuario intente acceder.
      * **AuthN pasivo habilitado**: si el indicador Auth/Aggregator está establecido en &quot;Sí&quot; y AuthN pasivo habilitado está establecido en &quot;Sí&quot;, el proceso de autenticación con otro canal se producirá en segundo plano sin necesidad de una redirección completa del explorador y se mostrará el selector.
      * **Sesión de autenticación/explorador**: si se establece en &quot;Sí&quot;, se cerrará la sesión del usuario después de cerrar el explorador. Si se establece en &quot;No&quot;, el usuario puede reiniciar el explorador y permanecer conectado.
      * **IFrame Necesario** - Si se establece en &quot;Sí&quot;, entonces indica que la ventana de inicio de sesión de MVPD requiere un iFrame. Los campos &quot;Anchura del iFrame&quot; y &quot;Altura del iFrame&quot; representan el tamaño necesario para que el iFrame cargue la página de inicio de sesión de MVPD.
* **Configuración de autenticación**
   * **Seleccionar extremo**
      * Este campo indica los extremos de autenticación expuestos por MVPD. El punto de conexión puede variar según el protocolo de autenticación utilizado.
   * **Configuración general de AuthN**
      * Esta subpestaña muestra el protocolo de autenticación utilizado por MVPD y la información relacionada con el protocolo.
   * **Certificados AuthN**
      * Esta subpestaña muestra los certificados que utiliza MVPD en el flujo de autenticación junto con su organización emisora, fecha de emisión y fecha de caducidad. Estos certificados sirven como claves privadas/públicas y se utilizan para fines de validación.
   * **Reglas dinámicas de AuthN**
      * Esta subpestaña muestra las reglas que se aplican al proceso de autenticación. Al pulsar el botón Request / Response / Token del diagrama, puede ver resaltados los parámetros aplicados a esa parte del flujo de autenticación.
* **Configuración de autorización**
   * **Seleccionar extremo**
      * Este campo indica el punto final de autorización expuesto por MVPD. El punto de conexión puede variar según el protocolo de autorización utilizado. SOAP Los protocolos de autorización disponibles son:, REST (para dispositivos sin cliente), SAML, XACML y OAUTH.
   * **Configuración general de AuthZ**
      * Esta subpestaña muestra el protocolo de autorización utilizado por MVPD y la información relacionada con el protocolo.
      * **Configuración de comprobación preliminar**
         * Describe la cantidad de recursos que una MVPD puede autorizar previamente en una sola llamada, el modelo PreFlight utilizado y el umbral de tiempo de espera. En ocasiones, el número de recursos puede ser diferente para una integración determinada. Esto se puede administrar editando la propiedad &quot;**Número máximo de recursos de comprobaciones**&quot;, disponible en la pestaña Configuración general. Esta propiedad solo está disponible para una integración determinada y, si se establece, se utilizará en lugar del valor definido en Configuración de autorización -> Configuración de prevuelo -> Recursos máximos de prevuelo.
      * **Protección DOS**
         * Describe la protección de denegación de servicio en el extremo de autorización de MVPD. Para obtener una descripción exacta de cada campo, consulte la información sobre herramientas pasando el ratón por encima de los campos Protección DOS.
      * Si MVPD es un **TempPass**, la **Configuración general de AuthZ** también contiene información sobre la duración de TempPass.
      * Si el MVPD es un **FlexibleTempPass**, la **Configuración general de AuthZ** también contiene información sobre la duración de TempPass, el número máximo de recursos y el campo de identificación (vea la imagen siguiente).
   * **Certificados AuthZ**
      * Esta subpestaña muestra los certificados que utiliza MVPD en el flujo de autorización junto con su organización emisora, fecha de emisión y fecha de caducidad. Estos certificados sirven como claves privadas/públicas y se utilizan para fines de validación.
   * **Reglas dinámicas de AuthZ**
      * Esta subpestaña muestra las reglas que se aplican al proceso de autorización. Al presionar **Solicitud / Respuesta / Token** en el diagrama, puede ver resaltados los parámetros aplicados a esa parte del flujo de autorización.
* **Configuración de cierre de sesión**
   * **Seleccionar extremo**
      * Este campo indica el punto final de cierre de sesión expuesto por MVPD. Los protocolos proporcionados pueden ser SAML u OAuth2.
      * **Configuración general de cierre de sesión**
         * Esta subpestaña muestra el protocolo de cierre de sesión utilizado por MVPD e información relacionada con el protocolo.
         * **Requerir respuesta de cierre de sesión firmada**: si se establece en &quot;Sí&quot;, la respuesta debe estar firmada por un certificado de confianza.
      * **Certificados de cierre de sesión**
         * Esta subpestaña muestra los certificados que utiliza MVPD en el flujo de cierre de sesión junto con su organización emisora, fecha de emisión y fecha de caducidad. Estos certificados sirven como claves privadas/públicas y se utilizan para fines de validación.
      * **Reglas dinámicas de cierre de sesión**
         * Esta subpestaña muestra las reglas que se aplican al proceso de cierre de sesión. Al presionar **Solicitud / Respuesta / Token** en el diagrama, puede ver como resaltados los parámetros aplicados a esa parte del flujo de cierre de sesión.

### Informes {#tve-db-reports-sec}

Para ir a esta sección, haga clic en &quot;Informes&quot; en el menú &quot;[Secciones de panel](#sections)&quot;. Navegará a una pantalla con 3 fichas, que se presentarán en detalle en las siguientes subsecciones: [Informes TTL AuthN](#authn-ttl-reports), [Informes TTL AuthZ](#authz-ttl-reports), [Informes SSO](#sso-reports).

Esta sección permite ver y exportar los datos agregados de varios tipos de informes para las integraciones de canales con varias MVPD en todas las plataformas.

#### Plataformas {#report-platforms}

Todos los informes acumulan valores en las siguientes plataformas:

**EXPLORADORES**
Muestra los valores que se aplicarán a las implementaciones de Programador a través de Autenticación de Adobe Pass JavaScript SDK.

**MÓVIL: IOS**
Muestra los valores que se aplicarán a las implementaciones de Programador a través de Autenticación de Adobe Pass iOS SDK.

**MÓVIL: ANDROID**
Muestra los valores que se aplicarán a las implementaciones de Programador a través de Autenticación de Adobe Pass Android SDK.

**MÓVIL: OTROS**
Muestra los valores que se aplicarán a las implementaciones del programador a través de la API de REST de autenticación de Adobe Pass desarrollada para dispositivos móviles.

**TVCD: ROKU**
Muestra los valores que se aplicarán a las implementaciones del programador a través de la API de REST de autenticación de Adobe Pass y que envían &quot;Roku&quot; como tipo de dispositivo.

**TVCD: FIRETV**
Muestra los valores que se aplicarán a las implementaciones de Programador a través de Adobe Pass Authentication FireTV SDK.

**TVCD: APPLETV**
Muestra los valores que se aplicarán a las implementaciones del programador a través de la autenticación de Adobe Pass tvOS SDK.

**TVCD: OTROS**
Muestra los valores que se aplicarán a las implementaciones del programador a través de la API de REST de autenticación de Adobe Pass desarrollada para dispositivos conectados a TV.

**PLATAFORMA: DESCONOCIDA**
Muestra los valores que se aplicarán a las implementaciones de Programador para las que los servicios de autenticación de Adobe Pass detectan un tipo de dispositivo desconocido.

Revise el mecanismo de [paso de información de cliente](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md) a las API de REST de autenticación de Adobe Pass para obtener más detalles sobre cómo enviar el tipo de dispositivo deseado (por ejemplo, &quot;Roku&quot;).

Todos los informes acumulan valores calculados en función de la configuración específica de cada entorno de autenticación de Adobe Pass. Por lo tanto, se pueden esperar diferentes datos de informes al cambiar entre diferentes entornos de TVE Dashboard.

Consulte la sección [Entornos](#authn-environments) para obtener más información relacionada con los entornos disponibles para la autenticación de Adobe Pass.


##### Selección de canales/MVPD específicos {#selecting-specific-channels-mvpds}

Todos los informes permiten el uso de filtros seleccionando canales específicos o MVPD específicos para incluirlos en los informes resultantes.

Para seleccionar uno o varios canales, utilice la **lista desplegable** colocada después de la etiqueta &quot;Canales seleccionados para el informe&quot;. Consulte la figura 8./9./10. imágenes de abajo.

Para seleccionar una o varias MVPD, utilice la **lista desplegable** que aparece después de la etiqueta &quot;MVPD seleccionadas para el informe&quot;. Consulte la figura 8./9./10. imágenes de abajo.

De forma predeterminada, los datos se agregan en todos los canales de su empresa (&quot;Todos los canales&quot;) y en las MVPD con las que están integrados (&quot;Todas las MVPD&quot;).

Si elige deseleccionar &quot;Todos los canales&quot; o &quot;Todas las MVPD&quot; sin elegir opciones específicas, la interfaz de usuario mostrará el marcador de posición &quot;No hay datos disponibles&quot;.


##### Exportar informe {#export-report}

Todos los informes permiten exportar datos en un archivo de formato de valores separados por comas (CSV).

Para exportar datos, utilice el botón &quot;Exportar informe&quot; situado en la esquina superior derecha de la ventana. Consulte la figura 8./9./10. imágenes de abajo.

Se descargará automáticamente en el equipo un archivo con el nombre **Report.csv**. Por lo tanto, asegúrese de que la configuración de su navegador permite descargar archivos.

El icono de carga &quot;Exportación de datos&quot; estará presente en la pantalla mientras se calcula el archivo Report.csv, lo que puede tardar de **a un par de minutos** según el tamaño de los datos que desee exportar.

#### Informes TTL de AuthN (#authn-ttl-reports)

Este informe muestra el tiempo de vida (TTL) del token de autenticación configurado para las integraciones de los canales con varias MVPD en todas las plataformas.

El token de autenticación Tiempo de vida, que también se conoce como **AuthN TTL**, se muestra en valores legibles como: **días, horas, minutos, segundos**.

En cuanto a la experiencia del usuario, los informes TTL de AuthN le permiten inspeccionar visualmente la cantidad de tiempo que se autenticará un usuario teniendo en cuenta una MVPD específica y una plataforma específica.

Para navegar a este tipo de informe, haga clic en la pestaña &quot;Informes TTL de AuthN&quot; de la sección &quot;Informes&quot;.

![Informes TTL de AuthN](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*Figura 8: Ficha Informe TTL de autenticación del panel de Adobe Primetime TVE*

La tabla Informes TTL de AuthN contiene páginas y se puede desplazar horizontal y verticalmente según el tamaño de la pantalla.

Si considera realizar un cambio en un valor TTL AuthN, revise la sección [Integraciones](#tve-db-integrations-sec).

>[!IMPORTANT]
>El marcador de posición &quot;**Set by MVPD**&quot; se usa en casos en los que MVPD será el que aplique el valor TTL AuthN y no la configuración de autenticación de Adobe Pass.


#### Informes TTL de AuthZ {#authz-ttl-reports}

Este informe muestra el tiempo de vida (TTL) del token de autorización configurado para las integraciones de los canales con varias MVPD en todas las plataformas.

El tiempo de vida del token de autorización, también conocido como **AuthZ TTL**, se muestra en valores legibles por humanos como: **días, horas, minutos, segundos**.

En cuanto a la experiencia del usuario, los informes TTL de AuthZ le permiten inspeccionar visualmente la cantidad de tiempo que se autorizará a un usuario teniendo en cuenta una MVPD específica y una plataforma específica.

Para navegar a este tipo de informe, haga clic en la pestaña &quot;Informes TTL de AuthZ&quot; de la sección &quot;Informes&quot;.

![Informes TTL de AuthZ](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Figura 9. Ficha Informe TTL de autenticación del panel de Adobe Primetime TVE*

La tabla Informes TTL de AuthZ contiene páginas y se puede desplazar horizontal y verticalmente según el tamaño de la pantalla.

Si considera realizar un cambio en un valor TTL de AuthZ, consulte la sección [Integraciones](#tve-db-integrations-sec).

>[!IMPORTANT]
>El marcador de posición &quot;**Set by MVPD**&quot; se usa en casos en los que MVPD será el que aplique el valor TTL de AuthZ y no la configuración de autenticación de Adobe Pass.


#### Informes de SSO {#sso-reports}

Este informe muestra el estado de inicio de sesión único (SSO) configurado para las integraciones de canales con varias MVPD en todas las plataformas.

El estado de inicio de sesión único, que también se conoce como **estado de SSO**, se muestra en tres estados con los siguientes valores posibles: **SSO deshabilitado, SSO habilitado, SSO incierto**.

En cuanto a la experiencia del usuario, los informes de SSO le permiten inspeccionar visualmente la experiencia de SSO de autenticación de usuario esperada teniendo en cuenta un MVPD y una plataforma específicos.

Para navegar a este tipo de informe, haga clic en la pestaña &quot;**Informes de SSO**&quot; de la sección &quot;**Informes**&quot;.


![Pestaña Informes SSO del panel de TVE](../../../assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)


*Figura 10: Pestaña Informes SSO del panel de Adobe Primetime TVE*

La tabla Informes de SSO contiene páginas y se puede desplazar horizontal y verticalmente en función del tamaño de la pantalla.

Si piensa hacer un cambio en el estado de SSO, consulte la sección [Integraciones](#tve-db-integrations-sec).

>[!IMPORTANT]
>El marcador de posición &quot;**SSO incierto**&quot; se usa en los casos en que el SSO está habilitado y es posible, pero la configuración de la plataforma de usuario o las decisiones del usuario (por ejemplo, la opción del explorador del usuario para bloquear cookies de terceros, la opción del usuario de denegar el acceso de la plataforma a su suscripción al proveedor de TV) o la configuración de MVPD (por ejemplo, si MVPD solicita la autenticación para cada canal) pueden impedir que se realice el SSO.

### Registro de cambios {#tve-db-changelog-sec}

Esta sección muestra una lista de todas las modificaciones insertadas a través del panel de TVE en el entorno y la configuración de autenticación de Adobe Pass.

Existen columnas que indican la fecha de inserción, el usuario que ha realizado la modificación y el estado de la inserción.

Esta sección también permite la comparación de dos entradas de tabla para reducir las modificaciones específicas que desea inspeccionar e incluso compartir la comparación como un elemento de correo.

### Comentarios {#tve-db-feedback-sec}

Esta sección permite a los usuarios enviar comentarios. Siga los pasos para proporcionar comentarios al equipo de producto de autenticación de Adobe Pass:

* haga clic en el botón &quot;Comentarios&quot; en la parte derecha de la pantalla
* introduzca el asunto
* introduzca el mensaje
* si es necesario, cargue una captura de pantalla en el mensaje haciendo clic en el botón &quot;Cargar captura de pantalla&quot;.
* haga clic en el botón &quot;Enviar&quot;

![formulario de comentarios del panel tve](../../../assets/tve-dashboard/old-tve-dashboard/tve-dashboard-feedback.png)

*Figura 11: Sección de comentarios del panel de Adobe Primetime TVE*

Para obtener instrucciones sobre cómo capturar capturas de pantalla, consulte los siguientes vínculos:

* [Cómo capturar capturas de pantalla en Windows](https://support.microsoft.com/en-us/windows/use-snipping-tool-to-capture-screenshots-00246869-1843-655f-f220-97299b865f6b#1TC=windows-7)

* [Cómo capturar capturas de pantalla en Mac](https://support.apple.com/en-us/HT201361)

## Resolución de problemas {#tve-db-troubleshoot}

### Modo de mantenimiento {#maintenance-mode}

![Aplicación TVE en modo de mantenimiento](../../../assets/tve-dashboard/old-tve-dashboard/tveapp-maintenance-mode.png)


*Figura: Aplicación TVE en modo de mantenimiento*


En caso de que el Tablero de TVE se encuentre en &quot;modo de mantenimiento&quot;, los usuarios no podrán visualizar ni realizar nuevos cambios.

Si esto ocurre, tendrá que esperar a que el equipo de ingeniería de autenticación de Adobe Pass termine el trabajo de mantenimiento en el Tablero de TVE.

### Estado degradado {#degraded-state}

![Aplicación TVE en estado degradado](../../../assets/tve-dashboard/old-tve-dashboard/tve-degraded-state.png)


*Figura: Aplicación TVE en estado degradado*

En caso de que el Tablero de TVE se encuentre en &quot;estado degradado&quot;, los usuarios carecerán de capacidades de búsqueda y clasificación, pero podrán ver o realizar nuevos cambios.

Si esto ocurre, tendrá que esperar a que el equipo de ingeniería de autenticación de Adobe Pass termine el trabajo de mantenimiento en el Tablero de TVE.
