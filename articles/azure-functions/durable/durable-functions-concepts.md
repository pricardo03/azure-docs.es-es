---
title: Patrones durable Functions y conceptos técnicos en Azure Functions
description: Obtenga información sobre cómo la extensión Durable Functions en Azure Functions ofrece las ventajas de la ejecución de código con estado en la nube.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: azfuncdf
ms.openlocfilehash: aa9563266f6b43e3bc2f21fbc0b340c86c5895ae
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59608738"
---
# <a name="durable-functions-patterns-and-technical-concepts-azure-functions"></a>Patrones durable Functions y conceptos técnicos (Azure Functions)

Durable Functions es una extensión de [Azure Functions](../functions-overview.md) y [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md). Puede utilizar Durable Functions para escribir funciones con estado en un entorno sin servidor. La extensión administra el estado, establece puntos de control y reinicia en su nombre. 

Este artículo se ofrece información detallada acerca de los comportamientos de la extensión Durable Functions para Azure Functions y patrones comunes de implementación. La información puede ayudarle a determinar cómo utilizar Durable Functions para ayudar a solucionar los desafíos de desarrollo.

> [!NOTE]
> Durable Functions es una extensión avanzada para Azure Functions que no es adecuada para todas las aplicaciones. En este artículo se supone que tiene está muy familiarizado con conceptos de [Azure Functions](../functions-overview.md) y los desafíos implicados en el desarrollo de aplicaciones sin servidor.

## <a name="patterns"></a>Patrones

Esta sección describen algunos patrones comunes de aplicaciones que Durable Functions puede resultar útil.

### <a name="chaining"></a>Patrón 1: Diagrama de encadenamiento de funciones

En el modelo de encadenamiento de funciones, una secuencia de funciones se ejecuta en un orden específico. En este patrón, el resultado de una función se aplica a la entrada de otra función.

![Un diagrama de la función de modelo de encadenamiento](./media/durable-functions-concepts/function-chaining.png)

Puede utilizar Durable Functions para implementar el patrón de forma concisa como se muestra en el siguiente ejemplo de encadenamiento de funciones:

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
> Existen diferencias sutiles entre escribir una función durable precompilada C# y escribir una función durable precompilada en el C# secuencia de comandos que se muestra en el ejemplo. En un C# precompilado de función, parámetros duraderos se deben decorar con atributos respectivos. Un ejemplo es el `[OrchestrationTrigger]` atributo para el `DurableOrchestrationContext` parámetro. En un C# precompilado función duradera, si los parámetros no son representativos correctamente, el tiempo de ejecución no puede insertar las variables en la función y se produce un error. Para obtener más ejemplos, vea el [azure-functions-durable-extension ejemplos en GitHub](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples).

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

En este ejemplo, los valores `F1`, `F2`, `F3`, y `F4` son los nombres de otras funciones en la aplicación de función. Puede implementar el flujo de control mediante el uso de construcciones de código imperativas normales. Código que se ejecuta de arriba hacia abajo. El código puede implicar existente semántica de flujo de control del lenguaje, como instrucciones condicionales y bucles. Puede incluir la lógica de control de errores `try` / `catch` / `finally` bloques.

Puede usar el `context` parámetro [DurableOrchestrationContext] \(.NET\) y `context.df` (objeto de JavaScript) para invocar otras funciones por nombre, pasar parámetros y devolver la función salida. Cada vez que el código llama a `await` (C#) o `yield` (JavaScript), los puntos de control de marco de Durable Functions el progreso de la instancia actual de la función. Si la máquina virtual o el proceso se recicla a mitad de la ejecución, la instancia de la función se reanuda desde la anterior `await` o `yield` llamar. Para obtener más información, consulte la sección siguiente, el patrón Nº 2: Ventilador fuera/ventilador en.

> [!NOTE]
> El `context` objeto de JavaScript representa toda la [contexto de la función](../functions-reference-node.md#context-object), no solo el [DurableOrchestrationContext] parámetro.

### <a name="fan-in-out"></a>Patrón 2: Ventilador fuera/ventilador en

En el ventilador out/ventilador de patrón, ejecutar varias funciones en paralelo y espere a que todas las funciones Finalizar. A menudo, se realiza algún trabajo de agregación en los resultados devueltos por las funciones.

![Un diagrama del ventilador fuera/ventilador patrón](./media/durable-functions-concepts/fan-out-fan-in.png)

Con las funciones normales, puede multiplicar por tener la función enviar varios mensajes a una cola. Distribución ramificada de vuelta es mucho más difícil. Para una función normal, de ventilador, escribir código para realizar un seguimiento cuando el final de las funciones desencadenadas por la cola y almacenarlo función salidas. 

La extensión Durable Functions controla este patrón con código relativamente sencillo:

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

El trabajo de distribución ramificada se distribuye en varias instancias de la `F2` función. El trabajo se realiza un seguimiento mediante el uso de una lista dinámica de las tareas. .NET `Task.WhenAll` API o JavaScript `context.df.Task.all` se denomina API, para esperar a que todas las funciones llamadas finalicen. A continuación, la `F2` función genera se agregan desde la lista de tareas dinámica y se pasan a la `F3` función.

Los puntos de comprobación automáticos que se produce en el `await` o `yield` llamar en `Task.WhenAll` o `context.df.Task.all` garantiza que un bloqueo de midway posibles o el reinicio no requiere reiniciar una tarea ya se ha completado.

### <a name="async-http"></a>Patrón 3: Las API de HTTP asincrónico

El patrón de HTTP APIs de async aborda el problema de coordinar el estado de las operaciones de larga ejecución con los clientes externos. Un enfoque común para implementar este patrón es tener un HTTP llamada al desencadenador de la acción de ejecución prolongada. A continuación, redirigir al cliente a un punto de conexión de estado que el cliente sondea para saber cuando finalice la operación.

![Un diagrama del modelo de API HTTP](./media/durable-functions-concepts/async-http-api.png)

Durable Functions proporciona API integradas que simplifican el código que escribe para interactuar con las ejecuciones de funciones de ejecución prolongada. Los ejemplos de inicio rápido de Durable Functions ([ C# ](durable-functions-create-first-csharp.md) y [JavaScript](quickstart-js-vscode.md)) muestran un comando REST simple que puede usar para iniciar nuevas instancias de función de orquestador. Cuando se inicia una instancia, la extensión expone webhook HTTP APIs que consultar el estado de la función de orquestador. 

El ejemplo siguiente muestra comandos REST que se inician un orquestador y consultar su estado. Para evitar confusiones, se omiten algunos detalles en el ejemplo.

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

Dado que el tiempo de ejecución de Durable Functions administra el estado, no es necesario implementar su propio mecanismo de seguimiento de estado.

La extensión Durable Functions tiene webhooks integrados que administrar las orquestaciones de larga ejecución. Puede implementar este patrón utilizando sus propios desencadenadores de función (por ejemplo, HTTP, una cola o Event Hubs de Azure) y el `orchestrationClient` enlace. Por ejemplo, podría utilizar un mensaje en cola para desencadenar la terminación. O bien, puede usar un desencadenador HTTP que está protegido por una directiva de autenticación de Azure Active Directory en lugar de webhooks integradas que utilizan una clave generada para la autenticación.

Estos son algunos ejemplos de cómo usar el patrón de la API de HTTP:

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

> [!WARNING]
> Al desarrollar localmente en JavaScript, para utilizar métodos en `DurableOrchestrationClient`, debe establecer la variable de entorno `WEBSITE_HOSTNAME` a `localhost:<port>` (por ejemplo, `localhost:7071`). Para obtener más información sobre este requisito, consulte [problema de GitHub 28](https://github.com/Azure/azure-functions-durable-js/issues/28).

En .NET, el parámetro `starter` [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) es un valor del enlace de salida `orchestrationClient` que forma parte de la extensión de Durable Functions. En JavaScript, este objeto se devuelve llamando a `df.getClient(context)`. Estos objetos proporcionan métodos que puede usar para iniciar, enviar eventos a, terminar y consultar para instancias de la función de orquestador nueva o existente.

En los ejemplos anteriores, una función desencadenada por HTTP toma un `functionName` valor desde la dirección URL entrante y pasa el valor a [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). El [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_System_Net_Http_HttpRequestMessage_System_String_) enlace de API, a continuación, devuelve una respuesta que contiene un `Location` encabezado e información adicional acerca de la instancia. Puede usar la información más adelante para consultar el estado de la instancia iniciada o para finalizar la instancia.

### <a name="monitoring"></a>Patrón 4: Supervisión

El patrón de supervisión hace referencia a un proceso flexible y periódico en un flujo de trabajo. Un ejemplo es el sondeo hasta que se cumplen determinadas condiciones. Puede usar normal [desencadenador de temporizador](../functions-bindings-timer.md) para abordar básico escenario, como un trabajo de limpieza periódica, pero su intervalo es estático y administrar la duración de la instancia pasa a ser compleja. Puede utilizar Durable Functions para crear intervalos de periodicidad flexibles, administrar la vigencia de la tarea y crear procesos de varios monitores desde una única orquestación.

Es un ejemplo del patrón de monitor invertir el escenario anterior de la API HTTP asincrónico. En lugar de exponer un punto de conexión para un cliente externo supervisar una operación de larga ejecución, el monitor de ejecución prolongada consume un punto de conexión externo y, a continuación, espera un cambio de estado.

![Un diagrama del patrón de monitor](./media/durable-functions-concepts/monitor.png)

En unas pocas líneas de código, puede utilizar Durable Functions para crear a varios monitores que observan puntos de conexión arbitrarios. Los monitores pueden finalizar la ejecución cuando se cumple una condición, o la [DurableOrchestrationClient](durable-functions-instance-management.md) pueden finalizar los monitores. Puede cambiar un monitor `wait` intervalo según una condición específica (por ejemplo, retroceso exponencial). 

El código siguiente implementa a un monitor básico:

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

Cuando se recibe una solicitud, se crea una nueva instancia de orquestación para ese identificador de trabajo. La instancia sondea un estado hasta que se cumple una condición y se cierra el bucle. Un temporizador durable controla el intervalo de sondeo. A continuación, se puede realizar más trabajo, o puede finalizar la orquestación. Cuando el `context.CurrentUtcDateTime` (. NET) o `context.df.currentUtcDateTime` (JavaScript) supera el `expiryTime` valor, el monitor finaliza.

### <a name="human"></a>Patrón 5: Interacción humana

Muchos procesos automatizados implican a algún tipo de interacción humana. Humana en un proceso automatizado es complicado, porque las personas que no sean como de alta disponibilidad y capacidad de respuesta como servicios en la nube. Un proceso automatizado podría permitir esto mediante la lógica de compensación y tiempos de espera.

Un proceso de aprobación es un ejemplo de un proceso empresarial que implica la interacción humana. Aprobación de un administrador podría ser necesario para un informe de gastos que supera una determinada cantidad de dólares. Si el administrador no aprueba el informe de gastos en 72 horas (quizás el administrador se fue de vacaciones), se inicia un proceso de escalado para obtener la aprobación de otra persona (quizás el jefe del administrador).

![Un diagrama del modelo de interacción humana](./media/durable-functions-concepts/approval.png)

Puede implementar el patrón en este ejemplo, mediante el uso de una función de orquestador. Usa el orquestador un [temporizador durable](durable-functions-timers.md) para solicitar la aprobación. El orquestador lo escala si se produce el tiempo de espera. El orquestador espera una [evento externo](durable-functions-external-events.md), como una notificación generada por una interacción humana.

Estos ejemplos crea un proceso de aprobación para demostrar el patrón de interacción humana:

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

Para crear el temporizador duradero, llame a `context.CreateTimer` (. NET) o `context.df.createTimer` (JavaScript). `context.WaitForExternalEvent` (.NET) o `context.df.waitForExternalEvent` (JavaScript) reciben la notificación. A continuación, `Task.WhenAny` (. NET) o `context.df.Task.any` (JavaScript) se llama para decidir si se escala (tiempo de espera sucede primero) o procesar la aprobación (se recibe la aprobación antes de tiempo de espera).

Un cliente externo puede entregar la notificación de eventos a una función de orquestador en espera utilizando el [integrados APIs HTTP](durable-functions-http-api.md#raise-event) o mediante el [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) API desde otra función:

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

## <a name="the-technology"></a>La tecnología

En segundo plano, la extensión Durable Functions se basa en la parte superior de la [Durable Task Framework](https://github.com/Azure/durabletask), una biblioteca de código abierto en GitHub que se usa para compilar las orquestaciones de tarea durables. Al igual que Azure Functions es la evolución sin servidor de Azure WebJobs, Durable Functions es la evolución sin servidor de Durable Task Framework. Microsoft y otras organizaciones utilizan Durable Task Framework ampliamente para automatizar los procesos críticos. Es una opción natural para el entorno de Azure Functions sin servidor.

### <a name="event-sourcing-checkpointing-and-replay"></a>Abastecimiento de eventos, puntos de control y reproducción

Las funciones de orquestador confiable mantienen su estado de ejecución mediante el uso de la [abastecimiento de eventos](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) patrón de diseño. En lugar de almacenar directamente el estado actual de una orquestación, la extensión Durable Functions usa un almacén de solo anexar para registrar toda la serie de acciones que realiza la función de orquestación. Un almacén de solo anexar tiene muchas ventajas en comparación con el estado de tiempo de ejecución completo "volcado". Ventajas incluyen un mayor rendimiento, escalabilidad y capacidad de respuesta. También obtendrá la coherencia final para los datos transaccionales y seguimientos de auditoría completa y el historial. Los seguimientos de auditoría admiten acciones de compensación confiables.

Durable Functions usa transparente abastecimiento de eventos. En segundo plano, el `await` (C#) o `yield` operador (JavaScript) en una función de orquestador cede el control del subproceso del orquestador al distribuidor de Durable Task Framework. El distribuidor, a continuación, confirma las nuevas acciones que la función de orquestador programó (como llamar a una o más funciones secundarias o programar un temporizador durable) al almacenamiento. La acción de confirmación transparente se anexa al historial de ejecución de la instancia de orquestación. El historial se guarda en una tabla de almacenamiento. La acción de confirmación, a continuación, agrega mensajes a una cola para programar el trabajo real. En este momento, la función del orquestador se puede descargar de la memoria. 

La facturación de la función de orquestador se detiene si usa el plan de consumo de Azure Functions. Cuando hay más trabajo que hacer, los reinicios de la función, y se reconstruye su estado.

Cuando una función de orquestación recibe más trabajo que realizar (por ejemplo, se recibe un mensaje de respuesta o expira un temporizador durable), el orquestador se reactiva y vuelve a ejecutar toda la función desde el principio para reconstruir el estado local. 

Durante la reproducción, si el código intenta llamar a una función (o realizar cualquier otra async trabajo), Durable Task Framework consulta el historial de ejecución de la orquestación actual. Si detecta que el [función de actividad](durable-functions-types-features-overview.md#activity-functions) ya ejecuta y se produjo un resultado, reproduce el resultado de la función y el código del orquestador continúa ejecutándose. Reproducción continúa hasta que finalice el código de función o hasta que se ha programado nuevo trabajo asincrónico.

### <a name="orchestrator-code-constraints"></a>Restricciones de código del orquestador

El comportamiento de reproducción del código de orchestrator crea restricciones sobre el tipo de código que puede escribir en una función de orquestador. Por ejemplo, código de orquestador debe ser determinista porque se reproducirá varias veces y se debe producir el mismo resultado cada vez. Para obtener una lista completa de las restricciones, vea [restricciones de código del orquestador](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints).

## <a name="monitoring-and-diagnostics"></a>Supervisión y diagnóstico

La extensión Durable Functions emite automáticamente datos de seguimiento estructurado a [Application Insights](../functions-monitoring.md) si configura la aplicación de función con una clave de instrumentación de Application Insights de Azure. Puede usar los datos de seguimiento para supervisar las acciones y el progreso de las orquestaciones.

Este es un ejemplo del aspecto de Durable Functions eventos de seguimiento en el portal de Application Insights cuando usas [Application Insights Analytics](../../application-insights/app-insights-analytics.md):

![Resultados de la consulta de Application Insights](./media/durable-functions-concepts/app-insights-1.png)

Puede encontrar útiles datos estructurados en el `customDimensions` campo en cada entrada de registro. Este es un ejemplo de una entrada que se expande totalmente:

![El campo customDimensions en una consulta de Application Insights](./media/durable-functions-concepts/app-insights-2.png)

Debido al comportamiento de la reproducción del distribuidor de Durable Task Framework, podrá ver entradas del registro redundantes para acciones reproducidas. Entradas del registro redundantes pueden ayudarle a entender el comportamiento de reproducción del motor principal. El [diagnósticos](durable-functions-diagnostics.md) artículo muestra ejemplos de consultas que filtran los registros de reproducción para que pueda ver los registros de "en tiempo real".

## <a name="storage-and-scalability"></a>Almacenamiento y la escalabilidad

La extensión Durable Functions usa blobs, tablas y colas de Azure Storage para conservar ejecución historial desencadenador y el estado de ejecución de la función. Puede usar la cuenta de almacenamiento predeterminada para la aplicación de función, o puede configurar una cuenta de almacenamiento independiente. Es posible que desee una cuenta independiente en función de los límites de rendimiento de almacenamiento. El código de orquestador que escribe no interactúa con las entidades en estas cuentas de almacenamiento. Durable Task Framework administra las entidades directamente como un detalle de implementación.

Las funciones del orquestador programan funciones de actividad y reciben sus respuestas a través de mensajes de la cola interna. Cuando se ejecuta una aplicación de función en el plan de consumo de Azure Functions, el [controlador de escala de Azure Functions](../functions-scale.md#how-the-consumption-and-premium-plans-work) supervisa estas colas. Se agregan nuevas instancias de proceso según sea necesario. Al escalar horizontalmente a varias máquinas virtuales, puede ejecutar una función de orquestador en una máquina virtual, mientras que las funciones de actividad que las llamadas de función de orquestador se pueden ejecutar en varias máquinas virtuales diferentes. Para obtener más información sobre el comportamiento de la escala de Durable Functions, consulte [de escala y rendimiento](durable-functions-perf-and-scale.md).

El historial de ejecución para las cuentas de orchestrator se almacena en table storage. Cada vez que una instancia se rehidrata en una máquina virtual concreta, el orquestador captura su historial de ejecución de table storage, por lo que puede volver a generar su estado local. Un aspecto cómodo de tener el historial disponible en table storage es que puede usar herramientas como [Explorador de Azure Storage](../../vs-azure-tools-storage-manage-with-storage-explorer.md) para ver el historial de las orquestaciones.

Los blobs de almacenamiento se utilizan principalmente como un mecanismo de concesiones para coordinar el escalado horizontal de las instancias de orquestación en varias máquinas virtuales. Los blobs de almacenamiento contienen datos para los mensajes grandes que no se puede almacenar directamente en las tablas o colas.

![Captura de pantalla del explorador de Azure Storage](./media/durable-functions-concepts/storage-explorer.png)

> [!WARNING]
> Aunque es fácil y cómodo ver el historial de ejecución en table storage, no realice ninguna dependencia en esta tabla. La tabla podría cambiar a medida que evoluciona la extensión Durable Functions.

## <a name="known-issues"></a>Problemas conocidos

Todos los problemas conocidos deben encontrarse en la lista de [problemas en GitHub](https://github.com/Azure/azure-functions-durable-extension/issues). Si experimenta un problema y no se encuentra el problema en GitHub, abra un nuevo problema. Incluya una descripción detallada del problema.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Durable Functions, consulte [tipos y características de funciones de Durable Functions](durable-functions-types-features-overview.md). 

Primeros pasos:

> [!div class="nextstepaction"]
> [Creación de su primera función duradera](durable-functions-create-first-csharp.md)

[DurableOrchestrationContext]: https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html
