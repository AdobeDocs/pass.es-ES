---
title: Comprobar flujo de autenticación por aplicación web en segunda pantalla
description: Comprobar flujo de autenticación por aplicación web en segunda pantalla
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: 689e2f86550d9fa59337c15dd38767975a1d6d30
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 1%

---

# (Heredado) Comprobar flujo de autenticación por aplicación web de segunda pantalla {#check-authentication-flow-by-second-screen-web-app}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

>[!NOTE]
>
> La implementación de la API de REST está limitada por [Mecanismo de limitación](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Extremos de API de REST {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Producción: [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo: [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descripción {#description}

Esta API debe ser consumida por la aplicación web de inicio de sesión en la segunda pantalla para confirmar que la autenticación de Adobe Pass ha confirmado que el inicio de sesión se ha realizado correctamente desde MVPD. Se recomienda llamar a esta API antes de mostrar un mensaje de éxito al usuario final que le indique que continúe con los flujos de trabajo en la consola del dispositivo.


| Extremo | Llamado </br> por | Parámetros de entrada </br> | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{registration code} | Iniciar sesión en aplicación web | &#x200B;1.  código de registro </br> (componente de ruta de acceso)</br>2.  solicitante </br> (obligatorio) | GET | XML o JSON con detalles de error si no se ha realizado correctamente. | 200 - Correcto </br>403 - Prohibido |

</br>

| Parámetro de entrada | Descripción |
| ----------------- | --------------------------------------------------------------------------------------------- |
| código de registro | El valor del código de registro proporcionado por el usuario al principio del flujo de autenticación. |
| solicitante | Identificador de solicitante del programador para el que es válida esta operación. |


### Respuesta de ejemplo (en caso de error) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

**[Volver a la referencia de API de REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)**
