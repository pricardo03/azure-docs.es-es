---
title: Enlaces de Azure Table Storage para Azure Functions
description: Descubra cómo usar los enlaces de Azure Table Storage en Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: azure functions, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.openlocfilehash: 3fc31306af1c85a67a1afca8a34be82a711f2527
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52999532"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Enlaces de Azure Table Storage para Azure Functions

En este artículo se explica cómo trabajar con enlaces de Azure Table Storage en Azure Functions. Azure Functions admite enlaces de entrada y salida para Table Storage de Azure.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paquetes: Functions 1.x

Los enlaces de Table Storage se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs), versión 2.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) de GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

Los enlaces de almacenamiento de Table se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage), versión 3.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Entrada

Use el enlace de entrada de Azure Table Storage para leer una tabla de una cuenta de Azure Storage.

## <a name="input---example"></a>Ejemplo de entrada

Vea el ejemplo específico del lenguaje:

* [C#: leer una entidad](#input---c-example---one-entity)
* [Enlace de C# a IQueryable](#input---c-example---iqueryable)
* [Enlace de C# a CloudTable](#input---c-example---cloudtable)
* [Script de C#: leer una entidad](#input---c-script-example---one-entity)
* [Enlace de script de C# a IQueryable](#input---c-script-example---iqueryable)
* [Enlace de script de C# a CloudTable](#input---c-script-example---cloudtable)
* [F#](#input---f-example)
* [JavaScript](#input---javascript-example)
* [Java](#input---java-example)

### <a name="input---c-example---one-entity"></a>Entrada - ejemplo de C# - una entidad

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que lee una única fila de la tabla. 

El valor "{queueTrigger}" de clave de fila indica que la clave de fila procede de la cadena del mensaje en la cola.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        ILogger log)
    {
        log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### <a name="input---c-example---iqueryable"></a>Entrada - ejemplo de C# - IQueryable

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que lee varias filas de la tabla. Tenga en cuenta que la clase `MyPoco` deriva de `TableEntity`.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        ILogger log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

### <a name="input---c-example---cloudtable"></a>Entrada - ejemplo de C# - CloudTable

`IQueryable` no es compatible con el [entorno de tiempo de ejecución de Functions v2](functions-versions.md). Una alternativa consiste en usar un parámetro del método `CloudTable` para leer la tabla mediante Azure Storage SDK. Este es un ejemplo de una función 2.x que consulta una tabla de registros de Azure Functions:

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;

namespace FunctionAppCloudTable2
{
    public class LogEntity : TableEntity
    {
        public string OriginalName { get; set; }
    }
    public static class CloudTableDemo
    {
        [FunctionName("CloudTableDemo")]
        public static async Task Run(
            [TimerTrigger("0 */1 * * * *")] TimerInfo myTimer, 
            [Table("AzureWebJobsHostLogscommon")] CloudTable cloudTable,
            ILogger log)
        {
            log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

            TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
                TableQuery.CombineFilters(
                    TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
                        "FD2"),
                    TableOperators.And,
                    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
                        "t")));

            // Execute the query and loop through the results
            foreach (LogEntity entity in 
                await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
            {
                log.LogInformation(
                    $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
            }
        }
    }
}
```

Para más información sobre cómo usar CloudTable, Consulte [Introducción a Azure Table Storage](../cosmos-db/table-storage-how-to-use-dotnet.md).

Si intenta enlazar a `CloudTable` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](#azure-storage-sdk-version-in-functions-1x).

### <a name="input---c-script-example---one-entity"></a>Entrada - ejemplo de script de C# - una entidad

En el ejemplo siguiente se muestra un enlace de entrada de la tabla en un archivo *function.json* y código de [script de C#](functions-reference-csharp.md) que usa el enlace. La función usa un desencadenador de cola para leer una fila de tabla única. 

El archivo *function.json* especifica un valor `partitionKey` y un valor `rowKey`. El valor `rowKey` "{queueTrigger}" de clave de fila indica que la clave de fila procede de la cadena del mensaje en la cola.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de script de C#:

```csharp
public static void Run(string myQueueItem, Person personEntity, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    log.LogInformation($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---iqueryable"></a>Entrada - ejemplo de script de C# - IQueryable

En el ejemplo siguiente se muestra un enlace de entrada de la tabla en un archivo *function.json* y código de [script de C#](functions-reference-csharp.md) que usa el enlace. La función lee las entidades de una clave de partición que se especifica en un mensaje en la cola.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

El código de script de C# agrega una referencia al SDK de Azure Storage con el fin de que el tipo de entidad pueda derivar de `TableEntity`:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Extensions.Logging;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.LogInformation($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---cloudtable"></a>Entrada - ejemplo de script de C# - CloudTable

`IQueryable` no es compatible con el [entorno de tiempo de ejecución de Functions v2](functions-versions.md). Una alternativa consiste en usar un parámetro del método `CloudTable` para leer la tabla mediante Azure Storage SDK. Este es un ejemplo de una función 2.x que consulta una tabla de registros de Azure Functions:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 */1 * * * *"
    },
    {
      "name": "cloudTable",
      "type": "table",
      "connection": "AzureWebJobsStorage",
      "tableName": "AzureWebJobsHostLogscommon",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, CloudTable cloudTable, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

    TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
            "FD2"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
            "a")));

    // Execute the query and loop through the results
    foreach (LogEntity entity in 
    await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
    {
        log.LogInformation(
            $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
    }
}

public class LogEntity : TableEntity
{
    public string OriginalName { get; set; }
}
```

Para más información sobre cómo usar CloudTable, Consulte [Introducción a Azure Table Storage](../cosmos-db/table-storage-how-to-use-dotnet.md).

Si intenta enlazar a `CloudTable` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](#azure-storage-sdk-version-in-functions-1x).

### <a name="input---f-example"></a>Entrada: ejemplo de F#

En el ejemplo siguiente se muestra un enlace de entrada de la tabla en un archivo *function.json* y código de [script de F#](functions-reference-fsharp.md) que usa el enlace. La función usa un desencadenador de cola para leer una fila de tabla única. 

El archivo *function.json* especifica un valor `partitionKey` y un valor `rowKey`. El valor `rowKey` "{queueTrigger}" de clave de fila indica que la clave de fila procede de la cadena del mensaje en la cola.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de F#:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.LogInformation(sprintf "Name in Person entity: %s" personEntity.Name)
```

### <a name="input---javascript-example"></a>Entrada: ejemplo de JavaScript

En el ejemplo siguiente se muestra un enlace de entrada de la tabla en un archivo *function.json* y el código de [JavaScript](functions-reference-node.md) que usa el enlace. La función usa un desencadenador de cola para leer una fila de tabla única. 

El archivo *function.json* especifica un valor `partitionKey` y un valor `rowKey`. El valor `rowKey` "{queueTrigger}" de clave de fila indica que la clave de fila procede de la cadena del mensaje en la cola.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#input---configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

### <a name="input---java-example"></a>Entrada: ejemplo de Java

En el ejemplo siguiente se muestra una función desencadenada por HTTP que devuelve el recuento total de elementos de una partición especificada en el almacenamiento de la tabla.

```java
@FunctionName("getallcount")
public int run(
   @HttpTrigger(name = "req",
                 methods = {"get"},
                 authLevel = AuthorizationLevel.ANONYMOUS) Object dummyShouldNotBeUsed,
   @TableInput(name = "items",
                tableName = "mytablename",  partitionKey = "myparkey",
                connection = "myconnvarname") MyItem[] items
) {
    return items.length;
}
```


## <a name="input---attributes"></a>Entrada: atributos
 
Para [bibliotecas de clases de C#](functions-dotnet-class-library.md), utilice los siguientes atributos para configurar un enlace de entrada de la tabla:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  El constructor del atributo toma el nombre de la tabla, la clave de partición y la clave de fila. Se puede usar en un parámetro de salida o en el valor devuelto de la función, como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Para obtener un ejemplo completo, consulte [Entrada: ejemplo de C#](#input---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Proporciona otra manera de especificar la cuenta de almacenamiento que se debe usar. El constructor toma el nombre de una configuración de aplicación que contiene una cadena de conexión de almacenamiento. El atributo se puede aplicar en el nivel de clase, método o parámetro. En el ejemplo siguiente se muestran el nivel de clase y de método:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

La cuenta de almacenamiento que se debe usar se determina en el orden siguiente:

* La propiedad `Connection` del atributo `Table`.
* El atributo `StorageAccount` aplicado al mismo parámetro que el atributo `Table`.
* El atributo `StorageAccount` aplicado a la función.
* El atributo `StorageAccount` aplicado a la clase.
* La cuenta de almacenamiento predeterminada de la aplicación de función (configuración de aplicación "AzureWebJobsStorage").

## <a name="input---java-annotations"></a>Entrada: anotaciones de Java

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@TableInput` en los parámetros cuyo valor provendría del almacenamiento de la tabla.  Esta anotación se puede usar con tipos nativos de Java, POJO o valores que aceptan valores NULL mediante Optional<T>. 

## <a name="input---configuration"></a>Entrada: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que establece en el archivo *function.json* y el atributo `Table`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `table`. Esta propiedad se establece automáticamente cuando se crea el enlace en Azure Portal.|
|**dirección** | N/D | Se debe establecer en `in`. Esta propiedad se establece automáticamente cuando se crea el enlace en Azure Portal. |
|**name** | N/D | Nombre de la variable que representa la tabla o la entidad en el código de la función. | 
|**tableName** | **TableName** | El nombre de la tabla.| 
|**partitionKey** | **PartitionKey** |Opcional. Clave de partición de la entidad de tabla que se va a leer. Consulte la sección acerca del [uso](#input---usage) para obtener información acerca de cómo usar esta propiedad.| 
|**rowKey** |**RowKey** | Opcional. Clave de fila de la entidad de tabla que se va a leer. Consulte la sección acerca del [uso](#input---usage) para obtener información acerca de cómo usar esta propiedad.| 
|**take** |**Take** | Opcional. Número máximo de entidades que se van a leer en JavaScript. Consulte la sección acerca del [uso](#input---usage) para obtener información acerca de cómo usar esta propiedad.| 
|**filter** |**Filter** | Opcional. Expresión de filtro de OData en la entrada de la tabla en JavaScript. Consulte la sección acerca del [uso](#input---usage) para obtener información acerca de cómo usar esta propiedad.| 
|**conexión** |**Connection** | El nombre de una configuración de aplicación que contiene la cadena de conexión de almacenamiento que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre aquí. Por ejemplo, si establece `connection` en "MyStorage", el entorno en tiempo de ejecución de Functions busca una configuración de aplicación denominada "AzureWebJobsMyStorage". Si deja `connection` vacía, el entorno en tiempo de ejecución de Functions usa la cadena de conexión de almacenamiento predeterminada en la configuración de aplicación que se denomina `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Uso de entradas

El enlace de entrada de Table Storage admite los siguientes escenarios:

* **Leer una fila en C# o en script de C#**

  Establezca `partitionKey` y `rowKey`. Obtenga acceso a los datos de la tabla mediante un parámetro de método `T <paramName>`. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json*. `T` suele ser un tipo que implementa `ITableEntity` o deriva de `TableEntity`. Las propiedades `filter` y `take` no se utilizan en este escenario. 

* **Leer una o varias filas en C# o en script de C#**

  Obtenga acceso a los datos de la tabla mediante un parámetro de método `IQueryable<T> <paramName>`. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json*. `T` debe ser un tipo que implementa `ITableEntity` o deriva de `TableEntity`. Puede usar métodos `IQueryable` para realizar cualquier operación de filtrado necesaria. Las propiedades `partitionKey`, `rowKey`, `filter` y `take` no se utilizan en este escenario.  

  > [!NOTE]
  > `IQueryable` no es compatible con el [entorno de tiempo de ejecución de Functions v2](functions-versions.md). Una alternativa consiste en [usar un parámetro del método CloudTable](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) para leer la tabla mediante Azure Storage SDK. Si intenta enlazar a `CloudTable` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](#azure-storage-sdk-version-in-functions-1x).

* **Leer una o varias filas en JavaScript**

  Establezca las propiedades `filter` y `take`. No establezca `partitionKey` o `rowKey`. Obtenga acceso a la entidad (o entidades) de la tabla de entradas mediante `context.bindings.<name>`. Los objetos deserializados tienen las propiedades `RowKey` y `PartitionKey`.

## <a name="output"></a>Salida

Use un enlace de salida de Azure Table Storage para escribir entidades en una tabla de una cuenta de Azure Storage.

> [!NOTE]
> Este enlace de salida no permite actualizar las entidades existentes. Use la operación `TableOperation.Replace`[del SDK de Azure Storage](https://docs.microsoft.com/azure/cosmos-db/table-storage-how-to-use-dotnet#replace-an-entity) para actualizar una entidad existente.   

## <a name="output---example"></a>Salida: ejemplo

Vea el ejemplo específico del lenguaje:

* [C#](#output---c-example)
* [Script de C# (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Salida: ejemplo de C#

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que usa un desencadenador HTTP para escribir una única fila de la tabla. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, ILogger log)
    {
        log.LogInformation($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### <a name="output---c-script-example"></a>Salida: ejemplo de script de C#

En el ejemplo siguiente se muestra un enlace de salida de la tabla en un archivo *function.json* y código de [script de C#](functions-reference-csharp.md) que usa el enlace. La función escribe varias entidades de tabla.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de script de C#:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, ILogger log)
{
    for (int i = 1; i < 10; i++)
        {
            log.LogInformation($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

### <a name="output---f-example"></a>Salida: ejemplo de F#

En el ejemplo siguiente se muestra un enlace de salida de la tabla en un archivo *function.json* y código de [script de F#](functions-reference-fsharp.md) que usa el enlace. La función escribe varias entidades de tabla.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de F#:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: ILogger) =
    for i = 1 to 10 do
        log.LogInformation(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### <a name="output---javascript-example"></a>Salida: ejemplo de JavaScript

En el ejemplo siguiente se muestra un enlace de salida de la tabla en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función escribe varias entidades de tabla.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

## <a name="output---attributes"></a>Salida: atributos

En las [bibliotecas de clase C#](functions-dotnet-class-library.md), use [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs).

El constructor del atributo toma el nombre de la tabla. Se puede usar en un parámetro `out` o en el valor devuelto de la función, como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Para obtener un ejemplo completo, consulte [Salida: ejemplo de C#](#output---c-example).

Puede usar el atributo `StorageAccount` para especificar la cuenta de almacenamiento en el nivel de clase, método o parámetro. Para obtener más información, consulte [Entrada: atributos](#input---attributes).

## <a name="output---configuration"></a>Salida: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `Table`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `table`. Esta propiedad se establece automáticamente cuando se crea el enlace en Azure Portal.|
|**dirección** | N/D | Se debe establecer en `out`. Esta propiedad se establece automáticamente cuando se crea el enlace en Azure Portal. |
|**name** | N/D | Nombre de la variable que se usa en el código de la función que representa la tabla o entidad. Se establece en `$return` para hacer referencia al valor devuelto de la función.| 
|**tableName** |**TableName** | El nombre de la tabla.| 
|**partitionKey** |**PartitionKey** | Clave de partición de la entidad de tabla que se va a escribir. Consulte la [sección acerca del uso](#output---usage) para obtener información acerca de cómo usar esta propiedad.| 
|**rowKey** |**RowKey** | Clave de fila de la entidad de tabla que se va a escribir. Consulte la [sección acerca del uso](#output---usage) para obtener información acerca de cómo usar esta propiedad.| 
|**conexión** |**Connection** | El nombre de una configuración de aplicación que contiene la cadena de conexión de almacenamiento que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre aquí. Por ejemplo, si establece `connection` en "MyStorage", el entorno en tiempo de ejecución de Functions busca una configuración de aplicación denominada "AzureWebJobsMyStorage". Si deja `connection` vacía, el entorno en tiempo de ejecución de Functions usa la cadena de conexión de almacenamiento predeterminada en la configuración de aplicación que se denomina `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uso de salidas

El enlace de salida de Table Storage admite los siguientes escenarios:

* **Escribir una fila en cualquier lenguaje**

  En C# y script de C#, obtenga acceso a la entidad de tabla de salida con un parámetro de método como `out T paramName` o el valor devuelto de la función. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json*. `T` puede ser cualquier tipo serializable si las claves de partición y fila se proporcionan con el archivo *function.json* o el atributo `Table`. En caso contrario, `T` debe ser un tipo que incluya las propiedades `PartitionKey` y `RowKey`. En este escenario, `T` implementa normalmente `ITableEntity` o deriva de `TableEntity`, pero no es necesario.

* **Escribir una o varias filas en C# o en script de C#**

  En C# y script de C#, obtenga acceso a la entidad de tabla de salida con un parámetro `ICollector<T> paramName` o `IAsyncCollector<T> paramName`. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json*. `T` especifica el esquema de las entidades que quiere agregar. Normalmente, `T` deriva de `TableEntity` o implementa `ITableEntity`, pero no tiene por qué ser así. Los valores de las claves de partición y de fila de *function.json* o del constructor del atributo `Table` no se usan en este escenario.

  Una alternativa consiste en usar un parámetro del método `CloudTable` para escribir en la tabla mediante Azure Storage SDK. Si intenta enlazar a `CloudTable` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](#azure-storage-sdk-version-in-functions-1x). Para un ejemplo de código que se enlaza a `CloudTable`, consulte los ejemplos de enlace de entrada para [C#](#input---c-example---cloudtable) o [script de C#](#input---c-script-example---cloudtable) que aparecieron anteriormente en este artículo.

* **Escribir una o varias filas en JavaScript**

  En las funciones de JavaScript, obtenga acceso a la salida de tabla mediante `context.bindings.<name>`.

## <a name="exceptions-and-return-codes"></a>Excepciones y códigos de retorno

| Enlace | Referencia |
|---|---|
| Tabla | [Códigos de error de tabla](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| Blob, tabla, cola | [Códigos de error de almacenamiento](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, tabla, cola | [Solución de problemas](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)
