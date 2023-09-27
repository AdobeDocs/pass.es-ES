---
title: Requisitos mínimos del sistema
description: Requisitos mínimos del sistema
exl-id: 57b21e2a-abd7-4b4b-85f1-25584a850e40
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 0%

---

# Requisitos mínimos del sistema {#minimum-system-requirements}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


## Información general {#overview}

Este documento presenta los requisitos actuales de software y hardware para implementar las integraciones de autenticación de Adobe Pass en las plataformas compatibles. Todos los navegadores web/móviles y sistemas operativos admitidos que se enumeran a continuación se beneficiarán del soporte completo del equipo de autenticación de Adobe Pass, sujeto a los SLA acordados.

Mientras que, como equipo de autenticación de Adobe Pass, fomentamos el uso de las últimas versiones estables de los navegadores y sistemas operativos; también reconocemos la existencia de plataformas y navegadores incompatibles o más antiguos que actualmente están en uso. Estos dispositivos obsoletos todavía pueden funcionar sin problemas, sin embargo, serán más propensos a errores.

El enfoque inicial para mitigar cualquier problema que aparezca en estas plataformas obsoletas debe ser actualizar a las versiones más recientes; puede ser la versión del sistema operativo, del explorador o de la aplicación instalada.

Cualquier problema que aparezca en estas plataformas se solucionará como la mejor solución por parte del equipo de autenticación de Adobe Pass.

Adobe Pass anima a sus clientes y socios a considerar la posibilidad de actualizar a las versiones más recientes para beneficiarse de la compatibilidad total de Adobe en cualquier problema potencial, además de las mejoras de rendimiento, eficacia y seguridad.


## Requisitos del explorador y del sistema operativo {#browser-OS-system-requirements}


| Navegador web/móvil (†) | Versiones compatibles |
|---|---|
| Google Chrome | **70** o posterior |
| Mozilla Firefox | **57** o posterior |
| Apple Safari | **14** o posterior |
| Microsoft Edge | **100** o posterior |

(†) El Adobe aconseja no utilizar el modo privado o de incógnito.

| Sistema operativo | Versiones compatibles |
|---|---|
| *Android* | **7,0** (Turrón) o posterior |
| *iOS* | **14** o posterior |
| *iPadOS* | **14** o posterior |
| *tvOS* | **14** o posterior |
| *Fire OS* | **5 (Android 5.1)** o posterior |
| *SO MAC* | **10,13** o posterior |
| *Microsoft Windows* | **10** o posterior |




>[!NOTE]
>
>Cookies de terceros: los flujos de derechos de autenticación de Adobe Pass pueden fallar cuando las cookies de terceros están desactivadas.  Este problema solo aparece cuando se modifica la configuración del explorador. Para todos los exploradores admitidos, la autenticación de Adobe Pass debe funcionar con la configuración predeterminada.


## Requisitos de dispositivo para implementaciones de sin cliente (REST) {#general_clientless_reqs}


Cualquier dispositivo que vaya a consumir los servicios de autenticación de Adobe Pass a través de implementaciones sin cliente **debe ser capaz de**:

* Proporcione un ID de dispositivo único con hash. Si el dispositivo no proporciona un ID de dispositivo con hash único, debe poder mantener un ID único proporcionado por la autenticación de Adobe Pass. El dispositivo debe poder mantener el ID único de forma permanente en su almacenamiento local y proporcionarlo como ID del dispositivo cuando realice llamadas a las API de autenticación de Adobe Pass.
* Generar firmas digitales con el algoritmo HMAC-SHA1
* Establecer encabezados HTTP arbitrarios
* Consumir servicios web RESTful
* Analizar formatos de datos XML y JSON
* Envío de tráfico mediante HTTPS
* Gestión de códigos de error HTTP
