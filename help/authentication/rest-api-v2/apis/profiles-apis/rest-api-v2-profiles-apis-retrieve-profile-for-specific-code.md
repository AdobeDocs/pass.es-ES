---
title: Recuperar perfil para código específico
description: 'API de REST V2: recuperar el perfil de un código específico'
exl-id: d6ead7d5-de5f-4033-8115-980953a370c0
source-git-commit: 6c328eb2c635a1d76fc7dae8148a4de291c126e0
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 1%

---

# Recuperar perfil para código específico {#retrieve-profile-for-specific-code}

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
      <td>/api/v2/{serviceProvider}/profiles/code/{code}</td>
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
      <td style="background-color: #DEEBFF;">código</td>
      <td>El código de autenticación obtenido después de crear la sesión de autenticación en el dispositivo de flujo continuo.</td>
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
        El cuerpo de la respuesta contiene un mapa de perfiles válidos, que pueden estar vacíos.
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
                    <li><b>mvpd (por ejemplo, Spectrum, Cablevision, etc.)</b><br/>El perfil se creó como resultado de: autenticación básica.</li>
                    <li><b>Adobe</b><br/>El perfil se creó como resultado de: acceso degradado, acceso temporal.</li>
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

### 1. Recupere perfiles autenticados existentes y válidos en un dispositivo secundario después de realizar una autenticación básica

>[!BEGINTABS]

>[!TAB Solicitud]

```JSON
GET /api/v2/REF30/profiles/Cablevision/XTC98W
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
```

>[!TAB Respuesta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Cablevision",
            "type" : "regular",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                },
                "householdId" : {
                    "value" : "BASE64_value_householdId",
                    "state" : "plain"
                },
                "zip" : {
                    "value" : "BASE64_value_zip",
                    "state" : "enc"
                },
                "parental-controls" : {
                    "value" : BASE64_value_parental-controls,
                    "state" : "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]
