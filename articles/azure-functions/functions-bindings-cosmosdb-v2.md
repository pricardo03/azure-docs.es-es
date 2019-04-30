---
title: Enlaces de Azure Cosmos DB para Functions 2.x
description: Descubra cómo utilizar desencadenadores y enlaces de almacenamiento de Azure Cosmos DB en Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: azure functions, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 5762e934d7735dd9617cefc1f56105823d74312f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61343035"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x"></a>Enlaces de Azure Cosmos DB para Azure Functions 2.x

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versión 1](functions-bindings-cosmosdb.md)
> * [Versión 2](functions-bindings-cosmosdb-v2.md)

En este artículo se explica cómo trabajar con enlaces de [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md) en Azure Functions 2.x. Azure Functions enlaces de desencadenador, de entrada y de salida para Azure Cosmos DB.

> [!NOTE]
> Este artículo va dirigido a [Azure Functions 2.x](functions-versions.md).  Para obtener información acerca del uso de estos enlaces en Functions 1.x, consulte [Enlaces de Azure Cosmos DB para Azure Functions 1.x](functions-bindings-cosmosdb.md).
>
> Este enlace se llamaba originalmente DocumentDB. En la versión 2.x de Functions, tanto el desencadenador, como los enlaces y el paquete se denominan Cosmos DB.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-apis"></a>API admitidas

[!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

Los enlaces de Azure Cosmos DB para la versión 2.x de Functions se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB), versión 3.x. El código fuente de los enlaces está en el repositorio [azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Desencadenador

El desencadenador de Azure Cosmos DB utiliza la [fuente de cambios de Azure Cosmos DB](../cosmos-db/change-feed.md) para estar atento a las inserciones y actualizaciones de las particiones. La fuente de cambios publica inserciones y actualizaciones, no eliminaciones.

## <a name="trigger---example"></a>Desencadenador: ejemplo

Vea el ejemplo específico del lenguaje:

* [C#](#trigger---c-example)
* [Script de C# (.csx)](#trigger---c-script-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

Ejemplos de omisión de desencadenador

### <a name="trigger---c-example"></a>Desencadenador: ejemplo de C#

En el ejemplo siguiente se muestra un [función de C#](functions-dotnet-class-library.md) que se invoca cuando hay inserciones y actualizaciones en la base de datos y colección especificadas.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents, 
            ILogger log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.LogInformation($"Documents modified: {documents.Count}");
                log.LogInformation($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

Ejemplos de omisión de desencadenador

### <a name="trigger---c-script-example"></a>Desencadenador: ejemplo de script de C#

En el ejemplo siguiente se muestra un enlace de desencadenador de Cosmos DB en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función escribe mensajes de registro cuando se modifican los registros de Cosmos DB.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Este es el código de script de C#:

```cs
    #r "Microsoft.Azure.DocumentDB.Core"

    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using Microsoft.Extensions.Logging;

    public static void Run(IReadOnlyList<Document> documents, ILogger log)
    {
      log.LogInformation("Documents modified " + documents.Count);
      log.LogInformation("First document Id " + documents[0].Id);
    }
```

Ejemplos de omisión de desencadenador

### <a name="trigger---javascript-example"></a>Desencadenador: ejemplo de JavaScript

En el ejemplo siguiente se muestra un enlace de desencadenador de Cosmos DB en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función escribe mensajes de registro cuando se modifican los registros de Cosmos DB.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Este es el código de JavaScript:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

### <a name="trigger---java-example"></a>Desencadenador: ejemplo de Java

En el ejemplo siguiente se muestra un enlace de desencadenador de Cosmos DB en el archivo *function.json* y una [función de Java](functions-reference-java.md) que usa el enlace. La función se invoca cuando hay inserciones y actualizaciones en la base de datos y colección especificadas.

```json
{
    "type": "cosmosDBTrigger",
    "name": "items",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "AzureCosmosDBConnection",
    "databaseName": "ToDoList",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": false
}
```

Este es el código de Java:

```java
    @FunctionName("cosmosDBMonitor")
    public void cosmosDbProcessor(
        @CosmosDBTrigger(name = "items",
            databaseName = "ToDoList",
            collectionName = "Items",
            leaseCollectionName = "leases",
            createLeaseCollectionIfNotExists = true,
            connectionStringSetting = "AzureCosmosDBConnection") String[] items,
            final ExecutionContext context ) {
                context.getLogger().info(items.length + "item(s) is/are changed.");
            }
```


En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@CosmosDBTrigger` en los parámetros cuyo valor provendría de Cosmos DB.  Esta anotación se puede usar con tipos nativos de Java, POJO o valores que aceptan valores NULL mediante Optional<T>.


Ejemplos de omisión de desencadenador

### <a name="trigger---python-example"></a>Ejemplo de desencadenador de Python

En el ejemplo siguiente se muestra un enlace del desencadenador de Cosmos DB en un archivo *function.json* y una [función de Python](functions-reference-python.md) que usa dicho enlace. La función escribe mensajes de registro cuando se modifican los registros de Cosmos DB.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDBTrigger",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

Este es el código de Python:

```python
    import logging
    import azure.functions as func


    def main(documents: func.DocumentList) -> str:
        if documents:
            logging.info('First document Id modified: %s', documents[0]['id'])
```

## <a name="trigger---c-attributes"></a>Desencadenador: atributos de C#

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs).

El constructor del atributo toma el nombre de la base de datos y el nombre de la colección. Para información sobre esos valores y otras propiedades que puede configurar, consulte [Desencadenador: configuración](#trigger---configuration). A continuación, se muestra un ejemplo del atributo `CosmosDBTrigger` en una signatura de método:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        ILogger log)
    {
        ...
    }
```

Para un ejemplo completo, consulte [Desencadenador: ejemplo de C#](#trigger---c-example).


## <a name="trigger---configuration"></a>Desencadenador: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `CosmosDBTrigger`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type** || Se debe establecer en `cosmosDBTrigger`. |
|**dirección** || Se debe establecer en `in`. Este parámetro se establece automáticamente cuando se crea el desencadenador en Azure Portal. |
|**name** || Nombre de la variable que se utiliza en el código de función y que representa la lista de documentos con los cambios. |
|**connectionStringSetting**|**ConnectionStringSetting** | Nombre de una configuración de aplicación que contiene la cadena de conexión utilizada para conectarse a la cuenta de Azure Cosmos DB que se está supervisando. |
|**databaseName**|**DatabaseName**  | Nombre de la base de datos de Azure Cosmos DB con la colección que se está supervisando. |
|**collectionName** |**CollectionName** | Nombre de la colección que se está supervisando. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (Opcional) Nombre de una configuración de aplicación que contiene la cadena de conexión al servicio que incluye la colección de concesiones. Si no se establece, se usa el valor `connectionStringSetting`. Este parámetro se establece automáticamente cuando se crea el enlace en el portal. La cadena de conexión para la colección de concesiones debe tener permisos de escritura.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (Opcional) Nombre de la base de datos que contiene la colección que se usa para almacenar las concesiones. Si no se establece, se usa el valor de la configuración `databaseName`. Este parámetro se establece automáticamente cuando se crea el enlace en el portal. |
|**leaseCollectionName** | **LeaseCollectionName** | (Opcional) Nombre de la colección que se usa para almacenar concesiones. Si no se establece, se usa el valor `leases`. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (Opcional) Cuando se establece en `true`, la colección de concesiones se crea automáticamente cuando todavía no existe. El valor predeterminado es `false`. |
|**leasesCollectionThroughput**| **LeasesCollectionThroughput**| (Opcional) Define la cantidad de unidades de solicitud que se asignan cuando se crea la colección de concesiones. Esta configuración solo se usa cuando `createLeaseCollectionIfNotExists` se establece en `true`. Este parámetro se establece automáticamente cuando el enlace se crea con el portal.
|**leaseCollectionPrefix**| **LeaseCollectionPrefix**| (Opcional) Al establecerse, agrega un prefijo a las concesiones creadas en la recopilación de concesiones para esta función, permitiendo de forma eficaz a dos funciones de Azure independientes compartir la misma recopilación de concesiones con prefijos diferentes.
|**feedPollDelay**| **FeedPollDelay**| (Opcional) Al establecerse, define, en milisegundos, el retraso entre sondeos de una partición en busca de nuevos cambios en la fuente, después de que todos los cambios actuales se purguen. El valor predeterminado es 5000 (5 segundos).
|**leaseAcquireInterval**| **LeaseAcquireInterval**| (Opcional) Al establecerse, define, en milisegundos, el intervalo para iniciar una tarea para calcular si las particiones se distribuyen uniformemente entre las instancias de host conocidas. El valor predeterminado es 13 000 (13 segundos).
|**leaseExpirationInterval**| **LeaseExpirationInterval**| (Opcional) Al establecerse, define, en milisegundos, el intervalo para el que se toma la concesión en una concesión que representa una partición. Si la concesión no se renueva dentro de este intervalo, expirará y la propiedad de la partición se moverá a otra instancia. El valor predeterminado es 60 000 (60 segundos).
|**leaseRenewInterval**| **LeaseRenewInterval**| (Opcional) Al establecerse, define, en milisegundos, el intervalo de renovación para todas las concesiones para las particiones que mantiene una instancia actualmente. El valor predeterminado es 17 000 (17 segundos).
|**checkpointFrequency**| **CheckpointFrequency**| (Opcional) Al establecerse, define, en milisegundos, el intervalo entre los puntos de comprobación de las concesiones. El valor predeterminado siempre se encuentra después de una llamada de función.
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (Opcional) Al establecerse, personaliza la cantidad máxima de elementos recibidos por la llamada de función.
|**startFromBeginning**| **StartFromBeginning**| (Opcional) Cuando se establece, indica al desencadenador que comience a leer los cambios desde el principio del historial de la colección en lugar de desde la hora actual. Esto solo funciona la primera vez que se inicia el desencadenador, ya que en las ejecuciones posteriores, los puntos de control ya están almacenados. Si esta opción se establece en `true` cuando hay concesiones creadas no tiene ningún efecto.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Desencadenador: uso

El desencadenador requiere una segunda colección que utiliza para almacenar _concesiones_ en las particiones. La colección que se está supervisando y la colección que contiene las concesiones deben estar disponibles para que el desencadenador funcione.

>[!IMPORTANT]
> Si hay varias funciones configuradas para usar un desencadenador de Cosmos DB para la misma colección, cada una de ellas debe usar una colección de concesión dedicada, o bien se debe especificar un valor de `LeaseCollectionPrefix` para cada función. En caso contrario, se desencadenará solo una de las funciones. Para obtener información acerca del prefijo, consulte la [sección de configuración](#trigger---configuration).

El desencadenador no indica si un documento se actualizó o se insertó; solo proporciona el propio documento. Si tiene que administrar las actualizaciones e inserciones de manera diferente, puede hacerlo mediante la implementación de campos de marca de tiempo para las inserciones o actualizaciones.

## <a name="input"></a>Entrada

El enlace de entrada de Azure Cosmos DB usa SQL API para recuperar uno o varios documentos de Azure Cosmos DB y los pasa al parámetro de entrada de la función. Se puede determinar el identificador de documento o los parámetros de consulta según el desencadenador que invoca la función.

## <a name="input---examples"></a>Entrada: ejemplos

Vea los ejemplos específicos del lenguaje que leen un solo documento mediante la especificación de un valor del identificador:

* [C#](#input---c-examples)
* [Script de C# (.csx)](#input---c-script-examples)
* [F#](#input---f-examples)
* [Java](#input---java-examples)
* [JavaScript](#input---javascript-examples)
* [Python](#input---python-examples)

[Omisión de los ejemplos de entrada](#input---attributes)

### <a name="input---c-examples"></a>Entrada: ejemplos en C#

En esta sección se incluyen los ejemplos siguientes:

* [Desencadenador de cola, buscar identificador de JSON](#queue-trigger-look-up-id-from-json-c)
* [Desencadenador de HTTP, buscar identificador de cadena de consulta](#http-trigger-look-up-id-from-query-string-c)
* [Desencadenador de HTTP, buscar identificador de datos de ruta](#http-trigger-look-up-id-from-route-data-c)
* [Desencadenador de HTTP, buscar identificador de datos de ruta, mediante SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [Desencadenador de HTTP, obtener varios documentos, mediante SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [Desencadenador de HTTP, obtener varios documentos, mediante DocumentClient](#http-trigger-get-multiple-docs-using-documentclient-c)

Los ejemplos hacen referencia a un tipo de `ToDoItem` simple:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Desencadenador de cola, buscar identificador de JSON (C#)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que recupera un documento individual. La función la desencadena un mensaje de la cola que contiene un objeto JSON. El desencadenador de la cola analiza el JSON en un objeto denominado `ToDoItemLookup`, que contiene el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>Desencadenador de HTTP, buscar identificador de cadena de consulta (C#)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza una cadena de consulta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

>[!NOTE]
>El parámetro de la cadena de consulta HTTP distingue mayúsculas de minúsculas.
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>Desencadenador de HTTP, buscar identificador de datos de ruta (C#)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza los datos de la ruta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>Desencadenador de HTTP, buscar identificador de datos de ruta, mediante SqlQuery (C#)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza los datos de la ruta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

En el ejemplo se muestra cómo utilizar una expresión de enlace en el parámetro `SqlQuery`. Puede pasar datos de ruta al parámetro `SqlQuery` tal como se muestra, pero actualmente [no se pueden pasar valores de una cadena de consulta](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).


```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>Desencadenador de HTTP, obtener varios documentos, mediante SqlQuery (C#)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que recupera una lista de documentos. La función la desencadena una solicitud HTTP. La consulta se especifica en la propiedad del atributo `SqlQuery`.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>Desencadenador de HTTP, obtener varios documentos, mediante DocumentClient (C#)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que recupera una lista de documentos. La función la desencadena una solicitud HTTP. El código usa una instancia de `DocumentClient` que proporciona el enlace de Azure Cosmos DB para leer una lista de documentos. La instancia de `DocumentClient` también se puede usar para las operaciones de escritura.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.LogInformation($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.LogInformation(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

### <a name="input---c-script-examples"></a>Entrada: ejemplos de script de C#

En esta sección se incluyen los ejemplos siguientes:

* [Desencadenador de cola, buscar identificador de una cadena](#queue-trigger-look-up-id-from-string-c-script)
* [Desencadenador de colas, obtener varios documentos, mediante SqlQuery](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [Desencadenador de HTTP, buscar identificador de cadena de consulta](#http-trigger-look-up-id-from-query-string-c-script)
* [Desencadenador de HTTP, buscar identificador de datos de ruta](#http-trigger-look-up-id-from-route-data-c-script)
* [Desencadenador de HTTP, obtener varios documentos, mediante SqlQuery](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [Desencadenador de HTTP, obtener varios documentos, mediante DocumentClient](#http-trigger-get-multiple-docs-using-documentclient-c-script)

Los ejemplos de desencadenador de HTTP hacen referencia a un tipo simple `ToDoItem`:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-string-c-script"></a>Desencadenador de cola, buscar identificador de una cadena (script de C#)

En el ejemplo siguiente se muestra un enlace de entrada de Cosmos DB en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función lee un solo documento y actualiza el valor de texto del documento.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de script de C#:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {   
      inputDocument.text = "This has changed.";
    }
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>Desencadenador de colas, obtener varios documentos, mediante SqlQuery (script de C#)

En el ejemplo siguiente se muestra un enlace de entrada de Azure Cosmos DB en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función recupera varios documentos especificados por una consulta SQL con un desencadenador de cola para personalizar los parámetros de la consulta.

El desencadenador de cola proporciona un parámetro `departmentId`. Un mensaje de la cola de `{ "departmentId" : "Finance" }` devolvería todos los registros para el departamento de finanzas.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de script de C#:

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {   
        foreach (var doc in documents)
        {
            // operate on each document
        }    
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c-script"></a>Desencadenador de HTTP, buscar identificador de cadena de consulta (script de C#)

En el ejemplo siguiente se muestra una [función de script de C#](functions-reference-csharp.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza una cadena de consulta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": false
}
```

Este es el código de script de C#:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c-script"></a>Desencadenador de HTTP, buscar identificador de datos de ruta (script de C#)

En el ejemplo siguiente se muestra una [función de script de C#](functions-reference-csharp.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza los datos de la ruta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Este es el código de script de C#:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>Desencadenador de HTTP, obtener varios documentos, mediante SqlQuery (script de C#)

En el ejemplo siguiente se muestra una [función de script de C#](functions-reference-csharp.md) que recupera una lista de documentos. La función la desencadena una solicitud HTTP. La consulta se especifica en la propiedad del atributo `SqlQuery`.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

Este es el código de script de C#:

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.LogInformation(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>Desencadenador de HTTP, obtener varios documentos, mediante DocumentClient (script de C#)

En el ejemplo siguiente se muestra una [función de script de C#](functions-reference-csharp.md) que recupera una lista de documentos. La función la desencadena una solicitud HTTP. El código usa una instancia de `DocumentClient` que proporciona el enlace de Azure Cosmos DB para leer una lista de documentos. La instancia de `DocumentClient` también se puede usar para las operaciones de escritura.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

Este es el código de script de C#:

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.LogInformation($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.LogInformation(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Omisión de los ejemplos de entrada](#input---attributes)

### <a name="input---javascript-examples"></a>Entrada: ejemplos en JavaScript

Esta sección contiene los siguientes ejemplos que leen un solo documento mediante la especificación de un valor de identificación desde varios orígenes:

* [Desencadenador de cola, buscar identificador de JSON](#queue-trigger-look-up-id-from-json-javascript)
* [Desencadenador de HTTP, buscar identificador de cadena de consulta](#http-trigger-look-up-id-from-query-string-javascript)
* [Desencadenador de HTTP, buscar identificador de datos de ruta](#http-trigger-look-up-id-from-route-data-javascript)
* [Desencadenador de colas, obtener varios documentos, mediante SqlQuery](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-javascript"></a>Desencadenador de cola, buscar identificador de JSON (JavaScript)

En el ejemplo siguiente se muestra un enlace de entrada de Cosmos DB en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función lee un solo documento y actualiza el valor de texto del documento.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {   
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-javascript"></a>Desencadenador de HTTP, buscar identificador de cadena de consulta (JavaScript)

En el ejemplo siguiente se muestra una [función de JavaScript](functions-reference-node.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza una cadena de consulta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": false
}
```

Este es el código de JavaScript:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-javascript"></a>Desencadenador de HTTP, buscar identificador de datos de ruta (JavaScript)

En el ejemplo siguiente se muestra una [función de JavaScript](functions-reference-node.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza una cadena de consulta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

Este es el código de JavaScript:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>Desencadenador de colas, obtener varios documentos, mediante SqlQuery (JavaScript)

En el ejemplo siguiente se muestra un enlace de entrada de Azure Cosmos DB en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función recupera varios documentos especificados por una consulta SQL con un desencadenador de cola para personalizar los parámetros de la consulta.

El desencadenador de cola proporciona un parámetro `departmentId`. Un mensaje de la cola de `{ "departmentId" : "Finance" }` devolvería todos los registros para el departamento de finanzas.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
    module.exports = function (context, input) {    
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }       
        context.done();
    };
```

[Omisión de los ejemplos de entrada](#input---attributes)

### <a name="input---python-examples"></a>Entrada: ejemplos de Python

Esta sección contiene los siguientes ejemplos que leen un solo documento mediante la especificación de un valor de identificación desde varios orígenes:

* [Desencadenador de cola, buscar identificador de JSON](#queue-trigger-look-up-id-from-json-python)
* [Desencadenador de HTTP, buscar identificador de cadena de consulta](#http-trigger-look-up-id-from-query-string-python)
* [Desencadenador de HTTP, buscar identificador de datos de ruta](#http-trigger-look-up-id-from-route-data-python)
* [Desencadenador de colas, obtener varios documentos, mediante SqlQuery](#queue-trigger-get-multiple-docs-using-sqlquery-python)

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-python"></a>Desencadenador de cola, buscar identificador de JSON (Python)

En el ejemplo siguiente se muestra un enlace de entrada de Cosmos DB en un archivo *function.json* y una [función de Python](functions-reference-python.md) que usa el enlace. La función lee un solo documento y actualiza el valor de texto del documento.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "$return",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de Python:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList) -> func.Document:
    if documents:
        document = documents[0]
        document['text'] = 'This was updated!'
        return document
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-python"></a>Desencadenador de HTTP, buscar identificador de cadena de consulta (Python)

En el ejemplo siguiente se muestra una [función de Python](functions-reference-python.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza una cadena de consulta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true,
  "scriptFile": "__init__.py"
}
```

Este es el código de Python:

```python
import logging
import azure.functions as func

def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])

    return 'OK'
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-python"></a>Desencadenador de HTTP, buscar identificador de datos de ruta (Python)

En el ejemplo siguiente se muestra una [función de Python](functions-reference-python.md) que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza una cadena de consulta para especificar el identificador que se va a buscar. Dicho identificador se usa para recuperar un documento de `ToDoItem` de la base de datos y colección especificadas.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Este es el código de Python:

```python
import logging
import azure.functions as func

def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])
    return 'OK'
```

[Omisión de los ejemplos de entrada](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-python"></a>Desencadenador de cola, obtener varios documentos, uso de SqlQuery (Python)

En el ejemplo siguiente se muestra un enlace de entrada de Azure Cosmos DB en un archivo *function.json* y una [función de Python](functions-reference-python.md) que usa dicho enlace. La función recupera varios documentos especificados por una consulta SQL con un desencadenador de cola para personalizar los parámetros de la consulta.

El desencadenador de cola proporciona un parámetro `departmentId`. Un mensaje de la cola de `{ "departmentId" : "Finance" }` devolvería todos los registros para el departamento de finanzas.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de Python:

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```


[Omisión de los ejemplos de entrada](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Entrada: ejemplos de F#

En el ejemplo siguiente se muestra un enlace de entrada de Cosmos DB en un archivo *function.json* y una [función de C#](functions-reference-fsharp.md) que usa el enlace. La función lee un solo documento y actualiza el valor de texto del documento.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de F#:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

Este ejemplo necesita un archivo `project.json` que especifique las dependencias `FSharp.Interop.Dynamic` y `Dynamitey` de NuGet:

```json
{
    "frameworks": {
        "net46": {
            "dependencies": {
                "Dynamitey": "1.0.2",
                "FSharp.Interop.Dynamic": "3.0.0"
            }
        }
    }
}
```

Para agregar un archivo `project.json`, consulte la [administración de paquetes de F #](functions-reference-fsharp.md#package).

### <a name="input---java-examples"></a>Entrada: ejemplos de Java

En esta sección se incluyen los ejemplos siguientes:

* [Desencadenador de HTTP, buscar identificador de cadena de consulta - Parámetro String](#http-trigger-look-up-id-from-query-string---string-parameter-java)
* [Desencadenador de HTTP, buscar identificador de cadena de consulta - Parámetro POJO](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)
* [Desencadenador de HTTP, buscar identificador de datos de ruta](#http-trigger-look-up-id-from-route-data-java)
* [Desencadenador de HTTP, buscar identificador de datos de ruta, mediante SqlQuery](#http-trigger-look-up-id-from-route-data-using-sqlquery-java)
* [Desencadenador de HTTP, obtener varios documentos de datos de ruta, mediante SqlQuery](#http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java)

Los ejemplos hacen referencia a un tipo de `ToDoItem` simple:

```java
public class ToDoItem {

  private String id;
  private String description;  

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }
  
  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}
```

#### <a name="http-trigger-look-up-id-from-query-string---string-parameter-java"></a>Desencadenador de HTTP, buscar identificador de cadena de consulta - Parámetro String (Java)

En el ejemplo siguiente se muestra una función de Java que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza una cadena de consulta para especificar el identificador que se va a consultar. Dicho identificador se usa para recuperar un documento de la base de datos y la colección especificadas, en forma de cadena.

```java
public class DocByIdFromQueryString {

    @FunctionName("DocByIdFromQueryString")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            Optional<String> item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string 
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@CosmosDBInput` en los parámetros de la función cuyo valor provendría de Cosmos DB.  Esta anotación se puede usar con tipos nativos de Java, POJO o valores que aceptan valores NULL mediante Optional<T>.

#### <a name="http-trigger-look-up-id-from-query-string---pojo-parameter-java"></a>Desencadenador de HTTP, buscar identificador de cadena de consulta - Parámetro POJO (Java)

En el ejemplo siguiente se muestra una función de Java que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza una cadena de consulta para especificar el identificador que se va a consultar. Dicho identificador se usa para recuperar un documento de la base de datos y colección especificadas. A continuación, el documento se convierte en una instancia de la anotación ```ToDoItem``` POJO creada previamente y se pasa como argumento a la función.

```java
public class DocByIdFromQueryStringPojo {

    @FunctionName("DocByIdFromQueryStringPojo")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Item from the database is " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-look-up-id-from-route-data-java"></a>Desencadenador de HTTP, buscar identificador de datos de ruta (Java)

En el ejemplo siguiente se muestra una función de Java que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza un parámetro de ruta para especificar el identificador que se va a consultar. Dicho identificador se usa para recuperar un documento de la base de datos y colección especificadas, y lo devuelve como ```Optional<String>```.

```java
public class DocByIdFromRoute {

    @FunctionName("DocByIdFromRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems/{id}")
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{id}",
              partitionKey = "{id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            Optional<String> item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string 
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-java"></a>Desencadenador de HTTP, buscar identificador de datos de ruta, mediante SqlQuery (Java)

En el ejemplo siguiente se muestra una función de Java que recupera un documento individual. La función la desencadena una solicitud HTTP que utiliza un parámetro de ruta para especificar el identificador que se va a consultar. Ese identificador se usa para recuperar un documento de la base de datos y la colección especificadas, convirtiendo el conjunto de resultados en una anotación ```ToDoItem[]```, ya que pueden devolverse muchos documentos, en función de los criterios de consulta.

```java
public class DocByIdFromRouteSqlQuery {

    @FunctionName("DocByIdFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems2/{id}") 
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where r.id = {id}",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem[] item,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Items from the database are " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

#### <a name="http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java"></a>Desencadenador de HTTP, obtener varios documentos de datos de ruta, mediante SqlQuery (Java)

En el ejemplo siguiente se muestra una función de Java que recupera varios documentos. La función la desencadena una solicitud HTTP que utiliza un parámetro de ruta ```desc``` para especificar la cadena que se va a buscar en el campo ```description```. El término de búsqueda se usa para recuperar una colección de documentos de la base de datos y de la colección especificadas, lo que convierte el conjunto de resultados en una anotación ```ToDoItem[]``` y la pasa como un argumento a la función.

```java
public class DocsFromRouteSqlQuery {

    @FunctionName("DocsFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems3/{desc}")
            HttpRequestMessage<Optional<String>> request,        
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where contains(r.description, {desc})",
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            ToDoItem[] items,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Number of items from the database is " + (items == null ? 0 : items.length));

        // Convert and display
        if (items == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("No documents found.")
                          .build();
        } 
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(items)
                          .build();
        }
    }
}
 ```

## <a name="input---attributes"></a>Entrada: atributos

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs).

El constructor del atributo toma el nombre de la base de datos y el nombre de la colección. Para información sobre esos valores y otras propiedades que puede configurar, consulte [la sección de configuración siguiente](#input---configuration).

## <a name="input---configuration"></a>Entrada: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que establece en el archivo *function.json* y el atributo `CosmosDB`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type**     || Se debe establecer en `cosmosDB`.        |
|**dirección**     || Se debe establecer en `in`.         |
|**name**     || Nombre del parámetro de enlace que representa al documento en la función.  |
|**databaseName** |**DatabaseName** |Base de datos que contiene el documento.        |
|**collectionName** |**CollectionName** | Nombre de la colección que contiene el documento. |
|**id**    | **Id** | Identificador del documento que se va a recuperar. Esta propiedad es compatible con [expresiones de enlace](./functions-bindings-expressions-patterns.md). No establezca las propiedades **id** y **sqlQuery** a la vez. Si no establece alguna de ellas, se recupera toda la colección. |
|**sqlQuery**  |**SqlQuery**  | Consulta SQL de Azure Cosmos DB que se usa para recuperar varios documentos. La propiedad es compatible con los enlaces en tiempo de ejecución, como en este ejemplo: `SELECT * FROM c where c.departmentId = {departmentId}`. No establezca las propiedades **id** y **sqlQuery** a la vez. Si no establece alguna de ellas, se recupera toda la colección.|
|**connectionStringSetting**     |**ConnectionStringSetting**|Nombre de la configuración de aplicación que contiene la cadena de conexión de Azure Cosmos DB.        |
|**partitionKey**|**PartitionKey**|Especifica el valor de la clave de partición para la búsqueda. Puede incluir parámetros de enlace.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Uso de entradas

En las funciones de C# y F#, cuando se sale de la función correctamente, los cambios realizados en el documento de entrada mediante parámetros de entrada con nombre se guardan automáticamente.

En las funciones de JavaScript, las actualizaciones no se realizan automáticamente al cerrar la función. Por el contrario, use `context.bindings.<documentName>In` y `context.bindings.<documentName>Out` para realizar las actualizaciones. Consulte el ejemplo de JavaScript.

## <a name="output"></a>Salida

El enlace de salida de Azure Cosmos DB permite escribir un nuevo documento en una base de datos de Azure Cosmos DB mediante SQL API.

## <a name="output---examples"></a>Salida: ejemplos

Vea el ejemplo específico del lenguaje:

* [C#](#output---c-examples)
* [Script de C# (.csx)](#output---c-script-examples)
* [F#](#output---f-examples)
* [Java](#output---java-examples)
* [JavaScript](#output---javascript-examples)

Vea también el [ejemplo de entrada](#input---c-examples) que utiliza `DocumentClient`.

[Omisión de ejemplos de salida](#output---attributes)

### <a name="output---c-examples"></a>Salida: ejemplos de C#

En esta sección se incluyen los ejemplos siguientes:

* Desencadenador de cola, escribir un documento
* Desencadenador de cola, escribir documentos mediante IAsyncCollector

Los ejemplos hacen referencia a un tipo de `ToDoItem` simple:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Omisión de ejemplos de salida](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Desencadenador de cola, escribir un documento (C#)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que agrega un documento a una base de datos mediante los datos que se proporcionan en el mensaje desde Queue Storage.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            ILogger log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.LogInformation($"C# Queue trigger function inserted one row");
            log.LogInformation($"Description={queueMessage}");
        }
    }
}
```

[Omisión de ejemplos de salida](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Desencadenador de cola, escribir documentos mediante IAsyncCollector (C#)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que agrega una colección de documentos a una base de datos mediante los datos que se proporcionan en un mensaje de cola JSON.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.LogInformation($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Omisión de ejemplos de salida](#output---attributes)

### <a name="output---c-script-examples"></a>Salida: ejemplos de script de C#

En esta sección se incluyen los ejemplos siguientes:

* Desencadenador de cola, escribir un documento
* Desencadenador de cola, escribir documentos mediante IAsyncCollector

[Omisión de ejemplos de salida](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c-script"></a>Desencadenador de cola, escribir un documento (script de C#)

En el ejemplo siguiente se muestra un enlace de salida de Azure Cosmos DB en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función usa un enlace de entrada de cola para una cola que recibe JSON en el formato siguiente:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

La función crea documentos de Azure Cosmos DB en el formato siguiente para cada registro:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de script de C#:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;
    using Microsoft.Extensions.Logging;

    public static void Run(string myQueueItem, out object employeeDocument, ILogger log)
    {
      log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");

      dynamic employee = JObject.Parse(myQueueItem);

      employeeDocument = new {
        id = employee.name + "-" + employee.employeeId,
        name = employee.name,
        employeeId = employee.employeeId,
        address = employee.address
      };
    }
```

#### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Desencadenador de cola, escribir documentos mediante IAsyncCollector

Para crear varios documentos, puede enlazar a `ICollector<T>` o `IAsyncCollector<T>`, donde `T` es uno de los tipos admitidos.

En este ejemplo se hace referencia a un tipo simple `ToDoItem`:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

Este es el archivo function.json:

```json
{
  "bindings": [
    {
      "name": "toDoItemsIn",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "todoqueueforwritemulti",
      "connectionStringSetting": "AzureWebJobsStorage"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Este es el código de script de C#:

```cs
using System;
using Microsoft.Extensions.Logging;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.LogInformation($"Description={toDoItem.Description}");
        await toDoItemsOut.AddAsync(toDoItem);
    }
}
```

[Omisión de ejemplos de salida](#output---attributes)

### <a name="output---javascript-examples"></a>Salida: ejemplos de JavaScript

En el ejemplo siguiente se muestra un enlace de salida de Azure Cosmos DB en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función usa un enlace de entrada de cola para una cola que recibe JSON en el formato siguiente:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

La función crea documentos de Azure Cosmos DB en el formato siguiente para cada registro:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

[Omisión de ejemplos de salida](#output---attributes)

### <a name="output---f-examples"></a>Salida: ejemplos de F#

En el ejemplo siguiente se muestra un enlace de salida de Azure Cosmos DB en un archivo *function.json* y una [función de F#](functions-reference-fsharp.md) que usa el enlace. La función usa un enlace de entrada de cola para una cola que recibe JSON en el formato siguiente:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

La función crea documentos de Azure Cosmos DB en el formato siguiente para cada registro:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```
En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de F#:

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json
    open Microsoft.Extensions.Logging

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: ILogger) =
      log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
      let employee = JObject.Parse(myQueueItem)
      employeeDocument <-
        { id = sprintf "%s-%s" employee?name employee?employeeId
          name = employee?name
          employeeId = employee?employeeId
          address = employee?address }
```

Este ejemplo necesita un archivo `project.json` que especifique las dependencias `FSharp.Interop.Dynamic` y `Dynamitey` de NuGet:

```json
{
    "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
           }
        }
    }
}
```

Para agregar un archivo `project.json`, consulte la [administración de paquetes de F #](functions-reference-fsharp.md#package).

### <a name="output---java-examples"></a>Salida: ejemplos de Java

* [Desencadenador de cola, guardar el mensaje en la base de datos a través de un valor devuelto](#queue-trigger-save-message-to-database-via-return-value-java)
* [Desencadenador de HTTP, guardar un documento en la base de datos a través de un valor devuelto](#http-trigger-save-one-document-to-database-via-return-value-java)
* [Desencadenador de HTTP, guardar un documento en la base de datos a través de OutputBinding](#http-trigger-save-one-document-to-database-via-outputbinding-java)
* [Desencadenador de HTTP, guardar varios documentos en la base de datos a través de OutputBinding](#http-trigger-save-multiple-documents-to-database-via-outputbinding-java)


#### <a name="queue-trigger-save-message-to-database-via-return-value-java"></a>Desencadenador de cola, guardar el mensaje en la base de datos a través de un valor devuelto (Java)

En el ejemplo siguiente se muestra una función de Java que agrega un documento a una base de datos con datos de un mensaje de Queue Storage.

```java
@FunctionName("getItem")
@CosmosDBOutput(name = "database", 
  databaseName = "ToDoList", 
  collectionName = "Items", 
  connectionStringSetting = "AzureCosmosDBConnection")
public String cosmosDbQueryById(
    @QueueTrigger(name = "msg", 
      queueName = "myqueue-items", 
      connection = "AzureWebJobsStorage") 
    String message,
    final ExecutionContext context)  {
     return "{ id: \"" + System.currentTimeMillis() + "\", Description: " + message + " }";
   }
```

#### <a name="http-trigger-save-one-document-to-database-via-return-value-java"></a>Desencadenador de HTTP, guardar el mensaje en la base de datos a través de un valor devuelto (Java)

En el ejemplo siguiente se muestra una función de Java, cuya firma está anotada con ```@CosmosDBOutput``` y tiene un valor devuelto de tipo ```String```. El documento JSON devuelto por la función se escribirá automáticamente en la colección de COSMOS DB correspondiente.

```java
    @FunctionName("WriteOneDoc")
    @CosmosDBOutput(name = "database", 
      databaseName = "ToDoList",
      collectionName = "Items", 
      connectionStringSetting = "Cosmos_DB_Connection_String")
    public String run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());

        // Parse query parameter        
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        return jsonDocument;
    }
```

#### <a name="http-trigger-save-one-document-to-database-via-outputbinding-java"></a>Desencadenador de HTTP guardar un documento en la base de datos a través de OutputBinding (Java)

En el ejemplo siguiente se muestra una función de Java que escribe un documento en Cosmos DB a través de un parámetro de salida ```OutputBinding<T>```. Tenga en cuenta que, en esta configuración, debe anotarse el parámetro ```outputItem``` con ```@CosmosDBOutput```, y no la signatura de función. El uso de ```OutputBinding<T>``` permite que la función aproveche el enlace para escribir el documento en Cosmos DB, mientras que también permite devolver un valor diferente para el autor de llamada de la función, como un documento JSON o XML.

```java
    @FunctionName("WriteOneDocOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database", 
              databaseName = "ToDoList", 
              collectionName = "Items", 
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            OutputBinding<String> outputItem,
            final ExecutionContext context) {
  
        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
      
        // Generate random ID
        final int id = Math.abs(new Random().nextInt());

        // Generate document
        final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                    "\"description\": \"" + name + "\"}";

        context.getLogger().info("Document to be saved: " + jsonDocument);

        // Set outputItem's value to the JSON document to be saved
        outputItem.setValue(jsonDocument);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Document created successfully.")
                      .build();
    }
```

#### <a name="http-trigger-save-multiple-documents-to-database-via-outputbinding-java"></a>Desencadenador de HTTP, guardar varios documentos en la base de datos a través de OutputBinding (Java)

En el ejemplo siguiente se muestra una función de Java que escribe varios documentos en Cosmos DB a través de un parámetro de salida ```OutputBinding<T>```. Tenga en cuenta que, en esta configuración, debe anotarse el parámetro ```outputItem``` con ```@CosmosDBOutput```, y no la signatura de función. El parámetro de salida, ```outputItem``` tiene una lista de objetos ```ToDoItem``` como su tipo de parámetro de plantilla. El uso de ```OutputBinding<T>``` permite que la función aproveche el enlace para escribir los documentos en Cosmos DB, mientras que también permite devolver un valor diferente para el autor de llamada de la función, como un documento JSON o XML.

```java
    @FunctionName("WriteMultipleDocsOutputBinding")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET, HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS) 
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBOutput(name = "database", 
              databaseName = "ToDoList", 
              collectionName = "Items", 
              connectionStringSetting = "Cosmos_DB_Connection_String") 
            OutputBinding<List<ToDoItem>> outputItem,
            final ExecutionContext context) {
  
        // Parse query parameter
        String query = request.getQueryParameters().get("desc");
        String name = request.getBody().orElse(query);

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
      
        // Generate documents
        List<ToDoItem> items = new ArrayList<>();

        for (int i = 0; i < 5; i ++) {
          // Generate random ID
          final int id = Math.abs(new Random().nextInt());

          // Create ToDoItem
          ToDoItem item = new ToDoItem(String.valueOf(id), name);
          
          items.add(item);
        }

        // Set outputItem's value to the list of POJOs to be saved
        outputItem.setValue(items);
        context.getLogger().info("Document to be saved: " + items);

        // return a different document to the browser or calling client.
        return request.createResponseBuilder(HttpStatus.OK)
                      .body("Documents created successfully.")
                      .build();
    }
```

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@CosmosDBOutput` en los parámetros que se van a escribir en Cosmos DB.  El tipo de parámetro de anotación debe ser ```OutputBinding<T>```, donde T es un tipo nativo de Java o un POJO.


## <a name="output---attributes"></a>Salida: atributos

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs).

El constructor del atributo toma el nombre de la base de datos y el nombre de la colección. Para información sobre esos valores y otras propiedades que puede configurar, consulte [Salida: configuración](#output---configuration). A continuación, se muestra un ejemplo del atributo `CosmosDB` en una signatura de método:

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Para obtener un ejemplo completo, consulte Salida: ejemplo de C#.

## <a name="output---configuration"></a>Salida: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `CosmosDB`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type**     || Se debe establecer en `cosmosDB`.        |
|**dirección**     || Se debe establecer en `out`.         |
|**name**     || Nombre del parámetro de enlace que representa al documento en la función.  |
|**databaseName** | **DatabaseName**|Base de datos que contiene la colección en la que se ha creado el documento.     |
|**collectionName** |**CollectionName**  | Nombre de la colección en la que se ha creado el documento. |
|**createIfNotExists**  |**CreateIfNotExists**    | Valor booleano que indica si la colección se ha creado si no existía. El valor predeterminado es *false* porque las colecciones nuevas se crean con rendimiento reservado, lo que afecta el costo. Consulte la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/)para obtener más información.  |
|**partitionKey**|**PartitionKey** |Cuando el valor de `CreateIfNotExists` es true, define la ruta de la clave de partición para la colección que se creó.|
|**collectionThroughput**|**CollectionThroughput**| Cuando el valor de `CreateIfNotExists` es true, define el [rendimiento](../cosmos-db/set-throughput.md) de la colección que se creó.|
|**connectionStringSetting**    |**ConnectionStringSetting** |Nombre de la configuración de aplicación que contiene la cadena de conexión de Azure Cosmos DB.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uso de salidas

De forma predeterminada, cuando se escribe en el parámetro de salida de la función, se crea un documento en la base de datos. Este documento tiene un GUID generado automáticamente como identificador de documento. Puede especificar el identificador de documento del documento de salida si especifica la propiedad `id` en el objeto JSON pasado al parámetro de salida.

> [!Note]  
> Cuando especifica el identificador de un documento existente, se sobrescribe con el nuevo documento de salida.

## <a name="exceptions-and-return-codes"></a>Excepciones y códigos de retorno

| Enlace | Referencia |
|---|---|
| CosmosDB | [Códigos de error de CosmosDB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>configuración de host.json

En esta sección se describen las opciones de configuración globales disponibles para este enlace en la versión 2.x. Para obtener más información acerca de las opciones de configuración globales de la versión 2.x, consulte la [referencia de host.json para Azure Functions, versión 2.x](functions-host-json.md).

```json
{
    "version": "2.0",
    "extensions": {
        "cosmosDB": {
            "connectionMode": "Gateway",
            "protocol": "Https",
            "leaseOptions": {
                "leasePrefix": "prefix1"
            }
        }
    }
}
```  

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|GatewayMode|Puerta de enlace|Modo de conexión que usa la función al conectarse al servicio de Azure Cosmos DB. Las opciones son `Direct` y `Gateway`|
|Protocol|Https|Protocolo de conexión que usa la función al conectarse al servicio de Azure Cosmos DB.  Lea [aquí para obtener una explicación de los dos modos](../cosmos-db/performance-tips.md#networking)| 
|leasePrefix|N/D|Prefijo de concesión que se usará en todas las funciones de una aplicación.| 

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre la informática de base de datos sin servidor con Cosmos DB](../cosmos-db/serverless-computing-database.md)
* [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
