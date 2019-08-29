---
title: 'Administración de instancias con Durable Functions: Azure'
description: Aprenda a administrar instancias en la extensión Durable Functions para Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 6548b84f9599116aaa5055324bfa4625ea621ec3
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087247"
---
# <a name="manage-instances-in-durable-functions-in-azure"></a>Administración de instancias con Durable Functions en Azure

Si usa la extensión de [Durable Functions](durable-functions-overview.md) para Azure Functions, o quiere empezar a hacerlo, asegúrese de hacerlo de la mejor manera posible. Puede optimizar las instancias de orquestación de Durable Functions aprendiendo más sobre cómo administrarlas. En este artículo se describen los detalles de cada operación de administración de instancias.

Puede iniciar y finalizar instancias, por ejemplo, y puede consultar instancias, con la posibilidad de consultar todas las instancias y consultar instancias con filtros. Además, puede enviar eventos a instancias, esperar a que finalice la orquestación y recuperar direcciones URL del webhook de administración de HTTP. En este artículo se tratan también otras operaciones de administración, como rebobinar instancias, purgar el historial de instancias y eliminar una central de tareas.

En Durable Functions, dispone de opciones para el modo en que desea implementar cada una de estas operaciones de administración. En este artículo se proporcionan ejemplos que utilizan [Azure Functions Core Tools](../functions-run-local.md) para .NET (C#) y JavaScript.

## <a name="start-instances"></a>Inicio de instancias

Es importante ser capaz de iniciar una instancia de orquestación. Normalmente, esto se hace cuando se usa un enlace de Durable Functions en el desencadenador de otra función.

El método [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) en [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) o `startNew` en `DurableOrchestrationClient`(JavaScript) inicia una nueva instancia. Las instancias de esta clase pueden adquirirse mediante el enlace `orchestrationClient`. Internamente, este método pone un mensaje en la cola de control, que, a su vez, desencadena el inicio de una función con el nombre especificado que utiliza el enlace de desencadenador `orchestrationTrigger`.

Esta operación de sincronización se completa cuando el proceso de orquestación se programa correctamente. El proceso de orquestación debería comenzar en 30 segundos. Si tarda más tiempo, verá `TimeoutException`.

> [!WARNING]
> Al desarrollar de forma local en JavaScript, establezca la variable de entorno `WEBSITE_HOSTNAME` en `localhost:<port>` (por ejemplo, `localhost:7071`) para usar métodos en `DurableOrchestrationClient`. Para más información sobre este requisito, consulte el [problema en GitHub](https://github.com/Azure/azure-functions-durable-js/issues/28).

### <a name="net"></a>.NET

Los parámetros de [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) son los siguientes:

* **Nombre**: el nombre de la función de orquestador que programar.
* **Entrada**: todos los datos serializables con JSON que deben pasarse como entrada a la función de orquestador.
* **InstanceId**: (Opcional) El identificador único de la instancia. Si no se especifica este parámetro, el método utiliza un identificador aleatorio.

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

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

Los parámetros de `startNew` son los siguientes:

* **Nombre**: el nombre de la función de orquestador que programar.
* **InstanceId**: (Opcional) El identificador único de la instancia. Si no se especifica este parámetro, el método utiliza un identificador aleatorio.
* **Entrada**: (Opcional) Todos los datos serializables con JSON que deben pasarse como entrada a la función de orquestador.

Este es un ejemplo sencillo de JavaScript:

```javascript
const df = require("durable-functions");

module.exports = async function(context, input) {
    const client = df.getClient(context);

    const instanceId = await client.startNew("HelloWorld", undefined, input);
    context.log(`Started orchestration with ID = ${instanceId}.`);
};
```

> [!TIP]
> Use un identificador aleatorio para el identificador de la instancia. Esto ayuda a garantizar una distribución equitativa de la carga al escalar las funciones de orquestador entre varias máquinas virtuales. El momento adecuado para usar identificadores de instancia no aleatorios es cuando el identificador debe proceder de un origen externo o cuando implementa el patrón de [orquestador singleton](durable-functions-singletons.md).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

También es posible iniciar una instancia directamente a través del comando `durable start-new` de [Azure Functions Core Tools](../functions-run-local.md). Toma los parámetros siguientes:

* **`function-name` (obligatorio)** : nombre de la función que se va a iniciar.
* **`input` (opcional)** : entrada a la función, ya sea en línea o a través de un archivo JSON. En el caso de los archivos, agregue un prefijo a la ruta de acceso al archivo con `@`, como `@path/to/file.json`.
* **`id` (opcional)** : identificador de la instancia de orquestación. Si no se especifica este parámetro, el método utiliza un GUID aleatorio.
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es DurableFunctionsHub. También puede establecerlo en [host.json](durable-functions-bindings.md#host-json) utilizando durableTask:HubName.

> [!NOTE]
> Los comandos de Core Tools suponen que los ejecuta desde el directorio raíz de una aplicación de función. Si proporciona explícitamente los parámetros `connection-string-setting` y `task-hub-name`, puede ejecutar los comandos desde cualquier directorio. Aunque puede ejecutar estos comandos sin la ejecución de un host de la aplicación de función, es posible que no pueda observar algunos efectos propios de una situación en la que el host se está ejecutando. Por ejemplo, el comando `start-new` pone en cola un mensaje de inicio en el centro de tareas de destino, pero la orquestación no se ejecuta realmente a menos que haya un proceso de host de la aplicación de función en ejecución que pueda procesar el mensaje.

El siguiente comando inicia la función denominada HelloWorld y le pasa el contenido del archivo `counter-data.json`:

```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="query-instances"></a>Consulta de instancias

Como parte de su esfuerzo para administrar las orquestaciones, probablemente deberá recopilar información sobre el estado de una instancia de orquestación (por ejemplo, si se ha completado con normalidad o no).

El método [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) de la clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) o el método `getStatus` en la clase `DurableOrchestrationClient` (JavaScript) consulta el estado de una instancia de orquestación.

Toma los valores `instanceId` (obligatorio), `showHistory` (opcional), `showHistoryOutput` (opcional) y `showInput` (opcional, solo para .NET) como parámetros.

* **`showHistory`** : si se establece en `true`, la respuesta contiene el historial de ejecución.
* **`showHistoryOutput`** : si se establece en `true`, el historial de ejecución contiene salidas de actividad.
* **`showInput`** : si se establece en `false`, la respuesta no contendrá la entrada de la función. El valor predeterminado es `true`. (Solo para .NET)

El método devuelve un objeto JSON con las siguientes propiedades:

* **Nombre**: el nombre de la función de orquestador.
* **InstanceId**: el identificador de instancia de la orquestación (debe ser el mismo que la entrada `instanceId`).
* **CreatedTime**: la hora en que la función de orquestador empezó a ejecutarse.
* **LastUpdatedTime**: la hora a la que la orquestación estableció el último punto de control.
* **Entrada**: la entrada de la función como un valor JSON. Este campo no se rellena si `showInput` es false.
* **CustomStatus**: estado de orquestación personalizada en formato JSON.
* **Salida**: la salida de la función como un valor JSON (si se ha completado la función). Si se produce un error en la función de orquestador, esta propiedad incluye los detalles del error. Si se finaliza la función de orquestador, esta propiedad incluye el motivo de la finalización (si lo hubiera).
* **RuntimeStatus**: uno de los valores siguientes:
  * **Pending**: la instancia se ha programado, pero aún no ha empezado a ejecutarse.
  * **Running**: la instancia ha empezado a ejecutarse.
  * **Completed**: la instancia se ha completado con normalidad.
  * **ContinuedAsNew**: la instancia se ha reiniciado con un nuevo historial. Este es un estado transitorio.
  * **Failed**: se ha producido un error en la instancia.
  * **Terminated**: la instancia se ha detenido abruptamente.
* **History**: el historial de ejecución de la orquestación. Este campo solo se rellena si `showHistory` está establecido en `true`.

Este método devuelve `null` si la instancia no existe o no ha empezado a ejecutarse.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const status = await client.getStatus(instanceId);
    // do something based on the current status.
}
```

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

También es posible obtener el estado de una instancia de orquestación directamente a través del comando `durable get-runtime-status` de [Azure Functions Core Tools](../functions-run-local.md). Toma los parámetros siguientes:

* **`id` (obligatorio)** : identificador de la instancia de orquestación.
* **`show-input` (opcional)** : si se establece en `true`, la respuesta contiene la entrada de la función. El valor predeterminado es `false`.
* **`show-output` (opcional)** : si se establece en `true`, la respuesta contiene la salida de la función. El valor predeterminado es `false`.
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en [host.json](durable-functions-bindings.md#host-json) mediante durableTask:HubName.

El siguiente comando recupera el estado (incluidas la entrada y salida) de una instancia con un identificador de instancia de orquestación de 0ab8c55a66644d68a3a8b220b12d209c. Se supone que está ejecutando el comando `func` desde el directorio raíz de la aplicación de función:

```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

Puede utilizar el comando `durable get-history` para recuperar el historial de una instancia de orquestación. Toma los parámetros siguientes:

* **`id` (obligatorio)** : identificador de la instancia de orquestación.
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en host.json mediante durableTask:HubName.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="query-all-instances"></a>Consulta de todas las instancias

En lugar de consultar las instancias de la orquestación de una en una, puede resultarle más eficaz consultarlas todas a la vez.

Puede usar el método `GetStatusAsync` (.NET) o `getStatusAll` (JavaScript) para consultar los estados de todas las instancias de orquestación. En .NET se puede pasar un objeto `CancellationToken` en caso de que desee cancelarlo. El método devuelve objetos con las mismas propiedades que el método `GetStatusAsync` con parámetros.

### <a name="c"></a>C#

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await client.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instances = await client.getStatusAll();
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

También es posible consultar instancias directamente a través del comando `durable get-instances` de [Azure Functions Core Tools](../functions-run-local.md). Toma los parámetros siguientes:

* **`top` (opcional)** : este comando admite la paginación. Este parámetro corresponde al número de instancias recuperadas por solicitud. El valor predeterminado es 10.
* **`continuation-token` (opcional)** : un token para indicar qué página o sección de instancias se va a recuperar. Cada ejecución `get-instances` devuelve un token para el siguiente conjunto de instancias.
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en [host.json](durable-functions-bindings.md#host-json) mediante durableTask:HubName.

```bash
func durable get-instances
```

## <a name="query-instances-with-filters"></a>Consulta de instancias con filtros

¿Qué ocurre si no necesita realmente toda la información que puede proporcionar una consulta de instancias estándar? Por ejemplo, ¿qué sucede si solo desea obtener la hora de creación de la orquestación o el estado del entorno de ejecución de la orquestación? Puede restringir la consulta aplicando filtros.

Use el método `GetStatusAsync` (.NET) o `getStatusBy` (JavaScript) para obtener una lista de instancias de orquestación que coinciden con un conjunto de filtros predefinidos.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const runtimeStatus = [
        df.OrchestrationRuntimeStatus.Completed,
        df.OrchestrationRuntimeStatus.Running,
    ];
    const instances = await client.getStatusBy(
        new Date(2018, 3, 10, 10, 1, 0),
        new Date(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    );
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

En Azure Functions Core Tools, también se puede usar el comando `durable get-instances` con filtros. Además de los parámetros `top`, `continuation-token`, `connection-string-setting` y `task-hub-name` mencionados anteriormente, puede usar tres parámetros de filtro (`created-after`, `created-before` y `runtime-status`).

* **`created-after` (opcional)** : recuperación de las instancias creadas después de esta fecha y hora (UTC). Se aceptan valores de datetime con formato ISO 8601.
* **`created-before` (opcional)** : recuperación de las instancias creadas antes de esta fecha y hora (UTC). Se aceptan valores de datetime con formato ISO 8601.
* **`runtime-status` (opcional)** : Recupere las instancias con un estado determinado (por ejemplo, en ejecución o completado). Puede proporcionar varios estados (separados por espacios).
* **`top` (opcional)** : número de instancias recuperadas por solicitud. El valor predeterminado es 10.
* **`continuation-token` (opcional)** : un token para indicar qué página o sección de instancias se va a recuperar. Cada ejecución `get-instances` devuelve un token para el siguiente conjunto de instancias.
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en [host.json](durable-functions-bindings.md#host-json) mediante durableTask:HubName.

Si no proporciona ningún filtro (`created-after`, `created-before`, o `runtime-status`), el comando simplemente recupera instancias `top`, sin tener en cuenta la hora de creación o el estado del entorno de ejecución.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
```

## <a name="terminate-instances"></a>Finalización de instancias

Si tiene una instancia de orquestación que está tardando demasiado tiempo en ejecutarse, o si simplemente tiene que detenerla antes por cualquier motivo, tiene la opción de finalizarla.

Puede usar el método [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) de la clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) o el método `terminate` de la clase `DurableOrchestrationClient` (JavaScript). Los dos parámetros son `instanceId` y una cadena `reason`, que se escriben en los registros y en el estado de la instancia. Una instancia finalizada deja de ejecutarse tan pronto como alcance el siguiente punto `await` (.NET) o `yield` (JavaScript), o finaliza inmediatamente si ya está en `await` o `yield`.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "It was time to be done.";
    return client.terminate(instanceId, reason);
};
```

> [!NOTE]
> La finalización de la instancia no se propaga actualmente. Las funciones de actividad y las suborquestaciones se ejecutan hasta completarse, independientemente de si ha finalizado la instancia de orquestación que las llamó.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

También es posible finalizar una instancia de orquestación directamente a través del comando `durable terminate` de [Azure Functions Core Tools](../functions-run-local.md). Toma los parámetros siguientes:

* **`id` (obligatorio)** : identificador de la instancia de orquestación que se va a finalizar.
* **`reason` (opcional)** : motivo de la finalización.
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en [host.json](durable-functions-bindings.md#host-json) mediante durableTask:HubName.

El siguiente comando finaliza una instancia de orquestación con un identificador de 0ab8c55a66644d68a3a8b220b12d209c:

```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="send-events-to-instances"></a>Envío de eventos a instancias

En algunos escenarios, para las funciones del orquestador es importante poder esperar y escuchar eventos externos. Esto incluye [supervisar funciones](durable-functions-concepts.md#monitoring) y funciones que están esperando la [interacción humana](durable-functions-concepts.md#human).

Envíe notificaciones de eventos a instancias en ejecución mediante el método [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) de la clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) o el método `raiseEvent` de la clase `DurableOrchestrationClient` (JavaScript). Las instancias que pueden controlar estos eventos son aquellas que están en espera de una llamada a [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) (.NET) o `waitForExternalEvent` (JavaScript).

Los parámetros de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) (.NET) y `raiseEvent` (JavaScript) son los siguientes:

* **InstanceId**: el identificador único de la instancia.
* **EventName**: el nombre del evento que se va a enviar.
* **EventData**: una carga que se puede serializar con JSON que enviar a la instancia.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const eventData = [ 1, 2, 3 ];
    return client.raiseEvent(instanceId, "MyEvent", eventData);
};
```

> [!IMPORTANT]
> Si no hay ninguna instancia de orquestación con el identificador de instancia especificado o si la instancia no está esperando el nombre de evento especificado, se descarta el mensaje de evento. Para más información acerca de este comportamiento, consulte el [problema de GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/29).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

También es posible generar un evento para una instancia de orquestación directamente a través del comando `durable raise-event` de [Azure Functions Core Tools](../functions-run-local.md). Toma los parámetros siguientes:

* **`id` (obligatorio)** : identificador de la instancia de orquestación.
* **`event-name` (opcional)** : nombre del evento que se va a generar. El valor predeterminado es `$"Event_{RandomGUID}"`.
* **`event-data` (opcional)** : datos que se van a enviar a la instancia de orquestación. Puede ser la ruta de acceso a un archivo JSON, o puede proporcionar los datos directamente en la línea de comandos.
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en [host.json](durable-functions-bindings.md#host-json) mediante durableTask:HubName.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```

```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Esperar a que finalice la orquestación

En las orquestaciones de larga ejecución, quizá prefiera esperar y obtener los resultados de una orquestación. En estos casos, también resulta útil poder definir un período de tiempo de espera en la orquestación. Si se agota el tiempo de espera, se debe devolver el estado de la orquestación en lugar de los resultados.

La clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) expone una API [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) en. NET. Puede utilizar esta API para obtener la salida real de una instancia de orquestación de forma sincrónica. En JavaScript, la clase `DurableOrchestrationClient` expone una `waitForCompletionOrCreateCheckStatusResponse` API para el mismo propósito. Cuando no se establecen, el método usa un valor predeterminado de 10 segundos para `timeout` y de 1 segundo para `retryInterval`.  

Este ejemplo de función que se desencadena mediante HTTP muestra cómo utilizar la API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpSyncStart/index.js)]

Llame a la función con la siguiente línea. Utilice 2 segundos para el tiempo de espera y 0,5 segundos para el intervalo de reintento:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Dependiendo del tiempo necesario para obtener la respuesta de la instancia de orquestación, se dan dos casos:

* Las instancias de orquestación se completan dentro del tiempo de espera definido (en este caso 2 segundos); y la respuesta es la salida de la instancia de orquestación real entregada de manera sincrónica:

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

* Las instancias de orquestación no se pueden completar dentro del tiempo de espera definido, y la respuesta es la predeterminada descrita en [Detección de la dirección URL de la API de HTTP](durable-functions-http-api.md):

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
> El formato de las direcciones URL del webhook puede diferir en función de la versión del host de Azure Functions en ejecución. El ejemplo anterior es para el host de Azure Functions 2.x.

## <a name="retrieve-http-management-webhook-urls"></a>Recuperación de las direcciones URL del webhook de administración de HTTP

Puede usar un sistema externo para supervisar o generar eventos en una orquestación. Los sistemas externos se pueden comunicar con Durable Functions a través de direcciones URL de webhook que forman parte de la respuesta predeterminada descrita en [Detección de la dirección URL de la API de HTTP](durable-functions-http-api.md). Sin embargo, también es posible acceder a las direcciones URL del webhook mediante programación en el cliente de orquestación o en una función de actividad. Para ello, use el método [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) de la clase [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) o el método `createHttpManagementPayload` de la clase `DurableOrchestrationClient` (JavaScript).

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) y `createHttpManagementPayload` tienen un parámetro:

* **instanceId**: el identificador único de la instancia.

Los métodos devuelven una instancia de [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) (.NET) o un objeto (JavaScript) con las siguientes propiedades de cadena:

* **Id**: el identificador de instancia de la orquestación (debe ser el mismo que la entrada `InstanceId`).
* **StatusQueryGetUri**: Dirección URL del estado de la instancia de orquestación.
* **SendEventPostUri**: Dirección URL de generación del evento de la instancia de orquestación.
* **TerminatePostUri**: Dirección URL de finalización de la instancia de orquestación.
* **RewindPostUri**: Dirección URL de rebobinado de la instancia de orquestación.

Las funciones de la actividad pueden enviar una instancia de estos objetos a sistemas externos para supervisar o generar eventos en una orquestación:

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, ctx) {
    const client = df.getClient(context);

    const payload = client.createHttpManagementPayload(ctx.instanceId);

    // send the payload to Cosmos DB
    context.bindings.document = JSON.stringify({
        id: ctx.instanceId,
        payload,
    });
};
```

## <a name="rewind-instances-preview"></a>Rebobinado de instancias (versión preliminar)

Si tiene un error de la orquestación por un motivo inesperado, puede *rebobinar* la instancia a un estado correcto anterior mediante una API creada con ese propósito.

> [!NOTE]
> Esta API no pretende ser un sustituto para el control de errores y las directivas de reintentos pertinentes. En su lugar, el objetivo es que se use solo en casos donde las instancias de orquestación producen un error por razones inesperadas. Para obtener más detalles sobre el control de errores y las directivas de reintento, consulte el tema sobre el [control de errores](durable-functions-error-handling.md).

Use la API [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) (. NET) o `rewindAsync` (JavaScript) para poner la orquestación de nuevo en estado *En ejecución*. Vuelva a ejecutar la actividad o los errores de ejecución de suborquestación que generaron el error en la orquestación.

Por ejemplo, supongamos que tiene un flujo de trabajo que requiere una serie de [aprobaciones realizadas por humanos](durable-functions-concepts.md#human). Suponga que hay una serie de funciones de actividad que notifican a alguien cuando se requiere su aprobación y que esperan la respuesta en tiempo real. Después de que todas las actividades de aprobación han recibido respuestas o se ha agotado el tiempo de espera, suponga que otra actividad devuelve un error debido a un error de configuración en la aplicación (por ejemplo, una cadena de conexión de base de datos no válida). El resultado es un error de orquestación en el flujo de trabajo. Con la API `RewindAsync` (.NET) o `rewindAsync` (JavaScript), un administrador de aplicaciones puede corregir el error de configuración y devolver la orquestación con error de vuelta al estado inmediatamente anterior al error. No es necesario volver a aprobar ninguno de los pasos de interacción humana y, ahora, la orquestación se puede completar correctamente.

> [!NOTE]
> La característica para *rebobinar* no admite el rebobinado de instancias de orquestación que utilizan temporizadores durables.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "Orchestrator failed and needs to be revived.";
    return client.rewind(instanceId, reason);
};
```

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

También es posible rebobinar una instancia de orquestación directamente a través del comando `durable rewind` de [Azure Functions Core Tools](../functions-run-local.md). Toma los parámetros siguientes:

* **`id` (obligatorio)** : identificador de la instancia de orquestación.
* **`reason` (opcional)** : motivo para devolver a un estado anterior a la instancia de orquestación.
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en [host.json](durable-functions-bindings.md#host-json) mediante durableTask:HubName.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Purga del historial de instancias

Para quitar todos los datos asociados con una orquestación, puede purgar el historial de instancias. Por ejemplo, puede deshacerse de filas de una tabla de Azure y los blobs de mensajes de gran tamaño, si los hubiera. Para ello, utilice la API [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_).

> [!NOTE]
> `PurgeInstanceHistoryAsync` API actualmente solo está disponible para C#.

 El método tiene dos sobrecargas. La primera de ellas purga el historial por el identificador de la instancia de orquestación:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

En el segundo ejemplo se muestra una función de desencadenador por temporizador que purga el historial de todas las instancias de orquestación que se completan después del intervalo de tiempo especificado. En este caso, se quitan los datos de todas las instancias que se completaron hace 30 días o más. Se programa para ejecutarse una vez al día a las 12 a. m.:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [TimerTrigger("0 0 12 * * *")]TimerInfo myTimer)
{
    return client.PurgeInstanceHistoryAsync(
                    DateTime.MinValue,
                    DateTime.UtcNow.AddDays(-30),  
                    new List<OrchestrationStatus>
                    {
                        OrchestrationStatus.Completed
                    });
}
```

> [!NOTE]
> Para que el proceso de la función desencadenada por hora se lleve a cabo correctamente, el estado del entorno de ejecución debe ser **Completado**, **Finalizado** o **Con errores**.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

También es posible purgar el historial de una instancia de orquestación a través del comando `durable purge-history` de [Azure Functions Core Tools](../functions-run-local.md). De forma similar al segundo ejemplo de C# de la sección anterior, se purga el historial de todas las instancias de orquestación creadas durante un intervalo de tiempo especificado. Puede filtrar aún más las instancias purgadas por estado del entorno de ejecución. El comando tiene varios parámetros:

* **`created-after` (opcional)** : purga del historial de instancias creado después de esta fecha y hora (UTC). Se aceptan valores de datetime con formato ISO 8601.
* **`created-before` (opcional)** : purga del historial de instancias creado antes de esta fecha y hora (UTC). Se aceptan valores de datetime con formato ISO 8601.
* **`runtime-status` (opcional)** : Purgue el historial de las instancias con un estado determinado (por ejemplo, en ejecución o completado). Puede proporcionar varios estados (separados por espacios).
* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en [host.json](durable-functions-bindings.md#host-json) mediante durableTask:HubName.

El comando siguiente elimina el historial de todas las instancias con error creadas antes del 14 de noviembre de 2018 a las 7:35 p. m. (UTC).

```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="delete-a-task-hub"></a>Eliminación de una central de tareas

Mediante el comando `durable delete-task-hub` de [Azure Functions Core Tools](../functions-run-local.md), es posible eliminar todos los artefactos de almacenamiento asociados a una central de tareas determinada. Se incluyen los blobs, las colas y las tablas de Azure Storage. El comando tiene dos parámetros:

* **`connection-string-setting` (opcional)** : nombre de la configuración de la aplicación que contiene la cadena de conexión de almacenamiento que se va a usar. El valor predeterminado es `AzureWebJobsStorage`.
* **`task-hub-name` (opcional)** : nombre de la central de tareas de Durable Functions que se va a usar. El valor predeterminado es `DurableFunctionsHub`. También puede establecerse en [host.json](durable-functions-bindings.md#host-json) mediante durableTask:HubName.

El siguiente comando elimina todos los datos de Azure Storage asociados a la central de tareas `UserTest`.

```bash
func durable delete-task-hub --task-hub-name UserTest
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información acerca de cómo usar las API HTTP para la instancia de administración](durable-functions-http-api.md)
