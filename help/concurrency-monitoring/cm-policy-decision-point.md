---
title: Punto de decisión de política
description: Punto de decisión de política
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---


# Punto de decisión de política {#policy-desc-pt}

## Modelo de dominio {#domain-model}

Esta página está pensada para servir como referencia para diferentes casos de uso e implementaciones de directivas. Le aconsejamos que consulte también el [Glosario](/help/concurrency-monitoring/cm-glossary.md) forma parte de la documentación de las definiciones de términos.

A **inquilino** posee **aplicaciones** para el que desea hacer cumplir **directivas**. **Aplicaciones cliente** debe configurarse con la variable **ID de aplicación** (proporcionado por el Adobe).

A continuación, el inquilino asocia cada aplicación con una o más directivas, creadas por él o creadas y compartidas por otros. Las políticas se pueden vincular entre varios inquilinos.

El **actividad del sujeto** consta de todos los flujos (independientemente de la aplicación) que se comunican al Servicio de monitorización de concurrencia para un tema determinado.

Cuando se va a autorizar una secuencia para un asunto determinado, el sistema comprobará primero todas las directivas definidas para la aplicación que creó la secuencia.

Para cada una de las políticas aplicables, es necesario recopilar todos los **actividad relevante** que se pasará a la regla. El **actividad relevante** para una política, P solo incluirá una secuencia S si cumple la siguiente condición:

**La secuencia &quot;S&quot; se inicia mediante una aplicación que incluye la política &quot;P&quot; entre sus directivas.**

![La secuencia &quot;S&quot; se inicia mediante una aplicación que incluye la política &quot;P&quot; entre sus directivas.](assets/pdp-domain-model.png)

## Casos de uso de Dry Run {#dry-run-use-cases}

El tutorial siguiente tiene como objetivo validar el modelo en algunos casos de uso. Lo haremos gradualmente, empezando con una configuración básica y agregando complejidad de varias maneras.

### 1. Un usuario. Una aplicación. Una póliza. Un flujo {#onetenant-oneapp-onepolicy-onestream}

Empezaremos con un solo inquilino, con una sola aplicación y una sola directiva asociada. Supongamos que la política establece que puede haber como máximo un flujo activo para cualquier usuario (se permite reproducir el flujo más reciente).

Una vez iniciado un flujo, la actividad solo consistirá en ese flujo y se le permitirá reproducir.

![Un inquilino. Una aplicación. Una póliza. Un flujo](assets/onetenant-app-policy-stream.png)


### 2. Un usuario. Una aplicación. Una póliza. Dos arroyos. {#onetenant-oneapp-onepolicy-twostreams}

Una vez que se inicia un segundo flujo (por el mismo sujeto que utiliza la misma aplicación), la actividad utilizada para la validación consistirá en lo siguiente **s1** y **s2**.

Se supera el límite porque la directiva establece que solo se permite reproducir un flujo, por lo que solo se permitirá el último flujo (**s2**) para jugar.

![Un inquilino. Una aplicación. Una póliza. Dos arroyos.](assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>Los diagramas representan la vista del sistema en la actividad del usuario. Para los intentos de inicialización de una secuencia, la decisión de acceso se incluirá en la respuesta. Para los flujos activos, la decisión se devolverá en la respuesta de latido.

### 3. Dos inquilinos. Dos aplicaciones. Una póliza. Dos arroyos. {#twotenant-twoapp-onepolicy-twostreams}

Supongamos ahora que un nuevo inquilino desea aplicar la misma directiva en sus aplicaciones:

![Dos inquilinos. Dos aplicaciones. Una póliza. Dos arroyos.](assets/onepolicy-twotenant-app-stream.png)

Debido a que los dos inquilinos están vinculados por la misma política, la situación descrita en el caso de uso 2 es aplicable aquí y **s3** se permite la reproducción, ya que es la última emisión.

### 4. Dos inquilinos. Tres aplicaciones. Dos políticas. Dos arroyos. {#twotenants-threeapps-twopolicies-twostreams}

Ahora, supongamos que el segundo inquilino implementa una nueva aplicación y desea definir una nueva directiva que se compartirá entre **app2** y **app3**.

![Dos inquilinos. Tres aplicaciones. Dos políticas. Dos arroyos.](assets/twotenant-policies-streams-threeapps.png)

En este momento, el flujo activo **s3** y **s4** están permitidos ambos. Para **s3**, cuando la directiva **P1** se evalúe, el sistema solo contará **s3** as **actividad relevante** (**s4** no está en modo alguno relacionado con la política **P1**), por lo que no hay ninguna infracción.

Política **P2** se aplica a ambos flujos e incluirá ambos **s3** y **s4** como actividad relevante. Como esta actividad se encuentra dentro de los límites de dos flujos, se permiten ambos flujos.

### 5. Dos inquilinos. Tres aplicaciones. Dos políticas. Tres arroyos. {#twotenants-threeapps-twopolicies-threestreams}

Ahora, suponiendo que se realice un nuevo intento de inicialización de flujo utilizando **app2**:

![Dos inquilinos. Tres aplicaciones. Dos políticas. Tres arroyos.](assets/twotenants-policies-threeapps-streams.png)

**s5** tiene permiso para empezar por **P1** (que permite que los flujos más nuevos se hagan cargo), pero lo deniega **P2**, para que no se inicie.

Lo mismo ocurrirá si se intenta iniciar una secuencia con app3: la misma directiva P2 denegará el acceso para ella.

![](assets/stream-init-attempted-app3.png)

Ahora, veamos qué sucedería si el usuario intenta crear un nuevo flujo con app1:

![](assets/new-stream-with-app1.png)

La aplicación app1 no está relacionada de ninguna manera con la directiva **P2**, por lo que solo se aplicará la directiva **P1**: que permite que se inicie el nuevo flujo y rechaza el más antiguo (**s3** en este caso).

