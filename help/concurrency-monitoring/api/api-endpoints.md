---
title: Extremos de API
description: Lista completa de las API de supervisión de concurrencia
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%
---
# Extremos de API

## Administración de sesiones principales

| Extremo | Método | Descripción |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | PUBLICAR | Crear una nueva sesión de flujo continuo |
| `/sessions/{idp}/{subject}/{session}` | PUBLICAR | Enviar latido para mantener viva la sesión |
| `/sessions/{idp}/{subject}/{session}` | DELETE | Finalizar una sesión |
| `/runningStreams/{idp}/{subject}` | GET | Obtener todas las sesiones activas de un asunto |

## Administración de metadatos

| Extremo | Método | Descripción |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | Obtener los campos de metadatos requeridos para la aplicación |
