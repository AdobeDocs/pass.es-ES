---
title: Información general de la API REST 2
description: Información general de la API REST 2
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: 63dc9636f74f8eee1af6205c4d31a01df4503050
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 0%

---

# Información general de la API REST 2 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

¿Desea mejorar la rentabilidad de sus aplicaciones de TVE?

¿Desea reducir el tiempo de desarrollo y los recursos necesarios para admitir aplicaciones de TVE en varias plataformas?

¿Desea garantizar una experiencia de usuario uniforme en todas las plataformas?

¿Desea reducir el esfuerzo de mantenimiento y simplificar la provisión de actualizaciones, correcciones de errores o mejoras?

## Introducción a la API de REST V2 {#rest-api-v2-introduction}

Adobe Pass Authentication se complace en anunciar el lanzamiento de la API de REST V2, diseñada para mejorar la experiencia del usuario y simplificar la integración con los servicios Pass.

Estamos entusiasmados con las posibilidades de nuestra nueva API de REST, que representa un gran paso adelante para nuestra plataforma y abre la puerta a nuevas funciones y flujos de aplicaciones.

## ¿Qué hay de nuevo? {#rest-api-v2-whats-new}

Implementación única en todas las plataformas

Las aplicaciones de los clientes ahora pueden utilizar la misma implementación en todas las plataformas, lo que facilita el lanzamiento de nuevas funciones o el mantenimiento de aplicaciones activas.

### SSO entre dispositivos {#rest-api-v2-cross-device-sso}

La API REST V2 permite pasar de forma segura una sesión de autenticación entre distintos dispositivos. Con solo pasar la sesión entre dispositivos, los usuarios pueden autenticarse en su dispositivo móvil y transmitir vídeo en un dispositivo conectado a la TV sin volver a autenticarse.

### Varias sesiones de autenticación activas {#rest-api-v2-multiple-active-authentication-sessions}

Ahora es posible realizar diferentes sesiones activas de MVPD y los clientes pueden elegir cambiar entre TempPass y la integración normal de MVPD cuando sea necesario.

### Mecanismo de seguridad mejorado {#rest-api-v2-enhanced-security-mechanism}

Registro dinámico de clientes es el mecanismo de seguridad utilizado en todos los flujos y funcionalidades. Permite un control más seguro y granular de las aplicaciones de los clientes y puede registrar aplicaciones en todas las plataformas.

### Rendimiento mejorado para tiempos de respuesta más rápidos {#rest-api-v2-improved-performance}

Los mecanismos mejorados de almacenamiento en caché permiten reducir el tráfico a las MVPD, mejorando los tiempos de respuesta y reduciendo la latencia. En general, hay un número reducido de llamadas de API hasta que comienza el vídeo.

### Códigos de error mejorados en todos los flujos {#rest-api-v2-enhanced-error-codes}

Los códigos de error avanzados ahora están disponibles en todos los flujos de pasos en el mismo formato, con detalles adicionales que guían las aplicaciones para mejorar la experiencia general del usuario.

### Control mejorado en todas las sesiones de autenticación {#rest-api-v2-improved-control}

La nueva API de REST V2 permite realizar acciones en varias sesiones autenticadas al mismo tiempo.

### Reducción de los costes de mantenimiento {#rest-api-v2-reduce-maintenance-costs}

Todas las respuestas y la información de error ahora están normalizadas.

## ¿Cuál es el siguiente paso? {#rest-api-v2-whats-next}

Todos los clientes que actualmente utilizan nuestras API a través de SDK o llamadas REST pueden seguir haciéndolo, ya que planeamos seguir proporcionando asistencia hasta finales de 2025.

Sin embargo, todos los desarrollos futuros se basarán en la API de REST V2. Recomendamos encarecidamente iniciar el proceso de migración para beneficiarse de las funcionalidades de Adobe Pass más recientes.

## ¿Desea obtener más información? {#rest-api-v2-want-to-learn-more}

Para empezar, visite nuestra documentación pública:

- [Seminario web](#rest-api-v2-webinar)
- [Glosario](rest-api-v2-glossary.md)
- [Lista de comprobación](rest-api-v2-checklist.md)
- [Reglas de IA](rest-api-v2-ai-rules.md)
- [Preguntas frecuentes](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [Flujos](flows/rest-api-v2-flows-overview.md)
- Libros
- Apéndice
- [Requisitos mínimos del sistema](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Nuestro equipo de soporte especializado también está disponible para ayudarle con cualquier pregunta o asistencia técnica que pueda necesitar.

### Seminario web {#rest-api-v2-webinar}

Vea el seminario web sobre la nueva API de REST V2, donde proporcionamos una visión general de las nuevas funciones y funcionalidades, así como una demostración en directo de la API de REST V2 en acción.

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## ¿Quiere probar la API de REST V2? {#rest-api-v2-want-to-try}

Ahora puede explorar la API REST V2 a través de nuestra página dedicada al producto desde el sitio web [Adobe Developer](https://developer.adobe.com/adobe-pass/).
