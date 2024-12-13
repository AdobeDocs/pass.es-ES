---
title: Flujo de API sin cliente en ausencia de ID de dispositivo
description: Flujo de API sin cliente en ausencia de ID de dispositivo
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# (Heredado) Flujo de API sin cliente en ausencia de ID de dispositivo {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>


## Problema

No todas las aplicaciones de dispositivos inteligentes podrán proporcionar un ID de dispositivo único.  Dado que deviceId es un parámetro obligatorio, el servicio devuelve un error 400 si no se pasa.


## Solución temporal/solución alternativa

Para clientes sin ID de dispositivo:

1. Llamar al servicio de código de registro la primera vez con `deviceId=dummy`
1. En la respuesta, extraiga el UUID. El UUID está disponible en el elemento &quot;id&quot; de la respuesta del código de registro (formatos de respuesta XML y JSON).
1. Vuelva a llamar al servicio de registro. Esta vez, pase `deviceId=<uuid obtained in step #2>`
1. Mostrar el código de registro obtenido en el paso 3 en la interfaz de usuario de la consola


Una vez completados estos pasos, la autenticación de Adobe Pass utilizará el UUID como ID del dispositivo. Almacene este ID de dispositivo (UUID) en el almacenamiento local del dispositivo. En caso de que el usuario genere un nuevo código de registro, debe volver a ejecutar los pasos del 1 al 4 y, a continuación, reemplazar el ID de dispositivo (UUID) almacenado anteriormente por el nuevo.



## Solución permanente

El Adobe cambiará esto en una versión futura, al hacer que `deviceId` sea una carga útil opcional al crear el código de registro y al usar UUID como clave de token en lugar de `deviceId`, cuando `deviceId` no está presente.

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
