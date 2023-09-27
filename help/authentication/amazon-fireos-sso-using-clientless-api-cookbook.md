---
title: Amazon FireOS SSO con la API de cliente Cookbook
description: Amazon FireOS SSO con la API de cliente Cookbook
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Amazon FireOS SSO con la API de cliente Cookbook {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>

## Introducción {#Introduction}

Este documento proporciona instrucciones para implementar la versión SSO de Amazon del flujo de autenticación de Adobe Pass mediante la API sin cliente. La primera parte de este documento se centra en la especificidad de la versión de Amazon de la arquitectura, para los muchos socios que ya están familiarizados y experimentados con su implementación.

La segunda parte del documento trata los pasos principales para implementar la API sin cliente de autenticación de Adobe Pass.

Para obtener una amplia descripción técnica del funcionamiento de la solución sin cliente, consulte la [Información general de API REST](/help/authentication/rest-api-overview.md). Adobe es el contacto preferido para obtener soporte sobre la arquitectura general y las primeras implementaciones.

## SSO sin cliente de Amazon {#AMZ-Clientless-SSO}

### Arquitectura de alto nivel {#High-Level-Arch}

La implementación de SSO sin cliente de Amazon es sencilla y, en su mayoría, idéntica a las API sin cliente de autenticación de Adobe Primetime normales.

Deberá utilizar el SDK de Amazon para recuperar una carga útil personalizada y utilizarla al llamar a las API de cliente de Adobe.

Si la carga útil se reconoce y corresponde a una sesión autenticada, las API sin cliente regresarán inmediatamente con el token de la sesión.

### Cómo generar la aplicación para utilizar el SDK de Amazon {#Build-entries}

* Descargue y copie la última versión [SDK de código auxiliar Amazon](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) en una carpeta /SOEnabler paralela al directorio de la aplicación
* Actualice los archivos de manifiesto/gradle para utilizar la biblioteca :

  **Agregue la línea siguiente al archivo Manifiesto:**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Entradas de archivo de Gradle:**

  En Repositorios:

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  En Dependencias, agregue:

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* Gestión de la ausencia de la aplicación complementaria de Amazon:

  Aunque no es probable, si el complemento no está presente en el dispositivo Amazon que ejecuta la aplicación, debería encontrar una excepción ClassNotFoundException en tiempo de ejecución en la siguiente clase: `com.amazon.ottssotokenlib.SSOEnabler`.

  Si esto sucede, todo lo que debe hacer es omitir el paso de carga útil y volver al flujo de PrimeTime normal. No se activará el SSO, pero el flujo de autenticación normal se producirá normalmente.

</br>

### Cómo obtener la carga útil SSO de Amazon mediante el SDK de Amazon {#UseAmazonSSO}

Durante la inicialización de la aplicación, obtenga una instancia de SSOEnabler. En función de la arquitectura de la aplicación, debe decidir entre una implementación sincrónica o asincrónica.

Si, por cualquier razón, las llamadas a la API no devuelven una carga útil, utilice el flujo normal que no sea de SSO y póngase en contacto con sus socios de Amazon y de Adobe para investigar.

**API asincrónica**

* Obtener instancia del habilitador de SSO:

  ```Java
  ssoEnabler = SSOEnabler.getInstance(context);
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```


* Establecer la llamada de retorno

  ```java
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

   * El paquete de respuesta correcta contendrá:
      * Token SSO como cadena con la clave &quot;SSOToken&quot;
   * El paquete de respuesta a errores contendrá:
      * código de error como un int con la clave &quot;ErrorCode&quot;
      * descripción del error como una cadena con la clave &quot;ErrorDescription&quot;


* Obtener token de SSO

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* Esta API proporciona la respuesta mediante la llamada de retorno establecida durante el inicio.

  **Ex**. llamada mediante la instancia singleton creada durante init:

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**API sincrónicas**

* Obtenga la instancia del Habilitador de SSO y establezca la llamada de retorno

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* Obtener token de SSO

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * Esta API bloqueará el hilo que llama y responderá con el paquete de resultados. Dado que se trata de una llamada sincrónica, asegúrese de no utilizarla en su subproceso principal.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * Valor en milisegundos. si se establece, anule el valor de tiempo de espera predeterminado de 1 minuto para la API de sincronización.


### Actualización de la API de Adobe Pass sin cliente para utilizar el registro de cliente dinámico {#clientlessdcr}

Si esta es su primera implementación, consulte la **Información técnica de Clientless** y póngase en contacto con el Adobe en caso de que necesite asistencia.

La API sin cliente de Adobe requiere que las aplicaciones utilicen el registro de cliente dinámico para realizar llamadas a los servidores de Adobe.

* Para utilizar el registro de cliente dinámico en su aplicación, siga las instrucciones de [Dynamic Client Registration Management para registrar la aplicación](/help/authentication/dynamic-client-registration-management.md).

* Para implementar la API de registro de cliente dinámico para realizar solicitudes de autenticación y autorización a los servidores de Adobe Pass, siga las instrucciones en [API de registro de cliente dinámico](/help/authentication/dynamic-client-registration-api.md) .

### Actualización de la API de Adobe Pass sin cliente para utilizar Amazon SSO {#clientlesssso}

La carga útil de SSO de Amazon obtenida del SDK de Amazon debe estar presente en las solicitudes realizadas a los extremos de autenticación de Adobe Pass

```
      /adobe-services/*
      /reggie/*
      /api/*
```


Todos los extremos de autenticación de Adobe Pass admiten los siguientes métodos para recibir el identificador de ámbito de dispositivo o el identificador de ámbito de plataforma (presentes en la carga útil SSO de Amazon):

* Como encabezado: &quot;Adobe-Subject-Token&quot;
* Como parámetro de consulta: &quot;ast&quot;
* Como parámetro de publicación: &quot;ast&quot;


>[!NOTE]
>
>Si el identificador con ámbito de dispositivo o el identificador con ámbito de plataforma se envía como parámetro de consulta/publicación, se debe incluir al generar la firma de solicitud.

>[!NOTE]
>
>Si se utiliza el parámetro de consulta &quot;ast&quot;, toda la dirección URL puede llegar a ser muy larga y rechazarse. En la llamada /authenticate, este parámetro se puede omitir porque se proporcionó en la llamada /regcode

**Ejemplos:**

**Enviar como encabezado personalizado**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**Envío como parámetro de consulta**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**Enviando como parámetro posterior**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>Si el SSO de Amazon no está presente o no es válido, la autenticación de Adobe Pass ignorará el atributo y las llamadas se ejecutarán como si el SSO no estuviera presente.
