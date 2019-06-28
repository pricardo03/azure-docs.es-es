---
title: Tipos de función y características en la extensión Durable Functions de Azure Functions
description: Aprenda acerca de los tipos de funciones y roles que admiten la comunicación de función a función en una orquestación de Durable Functions en Azure Functions.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 76b6f013333113d5a24b744bc962d36b1c0e21b3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60731129"
---
# <a name="durable-functions-types-and-features-azure-functions"></a>Tipos y características de Durable Functions (Azure Functions)

Durable Functions es una extensión de [Azure Functions](../functions-overview.md). Puede usar Durable Functions para una orquestación con estado de la ejecución de funciones. Una función durable es una solución compuesta de diferentes funciones de Azure Functions. Las funciones pueden desempeñar roles diferentes en una orquestación de Durable Functions. 

En este artículo se proporciona información general de los tipos de funciones que puede usar en una orquestación de Durable Functions. El artículo incluye algunos patrones comunes que puede usar para conectar funciones. Obtenga información acerca de cómo Durable Functions puede ayudarle a solucionar los desafíos que plantea el desarrollo de aplicaciones.

![Una imagen que muestra los tipos de Durable Functions][1]  

## <a name="types-of-durable-functions"></a>Tipos de funciones durables

Puede usar tres tipos de función durable en Azure Functions: actividad, orquestador y cliente.

### <a name="activity-functions"></a>Funciones de actividad

Las funciones de actividad son la unidad básica de trabajo en una orquestación de función durable. Las funciones de actividad son las funciones y tareas que se orquestan en el proceso. Por ejemplo, podría crear una función durable para procesar un pedido. Las tareas implican comprobar el inventario, cobrar al cliente y crear un envío. Cada tarea sería una función de actividad. 

Las funciones de actividad no tienen ninguna restricción en relación al tipo de trabajo que puede realizar en ellas. Puede escribir una función de actividad en cualquier [lenguaje compatible con Durable Functions](durable-functions-overview.md#language-support). El marco de trabajo de las tareas durables garantiza que cada función de actividad que se llame, se ejecutará al menos una vez durante una orquestación.

Use un [desencadenador de actividad](durable-functions-bindings.md#activity-triggers) para desencadenar una función de actividad. Las funciones .NET reciben [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) como parámetro. También puede enlazar el desencadenador con cualquier otro objeto para pasar en entradas a la función. En JavaScript, puede acceder a la entrada mediante la propiedad `<activity trigger binding name>` del [objeto `context.bindings`](../functions-reference-node.md#bindings).

La función de actividad también puede devolver valores al orquestador. Si envía o devuelve un gran número de valores de una función de actividad, puede usar [tuplas o matrices](durable-functions-bindings.md#passing-multiple-parameters). Puede desencadenar una función de actividad solo de una instancia de orquestación. Si bien parte del código puede compartirse entre una función de actividad y otra función (como una función desencadenada por HTTP), cada función solo puede tener un desencadenador.

Para obtener más información y ejemplos, vea [Desencadenadores de actividad](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Funciones de Orchestrator

Las funciones de Orchestrator describen cómo se ejecutan las acciones y el orden en que se ejecutan las acciones. Las funciones de Orchestrator describen la orquestación en código (C# o JavaScript) como se muestra en [Durable Functions patterns and technical concepts](durable-functions-concepts.md) (Patrones y conceptos técnicos de Durable Functions). Una orquestación puede tener muchos tipos diferentes de acciones, como [funciones de actividad](#activity-functions), [suborquestaciones](#sub-orchestrations), [espera por eventos externos](#external-events) y [temporizadores](#durable-timers). 

Una función de orquestador tiene que activarse mediante un [desencadenador de orquestación](durable-functions-bindings.md#orchestration-triggers).

Un orquestador se inicia con un [cliente orquestador](#client-functions). Puede desencadenar el orquestador desde cualquier origen (HTTP, cola, flujo de eventos). Cada instancia de una orquestación tiene un identificador de instancia. El identificador de instancia puede generarse automáticamente (recomendado) o por el usuario. Puede usar el identificador de instancia para [administrar instancias](durable-functions-instance-management.md) de la orquestación.

Para obtener más información y ejemplos, vea [Desencadenadores de orquestación](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>Funciones de cliente

Las funciones de cliente son las funciones desencadenadas que crean nuevas instancias de una orquestación. Las funciones de cliente son el punto de entrada para la creación de una instancia de una orquestación de Durable Functions. Puede desencadenar una función de cliente desde cualquier origen (HTTP, cola, flujo de eventos). Puede escribir una función de cliente en cualquier lenguaje que admita la aplicación. 

Las funciones de cliente también tienen un enlace de [cliente de orquestación](durable-functions-bindings.md#orchestration-client). Una función de cliente puede utilizar el enlace del cliente de orquestación para crear y administrar las orquestaciones durables del cliente. 

El ejemplo más básico de una función de cliente es una función desencadenada por HTTP que inicia una función de orquestador y devuelve una respuesta de comprobación de estado. Para obtener un ejemplo, vea [Detección de la dirección URL de la API de HTTP](durable-functions-http-api.md#http-api-url-discovery).

Para obtener más información y ejemplos, vea [Cliente de orquestación](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Características y patrones

Las secciones siguientes describen las características y los patrones de los tipos de Durable Functions.

### <a name="sub-orchestrations"></a>Suborquestaciones

Las funciones de Orchestrator pueden llamar a las funciones de actividad, pero pueden llamar también a otras funciones de orquestador. Por ejemplo, puede crear una orquestación mayor a partir de una biblioteca de funciones de orquestador. También puede ejecutar varias instancias de una función de orquestador en paralelo.

Para obtener más información y ejemplos, vea [Suborquestaciones](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Temporizadores durables

[Durable Functions](durable-functions-overview.md) proporciona *temporizadores durables* que puede usar en funciones de orquestador para implementar retrasos o configurar tiempos de expiración en acciones asincrónicas. Use temporizadores durables en funciones de orquestador en lugar de `Thread.Sleep` y `Task.Delay` (C#), o `setTimeout()` y `setInterval()` (JavaScript).

Para obtener más información y ejemplos, vea [Temporizadores en Durable Functions](durable-functions-timers.md).

### <a name="external-events"></a>Eventos externos

Las funciones de orquestador pueden esperar por los eventos externos para actualizar una instancia de orquestación. Esta característica de Durable Functions suele ser útil para controlar las interacciones humanas u otras devoluciones de llamada externas.

Para obtener más información y ejemplos, vea [Control de eventos externos con Durable Functions](durable-functions-external-events.md).

### <a name="error-handling"></a>Control de errores

Use código para implementar las orquestaciones de Durable Functions. Puede usar las características de control de errores del lenguaje de programación. Patrones como `try`/`catch` funcionan en la orquestación. 

Durable Functions también incluye algunas directivas de reintento integradas. Una acción puede retrasar y volver a intentar actividades de forma automática cuando ocurre una excepción. Puede usar reintentos para controlar las excepciones transitorias sin abandonar la orquestación.

Para obtener más información y ejemplos, vea [Control de errores con Durable Functions](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Comunicación entre aplicaciones de función

Aunque una orquestación durable se ejecuta en el contexto de una sola aplicación de función, puede usar patrones para coordinar las orquestaciones entre varias aplicaciones de función. La comunicación entre aplicaciones puede darse a través de HTTP, pero utilizar el marco durable para cada actividad significa que aún puede mantener un proceso durable entre dos aplicaciones.

En los ejemplos siguientes se demuestra la orquestación entre aplicaciones de función en C# y JavaScript. En cada ejemplo, una actividad inicia la orquestación externa. Otra actividad recupera y devuelve el estado. El orquestador espera a que el estado sea `Complete` antes de continuar.

Estos son algunos ejemplos de orquestación entre aplicaciones de función:

#### <a name="c"></a>C#

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration.
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete.
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now, go do more work...
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

    // Call a remote orchestration.
    const statusUrl = yield context.df.callActivity("StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete.
    while (true) {
        const isComplete = yield context.df.callActivity("CheckIsComplete", statusUrl);
        if (isComplete) {
            break;
        }

        const waitTime = moment(context.df.currentUtcDateTime).add(1, "m").toDate();
        yield context.df.createTimer(waitTime);
    }

    // B is done. Now, go do more work...
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

Para empezar, cree la primera función durable en [C#](durable-functions-create-first-csharp.md) o [JavaScript](quickstart-js-vscode.md).

> [!div class="nextstepaction"]
> [Más información sobre Durable Functions](durable-functions-bindings.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
