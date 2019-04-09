---
title: Enlaces de Azure Service Bus en Azure Functions
description: Descubra cómo utilizar desencadenadores y enlaces de Azure Service Bus en Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: azure functions, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 04/01/2017
ms.author: cshoe
ms.openlocfilehash: 1f4bf24ef5f96fea7602b38d857f12d950fb0b24
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59275646"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Enlaces de Azure Service Bus en Azure Functions

En este artículo se explica cómo trabajar con enlaces de Azure Service Bus en Azure Functions. Azure Functions admite enlaces de desencadenador y salida para colas y temas de Service Bus.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paquetes: Functions 1.x

Los enlaces de Service Bus se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus), versión 2.x. 

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

Los enlaces de Service Bus se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.Extensions.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus), versión 3.x. El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Desencadenador

Utilice el desencadenador de Service Bus para responder a mensajes de una cola o tema de Service Bus. 

## <a name="trigger---example"></a>Desencadenador: ejemplo

Vea el ejemplo específico del lenguaje:

* [C#](#trigger---c-example)
* [Script de C# (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Desencadenador: ejemplo de C#

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que lee [metadatos de mensaje](#trigger---message-metadata) y registra un mensaje de cola de Service Bus:

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    ILogger log)
{
    log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"DeliveryCount={deliveryCount}");
    log.LogInformation($"MessageId={messageId}");
}
```

Este ejemplo va dirigido a Azure Functions 1.x. Para que este código funcione con 2.x:

- [omita el parámetro de derechos de acceso](#trigger---configuration)
- cambiar el tipo del parámetro de registro `TraceWriter` a `ILogger`
- Cambio `log.Info` a `log.LogInformation`

### <a name="trigger---c-script-example"></a>Desencadenador: ejemplo de script de C#

En el ejemplo siguiente se muestra un enlace de desencadenador de Service Bus en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función lee [metadatos de mensaje](#trigger---message-metadata) y registra un mensaje de cola de Service Bus.

Estos son los datos de enlace del archivo *function.json*:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Este es el código de script de C#:

```cs
using System;

public static void Run(string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"DeliveryCount={deliveryCount}");
    log.Info($"MessageId={messageId}");
}
```

### <a name="trigger---f-example"></a>Desencadenador: ejemplo de F#

En el ejemplo siguiente se muestra un enlace de desencadenador de Service Bus en un archivo *function.json* y una [función de F#](functions-reference-fsharp.md) que usa el enlace. La función registra un mensaje de cola de Service Bus. 

Estos son los datos de enlace del archivo *function.json*:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Este es el código del script de F#:

```fsharp
let Run(myQueueItem: string, log: ILogger) =
    log.LogInformation(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---java-example"></a>Desencadenador: ejemplo de Java

La siguiente función de Java usa la `@ServiceBusQueueTrigger` anotación desde el [Java funciones de biblioteca en tiempo de ejecución](/java/api/overview/azure/functions/runtime) para describir la configuración de un desencadenador de cola de Service Bus. La función toma el mensaje colocado en la cola y lo agrega a los registros.

```java
@FunctionName("sbprocessor")
 public void serviceBusProcess(
    @ServiceBusQueueTrigger(name = "msg",
                             queueName = "myqueuename",
                             connection = "myconnvarname") String message,
   final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
```

Las funciones de Java también pueden activarse cuando se agrega un mensaje a un tema de Service Bus. En el ejemplo siguiente se usa el `@ServiceBusTopicTrigger` anotación para describir la configuración del desencadenador.

```java
@FunctionName("sbtopicprocessor")
    public void run(
        @ServiceBusTopicTrigger(
            name = "message",
            topicName = "mytopicname",
            subscriptionName = "mysubscription",
            connection = "ServiceBusConnection"
        ) String message,
        final ExecutionContext context
    ) {
        context.getLogger().info(message);
    }
```

### <a name="trigger---javascript-example"></a>Desencadenador: ejemplo de JavaScript

En el ejemplo siguiente se muestra un enlace de desencadenador de Service Bus en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función lee [metadatos de mensaje](#trigger---message-metadata) y registra un mensaje de cola de Service Bus. 

Estos son los datos de enlace del archivo *function.json*:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Este es el código del script de JavaScript:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('DeliveryCount =', context.bindingData.deliveryCount);
    context.log('MessageId =', context.bindingData.messageId);
    context.done();
};
```

## <a name="trigger---attributes"></a>Desencadenador: atributos

Para las [bibliotecas de clases de C#](functions-dotnet-class-library.md), utilice los siguientes atributos para configurar un desencadenador de Service Bus:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  El constructor del atributo toma el nombre de la cola o del tema y la suscripción. En la versión 1.x de Azure Functions, también puede especificar los derechos de acceso de la conexión. Si no los especifica, el valor predeterminado es `Manage`. Para más información, consulte la sección [Desencadenador: configuración](#trigger---configuration).

  A continuación, tiene un ejemplo que muestra el atributo que se usa con un parámetro de cadena:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Puede establecer la propiedad `Connection` para especificar la cuenta de Service Bus que se usará, tal como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Para un ejemplo completo, consulte [Desencadenador: ejemplo de C#](#trigger---c-example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAccountAttribute.cs)

  Proporciona otra manera de especificar la cuenta de Service Bus que se debe usar. El constructor toma el nombre de una configuración de aplicación que contiene una cadena de conexión de Service Bus. El atributo se puede aplicar en el nivel de clase, método o parámetro. En el ejemplo siguiente se muestran el nivel de clase y de método:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, ILogger log)
  {
      ...
  }
  ```

La cuenta de Service Bus que se debe usar se determina en el orden siguiente:

* La propiedad `Connection` del atributo `ServiceBusTrigger`.
* El atributo `ServiceBusAccount` aplicado al mismo parámetro que el atributo `ServiceBusTrigger`.
* El atributo `ServiceBusAccount` aplicado a la función.
* El atributo `ServiceBusAccount` aplicado a la clase.
* La configuración de aplicación "AzureWebJobsServiceBus".

## <a name="trigger---configuration"></a>Desencadenador: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `ServiceBusTrigger`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**Tipo** | N/D | Debe establecerse en "serviceBusTrigger". Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal.|
|**dirección** | N/D | Debe establecerse en "in". Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal. |
|**Nombre** | N/D | Nombre de la variable que representa el mensaje de cola o tema en el código de la función. Se establece en "$return" para hacer referencia al valor devuelto de la función. |
|**queueName**|**QueueName**|Nombre de la cola que se debe supervisar.  Se establece únicamente si se supervisa una cola, no un tema.
|**topicName**|**topicName**|Nombre del tema que se debe supervisar. Se establece únicamente si se supervisa un tema, no una cola.|
|**subscriptionName**|**SubscriptionName**|Nombre de la suscripción que se debe supervisar. Se establece únicamente si se supervisa un tema, no una cola.|
|**connection**|**Conexión**|Nombre de una configuración de aplicación que contiene la cadena de conexión de Service Bus que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre. Por ejemplo, si establece `connection` en "MyServiceBus", el entorno de ejecución de Functions busca una configuración de aplicación denominada "AzureWebJobsMyServiceBus". Si deja el valor de `connection` vacío, el entorno de ejecución de Functions usa la cadena de conexión de Service Bus predeterminada en la configuración de aplicación que se denomina "AzureWebJobsServiceBus".<br><br>Para obtener la cadena de conexión, siga los pasos mostrados en [Obtención de las credenciales de administración](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#get-the-connection-string). La cadena de conexión debe ser para un espacio de nombres de Service Bus y no estar limitada a una cola o un tema concretos. |
|**accessRights**|**Access**|Derechos de acceso para la cadena de conexión. Los valores disponibles son `manage` y `listen`. El valor predeterminado es `manage`, lo que indica que `connection` tiene el permiso **Administrar**. Si usa una cadena de conexión que no tiene el permiso **Administrar**, establezca `accessRights` en "listen". De lo contrario, el runtime de Functions puede intentar realizar operaciones que requieran derechos de administración y no conseguirlo. En la versión 2.x de Azure Functions, esta propiedad no está disponible porque la versión más reciente del SDK de Storage no admite las operaciones de administración.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Desencadenador: uso

En C# y el script de C#, puede usar los tipos de parámetros siguientes para el mensaje de cola o tema:

* `string` -Si el mensaje es texto.
* `byte[]` : Útil para datos binarios.
* Un tipo personalizado: si el mensaje contiene el archivo JSON, Azure Functions intenta deserializar los datos JSON.
* `BrokeredMessage`: proporciona el mensaje deserializado con el método [BrokeredMessage.GetBody<T>()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1).

Estos parámetros son para la versión de Azure Functions 1.x; para 2.x, use [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) en lugar de `BrokeredMessage`.

En JavaScript, puede obtener acceso al mensaje de cola o tema mediante el uso de `context.bindings.<name from function.json>`. El mensaje de Service Bus se pasa a la función como una cadena o como un objeto JSON.

## <a name="trigger---poison-messages"></a>Desencadenador: mensajes dudosos

El control de mensajes dudosos no se puede controlar ni configurar en Azure Functions. Service Bus controla este tipo de mensajes.

## <a name="trigger---peeklock-behavior"></a>Desencadenador: comportamiento de PeekLock

El entorno de ejecución de Functions recibe un mensaje en [modo PeekLock](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Llama a `Complete` en el mensaje si la función finaliza correctamente, o llama a `Abandon` si se produce un error en la función. Si la ejecución de la función dura más que el tiempo de espera de `PeekLock`, el bloqueo se renovará automáticamente siempre que la función esté en ejecución. 

El `maxAutoRenewDuration` se puede configurar en *host.json*, que se asigna a [OnMessageOptions.MaxAutoRenewDuration](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.messagehandleroptions.maxautorenewduration?view=azure-dotnet). El valor predeterminado de esta opción es de 5 minutos según la documentación de Service Bus, aunque que puede aumentar el límite de tiempo de Functions desde el valor predeterminado de 5 minutos a 10 minutos. Sin embargo puede que no quiera hacer eso con las funciones de Service Bus, ya que podría superar el límite de renovación de este.

## <a name="trigger---message-metadata"></a>Desencadenador: metadatos del mensaje

El desencadenador de Service Bus proporciona varias [propiedades de metadatos](./functions-bindings-expressions-patterns.md#trigger-metadata). Estas propiedades pueden usarse como parte de expresiones de enlace en otros enlaces o como parámetros del código. Estas son propiedades de la clase [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage).

|Propiedad|Escriba|DESCRIPCIÓN|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Número total de entregas.|
|`DeadLetterSource`|`string`|Origen de los mensajes fallidos.|
|`ExpiresAtUtc`|`DateTime`|Hora de expiración en UTC.|
|`EnqueuedTimeUtc`|`DateTime`|Hora de puesta en la cola en UTC.|
|`MessageId`|`string`|Se trata de un valor definido por el usuario que Service Bus puede utilizar para identificar mensajes duplicados, si está habilitado.|
|`ContentType`|`string`|Un identificador de tipo de contenido que usa el remitente y el receptor para la lógica específica de la aplicación.|
|`ReplyTo`|`string`|La respuesta a la dirección de cola.|
|`SequenceNumber`|`Int64`|El número único que Service Bus asigna a un mensaje.|
|`To`|`string`|Dirección de envío.|
|`Label`|`string`|Etiqueta específica de la aplicación.|
|`CorrelationId`|`string`|Identificador de correlación.|

> [!NOTE]
> Actualmente, el desencadenador solo funciona con colas y suscripciones que no usan sesiones. Realice un seguimiento de [este elemento de la característica](https://github.com/Azure/azure-functions-host/issues/563) para comprobar si hay actualizaciones relacionadas con ella. 

Consulte los [ejemplos de código](#trigger---example) que utilizan estas propiedades más arriba en este artículo.

## <a name="trigger---hostjson-properties"></a>Desencadenador: propiedades de host.json

El archivo [host.json](functions-host-json.md#servicebus) contiene opciones de configuración que controlan el comportamiento de desencadenador de Service Bus.

```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "maxAutoRenewDuration": "00:05:00"
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------|
|maxConcurrentCalls|16|Número máximo de llamadas simultáneas a la devolución de llamada que el bombeo de mensajes debe iniciar. De forma predeterminada, el entorno de ejecución de Functions procesa simultáneamente varios mensajes. Para indicar al entorno de ejecución que procese solo los mensajes de una única cola o tema, establezca `maxConcurrentCalls` en 1. |
|prefetchCount|N/D|Valor predeterminado de PrefetchCount que utilizará el receptor de mensajes subyacente.|
|maxAutoRenewDuration|00:05:00|Duración máxima dentro de la cual el bloqueo de mensajes se renovará automáticamente.|

## <a name="output"></a>Salida

Use el enlace de salida de Azure Service Bus para enviar mensajes de cola o tema.

## <a name="output---example"></a>Salida: ejemplo

Vea el ejemplo específico del lenguaje:

* [C#](#output---c-example)
* [Script de C# (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [Java](#output---java-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Salida: ejemplo de C#

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que envía un mensaje de cola de Service Bus:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, ILogger log)
{
    log.LogInformation($"C# function processed: {input.Text}");
    return input.Text;
}
```

### <a name="output---c-script-example"></a>Salida: ejemplo de script de C#

En el ejemplo siguiente se muestra un enlace de salida de Service Bus en un archivo *function.json* y una [función de script de C#](functions-reference-csharp.md) que usa el enlace. La función usa un desencadenador de temporizador para enviar un mensaje de cola cada 15 segundos.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Este es el código de script de C# que crea un único mensaje:

```cs
public static void Run(TimerInfo myTimer, ILogger log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue = message;
}
```

Este es el código de script de C# que crea varios mensajes:

```cs
public static void Run(TimerInfo myTimer, ILogger log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Salida: ejemplo de F#

En el ejemplo siguiente se muestra un enlace de salida de Service Bus en un archivo *function.json* y una [función de script de F#](functions-reference-fsharp.md) que usa el enlace. La función usa un desencadenador de temporizador para enviar un mensaje de cola cada 15 segundos.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Este es el código de script de F# que crea un único mensaje:

```fsharp
let Run(myTimer: TimerInfo, log: ILogger, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.LogInformation(message)
    outputSbQueue = message
```

### <a name="output---java-example"></a>Salida: ejemplo de Java

En el ejemplo siguiente se muestra una función de Java que envía un mensaje a una cola de Service Bus `myqueue` cuando una solicitud HTTP la desencadena.

```java
@FunctionName("httpToServiceBusQueue")
@ServiceBusQueueOutput(name = "message", queueName = "myqueue", connection = "AzureServiceBusConnection")
public String pushToQueue(
  @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
  final String message,
  @HttpOutput(name = "response") final OutputBinding<T> result ) {
      result.setValue(message + " has been sent.");
      return message;
 }
```

 En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@QueueOutput` en los parámetros de función cuyo valor se escribiría en una cola de Service Bus.  El parámetro type debe ser `OutputBinding<T>`, donde T es cualquier tipo nativo de Java de un POJO.

También pueden escribir funciones de Java a un tema de Service Bus. En el ejemplo siguiente se usa el `@ServiceBusTopicOutput` anotación para describir la configuración para el enlace de salida. 

```java
@FunctionName("sbtopicsend")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @ServiceBusTopicOutput(name = "message", topicName = "mytopicname", subscriptionName = "mysubscription", connection = "ServiceBusConnection") OutputBinding<String> message,
            final ExecutionContext context) {
        
        String name = request.getBody().orElse("Azure Functions");

        message.setValue(name);
        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        
    }
```

### <a name="output---javascript-example"></a>Salida: ejemplo de JavaScript

En el ejemplo siguiente se muestra un enlace de salida de Service Bus en un archivo *function.json* y una [función de JavaScript](functions-reference-node.md) que usa el enlace. La función usa un desencadenador de temporizador para enviar un mensaje de cola cada 15 segundos.

Estos son los datos de enlace del archivo *function.json*:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Este es el código de script de JavaScript que crea un único mensaje:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = message;
    context.done();
};
```

Este es el código de script de JavaScript que crea varios mensajes:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = [];
    context.bindings.outputSbQueue.push("1 " + message);
    context.bindings.outputSbQueue.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes"></a>Salida: atributos

En las [bibliotecas de clases de C#](functions-dotnet-class-library.md), use el atributo [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAttribute.cs).

El constructor del atributo toma el nombre de la cola o del tema y la suscripción. También puede especificar los derechos de acceso de la conexión. En la sección [Salida: configuración](#output---configuration) se explica cómo elegir la configuración de derechos de acceso. Este es un ejemplo que muestra el atributo aplicado al valor devuelto de la función:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Puede establecer la propiedad `Connection` para especificar la cuenta de Service Bus que se usará, tal como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Para obtener un ejemplo completo, consulte [Salida: ejemplo de C#](#output---c-example).

Puede usar el atributo `ServiceBusAccount` para especificar la cuenta de Service Bus en el nivel de clase, método o parámetro.  Para obtener más información, consulte [Desencadenador: atributos](#trigger---attributes).

## <a name="output---configuration"></a>Salida: configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `ServiceBus`.

|Propiedad de function.json | Propiedad de atributo |DESCRIPCIÓN|
|---------|---------|----------------------|
|**Tipo** | N/D | Debe establecerse en "serviceBus". Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal.|
|**dirección** | N/D | Debe establecerse en "out". Esta propiedad se establece automáticamente cuando se crea el desencadenador en Azure Portal. |
|**Nombre** | N/D | Nombre de la variable que representa la cola o el tema en el código de la función. Se establece en "$return" para hacer referencia al valor devuelto de la función. |
|**queueName**|**QueueName**|Nombre de la cola.  Se establece únicamente si se envían mensajes de cola, no de tema.
|**topicName**|**topicName**|Nombre del tema que se debe supervisar. Se establece únicamente si se envían mensajes de tema, no de cola.|
|**connection**|**Conexión**|Nombre de una configuración de aplicación que contiene la cadena de conexión de Service Bus que se usará para este enlace. Si el nombre de la configuración de aplicación comienza con "AzureWebJobs", puede especificar solo el resto del nombre. Por ejemplo, si establece `connection` en "MyServiceBus", el entorno de ejecución de Functions busca una configuración de aplicación denominada "AzureWebJobsMyServiceBus". Si deja el valor de `connection` vacío, el entorno de ejecución de Functions usa la cadena de conexión de Service Bus predeterminada en la configuración de aplicación que se denomina "AzureWebJobsServiceBus".<br><br>Para obtener la cadena de conexión, siga los pasos mostrados en [Obtención de las credenciales de administración](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#get-the-connection-string). La cadena de conexión debe ser para un espacio de nombres de Service Bus y no estar limitada a una cola o un tema concretos.|
|**accessRights**|**Access**|Derechos de acceso para la cadena de conexión. Los valores disponibles son `manage` y `listen`. El valor predeterminado es `manage`, lo que indica que `connection` tiene el permiso **Administrar**. Si usa una cadena de conexión que no tiene el permiso **Administrar**, establezca `accessRights` en "listen". De lo contrario, el runtime de Functions puede intentar realizar operaciones que requieran derechos de administración y no conseguirlo. En la versión 2.x de Azure Functions, esta propiedad no está disponible porque la versión más reciente del SDK de Storage no admite las operaciones de administración.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Uso de salidas

En Azure Functions 1.x, el entorno de ejecución crea la cola si no existe y se estableció `accessRights` en `manage`. En la versión 2.x de Functions, ya debe existir la cola o el tema. Si especifica una cola o un tema que no existe, la función generará un error. 

En C# y el script de C#, puede usar los tipos de parámetros siguientes para el enlace de salida:

* `out T paramName` - `T` puede ser cualquier tipo serializable de JSON. Si el valor del parámetro es null cuando finaliza la función, Functions crea el mensaje con un objeto null.
* `out string` -Si el valor del parámetro es nulo cuando sale la función, Functions no crea un mensaje.
* `out byte[]` -Si el valor del parámetro es nulo cuando sale la función, Functions no crea un mensaje.
* `out BrokeredMessage` -Si el valor del parámetro es nulo cuando sale la función, Functions no crea un mensaje.
* `ICollector<T>` o `IAsyncCollector<T>` : para crear varios mensajes. Se crea un mensaje al llamar al método `Add` .

En las funciones asincrónicas, use el valor devuelto o `IAsyncCollector` en lugar de un parámetro `out`.

Estos parámetros son para la versión de Azure Functions 1.x; para 2.x, use [`Message`](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) en lugar de `BrokeredMessage`.

En JavaScript, puede obtener acceso a la cola o al tema mediante el uso de `context.bindings.<name from function.json>`. Puede asignar una cadena, una matriz de bytes o un objeto de JavaScript (deserializado en JSON) a `context.binding.<name>`.

## <a name="exceptions-and-return-codes"></a>Excepciones y códigos de retorno

| Enlace | Referencia |
|---|---|
| Azure Service Bus | [Códigos de Error de Bus de servicio](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-exceptions) |
| Azure Service Bus | [Límites de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quotas) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>configuración de host.json

En esta sección se describen las opciones de configuración globales disponibles para este enlace en la versión 2.x. El siguiente archivo host.json de ejemplo contiene solo la configuración de la versión 2.x para este enlace. Para obtener más información acerca de las opciones de configuración globales de la versión 2.x, consulte la [referencia de host.json para Azure Functions, versión 2.x](functions-host-json.md).

> [!NOTE]
> Para obtener una referencia de host.json en Functions 1.x, consulte la [referencia de host.json para Azure Functions, versión 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "serviceBus": {
            "prefetchCount": 100,
            "messageHandlerOptions": {
                "autoComplete": false,
                "maxConcurrentCalls": 32,
                "maxAutoRenewDuration": "00:55:00"
            }
        }
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------|
|maxAutoRenewDuration|00:05:00|Duración máxima dentro de la cual el bloqueo de mensajes se renovará automáticamente.|
|autoComplete|true|Si el desencadenador debe marcar inmediatamente como completado (autocompletar) o esperar a que se complete el procesamiento de la llamada.|
|maxConcurrentCalls|16|Número máximo de llamadas simultáneas a la devolución de llamada que el bombeo de mensajes debe iniciar. De forma predeterminada, el entorno de ejecución de Functions procesa simultáneamente varios mensajes. Para indicar al entorno de ejecución que procese solo los mensajes de una única cola o tema, establezca `maxConcurrentCalls` en 1. |
|prefetchCount|N/D|Valor predeterminado de PrefetchCount que utilizará el receptor de mensajes subyacente.|


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre los enlaces y desencadenadores de Azure functions](functions-triggers-bindings.md)
