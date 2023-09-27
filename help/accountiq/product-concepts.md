---
title: Glosario de Account IQ
description: Un glosario de terminologías de productos.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1383'
ht-degree: 0%

---

# Conceptos de producto y glosario {#glossary}

Consulte las siguientes terminologías de productos y sus definiciones.

## [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Panel de panel con gráficos que divide los segmentos actuales que comparten puntuaciones en categorías de intervalo compartido de Muy bajo, Bajo, Moderado, Alto y Muy alto.

## [!UICONTROL Action] {#action-def}

Un evento directo o indirecto asociado con un [Operación](#operation-def) que afecta a las características (por ejemplo, puntuación de uso compartido o número de dispositivos en uso) de un segmento de operación relacionado (o cohorte).

## [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Panel de panel con gráficos que divide los segmentos actuales que comparten puntuaciones en categorías de intervalo compartido de Muy bajo, Bajo, Moderado, Alto y Muy alto, junto con el porcentaje de cada categoría de la cantidad total de flujo para el segmento.

## [!UICONTROL AuthN] {#authn-def}

Autenticación o el número de intentos de autenticación. Un intento de autenticación es el proceso por el cual un usuario sin un estado de autenticación válido actualmente es redirigido a su MVPD elegida, donde se identifica a sí mismo en la MVPD - normalmente con un nombre de usuario y contraseña.

## [!UICONTROL AuthN OK] {#authn-ok-def}

Número de autenticaciones correctas. La autenticación correcta se produce cuando un MVPD confirma la identificación de un usuario y provoca que el usuario se redirija de nuevo a la aplicación o al sitio del programador.

## [!UICONTROL AuthZ] {#authz-def}

Autorización o el número de solicitud de autorización. Una solicitud de autorización es el proceso mediante el cual un programador solicita permiso a una MVPD a través de un Adobe para comenzar a transmitir el contenido solicitado por un usuario. La MVPD suele conceder la solicitud en función de los derechos de contenido asociados con la suscripción de MVPD del usuario (por ejemplo, si el canal asociado con el contenido está en la suscripción del usuario). Algunas respuestas de solicitudes de autorización se almacenan en caché por Adobe, lo que permite que el Adobe responda inmediatamente sin pasar la solicitud al MVPD.

## [!UICONTROL AuthZ OK] {#authz-ok-def}

Número de autorizaciones correctas.

## [!UICONTROL Channel] {#channel-def}

El canal, también conocido como Propiedad, es una fuente de contenido de vídeo relacionada temáticamente. Representar tradicionalmente una fuente de vídeo continua distinta y con direcciones numéricas de una MVPD. El canal se asigna directamente a un canal de contenido accesible disponible para los suscriptores a través de su Set Top Box (STB).

## [!UICONTROL Cluster] {#cluster-def}

Un clúster es una colección de ubicaciones y dispositivos. Los clústeres se crean buscando ubicaciones comunes entre dispositivos. Se considerará que los dispositivos que se han visto en una ubicación común pertenecen al mismo clúster. Dos dispositivos pueden estar en el mismo clúster aunque no tengan ubicaciones comunes, pero se pueden conectar a través de las ubicaciones de otros dispositivos.

### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Clúster que no tiene dispositivos estáticos.

### [!UICONTROL Static cluster] {#static-cluster-def}

Clúster que tiene al menos un dispositivo estático.

## [!UICONTROL Concurrency] {#consurrency-def}

El concurrente se define por dos (o más) emisiones reproducidas al mismo tiempo o muy cerca en el tiempo, de modo que el intervalo entre ellas no puede justificarse viajando a una velocidad normal.
El uso simultáneo se calcula utilizando la velocidad máxima (millas/hora) entre 2 grupos diferentes. Se considera que un usuario tiene un uso simultáneo si tiene una velocidad superior a 124 m/h en una distancia inferior a 124 millas o si tiene una velocidad superior a 400 m/h en una distancia superior a 124 millas. La distancia se calcula entre ubicaciones de diferentes grupos. Se permite el uso simultáneo en el mismo clúster.

## [!UICONTROL Device] {#device-def}

Producto de hardware de vídeo digital compatible con Adobe Pass y capaz de reproducir contenido de TV Everywhere. Por ejemplo, teléfonos inteligentes, equipos portátiles y de escritorio, consolas de juegos y televisores inteligentes.

## [!UICONTROL Geographical Span] {#geographical-span-def}

Distancia entre los puntos más lejanos de un conjunto de ubicaciones.

## [!UICONTROL Granularity] {#granularity-def}

En referencia al lapso de tiempo, el tamaño del período; por ejemplo, una semana o un mes.

## [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Un valor calculado para cada uno de los índices de riesgo (cuentas, uso, total) en todos los programadores y MVPD durante el lapso de tiempo seleccionado.

## [!UICONTROL IP] {#ip-def}

La dirección de protocolo de Internet asignada a un dispositivo por un proveedor de servicios de Internet. Por ejemplo, el proveedor de servicios de cable y el proveedor de servicios de celda.

## [!UICONTROL Isolation Mode] {#isolation-mode-def}

Un tipo de análisis de uso compartido en el que la evaluación de una cuenta se limita a eventos que se produjeron directamente en los programadores del segmento seleccionado.  Normalmente, se evalúan todos los eventos de cuenta, lo que proporciona una estimación mucho más precisa del uso compartido.  Algunos datos de MVPD están estructurados de una manera que solo permite el análisis del modo de aislamiento.

## [!UICONTROL Location] {#location-def}

Un punto único en la tierra. También se denomina geolocalización para una solicitud de juego específica, detectada mediante los datos Pass, con una precisión de 1000mx1000m (un km cuadrado).

<!-- ### Home location {#home-location-def}

the precision is 0.01 ~ 2000mx2000m (4 square km) - at this moment the home location is detected using an ML algo, based on data provided by two mvpds. The probability is ~ 80%. We are not using the zip code for the majority of the users. Currently, this information is not used in assessing the sharing probability. -->

## [!UICONTROL Media Company] {#media-company-def}

Media Company es una empresa propietaria de un grupo de redes de medios.

## [!UICONTROL Metric] {#metric}

La métrica es un atributo de la cuenta del suscriptor (por ejemplo, su MVPD, los programadores y canales del contenido que transmiten, el número de dispositivos que utilizan).

## [!UICONTROL Mobile device] {#mobile-device-def}

Un dispositivo con alta movilidad. Por ejemplo, teléfono móvil y tableta.

## [!UICONTROL MVPD] {#mvpd-def}

MVPD, también conocido como Distribuidor, es agregador, revendedor y distribuidor de contenido de vídeo de Media Company.

## Operación {#operation-def}

Operación es un registro creado para realizar un seguimiento del efecto de un objeto concreto [acción](#action-def) en un segmento asociado. Un ejemplo de acción puede ser un límite colocado en el número de flujos simultáneos permitidos para las cuentas identificadas por el segmento.

## [!UICONTROL Overall sharing score] {#overall-sharing-score}

Valor que ayuda a los usuarios a comprender la magnitud del uso compartido de contraseñas en propiedades de Programador o por suscriptores de MVPD y les proporciona una sensación de urgencia para actuar en consecuencia.

<!--**Aggregated Risk Index**
Also known as Risk Index and Sharing Risk Index, it is a value that helps users understand the magnitude of password sharing on Programmer properties or by MVPD subscribers and provides them a sense of urgency to act upon it.-->

<!--**Risk Index - Overall**
A value computed as an average of "Risk Index - Accounts" and the "Risk Index - Usage". Overall Sharing Risk Index-->

## [!UICONTROL Play Request] {#play-requests-def}

Solicitud realizada por un sitio o aplicación cliente al Adobe para solicitar un token multimedia para registrar y asegurar el inicio de un flujo.

## [!UICONTROL Programmer] {#programmer-def}

El programador, también conocido como Red, es una compañía subsidiaria de una compañía más grande (corporación) que posee y administra uno o más canales.

## [!UICONTROL requestorID] {#requestorid-def}

El ID que utiliza una compañía de medios para identificarse a sí misma o a una filial de una MVPD.  Según la implementación del programador, esto podría asignarse a una empresa de medios, un programador o un canal.  El ID más común que utiliza una compañía de medios para identificarse a sí misma o a una filial de una MVPD.  Según la implementación del programador, esto podría asignarse a una empresa de medios, un programador o un canal.  Tradicionalmente, se asigna a un canal.  Con la creación de pseudocanales como MML (March Madness Live) y los movimientos impulsados técnicamente para abordar las limitaciones de datos impulsadas por MVPD, requestorID está empezando a asociarse más con Media Company.

## [!UICONTROL resourceID] {#resource-id-def}

El contenido solicitado por el usuario final.  Tradicionalmente, esto ha identificado el canal asociado con el contenido que el usuario ha solicitado.  Las mejoras del sistema permiten que el ID represente programas específicos (por ejemplo, con clasificaciones específicas), y el ID sigue identificando el canal asociado.

## [!UICONTROL Risk Index - Usage] {#risk-index-usage}

También conocido como Uso de cuentas compartidas, es un valor calculado en función del número de solicitudes de reproducción realizadas por cada cuenta ponderado por la probabilidad de uso compartido de cada cuenta. También se conoce como Índice de riesgos de uso por cuentas compartidas.

## [!UICONTROL Segment] {#segmet-def}

El segmento es un conjunto de cuentas que cumplen las condiciones definidas por el usuario especificadas por las métricas seleccionadas (por ejemplo, &quot;usuarios de MVPD A, B, C, D o E que han visto los canales X, Y o Z&quot;).

## [!UICONTROL Sharing level] {#sharing-level-def}

También conocido como Índice de Riesgo - Cuentas o Índice de Riesgo de Cuentas Compartidas, es un valor calculado en base a un promedio de la probabilidad de compartir calculada para cada cuenta en el conjunto de MVPD seleccionadas que se ha transmitido desde uno de los Canales de Programador seleccionados durante el lapso de tiempo seleccionado.

## [!UICONTROL Static device] {#static-device-def}

Un dispositivo con muy baja movilidad. Por ejemplo, consola de juegos, decodificador y televisor.

## [!UICONTROL Time frame] {#time-frame-def}

También conocido como Periodo o Ranura de Tiempo, es la ventana de tiempo que contiene la actividad de solicitud de reproducción representada en la interfaz de usuario y las tablas de principio a fin.

## [!UICONTROL Top MVPDs in segment] {#top-mvpds-def}

Las MVPD principales (como máximo 10) del segmento seleccionado según la medición del nivel de uso compartido, el uso desde cuentas compartidas o la puntuación de uso compartido general.

## [!UICONTROL Trend] {#trend-def}

Diferencia porcentual en la métrica asociada (por ejemplo, porcentaje del total de solicitudes de reproducción) entre el periodo actual y el anterior.

## [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Número de cuentas de MVPD únicas de un período determinado que han interactuado con aplicaciones o sitios de TV Everywhere del programador que involucran a Adobe Pass durante un período determinado.  Esa interacción incluye cualquier actividad en la aplicación o el sitio del programador que resulte en una llamada a un servicio de Adobe Pass. Por ejemplo, comprobar el estado authN o authZ, autenticar y autorizar. El número total de suscriptores únicos también incluirá el número de dispositivos únicos si el uso de TempPass de Adobe por parte de un programador (es decir, vista previa gratuita) es parte del segmento.

## [!UICONTROL Usage] {#usage-defs}

### [!UICONTROL Avid user] {#avid-user-def}

Más de 37 solicitudes de juego por mes.

### [!UICONTROL Infrequent user] {#infrequent-users-def}

Menos de 9 solicitudes de reproducción al mes.

### [!UICONTROL Regular user] {#regular-user-def}

De 9 a 37 solicitudes de juego por mes.

## [!UICONTROL Usage Pattern] {#usage-patern-def}

Una de un conjunto finito de etiquetas de categoría aplicadas a una cuenta que mejor caracteriza a los usuarios de la cuenta en términos de grupos sociales o comportamientos (por ejemplo, una familia pequeña, un viajero o un viajero que viaja al trabajo, compartir en redes sociales, etc.).

## [!UICONTROL Zip Code] {#zip-code-def}

El código postal de EE. UU. asociado con ubicaciones dentro de EE. UU.
