---
title: Guía de Amazon SSO (API de REST V1)
description: Guía de Amazon SSO (API de REST V1)
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Guía de Amazon SSO (API de REST V1) {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La API de REST de autenticación de Adobe Pass V1 es compatible con el inicio de sesión único (SSO) de Platform para los usuarios finales de aplicaciones cliente que se ejecutan en FireOS.

Este documento actúa como una extensión de la [Información general de la API REST V1](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md) existente que proporciona una vista de alto nivel.

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

La carga del token SSO de Amazon (identidad de plataforma) debe estar presente en todas las solicitudes HTTP realizadas con los extremos de autenticación de Adobe Pass:

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> La aplicación de streaming puede omitir el envío de la carga útil de token SSO de Amazon (identidad de plataforma) en la llamada `/authenticate`, ya que se proporcionó en la llamada `/regcode`.

La autenticación de Adobe Pass admite los siguientes métodos para recibir la carga útil del token SSO (identidad de plataforma), que es un identificador con ámbito de dispositivo o de plataforma:

* Como encabezado denominado: `Adobe-Subject-Token`
* Como parámetro de consulta denominado: `ast`
* Como parámetro de publicación denominado: `ast`

>[!IMPORTANT]
>
> Si se envía como parámetro de consulta, toda la dirección URL puede llegar a ser muy larga y rechazarse.
>
> Si se envía como parámetro de consulta/publicación, debe incluirse al generar la firma de solicitud.

#### Muestras

**Envío como encabezado**

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Envío como parámetro de consulta**

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**Envío como parámetro de publicación**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> En caso de que falte el encabezado `Adobe-Subject-Token` o el valor del parámetro `ast`, o no sea válido, la autenticación de Adobe Pass atenderá las solicitudes sin tener en cuenta el inicio de sesión único.
