---
title: Evitar que las MVPD aparezcan en el cuadro de diálogo de selección
description: Evitar que las MVPD aparezcan en el cuadro de diálogo de selección
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 0%

---

# (Heredado) Evitar que las MVPD aparezcan en el cuadro de diálogo de selección

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

## Problema {#issue-prevent-mvpd-sel-dialog}

Debe evitar que las MVPD específicas (&quot;lista de bloqueados&quot;) aparezcan en el selector de MVPD.


## Solución {#solution-prevent-mvpd-sel-dialog}

La solución es incluir en la lista de bloqueados cuando se llama a `displayProviderDialog()`.

Por ejemplo, si desea que CableCompany_1 y CableCompany_2 no se muestren dentro del selector de MVPD, debe hacer algo similar a lo que se muestra en el siguiente ejemplo.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```

<!--
**Related Information**

* [Allow MVPDs in the Selection Dialog](/help/authentication/allow-mvpd-selectn-dialog.md)
* **Code samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
