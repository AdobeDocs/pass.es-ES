---
title: Iniciar el cierre de sesión de un mvpd específico
description: 'API de REST V2: iniciar el cierre de sesión para un mvpd específico'
source-git-commit: 4598aaa0827b943de83a9e7d847227edf6b0b387
workflow-type: tm+mt
source-wordcount: '911'
ht-degree: 1%

---


# Iniciar el cierre de sesión de un mvpd específico {#initiate-logout-for-specific-mvpd}

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
      <td>/api/v2/{serviceProvider}/logout/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>GET</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.</td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Parámetros de consulta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        Dirección URL de redireccionamiento final a la que se desplaza el agente de usuario cuando finaliza el flujo de cierre de sesión de la MVPD.
        <br/><br/>
        El valor debe tener codificación URL.
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
        El cuerpo de respuesta contiene una lista de acciones que el cliente debe realizar para completar el flujo de cierre de sesión de cada perfil eliminado.
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
      <td style="background-color: #DEEBFF;">cierres de sesión</td>
      <td>
         JSON que contiene un mapa de pares de clave y valor.
         <br/><br/>
         El elemento clave se define con el siguiente valor:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Valor</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.</td>
               <td><i>obligatorio</i></td>
         </table>
         El elemento value se define mediante los atributos siguientes:
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">Atributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  Acción que el dispositivo de flujo continuo debe realizar para completar el flujo de cierre de sesión.
                  <br/><br/>
                  Los valores posibles son:
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">Valor</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">cierre de sesión</td>
                        <td>
                            El dispositivo de streaming debe abrir la dirección URL proporcionada en un agente de usuario.
                            <br/><br/>
                            Esta acción se aplica a los siguientes escenarios:
                            <ul>
                                <li>Cerrar sesión de MVPD con un punto final de cierre de sesión</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">complete</td>
                        <td>
                            El dispositivo de streaming no necesita realizar ninguna acción posterior.
                            <br/><br/>
                            Esta acción se aplica a los siguientes escenarios:
                            <ul>\
                                <li>Cerrar sesión de MVPD sin un punto final de cierre de sesión (función de cierre de sesión ficticio)</li>
                                <li>Cerrar sesión durante el acceso degradado</li>
                                <li>Cerrar sesión durante el acceso temporal</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">inválido</td>
                        <td>
                            El dispositivo de streaming no necesita realizar ninguna acción posterior.
                            <br/><br/>
                            Esta acción se aplica a los siguientes escenarios:
                            <ul>
                                <li>Cierre la sesión de MVPD cuando no se encuentre ningún perfil válido</li>
                            </ul>
                        </td>
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
                        <td style="background-color: #DEEBFF;">interactivo</td>
                        <td>
                            Este tipo se aplica a los siguientes valores del atributo actionName:
                            <ul>
                                <li>cierre de sesión</li>
                            </ul>
                        </td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">ninguno</td>
                        <td>
                            Este tipo se aplica a los siguientes valores del atributo actionName:
                            <ul>
                                <li>complete</li>
                                <li>inválido</li>
                            </ul>
                        </td>
                     </tr>
                  </table>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>El identificador único interno asociado con el proveedor de identidad durante el proceso de incorporación.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>
                  Dirección URL utilizada para realizar el flujo de cierre de sesión con el extremo de MVPD.
                  <br/><br/>
                  Esto no está presente para los siguientes valores del atributo actionName:
                  <ul>
                    <li>complete</li>
                    <li>inválido</li>
                  </ul>
               </td>
               <td>opcional</td>
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

### 1. Iniciar el cierre de sesión básico para mvpd que admita el flujo de cierre de sesión

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
GET /api/v2/logout/REF30/Cablevision?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)  
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "Cablevision" : {
            "actionName" : "logout",
            "actionType" : "interactive",
            "mvpd" : "Cablevision",
            "url": "https://sp.auth.adobe.com/adobe-services/logout?noflash=true&mso_id=Cablevision&requestor_id=REF30&redirect_url=http%3A%2F%2Fadobe.com"
        }
    }
}
```

>[!ENDTABS]

### 2. Iniciar el cierre de sesión básico para mvpd que no admite el flujo de cierre de sesión

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
GET /api/v2/logout/REF30/Dish?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "Dish" : {
            "actionName" : "complete",
            "actionType" : "none",
            "mvpd" : "Dish"
       }
    }
}
```

>[!ENDTABS]

### 3. Iniciar el cierre de sesión, incluidos los perfiles autenticados obtenidos mediante el inicio de sesión único mediante el método de Platform Identity

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
GET /api/v2/logout/REF30/Comcast_SSO?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Adobe-Subject-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJObUZtWmpjek5XVXROVFJoWWkwME5ERmlMV0V6Wm .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
   "logouts": {
      "Comcast_SSO": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Comcast_SSO",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Comcast_SSO&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### 4. Iniciar el cierre de sesión, incluidos los perfiles autenticados obtenidos mediante el inicio de sesión único mediante el método de token de servicio

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
GET /api/v2/logout/REF30/Spectrum?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info: .....
AD-Service-Token: eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJZemsxTXpNMk4yWXRZMk0wTWkwME1X .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8  
 
{
   "logouts": {
      "Spectrum": {
         "actionName": "logout",
         "actionType": "interactive",
         "mvpd": "Spectrum",
         "url": "/adobe-services/logout?requestor_id=REF30&mso_id=Spectrum&pt_device_id=c54fa2c80652b10abea58c...."
      }
   }
}
```

>[!ENDTABS]

### 5. Iniciar el cierre de sesión para el pase temporal (promocional)

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
GET /api/v2/logout/REF30/TempPass_5mins?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "TempPass_5mins" : {
            "actionName" : "invalid",
            "actionType" : "none",
            "mvpd" : "TempPass_5mins"
        }
    }
}
```

>[!ENDTABS]

### 6. Iniciar el cierre de sesión de mvpd degradado

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
GET /api/v2/logout/REF30/ATTOTT?redirectUrl=https%3A%2F%2Fadobe.com
 
Authorization: Bearer: ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01....
X-Device-Info .....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "logouts" : {
        "ATTOTT" : {
            "actionName" : "invalid",
            "actionType" : "none",
            "mvpd" : "ATTOTT",
        }
    }
}
```

>[!ENDTABS]
