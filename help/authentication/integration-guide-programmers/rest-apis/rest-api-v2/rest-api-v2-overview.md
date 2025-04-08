---
title: Información general de la API REST 2
description: Información general de la API REST 2
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: a02ba4ca1b6579781e40ecd0d12dbfdd23ea7398
workflow-type: tm+mt
source-wordcount: '528'
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

Adobe Pass Authentication se complace en anunciar el iniciar de REST API V2, diseñado para mejorar la experiencia del usuario y simplificar la integración con los servicios Pass.

Estamos entusiasmados con las posibilidades de nuestra nueva API REST, que representa un gran paso adelante para nuestra plataforma y abre la puerta a nuevas características y flujos de aplicación.

## ¿Qué es Nuevo? {#rest-api-v2-whats-new}

implementación únicas en todas las plataformas

Las aplicaciones de los clientes ahora pueden usar el mismo implementación en todas las plataformas, lo que facilita la iniciar nuevas funciones o el mantenimiento de aplicaciones en vivo.

### SSO dispositivos cruzada {#rest-api-v2-cross-device-sso}

La API de REST V2 permite pasar de forma segura una sesión de autenticación entre diferentes dispositivos. Simplemente pasando la sesión entre dispositivos, los usuarios pueden autenticarse en su dispositivo móvil y transmitir video en un dispositivos conectado al TV sin necesidad de volver a autenticarse.

### Múltiples sesiones de autenticación activas {#rest-api-v2-multiple-active-authentication-sessions}

Ahora es posible realizar diferentes sesiones activas de MVPD, y los clientes pueden optar por cambiar entre TempPass y la integración regular de MVPD cuando sea necesario.

### Mecanismo de seguridad mejorado {#rest-api-v2-enhanced-security-mechanism}

El registro dinámico de clientes es el mecanismo de seguridad utilizado en todos los flujos y funcionalidades. Permite un control más seguro y granular de las aplicaciones de los clientes y puede registrar aplicaciones en todas las plataformas.

### Rendimiento mejorado para tiempos de respuesta más rápidos {#rest-api-v2-improved-performance}

Los mecanismos mejorados de almacenamiento en caché permiten reducir el tráfico a las MVPD, mejorando los tiempos de respuesta y reduciendo la latencia. En general, hay un número reducido de llamadas de API hasta que comienza el vídeo.

### Códigos de error mejorados en todos los flujos {#rest-api-v2-enhanced-error-codes}

Avanzadas códigos de error ahora están disponibles en todos los flujos de Pass en el mismo formato, con detalles adicionales que guían las aplicaciones para mejorar el experiencia del usuario general.

### Control mejorado en todas las sesiones de autenticación {#rest-api-v2-improved-control}

La nueva API de REST V2 permite realizar acciones en varias sesiones autenticadas al mismo tiempo.

### Reducción de los costes de mantenimiento {#rest-api-v2-reduce-maintenance-costs}

Todas las respuestas y la información de error ahora están normalizadas.

## ¿Cuál es el siguiente paso? {#rest-api-v2-whats-next}

Todos los clientes que actualmente utilizan nuestras API a través de SDK o llamadas REST pueden seguir haciéndolo, ya que planeamos seguir proporcionando asistencia hasta finales de 2025.

Sin embargo, todos los desarrollos futuros se basarán en la API de REST V2. Recomendamos encarecidamente iniciar el proceso de migración para beneficiarse de las últimas funcionalidades Adobe Pass.

## ¿Quieres saber más? {#rest-api-v2-want-to-learn-more}

Para empezar, visita nuestra documentación pública:

- [Seminario web](#rest-api-v2-webinar)
- [Glosario](rest-api-v2-glossary.md)
- [Lista de verificación](rest-api-v2-checklist.md)
- [Preguntas más frecuentes](rest-api-v2-faqs.md)
- [Apis](apis/rest-api-v2-apis-overview.md)
- [Flujos](flows/rest-api-v2-flows-overview.md)
- Cocina
- Apéndice
- [Requisitos mínimos del sistema](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Nuestro equipo de soporte especializado también está disponible para ayudarle con cualquier pregunta o asistencia técnica que pueda necesitar.

### Seminario web {#rest-api-v2-webinar}

Vea el seminario web sobre la nueva API de REST V2, donde proporcionamos una visión general de las nuevas funciones y funcionalidades, así como una demostración en directo de la API de REST V2 en acción.

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## ¿Desea probar la API de REST V2? {#rest-api-v2-want-to-try}

Ahora puede explorar la API de REST V2 a través de nuestro Página dedicado al producto desde Adobe Systems sitio web para [desarrolladores](https://developer.adobe.com/adobe-pass/) .
