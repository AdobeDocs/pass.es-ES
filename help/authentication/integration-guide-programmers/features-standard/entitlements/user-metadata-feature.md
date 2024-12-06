---
title: Función de metadatos de usuario
description: Función de metadatos de usuario
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 0%

---

# Metadatos del usuario {#user-metadata}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

</br>
</br>

## Introducción {#intro}

La función de metadatos de usuario permite a los programadores acceder a diferentes tipos de datos específicos del usuario que mantienen las MVPD.  Los tipos de metadatos de usuario incluyen códigos postales, clasificaciones parentales, ID de usuario y más.  Los metadatos de *Usuario* son una extensión de los metadatos de *static* disponibles anteriormente (TTL de token de autenticación, TTL de token de autorización e ID de dispositivo).


Puntos clave de metadatos de usuario:

- Pasado a la aplicación del programador durante los flujos de autenticación y autorización
- Los valores se guardan en el token
- Los valores se pueden normalizar si diferentes MVPD proporcionan datos en diferentes formatos
- Algunos parámetros se pueden cifrar con la clave del programador (por ejemplo, código postal); consulte [Certificado de metadatos de usuario para cifrado](user-metadata-certificate.md) para la generación de certificados de cifrado
- Los valores específicos están disponibles por Adobe, a través de un cambio de configuración

## Obtención de metadatos de usuario {#obtaining}

Los metadatos de usuario están disponibles para los programadores a través de la función AccessEnabler `getMetadata()` y del extremo `/usermetadata` en la API sin cliente.  Consulte la documentación de la API de plataforma para obtener más información sobre el uso de `getMetadata()` y su llamada de retorno correspondiente `setMetadataStatus()` (o para los extremos y parámetros utilizados en la API sin cliente).

Los programadores obtienen metadatos al proporcionar una clave para el tipo de metadatos que desean obtener: `getMetadata(key)`.

Los metadatos se devuelven de la siguiente manera: `setMetadataStatus(key, encrypted, data)`:

| Parámetro | Tipo | Descripción |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | Cadena | Especifica el tipo de metadatos solicitado |
| `encrypted` | Booleano | Un indicador booleano que indica si el &quot;valor&quot; está cifrado o no. Si es &quot;true&quot;, &quot;value&quot; será en realidad una representación cifrada con web JSON del valor real. |
| `data` | Objeto | Un objeto JSON que contiene la representación de los metadatos |



La estructura del parámetro de datos y los valores varían entre tipos:

| Clave | Tipo de valor | Muestra | Descripción |
| --- | --- | --- | --- |
| `zip` | Matriz JSON | \[&quot;77754&quot;, &quot;12345&quot;\] | Código postal |
| `householdID` | Cadena JSON | &quot;1o7241p&quot; | Identificador del hogar. Si la MVPD no admite subcuentas, será idéntico a `userID` |
| `maxRating` | Objeto JSON | { MPAA: &quot;NR&quot;, <br> VCHIP: &quot;X&quot;, <br> URL: &quot;http://manage.my/parental&quot; } | Clasificación paterna máxima para el usuario |
| `userID` | Cadena JSON | &quot;1o7241p&quot; | El identificador de usuario. En caso de que una MVPD admita subcuentas y el usuario no sea la cuenta principal, `userID` será diferente a `householdID`. |
| `channelID` | Matriz JSON | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | Una lista de canales que un usuario puede ver |
| `is_hoh` | Cadena JSON | &quot;1&quot; | Un indicador que identifica si un usuario es el cabeza de familia |
| `encryptedZip` | Cadena JSON | &quot;&quot; | Comcast expone el código postal cifrado |
| `typeID` | Cadena JSON | Principal | Un indicador que identifica si la cuenta de usuario es la cuenta principal/secundaria |
| `primaryOID` | Cadena JSON | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Identificador del hogar. Si `typeID` es Principal, contendrá el valor de `userID` |
| hba_status | Booleano | &quot;true&quot; &quot;false&quot; | Un indicador booleano que indica si la autenticación basada en inicio está habilitada para una integración en particular |
| allowMirroring | Booleano | &quot;true&quot; &quot;false&quot; | Indica si se permite o no la duplicación de pantalla para este dispositivo |

>[!NOTE]
>
> **Nota:** Si el parámetro de datos está cifrado, como suele ser el caso de la **clave zip**, la representación de la clave de metadatos será una cadena JSON en lugar de una matriz o un objeto.


**Importante:** Los metadatos de usuario reales disponibles para un programador dependen de lo que una MVPD ponga a disposición.  Se deben firmar acuerdos legales con MVPD antes de que los metadatos confidenciales del usuario (como el código postal) estén disponibles en el entorno de producción.

</br>


| Nombre | Detalles | Requiere cifrado | Comentarios |
| --- | --- | --- | --- |
| ID de usuario | Según lo dispuesto por la MVPD | No | Este es el valor al que luego se le aplica un hash mediante Adobe y que se expone en el token de medios y en la llamada de retorno sendTrackingData().<br><br>El hash en este caso se realizó por motivos históricos<br><br>Este ID podría ser un ID doméstico o un ID de subcuenta. Normalmente no se especifica, solo es el ID vinculado al inicio de sesión que se utilizó en el momento (que puede ser uno principal o una subcuenta) |
| ID de usuario ascendente | Proporcionado por la MVPD para utilizarse únicamente para flujos de supervisión de concurrencia | No | Este valor se utiliza al aplicar límites de concurrencia en los sitios y las aplicaciones de MVPD y Programador. <br><br>El ID también puede contener políticas de supervisión de concurrencia<br><br>Para la mayoría de las MVPD, este valor es igual al ID de usuario |
| ID de usuario doméstico | Suministrado por la MVPD para ser utilizado principalmente para flujos de Control Parental | No | ID que permite a los programadores comprender el uso doméstico frente al de subcuentas.<br><br>A veces se usa como sustituto de Control parental si las clasificaciones verdaderas no están disponibles (si el usuario inició sesión con la cuenta del hogar que puede ver, de lo contrario, el contenido clasificado no se mostraría)<br><br>Hay muchas variaciones entre las MVPD para la manera en que se representa: ID de usuario del hogar, ID del jefe de hogar, indicador del jefe de hogar, etc. |
| Cabeza de familia | Indicador que indica si la cuenta corriente es cabeza de familia o no | No | véase más arriba |
| ID de tipo/ID principal | Identificadores de cuenta del hogar | No | Indicadores específicos de AT&amp;T para cabeza de familia.<br><br>Id. de tipo = Indicador que identifica si la cuenta de usuario es la cuenta principal/secundaria<br><br>OID principal = Identificador del hogar. Si TypeID es Primary, contendrá el valor de userID |
| Clasificación máxima | Clasificación máxima permitida para la cuenta actual | No | Permite a los programadores filtrar el contenido que no es adecuado para la cuenta de.<br><br>Tiene clasificaciones MPAA o VCHIP |
| Línea de canales | Lista de canales disponibles en el paquete del usuario | No | Se usa para permitir o quitar rápidamente varios canales de los portales que agregan varias redes</br></br> *Tenga en cuenta que la autorización de comprobación preliminar generalmente permite una mayor flexibilidad para este caso de uso y debe usarse en su lugar* <br><br>La especificación OLCA lo permite como AttributeStatement en la respuesta AuthN |
| Estado de HBA | Indica si se ha producido la autenticación mediante HBA | No |     |
| Código postal | Código postal de facturación del usuario | Sí | Utilizado para eventos deportivos o de radiodifusión<br><br>También se puede proporcionar con la respuesta AuthZ para actualizaciones rápidas<br><br>Datos confidenciales, necesita acuerdos legales de MVPD |
| Código postal cifrado | Código postal de facturación del usuario (Comcast) | Sí | Como arriba pero cifrado por Comcast |
| Idioma | Configuración de idioma del usuario | No | Se utiliza para mostrar mensajes según las preferencias del usuario |
| Permitir creación de reflejo | Indica si se permite o no la duplicación de pantalla para este dispositivo | No |     |



## Metadatos disponibles {#available_metadata}

La siguiente tabla muestra el estado actual de los metadatos de usuario en el ecosistema de autenticación de Adobe Pass:


|     | **Acuerdo **<br><br>**Legal **<br><br>**firmado (solo zip)** | **ID de usuario **<br><br>**en AuthN** | **Código postal **<br><br>**en AuthN/Z** | **Clasificación **<br><br>**en AuthN/Z** | **Hogar **<br><br>**ID en AuthN/Z** | **ID de canal en AuthN** | **Cabeza de familia en AuthN** | **Escribir ID en AuthN** | **OID principal en AuthN** | Idioma | Id. de usuario ascendente **en AuthN** | Estado de HBA | OnNet | inHome | Permitir creación de reflejo en AuthZ | **Notas** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Nombre formal** | n/a | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | idioma | upstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. Para AuthN: OiosamlMetadataParser debe cambiarse para que todos los analizadores tengan este nuevo atributo habilitado <br>2.  Para AuthZ: no hay una forma genérica, ya que la implementación de autorización es específica de MVPD |
| **Se requiere cifrado** | n/a | **No** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** |     |
| **Sensible** | n/a | **No** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** |     |     |
| **Id. de Adobe** | **Sí** | **Sí** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí** | **Sí** | **Sí** | **Sí** | **No** | **Sí** | **No** | **No** | **No** | **No** | No se necesita ningún acuerdo legal. Se puede habilitar. |
| **Sincronizador** | **Sí** | **Sí** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí** | **Sí** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | **El acuerdo legal no cubre todas las MVPD proxy.**   <br>Es compatibilidad genérica con Synacor; posiblemente no se acumule en todas sus MVPD. |
| Plato | **No** | **Sí** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | Comparte la misma lista que todas las MVPD de Synacor, además de upstreamUserID. |
| Comcast | **No** | **Sí** | **No** | **Sí (solo AuthZ)** | **Sí (solo AuthZ)** | **No** | **No** | **No** | **No** | **No** | **Sí** | **Sí** | **No** | **No** | **No** |     |
| **AT&amp;T** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **Sí (solo AuthN)** | **No** | **No** | **Sí** | **Sí** | **No** | **Sí** | **No** | **No** | **No** | **No** | Acuerdo legal firmado. |
| **Cablevision** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | Acuerdo legal firmado. |
| **HTC** | **No** | **Sí** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| **Massilon de proxy** | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | Acuerdo legal firmado. |
| **Borrar proxy** | **Sí** | **Sí** | **Sí (solo AuthN)** | **Sí (solo AuthZ)** | **No** | **No** | **No** | **No** | **No** | **Sí** | **Sí** | **No** | **No** | **No** | **No** | Acuerdo legal firmado. |
| Rogers | **No** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| RCN | **Sí** | **Sí** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| Carta | **Sí** | **Sí** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| Verizon | **No** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **Sí** | **No** | **No** | **No** |     |
| Eastlink | **No** | **Sí** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| GLDS de proxy | **No** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| DTV | **Sí** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| COX | **No** | **Sí** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| Cogeco | **No** | **Sí** | **Sí (solo AuthN)** | **No** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** |     |
| Videotron | **No** | **Sí** | **Sí (solo AuthN)** | **No** | **Sí*** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | Expone householdID con el mismo valor que userID |
| Espectro | **Sí** | **Sí** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **Sí (solo AuthN)** | **No** | **No** | **No** | **No** | **No** | **Sí** | **Sí** | **No** | **No** | **Sí** |     |
| **Todas las demás **<br><br>**MVPD** | **No** | **Sí** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **No** | **Sí** | **No** | **No** | **No** | **No** | **Aún no hay ningún acuerdo legal, los metadatos confidenciales no están disponibles para la producción.** <br>Para todas las MVPD, el identificador de usuario está disponible sin trabajo adicional. |


La lista de tipos de metadatos de usuario se ampliará a medida que haya nuevos tipos disponibles y añadidos al sistema de autenticación de Adobe Pass.

## Ejemplos de código {#code_samples}

- [Ejemplo de código 1](#code_sample1)
- [Ejemplo de código 2 (simulación de getMetadata)](#code_sample2)


### Ejemplo de código 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### Ejemplo de código 2 (simulación de getMetadata) {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
