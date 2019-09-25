---
title: 'Suborquestaciones en Durable Functions: Azure'
description: Aprenda a llamar orquestaciones desde otras orquestaciones en la extensión Durable Functions para Azure Functions.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/07/2019
ms.author: azfuncdf
ms.openlocfilehash: 7b5e811daecbb7687abe7a37b75e2730d7830c2c
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70983616"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Suborquestaciones en Durable Functions (Azure Functions)

Además de llamar a funciones de actividad, las funciones de orquestador pueden llamar a otras funciones de orquestador. Por ejemplo, puede crear una orquestación mayor a partir de una biblioteca de funciones de orquestador. También puede ejecutar varias instancias de una función de orquestador en paralelo.

Una función de orquestador puede llamar a otra función de orquestador mediante una llamada a los métodos [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) o [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) en .NET o a los métodos `callSubOrchestrator` o `callSubOrchestratorWithRetry` en JavaScript. En el artículo [Control de errores y compensación](durable-functions-error-handling.md#automatic-retry-on-failure) se ofrece más información sobre los reintentos automáticos.

Las funciones de suborquestador se comportan como funciones de actividad desde la perspectiva del llamador. Pueden devolver un valor, producir una excepción y ser esperadas por la función de orquestador primaria. 

> [!NOTE]
> Actualmente, es necesario proporcionar un valor de argumento `instanceId` a la API de suborquestación en JavaScript.

## <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra un escenario de IoT ("Internet de las cosas") donde hay varios dispositivos que deben aprovisionarse. Para cada uno de los dispositivos, debe producirse una orquestación concreta, lo que podría asemejarse a lo siguiente:

### <a name="c"></a>C#

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string deviceId = context.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await context.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await context.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await context.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceId = context.df.getInput();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    const sasUrl = yield context.df.callActivity("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    yield context.df.callActivity("SendPackageUrlToDevice", { id: deviceId, url: sasUrl });

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    yield context.df.waitForExternalEvent("DownloadCompletedAck");

    // Step 4: ...
});
```

Esta función de orquestador puede utilizarse tal cual para un aprovisionamiento de dispositivos único o puede ser parte de una orquestación mayor. En el último caso, la función de orquestador primaria puede programar instancias de `DeviceProvisioningOrchestration` mediante la API `CallSubOrchestratorAsync` (C#) o `callSubOrchestrator` (JavaScript).

El siguiente es un ejemplo que muestra cómo ejecutar varias funciones de orquestador en paralelo.

### <a name="c"></a>C#

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string[] deviceIds = await context.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = context.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const deviceIds = yield context.df.callActivity("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    const provisioningTasks = [];
    var id = 0;
    for (const deviceId of deviceIds) {
        const child_id = context.df.instanceId+`:${id}`;
        const provisionTask = context.df.callSubOrchestrator("DeviceProvisioningOrchestration", deviceId, child_id);
        provisioningTasks.push(provisionTask);
        id++;
    }

    yield context.df.Task.all(provisioningTasks);

    // ...
});
```

> [!NOTE]
> Las suborquestaciones se deben definir en la misma aplicación de funciones que la orquestación primaria. Si necesita llamar a y esperar a las orquestaciones en otra aplicación de funciones, considere la posibilidad de usar la compatibilidad integrada para las API HTTP y el patrón de consumidor de sondeo HTTP 202. Para más información, consulte el tema [Características HTTP](durable-functions-http-features.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Obtener información acerca de cómo establecer el estado de una orquestación personalizada](durable-functions-custom-orchestration-status.md)