---
title: 'Error de autenticación de iOS: no se encuentra adobepass.ios.app'
description: 'Error de autenticación de iOS: no se encuentra adobepass.ios.app'
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Error de autenticación de iOS: no se encuentra adobepass.ios.app {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Problema {#issue}

El usuario está pasando por el flujo de autenticación y, después de introducir correctamente sus credenciales con su proveedor, se le redirige a una página de error, a una página de búsqueda o a otra página personalizada que le informa de eso `adobepass.ios.app` no se pudo encontrar/resolver.

## Explicación {#explanation}

En iOS, `adobepass.ios.app` se utiliza como URL de redirección final para indicar que el flujo de AuthN ha finalizado. En este punto, la aplicación debe realizar una solicitud al AccessEnabler para obtener el token de AuthN y finalizar el flujo de AuthN.

El problema es que `adobepass.ios.app` no existe realmente y almacenará en déclencheur un mensaje de error en `webView`. Las versiones anteriores de la aplicación de demostración de iOS daban por hecho que este error siempre se activaba al final del flujo de AuthN y se configuraba para gestionarlo en consecuencia (`indidFailLoadWithError`).

**Nota:** Este problema se ha corregido en versiones posteriores de DemoApp (incluida con la descarga del SDK para iOS).

Desafortunadamente, esta suposición NO es correcta. Hay algunos servidores DNS o proxy &quot;inteligentes&quot; que no solo transmitirán el error generado, sino que, en su lugar, harán una de las siguientes acciones:

- Crear una página de error personalizada
- Reenviar a una página de búsqueda o a algún otro tipo de página o portal de cliente.

En estos casos, la respuesta que vuelva a iOS webView será una respuesta perfectamente válida en lo que respecta a webView y NO almacenará en déclencheur el error del que dependía la antigua DemoApp.

## Solución {#solution}

NO realice la misma suposición que hace DemoApp. En su lugar, intercepte la solicitud antes de ejecutarla (en `shouldStartLoadWithRequest`) y manejarlo apropiadamente.

Ejemplo de cómo interceptar la solicitud antes de ejecutarla:

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

Algunas cosas que hay que tener en cuenta:

- NUNCA use `adobepass.ios.app` directamente en cualquier parte del código. En su lugar, utilice la constante `ADOBEPASS_REDIRECT_URL`
- El `return NO;` evitará que la página se cargue
- Asegúrese de que la variable `getAuthenticationToken` se llama a una vez y solo una vez en el código. Varias llamadas a `getAuthenticationToken` dará como resultado resultados no definidos.
