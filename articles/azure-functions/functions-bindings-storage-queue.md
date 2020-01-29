---
title: Enlaces de Azure Queue Storage para Azure Functions
description: Obtenga información acerca de cómo usar enlaces de salida y desencadenador de Azure Queue Storage en Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: ea213921c736bc3b6bf88c0bdd81a96656ecbe5b
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76547292"
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Enlaces de Azure Queue Storage para Azure Functions

En este artículo se explica cómo trabajar con enlaces de Azure Queue Storage en Azure Functions. Azure Functions admite enlaces de salida y desencadenador para colas.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paquetes: Functions 1.x

Los enlaces de Queue Storage se proporcionan en el paquete de NuGet [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs), versión 2.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Queue) de GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x-and-higher"></a>Paquetes: Functions 2.x y versiones posteriores

Los enlaces de Queue Storage se proporcionan en el paquete de NuGet [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage), versión 3.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="encoding"></a>Encoding
Las funciones esperan una cadena codificada en *base64*. Cualquier ajuste al tipo de codificación (para preparar los datos como una cadena codificada en *base64*) debe implementarse en el servicio de llamada.

## <a name="trigger"></a>Desencadenador

Use el desencadenador de cola para iniciar una función cuando se reciba un nuevo elemento en una cola. El mensaje de la cola se proporciona a modo de entrada para la función.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que sondea la cola `myqueue-items` y escribe un registro cada vez que se procesa un elemento de cola.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

En el ejemplo siguiente se muestra un enlace de desencadenador de cola de un archivo *function.json* y código de [script de C# (.csx)](functions-reference-csharp.md) que usa el enlace. La función sondea la cola `myqueue-items` y escribe un registro cada vez que se procesa un elemento de cola.

Este es el archivo *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

En la sección de [configuración](#trigger---configuration) se explican estas propiedades.

Este es el código de script de C#:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

En la sección acerca del [uso](#trigger---usage) se explica `myQueueItem`, que recibe el nombre de la propiedad `name` de function.json.  En la [sección de metadatos del mensaje](#trigger---message-metadata) se explican el resto de variables que se muestran.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

En el ejemplo siguiente se muestra un enlace de desencadenador de cola de un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función sondea la cola `myqueue-items` y escribe un registro cada vez que se procesa un elemento de cola.

Este es el archivo *function.json*:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

En la sección de [configuración](#trigger---configuration) se explican estas propiedades.

> [!NOTE]
> El nombre del parámetro se muestra como `context.bindings.<name>` en el código JavaScript que contiene la carga del elemento de cola. Esta carga también se pasa como segundo parámetro a la función.

Este es el código de JavaScript:

```javascript
module.exports = async function (context, message) {
    context.log('Node.js queue trigger function processed work item', message);
    // OR access using context.bindings.<name>
    // context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

En la sección acerca del [uso](#trigger---usage) se explica `myQueueItem`, que recibe el nombre de la propiedad `name` de function.json.  En la [sección de metadatos del mensaje](#trigger---message-metadata) se explican el resto de variables que se muestran.

# <a name="pythontabpython"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra cómo leer un mensaje en cola pasado a una función a través de un desencadenador.

Un desencadenador de cola de almacenamiento se define en *function.json*, donde *type* está establecido en `queueTrigger`.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "msg",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "messages",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

El código *_\_init_\_.py* declara un parámetro como `func.ServiceBusMessage`, lo que permite leer el mensaje de la cola en la función.

```python
import logging
import json

import azure.functions as func

def main(msg: func.QueueMessage):
    logging.info('Python queue trigger function processed a queue item.')

    result = json.dumps({
        'id': msg.id,
        'body': msg.get_body().decode('utf-8'),
        'expiration_time': (msg.expiration_time.isoformat()
                            if msg.expiration_time else None),
        'insertion_time': (msg.insertion_time.isoformat()
                           if msg.insertion_time else None),
        'time_next_visible': (msg.time_next_visible.isoformat()
                              if msg.time_next_visible else None),
        'pop_receipt': msg.pop_receipt,
        'dequeue_count': msg.dequeue_count
    })

    logging.info(result)
```

# <a name="javatabjava"></a>[Java](#tab/java)

En el siguiente ejemplo de Java se muestra una función de desencadenador de cola de almacenamiento que registra el mensaje desencadenado que se pone en la cola `myqueuename`.

 ```java
 @FunctionName("queueprocessor")
 public void run(
    @QueueTrigger(name = "msg",
                   queueName = "myqueuename",
                   connection = "myconnvarname") String message,
     final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
 ```

 ---

## <a name="trigger---attributes-and-annotations"></a>Desencadenador: atributos y anotaciones

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Para [bibliotecas de clases de C#](functions-dotnet-class-library.md), use los siguientes atributos para configurar un desencadenador de cola:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues/QueueTriggerAttribute.cs)

  El constructor del atributo toma el nombre de la cola que debe supervisar, tal como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      ILogger log)
  {
      ...
  }
  ```

  Puede establecer la propiedad `Connection` para especificar la configuración de aplicación que contiene la cadena de conexión de la cuenta de almacenamiento, tal como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      ILogger log)
  {
      ....
  }
  ```

  Para un ejemplo completo, consulte [Desencadenador: ejemplo de C#](#trigger).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Proporciona otra manera de especificar la cuenta de almacenamiento que se debe usar. El constructor toma el nombre de una configuración de aplicación que contiene una cadena de conexión de almacenamiento. El atributo se puede aplicar en el nivel de clase, método o parámetro. En el ejemplo siguiente se muestran el nivel de clase y de método:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

La cuenta de almacenamiento que se debe usar se determina en el orden siguiente:

* La propiedad `Connection` del atributo `QueueTrigger`.
* El atributo `StorageAccount` aplicado al mismo parámetro que el atributo `QueueTrigger`.
* El atributo `StorageAccount` aplicado a la función.
* El atributo `StorageAccount` aplicado a la clase.
* La configuración de aplicación "AzureWebJobsStorage".

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="pythontabpython"></a>[Python](#tab/python)

Python no admite atributos.

# <a name="javatabjava"></a>[Java](#tab/java)

La anotación `QueueTrigger` proporciona acceso a la cola que desencadena la función. En el ejemplo siguiente, el mensaje de la cola se hace disponible para la función mediante el parámetro `message`.

```java
package com.function;
import com.microsoft.azure.functions.annotation.*;
import java.util.Queue;
import com.microsoft.azure.functions.*;

public class QueueTriggerDemo {
    @FunctionName("QueueTriggerDemo")
    public void run(
        @QueueTrigger(name = "message", queueName = "messages", connection = "MyStorageConnectionAppSetting") String message,
        final ExecutionContext context
    ) {
        context.getLogger().info("Queue message: " + message);
    }
}
```

| Propiedad    | Descripción |
|-------------|-----------------------------|
|`name`       | Declara el nombre del parámetro en la firma de la función. Cuando se desencadena la función, el valor de este parámetro tiene el contenido del mensaje de la cola. |
|`queueName`  | Declara el nombre de la cola en la cuenta de almacenamiento. |
|`connection` | Apunta a la cadena de conexión de la cuenta de almacenamiento. |

---

## <a name="trigger---configuration"></a>Desencadenador: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `QueueTrigger`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D| Se debe establecer en `queueTrigger`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal.|
|**direction**| N/D | Solo en el archivo *function.json*. Se debe establecer en `in`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal. |
|**name** | N/D |El nombre de la variable que contiene la carga del elemento de cola en el código de función.  |
|**queueName** | **QueueName**| Nombre de la cola que se sondea. |
|**connection** | **Connection** |El nombre de una configuración de aplicación que contiene la cadena de conexión de almacenamiento que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre aquí. Por ejemplo, si establece `connection` en "MyStorage", el entorno de ejecución de Functions busca una configuración de aplicación denominada "MyStorage". Si deja `connection` vacía, el entorno en tiempo de ejecución de Functions usa la cadena de conexión de almacenamiento predeterminada en la configuración de aplicación que se denomina `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Desencadenador: uso

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Acceda a los datos de mensaje mediante un parámetro de método, como `string paramName`. Puede enlazar a cualquiera de los siguientes tipos:

* Objeto: el entorno de tiempo de ejecución de Functions deserializa una carga JSON en una instancia de una clase arbitraria definida en el código. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Si intenta enlazar a `CloudQueueMessage` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](#azure-storage-sdk-version-in-functions-1x).

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

Acceda a los datos de mensaje mediante un parámetro de método, como `string paramName`. `paramName` es el valor especificado en la propiedad `name` de *function.json*. Puede enlazar a cualquiera de los siguientes tipos:

* Objeto: el entorno de tiempo de ejecución de Functions deserializa una carga JSON en una instancia de una clase arbitraria definida en el código. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Si intenta enlazar a `CloudQueueMessage` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](#azure-storage-sdk-version-in-functions-1x).

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

La carga del elemento de cola está disponible mediante `context.bindings.<NAME>`, donde `<NAME>` coincide con el nombre definido en *function.json*. Si la carga es JSON, el valor se deserializa en un objeto.

# <a name="pythontabpython"></a>[Python](#tab/python)

Puede acceder al mensaje de la cola mediante un parámetro de tipo [QueueMessage](https://docs.microsoft.com/python/api/azure-functions/azure.functions.queuemessage?view=azure-python).

# <a name="javatabjava"></a>[Java](#tab/java)

La anotación [QueueTrigger](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.queuetrigger?view=azure-java-stable) proporciona acceso al mensaje de la cola que desencadenó la función.

---

## <a name="trigger---message-metadata"></a>Desencadenador: metadatos del mensaje

El desencadenador de cola proporciona varias [propiedades de metadatos](./functions-bindings-expressions-patterns.md#trigger-metadata). Estas propiedades pueden usarse como parte de expresiones de enlace en otros enlaces o como parámetros del código. Estas son propiedades de la clase [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage).

|Propiedad|Tipo|Descripción|
|--------|----|-----------|
|`QueueTrigger`|`string`|Carga de cola (si hay una cadena válida). Si la carga del mensaje de la cola es una cadena, `QueueTrigger` tiene el mismo valor que la variable denominada por la propiedad `name` en *function.json*.|
|`DequeueCount`|`int`|Es el número de veces que se ha quitado de la cola este mensaje.|
|`ExpirationTime`|`DateTimeOffset`|Es la hora de expiración del mensaje.|
|`Id`|`string`|Es el identificador del mensaje de cola.|
|`InsertionTime`|`DateTimeOffset`|Es la hora en la que el mensaje se agregó a la cola.|
|`NextVisibleTime`|`DateTimeOffset`|Es la siguiente hora a la que será visible el mensaje.|
|`PopReceipt`|`string`|Es la confirmación de extracción del mensaje.|

## <a name="trigger---poison-messages"></a>Desencadenador: mensajes dudosos

Si se produce un error en una función del desencadenador de cola, Azure Functions volverá a intentar esa función hasta cinco veces para un determinado mensaje en la cola, incluido el primer intento. Si se produce un error en los cinco intentos, Functions agregará un mensaje a la cola denominada *&lt;nombreDeColaOriginal>-poison*. Puede escribir una función para procesar los mensajes desde la cola de mensajes dudosos registrándolos o enviando una notificación indicando que se necesita atención manual.

Para controlar manualmente los mensajes dudosos, compruebe el elemento [dequeueCount](#trigger---message-metadata) del mensaje de la cola.

## <a name="trigger---polling-algorithm"></a>Desencadenador: algoritmo de sondeo

El desencadenador de cola implementa un algoritmo de interrupción exponencial aleatorio para reducir el efecto del sondeo de cola inactiva en los costos de transacción de almacenamiento.

El algoritmo utiliza la siguiente lógica:

- Cuando se encuentra un mensaje, el entorno de ejecución espera dos segundos y, a continuación, busca otro mensaje.
- Si no encuentra ningún mensaje, espera unos cuatro segundos antes de volver a intentarlo.
- Después de varios intentos fallidos para obtener un mensaje de la cola, el tiempo de espera sigue aumentando hasta que alcanza el tiempo de espera máximo, predeterminado en un minuto.
- El tiempo de espera máximo se configura mediante la propiedad `maxPollingInterval` en el [archivo host.json](functions-host-json.md#queues).

En el desarrollo local, el intervalo máximo de sondeo predeterminado es de dos segundos.

En lo que respecta a la facturación, el tiempo dedicado al sondeo por el entorno de ejecución es "gratuito" y no se incluye en la cuenta.

## <a name="trigger---concurrency"></a>Desencadenador: simultaneidad

Cuando hay varios mensajes en cola en espera, el desencadenador de cola recupera un lote de mensajes e invoca instancias de función de manera simultánea para procesarlas. De manera predeterminada, el tamaño de lote es 16. Cuando el número que se está procesando llega a 8, el entorno en tiempo de ejecución obtiene otro lote y empieza a procesar esos mensajes. Por lo tanto, el número máximo de mensajes simultáneos que se procesan por función en una máquina virtual es 24. Este límite se aplica por separado a cada función desencadenada por la cola en cada máquina virtual. Si la aplicación de función se escala horizontalmente a varias máquinas virtuales, cada máquina virtual esperará los desencadenadores e intentará ejecutar las funciones. Por ejemplo, si una aplicación de función se escala horizontalmente a 3 máquinas virtuales, el número máximo predeterminado de instancias simultáneas de una función desencadenada por la cola es 72.

El tamaño de lote y el umbral para obtener un lote nuevo se configuran en el [archivo host.json](functions-host-json.md#queues). Si quiere minimizar la ejecución en paralelo para las funciones desencadenadas por la cola en una aplicación de función, puede establecer el tamaño de lote en 1. Este valor solo elimina la simultaneidad siempre y cuando la aplicación de función se ejecute en una única máquina virtual (VM). 

El desencadenador de la cola automáticamente impide que una función procese un mensaje de cola varias veces; las funciones no tienen que escribirse para que sean idempotentes.

## <a name="trigger---hostjson-properties"></a>Desencadenador: propiedades de host.json

El archivo [host.json](functions-host-json.md#queues) contiene opciones de configuración que controlan el comportamiento de desencadenador de cola. Consulte la sección de [configuración de host.json](#hostjson-settings) para más información sobre las opciones de configuración disponibles.

## <a name="output"></a>Output

Use el enlace de salida de Azure Queue Storage para escribir mensajes en una cola.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que crea un mensaje de cola para cada una de las solicitudes HTTP que se reciben.

```csharp
[StorageAccount("MyStorageConnectionAppSetting")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  ILogger log)
    {
        log.LogInformation($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

En el ejemplo siguiente se muestra un enlace de desencadenador HTTP de un archivo *function.json* y el código de [script de C# (.csx)](functions-reference-csharp.md) que usa el enlace. La función crea un elemento de cola con una carga del objeto **CustomQueueMessage** para cada una de las solicitudes HTTP recibidas.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de script de C# que crea un único mensaje de cola:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, ILogger log)
{
    return input;
}
```

Puede enviar varios mensajes a la vez mediante el uso de un parámetro `ICollector` o `IAsyncCollector`. Este es el código de script de C# que envía varios mensajes, uno con los datos de la solicitud HTTP y otro con valores codificados de forma rígida:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItems, 
    ILogger log)
{
    myQueueItems.Add(input);
    myQueueItems.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

En el ejemplo siguiente se muestra un enlace de desencadenador HTTP de un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función crea un elemento de cola para cada una de las solicitudes HTTP recibidas.

Este es el archivo *function.json*:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

En la sección de [configuración](#output---configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Puede enviar varios mensajes a la vez mediante la definición de una matriz de mensajes para el enlace de salida `myQueueItem`. El código JavaScript envía dos mensajes de cola con valores codificados de forma rígida para cada una de las solicitudes HTTP recibidas.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

En el ejemplo siguiente se muestra cómo generar valores únicos y múltiples en las colas de almacenamiento. La configuración necesaria para *function.json* es la misma en cualquier caso.

Un enlace de cola de almacenamiento se define en *function.json*, donde *type* está establecido en `queue`.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureStorageQueuesConnectionString"
    }
  ]
}
```

Para establecer un mensaje individual en la cola, se pasa un valor único al método `set`.

```python
import azure.functions as func

def main(req: func.HttpRequest, msg: func.Out[str]) -> func.HttpResponse:

    input_msg = req.params.get('message')

    msg.set(input_msg)

    return 'OK'
```

Para crear varios mensajes en la cola, declare un parámetro como el tipo de lista adecuado y pase una matriz de valores (que coincidan con el tipo de lista) al método `set`.

```python
import azure.functions as func
import typing

def main(req: func.HttpRequest, msg: func.Out[typing.List[str]]) -> func.HttpResponse:

    msg.set(['one', 'two'])

    return 'OK'
```

# <a name="javatabjava"></a>[Java](#tab/java)

 En el ejemplo siguiente se muestra una función de Java que crea un mensaje de cola cuando una solicitud HTTP la desencadena.

```java
@FunctionName("httpToQueue")
@QueueOutput(name = "item", queueName = "myqueue-items", connection = "MyStorageConnectionAppSetting")
 public String pushToQueue(
     @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
     final String message,
     @HttpOutput(name = "response") final OutputBinding<String> result) {
       result.setValue(message + " has been added.");
       return message;
 }
```

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@QueueOutput` en los parámetros cuyo valor se escribiría en Queue Storage.  El tipo de parámetro debe ser `OutputBinding<T>`, donde `T` es cualquier tipo nativo de Java de un POJO.

---

## <a name="output---attributes-and-annotations"></a>Salida: atributos y anotaciones

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

En las [bibliotecas de clase C#](functions-dotnet-class-library.md), use [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs).

El atributo se aplica a un parámetro `out` o al valor de retorno de la función. El constructor del atributo toma el nombre de la cola, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

Para obtener un ejemplo completo, consulte [Salida: ejemplo de C#](#output).

Puede usar el atributo `StorageAccount` para especificar la cuenta de almacenamiento en el nivel de clase, método o parámetro. Para obtener más información, consulte Desencadenador: atributos.

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="pythontabpython"></a>[Python](#tab/python)

Python no admite atributos.

# <a name="javatabjava"></a>[Java](#tab/java)

La anotación `QueueOutput` permite tener acceso para escribir un mensaje con la salida de una función. En el ejemplo siguiente se muestra una función desencadenada mediante HTTP que crea un mensaje de cola.

```java
package com.function;
import java.util.*;
import com.microsoft.azure.functions.annotation.*;
import com.microsoft.azure.functions.*;

public class HttpTriggerQueueOutput {
    @FunctionName("HttpTriggerQueueOutput")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "message", queueName = "messages", connection = "MyStorageConnectionAppSetting") OutputBinding<String> message,
            final ExecutionContext context) {

        message.setValue(request.getQueryParameters().get("name"));
        return request.createResponseBuilder(HttpStatus.OK).body("Done").build();
    }
}
```

| Propiedad    | Descripción |
|-------------|-----------------------------|
|`name`       | Declara el nombre del parámetro en la firma de la función. Cuando se desencadena la función, el valor de este parámetro tiene el contenido del mensaje de la cola. |
|`queueName`  | Declara el nombre de la cola en la cuenta de almacenamiento. |
|`connection` | Apunta a la cadena de conexión de la cuenta de almacenamiento. |

El parámetro asociado a la anotación `QueueOutput` tiene como tipo una instancia de [OutputBinding\<T\>](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/OutputBinding.java).

---

## <a name="output---configuration"></a>Salida: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `Queue`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `queue`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal.|
|**direction** | N/D | Se debe establecer en `out`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal. |
|**name** | N/D | Nombre de la variable que representa la cola en el código de la función. Se establece en `$return` para hacer referencia al valor devuelto de la función.|
|**queueName** |**QueueName** | Nombre de la cola. |
|**connection** | **Connection** |El nombre de una configuración de aplicación que contiene la cadena de conexión de almacenamiento que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre aquí. Por ejemplo, si establece `connection` en "MyStorage", el entorno de ejecución de Functions busca una configuración de aplicación denominada "MyStorage". Si deja `connection` vacía, el entorno en tiempo de ejecución de Functions usa la cadena de conexión de almacenamiento predeterminada en la configuración de aplicación que se denomina `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uso de salidas

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Escriba un mensaje de cola único mediante un parámetro de método como `out T paramName`. Puede usar el tipo de valor devuelto del método en lugar de un parámetro `out`, y `T` puede ser cualquiera de los siguientes tipos:

* Un objeto serializable como JSON
* `string`
* `byte[]`
* [CloudQueueMessage] 

Si intenta enlazar a `CloudQueueMessage` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](#azure-storage-sdk-version-in-functions-1x).

En C# y script de C#, escriba varios mensajes de cola mediante uno de los siguientes tipos: 

* `ICollector<T>` o `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.azure.storage.queue.cloudqueue)

# <a name="c-scripttabcsharp-script"></a>[Script de C#](#tab/csharp-script)

Escriba un mensaje de cola único mediante un parámetro de método como `out T paramName`. `paramName` es el valor especificado en la propiedad `name` de *function.json*. Puede usar el tipo de valor devuelto del método en lugar de un parámetro `out`, y `T` puede ser cualquiera de los siguientes tipos:

* Un objeto serializable como JSON
* `string`
* `byte[]`
* [CloudQueueMessage] 

Si intenta enlazar a `CloudQueueMessage` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](#azure-storage-sdk-version-in-functions-1x).

En C# y script de C#, escriba varios mensajes de cola mediante uno de los siguientes tipos: 

* `ICollector<T>` o `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.azure.storage.queue.cloudqueue)

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

El elemento de la cola de salida está disponible mediante `context.bindings.<NAME>`, donde `<NAME>` coincide con el nombre definido en *function.json*. Puede usar una cadena o un objeto JSON serializable para la carga del elemento de cola.

# <a name="pythontabpython"></a>[Python](#tab/python)

Hay dos opciones para la generación de un mensaje del centro de eventos desde una función:

- **Valor devuelto**: Establezca la propiedad `name` de *function.json* en `$return`. Con esta configuración, el valor devuelto de la función se conserva como un mensaje de Queue Storage.

- **Imperativa**: Pase un valor al método [set](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python#set-val--t-----none) del parámetro declarado como tipo [Out](https://docs.microsoft.com/python/api/azure-functions/azure.functions.out?view=azure-python). El valor pasado a `set` se conserva como un mensaje de Queue Storage.

# <a name="javatabjava"></a>[Java](#tab/java)

Hay dos opciones para la generación de un mensaje del centro de eventos desde una función mediante la anotación [QueueOutput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.queueoutput):

- **Valor devuelto**: Al aplicar la anotación a la propia función, el valor devuelto de la función se conserva como un mensaje del centro de eventos.

- **Imperativa**: Para establecer explícitamente el valor del mensaje, aplique la anotación a un parámetro específico del tipo [`OutputBinding<T>`](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.OutputBinding), donde `T` es un POJO o cualquier tipo de Java nativo. Con esta configuración, pasar un valor al método `setValue` conserva el valor como un mensaje del centro de eventos.

---

## <a name="exceptions-and-return-codes"></a>Excepciones y códigos de retorno

| Enlace |  Referencia |
|---|---|
| Cola | [Códigos de error de cola](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| Blob, tabla, cola | [Códigos de error de almacenamiento](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Blob, tabla, cola |  [Solución de problemas](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>configuración de host.json

En esta sección se describen las opciones de configuración globales disponibles para este enlace en las versiones 2.x y posteriores. El siguiente archivo host.json de ejemplo contiene solo la configuración de la versión 2.x+ para este enlace. Para más información acerca de las opciones de configuración globales de la versión 2.x y posteriores, consulte [Referencia de host.json para Azure Functions](functions-host-json.md).

> [!NOTE]
> Para obtener una referencia de host.json en Functions 1.x, consulte la [referencia de host.json para Azure Functions, versión 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "queues": {
            "maxPollingInterval": "00:00:02",
            "visibilityTimeout" : "00:00:30",
            "batchSize": 16,
            "maxDequeueCount": 5,
            "newBatchThreshold": 8
        }
    }
}
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------|
|maxPollingInterval|00:00:01|Intervalo máximo entre sondeos de la cola. El mínimo es 00:00:00.100 (100 ms) y se incrementa hasta 00:01:00 (1 min).  En la versión 1.x, el tipo de datos es milisegundos. En cambio, en la 2.x y posteriores, es un intervalo de tiempo.|
|visibilityTimeout|00:00:00|Intervalo de tiempo entre los reintentos cuando se produce un error al procesar un mensaje. |
|batchSize|16|El número de mensajes en cola que el runtime de Functions recupera simultáneamente y procesa en paralelo. Cuando el número que se está procesando llega a `newBatchThreshold` el runtime obtiene otro lote y empieza a procesar esos mensajes. Por lo tanto, el número máximo de mensajes simultáneos que se procesan por función es `batchSize` más `newBatchThreshold`. Este límite se aplica por separado a cada función desencadenada por la cola. <br><br>Si desea evitar la ejecución en paralelo de los mensajes de una cola, puede establecer `batchSize` en 1. Sin embargo, este valor solo elimina la simultaneidad siempre y cuando la aplicación de función se ejecute en una única máquina virtual (VM). Si la aplicación de función se escala horizontalmente a varias máquinas virtuales, cada una de ellas podría ejecutar una instancia de cada función desencadenada por la cola.<br><br>El valor máximo de `batchSize` es 32. |
|maxDequeueCount|5|Número de veces que se intenta procesar un mensaje antes de pasarlo a la cola de mensajes dudosos.|
|newBatchThreshold|batchSize/2|Siempre que el número de mensajes que se procesan simultáneamente llega a este número, el runtime recupera otro lote.|

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)

<!--
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Queue storage trigger](functions-create-storage-queue-triggered-function.md)
-->

> [!div class="nextstepaction"]
> [Ir a un tutorial que usa un enlace de salida de Queue Storage](functions-integrate-storage-queue-output-binding.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
