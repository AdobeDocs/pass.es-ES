---
title: Evite utilizar '&'reg_code en la solicitud /authentication
description: Evite utilizar '&'reg_code en la solicitud /authentication
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# (Heredado) Evite utilizar &#39;&amp;&#39;reg_code en la solicitud /authentication {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Asegúrese de mantenerse informado sobre los últimos anuncios de productos de autenticación de Adobe Pass y las escalas de tiempo de retirada de servicio agregadas en la página [Anuncios de productos](/help/authentication/product-announcements.md).

</br>



## Problema

El navegador IE 9 interpreta &#39;\&amp;reg&#39; como un comando especial y lo convierte a ®.

## Explicación

Si la solicitud `/authenticate` está compuesta de la siguiente manera...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


... será interpretado por el navegador IE como se muestra a continuación y se enviará a Adobe en este formato:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


El solicitante\_id se interpretará como univision®\_code=EKAFMFI, ya que no hay &#39;&amp;&#39;, y Adobe no encontrará un parámetro `regCode` al que asociar el token.  Existe la posibilidad de que no se cree el token de AuthN, en cuyo caso las llamadas a `/checkauthn` no encontrarán ningún token.



## Solución

Una de las siguientes opciones debe resolver este problema:

1. Evite utilizar el parámetro `&reg_code` entre los demás parámetros de cadena de consulta.  En su lugar, muévalo al primer parámetro de cadena de consulta de la dirección URL de solicitud, haciendo que la dirección URL sea la siguiente:


       &lt;FQDN>authentication?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   De este modo, el parámetro `&reg` no se interpretará incorrectamente.

1. Normalizar `&reg_code` como si usara `&amp;reg_code`.

1. Adobe podría introducir una nueva función para enviar un código de error de nuevo a la segunda pantalla en respuesta a una llamada de autenticación, si fallaba la creación del token de AuthN.
