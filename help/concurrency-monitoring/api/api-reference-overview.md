---
title: Información general de referencia de API
description: Referencia completa para la API de supervisión de concurrencia, incluidos los puntos de conexión, la autenticación y los formatos de respuesta
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# Información general de referencia de API {#api-reference-overview}

La API de supervisión de concurrencia proporciona una interfaz RESTful para administrar sesiones de flujo continuo y aplicar políticas de uso simultáneo. Esta referencia proporciona documentación completa para todos los extremos, métodos de autenticación, formatos de solicitud/respuesta y administración de errores.

## URL base de API

### Entorno de producción

```
https://streams.adobeprimetime.com/v2/
```

### Entorno de ensayo

```
https://streams-stage.adobeprimetime.com/v2/
```

**Nota:** Use siempre el entorno de ensayo para el desarrollo y las pruebas. Las credenciales de producción solo se proporcionan después de una integración de ensayo correcta.

## Autenticación

Todas las llamadas a la API requieren la autenticación HTTP Basic con las credenciales de la aplicación:

- **Nombre de usuario:** Su ID de aplicación (proporcionado por Adobe)
- **Contraseña:** Cadena vacía

### Ejemplo de encabezado de autenticación

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## Estándares de formato de respuesta

### Respuestas de éxito

Todas las respuestas correctas siguen esta estructura:

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Respuestas de error

Todas las respuestas de error siguen esta estructura:

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### Formato de resultado de evaluación

Cuando se evalúan las políticas (especialmente en el caso de conflictos de tipo 409), las respuestas incluyen un resultado de evaluación:

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Códigos de estado HTTP comunes

| Código | Descripción | Al Devolver |
|------|----------------------|------------------------------------------------|
| 200 | OK | Solicitudes GET correctas |
| 202 | Creado/aceptado | Creación de sesión/latido correctos registrados |
| 400 | Solicitud incorrecta | Faltan campos obligatorios o parámetros no válidos |
| 401 | No autorizado | Falta la autenticación o no es válida |
| 403 | Prohibido | Permisos insuficientes |
| 404 | ID de sesión no encontrado | El servicio CM no ha generado el ID de la sesión |
| 409 | Conflicto | Infracción de directiva (límite simultáneo alcanzado) |
| 410 | Gone | La sesión ha caducado o ha finalizado |
| 429 | Demasiadas solicitudes | Límite de velocidad excedido |

## Métodos de paso de parámetros

### Parámetros de ruta

Parámetros necesarios que forman parte de la ruta de la URL:

1. `{idp}` - Identificador del proveedor de identidad
2. `{subject}` - Identificador de usuario (generalmente de Adobe Pass)
3. `{sessionId}` - Identificador de sesión (devuelto en el encabezado Ubicación)

### Parámetros adicionales

Los parámetros opcionales se pasan en la dirección URL:

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### Datos de formulario (POST/PUT)

Metadatos y datos de sesión en el cuerpo de la solicitud:

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### Encabezados

Parámetros especiales pasados en encabezados HTTP:

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## Prácticas recomendadas de administración de errores

### 409 Gestión de conflictos

Cuando recibe una respuesta 409 Conflict:

1. **Analice el resultado de la evaluación** para comprender la infracción de directiva
2. **Extraer información de conflicto** de `associatedAdvice`
3. **Presentar opciones al usuario** según su estrategia LIFO/FIFO
4. **Usar códigos de terminación** si se implementa el comportamiento LIFO

### 410 Gestión desaparecida

Cuando recibe una respuesta 410 Gone:

1. **Comprobar si la respuesta tiene un cuerpo** - indica una finalización remota
2. **Analizar aviso** para comprender por qué se terminó la sesión
3. **Actualizar la interfaz de usuario** para reflejar la finalización de la sesión
4. **Controlar correctamente**: es posible que la sesión haya expirado de forma natural
5. **Iniciar una nueva sesión**: si lo considera apropiado, inicie una nueva sesión

### Limitación de velocidad

Cuando recibe una 429 Too Many Requests:

1. **Limitar la frecuencia de llamada** hasta 200 solicitudes por minuto, que es el nivel máximo aceptado por CM
2. **Envíe latidos en el intervalo requerido**, que es uno por minuto.

## Herramientas de prueba

### Explorador de API interactivo

Use nuestra [interfaz de usuario de Swagger](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) para las pruebas interactivas:

1. Introduzca su ID de aplicación en la esquina superior derecha
2. Haga clic en &quot;Explorar&quot; para establecer la autenticación
3. Puntos finales de prueba con parámetros reales
4. Ver ejemplos de solicitud/respuesta
