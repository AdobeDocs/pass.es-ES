---
title: Crear sesión de autenticación
description: 'API de REST V2: crear sesión de autenticación'
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 1%

---


# Crear sesión de autenticación {#create-authentication-session}

>[!NOTE]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API REST V2 está limitada por la documentación de [Mecanismo de limitación](/help/authentication/throttling-mechanism.md).

## Solicitud {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/api/v2/{serviceProvider}/session</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Parámetros de ruta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>El identificador único interno asociado con el proveedor de servicios durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Parámetros de cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>
        El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.
        <br/><br/>
        Si la plataforma del dispositivo de streaming tiene limitaciones para proporcionar un valor, la aplicación tendrá que reanudar la sesión de autenticación y proporcionar un valor válido.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        Dominio de origen de la aplicación que realiza el inicio de sesión de MVPD.
        <br/><br/>
        Si la plataforma del dispositivo de streaming tiene limitaciones para proporcionar un valor, la aplicación tendrá que reanudar la sesión de autenticación y proporcionar un valor válido.
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
        </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorización</td>
      <td>La generación de la carga útil del token de portador se describe en la <a href="../../../dynamic-client-registration-api.md">documentación de registro dinámico de cliente</a>.</td>
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
      <td>La generación de la carga del identificador de dispositivo se describe en la <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">documentación de AP-Device-Identifier</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La generación de la carga de información del dispositivo se describe en la <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">documentación de X-Device-Info</a>.
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
        La generación de la carga de inicio de sesión único para el método de identidad de Platform se describe en la documentación de <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a>.
        <br/><br/>
        Para obtener más información sobre los flujos habilitados para el inicio de sesión único que utilizan una identidad de plataforma, consulte la documentación de <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Inicio de sesión único mediante flujos de identidad de plataforma</a>.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        La generación de la carga de inicio de sesión único para el método del token de servicio se describe en la documentación de <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
        <br/><br/>
        Para obtener más información sobre los flujos habilitados para el inicio de sesión único que utilizan un token de servicio, consulte la documentación de <a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md">Inicio de sesión único mediante flujos de token de servicio</a>.
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">Código</th>
      <th style="background-color: #EFF2F7; width: 20%;">Texto</th>
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
        El token de acceso no es válido, el cliente debe obtener un nuevo token de acceso e intentarlo de nuevo. Para obtener más información, consulte la <a href="../../../dynamic-client-registration-api.md">documentación de registro dinámico de clientes</a>.
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Encabezados</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">Cuerpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         El objeto JSON tiene los atributos siguientes:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Atributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Acción que el dispositivo de flujo continuo debe realizar para completar el flujo de autenticación.
                  <br/><br/>
                  Los valores posibles son:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valor</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">autentificar</td>
                        <td>El dispositivo de streaming u otro dispositivo deben abrir la dirección URL proporcionada en un agente de usuario.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">reanudar</td>
                        <td>El dispositivo de streaming u otro dispositivo debe proporcionar los parámetros que faltan y reanudar la sesión de autenticación con el código.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">autorizar</td>
                        <td>El dispositivo de streaming puede continuar directamente con los flujos de decisiones.</td>
                     </tr>
                  </table>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  El tipo de interacción que debe realizar el dispositivo de flujo continuo para continuar el flujo con la acción especificada por el atributo actionName.
                  <br/><br/>
                  Los valores posibles son:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valor</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">dirigir</td>
                        <td>El flujo continúa con una llamada directa a la URL proporcionada mediante un cliente HTTP disponible para la implementación del cliente.</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">interactivo</td>
                        <td>El flujo continúa con una navegación a la URL proporcionada mediante un agente de usuario.</td>
                     </tr>
                  </table>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>Los parámetros que faltan que deben proporcionarse para completar el flujo de autenticación básico.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>Dirección URL a la que debe navegar la aplicación cliente.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">código</td>
               <td>El código de autenticación que se puede utilizar en una aplicación secundaria para reanudar la sesión de autenticación.</td>
               <td><i>obligatorio</i></td>
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
      <td><i>obligatorio</i></td>
</table>

### Error {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">Cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">error</td>
      <td>El error proporciona información adicional que se adhiere a la documentación de <a href="../../../enhanced-error-codes.md">Códigos de error mejorados</a>.</td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples}

### 1. Cree una sesión de autenticación y proporcione los valores de todos los parámetros necesarios

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "authenticate",
   "actionType" : "interactive",
   "url" : "/v2/authenticate/REF30/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 2. Cree una sesión de autenticación y proporcione valores para ninguno o algunos parámetros necesarios

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "resume",
   "actionType" : "direct",
   "url" : "/v2/REF30/sessions/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "missingParameters" : ["mvpd","domain","redirectUrl"],
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 3. Cree una sesión de autenticación y proporcione un valor para el parámetro mvpd para el que ya existe un perfil autenticado válido

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "profile",
   "actionType" : "direct",
   "url" : "/v2/REF30/profiles/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 4. Cree una sesión de autenticación mientras proporciona valor para el parámetro mvpd y para el que existe una degradación

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/v2/REF30/decisions/authorize",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
