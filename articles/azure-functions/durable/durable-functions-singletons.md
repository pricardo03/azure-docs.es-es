---
title: 'Singletons en Durable Functions: Azure'
description: Aprenda a utilizar singletons en la extensión Durable Functions para Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 4832a48489a043493639bdedd6c6adf3c828de11
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53434705"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Orquestadores singleton con Durable Functions (Azure Functions)

Para trabajos en segundo plano, a menudo debe asegurarse de que solo una instancia de un determinado orquestador se ejecute a la vez. Esto puede hacerse en [Durable Functions](durable-functions-overview.md) mediante la asignación de un identificador de instancia determinado a un orquestador durante su creación.

## <a name="singleton-example"></a>Ejemplo de singleton

En los ejemplos de C# y JavaScript siguientes se muestra una función de desencadenador de HTTP que crea una orquestación de trabajo en segundo plano de singleton. El código garantiza que solo existe una instancia de un identificador de instancia especificado.

### <a name="c"></a>C#

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionsName;

    // Check if an instance with the specified ID already exists.
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance) {
        // An instance with the specified ID doesn't exist, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

De forma predeterminada, los identificadores de instancia son identificadores únicos globales generados aleatoriamente. Pero, en este caso, el identificador de instancia se pasa en los datos de ruta desde la dirección URL. El código llama a [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) (C#) o `getStatus` (JavaScript) para comprobar si ya se está ejecutando una instancia con el identificador especificado. Si no es así, se crea una instancia con ese identificador.

> [!WARNING]
> Al desarrollar de forma local en JavaScript, deberá establecer la variable de entorno `WEBSITE_HOSTNAME` en `localhost:<port>`, por ejemplo `localhost:7071` para usar métodos en `DurableOrchestrationClient`. Para obtener más información sobre este requisito, vea el [problema de GitHub](https://github.com/Azure/azure-functions-durable-js/issues/28).

> [!NOTE]
> En este ejemplo hay una posible condición de carrera. Si dos instancias de **HttpStartSingle** se ejecutan al mismo tiempo puede que el resultado sean dos instancias creadas diferentes del singleton, en la que una sobrescribirá a la otra. Según los requisitos, esto puede tener efectos secundarios no deseados. Por este motivo, es importante asegurarse de que no hay dos solicitudes que puedan ejecutar esta función de desencadenador simultáneamente.

Los detalles de implementación de la función de orquestador no importan realmente. Podría tratarse de una función de orquestador normal, con un principio y un final, o de una que se ejecuta sin fin (es decir, una [orquestación infinita](durable-functions-eternal-orchestrations.md)). Lo importante es que haya solo una instancia en ejecución cada vez.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo llamar a suborquestaciones](durable-functions-sub-orchestrations.md)
