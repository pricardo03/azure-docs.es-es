---
title: 'Uso del SDK de WebJobs: Azure'
description: Obtenga más información sobre cómo escribir código para el SDK de WebJobs. Cree trabajos de procesamiento en segundo plano basados en eventos que tengan acceso a los datos de los servicios de Azure y de terceros.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/19/2019
ms.author: glenga
ms.openlocfilehash: ab502c25a632977065e55d2eeafd684203636b14
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109918"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Uso del SDK de Azure WebJobs para el procesamiento en segundo plano basado en eventos

En este artículo se proporciona orientación sobre cómo escribir código para el [SDK de Azure WebJobs](webjobs-sdk-get-started.md). La documentación se aplica a ambas versiones, 3.x y 2.x, del SDK de WebJobs. Si existen diferencias de API, se proporcionan ejemplos de ambos. El cambio principal que presenta la versión 3.x es el uso de .NET Core en lugar de .NET Framework.

>[!NOTE]
> [Azure Functions](../azure-functions/functions-overview.md) se integra en el SDK de WebJobs, así que si quiere ver algún tema relacionado, este artículo contiene vínculos a la documentación de Azure Functions. A continuación se indican las diferencias entre Functions y el SDK de WebJobs:
> * La versión 2.x de Azure Functions corresponde a la versión 3.x del SDK de WebJobs, y Azure Functions 1.x corresponde al SDK de WebJobs 2.x. Los repositorios de código fuente siguen la numeración del SDK de WebJobs.
> * El código de ejemplo de las bibliotecas de clases de C# de Azure Functions es como el código del SDK de WebJobs, salvo que no es necesario tener un atributo `FunctionName` en un proyecto de SDK de WebJobs.
> * Algunos tipos de enlace solo se admiten en Functions, como HTTP, webhook y Event Grid (que se basa en HTTP).
> 
> Para obtener más información, consulte [Compare the WebJobs SDK and Azure Functions](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs) (Comparación del SDK de WebJobs con Azure Functions).

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por supuesto que ha leído y completado las tareas de [Introducción al SDK de WebJobs](webjobs-sdk-get-started.md).

## <a name="webjobs-host"></a>Host de WebJobs

El host es un contenedor en tiempo de ejecución de las funciones.  Realiza escuchas de desencadenadores y llamadas de función. En la versión 3.x, el host es una implementación de `IHost` y, en la versión 2.x, se usará el objeto `JobHost`. Puede crear una instancia de host en su código y escribir código para personalizar su comportamiento.

Esta es una diferencia clave entre lo que supone usar directamente el SDK de WebJobs o usarlo de forma indirecta mediante Azure Functions. En Azure Functions, el servicio controla el host, y no se puede personalizar mediante la escritura de código. Azure Functions le permite personalizar el comportamiento del host a través de la configuración del archivo *host.json*. Esos valores de configuración son cadenas y no código, lo que limita los tipos de personalizaciones que puede realizar.

### <a name="host-connection-strings"></a>Cadenas de conexión del host 

El SDK de WebJobs busca cadenas de conexión de Azure Storage y Azure Service Bus en el archivo *local.settings.json* cuando se ejecuta de forma local, o en el entorno de WebJobs cuando se ejecuta en Azure. De forma predeterminada, se requiere un valor de cadena de conexión llamado `AzureWebJobsStorage`.  

La versión 2.x del SDK le permite usar sus propios nombres para estas cadenas de conexión o almacenarlas en otra parte. Puede establecerlas en el código, como aquí se muestra:

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

Como se usan las API de la configuración predeterminada de .NET Core, no hay ninguna API de la versión 3.x para cambiar los nombres de cadena de conexión.

### <a name="host-development-settings"></a>Configuración de desarrollo del host

Puede ejecutar el host en modo de desarrollo para aumentar la eficacia del desarrollo local. Estos son algunos de los valores de configuración que se cambian cuando se ejecuta en modo de desarrollo:

| Propiedad | Configuración de desarrollo |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` para maximizar la salida de registros. |
| `Queues.MaxPollingInterval`  | Un valor bajo para asegurar que los métodos de cola se activan inmediatamente.  |
| `Singleton.ListenerLockPeriod` | 15 segundos para ayudar en el desarrollo iterativo rápido. |

La forma de habilitar el modo de desarrollo depende de la versión del SDK. 

#### <a name="version-3x"></a>Versión 3.x

La versión 3.x usa las API estándar de ASP.NET Core. Llame al método [UseEnvironment](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useenvironment) en la instancia [`HostBuilder`](/dotnet/api/microsoft.extensions.hosting.hostbuilder). Pase una cadena denominada `development`, como en el ejemplo siguiente:

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

#### <a name="version-2x"></a>Versión 2.x

La clase `JobHostConfiguration` tiene un método `UseDevelopmentSettings` que permite el modo de desarrollo.  En el ejemplo siguiente se muestra cómo utilizar la configuración de desarrollo. Para hacer que `config.IsDevelopment` devuelva `true` cuando se ejecuta localmente, debe establecer una variable de entorno local denominada `AzureWebJobsEnv` con un valor `Development`.

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

### <a name="jobhost-servicepointmanager-settings"></a>Administración de conexiones simultáneas (v2.x)

En la versión 3.x, el límite de conexiones tiene como valor predeterminado un número infinito. Si por algún motivo tiene que cambiar este límite, puede usar la propiedad [MaxConnectionsPerServer](/dotnet/api/system.net.http.winhttphandler.maxconnectionsperserver) de la clase [WinHttpHander](/dotnet/api/system.net.http.winhttphandler).

En la versión 2.x, el número de conexiones simultáneas a un host se controla mediante la API [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit). En 2.x, se recomienda aumentar el valor predeterminado de 2 antes de iniciar el host de WebJobs.

Todas las solicitudes HTTP que realice desde una función mediante `HttpClient`, pasarán por `ServicePointManager`. Una vez que se alcanza `DefaultConnectionLimit`, `ServicePointManager` inicia las solicitudes de puesta en cola antes de enviarlas. Suponga que `DefaultConnectionLimit` se establece en 2 y que el código realiza 1000 solicitudes de HTTP. Inicialmente, solo se permiten dos solicitudes mediante el sistema operativo. Las otras 998 se ponen en cola hasta que haya espacio para ellas. Es decir, `HttpClient` puede agotar el tiempo de espera porque *considera* que ya se ha realizado la solicitud, pero en realidad el sistema operativo nunca la envió al servidor de destino. Por lo tanto, es posible que vea cierto comportamiento que no parezca tener sentido: el `HttpClient` local tarda 10 segundos en completar una solicitud, pero el servicio devuelve todas las solicitudes en 200 ms. 

El valor predeterminado de las aplicaciones ASP.NET es `Int32.MaxValue`, y es probable que funcione bien en las instancias de WebJobs que se ejecuten en un plan de App Service de nivel básico o superior. WebJobs normalmente necesita la configuración de Always On que solo es compatible con los planes de App Service de nivel básico o superior. 

Si WebJobs se ejecuta en un plan de App Service gratuito o compartido, un espacio aislado de App Service con un [límite de 300 conexiones](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits) se encargará de restringir la aplicación. Con un límite de conexiones independientes en `ServicePointManager`, es más probable que se alcance el umbral de conexión del espacio aislado y que el sitio se apague. En ese caso, reduzca el valor de `DefaultConnectionLimit` (use algo como 50 o 100) para evitar que esto suceda y permitir un nivel de rendimiento suficiente.

La configuración debe establecerse antes de que se realice cualquier solicitud de HTTP. Por este motivo, el host de WebJobs no debe intentar ajustar la configuración automáticamente, ya que puede haber solicitudes HTTP antes de iniciar el host y esto puede provocar un comportamiento inesperado. El mejor método consiste en establecer el valor inmediatamente en el método `Main` antes de inicializar `JobHost`, tal como se muestra en el ejemplo siguiente:

```csharp
static void Main(string[] args)
{
    // Set this immediately so that it is used by all requests.
    ServicePointManager.DefaultConnectionLimit = Int32.MaxValue;

    var host = new JobHost();
    host.RunAndBlock();
}
```

## <a name="triggers"></a>Desencadenadores

Las funciones deben ser métodos públicos y deben tener un atributo de desencadenador o el atributo [NoAutomaticTrigger](#manual-trigger).

### <a name="automatic-trigger"></a>Desencadenador automático

Los desencadenadores automáticos llaman a una función en respuesta a un evento. Para obtener un ejemplo, consulte el desencadenador de cola en el [artículo de introducción](webjobs-sdk-get-started.md).

### <a name="manual-trigger"></a>Desencadenador manual

Para desencadenar manualmente una función, utilice el atributo `NoAutomaticTrigger`, tal como se muestra en el ejemplo siguiente:

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

La forma en que desencadena manualmente la función depende de la versión del SDK.

#### <a name="version-3x"></a>Versión 3.x

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

#### <a name="version-2x"></a>Versión 2.x

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

## <a name="input-and-output-bindings"></a>Enlaces de entrada y de salida

Los enlaces de entrada proporcionan una forma declarativa de hacer que los datos de servicios de Azure o de terceros estén disponibles para su código. Los enlaces de salida, a su vez, proporcionan una manera de actualizar los datos. En el [artículo de introducción](webjobs-sdk-get-started.md) se muestra un ejemplo de cada uno.

Puede usar un valor devuelto de método para un enlace de salida mediante la aplicación del atributo al valor devuelto de método. Consulte el ejemplo en el artículo de Azure Functions [Desencadenadores y enlaces](../azure-functions/functions-triggers-bindings.md#using-the-function-return-value).

## <a name="binding-types"></a>Tipos de enlaces

La forma en que se instalan y administran los tipos de enlace es diferente entre las versiones 3.x y 2.x del SDK. Puede encontrar el paquete que desea instalar para un tipo de enlace concreto en la sección **Paquetes** del [artículo de referencia](#binding-reference-information) de ese tipo de enlace de Azure Functions. Como excepción, tenemos el desencadenador y el enlace de Files (para el sistema de archivos local), que no es compatible con Azure Functions.

#### <a name="version-3x"></a>Versión 3.x

En la versión 3.x, los enlaces de almacenamiento se incluyen en el paquete `Microsoft.Azure.WebJobs.Extensions.Storage`. Llame al método de extensión `AddAzureStorage` en el método `ConfigureWebJobs`, como se muestra en el ejemplo siguiente:

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

Para usar otros tipos de enlaces y desencadenadores, instale el paquete NuGet que los contenga y llame al método de extensión `Add<binding>` implementado en la extensión. Por ejemplo, si quiere usar un enlace de Azure Cosmos DB, instale `Microsoft.Azure.WebJobs.Extensions.CosmosDB` y llame a `AddCosmosDB`, como en el ejemplo siguiente:

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

#### <a name="version-2x"></a>Versión 2.x

En la versión 2.x del paquete `Microsoft.Azure.WebJobs` se incluyen los siguientes tipos de desencadenadores y enlaces:

* Almacenamiento de blobs
* Queue Storage
* Almacenamiento de tablas

Para usar otros tipos de enlaces y desencadenadores, instale el paquete de NuGet que los contenga y llame a un método en `Use<binding>` el objeto `JobHostConfiguration`. Por ejemplo, si desea usar un desencadenador de tipo Temporizador, instale `Microsoft.Azure.WebJobs.Extensions` y llame a `UseTimers` en el método `Main`, tal como se indica en este ejemplo:

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

La forma de enlazar con el objeto [`ExecutionContext`] depende de la versión del SDK.

#### <a name="version-3x"></a>Versión 3.x

Llame al método de extensión `AddExecutionContextBinding` en el método `ConfigureWebJobs`, como se muestra en el ejemplo siguiente:

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

#### <a name="version-2x"></a>Versión 2.x

El paquete `Microsoft.Azure.WebJobs.Extensions` que se ha mencionado anteriormente también proporciona un tipo de enlace especial que se puede registrar mediante una llamada al método `UseCore`. Este enlace le permite definir un parámetro [`ExecutionContext`] en la signatura de función, que está habilitado de la manera siguiente:

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

Algunos desencadenadores y enlaces le permiten configurar su comportamiento. La forma de configurarlos depende de la versión del SDK.

* **Versión 3.x:** La configuración se establece cuando se llama al método `Add<Binding>` en `ConfigureWebJobs`.
* **Versión 2.x** Mediante la definición de las propiedades en un objeto de configuración que se pasa al objeto `JobHost`.

### <a name="queue-trigger-configuration"></a>Poner en cola la configuración del desencadenador

Las opciones que puede configurar para el desencadenador de cola de Storage se explican en la [referencia host.json](../azure-functions/functions-host-json.md#queues) de Azure Functions. En los ejemplos siguientes se muestra cómo establecerlas en la configuración:

#### <a name="version-3x"></a>Versión 3.x

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

#### <a name="version-2x"></a>Versión 2.x

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

### <a name="configuration-for-other-bindings"></a>Configuración de otros enlaces

Algunos tipos de desencadenador y enlace definen su propio tipo de configuración personalizada. Por ejemplo, el desencadenador File le permite especificar la ruta de acceso raíz para supervisar, como en los ejemplos siguientes:

#### <a name="version-3x"></a>Versión 3.x

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

#### <a name="version-2x"></a>Versión 2.x

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

Para obtener más información acerca de las expresiones de enlace, consulte [Patrones y expresiones de enlace](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns) en la documentación de Azure Functions.

### <a name="custom-binding-expressions"></a>Expresiones de enlace personalizadas

En ocasiones, deseará especificar un nombre de cola, un contenedor o nombre de blob o un nombre de cola en el código en lugar de codificarlo. Por ejemplo, es posible que desee especificar el nombre de la cola del atributo `QueueTrigger` en un archivo de configuración o en una variable de entorno.

Para hacerlo, puede pasar un objeto `NameResolver` al objeto `JobHostConfiguration`. Incluya marcadores de posición en los parámetros del constructor de atributos de desencadenador o de enlace, y el código `NameResolver` le proporcionará los valores reales que se utilizarán en lugar de esos marcadores de posición. Los marcadores de posición se identifican enmarcándolos con signos de porcentaje (%), tal como se muestra en el ejemplo siguiente:

```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Este código le permite usar una cola llamada `logqueuetest` en el entorno de prueba y otra llamada `logqueueprod` en producción. En lugar de codificar de forma rígida un nombre de cola, es preferible especificar el nombre de una entrada en la colección `appSettings`.

Existe un valor predeterminado NameResolver que surte efecto si no se proporciona uno personalizado. El valor predeterminado obtiene valores de configuración de la aplicación o de las variables de entorno.

La clase `NameResolver` obtiene el nombre de cola de `appSettings`, tal como se muestra en el ejemplo siguiente:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

#### <a name="version-3x"></a>Versión 3.x

La resolución se configura mediante la inserción de dependencias. En estos ejemplos se requiere la siguiente instrucción `using`:

```cs
using Microsoft.Extensions.DependencyInjection;
```

Para agregar la resolución se llama al método de extensión [`ConfigureServices`] en [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder), como en el ejemplo siguiente:

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

#### <a name="version-2x"></a>Versión 2.x

Pase la clase `NameResolver` al objeto `JobHost`, tal como se muestra en el ejemplo siguiente:

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

Si necesita realizar algún trabajo en la función antes de usar un atributo de enlace como `Queue`, `Blob`, o `Table`, puede usar la interfaz `IBinder`.

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

En la documentación de Azure Functions se proporciona información de referencia acerca de cada tipo de enlace. Si usa una cola de Storage como ejemplo, encontrará la siguiente información en cada artículo de referencia de enlace:

* [Paquetes](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x): qué paquete debe instalar con el fin de incluir soporte para el enlace en un proyecto de SDK de WebJobs.
* [Ejemplos](../azure-functions/functions-bindings-storage-queue.md#trigger---example): el ejemplo de la biblioteca de clases de C# se aplica a los SDK de WebJobs; simplemente omita el atributo `FunctionName`.
* [Atributos](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes): los atributos que se usarán para el tipo de enlace.
* [Configuración](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration): explicaciones de las propiedades de atributo y de los parámetros del constructor.
* [Uso](../azure-functions/functions-bindings-storage-queue.md#trigger---usage): qué tipos puede enlazar e información acerca de cómo funciona el enlace. Por ejemplo: sondear algoritmos o procesar colas dudosas.
  
Para obtener una lista de artículos de referencia de enlace, consulte la sección **Enlaces admitidos**  del artículo [Enlaces y desencadenadores](../azure-functions/functions-triggers-bindings.md#supported-bindings) de Azure Functions. En esa lista, solo Azure Functions admite los enlaces de HTTP, webhook y Event Grid; el SDK de WebJobs no los admite.

## <a name="disable-attribute"></a>Atributo Disable 

El atributo [Disable](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) le permite controlar si una función se puede desencadenar. 

En el ejemplo siguiente, si la configuración de la aplicación `Disable_TestJob` tiene un valor de "1" o "True" (no se distingue entre mayúsculas y minúsculas), la función no se ejecutará. En ese caso, el tiempo de ejecución crea el mensaje de registro *La función "Functions.TestJob" está deshabilitada*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Cuando cambia los valores de configuración de la aplicación en Azure Portal, se reinicia WebJobs para así poder aplicar la nueva configuración.

El atributo se puede declarar en el nivel de clase, método o parámetro. El nombre de la configuración también puede contener expresiones de enlace.

## <a name="timeout-attribute"></a>Atributo Timeout

El atributo [Timeout](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) ocasiona que se cancele una función si no se completa dentro de un período de tiempo especificado. En el ejemplo siguiente, la función se ejecuta durante un día sin el tiempo de expiración. Con el tiempo de expiración, la función se cancela después de 15 segundos.

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

Puede aplicar el atributo Timeout en el nivel de clase o método; asimismo, puede especificar un tiempo de expiración global mediante `JobHostConfiguration.FunctionTimeout`. Los tiempos de expiración de nivel de clase o método invalidan el tiempo de expiración global.

## <a name="singleton-attribute"></a>Atributo Singleton

Use el atributo [Singleton](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) si desea asegurarse de que solo una instancia de una función se ejecuta incluso cuando hay varias instancias de la aplicación web de host. Puede realizar esta operación si implementa el [bloqueo distribuido](#viewing-lease-blobs).

En el ejemplo siguiente, solo una instancia de la función `ProcessImage` se ejecuta en un momento dado:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Algunos de los desencadenadores tienen compatibilidad integrada para poder administrar la simultaneidad:

* **QueueTrigger**: establece `JobHostConfiguration.Queues.BatchSize` en 1.
* **ServiceBusTrigger**: establece `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` en 1.
* **FileTrigger**: establece `FileProcessor.MaxDegreeOfParallelism` en 1.

Puede usar esta configuración para asegurarse de que la función se ejecuta como una singleton en una sola instancia. Para asegurarse de que solo una instancia de la función se está ejecutando cuando la aplicación web escale de forma horizontal varias instancias, aplique un bloqueo de Singleton de nivel de escucha (`[Singleton(Mode = SingletonMode.Listener)]`). Los bloqueos de escucha se adquieren al inicio de la instancia de JobHost. Si tres instancias de escalado horizontal se inician al mismo tiempo, solo una de ellas adquiere el bloqueo y solo un agente de escucha se inicia.

### <a name="scope-values"></a>Valores de ámbito

Puede especificar un **valor o expresión de ámbito** en el singleton que garantiza que todas las ejecuciones de la función en dicho ámbito se serializarán. La implementación de un bloqueo más granular de este tipo puede permitir algún nivel de paralelismo de la función, mientras se serializan otras invocaciones en función de los requisitos. Por ejemplo, en el ejemplo que se muestra a continuación, la expresión de ámbito se enlaza con el valor `Region` del mensaje entrante. Cuando la cola contiene tres mensajes en las regiones "Este", "Este" y "Oeste", respectivamente, los mensajes que tienen la región "Este" se ejecutarán en serie mientras que el mensaje con la región "Oeste"se ejecutará en paralelo con los de "Este".

```csharp
[Singleton("{Region}")]
public static async Task ProcessWorkItem([QueueTrigger("workitems")] WorkItem workItem)
{
     // Process the work item
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

El ámbito predeterminado de un bloqueo es `SingletonScope.Function`, lo que significa que el ámbito de bloqueo (la ruta de acceso de concesión del blob) está asociado al nombre de función completo. Para bloquear elementos en las funciones, debe especificar `SingletonScope.Host` y usar un nombre de id. de ámbito que sea el mismo en todas las funciones que no quiera ejecutar simultáneamente. En el ejemplo siguiente, solo una instancia de `AddItem` o de `RemoveItem` se ejecuta a la vez:

```csharp
[Singleton("ItemsLock", SingletonScope.Host)]
public static void AddItem([QueueTrigger("add-item")] string message)
{
     // Perform the add operation
}

[Singleton("ItemsLock", SingletonScope.Host)]
public static void RemoveItem([QueueTrigger("remove-item")] string message)
{
     // Perform the remove operation
}
```

### <a name="viewing-lease-blobs"></a>Ver blobs de concesión

El SDK de WebJobs usa las [concesiones de blobs de Azure](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) en segundo plano para poder implementar el bloqueo distribuido. Los blobs de concesión que usa Singleton se pueden encontrar en el contenedor `azure-webjobs-host` de la cuenta de almacenamiento `AzureWebJobsStorage` en la ruta de acceso "bloqueos". Por ejemplo, la ruta de acceso del blob de concesión que se indicó en el primer ejemplo `ProcessImage` mostrado, puede ser `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Todas las rutas de acceso incluyen el identificador de JobHost, que en este caso es 061851c758f04938a4426aa9ab3869c0.

## <a name="async-functions"></a>Funciones asincrónicas

Para obtener información acerca de cómo programar las funciones asincrónicas, consulte la documentación de Azure Functions en [Funciones asincrónicas](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>Tokens de cancelación

Para obtener información sobre cómo controlar los tokens de cancelación, consulte la documentación de Azure Functions acerca de los [tokens de cancelación y el apagado estable](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Varias instancias

Si la aplicación web se ejecuta en varias instancias, un trabajo de WebJobs continuo se ejecuta en cada instancia, en la que escuchará a los desencadenadores e intentará llamar a las funciones. Los distintos enlaces de desencadenador están diseñados para compartir el trabajo de forma eficiente y colaborar en todas las instancias, por lo que el escalado horizontal de varias instancias le permitirá administrar más carga.

Los desencadenadores de cola y blob impiden automáticamente que una función procese un mensaje de cola o blob varias veces; las funciones no tienen que escribirse para que sean idempotentes.

El desencadenador del temporizador garantiza automáticamente que solo una instancia del temporizador se ejecute, por lo que no se ejecutará más de una instancia de función en un momento programado.

Sin embargo, si desea asegurarse de que solo una instancia de una función se ejecute incluso cuando hay varias instancias de la aplicación web del host, puede utilizar el [atributo Singleton](#singleton-attribute).

## <a name="filters"></a>Filtros

Los filtros de función (versión preliminar) proporcionan una manera de personalizar la canalización de la ejecución de WebJobs con su propia lógica. Los filtros son similares a los [filtros de ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). Se pueden implementar como atributos declarativos que se aplican a las funciones o clases. Para obtener más información, consulte [Filtros de función](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Registro y supervisión

Le recomendamos que eche un vistazo a la plataforma de registro que se desarrolló para ASP.NET, y al artículo [Introducción](webjobs-sdk-get-started.md) que muestra cómo se usa. 

### <a name="log-filtering"></a>Filtrado de registros

Cada registro que creó una instancia `ILogger` tiene asociados los valores `Category` y `Level`. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) es una enumeración y el código entero indica la importancia relativa:

|LogLevel    |Código|
|------------|---|
|Seguimiento       | 0 |
|Depurar       | 1 |
|Información | 2 |
|Advertencia     | 3 |
|Error       | 4 |
|Crítico    | 5 |
|None        | 6 |

Cada categoría se puede filtrar por separado a un elemento [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) determinado. Por ejemplo, es posible que quiera ver todos los registros referentes al procesamiento del desencadenador de blobs, pero solo el valor `Error` y superiores para todo lo demás.

#### <a name="version-3x"></a>Versión 3.x

La versión 3.x del SDK se basa en el filtrado integrado en .NET Core. La clase `LogCategories` le permite definir las categorías de funciones, desencadenadores o usuarios específicos. También define los filtros de estados de host específicos, como `Startup` y `Results`. De esta manera, puede ajustar la salida del registro. Si no se encuentra ninguna coincidencia en las categorías definidas, el filtro volverá a tener el valor `Default` al decidir si filtrar el mensaje.

`LogCategories` requiere la siguiente instrucción using:

```cs
using Microsoft.Azure.WebJobs.Logging; 
```

En el ejemplo siguiente se crea un filtro que filtra todos los registros del nivel `Warning` de forma predeterminada. Las categorías de `Function` o `results` (equivalente a `Host.Results` en la versión 2.x) se filtran en el nivel `Error`. El filtro compara la categoría actual con todos los niveles registrados en la instancia `LogCategories` y elige la coincidencia más larga. Esto significa que el nivel `Debug` registrado para `Host.Triggers` coincidirá con `Host.Triggers.Queue` o `Host.Triggers.Blob`. Esto le permite controlar categorías más amplias sin necesidad de agregar cada valor.

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

#### <a name="version-2x"></a>Versión 2.x

En la versión 2.x del SDK, la clase `LogCategoryFilter` se usa para controlar el filtrado. `LogCategoryFilter` tiene una propiedad `Default` con un valor inicial de `Information`, lo que significa que los mensajes con niveles de `Information`, `Warning`, `Error` o `Critical` se registran, pero los mensajes con niveles de `Debug` o `Trace` se filtran inmediatamente.

Al igual que sucede con `LogCategories` en la versión 3.x, la propiedad `CategoryLevels` le permite especificar los niveles de registro de determinadas categorías, de modo que puede ajustar la salida de registros como desee. Si no encuentra ninguna coincidencia en el diccionario `CategoryLevels`, el filtro volverá a tener el valor `Default` cuando decida filtrar el mensaje.

En el ejemplo siguiente se crea un filtro que filtra todos los registros del nivel `Warning` de forma predeterminada. Las categorías de `Function` o `Host.Results` se filtran en el nivel `Error`. `LogCategoryFilter` compara la categoría actual con todos los valores `CategoryLevels` registrado y elige la coincidencia más larga. Esto significa que el nivel `Debug` registrado para `Host.Triggers` coincidirá con `Host.Triggers.Queue` o `Host.Triggers.Blob`. Esto le permite controlar categorías más amplias sin necesidad de agregar cada valor.

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

La forma en que implemente la telemetría personalizada de [Application Insights](../azure-monitor/app/app-insights-overview.md) depende de la versión del SDK que use. Para saber cómo configurar Application Insights, consulte [Incorporación del registro de Application Insights](webjobs-sdk-get-started.md#add-application-insights-logging).

#### <a name="version-3x"></a>Versión 3.x

Desde la versión 3.x del SDK de WebJobs basado en el host genérico de .NET Core, ya no se proporciona una factoría de telemetría personalizada. Sin embargo, puede agregar telemetría personalizada a la canalización mediante la inserción de dependencias. En los ejemplos de esta sección se requieren las siguientes instrucciones `using`:

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
        // Add Logging Providers
        b.AddConsole();

        // If this key exists in any config, use it to enable App Insights
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

Cuando se construye [`TelemetryConfiguration`], se incluyen todos los tipos registrados de [`ITelemetryInitializer`]. Para más información, consulte [API de Application Insights para eventos y métricas personalizados](../azure-monitor/app/api-custom-events-metrics.md).

En la versión 3.x, ya no tiene que vaciar el elemento [`TelemetryClient`] cuando se detiene el host. El sistema de inserción de dependencias de .NET Core desecha automáticamente el elemento `ApplicationInsightsLoggerProvider` registrado, que vacía [`TelemetryClient`].

#### <a name="version-2x"></a>Versión 2.x

En la versión 2.x, el elemento [`TelemetryClient`] creado internamente mediante el proveedor de Application Insights para el SDK de WebJobs usa [ServerTelemetryChannel](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Cuando el punto de conexión de Application Insights no está disponible o limita las solicitudes entrantes, este canal [guarda las solicitudes en el sistema de archivos de la aplicación web y vuelve a enviarlas más adelante](https://apmtips.com/blog/2015/09/03/more-telemetry-channels).

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

        // change the default from 30 seconds to 15 seconds
        channel.MaxTelemetryBufferDelay = TimeSpan.FromSeconds(15);

        return channel;
    }
}
```

El objeto SamplingPercentageEstimatorSettings configura el [muestreo adaptable](https://docs.microsoft.com/azure/application-insights/app-insights-sampling#adaptive-sampling-at-your-web-server). Esto significa que, en ciertos escenarios de gran volumen, App Insights envía un subconjunto de datos de telemetría seleccionados al servidor.

Después de crear la fábrica de telemetría, la puede pasar al proveedor de registro de Application Insights:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Pasos siguientes

En esta guía se proporcionan fragmentos de código que muestran cómo controlar los escenarios comunes para trabajar con el SDK de WebJobs. Para obtener ejemplos completos, consulte [azure-webjobs-sdk-samples](https://github.com/Azure/azure-webjobs-sdk-samples).

["ExecutionContext"]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs
["TelemetryClient"]: /dotnet/api/microsoft.applicationinsights.telemetryclient
["ConfigureServices"]: /dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices
["ITelemetryInitializer"]: /dotnet/api/microsoft.applicationinsights.extensibility.itelemetryinitializer
["TelemetryConfiguration"]: /dotnet/api/microsoft.applicationinsights.extensibility.telemetryconfiguration
