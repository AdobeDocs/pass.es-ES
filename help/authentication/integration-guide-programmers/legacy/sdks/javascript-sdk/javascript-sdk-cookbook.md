---
title: Guía de JavaScript SDK
description: Guía de JavaScript SDK
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '935'
ht-degree: 0%

---

# Guía de JavaScript SDK (heredada) {#javascript-sdk-cookbook}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#intro}

En este documento se describen los flujos de trabajo de asignación de derechos que implementa la aplicación de nivel superior de un programador para una integración de JavaScript con el servicio de autenticación de Adobe Pass. Los vínculos a la Referencia de la API de JavaScript se incluyen en.

Tenga en cuenta también que la sección [Información relacionada](#related) incluye una
vínculo a un conjunto de ejemplos de código JavaScript.

## Flujos de derecho {#entitlement}

1. [Requisitos previos](#prereq)
2. [Flujo de inicio](#startup)
3. [Flujo de autenticación](#authn)
4. [Flujo de autorización](#authz)
5. [Ver flujo de medios](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## Requisitos previos {#prereq}

**Dependencias:**

- Biblioteca de autenticación de Adobe Pass (AccessEnabler), colabore con el administrador de cuentas de autenticación de Adobe Pass para organizarlo.
- ID de solicitante de autenticación de Adobe Pass válido, colabore con el administrador de cuentas de autenticación de Adobe Pass para organizarlo.

Cree sus funciones de devolución de llamada:

- `entitlementLoaded`
</br>

**Déclencheur:** AccessEnabler se ha cargado y ha finalizado la inicialización.

- `displayProviderDialog(mvpds)`

  **Déclencheur:** `getAuthentication(),` solo si el usuario no ha seleccionado ningún proveedor (un MVPD) y aún no se ha autenticado
El parámetro mvpds es una matriz de proveedores disponibles para el usuario.

- `setAuthenticationStatus(status, errorcode)`

  **Déclencheur:**
   - `checkAuthentication()` cada vez.
   - `getAuthentication()` solo si el usuario ya se ha autenticado y ha seleccionado un proveedor.

  El estado devuelto es éxito o error; el código de error describe el tipo de error.

- `createIFrame(width, height)`

  **Déclencheur:** `setSelectedProvider(providerID)`, solo si el proveedor seleccionado está configurado para mostrarse en un IFrame.

  >[!NOTE]
  >
  >Un proveedor está configurado para procesar su pantalla de autenticación como redireccionamiento o en un iFrame, y el programador debe tener en cuenta ambos.

- `sendTrackingData(event, data)`

  **Déclencheur:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  El parámetro `event` indica qué evento de asignación de derechos se produjo; el parámetro `data` es una lista de valores relacionados con el evento.
- `setToken(token, resource)`
  **Déclencheur:** `checkAuthorization()`y `getAuthorization()` después de una autorización correcta para ver un recurso.   El parámetro `token` es el token de medios de corta duración; el parámetro `resource` es el contenido que el usuario tiene autorización para ver.

- `tokenRequestFailed(resource, code, description)`
  **Déclencheur:**`checkAuthorization()` y`getAuthorization()` tras una autorización incorrecta.\
  El parámetro `resource` es el contenido que el usuario intentaba ver; el parámetro `code` es el código de error que indica qué tipo de error se produjo; el parámetro `description` describe el error asociado con el código de error.

- `selectedProvider(mvpd)`

  **Déclencheur:** [`getSelectedProvider()`](#$getSelProv El parámetro `mvpd` proporciona información sobre el proveedor seleccionado por
el usuario.

- `setMetadataStatus(metadata, key, arguments)`

  **Déclencheur:** `getMetadata().`\
  El parámetro `metadata` proporciona los datos específicos solicitados; el parámetro clave es la clave utilizada en la `getMetadata()`solicitud; y el parámetro `arguments` es el mismo diccionario que se pasó a `getMetadata()`.


## 2. Flujo de inicio

**I. Cargue el JavaScript AccessEnabler:**

**Para el perfil de ensayo**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

o...

**Para el perfil de producción**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Déclencheur:** Una vez completada la inicialización, Adobe Pass
la autenticación llama a su función de devolución de llamada `entitlementLoaded()`. Este es el punto de entrada a la comunicación de la aplicación con AccessEnabler.


**II.** Llame a `setRequestor()`para establecer el
identidad del programador; pase en `requestorID` del programador y
(Opcional) una matriz de puntos finales de autenticación de Adobe Pass.

**Déclencheur:** Ninguno, pero permite llamar a `displayProviderDialog()` cuando sea necesario.


**III.** Llame a `checkAuthentication()` para comprobar si hay una autenticación existente sin iniciar el [flujo de autenticación completo].  Si esta llamada se realiza correctamente, puede continuar directamente a `authorization flow`.  Si no es así, continúe con `authentication flow`.

**Dependencia:** Una llamada correcta a `setRequestor()` (esta dependencia también se aplica a todas las llamadas subsiguientes).

**Déclencheur:** `setAuthenticationStatus()` devolución de llamada

</br>

## 3. Flujo de autenticación</span>


**Dependencia:** Una llamada correcta a `setRequestor()` (esta dependencia también se aplica a todas las llamadas subsiguientes).


Llame a `getAuthentication()` para obtener el estado de autenticación O para almacenar en déclencheur el flujo de autenticación del proveedor.

**Desencadenadores:**

- `displayProviderDialog()`si el usuario aún no se ha autenticado
- `setAuthenticationStatus()` si la autenticación ya se ha realizado

Se llega a la finalización del flujo de autenticación cuando AccessEnabler llama a `setAuthenticationStatus()`con `isAuthenticated == 1`.

## 4. Flujo de autorización {#authz}

**Dependencias:**

- Una llamada correcta a `setRequestor()` (esta dependencia se aplica también a todas las llamadas subsiguientes).
- ID de recurso válidos acordados con los MVPD. Tenga en cuenta que los ResourceID deben ser los mismos que se usan en cualquier otro dispositivo o plataforma, y serán los mismos en todas las MVPD.

Llame a `getAuthorization()` y pase el ResourceID para los medios solicitados. Una llamada correcta devolverá un token de medios corto, que confirma que el usuario está autorizado para ver los medios solicitados.

- Si la llamada pasa: El usuario tiene un token de AuthN válido y está autorizado para ver el contenido solicitado.
- Si la llamada falla: Examine la excepción producida para determinar su tipo (AuthN, AuthZ o algo más):
- Si la llamada fue un error de AuthN, reinicie el flujo de AuthN.
- Si la llamada fue un error de AuthZ, el usuario no tiene autorización para ver el contenido solicitado y se debe mostrar algún tipo de mensaje de error al usuario.
- Si se ha producido algún otro error (error de conexión, error de red, etc.), muestre un mensaje de error apropiado al usuario.

Utilice el verificador de tokens de medios para validar el shortMediaToken devuelto por una llamada correcta de `getAuthorization()`.


**Dependencia:** El Verificador de token de medios corto (incluido con el
AccessEnabler (biblioteca)

- Si se supera la validación: mostrar/reproducir el medio solicitado para el usuario.
- Si falla: El token de AuthZ no era válido, la solicitud de medios debe rechazarse y se debe mostrar un mensaje de error al usuario.

## 5. Ver flujo de medios {#logout}

- El usuario selecciona los medios que desea ver.
   - ¿Están protegidos los medios?
      - La aplicación comprueba si los medios están protegidos:
         - Si el medio está protegido, la aplicación inicia el flujo de autorización (AuthZ) anterior.
         - Si los medios no están protegidos, continúe con el flujo de Ver medios.
         - Medios de reproducción

## Configuración del ID de visitante {#visitorID}

La configuración de un valor [Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html) es muy importante desde el punto de vista del análisis. Una vez establecido un valor EC visitorID, SDK enviará esta información junto con cada llamada de red y el servicio de autenticación de Adobe Pass recopilará esta información. De este modo, podrá correlacionar los datos de análisis del servicio de autenticación de Adobe Pass con cualquier otro informe de análisis que pueda tener de otras aplicaciones o sitios web. Encontrará información sobre cómo configurar el ID de visitante de EC [aquí](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Tenga en cuenta que esta compatibilidad con la funcionalidad está disponible a partir de JS SDK versión 3.1.0.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
