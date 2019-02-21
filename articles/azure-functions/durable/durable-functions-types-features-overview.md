---
title: Información general de los tipos de función y características para Durable Functions - Azure
description: Aprenda acerca de los tipos de funciones y roles que permiten la comunicación de función a función como parte de una orquestación de función durable.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 2885ce740fab58e675c529dfab8d0dadeed2904c
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301628"
---
# <a name="overview-of-function-types-and-features-for-durable-functions-azure-functions"></a>Información general de los tipos de función y características para Durable Functions (Azure Functions)

Durable Functions proporciona una orquestación con estado de la ejecución de funciones. Una función durable es una solución compuesta de diferentes funciones de Azure Functions. Cada una de estas funciones puede desempeñar roles diferentes como parte de una orquestación. El siguiente documento proporciona información general sobre los tipos de funciones implicadas en una orquestación de función durable. También incluye algunos patrones comunes en la conexión de funciones.  Para empezar ahora mismo, cree la primera función de Durable en [C#](durable-functions-create-first-csharp.md) o [JavaScript](quickstart-js-vscode.md).

![Tipos de funciones durables][1]  

## <a name="types-of-functions"></a>Tipos de funciones

### <a name="activity-functions"></a>Funciones de actividad

Las funciones de actividad son la unidad básica de trabajo en una orquestación durable.  Las funciones de actividad son las funciones y tareas que se organizan en el proceso de orquestación.  Por ejemplo, puede crear una función durable para procesar un pedido: comprobar el inventario, cobrar al cliente y crear un envío.  Cada una de esas tareas sería una función de actividad.  Las funciones de actividad no tienen ninguna restricción en relación al tipo de trabajo que puede realizar en ellas.  Pueden escribirse en cualquier [lenguaje que sea compatible con Durable Functions](durable-functions-overview.md#language-support). El marco de trabajo de las tareas durables garantiza que cada función de actividad que se llame, se ejecutará al menos una vez durante una orquestación.

Una función de actividad tiene que activarse mediante un [desencadenador de actividad](durable-functions-bindings.md#activity-triggers).  Las funciones .NET recibirán [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) como parámetro. También puede enlazar el desencadenador con cualquier otro objeto para pasar en entradas a la función. En JavaScript, se puede acceder a la entrada mediante la propiedad `<activity trigger binding name>` del [objeto `context.bindings`](../functions-reference-node.md#bindings).

La función de actividad también puede devolver valores al orquestador.  Si envía o devuelve muchos valores a una función de actividad, puede [aprovechar las tuplas o las matrices](durable-functions-bindings.md#passing-multiple-parameters).  Las funciones de actividad solo se pueden desencadenar desde una instancia de orquestación.  Mientras que parte del código puede compartirse entre una función de actividad y otra función (como una función desencadenada por HTTP), cada función solo puede tener un desencadenador.

Encontrará más información y ejemplos en el artículo sobre [Enlaces para Durable Functions](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Funciones de Orchestrator

Las funciones de Orchestrator son el corazón de una función durable.  Las funciones de Orchestrator describen la forma y el orden en el que se ejecutan las acciones.  Las funciones del orquestador describen la orquestación en código (C# o JavaScript) como se muestra en [Durable Functions patterns and technical concepts](durable-functions-concepts.md) (Patrones y conceptos técnicos de Durable Functions).  Una orquestación puede tener muchos tipos diferentes de acciones, como [funciones de actividad](#activity-functions), [suborquestaciones](#sub-orchestrations), [espera por eventos externos](#external-events) y [temporizadores](#durable-timers).  

Una función de orquestador tiene que activarse mediante un [desencadenador de orquestación](durable-functions-bindings.md#orchestration-triggers).

Un orquestador se inicia con un [cliente de orquestador](#client-functions) que a su vez se puede desencadenar desde cualquier origen (HTTP, colas, flujos de eventos).  Cada instancia de una orquestación tiene un identificador de instancia, que se puede generar automáticamente (recomendado) o puede generarlo el usuario.  Este identificador se puede usar para [administrar instancias](durable-functions-instance-management.md) de la orquestación.

Encontrará más información y ejemplos en el artículo sobre [Enlaces para Durable Functions](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>Funciones de cliente

Las funciones de cliente son las funciones desencadenadas que crearán nuevas instancias de una orquestación.  Son el punto de entrada para la creación de una instancia de una orquestación durable.  Las funciones de cliente pueden activarse con cualquier desencadenador (HTTP, colas, flujos de eventos, etc.) y escribirse en cualquier lenguaje compatible con la aplicación.  Además del desencadenador, las funciones de cliente tienen un enlace de [cliente de orquestación](durable-functions-bindings.md#orchestration-client) que les permite crear y administrar las orquestaciones durables.  El ejemplo más básico de una función de cliente es una función desencadenada por HTTP que inicia una función de orquestador y devuelve una respuesta de comprobación de estado como [se muestra en el siguiente ejemplo](durable-functions-http-api.md#http-api-url-discovery).

Encontrará más información y ejemplos en el artículo sobre [Enlaces para Durable Functions](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Características y patrones

### <a name="sub-orchestrations"></a>Suborquestaciones

Además de llamar a funciones de actividad, las funciones de orquestador pueden llamar a otras funciones de orquestador. Por ejemplo, puede crear una orquestación mayor a partir de una biblioteca de funciones de orquestador. También puede ejecutar varias instancias de una función de orquestador en paralelo.

Encontrará más información y ejemplos en el [artículo sobre suborquestación](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Temporizadores durables

[Durable Functions](durable-functions-overview.md) proporciona *temporizadores durables* para usarlos en funciones de orquestador para implementar retrasos o configurar tiempos de expiración en acciones asincrónicas. Los temporizadores durables deben usarse en funciones de orquestador en lugar de `Thread.Sleep` y `Task.Delay` (C#), o `setTimeout()` y `setInterval()` (JavaScript).

Encontrará más información y ejemplos de temporizadores durables en el [artículo sobre temporizadores durables](durable-functions-timers.md)

### <a name="external-events"></a>Eventos externos

Las funciones de orquestador pueden esperar por los eventos externos para actualizar una instancia de orquestación. Esta característica de Durable Functions suele ser útil para controlar las interacciones humanas u otras devoluciones de llamada externas.

Encontrará más información y ejemplos en el [artículo sobre eventos externos](durable-functions-external-events.md).

### <a name="error-handling"></a>Control de errores

Las orquestaciones de Durable Functions se implementan en código y pueden utilizar las funciones de control de errores del lenguaje de programación.  Esto significa que patrones como "try/catch" funcionarán en la orquestación.  Durable Functions también incluyen algunas directivas de reintento integradas.  Una acción puede retrasar y volver a intentar actividades de forma automática en las excepciones.  Los reintentos le permiten controlar las excepciones transitorias sin tener que abandonar la orquestación.

Encontrará más información y ejemplos en el [artículo sobre control de errores](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Comunicación entre aplicaciones de función

Mientras que una orquestación durable generalmente se encuentren dentro de un contexto de una sola aplicación de función, hay patrones que le permiten coordinar las orquestaciones entre varias aplicaciones de función.  Aunque puede haber comunicación entre aplicaciones a través de HTTP, utilizar el marco durable para cada actividad significa que puede mantener un proceso durable entre dos aplicaciones.

A continuación se proporcionan ejemplos de orquestación entre aplicaciones de función en C# y JavaScript.  Una actividad iniciará la orquestación externa. Otra actividad recuperará y devolverá el estado.  El orquestador esperará a que se complete el estado antes de continuar.

#### <a name="c"></a>C#

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now go do more work...
}

[FunctionName("StartRemoteOrchestration")]
public static async Task<string> StartRemoteOrchestration([ActivityTrigger] string orchestratorName)
{
    using (var response = await HttpClient.PostAsync(
        $"https://appB.azurewebsites.net/orchestrations/{orchestratorName}",
        new StringContent("")))
    {
        string statusUrl = await response.Content.ReadAsAsync<string>();
        return statusUrl;
    }
}

[FunctionName("CheckIsComplete")]
public static async Task<bool> CheckIsComplete([ActivityTrigger] string statusUrl)
{
    using (var response = await HttpClient.GetAsync(statusUrl))
    {
        // 200 = Complete, 202 = Running
        return response.StatusCode == HttpStatusCode.OK;
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (solo Functions 2.x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    // Do some work...

    // Call a remote orchestration
    const statusUrl = yield context.df.callActivity("StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true) {
        const isComplete = yield context.df.callActivity("CheckIsComplete", statusUrl);
        if (isComplete) {
            break;
        }

        const waitTime = moment(context.df.currentUtcDateTime).add(1, "m").toDate();
        yield context.df.createTimer(waitTime);
    }

    // B is done. Now go do more work...
});
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, orchestratorName) {
    const options = {
        method: "POST",
        uri: `https://appB.azurewebsites.net/orchestrations/${orchestratorName}`,
        body: ""
    };

    const statusUrl = await request(options);
    return statusUrl;
};
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, statusUrl) {
    const options = {
        method: "GET",
        uri: statusUrl,
        resolveWithFullResponse: true,
    };

    const response = await request(options);
    // 200 = Complete, 202 = Running
    return response.statusCode === 200;
};
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Seguir leyendo la documentación sobre Durable Functions](durable-functions-bindings.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
