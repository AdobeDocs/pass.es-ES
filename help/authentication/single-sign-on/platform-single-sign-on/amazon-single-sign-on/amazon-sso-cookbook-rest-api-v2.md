---
title: Guía de Amazon SSO (API REST V2)
description: Guía de Amazon SSO (API REST V2)
source-git-commit: e5ef8c0cba636ac4d2bda1abe0e121d0ecc1b795
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# Guía de Amazon SSO (API REST V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La API de REST de autenticación de Adobe Pass V2 es compatible con el inicio de sesión único (SSO) de Platform para los usuarios finales de aplicaciones cliente que se ejecutan en FireOS.

Este documento actúa como una extensión de la [Información general de la API REST V2](/help/authentication/rest-api-v2/rest-api-v2-overview.md) existente que proporciona una vista de alto nivel y el documento que describe cómo implementar el [inicio de sesión único mediante flujos de identidad de la plataforma](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Inicio de sesión único de Amazon con flujos de identidad de plataforma {#cookbook}

### Requisitos previos {#prerequisites}

Antes de continuar con el inicio de sesión único de Amazon mediante los flujos de identidad de la plataforma, asegúrese de que se cumplan los siguientes requisitos previos.

#### Integración del SDK de SSO de Amazon {#integrate-amazon-sso-sdk}

La aplicación de streaming debe integrar la biblioteca [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) para el inicio de sesión único (SSO) en su compilación.

* Descargue y copie la biblioteca más reciente del SDK SSO de Amazon en una carpeta `/SSOEnabler` paralela al directorio de la aplicación.

* Actualice el manifiesto y los archivos de Gradle para utilizar la biblioteca del SDK de SSO de Amazon.

  **Manifiesto:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  En Repositorios:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  En dependencias:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Uso del SDK de SSO de Amazon {#use-amazon-sso-sdk}

La aplicación de streaming debe utilizar el SDK para SSO de Amazon para obtener la carga útil del token SSO (identidad de plataforma).

El SDK de SSO de Amazon proporciona API sincrónicas y asincrónicas para obtener la carga útil del token de SSO (identidad de plataforma).

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

  Esta API proporciona la respuesta a través de la llamada de retorno establecida durante la inicialización.

##### API sincrónicas

* Obtener la instancia `SSOEnabler`:

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

  Esta API establece el valor de tiempo de espera para la llamada sincrónica. El valor predeterminado de tiempo de espera es 1 minuto.

#### Reserva para Amazon SSO {#fallback-amazon-sso}

La aplicación de streaming debe gestionar escenarios de reserva desde el flujo de SSO de Amazon al flujo de autenticación normal.

Asegúrese de que la aplicación de flujo continuo administra lo siguiente:

* La ausencia de la aplicación complementaria de Amazon que debería ejecutarse en el dispositivo Amazon.
   * La aplicación de flujo continuo puede encontrar un `ClassNotFoundException` en tiempo de ejecución en la siguiente clase `com.amazon.ottssotokenlib.SSOEnabler`.

* La ausencia de la carga útil del token SSO (identidad de plataforma) que deben devolver las API anteriores.
   * La aplicación de streaming puede ponerse en contacto con el Amazon y con los representantes del Adobe para investigar.

### Flujo de trabajo {#workflow}

La carga del token SSO de Amazon (identidad de plataforma) debe estar presente en todas las solicitudes HTTP realizadas con los extremos de la API de REST de autenticación de Adobe Pass V2:

```
/api/v2/*
```

La API de REST de autenticación de Adobe Pass V2 admite los siguientes métodos para recibir la carga útil de token de SSO (identidad de plataforma), que es un identificador con ámbito de dispositivo o de plataforma:

* Como encabezado denominado: `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> Para obtener más información sobre el encabezado `Adobe-Subject-Token`, consulte la documentación de [Adobe-Subject-Token](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

#### Muestras

**Envío como encabezado**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> En caso de que falte el valor del encabezado `Adobe-Subject-Token` o no sea válido, la autenticación de Adobe Pass atenderá las solicitudes sin tener en cuenta el inicio de sesión único.
