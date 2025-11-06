---
title: Glosario de registro dinámico de clientes (DCR)
description: Glosario de registro dinámico de clientes (DCR)
exl-id: 4ce67fa5-b0e5-4967-b83d-c682426d9329
source-git-commit: ae02f53afc58b7d31f57bcc1e4dd1328f12abc3e
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Glosario de registro dinámico de clientes (DCR) {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

Este documento proporciona definiciones de los términos utilizados al integrar el Registro dinámico de clientes (DCR) de autenticación de Adobe Pass.

>[!MORELIKETHIS]
> 
> * [Glosario de la API REST v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## Términos del glosario {#glossary-terms}

### A {#a}

#### Token de acceso {#access-token}

El token de acceso es un token generado por la autenticación de Adobe Pass como resultado del proceso de [Registro dinámico de clientes (DCR)](#dcr) diseñado para garantizar el acceso a las API protegidas.

### C {#c}

#### Credenciales del cliente {#client-credentials}

Las credenciales del cliente son un conjunto de valores únicos que se generan durante el proceso de [Registro dinámico de clientes (DCR)](#dcr) y que están pensados para utilizarse para obtener un [token de acceso](#access-token).

#### Esquema personalizado {#custom-scheme}

El esquema personalizado es un valor único que hace referencia a una aplicación de [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) que se puede generar y descargar desde Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) y está pensado para utilizarse como redireccionamiento final en aplicaciones que se ejecutan en dispositivos iOS.

### D {#d}

#### DCR {#dcr}

El registro de cliente dinámico (DCR) es un mecanismo de autorización definido por [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591), y se basa en el marco de autorización OAuth 2.0 descrito por [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

El DCR se entrega a [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) como un servicio de autenticación de Adobe Pass que puede habilitar aún más el acceso a las API protegidas.

Para obtener más información, consulte la [Información general sobre el registro de clientes dinámicos](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

### R {#r}

#### Aplicación registrada {#registered-application}

La aplicación registrada es un concepto de autenticación de Adobe Pass que almacena información sobre la aplicación [Programmer](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) que requiere continuar con el proceso de [Registro dinámico de clientes (DCR)](#dcr).

### S {#s}

#### Declaración de software {#software-statement}

La instrucción de software es un token web JSON (JWT) que se puede descargar desde el [Tablero de TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) de Adobe Pass y está pensado para utilizarse como parte del proceso de [Registro dinámico de clientes (DCR)](#dcr).
