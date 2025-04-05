---
title: Guía de Amazon SSO (API REST V2)
description: Guía de Amazon SSO (API REST V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Guía de Amazon SSO (API REST V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La API de REST de autenticación de Adobe Pass V2 es compatible con el inicio de sesión único (SSO) de Platform para los usuarios finales de aplicaciones cliente que se ejecutan en FireOS.

Este documento actúa como una extensión de la [Información general de la API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existente que proporciona una vista de alto nivel y el documento que describe cómo implementar el [inicio de sesión único mediante flujos de identidad de la plataforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Inicio de sesión único de Amazon con flujos de identidad de plataforma {#cookbook}

La autenticación de Adobe Pass colabora con Amazon para mejorar la experiencia del usuario que inicia sesión y para facilitar el inicio de sesión único (SSO) en las aplicaciones de TV Everywhere para suscriptores de TV.

### Requisitos previos {#prerequisites}

Antes de continuar con el Amazon inicio de sesión único utilizar flujos de identidad de plataforma, asegúrese de que se cumplan los siguientes requisitos previos.

#### Integración Amazon SSO SDK {#integrate-amazon-sso-sdk}

El aplicación de flujo continuo debe integrar el biblioteca SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) de [SSO de Amazon para el inicio de sesión único (SSO) en su versión.

* Descargue y copie la biblioteca de SDK SSO de Amazon más reciente en una carpeta `/SSOEnabler` paralela al directorio de la aplicación.

* Actualice el manifiesto y los archivos de Gradle para utilizar la biblioteca SDK SSO de Amazon.

  **Manifiesto:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  En repositorios:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  En dependencias:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Uso Amazon SDK de SSO {#use-amazon-sso-sdk}

La aplicación de streaming debe utilizar Amazon SSO SDK para obtener la carga útil del token SSO (identidad de plataforma).

Amazon SSO SDK proporciona API sincrónicas y asincrónicas para obtener la carga útil del token SSO (identidad de plataforma).

La aplicación de streaming puede elegir una de las dos opciones en función de su arquitectura.

##### API asíncronas

* Obtenga la instancia `SSOEnabler` y establezca `SSOEnablerCallback`:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  Esto se puede hacer durante la inicialización de la aplicación de streaming.

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  El paquete de respuesta de éxito del token SSO contendrá lo siguiente:
   * Un token SSO como `string` con la clave &quot;SSOToken&quot;.

  <br/>

  El paquete de respuesta de error del token SSO contendrá lo siguiente:
   * Un código de error como `int` con la clave &quot;ErrorCode&quot;.
   * Una descripción de error como `string` con la clave &quot;ErrorDescription&quot;.

  <br/>

* Obtenga el token de SSO:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  Esta API proporcionará la respuesta mediante la devolución de llamada establecida durante la inicialización.

##### API sincrónicas

* Obtenga la `SSOEnabler` instancia:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* Obtenga el token de SSO:

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  Esta API bloqueará el hilo que llama y responderá con el paquete de resultados. Dado que se trata de una llamada sincrónica, asegúrese de no utilizarla en el subproceso principal.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  Esta API establecerá el valor de tiempo de espera para la llamada sincrónica. El valor predeterminado del tiempo de espera es 1 minuto.

#### Alternativa para Amazon SSO {#fallback-amazon-sso}

El aplicación de streaming debe gestionar escenarios de reserva desde el flujo de SSO de Amazon al flujo de autenticación normal.

Asegúrese de que el aplicación de flujo continuo gestiona:

* La ausencia de la aplicación complementaria de Amazon que debería ejecutarse en el dispositivo Amazon.
   * La aplicación de flujo continuo puede encontrar un `ClassNotFoundException` en tiempo de ejecución en la siguiente clase `com.amazon.ottssotokenlib.SSOEnabler`.

* La ausencia de la carga útil del token SSO (identidad de plataforma) que deben devolver las API anteriores.
   * La aplicación de streaming puede ponerse en contacto con los representantes de Amazon y Adobe para investigar.

### Flujo de trabajo {#workflow}

La carga útil de token de SSO (identidad de plataforma) de Amazon debe estar presente en todas las solicitudes HTTP realizadas en Adobe Pass puntos finales Authentication API de REST V2:

```
/api/v2/*
```

Adobe Pass Authentication API de REST V2 admite los siguientes métodos para recibir la carga útil del token de SSO (identidad de plataforma), que es un identificador con ámbito de dispositivos o de plataforma:

* Como encabezado denominado: `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Para obtener más detalles sobre `Adobe-Subject-Token` el encabezado, consulte la documentación de [Adobe Systems-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) .

#### Muestras

**Envío como encabezado**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> En caso de que falte el valor del `Adobe-Subject-Token` encabezado o esté no válido, Adobe Pass Authentication atenderá las solicitudes sin tener que incluir el inicio de sesión único en cuenta.
