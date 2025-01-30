---
title: Tokens de medios
description: Tokens de medios
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '667'
ht-degree: 0%

---

# Tokens de medios {#media-tokens}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

El token de medios es un token generado por la autenticación de Adobe Pass [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) como resultado de una decisión de autorización destinada a proporcionar acceso de visualización al contenido protegido (recurso).

El token de medios es válido durante un periodo de tiempo limitado y corto (7 minutos predeterminados) especificado en el momento del problema, lo que indica el límite de tiempo antes de que la aplicación cliente deba verificarlo y utilizarlo. El token de medios está restringido a un solo uso y nunca debe almacenarse en caché.

El token de medios consiste en una cadena firmada basada en la Infraestructura de claves públicas (PKI) enviada en texto no cifrado. Con la protección basada en PKI, el token se firma con una clave asimétrica emitida para su Adobe por una entidad de certificación (CA).

El token de medios se pasa al programador, que puede validarlo mediante el verificador de tokens de medios antes de iniciar el flujo de vídeo para garantizar la seguridad de acceso para ese recurso.

El verificador de tokens de medios es una biblioteca distribuida por la autenticación de Adobe Pass que se encarga de verificar la autenticidad de un token de medios.

## Media Token Verifier {#media-token-verifier}

La autenticación de Adobe Pass recomienda que los programadores envíen el token de medios a su propio servicio back-end que integra la biblioteca del verificador de tokens de medios para garantizar el acceso seguro antes de iniciar el flujo de vídeo. El tiempo de vida (TTL) del token de medios está diseñado para tener en cuenta los posibles problemas de sincronización de reloj entre el servidor que genera el token y el servidor de validación.

La autenticación de Adobe Pass recomienda encarecidamente no analizar el token de medios y extraer directamente sus datos, ya que el formato del token no está garantizado y puede cambiar en el futuro. La biblioteca del verificador de tokens de medios debe ser la única herramienta utilizada para analizar el contenido del token.

La biblioteca de Media Token Verifier se puede descargar desde el siguiente vínculo:

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

La biblioteca Media Token Verifier requiere la versión 1.5 o superior de JDK y admite el uso de un proveedor JCE (Extensión de criptografía Java) preferido para el algoritmo de firma (`SHA256WithRSA`).

La biblioteca de Media Token Verifier representada por el archivo Java `mediatoken-verifier-VERSION.jar` incluye:

* Clave pública de Adobe.
* API de verificación de token (`ITokenVerifier.java`).
* Implementación de referencia (`com.adobe.entitlement.test.EntitlementVerifierTest.java`).
* Dependencias y almacenes de claves de certificados.

>[!IMPORTANT]
> 
> La contraseña predeterminada para el almacén de claves de certificado incluido es `123456`.

### Métodos {#methods}

La clase `ITokenVerifier` define los siguientes métodos:

* El método `isValid()` utilizado para validar el token de medios. Acepta un solo argumento, el [identificador de recurso](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier). Si el identificador de recurso proporcionado es `null`, el método validará solamente la autenticidad y el período de validez del token de medios.

  El método `isValid()` devuelve uno de los siguientes valores de estado:

  | VALID_TOKEN | Validaciones de token correctas |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | Formato de token no válido |
  | INVALID_SIGNATURE | No se pudo validar la autenticidad del token |
  | TOKEN_EXPIRED | TTL de token no válido |
  | INVALID_RESOURCE_ID | El token no es válido para el recurso determinado |
  | ERROR_DESCONOCIDO | El token aún no se ha validado |

* El método `getResourceID()` se utilizó para recuperar el identificador de recurso asociado con el token de medios y compararlo con el identificador devuelto por la respuesta de decisión de autorización.

* El método `getTimeIssued()` utilizado para recuperar la hora en que se emitió el token de medios.

* El método `getTimeToLive()` utilizado para recuperar el TTL del token de medios.

* El método `getUserSessionGUID()` utilizado para recuperar un GUID anónimo establecido por MVPD.

* El método `getMvpdId()` utilizado para recuperar el identificador de MVPD que autenticó al usuario.

* El método `getProxyMvpdId()` utilizado para recuperar el identificador del MVPD proxy que autenticó al usuario.

### Muestra {#sample}

El archivo Media Token Verifier contiene una implementación de referencia (`com.adobe.entitlement.test.EntitlementVerifierTest.java`) y un ejemplo de invocación de la API con la clase de prueba. Este ejemplo (`com.adobe.entitlement.text.EntitlementVerifierTest.java`) ilustra la integración de la biblioteca de Media Token Verifier en un servidor multimedia.

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## API DE REST V2 {#rest-api-v2}

El token de medios se puede recuperar mediante la siguiente API:

* [Recuperar decisiones de autorización utilizando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulte las secciones **Respuesta** y **Ejemplos** de la API anterior para comprender la estructura de las decisiones de autorización y los tokens de medios.

Para obtener más información acerca de cómo y cuándo integrar la API anterior, consulte el siguiente documento:

* [Flujo de autorización básico realizado en la aplicación principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!IMPORTANT]
>
> La aplicación cliente debe pasar el valor `serializedToken` del `token` devuelto al [Verificador de token de medios](#media-token-verifier) para su validación.
