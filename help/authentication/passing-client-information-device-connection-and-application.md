---
title: Pasar información del cliente (dispositivo, conexión y aplicación)
description: Pasar información del cliente (dispositivo, conexión y aplicación)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1643'
ht-degree: 1%

---

# Pasar información del cliente (dispositivo, conexión y aplicación) {#pass-client-info}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


## Ámbito {#pass-client-info-scope}

Este documento agrega detalles y libros de cocina para pasar información del cliente (dispositivo, conexión y aplicación) desde una aplicación de programador a las API de REST de autenticación de Adobe Pass o a los SDK.

Las ventajas de proporcionar información al cliente son las siguientes:

* La capacidad de habilitar correctamente la autenticación de base doméstica (HBA) en el caso de algunos tipos de dispositivos y MVPD que pueden admitir HBA.
* La capacidad de aplicar correctamente los TTL en el caso de algunos tipos de dispositivos (por ejemplo, configurar TTL más largos para sesiones de autenticación en dispositivos conectados a TV).
* La capacidad de acumular correctamente métricas empresariales en informes desglosados por tipo de dispositivo mediante la Monitorización del servicio de derecho (ESM).
* Desbloquea la capacidad de aplicar correctamente varias reglas empresariales (por ejemplo, degradación) en tipos de dispositivos específicos.

## Información general {#pass-client-info-overview}

La información del cliente consta de:

* **Dispositivo** contiene información sobre los atributos de hardware y software del dispositivo desde el que el usuario intenta consumir el contenido del programador.
* **Conexión** información sobre los atributos de conexión del dispositivo desde el que el usuario se conecta a los servicios de autenticación de Adobe Pass o a los servicios del programador (por ejemplo, implementaciones servidor a servidor).
* **Aplicación** información sobre la aplicación registrada desde la que el usuario intenta consumir el contenido del programador.

La información del cliente es un objeto JSON creado con claves presentadas en la siguiente tabla.

>[!NOTE]
>
>Las siguientes **claves** son **obligatorias** para enviarlas en el objeto JSON de información del cliente: **modelo**, **osName**.
>
>Las siguientes claves tienen **valores restringidos**: `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Clave | Restringido | Descripción | Valores posibles |
|---|---|---|---|---|
|            | primaryHardwareType | # Sí | El tipo de hardware principal del dispositivo. | # Los valores están restringidos:                                                                     Cámara                                                      DataCollectionTerminal                                                      Escritorio                                                      EmbeddedNetworkModule                                                      eReader                                                      GamesConsole                                                      GeolocationTracker                                                      Gafas                                                      MediaPlayer                                                      MobilePhone                                                      PaymentTerminal                                                      PluginModem                                                      SetTopBox                                                      TV                                                      Tableta                                                      WirelessHotspot                                                      Reloj de pulsera                                                      Desconocido |
| #mandatory | model | No | El nombre del modelo del dispositivo. | por ejemplo, iPhone, SM-G930V, AppleTV, etc. |
|            | version | No | La versión del dispositivo. | p. ej., 2.0.1, etc. |
|            | fabricante | No | La empresa u organización fabricante del dispositivo. | Por ejemplo: Samsung, LG, ZTE, Huawei, Motorola, Apple, etc. |
|            | vendedor | No | La empresa u organización vendedora del dispositivo. | por ejemplo, Apple, Samsung, LG, Google, etc. |
| #mandatory | osName | # Sí | El nombre del sistema operativo (SO) del dispositivo. | # Los valores están restringidos:                                                   Android                   SO CHROME                   Linux                   SO MAC                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | Sí | El nombre del grupo del sistema operativo (SO) del dispositivo. | # Los valores están restringidos:                                                   Android                   BSD                   Linux                   PlayStation OS                   Roku OS                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | No | El proveedor del sistema operativo (SO) del dispositivo. | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   Nintendo                   Nokia                   Roku                   Samsung                   Sony                   Proyecto Tizen |
|            | osVersion | No | La versión del sistema operativo (SO) del dispositivo. | p. ej. 10.2, 9.0.1, etc. |
|            | browserName | # Sí | El nombre del explorador. | # Los valores están restringidos:                                                   Navegador Android                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   SeaMonkey                   Navegador Symbian |
|            | browserVendor | # Sí | La empresa u organización que crea el explorador. | # Los valores están restringidos:                                                   Amazon                   Apple                   Google                   Microsoft                   Motorola                   Mozilla                   Netscape                   Nintendo                   Nokia                   Samsung                   Sony Ericsson |
|            | browserVersion | No | La versión del explorador del dispositivo. | p. ej. 60.0.3112 |
|            | userAgent | No | El agente de usuario del dispositivo. | Por ejemplo, Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, como Gecko) Versión/10.0.3 Safari/602.4.8 |
|            | displayWidth | No | Ancho de pantalla físico del dispositivo. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | No | Altura de la pantalla física del dispositivo. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | No | La densidad de píxeles de la pantalla física del dispositivo. | p.ej. 294 |
|            | diagonalScreenSize | No | Dimensión diagonal de la pantalla física del dispositivo en pulgadas. | p. ej. 5.5, 10.1 |
|            | connectionIp | No | IP del dispositivo que se utiliza para enviar solicitudes HTTP. | p. ej., 8.8.4.4 |
|            | connectionPort | No | Puerto del dispositivo utilizado para enviar solicitudes HTTP. | p. ej. 53124 |
|            | connectionType | No | Tipo de conexión de red. | por ejemplo, WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Sí | Estado de seguridad de la conexión de red. | # Los valores están restringidos:                                                   true: en el caso de una red segura                   false: en el caso de un punto interactivo público |
|            | applicationId | No | El identificador único de la aplicación. | Por ejemplo, CNN |

## Referencias de API {#api-ref}

Esta sección presenta la API responsable de administrar la información del cliente cuando se utilizan las API de REST de autenticación de Adobe Pass o los SDK.

### API DE REST {#rest-api}

Los servicios de autenticación de Adobe Pass admiten la recepción de la información del cliente de las siguientes maneras:

* Como **encabezado: &quot;X-Device-Info&quot;**
* Como **parámetro de consulta: &quot;device_info&quot;**
* Como **parámetro de publicación: &quot;device_info&quot;**

>[!IMPORTANT]
>
>En los tres casos, la carga útil del encabezado o parámetro debe estar codificado en **Base64 y la dirección URL debe estar codificada**.

**SDK**

#### SDK de JavaScript {#js-sdk}

El SDK de JavaScript de AccessEnabler crea de forma predeterminada un objeto JSON de información del cliente, que se pasará a los servicios de autenticación de Adobe Pass, a menos que se anule.

El SDK de JavaScript de AccessEnabler admite **anular solamente** la clave &quot;applicationId&quot; del objeto JSON de información del cliente mediante el parámetro de opciones [setRequestor](/help/authentication/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options)) *applicationId*.

>[!CAUTION]
>
>El valor del parámetro `applicationId` debe ser un valor de cadena de texto sin formato.
>Si la aplicación Programador decide pasar el applicationId, el SDK de JavaScript de AccessEnabler seguirá calculando el resto de las claves de información del cliente.

#### SDK de iOS/tvOS {#ios-tvos-sdk}

El SDK de AccessEnabler para iOS/tvOS crea de forma predeterminada un objeto JSON de información del cliente, que se pasará a los servicios de autenticación de Adobe Pass, a menos que se anule.

El SDK de AccessEnabler para iOS/tvOS admite que **se anule todo** el objeto JSON de información del cliente a través del parámetro device_info de [setOptions](/help/authentication/iostvos-sdk-api-reference.md#setoptions).

>[!CAUTION]
>
>El valor del parámetro *device_info* debe ser un valor **Base64 encoded** *NSString*.
>
>Si la aplicación Programador decide pasar *device_info*, se anularán todas las claves de información de cliente calculadas por el SDK de AccessEnabler para iOS/tvOS. Por lo tanto, es muy importante calcular y pasar los valores para tantas claves como sea posible. Para obtener más información sobre la implementación, consulte la tabla [Información general](#pass-client-info-overview) y el [manual de iOS/tvOS](#ios-tvos).

#### SDK de Android/FireOS {#and-fire-os-sdk}

El SDK de Android/FireOS `AccessEnabler` crea de forma predeterminada un objeto JSON de información de cliente, que se pasará a los servicios de autenticación de Adobe Pass, a menos que se anule.

El SDK de Android/FireOS `AccessEnabler` admite **la anulación del objeto JSON de información de cliente** completo a través del parámetro `device_info` de [setOptions](/help/authentication/android-sdk-api-reference.md#setOptions)/[setOptions](/help/authentication/amazon-fireos-native-client-api-reference.md#fire_setOption).

>[!NOTE]
>
>El valor del parámetro `device_info` debe ser un valor de cadena **codificado en Base64**.

>[!IMPORTANT]
>
>Si la aplicación Programador decide pasar `device_info`, se anularán todas las claves de información de cliente calculadas por el SDK Android/FireOS de `AccessEnabler`. Por lo tanto, es muy importante calcular y pasar los valores para tantas claves como sea posible. Para obtener más información sobre la implementación, consulte la tabla [Información general](#pass-client-info-overview) y los manuales de [Android](#android) y [FireOS](#fire-tv).

## Libros {#cookbooks}

Esta sección presenta un libro de cocina para crear el objeto JSON de información del cliente en el caso de diferentes tipos de dispositivos.

>[!IMPORTANT]
>
>¡Las claves que están marcadas con **!** son obligatorios para ser enviados.

### Android {#android}

La información del dispositivo se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---------------|-----------------------------|---------------|
| ! | model | Build.MODEL | GT-I9505 |
|   | vendedor | Build.BRAND | samsung |
|   | fabricante | Build.MANUFACTURER | samsung |
| ! | version | Build.DEVICE | jflte |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | codificado | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

La información de conexión se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

La información de la aplicación se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---------------|-----------|--------------|
|   | applicationId | codificado | CNN |

>[!IMPORTANT]
>
La información del dispositivo, la conexión y la aplicación debe agregarse al mismo objeto JSON. Después, el objeto resultante debe estar **codificado en Base64**. Además, en el caso de las API de REST de autenticación de Adobe Pass, el valor debe estar **codificado en la dirección URL**.

**Código de muestra**

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
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
**Recursos:**
* clase pública [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} en la documentación para desarrolladores de Java.

### FireTV {#fire-tv}

La información del dispositivo se puede construir de la siguiente manera:

|   | Clave | Source | Valor (p. ej., ) |
|---|---------------|-----------------------------|--------------|
| ! | model | Build.MODEL | AFTM |
|   | vendedor | Build.BRAND | Amazon |
|   | fabricante | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | codificado | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

La información de conexión se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

La información de la aplicación se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---------------|-----------|--------------|
|   | applicationId | codificado | CNN |

>[!IMPORTANT]
>
La información del dispositivo, la conexión y la aplicación debe agregarse al mismo objeto JSON. Después, el objeto resultante debe estar **codificado en Base64**. Además, en el caso de las API de REST de autenticación de Adobe Pass, el valor debe estar **codificado en la dirección URL**.

>[!NOTE]
>
**Recursos:**
* clase pública [Build](https://developer.android.com/reference/android/os/Build.html){target=_blank} en la documentación de desarrolladores de Android.
* [Identificando dispositivos FireTV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

La información del dispositivo se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---------------|------------------------|--------------|
| ! | model | uname.machine | iPhone |
|   | vendedor | codificado | Apple |
|   | fabricante | codificado | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10,2 |

La información de conexión se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [Reachability currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


La información de la aplicación se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---------------|-----------|--------------|
|   | applicationId | codificado | CNN |

>[!IMPORTANT]
>
La información del dispositivo, la conexión y la aplicación debe agregarse al mismo objeto JSON. Después, el objeto resultante debe tener codificación Base64. Además, en el caso de las API de REST de autenticación de Adobe Pass, el valor debe tener codificación de dirección URL.

**Código de muestra**

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
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
**Recursos:**
* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
* [Acerca de la accesibilidad](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

La información del dispositivo se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | model | codificado | &quot;Roku&quot; |
|     | vendedor | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
|     | fabricante | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| ! | version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | codificado | &quot;Roku&quot; |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

La información de conexión se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | codificado | true si la conexión está cableada |

La información de la aplicación se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---------------|-----------|--------------|
|   | applicationId | codificado | CNN |

>[!IMPORTANT]
>
La información del dispositivo, la conexión y la aplicación debe agregarse al mismo objeto JSON. Después, el objeto resultante debe estar **codificado en Base64**. Además, en el caso de las API de REST de autenticación de Adobe Pass, el valor debe tener codificación de dirección URL.

>[!NOTE]
>
Para obtener más información, consulte [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

La información del dispositivo se puede construir de la siguiente manera:

|   | Clave | Source | Valor (ejemplo) |
|---|---|---|---|
| ! | model | EasClientDeviceInformation.SystemProductName |                 |
|   | vendedor | codificado | Microsoft |
|   | fabricante | codificado | Microsoft |
| ! | version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

La información de conexión se puede construir de la siguiente manera:

|   | Clave | Source | Ejemplo |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | &quot;None&quot;, &quot;Wpa&quot;, etc |

La información de la aplicación se puede construir de la siguiente manera:

| Clave | Source | Valor (ejemplo) |
|---|---|---|
| applicationId | codificado | CNN |

>[!IMPORTANT]
>
La información del dispositivo, la conexión y la aplicación debe agregarse al mismo objeto JSON. Después, el objeto resultante debe estar **codificado en Base64**. Además, en el caso de las API de REST de autenticación de Adobe Pass, el valor debe estar **codificado en la dirección URL**.

**Recursos**

* [Clase EasClientDeviceInformation](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [Clase DisplayInformation](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
