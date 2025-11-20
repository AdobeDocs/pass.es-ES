---
title: Introducción a la monitorización de concurrencia
description: Conozca los conceptos básicos de la Monitorización de concurrencia y cómo empezar a utilizar su integración
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Introducción a la monitorización de concurrencia {#getting-started-overview}

Bienvenido a la Monitorización de concurrencia. Esta guía le ayudará a comprender los aspectos básicos y poner en marcha su integración rápidamente.

## La Monitorización De La Concurrencia Resuelve Problemas {#problem-solved}

En el panorama actual del streaming, los suscriptores esperan acceder al contenido en varios dispositivos y aplicaciones. Sin embargo, esto crea desafíos para los proveedores de contenido:

- **Los acuerdos de licencia de contenido** suelen limitar el número de transmisiones simultáneas
- **Los costos de ancho de banda** aumentan con el uso simultáneo excesivo
- **La experiencia del usuario** sufre cuando demasiadas secuencias degradan el rendimiento
- **La protección de ingresos** requiere evitar el abuso de uso compartido de cuentas

La Monitorización de concurrencia proporciona una solución centralizada a estos desafíos.


## Funcionamiento de la monitorización de concurrencia

### Funcionalidad principal

La supervisión de concurrencia funciona como un **servicio de administración de sesiones** que:

1. **Rastrea sesiones de streaming activas** en tiempo real en todas las aplicaciones
2. **Evalúa las políticas empresariales** con respecto a los patrones de uso actuales
3. **Aplica límites** cuando se infringen directivas
4. **Proporciona opciones de resolución** cuando se producen conflictos

## Ventajas para diferentes partes interesadas

### Proveedores de contenido (programadores)

- **Proteja los derechos de contenido** y cumpla con los acuerdos de licencia
- **Optimizar los costos de ancho de banda** al evitar el uso excesivo
- **Mejore la experiencia del usuario** con comentarios claros sobre los límites
- **Obtener información de uso** mediante informes detallados

### Proveedores de identidad (MVPD)

- **Aplicar acuerdos de suscriptor** entre todos los socios de contenido
- **Administración centralizada de directivas** para varias aplicaciones
- **Supervisión en tiempo real** de los patrones de uso
- **Arquitectura escalable** que administra millones de sesiones

### Usuarios finales

- **Borrar la comprensión** de sus límites de uso
- **Experiencia coherente** en todas las aplicaciones
- **Opciones para resolver conflictos** cuando se alcancen los límites
- **Comentarios transparentes** sobre por qué está restringido el acceso


## Ruta de inicio rápido {#quick-start-path}

1. **Revisar conceptos clave**: obtenga información acerca de [sesiones, directivas y metadatos](key-concepts.md)
2. **Elige tu estrategia** - revisa [estrategias LIFO vs FIFO](../use-cases/lifo-fifo-strategies.md)
3. **Empiece a implementar** - Siga las [referencias de la API](../api/api-reference-overview.md)

## ¿Listo para la integración? {#ready-to-integrate}

- [Referencia de API](../api/api-reference-overview.md)
- [Casos de uso](../use-cases/cm-use-cases.md)


## Registro de servicio {#service-registration}

Para comenzar a usar la Monitorización de concurrencia, comuníquese con nuestro [Equipo de soporte](mailto:tve-support@adobe.com) y comuníquese con la siguiente información:

1. **Nombre de empresa** y datos de contacto
2. **Aplicaciones** que desea integrar con la Monitorización de concurrencia. Para cada solicitud, indique:
   1. Nombre de aplicación
   2. Plataforma(s) de aplicación
3. **Socio de integración** (en caso de que se esté suscribiendo a la Monitorización de concurrencia a solicitud de otro, programador o MVPD)


## ¿Necesita ayuda? {#need-help}

- **Explorador de API** - Probar API de forma interactiva en [IU de Swagger](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)
- **Términos y definiciones clave** - [Glosario](../cm-glossary.md)
- **¿Cómo obtener ayuda?** - [Procedimientos de soporte](../support/cm-escalation-procedures.md)
- **Soporte técnico** - Contacto [tve-support@adobe.com](mailto:tve-support@adobe.com)
