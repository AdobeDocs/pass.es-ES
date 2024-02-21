---
title: 'Implementación de API sin cliente: códigos Error / mensajes con motivo probable / causa'
description: 'Implementación de API sin cliente: códigos Error / mensajes con motivo probable / causa'
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# Implementación de API sin cliente: códigos Error / mensajes con motivo probable / causa {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>Los contenido de este Página se proporcionan únicamente con fines informativos. El uso de esta API requiere una licencia vigente de Adobe Systems. No se permite el uso no autorizado.

</br>


## Error: No autorizado

### Causas:

1. Falta el encabezado de autorización en el POST
1. Problema con el encabezado de autorización: compruebe si solicitud tiempo es en milisegundos.

## Error: SC 400 durante la autenticación

### Causas:

1. El servidor no encontró el código registro, que fue creado para un solicitante y un entorno específicos.
1. Podría estar teniendo problemas de script entre dominios
1. Se debe agregar la suplantación de identidad adecuada al archivo /etc/hosts

## Error: 400 Solicitud errónea

### Causas:

1. URL con formato incorrecto para POST/GET
1. SAMLAssertionParserException: la afirmación SAML cifrada no se pudo descifrar al final de Adobe Systems

## Error: 403 Prohibido

### Causas:

1. Demasiadas solicitudes rápidas: una característica de la administración de API para evitar ataques DoS.
2. Si utiliza prequal entorno agregue suplantación de identidad, de lo contrario, asegúrese de que la suplantación de identidad se haya eliminado del archivo /etc/hosts.

## Error: No se puede iniciar sesión en MVPD Página

### Causas:

1. El nombre de usuario y contraseña no coinciden
2. Es posible que se haya deshabilitado el inicio de sesión
3. Compruebe si el inicio de sesión es para producción o ensayo


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
