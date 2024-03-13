---
title: Proporcionar lista de MVPD
description: Proporcionar lista de MVPD
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---

# Proporcionar lista de MVPD {#provide-mvpd-list}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/throttling-mechanism.md)

## Extremos de API de REST {#clientless-endpoints}

&lt;reggie_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Devuelve la lista de MVPD configuradas para el solicitante.

| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/config/{requestorId}</br></br>Por ejemplo:</br></br>&lt;sp_fqdn>/api/v1/config/sampleRequestorId | Adobe Pass Authentication | 1. Solicitante</br>    (Componente Ruta)</br>_2.  deviceType (obsoleto)_ | GET | XML o JSON que contienen la lista de MVPD. | 200 |

{style="table-layout:auto"}


| Parámetro de entrada | Descripción |
| --------------- | ------------------------------------------------------------- |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |
| *deviceType* | Tipo de dispositivo. |

{style="table-layout:auto"}

### Respuesta de ejemplo {#sample-response}

Igual que la respuesta XML de MVPD existente al servlet /config

Nota: Todas las MVPD configuradas para utilizar el SSO de Platform tendrán las siguientes propiedades adicionales dentro de su nodo correspondiente (JSON/XML):

* **enablePlatformServices (booleano):** indicador que indica si este MVPD está integrado mediante el SSO de la plataforma
* **boardingStatus (cadena):** Indicador que indica si la MVPD es totalmente compatible con el SSO de la plataforma (COMPATIBLE) o si la MVPD solo aparece en el selector de plataformas (SELECTOR)
* **displayInPlatformPicker (booleano):** si este MVPD aparece en el selector de plataformas
* **platformMappingId (cadena):** el identificador de esta MVPD conocido por la plataforma
* **requiredMetadataFields (matriz de cadenas):** se espera que los campos de metadatos del usuario estén disponibles cuando el inicio de sesión se realice correctamente
