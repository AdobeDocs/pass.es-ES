---
title: Restablecer pase temporal en iOS
description: Restablecer pase temporal en iOS
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---

# Restablecer pase temporal en iOS {#reset-temp-pass-on-ios}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

La aplicación de demostración de iOS incluye una pantalla dedicada para restablecer el TTL de pase temporal. Se requiere la siguiente información para la operación de restablecimiento:

- **Entorno:** especifica el extremo del servidor de pase de TV de pago de Adobe que recibirá la llamada de red de Pase temporal restablecida. Valores posibles: **Prequal** (*mgmt-prequal.auth-staging.adobe.com*), **Release** (*mgmt.auth.adobe.com*) o **Custom** (reservado para pruebas internas de Adobe).
- **Token de portador de OAuth2:** el token de OAuth2 es necesario para autorizar al programador para la autenticación de Adobe de TV de pago. Este token se puede obtener del extremo OAuth2 de autenticación de Pay-TV dedicado (por ejemplo, *curl -u &quot;\&lt;consumer\_key\>:\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken?grant\_type=client\_credentials&quot;*).
- **Id. de solicitante:** es el id. único del programador actual. Este valor se lee desde la pantalla principal de la aplicación de demostración (el campo del solicitante).
- **Id. de pase temporal:** es el Id. único de la MVPD de pase temporal.
- **ID de dispositivo:** ID de dispositivo con hash calculado por la aplicación de demostración.
- **Clave genérica:** algunas MVPD de Temp Pass (es decir, la siguiente funcionalidad de Temp Pass extensible) admiten una clave genérica para restablecer Temp Pass (junto con el ID del dispositivo).

Todos los parámetros anteriores (excepto la *clave genérica*) son obligatorios. A continuación, se muestra un ejemplo de los parámetros y la llamada de red asociada que realizará la aplicación de demostración (el ejemplo tiene la forma de un comando *curl *):

- **Entorno:** versión (*mgmt.auth.adobe.com*)
- **Token de portador de OAuth2:** H4j7cF3GtJX81BrsgDa10GwSizVz
- **Id. de programador:** REF
- **Id. de pase temporal:** TempPassREF
- **ID del dispositivo:** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **Clave genérica:** nula (no se proporcionó ningún valor)

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

se realizará una solicitud HTTP del DELETE al extremo **/reset**, pasando el *token de portador de OAuth2* en el encabezado Autorización y el *ID de dispositivo*, *ID de solicitante* y *ID de pase temporal (ID de MVPD)* como parámetros.

Si el programador proporciona un valor para la *clave genérica*, se realizará otra llamada HTTP (esta vez al extremo **/reset/generic**), pasando la *clave genérica* dentro del parámetro de solicitud *key*.

Por ejemplo, si establece la *clave genérica* en un hash de dirección de correo electrónico (para
Las MVPD de pase temporal (que admiten este tipo de funcionalidad) producirán el
siguiente llamada HTTP (el correo electrónico es `user@domain.com` su SHA-256)
hash es `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

Es importante resaltar que el restablecimiento de Temp Pass en la aplicación de demostración puede no tener el mismo efecto para una aplicación de programador en el mismo dispositivo. Esto se debe a que el ID del dispositivo (calculado por la aplicación de demostración y el AccessEnabler) puede no ser el mismo para todas las aplicaciones del dispositivo:

- iOS 6 y versiones posteriores: el ID del dispositivo se calcula mediante la dirección de MAC (que es única para todas las aplicaciones), por lo que al restablecer Temp Pass en la aplicación de demostración se restablecerá en todas las demás aplicaciones de programador del dispositivo.

- iOS 7 y superior: el ID del dispositivo se calcula en función del valor IDFV (ID del proveedor), que es único para todas las aplicaciones que tienen el mismo prefijo de ID de paquete (es decir, todos los componentes excepto el último). Dado que la aplicación de demostración y una aplicación de programador tienen diferentes ID de paquete, el restablecimiento de la Temp Pass en la aplicación de demostración no tendrá ningún efecto en una aplicación de programador.

El último caso de uso (iOS 7 y versiones posteriores) es el más común, así que veamos cómo los programadores pueden restablecer Temp Pass para sus aplicaciones en esta situación. Hay varias opciones:

1. Transfiera el código de la aplicación de demostración a la aplicación del programador. Las clases *TempPassResetViewController* y *DeviceIdDemoApp* contienen la lógica principal para restablecer la Temp Pass, y pueden modificarse e incluirse fácilmente en la aplicación Programador.

1. Ejecute la solicitud HTTP para restablecer la aprobación temporal con *curl*. El parámetro device\_Id se puede obtener calculando el IDFV de la aplicación Programmer y aplicándole un hash SHA-256 (código de ejemplo en la clase *DeviceIdDemoApp*).

1. Simplemente realice el restablecimiento desde la aplicación de demostración especificando el IDFV con hash de la aplicación del programador como *Clave genérica*. Esto resultará en dos llamadas de red: una para restablecer Temp Pass para la aplicación de demostración (irrelevante para el programador) y otra para restablecer Temp Pass para la aplicación del programador.

Todas las opciones anteriores son similares, depende del Programador elegir una en función de la facilidad de implementación.
