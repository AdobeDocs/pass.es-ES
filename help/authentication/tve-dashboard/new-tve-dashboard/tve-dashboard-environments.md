---
title: Entornos del panel de TVE
description: Comprender el uso y el funcionamiento de los distintos entornos en el Tablero de TVE.
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Entornos {#environments}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El Tablero de TVE proporciona diferentes entornos personalizados para cumplir propósitos específicos dentro de la Autenticación de Adobe Pass. Hay dos entornos principales:

* **Prequal**: el entorno de precalificación sirve como campo de prueba para preparar y probar nuevas compilaciones antes de su implementación en producción.

* **Versión**: El entorno de la versión aloja las compilaciones finalizadas y probadas para producción.

Dentro de cada entorno, hay dos perfiles diferentes:

* **Ensayo**: el perfil de ensayo se conecta al servidor de ensayo de MVPD para probar y validar las integraciones antes de activarse.

* **Producción**: el perfil de producción se conecta al perfil de producción de MVPD para las actividades de producción reales.

## Casos de uso

Los entornos del Tablero de TVE sirven para varios casos de uso a lo largo del ciclo de vida de la aplicación. Estos entornos le permiten lo siguiente:

### Ensayo de preigualación

* Valide las nuevas funciones no publicadas del servidor de autenticación de Adobe Pass mediante los extremos de ensayo de MVPD.
* Utilizado principalmente por el equipo de producto de autenticación de Adobe Pass para añadir y validar nuevas integraciones de MVPD.

### Producción de preigualación

* Valide las nuevas funciones o configuraciones no publicadas del servidor de autenticación de Adobe Pass mediante los extremos de producción de MVPD.
* Validar nuevas versiones de aplicaciones para cada canal utilizando los extremos de producción de MVPD.
* Valide cada cambio de configuración antes de colocarlo en producción.

### Ensayo de lanzamiento

* Valide las nuevas versiones de las aplicaciones para cada canal mediante los extremos de ensayo de MVPD.
* Realizar pruebas de rendimiento o capacidad dentro de este entorno.

### Producción de versiones

* Representa el entorno en directo con la última versión de Adobe Pass disponible generalmente para todos los usuarios finales.
* Mantiene la estabilidad del código y la configuración y refleja inmediatamente los cambios de configuración en la aplicación del usuario final.

## Cambiar entornos {#switch-environments}

Siga los pasos para cambiar entre los entornos del tablero de TVE de autenticación de Adobe Pass.

1. Inicie sesión con sus credenciales de programador.

1. Seleccione el entorno de ensayo o producción necesario en el menú desplegable **Entorno** que se encuentra en la parte superior del panel izquierdo.

   ![Menú desplegable de entornos del panel de TVE](../../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Menú desplegable del entorno del Panel de TVE de autenticación de Adobe Pass*

>[!NOTE]
>
> Las configuraciones pueden variar en cada entorno según la configuración.
