---
title: Iniciar autenticación
description: Iniciar autenticación
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# (Heredado) Iniciar autenticación {#initiate-authentication}

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

Inicia el proceso de autenticación al informar de un evento de selección de MVPD. Crea un registro en la base de datos de autenticación de Adobe Pass, que se concilia cuando se recibe una respuesta correcta de MVPD.



| Extremo | Llamado </br> por | Entrada   </br>Parámetros | Método HTTP </br> | Respuesta | Respuesta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authentication | Módulo AuthN | &#x200B;1. requestor_id (obligatorio)</br>2.  mso_id (obligatorio)</br>3.  reg_code (obligatorio)</br>4.  nombre_dominio (obligatorio)</br>5.  noflash=true - </br>    (obligatorio, parámetro residual)</br>6.  no_iframe=true (obligatorio, parámetro residual)</br>7.  parámetros adicionales (opcional)</br>8.  redirect_url (obligatorio) | GET | La aplicación web de inicio de sesión se redirige a la página de inicio de sesión de MVPD. | 302 para implementaciones de redirección completas |

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
>**Importante: parámetros obligatorios -** Independientemente de la implementación del lado del cliente, todos los parámetros anteriores son obligatorios.
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
> * generic\_data: habilita el uso de [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Notas** {#notes}

* El valor del parámetro `domain_name` debe establecerse en uno de los nombres de dominio registrados con autenticación de Adobe Pass.

* [Evite utilizar &#39;&amp;&#39;reg\_code en la solicitud /authentication (Nota técnica)](/help/authentication/integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* El parámetro `redirect_url` debe ser el último en orden

* El valor del parámetro `redirect_url` debe tener codificación de dirección URL
