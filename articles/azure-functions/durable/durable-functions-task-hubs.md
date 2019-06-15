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
ms.date: 12/07/2017
ms.author: azfuncdf
ms.openlocfilehash: 596eedab39ff926fcdc880c82c49ac464b7ff23b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60730281"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Centrales de tareas en Durable Functions (Azure Functions)

Una *central de tareas* en [Durable Functions](durable-functions-overview.md) es un contenedor lógico para los recursos de Azure Storage que se usan para las orquestaciones. Las funciones de orquestador y actividad solo pueden interactuar entre sí si pertenecen a la misma central de tareas.

Si varias aplicaciones de función comparten una cuenta de almacenamiento, se *debe* configurar cada una de ellas con un nombre de central de tareas independiente. Una cuenta de almacenamiento puede contener varias centrales de tareas. El siguiente diagrama muestra una central de tareas por cada aplicación de función en las cuentas de almacenamiento compartidas y dedicadas.

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

### <a name="hostjson-functions-1x"></a>host.json (Functions 1.x)

```json
{
  "durableTask": {
    "hubName": "MyTaskHub"
  }
}
```

### <a name="hostjson-functions-2x"></a>host.json (Functions 2.x)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "MyTaskHub"
    }
  }
}
```

Las centrales de tareas también se pueden configurar mediante la configuración de la aplicación, tal como se muestra en el siguiente archivo de ejemplo *host.json*:

### <a name="hostjson-functions-1x"></a>host.json (Functions 1.x)

```json
{
  "durableTask": {
    "hubName": "%MyTaskHub%"
  }
}
```

### <a name="hostjson-functions-2x"></a>host.json (Functions 2.x)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "%MyTaskHub%"
    }
  }
}
```

El nombre de la central de tareas se establecerá en el valor de la configuración `MyTaskHub` de la aplicación. El siguiente valor de `local.settings.json` muestra cómo definir la configuración `MyTaskHub` como `samplehubname`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" : "samplehubname"
  }
}
```

Este es un ejemplo precompilado en C# de cómo escribir una función que usa [OrchestrationClientBinding](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) para trabajar con una central de tareas configurada como configuración de aplicación:

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [OrchestrationClient(TaskHub = "%MyTaskHub%")] DurableOrchestrationClientBase starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

Y la siguiente es la configuración necesaria para JavaScript. La propiedad de la central de tareas del archivo `function.json` se establece mediante la configuración de aplicación:

```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

Los nombres de la central de tareas deben empezar por una letra y estar formados únicamente por letras y números. Si no se especifica, el nombre predeterminado es **DurableFunctionsHub**.

> [!NOTE]
> El nombre es lo que diferencia una central de tareas de otra cuando hay varias de ellas en una cuenta de almacenamiento compartido. Si tiene varias aplicaciones de función que comparten una cuenta de almacenamiento, deberá configurar explícitamente nombres diferentes para cada central de tareas en el archivo *host.json*. En caso contrario, las diversas aplicaciones de función competirán entre sí por los mensajes, lo cual podría provocar un comportamiento indefinido.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda a controlar las versiones](durable-functions-versioning.md)
