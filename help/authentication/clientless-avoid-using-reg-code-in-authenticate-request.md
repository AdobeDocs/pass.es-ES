---
title: Evite utilizar '&'reg_code en la solicitud /authentication
description: Evite utilizar '&'reg_code en la solicitud /authentication
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# Evite utilizar &#39;&amp;&#39;reg_code en la solicitud /authentication {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>



## Problema

El explorador IE 9 interpreta &#39;\®&#39; como un comando especial y lo convierte a ®.

## Explicación

Si la variable `/authenticate` La solicitud se compone de la siguiente manera...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


... lo interpretará el navegador IE como se muestra a continuación y se enviará al Adobe en este formato:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


El solicitante\_id se interpretará como univision®\_code=EKAFMFI, ya que no hay &quot;&amp;&quot;, y el Adobe no encuentra un `regCode` parámetro con el que se asociará el token.  Existe la posibilidad de que el token de AuthN no se cree en absoluto, en cuyo caso `/checkauthn` las llamadas de no encontrarán ningún token.



## Solución

Una de las siguientes opciones debe resolver este problema:

1. Evite utilizar el `&reg_code` parámetro entre los demás parámetros de cadena de consulta.  En su lugar, muévalo al primer parámetro de cadena de consulta de la dirección URL de solicitud, haciendo que la dirección URL sea la siguiente:


       &lt;fqdn>authentication?reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   De este modo, la variable `&reg` param no se interpretará incorrectamente.

1. Normalizar `&reg_code` como usando `&amp;reg_code`.

1. El Adobe podría introducir una nueva función para enviar un código de error de nuevo a la segunda pantalla en respuesta a una llamada de autenticación, si fallaba la creación del token de AuthN.
