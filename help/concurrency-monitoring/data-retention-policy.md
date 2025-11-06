---
title: Política de retención de datos
description: Política de retención de datos
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# Política de retención de datos {#data-retention-policy}

>[!WARNING]
>
>**Aviso:** El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


## Introducción {#introduction}

Adobe, como responsable del tratamiento de sus datos, debe tomar las medidas adecuadas para ayudar a sus clientes a cumplir con solicitudes de acceso, eliminación y otras solicitudes de particulares. La aplicación de directivas de eliminación adecuadas, seguras y oportunas es una parte importante del cumplimiento de esta obligación.

## Definiciones {#definitions}

Una directiva de retención de datos determina cuánto tiempo almacena Adobe los datos del cliente. La directiva de retención de datos predeterminada para la supervisión de concurrencia es de **25 meses**.

| Período de retención de datos | El período de retención de datos es el período de retención de datos predeterminado (25 meses). |
|---|---|
| **Ventana de retención de datos** | El período de retención de datos define los parámetros para los que se pueden consultar los datos e informar al respecto. El período de retención de datos se determina de la siguiente manera:<br/> *Fecha de inicio* = fecha actual - período de retención de datos <br/>*Fecha de finalización* = fecha actual |

## Recopilación de datos {#data-collection}

*Los datos del flujo de navegación* representan datos compartidos por clientes en los latidos de la sesión (por ejemplo, subjectID, mvpdName y metadatos). Se hace referencia a todos los campos de metadatos personalizados en los [atributos de metadatos estándar](/help/concurrency-monitoring/standard-metadata-attributes.md).

## Tipos de cliente {#customer-types}

### Clientes actuales {#current-customers}

A menos que el cliente compre extensiones de retención de datos, la monitorización de concurrencia cumplirá con los siguientes requisitos de retención de datos del cliente:

* Los *datos del flujo de navegación* recopilados por la Monitorización de simultaneidad se deben eliminar a más tardar **25 meses** a partir de la fecha de recopilación.

### Clientes finalizados {#terminated-customers}

Un cliente finalizado es un cliente que ha finalizado la relación con Adobe y ya no utiliza el control de concurrencia.

* Los *datos del flujo de navegación* recopilados por el monitoreo de concurrencia deben eliminarse en un plazo de **6 meses** a partir de la fecha de finalización del contrato del cliente.

## Eliminación de datos {#data-deletion}

Adobe se reserva el derecho de eliminar datos de fechas que no entren en el período de retención de datos sin opción de recuperarlos. Los datos de los clientes actuales deben eliminarse mensualmente de forma gradual.
