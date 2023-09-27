---
title: Iniciar autenticación
description: Iniciar autenticación
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# Iniciar autenticación {#initiate-authentication}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Extremos de API de REST {#clientless-endpoints}

&lt;reggie_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Producción - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Ensayo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Descripción {#description}

Inicia el proceso de autenticación al informar de un evento de selección de MVPD. Crea un registro en la base de datos de autenticación de Adobe Pass, que se concilia cuando se recibe una respuesta correcta de MVPD.



| Extremo | Llamado  </br>Por | Entrada   </br>Parámetros | HTTP  </br>Método | Respuesta | HTTP  </br>Respuesta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate | Módulo AuthN | 1. requestor_id (obligatorio)</br>2.  mso_id (obligatorio)</br>3.  reg_code (Obligatorio)</br>4.  domain_name (obligatorio)</br>5.  noflash=true -  </br>    (Obligatorio, Parámetro residual)</br>6.  no_iframe=true (obligatorio, parámetro residual)</br>7.  parámetros adicionales (opcional)</br>8.  redirect_url (obligatorio) | GET | La aplicación web Login se redirige a la página de inicio de sesión de MVPD. | 302 para implementaciones de redirección completas |

{style="table-layout:auto"}


| Parámetro de entrada | Descripción |
| --- | --- |
| requestor_id | El solicitante del programador para el que es válida esta operación. |
| mso_id | ID de MVPD para el que es válida esta operación. |
| reg_code | El código de registro generado por el servicio Reggie. |
| domain_name | El dominio de origen. |
| redirect_url | La URL de redireccionamiento de la aplicación web de inicio de sesión tras finalizar la autenticación. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Importante: Parámetros obligatorios -** Independientemente de la implementación del lado del cliente, todos los parámetros anteriores son obligatorios.
>
>
>Ejemplo:
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**Importante: Parámetros opcionales**
>
>La llamada también puede contener parámetros opcionales que habilitan otras funcionalidades como:
>
> * generic\_data: habilita el uso de [TempPass promocional](/help/authentication/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Notas** {#notes}

* El valor del `domain_name` El parámetro debe establecerse en uno de los nombres de dominio registrados con autenticación de Adobe Pass. Para obtener más información, consulte [Registro e inicialización](/help/authentication/programmer-overview.md).

* [Evite utilizar &#39;&amp;&#39;reg\_code en la solicitud /authentication (Nota técnica)](/help/authentication/clientless-avoid-using-reg-code-in-authenticate-request.md)

* El `redirect_url` el parámetro debe ser el último en orden

* El valor del `redirect_url` El parámetro debe tener codificación URL
