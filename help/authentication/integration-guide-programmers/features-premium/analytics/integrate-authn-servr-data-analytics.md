---
title: Integración de datos del lado del servidor de Adobe Pass Authentication en Adobe Analytics
description: Integración de datos del lado del servidor de Adobe Pass Authentication en Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Integración de datos del lado del servidor de Adobe Pass Authentication en Adobe Analytics

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Los clientes de la autenticación de Adobe Pass desean ver los datos del lado del servidor de autenticación de Adobe Pass (Adobe Pass) en el panel de Adobe Analytics para facilitar el consumo.

Los datos servirán para rastrear métricas importantes de TVE, como tasas de conversión de autenticación por MVPD, usuarios únicos basados en el ID de usuario de MVPD y más.

No está pensado para reemplazar una implementación del lado del cliente si ya existe, ya que la actividad del usuario no se puede rastrear más allá de los eventos específicos siguientes en ausencia de un ID de visitante. Si los clientes proporcionan un ID de visitante en las llamadas de Pass, podemos desbloquear otro tipo de integración de Analytics (en tiempo real) que pueda unir todos los eventos de Pass con los datos de clientes existentes. Encuentre más detalles sobre este nuevo tipo de integración posible aquí: &quot;[Uso del ID de Experience Cloud en la autenticación de Adobe Pass](/help/authentication/integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)&quot;

## Métricas incluidas {#metrics-included-int-authn-analyt}

| Evento | Descripción |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| Se solicitó AuthN | Número de flujos de autenticación iniciados |
| AuthN pendiente | Número de tokens de autenticación generados correctamente (sin tener en cuenta si el cliente los obtuvo o no) |
| AuthN OK | Número de tokens de autenticación obtenidos correctamente por los usuarios |
| AuthZ solicitado | Número de intentos de autorización |
| AuthZ OK | Número de autorizaciones correctas |
| Error de AuthZ | Número de autorizaciones denegadas por MVPD en el nivel de aplicación |
| Reproducir solicitud | Número de tokens de medios cortos generados (que se asimilan al número de solicitudes de reproducción) |
| Cierre de sesión solicitado | Número de flujos de cierre de sesión iniciados |
| Cierre de sesión completado | Número de flujos de cierre de sesión completados correctamente |
| Error al cerrar sesión | Número de flujos de cierre de sesión erróneos |
| Preautorización solicitada | Número de flujos de preautorización iniciados |
| Autorización previa OK | Número de eventos de preautorización correctos con los recursos preautorizados |
| Preautorización denegada | Número de eventos de preautorización con los recursos a los que se denegó la preautorización |
| Error de preautorización | Número de eventos de preautorización con errores |

| Nombre de Adobe Analytics | Descripción |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Canal | ID del solicitante utilizado para realizar la solicitud de asignación de derechos |
| MVPD | La MVPD responsable de conceder el derecho al usuario |
| Proxy | La MVPD proxy (que será &quot;Directa&quot; para integraciones directas) |
| Tipo de SDK | El SDK de cliente utilizado (Flash, HTML5, nativo de Android, iOS, sin cliente, etc.) |
| Versión de SDK | La versión del SDK del cliente de autenticación de Adobe Pass |
| ID de recurso | El título real del recurso implicado en la solicitud de autorización (extraído de la carga útil MRSS como el artículo/título si se proporciona) |
| Tipo de error de AuthZ | El motivo de los errores, según lo informado por la autenticación de Adobe Pass <br/> Estos son los valores más comunes <br/> **noAuthZ** = la MVPD respondió que el usuario no tiene el canal en su paquete<br/> **red** = no pudimos conectar con la MVPD (la MVPD tiene un problema en el momento de la llamada y no respondió)<br/> **norefreshtoken** = esto es estrictamente para implementaciones de OAuth y podría resultar si el usuario cambió su contraseña o la MVPD la negó por alguna razón. Generalmente genera una nueva autenticación<br/> **mismatch** = si la solicitud se realiza desde un dispositivo diferente al que tenía el token de autenticación. Esto puede ocurrir si los usuarios intentan engañar al sistema, pero la mayoría de estos errores se produjeron en el contexto del antiguo SDK de JavaScript, en el que el ID del dispositivo utilizaba la dirección IP como parte del cálculo. Si un usuario viera TVE en casa y luego en el trabajo, este error se activaría y tendría que autenticarse de nuevo<br/> **no válido** = solicitud no válida, faltan parámetros o no son válidos<br/>  **authzNone** = Los programadores tienen la capacidad de denegar autorizaciones para una combinación específica de channelMaxMVPD. Esto se activa mediante una API back-end a la cual los programadores tienen acceso <br/> **fraude** = es un mecanismo de protección de nuestro lado. Si el usuario no consigue la autorización y luego la vuelve a solicitar varias veces en un corto intervalo (segundos), se deniega la llamada directamente. Suele ocurrir cuando un programador tiene un error en la implementación que solicita autorización constantemente si falla. |
| Tipo de token | Cuando se crean tokens debido a AuthZ All y AuthN All, tenemos que saber qué se hace debido a una medida de degradación.<br/> Son:<br/> &quot;normal&quot; = El caso normal<br/> &quot;authnall&quot; = Cuando AuthN All está habilitado<br/> &quot;authzall&quot; = Cuando AuthZ All está habilitado<br/> &quot;hba&quot; = Cuando HBA está habilitado |
| Tipo de dispositivo sin cliente | La plataforma del dispositivo (alternativa), utilizada actualmente para Clientes sin conexión.<br/> Los valores pueden ser:<br/> N/A - el evento no se originó desde un SDK sin cliente<br/> Desconocido - Dado que el parámetro deviceType de una **API sin cliente** es opcional, hay llamadas que no contienen ningún valor.<br/> Cualquier otro valor enviado a través de la **API sin cliente**. Por ejemplo, xbox, appletv y roku. |
| ID de usuario de MVPD | Reemplaza el ID de visitante basado en cookies |


## Detalles {#details-int-authn-analyt}

* Las métricas se insertarán evento a evento en el grupo de informes específico mediante la API de inserción de datos de Adobe Analytics
* La inserción se procesa por lotes y se envía cada 30 minutos. Debido a esto, el informe debe estar marcado con hora
* Cada cliente tendrá uno o varios grupos de informes. Un ID de solicitante (canal) solo se asignará a un grupo de informes. Varios ID de solicitante solo se pueden asignar a un grupo de informes.
* Se pueden proporcionar datos históricos, pero deberá tener especial cuidado debido a problemas de tráfico/rendimiento.
* La variable única de visitante se establece en el ID de usuario de MVPD
* La asignación de eventos y eVars no se puede configurar.


## SLA {#sla-int-authn-serv-anal}

Al no ser un componente esencial, SLA no garantiza la integración.

## Configuración del grupo de informes {#report-suite-config}

El informe debe tener una marca de hora, ya que los eventos se enviarán por lotes.

### Eventos {#report-suite-config-events}


>[!NOTE]
>Todo debe configurarse con:
>
>* Contador (sin subrelaciones)

| Evento | Evento de Adobe Analytics |
|---------------------------------------|-----------------------|
| Se solicitó AuthN | event1 |
| AuthN pendiente | event2 |
| AuthN OK | event3 |
| AuthZ solicitado | event4 |
| AuthZ OK | event5 |
| Error de AuthZ | event6 |
| Reproducir solicitud | event7 |
| Error de AuthN | event8 |
| Solicitud de token AuthN sin cliente OK | event9 |
| Error de solicitud de token de AuthN sin cliente | event10 |
| Error de solicitud de reproducción | event11 |
| Solicitud de desconexión | event12 |
| Cierre de sesión completado | event13 |
| Error al cerrar sesión | event14 |
| Solicitud de verificación previa | event15 |
| Error de comprobación preliminar | event16 |
| Comprobación preliminar concedida | event17 |
| Comprobación preliminar denegada | event18 |


### eVars {#evars}


>[!NOTE]
>Todo debe configurarse con:
>
>* Asignación: Más reciente (último)
>* Caduca después de: Visita
>* Tipo: Cadena de texto

| Propiedad | eVar |
|-----------------------------------|--------------------------------|
| Canal | EVAR 1 |
| MVPD | EVAR 2 |
| Proxy | EVAR 3 |
| Tipo de SDK | EVAR 4 |
| Versión de SDK | EVAR 5 |
| ID de recurso | EVAR 6 |
| Tipo de error de AuthZ | EVAR 7 |
| Tipo de token | EVAR 8 |
| Tipo de dispositivo sin cliente | EVAR 9 |
| ID de usuario de MVPD | visitorID (hecho automáticamente) |
| ID de usuario de MVPD | eVar 10 |
| Tipo de dispositivo | eVar 11 |
| Sistema operativo | eVar 12 |
| Tipo de hardware principal | eVar 13 |
| TTL | eVar 14 |
| Tipo de autenticación | eVar 15 |
| Versión de la arquitectura del servidor | eVar 16 |
| Proveedor de autenticación externa | eVar 17 |
| Latencia | eVar 18 |
| ID de visitante | eVar 19 |
| Mecanismo de SSO | eVar 20 |
| Modelo de dispositivo | eVar 21 |
| Versión del dispositivo | eVar 22 |
| Modelo de hardware de dispositivo | eVar 23 |
| Proveedor de hardware de dispositivo | eVar 24 |
| Fabricante de hardware de dispositivo | eVar 25 |
| Versión de hardware del dispositivo | eVar 26 |
| Nombre del SO del dispositivo | eVar 27 |
| Familia de SO del dispositivo | eVar 28 |
| Proveedor de SO del dispositivo | eVar 29 |
| Versión del SO del dispositivo | eVar 30 |
| Nombre del explorador del dispositivo | eVar 31 |
| Proveedor de explorador de dispositivos | eVar 32 |
| Versión del explorador del dispositivo | eVar 33 |
| Tipo de dispositivo sin cliente normalizado | eVar 34 |

## Precio {#pricing}

Póngase en contacto con su TAM para obtener más información.
