---
title: 'Características en versión preliminar de Durable Functions: Azure Functions'
description: Obtenga información sobre las características en versión preliminar para Durable Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: article
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: 8f2560141eac4e7d9fed267d347990e65bfe9c26
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933240"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Versión preliminar de Durable Functions 2.0 (Azure Functions)

*Durable Functions* es una extensión de [Azure Functions](../functions-overview.md) y [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md) que le permite escribir funciones con estado en un entorno sin servidor. La extensión administra el estado, establece puntos de control y reinicia en su nombre. Si todavía no está familiarizado con Durable Functions, vea la [documentación de introducción](durable-functions-overview.md).

Durable Functions 1.x es una característica GA (disponible con carácter general) de Azure Functions, pero también contiene varias características secundarias que se encuentran actualmente en versión preliminar pública. En este artículo se describen las características de versión preliminar más recientes y se detalla cómo funcionan y cómo puede comenzar a usarlas.

> [!NOTE]
> Estas características en versión preliminar forman parte de una versión 2.0 de Durable Functions, que actualmente tiene **calidad de versión preliminar** con varios cambios importantes. En nuget.org puede encontrar compilaciones de paquete de la extensión Durable de Azure Functions, con el formato **2.0.0-betaX** para las versiones. Estas compilaciones no están diseñadas para cargas de trabajo de producción, y es posible que las versiones posteriores contengan cambios adicionales.

## <a name="breaking-changes"></a>Cambios drásticos

En Durable Functions 2.0 se han presentado varios cambios importantes. No se espera que las aplicaciones existentes sean compatibles con Durable Functions 2.0 sin cambios de código. En esta sección se enumeran algunos de los cambios:

### <a name="hostjson-schema"></a>Esquema de host.json

En el fragmento siguiente se muestra el nuevo esquema para host.json. Los principales cambios que se deben tener en cuenta son las nuevas subsecciones que se indican a continuación:

* `"storageProvider"` (y la subsección `"azureStorage"`) para la configuración específica del almacenamiento
* `"tracking"` para la configuración de seguimiento y registro
* `"notifications"` (y la subsección `"eventGrid"`) para la configuración de notificaciones de Event Grid

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
      "tracking": {
        "traceInputsAndOutputs": <bool?>,
        "traceReplayEvents": <bool?>,
      },
      "notifications": {
        "eventGrid": {
          "topicEndpoint": <string?>,
          "keySettingName": <string?>,
          "publishRetryCount": <string?>,
          "publishRetryInterval": <hh:mm:ss?>,
          "publishRetryHttpStatus": <int[]?>,
          "publishEventTypes": <string[]?>,
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "customLifeCycleNotificationHelperType": <string?>
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
| OrchestrationClientAttribute | DurableClientAttribute |

Si una clase base abstracta contiene métodos virtuales, estos métodos virtuales se han reemplazado por los métodos de extensión definidos en `DurableContextExtensions`.

## <a name="entity-functions"></a>Funciones de entidad

A partir de Durable Functions v2.0.0-alpha, incluimos un nuevo concepto de [funciones de entidad](durable-functions-entities.md).

Las funciones de entidad definen las operaciones de lectura y actualización de pequeños fragmentos de estado, denominados *entidades duraderas*. Al igual que las funciones de orquestador, las de entidad son funciones con un tipo especial de desencadenador, el *desencadenador de entidad*. A diferencia de las funciones de orquestador, las funciones de entidad no tienen restricciones de código específicas. Las funciones de entidad también administran el estado de forma explícita, en lugar de representarlo de forma implícita a través del flujo de control.

Basándonos en los primeros comentarios de los usuarios, agregamos posteriormente compatibilidad con un modelo de programación basado en clases para las entidades en Durable Functions v2.0.0-beta1.

Para obtener más información, consulte el artículo sobre las [funciones de entidad](durable-functions-entities.md).

## <a name="durable-http"></a>Durable HTTP

A partir de Durable Functions v2.0.0-beta2, incluimos una nueva característica denominada [Durable HTTP](durable-functions-http-features.md) que le permite:

* Llamar directamente a las API HTTP desde las funciones de orquestación (con algunas limitaciones documentadas).
* Implementar sondeos automáticos del estado HTTP 202 en el cliente.
* Compatibilidad integrada con [Azure Managed Identities](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

Para obtener más información, consulte el artículo sobre [características HTTP](durable-functions-http-features.md#consuming-http-apis).

## <a name="alternate-storage-providers"></a>Proveedores de almacenamiento alternativos

En la actualidad, Durable Task Framework admite varios proveedores de almacenamiento incluidos [Azure Storage](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [Azure Service Bus](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus), un [emulador en memoria](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator) y un proveedor [Redis](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis) experimental. Pero hasta ahora, la extensión Durable Task para Azure Functions solo admitía el proveedor Azure Storage. A partir de Durable Functions 2.0, se va a agregar compatibilidad con proveedores de almacenamiento alternativos, empezando por el proveedor de Redis.

> [!NOTE]
> Durable Functions 2.0 solo admite proveedores compatibles con .NET Standard 2.0.

### <a name="emulator"></a>Emulador

[DurableTask.Emulator](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/) es un proveedor de almacenamiento no duradero en memoria local adecuado para escenarios de pruebas locales. Se puede configurar mediante el esquema mínimo de **host.json** siguiente:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { "enabled": true }
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
> En la actualidad, el proveedor de Redis es experimental y solo es compatible con aplicaciones de función que se ejecutan en un único nodo. No se garantiza que el proveedor de Redis llegue a estar disponible con carácter general y podría quitarse en una versión futura.
