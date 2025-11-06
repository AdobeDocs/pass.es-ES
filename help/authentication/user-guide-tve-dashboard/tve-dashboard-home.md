---
title: Tablero
description: Más información sobre la página de inicio del Tablero de TVE.
exl-id: 3073cd86-89f8-4c65-996b-24edda24f25b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---

# Tablero {#dashboard}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La sección **Tablero** del panel izquierdo sirve como página principal del Tablero de TVE de autenticación de Adobe Pass.

Hay dos secciones disponibles en la página principal:

* [Pantalla de bienvenida](#welcome-screen)
* [Estado de configuración](#configuration-status)

## Pantalla de bienvenida {#welcome}

En esta sección, puede acceder a la documentación pública directamente desde el mensaje de bienvenida y ver una instantánea de las configuraciones actuales.

* **Integraciones activas**: número de integraciones activas en el entorno actual. Seleccione **Ver más en la sección de integración** para obtener acceso a información detallada en la sección [Integraciones](tve-dashboard-integrations.md).
* **Canales activos**: El número de canales activos en el entorno actual. Seleccione **Ver más en la sección Canales** para obtener acceso a información detallada en la sección [Canales](tve-dashboard-channels.md).
* **Actualizaciones de la base de datos**: El número de cambios de configuración realizados en el entorno actual. Seleccione **Ver más en la sección Registro de cambios** para obtener acceso a información detallada en la sección [Registro de cambios](tve-dashboard-changes-log.md).
* **Panel de ESM**: Esté atento al próximo Panel de ESM, que ofrece métricas detalladas sobre el uso de propiedades en el entorno actual. Se podrá acceder a esta funcionalidad en futuras actualizaciones.

![Pantalla de bienvenida](../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-welcome-panel-view.png)

*Pantalla de bienvenida*

## Estado de configuración {#conf-status}

Esta sección presenta los 10 cambios de configuración más recientes que incluyen:

* **Descripción de los cambios**: Una breve descripción del cambio seleccionado por el usuario.
* **Insertado por**: La cuenta responsable del cambio.
* **Fecha push**: La fecha en la que se realizó el cambio.

![Estado de configuración de un registro de cambios](../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-configuration-status-panel-view.png)

*Estado de configuración de un registro de cambios*

Para ver la lista completa de cambios, seleccione **Ver más en el registro de cambios** en la parte inferior derecha para ver la sección [Registro de cambios](tve-dashboard-changes-log.md).
