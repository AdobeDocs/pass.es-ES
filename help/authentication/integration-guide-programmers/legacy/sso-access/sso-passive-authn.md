---
title: SSO mediante autenticación pasiva
description: SSO mediante autenticación pasiva
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '779'
ht-degree: 0%

---

# (Heredado) SSO mediante autenticación pasiva

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.


## Introducción

El ámbito de este documento es describir la implementación del flujo de autenticación pasiva y cómo funciona con nuestro método estándar de inicio de sesión único.

## Usecases

La autenticación de Adobe Pass habilita el inicio de sesión único (SSO) entre aplicaciones y sitios. Una vez que un usuario inicia sesión con sus credenciales de MVPD, la autenticación de Adobe Pass genera un token seguro que representa la sesión de autenticación de MVPD y enlaza ese token al dispositivo del usuario mediante un ID de dispositivo. La autenticación de Adobe Pass almacena el token o el ID de dispositivo en un servidor o en el dispositivo.

Mientras el token siga siendo válido, los usuarios aparecerán directamente como autenticados. Esto permite a los usuarios introducir sus credenciales con menos frecuencia a la vez que mantiene las transacciones seguras.



El caso de uso comercial detallado aquí es un requisito muy específico: que el usuario DEBE autenticarse al menos una vez para cada sitio visitado. Esto permite que MVPD aplique reglas empresariales asociadas con la sesión authN que pueden variar según la red. Esto entra en conflicto con la promesa actual de TVE de que un usuario solo debe iniciar sesión una vez y se autenticará en todos los sitios que forman parte del ecosistema de autenticación de Adobe Pass.



Para mantener la regla de negocio pero también una buena experiencia de usuario, MVPD NO requiere que un usuario proporcione manualmente información de credenciales. Podemos beneficiarnos de la cookie de sesión previamente establecida e intentar realizar una reautenticación automática utilizando el flujo pasivo; desde la perspectiva del usuario, aparecerá como si hubiera iniciado sesión automáticamente.



Para resolver estos problemas, implementamos dos características distintas: autenticación por red y compatibilidad con autenticación pasiva. Las MVPD admitirán authN pasivo de SAML cuando simplemente vuelvan a autenticar a un usuario si existe una sesión de authN en el IdP, independientemente del sitio en el que se haya creado esa sesión.



## Autenticación por red

Esta función garantiza que MVPD reciba una solicitud de autenticación una vez por cada sitio visitado. Esta funcionalidad significa que un token de autenticación de autenticación de Adobe Pass se enlazará al ID del solicitante, por lo que solo es válido para la red que lo solicitó. Como resultado, una vez que el usuario se autentica en el sitio &quot;A&quot; y luego visita el sitio &quot;B&quot;, se le pedirá que se autentique.



Tenga en cuenta que, debido al hecho de que en el MVPD IdP el usuario ya está autenticado, no será necesario que proporcione la información de inicio de sesión, sino que, en su lugar, el explorador simplemente se redirigirá del sitio &quot;B&quot; al MVPD IdP y, a continuación, volverá. El mismo usuario seguirá autenticado en el sitio &quot;A&quot; si vuelve.



El siguiente flujo muestra la función básica Autenticación por red:





## Autenticación pasiva

El objetivo de esto es hacer que el proceso de reautenticación se produzca en segundo plano sin necesidad de una redirección completa del explorador y sin que se muestre el selector. Como resultado, el usuario que pase del sitio A al sitio B se autenticará automáticamente.



Desde la perspectiva de UX, no habrá diferencia entre este flujo y un flujo ejecutado con un MVPD normal. Lo que el usuario ve es que, después de introducir credenciales como resultado de la visita al sitio A, se autentica automáticamente en el sitio B.



Para que este flujo sea viable, MVPD deberá admitir la autenticación pasiva para que el iframe oculto no se &quot;atasque&quot; en la página de inicio de sesión en caso de que el MVPD ya no tenga una sesión. Esto se realiza mediante el atributo estándar &quot;isPassive&quot;.



El diagrama siguiente muestra el flujo mejorado y la autenticación pasiva &quot;entre bastidores&quot;:





Ejemplo de solicitud de SAML
Este es un ejemplo de solicitud de SAML para el flujo authN pasivo:


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## Reglas empresariales

Las MVPD tienen restricciones de dominio de ámbito SSO específicas. Por ejemplo, algunas MVPD (SSO para la misma empresa de medios, pero no entre empresas) solo podrían permitir algunos dominios.
Algunas MVPD pueden requerir que se apliquen reglas de autenticación diferentes. Por ejemplo, las MVPD pueden tener TTL de autenticación diferentes por redes diferentes. Además, las MVPD pueden habilitar la autenticación basada en el hogar para algunas redes, pero no para otras (los casos de uso de control parental se representan con fuerza aquí).


Estos requisitos comerciales deben tener en cuenta que el caso de uso principal es que no se debe exigir al usuario que vuelva a iniciar sesión después de iniciar sesión correctamente con su MVPD.

Esto se puede lograr utilizando la autenticación por red con el indicador pasivo authN.



## Limitaciones conocidas

iOS: debido a la naturaleza del almacenamiento local de iOS, los flujos de SSO no funcionan en iOS para aplicaciones desarrolladas por distintos proveedores. Para obtener más información sobre SSO en iOS 8 y versiones posteriores, consulte esta nota técnica.


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
