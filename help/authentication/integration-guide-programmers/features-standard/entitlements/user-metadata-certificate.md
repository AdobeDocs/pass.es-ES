---
title: Certificado de metadatos de usuario para cifrado
description: Certificado de metadatos de usuario para cifrado
exl-id: 6f5d9a31-945e-418b-a9df-985bdbf29dff
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Certificado de metadatos de usuario para cifrado

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Para la integración de Adobe Pass Authentication para los metadatos de usuario cifrados, debe tener un par clave-público.

En este documento se describe un proceso de generación de certificados de clave pública para su uso en la autenticación de Adobe Pass. El proceso que se describe aquí hace uso del kit de herramientas OpenSSL.

## Tutorial del proceso de generación de certificados (#generation)

1. Descargue e instale el kit de herramientas de OpenSSL (http://www.openssl.org).

1. Generar una solicitud de firma de certificado (CSR):

   * Genere un par de claves.  Abra una ventana Comando / terminal y ejecute el siguiente comando:

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * Genere la CSR. En la línea de comandos, ejecute lo siguiente:

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

1. La CA le enviará el certificado en formato .p7b (PKCS#7, Cryptographic Message Syntax Standard)

1. Implemente el certificado .p7b. Convierta el archivo PKCS#7 (.p7b) a PKCS#12 (archivo PFX, Estándar de sintaxis de intercambio de información personal) con su clave privada y genere el archivo PEM (archivo contenedor de certificado concatenado):

   * Convierta el archivo PKCS#7 en un archivo PEM temporal. En la línea de comandos, ejecute lo siguiente:

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * Convierta el archivo PEM temporal en un archivo PFX.  En la línea de comandos, ejecute lo siguiente:

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * Convierta el archivo PEM temporal en un archivo PEM final. En la línea de comandos, ejecute lo siguiente:

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. Envíe el archivo PEM final al Adobe para configurarlo.

   * La persona que necesita recibir finalmente el archivo PEM es el ingeniero de habilitación de Adobe asignado a su integración/validación. Si no trabaja directamente con esa persona, puede averiguar a quién enviar el archivo desde el representante del Adobe.
   * El Adobe admite un certificado principal y un certificado de copia de seguridad. Si el certificado principal se ve comprometido de alguna manera, puede revocarlo y cambiar al certificado secundario para firmar el ID de solicitante en la aplicación. Esto garantizará una transición sin problemas entre los certificados en producción, con un impacto mínimo en el cliente.
   * Una vez que el Adobe reciba el archivo PEM, los ingenieros de autenticación lo añadirán a la configuración del lado del servidor y confirmarán la recepción del archivo.
