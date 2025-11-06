---
title: Uso del Experience Cloud ID en la autenticación de Adobe Pass
description: Uso del Experience Cloud ID en la autenticación de Adobe Pass
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# Uso del Experience Cloud ID en la autenticación de Adobe Pass

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## ¿Qué es Experience Cloud ID y cómo obtenerlo? {#what-exp-cloud-id-obtain}

Experience Cloud ID (ECID) es un ID único que Adobe Experience Cloud genera para cada usuario individual en su aplicación o sitio web. ECID se utiliza principalmente en todos los informes de Experience Cloud para vincular información sobre un usuario específico en varias aplicaciones o sitios web.

Si ya dispone de un sistema que proporciona un ID de visitante, debe utilizar el mismo ID para el ámbito de este documento.

Una forma de obtener el ECID es utilizar el servicio de Experience Cloud ID. Puede utilizar el tipo de implementación que prefiera, ya sea en base a TDM, biblioteca JS, lado del servidor, integración directa o bibliotecas nativas para plataformas móviles. Para obtener una vista completa de los servicios, bibliotecas, guías de implementación y de SDK disponibles, consulte: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html?lang=es>

## ¿Cuál es la ventaja de utilizar el Experience Cloud ID en la autenticación de Adobe Pass? {#benefit-ex-cloud-id}

Si configura los SDK y la API de REST sin cliente para utilizar el ECID, más adelante podrá vincular los datos recopilados por la autenticación de Adobe Pass a las soluciones de Experience Cloud existentes. Esto le permitirá comprender mejor el recorrido y la experiencia de sus clientes en todas las soluciones proporcionadas por Adobe.

## ¿Cómo se utiliza el Experience Cloud ID en la autenticación de Adobe Pass? {#how-to-ex-cloud-id-authn}

Después de obtener el ECID (explicado anteriormente), debe pasar esta información a nuestros SDK y a nuestra API de REST sin cliente. Esta información se pasará posteriormente a nuestros servidores en cada llamada de red que realice SDK. El proceso de configuración es diferente para cada SDK de la siguiente manera:

### JS SDK {#js-sdk}

Para JavaScript, debe pasar el ECID en una asignación como el tercer parámetro a la llamada setRequestor.

**Ejemplo de uso:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

Para iOS/tvOS SDK hay un método específico denominado setOptions.

**Ejemplo de uso:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Para Android/fireTV SDK, el mecanismo es similar al de iOS. El nombre del parámetro es diferente. La API está documentada aquí.

**Ejemplo de uso:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### API sin cliente {#clientless-api}

Cuando se usa Adobe Pass a través de su API REST v1, el valor **ECID** debe enviarse **en todas las API** como un parámetro denominado **&#39;ap_vi&#39;**.

**Ejemplo de uso:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### API DE REST V2 {#rest-api-v2}

Cuando se usa Adobe Pass a través de su API REST v2, el valor **ECID** debe enviarse **en todas las API** como un encabezado denominado **&#39;AP-Visitor-Identifier&#39;**.

**Ejemplo de uso:**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
Encabezados:\
`AP-Visitor-Identifier: THE_ECID_VALUE`

