---
title: Configurar el entorno y realizar pruebas en la calidad previa
description: Configurar el entorno y realizar pruebas en la calidad previa
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# Configurar el entorno y realizar pruebas en la calidad previa{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El propósito de esta nota técnica es ayudar a nuestros socios a configurar su entorno e iniciar la prueba de una nueva compilación implementada en el entorno de precalificación de Adobe.

Dado que hay dos variantes de compilación: ***production*** y ***staging***, en este documento nos centraremos en la configuración de producción con la mención de que todos los pasos son iguales para el ensayo; solo las direcciones URL son diferentes.

Los pasos 1 y 2 configuran el entorno de prueba en una de las máquinas de prueba, el paso 3 es una verificación del flujo básico y los pasos 4 y 5 presentan algunas directrices de prueba.

>[!IMPORTANT]
>
> Es muy importante ejecutar los pasos 1 y 2 cada vez que desee cambiar el entorno de prueba (pasando del ensayo al perfil de producción o al contrario)


## PASO 1. Resolver el paso del dominio a una dirección IP {#resolving-pass-domain-to-an-ip}

* Para buscar una IP del equilibrador de carga que se pueda usar para la suplantación, ejecute el siguiente comando:

* **En Windows**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* **En Linux/Mac**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>Los dominios excluidos de la respuesta porque no son relevantes y podrían diferir de un usuario a otro.

>[!IMPORTANT]
>
> Estas direcciones IP pueden cambiar en el futuro y es posible que no sean iguales para los usuarios de diferentes regiones geográficas.


## PASO 2.  Suplantación del entorno de precalificación para que sea producción {#spoofing-the-prequalification-environment}

* Edite el archivo *c:\\windows\\System32\\drivers\\etc\\hosts* (en Windows) o el archivo */etc/hosts* (en Macintosh/Linux/Android) y agregue lo siguiente:

* Perfil de producción de parodia
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**Suplantación de identidad en Android:** Para suplantar a Android, debe usar un emulador de Android.

* Una vez que se implemente la suplantación, solo tiene que usar las direcciones URL normales para los perfiles de producción y ensayo: (es decir, `http://sp.auth-staging.adobe.com` y `http://entitlement.auth-staging.adobe.com`. De hecho, llegará al *entorno de precalificación/ producción* de la nueva compilación*.


## PASO 3.  Compruebe que está apuntando al entorno correcto {#Verify-you-are-pointing-to-the-right-environment}

**Este es un paso fácil:**

* cargar [entorno de precuación de derechos](https://entitlement-prequal.auth.adobe.com/environment.html) y [derecho](https://entitlement.auth.adobe.com/environment.html). Deberían devolver la misma respuesta.


## PASO 4.  Realice un flujo de autenticación/autorización sencillo utilizando el sitio web del programador {#peform-a-simple-auth-flow}

* Este paso requiere la dirección del sitio web del programador y algunas credenciales de MVPD válidas (un usuario que esté autenticado y autorizado).

## PASO 5.  Realizar pruebas de situación utilizando los sitios web del programador {#perform-scenario-testing-using-programmer-website}

* Después de completar la configuración del entorno y asegurarse de que el flujo básico de autenticación-autorización funciona, puede continuar con la prueba de escenarios más complejos.


## PASO 6.  Realizar pruebas mediante el sitio de prueba de la API {#perform-testing-using-api-testing-site}

* Si desea profundizar en la prueba de la autenticación de Adobe Pass, le recomendamos que utilice el [sitio de prueba de API](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

Puede encontrar más detalles en el sitio de prueba de API en [Cómo probar los flujos de autenticación y autorización mediante el sitio de prueba de API de Adobe](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md).
