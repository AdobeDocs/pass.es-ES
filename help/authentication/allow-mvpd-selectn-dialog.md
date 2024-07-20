---
title: Permitir MVPD en el cuadro de diálogo de selección
description: Permitir MVPD en el cuadro de diálogo de selección
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 0%

---

# Permitir MVPD en el cuadro de diálogo de selección {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Problema {#issue}

Es posible que el programador desee probar o comprobar la experiencia del usuario con las nuevas integraciones de MVPD antes de hacerlas públicas a los usuarios finales.

## Solución {#solution}

En la llamada de retorno `displayProviderDialog()`, la autenticación de Adobe Pass devuelve todas las MVPD integradas con el programador seleccionado (ID del solicitante). Sin embargo, el programador puede aplicar un filtro en la matriz de retorno de MVPD y mostrar solo las que están en ambas listas.

## Ejemplo {#example}

En este ejemplo se muestra cómo mostrar sólo CableCompany_1 y CableCompany_2 dentro del cuadro de diálogo selector de MVPD y cómo no mostrar CableCompany_NewIntegration.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
