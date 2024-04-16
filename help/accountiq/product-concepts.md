---
title: Glosario de Account IQ
description: Un glosario de terminologías de productos.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# Conceptos de producto y glosario {#glossary}

## Terminologías comunes en D2C y TV en todas partes

Las siguientes terminologías de productos y sus definiciones son comunes a todos los [versiones de Account IQ](versions-aiq.md).

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Panel de panel con gráficos que divide las puntuaciones de uso compartido del segmento actual en categorías de intervalo compartido de Muy bajo, Bajo, Moderado, Alto y Muy alto.

### [!UICONTROL Action] {#action-def}

Un evento directo o indirecto asociado con un [Operación](#operation-def) que afecta a las características (por ejemplo, la puntuación de uso compartido o el número de dispositivos en uso) de un segmento de operación relacionado.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Panel de panel con gráficos que divide las puntuaciones de uso compartido del segmento actual en categorías de intervalo compartido de Muy bajo, Bajo, Moderado, Alto y Muy alto, junto con el porcentaje de cada categoría de la cantidad total de flujo para el segmento.

### [!UICONTROL AuthN] {#authn-def}

Número de intentos de autenticación. Un intento de autenticación es el proceso mediante el cual un usuario intenta iniciar sesión con el servicio D2C o MVPD. Para los usuarios de TV Everywhere, el usuario es redirigido a su MVPD elegido, donde se identifican a sí mismos en la MVPD - normalmente con un nombre de usuario y contraseña.

### [!UICONTROL AuthN OK] {#authn-ok-def}

Número de autenticaciones correctas. Una autenticación correcta se produce cuando un servicio D2C o MVPD confirman la identidad de un usuario. Para los usuarios de TV en todas partes, esto hace que el usuario se redirija de nuevo a la aplicación o al sitio del programador.

### [!UICONTROL Cluster] {#cluster-def}

Un clúster es una colección de ubicaciones y dispositivos. Los clústeres se crean buscando ubicaciones comunes entre dispositivos. Se considerará que los dispositivos que se han visto en una ubicación común pertenecen al mismo clúster. Dos dispositivos pueden estar en el mismo clúster aunque no tengan ubicaciones comunes, pero se pueden conectar a través de las ubicaciones de otros dispositivos.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Clúster que no tiene dispositivos estáticos.

#### [!UICONTROL Static cluster] {#static-cluster-def}

Clúster que tiene al menos un dispositivo estático.

### [!UICONTROL Concurrency] {#consurrency-def}

El concurrente se define por dos (o más) emisiones reproducidas al mismo tiempo o muy cerca en el tiempo, de modo que el intervalo entre ellas no puede justificarse viajando a una velocidad normal.
El uso simultáneo se calcula utilizando la velocidad máxima (millas/hora) entre 2 grupos diferentes. Se considera que un usuario tiene un uso simultáneo si tiene una velocidad superior a 124 m/h en una distancia inferior a 124 millas o si tiene una velocidad superior a 400 m/h en una distancia superior a 124 millas. La distancia se calcula entre ubicaciones de diferentes grupos. Se permite el uso simultáneo en el mismo clúster.

### [!UICONTROL Device] {#device-def}

Producto de hardware de vídeo digital capaz de reproducir contenido de flujo ascendente. Por ejemplo, teléfonos inteligentes, portátiles y equipos de escritorio, consolas de juegos y televisores inteligentes.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

El periodo de evaluación es el tiempo que transcurre desde el comienzo de la acción asociada con Operation hasta el final de la acción o hasta su medición.

### [!UICONTROL Geographical Span] {#geographical-span-def}

Distancia entre los puntos más lejanos de un conjunto de ubicaciones.

### [!UICONTROL Granularity] {#granularity-def}

En referencia al intervalo de tiempo, el tamaño del período; por ejemplo, una semana o un mes.

### [!UICONTROL IP] {#ip-def}

La dirección de protocolo de Internet asignada a un dispositivo por un proveedor de servicios de Internet. Por ejemplo, el proveedor de servicios de cable y el proveedor de servicios de celda.

### [!UICONTROL Location] {#location-def}

Un punto único en la tierra. También se denomina geolocalización para una solicitud de juego específica con una precisión de 1000 m x 1000 m (un km cuadrado).

### [!UICONTROL Media Company] {#media-company-def}

Media Company es una empresa propietaria de un grupo de redes de medios.

### [!UICONTROL Metric] {#metric}

La métrica es un atributo de la cuenta del suscriptor (por ejemplo, su MVPD, los programadores y canales del contenido que transmiten y el número de dispositivos que utilizan).

### [!UICONTROL Mobile device] {#mobile-device-def}

Un dispositivo con alta movilidad. Por ejemplo, teléfono móvil y tableta.

### Operación {#operation-def}

Operación es un registro creado para realizar un seguimiento del efecto de un objeto concreto [acción](#action-def) en un segmento asociado. Un ejemplo de acción puede ser un límite colocado en el número de flujos simultáneos permitidos para las cuentas identificadas por el segmento.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

Valor que ayuda a los usuarios a comprender la magnitud del uso compartido de contraseñas y les proporciona una sensación de urgencia para actuar en consecuencia.

### [!UICONTROL Play Request] {#play-requests-def}

Equivale a un inicio de flujo. Este evento marca el comienzo de un flujo de contenido de usuario.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

También conocido como Uso de cuentas compartidas, es un valor calculado en función del número de solicitudes de reproducción realizadas por cada cuenta ponderado por la probabilidad de uso compartido de cada cuenta. También se conoce como Índice de riesgos de uso por cuentas compartidas.

### [!UICONTROL Segment] {#segmet-def}

El segmento es un conjunto de cuentas que cumplen las condiciones definidas por el usuario especificadas por las métricas seleccionadas. Por ejemplo, &quot;usuarios de las regiones A, B, C, D o E que tienen más de tres dispositivos&quot;.

### [!UICONTROL Sharing level] {#sharing-level-def}

También conocido como Índice de Riesgo - Cuentas o Índice de Riesgo de Cuentas Compartidas, es un valor calculado en base a un promedio de la probabilidad de compartir calculada para cada cuenta en el segmento actual que se ha transmitido al menos una vez durante el intervalo de tiempo seleccionado.

### [!UICONTROL Static device] {#static-device-def}

Un dispositivo con muy baja movilidad. Por ejemplo, consola de juegos, decodificador y televisor.

### [!UICONTROL Time interval] {#time-interval-def}

También conocido como punto, es la ventana de tiempo que contiene la actividad de solicitud de reproducción representada en la interfaz de usuario y las tablas de principio a fin.

### [!UICONTROL Trend] {#trend-def}

Diferencia porcentual en la métrica asociada (por ejemplo, porcentaje del total de solicitudes de reproducción) entre el periodo actual y el anterior.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

Número de cuentas únicas que se han transmitido al menos una vez durante un período determinado.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

Más de 37 solicitudes de juego por mes.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

Menos de 9 solicitudes de reproducción al mes.

#### [!UICONTROL Regular user] {#regular-user-def}

De 9 a 37 solicitudes de juego por mes.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

Una de un conjunto finito de etiquetas de categoría aplicadas a una cuenta que mejor caracteriza a los usuarios de la cuenta en términos de grupos sociales o comportamientos (por ejemplo, una familia pequeña, un viajero o un viajero que viaja al trabajo, compartir en redes sociales, etc.).

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

Una categoría de vídeo es una etiqueta específica que califica la naturaleza del vídeo. Por ejemplo, la región del origen, los tipos de contenido, como VOD o en directo, el evento o etiquetas específicas como el equipo.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

Una categoría de vídeo se define mediante la combinación de Programador, canal y MVPD asociados al flujo.

### [!UICONTROL Zip Code] {#zip-code-def}

El código postal de EE. UU. asociado con ubicaciones dentro de EE. UU.
<!--calculated metrics-->


## Terminologías específicas de TV Everywhere

### [!UICONTROL AuthZ] {#authz-def}

Autorización o el número de solicitud de autorización. Una solicitud de autorización es el proceso mediante el cual un programador solicita permiso a una MVPD a través de un Adobe para comenzar a transmitir el contenido solicitado por un usuario. La MVPD suele conceder la solicitud en función de los derechos de contenido asociados con la suscripción de MVPD del usuario (por ejemplo, si el canal asociado con el contenido está en la suscripción del usuario). Algunas respuestas de solicitudes de autorización se almacenan en caché por Adobe, lo que permite que el Adobe responda inmediatamente sin pasar la solicitud al MVPD.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

Número de autorizaciones correctas.

### [!UICONTROL Channel] {#channel-def}

El canal, también conocido como Propiedad, es una fuente de contenido de vídeo relacionada temáticamente. Representar tradicionalmente una fuente de vídeo continua distinta y con direcciones numéricas de una MVPD. El canal se asigna directamente a un canal de contenido accesible disponible para los suscriptores a través de su Set Top Box (STB).

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Un valor calculado para cada uno de los índices de riesgo (cuentas, uso, total) en todos los programadores y MVPD durante el intervalo de tiempo seleccionado.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

Un tipo de análisis de uso compartido en el que la evaluación de una cuenta se limita a eventos que se produjeron directamente en los programadores del segmento seleccionado.  Normalmente, se evalúan todos los eventos de cuenta, lo que proporciona una estimación mucho más precisa del uso compartido.  Algunos datos de MVPD están estructurados de una manera que solo permite el análisis del modo de aislamiento.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD, también conocido como Distribuidor, es un agregador, revendedor y distribuidor de contenido de vídeo de Media Company.

### [!UICONTROL Programmer] {#programmer-def}

El programador, también conocido como Red, es una compañía subsidiaria de una compañía más grande (corporación) que posee y administra uno o más canales.

### [!UICONTROL requestorID] {#requestorid-def}

El ID que utiliza una compañía de medios para identificarse a sí misma o a una filial de una MVPD.  Según la implementación del programador, esto podría asignarse a una empresa de medios, un programador o un canal. Tradicionalmente, se asigna a un canal.  Con la creación de pseudocanales como MML (March Madness Live) y los movimientos impulsados técnicamente para abordar las limitaciones de datos impulsadas por MVPD, requestorID está empezando a asociarse más con Media Company.

### [!UICONTROL resourceID] {#resource-id-def}

El contenido solicitado por el usuario final. Tradicionalmente, esto ha identificado el canal asociado con el contenido que el usuario ha solicitado.  Las mejoras del sistema permiten que el ID represente programas específicos (por ejemplo, con clasificaciones específicas), y el ID sigue identificando el canal asociado.


