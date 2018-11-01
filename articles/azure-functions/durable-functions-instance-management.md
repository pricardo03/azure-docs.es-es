---
title: 'Administración de instancias con Durable Functions: Azure'
description: Aprenda a administrar instancias en la extensión Durable Functions para Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: azfuncdf
ms.openlocfilehash: bcb87b3030eb673dd3c9a8b93a045c75fae964a9
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50087039"
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Administración de instancias con Durable Functions (Azure Functions)

Las instancias de orquestación de [Durable Functions](durable-functions-overview.md) pueden iniciarse, finalizarse, consultarse y recibir eventos de notificación. Toda la administración de instancias se realiza mediante el [enlace de clientes de orquestación](durable-functions-bindings.md). En este artículo se describen los detalles de cada operación de administración de instancias.

## <a name="starting-instances"></a>Inicio de instancias

El método [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) en [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) inicia una nueva instancia de una función de orquestador. Las instancias de esta clase pueden adquirirse mediante el enlace `orchestrationClient`. Internamente, este método pone un mensaje en la cola de control, que, a su vez, desencadena el inicio de una función con el nombre especificado que utiliza el enlace de desencadenador `orchestrationTrigger`. 

La tarea se completa cuando se inicia el proceso de orquestación. El proceso de orquestación debería comenzar en 30 segundos. Si tarda más tiempo, se produce una excepción `TimeoutException`. 

Los parámetros de [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) son los siguientes:

* **Name**: el nombre de la función de orquestador que programar.
* **Input**: todos los datos serializables con JSON que deben pasarse como entrada a la función de orquestador.
* **InstanceId** (opcional): el identificador único de la instancia. Si no se especifica, se generará un identificador de instancia aleatorio.

A continuación, se muestra un ejemplo de C# sencillo:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
}
```

Para los lenguajes que no son de .NET, el enlace de salida de la función también se puede usar para iniciar nuevas instancias. En este caso, puede utilizarse cualquier objeto que se pueda serializar con JSON que tenga como campos los tres parámetros anteriores. Por ejemplo, considere la siguiente función de JavaScript:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null);
};
```
En el código anterior se supone que, en el archivo function.json, ha definido un enlace de salida con el nombre "starter" y el tipo "orchestrationClient". Si el enlace no está definido, no se creará la instancia de función duradera.

Para que la instancia de función duradera se invoque, el archivo function.json debe modificarse de forma que contenga un enlace para el cliente de orquestación, tal como se describe a continuación

```js
{
    "bindings": [{
        "name":"starter",
        "type":"orchestrationClient",
        "direction":"out"
    }]
}
```

> [!NOTE]
> Se recomienda que use un identificador aleatorio para el identificador de la instancia. Esto ayudará a garantizar una distribución equitativa de la carga al escalar las funciones de orquestador entre varias máquinas virtuales. El momento adecuado para usar identificadores de instancia no aleatorios es cuando el identificador debe proceder de un origen externo o cuando se implementa el patrón de [orquestador singleton](durable-functions-singletons.md).

## <a name="querying-instances"></a>Consulta de instancias

El método [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) de la clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) consulta el estado de una instancia de orquestación. Toma los valores `instanceId` (obligatorio), `showHistory` (opcional) y `showHistoryOutput` (opcional) como parámetros. Si `showHistory` se establece en `true`, la respuesta contendrá el historial de ejecución. Si `showHistoryOutput` se establece en `true`, el historial de ejecución contendrá salidas de actividad. El método devuelve un objeto con las siguientes propiedades:

* **Name**: el nombre de la función de orquestador.
* **InstanceId**: el identificador de instancia de la orquestación (debe ser el mismo que la entrada `instanceId`).
* **CreatedTime**: la hora en que la función de orquestador empezó a ejecutarse.
* **LastUpdatedTime**: la hora a la que la orquestación estableció el último punto de control.
* **Input**: la entrada de la función como un valor JSON.
* **CustomStatus**: estado de orquestación personalizada en formato JSON. 
* **Output**: la salida de la función como un valor JSON (si se ha completado la función). Si se produce un error en la función de orquestador, esta propiedad incluirá los detalles del error. Si se finaliza la función de orquestador, esta propiedad incluirá el motivo de la finalización indicado (si lo hubiera).
* **RuntimeStatus**: uno de los siguientes valores:
    * **Pending**: la instancia se ha programado, pero aún no ha empezado a ejecutarse.
    * **Running**: la instancia ha empezado a ejecutarse.
    * **Completed**: la instancia se ha completado con normalidad.
    * **ContinuedAsNew**: la instancia se ha reiniciado con un nuevo historial. Este es un estado transitorio.
    * **Failed**: se ha producido un error en la instancia.
    * **Terminated**: la instancia se ha finalizado abruptamente.
* **History**: el historial de ejecución de la orquestación. Este campo solo se rellena si `showHistory` está establecido en `true`.
    
Este método devuelve `null` si la instancia no existe o no ha empezado a ejecutarse.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```
## <a name="querying-all-instances"></a>Consulta de todas las instancias

Puede usar el método `GetStatusAsync` para consultar los estados de todas las instancias de orquestación. No es necesario ningún parámetro y puede pasar un objeto `CancellationToken` en caso de que desee cancelar la consulta. El método devuelve objetos con las mismas propiedades que el método `GetStatusAsync` con parámetros, salvo que no devuelve el historial. 

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```
## <a name="querying-instances-with-filters"></a>Consulta de instancias con filtros

También puede usar el método `GetStatusAsync` para obtener una lista de instancias de orquestación que coinciden con un conjunto de filtros predefinidos. Las opciones de filtro posibles incluyen la hora de creación de la orquestación y el estado en tiempo de ejecución de la orquestación.

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IEnumerable<OrchestrationRuntimeStatus> runtimeStatus = new List<OrchestrationRuntimeStatus> {
        OrchestrationRuntimeStatus.Completed,
        OrchestrationRuntimeStatus.Running
    };
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(
        new DateTime(2018, 3, 10, 10, 1, 0),
        new DateTime(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    ); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

## <a name="terminating-instances"></a>Finalización de instancias

Una instancia de orquestación en ejecución se puede finalizar mediante el método [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) de la clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html). Los dos parámetros son `instanceId` y una cadena `reason`, que se escribirá en los registros y en el estado de la instancia. Una instancia finalizada dejará de ejecutarse tan pronto como alcance el siguiente punto `await` o bien se finalizará inmediatamente si ya está en un punto `await`. 

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> La finalización de la instancia no se propaga actualmente. Las funciones de actividad y las suborquestaciones se ejecutarán hasta completarse, independientemente de si ha finalizado la instancia de orquestación que las llamó.

## <a name="sending-events-to-instances"></a>Envío de eventos a instancias

Pueden enviarse notificaciones de eventos a instancias en ejecución mediante el método [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) de la clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html). Las instancias que pueden controlar estos eventos son aquellas que están en espera de una llamada a [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

Los parámetros de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) son los siguientes:

* **InstanceId**: el identificador único de la instancia.
* **EventName**: el nombre del evento que se va a enviar.
* **EventData**: una carga que se puede serializar con JSON que enviar a la instancia.

```csharp
[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!WARNING]
> Si no hay ninguna instancia de orquestación con el *identificador de instancia* especificado o si la instancia no está esperando el *nombre de evento* especificado, se descarta el mensaje de evento. Para más información acerca de este comportamiento, consulte el [problema de GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="wait-for-orchestration-completion"></a>Esperar a que finalice la orquestación

La clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) expone una API [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) que se puede usar para obtener la salida real de manera sincrónica de una instancia de orquestación. El método usa un valor predeterminado de 10 segundos para `timeout` y de 1 segundo para `retryInterval` cuando no se establecen.  

Este ejemplo de función que se desencadena mediante HTTP muestra cómo utilizar la API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

La función se puede llamar con la siguiente línea mediante el tiempo de espera de 2 segundos y el intervalo de reintento de 0,5 segundos:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Dependiendo del tiempo necesario para obtener la respuesta de la instancia de orquestación, se dan dos casos:

1. Las instancias de orquestación se completan dentro del tiempo de espera definido (en este caso 2 segundos); la respuesta es la salida de la instancia de orquestación real entregada de manera sincrónica:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

2. Las instancias de orquestación no se pueden completar dentro del tiempo de espera definido (en este caso 2 segundos); la respuesta es la predeterminada descrita en **HTTP API URL discovery** (Detección de la URL de API HTTP):

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}",
            "rewindPostUri": "https://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/rewind?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> El formato de las direcciones URL del webhook puede diferir en función de la versión del host de Azure Functions en ejecución. El ejemplo anterior es para el host de Azure Functions 2.0.

## <a name="retrieving-http-management-webhook-urls"></a>Recuperación de las direcciones URL del webhook de administración de HTTP

Los sistemas externos se pueden comunicar con Durable Functions a través de direcciones URL de webhook que forman parte de la respuesta predeterminada descrita en [Detección de la dirección URL de la API de HTTP](durable-functions-http-api.md). No obstante, se puede acceder también a las direcciones URL de webhook mediante programación en el cliente de orquestación o en una función de actividad mediante el método [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) de la clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html). 

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) tiene un parámetro:

* **instanceId**: el identificador único de la instancia.

El método devuelve una instancia de la carga [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) con las siguientes propiedades de cadena:

* **Id**: el identificador de instancia de la orquestación (debe ser el mismo que la entrada `InstanceId`).
* **StatusQueryGetUri**: la dirección URL del estado de la instancia de orquestación.
* **SendEventPostUri**: la dirección URL de generación del evento de la instancia de orquestación.
* **TerminatePostUri**: la dirección URL de generación del evento de la instancia de orquestación.
* **RewindPostUri**: la dirección URL de "rebobinado" de la instancia de orquestación.

Las funciones de la actividad pueden enviar una instancia de [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) a sistemas externos para supervisar o generar eventos en una orquestación:

```csharp
[FunctionName("SendInstanceInfo")]
public static void SendInstanceInfo(
    [ActivityTrigger] DurableActivityContext ctx,
    [OrchestrationClient] DurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

## <a name="rewinding-instances-preview"></a>Rebobinado de instancias (versión preliminar)

Una instancia de orquestación con error se puede *rebobinar* de vuelta a un estado correcto anterior mediante la API [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_). Funciona colocando la orquestación de nuevo en el estado *En ejecución* y ejecutando nuevamente los errores de la actividad o de la ejecución de suborquestación que produjeron el error en la orquestación.

> [!NOTE]
> Esta API no pretende ser un sustituto para el control de errores y las directivas de reintentos pertinentes. En su lugar, el objetivo es que se use solo en casos donde las instancias de orquestación producen un error por razones inesperadas. Para obtener más detalles sobre el control de errores y las directivas de reintento, consulte el tema sobre [control de errores](durable-functions-error-handling.md).

Un ejemplo de caso de uso para el *rebobinado* es un flujo de trabajo que requiere de una serie de [aprobaciones realizadas por humanos](durable-functions-overview.md#pattern-5-human-interaction). Suponga que hay una serie de funciones de actividad que notifican a alguien cuando se requiere su aprobación y que esperan la respuesta en tiempo real. Después de que todas las actividades de aprobación han recibido respuestas o se ha agotado el tiempo de espera, otra actividad devuelve un error debido a un error de configuración en la aplicación (por ejemplo, una cadena de conexión de base de datos no válida). El resultado es un error de orquestación en el flujo de trabajo. Con la API `RewindAsync`, un administrador de aplicaciones puede corregir el error de configuración y *rebobinar* la orquestación con error de vuelta al estado inmediatamente anterior al error. Ninguno de los pasos de interacción humana deben volverse a aprobar, y la orquestación ahora se puede completar correctamente.

> [!NOTE]
> La característica para *rebobinar* no admite el rebobinado de instancias de orquestación que utilizan temporizadores durables.

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información acerca de cómo usar las API HTTP para la instancia de administración](durable-functions-http-api.md)
