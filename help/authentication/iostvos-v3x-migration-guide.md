---
title: Guía de migración de iOS/tvOS v3.x
description: Guía de migración de iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%

---

# Guía de migración de iOS/tvOS v3.x {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!TIP]
> 
> **Notas:**
>
> - A partir de la versión 3.1 del sdk para iOS, los implementadores ahora pueden utilizar WKWebView o UIWebView de forma intercambiable. Dado que UIWebView está en desuso, las aplicaciones deben migrar a WKWebView para evitar problemas con futuras versiones de iOS.
> - Tenga en cuenta que la migración implicaría simplemente cambiar la clase UIWebView con WKWebView, no hay ningún trabajo específico por hacer en relación con el AccessEnabler del Adobe.

</br>

## Actualizar configuración de compilación {#update}

Esta versión incluye funciones escritas en el idioma de SWIFT. Si la aplicación es completamente Objective-C, debe establecer la casilla &quot;Incorporar siempre bibliotecas estándar de Swift&quot; en la configuración de compilación de Target en &quot;Sí&quot;. Cuando se establece esta opción, Xcode analiza los marcos agrupados en la aplicación y, si alguno de ellos contiene código Swift, copia las bibliotecas pertinentes en el paquete de la aplicación. Si no actualiza la configuración de compilación, es posible que la aplicación se bloquee con errores que indiquen que no puede cargar AccessEnabler.framework o varias bibliotecas de `ibswift*`.

</br>

## Adición de la declaración de software {#add}

> Para obtener información sobre cómo obtener la declaración del software, consulte esta sección
> página:
> [Registro de aplicación](/help/authentication/iostvos-application-registration.md)

Una vez que tenga la declaración de software, le recomendamos que la aloje en un servidor remoto para que pueda revocarla o cambiarla fácilmente sin implementar una nueva versión de la aplicación en App Store. Cuando se inicie la aplicación, obtenga la instrucción de software de la ubicación remota y pásela en el constructor AccessEnabler:

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> Información de la API aquí: [Referencia de la API de iOS/tvOS](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Añadir el esquema de URL personalizado {#add-custom}

> Para obtener información sobre cómo obtener un esquema de URL personalizado, vaya a esta página: [Obtener un esquema de URL de cliente](/help/authentication/iostvos-application-registration.md)

Después de obtener el esquema de URL personalizado, debe añadirlo al archivo info.plist de la aplicación. El esquema personalizado tiene este formato: `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. Debe omitir los dos puntos y las barras diagonales al agregarlos al archivo. El ejemplo anterior se convertirá en `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## Interceptación de llamadas en el esquema de URL personalizado {#intercept}

Esto solo se aplica en el caso de que la aplicación haya habilitado anteriormente la administración manual de Safari View Controller (SVC) a través de la llamada [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/iostvos-sdk-api-reference.md) y para MVPD específicas que requieren Safari View Controller (SVC), por lo que es necesario cargar las direcciones URL de los extremos de autenticación y cierre de sesión mediante un controlador SFSafariViewController en lugar de un controlador UIWebView/WKWebView.

Durante los flujos de autenticación y cierre de sesión, la aplicación debe supervisar la actividad del controlador `SFSafariViewController ` a medida que pasa por varias redirecciones. Su aplicación debe detectar el momento en que carga una dirección URL personalizada específica definida por su `application's custom URL scheme` (p. ej.`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. Cuando el controlador carga esta dirección URL personalizada específica, la aplicación debe cerrar `SFSafariViewController` y llamar al método de API `handleExternalURL:url `de AccessEnabler.

En su `AppDelegate` agregue el siguiente método:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> Información de API aquí: [Administrar URL externa](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Actualizar la firma del método setRequestor {#update-setreq}

Dado que el nuevo SDK utiliza un nuevo mecanismo de autenticación, no es necesario el parámetro signedRequestId ni la clave pública y secreto (para tvOS). El método `setRequestor` se ha simplificado y solo necesita el requestorID.

### iOS

Este código:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

se convierte en:

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

Este código:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

se convierte en:

```swift
    accessEnabler.setRequestor(requestorId)
```

> Información de API aquí: [Establecer solicitante](/help/authentication/iostvos-sdk-api-reference.md)

</br>

## Reemplace el método getAuthenticationToken por el método handleExternalURL {#replace}

El método `getAuthentication` se ha usado en el pasado para completar el flujo de autenticación. Dado que su nombre era engañoso, se le cambió el nombre a `handleExternalURL` y toma la dirección URL como parámetro.

Cambiar todas las apariciones de esto:

```swift
    accessEnabler.getAuthenticationToken()
```

en esto:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> Información de API aquí: [Administrar URL externa](/help/authentication/iostvos-sdk-api-reference.md)
