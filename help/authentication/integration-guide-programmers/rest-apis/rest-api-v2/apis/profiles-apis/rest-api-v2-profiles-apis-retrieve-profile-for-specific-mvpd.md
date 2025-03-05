---
title: Recuperar perfil para mvpd específico
description: 'API de REST V2: recuperar el perfil de mvpd específico'
exl-id: ed1abc33-c279-4465-b5a0-b4e5b892076e
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 1%

---

# Recuperar perfil para mvpd específico {#retrieve-profile-for-specific-mvpd}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Asegúrese de visitar también las [Preguntas frecuentes sobre la API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

## Solicitud {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/api/v2/{serviceProvider}/profiles/{mvpd}</td>
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
      <td>El identificador único interno asociado con el proveedor de servicios durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.</td>
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
        La generación de la carga de inicio de sesión único para el método de identidad de Platform se describe en la <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">documentación del encabezado Adobe-Subject-Token</a>.
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
        El cuerpo de la respuesta contiene un mapa de perfiles válidos, que pueden estar vacíos.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitud incorrecta</td>
      <td>
        La solicitud no es válida, el cliente debe corregirla e intentarlo de nuevo. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Códigos de error mejorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>No autorizado</td>
      <td>
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">Información general sobre el registro de clientes dinámicos</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Prohibido</td>
      <td>
        El tiempo de vida del acceso temporal (TTL) ha caducado o se ha superado el número máximo de recursos, por lo que el cliente debe indicar al usuario que inicie un flujo de autenticación básico con un MVPD normal. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Códigos de error mejorados</a>.
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
        El lado del servidor ha encontrado un problema. El cuerpo de respuesta puede contener información de error que se adhiera a la documentación de <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Códigos de error mejorados</a>.
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
      <td style="background-color: #DEEBFF;">perfiles</td>
      <td>
        JSON que contiene un mapa de pares de clave y valor.
        <br/><br/>
        El elemento clave se define con el siguiente valor:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valor</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.</td>
               <td><i>obligatorio</i></td>
            </tr>
         </table>
         El elemento value se define mediante los atributos siguientes:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Atributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>La marca de tiempo antes de la cual el perfil no es válido.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>La marca de tiempo después de la cual el perfil no es válido.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">emisor</td>
               <td>
                  La entidad propietaria del perfil.
                  <br/><br/>
                  Los valores posibles son:
                  <ul>
                    <li><b>mvpd (por ejemplo, Spectrum, Cablevision, etc.)</b><br/>El perfil se creó como resultado de la autenticación básica, el inicio de sesión único mediante la identidad de plataforma o el inicio de sesión único mediante el token de servicio.</li>
                    <li><b>Adobe</b><br/>El perfil se creó como resultado de: acceso degradado, acceso temporal.</li>
                    <li><b>Apple</b><br/>El perfil se creó como resultado de: inicio de sesión único con Apple de socio.</li>
                  </ul>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">type</td>
               <td>
                  El tipo de perfil.
                  <br/><br/>
                  Los valores posibles son:
                  <ul>
                    <li><b>regular</b><br/>El perfil se creó como resultado de: autenticación básica.</li>
                    <li><b>degradado</b><br/>El perfil se creó como resultado de: acceso degradado.</li>
                    <li><b>temporal</b><br/>El perfil se creó como resultado de: acceso temporal.</li>
                    <li><b>appleSSO</b><br/>El perfil se creó como resultado de: inicio de sesión único con Apple de socio.</li>
                    <li><b>platformSSO</b><br/>El perfil se creó como resultado de: inicio de sesión único mediante la identidad de la plataforma.</li>
                    <li><b>serviceTokenSSO</b><br/>El perfil se creó como resultado de: inicio de sesión único mediante el token de servicio.</li>
                  </ul>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">atributos</td>
               <td>
                    La lista de atributos de metadatos de usuario.
                    <br/><br/>
                    Estos atributos pueden ser:
                    <ul>
                        <li>Obligatorio, como "userId"</li>
                        <li>No obligatorio, como "zip", "householdId", "maxRating", etc.</li>
                    </ul>
                    Los valores de los atributos pueden ser:
                    <ul>
                        <li>simple</li>
                        <li>lista</li>
                        <li>asignar</li>
                    </ul>
               </td>
               <td><i>obligatorio</i></td>
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
      <td>400, 401, 403, 405, 500</td>
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
      <td>El cuerpo de respuesta puede proporcionar información de error adicional que se adhiera a la documentación de <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples}

### 1. Recupere el perfil de mvpd específico obtenido mediante autenticación básica

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/v2/REF30/profiles/Spectrum HTTP/1.1 

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "Spectrum": {
            "notBefore": 1623943955,
            "notAfter": 1623951155,
            "issuer": "Spectrum",
            "type": "regular",
            "attributes": {
                "userId": {
                    "value": "BASE64_value_userId",
                    "state": "plain"
                },
                "householdId" : {
                    "value": "BASE64_value_householdId",
                    "state": "plain"
                },
                "zip" : {
                    "value": "BASE64_value_zip",
                    "state": "enc"
                },
                "parental-controls" : {
                    "value": BASE64_value_parental-controls,
                    "state": "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### 2. Recupere el perfil de mvpd específico obtenido mediante autenticación básica o inicio de sesión único mediante el método de token de servicio

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/v2/REF30/profiles/AdobeShibboleth HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJkZDNmYWIyN2NmMjg0ZmU2ZWU0ZDY3ZmExZjY4MzE3YyIsImlzcyI6IkFkb2JlIiw.....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "profiles": {
      "AdobeShibboleth": {
         "notBefore": 1748073636999,
         "notAfter": 1748105173000,
         "issuer": "AdobeShibboleth",
         "type": "serviceTokenSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd1...",
               "state": "plain"
            },
            "userID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd14aTV....",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobeShibboleth",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. Recupere el perfil de mvpd específico obtenido mediante autenticación básica o inicio de sesión único mediante el método de identidad de Platform

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/v2/REF30/profiles/AdobePass_SMI HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIyMmM4MDU1MjEzMDIwYzhmZGYzOGZkMTI1YWViMzUzYSIsImlzcyI6....
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
   "profiles": {
      "AdobePass_SMI": {
         "notBefore": 1724337476000,
         "notAfter": 1724345252000,
         "issuer": "AdobePass_SMI",
         "type": "platformSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "userID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobePass_SMI",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 4. Recuperar perfil para TempPass básico

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/v2/REF30/profiles/TempPass_TEST40 HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Respuesta - Disponible]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "TempPass_TEST40": {
            "notBefore": 1697718650206,
            "notAfter": 1697718710206,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "expiration_date": {
                    "value": 1697718710206,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB Respuesta: límite de duración superado]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_duration_limit_exceeded",
    "message": "The temporary access duration limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB Respuesta: configuración no válida]

```HTTPS
HTTP/1.1 500 Internal Server Error

Content-Type: application/json;charset=UTF-8

{
    "status": 500,
    "code": "invalid_configuration_temporary_access",
    "message": "The temporary access configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "configuration"
}
```

>[!ENDTABS]

### 5. Recuperar perfil para TempPass promocional

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/v2/REF30/profiles/flexibleTempPass HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Respuesta - Disponible]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "flexibleTempPass": {
            "notBefore": 1697720528524,
            "notAfter": 1697720588524,
            "issuer": "Adobe",
            "type": "temporary",
            "attributes": {
                "remaining_resources": {
                    "value": 1,
                    "state": "plain"
                },
                "used_assets": {
                    "value": [
                        "res04",
                        "res02",
                        "res03",
                        "res01"
                    ],
                    "state": "plain"
                },
                "expiration_date": {
                    "value": 1697720528524,
                    "state": "plain"
                },
                "userID": {
                    "value": "temppass_0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!TAB Respuesta: límite de duración superado]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_duration_limit_exceeded",
    "message": "The temporary access duration limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB Respuesta: Se Superó El Límite De Recursos]

```HTTPS
HTTP/1.1 403 Forbidden

Content-Type: application/json;charset=UTF-8

{
    "status": 403,
    "code": "temporary_access_resources_limit_exceeded",
    "message": "The temporary access resources limit has been exceeded.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "authentication"
}
```

>[!TAB Respuesta: configuración no válida]

```HTTPS
HTTP/1.1 500 Internal Server Error

Content-Type: application/json;charset=UTF-8

{
    "status": 500,
    "code": "invalid_configuration_temporary_access",
    "message": "The temporary access configuration is invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!TAB Respuesta - Identidad no válida]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{
    "status": 400,
    "code": "invalid_header_identity_for_temporary_access",
    "message": "The identity for temporary access header value is missing or invalid.",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
    "action": "none"
}
```

>[!ENDTABS]

### 6. Recuperar el perfil para mvpd específico mientras se aplica la degradación

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/v2/REF30/profiles/${degradedMvpd} HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Respuesta - Degradación de AuthNAll]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "profiles": {
        "${degradedMvpd}": {
            "notBefore": 1697719042666,
            "notAfter": 1697719102666,
            "issuer": "Adobe",
            "type": "degraded",
            "attributes":
                "userID": {
                    "value": "95cf93bcd183214a0bdf451aa9c8fa60e80f6b99ab48310c73b480f1",
                    "state": "plain"
                }
            }
        }
    }
}
```

>[!IMPORTANT]
>
> `95cf93bcd183214a` es un prefijo específico de degradación.

>[!ENDTABS]
