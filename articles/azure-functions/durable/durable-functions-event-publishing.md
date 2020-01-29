---
title: Publicación de Durable Functions en Azure Event Grid (versión preliminar)
description: Aprenda a configurar la publicación automática en Azure Event Grid de Durable Functions.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 768af2e89d6523f50bd9fcc3d13cc84b711cc6f0
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76547479"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Publicación de Durable Functions en Azure Event Grid (versión preliminar)

En este artículo se muestra cómo configurar Durable Functions para publicar eventos de ciclo de vida de orquestación (como los creados, los completados y los que contienen errores) en un [tema de Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) personalizado.

A continuación se muestran algunos escenarios donde esta característica resulta útil:

* **Escenarios de DevOps, como las implementaciones Azul/Verde**: por si quiere saber si hay tareas en ejecución antes de implementar la [estrategia de implementación en paralelo](durable-functions-versioning.md#side-by-side-deployments).

* **Compatibilidad con la supervisión y el diagnóstico avanzados**: puede realizar el seguimiento de la información de estado de la orquestación en un almacén externo optimizado para las consultas, como SQL Database o CosmosDB.

* **Actividad de ejecución en segundo plano de larga duración**: si se utiliza Durable Functions para una actividad de ejecución en segundo plano de larga duración, esta característica le ayuda a conocer el estado actual.

[!INCLUDE [v1-note](../../../includes/functions-durable-v1-tutorial-note.md)]

## <a name="prerequisites"></a>Prerequisites

* Instale [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) en el proyecto de Durable Functions.
* Instale el [Emulador de Azure Storage](../../storage/common/storage-use-emulator.md).
* Instale la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) o use [Azure Cloud Shell](../../cloud-shell/overview.md).

## <a name="create-a-custom-event-grid-topic"></a>Creación de un tema de Event Grid personalizado

Cree un tema de Event Grid para enviar eventos desde Durable Functions. Las instrucciones siguientes muestran cómo crear un tema con la CLI de Azure. Para información acerca de cómo hacerlo con PowerShell o Azure Portal, consulte los artículos siguientes:

* [Inicios rápidos de EventGrid: Creación de eventos personalizados: PowerShell](../../event-grid/custom-event-quickstart-powershell.md)
* [Inicios rápidos de EventGrid: Creación de eventos personalizados: Azure Portal](../../event-grid/custom-event-quickstart-portal.md)

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando `az group create`. Actualmente, Azure Event Grid no se admite en todas las regiones. Para información sobre las regiones donde se admite, consulte la [introducción a Azure Event Grid](../../event-grid/overview.md).

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Creación de un tema personalizado

Un tema de Event Grid proporciona un punto de conexión definido por el usuario en el que publicar eventos. Reemplace `<topic_name>` por un nombre único para el tema. El nombre del tema debe ser único, ya que se convierte en entrada DNS.

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>Obtención del punto de conexión y la clave

Obtenga el punto de conexión del tema. Reemplace `<topic_name>` por el nombre que haya elegido.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Obtenga la clave del tema. Reemplace `<topic_name>` por el nombre que haya elegido.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Ahora puede enviar eventos al tema.

## <a name="configure-azure-event-grid-publishing"></a>Configuración de la publicación en Azure Event Grid

En su proyecto de Durable Functions, busque el archivo `host.json`.

Agregue `eventGridTopicEndpoint` y `eventGridKeySettingName` en una propiedad `durableTask`.

```json
{
    "durableTask": {
        "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "eventGridKeySettingName": "EventGridKey"
    }
}
```

Las posibles propiedades de configuración de Azure Event Grid se pueden encontrar en la [documentación de host.json](../functions-host-json.md#durabletask). Después de configurar el archivo `host.json`, la aplicación de función envía eventos del ciclo de vida al tema de Event Grid. Esto funciona cuando se ejecuta la aplicación de función tanto localmente como en Azure.```

Establezca la configuración de la aplicación para la clave del tema en la aplicación de función y en `local.settings.json`. El siguiente JSON es un ejemplo de `local.settings.json` para la depuración local. Reemplace `<topic_key>` por la clave del tema.  

```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
        "EventGridKey": "<topic_key>"
    }
}
```

Asegúrese de que el [emulador de Storage](../../storage/common/storage-use-emulator.md) funciona. Es conveniente ejecutar el comando `AzureStorageEmulator.exe clear all` antes de la ejecución.

## <a name="create-functions-that-listen-for-events"></a>Creación de funciones que realicen escuchas para los eventos

Cree una aplicación de función. Se recomienda ubicarla en la misma región que el tema de Event Grid.

### <a name="create-an-event-grid-trigger-function"></a>Creación de una función de desencadenador de Event Grid

Cree una función que reciba los eventos de ciclo de vida. Seleccione **Función personalizada**.

![Selección de creación de una función personalizada](./media/durable-functions-event-publishing/functions-portal.png)

Elija Desencadenador de Event Grid y seleccione `C#`.

![Selección de Desencadenador de Event Grid](./media/durable-functions-event-publishing/eventgrid-trigger.png)

Escriba el nombre de la función y seleccione `Create`.

![Creación de Desencadenador de Event Grid](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Se creará una función con el código siguiente:

#### <a name="precompiled-c"></a>C# precompilado
```csharp
public static void Run([HttpTrigger] JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

#### <a name="c-script"></a>Script de C#

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

public static void Run(JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

Seleccione `Add Event Grid Subscription`. Esta operación agrega una suscripción a Event Grid para el tema de Event Grid que ha creado. Para más información, consulte [Conceptos de Azure Event Grid](https://docs.microsoft.com/azure/event-grid/concepts).

![Selección del vínculo Desencadenador de Event Grid](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Seleccione `Event Grid Topics` como **Tipo de tema**. Seleccione el grupo de recursos que ha creado para el tema de Event Grid. A continuación, seleccione la instancia del tema de Event Grid. Presione `Create`.

![Cree una suscripción de Event Grid.](./media/durable-functions-event-publishing/eventsubscription.png)

Ahora está preparado para recibir eventos de ciclo de vida.

## <a name="create-durable-functions-to-send-the-events"></a>Creación de una instancia de Durable Functions para enviar eventos

En el proyecto de Durable Functions, inicie la depuración en la máquina local.  El código siguiente es el mismo que el de la plantilla de Durable Functions. Ya se han configurado `host.json` y `local.settings.json` en la máquina local.

### <a name="precompiled-c"></a>C# precompilado

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.DurableTask;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace LifeCycleEventSpike
{
    public static class Sample
    {
        [FunctionName("Sample")]
        public static async Task<List<string>> RunOrchestrator(
            [OrchestrationTrigger] IDurableOrchestrationContext context)
        {
            var outputs = new List<string>();

            // Replace "hello" with the name of your Durable Activity Function.
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Tokyo"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Seattle"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "London"));

            // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
            return outputs;
        }

        [FunctionName("Sample_Hello")]
        public static string SayHello([ActivityTrigger] string name, ILogger log)
        {
            log.LogInformation($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")] HttpRequestMessage req,
            [DurableClient] IDurableOrchestrationClient starter,
            ILogger log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

> [!NOTE]
> El código anterior corresponde a Durable Functions 2.x. En el caso de Durable Functions 1.x, debe usar `DurableOrchestrationContext` en lugar de `IDurableOrchestrationContext`, el atributo `OrchestrationClient` en lugar del atributo `DurableClient` además de usar el tipo de parámetro `DurableOrchestrationClient` en lugar de `IDurableOrchestrationClient`. Para obtener más información sobre las diferencias entre versiones, vea el artículo [Versiones de Durable Functions](durable-functions-versions.md).

Si se llama a `Sample_HttpStart` con Postman o el explorador, la instancia de Durable Functions comienza a enviar eventos de ciclo de vida. El punto de conexión suele ser `http://localhost:7071/api/Sample_HttpStart` para la depuración local.

Consulte los registros de la función que ha creado en Azure Portal.

```
2019-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2019-04-20T09:28:21.104 [Info] {
    "id": "054fe385-c017-4ce3-b38a-052ac970c39d",
    "subject": "durable/orchestrator/Running",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Running"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2019-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2019-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2019-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2019-04-20T09:28:37.098 [Info] {
    "id": "8cf17246-fa9c-4dad-b32a-5a868104f17b",
    "subject": "durable/orchestrator/Completed",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Completed"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2019-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2019-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Esquema de eventos

En la lista siguiente se explica el esquema de los eventos de ciclo de vida:

* **`id`** : identificador único del evento de Event Grid.
* **`subject`** : ruta de acceso al asunto del evento. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}` será `Running`, `Completed`, `Failed` y `Terminated`.  
* **`data`** : parámetros específicos de Durable Functions.
  * **`hubName`** : nombre de [TaskHub](durable-functions-task-hubs.md).
  * **`functionName`** : nombre de una función de orquestador.
  * **`instanceId`** : instanceId de Durable Functions.
  * **`reason`** : información adicional asociada al evento de seguimiento. Para más información, consulte [Diagnóstico con Durable Functions (Azure Functions)](durable-functions-diagnostics.md)
  * **`runtimeStatus`** : estado del entorno en tiempo de ejecución de la orquestación. Running, Completed, Failed, Canceled.
* **`eventType`** : "orchestratorEvent"
* **`eventTime`** : hora del evento (UTC).
* **`dataVersion`** : versión del esquema de los eventos de ciclo de vida.
* **`metadataVersion`** :  versión de los metadatos.
* **`topic`** : recurso del tema de Event Grid.

## <a name="how-to-test-locally"></a>Prueba local

Para las pruebas locales, consulte [Depuración local de funciones de Azure desencadenadas por Event Grid](../functions-debug-event-grid-trigger-local.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda a administrar instancias en Durable Functions](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Aprenda sobre el control de versiones en Durable Functions](durable-functions-versioning.md)
