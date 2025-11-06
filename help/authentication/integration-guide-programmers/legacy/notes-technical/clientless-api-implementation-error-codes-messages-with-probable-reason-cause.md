---
title: 'Implementación de API sin cliente: códigos de error/mensajes con motivo/causa probable'
description: 'Implementación de API sin cliente: códigos de error/mensajes con motivo/causa probable'
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Implementación de API sin cliente (heredada): códigos de error/mensajes con motivo/causa probable {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

</br>


## Error: no autorizado

### Causas:

1. Falta el encabezado de autorización en el POST
1. Problema con el encabezado de autorización: compruebe si el tiempo de solicitud está en milisegundos.

## Error: SC 400 durante la autenticación

### Causas:

1. El servidor no encontró el código de registro, que se creó para un solicitante y un entorno específicos.
1. Podría estar teniendo problemas con scripts entre dominios
1. Se debe añadir una suplantación adecuada al archivo /etc/hosts

## Error: 400 Solicitud incorrecta

### Causas:

1. URL mal formada para POST/GET
1. SAMLAssertionParserException: la afirmación de SAML cifrada no se pudo descifrar al final de Adobe

## Error: 403 prohibido

### Causas:

1. Demasiadas solicitudes rápidas: una función de la administración de API para evitar ataques DoS.
2. Si utiliza un entorno de calidad previa, agregue la suplantación; de lo contrario, asegúrese de que la suplantación se haya eliminado del archivo /etc/hosts

## Error: No se puede iniciar sesión en la página de MVPD

### Causas:

1. El nombre de usuario y la contraseña no coinciden
2. Es posible que se haya deshabilitado el inicio de sesión
3. Compruebe si el inicio de sesión es para producción o ensayo


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
