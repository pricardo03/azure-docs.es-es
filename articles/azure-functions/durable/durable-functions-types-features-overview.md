---
title: Tipos de funciones y características en la extensión Durable Functions de Azure Functions
description: Obtenga información sobre los tipos de funciones y funciones que admiten la comunicación de la función a la función en una orquestación de Durable Functions en Azure Functions.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
origin.date: 12/07/2018
ms.date: 03/19/2019
ms.author: v-junlch
ms.openlocfilehash: 76b6f013333113d5a24b744bc962d36b1c0e21b3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60731129"
---
# <a name="durable-functions-types-and-features-azure-functions"></a>Tipos de funciones duraderos y características (Azure Functions)

Durable Functions es una extensión de [Azure Functions](../functions-overview.md). Puede utilizar Durable Functions para la orquestación con estado de ejecución de la función. Una función durable es una solución que se compone de distintas funciones de Azure. Las funciones pueden desempeñan roles diferentes en una orquestación de función duradera. 

En este artículo se proporciona información general de los tipos de funciones que puede usar en una orquestación de Durable Functions. El artículo incluye algunos patrones comunes que puede usar para conectarse a las funciones. Obtenga información acerca de cómo Durable Functions puede ayudarle a solucionar los desafíos de desarrollo de aplicaciones.

![Una imagen que se muestra los tipos de durable functions][1]  

## <a name="types-of-durable-functions"></a>Tipos de funciones durables

Puede usar tres tipos de función duradera en Azure Functions: actividad, orchestrator y cliente.

### <a name="activity-functions"></a>Funciones de actividad

Las funciones de actividad son la unidad básica de trabajo en una orquestación de función duradera. Las funciones de actividad son las funciones y tareas que están organizadas en el proceso. Por ejemplo, podría crear una función durable para procesar un pedido. Las tareas implican comprobar el inventario, cobra al cliente y la creación de un envío. Cada tarea sería una función de actividad. 

Las funciones de actividad no impone restricciones en el tipo de trabajo que puede realizar en ellos. Puede escribir una función de actividad en cualquier [idiomas compatibles con Durable Functions](durable-functions-overview.md#language-support). El marco de trabajo de las tareas durables garantiza que cada función de actividad que se llame, se ejecutará al menos una vez durante una orquestación.

Use un [desencadenador de actividad](durable-functions-bindings.md#activity-triggers) para desencadenar una función de actividad. Funciones de .NET reciben un [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) como un parámetro. También puede enlazar el desencadenador con cualquier otro objeto para pasar en entradas a la función. En JavaScript, puede tener acceso a una entrada a través de la `<activity trigger binding name>` propiedad en el [ `context.bindings` objeto](../functions-reference-node.md#bindings).

La función de actividad también puede devolver valores para el orquestador. Si envía o devolver un gran número de valores de una función de actividad, puede usar [tuplas o matrices](durable-functions-bindings.md#passing-multiple-parameters). Puede desencadenar una función de actividad solo desde una instancia de orquestación. Aunque una función de actividad y otra función (por ejemplo, una función desencadenada por HTTP) podrían compartir código, cada función puede tener un solo desencadenador.

Para obtener más información y ejemplos, vea [las funciones de actividad](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Funciones de Orchestrator

Las funciones de orquestador describen cómo se ejecutan las acciones y el orden en que se ejecutan las acciones. La orquestación en el código describen las funciones de orquestador (C# o JavaScript) como se muestra en [patrones de Durable Functions y conceptos técnicos](durable-functions-concepts.md). Una orquestación puede tener muchos tipos diferentes de acciones, incluyendo [las funciones de actividad](#activity-functions), [suborquestaciones](#sub-orchestrations), [espera eventos externos](#external-events)y [temporizadores](#durable-timers). 

Una función de orquestador tiene que activarse mediante un [desencadenador de orquestación](durable-functions-bindings.md#orchestration-triggers).

Un orquestador se inicia con un [cliente orchestrator](#client-functions). Puede desencadenar el orquestador de cualquier origen (HTTP, cola, flujo de eventos). Cada instancia de una orquestación tiene un identificador de instancia. El identificador de instancia puede generarse automáticamente (recomendado) o generados por el usuario. Puede usar el identificador de instancia a [administrar instancias de](durable-functions-instance-management.md) de la orquestación.

Para obtener más información y ejemplos, vea [los desencadenadores de orquestación](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>Funciones de cliente

Las funciones del cliente son las funciones desencadenadas que crean nuevas instancias de una orquestación. Las funciones del cliente son el punto de entrada para la creación de una instancia de una orquestación de Durable Functions. Puede desencadenar una función de cliente de cualquier origen (HTTP, cola, flujo de eventos). Puede escribir una función de cliente en cualquier lenguaje que admita la aplicación. 

Las funciones de cliente también tienen un [de cliente de orquestación](durable-functions-bindings.md#orchestration-client) enlace. Una función de cliente puede utilizar el enlace para crear y administrar las orquestaciones duraderas del cliente de orquestación. 

El ejemplo más básico de una función de cliente es una función desencadenada por HTTP que se inicia una función de orquestador y, a continuación, devuelve una respuesta de comprobación de estado. Para obtener un ejemplo, vea [detección de direcciones URL de API HTTP](durable-functions-http-api.md#http-api-url-discovery).

Para obtener más información y ejemplos, vea [de cliente de orquestación](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Características y patrones

Las secciones siguientes describen las características y los patrones de los tipos de Durable Functions.

### <a name="sub-orchestrations"></a>Suborquestaciones

Las funciones de orquestador pueden llamar a funciones de actividad, pero también puede llamar a otras funciones de orquestador. Por ejemplo, puede crear una orquestación mayor a partir de una biblioteca de funciones de orquestador. O bien, puede ejecutar varias instancias de una función de orquestador en paralelo.

Para obtener más información y ejemplos, vea [suborquestaciones](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Temporizadores durables

[Durable Functions](durable-functions-overview.md) proporciona *temporizadores duraderos* que puede usar en las funciones de orquestador para implementar retrasos o configurar los tiempos de espera en acciones asincrónicas. Utilizar temporizadores durables en las funciones de orquestador en lugar de `Thread.Sleep` y `Task.Delay` (C#) o `setTimeout()` y `setInterval()` (JavaScript).

Para obtener más información y ejemplos, vea [temporizadores duraderos](durable-functions-timers.md).

### <a name="external-events"></a>Eventos externos

Las funciones de orquestador pueden esperar por los eventos externos para actualizar una instancia de orquestación. Esta característica de Durable Functions a menudo es útil para controlar la interacción humana u otras devoluciones de llamada externas.

Para obtener más información y ejemplos, vea [eventos externos](durable-functions-external-events.md).

### <a name="error-handling"></a>Control de errores

Usar código para implementar las orquestaciones de Durable Functions. Puede usar las características de control de errores del lenguaje de programación. Patrones como `try` / `catch` profesional en la orquestación. 

Durable Functions también vienen con directivas de reintento integradas. Una acción puede retrasar y vuelva a intentar actividades automáticamente cuando se produce una excepción. Puede usar reintentos para controlar las excepciones transitorias sin abandonar la orquestación.

Para obtener más información y ejemplos, vea [control de errores](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Comunicación entre aplicaciones de función

Aunque una orquestación durable se ejecuta en el contexto de una aplicación de función única, puede usar patrones para coordinar las orquestaciones en muchas aplicaciones de función. Comunicación entre aplicaciones que puede producirse a través de HTTP, pero mediante el marco duradero para cada actividad significa que todavía puede mantener un proceso duradero entre dos aplicaciones.

Los ejemplos siguientes muestran la orquestación de aplicación de función entre en C# y JavaScript. En cada ejemplo, una actividad inicia la orquestación externa. Otra actividad recupera y devuelve el estado. El orquestador espera a que el estado sea `Complete` antes de continuar.

Estos son algunos ejemplos de orquestación de aplicación de función de entre:

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
        $"https://appB.chinacloudsites.cn/orchestrations/{orchestratorName}",
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
        uri: `https://appB.chinacloudsites.cn/orchestrations/${orchestratorName}`,
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

Para empezar, cree la primera función duradera en [ C# ](durable-functions-create-first-csharp.md) o [JavaScript](quickstart-js-vscode.md).

> [!div class="nextstepaction"]
> [Obtenga más información acerca de Durable Functions](durable-functions-bindings.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png

<!-- Update_Description: wording update -->