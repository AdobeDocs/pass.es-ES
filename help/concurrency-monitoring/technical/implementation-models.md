---
title: Modelos de implementación
description: Modelos de implementación
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%
---
# Modelos de implementación {#imp-models}

## Políticas del lado del servidor {#ss-policies}

Este modelo utilizará CM como punto de decisión de política, delegando así la decisión de acceso al servicio.

Dado que el cliente no debe asumir ninguna política con respecto a las políticas aplicadas, la implementación debe comprobar la decisión sobre la inicialización de la sesión, así como de forma regular, durante la reproducción desde la respuesta de latido.
