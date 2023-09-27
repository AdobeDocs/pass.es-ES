---
title: Restablecer pase temporal en iOS
description: Restablecer pase temporal en iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Restablecer pase temporal en iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

La aplicación de demostración de iOS incluye una pantalla dedicada para restablecer el TTL de pase temporal. Se requiere la siguiente información para la operación de restablecimiento:

- **Entorno:** especifica el punto final del servidor de pase de TV de pago de Adobe que recibirá la llamada de red de Pase temporal restablecida. Valores posibles: **Preigualar** (*mgmt-prequal.auth-staging.adobe.com*), **Versión** (*mgmt.auth.adobe.com*) o **Personalizado** (reservado para pruebas internas de Adobe).
- **Token de portador de OAuth2:** el token de OAuth2 es necesario para autorizar al programador para la autenticación de Adobe de TV paga. Este token se puede obtener del punto final OAuth2 de autenticación de TV paga dedicado (p. ej., *curl -u &quot;\&lt;consumer _key=&quot;&quot;>:\&lt;consumer _secret=&quot;&quot; _key=&quot;&quot;>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*).
- **ID de solicitante:** el ID único del programador actual. Este valor se lee desde la pantalla principal de la aplicación de demostración (el campo del solicitante).
- **Identificador de pase temporal:** el ID único de la MVPD Temp Pass.
- **ID de dispositivo:** ID de dispositivo con hash calculado por la aplicación de demostración.
- **Clave genérica:** algunas MVPD de Temp Pass (es decir, la siguiente funcionalidad extensible de Temp Pass) admiten una clave genérica para restablecer Temp Pass (junto con el ID del dispositivo).

Todos los parámetros anteriores (excepto el *Clave genérica*) son obligatorios. A continuación, se muestra un ejemplo de los parámetros y la llamada de red asociada que realizará la aplicación de demostración (el ejemplo tiene la forma de un comando *curl *):

- **Entorno:** Versión (*mgmt.auth.adobe.com*)
- **Token de portador de OAuth2:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **ID de programador:** REF
- **Identificador de pase temporal:** TempPassREF
- **ID de dispositivo:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **Clave genérica:** null (sin valor proporcionado)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

se realizará una solicitud HTTP del DELETE al **/reset** punto final, pasar el *Token de portador de OAuth2* en el encabezado Autorización y *ID de dispositivo*, *ID de solicitante* y *Identificador de pase temporal (ID de MVPD)* como parámetros.

Si el programador proporciona un valor para *Clave genérica*, se realizará otra llamada HTTP (esta vez al **/reset/generic** extremo), pasando el *Clave genérica* dentro de *key* parámetro de solicitud.

Por ejemplo, configurar la variable *Clave genérica* a un hash de dirección de correo electrónico (para las MVPD de paso temporal que admiten este tipo de funcionalidad) dará como resultado la siguiente llamada HTTP (el correo electrónico es `user@domain.com` su hash SHA-256 es `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Es importante resaltar que el restablecimiento de Temp Pass en la aplicación de demostración puede no tener el mismo efecto para una aplicación de programador en el mismo dispositivo. Esto se debe a que el ID del dispositivo (calculado por la aplicación de demostración y el AccessEnabler) puede no ser el mismo para todas las aplicaciones del dispositivo:

- iOS 6 y versiones posteriores: el ID del dispositivo se calcula mediante la dirección de MAC (que es única para todas las aplicaciones), por lo que al restablecer Temp Pass en la aplicación de demostración se restablecerá en todas las demás aplicaciones de programador del dispositivo.

- iOS 7 y superior: el ID del dispositivo se calcula en función del valor IDFV (ID del proveedor), que es único para todas las aplicaciones que tienen el mismo prefijo de ID de paquete (es decir, todos los componentes excepto el último). Dado que la aplicación de demostración y una aplicación de programador tienen diferentes ID de paquete, el restablecimiento de la Temp Pass en la aplicación de demostración no tendrá ningún efecto en una aplicación de programador.

El último caso de uso (iOS 7 y versiones posteriores) es el más común, así que veamos cómo los programadores pueden restablecer Temp Pass para sus aplicaciones en esta situación. Hay varias opciones:

1. Transfiera el código de la aplicación de demostración a la aplicación del programador. El *TempPassResetViewController* y *DeviceIdDemoApp* Las clases contienen la lógica principal para restablecer Temp Pass y se pueden modificar e incluir fácilmente en la aplicación del programador.

1. Ejecute la solicitud HTTP para restablecer la Temp Pass con *rizar*. El parámetro device\_Id se puede obtener calculando el IDFV de la aplicación Programador y aplicándole un hash SHA-256 (código de ejemplo en la variable *DeviceIdDemoApp* class).

1. Simplemente realice el restablecimiento desde la aplicación de demostración especificando el IDFV con hash de la aplicación del programador como *Clave genérica*. Esto resultará en dos llamadas de red: una para restablecer Temp Pass para la aplicación de demostración (irrelevante para el programador) y otra para restablecer Temp Pass para la aplicación del programador.

Todas las opciones anteriores son similares, depende del Programador elegir una en función de la facilidad de implementación.
