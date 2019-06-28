---
title: 'Características en versión preliminar de Durable Functions: Azure Functions'
description: Obtenga información sobre las características en versión preliminar para Durable Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.date: 04/23/2019
ms.author: azfuncdf
ms.openlocfilehash: 8ceb84ab9e9c41ff6a9cbde62571fb12ae67d790
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65596085"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Versión preliminar de Durable Functions 2.0 (Azure Functions)

*Durable Functions* es una extensión de [Azure Functions](../functions-overview.md) y [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md) que le permite escribir funciones con estado en un entorno sin servidor. La extensión administra el estado, establece puntos de control y reinicia en su nombre. Si todavía no está familiarizado con Durable Functions, vea la [documentación de introducción](durable-functions-overview.md).

Durable Functions es una característica GA (disponible con carácter general) de Azure Functions, pero también contiene varias características secundarias que se encuentran actualmente en versión preliminar pública. En este artículo se describen las características de versión preliminar más recientes y se detalla cómo funcionan y cómo puede comenzar a usarlas.

> [!NOTE]
> Estas características en versión preliminar forman parte de una versión 2.0 de Durable Functions, que actualmente es una **versión de calidad alfa** con varios cambios importantes. En nuget.org puede encontrar compilaciones de paquete de la extensión Durable de Azure Functions, con el formato **2.0.0-alpha** para las versiones. Estas compilaciones no son adecuadas para cargas de trabajo de producción, y es posible que las versiones posteriores contengan cambios adicionales.

## <a name="breaking-changes"></a>Cambios drásticos

En Durable Functions 2.0 se han presentado varios cambios importantes. No se espera que las aplicaciones existentes sean compatibles con Durable Functions 2.0 sin cambios de código. En esta sección se enumeran algunos de los cambios:

### <a name="dropping-net-framework-support"></a>Eliminación de la compatibilidad con .NET Framework

En Durable Functions 2.0 se ha quitado la compatibilidad con .NET Framework (y, por tanto, con Functions 1.0). La razón principal es para permitir que los colaboradores que no usen Windows tengan facilidad para compilar y probar los cambios que realicen en Durable Functions desde las plataformas macOS y Linux. La razón secundaria es para animar a los desarrolladores a que cambien a la versión más reciente del runtime de Azure Functions.

### <a name="hostjson-schema"></a>Esquema de host.json

En el fragmento siguiente se muestra el nuevo esquema para host.json. El cambio principal que tener en cuenta es la nueva sección `"storageProvider"` y la sección `"azureStorage"` debajo de ella. Este cambio se ha realizado para admitir [proveedores de almacenamiento alternativos](durable-functions-preview.md#alternate-storage-providers).

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "azureStorage": {
          "connectionStringName": <string>,
          "controlQueueBatchSize": <int?>,
          "partitionCount": <int?>,
          "controlQueueVisibilityTimeout": <hh:mm:ss?>,
          "workItemQueueVisibilityTimeout": <hh:mm:ss?>,
          "trackingStoreConnectionStringName": <string?>,
          "trackingStoreNamePrefix": <string?>,
          "maxQueuePollingInterval": <hh:mm:ss?>
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "traceInputAndOutputs": <bool?>,
      "eventGridTopicEndpoint": <string?>,
      "eventGridKeySettingName": <string?>,
      "eventGridPublishRetryCount": <string?>,
      "eventGridPublishRetryInterval": <hh:mm:ss?>,
      "eventGridPublishRetryHttpStatus": <int[]?>,
      "eventgridPublishEventTypes": <string[]?>,
      "customLifeCycleNotificationHelperType"
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "logReplayEvents": <bool?>
  }
}
```

A medida que Durable Functions 2.0 se estabilice, se introducirán más cambios en la sección `durableTask` de host.json. Para más información sobre estos cambios, vea [este problema de GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/641).

### <a name="public-interface-changes"></a>Cambios en la interfaz pública

Los distintos objetos de "contexto" compatibles con Durable Functions tenían clases base abstractas destinadas a usarse en pruebas unitarias. Como parte de Durable Functions 2.0, estas clases base abstractas se han reemplazado con interfaces. El código de función que usa directamente los tipos concretos no se ve afectado.

En la tabla siguiente se representan los cambios principales:

| Tipo anterior | Tipo nuevo |
|----------|----------|
| DurableOrchestrationClientBase | IDurableOrchestrationClient |
| DurableOrchestrationContextBase | IDurableOrchestrationContext |
| DurableActivityContextBase | IDurableActivityContext |

Si una clase base abstracta contiene métodos virtuales, estos métodos virtuales se han reemplazado por los métodos de extensión definidos en `DurableContextExtensions`.

## <a name="entity-functions"></a>Funciones de entidad

Las funciones de entidad definen las operaciones de lectura y actualización de pequeños fragmentos de estado, denominados *entidades duraderas*. Al igual que las funciones de orquestador, las de entidad son funciones con un tipo especial de desencadenador, el *desencadenador de entidad*. A diferencia de las funciones de orquestador, las funciones de entidad no tienen restricciones de código específicas. Las funciones de entidad también administran el estado de forma explícita, en lugar de representarlo de forma implícita a través del flujo de control.

El código siguiente es un ejemplo de una función de entidad simple que define una entidad *Counter*. La función define tres operaciones (`add`, `subtract` y `reset`) y cada una actualiza un valor entero, `currentValue`.

```csharp
[FunctionName("Counter")]
public static async Task Counter(
    [EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();
    int operand = ctx.GetInput<int>();

    switch (ctx.OperationName)
    {
        case "add":
            currentValue += operand;
            break;
        case "subtract":
            currentValue -= operand;
            break;
        case "reset":
            await SendResetNotificationAsync();
            currentValue = 0;
            break;
    }

    ctx.SetState(currentValue);
}
```

A las *instancias* de entidad se accede a través de un identificador único, el *id. de entidad*. Un identificador de entidad es simplemente un par de cadenas que identifica de forma exclusiva una instancia de entidad. Consta de:

1. un **nombre de entidad**: un nombre que identifica el tipo de la entidad (por ejemplo, "Counter")
2. una **clave de entidad**: una cadena que identifica de forma exclusiva la entidad entre todas las demás entidades del mismo nombre (por ejemplo, un GUID)

Por ejemplo, se podría usar una función de entidad de *contador* para mantener la puntuación en un juego en línea. Cada instancia del juego tendrá un identificador de entidad único, como `@Counter@Game1`, `@Counter@Game2` y así sucesivamente.

### <a name="comparison-with-virtual-actors"></a>Comparación con actores virtuales

El [modelo de actor](https://en.wikipedia.org/wiki/Actor_model) tiene una gran influencia en el diseño de las entidades duraderas. Si ya tiene experiencia con los actores, los conceptos subyacentes a las entidades duraderas deberían resultarle familiares. En concreto, las entidades duraderas son similares a los [actores virtuales](https://research.microsoft.com/en-us/projects/orleans/) en muchos aspectos:

* Las entidades duraderas son direccionables a través de un *id. de entidad*.
* Las operaciones de las entidades duraderas se ejecutan en serie, una cada vez, para evitar condiciones de carrera.
* Las entidades duraderas se crean de forma automática cuando se las llama o se señalan.
* Cuando no ejecutan operaciones, las entidades duraderas se descargan en modo silencioso de la memoria.

Pero hay algunas diferencias importantes que conviene destacar:

* Las entidades duraderas se modelan como funciones puras. Este diseño es diferente al de la mayoría de marcos orientados a objetos que representan actores mediante compatibilidad específica del lenguaje para clases, propiedades y métodos.
* Las entidades duraderas priorizan la *durabilidad* sobre la *latencia* y, por tanto, es posible que no sean adecuadas para aplicaciones con requisitos estrictos de latencia.
* Los mensajes enviados entre las entidades se entregan en orden y de forma confiable.
* Las entidades duraderas se pueden usar junto con las orquestaciones duraderas y pueden servir como bloqueos distribuidos, que se describen más adelante en este artículo.
* Los patrones de solicitud y respuesta en las entidades se limitan a las orquestaciones. Para la comunicación entre entidades, solo se permiten mensajes unidireccionales (lo que también se conoce como "señalización"), como en el modelo de actor original. Este comportamiento evita los interbloqueos distribuidos.

### <a name="durable-entity-apis"></a>API de entidades de Durable

La compatibilidad con las entidades implica varias API. En primer lugar, hay una nueva API para definir funciones de entidad, como se ha mostrado antes, que especifica lo que debe ocurrir cuando se invoca una operación en una entidad. Además, las API existentes para clientes y orquestaciones se han actualizado con nuevas funciones para la interacción con las entidades.

### <a name="implementing-entity-operations"></a>Implementación de operaciones de entidad

La ejecución de una operación en una entidad puede llamar a estos miembros del objeto de contexto (`IDurableEntityContext` en. NET):

* **OperationName**: obtiene el nombre de la operación.
* **GetInput\<T>** : obtiene la entrada para la operación.
* **GetState\<T>** : obtiene el estado actual de la entidad.
* **SetState**: actualiza el estado de la entidad.
* **SignalEntity**: envía un mensaje unidireccional a una entidad.
* **Self**: obtiene el identificador de la entidad.
* **Return**: devuelve un valor al cliente o la orquestación que ha llamado a la operación.
* **IsNewlyConstructed**: devuelve `true` si la entidad no existía antes de la operación.
* **DestructOnExit**: elimina la entidad después de finalizar la operación.

Las operaciones tienen menos restricciones que las orquestaciones:

* Las operaciones pueden llamar a E/S externa, mediante API sincrónicas o asincrónicas (se recomienda usar solo las asincrónicas).
* Las operaciones pueden ser no deterministas. Por ejemplo, es seguro llamar a `DateTime.UtcNow`, `Guid.NewGuid()` o `new Random()`.

### <a name="accessing-entities-from-clients"></a>Acceso a entidades desde clientes

Las entidades duraderas se pueden invocar desde funciones normales a través del enlace `orchestrationClient` (`IDurableOrchestrationClient` en .NET). Se admiten los métodos siguientes:

* **ReadEntityStateAsync\<T >** : lee el estado de una entidad.
* **SignalEntityAsync**: envía un mensaje unidireccional a una entidad y espera a que se ponga en cola.

Estos métodos priorizan el rendimiento a la coherencia: `ReadEntityStateAsync` puede devolver un valor obsoleto, y `SignalEntityAsync` puede devolver antes de que haya finalizado la operación. En cambio, la llamada a entidades desde orquestaciones (como se describe a continuación) es altamente coherente.

### <a name="accessing-entities-from-orchestrations"></a>Acceso a entidades desde orquestaciones

Las orquestaciones pueden acceder a las entidades mediante el objeto de contexto. Pueden elegir entre la comunicación unidireccional ("desencadenar y olvidar") y la comunicación bidireccional (solicitud y respuesta). Los métodos correspondientes son

* **SignalEntity**: envía un mensaje unidireccional a una entidad.
* **CallEntityAsync**: envía un mensaje a una entidad y espera una respuesta en la que se indique que se ha completado la operación.
* **CallEntityAsync\<T>** : envía un mensaje a una entidad y espera una respuesta que contenga un resultado de tipo T.

Al usar la comunicación bidireccional, las excepciones iniciadas durante la ejecución de la operación también se devuelven a la orquestación que realiza la llamada y se vuelven a iniciar. En cambio, cuando se usa "desencadenar y olvidar", las excepciones no se observan.

### <a name="locking-entities-from-orchestrations"></a>Bloqueado de entidades desde orquestaciones

Las orquestaciones pueden bloquear las entidades. Esta capacidad proporciona una manera sencilla de evitar carreras no deseadas mediante *secciones críticas*.

El objeto de contexto proporciona los métodos siguientes:

* **LockAsync**: adquiere bloqueos en una o más entidades.
* **IsLocked**: devuelve true si actualmente se encuentra en una sección crítica, false en caso contrario.

La sección crítica finaliza, y se liberan todos los bloqueos, cuando finaliza la orquestación. En .NET, `LockAsync` devuelve un objeto `IDisposable` que finaliza la sección crítica cuando se elimina, que se puede usar junto con una cláusula `using` para obtener una representación sintáctica de la sección crítica.

Por ejemplo, imagine una orquestación que necesita probar si dos jugadores deben estar disponibles y, después, asignarlos a un juego. Esta tarea se puede implementar mediante una sección crítica como sigue:

```csharp
[FunctionName("Orchestrator")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext ctx)
{
    EntityId player1 = /* ... */;
    EntityId player2 = /* ... */;

    using (await ctx.LockAsync(player1, player2))
    {
        bool available1 = await ctx.CallEntityAsync<bool>(player1, "is-available");
        bool available2 = await ctx.CallEntityAsync<bool>(player2, "is-available");

        if (available1 && available2)
        {
            Guid gameId = ctx.NewGuid();

            await ctx.CallEntityAsync(player1, "assign-game", gameId);
            await ctx.CallEntityAsync(player2, "assign-game", gameId);
        }
    }
}
```

En la sección crítica, las dos entidades de jugador están bloqueadas, lo que significa que no ejecutan ninguna operación que no sea las que se llaman desde dentro de la sección crítica. Este comportamiento impide carreras con operaciones en conflicto, como que se asigne a los jugadores a otro juego o que se cierre la sesión.

Se imponen varias restricciones a la forma de usar las secciones críticas. Estas restricciones sirven para evitar interbloqueos y reentradas.

* Las secciones críticas no se pueden anidar.
* Las secciones críticas no pueden crear suborquestaciones.
* Las secciones críticas solo pueden llamar a las entidades que hayan bloqueado.
* Las secciones críticas no pueden llamar a la misma entidad mediante varias llamadas paralelas.
* Las secciones críticas solo pueden señalar a las entidades que no hayan bloqueado.

## <a name="alternate-storage-providers"></a>Proveedores de almacenamiento alternativos

En la actualidad, Durable Task Framework admite varios proveedores de almacenamiento incluidos [Azure Storage](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [Azure Service Bus](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus), un [emulador en memoria](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator) y un proveedor [Redis](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis) experimental. Pero hasta ahora, la extensión Durable Task para Azure Functions solo admitía el proveedor Azure Storage. A partir de Durable Functions 2.0, se va a agregar compatibilidad con proveedores de almacenamiento alternativos, empezando por el proveedor de Redis.

> [!NOTE]
> Durable Functions 2.0 solo admite proveedores compatibles con .NET Standard 2.0. En el momento de escribir este artículo, el proveedor de Azure Service Bus no admite .NET Standard 2.0 y, por tanto, no está disponible como un proveedor de almacenamiento alternativo.

### <a name="emulator"></a>Emulador

[DurableTask.Emulator](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/) es un proveedor de almacenamiento no duradero en memoria local adecuado para escenarios de pruebas locales. Se puede configurar mediante el esquema mínimo de **host.json** siguiente:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { }
      }
    }
  }
}
```

### <a name="redis-experimental"></a>Redis (experimental)

El proveedor [DurableTask.Redis](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Redis/) conserva todos los estados de orquestación en un clúster de Redis configurado.

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "redis": {
          "connectionStringName": <string>,
        }
      }
    }
  }
}
```

`connectionStringName` debe hacer referencia al nombre de una configuración de aplicación o variable de entorno. Esa variable de entorno o configuración de aplicación debe contener un valor de cadena de conexión de Redis con el formato *servidor:puerto*. Por ejemplo, `localhost:6379` para conectarse a un clúster de Redis local.

> [!NOTE]
> En la actualidad, el proveedor de Redis es experimental y solo es compatible con aplicaciones de función que se ejecutan en un único nodo.
