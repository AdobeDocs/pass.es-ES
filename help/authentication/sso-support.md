---
title: Compatibilidad con inicio de sesión único
description: Compatibilidad con inicio de sesión único
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%

---

# Compatibilidad con inicio de sesión único

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview-sso-support}

En este documento se describen los tipos de inicio de sesión único que admite y alimenta la autenticación de Adobe Pass en diferentes plataformas. El objetivo de este documento es arrojar luz sobre lo que se admite y lo que no, cuál es la cobertura de MVPD para cada método de SSO y qué se requiere de los programadores para poder beneficiarse de SSO en cada plataforma.

Una vez que un usuario inicia sesión con sus credenciales de MVPD, la autenticación de Adobe Pass genera un token seguro que representa la sesión de autenticación de MVPD y enlaza ese token al dispositivo del usuario mediante un ID de dispositivo. La autenticación de Adobe Pass almacena el token o el ID de dispositivo en un servidor o en el dispositivo. Esto permite a los usuarios introducir sus credenciales con menos frecuencia a la vez que mantiene las transacciones seguras.

>[!NOTE]
>
>Los flujos de trabajo de SSO forman parte del paquete Flujo de trabajo Premium. Póngase en contacto con su representante de ventas de Adobe Pass si está interesado en utilizar esta funcionalidad.

## Estado actual de SSO en varias plataformas {#current-sso-status-platforms}

| Plataforma/dispositivo | Compatibilidad con SSO | Tipo de SSO | Cobertura de MVPD | Notas |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Web (JavaScript) | Sí | Token de autenticación compartido (Adobe SSO) | Todo | No hay SSO entre exploradores. Siga las instrucciones de la Guía de integración del programador para JavaScript. Siguiendo las instrucciones, el SSO está habilitado de forma predeterminada.  Al habilitar la autenticación por solicitante se interrumpe el SSO |
| iOS | Sí | Platform SSO: intercambio de tokens | Según la compatibilidad con Apple, la lista está aquí | Desde iOS 10, Apple y Adobe introdujeron la funcionalidad SSO para los programadores participantes y las MVPD. Al utilizar el SDK de iOS de Adobe más reciente o la API de REST sin cliente de Adobe e implementar la funcionalidad de SSO de Apple, puede beneficiarse de SSO en dispositivos iOS. Obtenga más información sobre la implementación del SDK aquí y más detalles sobre la implementación sin cliente aquí. Notas adicionales: Si no desea utilizar el SSO de Apple, puede tener un SSO limitado entre aplicaciones del mismo proveedor (mismo ID de paquete) que puedan compartir almacenamiento y un ID (IDFV), por lo que el SSO está limitado únicamente a las aplicaciones del mismo proveedor. |
| Android | Sí | Token de autenticación compartido (Adobe SSO) | Todo | Si el usuario no acepta la solicitud de permiso WRITE_EXTERNAL_STORAGE, la biblioteca utilizará un almacenamiento local limitado. Esto implica que no habrá SSO entre diferentes aplicaciones al utilizar el almacenamiento local. |
| tvOS - nuevo Apple TV | Sí | Platform SSO: intercambio de tokens | Según la compatibilidad con Apple, la lista está aquí | Desde tvOS 10, Apple y Adobe introdujeron la funcionalidad SSO para los programadores participantes y las MVPD. Al utilizar el SDK de tvOS de Adobe más reciente o la API de REST sin cliente de Adobe e implementar la funcionalidad de SSO de Apple, puede beneficiarse de SSO en dispositivos tvOS. Más detalles sobre el SDK de tvOS: aquí y aquí, y más detalles sobre la implementación sin cliente aquí. |
| Roku | Sí | Token de autenticación compartido (Adobe SSO) | La lista completa de cobertura significativa se proporcionará pronto. | Roku SSO funciona de forma predeterminada con la API sin cliente para todos los clientes, respetando las directrices de Roku, no se requiere implementación especial. SSO se basa en la información de identificación del dispositivo que Roku envía de forma segura al Adobe. |
| Amazon FireTV | Sí | Token de autenticación compartido (Adobe SSO) | La lista completa de cobertura significativa se proporcionará pronto. | El SDK de FireTV proporciona compatibilidad con el inicio de sesión único en función de las capacidades de Android. El SSO en esta plataforma solo es posible entre aplicaciones que utilicen el SDK de Adobe FireTV por ahora. Obtenga más información acerca del nuevo SDK de FireTV aquí. Las aplicaciones FireTV implementadas sobre la API sin cliente podrán beneficiarse del SSO en el año 2018. |
| Xbox 360 | No |                                         |                                                     | No hay ningún ID de dispositivo que podamos aprovechar. Hay un ID de aplicación, por lo que los usuarios no tienen que autenticarse cada vez. |
| Xbox One | No |                                         |                                                     | No hay ningún ID de dispositivo que podamos aprovechar. Hay un ID de aplicación, por lo que los usuarios no tienen que autenticarse cada vez. |
| Windows 8/10 | No |                                         |                                                     | No hay ningún ID de dispositivo que podamos aprovechar. Hay un ID de aplicación, por lo que los usuarios no tienen que autenticarse cada vez. |
| Televisores Samsung | No |                                         |                                                     | No hay ningún ID de dispositivo que podamos aprovechar. Hay un ID de aplicación, por lo que los usuarios no tienen que autenticarse cada vez. |

### Notas sobre Xbox 360 y Xbox One {#notes-xbox-360}

* **Xbox 360**- Xbox 360 depende del servicio en vivo para proporcionar el token que incrusta el ID del dispositivo. Las capas del servicio activo en el valor appID para deviceID, lo que lo convierte en ámbito solo de la aplicación. Para Xbox 360, Microsoft proporcionó al Adobe una biblioteca Java para ayudarle a analizar el token.

* **Xbox One**: se emitirá un token web JSON cifrado con el certificado o la clave del editor y firmado por Microsoft. El Adobe extrae el deviceID de un parámetro denominado DPI (ID de dispositivo por pares), distinto del PDID del parámetro de Xbox 360 (ID de dispositivo asociado). El PDID también existe en Xbox One, pero está pensado para ser reemplazado por este nuevo parámetro &quot;ID de dispositivo en pares&quot; (DPI).


### Desactivación de SSO {#disable-sso}

En determinadas situaciones, algunas aplicaciones o sitios querrán deshabilitar el SSO para satisfacer los casos comerciales avanzados.

* **Para JS y SDK nativos**: el equipo de soporte de autenticación de Adobe Pass puede deshabilitar SSO para un par ID de solicitante/MVPD. No es necesario trabajar en los sitios ni en las aplicaciones nativas.  Una vez que el equipo de soporte de Autenticación de Adobe Pass ha deshabilitado el SSO, las autenticaciones realizadas mediante el par RequestorId / MVPD especificado no se compartirán con sitios o aplicaciones que utilicen ID de solicitante diferentes. Además, las autenticaciones existentes con distintos ID de solicitante no serán válidas para la combinación ID de solicitante/MVPD en la que se deshabilitó SSO. Técnicamente, la desactivación de SSO se realiza enlazando el token AuthN a la combinación específica de ID de solicitante/MVPD.
* **Para la API sin cliente**: puede deshabilitar el SSO en el flujo de autenticación sin cliente especificando un parámetro appId no vacío en las llamadas REST. Puede utilizar cualquier cadena como valor, siempre que esa cadena sea única para el ID del solicitante. Tenga en cuenta que para la API sin cliente, el programador/implementador debe cambiar el sitio o la aplicación para añadir este parámetro específico del solicitante.

>[!IMPORTANT]
>
>NOTA IMPORTANTE PARA SSO DE API SIN CLIENTE: Algunas MVPD requieren que cada red (ID de solicitante) realice su propio flujo de autenticación. Para los flujos basados en SDK (iOS, etc.), el SDK lo gestiona automáticamente. Sin embargo, para las API sin cliente, esto debe gestionarlo el programador. Recomendamos a los programadores que no habiliten flujos SSO para API sin cliente en este momento y que, en su lugar, utilicen una combinación ID de dispositivo + ID de aplicación para el ID de dispositivo. Adobe también trabajará en la mejora de los flujos de API sin cliente para que se pueda establecer un SSO adecuado.

### Cerrar sesión {#logout-sso-support}

Los programadores deben tener en cuenta que la acción &quot;Cerrar sesión&quot; en el contexto del inicio de sesión único, cuando se realiza en una aplicación o en un sitio, eliminará todos los tokens en el dispositivo y se cerrará la sesión del usuario en todas las aplicaciones o sitios.

Si se cumplen las condiciones de SSO (independientemente de si el SSO está habilitado o deshabilitado), se cerrará la sesión y se eliminará toda la información de autenticación y autorización.
