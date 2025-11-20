---
title: Gestión de Errores de Conflicto 409
description: Aprenda a gestionar errores de conflicto 409 cuando se alcancen los límites de uso simultáneos
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---


# Gestión de Errores de Conflicto 409 {#handling-409-errors}

Cuando un usuario intenta iniciar un nuevo flujo y alcanza un límite de uso simultáneo, la Monitorización de concurrencia devuelve una respuesta de **409 Conflict**. Comprender cómo gestionar este error es crucial para proporcionar una buena experiencia de usuario.

## ¿Qué es un conflicto 409? {#what-is-a-409-conflict}

Se produce un conflicto 409 cuando:

1. **El usuario intenta iniciar un nuevo flujo**
2. **La evaluación de la directiva determina** que la solicitud excedería los límites
3. **El sistema devuelve 409** con información detallada sobre conflictos
4. **La aplicación debe decidir** cómo manejar el conflicto

### Estructura de respuesta 409

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
        "id": "advice-1",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1",
                "contentType": "live"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Explicación de la respuesta {#understanding-the-response}

### Campos clave

- **`decision`**: siempre &quot;DENEGAR&quot; para los conflictos de tipo 409
- **`associatedAdvice`**: matriz de objetos de asesoramiento que explica la infracción
- **`conflicts`**: lista de sesiones activas que están causando el conflicto
- **`terminationCode`**: código único para finalizar una sesión específica

### Tipos de consejos

#### Consejo de infracción de regla

```json
{
  "adviceType": "rule-violation",
  "attributes": {
    "rule": "max_streams",
    "threshold": 3,
    "current": 4,
    "conflicts": [...]
  }
}
```

#### Aviso de terminación remota

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## Estrategias de manejo {#handling-strategies}

- En el modo FIFO, CM permite que la nueva sesión comience terminando una existente.
- En el modo LIFO, CM bloquea la nueva sesión e informa al usuario


## Prácticas recomendadas {#best-practices}

### &#x200B;1. Borrar comunicación del usuario

- **Explicar el límite**: los usuarios deben comprender por qué están bloqueados
- **Mostrar opciones disponibles**: qué pueden hacer para resolver el conflicto
- **Proporcionar contexto** - Mostrar qué sesiones están activas

### &#x200B;2. Gestión correcta de errores

- **No se bloquee** - Controlar correctamente los errores 409
- **Proporcionar alternativas**: ofrezca formas de resolver el conflicto
- **Guardar estado de usuario** - No pierda su selección de contenido

### &#x200B;3. Consideraciones sobre la experiencia del usuario

- **Resolución rápida**: facilite la resolución de conflictos
- **Borrar opciones**: los usuarios deben entender sus opciones
- **Comportamiento coherente** - Controlar conflictos de la misma manera cada vez

### &#x200B;4. Consideraciones técnicas

- **Analizar la respuesta con cuidado**: extraiga toda la información relevante
- **Controlar casos extremos**. ¿Qué sucede si no se devuelven conflictos?
- **Conflictos de registro**: haga un seguimiento de las infracciones de directivas para su análisis


