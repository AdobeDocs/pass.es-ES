---
title: 'Encabezado: X-Device-Info'
description: 'API de REST V2: encabezado: X-Device-Info'
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Encabezado: X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Información general {#overview}

El encabezado de la solicitud <b>X-Device-Info</b> contiene la información del cliente (dispositivo, conexión y aplicación) relacionada con el dispositivo de flujo continuo real.

## Sintaxis {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;device_information&gt;</td>
   </tr>
   <tr>
      <td>Tipo de encabezado</td>
      <td>Encabezado de solicitud</td>
   </tr>
   <tr>
      <td>Standard</td>
      <td>No</td>
   </tr>
</table>

## Directivas {#directives}

<b>&lt;device_information></b>

El valor `Base64-encoded` del elemento JSON que contiene al menos los atributos marcados como requeridos por la siguiente tabla.

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Clave</th>
        <th style="background-color: #EFF2F7;">Descripción</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Presencia</th>
        <th style="background-color: #EFF2F7;">Valores posibles</th>
    </tr>
    <tr>
        <td>primaryHardwareType</td>
        <td>El tipo de hardware principal del dispositivo.</td>
        <td></td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Cámara</li>
                <li>DataCollectionTerminal</li>
                <li>Escritorio</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>Gafas</li>
                <li>MediaPlayer</li>
                <li>MobilePhone</li>
                <li>PaymentTerminal</li>
                <li>PluginModem</li>
                <li>SetTopBox</li>
                <li>TV</li>
                <li>Tableta</li>
                <li>WirelessHotspot</li>
                <li>Reloj de pulsera</li>
                <li>Desconocido</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>model</td>
        <td>El nombre del modelo del dispositivo.</td>
        <td><i>obligatorio</i></td>
        <td>por ejemplo, iPhone, SM-G930V, AppleTV, etc.</td>
    </tr>
    <tr>
        <td>version</td>
        <td>La versión del dispositivo.</td>
        <td></td>
        <td>p. ej., 2.0.1, etc.</td>
    </tr>
    <tr>
        <td>fabricante</td>
        <td>La empresa u organización de fabricación del dispositivo.</td>
        <td></td>
        <td>Por ejemplo: Samsung, LG, ZTE, Huawei, Motorola, Apple, etc.</td>
    </tr>
    <tr>
        <td>vendedor</td>
        <td>La empresa u organización vendedora del dispositivo.</td>
        <td></td>
        <td>por ejemplo, Apple, Samsung, LG, Google, etc.</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>El nombre del sistema operativo (SO) del dispositivo.</td>
        <td><i>obligatorio</i></td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Android</li>
                <li>SO CHROME</li>
                <li>Linux</li>
                <li>SO MAC</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osFamily</td>
        <td>El nombre del grupo del sistema operativo (SO) del dispositivo.</td>
        <td></td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation OS</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVendor</td>
        <td>El proveedor del sistema operativo (SO) del dispositivo.</td>
        <td></td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Proyecto Tizen</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVersion</td>
        <td>Versión del sistema operativo (SO) del dispositivo.</td>
        <td></td>
        <td>p. ej. 10.2, 9.0.1, etc.</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>El nombre del explorador.</td>
        <td></td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Navegador Android</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Navegador Symbian</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVendor</td>
        <td>La empresa u organización que crea el explorador.</td>
        <td></td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVersion</td>
        <td>La versión del explorador del dispositivo.</td>
        <td></td>
        <td>p. ej. 60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>El agente de usuario del dispositivo.</td>
        <td></td>
        <td>Por ejemplo, Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, como Gecko) Versión/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>Ancho de pantalla físico del dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>Altura física de la pantalla del dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>La densidad de píxeles de la pantalla física del dispositivo.</td>
        <td></td>
        <td>p.ej. 294</td>
    </tr>
    <tr>
        <td>diagonalScreenSize</td>
        <td>Dimensión diagonal de la pantalla física del dispositivo en pulgadas.</td>
        <td></td>
        <td>p. ej. 5.5, 10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>IP del dispositivo utilizada para enviar solicitudes HTTP.</td>
        <td></td>
        <td>p. ej., 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>Puerto del dispositivo utilizado para enviar solicitudes HTTP.</td>
        <td></td>
        <td>p. ej. 53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>Tipo de conexión de red.</td>
        <td></td>
        <td>por ejemplo, WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>Estado de seguridad de la conexión de red.</td>
        <td></td>
        <td>
            Los valores están restringidos:
            <ul>
                <li>true: en el caso de una red segura</li>
                <li>false: en el caso de un punto interactivo público</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>El identificador único de la aplicación.</td>
        <td></td>
        <td>Por ejemplo, CNN</td>
    </tr>
</table>


## Ejemplos {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```
