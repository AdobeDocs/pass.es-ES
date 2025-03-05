---
title: 'Encabezado: X-Device-Info'
description: 'API de REST V2: encabezado: X-Device-Info'
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 2%

---

# Encabezado: X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de la solicitud <b>X-Device-Info</b> contiene la información del cliente (dispositivo, conexión y aplicación) relacionada con el dispositivo de flujo continuo real.

## Sintaxis {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;device_information&gt;</td>
   </tr>
   <tr>
      <td>Tipo de encabezado</td>
      <td>Encabezado de solicitud</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>No</td>
   </tr>
</table>

## Directivas {#directives}

<b>&lt;device_information></b>

El valor `Base64-encoded` del elemento JSON que contiene al menos los atributos marcados como requeridos por la siguiente tabla.

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Presencia</th>
        <th style="background-color: #EFF2F7; width: 15%;">Clave</th>
        <th style="background-color: #EFF2F7;">Descripción</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Restringido</th>
        <th style="background-color: #EFF2F7;">Valores posibles</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwareType</td>
        <td>El tipo de hardware principal del dispositivo.</td>
        <td>&amp;check;</td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Cámara</li>
                <li>DataCollectionTerminal</li>
                <li>Escritorio</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>Gafas</li>
                <li>MediaPlayer</li>
                <li>MobilePhone</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tableta</li>
                <li>WirelessHotspot</li>
                <li>Reloj de pulsera</li>
                <li>Desconocido</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obligatorio</i></td>
        <td>model</td>
        <td>El nombre del modelo del dispositivo.</td>
        <td></td>
        <td>por ejemplo, iPhone, SM-G930V, AppleTV, etc.</td>
    </tr>
    <tr>
        <td><i>obligatorio</i></td>
        <td>version</td>
        <td>La versión del dispositivo.</td>
        <td></td>
        <td>p. ej., 2.0.1, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>fabricante</td>
        <td>La empresa u organización de fabricación del dispositivo.</td>
        <td></td>
        <td>Por ejemplo: Samsung, LG, ZTE, Huawei, Motorola, Apple, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>vendedor</td>
        <td>La empresa u organización vendedora del dispositivo.</td>
        <td></td>
        <td>por ejemplo, Apple, Samsung, LG, Google, etc.</td>
    </tr>
    <tr>
        <td><i>obligatorio</i></td>
        <td>osName</td>
        <td>El nombre del sistema operativo (SO) del dispositivo.</td>
        <td>&amp;check;</td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Android</li>
                <li>SO CHROME</li>
                <li>Linux</li>
                <li>SO MAC</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>El nombre del grupo del sistema operativo (SO) del dispositivo.</td>
        <td>&amp;check;</td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation OS</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>El proveedor del sistema operativo (SO) del dispositivo.</td>
        <td>&amp;check;</td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Proyecto Tizen</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obligatorio</i></td>
        <td>osVersion</td>
        <td>Versión del sistema operativo (SO) del dispositivo.</td>
        <td></td>
        <td>p. ej. 10.2, 9.0.1, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>El nombre del explorador.</td>
        <td>&amp;check;</td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Navegador Android</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Navegador Symbian</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>La empresa u organización que crea el explorador.</td>
        <td>&amp;check;</td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>La versión del explorador del dispositivo.</td>
        <td></td>
        <td>p. ej. 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>El agente de usuario del dispositivo.</td>
        <td></td>
        <td>Por ejemplo, Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, como Gecko) Versión/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>Ancho de pantalla físico del dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>Altura física de la pantalla del dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>La densidad de píxeles de la pantalla física del dispositivo.</td>
        <td></td>
        <td>p.ej. 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>Dimensión diagonal de la pantalla física del dispositivo en pulgadas.</td>
        <td></td>
        <td>p. ej. 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>IP del dispositivo utilizada para enviar solicitudes HTTP.</td>
        <td></td>
        <td>p. ej., 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>Puerto del dispositivo utilizado para enviar solicitudes HTTP.</td>
        <td></td>
        <td>p. ej. 53124</td>
    </tr>
    <tr>
        <td><i>obligatorio</i></td>
        <td>connectionType</td>
        <td>Tipo de conexión de red.</td>
        <td></td>
        <td>por ejemplo, WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>Estado de seguridad de la conexión de red.</td>
        <td>&amp;check;</td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>true: en el caso de una red segura</li>
                <li>false: en el caso de un punto interactivo público</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>El identificador único de la aplicación.</td>
        <td></td>
        <td>p. ej. REF30</td>
    </tr>
</table>


## Ejemplos {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## Libros {#cookbooks}

>[!IMPORTANT]
> 
> Los fragmentos de código y los recursos de documentación se proporcionan con fines de referencia.
> 
> Los fragmentos de código no son exhaustivos y pueden requerir modificaciones adicionales para funcionar en el proyecto.
>
> Independientemente de su implementación real, el encabezado `X-Device-Info` debe contener un valor con el formato descrito en la sección [Directivas](#directives).

### Navegadores {#browsers}

Para las aplicaciones cliente que se ejecutan en un explorador, se puede omitir el encabezado `X-Device-Info`, ya que el explorador enviará automáticamente un conjunto mínimo de información necesaria en el encabezado `User-Agent`.

Puede seguir utilizando el encabezado `X-Device-Info` para proporcionar información adicional sobre el dispositivo, la conexión y la aplicación, en caso de que la aplicación cliente integre una biblioteca o servicio que proporcione un mecanismo de identificación del dispositivo.

### Dispositivos móviles {#mobile-devices}

#### iOS y iPadOS {#ios-ipados}

Para generar el encabezado `X-Device-Info` para los dispositivos que ejecutan [iOS o iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes), puede consultar los siguientes documentos y el siguiente fragmento de código:

* Documentación para desarrolladores de Apple para [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Documentación para desarrolladores de Apple para [Reachability](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Documentación manual de Linux para [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

La información del dispositivo se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|------------------------|-----------------|
| model | uname.machine | iPhone |
| vendedor | codificado | Apple |
| fabricante | codificado | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10,2 |

La información de conexión se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


La información de la aplicación se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

#### Android {#android}

Para generar el encabezado `X-Device-Info` para los dispositivos que ejecutan [Android](https://developer.android.com/about/versions), puede consultar los siguientes documentos y el siguiente fragmento de código:

* Documentación para desarrolladores de Android para la clase [Build](https://developer.android.com/reference/android/os/Build.html).

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

La información del dispositivo se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|-----------------------------|-----------------|
| model | Build.MODEL | GT-I9505 |
| vendedor | Build.BRAND | samsung |
| fabricante | Build.MANUFACTURER | samsung |
| version | Build.DEVICE | jflte |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | codificado | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

La información de conexión se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

La información de la aplicación se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

### Dispositivos conectados a TV {#tv-connected-devices}

#### tvOS {#tvos}

Para generar el encabezado `X-Device-Info` para los dispositivos que ejecutan [tvOS](https://developer.apple.com/documentation/tvos-release-notes), puede consultar los siguientes documentos y el siguiente fragmento de código:

* Documentación para desarrolladores de Apple para [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Documentación para desarrolladores de Apple para [Reachability](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Documentación manual de Linux para [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

La información del dispositivo se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|------------------------|-----------------|
| model | uname.machine | AppleTV |
| vendedor | codificado | Apple |
| fabricante | codificado | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10,2 |

La información de conexión se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|------------------|------------------------------------------|-----------------|
| connectionType | [Reachability currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

La información de la aplicación se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

#### Fire OS {#fireos}

Para generar el encabezado `X-Device-Info` para los dispositivos que ejecutan [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html), puede consultar los siguientes documentos:

* Documentación para desarrolladores de Android para la clase [Build](https://developer.android.com/reference/android/os/Build.html).
* Documentación para desarrolladores de Amazon para [Identificación de dispositivos Fire TV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html).

La información del dispositivo se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|-----------------------------|-----------------|
| model | Build.MODEL | AFTM |
| vendedor | Build.BRAND | Amazon |
| fabricante | Build.MANUFACTURER | Amazon |
| version | Build.DEVICE | montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | codificado | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

La información de conexión se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

La información de la aplicación se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

#### Roku OS {#rokuos}

Para generar el encabezado `X-Device-Info` para los dispositivos que ejecutan [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md), puede consultar los siguientes documentos:

* Documentación para desarrolladores de Roku para [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md).

La información del dispositivo se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|--------------------------------------------|-----------------|
| model | codificado | &quot;Roku&quot; |
| vendedor | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| fabricante | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | codificado | &quot;Roku&quot; |
| osVersion | ifDeviceInfo.getVersion() |                 |

La información de conexión se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | codificado | true si la conexión está cableada |

La información de la aplicación se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

### Otros {#others}

En el caso de las plataformas de dispositivo no incluidas en la documentación, la información del cliente (dispositivo, conexión y aplicación) debe estar vinculada a cualquier atributo de hardware y sistema operativo (SO) disponible, normalmente especificado en los manuales de hardware y sistema operativo del dispositivo.
