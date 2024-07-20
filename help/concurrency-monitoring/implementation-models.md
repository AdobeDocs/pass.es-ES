---
title: Modelos de implementación
description: Modelos de implementación
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# Modelos de implementación {#imp-models}

## Políticas del lado del servidor {#ss-policies}

Este modelo utilizará CM como punto de decisión de política, delegando así la decisión de acceso al servicio.

Dado que el cliente no debe asumir ninguna política con respecto a las políticas aplicadas, la implementación debe comprobar la decisión sobre la inicialización de la sesión, así como de forma regular, durante la reproducción desde la respuesta de latido.
