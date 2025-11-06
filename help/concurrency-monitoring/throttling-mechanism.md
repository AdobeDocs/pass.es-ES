---
title: Mecanismo de limitación
description: Mecanismo de limitación
exl-id: 15236570-1a75-42fb-9bba-0e2d7a59c9f6
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 1%

---

# Mecanismo de limitación {#throttling-mechanism}

## Introducción {#introduction}

Adobe, como responsable del tratamiento de sus datos, debe tomar las medidas adecuadas para garantizar que los usuarios de nuestros clientes utilicen los recursos de forma equitativa y que el servicio no se vea inundado de solicitudes de API innecesarias. Para ello hemos puesto en marcha un mecanismo de limitación.
Una aplicación de Monitorización de concurrencia puede ser utilizada por varios usuarios y un usuario puede tener varias sesiones. Por lo tanto, el servicio tendrá límites configurados para el número de llamadas aceptadas por usuario/sesión dentro de un intervalo de tiempo específico.
Cuando se alcanza el límite, las solicitudes se marcan con un estado de respuesta específico (HTTP 429 Too Many Requests). Cualquier llamada posterior realizada después de recibir una respuesta de &quot;429 demasiadas solicitudes&quot; debe realizarse con un período de enfriamiento de al menos un minuto para garantizar que obtenga una respuesta comercial válida.

## Resumen del mecanismo {#mechanism-overview}

El mecanismo determina el número máximo de llamadas aceptadas para cada extremo de Monitorización de concurrencia dentro de un intervalo de tiempo específico.
Una vez alcanzado este número máximo de llamadas, nuestro servicio responderá con &#39;429 Demasiadas solicitudes&#39;. El encabezado &quot;Caduca&quot; de la respuesta 429 incluye la marca de tiempo cuando la siguiente llamada se consideraría válida o cuando caduca el acelerador. En este momento, la restricción caduca después de una   minuto desde la primera respuesta 429.

Los extremos configurados con restricción son los siguientes:
1. Crear una nueva sesión: POST /session/{idp}/{subject}
2. Llamada de Heartbeat: POST /session/{idp}/{subject}/{sessionId}
3. Finalizar una sesión: DELETE /session/{idp}/{subject}/{sessionId}

La restricción se configura en dos niveles:
1. sesión: se envió el mismo parámetro {sessionId} único en la llamada `Heartbeat` y en la llamada `Terminate a session`.
2. usuario: se envió el mismo parámetro {subject} único en la llamada a `Create a new session`.

El límite de restricción de nivel de sesión se establece en 200 solicitudes en un minuto.\
El límite de restricción de nivel de usuario se establece en 200 solicitudes en un minuto.\
Ambos límites (limitación de nivel de sesión y de nivel de usuario) se pueden configurar y se actualizarán en caso de que se alcancen a través de escenarios de integración válidos. Para ello, recomendamos que se contacte con el equipo de asistencia.

**Escenario para la restricción de nivel de sesión:**

| Hora | Solicitar envío a CM | Número de solicitudes | Respuesta recibida de CM | Explicación |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 segundos | POST/session/idp1/subject1/session1 | 50 | Todas las llamadas reciben &quot;202 Accepted&quot; | 50 llamadas consumidas desde el límite |
| Segundos 50 | POST/session/idp1/subject1/session1 | 151 | 150 llamadas reciben &quot;202 aceptadas&quot; y 1 llamada recibe &quot;429 demasiadas solicitudes&quot; | 200 llamadas consumidas desde el límite y 1 llamada recibirá 429 respuestas |
| Segundo 61 | DELETE /session/idp1/subject1/session1 | 1 | 1 llamada recibe &quot;429 Too many requests&quot; (Demasiadas solicitudes) | Todavía no hay llamadas dentro del límite disponible |
| Second 70 | DELETE /session/idp1/subject1/session1 | 1 | 1 llamada recibe &quot;202 Accepted&quot; | Se ha establecido el límite de 200 llamadas disponibles porque han pasado 60 segundos desde las 10 segundas |

**Escenario para la restricción de nivel de usuario:**

| Hora | Solicitar envío a CM | Número de solicitudes | Respuesta recibida de CM | Explicación |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 segundos | POST/session/idp1/subject1 | 50 | 50 llamadas reciben &quot;202 aceptadas&quot; | 50 llamadas consumidas desde el límite |
| Segundos 50 | POST/session/idp1/subject1 | 151 | 150 llamadas reciben &quot;202 aceptadas&quot; y 1 llamada recibe &quot;429 demasiadas solicitudes&quot; | 200 llamadas consumidas desde el límite y 1 llamada recibirá 429 respuestas |
| Segundo 61 | POST/session/idp1/subject1 | 1 | 1 llamada recibe &quot;429 Too many requests&quot; (Demasiadas solicitudes) | Todavía no hay llamadas dentro del límite disponible |
| Second 70 | POST/session/idp1/subject1 | 1 | 1 llamada recibe &quot;202 Accepted&quot; | Se ha establecido el límite de 200 llamadas disponibles porque han pasado 60 segundos desde las 10 segundas |

**Ejemplo de respuesta 429:**

```
HTTP/2 429
date: Thu, 15 Feb 2024 07:54:20 GMT
content-length: 0
vary: Origin
vary: Access-Control-Request-Method
vary: Access-Control-Request-Headers
cache-control: no-store
expires: Thu, 15 Feb 2024 07:54:41 GMT
strict-transport-security: max-age=31536000 ; includeSubDomains
x-xss-protection: 1; mode=block
x-frame-options: DENY
x-content-type-options: nosniff
```

## Recomendaciones de integración de clientes {#customer-integration-recommendations}

Con una implementación correcta, los clientes no recibirán la respuesta &quot;429 demasiadas solicitudes&quot;.
Aun así, Adobe recomienda que cada cliente gestione adecuadamente la respuesta &quot;429 demasiadas solicitudes&quot; utilizando los detalles técnicos presentados anteriormente. Al gestionar la respuesta, se debe utilizar el encabezado &quot;Caduca&quot; para determinar cuándo enviar la siguiente solicitud válida.
