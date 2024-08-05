---
title: Perfiles básicos - Aplicación secundaria - Flujo
description: API REST V2 - Perfiles básicos - Aplicación secundaria - Flujo
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---


# Flujo de perfiles básicos realizado en la aplicación secundaria {#basic-profiles-flow-secondary-application}

El **flujo de perfiles** dentro del derecho de autenticación de Adobe Pass permite que la aplicación secundaria acceda a la información sobre los inicios de sesión activos de los usuarios.

El flujo de perfiles básicos le permite consultar los siguientes escenarios:

* [Recuperar perfil para código específico](#retrieve-profile-for-specific-code)

## Recuperar perfil para código específico {#retrieve-profile-for-specific-code}

### Requisitos previos {#prerequisites-retrieve-profile-for-specific-code}

Antes de recuperar el perfil de un código de autenticación específico, asegúrese de que se cumplen los siguientes requisitos previos:

* La aplicación secundaria, que tiene un(a) `code` utilizado(a) para realizar la autenticación interactiva con la MVPD, desea recuperar el perfil para un código de autenticación específico.

### Flujo de trabajo {#workflow-retrieve-profile-for-specific-code}

Siga los pasos dados para implementar el flujo básico de recuperación de perfiles para un código de autenticación específico realizado dentro de una aplicación secundaria, como se muestra en el diagrama siguiente.

![Recuperar perfil para código específico](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*Recuperar perfil para código específico*

1. **Recuperar perfil para código específico:** La aplicación secundaria recopila todos los datos necesarios para recuperar información de perfil para ese código de autenticación específico enviando una solicitud al extremo de perfiles.

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

1. **Indique que el flujo de autenticación ha finalizado correctamente:** Si la respuesta del extremo de perfiles contiene un perfil, la aplicación secundaria procesa la respuesta y puede utilizarla para mostrar opcionalmente un mensaje específico en la interfaz de usuario.

1. **Indica que el flujo de autenticación encontró un problema:** Si la respuesta del extremo de perfiles no contiene un perfil, la aplicación secundaria procesa la respuesta y puede utilizarla para mostrar opcionalmente un mensaje específico en la interfaz de usuario.
