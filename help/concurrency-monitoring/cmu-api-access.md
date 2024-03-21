---
title: Acceso a API de CMU
description: Acceso a API de CMU
source-git-commit: 30631ac006b7944cb2eb8996c2c165343b6be5fe
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 0%

---

# Acceso a API de uso de supervisión de concurrencia {#cmu-api-usage-access}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado. Póngase en contacto con el representante del Adobe para preguntas sobre disponibilidad.

## Introducción al procedimiento de acceso {#api-access-procedure-overview}

Hemos actualizado el acceso a los informes de CMU para que sea compatible con el Protocolo de registro dinámico de clientes OAuth 2.0.
Se implementa un servidor de autorización OAuth 2.0 personalizado para satisfacer las necesidades de la aplicación de Monitorización de concurrencia. \
Para que las aplicaciones cliente utilicen la autorización de OAuth 2.0, el servidor debe registrarse dinámicamente para obtener información específica (credenciales del cliente) para poder interactuar con ella. Como parte del proceso de registro, el cliente debe presentar un conjunto de metadatos integrados al extremo de registro del cliente.
Estos metadatos se comunican como una declaración de software, que contiene un &quot;software_id&quot; para permitir que nuestro servidor de autorización correlacione diferentes instancias de una aplicación utilizando la misma declaración de software.
Una instrucción de software es un token web JSON (JWT) que afirma valores de metadatos sobre el software cliente como un paquete. Cuando se presenta al servidor de autorización como parte de una solicitud de registro de cliente, la instrucción de software debe estar firmada digitalmente o ser editada en MAC mediante la firma web JSON (JWS). \
Puede encontrar una explicación más detallada sobre las declaraciones de software y cómo funcionan en la documentación oficial  <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a>.
Siga los pasos de las secciones siguientes para obtener acceso.

## Pasos del procedimiento de acceso {#access-procedure-steps}

1. Tener una aplicación registrada en el servidor de Adobe Pass DCR. Para realizar este paso, póngase en contacto con nuestro [Equipo de soporte](mailto:tve-support@adobe.com).
2. Obtener la declaración del software
   1. Ir al panel de TVE <a href="https://console-preprod.auth.adobe.com/#!/" target="_blank"> Pre-Prod </a> o <a href="https://console.auth.adobe.com/" target="_blank">PROD</a>
   2. Seleccionar programador
   3. Vaya a la pestaña Aplicaciones
   4. Seleccionar aplicación
   5. Haga clic en DownLoad Software Statement para obtener un archivo similar a la siguiente captura
      <figure>
          <img src="assets/software_statement_1_download.png"
               alt="Descargar declaración de software">
       </figure>
      <figure>
          <img src="assets/software_statement_2.png"
               alt="Ejemplo de declaración de software">
       </figure>

3. Obtener token de acceso
   1. Obtenga las credenciales del cliente utilizando la instrucción de software obtenida anteriormente y realizando la llamada siguiente. De este modo se obtiene un par client_id - client_secret que se puede utilizar para obtener el token de acceso.
      *Este paso no se debe realizar cada vez. Solo debe hacerse de nuevo cuando caduquen las credenciales.*
      <figure>
          <img src="assets/dcr_request_1_get_client_credentials.png"
               alt="Obtener credenciales del cliente">
       </figure>

   2. Obtenga el token de acceso mediante la siguiente llamada. Utilice este token de acceso para llamar a cualquier API de CMU hasta que el token caduque.
      *Este paso solo debe realizarse si caducó el último token generado.*
      <figure>
          <img src="assets/dcr_get_access_token_call.png"
               alt="Obtener token de acceso">
       </figure>

4. Llamar a la API de CMU: consulte la información relacionada a continuación.
   <figure>
          <img src="assets/call_cmu_reports_sample.png"
               alt="Llamar a API de CMU">
       </figure>

## Información relacionada {#related-information}

* [Información general de CMU](/help/concurrency-monitoring/cm-usage-reports.md)
* [API de CMU](/help/concurrency-monitoring/cmu-api.md)
