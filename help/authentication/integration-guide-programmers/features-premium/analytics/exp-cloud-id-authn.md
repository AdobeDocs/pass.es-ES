---
title: Uso del ID de Experience Cloud en la autenticación de Adobe Pass
description: Uso del ID de Experience Cloud en la autenticación de Adobe Pass
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Uso del ID de Experience Cloud en la autenticación de Adobe Pass

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## ¿Qué es el ID de Experience Cloud y cómo obtenerlo? {#what-exp-cloud-id-obtain}

El ID del Experience Cloud (ECID, por sus siglas en inglés) es un ID único que Adobe Experience Cloud genera para cada usuario individual en su aplicación/sitio web. ECID se utiliza principalmente en todos los informes de Experience Cloud para vincular información sobre un usuario específico en varias aplicaciones o sitios web.

Si ya dispone de un sistema que proporciona un ID de visitante, debe utilizar el mismo ID para el ámbito de este documento.

Una forma de obtener el ECID es utilizar el servicio de ID de Experience Cloud. Puede utilizar el tipo de implementación que prefiera, ya sea en base a TDM, biblioteca JS, lado del servidor, integración directa o bibliotecas nativas para plataformas móviles. Para obtener una vista completa de los servicios, bibliotecas, SDK y guías de implementación disponibles, consulte: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>

## ¿Cuál es la ventaja de utilizar el ID de Experience Cloud en la autenticación de Adobe Pass? {#benefit-ex-cloud-id}

Si configura los SDK y la API de REST sin cliente para utilizar el ECID, más adelante podrá vincular los datos recopilados por la autenticación de Adobe Pass a las soluciones de Experience Cloud existentes. Esto le permitirá comprender mejor el recorrido y la experiencia de sus clientes en todas las soluciones proporcionadas por Adobe.

## ¿Cómo se utiliza el ID de Experience Cloud en la autenticación de Adobe Pass? {#how-to-ex-cloud-id-authn}

Después de obtener el ECID (explicado anteriormente), debe pasar esta información a nuestros SDK y a nuestra API de REST sin cliente. Esta información se pasará posteriormente a nuestros servidores en cada llamada de red que realice el SDK. El proceso de configuración es diferente para cada SDK de la siguiente manera:

### SDK DE JS {#js-sdk}

Para JavaScript, debe pasar el ECID en una asignación como el tercer parámetro a la llamada setRequestor.

**Ejemplo de uso:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### SDK de iOS/tvOS {#ios-sdk}

Para el SDK de iOS/tvOS hay un método específico denominado setOptions.

**Ejemplo de uso:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### SDK de Android/fireTV {#android-sdk}

Para el SDK de Android/fireTV, el mecanismo es similar al de iOS. El nombre del parámetro es diferente. La API está documentada aquí.

**Ejemplo de uso:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### API sin cliente {#clientless-api}

Cuando se usa Adobe Pass a través de su API de REST, el valor **ECID** debe enviarse **en todas las API** como un parámetro denominado **&#39;ap_vi&#39;**.

**Ejemplo de uso:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`
