---
title: Metadatos del usuario
description: Metadatos del usuario
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '1902'
ht-degree: 0%

---

# Metadatos del usuario {#user-metadata}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Los metadatos del usuario hacen referencia a [atributos](#attributes) específicos del usuario (por ejemplo, códigos postales, clasificaciones parentales, ID de usuario, etc.) que mantienen las MVPD y que se proporcionan a los programadores mediante la autenticación de Adobe Pass [API de REST V2](#apis).

Los metadatos del usuario están disponibles una vez finalizado el flujo de autenticación, pero algunos atributos de metadatos pueden actualizarse durante el flujo de autorización, según el MVPD y el atributo de metadatos específico en cuestión.

Los metadatos de usuario se pueden utilizar para mejorar la personalización de los usuarios, pero también se pueden utilizar para los análisis. Por ejemplo, un programador puede utilizar el código postal de un usuario para proporcionar noticias localizadas o actualizaciones meteorológicas, o para aplicar el control parental.

La autenticación de Adobe Pass normaliza los valores de metadatos del usuario cuando las MVPD proporcionan datos en diferentes formatos. Además, para ciertos atributos (por ejemplo, código postal), los valores pueden estar [cifrados](#encryption) mediante un certificado de Programador.

La autenticación de Adobe Pass permite a los programadores revisar los metadatos de usuario disponibles en sus integraciones de MVPD y [administrarlos](#management) a través del [tablero de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

## Atributos de metadatos de usuario {#attributes}

En la tabla siguiente se enumeran algunos de los atributos de metadatos de usuario que están disponibles para los programadores:

| Clave | Tipo | Muestra | Requiere cifrado | Descripción | Detalles |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | Cadena | &quot;1o7241p&quot; | No | Identificador de cuenta. | El valor del atributo puede ser un identificador del hogar o un identificador de subcuenta. El valor `userID` será diferente de `householdID` si MVPD admite subcuentas y el usuario actual no es el titular de la cuenta principal. |
| `upstreamUserID` | Cadena | &quot;1o7241p&quot; | No | Identificador de cuenta para la monitorización de concurrencia. | El valor del atributo se puede utilizar para aplicar límites de concurrencia en los sitios y las aplicaciones de MVPD y Programmer. El valor `upstreamUserID` es el mismo que el valor `userID` para la mayoría de las MVPD. |
| `householdID` | Cadena | &quot;1o7241p&quot; | No | Identificador de cuenta para el control parental. | El valor del atributo puede utilizarse para diferenciar entre el uso de los hogares y el de las cuentas secundarias. A veces se puede utilizar como sustituto del control parental si no hay clasificaciones verdaderas disponibles, si el usuario ha iniciado sesión con la cuenta del hogar, puede ver, de lo contrario, el contenido clasificado no se mostraría. Hay muchas variaciones entre las MVPD en cuanto a cómo se representa (por ejemplo, ID de usuario doméstico, ID de cabeza de familia, indicador de cabeza de familia, etc.), si MVPD no admite subcuentas, será idéntico a `userID`. |
| `primaryOID` | Cadena | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | No | Identificador de cuenta. | El atributo es específico de AT&amp;T. El valor `primaryOID` es el mismo que el valor `userID` cuando el valor `typeID` está establecido en &quot;Principal&quot;. |
| `typeID` | Cadena | &quot;Principal&quot; | No | Atributo que indica si el usuario actual es titular de una cuenta principal o secundaria. | El atributo es específico de AT&amp;T. El valor `primaryOID` es el mismo que el valor `userID` cuando el valor `typeID` está establecido en &quot;Principal&quot;. |
| `is_hoh` | Cadena | &quot;1&quot; | No | Atributo que indica si el usuario actual es cabeza de familia o no. | El atributo es específico de Synacor. |
| `hba_status` | Booleano | &quot;true&quot; | No | Atributo que indica si el usuario actual se ha autenticado mediante HBA o no. |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | Booleano | &quot;true&quot; | No | Atributo que indica si el dispositivo actual puede reflejar la pantalla o no. | El atributo es específico de Spectrum. |
| `zip` | Matriz | \[&quot;77754&quot;, &quot;12345&quot;\] | Sí | Código postal del usuario. | El valor del atributo puede utilizarse para ofrecer noticias localizadas, actualizaciones meteorológicas o eventos deportivos. El valor `zip` representa datos confidenciales que necesitan acuerdos legales con MVPD. Cuando está cifrada, la representación de la clave `zip` será un `String` en lugar de un `Array`. |
| `encryptedZip` | Cadena | &quot;&quot; | Sí | Código postal cifrado del usuario. | El atributo es específico de Comcast. |
| `channelID` | Matriz | \[&quot;channel-1&quot;, &quot;channel-2&quot;\] | No | Lista de canales que el usuario puede ver. | El valor del atributo se puede utilizar para filtrar varios canales a partir de portales que agregan varias redes. Se recomienda usar la [API de preautorización](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) en lugar de este atributo de metadatos de usuario para filtrar los canales que no están disponibles para el usuario. |
| `maxRating` | Objeto | { MPAA: &quot;NR&quot;, VCHIP: &quot;X&quot;, URL: &quot;http://manage.my/parental&quot; } | No | Clasificación parental máxima del usuario actual. | El valor del atributo puede utilizarse para filtrar contenido que no sea adecuado para el usuario actual según las clasificaciones &quot;MPAA&quot; o &quot;VCHIP&quot;. |
| `language` | Cadena | &quot;Inglés&quot; | No | Configuración de idioma. | El valor del atributo se puede utilizar para mostrar mensajes según las preferencias de idioma del usuario. |

Los atributos de metadatos de usuario disponibles para un programador dependen de lo que proporcione un MVPD. En la tabla siguiente se enumeran los atributos disponibles en varias MVPD:

|                         | **Acuerdo legal firmado (solo zip)** | **ID de usuario en AuthN** | **ID de usuario ascendente en AuthN** | **ID de hogar en AuthN/Z** | **OID principal en AuthN** | **Escribir ID en AuthN** | **Cabeza de familia en AuthN** | **Estado HBA** | **Permitir creación de reflejo en AuthZ** | **Código postal en AuthN/Z** | **ID de canal en AuthN** | **Clasificación de AuthN/Z** | **Idioma** | **onNet** | **inHome** | **Notas** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **Nombre formal** | n/a | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **Requiere cifrado** | n/a | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| **Sensible** | n/a | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Adobe IdP | **Sí** | **Sí** | **Sí** | **Sí (solo AuthN)** | **Sí** | **Sí** | **Sí** | **No** | **No** | **Sí (solo AuthN)** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | No se necesita un acuerdo legal. |
| Synacor | **Sí** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **Sí** | **No** | **No** | **Sí (solo AuthN)** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | Acuerdo legal que no cubre todas las MVPD proxy. Se trata de una compatibilidad genérica con Synacor y posiblemente no se acumule en todas sus MVPD. |
| Plato | **No** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | Comparte la misma lista que todas las MVPD de Synacor, además de `upstreamUserID`. |
| Comcast | **No** | **Sí** | **Sí** | **Sí (solo AuthZ)** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **Sí (solo AuthZ)** | **No** | **No** | **No** |                                                                                                                                           |
| AT&amp;T | **Sí** | **Sí** | **Sí** | **Sí (solo AuthN)** | **Sí** | **Sí** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | Acuerdo legal firmado. |
| DTV | **Sí** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| COX | **No** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Cablevision | **Sí** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **Sí** | **No** | **No** | **No** | **No** | Acuerdo legal firmado. |
| Espectro | **Sí** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** |                                                                                                                                           |
| Carta | **Sí** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** |                                                                                                                                           |
| Verizon | **No** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **Sí** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| HTC | **No** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Rogers | **No** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| RCN | **Sí** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** |                                                                                                                                           |
| Eastlink | **No** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** |                                                                                                                                           |
| Cogeco | **No** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Videotron | **No** | **Sí** | **Sí** | **Sí*** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | Expone `householdID` con el mismo valor que `userID`. |
| Proxy Massilon | **Sí** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | Acuerdo legal firmado. |
| Borrado de proxy | **Sí** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **Sí (solo AuthZ)** | **Sí** | **No** | **No** | Acuerdo legal firmado. |
| GLDS de proxy | **No** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** |                                                                                                                                           |
| Otras MVPD | **No** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | Aún no hay ningún acuerdo legal. Los metadatos confidenciales no están disponibles para la producción. Para todas las MVPD, `userID` está disponible sin trabajo adicional. |

>[!IMPORTANT]
>
> Se deben firmar acuerdos legales con MVPD antes de que estén disponibles los metadatos confidenciales del usuario (por ejemplo, el código postal).

## Cifrado de metadatos de usuario {#encryption}

Para cifrar y descifrar atributos de metadatos de usuario, el programador debe generar un certificado (par de claves pública y privada) y [autoconfigurar](#management) el certificado a través de [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) o compartir la clave pública con los representantes de autenticación de Adobe Pass.

Siga los pasos a continuación para asegurarse de que el certificado se genera y configura correctamente:

1. Descargue e instale el kit de herramientas de OpenSSL (http://www.openssl.org).

1. Generar una solicitud de firma de certificado (CSR):

   * Genere un par de claves. En el terminal de comandos, ejecute lo siguiente:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Genere la CSR. En el terminal de comandos, ejecute lo siguiente:

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     Se le pedirá que introduzca la contraseña de la clave privada.

   * Cree una copia de seguridad de la clave privada y la contraseña. CSR de muestra:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. Envíe el CSR a una autoridad de certificación (CA) (por ejemplo, Verisign).

1. La CA le enviará el certificado en formato .p7b (PKCS#7, Cryptographic Message Syntax Standard).

1. Implemente el certificado .p7b. Convierta el archivo PKCS#7 (.p7b) a PKCS#12 (archivo PFX, Estándar de sintaxis de intercambio de información personal) con su clave privada y genere el archivo PEM (archivo contenedor de certificado concatenado):

   * Convierta el archivo PKCS#7 en un archivo PEM temporal. En la línea de comandos, ejecute lo siguiente:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Convierta el archivo PEM temporal en un archivo PFX. En la línea de comandos, ejecute lo siguiente:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Convierta el archivo PEM temporal en un archivo PEM final. En la línea de comandos, ejecute lo siguiente:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Use el archivo PEM para [configurar](#management) el certificado a través de [Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication) o envíe el archivo PEM a los representantes de autenticación de Adobe Pass.

   * Consulte la siguiente sección para obtener más información sobre cómo administrar certificados a través del [Tablero de Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

   * La autenticación de Adobe Pass admite un certificado principal y uno de copia de seguridad. Si el certificado principal se ve comprometido de alguna manera, puede revocarlo y cambiar al certificado secundario. Esto garantizará una transición sin problemas entre certificados con un impacto mínimo en el cliente.

## Administración de metadatos de usuario {#management}

>[!IMPORTANT]
>
> En caso de que no tengas acceso al Tablero de Adobe Pass TVE, crea un ticket a través de nuestro [Zendesk](https://adobeprimetime.zendesk.com) y pídele a tu gestor técnico de cuentas (TAM) que realice los cambios correspondientes por ti.

El Tablero de Adobe Pass TVE es una herramienta para que los clientes (programadores) de autenticación de Adobe Pass administren su configuración y sus datos. Este tablero de autoservicio habilita una serie de funcionalidades que se describen en la [Guía del usuario del tablero de Adobe Pass TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

Para revisar y administrar los atributos de metadatos de usuario disponibles en un MVPD, siga los pasos de la [Guía del usuario del panel de TVE para integraciones](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata).

Para revisar y administrar los certificados utilizados para cifrar atributos de metadatos de usuario, siga los pasos de la [Guía del usuario del panel de TVE para programadores](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates) o la [Guía del usuario del panel de TVE para canales](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates).

## API DE REST V2 {#rest-api-v2}

Los atributos de metadatos del usuario se pueden recuperar mediante las siguientes API:

* [Recuperación de perfiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Consulte las secciones **Respuesta** y **Ejemplos** de las API anteriores para comprender la estructura de los atributos de metadatos del usuario.

>[!IMPORTANT]
>
> Los metadatos del usuario están disponibles una vez finalizado el flujo de autenticación, por lo que la aplicación cliente no necesita consultar un extremo independiente para recuperar la información de [metadatos del usuario](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), ya que ya está incluida en la información del perfil.

Para obtener más información acerca de cómo y cuándo integrar las API anteriores, consulte los siguientes documentos:

* [Flujo de perfiles básicos realizado dentro de la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Flujo de perfiles básicos realizado en la aplicación secundaria](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Algunos atributos de metadatos se pueden actualizar durante el flujo de autorización, según la MVPD y el atributo de metadatos específico. Como resultado, es posible que la aplicación cliente tenga que volver a consultar las API anteriores para recuperar los metadatos de usuario más recientes.

>[!MORELIKETHIS]
>
> [Preguntas frecuentes sobre la fase de autenticación](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
