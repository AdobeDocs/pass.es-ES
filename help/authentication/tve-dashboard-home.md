---
title: Tablero
description: Más información sobre la página de inicio del Tablero de TVE.
source-git-commit: 06c2e1e54515a2ec47722ba1f360467dadd1f73b
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# Tablero {#dashboard}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El **Tablero** La sección del panel izquierdo sirve como página principal del tablero de TVE de autenticación de Adobe Pass.

Hay dos secciones disponibles en la página principal:

* [Pantalla de bienvenida](#welcome-screen)
* [Estado de configuración](#configuration-status)

## Pantalla de bienvenida {#welcome}

En esta sección, puede acceder a la documentación pública directamente desde el mensaje de bienvenida y ver una instantánea de las configuraciones actuales.

* **Integraciones activas**: el número de integraciones activas en el entorno actual. Seleccionar **Ver más en la sección Integración** para acceder a información detallada en [Integraciones](tve-dashboard-integrations.md) sección.
* **Canales activos**: el número de canales activos en el entorno actual. Seleccionar **Ver más en la sección Canales** para acceder a información detallada en [Canales](tve-dashboard-channels.md) sección.
* **Actualizaciones de base de datos**: el número de cambios de configuración realizados en el entorno actual. Seleccionar **Ver más en la sección Registro de cambios** para acceder a información detallada en [Registro de cambios](tve-dashboard-changes-log.md) sección.
* **Panel de ESM**: Esté atento al próximo panel de ESM, que ofrece métricas detalladas sobre el uso de la propiedad en el entorno actual. Se podrá acceder a esta funcionalidad en futuras actualizaciones.

![Pantalla de bienvenida](assets/welcome-screen.png)

*Pantalla de bienvenida*

## Estado de configuración {#conf-status}

Esta sección presenta los 10 cambios de configuración más recientes que incluyen:

* **Descripción de cambios**: una breve descripción del cambio seleccionado por el usuario.
* **Empujado por**: la cuenta responsable del cambio.
* **Fecha push**: La fecha en la que se realizó el cambio.

![Estado de configuración de un registro de cambios](assets/configuration-status.png)

*Estado de configuración de un registro de cambios*

Para ver la lista completa de cambios, seleccione **Ver más en Registro de cambios** en la parte inferior derecha para ver la [Registro de cambios](tve-dashboard-changes-log.md) sección.
