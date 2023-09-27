---
title: Glosario
description: Glosario de términos de supervisión de concurrencia
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# Glosario {#glossary}

## ID de cuenta {#accid-defn}

* Cuenta MVPD de un suscriptor, que generalmente corresponde a la cuenta de facturación real. La MVPD debe identificar esta cuenta en su propio sistema.

## Acción {#action-defn}

* El tipo de acceso que solicita el sujeto; los valores posibles para CM son ***iniciar*** o ***continuar*** una sesión de streaming.

## Flujo activo {#active-stream-defn}

* Un flujo que ha recibido al menos 1 evento (latido) en los últimos 90 segundos.

* ***Nota:*** Si el último evento de la secuencia es del tipo parada (`?event=stop`), no se va a contar. Se trata de una optimización que permite a un reproductor cerrar explícitamente un flujo para que ya no se considere &quot;activo&quot;.

## Aplicación {#application-defn}

* Desarrollado por el inquilino para el acceso al contenido de vídeo
* Toma y aplica decisiones sobre el acceso al contenido en función de la información proporcionada por el Servicio de monitoreo de concurrencia (esto es válido en el [Punto de información de política](/help/concurrency-monitoring/policy-info-pt-versionone.md) case)
* Tendrá un único **ID de aplicación** proporcionadas por el Adobe.

## Servicio de supervisión de concurrencia {#cm-service-defn}

* Actúa como un sistema de monitorización para los suscriptores, apoyando a los MVPD y programadores en sus requisitos de aplicación de políticas entre aplicaciones.
* Recibe latidos que indican actividad de flujo.
* Actúa como _Punto de decisión de política_ evaluando las solicitudes de autorización en función de la actividad del usuario y proporcionando una respuesta de permitir/denegar.
* Actúa como _Punto de información de política_ al informar del número de flujos activos (y metadatos de flujo adicionales) para un suscriptor.

## Entorno {#env-defn}

* Información adicional relevante para la solicitud, como los ajustes de configuración o la hora del sistema.

## MVPD {#mvpd-defn}

* Distribuidor de programación de vídeo multicanal.
* Actúa como proveedor de autenticación y autorización, pero también puede ser proveedor de servicios o de contenido.

## Política {#policy-defn}

* El concepto de control de acceso principal en CM se define como un destino y una o más reglas agrupadas con un nombre único.

## Punto de administración de directivas (PAP) {#policy-admin-pt-defn}

* Punto que administra las directivas de autorización de acceso. Esto no se documentará aquí, pero finalmente vamos a proporcionar una consola de autoservicio para que los clientes administren sus políticas de acceso.

## Punto de decisión de política (PDP) {#policy-decn-pt-defn}

* Punto que evalúa las solicitudes de acceso con respecto a las políticas de autorización antes de emitir decisiones de acceso.

## Punto de aplicación de políticas (PEP) {#policy-ef-pt-defn}

* Punto que intercepta la solicitud de acceso del usuario a un recurso, realiza una solicitud de decisión al PDP y aplica esa decisión a la solicitud. Actualmente, esta es la aplicación cliente y no hay ningún plan para transferir esta función al control de concurrencia.

## Punto de información de la política (PIP) {#policy-info-pt-defn}

* Una fuente de valores de atributo. La Monitorización de concurrencia actúa como punto de información al proporcionar:
   * metadatos de flujo de paso a través.
   * métricas de actividad relativas a flujos simultáneos.

## Programador {#programmer-defn}

* Actúa como proveedor de servicios y contenido.
* Se basa en la aplicación cliente implementada que se integra con el servicio de supervisión de concurrencia para aplicar las políticas de seguridad definidas en función de los datos de servicio mencionados anteriormente.
* Necesita admitir la MVPD para recopilar la actividad del suscriptor y aplicar las reglas de limitación en sus propiedades.
* También puede estar interesado en limitar el acceso simultáneo a su contenido en todos los portales de destino, como regla independiente.

  *P: ¿Por qué al programador y no al solicitante les gusta el ID en el resto de la autenticación de Adobe Pass?*

  *R: El motivo es permitir que los programadores utilicen este parámetro de forma flexible para pasar o aislar datos entre sus propiedades según sus casos de uso.*

## Recurso {#resource-defn}

* El contenido real que un sujeto desea consumir. El recurso generalmente lleva atributos relacionados con el propietario (editor) y también puede proporcionar información adicional como género o lo que sea.

## Regla {#rule-defn}

* Una función booleana que se evalúa frente a un flujo en particular y la actividad del usuario relevante para determinar si el acceso debe permitirse o denegarse para ese flujo.

## Sesión de streaming {#streaming-session-defn}

* Sesión de vídeo de flujo continuo iniciada por un sujeto para consumir un recurso específico.

## Asunto {#subj-defn}

* El consumidor del contenido (vídeo) a través de Internet. Estamos evitando deliberadamente el término _**usuario**_, ya que la Monitorización de la concurrencia generalmente trata con los ID de cuenta de MVPD (que implican a varios usuarios reales que comparten el mismo contrato, por ejemplo, miembros de la familia de un hogar).

* Para cada flujo, el asunto se puede mejorar con atributos relacionados con la persona real que utiliza el servicio, su dispositivo conectado a la red, etc.

## Suscriptor {#subscriber-defn}

* El cliente pagador de una MVPD o una persona que comparte las credenciales de un cliente pagador
* El Servicio de supervisión de concurrencia puede impedir que vea contenido mediante la aplicación cliente que utiliza el servicio mencionado anteriormente.
* En el mejor de los casos, nunca se da cuenta de la existencia del Servicio de Monitoreo de Concurrencia

## Target {#target-defn}

* Un predicado de flujo que devolverá si la regla es aplicable a un flujo determinado. El destino implícito en CM será cualquier secuencia creada por una aplicación que haga referencia a la directiva en cuestión. Además, se pueden añadir condiciones de valor de atributo para ajustar el filtro de actividad antes de aplicar las reglas.

## Inquilino {#tenant-defn}

* Una organización del cliente de Monitorización de concurrencia.
