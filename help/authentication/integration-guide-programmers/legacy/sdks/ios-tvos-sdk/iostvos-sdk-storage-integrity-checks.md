---
title: Mecanismo de comprobación de integridad del almacenamiento de iOS/tvOS
description: Mecanismo de comprobación de integridad de iOS/tvOS
exl-id: 5d7cdc46-3e51-4e14-9e30-d7f48bc87506
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# Mecanismo de comprobación de integridad de iOS/tvOS {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>El contenido de esta página se proporciona únicamente con fines informativos. El uso de esta API requiere una licencia actual de Adobe. No se permite el uso no autorizado.

## Introducción {#Intro}

A partir de la versión 3.8.3 del SDK de AccessEnabler de iOS/tvOS, la opción de realizar comprobaciones de integridad del almacenamiento está disponible en la inicialización de AccessEnabler.

Para utilizar este mecanismo, la API se amplió con un método de inicialización adicional para la clase AccessEnabler.

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## Comprobaciones de integridad {#Checks}

Las comprobaciones de integridad del almacenamiento son útiles cuando se sospecha que el almacenamiento AccessEnabler está dañado (como cuando se produce una condición de carrera durante una operación de almacenamiento de lectura y escritura).

Las siguientes comprobaciones están disponibles para realizar en la inicialización de AccessEnabler:
- Funcionalidad de almacenamiento: comprueba el éxito en operaciones de lectura y escritura
- Integridad de valores almacenados: comprueba que todos los valores son válidos y están en el formato esperado

>[!IMPORTANT]
> 
>En caso de que una de las comprobaciones falle, todos los valores del almacenamiento se borran y se cierra la sesión del usuario, lo que puede provocar una mala experiencia de usuario. Utilice comprobaciones de integridad del almacenamiento solo cuando lo considere necesario.


## Comportamiento predeterminado {#Default}

Las comprobaciones de integridad del almacenamiento están desactivadas de forma predeterminada al inicializar AccessEnabler mediante el método de inicialización predeterminado:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

Para especificar explícitamente qué comprobaciones de integridad de almacenamiento se realizarán en la inicialización de AccessEnabler, utilice el siguiente método de inicialización:

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## IntegrityCheckType {#Switcher}

La enumeración IntegrityCheckType se expone a la aplicación cliente y tiene los siguientes valores:

| Valor | Comprobaciones realizadas | Almacenamiento borrado | Descripción | Caso de uso recomendado |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | Ninguno | Nunca | No se realizan comprobaciones de integridad en la inicialización del almacenamiento | Cuando los flujos del SDK funcionan según lo esperado |
| INTEGRITY_CHECK_ALL | Operabilidad de almacenamiento <br/> Validez de los valores almacenados | Error al comprobar | Todas las comprobaciones de integridad disponibles se realizan durante la inicialización del almacenamiento | Cuando se sospecha que el almacenamiento del SDK está dañado. <br/> En caso de que alguna de las comprobaciones de integridad falle, se cerrará la sesión del usuario |
| INTEGRITY_CHECK_CLEAR | Ninguno | Siempre | El almacenamiento se borra al inicializar el almacenamiento | Cuando los flujos del SDK no se pueden completar según lo esperado |
