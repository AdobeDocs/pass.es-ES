---
title: Política de retención de datos
description: Política de retención de datos
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---


# Política de retención de datos {#data-retention-policy}

>[!WARNING]
>
>**Aviso:** El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


## Introducción {#introduction}

El Adobe, en su calidad de encargado del tratamiento de datos, debe tomar las medidas adecuadas para ayudar a sus clientes a cumplir con solicitudes de acceso, eliminación y otras solicitudes de particulares. La aplicación de directivas de eliminación adecuadas, seguras y oportunas es una parte importante del cumplimiento de esta obligación.

## Definiciones {#definitions}

Una directiva de retención de datos determina cuánto tiempo almacena el Adobe los datos del cliente. La política de retención de datos predeterminada para la monitorización de concurrencia es **25 meses**.

| Período de retención de datos | El período de retención de datos es el período de retención de datos predeterminado (25 meses). |
|---|---|
| **Ventana de retención de datos** | El período de retención de datos define los parámetros para los que se pueden consultar los datos e informar al respecto. El período de retención de datos se determina de la siguiente manera:<br/> *Fecha de inicio* = fecha actual - período de retención de datos <br/>*Fecha de finalización* = fecha actual |

## Recopilación de datos {#data-collection}

*Datos del flujo de navegación* representa los datos compartidos por los clientes en los latidos de la sesión (por ejemplo, subjectID, mvpdName y metadata). Se hace referencia a todos los campos de metadatos personalizados en la variable [Atributos de metadatos estándar](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Tipos de cliente {#customer-types}

### Clientes actuales {#current-customers}

A menos que el cliente compre extensiones de retención de datos, la monitorización de concurrencia cumplirá con los siguientes requisitos de retención de datos del cliente:

* *Datos del flujo de navegación* recopilado por la Monitorización de concurrencia debe eliminarse antes de **25 meses** desde la fecha de recopilación.

### Clientes finalizados {#terminated-customers}

Un cliente terminado es un cliente que ha finalizado la relación con el Adobe y ya no utiliza la supervisión de concurrencia.

* *Datos del flujo de navegación* recopilado por el control de concurrencia debe eliminarse en **6 meses** desde la fecha de finalización del contrato del cliente.

## Eliminación de datos {#data-deletion}

El Adobe se reserva el derecho de borrar los datos de las fechas posteriores al período de retención de datos sin opción de recuperarlos. Los datos de los clientes actuales deben eliminarse mensualmente de forma gradual.

