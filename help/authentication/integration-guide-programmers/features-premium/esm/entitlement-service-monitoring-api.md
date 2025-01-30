---
title: API de supervisión del servicio de derechos
description: API de supervisión del servicio de derechos
exl-id: a9572372-14a6-4caa-9ab6-4a6baababaa1
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '2027'
ht-degree: 0%

---

# API de supervisión del servicio de derechos {#entitlement-service-monitoring-api}

>[!IMPORTANT]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

>[!IMPORTANT]
>
> Antes de usar la API de degradación, asegúrese de que se cumplan los siguientes requisitos previos:
>
> * Obtenga las credenciales del cliente como se describe en la [Documentación de la API Retrieve client credentials](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenga el token de acceso como se describe en la [Documentación de la API Recuperar token de acceso](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Consulte la documentación de [Información general sobre el registro dinámico de clientes](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) para obtener más información sobre cómo crear una aplicación registrada y descargar la instrucción de software.

## Resumen de API {#api-overview}

La supervisión del servicio de derechos (ESM) se implementa como un proyecto WOLAP (Web-based [Online Analytical Processing](https://en.wikipedia.org/wiki/Online_analytical_processing){target=_blank}). ESM es una API web genérica de informes empresariales respaldada por un almacén de datos. Actúa como lenguaje de consulta HTTP que permite realizar operaciones OLAP típicas con RESTful.

>[!NOTE]
>
>La API de ESM no está disponible de forma general. Póngase en contacto con el representante del Adobe para preguntas sobre disponibilidad.

La API de ESM proporciona una vista jerárquica de los cubos OLAP subyacentes. Cada recurso ([dimensión](#esm_dimensions) en la jerarquía de dimensiones, asignado como segmento de ruta de URL) genera informes con [métricas](#esm_metrics) (agregadas) para la selección actual. Cada recurso señala a su recurso principal (para el resumen) y a sus subrecursos (para el desglose). El corte y el trozo se logran mediante parámetros de cadena de consulta que anclan dimensiones a valores o rangos específicos.

La API de REST proporciona los datos disponibles en un intervalo de tiempo especificado en la solicitud (volviendo a los valores predeterminados si no se proporciona ninguno), según la ruta de dimensión, los filtros proporcionados y las métricas seleccionadas. El intervalo de tiempo no se aplicará a los informes que no contengan dimensiones de tiempo (año, mes, día, hora, minuto, segundo).

La ruta raíz de la URL del punto de conexión devolverá las métricas agregadas generales dentro de un único registro, junto con los vínculos a las opciones de desglose disponibles. La versión de la API se asigna como el segmento final de la ruta del URI del extremo. Por ejemplo, `https://mgmt.auth.adobe.com/esm/v3` significa que los clientes tendrán acceso a WOLAP versión 3.

Las rutas URL disponibles se pueden detectar mediante los vínculos contenidos en la respuesta. Las rutas de URL válidas se mantienen para asignar una ruta dentro del árbol de desglose subyacente que contiene las métricas agregadas (previamente). Una ruta de acceso en el formulario `/dimension1/dimension2/dimension3` reflejará una agregación previa de esas tres dimensiones (el equivalente de un SQL `clause GROUP` BY `dimension1`, `dimension2`, `dimension3`). Si esta agregación previa no existe y el sistema no puede calcularla sobre la marcha, la API devolverá una respuesta 404 Not Found.

## Árbol de desglose {#drill-down-tree}

Los siguientes árboles de desglose ilustran las dimensiones (recursos) disponibles en ESM 3.0 para [programadores](#progr-dimensions) y [MVPD](#mvpd-dimensions).


### Dimension disponibles para programadores {#progr-dimensions}

#### Día

![](../../../assets/esm-progr-dimensions-day.png)

#### Hora

![](../../../assets/esm-progr-dimensions-hour.png)

#### Minuto

![](../../../assets/esm-progr-dimensions-minute.png)

### Dimension disponibles para MVPD {#mvpd-dimensions}

![](../../../assets/esm-mvpd-dimensions.png)

Una GET al extremo de API `https://mgmt.auth.adobe.com/esm/v3` devolverá una representación que contiene:

* Vínculos a las rutas desplegables raíz disponibles:

   * `<link rel="drill-down" href="/v3/dimensionA"/>`

   * `<link rel="drill-down" href="/v3/dimensionB"/>`

* Un resumen (valores agregados) para todas las métricas (en el valor predeterminado
, ya que no se proporcionan parámetros de cadena de consulta, consulte a continuación).


Siguiendo una ruta detallada (paso a paso):
`/dimensionA/year/month/day/dimensionX` recupera lo siguiente
respuesta:

* Vínculos a las opciones de desglose &quot;`dimensionY`&quot; y &quot;`dimensionZ`&quot;

* Informe que contiene agregados diarios para cada valor de `dimensionX`


### Filtros

Excepto para las dimensiones de fecha y hora, cualquier dimensión disponible para la proyección actual (ruta de dimensión) puede filtrarse utilizando su nombre como parámetro de cadena de consulta.

Están disponibles las siguientes opciones de filtrado:

* Los filtros **Es igual que** se proporcionan al establecer el nombre de dimensión en un valor en particular en la cadena de consulta.

* Los filtros **IN** se pueden especificar agregando el mismo parámetro de nombre de dimensión varias veces con valores diferentes: dimension=value1\&amp;dimension=value2

* **No es igual a** filtros deben usar &#39;\!&#39; símbolo después del nombre de dimensión que genera &#39;\!=&#39; &quot;operator&quot;: dimension\!=valor

* **NO EN** filtros requieren &#39;\!operador =&#39; que se va a utilizar varias veces, una vez para cada valor del conjunto: dimensión\!=valor1\&amp;dimensión\!=valor2&amp;...

También existe un uso especial para los nombres de dimensión en la cadena de consulta: Si el nombre de dimensión se utiliza como parámetro de cadena de consulta sin valor, se indica a la API que devuelva una proyección que incluya esa dimensión en el informe.

### Ejemplo de consultas de ESM

| *URL* | *Equivalente de SQL* |
|---|---|
| /dimension1/dimension2/dimension3?dimension1=value1 | SELECT * desde la proyección WHERE dimensión1 = &#39;valor1&#39; </br> GROUP BY dimensión1, dimensión2, dimensión3 |
| /dimension1/dimension2/dimension3?dimension1=value1&amp;dimension1=value2 | SELECT * desde la proyección WHERE dimensión1 IN (&#39;valor1&#39;, &#39;valor2&#39;) </br> GROUP BY dimensión1, dimensión2, dimensión3 |
| /dimension1/dimension2/dimension3?dimension1!=valor1 | SELECT * desde proyección WHERE dimensión1 &lt;> &#39;valor1&#39; | </br> AGRUPAR POR dimensión1, dimensión2, dimensión3 |
| /dimension1/dimension2/dimension3?dimension1!=valor1&amp;dimensión2!=valor2 | SELECT * desde proyección WHERE dimensión1 NOT IN (&#39;valor1&#39;, &#39;valor2&#39;) | </br> AGRUPAR POR dimensión1, dimensión2, dimensión3 |
| Suponiendo que no hay una ruta de acceso directa: /dimension1/dimension3 </br> pero hay una ruta de acceso: /dimension1/dimension2/dimension3 </br> </br> /dimension1?dimension3 | SELECT * desde la proyección GROUP BY dimensión1, dimensión3 |

>[!NOTE]
>
>Ninguna de estas técnicas de filtrado funcionará para `date/time` dimensiones. La única manera de filtrar las dimensiones `date/time` es establecer los parámetros de cadena de consulta `start` y `end` (descritos a continuación) en los valores necesarios.

Los siguientes parámetros de cadena de consulta tienen significados reservados para la API (y, por lo tanto, no se pueden usar como nombres de dimensión o, de lo contrario, no sería posible ningún filtrado para una dimensión de este tipo).

### Parámetros de cadena de consulta reservada de API de ESM

| Parámetro | Opcional | Descripción | Valor predeterminado | Ejemplo |
| --- | ---- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ---- | --- |
| access_token | Sí | El token de DCR se puede pasar como un token de portador de autorización estándar. | Ninguno | access_token=XXXXXX |
| dimension-name | Sí | Cualquier nombre de dimensión: contenido en la ruta URL actual o en cualquier subruta válida; el valor se tratará como un filtro igual a. Si no se proporciona ningún valor, esto obligará a que la dimensión especificada se incluya en la salida aunque no esté incluida o sea adyacente a la ruta actual | Ninguno | someDimension=someValue&amp;someOtherDimension |
| fin | Sí | Hora de finalización del informe en milis | Hora actual del servidor | end=30-07-2024 |
| formato | Sí | Se utiliza para la negociación de contenido (con el mismo efecto, pero con una prioridad menor que la ruta &quot;extensión&quot;; consulte a continuación). | Ninguno: la negociación de contenido probará las otras estrategias | format=json |
| límite | Sí | Número máximo de filas que se devolverán | Valor predeterminado notificado por el servidor en el autovínculo si no se especifica ningún límite en la solicitud | limit=1500 |
| métricas | Sí | Lista separada por comas de los nombres de métricas que se van a devolver; esto debe utilizarse tanto para filtrar un subconjunto de las métricas disponibles (para reducir el tamaño de la carga útil) como para forzar a la API a que devuelva una proyección que contenga las métricas solicitadas (en lugar de la proyección óptima predeterminada). | Todas las métricas disponibles para la proyección actual se devolverán en caso de que no se proporcione este parámetro. | métricas=m1,m2 |
| start | Sí | Hora de inicio del informe como ISO8601; el servidor rellenará la parte restante si solo se proporciona un prefijo: por ejemplo, start=2024 resultará en start=2024-01-01:00:00:00 | Notificado por el servidor en el autovínculo; el servidor intenta proporcionar valores predeterminados razonables en función de la granularidad de tiempo seleccionada | inicio=15-07-2024 |

El único método HTTP disponible actualmente es GET.

## Códigos de estado de API de ESM {#esm-api-status-codes}

| Código de estado | Frase de razón | Descripción |
|---|---|---|
| 200 | OK | La respuesta contendrá vínculos de &quot;resumen&quot; y &quot;desglose&quot; (si corresponde). El informe se representará como un atributo del recurso: un elemento o propiedad &quot;report&quot; anidado. |
| 400 | Solicitud incorrecta | El cuerpo de la respuesta contendrá un mensaje de texto que explica qué sucede con la solicitud. </br> </br>: un estado de solicitud 400 incorrecto está acompañado por un texto explicativo en el cuerpo de la respuesta (tipo de medios sin formato/texto) que proporciona información útil con respecto al error del cliente. Además de los escenarios triviales como formatos de fecha no válidos o filtros aplicados a dimensiones no existentes, el sistema también se negará a responder a consultas que requieran un volumen masivo de datos para ser devueltos o acumulados sobre la marcha. |
| 401 | No autorizado | Causado por una solicitud que no contiene los encabezados OAuth adecuados para autenticar al usuario |
| 403 | Prohibido | Indica que la solicitud no está permitida en el contexto de seguridad actual; esto ocurre cuando el usuario se autentica pero no tiene permiso para acceder a la información solicitada |
| 404 | No encontrado | Se produce en caso de que se proporcione una ruta de URL no válida con la solicitud. Esto no debería suceder nunca si el cliente sigue los vínculos de &quot;desglose&quot;/&quot;resumen&quot; proporcionados con 200 respuestas |
| 405 | Método no permitido | Indica que se ha utilizado un método no compatible en la solicitud. Aunque actualmente solo se admite el método de GET, las versiones futuras pueden permitir HEAD o OPTIONS |
| 406 | No aceptable | Indica que el cliente ha solicitado un tipo de medio no compatible |
| 500 | Error interno del servidor | &quot;Esto nunca debería suceder&quot; |
| 503 | Servicio no disponible | Indica un error dentro de la aplicación o sus dependencias |

## Formatos de datos {#data-formats}

Los datos están disponibles en los siguientes formatos:

* JSON (predeterminado)
* XML
* CSV
* HTML (para fines de demostración)

Los clientes pueden utilizar las siguientes estrategias de negociación de contenido (la prioridad viene dada por la posición en la lista: lo primero es lo primero):

1. Una &quot;extensión de archivo&quot; anexada al último segmento de la ruta de URL: p. ej., `/esm/v3/media-company/year/month/day.xml`. Si la dirección URL contiene una cadena de consulta, la extensión debe ir antes del signo de interrogación: `/esm/v3/media-company/year/month/day.csv?mvpd= SomeMVPD`
1. Un parámetro de cadena de consulta de formato: por ejemplo, `/esm/report?format=json`
1. El encabezado HTTP Accept estándar: por ejemplo, `Accept: application/xml`

Tanto la &quot;extensión&quot; como el parámetro de consulta admiten los siguientes valores:

* xml
* json
* csv
* html

Si ninguna de las estrategias especifica ningún tipo de medio, la API producirá contenido JSON de forma predeterminada.

## Lenguaje de aplicación de hipertexto {#hypertext-application-language}

Para JSON y XML, la carga útil se codificará como HAL, tal como se describe aquí: <http://stateless.co/hal_specification.html>.

El informe real (una etiqueta o propiedad anidada denominada &quot;informe&quot;) constará de la lista real de registros que contienen todas las dimensiones y métricas seleccionadas o aplicables con sus valores, codificados de la siguiente manera:

### JSON

```JSON
 "report": [
  {
    "dimension1": "d1",
    ...
    "metric1": "m1",
    ...
  }, {
    ...
  }
]
```

### XML

```XML
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report
```

Para los formatos XML y JSON, el orden de los campos (dimensiones y métricas) dentro de un registro no está especificado, pero es coherente (el orden será el mismo en todos los registros). Sin embargo, los clientes no deben depender de ningún orden en particular de los campos de un registro.

El vínculo de recurso (la base &quot;self&quot; en JSON y el atributo de recurso &quot;href&quot; en XML) contiene la ruta actual y la cadena de consulta utilizada para el informe en línea. La cadena de consulta revelará todos los parámetros implícitos y explícitos, de modo que la carga útil señalará explícitamente el intervalo de tiempo utilizado, los filtros implícitos (si los hay), etc. El resto de los vínculos dentro del recurso contienen todos los segmentos disponibles que se pueden seguir para explorar en profundidad los datos actuales. También se proporciona un vínculo para el resumen y señalará la ruta principal (si la hay). El valor `href` de los vínculos de exploración o resumen solo contiene la ruta de acceso a la dirección URL (no incluye la cadena de consulta, por lo que el cliente debe agregarla si es necesario). Tenga en cuenta que no todos los parámetros de cadena de consulta utilizados (o implícitos) por el recurso actual serán aplicables para los vínculos de &quot;resumen&quot; o &quot;desglose&quot; (por ejemplo, es posible que los filtros no se apliquen a los subrecursos o superrecursos).

Ejemplo (suponiendo que tenemos una sola métrica llamada `clients` y que hay una agregación previa para `year/month/day/...`):

* https://mgmt.auth.adobe.com/esm/v3/year/month.xml

```XML
   <resource href="/esm/v3/year/month?start=2024-07-20T00:00:00&end=2024-08-20T14:35:21">
   <links>
   <link rel="roll-up" href="/esm/v3/year"/>
   <link rel="drill-down" href="/esm/v3/year/month/day"/>
   </links>
   <report>
   <record month="6" year="2024" clients="205"/>
   <record month="7" year="2024" clients="466"/>
   </report>
   </resource>
```

* https://mgmt.auth.adobe.com/esm/v3/year/month.json

  ```JSON
      {
        "_links" : {
          "self" : {
            "href" : "/esm/v3/year/month?start=2024-07-20T00:00:00&end=2024-08-20T14:35:21"
          },
          "roll-up" : {
            "href" : "/esm/v3/year"
          },
          "drill-down" : {
            "href" : "/esm/v3/year/month/day"
          }
        },
        "report" : [ {
          "month" : "6",
          "year" : "2024",
          "clients" : "205"
        }, {
          "month" : "7",
          "year" : "2024",
          "clients" : "466"
        } ]
      }
  ```

### CSV

En el formato de datos CSV, no se proporcionarán vínculos ni otros metadatos en línea (excepto la fila de encabezado); en su lugar, los metadatos de selección se proporcionarán en el nombre del archivo, que seguirá este patrón:

```CSV
    esm__<start-date>_<end-date>_<filter-values,...>.csv
```

El CSV contendrá una fila de encabezado y, a continuación, los datos del informe como filas posteriores. La fila de encabezado contendrá todas las dimensiones seguidas de todas las métricas. El orden de los datos del informe se reflejará en el orden de las dimensiones. Por lo tanto, si los datos se ordenan por `D1` y después por `D2`, el encabezado CSV tendrá el siguiente aspecto: `D1, D2, ...metrics...`.

El orden de los campos en la fila de encabezado reflejará el orden de los datos de tabla.


Ejemplo: https://mgmt.auth.adobe.com/esm/v3/year/month.csv producirá un archivo de nombre `report__2024-07-20_2024-08-20_1000.csv` con el siguiente contenido:


| Año | Mes | Clientes |
| ---- | :---: | ------- |
| 2024 | 6 | 580 |
| 2024 | 7 | 231 |

## Actualización de datos {#data-freshness}

Las respuestas HTTP correctas contienen un encabezado `Last-Modified` que indica la hora en la que se actualizó por última vez el informe del cuerpo. La falta de un encabezado Última modificación indica que los datos del informe se calculan en tiempo real.

Por lo general, los datos granulados se actualizarán con menos frecuencia que los datos granulados (por ejemplo, los valores por minuto o por hora pueden estar más actualizados que los valores diarios, especialmente para las métricas que no se pueden calcular en función de granularidades más pequeñas, como los recuentos únicos).

## Compresión GZIP {#gzip-compression}

Adobe recomienda habilitar la compatibilidad con gzip en los clientes que recuperen informes de ESM. Al hacerlo, se reducirá en gran medida el tamaño de la respuesta, lo que a su vez reduce el tiempo de respuesta. (El índice de compresión de los datos de ESM está en el rango de 20 a 30).

Para habilitar la compresión gzip en su cliente, establezca el encabezado `Accept-Encoding:` de la siguiente manera:

* Accept-Encoding: gzip, deflate
