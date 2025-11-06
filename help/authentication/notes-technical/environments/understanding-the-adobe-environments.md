---
title: Explicación de los entornos de Adobe
description: Explicación de los entornos de Adobe
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# Explicación de los entornos de Adobe {#understanding-the-adobe-environments}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

La documentación oficial que describe los entornos de Adobe está disponible [Configurando su entorno y probando en Pre-qual](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md):

Los entornos de Adobe, resumidos en pocas palabras:

Adobe tiene dos entornos: **Calificación previa** y **Versión**.

* En el entorno de precalificación, estamos preparando la nueva compilación para su lanzamiento.

* La versión actual se encuentra en el entorno de la versión.

Cada entorno tiene dos perfiles: **staging** y **production**.

* El perfil de ensayo se conecta al servidor de ensayo de MVPD
* El perfil de producción se conecta al perfil de producción de las MVPD.

El motivo de tener los dos perfiles es que en el perfil de ensayo estamos preparando nuevos socios para lanzarlos, y les gustaría probar el sistema con la próxima compilación (precalificación) o con la versión (más estable).

Si un socio desea probar la nueva versión, hay que realizar un par de pasos adicionales. Vea [Configuración de su entorno y pruebas en Pre-qual](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md).

Al seguir los pasos anteriores, se garantiza que la próxima versión se probará en el entorno de precalificación.
