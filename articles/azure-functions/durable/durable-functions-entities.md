---
title: 'Entidades duraderas: Azure Functions'
description: Obtenga información sobre qué son las entidades duraderas y cómo usarlas en la extensión Durable Functions para Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: overview
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 62ca71e1b42e000f7528a2963793f9bf40663bf3
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73818514"
---
# <a name="entity-functions"></a>Funciones de entidad

Las funciones de entidad definen las operaciones de lectura y actualización de pequeños fragmentos de estado, denominados *entidades duraderas*. Al igual que las funciones de orquestador, las de entidad son funciones con un tipo especial de desencadenador, el *desencadenador de entidad*. A diferencia de las funciones de orquestador, las de entidad administran el estado de una entidad de forma explícita, en lugar de representarlo de forma implícita a través del flujo de control.
Las entidades proporcionan un medio para el escalado horizontal de aplicaciones mediante la distribución del trabajo entre muchas entidades, cada una de ellas con un estado de tamaño modesto.

> [!NOTE]
> Las funciones de entidad y la funcionalidad relacionada solo están disponibles en Durable Functions 2.0 y versiones superiores.

## <a name="general-concepts"></a>Conceptos generales

Las entidades se comportan de forma algo parecida a pequeños servicios que se comunican mediante mensajes. Cada entidad tiene una identidad única y un estado interno (si existe). Al igual que los servicios u objetos, las entidades realizan operaciones cuando se les pide que lo hagan. Cuando se ejecuta, una operación puede actualizar el estado interno de la entidad. También puede llamar a servicios externos y esperar una respuesta. Las entidades se comunican con otras entidades, orquestaciones y clientes mediante mensajes que se envían implícitamente a través de colas de confianza. 

Para evitar conflictos, se garantiza la ejecución en serie de todas las operaciones de una sola entidad, es decir, una después de otra. 

### <a name="entity-id"></a>El identificador de entidad
A las entidades se accede a través de un identificador único, el *identificador de entidad*. Un identificador de entidad es simplemente un par de cadenas que identifica de forma exclusiva una instancia de entidad. Consta de:

* Un **nombre de entidad**: nombre que identifica el tipo de la entidad (por ejemplo, "Counter"). Este nombre debe coincidir con el nombre de la función que implementa la entidad correspondiente. No distingue entre mayúsculas y minúsculas.
* Una **clave de entidad**: cadena que identifica de forma exclusiva la entidad entre todas las demás entidades del mismo nombre (por ejemplo, un GUID).

Por ejemplo, se podría usar una función de entidad de *contador* para mantener la puntuación en un juego en línea. Cada instancia del juego tendrá un identificador de entidad único, como `@Counter@Game1`, `@Counter@Game2` y así sucesivamente. Todas las operaciones que tienen como destino una entidad determinada requieren la especificación de un identificador de entidad como parámetro.

### <a name="entity-operations"></a>Operaciones de entidad ###

Para invocar una operación en una entidad, hay que especificar

* El *identificador de entidad* de la entidad de destino.
* El *nombre de la operación*, una cadena que especifica la operación que se va a realizar. Por ejemplo, la entidad "Counter" podría admitir las operaciones "add" (agregar), "get" (obtener) o "reset" (restablecer).
* La *entrada de operación*, que es un parámetro de entrada opcional de la operación. Por ejemplo, la operación "add" (agregar) puede tomar una cantidad entera como entrada.

Las operaciones pueden devolver un valor de resultado o un resultado de error (como un error de JavaScript o una excepción de .NET). Las orquestaciones que llamaron a la operación pueden observar este resultado o error.

Una operación de entidad también puede crear, leer, actualizar y eliminar el estado de la entidad. El estado de la entidad se conserva siempre de forma duradera en el almacenamiento.

## <a name="defining-entities"></a>Definición de entidades

Actualmente se ofrecen dos API diferentes para definir las entidades.

Una **sintaxis basada en funciones** en la que las entidades se representan como funciones y la aplicación envía explícitamente las operaciones. Esta sintaxis funciona bien para las entidades con un estado simple, pocas operaciones o un conjunto dinámico de estas (por ejemplo, en los marcos de trabajo de las aplicaciones). Sin embargo, su mantenimiento puede resultar laborioso, ya que no detecta errores de tipo en tiempo de compilación.

Una **sintaxis basada en clases** en la que las entidades y operaciones se representan como clases y métodos. Esta sintaxis genera código más fácilmente legible y permite invocar operaciones con seguridad de tipos. La sintaxis basada en clases es simplemente una fina capa encima de la sintaxis basada en funciones, por lo que ambas variantes se pueden usar indistintamente en la misma aplicación.

### <a name="example-function-based-syntax---c"></a>Ejemplo: Sintaxis basada en funciones: C#

El código siguiente es un ejemplo de una entidad *Counter* simple implementada como una función duradera. Esta función define tres operaciones (`add`, `reset` y `get`), cada una de las cuales opera en un valor de estado entero.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            ctx.SetState(ctx.GetState<int>() + ctx.GetInput<int>());
            break;
        case "reset":
            ctx.SetState(0);
            break;
        case "get":
            ctx.Return(ctx.GetState<int>());
            break;
    }
}
```

Para más información sobre la sintaxis basada en funciones y cómo utilizarla, consulte [Sintaxis basada en funciones](durable-functions-dotnet-entities.md#function-based-syntax).

### <a name="example-class-based-syntax---c"></a>Ejemplo: Sintaxis basada en clases: C#

El ejemplo siguiente es una implementación equivalente de la entidad `Counter` que usa clases y métodos.

```csharp
[JsonObject(MemberSerialization.OptIn)]
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

El estado de esta entidad es un objeto de tipo `Counter`, que contiene un campo que almacena el valor actual del contador. Para conservar este objeto en el almacenamiento, la biblioteca [Json.NET](https://www.newtonsoft.com/json) lo serializa y deserializa. 

Para más información sobre la sintaxis basada en clases y cómo utilizarla, consulte [Definición de las clases de entidad](durable-functions-dotnet-entities.md#defining-entity-classes).

### <a name="example-javascript-entity"></a>Ejemplo: Entidad de JavaScript

Las entidades durables están disponibles en JavaScript a partir de la versión **1.3.0** del paquete de npm `durable-functions`. El código siguiente es la entidad *Counter* implementada como función durable escrita en JavaScript.

**function.json**
```json
{
  "bindings": [
    {
      "name": "context",
      "type": "entityTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

**index.js**
```javascript
const df = require("durable-functions");

module.exports = df.entity(function(context) {
    const currentValue = context.df.getState(() => 0);
    switch (context.df.operationName) {
        case "add":
            const amount = context.df.getInput();
            context.df.setState(currentValue + amount);
            break;
        case "reset":
            context.df.setState(0);
            break;
        case "get":
            context.df.return(currentValue);
            break;
    }
});
```

## <a name="accessing-entities"></a>Acceso a las entidades

Se puede acceder a las entidades mediante una comunicación unidireccional o bidireccional. Se usa la siguiente terminología para distinguir: 

* **Llamar** a una entidad significa que usamos una comunicación bidireccional (de ida y vuelta): se envía un mensaje de operación a la entidad y, a continuación, se espera el mensaje de respuesta antes de continuar. El mensaje de respuesta puede proporcionar un valor de resultado o un resultado de error (como un error de JavaScript o una excepción de .NET). El autor de la llamada observa este resultado o error.
* **Señalar** una entidad significa que usamos una comunicación unidireccional (desencadenar y olvidar): se envía un mensaje de operación, pero no se espera una respuesta para continuar. Aunque se garantiza la entrega del mensaje, el emisor no sabe cuándo se realizará esta y no puede observar ningún valor de resultado ni errores.

Se puede acceder a las entidades desde las funciones de cliente, desde las de orquestador o desde las de entidad. No todos los contextos admiten todas las formas de comunicación:

* Desde los clientes, puede *señalar* entidades y puede *leer* el estado de la entidad.
* Desde las orquestaciones, puede *señalar* entidades y puede *llamar* a estas.
* Desde las entidades, puede *señalar* entidades.

A continuación, se muestran algunos ejemplos que muestran estas diversas formas de acceder a las entidades.

> [!NOTE]
> Para simplificar, los ejemplos siguientes muestran una sintaxis débilmente tipada para acceder a las entidades. En general, se recomienda [acceder a las entidades mediante interfaces](durable-functions-dotnet-entities.md#accessing-entities-through-interfaces), ya que así se proporciona una comprobación de más tipos.

### <a name="example-client-signals-an-entity"></a>Ejemplo: el cliente señala una entidad

Para acceder a las entidades desde una función de Azure normal, también conocida como *función de cliente*, use el [enlace de salida del cliente de entidad](durable-functions-bindings.md#entity-client). En el ejemplo siguiente se muestra una función desencadenada por una cola que *señaliza* una entidad mediante este enlace.

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

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    await context.df.signalEntity(entityId, "add", 1);
};
```

El término *señal* significa que la invocación de la API de entidad es unidireccional y asincrónica. Una *función de cliente* no puede saber cuándo la entidad ha procesado la operación. La función de cliente tampoco puede observar valores de resultado ni excepciones. 

### <a name="example-client-reads-an-entity-state"></a>Ejemplo: El cliente lee el estado de una entidad

Las funciones de cliente también pueden consultar el estado de una entidad, tal y como se muestra en el ejemplo siguiente:

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

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const entityId = new df.EntityId("Counter", "myCounter");
    return context.df.readEntityState(entityId);
};
```

Las consultas de estado de la entidad se envían al almacén de seguimiento duradero y devuelven el estado *guardado* más recientemente de la entidad. Este estado es siempre un estado "confirmado", es decir, nunca es un estado temporal intermedio que se presupone en medio de la ejecución de una operación. No obstante, es posible que este estado esté obsoleto en comparación con el estado en memoria de la entidad. Solo las orquestaciones pueden leer el estado en memoria de una entidad, tal y como se describe en la sección siguiente.

### <a name="example-orchestration-signals-and-calls-an-entity"></a>Ejemplo: La orquestación señala y llama a una entidad

Las funciones de orquestador pueden acceder a las entidades mediante API en el [enlace del desencadenador de orquestación](durable-functions-bindings.md#orchestration-trigger). En el ejemplo de código siguiente se muestra una función de orquestador que *llama* y *señaliza* una entidad *Counter*.

```csharp
[FunctionName("CounterOrchestration")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var entityId = new EntityId(nameof(Counter), "myCounter");

    // Two-way call to the entity which returns a value - awaits the response
    int currentValue = await context.CallEntityAsync<int>(entityId, "Get");
    if (currentValue < 10)
    {
        // One-way signal to the entity which updates the value - does not await a response
        context.SignalEntity(entityId, "Add", 1);
    }
}
```

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    const entityId = new df.EntityId("Counter", "myCounter");

    // Two-way call to the entity which returns a value - awaits the response
    currentValue = yield context.df.callEntity(entityId, "get");
    if (currentValue < 10) {
        // One-way signal to the entity which updates the value - does not await a response
        yield context.df.signalEntity(entityId, "add", 1);
    }
});
```

Solo las orquestaciones son capaces de llamar a las entidades y obtener una respuesta que puede ser un valor devuelto o una excepción. Las funciones de cliente que usan el [enlace de cliente](durable-functions-bindings.md#entity-client) solo pueden *señalizar* entidades.

> [!NOTE]
> Llamar a una entidad desde una función de orquestador es similar a llamar a una [función de actividad](durable-functions-types-features-overview.md#activity-functions) desde una función de orquestador. La principal diferencia es que las funciones de entidad son objetos duraderos con una dirección (el *identificador de entidad*) y admiten la especificación de un nombre de operación. Por otro lado, las funciones de actividad no tienen estado y no tienen el concepto de operaciones.

### <a name="example-entity-signals-an-entity"></a>Ejemplo: la entidad señala una entidad

Una función de entidad puede enviar señales a otras entidades (o incluso a sí misma) mientras ejecuta una operación.
Por ejemplo, podemos modificar el ejemplo de la entidad Counter anterior para que envíe una señal de "hito alcanzado" a alguna entidad de supervisión cuando el contador alcance el valor 100:

```csharp
   case "add":
        var currentValue = ctx.GetState<int>();
        var amount = ctx.GetInput<int>();
        if (currentValue < 100 && currentValue + amount >= 100)
        {
            ctx.SignalEntity(new EntityId("MonitorEntity", ""), "milestone-reached", ctx.EntityKey);
        }

        ctx.SetState(currentValue + amount);
        break;
```

```javascript
    case "add":
        const amount = context.df.getInput();
        if (currentValue < 100 && currentValue + amount >= 100) {
            const entityId = new df.EntityId("MonitorEntity", "");
            context.df.signalEntity(entityId, "milestone-reached", context.df.instanceId);
        }
        context.df.setState(currentValue + amount);
        break;
```

## <a name="entity-coordination"></a>Coordinación de entidades

Puede haber ocasiones en las que necesite coordinar las operaciones entre varias entidades. Por ejemplo, en una aplicación de banca, puede tener entidades que representan cuentas bancarias individuales. Al transferir fondos de una cuenta a otra, debe asegurarse de que la cuenta de _origen_ tiene fondos suficientes y de que las actualizaciones tanto en la cuenta de _origen_ como en la de _destino_ se realizan de manera transaccional y coherente.

### <a name="example-transfer-funds-c"></a>Ejemplo: Transferencia de fondos (C#)

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

En el ejemplo anterior, una función de orquestador transfirió fondos de una entidad de _origen_ a una entidad de _destino_. El método `LockAsync` bloqueó las entidades de la cuenta de _origen_ y de _destino_. Este bloqueo garantiza que ningún otro cliente podría consultar o modificar el estado de ninguna de las cuentas hasta que la lógica de orquestación salga de la _sección crítica_ al final de la instrucción `using`. Este comportamiento evita la posibilidad de que se produzca un descubierto en la cuenta de _origen_.

> [!NOTE] 
> Cuando una orquestación termina (ya sea de forma normal o con un error), todas las secciones críticas en curso se terminan implícitamente y se liberan todos los bloqueos.

### <a name="critical-section-behavior"></a>Comportamiento de la sección crítica

El método `LockAsync` crea una _sección crítica_ en una orquestación. Estas _secciones críticas_ impiden que otras orquestaciones realicen cambios superpuestos en un conjunto especificado de entidades. Internamente, la API `LockAsync` envía operaciones de "bloqueo" a las entidades y vuelve cuando recibe un mensaje de respuesta "bloqueo adquirido" de cada una de estas mismas entidades. *Bloquear* y *desbloquear* son operaciones integradas admitidas por todas las entidades.

No se permiten operaciones de otros clientes en una entidad mientras está en estado bloqueado. Este comportamiento garantiza que solo una instancia de orquestación pueda bloquear una entidad a la vez. Si un llamador intenta invocar una operación en una entidad mientras está bloqueada por una orquestación, esa operación se colocará en una *cola de operaciones pendientes*. Las operaciones pendientes no se procesarán hasta que la orquestación que la contiene libere el bloqueo.

> [!NOTE] 
> Esto es ligeramente diferente de las primitivas de sincronización que se usan en la mayoría de los lenguajes de programación, como la instrucción `lock` de C#. Por ejemplo, en C#, todos los subprocesos deben usar la instrucción `lock` para garantizar una sincronización adecuada entre varios subprocesos. Sin embargo, las entidades no requieren que todos los llamadores _bloqueen_ explícitamente una entidad. Si algún llamador bloquea una entidad, todas las demás operaciones de esa entidad se bloquearán y se pondrán en cola después de ese bloqueo.

Los bloqueos en entidades son duraderos, por lo que se conservarán incluso si se recicla el proceso de ejecución. Los bloqueos se conservan internamente como parte del estado duradero de una entidad.

A diferencia de las transacciones, las secciones críticas no revierten automáticamente los cambios en caso de que se produzcan errores. En su lugar, es necesario codificar explícitamente cualquier control de errores (reversión, reintento u otros); por ejemplo, detectando errores o excepciones. Esta opción de diseño es intencionada. La reversión automática de todos los efectos de una orquestación es difícil o imposible por lo general, ya que las orquestaciones pueden ejecutar actividades y realizar llamadas a servicios externos que no se pueden revertir. Además, los intentos de reversión pueden producir errores ellos mismos y requieren un mayor control de errores.

### <a name="critical-section-rules"></a>Reglas de secciones críticas

A diferencia de las primitivas de bloqueo de bajo nivel de la mayoría de los lenguajes de programación, se **garantiza que las secciones críticas no tendrán interbloqueos**. Para evitar los interbloqueos, se aplican las siguientes restricciones: 

* Las secciones críticas no se pueden anidar.
* Las secciones críticas no pueden crear suborquestaciones.
* Las secciones críticas solo pueden llamar a las entidades que hayan bloqueado.
* Las secciones críticas no pueden llamar a la misma entidad mediante varias llamadas paralelas.
* Las secciones críticas solo pueden señalar a las entidades que no hayan bloqueado.

Las infracciones de estas reglas provocan un error de tiempo de ejecución (por ejemplo, un `LockingRulesViolationException` en .NET) que incluye un mensaje que explica qué regla se ha infringido.

## <a name="comparison-with-virtual-actors"></a>Comparación con actores virtuales

Muchas de las características de las entidades duraderas están inspiradas en el [modelo de actor](https://en.wikipedia.org/wiki/Actor_model). Si ya está familiarizado con los actores, puede que reconozca muchos de los conceptos que se describen en este artículo. Las entidades duraderas se parecen especialmente a [actores virtuales](https://research.microsoft.com/projects/orleans/) o *granos*, tal y como se ha popularizado con el [proyecto Orleans](http://dotnet.github.io/orleans/). Por ejemplo:

* Las entidades duraderas son direccionables a través de un *id. de entidad*.
* Las operaciones de las entidades duraderas se ejecutan en serie, una cada vez, para evitar condiciones de carrera.
* Las entidades duraderas se crean de forma implícita cuando se las llama o se señalan.
* Cuando no ejecutan operaciones, las entidades duraderas se descargan en modo silencioso de la memoria.

Pero hay algunas diferencias importantes que conviene destacar:

* Las entidades duraderas priorizan la *durabilidad* sobre la *latencia* y, por tanto, es posible que no sean adecuadas para aplicaciones con requisitos estrictos de latencia.
* Las entidades duraderas no tienen tiempos de espera integrados para los mensajes. En Orleans, se agota el tiempo de espera de todos los mensajes después de un tiempo configurable (30 segundos como valor predeterminado).
* Los mensajes enviados entre las entidades se entregan en orden y de forma confiable. En Orleans, la entrega confiable u ordenada es compatible con el contenido que se envía mediante secuencias, pero no está garantizada para todos los mensajes entre granos.
* Los patrones de solicitud y respuesta en las entidades se limitan a las orquestaciones. Desde las entidades, solo se permite la mensajería unidireccional (conocida también como "señalización"), como en el modelo de actor original, y a diferencia del modelo de granos de Orleans. 
* Las entidades duraderas no se interbloquean. En Orleans, se pueden producir interbloqueos (y no se resolverán hasta que se agote el tiempo de espera de los mensajes).
* Las entidades duraderas se pueden usar junto con las orquestaciones duraderas y admiten mecanismos de bloqueo distribuidos. 


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Leer la Guía del desarrollador de entidades duraderas en .NET](durable-functions-dotnet-entities.md)

> [!div class="nextstepaction"]
> [Más información acerca de las centrales de tareas](durable-functions-task-hubs.md)
