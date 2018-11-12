---
title: Referencia de host.json para Azure Functions 2.x
description: Documentación de referencia para el archivo host.json de Azure Functions con el entorno en tiempo de ejecución de la versión 2.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/08/2018
ms.author: glenga
ms.openlocfilehash: d794648d3af086263ccffc782f3f3fdf6456eacc
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51013384"
---
# <a name="hostjson-reference-for-azure-functions-2x"></a>Referencia de host.json para Azure Functions 2.x  

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Versión 1](functions-host-json-v1.md)
> * [Versión 2](functions-host-json.md)

El archivo de metadatos *host.json* contiene las opciones de configuración global que afectan a todas las funciones de dicha aplicación de función. En este artículo se incluye una lista de las opciones de configuración disponibles para el entorno en tiempo de ejecución de la versión 2.  

> [!NOTE]
> Este artículo trata sobre Azure Functions 2.x.  Para obtener una referencia de host.json en Functions 1.x, consulte la [referencia de host.json para Azure Functions, versión 1.x](functions-host-json-v1.md).

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
            "sampling": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 5
            }
        }
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

[!INCLUDE [applicationInsights](../../includes/functions-host-json-applicationinsights.md)]

## <a name="cosmosdb"></a>cosmosDb

Las opciones de configuración se pueden encontrar en los [desencadenadores y enlaces de Cosmos DB](functions-bindings-cosmosdb-v2.md#host-json).

## <a name="durabletask"></a>durableTask

Las opciones de configuración se puede encontrar en los [enlaces para Durable Functions](durable-functions-bindings.md#host-json).

## <a name="eventhub"></a>eventHub

Las opciones de configuración se pueden encontrar en [desencadenadores y enlaces del centro de eventos](functions-bindings-event-hubs.md#host-json). 

## <a name="extensions"></a>extensions

Propiedad que devuelve un objeto que contiene todas las configuraciones específicas de enlace, como [http](#http) y [eventHub](#eventhub).

## <a name="functions"></a>functions

Lista de las funciones que el host de trabajo ejecuta. Una matriz vacía significa ejecutar todas las funciones. Su uso está previsto solo cuando se [ejecuta localmente](functions-run-local.md). En cambio, en las aplicaciones de función de Azure, siga los pasos de [Deshabilitamiento de funciones en Azure Functions](disable-function.md) para deshabilitar funciones específicas en lugar de usar esta configuración.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Indica la duración del tiempo de espera para todas las funciones. En un plan de consumo sin servidor, el intervalo válido es de 1 segundo a 10 minutos, y el valor predeterminado es 5 minutos. En un plan de App Service, no hay límite total y el valor predeterminado depende de la versión del entorno de ejecución. En la versión 2.x, el valor predeterminado en un plan de App Service es 30 minutos. En la versión 1.x, es *null*, lo que indica que no hay tiempo de espera.

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
|Enabled|true|Especifica si está habilitada la característica. | 
|healthCheckInterval|10 segundos|El intervalo de tiempo entre las comprobaciones periódicas de mantenimiento en segundo plano. | 
|healthCheckWindow|2 minutes|Una ventana de tiempo deslizante usada en combinación con el valor `healthCheckThreshold`.| 
|healthCheckThreshold|6|Número máximo de veces que puede producirse un error en la comprobación de mantenimiento antes de que se inicie un reciclaje del host.| 
|counterThreshold|0.80|El umbral en el que un contador de rendimiento se considerará incorrecto.| 

## <a name="http"></a>http

Las opciones de configuración se pueden encontrar en los [desencadenadores y enlaces http](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="logging"></a>logging

Controla los comportamientos de registro de la aplicación de función, Application Insights incluido.

```json
"logging": {
    "fileLoggingMode": "debugOnly",
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "applicationInsights": {
        ...
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------|
|fileLoggingMode|debugOnly|Define qué nivel de registro de archivos está habilitado.  Las opciones son `never`, `always`, `debugOnly`. |
|logLevel|N/D|Objeto que define el filtrado por categoría de registro para las funciones de la aplicación. En la versión 2.x se sigue el diseño de filtrado por categoría de registro de ASP.NET Core. Esto permite el filtrado del registro de funciones específicas. Para más información, consulte [Filtrado del registro](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) en la documentación de ASP.NET Core. |
|applicationInsights|N/D| Configuración de [applicationInsights](#applicationinsights). |

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
