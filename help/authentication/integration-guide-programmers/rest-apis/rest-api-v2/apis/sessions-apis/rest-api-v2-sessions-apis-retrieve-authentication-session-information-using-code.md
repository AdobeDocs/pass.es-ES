---
title: Recuperar sesión de autenticación mediante código
description: 'API REST V2: Recuperar sesión de autenticación mediante código'
exl-id: 5cc209eb-ee6b-4bb9-9c04-3444408844b7
source-git-commit: 92d2befd154b21abf743075c78ad617cff79b7e9
workflow-type: tm+mt
source-wordcount: '529'
ht-degree: 2%

---

# Recuperar sesión de autenticación mediante código {#retrieve-authentication-session-using-code}

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
      <td>/api/v2/{serviceProvider}/session/{code}</td>
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
      <td style="background-color: #DEEBFF;">AP-Visitor-Identifier</td>
      <td>
        La generación de la carga del identificador de visitante se describe en la documentación del encabezado <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md">AP-Visitor-Identifier</a>.
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
        El cuerpo de respuesta contiene información sobre la sesión de autenticación.
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
               <td style="background-color: #DEEBFF;">existingParameters</td>
               <td>Los parámetros existentes que ya se proporcionaron.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>Los parámetros que faltan que deben proporcionarse para completar el flujo de autenticación.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">dispositivo</td>
               <td>La información del dispositivo relacionada con el dispositivo de flujo continuo real.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>La marca de tiempo en milisegundos antes de la cual el código de autenticación no es válido.</td>
               <td><i>obligatorio</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>La marca de tiempo en milisegundos tras la cual el código de autenticación no es válido.</td>
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
      <td>
            El cuerpo de respuesta puede proporcionar información de error adicional que se adhiera a la documentación de <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">Códigos de error mejorados</a>.
            <br/><br/>
            La aplicación cliente debe implementar un mecanismo de gestión de errores capaz de procesar correctamente los códigos de error devueltos con más frecuencia por esta API:
            <ul>
                <li>invalid_authentication_session</li>
                <li>invalid_parameter_code</li>
                <li>etc.</li>
            </ul>
            La lista anterior no es exhaustiva. La aplicación cliente debe poder administrar todos los códigos de error mejorados definidos en la <a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">documentación pública</a>.
      </td>
      <td><i>obligatorio</i></td>
   </tr>
</table>

## Muestras {#samples}

### &#x200B;1. Recupere la sesión de autenticación sin parámetros

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/v2/sessions/REF30/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{        
    "existingParameters": {
        "mvpd": "apassidp",
        "domain": "adobe.com"
        "redirectUrl": "https://www.adobe.com",
        "serviceProvider": "REF30"        
    },
    "device": {
        "type": "Desktop",
        "model": null,
        "version": {
            "major": 0,
            "minor": 0,
            "patch": 0,
            "profile": ""
        },
    "hardware": {
      "name": null,
      "vendor": "Apple",
      "version": {
        "major": 0,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "manufacturer": "Apple"
    },
    "operatingSystem": {
      "name": "macOS",
      "family": "macOS",
      "vendor": "Apple",
      "version": {
        "major": 10,
        "minor": 15,
        "patch": 7,
        "profile": ""
      }
    },
    "browser": {
      "name": "Chrome",
      "vendor": "Google",
      "version": {
        "major": 140,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36",
      "originalUserAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36"
    },
    "display": {
      "width": 0,
      "height": 0,
      "ppi": 0,
      "name": "DISPLAY",
      "vendor": null,
      "version": null,
      "diagonalSize": null
    },
    "applicationId": null,
    "connection": {
      "ipAddress": "...",
      "port": "55161",
      "secure": false,
      "type": null
    }
    }
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"    
}
```

>[!ENDTABS]

### &#x200B;1. Recupere la sesión de autenticación sin parámetros

>[!BEGINTABS]

>[!TAB Solicitud]

```HTTPS
GET /api/v2/sessions/REF30/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB Respuesta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
  "missingParameters": [
    "mvpd"
  ],
  "existingParameters": {
    "redirectUrl": "https://adobe.com",
    "domainName": "adobe.com",
    "serviceProvider": "REF30"
  },
  "device": {
    "type": "Desktop",
    "model": null,
    "version": {
      "major": 0,
      "minor": 0,
      "patch": 0,
      "profile": ""
    },
    "hardware": {
      "name": null,
      "vendor": "Apple",
      "version": {
        "major": 0,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "manufacturer": "Apple"
    },
    "operatingSystem": {
      "name": "macOS",
      "family": "macOS",
      "vendor": "Apple",
      "version": {
        "major": 10,
        "minor": 15,
        "patch": 7,
        "profile": ""
      }
    },
    "browser": {
      "name": "Chrome",
      "vendor": "Google",
      "version": {
        "major": 140,
        "minor": 0,
        "patch": 0,
        "profile": ""
      },
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36",
      "originalUserAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36"
    },
    "display": {
      "width": 0,
      "height": 0,
      "ppi": 0,
      "name": "DISPLAY",
      "vendor": null,
      "version": null,
      "diagonalSize": null
    },
    "applicationId": null,
    "connection": {
      "ipAddress": "...",
      "port": "3061",
      "secure": false,
      "type": null
    }
  },
  "notBefore": "1761299929958",
  "notAfter": "1761301729958"
}
```

>[!ENDTABS]
