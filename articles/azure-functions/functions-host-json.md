---
title: Referencia de host.json para Azure Functions 2.x
description: Documentación de referencia para el archivo host.json de Azure Functions con el entorno en tiempo de ejecución de la versión 2.
ms.topic: conceptual
ms.date: 09/08/2018
ms.openlocfilehash: 08d772fc9b2871262b449a017f8be59a344576b2
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74975455"
---
# <a name="hostjson-reference-for-azure-functions-2x-and-later"></a>Referencia de host.json para Azure Functions 2.x y versiones posteriores 

> [!div class="op_single_selector" title1="Seleccione la versión del entorno de ejecución de Azure Functions que usa: "]
> * [Versión 1](functions-host-json-v1.md)
> * [Versión 2](functions-host-json.md)

El archivo de metadatos *host.json* contiene las opciones de configuración global que afectan a todas las funciones de dicha aplicación de función. En este artículo se enumeran los valores que están disponibles a partir de la versión 2.x del entorno en tiempo de ejecución de Azure Functions.  

> [!NOTE]
> Este artículo trata sobre Azure Functions 2.x y versiones posteriores.  Para obtener una referencia de host.json en Functions 1.x, consulte la [referencia de host.json para Azure Functions, versión 1.x](functions-host-json-v1.md).

Otras opciones de configuración de aplicación de función se administran en su [configuración de la aplicación](functions-app-settings.md).

Algunas opciones de configuración de host.json solo se usan con la ejecución local en el archivo [local.settings.json](functions-run-local.md#local-settings-file).

## <a name="sample-hostjson-file"></a>Archivo host.json de ejemplo

El siguiente archivo *host.json* de ejemplo tiene especificadas todas las opciones posibles.

```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "cosmosDb": {},
        "durableTask": {},
        "eventHubs": {},
        "http": {},
        "queues": {},
        "sendGrid": {},
        "serviceBus": {}
    },
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "samplingSettings": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 20
            }
        }
    },
    "managedDependency": {
        "enabled": true
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

Las siguientes secciones de este artículo explican cada propiedad de nivel superior. Todas son opcionales, a menos que se indique lo contrario.

## <a name="aggregator"></a>aggregator

[!INCLUDE [aggregator](../../includes/functions-host-json-aggregator.md)]

## <a name="applicationinsights"></a>applicationInsights

Esta configuración es un elemento secundario de [logging](#logging).

Controla la [característica de muestreo de Application Insights](./functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "samplingSettings": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 20
        }
    }
}
```

> [!NOTE]
> Los muestreos de registros pueden provocar que algunas ejecuciones no aparezcan en la hoja de supervisión de Application Insights.

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|isEnabled|true|Habilita o deshabilita el muestreo.| 
|maxTelemetryItemsPerSecond|20|Umbral donde comienza el muestreo.| 
|EnableLiveMetrics |true|Habilita la colección de Live Metrics.|
|EnableDependencyTracking|true|Habilita el seguimiento de dependencias.|
|EnablePerformanceCountersCollection|true|Habilita la colección de contadores de rendimiento Kudu.|

## <a name="cosmosdb"></a>cosmosDb

Las opciones de configuración se pueden encontrar en los [desencadenadores y enlaces de Cosmos DB](functions-bindings-cosmosdb-v2.md#host-json).

## <a name="durabletask"></a>durableTask

Las opciones de configuración se puede encontrar en los [enlaces para Durable Functions](durable/durable-functions-bindings.md#host-json).

## <a name="eventhub"></a>eventHub

Las opciones de configuración se pueden encontrar en [desencadenadores y enlaces del centro de eventos](functions-bindings-event-hubs.md#host-json). 

## <a name="extensions"></a>extensions

Propiedad que devuelve un objeto que contiene todas las configuraciones específicas de enlace, como [http](#http) y [eventHub](#eventhub).

## <a name="extensionbundle"></a>extensionBundle 

Las agrupaciones de extensiones permiten agregar un conjunto compatible de extensiones de enlace de Functions a la aplicación de función. Para obtener más información, consulte [Agrupaciones de extensiones para el desarrollo local](functions-bindings-register.md#extension-bundles).

[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

## <a name="functions"></a>functions

Lista de las funciones que el host de trabajo ejecuta. Una matriz vacía significa ejecutar todas las funciones. Su uso está previsto solo cuando se [ejecuta localmente](functions-run-local.md). En cambio, en las aplicaciones de función de Azure, siga los pasos de [Deshabilitamiento de funciones en Azure Functions](disable-function.md) para deshabilitar funciones específicas en lugar de usar esta configuración.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Indica la duración del tiempo de espera para todas las funciones. Sigue el formato de cadena TimeSpan. En un plan de consumo sin servidor, el intervalo válido es de 1 segundo a 10 minutos, y el valor predeterminado es 5 minutos.  

En el plan Premium, el intervalo válido es de entre 1 segundo y 60 minutos, y el valor predeterminado es 30 minutos.

En un plan de App Service dedicado, no hay ningún límite general y el valor predeterminado es 30 minutos. Un valor de `-1` indica una ejecución no vinculada, pero se recomienda mantener un límite superior fijo.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Configuración del [monitor de estado de host](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|enabled|true|Especifica si está habilitada la característica. | 
|healthCheckInterval|10 segundos|El intervalo de tiempo entre las comprobaciones periódicas de mantenimiento en segundo plano. | 
|healthCheckWindow|2 minutes|Una ventana de tiempo deslizante usada en combinación con el valor `healthCheckThreshold`.| 
|healthCheckThreshold|6|Número máximo de veces que puede producirse un error en la comprobación de mantenimiento antes de que se inicie un reciclaje del host.| 
|counterThreshold|0.80|El umbral en el que un contador de rendimiento se considerará incorrecto.| 

## <a name="http"></a>http

Las opciones de configuración se pueden encontrar en los [desencadenadores y enlaces http](functions-bindings-http-webhook.md#hostjson-settings).

## <a name="logging"></a>logging

Controla los comportamientos de registro de la aplicación de función, Application Insights incluido.

```json
"logging": {
    "fileLoggingMode": "debugOnly"
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "console": {
        ...
    },
    "applicationInsights": {
        ...
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------|
|fileLoggingMode|debugOnly|Define qué nivel de registro de archivos está habilitado.  Las opciones son `never`, `always`, `debugOnly`. |
|logLevel|N/D|Objeto que define el filtrado por categoría de registro para las funciones de la aplicación. En las versiones 2.x y posteriores se sigue el diseño de filtrado por categoría de registro de ASP.NET Core. Esto permite el filtrado del registro de funciones específicas. Para más información, consulte [Filtrado del registro](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) en la documentación de ASP.NET Core. |
|console|N/D| Configuración del registro de [consola](#console). |
|applicationInsights|N/D| Configuración de [applicationInsights](#applicationinsights). |

## <a name="console"></a>console

Esta configuración es un elemento secundario de [logging](#logging). Controla el registro de la consola cuando no está en modo de depuración.

```json
{
    "logging": {
    ...
        "console": {
          "isEnabled": "false"
        },
    ...
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|isEnabled|false|Habilita o deshabilita el registro de la consola.| 

## <a name="manageddependency"></a>managedDependency

La dependencia administrada es una característica en versión preliminar que actualmente solo se admite con funciones basadas en PowerShell. Permite que el servicio administre de forma automática las dependencias. Cuando la propiedad `enabled` está establecida en `true`, se procesa el archivo `requirements.psd1`. Las dependencias se actualizarán cuando se publique alguna versión secundaria. Para obtener más información, lea [Dependencia administrada](functions-reference-powershell.md#dependency-management) en el artículo de PowerShell.

```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

## <a name="queues"></a>queues

Las opciones de configuración se pueden encontrar en los [desencadenadores y enlaces de la cola de Storage](functions-bindings-storage-queue.md#host-json).  

## <a name="sendgrid"></a>sendGrid

Las opciones de configuración se pueden encontrar en los [desencadenadores y enlaces de SendGrid](functions-bindings-sendgrid.md#host-json).

## <a name="servicebus"></a>serviceBus

Las opciones de configuración se pueden encontrar en los [desencadenadores y enlaces de Service Bus](functions-bindings-service-bus.md#host-json).

## <a name="singleton"></a>singleton

Opciones de configuración para el comportamiento de bloqueo Singleton. Para más información, consulte [problema de compatibilidad de GitHub con singleton](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|lockPeriod|00:00:15|Período durante el cual se producen los bloqueos de nivel de función. Los bloqueos se renuevan automáticamente.| 
|listenerLockPeriod|00:01:00|Período durante el cual se producen los bloqueos de agente de escucha.| 
|listenerLockRecoveryPollingInterval|00:01:00|Intervalo de tiempo que se utiliza para la recuperación de bloqueos del agente de escucha si no se pudo adquirir un bloqueo de agente de escucha durante el inicio.| 
|lockAcquisitionTimeout|00:01:00|Cantidad máxima de tiempo que el entorno en tiempo de ejecución intentará adquirir un bloqueo.| 
|lockAcquisitionPollingInterval|N/D|Intervalo entre intentos de adquisición de bloqueo.| 

## <a name="version"></a>version

La cadena de versión `"version": "2.0"` es necesaria para una aplicación de función que tenga como destino la versión v2 del entorno de ejecución.

## <a name="watchdirectories"></a>watchDirectories

Conjunto de [directorios de código compartido](functions-reference-csharp.md#watched-directories) en los que se deben supervisar los cambios.  Garantiza que cuando se cambie el código en estos directorios, las funciones recibirán los cambios.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Vea cómo actualizar el archivo host.json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Consulte las opciones de configuración global en las variables de entorno](functions-app-settings.md)
