---
title: Información general de la API REST 2
description: Información general de la API REST 2
source-git-commit: dd3451f8761ce6183e9a11099fb3094abae09466
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---

# Información general de la API REST 2 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

¿Desea mejorar la rentabilidad de sus aplicaciones de TVE?

¿Desea reducir el tiempo de desarrollo y los recursos necesarios para admitir aplicaciones de TVE en varias plataformas?

¿Desea garantizar una experiencia de usuario uniforme en todas las plataformas?

¿Desea reducir el esfuerzo de mantenimiento y simplificar la provisión de actualizaciones, correcciones de errores o mejoras?

## Introducción a la API de REST V2

Adobe Pass Authentication se complace en anunciar el lanzamiento de la API de REST V2, diseñada para mejorar la experiencia del usuario y simplificar la integración con los servicios Pass.

Estamos entusiasmados con las posibilidades de nuestra nueva API de REST, que representa un gran paso adelante para nuestra plataforma y abre la puerta a nuevas funciones y flujos de aplicaciones.

## ¿Qué hay de nuevo?

Implementación única en todas las plataformas

Las aplicaciones de los clientes ahora pueden utilizar la misma implementación en todas las plataformas, lo que facilita el lanzamiento de nuevas funciones o el mantenimiento de aplicaciones activas.

### SSO entre dispositivos

La API REST V2 permite pasar de forma segura una sesión de autenticación entre distintos dispositivos. Con solo pasar la sesión entre dispositivos, los usuarios pueden autenticarse en su dispositivo móvil y transmitir vídeo en un dispositivo conectado a la TV sin volver a autenticarse.

### Varias sesiones de autenticación activas

Ahora es posible realizar diferentes sesiones de MVPD activas, y los clientes pueden elegir entre TempPass e integración de MVPD regular cuando sea necesario.

### Mecanismo de seguridad mejorado

Registro dinámico de clientes es el mecanismo de seguridad utilizado en todos los flujos y funcionalidades. Permite un control más seguro y granular de las aplicaciones de los clientes y puede registrar aplicaciones en todas las plataformas.

### Rendimiento mejorado para tiempos de respuesta más rápidos

Los mecanismos mejorados de almacenamiento en caché permiten reducir el tráfico a las MVPD, mejorando los tiempos de respuesta y reduciendo la latencia. En general, hay un número reducido de llamadas de API hasta que comienza el vídeo.

### Códigos de error mejorados en todos los flujos

Los códigos de error avanzados ahora están disponibles en todos los flujos de pasos en el mismo formato, con detalles adicionales que guían las aplicaciones para mejorar la experiencia general del usuario.

### Control mejorado en todas las sesiones de autenticación

La nueva API de REST V2 permite realizar acciones en varias sesiones autenticadas al mismo tiempo.

### Reducción de los costes de mantenimiento

Todas las respuestas y la información de error ahora están normalizadas.

## ¿Cuál es el siguiente paso?

Todos los clientes que actualmente utilizan nuestras API a través de SDK o llamadas REST pueden seguir haciéndolo, ya que planeamos seguir proporcionando asistencia hasta finales de 2025.

Sin embargo, todos los desarrollos futuros se basarán en la API de REST V2. Recomendamos encarecidamente iniciar el proceso de migración para beneficiarse de las funcionalidades de Adobe Pass más recientes.

## ¿Desea obtener más información?

Para empezar, visite nuestra documentación pública:

- [Glosario](rest-api-v2-glossary.md)
- [API](./apis/rest-api-v2-apis-overview.md)
- [Flujos](./flows/rest-api-v2-flows-overview.md)

Nuestro equipo de soporte especializado también está disponible para ayudarle con cualquier pregunta o asistencia técnica que pueda necesitar.

## ¿Quiere probar la API de REST V2?

Ahora puede explorar la API REST V2 a través de nuestra página dedicada al producto desde el sitio web [Adobe Developer](https://developer.adobe.com/adobe-pass/).
