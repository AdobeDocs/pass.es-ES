---
title: Funciones de integración de MVPD
description: Funciones de integración de MVPD
exl-id: fcd65940-9a86-49b2-9d52-9031fb763338
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1733'
ht-degree: 2%

---

# Funciones de integración de MVPD

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#mvpd-int-features-overview}

La autenticación de Adobe Pass es compatible con los estándares emergentes para TV en todas partes. Cumple con la especificación **CableLabs OLCA (Acceso de contenido en línea)**, que proporciona requisitos técnicos y arquitectura para la entrega de vídeo a un cliente de TV de pago desde fuentes en línea. En junio de 2011, el Adobe participó en el proyecto conjunto de pruebas de interopt de CableLabs y aprobó el proceso de prueba para la implementación de un proveedor de servicios.  El Adobe también es miembro activo del **Consorcio Técnico de Autenticación Abierta (OATC)** y participa en varios de los proyectos de redacción de especificaciones de los subcomités como parte de ese organismo.

Después de haber descrito el cumplimiento OLCA de la autenticación de Adobe Pass y la participación de Adobe en OATC, también es importante tener en cuenta que la autenticación de Adobe Pass no depende del protocolo.  Pero en esta etapa de la era de TVE, la autenticación de Adobe Pass está definitivamente orientada hacia los estándares OLCA.  Las normas definen las formas actualmente acordadas para que los diferentes reproductores de TVE (programadores, MVPD y proveedores de servicios) implementen las funciones de TVE. Muchas de estas funciones se enumeran en las tablas siguientes, con vínculos a páginas relacionadas que proporcionan detalles y ejemplos de cómo implementar las funciones.

La información contenida en las tablas tiene por objeto dirigir el proceso de integración de autenticación de Adobe Pass hacia una funcionalidad coherente en todas las integraciones de MVPD. La columna de prioridad clasifica las funciones en A, B y C:

* R - Son funciones &quot;obligatorias&quot; para el despliegue inicial de una integración.
* B - Se trata de mejoras importantes en la integración inicial, que se añadirán después del despliegue inicial.
* C - Se trata de mejoras adicionales en la integración que pueden implementarse después de los requisitos &quot;B&quot;.


## 1. Funciones principales {#core-func-features}


| No. | Función | Descripción | Prioridad | Notas |
|------|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1,1 | [Inicio de sesión iniciado por el programador](/help/authentication/authn-oauth2-protocol.md) | El visor selecciona la MVPD e inicia el flujo de autenticación (AuthN) desde el sitio web o la aplicación de la marca Programmer. | A+ |                                                                                                                                                         |
| 1,2 | [Autorización basada en el canal](/help/authentication/authz-usecase.md) | Una vez autenticada, la autorización (AuthZ) puede ocurrir en segundo plano, con el paso de solo un identificador de canal de red, y un identificador de usuario, a la MVPD. | A+ |                                                                                                                                                         |
| 1,3 | Persistencia del ID de usuario | La MVPD proporciona un UserID confuso y persistente. | A |                                                                                                                                                         |
| 1,4 | [Compatibilidad con inicio de sesión único](/help/authentication/sso-support.md) | El usuario inicia sesión con la MVPD en el sitio de una marca y luego puede ir a otra marca y no se le pedirá que vuelva a iniciar sesión. La sesión de AuthN se comparte entre las marcas. La compatibilidad con SSO es para sitios web y aplicaciones móviles/de dispositivos.  Para las MVPD, requiere devolver un ID de usuario o algún otro token de usuario que se pueda usar para la AuthZ en todas las marcas. | A |                                                                                                                                                         |
| 1,5 | Experiencia del usuario de inicio de sesión (UX) optimizada para dispositivos | La MVPD admite el cambio de tamaño de la pantalla de inicio de sesión para que se ajuste a las dimensiones del dispositivo que utiliza la vista. | A |                                                                                                                                                         |
| 1,6 | Logotipo de selector de MVPD predeterminado | La MVPD proporciona una URL a un logotipo predeterminado de dimensiones adecuadas (112 x 33 píxeles). | A | El logotipo debe alojarse en la MVPD y CDN debe almacenarlo en caché. |
| 1,7 | [Ámbito de proveedor de servicios](/help/authentication/serv-provider-scoping.md) | MVPD admite pasar el identificador de marca (valor del solicitante) en la solicitud AuthN. | A- | Esto habilita una experiencia de inicio de sesión específica de Service Provider. |
| 1,8 | [Autorización de varios canales](/help/authentication/mvpd-preflight-authz.md#preflight-multich-authz) | La MVPD admite que un programador envíe varios canales en una única solicitud de autorización. | B+ |                                                                                                                                                         |
| 1,9 | Autenticación basada en iFrame o &quot;JS Pop-up&quot; | El programador puede integrar el flujo de inicio de sesión en una experiencia iFrame o emergente en lugar de una redirección HTTP. | B |                                                                                                                                                         |
| 1,10 | [Cierre de sesión iniciado por el programador](/help/authentication/usecase-mvpd-logout.md) | El visualizador tiene una sesión autenticada en el sitio o la aplicación del programador y con la MVPD. El visor puede iniciar un cierre de sesión federado desde el sitio del programador, y el cierre de sesión también borra la sesión en el portal MVPD. | B | Garantiza que los equipos compartidos estén más protegidos contra el uso indebido desde la perspectiva del programador. |
| 1,11 | [Mensajería de error de autorización personalizada](/help/authentication/error-reporting.md) | La MVPD pasa su propia cadena de error personalizada, que es apropiada para que el sitio o la aplicación federados del programador se muestren al usuario. | B | Habilita los escenarios de ampliación de venta |
| 1,12 | **Metadatos de usuario en la respuesta de autenticación** | La respuesta de MVPD AuthN puede incluir metadatos de usuario que actúan como una sugerencia para personalizar la experiencia del usuario durante el flujo de asignación de derechos. Este requisito permite al programador recibir sugerencias de control parental de la MVPD. | B- |                                                                                                                                                         |
| 1,13 | Autenticación iniciada por MVPD | El visor completa una sesión de AuthN correcta en el portal de MVPD y, a continuación, navega al sitio de TVE del programador. No se solicita al usuario el selector de MVPD y se autentica automáticamente. | B- |                                                                                                                                                         |
| 1,14 | Ámbito de UserID | El UserID de MVPD debe tener dos formas: una dirigida a los programadores y otra a todo el Adobe para detectar fraudes.  Esto permite al Adobe compartir el ID de usuario de MVPD con alcance de programador sin más cifrado u ofuscación. | C |                                                                                                                                                         |
| 1,15 | Autorización basada en recursos | Una vez completado AuthN, AuthZ puede ocurrir en segundo plano, pasando datos estructurados que pueden incluir red, programa, recurso, clasificación de control parental y más según sea necesario. Esto permite el control parental en cada llamada de AuthZ desde el Programador al MVPD. | C |                                                                                                                                                         |
| 1,16 | Cierre de sesión iniciado por MVPD | El visualizador tiene una sesión autenticada en el sitio o la aplicación del programador y con la MVPD. El visor puede iniciar un cierre de sesión federado desde el sitio de MVPD que también borra la sesión en todos los sitios de Federated Programmer. | C |                                                                                                                                                         |
| 1,17 | Obligaciones de autorización | La MVPD proporciona condiciones adicionales en la respuesta AuthZ, como el registro, o una clasificación de control parental máxima (OLCA) actualizada. | C |                                                                                                                                                         |
| 1,18 | Contexto de dirección IP | La MVPD requiere pasar explícitamente la dirección IP. En el caso de AuthZ, donde las llamadas son del lado del servidor, proporciona a la MVPD información de seguimiento de fraude sobre la procedencia del usuario en la llamada AuthZ. | C |                                                                                                                                                         |
| 1,19 | Valor de tiempo de vida (TTL) de persistencia de token definido dinámicamente | La MVPD puede establecer el TTL del token de autenticación de Adobe Pass de forma dinámica a través de las propiedades de la respuesta, de modo que el Adobe esté fuera del bucle para los cambios de TTL. | C- |                                                                                                                                                         |
| 1,20 | Tipo de dispositivo | La MVPD admite pasar el tipo de dispositivo en la solicitud AuthN o AuthZ. Esta propiedad informa a la MVPD de la naturaleza del dispositivo donde se consumirá el contenido, por lo que podría ajustar el TTL del token de AuthN o AuthZ para ajustarse a sus propias consideraciones de seguridad para el dispositivo. | C- | Esto resulta útil para la plataforma sin cliente, donde puede resultar difícil exponer cada tipo de dispositivo como una configuración en el lado de la autenticación de Adobe Pass. |



## 2. Características operativas {#operational-features}

| No | Función | Descripción | Prioridad | Notas |
|-----|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------|
| 2,1 | Tiempo de actividad 24/7 | Un acuerdo de MVPD para luchar por un tiempo de actividad 24/7 y medir con eso a lo largo del tiempo. | A |       |
| 2,2 | Probar Las Credenciales De Cada Programador | La MVPD proporciona cuentas de prueba independientes para cada Programador. | A |       |
| 2,3 | Plan de programación de caducidad y renovación de certificados | Un plan documentado por MVPD con un cronograma para garantizar que los certificados SAML estén actualizados. | A- |       |
| 2,4 | Latencia Media Por Debajo De 250 Ms/Respuesta | Un acuerdo de MVPD para luchar por una baja latencia y medirla con el tiempo. | A- |       |
| 2,5 | Alojamiento Propio Logotipo de Selector de MVPD | La MVPD necesita alojar su propio logotipo y debe estar protegido por el almacenamiento en caché de la CDN. | B |       |
| 2,6 | Plan de escalación de soporte | Un plan documentado por MVPD para la escalación que se mantiene actualizado y se revisa al menos trimestralmente. | A |       |
| 2,7 | Plan de protección contra interrupciones | Un plan conjunto de MVPD con Adobe sobre medidas de degradación que puedan utilizarse en general según sea necesario. | B |       |
| 2,8 | Mantener puntos finales de ensayo y producción independientes | Una prueba de integración de MVPD que no requiera la suplantación de archivos host para probar la integración de ensayo. | B |       |
| 2,9 | Punto final de control de calidad adicional | La MVPD mantiene una integración de QA IdP adicional para el desarrollo conjunto de nuevas funciones.  Si se admite esto, es menos probable que necesitemos solicitudes especiales de UAT para probar los certificados de la tienda de aplicaciones. | C |       |





## 3. Funciones de experiencia de autenticación {#authn-exp-features}


| No | Función | Descripción | Prioridad | Notas |
|-----|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 3,1 | Conversión de autenticación sobre la expectativa mínima | La MVPD garantiza una tasa de conversión mínima para demostrar su funcionalidad (5%) y razonable (30%). | A |                                                                                                                                                           |
| 3,2 | Recuperación de contraseña en línea | La MVPD proporciona un medio para recuperar contraseñas en línea al flujo federado AuthN. | A |                                                                                                                                                           |
| 3,3 | Registro de cuenta en línea | La MVPD proporciona un medio para crear una nueva cuenta en línea para el flujo de AuthN federado. | A |                                                                                                                                                           |
| 3,4 | Ayuda y asistencia técnica en línea | La MVPD proporciona un medio para proporcionar ayuda durante el flujo de AuthN federado. | A |                                                                                                                                                           |
| 3,5 | Autenticación en el hogar basada en módem | La MVPD autentica automáticamente un dispositivo cuando está en la red local de un modelo registrado (sólo MVPD del ISP). | B | Esta es una prioridad más baja porque es una optimización que muchos aún no pueden soportar e introduce algunos desafíos para la mitigación del fraude y el control parental |
Ahora puede importar código de tabla de Markdown directamente mediante el cuadro de diálogo File/Paste table data... .



## 4. Funciones de Analytics {#analytics-features}


| No | Función | Descripción | Prioridad |
|-----|--------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 4,1 | Alineación del canal de conversión de autenticación | La MVPD tiene una métrica para solicitudes de AuthN que comienza con la solicitud de AuthN de SAML para alinearse con las métricas de solicitud de AuthN de programador y autenticación de Adobe Pass. | A |
| 4,2 | Usuarios únicos | Usuarios que se han autenticado y deduplicado correctamente en un resumen mensual a lo largo de los días. | A |
| 4,3 | Contabilidad de husos horarios | Los informes incluyen una zona horaria para cuando se renueva el día. | A |





## 5. Funciones de mitigación de fraude {#fraud-mitgn-features}


| No | Función | Descripción | Prioridad | Notas |
|-----|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|------------------------------------------------------------------------------------------------|
| 5,1 | Validación de suscriptor de vídeo en autenticación | La MVPD garantiza que el usuario sea un suscriptor de vídeo válido durante el flujo de AuthN. | A |                                                                                                |
| 5,2 | Limitación de velocidad para autenticación/autorización | La MVPD admite la limitación de sus servicios web para limitar el uso de una cuenta de usuario específica cuando corresponda. | B |                                                                                                |
| 5,3 | ID de usuario persistente con ámbito global de Adobe | La MVPD garantiza que el Adobe tenga un ID de usuario que se pueda rastrear entre programadores para detectar fraudes. Esto no necesita proporcionarse directamente al cliente. | B | Especificación del Protocolo de Autorización Multimedia en Línea (OMAP) y Monitoreo de Usuario Real (RUM). |
| 5,4 | Validación de uso simultáneo | La MVPD tiene un medio para rastrear y limitar el uso simultáneo de la cuenta del suscriptor más allá de un umbral comercial. | B |                                                                                                |

## P1. Funciones funcionales específicas de proxy {#proxy-sp-func-features}

| No | Descripción | Prioridad | Notas |
|-------|--------------------------------------------------------------------------------|----------|---------------------------------------------------|
| P 1.1 | Proxy responsable de mantener la precisión de la lista actualizada de subMVPD | A |                                                   |
| P 1.2 | El proxy ofrece un logotipo del tamaño adecuado para cada subdispositivo MVPD | A | Algunas subMVPD activas no tienen el tamaño de logotipo correcto |
| P 1.3 | El proxy ofrece una página de inicio de sesión con la marca adecuada para cada subMVPD | A |                                                   |
| P 1.4 | Proxy responsable de segmentar marcas de proveedores de servicios específicas con la lista subMVPD | B |                                                   |
| P 1,5 | El proxy especifica correctamente todas las propiedades de subMVPD (tamaño de iFrame, TTL, logotipo, etc.) | A |                                                   |
| P 1.6 | Proxy especifica entityID independiente | B |                                                   |

## P2. Funciones operativas del proxy {#proxy-op-features}

| No | Descripción | Prioridad |
|-------|----------------------------------------------------------------------------------------|----------|
| P 2.1 | La clave API está protegida | A |
| P 2.2 | Credenciales de prueba para la primera subMVPD utilizada en la integración activa para cada nuevo solicitante | A |
| P 2.3 | Las listas subMVPD son precisas y completas por solicitante | A |
