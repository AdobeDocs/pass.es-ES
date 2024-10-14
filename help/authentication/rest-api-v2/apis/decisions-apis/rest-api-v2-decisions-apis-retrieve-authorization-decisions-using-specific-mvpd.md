---
title: Recuperar decisiones de autorización utilizando mvpd específico
description: 'API REST V2: Recupere decisiones de autorización utilizando mvpd específico'
exl-id: e8889395-4434-4bec-a212-a8341bb9c310
source-git-commit: 6c328eb2c635a1d76fc7dae8148a4de291c126e0
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 1%

---

# Recuperar decisiones de autorización utilizando mvpd específico {#retrieve-authorization-decisions-using-specific-mvpd}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/throttling-mechanism.md).

## Solicitud {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/api/v2/{serviceProvider}/decisions/authorize/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de ruta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>El identificador único interno asociado con el proveedor de servicios durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">recursos</td>
      <td>La lista de recursos que requieren una decisión de MVPD antes de que se puedan reproducir.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la documentación del encabezado <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Autorización</a>.</td>
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
      <td>La generación de la carga del identificador de dispositivo se describe en la documentación del encabezado <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La generación de la carga de información del dispositivo se describe en la <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">documentación del encabezado X-Device-Info</a>.
         <br/><br/>
         Se recomienda utilizarlo siempre que la plataforma de dispositivos de la aplicación permita la provisión explícita de valores válidos.
         <br/><br/>
         Cuando se proporciona, el backend de autenticación de Adobe Pass combina explícitamente los valores establecidos con los valores extraídos implícitamente (de forma predeterminada).
         <br/><br/>
         Cuando no se proporciona, el backend de autenticación de Adobe Pass utilizará valores extraídos implícitamente (de forma predeterminada).
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         La dirección IP del dispositivo de flujo continuo.
         <br/><br/>
         Se recomienda utilizarlo siempre para implementaciones de servidor a servidor, especialmente cuando la llamada la realice el servicio del programador en lugar del dispositivo de flujo continuo.
         <br/><br/>
         Para implementaciones de cliente a servidor, la dirección IP del dispositivo de flujo continuo se envía implícitamente.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token</td>
      <td>
        La generación de la carga de inicio de sesión único para el método de identidad de Platform se describe en la documentación del encabezado <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a>.
        <br/><br/>
        Para obtener más información sobre los flujos habilitados para el inicio de sesión único que utilizan una identidad de plataforma, consulte la documentación de <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Inicio de sesión único mediante flujos de identidad de plataforma</a>.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        La generación de la carga de inicio de sesión único para el método del token de servicio se describe en la documentación del encabezado <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
        <br/><br/>
        Para obtener más información sobre los flujos habilitados para el inicio de sesión único que utilizan un token de servicio, consulte la documentación de <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">Inicio de sesión único mediante flujos de token de servicio</a>.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La generación de la carga de inicio de sesión único para el método Partner se describe en la <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">documentación del encabezado AP-Partner-Framework-Status</a>.
        <br/><br/>
        Para obtener más información sobre los flujos habilitados para el inicio de sesión único que usan un socio, consulte la documentación de <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Inicio de sesión único con flujos de socios</a>.</td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td>La generación de la carga del identificador único de usuario se describe en la documentación del encabezado <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a>.</td>
      <td>opcional</td>
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

## Respuesta {#response}

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
        El cuerpo de respuesta contiene una lista de decisiones con información adicional.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo. El cuerpo de respuesta puede contener información de error conforme a la documentación de <a href="../../../enhanced-error-codes.md">Códigos de error mejorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="../../../dcr-api/dynamic-client-registration-overview.md">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Método no permitido</td>
      <td>
        El método HTTP no es válido, el cliente debe utilizar un método HTTP permitido para el recurso solicitado e intentarlo de nuevo. Para obtener más información, consulte la sección <a href="#request">Solicitud</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Error interno del servidor</td>
      <td>
        El lado del servidor ha encontrado un problema. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="../../../enhanced-error-codes.md">Códigos de error mejorados</a>.
      </td>
   </tr>
</table>

### Correcto {#success}

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
      <td style="background-color: #DEEBFF;">decisiones</td>
      <td>
         JSON que contiene una lista de elementos, cada uno con los siguientes atributos:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Atributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">resource</td>
                <td>El identificador de recurso para el que se devuelve la decisión de autorización.</td>
                <td><i>obligatorio</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">serviceProvider</td>
                <td>El identificador único interno asociado con el proveedor de servicios durante el proceso de incorporación.</td>
                <td><i>obligatorio</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpd</td>
                <td>El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.</td>
                <td><i>obligatorio</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">authorized</td>
                <td>El estado de decisión del recurso, que puede ser "true" o "false".</td>
                <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">origen</td>
               <td>
                  Información sobre la fuente de decisión.
                  <br/><br/>
                  Los valores posibles son:
                  <ul>
                    <li><b>mvpd</b><br/>La decisión la emite el extremo de autorización de MVPD.</li>
                    <li><b>degradación</b><br/>La decisión se emite como resultado del acceso degradado.</li>
                    <li>La decisión <b>temppass</b><br/>se ha emitido como resultado del acceso temporal.</li>
                    <li><b>dummy</b><br/>La decisión se emite como resultado de la función de autorización de dummy.</li>
                  </ul>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token</td>
               <td>
                  Información sobre el token de medios.
                  <br/><br/>
                  El objeto JSON tiene los atributos siguientes:
                  <ul>
                    <li><b>notBefore</b><br/>Marca de tiempo antes de la cual el token de medios no es válido.</li>
                    <li><b>notAfter</b><br/>Marca de tiempo después de la cual el token de medios no es válido.</li>
                    <li><b>serializedToken</b><br/>El token multimedia codificado en Base64.</li>
                  </ul>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>La marca de tiempo antes de la cual la decisión no es válida.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>La marca de tiempo después de la cual la decisión no es válida.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">error</td>
               <td>El error proporciona información adicional sobre la decisión "Denegar" que se adhiere a la documentación de <a href="../../../enhanced-error-codes.md">Códigos de error mejorados</a>.</td>
               <td>opcional</td>
            </tr>
         </table>
      </td>
      <td><i>obligatorio</i></td>
</table>

### Error {#error}

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
      <td>El cuerpo de respuesta puede proporcionar información de error adicional que se adhiera a la documentación de <a href="../../../enhanced-error-codes.md">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples}

### 1. Recupere las decisiones de autorización utilizando mvpd regular

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/decisions/authorize/Cablevision

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Respuesta - Disponible]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "Cablevision",
            "source": "mvpd",
            "authorized": true,
            "token": {
                "issuedAt": 1697094207324,
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!ENDTABS]

### 2. Recuperar decisiones de autorización utilizando un pase temporal

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/apasstest1/decisions/authorize/TempPass_TEST40 HTTP/1.1

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Respuesta - Disponible]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "issuedAt": 1697094207324,
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!TAB Respuesta - Iniciada]

```JSON
HTTP/1.1 200 OK
Content-Type: applicationjson
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "issuedAt": 1695360527896,
                "notBefore": 1695360527896,
                "notAfter": 1695360707896,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            }
        }
    ]
}
```

>[!TAB Respuesta - Caducada]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_expired",
                "message": "TempPass has expired.",
                 "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Respuesta: configuración no válida]

```JSON
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 3. Recuperar las decisiones de autorización utilizando un pase temporal promocional

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/apasstest1/decisions/authorize/flexibleTempPass HTTP/1.1

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB Respuesta - Disponible]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
    ]
}
```

>[!TAB Respuesta - Iniciada]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
    ]
}
```

>[!TAB Respuesta - Caducada]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_expired",
                "message": "TempPass has expired.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Respuesta - Consumida]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "temppass_max_resources_exceeded",
                "message": "Flexible TempPass maximum resources exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!TAB Respuesta: configuración no válida]

```JSON
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8      
 
{
    "status": 500,
    "code": "temppass_invalid_configuration",
    "message": "TempPass configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB Respuesta - Identidad no válida]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8 
 
{
    "status": 400,
    "code": "temppass_invalid_identity",
    "message": "TempPass is not available for the specified identity.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 4. Recuperar decisiones de autorización utilizando mvpd degradado

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/decisions/authorize/degradedMvpd

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB Respuesta - Degradación de AuthNAll]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

**Nota:** En este caso, la regla AuthZAll tiene &quot;channel&quot; : &quot;apasstest1&quot;

>[!TAB Respuesta: degradación de AuthZAll]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

**Nota:** En este caso, la regla AuthZAll tiene &quot;channel&quot; : &quot;apasstest1&quot;

>[!TAB Respuesta: degradación de AuthZNone]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
 
    ]
}
```

**Nota:** En este caso, la regla AuthZNone tiene &quot;channel&quot; : &quot;apasstest1&quot;

>[!TAB Respuesta: Regla de degradación caducada]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8  
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_configuration_change",
                "message": "AuthXAll degradation configuration changed, please try again!",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
