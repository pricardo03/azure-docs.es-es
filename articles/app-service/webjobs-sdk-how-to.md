---
title: 'Uso del SDK de WebJobs: Azure'
description: Obtenga más información sobre cómo escribir código para el SDK de WebJobs. Cree en segundo plano basado en eventos de trabajos de procesamiento que tienen acceso a datos en servicios de Azure y servicios de terceros.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: jeconnoc
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/18/2019
ms.author: glenga
ms.openlocfilehash: 81401d95b9c40f16a6e593d61b79f5c2d647c0c5
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518837"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Uso del SDK de Azure WebJobs para el procesamiento en segundo plano basado en eventos

Este artículo proporcionan instrucciones sobre cómo trabajar con el SDK de WebJobs de Azure. Para comenzar inmediatamente con WebJobs, consulte [empezar a trabajar con el SDK de WebJobs de Azure para el procesamiento en segundo plano basado en eventos](webjobs-sdk-get-started.md). 

## <a name="webjobs-sdk-versions"></a>Versiones del SDK de WebJobs

Estas son las principales diferencias entre la versión 3. *x* y la versión 2. *x* del SDK de WebJobs:

* Versión 3. *x* agrega compatibilidad con .NET Core.
* En la versión 3. *x*, es necesario instalar explícitamente la extensión de enlace de almacenamiento requerida por el SDK de WebJobs. En la versión 2. *x*, los enlaces de almacenamiento se incluyeron en el SDK.
* Visual Studio herramientas para .NET Core (3. *x*) proyectos difiere de las herramientas de .NET Framework (2. *x*) proyectos. Para obtener más información, consulte [desarrollar e implementar WebJobs con Visual Studio: Azure App Service](webjobs-dotnet-deploy-vs.md).

Cuando sea posible, se proporcionan ejemplos de versión 3. *x* y la versión 2. *x*.

> [!NOTE]
> [Las funciones de Azure](../azure-functions/functions-overview.md) se basa en el SDK de WebJobs, y este artículo proporcionan vínculos a documentación de Azure Functions para algunos temas. Tenga en cuenta estas diferencias entre Functions y el SDK de WebJobs:
> * Versión de Azure Functions 2. *x* corresponde a la versión 3 del SDK de WebJobs. *x*y Azure Functions 1. *x* corresponde a 2 del SDK de WebJobs. *x*. Repositorios de código fuente utilice el SDK de WebJobs de numeración.
> * Código de ejemplo para Azure Functions C# bibliotecas de clases es como el código del SDK de WebJobs, salvo que no es necesario un `FunctionName` atributo en un proyecto de SDK de WebJobs.
> * Algunos tipos de enlace solo se admiten en las funciones, como HTTP (Webhooks) y Event Grid (que se basa en HTTP).
>
> Para obtener más información, consulte [Compare the WebJobs SDK and Azure Functions](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs) (Comparación del SDK de WebJobs con Azure Functions).

## <a name="webjobs-host"></a>Host de WebJobs

El host es un contenedor en tiempo de ejecución de las funciones.  Realiza escuchas de desencadenadores y llamadas de función. En la versión 3. *x*, el host es una implementación de `IHost`. En la versión 2. *x*, usa el `JobHost` objeto. Puede crear una instancia de host en su código y escribir código para personalizar su comportamiento.

Se trata de una diferencia clave entre utilizar el SDK de WebJobs directamente y utilizándolo indirectamente a través de Azure Functions. En Azure Functions, el servicio controla el host y el host no puede personalizar mediante la escritura de código. Las funciones de Azure le permite personalizar el comportamiento de host a través de la configuración en el archivo host.json. Los valores son cadenas, no código, y esto limita los tipos de personalizaciones que puede realizar.

### <a name="host-connection-strings"></a>Cadenas de conexión del host

El SDK de WebJobs busca cadenas de conexión de Azure Storage y Azure Service Bus en el archivo local.settings.json cuando se ejecuta localmente o en el entorno del WebJob cuando se ejecuta en Azure. De forma predeterminada, una conexión de almacenamiento de cadenas configuración denominada `AzureWebJobsStorage` es necesario.  

Versión 2. *x* del SDK permite utilizar sus propios nombres para estas cadenas de conexión o almacenarlas en otra parte. Puede establecer los nombres en el código mediante el [ `JobHostConfiguration` ], tal y como se muestra aquí:

```cs
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    //// Dashboard logging is deprecated; use Application Insights.
    //var _dashboardConn = ConfigurationManager
    //    .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    //config.DashboardConnectionString = _dashboardConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Dado que la versión 3. *x* usa la configuración de .NET Core predeterminada de las API, no hay ninguna API para cambiar los nombres de cadena de conexión.

### <a name="host-development-settings"></a>Configuración de desarrollo del host

Puede ejecutar el host en modo de desarrollo para aumentar la eficacia del desarrollo local. Estas son algunas de las opciones que cambian cuando se ejecuta en modo de desarrollo:

| Propiedad | Configuración de desarrollo |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` para maximizar la salida de registros. |
| `Queues.MaxPollingInterval`  | Un valor bajo para asegurar que los métodos de cola se activan inmediatamente.  |
| `Singleton.ListenerLockPeriod` | 15 segundos para ayudar en el desarrollo iterativo rápido. |

El proceso para habilitar el modo de desarrollo depende de la versión del SDK. 

#### <a name="version-3x"></a>Versión 3. *x*

Versión 3. *x* usa las API estándar de ASP.NET Core. Llame a la [ `UseEnvironment` ](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useenvironment) método en el [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder) instancia. Pasar una cadena denominada `development`, como en este ejemplo:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.UseEnvironment("development");
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

#### <a name="version-2x"></a>Versión 2. *x*

La clase `JobHostConfiguration` tiene un método `UseDevelopmentSettings` que permite el modo de desarrollo.  En el ejemplo siguiente se muestra cómo utilizar la configuración de desarrollo. Para realizar `config.IsDevelopment` devolver `true` cuando se ejecuta localmente, establezca una variable de entorno local denominada `AzureWebJobsEnv` con el valor `Development`.

```cs
static void Main()
{
    config = new JobHostConfiguration();

    if (config.IsDevelopment)
    {
        config.UseDevelopmentSettings();
    }

    var host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-servicepointmanager-settings"></a>Administración de conexiones simultáneas (versión 2. *x*)

En la versión 3. *x*, el valor predeterminado es el límite de conexiones para conexiones infinitas. Si por algún motivo tiene que cambiar este límite, puede usar el [ `MaxConnectionsPerServer` ](/dotnet/api/system.net.http.winhttphandler.maxconnectionsperserver) propiedad de la [ `WinHttpHandler` ](/dotnet/api/system.net.http.winhttphandler) clase.

En la versión 2. *x*, controlar el número de conexiones simultáneas a un host mediante la [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) API. En 2. *x*, debe aumentar este valor desde el valor predeterminado de 2 antes de iniciar el host de WebJobs.

Todas las solicitudes HTTP que se realice desde una función mediante el uso de `HttpClient` fluya a través de `ServicePointManager`. Después de alcanzar el valor establecido en `DefaultConnectionLimit`, `ServicePointManager` inicia las solicitudes de puesta en cola antes de enviarlos. Suponga que `DefaultConnectionLimit` se establece en 2 y que el código realiza 1000 solicitudes de HTTP. Inicialmente, solo se permiten dos solicitudes mediante el sistema operativo. Las otras 998 se ponen en cola hasta que haya espacio para ellas. Esto significa que su `HttpClient` es posible que el tiempo de espera porque parece que ha realizado la solicitud, pero nunca se envió la solicitud por el sistema operativo para el servidor de destino. Por lo tanto, es posible que vea cierto comportamiento que no parezca tener sentido: el `HttpClient` local tarda 10 segundos en completar una solicitud, pero el servicio devuelve todas las solicitudes en 200 ms. 

El valor predeterminado para las aplicaciones ASP.NET es `Int32.MaxValue`, y que es probable que funcione bien para WebJobs que se ejecuta en un básico o superior Plan de App Service. WebJobs normalmente necesita la configuración Always On, y que solo es compatible con básica y superiores planes de App Service.

Si WebJobs se ejecuta en un plan de App Service gratuito o compartido, un espacio aislado de App Service con un [límite de 300 conexiones](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits) se encargará de restringir la aplicación. Con un límite de conexiones independientes en `ServicePointManager`, es más probable que se alcance el umbral de espacio aislado de conexión y el sitio se apagará. En ese caso, reduzca el valor de `DefaultConnectionLimit` (use algo como 50 o 100) para evitar que esto suceda y permitir un nivel de rendimiento suficiente.

La configuración debe establecerse antes de que se realice cualquier solicitud de HTTP. Por este motivo, el host de WebJobs no debe ajustar la configuración automáticamente. Podría haber solicitudes HTTP que se producen antes de iniciar el host, lo que puede provocar un comportamiento inesperado. Es el mejor enfoque establecer el valor inmediatamente en su `Main` método antes de inicializar `JobHost`, tal y como se muestra aquí:

```csharp
static void Main(string[] args)
{
    // Set this immediately so that it's used by all requests.
    ServicePointManager.DefaultConnectionLimit = Int32.MaxValue;

    var host = new JobHost();
    host.RunAndBlock();
}
```

## <a name="triggers"></a>Desencadenadores

Las funciones deben ser métodos públicos y debe tener un atributo de desencadenador o la [ `NoAutomaticTrigger` ](#manual-triggers) atributo.

### <a name="automatic-triggers"></a>Desencadenadores automáticos

Los desencadenadores automáticos llaman a una función en respuesta a un evento. Considere este ejemplo de una función que se desencadena mediante un mensaje agregado a Azure Queue storage. Responde al leer un blob de almacenamiento de blobs de Azure:

```cs
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{myQueueItem}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

El `QueueTrigger` atributo indica el tiempo de ejecución para llamar a la función cada vez que aparezca un mensaje en cola en el `myqueue-items` cola. El `Blob` atributo indica al runtime que utilice el mensaje de cola para leer un blob en el *elementos de trabajo de ejemplo* contenedor. El contenido del mensaje en cola, pasa a la función en el `myQueueItem` parámetro, es el nombre del blob.


### <a name="manual-triggers"></a>Desencadenadores manuales

Para desencadenar una función manualmente, use el `NoAutomaticTrigger` de atributo, como se muestra aquí:

```cs
[NoAutomaticTrigger]
public static void CreateQueueMessage(
ILogger logger,
string value,
[Queue("outputqueue")] out string message)
{
    message = value;
    logger.LogInformation("Creating queue message: ", message);
}
```

El proceso para desencadenar manualmente la función depende de la versión del SDK.

#### <a name="version-3x"></a>Versión 3. *x*

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddAzureStorage();
    });
    var host = builder.Build();
    using (host)
    {
        var jobHost = host.Services.GetService(typeof(IJobHost)) as JobHost;
        var inputs = new Dictionary<string, object>
        {
            { "value", "Hello world!" }
        };

        await host.StartAsync();
        await jobHost.CallAsync("CreateQueueMessage", inputs);
        await host.StopAsync();
    }
}
```

#### <a name="version-2x"></a>Versión 2. *x*

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

## <a name="input-and-output-bindings"></a>Enlaces de entrada y de salida

Los enlaces de entrada proporcionan una forma declarativa de hacer que los datos de servicios de Azure o de terceros estén disponibles para su código. Los enlaces de salida, a su vez, proporcionan una manera de actualizar los datos. El [comenzar](webjobs-sdk-get-started.md) artículo muestra un ejemplo de cada uno.

Puede usar un valor devuelto del método para un enlace de salida aplicando el atributo para el valor devuelto del método. Vea el ejemplo de [mediante la función de Azure devuelven el valor](../azure-functions/functions-bindings-return-value.md).

## <a name="binding-types"></a>Tipos de enlaces

El proceso para instalar y administrar tipos de enlace depende de si está usando la versión 3. *x* o versión 2. *x* del SDK. Puede encontrar el paquete de instalación para un tipo de enlace concreto en la sección "Paquetes" de Azure Functions de ese tipo de enlace [artículo de referencia](#binding-reference-information). Una excepción es el desencadenador de archivos y el enlace (para el sistema de archivos local), que no es compatible con Azure Functions.

#### <a name="version-3x"></a>Versión 3. *x*

En la versión 3. *x*, los enlaces de almacenamiento se incluyen en el `Microsoft.Azure.WebJobs.Extensions.Storage` paquete. Llame a la `AddAzureStorage` método de extensión en el `ConfigureWebJobs` método, como se muestra aquí:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddAzureStorage();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

Para usar otros tipos de enlaces y desencadenadores, instale el paquete NuGet que los contenga y llame al método de extensión `Add<binding>` implementado en la extensión. Por ejemplo, si desea utilizar un enlace de Azure Cosmos DB, instalar `Microsoft.Azure.WebJobs.Extensions.CosmosDB` y llamar a `AddCosmosDB`, similar al siguiente:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddCosmosDB();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

Para usar el desencadenador Timer o el enlace Files, que forman parte de los servicios principales, llame a los métodos de extensión `AddTimers` o `AddFiles`, respectivamente.

#### <a name="version-2x"></a>Versión 2. *x*

Estos tipos de desencadenador y enlace se incluyen en la versión 2. *x* de la `Microsoft.Azure.WebJobs` paquete:

* Almacenamiento de blobs
* Queue Storage
* Almacenamiento de tablas

Para usar otros tipos de enlaces y desencadenadores, instale el paquete de NuGet que los contenga y llame a un método en `Use<binding>` el objeto `JobHostConfiguration`. Por ejemplo, si desea usar un desencadenador de temporizador, instale `Microsoft.Azure.WebJobs.Extensions` y llamar a `UseTimers` en el `Main` método, como se muestra aquí:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

Para usar el enlace Files, instale `Microsoft.Azure.WebJobs.Extensions` y llame a `UseFiles`.

### <a name="executioncontext"></a>ExecutionContext

WebJobs permite enlazar con un objeto [`ExecutionContext`]. Con este enlace, puede acceder a [`ExecutionContext`] como parámetro en la signatura de función. Por ejemplo, el siguiente código usa el objeto de contexto para acceder al identificador de invocación, que puede usar para correlacionar todos los registros generados por la invocación de una función específica.  

```cs
public class Functions
{
    public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        ExecutionContext executionContext,
        ILogger logger)
    {
        logger.LogInformation($"{message}\n{executionContext.InvocationId}");
    }
}
```

El proceso para el enlace a la [ `ExecutionContext` ] depende de la versión del SDK.

#### <a name="version-3x"></a>Versión 3. *x*

Llame a la `AddExecutionContextBinding` método de extensión en el `ConfigureWebJobs` método, como se muestra aquí:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddExecutionContextBinding();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

#### <a name="version-2x"></a>Versión 2. *x*

El paquete `Microsoft.Azure.WebJobs.Extensions` que se ha mencionado anteriormente también proporciona un tipo de enlace especial que se puede registrar mediante una llamada al método `UseCore`. Este enlace le permite definir un [ `ExecutionContext` ] parámetro en la firma de función, que está habilitada de forma similar al siguiente:

```cs
class Program
{
    static void Main()
    {
        config = new JobHostConfiguration();
        config.UseCore();
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
```

## <a name="binding-configuration"></a>Configuración de enlace

Puede configurar el comportamiento de algunos enlaces y desencadenadores. El proceso para configurarlas depende de la versión del SDK.

* **Versión 3. *x*:** Configuración del conjunto cuando el `Add<Binding>` se llama al método `ConfigureWebJobs`.
* **Versión 2. *x*:** Configuración del conjunto estableciendo propiedades en un objeto de configuración que se pasa a `JobHost`.

Estos valores específicos de enlace son equivalentes en la configuración de la [archivo de proyecto host.json](../azure-functions/functions-host-json.md) en Azure Functions.

Puede configurar los siguientes enlaces:

* [Desencadenador de Azure COSMOS DB](#azure-cosmosdb-trigger-configuration-version-3x)
* [Desencadenador de Event Hubs](#event-hubs-trigger-configuration-version-3x)
* Desencadenador de Queue Storage
* [Enlace de SendGrid](#sendgrid-binding-configuration-version-3x)
* [Desencadenador de Service Bus](#service-bus-trigger-configuration-version-3x)

### <a name="azure-cosmosdb-trigger-configuration-version-3x"></a>Configuración de desencadenador de Azure COSMOS DB (versión 3. *x*)

En este ejemplo se muestra cómo configurar el desencadenador de Azure Cosmos DB:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddCosmosDB(a =>
        {
            a.ConnectionMode = ConnectionMode.Gateway;
            a.Protocol = Protocol.Https;
            a.LeaseOptions.LeasePrefix = "prefix1";

        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Para obtener más información, consulte el [enlace de Azure COSMOS DB](../azure-functions/functions-bindings-cosmosdb-v2.md#hostjson-settings) artículo.

### <a name="event-hubs-trigger-configuration-version-3x"></a>Configuración de desencadenador de Event Hubs (versión 3. *x*)

En este ejemplo se muestra cómo configurar el desencadenador de Event Hubs:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddEventHubs(a =>
        {
            a.BatchCheckpointFrequency = 5;
            a.EventProcessorOptions.MaxBatchSize = 256;
            a.EventProcessorOptions.PrefetchCount = 512;
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Para obtener más información, consulte el [enlace Event Hubs](../azure-functions/functions-bindings-event-hubs.md#hostjson-settings) artículo.

### <a name="queue-storage-trigger-configuration"></a>Configuración de desencadenador de Queue storage

Estos ejemplos muestran cómo configurar el desencadenador de Queue storage:

#### <a name="version-3x"></a>Versión 3. *x*

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddAzureStorage(a => {
            a.BatchSize = 8;
            a.NewBatchThreshold = 4;
            a.MaxDequeueCount = 4;
            a.MaxPollingInterval = TimeSpan.FromSeconds(15);
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Para obtener más información, consulte el [enlace de almacenamiento en cola](../azure-functions/functions-bindings-storage-queue.md#hostjson-settings) artículo.

#### <a name="version-2x"></a>Versión 2. *x*

```cs
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.NewBatchThreshold = 4;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Para obtener más información, consulte el [referencia de host.json v1.x](../azure-functions/functions-host-json-v1.md#queues).

### <a name="sendgrid-binding-configuration-version-3x"></a>Configuración de enlace de SendGrid (versión 3. *x*)

En este ejemplo se muestra cómo configurar el SendGrid enlace de salida:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddSendGrid(a =>
        {
            a.FromAddress.Email = "samples@functions.com";
            a.FromAddress.Name = "Azure Functions";
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Para obtener más información, consulte el [SendGrid enlace](../azure-functions/functions-bindings-sendgrid.md#hostjson-settings) artículo.

### <a name="service-bus-trigger-configuration-version-3x"></a>Configuración de desencadenador de Service Bus (versión 3. *x*)

En este ejemplo se muestra cómo configurar el desencadenador de Service Bus:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddServiceBus(sbOptions =>
        {
            sbOptions.MessageHandlerOptions.AutoComplete = true;
            sbOptions.MessageHandlerOptions.MaxConcurrentCalls = 16;
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Para obtener más información, consulte el [enlace de Service Bus](../azure-functions/functions-bindings-service-bus.md#hostjson-settings) artículo.

### <a name="configuration-for-other-bindings"></a>Configuración de otros enlaces

Algunos tipos de desencadenador y enlace definen sus propios tipos de configuración personalizados. Por ejemplo, el desencadenador de archivo le permite especificar la ruta de acceso raíz para supervisar, como en estos ejemplos:

#### <a name="version-3x"></a>Versión 3. *x*

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddFiles(a => a.RootPath = @"c:\data\import");
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

#### <a name="version-2x"></a>Versión 2. *x*

```cs
static void Main()
{
    config = new JobHostConfiguration();
    var filesConfig = new FilesConfiguration
    {
        RootPath = @"c:\data\import"
    };
    config.UseFiles(filesConfig);
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

## <a name="binding-expressions"></a>Expresiones de enlace

En los parámetros de constructor del atributo, puede utilizar expresiones que dan como resultado valores procedentes de diversos orígenes. Por ejemplo, en el código siguiente, la ruta de acceso del atributo `BlobTrigger` crea una expresión con el nombre `filename`. Cuando se utiliza para el enlace de salida, `filename` se resuelve como el nombre del blob desencadenador.

```cs
public static void CreateThumbnail(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger logger)
{
    logger.Info($"Blob trigger processing: {filename}");
    // ...
}
```

Para obtener más información acerca de las expresiones de enlace, consulte [Patrones y expresiones de enlace](../azure-functions/functions-bindings-expressions-patterns.md) en la documentación de Azure Functions.

### <a name="custom-binding-expressions"></a>Expresiones de enlace personalizadas

A veces desea especificar un nombre de cola, un nombre de blob o contenedor o un nombre de tabla en el código en lugar de codificarla de forma. Por ejemplo, es posible que desee especificar el nombre de la cola del atributo `QueueTrigger` en un archivo de configuración o en una variable de entorno.

Puede hacerlo pasando un `NameResolver` objeto en el `JobHostConfiguration` objeto. Incluya marcadores de posición en los parámetros del constructor de atributos de desencadenador o de enlace, y el código `NameResolver` le proporcionará los valores reales que se utilizarán en lugar de esos marcadores de posición. Identificar los marcadores de posición enmarcándolos con porcentaje (%) signos, como se muestra aquí:

```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Este código le permite usar una cola llamada `logqueuetest` en el entorno de prueba y otra llamada `logqueueprod` en producción. En lugar de codificar de forma rígida un nombre de cola, es preferible especificar el nombre de una entrada en la colección `appSettings`.

No hay valor predeterminado es `NameResolver` que surte efecto si no se proporciona uno personalizado. El valor predeterminado obtiene valores de configuración de la aplicación o de las variables de entorno.

Su `NameResolver` clase obtiene el nombre de cola de `appSettings`, tal y como se muestra aquí:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

#### <a name="version-3x"></a>Versión 3. *x*

Configurar a la resolución mediante la inyección de dependencia. En estos ejemplos se requiere la siguiente instrucción `using`:

```cs
using Microsoft.Extensions.DependencyInjection;
```

Agregar la resolución mediante una llamada a la [ `ConfigureServices` ] método de extensión en [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder), como en este ejemplo:

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    var resolver = new CustomNameResolver();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureServices(s => s.AddSingleton<INameResolver>(resolver));
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Versión 2. *x*

Pasar su `NameResolver` clase a la `JobHost` de objeto, como se muestra aquí:

```cs
 static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new CustomNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Azure Functions implementa `INameResolver` para obtener los valores de configuración de la aplicación, tal como se muestra en el ejemplo. Cuando se usa el SDK de WebJobs directamente, puede escribir una implementación personalizada que obtiene los valores de reemplazo del marcador de posición de cualquier origen que prefiera.

## <a name="binding-at-runtime"></a>Enlace en tiempo de ejecución

Si necesita realizar algún trabajo en la función antes de utilizar un atributo de enlace como `Queue`, `Blob`, o `Table`, puede usar el `IBinder` interfaz.

En el siguiente ejemplo se toma un mensaje de la cola de entrada y se crea un mensaje nuevo con el mismo contenido en una cola de salida. El nombre de la cola de salida se establece por el código en el cuerpo de la función.

```cs
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessageAsync(new CloudQueueMessage(queueMessage));
}
```

Para obtener más información, consulte [Enlace en tiempo de ejecución](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) en la documentación de Azure Functions.

## <a name="binding-reference-information"></a>Información de referencia de enlace

La documentación de Azure Functions proporciona información de referencia sobre cada tipo de enlace. Encontrará la información siguiente en cada artículo de referencia de enlace. (En este ejemplo se basa en la cola de almacenamiento).

* [Paquetes](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x). El paquete que debe instalar para incluir compatibilidad con el enlace en un proyecto de SDK de WebJobs.
* [Ejemplos](../azure-functions/functions-bindings-storage-queue.md#trigger---example). Ejemplos de código. El C# ejemplo de la biblioteca de clase se aplica al SDK de WebJobs. Simplemente omita la `FunctionName` atributo.
* [Atributos](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes). Los atributos que se usará para el tipo de enlace.
* [Configuración](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration). Explicaciones de las propiedades de los atributos y los parámetros del constructor.
* [Uso](../azure-functions/functions-bindings-storage-queue.md#trigger---usage). Los tipos a que puede enlazar y la información sobre cómo funciona el enlace. Por ejemplo: sondear algoritmos o procesar colas dudosas.
  
Para obtener una lista de artículos de referencia de enlace, vea "Enlaces compatibles" en el [desencadenadores y enlaces](../azure-functions/functions-triggers-bindings.md#supported-bindings) artículo para Azure Functions. En la lista, los enlaces HTTP, Webhook y Event Grid son compatibles únicamente con Azure Functions, no por el SDK de WebJobs.

## <a name="disable-attribute"></a>Atributo Disable 

El [ `Disable` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) atributo le permite controlar si una función puede activarse. 

En el ejemplo siguiente, si la configuración de aplicación `Disable_TestJob` tiene un valor de `1` o `True` (distingue entre mayúsculas y minúsculas), no ejecute la función. En ese caso, el tiempo de ejecución crea el mensaje de registro *La función "Functions.TestJob" está deshabilitada*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Al cambiar los valores de configuración de aplicación en Azure portal, se reinicia el trabajo Web para recoger la nueva configuración.

El atributo se puede declarar en el nivel de clase, método o parámetro. El nombre de la configuración también puede contener expresiones de enlace.

## <a name="timeout-attribute"></a>Atributo Timeout

El [ `Timeout` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) atributo hace que una función se cancele si no finaliza dentro de un período de tiempo especificado. En el ejemplo siguiente, la función se ejecuta durante un día sin el atributo de tiempo de espera. Tiempo de espera hace que la función se cancela después de 15 segundos.

```cs
[Timeout("00:00:15")]
public static async Task TimeoutJob(
    [QueueTrigger("testqueue2")] string message,
    CancellationToken token,
    TextWriter log)
{
    await log.WriteLineAsync("Job starting");
    await Task.Delay(TimeSpan.FromDays(1), token);
    await log.WriteLineAsync("Job completed");
}
```

Puede aplicar el atributo de tiempo de espera en el nivel de clase o método, y puede especificar un tiempo de espera global mediante `JobHostConfiguration.FunctionTimeout`. Los tiempos de espera de nivel de método o clase invalidan los tiempos de espera global.

## <a name="singleton-attribute"></a>Atributo Singleton

El [ `Singleton` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) atributo garantiza que solo una instancia de una función se ejecuta, incluso cuando hay varias instancias de la aplicación web de host. Para ello, utiliza [bloqueo distribuido](#viewing-lease-blobs).

En este ejemplo, una sola instancia de la `ProcessImage` la función se ejecuta en un momento dado:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image.
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Algunos de los desencadenadores tienen compatibilidad integrada para poder administrar la simultaneidad:

* **QueueTrigger**. Establezca `JobHostConfiguration.Queues.BatchSize` en `1`.
* **ServiceBusTrigger**. Establezca `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` en `1`.
* **FileTrigger**. Establezca `FileProcessor.MaxDegreeOfParallelism` en `1`.

Puede usar esta configuración para asegurarse de que la función se ejecuta como una singleton en una sola instancia. Para asegurarse de que se está ejecutando una sola instancia de la función cuando la aplicación web se escala horizontalmente a varias instancias, aplique un bloqueo de nivel de escucha singleton en la función (`[Singleton(Mode = SingletonMode.Listener)]`). Los bloqueos de escucha se adquieren cuando se inicia el objeto JobHost. Si tres instancias de escalado horizontal se inician al mismo tiempo, solo una de ellas adquiere el bloqueo y solo un agente de escucha se inicia.

### <a name="scope-values"></a>Valores de ámbito

Puede especificar un *expresión/valor de ámbito* en un singleton. El valor de expresión garantiza que se va a serializar todas las ejecuciones de la función en un ámbito específico. Implementación de bloqueo más granular de esta manera puede permitir cierto grado de paralelismo de la función durante la serialización de las demás invocaciones dictado por sus requisitos. Por ejemplo, en el código siguiente, la expresión scope enlaza a la `Region` valor del mensaje entrante. Cuando la cola contiene tres mensajes en las regiones oriental, este y oeste, respectivamente, los mensajes que tengan la región oriental se ejecutan en serie mientras el mensaje con la región occidental que se ejecuta en paralelo con los de Asia.

```csharp
[Singleton("{Region}")]
public static async Task ProcessWorkItem([QueueTrigger("workitems")] WorkItem workItem)
{
     // Process the work item.
}

public class WorkItem
{
     public int ID { get; set; }
     public string Region { get; set; }
     public int Category { get; set; }
     public string Description { get; set; }
}
```

### <a name="singletonscopehost"></a>SingletonScope.Host

Es el ámbito predeterminado para un bloqueo `SingletonScope.Function`, lo que significa que el ámbito del bloqueo (la ruta de concesión del blob) está asociado al nombre de función completo. Para bloquear las funciones, especifique `SingletonScope.Host` y usar un nombre de Id. de ámbito es el mismo en todas las funciones que no desea ejecutar al mismo tiempo. En el ejemplo siguiente, solo una instancia de `AddItem` o de `RemoveItem` se ejecuta a la vez:

```csharp
[Singleton("ItemsLock", SingletonScope.Host)]
public static void AddItem([QueueTrigger("add-item")] string message)
{
     // Perform the add operation.
}

[Singleton("ItemsLock", SingletonScope.Host)]
public static void RemoveItem([QueueTrigger("remove-item")] string message)
{
     // Perform the remove operation.
}
```

### <a name="viewing-lease-blobs"></a>Ver blobs de concesión

El SDK de WebJobs usa las [concesiones de blobs de Azure](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) en segundo plano para poder implementar el bloqueo distribuido. Los blobs de concesión que usa Singleton se pueden encontrar en el `azure-webjobs-host` contenedor en el `AzureWebJobsStorage` cuenta de almacenamiento en la ruta de acceso "bloqueos". Por ejemplo, la ruta de acceso del blob de concesión que se indicó en el primer ejemplo `ProcessImage` mostrado, puede ser `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Todas las rutas de acceso incluyen el identificador de JobHost, que en este caso es 061851c758f04938a4426aa9ab3869c0.

## <a name="async-functions"></a>Funciones asincrónicas

Para obtener información acerca de cómo programar las funciones asincrónicas, vea el [documentación de Azure Functions](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>Tokens de cancelación

Para obtener información sobre cómo controlar los tokens de cancelación, consulte la documentación de Azure Functions acerca de los [tokens de cancelación y el apagado estable](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Varias instancias

Si la aplicación web se ejecuta en varias instancias, un trabajo de WebJobs continuo se ejecuta en cada instancia, en la que escuchará a los desencadenadores e intentará llamar a las funciones. Los distintos enlaces de desencadenador están diseñados para compartir el trabajo de forma eficiente y colaborar en todas las instancias, por lo que el escalado horizontal de varias instancias le permitirá administrar más carga.

Los desencadenadores de cola y blob automática impedir que una función procese un mensaje de cola o blob varias veces; las funciones no tienen que ser idempotentes.

El desencadenador del temporizador garantiza automáticamente que solo una instancia del temporizador se ejecute, por lo que no se ejecutará más de una instancia de función en un momento programado.

Si desea asegurarse de que solo una instancia de una función se ejecuta incluso cuando hay varias instancias de la aplicación web de host, puede usar el [ `Singleton` ](#singleton-attribute) atributo.

## <a name="filters"></a>Filtros

Los filtros de función (versión preliminar) proporcionan una manera de personalizar la canalización de la ejecución de WebJobs con su propia lógica. Los filtros son similares a [filtros de ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). Se puede implementar como atributos declarativos que se aplican a las funciones o clases. Para obtener más información, consulte [Filtros de función](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Registro y supervisión

Se recomienda la plataforma de registro que se desarrolló para ASP.NET. El [comenzar](webjobs-sdk-get-started.md) artículo muestra cómo se usa. 

### <a name="log-filtering"></a>Filtrado de registros

Cada registro que creó una instancia `ILogger` tiene asociados los valores `Category` y `Level`. [`LogLevel`](/dotnet/api/microsoft.extensions.logging.loglevel) es una enumeración y el código entero indica la importancia relativa:

|LogLevel    |Código|
|------------|---|
|Seguimiento       | 0 |
|Depurar       | 1 |
|Información | 2 |
|Advertencia     | 3 |
|Error       | 4 |
|Crítico    | 5 |
|None        | 6 |

Puede filtrar por separado cada categoría para una determinada [ `LogLevel` ](/dotnet/api/microsoft.extensions.logging.loglevel). Por ejemplo, es posible que quiera ver todos los registros referentes al procesamiento del desencadenador de blobs, pero solo el valor `Error` y superiores para todo lo demás.

#### <a name="version-3x"></a>Versión 3. *x*

Versión 3. *x* del SDK se basa en el filtrado que se integran en .NET Core. La clase `LogCategories` le permite definir las categorías de funciones, desencadenadores o usuarios específicos. También define los filtros para los Estados de host específico, como `Startup` y `Results`. Esto le permite ajustar la salida del registro. Si no se encuentra ninguna coincidencia en las categorías definidas, el filtro volverá a tener el valor `Default` al decidir si filtrar el mensaje.

`LogCategories` requiere la siguiente instrucción using:

```cs
using Microsoft.Azure.WebJobs.Logging; 
```

El ejemplo siguiente crea un filtro que, de forma predeterminada, filtra todos los registros en el `Warning` nivel. El `Function` y `results` categorías (equivalente a `Host.Results` en la versión 2. *x*) se filtran en la `Error` nivel. El filtro compara la categoría actual con todos los niveles registrados en la instancia `LogCategories` y elige la coincidencia más larga. Esto significa que el `Debug` nivel registrado para `Host.Triggers` coincide con `Host.Triggers.Queue` o `Host.Triggers.Blob`. Esto le permite controlar categorías más amplias sin necesidad de agregar cada valor.

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureLogging(logging =>
            {
                logging.SetMinimumLevel(LogLevel.Warning);
                logging.AddFilter("Function", LogLevel.Error);
                logging.AddFilter(LogCategories.CreateFunctionCategory("MySpecificFunctionName"),
                    LogLevel.Debug);
                logging.AddFilter(LogCategories.Results, LogLevel.Error);
                logging.AddFilter("Host.Triggers", LogLevel.Debug);
            });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Versión 2. *x*

En la versión 2. *x* del SDK, usa el `LogCategoryFilter` clase para controlar el filtrado. El `LogCategoryFilter` tiene un `Default` propiedad con un valor inicial de `Information`, lo que significa que los mensajes en el `Information`, `Warning`, `Error`, o `Critical` niveles se registran, pero los mensajes en el `Debug` o `Trace` niveles se filtran inmediatamente.

Igual que con `LogCategories` en la versión 3. *x*, el `CategoryLevels` propiedad le permite especificar los niveles de registro para categorías específicas de modo que puede ajustar la salida del registro. Si no encuentra ninguna coincidencia en el diccionario `CategoryLevels`, el filtro volverá a tener el valor `Default` cuando decida filtrar el mensaje.

En el ejemplo siguiente se crea un filtro que filtra todos los registros del nivel `Warning` de forma predeterminada. El `Function` y `Host.Results` categorías se filtran en la `Error` nivel. `LogCategoryFilter` compara la categoría actual con todos los valores `CategoryLevels` registrado y elige la coincidencia más larga. Por lo que la `Debug` nivel registrado para `Host.Triggers` coincidirá con `Host.Triggers.Queue` o `Host.Triggers.Blob`. Esto le permite controlar categorías más amplias sin necesidad de agregar cada valor.

```csharp
var filter = new LogCategoryFilter();
filter.DefaultLevel = LogLevel.Warning;
filter.CategoryLevels[LogCategories.Function] = LogLevel.Error;
filter.CategoryLevels[LogCategories.Results] = LogLevel.Error;
filter.CategoryLevels["Host.Triggers"] = LogLevel.Debug;

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(instrumentationKey, filter.Filter)
    .AddConsole(filter.Filter);
```

### <a name="custom-telemetry-for-application-insights"></a>Telemetría personalizada de Application Insights​

El proceso para implementar telemetría personalizada para [Application Insights](../azure-monitor/app/app-insights-overview.md) depende de la versión del SDK. Para saber cómo configurar Application Insights, consulte [Incorporación del registro de Application Insights](webjobs-sdk-get-started.md#add-application-insights-logging).

#### <a name="version-3x"></a>Versión 3. *x*

Dado que la versión 3. *x* del SDK de WebJobs se basa en .NET Core ya no se proporciona el host genérico, una factoría de datos de telemetría personalizados. Pero puede agregar telemetría personalizada a la canalización mediante la inyección de dependencia. En los ejemplos de esta sección se requieren las siguientes instrucciones `using`:

```cs
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.Channel;
```

La siguiente implementación personalizada de [`ITelemetryInitializer`] le permite agregar su propio valor de [`ITelemetry`](/dotnet/api/microsoft.applicationinsights.channel.itelemetry) al valor predeterminado de [`TelemetryConfiguration`].

```cs
internal class CustomTelemetryInitializer : ITelemetryInitializer
{
    public void Initialize(ITelemetry telemetry)
    {
        // Do something with telemetry.
    }
}
```

Llame a [`ConfigureServices`] en el compilador para agregar su valor de [`ITelemetryInitializer`] personalizado a la canalización.

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureLogging((context, b) =>
    {
        // Add logging providers.
        b.AddConsole();

        // If this key exists in any config, use it to enable Application Insights.
        string appInsightsKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
        if (!string.IsNullOrEmpty(appInsightsKey))
        {
            // This uses the options callback to explicitly set the instrumentation key.
            b.AddApplicationInsights(o => o.InstrumentationKey = appInsightsKey);
        }
    });
    builder.ConfigureServices(services =>
        {
            services.AddSingleton<ITelemetryInitializer, CustomTelemetryInitializer>();
        });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Cuando se construye [`TelemetryConfiguration`], se incluyen todos los tipos registrados de [`ITelemetryInitializer`]. Para obtener más información, consulte [API de Application Insights para eventos y métricas personalizados](../azure-monitor/app/api-custom-events-metrics.md).

En la versión 3. *x*, ya no debe vaciar la [ `TelemetryClient` ] cuando se detenga el host. El sistema de inserción de dependencias de .NET Core desecha automáticamente el elemento `ApplicationInsightsLoggerProvider` registrado, que vacía [`TelemetryClient`].

#### <a name="version-2x"></a>Versión 2. *x*

En la versión 2. *x*, [ `TelemetryClient` ] creó internamente el proveedor de Application Insights para el SDK de WebJobs usa [ `ServerTelemetryChannel` ](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Cuando el punto de conexión de Application Insights no está disponible o limita las solicitudes entrantes, este canal [guarda las solicitudes en el sistema de archivos de la aplicación web y vuelve a enviarlas más adelante](https://apmtips.com/blog/2015/09/03/more-telemetry-channels).

El elemento [`TelemetryClient`] se crea mediante una clase que implementa `ITelemetryClientFactory`. De manera predeterminada, será [`DefaultTelemetryClientFactory`](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs).

Si quiere modificar alguna parte de la canalización de Application Insights, puede proporcionar su propio elemento `ITelemetryClientFactory`, y el host usará la clase para construir un elemento [`TelemetryClient`]. Por ejemplo, este código invalida `DefaultTelemetryClientFactory` para modificar una propiedad de `ServerTelemetryChannel`:

```csharp
private class CustomTelemetryClientFactory : DefaultTelemetryClientFactory
{
    public CustomTelemetryClientFactory(string instrumentationKey, Func<string, LogLevel, bool> filter)
        : base(instrumentationKey, new SamplingPercentageEstimatorSettings(), filter)
    {
    }

    protected override ITelemetryChannel CreateTelemetryChannel()
    {
        ServerTelemetryChannel channel = new ServerTelemetryChannel();

        // Change the default from 30 seconds to 15 seconds.
        channel.MaxTelemetryBufferDelay = TimeSpan.FromSeconds(15);

        return channel;
    }
}
```

El `SamplingPercentageEstimatorSettings` objeto configura [muestreo adaptable](https://docs.microsoft.com/azure/application-insights/app-insights-sampling). Esto significa que, en ciertos escenarios de gran volumen, Application Insights envía un subconjunto seleccionado de los datos de telemetría al servidor.

Después de crear la factoría de datos de telemetría, pasa en el proveedor de registro de Application Insights:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Pasos siguientes

Este artículo proporciona fragmentos de código que muestran cómo tratar escenarios comunes para trabajar con el SDK de WebJobs. Para obtener ejemplos completos, consulte [azure-webjobs-sdk-samples](https://github.com/Azure/azure-webjobs-sdk-samples).

["ExecutionContext"]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs
["TelemetryClient"]: /dotnet/api/microsoft.applicationinsights.telemetryclient
["ConfigureServices"]: /dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices
["ITelemetryInitializer"]: /dotnet/api/microsoft.applicationinsights.extensibility.itelemetryinitializer
["TelemetryConfiguration"]: /dotnet/api/microsoft.applicationinsights.extensibility.telemetryconfiguration
[`JobHostConfiguration`]: https://github.com/Azure/azure-webjobs-sdk/blob/v2.x/src/Microsoft.Azure.WebJobs.Host/JobHostConfiguration.cs
