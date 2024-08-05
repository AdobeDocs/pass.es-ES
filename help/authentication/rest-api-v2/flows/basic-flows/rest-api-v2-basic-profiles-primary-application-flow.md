---
title: Perfiles básicos - Aplicación principal - Flujo
description: 'API de REST V2: Perfiles básicos: Aplicación principal: Flujo'
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 0%

---


# Flujo de perfiles básicos realizado dentro de la aplicación principal {#basic-profiles-flow-primary-application}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El **flujo de perfiles** dentro del derecho de autenticación de Adobe Pass permite que la aplicación de streaming acceda a la información de los inicios de sesión activos de los usuarios.

El flujo de perfiles básicos le permite consultar los siguientes escenarios:

* [Recuperación de perfiles](#retrieve-profiles)
* [Recuperar perfil para mvpd específico](#retrieve-profile-for-specific-mvpd)
* [Recuperar perfil para código específico](#retrieve-profile-for-specific-code)

## Recuperación de perfiles {#retrieve-profiles}

### Requisitos previos {#prerequisites-retrieve-profiles}

Antes de recuperar perfiles, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación de streaming quiere recuperar todos los perfiles normales.

### Flujo de trabajo {#workflow-retrieve-profiles}

Siga los pasos dados para implementar el flujo de recuperación de perfiles básico realizado dentro de una aplicación principal, como se muestra en el siguiente diagrama.

![Recuperar perfiles](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profiles-within-primary-application.png)

*Recuperar perfiles*

1. **Recuperar perfiles:** La aplicación de transmisión recopila todos los datos necesarios para recuperar toda la información de perfiles enviando una solicitud al extremo de perfiles.

   Consulte la documentación de la API [Recuperar perfiles](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) para obtener más información sobre lo siguiente:
   * Todos los _parámetros necesarios_, como `serviceProvider`
   * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

1. **Buscar perfiles regulares:** El servidor de Adobe Pass identifica todos los perfiles válidos en función de los parámetros y encabezados recibidos.

1. **Devuelve información sobre perfiles normales:** La respuesta del extremo de perfiles contiene información sobre los perfiles encontrados asociados con los parámetros y encabezados recibidos.

   Consulte la documentación de la API [Recuperar perfiles](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) para obtener más información sobre la información proporcionada en una respuesta de perfil.

   >[!NOTE]
   >
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Elija un perfil y continúe con los flujos de decisiones:** Si la respuesta del extremo de perfiles contiene perfiles, la aplicación de transmisión utiliza su lógica interna (finalmente interactuando con el usuario final) para elegir uno de los perfiles disponibles y continuar con los flujos de decisiones posteriores.

1. **Indique un nuevo flujo de autenticación básico:** Si la respuesta del extremo de perfiles no contiene un perfil, la aplicación de transmisión indica al usuario que inicie un nuevo flujo de autenticación básico.

## Recuperar perfil para mvpd específico {#retrieve-profile-for-specific-mvpd}

### Requisitos previos {#prerequisites-retrieve-profile-for-specific-mvpd}

Antes de recuperar el perfil de una MVPD específica, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación de streaming, que tiene un identificador `mvpd` seleccionado o almacenado en caché, desea recuperar el perfil regular para una MVPD específica.

### Flujo de trabajo {#workflow-retrieve-profile-for-specific-mvpd}

Siga los pasos dados para implementar el flujo básico de recuperación de perfiles para una MVPD específica realizada dentro de una aplicación principal, como se muestra en el diagrama siguiente.

![Recuperar perfil para mvpd específico](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-mvpd.png)

*Recuperar perfil para mvpd específico*

1. **Recuperar perfil para mvpd específico:** La aplicación de streaming recopila todos los datos necesarios para recuperar información de perfil para esa MVPD específica enviando una solicitud al extremo de perfiles.

   Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) específica para obtener más información sobre lo siguiente:
   * Todos los _parámetros necesarios_, como `serviceProvider` y `mvpd`
   * Todos los _encabezados_ necesarios, como `Authorization`, `AP-Device-Identifier`
   * Todos los _parámetros y encabezados_ opcionales

1. **Buscar perfil normal:** El servidor de Adobe Pass identifica un perfil válido en función de los parámetros y encabezados recibidos.

1. **Devuelve información sobre el perfil normal:** La respuesta del extremo de perfiles contiene información sobre el perfil encontrado asociado a los parámetros y encabezados recibidos.

   Consulte la documentación de la API [Recuperar perfil para mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) específica para obtener más información sobre la información proporcionada en una respuesta de perfil.

   >[!NOTE]
   >
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   > * La integración entre `serviceProvider` y `mvpd` proporcionados debe estar activa.
   >
   > <br/>
   > 
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Continúe con los flujos de decisiones:** Si la respuesta del extremo de perfiles contiene un perfil, la aplicación de transmisión utiliza la información del perfil para continuar con los flujos de decisiones subsiguientes.

1. **Indique un nuevo flujo de autenticación básico:** Si la respuesta del extremo de perfiles no contiene un perfil, la aplicación de transmisión indica al usuario que inicie un nuevo flujo de autenticación básico.

## Recuperar perfil para código específico {#retrieve-profile-for-specific-code}

### Requisitos previos {#prerequisites-retrieve-profile-for-specific-code}

Antes de recuperar el perfil de un código de autenticación específico, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación de streaming, que tiene un(a) `code` usado(a) para realizar la autenticación interactiva con la MVPD, desea recuperar el perfil para un código de autenticación específico.

### Flujo de trabajo {#workflow-retrieve-profile-for-specific-code}

Siga los pasos dados para implementar el flujo de recuperación de perfiles básico para un código de autenticación específico realizado dentro de una aplicación principal, como se muestra en el diagrama siguiente.

![Recuperar perfil para código específico](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-code.png)

*Recuperar perfil para código específico*

1. **Recuperar perfil para código específico:** La aplicación de flujo continuo recopila todos los datos necesarios para recuperar información de perfil para ese código de autenticación específico enviando una solicitud al extremo de perfiles.

   Consulte la documentación de la API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) para obtener detalles sobre:
   * Todos los _parámetros necesarios_, como `serviceProvider` y `code`
   * Todos los _encabezados_ requeridos, como `Authorization`
   * Todos los _parámetros y encabezados_ opcionales

1. **Buscar perfil normal:** El servidor de Adobe Pass identifica un perfil válido en función de los parámetros y encabezados recibidos.

1. **Devuelve información sobre el perfil normal:** La respuesta del extremo de perfiles contiene información sobre el perfil encontrado asociado a los parámetros y encabezados recibidos.

   Consulte la documentación de la API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) para obtener detalles sobre la información proporcionada en una respuesta de perfil.

   >[!NOTE]
   >
   > El extremo de perfiles valida los datos de solicitud para garantizar que se cumplan las condiciones básicas:
   >
   > * Los parámetros y encabezados _required_ deben ser válidos.
   >
   > <br/>
   >
   > Si la validación falla, se generará una respuesta de error, que proporcionará información adicional que se ajustará a la documentación de [Códigos de error mejorados](../../../enhanced-error-codes.md).

1. **Continúe con los flujos de decisiones:** Si la respuesta del extremo de perfiles contiene un perfil, la aplicación de transmisión utiliza la información del perfil para continuar con los flujos de decisiones subsiguientes.

1. **Indique un nuevo flujo de autenticación básico:** Si la respuesta del extremo de perfiles no contiene un perfil, la aplicación principal indica al usuario que debe iniciar un nuevo flujo de autenticación básico.
