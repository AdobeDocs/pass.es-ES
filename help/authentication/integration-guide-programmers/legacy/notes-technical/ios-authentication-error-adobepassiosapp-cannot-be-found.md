---
title: 'Error de autenticación de iOS: no se encuentra adobepass.ios.app'
description: 'Error de autenticación de iOS: no se encuentra adobepass.ios.app'
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# (Heredado) Error de autenticación de iOS - adobepass.ios.app No se encuentra {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Problema {#issue}

El usuario está siguiendo el flujo de autenticación y, después de que haya escrito correctamente sus credenciales con su proveedor, se le redirigirá a una página de error, a una página de búsqueda o a otra página personalizada que le informará de que no se pudo encontrar o resolver `adobepass.ios.app`.

## Explicación {#explanation}

En iOS, `adobepass.ios.app` se usa como dirección URL de redirección final para indicar que el flujo de AuthN ha finalizado. En este punto, la aplicación debe realizar una solicitud al AccessEnabler para obtener el token de AuthN y finalizar el flujo de AuthN.

El problema es que `adobepass.ios.app` no existe realmente y almacenará en déclencheur un mensaje de error en `webView`. Las versiones anteriores de iOS DemoApp daban por hecho que este error siempre se activaría al final del flujo de AuthN y se configuraba para gestionarlo en consecuencia (`indidFailLoadWithError`).

**Nota:** Este problema se ha corregido en versiones posteriores de DemoApp (incluida en la descarga de iOS SDK).

Desafortunadamente, esta suposición NO es correcta. Hay algunos servidores DNS o proxy &quot;inteligentes&quot; que no solo transmitirán el error generado, sino que, en su lugar, harán una de las siguientes acciones:

- Crear una página de error personalizada
- Reenviar a una página de búsqueda o a algún otro tipo de página o portal de cliente.

En estos casos, la respuesta que vuelva a iOS webView será una respuesta perfectamente válida en lo que respecta a webView y NO almacenará en déclencheur el error del que dependía la antigua DemoApp.

## Solución {#solution}

NO realice la misma suposición que hace DemoApp. En su lugar, intercepte la solicitud antes de que se ejecute (en `shouldStartLoadWithRequest`) y gestiónela adecuadamente.

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

- NUNCA use `adobepass.ios.app` directamente en ningún lugar del código. En su lugar, utilice la constante `ADOBEPASS_REDIRECT_URL`
- La instrucción `return NO;` evitará que la página se cargue
- Asegúrese de que la llamada a `getAuthenticationToken` se realice una vez y solo una vez en el código. Si llama varias veces a `getAuthenticationToken`, se obtendrán resultados no definidos.
