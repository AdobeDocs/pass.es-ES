---
title: SSO en iOS al utilizar el Habilitador de acceso a autenticación de Adobe Pass
description: SSO en iOS al utilizar el Habilitador de acceso a autenticación de Adobe Pass
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# SSO en iOS al utilizar el Habilitador de acceso a autenticación de Adobe Pass {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

## Información general

El inicio de sesión único (SSO) entre aplicaciones con autenticación de Adobe Pass funciona de diferentes maneras según el sistema operativo subyacente.

Este documento aborda el **SSO en iOS**, al usar el habilitador de acceso **Autenticación de Adobe Pass**.

**Habilitador de acceso** **1.10** es la última versión del SDK nativo de Adobe Pass Authentication iOS. El Adobe recomienda encarecidamente pasar a esta versión en lugar de quedarse con una versión anterior. Si usa una versión anterior de Access Enabler, puede descargar la versión más reciente [aquí](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

El SSO en iOS viene determinado por las siguientes condiciones:

- Las aplicaciones deben usar el mismo **almacenamiento de tokens** (en forma de una mesa de trabajo personalizada creada por el Access Enabler).
- Las aplicaciones deben generar el mismo **ID del dispositivo** (el Habilitador de acceso calcula el ID del dispositivo en función de la dirección de MAC o el IDFV, según la versión del sistema operativo).

## Comportamiento

El comportamiento de SSO es el siguiente:

- **iOS 6 y versiones posteriores**: El SSO funciona automáticamente entre aplicaciones desarrolladas por el mismo equipo o equipos diferentes. El ID del dispositivo se calcula en función de la dirección de MAC (el mismo valor se genera en todas las aplicaciones) y el área de almacenamiento es común a todas las aplicaciones (la mesa de trabajo personalizada se puede compartir entre las aplicaciones en iOS 6 y versiones posteriores).
   - **Importante:** Tenga en cuenta que la versión 1.9.4 del SDK para iOS ha [aumentado el objetivo mínimo de implementación de iOS a iOS 7.](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7 y versiones posteriores**: SSO funcionará en las siguientes condiciones:

1. Las aplicaciones se publican con el mismo perfil de distribución de Apple o perfiles que pertenecen al mismo equipo. Esta es la única manera en que las aplicaciones comparten paneles de trabajo personalizados en iOS 7 y versiones posteriores. En todos los demás casos, la mesa de trabajo se coloca en una zona protegida por aplicación. Desde [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html): \+\[`UIPasteboard pasteboardWithName:create:\`] y +\[`UIPasteboard pasteboardWithUniqueName`\] ahora el nombre dado es único para permitir que solo aquellas aplicaciones del mismo grupo de aplicaciones tengan acceso a la mesa de trabajo. Si el desarrollador intenta crear una mesa de trabajo con un nombre que ya existe y no forma parte del mismo grupo de aplicaciones, obtendrá su propia mesa de trabajo única y privada. Tenga en cuenta que esto no afecta al sistema, a los paneles de trabajo que se proporcionan, a las herramientas generales y a los buscadores.

1. Las aplicaciones tienen el mismo prefijo de ID de paquete (todos los componentes excepto el último). Solo las aplicaciones que comparten el mismo prefijo de ID de paquete calcularán el mismo IDFV. Desde [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor): en IOS 7, se usan todos los componentes del paquete excepto el último componente para generar el ID del proveedor. Si el ID de paquete solo tiene un componente, se utiliza el ID de paquete completo.

Ahora vamos a centrarnos en el escenario **de**&#39;iOS 7 y versiones posteriores&#39;, ya que es el más frecuente para usuarios reales:

Ambas condiciones (compartir un perfil del mismo equipo de desarrollo y tener un prefijo de identificador de paquete común) son condiciones obligatorias para SSO.

Estas son las combinaciones posibles y sus resultados:

- **Perfiles del mismo equipo y del mismo prefijo de ID de paquete**: las aplicaciones compartirán el mismo almacenamiento de mesa de trabajo y tendrán el mismo ID de dispositivo (IDFV). Un usuario solo tendrá que autenticarse una vez (en la primera aplicación utilizada) y el estado de autenticación se compartirá entre todas las demás aplicaciones. Flujo de ejemplo:

1. El usuario abre la aplicación A (con ID de paquete *com.x.y.AppA*) y no se autentica
1. El usuario realiza la autenticación en la aplicación A
1. El usuario abre la aplicación B (con ID de paquete *com.x.y.AppB*) y se autentica automáticamente compartiendo los datos de asignación de derechos de la aplicación
A (del paso 2)
1. El usuario abre la aplicación A y sigue estando autenticado (desde el paso 2)



- **Perfiles del mismo equipo pero con diferentes prefijos de ID de paquete**: las aplicaciones compartirán el mismo almacenamiento de mesa de trabajo, pero tendrán ID de dispositivo (IDFV) diferentes. Un usuario deberá autenticarse una vez por cada aplicación. Flujo de ejemplo:

1. El usuario abre la aplicación A (con ID de paquete *com.x.y.AppA*) y no se autentica
1. El usuario realiza la autenticación en la aplicación A
1. El usuario abre la aplicación B (con ID de paquete *com.z.AppB*) y el activador de acceso detecta el token obtenido por la primera aplicación (porque el almacenamiento está compartido), pero no intentará utilizarlo mediante SSO debido a que los ID de dispositivo son diferentes
1. El usuario realiza la autenticación en la aplicación B
1. El usuario abre la aplicación A y sigue estando autenticado (desde el paso 2)



- **Perfiles de equipos diferentes (el ID del paquete no es relevante en este caso)**: las aplicaciones tendrán diferentes almacenes de mesa de trabajo y se deshabilitará el SSO entre ellos. Un usuario deberá autenticarse una vez por cada aplicación y las sesiones de autenticación permanecerán persistentes al cambiar entre aplicaciones. Flujo de ejemplo:


1. El usuario abre la aplicación A y no está autenticado
1. El usuario realiza la autenticación en la aplicación A
1. El usuario abre la aplicación B y no está autenticado
1. El usuario realiza la autenticación en la aplicación B
1. El usuario abre la aplicación A y se autentica (desde el paso 2)
1. El usuario abre la aplicación B y se autentica (desde el paso 4)

**Nota:** Tenga en cuenta que las condiciones anteriores para SSO son aplicables al instalar aplicaciones a través de **Apple App Store**. Si las aplicaciones se implementan en un simulador (donde no se aplica la firma de aplicaciones), se instalan con Xcode o se distribuyen a través de un perfil ad hoc, puede obtener resultados diferentes.

**Importante:** Nota (**con respecto a AccessEnabler v1.8**): El segundo escenario descrito anteriormente (perfiles del mismo equipo pero con diferentes prefijos de ID de paquete) creará una experiencia de usuario muy desagradable para los usuarios de **AccessEnabler v1.8** en todas las aplicaciones desarrolladas por el mismo equipo (empresa de medios). Se cerrará la sesión del usuario automáticamente al realizar la transición entre aplicaciones de la misma empresa de medios, por lo que los desarrolladores de aplicaciones deben tener cuidado al decidir el ID del paquete y el perfil de distribución. El escenario exacto en este caso se presenta a continuación:

Las aplicaciones compartirán el mismo almacenamiento de mesa de trabajo, pero tendrán ID de dispositivo (IDFV) diferentes. Un usuario tendrá que autenticarse una vez por cada aplicación, **pero las sesiones de autenticación se borrarán al cambiar entre aplicaciones**. Flujo de ejemplo:

1. El usuario abre la aplicación A (con ID de paquete *com.x.y.AppA*) y no se autentica
1. El usuario realiza la autenticación en la aplicación A
1. El usuario abre la aplicación B (con el ID de paquete *com.z.AppB*) y el activador de acceso (mecanismo de seguridad que detecta un conflicto entre el ID de dispositivo calculado actualmente en la aplicación B y el almacenado en los tokens de derechos creados por la aplicación A) borra automáticamente los datos de derechos creados por la aplicación A
1. El usuario realiza la autenticación en la aplicación B
1. El usuario abre la aplicación A, y el activador de acceso borra automáticamente los datos de derechos creados por la aplicación B (mecanismo de seguridad que detecta un conflicto entre el ID de dispositivo calculado actualmente en la aplicación A y el almacenado en los tokens de derechos creados por la aplicación B)
