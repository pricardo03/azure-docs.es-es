---
title: Patrones de Durable Functions y conceptos técnicos en Azure Functions
description: Obtenga información sobre cómo la extensión Durable Functions en Azure Functions le ofrece las ventajas de la ejecución de código con estado en la nube.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: azfuncdf
ms.openlocfilehash: a244883f470f4906879725daf0d37bd1759e65c4
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812908"
---
# <a name="durable-functions-patterns-and-technical-concepts-azure-functions"></a>Patrones de Durable Functions y conceptos técnicos (Azure Functions)

Durable Functions es una extensión de [Azure Functions](../functions-overview.md) y [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md). Puede utilizar Durable Functions para escribir funciones con estado en un entorno sin servidor. La extensión administra el estado, establece puntos de control y reinicia en su nombre. 

En este artículo se proporciona información detallada sobre los comportamientos de la extensión Durable Functions para Azure Functions y sus patrones comunes de implementación. La información puede ayudarle a determinar cómo utilizar Durable Functions para ayudarle a solucionar desafíos de desarrollo.

> [!NOTE]
> Durable Functions es una extensión avanzada para Azure Functions que no es adecuada para todas las aplicaciones. En este artículo se da por supuesto que está muy familiarizado con los conceptos de [Azure Functions](../functions-overview.md) y los desafíos que implica el desarrollo de aplicaciones sin servidor.

## <a name="patterns"></a>Patrones

En esta sección se describen algunos patrones comunes de aplicaciones en los que Durable Functions puede resultar útil.

### <a name="chaining"></a>Patrón 1: Diagrama de encadenamiento de funciones

En el modelo de encadenamiento de funciones, una secuencia de funciones se ejecuta en un orden específico. Con este patrón, la salida de una función se aplica a la entrada de otra función.

![Un diagrama del patrón de encadenamiento de funciones](./media/durable-functions-concepts/function-chaining.png)

Puede utilizar Durable Functions para implementar el patrón de encadenamiento de funciones de forma concisa como se muestra en el siguiente ejemplo:

#### <a name="c-script"></a>Script de C#

```csharp
public static async Task<object> Run(DurableOrchestrationContext context)
{
    try
    {
        var x = await context.CallActivityAsync<object>("F1");
        var y = await context.CallActivityAsync<object>("F2", x);
        var z = await context.CallActivityAsync<object>("F3", y);
        return  await context.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // Error handling or compensation goes here.
    }
}
```

> [!NOTE]
> Existen diferencias sutiles entre escribir una función durable precompilada en C# y escribir una función durable precompilada en el script de C# que se muestra en el ejemplo. En una función precompilada en C#, los parámetros durables deben incluir los atributos adecuados. Un ejemplo es el atributo `[OrchestrationTrigger]` para el parámetro `DurableOrchestrationContext`. En una función durable precompilada en C#, si los parámetros no se incluyen correctamente, el tiempo de ejecución no puede insertar las variables en la función y se produce un error. Para obtener más ejemplos, consulte los [ejemplos azure-functions-durable-extension en GitHub](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples).

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const x = yield context.df.callActivity("F1");
    const y = yield context.df.callActivity("F2", x);
    const z = yield context.df.callActivity("F3", y);
    return yield context.df.callActivity("F4", z);
});
```

En este ejemplo, los valores `F1`, `F2`, `F3` y `F4` son los nombres de otras funciones en la aplicación de función. Puede implementar el flujo de control mediante construcciones de código imperativas normales. El código se ejecuta de arriba hacia abajo. y puede implicar semántica de flujo de control del lenguaje ya existente, como instrucciones condicionales y bucles. Puede incluir lógica de control de errores en bloques `try`/`catch`/`finally`.

Puede usar el parámetro de `context` [DurableOrchestrationContext] \(.NET\) y el objeto `context.df` (JavaScript) para invocar otras funciones por nombre, pasar parámetros y devolver la salida de la función. Cada vez que el código llama a `await` (C#) o `yield` (JavaScript), el marco de Durable Functions establece puntos de control en el progreso de la instancia actual de la función. Si el proceso o la máquina virtual se recicla a mitad de la ejecución, la instancia de la función se reanuda desde la llamada `await` o `yield` anterior. Para obtener más información, consulte la siguiente sección, Patrón 2: Distribución ramificada de entrada y salida.

> [!NOTE]
> El objeto `context` de JavaScript representa el [contexto de la función](../functions-reference-node.md#context-object) completo, no solo el parámetro [DurableOrchestrationContext].

### <a name="fan-in-out"></a>Patrón 2: distribución ramificada de entrada y salida

En el patrón de distribución ramificada de salida y entrada, se ejecutan en paralelo varias funciones y después se espera a que todas finalicen. A menudo se realiza algún trabajo de agregación en los resultados devueltos de las funciones.

![Un diagrama del patrón de distribución ramificada de entrada y salida](./media/durable-functions-concepts/fan-out-fan-in.png)

Con las funciones normales, puedes realizar la distribución ramificada de salida al hacer que la función envíe varios mensajes a una cola. La distribución ramificada de entrada es mucho más compleja. Para realizarla en una función normal, escribe código para realizar un seguimiento de cuándo finalizan las funciones desencadenadas por la cola y después almacenar la salida de la función. 

La extensión de Durable Functions controla este patrón con código relativamente sencillo:

#### <a name="c-script"></a>Script de C#

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var parallelTasks = new List<Task<int>>();

    // Get a list of N work items to process in parallel.
    object[] workBatch = await context.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = context.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }

    await Task.WhenAll(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    int sum = parallelTasks.Sum(t => t.Result);
    await context.CallActivityAsync("F3", sum);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const parallelTasks = [];

    // Get a list of N work items to process in parallel.
    const workBatch = yield context.df.callActivity("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(context.df.callActivity("F2", workBatch[i]));
    }

    yield context.df.Task.all(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield context.df.callActivity("F3", sum);
});
```

El trabajo de distribución ramificada se distribuye en varias instancias de la función `F2`. Se realiza un seguimiento del trabajo mediante una lista dinámica de las tareas. Se llama a la API de .NET `Task.WhenAll` o la API de JavaScript `context.df.Task.all` para esperar a que todas las funciones llamadas finalicen. Después, los resultados de la función `F2` se agregan desde la lista de tareas dinámica y se pasan a la función `F3`.

El establecimiento automático de puntos de control que se produce en la llamada `await` o `yield` en `Task.WhenAll` o `context.df.Task.all` garantiza que cualquier bloqueo o reinicio posible a mitad del proceso no requiera un reinicio de las tareas ya completadas.

### <a name="async-http"></a>Patrón 3: Las API de HTTP asincrónico

El patrón de API de HTTP asincrónico soluciona el problema de coordinar el estado de las operaciones de larga duración con los clientes externos. Un enfoque común para implementar este patrón es que una llamada HTTP desencadene la acción de larga duración. A continuación, el cliente se redirige a un punto de conexión de estado al que sondea para saber cuando finalice la operación.

![Un diagrama del modelo de API de HTTP](./media/durable-functions-concepts/async-http-api.png)

Durable Functions proporciona API integradas que simplifican el código que tiene que escribir para interactuar con las ejecuciones de función de larga duración. Las muestras de la guía de inicio rápido de Durable Functions ([C#](durable-functions-create-first-csharp.md) y [JavaScript](quickstart-js-vscode.md)) muestran un comando REST simple que puede usar para iniciar nuevas instancias de la función del orquestador. Tras iniciar una instancia, la extensión expone las API de HTTP de webhook que consultan el estado de la función del orquestador. 

En el ejemplo siguiente se muestran los comandos REST para iniciar un orquestador y consultar su estado. Para evitar confusiones, se omiten algunos detalles en el ejemplo.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2017-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2017-03-16T21:20:57Z", ...}
```

Dado que el tiempo de ejecución de Durable Functions administra el estado, no tendrá que implementar su propio mecanismo de seguimiento de estado.

La extensión Durable Functions tiene webhooks integrados que administran las orquestaciones de larga duración. Puede implementar este patrón usted mismo si utiliza sus propios desencadenadores de función (por ejemplo, HTTP, una cola o Azure Event Hubs) y el enlace `orchestrationClient`. Por ejemplo, puede utilizar un mensaje de cola para desencadenar la terminación. O bien puede usar un desencadenador HTTP que esté protegido por una directiva de autenticación de Azure Active Directory en lugar de webhooks integradas que utilicen una clave generada para la autenticación.

Estos son algunos ejemplos de cómo usar el patrón de API de HTTP:

#### <a name="c"></a>C#

```csharp
// An HTTP-triggered function starts a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // The function name comes from the request URL.
    // The function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
// An HTTP-triggered function starts a new orchestrator function instance.
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);

    // The function name comes from the request URL.
    // The function input comes from the request content.
    const eventData = req.body;
    const instanceId = await client.startNew(req.params.functionName, undefined, eventData);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    return client.createCheckStatusResponse(req, instanceId);
};
```

En .NET, el parámetro `starter` [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) es un valor del enlace de salida `orchestrationClient` que forma parte de la extensión de Durable Functions. En JavaScript, este objeto se devuelve llamando a `df.getClient(context)`. Estos objetos proporcionan métodos que puede usar para iniciar, enviar eventos, terminar y consultar instancias de función de orquestador nuevas o existentes.

En el ejemplo anterior, una función desencadenada por HTTP toma un valor `functionName` de la dirección URL de entrada y pasa ese valor a [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). A continuación, la API de enlace [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_System_Net_Http_HttpRequestMessage_System_String_) devuelve una respuesta que contiene un encabezado `Location` e información adicional acerca de la instancia. Puede usar la siguiente información para consultar el estado de la instancia iniciada o para finalizar la instancia.

### <a name="monitoring"></a>Patrón 4: Supervisión

El patrón de supervisión hace referencia a un proceso flexible y periódico en un flujo de trabajo. Un ejemplo es el sondeo hasta que se cumplen condiciones específicas. Puede usar un [desencadenador de temporizador](../functions-bindings-timer.md) normal para solucionar un escenario simple (como un trabajo de limpieza periódico), pero el intervalo es estático y resulta más difícil administrar los ciclos de vida de las instancias. Puede usar Durable Functions para crear intervalos de periodicidad flexible, administrar el ciclo de vida de las tareas y crear varios procesos de supervisión a partir de una única orquestación.

Un ejemplo del patrón de supervisión es invertir el escenario anterior de la API de HTTP asincrónica. En lugar de exponer un punto de conexión para que un cliente externo supervise una operación de larga duración, el monitor de larga duración consume el punto de conexión externo y espera algún cambio de estado.

![Un diagrama del patrón de supervisión](./media/durable-functions-concepts/monitor.png)

Con unas cuantas líneas de código, puede utilizar Durable Functions para crear varios monitores que observen puntos de conexión arbitrarios. Los monitores pueden finalizar la ejecución cuando se cumple una condición, o el [DurableOrchestrationClient](durable-functions-instance-management.md) puede finalizar a los monitores. Puede cambiar el intervalo de `wait` del monitor según una condición específica (por ejemplo, retroceso exponencial). 

El código siguiente implementa un monitor básico:

#### <a name="c-script"></a>Script de C#

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    int jobId = context.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();

    while (context.CurrentUtcDateTime < expiryTime)
    {
        var jobStatus = await context.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform an action when a condition is met.
            await context.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        var nextCheck = context.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await context.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform more work here, or let the orchestration end.
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const jobId = context.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(context.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield context.df.callActivity("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform an action when a condition is met.
            yield context.df.callActivity("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        const nextCheck = moment.utc(context.df.currentUtcDateTime).add(pollingInterval, 's');
        yield context.df.createTimer(nextCheck.toDate());
    }

    // Perform more work here, or let the orchestration end.
});
```

Cuando se recibe una solicitud, se crea una nueva instancia de orquestación para ese identificador de trabajo. La instancia sondea un estado hasta que se cumple una condición y se cierra el bucle. Un temporizador durable controla el intervalo de sondeo. Después, se puede realizar trabajo adicional o puede finalizar la orquestación. Cuando `context.CurrentUtcDateTime` (.NET) o `context.df.currentUtcDateTime` (JavaScript) superan el valor `expiryTime`, el monitor finaliza.

### <a name="human"></a>Patrón 5: Interacción humana

Muchos procesos automatizados implican algún tipo de interacción humana. La intervención humana en un proceso automatizado es más difícil, ya que las personas no tienen la misma alta disponibilidad y capacidad de respuesta que los servicios en la nube. Un procesos automatizado puede tener esto en cuenta utilizando los tiempos de expiración y la lógica de compensación.

Un proceso de aprobación es un ejemplo de un proceso empresarial que implica la interacción humana. Para un informe de gastos que supera un importe en dólares determinado puede ser necesaria la aprobación de un administrador. Si el administrador no aprueba el informe de gastos en un plazo de 72 horas (puede estar de vacaciones), se inicia un proceso de escalado para obtener la aprobación de otra persona (por ejemplo el jefe del administrador).

![Un diagrama del patrón de interacción humana](./media/durable-functions-concepts/approval.png)

Puede implementar el patrón en este ejemplo mediante una función de orquestador. El orquestador usa un [temporizador durable](durable-functions-timers.md) para solicitar la aprobación, y la escala si se agota el tiempo de espera. El orquestador espera a que ocurra un [evento externo](durable-functions-external-events.md), como una notificación generada por interacción humana.

Estos ejemplos crean un proceso de aprobación para hacer una demostración del patrón de interacción humana:

#### <a name="c-script"></a>Script de C#

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    await context.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = context.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = context.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = context.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await context.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await context.CallActivityAsync("Escalate");
        }
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");
const moment = require('moment');

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("RequestApproval");

    const dueTime = moment.utc(context.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = context.df.createTimer(dueTime.toDate());

    const approvalEvent = context.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield context.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield context.df.callActivity("ProcessApproval", approvalEvent.result);
    } else {
        yield context.df.callActivity("Escalate");
    }
});
```

Para crear el temporizador durable, realice una llamada a `context.CreateTimer` (. NET) o `context.df.createTimer` (JavaScript). `context.WaitForExternalEvent` (.NET) o `context.df.waitForExternalEvent` (JavaScript) reciben la notificación. Después, se realiza una llamada a `Task.WhenAny` (.NET) o `context.df.Task.any` (JavaScript) para decidir si se escala (se agota primero el tiempo de expiración) o se aprueba el proceso (se recibe la aprobación antes de que se agote el tiempo de expiración).

Un cliente externo puede entregar la notificación de eventos a una función de orquestador en espera utilizando las [API de HTTP integradas](durable-functions-http-api.md#raise-event) o la API [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) desde otra función:

```csharp
public static async Task Run(string instanceId, DurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const isApproved = true;
    await client.raiseEvent(instanceId, "ApprovalEvent", isApproved);
};
```

### <a name="aggregator"></a>Patrón 6: Agregación (versión preliminar)

El sexto patrón se trata de agregar datos de eventos durante un período de tiempo en una sola*entidad* direccionable. En este patrón, los datos que se agreguen pueden proceder de varios orígenes, pueden entregarse en lotes o pueden estar dispersos en largos períodos de tiempo. Es posible que el agregador tome medidas según datos de eventos a medida que llegan, y puede que los clientes externos necesiten consultar los datos agregados.

![Diagrama de agregador](./media/durable-functions-concepts/aggregator.png)

El aspecto más difícil de implementar este patrón con funciones normales sin estado es que el control de simultaneidad se convierte en un gran desafío. No solo es necesario preocuparse por varios subprocesos que modifican los mismos datos al mismo tiempo, también deberá preocuparse por garantizar que el agregador se ejecuta solo en una única VM a la vez.

Mediante una [función de entidad duradera](durable-functions-preview.md#entity-functions) se puede implementar este patrón fácilmente como una sola función.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();

    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            currentValue += operand;
            break;
        case "reset":
            currentValue = 0;
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }

    ctx.SetState(currentValue);
}
```

Las entidades durables también se pueden modelar como clases .NET. Esto puede ser útil si la lista de operaciones se hace muy grande y si es principalmente estática. El ejemplo siguiente es una implementación equivalente de la entidad `Counter` que utiliza clases y métodos .NET.

```csharp
public class Counter
{
    [JsonProperty("value")]
    public int CurrentValue { get; set; }

    public void Add(int amount) => this.CurrentValue += amount;
    
    public void Reset() => this.CurrentValue = 0;
    
    public int Get() => this.CurrentValue;

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

Los clientes pueden poner en cola las *operaciones* para (también conocido como "señalización") una función de entidad mediante el enlace `orchestrationClient`.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static async Task Run(
    [EventHubTrigger("device-sensor-events")] EventData eventData,
    [OrchestrationClient] IDurableOrchestrationClient entityClient)
{
    var metricType = (string)eventData.Properties["metric"];
    var delta = BitConverter.ToInt32(eventData.Body, eventData.Body.Offset);

    // The "Counter/{metricType}" entity is created on-demand.
    var entityId = new EntityId("Counter", metricType);
    await entityClient.SignalEntityAsync(entityId, "add", delta);
}
```

Los proxies generados dinámicamente también están disponibles para la señalización de entidades con seguridad de tipos. Y, además de la señalización, los clientes también pueden consultar el estado de una función de entidad mediante métodos del enlace `orchestrationClient`.

> [!NOTE]
> Las funciones de entidad actualmente solo están disponibles en [Durable Functions 2.0 preview](durable-functions-preview.md).

## <a name="the-technology"></a>La tecnología

En segundo plano, la extensión Durable Functions se crea a partir de [Durable Task Framework](https://github.com/Azure/durabletask), una biblioteca de código abierto en GitHub que se usa para crear orquestaciones de tareas durables. Así como Azure Functions es la evolución sin servidor de Azure WebJobs, Durable Functions es la evolución sin servidor de Durable Task Framework. Microsoft y otras organizaciones utilizan Durable Task Framework ampliamente para automatizar los procesos críticos. Es una opción natural para el entorno de Azure Functions sin servidor.

### <a name="event-sourcing-checkpointing-and-replay"></a>Abastecimiento de eventos, puntos de control y reproducción

Las funciones del orquestador mantienen de forma confiable su estado de ejecución mediante el patrón de diseño [origen de eventos](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing). En lugar de almacenar directamente el estado actual de una orquestación, la extensión Durable Functions utiliza un almacén de solo anexar para registrar la serie completa de acciones que la orquestación de función realiza. Un almacén de solo anexar tiene muchas ventajas en comparación con el "volcado" del estado de tiempo de ejecución completo. Entre las ventajas se incluyen un mayor rendimiento, escalabilidad y capacidad de respuesta. También obtiene coherencia ocasional para los datos transaccionales y registros e historiales de auditoría completa. Los registros de auditoría son compatibles con las acciones de compensación confiables.

Durable Functions usa el aprovisionamiento de eventos transparente. En un segundo plano, el operador `await` (C#) o `yield` (JavaScript) en una función del orquestador devuelve el control del subproceso del orquestador al distribuidor de Durable Task Framework. El distribuidor, a continuación, confirma las nuevas acciones que la función de orquestador programó (como llamar a una o más funciones secundarias o programar un temporizador durable) al almacenamiento. Esta acción de confirmación transparente se anexa al historial de ejecución de la instancia de orquestación. El historial se guarda en una tabla de almacenamiento. La acción de confirmación, a continuación, agrega mensajes a una cola para programar el trabajo real. En este momento, la función del orquestador se puede descargar de la memoria. 

La facturación para dicha función del orquestador se detiene si usa el plan de consumo de Azure Functions. Cuando hay más trabajo, la función se reinicia y se reconstruye su estado.

Cuando una función de orquestación recibe más trabajo para realizar (por ejemplo, se recibe un mensaje de respuesta o expira un temporizador durable), el orquestador se activa y vuelve a ejecutar toda la función desde el principio con el fin de recompliar el estado local. 

Durante esta reproducción, si el código intenta realizar una llamada a una función (o realizar cualquier otro trabajo asincrónico), Durable Task Framework consulta el historial de ejecución de la orquestación actual. Si descubre que la [función de actividad](durable-functions-types-features-overview.md#activity-functions) ya se ha ejecutado y ofrece un resultado, reproduce el resultado de esa función y el código del orquestador sigue ejecutándose. La reproducción continúa hasta que finaliza el código de función o hasta que se programa un nuevo trabajo asincrónico.

### <a name="orchestrator-code-constraints"></a>Restricciones de código del orquestador

El comportamiento de reproducción del código de orquestador crea restricciones sobre el tipo de código que puede escribir en una función de orquestador. Por ejemplo, el código del orquestador tiene que ser determinista, ya que se reproducirá varias veces y tiene que producir siempre el mismo resultado. Para obtener una lista completa de las restricciones, consulte [restricciones de código del orquestador](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints).

## <a name="monitoring-and-diagnostics"></a>Supervisión y diagnóstico

La extensión de Durable Functions emite automáticamente datos de seguimiento estructurado a [Application Insights](../functions-monitoring.md) cuando la aplicación de función se configura con una clave de instrumentación de Azure Application Insights. Puede usar los datos de seguimiento para supervisar las acciones y el progreso de las orquestaciones.

Este es un ejemplo del aspecto de los eventos de seguimiento de Durable Functions en el portal de Application Insights cuando se usa [Application Insights Analytics](../../application-insights/app-insights-analytics.md):

![Resultados de consultas en Application Insights](./media/durable-functions-concepts/app-insights-1.png)

Puede encontrar datos estructurados útiles en el campo `customDimensions` de cada entrada de registro. Este es un ejemplo de una entrada que está expandida por completo:

![El campo customDimensions en una consulta de Application Insights](./media/durable-functions-concepts/app-insights-2.png)

Debido al comportamiento de la reproducción del distribuidor de Durable Task Framework, podrá ver entradas del registro redundantes para acciones reproducidas. Las entradas de registro redundantes pueden ser útiles para comprender el comportamiento de la reproducción del motor principal. El artículo sobre [diagnósticos](durable-functions-diagnostics.md) muestra consultas de ejemplo que filtran los registros de reproducción para que pueda ver los registros que son "en tiempo real".

## <a name="storage-and-scalability"></a>Almacenamiento y la escalabilidad

La extensión de Durable Functions usa blobs, tablas y colas en Azure Storage para conservar el estado del historial de ejecución y desencadenar la ejecución de la función. Puede usar la cuenta de almacenamiento predeterminada de la aplicación de la función, o puede configurar una cuenta de almacenamiento independiente. Puede resultar conveniente usar una cuenta independiente de acuerdo con los límites de rendimiento de almacenamiento. El código de orquestador que escribe no interactúa con las entidades de estas cuentas de almacenamiento. Durable Task Framework administra directamente las entidades como un detalle de implementación.

Las funciones del orquestador programan funciones de actividad y reciben sus respuestas a través de mensajes de la cola interna. Cuando una aplicación de función se ejecuta en el plan de consumo de Azure Functions, el [controlador de escala de Azure Functions](../functions-scale.md#how-the-consumption-and-premium-plans-work) supervisa estas colas. Se agregan nuevas instancias de proceso según sea necesario. Al escalar horizontalmente a varias máquinas virtuales, una función de orquestador se puede ejecutar en una máquina virtual, mientras que las funciones de actividad a las que la función de orquestador llama se pueden ejecutar en varias máquinas virtuales diferentes. Para obtener más información sobre el comportamiento de Durable Functions, consulte [Rendimiento y escala](durable-functions-perf-and-scale.md).

El historial de ejecución para las cuentas del orquestador se almacena en Table Storage. Cada vez que una instancia se rehidrata en una máquina virtual concreta, el orquestador captura su historial de ejecución de Table Storage para que pueda volver a recompilar su estado local. Un aspecto práctico de tener el historial disponible en Table Storage es que puede usar herramientas como el [Explorador de Microsoft Azure Storage](../../vs-azure-tools-storage-manage-with-storage-explorer.md) para ver el historial de las orquestaciones.

Los blobs de almacenamiento se utilizan principalmente como un mecanismo de concesiones para coordinar la escalabilidad horizontal de las instancias de orquestación en varias máquinas virtuales. Los blobs de almacenamiento almacenan datos de mensajes de gran tamaño que no se pueden almacenar directamente en tablas o colas.

![Captura de pantalla del Explorador de Azure Storage](./media/durable-functions-concepts/storage-explorer.png)

> [!NOTE]
> Aunque es sencillo y práctico ver el historial de ejecución en Table Storage, no se fie de esta tabla. Es posible que la tabla cambie a medida que la extensión Durable Functions progresa.

## <a name="known-issues"></a>Problemas conocidos

Todos los problemas conocidos deben encontrarse en la lista de [problemas en GitHub](https://github.com/Azure/azure-functions-durable-extension/issues). Si le surge algún problema y no lo encuentra en GitHub, abra un nuevo problema e incluya una descripción detallada del mismo.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Durable Functions, consulte [Tipos y características de las funciones de Durable Functions](durable-functions-types-features-overview.md). 

Primeros pasos:

> [!div class="nextstepaction"]
> [Creación de su primera función duradera](durable-functions-create-first-csharp.md)

[DurableOrchestrationContext]: https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html
