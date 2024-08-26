---
title: Recuperar solicitud de autenticación de socio
description: 'API de REST V2: recuperar la solicitud de autenticación del socio'
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '1095'
ht-degree: 1%

---


# Recuperar solicitud de autenticación de socio {#retrieve-partner-authentication-request}

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
      <td>/api/v2/{serviceProvider}/session/sso/{partner}</td>
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
      <td style="background-color: #DEEBFF;">socio</td>
      <td>El nombre del socio (por ejemplo, Apple) que proporciona el marco de inicio de sesión único integrado con los flujos de autenticación de Adobe Pass.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Dominio de origen de la aplicación que realiza el inicio de sesión de MVPD.
        <br/><br/>
        Si la plataforma del dispositivo de streaming tiene limitaciones para proporcionar un valor, la aplicación tendrá que reanudar la sesión de autenticación y proporcionar un valor válido.
        <br/><br/>
        Se utilizará en el caso de escenarios de reserva en los que la respuesta indique que la aplicación de flujo continuo debe continuar con el flujo de autenticación básico.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        Dirección URL de redireccionamiento final a la que se desplaza el agente de usuario cuando se completa el flujo de autenticación de la MVPD.
        <br/><br/>
        El valor debe tener codificación URL.
        <br/><br/>
        Si la plataforma del dispositivo de streaming tiene limitaciones para proporcionar un valor, la aplicación tendrá que reanudar la sesión de autenticación y proporcionar un valor válido.
        <br/><br/>
        Se utilizará en el caso de escenarios de reserva en los que la respuesta indique que la aplicación de flujo continuo debe continuar con el flujo de autenticación básico.
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
      <td>La generación de la carga útil del token de portador se describe en la documentación del encabezado <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Autorización</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         El tipo de medio aceptado para los recursos que se envían.
         <br/><br/>
         Debe ser application/x-www-form-urlencoded.
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
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        La generación de la carga de inicio de sesión único para el método Partner se describe en la <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">documentación del encabezado AP-Partner-Framework-Status</a>.
        <br/><br/>
        Para obtener más información sobre los flujos habilitados para el inicio de sesión único que usan un socio, consulte la documentación de <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Inicio de sesión único con flujos de socios</a>.</td>
      <td>opcional</td>
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
        El cuerpo de respuesta contiene información sobre las siguientes acciones necesarias para realizar la autenticación.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="../../../enhanced-error-codes.md">Códigos de error mejorados</a>.
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
      <td style="background-color: #DEEBFF;"></td>
      <td>
         El objeto JSON tiene los atributos siguientes:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Atributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  La acción que debe realizar el dispositivo de flujo continuo para completar el flujo de autenticación.
                  <br/><br/>
                  Los valores posibles son:
                  <ul>
                    <li><b>perfil_de_socio</b><br/>El dispositivo de transmisión puede usar la solicitud de autenticación de socio proporcionada para obtener una respuesta de autenticación de socio que se pueda aprovechar para recuperar un perfil.</li>
                    <li><b>autenticar</b><br/>Cuando el flujo de inicio de sesión único del socio no puede continuar, el dispositivo de flujo puede volver al flujo de autenticación básico.<br/>El dispositivo de transmisión por secuencias u otro dispositivo debe abrir la dirección URL proporcionada en un agente de usuario.</li>
                    <li><b>reanudar</b><br/>Cuando el flujo de inicio de sesión único del socio no puede continuar, el dispositivo de flujo puede volver al flujo de autenticación básico.<br/>El dispositivo de transmisión por secuencias u otro dispositivo debe proporcionar los parámetros que faltan y reanudar la sesión de autenticación mediante el código.</li>
                    <li><b>authorize</b><br/>El dispositivo de transmisión puede continuar directamente con los flujos de decisiones.</li>
                  </ul>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  El tipo de interacción que debe realizar el dispositivo de flujo continuo para continuar el flujo con la acción especificada por el atributo actionName.
                  <br/><br/>
                  Los valores posibles son:
                  <ul>
                    <li><b>interactivo</b><br/>El flujo continúa con una navegación a la dirección URL proporcionada mediante un agente de usuario.</li>
                    <li><b>direct</b><br/>El flujo continúa con una llamada directa a la dirección URL proporcionada mediante un cliente HTTP disponible para la implementación del cliente.</li>
                  </ul>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>
                    Los parámetros que faltan que deben proporcionarse para completar el flujo de autenticación básico.
                    <br/><br/>
                    Este campo está presente cuando el flujo de inicio de sesión único del socio no puede continuar.
               </td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>Dirección URL a la que debe navegar la aplicación cliente.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">código</td>
               <td>
                    El código de autenticación que se puede utilizar en una aplicación secundaria para reanudar la sesión de autenticación.
                    <br/><br/>
                    Este campo está presente cuando el flujo de inicio de sesión único del socio no puede continuar.
               </td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">authenticationRequest</td>
               <td>
                    La solicitud de autenticación del socio que se utilizará en el flujo de autenticación con el socio fuera del sistema de autenticación de Adobe Pass.
                    <br/><br/>
                    Este campo está presente cuando puede continuar el flujo de inicio de sesión único del socio.
                    <br/><br/>
                    El objeto JSON tiene los atributos siguientes:
                    <ul>
                        <li><b>type</b><br/>Indica el tipo de protocolo admitido por MVPD (solo SAML).</li>
                        <li><b>solicitud</b><br/>La solicitud SAML.</li>
                        <li><b>atributos</b><br/>Atributos de solicitud de SAML.</li>
                    </ul>
               </td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>Identificador opaco que se puede utilizar para rastrear la actividad del usuario.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>El identificador único interno asociado con el proveedor de servicios durante el proceso de incorporación.</td>
               <td><i>obligatorio</i></td>
            </tr>
         </table>
      </td>
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
      <td style="background-color: #DEEBFF;">error</td>
      <td>El error proporciona información adicional que se adhiere a la documentación de <a href="../../../enhanced-error-codes.md">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples}

### 1. SSO de socio válido activado

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"partner_profile",
   "actionType":"direct",
   "url":"/v2/REF30/profiles/sso/Apple/Cablevision",
   "sessionId":"83c046be-ea4b-4581-b5f2-13e56e69dee9",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30",
   "authenticationRequest":{
      "type":"saml",
      "request":"PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRG...."
   }
}    
```

>[!ENDTABS]

### 2. MVPD degradada

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
 
{          
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/api/v2/REF30/decisions",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30",
   "sessionId":"14d4f239-e3b1-4a4a-b8b3-6395b968a260"
}
```

>[!ENDTABS]

### 3. Integración deshabilitada

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 403
Content-Type: application/json; charset=utf-8
 
{
    "errors" : [
        {
            "code": "unknown_integration",
            "message": "The integration between the specified programmer and identity provider doesn't exist or it's disabled. Use the TVE Dashboard to register or enable the required integration.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}
```

>[!ENDTABS]

### 4. SSO de socio no habilitado (en la configuración Pass o en VSA): volver a la autenticación regular, todos los parámetros obligatorios presentes

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status : ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"authenticate",
   "actionType":"interactive",
   "url":"/v2/authenticate/REF30/OKTWW2W",
   "code":"OKTWW2W",
   "sessionId":"748f0b9e-a2ae-46d5-acd9-4b4e6d71add7",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}  
```

>[!ENDTABS]

### 5. SSO de socio no habilitado (en la configuración de Pass o en VSA): volver a la autenticación normal, faltan parámetros obligatorios y la sesión de autenticación debe reanudarse

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
domainName=adobe.com
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK  
 
{
   "actionName":"resume",
   "actionType":"direct",
   "missingParameters":[
      "redirectUrl"
   ],
   "url":"/v2/REF30/sessions/SB7ZRIO",
   "code":"SB7ZRIO",
   "sessionId":"1476173f-5088-43b8-b7c3-8cf3a185de0a",
   "mvpd":"Cablevision",
   "serviceProvider":"REF30"
}
```

>[!ENDTABS]
