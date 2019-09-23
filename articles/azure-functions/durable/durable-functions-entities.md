---
title: 'Entidades duraderas: Azure Functions'
description: Obtenga información sobre qué son las entidades duraderas y cómo usarlas en la extensión Durable Functions para Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 08/31/2019
ms.author: azfuncdf
ms.openlocfilehash: 99e61cef55bd97704063e4d2da90909d0376c327
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961463"
---
# <a name="entity-functions-preview"></a>Funciones de entidad (versión preliminar)

Las funciones de entidad definen las operaciones de lectura y actualización de pequeños fragmentos de estado, denominados *entidades duraderas*. Al igual que las funciones de orquestador, las de entidad son funciones con un tipo especial de desencadenador, el *desencadenador de entidad*. A diferencia de las funciones de orquestador, las funciones de entidad no tienen restricciones de código específicas. Las funciones de entidad también administran el estado de forma explícita, en lugar de representarlo de forma implícita a través del flujo de control.

> [!NOTE]
> Las funciones de entidad y la funcionalidad relacionada solo están disponibles en Durable Functions 2.0 y versiones superiores. Las funciones de entidad están actualmente en versión preliminar pública.

## <a name="entity-identity"></a>Identidad de la entidad

A las entidades (a veces llamadas *instancias* de entidad) se accede mediante un identificador único, el *identificador de entidad*. Un identificador de entidad es simplemente un par de cadenas que identifica de forma exclusiva una instancia de entidad. Consta de:

* Un **nombre de entidad**: nombre que identifica el tipo de la entidad (por ejemplo, "Counter").
* Una **clave de entidad**: cadena que identifica de forma exclusiva la entidad entre todas las demás entidades del mismo nombre (por ejemplo, un GUID).

Por ejemplo, se podría usar una función de entidad de *contador* para mantener la puntuación en un juego en línea. Cada instancia del juego tendrá un identificador de entidad único, como `@Counter@Game1`, `@Counter@Game2` y así sucesivamente. Todas las operaciones que tienen como destino una entidad determinada requieren la especificación de un identificador de entidad como parámetro.

## <a name="programming-models"></a>Modelos de programación

Las entidades duraderas admiten dos modelos de programación diferentes. El primer modelo es un modelo "funcional" dinámico en el que la entidad se define mediante una sola función. El segundo modelo es un modelo orientado a objetos en el que la entidad se define mediante una clase y métodos. Estos modelos y los modelos de programación para interactuar con entidades se describen en las secciones siguientes.

### <a name="defining-entities"></a>Definición de entidades

Hay dos modelos de programación opcionales para crear entidades duraderas. El código siguiente es un ejemplo de una entidad *Counter* simple implementada como una función estándar. Esta función define tres *operaciones* (`add`, `reset` y `get`), cada una de las cuales opera en un valor de estado entero, `currentValue`.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();

    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            currentValue += operand;
            break;
        case "reset":
            currentValue = 0;
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }

    ctx.SetState(currentValue);
}
```

Este modelo funciona mejor para implementaciones de entidades simples o para implementaciones que tienen un conjunto dinámico de operaciones. Aun así, también puede usar un modelo de programación basado en clases que resulta útil para las entidades que son estáticas, pero que tienen implementaciones más complejas. El ejemplo siguiente es una implementación equivalente de la entidad `Counter` que usa clases y métodos.

```csharp
public class Counter
{
    [JsonProperty("value")]
    public int CurrentValue { get; set; }

    public void Add(int amount) => this.CurrentValue += amount;
    
    public void Reset() => this.CurrentValue = 0;
    
    public int Get() => this.CurrentValue;

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

> [!NOTE]
> El método de punto de entrada de la función con el atributo `[FunctionName]` se *debe* declarar `static` cuando se usan clases de entidad. Los métodos de punto de entrada no estáticos pueden dar lugar a la inicialización de varios objetos y, potencialmente, a otros comportamientos no definidos.

En el modelo de programación basado en clases, el objeto `IDurableEntityContext` está disponible en la propiedad estática `Entity.Current`.

El modelo basado en clases es similar al modelo de programación popularizado por [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/). En este modelo, un tipo de entidad se define como una clase .NET. Cada método de la clase es una operación que se puede invocar a través de un cliente externo. Pero, a diferencia de Orleans, las interfaces .NET son opcionales. En el ejemplo de *Counter* anterior no se usaba una interfaz, pero aun así se puede invocar a través de otras funciones o de llamadas a la API HTTP.

> [!NOTE]
> Las funciones de desencadenador de entidad están disponibles en Durable Functions 2.0 y versiones posteriores. Actualmente, las funciones de desencadenador de entidad solo están disponibles para las aplicaciones de funciones de .NET.

### <a name="accessing-entities-from-clients"></a>Acceso a entidades desde clientes

Las entidades duraderas se pueden invocar o consultar desde funciones ordinarias, también conocidas como *funciones cliente*, mediante el [enlace de salida de cliente de la entidad](durable-functions-bindings.md#entity-client). En el ejemplo siguiente se muestra una función desencadenada por una cola que *señaliza* una entidad mediante este enlace.

```csharp
[FunctionName("AddFromQueue")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [DurableClient] IDurableEntityClient client)
{
    // Entity operation input comes from the queue message content.
    var entityId = new EntityId(nameof(Counter), "myCounter");
    int amount = int.Parse(input);
    return client.SignalEntityAsync(entityId, "Add", amount);
}
```

> [!NOTE]
> Las funciones de .NET admiten métodos débilmente tipados y métodos con seguridad de tipos para señalizar las entidades. Consulte la documentación de referencia del [enlace de cliente de la entidad](durable-functions-bindings.md#entity-client-usage) para más información.

El término *señal* significa que la invocación de la API de entidad es unidireccional y asincrónica. No es posible que una *función cliente* sepa cuándo la entidad ha procesado la operación, ni tampoco es posible que una función de entidad devuelva un valor a una función cliente. La mensajería unidireccional basada en colas era una opción de diseño intencionada para que las entidades duraderas dieran prioridad a la durabilidad sobre el rendimiento. Esta opción de diseño es uno de los inconvenientes de las entidades duraderas en comparación con otras tecnologías similares. Actualmente, solo las orquestaciones son capaces de controlar los valores devueltos por las entidades, como se describe en la sección siguiente.

Las funciones cliente también pueden consultar el estado de las entidades, tal y como se muestra en el ejemplo siguiente:

```csharp
[FunctionName("QueryCounter")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function)] HttpRequestMessage req,
    [DurableClient] IDurableEntityClient client)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");
    JObject state = await client.ReadEntityStateAsync<JObject>(entityId);
    return req.CreateResponse(HttpStatusCode.OK, state);
}
```

Las consultas de estado de la entidad se envían al almacén de seguimiento duradero y devuelven el estado *guardado* más recientemente de la entidad. Es posible que el estado devuelto esté obsoleto en comparación con el estado en memoria de la entidad. Solo las orquestaciones pueden leer el estado en memoria de una entidad, tal y como se describe en la sección siguiente.

### <a name="accessing-entities-from-orchestrations"></a>Acceso a entidades desde orquestaciones

Las funciones de orquestador pueden acceder a las entidades mediante API en el [enlace del desencadenador de orquestación](durable-functions-bindings.md#orchestration-trigger). Las funciones de orquestador pueden elegir entre la comunicación unidireccional (desencadenar y olvidar, lo que también se conoce como *señalización*) y la comunicación bidireccional (solicitud y respuesta, también denominada *llamada*). En el ejemplo de código siguiente se muestra una función de orquestador que *llama* y *señaliza* una entidad *Counter*.

```csharp
[FunctionName("CounterOrchestration")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");

    // Synchronous call to the entity which returns a value
    int currentValue = await context.CallEntityAsync<int>(entityId, "Get");
    if (currentValue < 10)
    {
        // Asynchronous call which updates the value
        await context.SignalEntityAsync<int>(entityId, "Add", 1);
    }
}
```

Solo las orquestaciones son capaces de llamar a las entidades y obtener una respuesta que puede ser un valor devuelto o una excepción. Las funciones de cliente que usan el [enlace de cliente](durable-functions-bindings.md#entity-client) solo pueden *señalizar* entidades.

> [!NOTE]
> Llamar a una entidad desde una función de orquestador es similar a llamar a una [función de actividad](durable-functions-types-features-overview.md#activity-functions) desde una función de orquestador. La principal diferencia es que las funciones de entidad son objetos duraderos con una dirección (el *identificador de entidad*) y admiten la especificación de un nombre de operación. Por otro lado, las funciones de actividad no tienen estado y no tienen el concepto de operaciones.

### <a name="dependency-injection-in-entity-classes-net"></a>Inserción de dependencias en clases de entidad (.NET)

Las clases de entidad admiten la [inserción de dependencias de Azure Functions](../functions-dotnet-dependency-injection.md). En el ejemplo siguiente se muestra cómo registrar un servicio `IHttpClientFactory` en una entidad basada en clases.

```csharp
[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
        }
    }
}
```

En el fragmento de código siguiente se muestra cómo incorporar el servicio insertado en la clase de la entidad.

```csharp
public class HttpEntity
{
    private readonly HttpClient client;

    public class HttpEntity(IHttpClientFactory factory)
    {
        this.client = factory.CreateClient();
    }

    public Task<int> GetAsync(string url)
    {
        using (var response = await this.client.GetAsync(url))
        {
            return (int)response.StatusCode;
        }
    }

    // The function entry point must be declared static
    [FunctionName(nameof(HttpEntity))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<HttpEntity>();
}
```

> [!NOTE]
> A diferencia de cuando se usa la inserción de constructores en Azure Functions para .NET normal, el método de punto de entrada de las funciones se *debe* declarar `static` para las entidades basadas en clases. Declarar un punto de entrada de función no estático puede producir conflictos entre el inicializador de objetos de Azure Functions normal y el inicializador de objetos de las entidades duraderas.

### <a name="bindings-in-entity-classes-net"></a>Enlaces en clases de entidad (.NET)

A diferencia de las funciones normales, los métodos de las clases de la entidad no tienen acceso directo a los enlaces de entrada y salida. En su lugar, los datos de enlace se deben capturar en la declaración de la función de punto de entrada y, a continuación, se deben pasar al método `DispatchAsync<T>`. Cualquier objeto pasado a `DispatchAsync<T>` se pasará automáticamente al constructor de clases de la entidad como un argumento.

En el ejemplo siguiente se muestra cómo se puede poner a disposición de una entidad basada en clases una referencia a `CloudBlobContainer` desde el [enlace de entrada del blob](../functions-bindings-storage-blob.md#input).

```csharp
public class BlobBackedEntity
{
    private readonly CloudBlobContainer container;

    public BlobBackedEntity(CloudBlobContainer container)
    {
        this.container = container;
    }

    // ... entity methods can use this.container in their implementations ...
    
    [FunctionName(nameof(BlobBackedEntity))]
    public static Task Run(
        [EntityTrigger] IDurableEntityContext context,
        [Blob("my-container", FileAccess.Read)] CloudBlobContainer container)
    {
        // passing the binding object as a parameter makes it available to the
        // entity class constructor
        return context.DispatchAsync<BlobBackedEntity>(container);
    }
}
```

Para más información sobre los enlaces en Azure Functions, consulte la documentación [Desencadenadores y enlaces de Azure Functions](../functions-triggers-bindings.md).

## <a name="entity-coordination"></a>Coordinación de entidades

Puede haber ocasiones en las que necesite coordinar las operaciones entre varias entidades. Por ejemplo, en una aplicación de banca, puede tener entidades que representan cuentas bancarias individuales. Al transferir fondos de una cuenta a otra, debe asegurarse de que la cuenta de _origen_ tiene fondos suficientes y de que las actualizaciones tanto en la cuenta de _origen_ como en la de _destino_ se realizan de manera transaccional y coherente.

### <a name="transfer-funds-example-in-c"></a>Ejemplo de transferencia de fondos en C#

En el siguiente código de ejemplo se transfieren fondos entre dos entidades de tipo _cuenta_ mediante una función de orquestador. La coordinación de las actualizaciones de las entidades requiere el uso del método `LockAsync` para crear una _sección crítica_ en la orquestación:

> [!NOTE]
> Para simplificar, en este ejemplo se reutiliza la entidad `Counter` definida previamente. Sin embargo, en una aplicación real, sería mejor definir una entidad `BankAccount` más detallada.

```csharp
// This is a method called by an orchestrator function
public static async Task<bool> TransferFundsAsync(
    string sourceId,
    string destinationId,
    int transferAmount,
    IDurableOrchestrationContext context)
{
    var sourceEntity = new EntityId(nameof(Counter), sourceId);
    var destinationEntity = new EntityId(nameof(Counter), destinationId);

    // Create a critical section to avoid race conditions.
    // No operations can be performed on either the source or
    // destination accounts until the locks are released.
    using (await context.LockAsync(sourceEntity, destinationEntity))
    {
        ICounter sourceProxy = 
            context.CreateEntityProxy<ICounter>(sourceEntity);
        ICounter destinationProxy =
            context.CreateEntityProxy<ICounter>(destinationEntity);

        int sourceBalance = await sourceProxy.Get();

        if (sourceBalance >= transferAmount)
        {
            await sourceProxy.Add(-transferAmount);
            await destinationProxy.Add(transferAmount);

            // the transfer succeeded
            return true;
        }
        else
        {
            // the transfer failed due to insufficient funds
            return false;
        }
    }
}
```

En .NET, `LockAsync` devuelve un elemento `IDisposable` que finaliza la sección crítica cuando se desecha. Este resultado `IDisposable` se puede usar junto con un bloque `using` para obtener una representación sintáctica de la sección crítica.

En el ejemplo anterior, una función de orquestador transfirió fondos de una entidad de _origen_ a una entidad de _destino_. El método `LockAsync` bloqueó las entidades de la cuenta de _origen_ y de _destino_. Este bloqueo garantiza que ningún otro cliente podría consultar o modificar el estado de ninguna de las cuentas hasta que la lógica de orquestación salga de la _sección crítica_ al final de la instrucción `using`. Esto evita de manera eficaz la posibilidad de que se produzca un descubierto en la cuenta de _origen_.

### <a name="critical-section-behavior"></a>Comportamiento de la sección crítica

El método `LockAsync` crea una _sección crítica_ en una orquestación. Estas _secciones críticas_ impiden que otras orquestaciones realicen cambios superpuestos en un conjunto especificado de entidades. Internamente, la API `LockAsync` envía operaciones de "bloqueo" a las entidades y vuelve cuando recibe un mensaje de respuesta "bloqueo adquirido" de cada una de estas mismas entidades. *Bloquear* y *desbloquear* son operaciones integradas admitidas por todas las entidades.

No se permiten operaciones de otros clientes en una entidad mientras está en estado bloqueado. Este comportamiento garantiza que solo una instancia de orquestación pueda bloquear una entidad a la vez. Si un llamador intenta invocar una operación en una entidad mientras está bloqueada por una orquestación, esa operación se colocará en una *cola de operaciones pendientes*. Las operaciones pendientes no se procesarán hasta que la orquestación que la contiene libere el bloqueo.

> [!NOTE] 
> Esto es ligeramente diferente de las primitivas de sincronización que se usan en la mayoría de los lenguajes de programación, como la instrucción `lock` de C#. Por ejemplo, en C#, todos los subprocesos deben usar la instrucción `lock` para garantizar una sincronización adecuada entre varios subprocesos. Sin embargo, las entidades no requieren que todos los llamadores _bloqueen_ explícitamente una entidad. Si algún llamador bloquea una entidad, todas las demás operaciones de esa entidad se bloquearán y se pondrán en cola después de ese bloqueo.

Los bloqueos en entidades son duraderos, por lo que se conservarán incluso si se recicla el proceso de ejecución. Los bloqueos se conservan internamente como parte del estado duradero de una entidad.

### <a name="critical-section-restrictions"></a>Restricciones de la sección crítica

Se imponen varias restricciones a la forma de usar las secciones críticas. Estas restricciones sirven para evitar interbloqueos y reentradas.

* Las secciones críticas no se pueden anidar.
* Las secciones críticas no pueden crear suborquestaciones.
* Las secciones críticas solo pueden llamar a las entidades que hayan bloqueado.
* Las secciones críticas no pueden llamar a la misma entidad mediante varias llamadas paralelas.
* Las secciones críticas solo pueden señalar a las entidades que no hayan bloqueado.

## <a name="comparison-with-virtual-actors"></a>Comparación con actores virtuales

Muchas de las características de las entidades duraderas están inspiradas en el [modelo de actor](https://en.wikipedia.org/wiki/Actor_model). Si ya está familiarizado con los actores, puede que reconozca muchos de los conceptos que se describen en este artículo. En concreto, las entidades duraderas son similares a los [actores virtuales](https://research.microsoft.com/projects/orleans/) en muchos aspectos:

* Las entidades duraderas son direccionables a través de un *id. de entidad*.
* Las operaciones de las entidades duraderas se ejecutan en serie, una cada vez, para evitar condiciones de carrera.
* Las entidades duraderas se crean de forma automática cuando se las llama o se señalan.
* Cuando no ejecutan operaciones, las entidades duraderas se descargan en modo silencioso de la memoria.

Pero hay algunas diferencias importantes que conviene destacar:

* Las entidades duraderas priorizan la *durabilidad* sobre la *latencia* y, por tanto, es posible que no sean adecuadas para aplicaciones con requisitos estrictos de latencia.
* Los mensajes enviados entre las entidades se entregan en orden y de forma confiable.
* Las entidades duraderas se pueden usar junto con las orquestaciones duraderas y admiten mecanismos de bloqueo distribuidos.
* Los patrones de solicitud y respuesta en las entidades se limitan a las orquestaciones. Para la comunicación *cliente-entidad* y *entidad-entidad*, solo se permiten mensajes unidireccionales (lo que también se conoce como "señalización"), como en el modelo de actor original. Este comportamiento evita los interbloqueos distribuidos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información acerca de las centrales de tareas](durable-functions-task-hubs.md)