---
title: Conceptos clave
description: Conozca los conceptos fundamentales de la Monitorización de concurrencia, incluidas las sesiones, las políticas, los metadatos y mucho más
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# Conceptos clave {#key-concepts}

Comprender los conceptos básicos de la monitorización de la concurrencia es esencial para una implementación exitosa. Esta guía explica los componentes básicos fundamentales y cómo funcionan juntos.

## Conceptos principales {#core-concepts}

### Sesiones

Una **sesión** representa una instancia de flujo de vídeo activa. Considérelo como un &quot;ticket&quot; que permite a un usuario ver contenido.

#### Ciclo de vida de sesión

1. **Creación** - Cuando el usuario empieza a ver contenido
2. **Activo** - Mientras el usuario está mirando (mantenido por latidos)
3. **Finalización** - Cuando el usuario deja de ver o la sesión caduca

#### Propiedades de sesión

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### Políticas

**Políticas** son reglas comerciales que definen límites y restricciones para el streaming simultáneo. Determinan cuándo los usuarios pueden iniciar nuevas transmisiones.

#### Componentes de política

- **Reglas**: límites y condiciones específicos
- **Requisitos de metadatos** - Qué datos se necesitan
- **Lógica de evaluación** - Cómo se aplica la directiva

#### Política de ejemplo

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### Metadatos

**Metadatos** proporciona contexto sobre cada sesión. Incluye información sobre el dispositivo, el contenido, el usuario y la aplicación.

#### Categorías de metadatos

| Categoría | Ejemplos | Finalidad |
|-----------------|------------------------------------------|---------------------------------|
| **Dispositivo** | `deviceId`, `deviceType`, `osName` | Identificar y categorizar dispositivos |
| **Contenido** | `channel`, `contentType`, `assetId` | Describa lo que se está viendo |
| **Usuario** | `subject`, `subscriptionTier` | Información específica del usuario |
| **Aplicación** | `applicationName`, `applicationPlatform` | Detalles específicos de la aplicación |
| **Ubicación** | `country`, `hba` | Información geográfica y de red |

#### Metadatos obligatorios frente a opcionales

- **Metadatos requeridos** - Deben proporcionarse para la creación de la sesión
- **Metadatos opcionales**: se pueden proporcionar para directivas mejoradas
- **Metadatos estándar**: campos predefinidos disponibles para todas las aplicaciones
- **Metadatos personalizados** - Campos específicos de la aplicación que defina

## Identidad y autenticación {#identity-and-authentication}

### Proveedor de identidad (IdP)

Un **proveedor de identidad** es el servicio que autentica a los usuarios y proporciona su información de identidad. En la integración con Adobe Pass, suele ser un MVPD.

#### Ejemplos de IdP

- Compañías de cable (Comcast, Charter, etc.)
- Proveedores de satélite (DirecTV, Dish)
- Servicios de streaming (Netflix, Hulu)
- Operadores de telefonía móvil (Verizon, AT&amp;T)

### Asunto

Un **asunto** es el identificador único de un usuario dentro de un proveedor de identidad. Normalmente, es el ID de cuenta o el ID de suscriptor del usuario.

## Administración de sesión {#session-management}

### Latidos

**Heartbeats** son llamadas periódicas API que mantienen las sesiones activas. Le dicen al sistema &quot;este usuario sigue mirando&quot;.

#### Requisitos de Heartbeat

- **Frecuencia** - Cada 60 segundos (recomendado)
- **Propósito**: mantener la sesión activa, actualizar metadatos
- **Finalización** - La sesión caduca si se detienen los latidos


### Finalización de sesión

Las sesiones se pueden finalizar de varias formas:

1. **El usuario deja de ver** - La aplicación llama al extremo de DELETE
2. **La sesión caduca** - No se recibió ningún latido en el tiempo de espera
3. **Infracción de directiva** - La nueva sesión finaliza la anterior (FIFO)
4. **Terminación remota**: otro dispositivo termina la sesión

## Evaluación de políticas {#policy-evaluation}

### Cómo funcionan las políticas

1. **El usuario solicita un nuevo flujo**
2. **El sistema evalúa las directivas** con respecto al uso actual
3. **Decisión tomada** - Permitir o denegar
4. **Respuesta enviada** - Información de éxito o de conflicto

## Resolución de conflictos {#conflict-resolution}

### ¿Qué es un conflicto?

Se produce un **conflicto** cuando un usuario intenta iniciar un nuevo flujo, pero sobrepasaría sus límites.

### Respuesta al conflicto

Cuando se produce un conflicto, el sistema devuelve una respuesta de conflicto 409 con información detallada:

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### Estrategias de resolución

#### LIFO (Última entrada, primera salida)

- **Las sesiones antiguas están protegidas**
- **Se ha bloqueado la nueva sesión**
- **IU más sencilla**
- **Mejor para una visualización prolongada**


#### FIFO (Primero en entrar, primero en salir)

- **La nueva sesión finaliza una existente**
- **El usuario elige qué sesión detener**
- **Se requiere una IU más compleja**
- **Mejor para cambiar de contenido**

## Solicitudes e inquilinos {#applications-and-tenants}

### Aplicación

Una **aplicación** es un programa de software que se integra con la Monitorización de concurrencia. Cada aplicación tiene un ID único y puede tener sus propias políticas.

#### Ejemplos de aplicaciones

- Aplicación móvil (iOS/Android)
- aplicación web
- Aplicación Smart TV
- Aplicación de decodificador

### Inquilino

Un **inquilino** es una organización que posee una o más aplicaciones. Los inquilinos pueden compartir directivas entre aplicaciones.

#### Estructura del inquilino

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## Entornos {#environments}

### Entorno de ensayo

- **Objetivo**: desarrollo y pruebas
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **Credenciales** - Identificadores de aplicación de prueba
- **Datos** - Solo datos de prueba

### Entorno de producción

- **Propósito**: tráfico de usuarios activo
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **Credenciales** - Id. de aplicación de producción
- **Datos** - Datos reales del usuario

## Flujo de integración {#integration-flow}

### Flujo básico

1. **Autenticación de usuario** - Autenticar con Adobe Pass
2. **Creación de sesión** - Crear sesión de CM con metadatos de usuario
3. **Administración de latidos** - Enviar latidos periódicos
4. **Finalización de sesión** - Finaliza cuando el usuario deja de ver

### Control de errores

1. **404 no encontrado** - Administrar solicitudes con ID de sesión no generado por el servicio CM
2. **409 conflictos** - Controlar violaciones de directivas
3. **410 se ha ido** - Controlar la finalización de la sesión
4. **Errores de red** - Controlar problemas de conectividad
5. **Errores de autenticación** - Controlar problemas de credenciales


## Terminología común {#common-terminology}

| Término | Definición |
|-----------------|--------------------------------------------|
| **Sesión** | Instancia de flujo de vídeo activo |
| **Directiva** | Regla empresarial que define los límites de uso |
| **Metadatos** | Información contextual sobre sesiones |
| **IDP** | Proveedor de identidad (servicio de autenticación) |
| **Asunto** | Identificador de usuario dentro de un IDP |
| **Latido** | Llamada periódica para mantener la sesión activa |
| **Conflicto** | Infracción de directiva al iniciar un nuevo flujo |
| **LIFO** | Última entrada, Primera salida resolución de conflictos |
| **FIFO** | Resolución de conflictos de primera entrada y primera salida |
| **inquilino** | Aplicaciones propietarias de organización |
| **Aplicación** | Programa de software que utiliza CM |

