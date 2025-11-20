---
title: Extremos de API
description: Lista completa de las API de supervisión de concurrencia
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
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
