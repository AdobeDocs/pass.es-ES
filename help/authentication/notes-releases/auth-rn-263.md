---
title: Notas de la versión de Adobe Pass Authentication 2.63
description: Notas de la versión de Adobe Pass Authentication 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Notas de la versión de Adobe Pass Authentication 2.63 {#pt-authn-263-rn}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

En esta página se describen las nuevas funciones, los cambios y los problemas conocidos de esta versión:

## Lado del servidor y clientes web {#server-side-web-clients-263}

* [Número de compilación](#build-number)
* [Nuevas funciones](#new-features)

### Número de compilación {#build-number-263}

Autenticación de Adobe Pass: adobe-pass-**2.63**
Fecha de versión: **20/09/2022 - 22/09/2022**

### Nuevas funciones {#new-features-263}

#### Mejora del mecanismo de identificación de plataformas {#pf-identification-mech}

* A partir de esta versión, mejoramos el mecanismo utilizado para identificar un dispositivo y ya no dependerá de la implementación del lado del cliente. Esto proporcionará una granularidad más precisa al aplicar reglas empresariales a nivel de plataforma y una mejor comprensión de los valores de tráfico en los informes de ESM.

* Próximamente habrá una nueva versión de ESM, con informes nuevos y mejorados que exponen los campos relacionados con la plataforma.

* Para obtener más información sobre los cambios planificados, póngase en contacto con su TAM.

#### Autodegradación de MVPD {#mvpd-self-degradation}

Esta función proporciona a las MVPD la capacidad de omitir temporalmente sus propios extremos de autenticación y autorización para situaciones de alto tráfico cuando la carga en esos respectivos extremos se vuelve demasiado alta.


#### Añadir ID proxy en el encabezado de las llamadas de autorización {#add-proxied-id}

Esta función añade el ID de una MVPD proxy de Synacor en el encabezado de la llamada de autorización. Esto permite a Synacor configurar reglas de negocio para cada proxy individual (p. ej. enrutamiento a diferentes dominios (por MVPD proxy).


#### Tablero de TVE {#tve-dashboard}

En esta versión, se ha corregido un problema por el cual los valores de authN o authZ TTL establecidos en el nivel de MVPD no se calculaban correctamente en los informes de configuración.


#### SDK para JavaScript 4.6.0 {#js-sdk}

* Se ha eliminado el uso de la función `eval`, por lo que el SDK es compatible con la directiva de seguridad de contenido.
* Se ha corregido un problema que impedía que el flujo de autenticación finalizara correctamente cuando una aplicación de socio borraba explícitamente el almacenamiento local del explorador.
