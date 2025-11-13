---
title: Servicio de inicio de sesión único Adobe
description: Obtenga información sobre el servicio SSO de Adobe Pass que permite una autenticación perfecta entre varios dispositivos y aplicaciones.
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 2%

---


# Servicio de inicio de sesión único Adobe {#sso-service}

Este documento describe casos de uso, extremos y API para el servicio de inicio de sesión único de Adobe.

**Revisión actual - 1.0.0**

## Ámbito {#scope}

El servicio SSO de Adobe Pass permite una autenticación perfecta entre varios dispositivos y aplicaciones, lo que ofrece una experiencia de usuario unificada y, al mismo tiempo, mantiene los estándares de seguridad y conformidad. Este servicio responde a la creciente necesidad de autenticación multiplataforma en el panorama actual de la transmisión multidispositivo.

## Información general {#overview}

### Qué es {#what-it-is}

Una solución completa de inicio de sesión único que permite a los usuarios autenticarse una vez y administrar la transferencia de autenticación de forma informada a otros dispositivos

### Retos actuales para la autenticación {#current-challenges}

* El usuario debe autenticarse con cada servicio de streaming al que esté suscrito
* El usuario debe autenticarse por separado en cada dispositivo o aplicación
* Debido a la dificultad para escribir una contraseña en el flujo de autenticación en algunas plataformas, esto puede provocar un aumento en las tasas de abandono

### Oportunidad para un servicio SSO de Adobe Pass {#opportunity}

* Aumento del número de servicios de streaming que requieren su propia autenticación
* Creciente demanda de experiencias entre dispositivos sin problemas
* Aumento del número de dispositivos de transmisión por secuencias por hogar
* Necesidad de autenticación unificada por hogar

### Ventajas comerciales {#business-benefits}

#### Proveedores de contenido {#content-providers}

* **Mayor participación del usuario**: La experiencia perfecta lleva a tiempos de sesión más largos
* **Fricción reducida**: las barreras de autenticación más bajas aumentan el consumo de contenido
* **Retención mejorada**: una mejor experiencia del usuario reduce la pérdida
* **Reducción de costos** - Menos tickets de soporte relacionados con problemas de autenticación

#### Usuarios finales {#end-users}

* **Experiencia perfecta** - Autenticar una vez y acceder a todas partes
* **Ahorro de tiempo** - No hay procesos de inicio de sesión repetidos
* **Flexibilidad del dispositivo** - Cambiar entre dispositivos sin interrupción
* **Experiencia coherente**: autenticación uniforme en todas las plataformas

#### IdPs (MVPD, telecomunicaciones, etc.) {#idps}

* Se puede informar a las MVPD de dispositivos adicionales mediante un SSO autenticado
* **Satisfacción mejorada del usuario**: experiencia de autenticación mejorada
* **Carga de soporte reducida**: menos llamadas de soporte relacionadas con la autenticación
* **Ventaja competitiva**: Mejor experiencia de usuario que la competencia

## Casos de uso {#use-cases}

### Asignación de identidad {#identity-mapping}

El servicio crea la capacidad de vincular una cuenta de D2C y una de TVE en la misma aplicación.

Más servicios de streaming están construyendo paquetes para ser vendidos a un tercero (MVPD/virtual MVPD/Telco, etc.). Es posible que los usuarios tengan que gestionar varias cuentas en la misma aplicación. Para crear una experiencia de autenticación perfecta, un servicio debe vincular estas cuentas para simplificar el inicio de sesión.

El usuario se autenticará con su cuenta D2C y, a continuación, autenticará ONCE con una cuenta diferente (por ejemplo, MVPD). Con nuestro servicio, estas cuentas se vincularán, lo que significa que en adelante, las autenticaciones posteriores en diferentes dispositivos se pueden realizar solo con la cuenta D2C.

### SSO entre dispositivos {#cross-device-sso}

La autenticación en un dispositivo conectado a la TV puede ser más engorrosa que en un teléfono móvil. Una buena experiencia de usuario sería autenticarse por teléfono y luego pasar esa autenticación al televisor inteligente.

## Componentes clave {#key-components}

* **API de token de servicio**: componente clave que administra de forma segura el mecanismo de inicio de sesión único
* **API de lista**: la aplicación puede ayudar al usuario a comprender la lista de dispositivos en su ecosistema
* **API de vínculo**: la aplicación permite al usuario agregar dispositivos adicionales en su ecosistema
* **Desvincular API**: la aplicación permite al usuario quitar dispositivos de su ecosistema

## Casos de uso detallados {#use-cases-detailed}

### SSO D2C-TVE {#d2c-tve-sso}

Este caso de uso permite la vinculación entre las credenciales de D2C y TVE (MVPD) en un dispositivo y aprovechar ese perfil vinculado en otros dispositivos dentro de la misma aplicación.

![Flujo SSO D2C-TVE](../../../assets/sso_service_d2c_1.png)

![Flujo de SSO entre dispositivos](../../../assets/sso_service_d2c_2.png)

### SSO entre dispositivos {#cross-device-sso-detailed}

Este caso de uso permite a los usuarios que ya se han autenticado en un dispositivo reutilizar el perfil autenticado en la misma aplicación D2C o TVE instalada en otros dispositivos (aplicación necesaria para implementar Adobe Pass REST V2 para la autenticación y autorización).

## Cómo integrar Adobe SSO Service en una aplicación D2C-TVE {#integration}

### Paso 1: Obtención de un identificador común {#step-1}

Para integrar el servicio SSO de Adobe, una implementación de aplicación debe establecer un ID único y persistente para utilizarlo como atributo SSO de identificador común en X-SSO-ID. Esto se puede obtener autenticando a un usuario con un servicio D2C y conservando un atributo sobre esta autenticación.

### Paso 2: Obtención de un token de servicio {#step-2}

El uso del identificador común en X-SSO-ID en el extremo POST /serviceToken recuperará un JWT firmado con la siguiente carga útil:

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

El token de servicio tiene una fecha de caducidad &quot;iat&quot; (emitida el y &quot;exp&quot;) en la que el intervalo de validez del token de servicio es válido. En caso de caducidad, se puede obtener un nuevo JWT mediante el extremo GET /serviceToken con el token de servicio caducado.

### Paso 3: Autenticación mediante la API de REST de Adobe Pass V2 con un MVPD de TVE {#step-3}

La autenticación con Adobe Pass debe implementarse usando el token de servicio: [API REST V2 - Flujos de token de servicio de inicio de sesión único](https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### Paso 4: Vincular otro dispositivo {#step-4}

Desde la aplicación autenticada, se puede crear un vínculo para utilizarlo en un dispositivo diferente mediante la API /link

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

El &quot;código&quot; es un código de vínculo de corta duración en forma de secuencia de 6 dígitos que el usuario introduce en una aplicación no autenticada en un dispositivo secundario

### Paso 5: Recuperación de la autenticación de inicio de sesión único {#step-5}

En un dispositivo diferente, una vez que el usuario introdujo el código, la aplicación puede:

* recuperar identidad del servicio D2C
* recuperar el perfil de MVPD de Adobe Pass con la API de REST V2

El perfil de MVPD será válido mediante SSO durante el período en el que se obtuvo la autenticación inicial.

En caso de que MVPD invalide el perfil o el usuario decida cerrar la sesión desde MVPD, la aplicación que utiliza la API de REST de Adobe Pass V2 dejará de tener un registro de perfil y deberá exigir al usuario que vuelva a autenticarse con MVPD.

### Paso 6: Administración del inicio de sesión único en otros dispositivos {#step-6}

Una aplicación puede obtener información sobre todos los demás dispositivos vinculados con el mismo identificador común mediante la API /list.

En cualquier momento, se puede eliminar un dispositivo del inicio de sesión único desvinculando ese dispositivo con la API /unlink.

## API {#apis}

### API de token de servicio {#service-token-api}

#### Descripción {#service-token-description}

La API de token de servicio se puede utilizar para solicitar y administrar tokens de servicio que habilitan la funcionalidad de inicio de sesión único (SSO) entre varias aplicaciones o dispositivos. Estos tokens de servicio identifican perfiles autenticados (perfiles SSO) y son esenciales para establecer y mantener conexiones SSO.

>[!WARNING]
>
>Los tokens de servicio contienen información de autenticación confidencial. Las aplicaciones deben gestionar estos tokens de forma segura y nunca exponerlos a partes que no son de confianza.

La API de token de servicio proporciona dos extremos principales:

* **POST /api/{serviceProvider}/serviceToken**: obtenga un token de servicio JWS recién creado
* **GET /api/{serviceProvider}/serviceToken**: actualice un token de servicio JWS existente

En caso de que la solicitud de API de token de servicio no se haya podido atender debido a un error de los servicios de autenticación de Adobe Pass, se incluirá información de error adicional como parte de la respuesta de la API.

#### POST: serviceToken {#post-service-token}

##### Solicitud {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>PUBLICAR</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de ruta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificador del proveedor de servicios para el que se solicita el token.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorización</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>
         La generación de la carga del identificador de dispositivo se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.
         <br/><br/>
         Este identificador se utiliza como identificador SSO predeterminado cuando no se proporciona X-SSO-ID.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La información del dispositivo especificada en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a>.
         <br/><br/>
         <b>Recomendamos</b> que se use cuando la plataforma de dispositivo de la aplicación permita proporcionar valores válidos de forma explícita.
         <br/><br/>
         El backend de autenticación de Adobe Pass combinará valores establecidos explícitamente con valores extraídos implícitamente. Cuando no se proporciona, se utilizan los valores extraídos predeterminados.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-LINK</td>
      <td>
         El código de vínculo que asocia esta solicitud con un perfil autenticado existente. Cuando se proporciona, la respuesta incluye un token de servicio para SSO con el perfil que generó el código de vínculo.
         <br/><br/>
         Esto suele utilizarse cuando una aplicación o dispositivo secundario desea conectarse a un perfil autenticado desde una aplicación o dispositivo principal.
      </td>
      <td>obligatorio si no se proporciona x-sso-id</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         El identificador común en el que la aplicación solicita basar el SSO.
         <br/><br/>
         Cuando se proporcione, este identificador se utilizará para establecer un perfil de SSO común entre dispositivos o aplicaciones.
      </td>
      <td>obligatorio si no se proporciona x-sso-link</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceptar</td>
      <td>
         El tipo de medio aceptado por la aplicación cliente.
         <br/><br/>
         Si se especifica, debe ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>El agente de usuario de la aplicación cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Respuesta {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descripción</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Creado</td>
      <td>
        El token de servicio se generó correctamente y se devolvió en el cuerpo de respuesta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Error interno del servidor</td>
      <td>
        El lado del servidor ha encontrado un problema. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
</table>

###### Correcto {#success-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>201</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Estado HTTP (por ejemplo, "CREATED")</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Una firma web JSON (JWS) codificada en Base64 que contiene el token de servicio. Este token se puede utilizar en llamadas de API posteriores para identificar el perfil autenticado y habilitar la funcionalidad de SSO.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoch ms o 0 si hay error</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoch ms o 0 si hay error</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

###### Error {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>400, 401, 500</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>El cuerpo de respuesta puede proporcionar información de error adicional que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples-post-service-token}

### &#x200B;1. Solicite un nuevo token de servicio (con ID de SSO)

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### &#x200B;2. Solicite un nuevo token de servicio (con vínculo SSO)

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET: serviceToken {#get-service-token}

##### Solicitud {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de ruta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificador del proveedor de servicios para el que se solicita el token.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorización</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         Un token de servicio obtenido anteriormente que debe actualizarse.
         <br/><br/>
         Este token debe ser válido o haber caducado recientemente para poder actualizarlo.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceptar</td>
      <td>
         El tipo de medio aceptado por la aplicación cliente.
         <br/><br/>
         Si se especifica, debe ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>El agente de usuario de la aplicación cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Respuesta {#get-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descripción</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        El token de servicio se actualizó correctamente y se devolvió en el cuerpo de respuesta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso o el token de servicio no es válido. El cliente debe obtener un nuevo token de acceso o token de servicio e intentarlo de nuevo. Para obtener más información, consulte la <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Error interno del servidor</td>
      <td>
        El lado del servidor ha encontrado un problema. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
</table>

###### Correcto {#success-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>200</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Estado HTTP (por ejemplo, "OK")</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Firma web JSON (JWS) codificada en Base64 que contiene el token de servicio actualizado. Este token se puede utilizar en llamadas de API posteriores para identificar el perfil autenticado y habilitar la funcionalidad de SSO.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Epoch ms o 0 si hay error</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Epoch ms o 0 si hay error</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

###### Error {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>400, 401, 500</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>El cuerpo de respuesta puede proporcionar información de error adicional que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples-get-service-token}

### &#x200B;1. Solicite actualizar un token de servicio

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### API de vínculo {#link-api}

#### Descripción {#link-description}

La API de vínculo se puede utilizar para solicitar un código de vínculo (o código QR) que pueda habilitar el inicio de sesión único (SSO) entre varias aplicaciones o dispositivos. Este código de vínculo permite a los usuarios conectar una nueva aplicación o dispositivo a un perfil autenticado existente (perfil SSO), lo que proporciona una experiencia SSO perfecta entre aplicaciones o dispositivos.

La API de vínculo requiere que se proporcione un token de servicio válido en el encabezado AD-Service-Token.

El código de vínculo generado generalmente se muestra a los usuarios de una aplicación o dispositivo principal y se introduce en una aplicación o dispositivo secundario para establecer la conexión SSO. El código de vínculo tiene un periodo de validez limitado (normalmente de 5 a 30 minutos) y está diseñado para utilizarse una sola vez.

En caso de que la solicitud de API de vínculo no se haya podido atender debido a un error de los servicios de autenticación de Adobe Pass, se incluirá información de error adicional como parte del resultado de la respuesta de la API de vínculo.

#### POST: vínculo {#post-link}

##### Solicitud {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>PUBLICAR</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de ruta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>Identificador del proveedor de servicios para el que se solicita el token.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorización</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La generación de la carga del identificador de dispositivo se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         La generación del token de servicio se describe en la documentación de la API del token de servicio.
         <br/><br/>
         Este token de servicio identifica el perfil autenticado para el que se generará el código de vínculo.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceptar</td>
      <td>
         El tipo de medio aceptado por la aplicación cliente.
         <br/><br/>
         Si se especifica, debe ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>El agente de usuario de la aplicación cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Respuesta {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descripción</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Creado</td>
      <td>
        El código de vínculo se generó correctamente y se devolvió en el cuerpo de respuesta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Error interno del servidor</td>
      <td>
        El lado del servidor ha encontrado un problema. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
</table>

###### Correcto {#success-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>201</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Estado HTTP (por ejemplo, "CREATED")</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">vincular</td>
      <td>Código numérico o alfanumérico corto que se puede utilizar en una aplicación o dispositivo secundario para establecer una conexión SSO.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>La marca de tiempo (en milisegundos desde la época) cuando el código de vínculo se vuelve válido.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>La marca de tiempo (en milisegundos desde la época) cuando caduca el código de vínculo.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

###### Error {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>400, 401, 500</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>El cuerpo de respuesta puede proporcionar información de error adicional que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples-post-link}

### &#x200B;1. Solicite un código de vínculo para un perfil autenticado existente

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### Desvincular API {#unlink-api}

#### Descripción {#unlink-description}

La API de desvinculación se puede utilizar para solicitar la eliminación de uno o varios dispositivos de un perfil autenticado (perfil SSO). Esta API permite a los usuarios desconectar dispositivos de su configuración de SSO, lo que permite controlar qué dispositivos tienen acceso a su perfil autenticado.

>[!WARNING]
>
>La API de desvinculación requiere que se proporcione un token de servicio válido en el encabezado AD-Service-Token.

En caso de que la solicitud de API de desvinculación no se haya podido atender debido a un error de los servicios de autenticación de Adobe Pass, se incluirá información de error adicional como parte del resultado de la respuesta de API de desvinculación.

#### POST: desvincular {#post-unlink}

##### Solicitud {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/api/{serviceProvider}/unlink</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>PUBLICAR</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de ruta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>El identificador del proveedor de servicios.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">dispositivos</td>
      <td>
         Matriz de identificadores de dispositivo para desvincular.
         <br/><br/>
         Ejemplo:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorización</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         El tipo de medio aceptado para los recursos que se envían.
         <br/><br/>
         Debe ser application/json.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La generación de la carga del identificador de dispositivo se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         La generación del token de servicio se describe en la documentación de la API del token de servicio.
         <br/><br/>
         Este token de servicio identifica el perfil autenticado para el que se desvincularán los dispositivos.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceptar</td>
      <td>
         El tipo de medio aceptado por la aplicación cliente.
         <br/><br/>
         Si se especifica, debe ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>El agente de usuario de la aplicación cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Respuesta {#post-unlink-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descripción</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Los dispositivos solicitados se han desvinculado correctamente de la configuración de SSO.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Método no permitido</td>
      <td>
        El método HTTP no es válido, el cliente debe utilizar un método HTTP permitido para el recurso solicitado e intentarlo de nuevo.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Error interno del servidor</td>
      <td>
        El lado del servidor ha encontrado un problema. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
</table>

###### Correcto {#success-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>200</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Información sobre el resultado de la operación: "OK"</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         Lista de dispositivos desvinculados correctamente.
         <br/><br/>
         Ejemplo:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

###### Error {#error-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>400, 401, 405, 500</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>El cuerpo de respuesta puede proporcionar información de error adicional que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples-post-unlink}

### &#x200B;1. Solicite desvincular dispositivos de un perfil SSO

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### &#x200B;2. Solicite desvincular dispositivos con éxito parcial

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### API de lista {#list-api}

#### Descripción {#list-description}

La API de lista se puede utilizar para obtener una lista de dispositivos conectados actualmente a un perfil autenticado (perfil SSO). Esta API permite a los usuarios y las aplicaciones ver qué dispositivos forman parte de su configuración de SSO, lo que proporciona visibilidad y capacidades de administración para su experiencia autenticada en varios dispositivos.

>[!WARNING]
>
>La API de lista requiere que se proporcione un token de servicio válido en el encabezado AD-Service-Token.

La API de lista devuelve detalles sobre cada dispositivo en el perfil autenticado (perfil SSO), incluidos los identificadores de dispositivo y los metadatos que pueden ayudar a los usuarios a reconocer sus dispositivos. Esta información se puede utilizar para ayudar a los usuarios a tomar decisiones informadas sobre qué dispositivos deben permanecer en su configuración de SSO.

En caso de que la solicitud de API de lista no se haya podido atender debido a un error de los servicios de autenticación de Adobe Pass, se incluirá información de error adicional como parte del resultado de la respuesta de API de lista.

#### GET: lista {#get-list}

##### Solicitud {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/api/{serviceProvider}/list</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de ruta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>El identificador del proveedor de servicios.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorización</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>La generación de la carga del identificador de dispositivo se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         La generación del token de servicio se describe en la documentación de la API del token de servicio.
         <br/><br/>
         Este token de servicio identifica el perfil autenticado para el que se recuperará la lista de dispositivos.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceptar</td>
      <td>
         El tipo de medio aceptado por la aplicación cliente.
         <br/><br/>
         Si se especifica, debe ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>El agente de usuario de la aplicación cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Respuesta {#get-list-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descripción</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        La lista de dispositivos en la configuración de SSO se recuperó correctamente y se devolvió en el cuerpo de la respuesta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Método no permitido</td>
      <td>
        El método HTTP no es válido, el cliente debe utilizar un método HTTP permitido para el recurso solicitado e intentarlo de nuevo.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Error interno del servidor</td>
      <td>
        El lado del servidor ha encontrado un problema. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.
      </td>
   </tr>
</table>

###### Correcto {#success-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>200</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">dispositivos</td>
      <td>
         JSON que contiene un mapa de pares de clave y valor.
         <br/><br/>
         <b>Clave:</b> deviceId: la carga del identificador del dispositivo tal como se describe en la documentación del encabezado <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>
         <br/><br/>
         <b>Valor:</b> atributos - JSON que contiene un mapa de atributos de metadatos de dispositivo, incluidos:
         <ul>
            <li>tipo de dispositivo</li>
            <li>plataforma</li>
            <li>agente de usuario</li>
            <li>otros metadatos relevantes que ayuden a identificar el dispositivo</li>
         </ul>
         Los valores de los atributos pueden ser simples (cadena, entero, booleano, etc.)
      </td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

###### Error {#error-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Estado</td>
      <td>400, 401, 405, 500</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>El cuerpo de respuesta puede proporcionar información de error adicional que se adhiera a la documentación de <a href="https://experienceleague.adobe.com/es/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples-get-list}

### &#x200B;1. Solicitud para enumerar dispositivos en un perfil SSO

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. Solicitar la lista de dispositivos sin dispositivos vinculados

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## Códigos de error {#error-codes}

### Estructura de respuesta de error {#error-structure}

Todas las respuestas de error incluyen estos campos:

| Campo | Tipo | Descripción |
|:---|:---|:---|
| status | Entero | Código de estado HTTP (400, 401, 500, etc.) |
| código | Cadena | Código de error legible por máquina |
| message | Cadena | Descripción legible en lenguaje natural |
| acción | Cadena | Acción sugerida para el cliente |
| helpUrl | Cadena | Vínculo de documentación |
| trazar | Cadena | ID único de solicitud (UUID) para correlación |

**Ejemplo de respuesta de error**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=es",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**Ejemplo de respuesta correcta**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>Las respuestas de éxito NO incluyen un campo de error. Las respuestas de error NO incluyen campos de éxito.

### Catálogo de códigos de error {#error-catalog}

#### Errores generales

| código | status | message | acción |
|:---|:---|:---|:---|
| token_invalid | 400 | El token proporcionado no es válido | get_new_token |
| token_expire | 401 | El token ha caducado | get_new_token |
| header_missing | 400 | Falta un encabezado obligatorio | check_headers |
| no autorizado | 401 | Acceso no autorizado | ninguno |
| internal_error | 500 | Se ha producido un error interno | ninguno |

#### Errores de validación

| código | status | message | acción |
|:---|:---|:---|:---|
| request_null | 400 | El objeto de solicitud no puede ser nulo | ninguno |

#### Errores de ServiceToken de POST

| código | status | message | acción |
|:---|:---|:---|:---|
| header_missing | 400 | Se requiere el encabezado x-sso-id o x-sso-link para las solicitudes POST | check_headers |
| header_missing | 400 | El encabezado AP-Device-Identifier es necesario para las solicitudes POST | check_headers |

#### Errores de ServiceToken de GET

| código | status | message | acción |
|:---|:---|:---|:---|
| header_missing | 400 | Se requiere el encabezado AD-Service-Token para las solicitudes de GET | check_headers |
| header_invalid | 401 | Firma JWT no válida en AD-Service-Token | get_new_token |
| header_invalid | 401 | Error al validar la firma JWT | get_new_token |
| header_invalid | 401 | Falta el asunto (sub) JWT o está vacío en AD-Service-Token | get_new_token |
| header_invalid | 401 | Error al extraer el asunto JWT | get_new_token |

#### Errores de validación de vínculos

| código | status | message | acción |
|:---|:---|:---|:---|
| header_missing | 401 | Se requiere el encabezado AD-Service-Token para las solicitudes de vínculo | check_headers |
| header_invalid | 401 | Firma JWT no válida en AD-Service-Token | get_new_token |
| header_invalid | 401 | Error al validar la firma JWT | get_new_token |

#### Desvincular errores de validación

| código | status | message | acción |
|:---|:---|:---|:---|
| header_missing | 401 | Se requiere el encabezado AD-Service-Token para las solicitudes de desvinculación | check_headers |
| header_invalid | 401 | Firma JWT no válida en AD-Service-Token | get_new_token |
| header_invalid | 401 | Error al validar la firma JWT | get_new_token |
| header_invalid | 401 | Falta el asunto (sub) JWT o está vacío en AD-Service-Token | get_new_token |
| request_invalid | 400 | La lista de dispositivos no puede ser nula ni estar vacía | check_request_body |

#### Errores de validación de lista

| código | status | message | acción |
|:---|:---|:---|:---|
| header_missing | 401 | Se requiere el encabezado AD-Service-Token para las solicitudes de lista | check_headers |
| header_invalid | 401 | Firma JWT no válida en AD-Service-Token | get_new_token |
| header_invalid | 401 | Error al validar la firma JWT | get_new_token |
| header_invalid | 401 | Falta el asunto (sub) JWT o está vacío en AD-Service-Token | get_new_token |
| header_invalid | 401 | Error al extraer el asunto JWT | get_new_token |

