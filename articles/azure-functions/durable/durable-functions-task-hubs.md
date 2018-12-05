---
title: 'Centrales de tareas en Durable Functions: Azure'
description: Aprenda qué son las centrales de tareas en la extensión Durable Functions para Azure Functions. Obtenga información acerca de cómo configurar las centrales de tareas.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 7a6346e594c5a7cc4cf02f3ea658aac4977e641a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52637460"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Centrales de tareas en Durable Functions (Azure Functions)

Una *central de tareas* en [Durable Functions](durable-functions-overview.md) es un contenedor lógico para los recursos de Azure Storage que se usan para las orquestaciones. Las funciones de orquestador y actividad solo pueden interactuar entre sí si pertenecen a la misma central de tareas.

Cada aplicación de función tiene una central de tareas independiente. Si varias aplicaciones de función comparten una cuenta de almacenamiento, esta contendrá varias centrales de tareas. El siguiente diagrama muestra una central de tareas por cada aplicación de función en las cuentas de almacenamiento compartidas y dedicadas.

![Diagrama de almacenamiento que muestra las cuentas de almacenamiento compartidas y dedicadas.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Recursos de Azure Storage

Una central de tareas consta de los siguientes recursos de almacenamiento: 

* Una o más colas de control.
* Una cola de elementos de trabajo.
* Una tabla de historial.
* Una tabla de instancias.
* Un contenedor de almacenamiento que contiene uno o varios blobs de concesión.

Todos estos recursos se crean automáticamente en la cuenta de Azure Storage predeterminada cuando se ejecutan funciones de orquestador o de actividad o cuando se programa su ejecución. El artículo [Rendimiento y escala](durable-functions-perf-and-scale.md) explica cómo se utilizan estos recursos.

## <a name="task-hub-names"></a>Nombres de las centrales de tareas

Las centrales de tareas se identifican mediante un nombre que se declara en el archivo *host.json*, como se muestra en el ejemplo siguiente:

### <a name="hostjson-functions-v1"></a>host.json (Functions v1)
```json
{
  "durableTask": {
    "HubName": "MyTaskHub"
  }
}
```
### <a name="hostjson-functions-v2"></a>host.json (Functions v2)
```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "HubName": "MyTaskHub"
    }
  }
}
```
Las centrales de tareas también se pueden configurar mediante la configuración de la aplicación, tal como se muestra en el siguiente archivo de ejemplo *host.json*:

### <a name="hostjson-functions-v1"></a>host.json (Functions v1)
```json
{
  "durableTask": {
    "HubName": "%MyTaskHub%"
  }
}
```
### <a name="hostjson-functions-v2"></a>host.json (Functions v2)
```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "HubName": "%MyTaskHub%"
    }
  }
}
```
El nombre de la central de tareas se establecerá en el valor de la configuración `MyTaskHub` de la aplicación. El siguiente valor de `local.settings.json` muestra cómo definir la configuración `MyTaskHub` como `samplehubname`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" :  "samplehubname" 
  }
}
```

Los nombres de la central de tareas deben empezar por una letra y estar formados únicamente por letras y números. Si no se especifica, el nombre predeterminado es **DurableFunctionsHub**.

> [!NOTE]
> El nombre es lo que diferencia una central de tareas de otra cuando hay varias de ellas en una cuenta de almacenamiento compartido. Si tiene varias aplicaciones de función que comparten una cuenta de almacenamiento, tendrá que configurar nombres diferentes para cada central de tareas en el archivo *host.json*.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda a controlar las versiones](durable-functions-versioning.md)
