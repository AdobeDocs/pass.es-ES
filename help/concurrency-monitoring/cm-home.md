---
title: Introducción a la monitorización de concurrencia
description: Introducción a la monitorización de concurrencia
exl-id: 725cc64b-6b03-46e3-a038-41e9b1341c6b
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%
---
# Introducción a la monitorización de concurrencia {#intro}

La Monitorización de concurrencia es un servicio que permite a los proveedores de contenido y de identidad (MVPD y programadores) definir y hacer cumplir límites en el streaming de vídeo simultáneo en varias aplicaciones, dispositivos y plataformas. Tanto si es un programador que busca controlar cuántos flujos puede ver un suscriptor simultáneamente, como si es un MVPD que desea aplicar políticas de uso a todos sus socios de contenido, la Monitorización de concurrencia proporciona las herramientas que necesita.

## ¿Qué es la monitorización de concurrencia? {#what-is-cm}

La Monitorización de concurrencia es un servicio centralizado que rastrea y administra sesiones de streaming de vídeo activas en tiempo real. Le permite hacer lo siguiente:

- **Limitar transmisiones simultáneas por suscriptor** - Controlar el número de transmisiones simultáneas de vídeo a las que puede acceder un usuario
- **Aplicar restricciones de dispositivos** - Limitar la transmisión a tipos o cantidades de dispositivos específicos
- **Implementar directivas basadas en la ubicación** - Restringir el streaming según la ubicación geográfica
- **Crear reglas específicas para el contenido**: aplique límites diferentes para el contenido en directo frente al contenido de VOD
- **Monitorizar patrones de uso**: obtenga información sobre cómo se consume el contenido

## Cómo funciona {#how-it-works}

La Monitorización de concurrencia funciona a través de una API simple pero potente:

1. **Inicialización de sesión**: cuando un usuario empieza a ver contenido, la aplicación crea una sesión
2. **Evaluación de directivas**: el servicio evalúa las directivas definidas según el uso actual
3. **Supervisión en tiempo real**: Las llamadas de Heartbeat mantienen las sesiones activas y supervisan el cumplimiento

## ¿Es nuevo en la monitorización de concurrencia? {#new-to-cm}

Empiece con nuestra [Guía de introducción](getting-started/getting-started-overview.md) para comprender los conceptos básicos y configurar su primera integración.

