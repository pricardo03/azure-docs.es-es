---
title: Enlaces de Azure Event Hubs para Azure Functions
description: Descubra cómo usar los enlaces de Azure Event Hubs de Azure Functions.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure functions, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/08/2017
ms.author: glenga
ms.openlocfilehash: d79a57db6f56264d4070debbca83de4192f7f503
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987093"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Enlaces de Azure Event Hubs para Azure Functions

En este artículo se explica cómo usar enlaces de [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) para Azure Functions. Azure Functions admite enlaces de desencadenador y salida para Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paquetes: Functions 1.x

En Azure Functions versión 1.x, los enlaces de Event Hubs se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus), versión 2.x.
El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) de GitHub.


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

En Functions 2.x, use el paquete [Microsoft.Azure.WebJobs.Extensions.EventHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs), versión 3.x.
El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Desencadenador

Use el desencadenador de Event Hubs para responder a un evento enviado a una secuencia de eventos de centro de eventos. Debe tener acceso de lectura al centro de eventos para configurar el desencadenador.

Cuando se activa una función de desencadenador de Event Hubs, el mensaje que la activa se pasa a la función como una cadena.

## <a name="trigger---scaling"></a>Desencadenador: escalado

Cada instancia e una función de desencadenador de Event Hubs está respaldada por una única instancia de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor). Event Hubs garantizan que solo una instancia de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) puede obtener una concesión en una partición determinada.

Por ejemplo, considere una instancia de Event Hubs con las siguientes características:

* 10 particiones.
* 1000 eventos distribuidos uniformemente en todas las particiones, con 100 mensajes en cada partición.

Cuando se habilita la función por primera vez, solo hay una instancia de la función. Vamos a asignar a esta instancia de función el nombre `Function_0`. `Function_0` tiene una sola instancia de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) que tiene una concesión en las diez particiones. Esta instancia lee eventos de las particiones 0-9. A partir de este punto, se producirá una de las siguientes acciones:

* **No se necesitan nuevas instancias de función**: `Function_0` puede procesar los 1000 eventos antes de que la lógica de escalado de Azure Functions se inicie. En este caso, `Function_0` procesa los 1000 mensajes.

* **Se agrega una instancia de función adicional**: la lógica de escalado de Azure Functions determina que `Function_0` tiene más mensajes de los que puede procesar. En este caso, se crea una instancia de aplicación de función (`Function_1`), junto con una nueva instancia de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor). Event Hubs detecta que una nueva instancia de host está tratando de leer los mensajes. Event Hubs equilibra la carga de las particiones entre sus instancias de host. Por ejemplo, las particiones 0-4 pueden asignarse a `Function_0` y las particiones 5-9, a `Function_1`. 

* **Se agregan N instancias de función más**: la lógica de escalado de Azure Functions determina que `Function_0` y `Function_1` tienen más mensajes de los que pueden procesar. Se crean instancias de aplicación de función `Function_2`...`Functions_N`, donde `N` es mayor que el número de particiones del centro de eventos. En nuestro ejemplo, Event Hubs vuelve a equilibrar la carga de las particiones, en este caso, entre las instancias `Function_0`...`Functions_9`. 

Téngalo en cuenta cuando Functions escale a `N` instancias, que es un número mayor que el número de particiones del centro de eventos. Esto se hace para asegurarse de que siempre haya instancias de [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) disponibles para obtener bloqueos de las particiones a medida que estén disponibles en otras instancias. Solo se le cobra por los recursos usados cuando se ejecuta la instancia de la función, y no por este aprovisionamiento en exceso.

Cuando se completa la ejecución de todas las funciones con o sin errores, se agregan puntos de comprobación a la cuenta de almacenamiento asociada. Cuando estos puntos de conexión se agregan correctamente, los 1000 mensajes ya no se vuelven a recuperar.

## <a name="trigger---example"></a>Desencadenador: ejemplo

Vea el ejemplo específico del lenguaje:

* [C#](#trigger---c-example)
* [Script de C# (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

### <a name="trigger---c-example"></a>Desencadenador: ejemplo de C#

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que registra el cuerpo del mensaje del desencadenador del centro de eventos.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Para acceder a los [metadatos del evento](#trigger---event-metadata) en el código de función, cree un enlace a un objeto [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) (requiere el uso de una instrucción para `Microsoft.Azure.EventHubs`). También puede acceder a las mismas propiedades mediante el uso de expresiones de enlace en la firma del método.  El ejemplo siguiente se muestran las dos maneras de obtener los mismos datos:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage, 
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.SystemProperties.Offset}");
    // Metadata accessed by using binding expressions in method parameters
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

Para recibir eventos en un lote, convierta `string` o `EventData` en una matriz.  

> [!NOTE]
> Cuando se reciben en un lote, no se puede establecer un enlace a parámetros de método como en el ejemplo anterior con `DateTime enqueuedTimeUtc` y debe recibirlos desde cada objeto `EventData`.  

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {Encoding.UTF8.GetString(message.Body)}");
        log.Info($"EnqueuedTimeUtc={message.SystemProperties.EnqueuedTimeUtc}");
    }
}
```

### <a name="trigger---c-script-example"></a>Desencadenador: ejemplo de script de C#

En el ejemplo siguiente se muestra un enlace de desencadenador de centro de eventos en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función registra el cuerpo del mensaje del desencadenador de centro de eventos.

Los ejemplos siguientes muestran datos de enlace de Event Hubs en el archivo *function.json*. El primer ejemplo es para Functions 2.x y el segundo para Functions 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Este es el código de script de C#:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Para acceder a los [metadatos del evento](#trigger---event-metadata) en el código de función, cree un enlace a un objeto [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) (requiere el uso de una instrucción para `Microsoft.Azure.EventHubs`). También puede acceder a las mismas propiedades mediante el uso de expresiones de enlace en la firma del método.  El ejemplo siguiente se muestran las dos maneras de obtener los mismos datos:

```cs
#r "Microsoft.Azure.EventHubs"

using System.Text;
using System;
using Microsoft.Azure.EventHubs;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    ILogger log)
{
    log.LogInformation($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    log.LogInformation($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.LogInformation($"Offset={myEventHubMessage.SystemProperties.Offset}");

    // Metadata accessed by using binding expressions
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={sequenceNumber}");
    log.LogInformation($"Offset={offset}");
}
```

Para recibir eventos en un lote, convierta `string` o `EventData` en una matriz:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Desencadenador: ejemplo de F#

En el ejemplo siguiente se muestra un enlace de desencadenador de centro de eventos en un archivo *function.json* y una [función de F#](functions-reference-fsharp.md) que usa el enlace. La función registra el cuerpo del mensaje del desencadenador de centro de eventos.

Los ejemplos siguientes muestran datos de enlace de Event Hubs en el archivo *function.json*. El primer ejemplo es para Functions 2.x y el segundo para Functions 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Este es el código de F#:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Desencadenador: ejemplo de JavaScript

En el ejemplo siguiente se muestra un enlace de desencadenador de centro de eventos en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función lee los [metadatos del evento](#trigger---event-metadata) y registra el mensaje.

Los ejemplos siguientes muestran datos de enlace de Event Hubs en el archivo *function.json*. El primer ejemplo es para Functions 2.x y el segundo para Functions 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Este es el código de JavaScript:

```javascript
module.exports = function (context, eventHubMessage) {
    context.log('Event Hubs trigger function processed message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);
     
    context.done();
};
```

Para recibir eventos en un lote, establezca `cardinality` en `many` en el archivo *function.json*, como se muestra en los ejemplos siguientes. El primer ejemplo es para Functions 2.x y el segundo para Functions 1.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "path": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Este es el código de JavaScript:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);
    
    eventHubMessages.forEach((message, index) => {
        context.log(`Processed message ${message}`);
        context.log(`EnqueuedTimeUtc = ${context.bindingData.enqueuedTimeUtcArray[index]}`);
        context.log(`SequenceNumber = ${context.bindingData.sequenceNumberArray[index]}`);
        context.log(`Offset = ${context.bindingData.offsetArray[index]}`);
    });

    context.done();
};
```

### <a name="trigger---java-example"></a>Desencadenador: ejemplo de Java

En el ejemplo siguiente se muestra un enlace de desencadenador de centro de eventos en un archivo *function.json* y una [función de Java](functions-reference-java.md) que usa el enlace. La función registra el cuerpo del mensaje del desencadenador del centro de eventos.

```json
{
  "type": "eventHubTrigger",
  "name": "msg",
  "direction": "in",
  "eventHubName": "myeventhubname",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

```java
@FunctionName("ehprocessor")
public void eventHubProcessor(
  @EventHubTrigger(name = "msg",
                  eventHubName = "myeventhubname",
                  connection = "myconnvarname") String message,
       final ExecutionContext context ) 
       {
          context.getLogger().info(message);
 }
 ```

 En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `EventHubTrigger` en los parámetros cuyo valor provendría del centro de eventos. Los parámetros con estas anotaciones hacen que la función se ejecuta cuando llega un evento.  Esta anotación se puede usar con tipos nativos de Java, POJO o valores que aceptan valores NULL mediante Optional<T>. 

## <a name="trigger---attributes"></a>Desencadenador: atributos

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubTriggerAttribute.cs).

El constructor del atributo toma el nombre del centro de eventos, el nombre del grupo de consumidores y el nombre de una configuración de aplicación que contenga la cadena de conexión. Para obtener más información sobre estas configuraciones, vea la sección [Configuración de desencadenador](#trigger---configuration). Este es un ejemplo de atributo `EventHubTriggerAttribute`:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Para un ejemplo completo, consulte [Desencadenador: ejemplo de C#](#trigger---c-example).

## <a name="trigger---configuration"></a>Desencadenador: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `EventHubTrigger`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `eventHubTrigger`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal.|
|**dirección** | N/D | Se debe establecer en `in`. Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal. |
|**name** | N/D | Nombre de la variable que representa el elemento de evento en el código de la función. | 
|**path** |**EventHubName** | Solo Functions 1.x. El nombre del centro de eventos. Cuando el nombre del centro de eventos también está presente en la cadena de conexión, ese valor reemplaza esta propiedad en tiempo de ejecución. | 
|**eventHubName** |**EventHubName** | Solo Functions 2.x. El nombre del centro de eventos. Cuando el nombre del centro de eventos también está presente en la cadena de conexión, ese valor reemplaza esta propiedad en tiempo de ejecución. |
|**consumerGroup** |**ConsumerGroup** | Una propiedad opcional que establece el [grupo de consumidores](../event-hubs/event-hubs-features.md#event-consumers) que se usará para suscribirse a los eventos del centro. Si se pasa por alto, se utilizará el grupo de consumidores `$Default`. | 
|**cardinalidad** | N/D | Para JavaScript. Defínalo como `many` para permitir el procesamiento por lotes.  Si se omite o se define como `one`, se pasa un único mensaje a la función. | 
|**conexión** |**Connection** | El nombre de una configuración de aplicación que contenga la cadena de conexión para el espacio de nombres del centro de eventos. Copie esta cadena de conexión haciendo clic en el botón **Información de conexión** del [espacio de nombres](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace), no del propio centro de eventos. Esta cadena de conexión debe tener al menos permisos de lectura para activar el desencadenador.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Desencadenador: metadatos de evento

El desencadenador de Event Hubs proporciona varias [propiedades de metadatos](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Estas propiedades pueden usarse como parte de expresiones de enlace en otros enlaces o como parámetros del código. Estas son propiedades de la clase [EventData](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata).

|Propiedad|Escriba|DESCRIPCIÓN|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|Instancia de `PartitionContext`.|
|`EnqueuedTimeUtc`|`DateTime`|Hora de puesta en la cola en UTC.|
|`Offset`|`string`|El desplazamiento de los datos relacionados con el flujo de partición de Event Hubs. El desplazamiento es un marcador o identificador del flujo de Event Hubs. El identificador es único dentro de una partición del flujo de Event Hubs.|
|`PartitionKey`|`string`|La partición a la que se deben enviar los datos del evento.|
|`Properties`|`IDictionary<String,Object>`|Las propiedades de usuario de los datos del evento.|
|`SequenceNumber`|`Int64`|El número de secuencia de registro del evento.|
|`SystemProperties`|`IDictionary<String,Object>`|Las propiedades del sistema, incluidos los datos del evento.|

Consulte los [ejemplos de código](#trigger---example) que utilizan estas propiedades más arriba en este artículo.

## <a name="trigger---hostjson-properties"></a>Desencadenador: propiedades de host.json

El archivo [host.json](functions-host-json.md#eventhub) contiene opciones de configuración que controlan el comportamiento de Event Hubs.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Salida

Use el enlace de salida de Event Hubs para escribir eventos en una secuencia. Debe tener permiso de envío a un centro de eventos para escribir eventos en él.

Asegúrese de que las referencias de los paquetes necesarios están en su lugar: [Functions 1.x](#packages---functions-1.x) o [Functions 2.x](#packages---functions-2.x) 

## <a name="output---example"></a>Salida: ejemplo

Vea el ejemplo específico del lenguaje:

* [C#](#output---c-example)
* [Script de C# (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)
* [Java](#output---java-example)

### <a name="output---c-example"></a>Salida: ejemplo de C#

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que escribe un mensaje en un centro de eventos usando el valor devuelto del método como resultado:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Salida: ejemplo de script de C#

En el ejemplo siguiente se muestra un enlace de desencadenador de centro de eventos en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función escribe un mensaje a un centro de eventos.

Los ejemplos siguientes muestran datos de enlace de Event Hubs en el archivo *function.json*. El primer ejemplo es para Functions 2.x y el segundo para Functions 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Este es el código de script de C# que crea un mensaje:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Este es el código de script de C# que crea varios mensajes:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Salida: ejemplo de F#

En el ejemplo siguiente se muestra un enlace de desencadenador de centro de eventos en un archivo *function.json* y una [función de F#](functions-reference-fsharp.md) que usa el enlace. La función escribe un mensaje a un centro de eventos.

Los ejemplos siguientes muestran datos de enlace de Event Hubs en el archivo *function.json*. El primer ejemplo es para Functions 2.x y el segundo para Functions 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Este es el código de F#:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Salida: ejemplo de JavaScript

En el ejemplo siguiente se muestra un enlace de desencadenador de centro de eventos en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función escribe un mensaje a un centro de eventos.

Los ejemplos siguientes muestran datos de enlace de Event Hubs en el archivo *function.json*. El primer ejemplo es para Functions 2.x y el segundo para Functions 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Este es el código JavaScript que envía un mensaje:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Este es el código JavaScript que envía varios mensajes:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

### <a name="output---java-example"></a>Salida: ejemplo de Java

El ejemplo siguiente muestra una función de Java que escribe un mensaje que contiene la hora actual en un centro de eventos.

```java
@}FunctionName("sendTime")
@EventHubOutput(name = "event", eventHubName = "samples-workitems", connection = "AzureEventHubConnection")
public String sendTime(
   @TimerTrigger(name = "sendTimeTrigger", schedule = "0 *&#47;5 * * * *") String timerInfo)  {
     return LocalDateTime.now().toString();
 }
 ```

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@EventHubOutput` en los parámetros cuyo valor se publicaría en el centro de eventos.  El parámetro debe ser del tipo `OutputBinding<T>`, donde T es un tipo POJO o cualquier tipo nativo de Java. 

## <a name="output---attributes"></a>Salida: atributos

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs).

El constructor del atributo toma el nombre del centro de eventos y el nombre de una configuración de aplicación que contenga la cadena de conexión. Para obtener más información sobre estas configuraciones, vea [Salida: configuración](#output---configuration). Este es un ejemplo de atributo `EventHub`:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Para obtener un ejemplo completo, consulte [Salida: ejemplo de C#](#output---c-example).

## <a name="output---configuration"></a>Salida: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `EventHub`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**type** | N/D | Debe establecerse en "eventHub". |
|**dirección** | N/D | Debe establecerse en "out". Este parámetro se establece automáticamente cuando se crea el enlace en Azure Portal. |
|**name** | N/D | Nombre de la variable que se usa en el código de la función que representa el evento. | 
|**path** |**EventHubName** | Solo Functions 1.x. El nombre del centro de eventos. Cuando el nombre del centro de eventos también está presente en la cadena de conexión, ese valor reemplaza esta propiedad en tiempo de ejecución. | 
|**eventHubName** |**EventHubName** | Solo Functions 2.x. El nombre del centro de eventos. Cuando el nombre del centro de eventos también está presente en la cadena de conexión, ese valor reemplaza esta propiedad en tiempo de ejecución. |
|**conexión** |**Connection** | El nombre de una configuración de aplicación que contenga la cadena de conexión para el espacio de nombres del centro de eventos. Copie esta cadena de conexión haciendo clic en el botón **Información de conexión** del *espacio de nombres*, no del propio centro de eventos. Esta cadena de conexión debe tener permisos de envío para enviar el mensaje a la secuencia de eventos.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uso de salidas

En C# y scripts de C#, envíe mensajes mediante un parámetro de método, como `out string paramName`. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json*. Para escribir varios mensajes, puede usar `ICollector<string>` o `IAsyncCollector<string>` en lugar de `out string`.

En JavaScript, puede obtener acceso al evento de salida usando `context.bindings.<name>`. `<name>` es el valor especificado en la propiedad `name` de *function.json*.

## <a name="exceptions-and-return-codes"></a>Excepciones y códigos de retorno

| Enlace | Referencia |
|---|---|
| Centro de eventos | [Guía de operaciones](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)
