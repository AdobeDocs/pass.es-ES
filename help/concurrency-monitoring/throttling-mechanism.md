---
title: Mecanismo de limitación
description: Mecanismo de limitación
source-git-commit: 1390c09d3de6b4608cdcc97b3f053cce71e84639
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 1%

---


# Mecanismo de limitación {#throttling-mechanism}

## Introducción {#introduction}

El Adobe, en su función de encargado del tratamiento de datos, debe tomar las medidas adecuadas para garantizar que los usuarios de nuestros clientes utilicen los recursos de forma equitativa y que el servicio no se vea inundado de solicitudes de API innecesarias. Para ello hemos puesto en marcha un mecanismo de limitación.\
Una aplicación de Monitorización de concurrencia puede ser utilizada por varios usuarios y un usuario puede tener varias sesiones. Por lo tanto, el servicio tendrá límites configurados para el número de llamadas aceptadas por usuario/sesión dentro de un intervalo de tiempo específico.\
Cuando se alcanza el límite, las solicitudes se marcan con un estado de respuesta específico (HTTP 429 Too Many Requests). Cualquier llamada posterior realizada después de recibir una respuesta de &quot;429 demasiadas solicitudes&quot; debe realizarse con un período de reutilización de al menos 1 minuto para garantizar que obtenga una respuesta empresarial válida.

## Resumen del mecanismo {#mechanism-overview}

El mecanismo determina el número máximo de llamadas aceptadas para cada extremo de Monitorización de concurrencia dentro de un intervalo de tiempo específico.
Una vez alcanzado este número máximo de llamadas, nuestro servicio responderá con &#39;429 Demasiadas solicitudes&#39;. El servicio necesita otros 60 segundos para volver a inicializar el límite con su valor máximo.

Los extremos configurados con restricción son los siguientes:
1. Cree una nueva sesión: POST /session/{idp}/{subject}
2. Llamada de Heartbeat: POST /session/{idp}/{subject}/{sessionId}
3. Finalizar una sesión: DELETE /session/{idp}/{subject}/{sessionId}

La restricción se configura en dos niveles:
1. session: same unique {sessionId} parámetro enviado `Heartbeat` llame a y `Terminate a session` llamada.
2. usuario: mismo único {subject} parámetro enviado `Create a new session` llamada.

El límite de restricción de nivel de sesión se establece en 200 solicitudes en un minuto.\
El límite de restricción de nivel de usuario se establece en 200 solicitudes en un minuto.\
Ambos límites (limitación de nivel de sesión y de nivel de usuario) se pueden configurar y se actualizarán en caso de que se alcancen a través de escenarios de integración válidos. Para ello, recomendamos que se contacte con el equipo de asistencia.

Este es un escenario para la restricción de nivel de sesión:

| Hora | Solicitar envío a CM | Número de solicitudes | Respuesta recibida de CM | Explicación |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 segundos | POST /session/idp1/subject1/session1 | 50 | Todas las llamadas reciben &quot;202 Accepted&quot; | 50 llamadas consumidas desde el límite |
| Segundos 50 | POST /session/idp1/subject1/session1 | 151 | 150 llamadas reciben &quot;202 aceptadas&quot; y 1 llamada recibe &quot;429 demasiadas solicitudes&quot; | 200 llamadas consumidas desde el límite y 1 llamada recibirá 429 respuestas |
| Segundo 61 | DELETE /session/idp1/subject1/session1 | 1 | 1 llamada recibe &quot;429 Too many requests&quot; (Demasiadas solicitudes) | Todavía no hay llamadas dentro del límite disponible |
| Second 70 | DELETE /session/idp1/subject1/session1 | 1 | 1 llamada recibe &quot;202 Accepted&quot; | Se ha establecido el límite de 200 llamadas disponibles porque han pasado 60 segundos desde las 10 segundas |

y un escenario para la restricción de nivel de usuario:

| Hora | Solicitar envío a CM | Número de solicitudes | Respuesta recibida de CM | Explicación |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 segundos | POST /session/idp1/subject1 | 50 | 50 llamadas reciben &quot;202 aceptadas&quot; | 50 llamadas consumidas desde el límite |
| Segundos 50 | POST /session/idp1/subject1 | 151 | 150 llamadas reciben &quot;202 aceptadas&quot; y 1 llamada recibe &quot;429 demasiadas solicitudes&quot; | 200 llamadas consumidas desde el límite y 1 llamada recibirá 429 respuestas |
| Segundo 61 | POST /session/idp1/subject1 | 1 | 1 llamada recibe &quot;429 Too many requests&quot; (Demasiadas solicitudes) | Todavía no hay llamadas dentro del límite disponible |
| Second 70 | POST /session/idp1/subject1 | 1 | 1 llamada recibe &quot;202 Accepted&quot; | Se ha establecido el límite de 200 llamadas disponibles porque han pasado 60 segundos desde las 10 segundas |


## Recomendaciones de integración de clientes {#customer-integration-recommendations}

Con una implementación correcta, los clientes no recibirán la respuesta &quot;429 demasiadas solicitudes&quot;.\
Aún así, el Adobe recomienda que cada cliente gestione correctamente la respuesta &quot;429 demasiadas solicitudes&quot; utilizando los detalles técnicos presentados anteriormente.
