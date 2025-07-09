---
title: Recuperar token de acceso
description: 'API de registro de cliente dinámico: recuperar el token de acceso'
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: ebe0a53e3ba54c2effdef45c1143deea0e6e57d3
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 1%

---

# Recuperar token de acceso {#retrieve-access-token}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> La implementación de la API de registro de cliente dinámico está limitada por la documentación de [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Solicitud {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">ruta</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>PUBLICAR</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parámetros de cuerpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            La cadena del identificador de la aplicación cliente.
            <br/><br/>
            Para obtener más información sobre cómo obtener la cadena de identificador de cliente, consulte la documentación de la API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperar credenciales de cliente</a>.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secret</td>
      <td>
            La cadena secreta de la aplicación cliente.
            <br/><br/>
            Para obtener más información sobre cómo obtener la cadena secreta del cliente, consulte la documentación de la API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperar credenciales del cliente</a>.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            Cadena de tipo de concesión (por ejemplo, "client_credentials") que la aplicación cliente puede utilizar para el extremo del token de cliente.
            <br/><br/>
            Para obtener más información sobre cómo obtener la cadena de tipo de concesión, consulte la documentación de la API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperar credenciales de cliente</a>.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Encabezados</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         La generación de la carga de información del dispositivo se describe en la <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">documentación de X-Device-Info</a>.
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

### Correcto {#success}

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
               <td style="background-color: #DEEBFF;">id</td>
               <td>Identificador opaco que se puede utilizar para rastrear la actividad del usuario.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>Valor del token de acceso que la aplicación cliente debe utilizar para el encabezado Autorización.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>Tiempo en milisegundos durante el cual se emitió el token de acceso.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>Tiempo en segundos hasta que caduca el token de acceso.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>El tipo de token (por ejemplo, "portador").</td>
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
      <td>400</td>
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
      <td>
        Los valores posibles son:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valor</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    La solicitud no es válida debido a uno de los siguientes motivos:
                    <ul>
                        <li>La solicitud omite un parámetro requerido.</li>
                        <li>La solicitud incluye un valor de parámetro no admitido (que no sea de tipo de concesión).</li>
                        <li>La solicitud repite un parámetro.</li>
                        <li>La solicitud incluye varias credenciales.</li>
                        <li>La solicitud utiliza más de un mecanismo para autenticar al cliente.</li>
                        <li>La solicitud tiene un formato incorrecto.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>Las credenciales del cliente no son válidas, el cliente debe obtener nuevas credenciales de cliente e intentarlo de nuevo. Para obtener más información, consulte la documentación de la API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperar credenciales del cliente</a>.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>El tipo de concesión utilizado no es válido.</td>
            </tr>
         </table>
      </td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples}

### Recuperar token de acceso {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB Respuesta - Correcta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1723227212,
  "expires_in": 86400,
  "token_type": "bearer"
}
```

>[!TAB Respuesta: error]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
