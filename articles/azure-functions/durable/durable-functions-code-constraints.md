---
title: 'Restricciones de código de orquestador Durable: Azure Functions'
description: Reproducción de funciones de orquestación y restricciones de código para Azure Durable Functions.
author: cgillum
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 08/18/2019
ms.author: azfuncdf
ms.openlocfilehash: 87851a4879760c76950f765d05de4662ecb04bea
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935541"
---
# <a name="orchestrator-function-code-constraints"></a>Restricciones de código de las funciones de orquestador

Durable Functions es una extensión de [Azure Functions](../functions-overview.md) que le permite escribir aplicaciones con estado. Puede usar una [función de orquestador](durable-functions-orchestrations.md) para organizar la ejecución de otras funciones de Durable Functions dentro de una aplicación de función. Las funciones de orquestador tienen estado y son confiables y potencialmente de larga duración.

## <a name="orchestrator-code-constraints"></a>Restricciones de código del orquestador

Las funciones de orquestador usan [el suministro de eventos](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) para garantizar la ejecución confiable y mantener el estado de la variable local. El [comportamiento de reproducción](durable-functions-orchestrations.md#reliability) del código de orquestador crea restricciones sobre el tipo de código que puede escribir en una función de orquestador. Por ejemplo, el código de orquestador debe ser *determinista*.  Las funciones de orquestador se reproducirán varias veces y deben producir el mismo resultado siempre.

En las secciones siguientes se proporcionan algunas directrices sencillas para asegurarse de que el código sea determinista.

### <a name="using-deterministic-apis"></a>Uso de API deterministas

Las funciones de orquestador son gratuitas para llamar a cualquier API que deseen en su idioma de destino. Sin embargo, es importante que las funciones de orquestador solo llamen a las API *deterministas*. Una *API determinista* es aquella que siempre devuelve exactamente el mismo valor con la misma entrada, con independencia de cuándo o con qué frecuencia se llama.

A continuación se muestran algunos ejemplos de API que deben evitarse porque *no* son deterministas. Estas restricciones solo se aplican a las funciones de orquestador. Otros tipos de funciones no tienen estas restricciones.

| Categoría de API | Motivo | Solución alternativa |
| ------------ | ------ | ---------- |
| Fechas/horas  | Las API que devuelven la fecha o la hora _actuales_ son no deterministas porque el valor devuelto será diferente para cada reproducción. | Use la API [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) (.NET) o `currentUtcDateTime` (JavaScript) API, que es segura para la reproducción. |
| GUID/UUID  | Las API que devuelven un GUID/UUID _aleatorio_ son no deterministas porque el valor devuelto será diferente para cada reproducción. | Use [NewGuid](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_NewGuid) (.NET) o `newGuid` (JavaScript) para generar GUID aleatorios de forma segura. |
| Números aleatorios | Las API que devuelven números aleatorios son no deterministas porque el valor generado será diferente para cada reproducción. | Use una función de actividad para devolver números aleatorios a una orquestación. Los valores devueltos de las funciones de actividad siempre son seguros para la reproducción. |
| Enlaces | Los enlaces de entrada y salida normalmente realizan operaciones de E/S y no son deterministas. No se deben usar tampoco el [cliente de orquestación](durable-functions-bindings.md#orchestration-client) ni el [cliente de entidad](durable-functions-bindings.md#entity-client) directamente en una función de orquestador. | Use los enlaces de entrada y salida dentro de las funciones de cliente o actividad. |
| Red | Las llamadas de red implican sistemas externos y son no deterministas. | Use funciones de actividad para realizar llamadas de red. Si necesita realizar una llamada HTTP desde la función de orquestador, también tiene la opción de usar las [API HTTP duraderas](durable-functions-http-features.md#consuming-http-apis). |
| Bloqueo de API | Las API de bloqueo como `Thread.Sleep` (.NET) u otras similares pueden provocar problemas de rendimiento y escalado para las funciones de orquestador y deben evitarse. En el plan de consumo de Azure Functions, incluso pueden provocar cargos innecesarios en tiempo de ejecución. | Use alternativas a las API de bloqueo cuando estén disponibles, por ejemplo `CreateTimer`, para introducir retrasos en la ejecución de la orquestación. Los retrasos del [temporizador duradero](durable-functions-timers.md) no cuentan en el tiempo de ejecución de una función de orquestador. |
| API asincrónicas | El código de orquestador **no debe iniciar operaciones asincrónicas**, excepto con [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) API o `context.df` API del objeto. Por ejemplo, no hay `Task.Run`, `Task.Delay` o `HttpClient.SendAsync` en .NET, o `setTimeout()` y `setInterval()` en JavaScript. Durable Task Framework ejecuta código de orquestador en un solo subproceso y no puede interactuar con otros puedan estar programados por otras API asincrónicas. | Una función de orquestador solo debe realizar llamadas asincrónicas *duraderas*. Cualquier otra llamada de API asincrónica debe realizarse desde las funciones de actividad. |
| Funciones asincrónicas de JavaScript | Las funciones de orquestador de JavaScript no pueden ser `async` porque el tiempo de ejecución de node.js no garantiza que las funciones asincrónicas sean deterministas. | Las funciones de orquestador JavaScript deben declararse como funciones del generador sincrónico. |
| API de subprocesos | Durable Task Framework ejecuta código de orquestador en un solo subproceso y no puede interactuar con otros. La introducción de nuevos subprocesos en la ejecución de una orquestación puede dar lugar a interbloqueos o ejecuciones no deterministas. | Las API de subprocesos nunca deben usarse en las funciones de orquestador. Si son necesarias, deben limitarse a las funciones de actividad. |
| Variables estáticas | Las variables estáticas no constantes deben evitarse en las funciones de orquestador porque sus valores pueden cambiar con el tiempo, lo que da lugar a un comportamiento de ejecución no determinista. | Use constantes o limite el uso de variables estáticas a funciones de actividad. |
| Variables de entorno | No use variables de entorno en las funciones de orquestador. Sus valores pueden cambiar con el tiempo, lo que da lugar a un comportamiento de ejecución no determinista. | Solo se debe hacer referencia a las variables de entorno desde las funciones de cliente o las funciones de actividad. |
| Bucles infinitos | En las funciones de orquestador deben evitarse los bucles infinitos. Durable Task Framework guarda el historial de ejecución a medida que avanza la función de orquestación, de modo que un bucle infinito podría provocar que una instancia de orquestador se quedara sin memoria. | Para escenarios de bucle infinito, use API como [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) (.NET) o `continueAsNew` (JavaScript) para reiniciar la ejecución de la función y descartar el historial de ejecución anterior. |

Aunque estas restricciones pueden intimidar al principio, en la práctica se siguen fácilmente. Durable Task Framework intenta detectar las infracciones de las reglas anteriores y produce `NonDeterministicOrchestrationException`. Sin embargo, este comportamiento de detección no es confiable al 100 % y no debe depender de él.

## <a name="versioning"></a>Control de versiones

Una orquestación duradera puede ejecutarse continuamente durante días, meses, años o incluso [indefinidamente](durable-functions-eternal-orchestrations.md). Las actualizaciones de código realizadas en aplicaciones Durable Functions que afecten a las orquestaciones que no se hayan completado todavía pueden interrumpir su comportamiento de reproducción. Por lo tanto, es importante planear cuidadosamente la realización de actualizaciones en el código. Para una descripción más detallada de cómo se debe hacer una versión del código, consulte el artículo [Control de versiones](durable-functions-versioning.md).

## <a name="durable-tasks"></a>Tareas durables

> [!NOTE]
> En esta sección se describen los detalles internos de implementación de Durable Task Framework. Puede utilizar Durable Functions sin necesidad de conocer esta información. Su único fin es ayudarle a entender el comportamiento de reproducción.

Las tareas que se pueden esperar de forma segura en las funciones de orquestador en ocasiones se conocen como *tareas durables*. Estas tareas se crean y se administran mediante Durable Task Framework. Algunos ejemplos son las tareas devueltas por `CallActivityAsync`, `CreateTimer` y `WaitForExternalEvent` en las funciones de orquestador de .NET.

Estas *tareas duraderas* se administran internamente mediante una lista de objetos `TaskCompletionSource` en .NET. Durante la reproducción, estas tareas se crean como parte de la ejecución del código de orquestador y se llevan a cabo según el distribuidor va enumerando los eventos correspondientes del historial. La ejecución se lleva a cabo sincrónicamente con un único subproceso hasta que se reproduce todo el historial. Las tareas duraderas, que no se completan al final de la reproducción del historial, llevan las acciones correspondientes asociadas. Por ejemplo, un mensaje puede ponerse en cola para llamar a una función de actividad.

El comportamiento de ejecución descrito aquí le ayudará a entender por qué el código de la función de orquestador no debe ser nunca `await` ni `yield` para una tarea que no sea duradera: el subproceso del distribuidor no puede esperar a que finalice y cualquier devolución de llamada de esa tarea podría dañar el estado de seguimiento de la función de orquestador. Existen comprobaciones en tiempo de ejecución para intentar detectar estas infracciones.

Si desea más información acerca de cómo Durable Task Framework ejecuta funciones de orquestador, lo mejor es consultar el [código fuente de la instancia de Durable Task en GitHub](https://github.com/Azure/durabletask). En concreto, consulte [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) y [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo invocar a suborquestaciones](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [Aprenda a controlar las versiones](durable-functions-versioning.md)
