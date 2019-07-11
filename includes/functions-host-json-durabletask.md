---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 03/14/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 2e30184c7273fad2f9bc8adb34834ee14840733b
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608368"
---
Configuración de [Durable Functions](../articles/azure-functions/durable-functions-overview.md).

```json
{
  "durableTask": {
    "hubName": "MyTaskHub",
    "controlQueueBatchSize": 32,
    "partitionCount": 4,
    "controlQueueVisibilityTimeout": "00:05:00",
    "workItemQueueVisibilityTimeout": "00:05:00",
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
    "maxQueuePollingInterval": "00:00:30",
    "azureStorageConnectionStringName": "AzureWebJobsStorage",
    "trackingStoreConnectionStringName": "TrackingStorage",
    "trackingStoreNamePrefix": "DurableTask",
    "traceInputsAndOutputs": false,
    "logReplayEvents": false,
    "eventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName":  "EventGridKey",
    "eventGridPublishRetryCount": 3,
    "eventGridPublishRetryInterval": "00:00:30",
    "eventGridPublishEventTypes": ["Started", "Completed", "Failed", "Terminated"]
  }
}
```

Los nombres de la central de tareas deben empezar por una letra y estar formados únicamente por letras y números. Si no se especifica, el nombre predeterminado de la central de tareas de la aplicación de función es **DurableFunctionsHub**. Para más información, consulte el artículo sobre las [centrales de tareas](../articles/azure-functions/durable-functions-task-hubs.md).

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------|
|hubName|DurableFunctionsHub|Se pueden usar nombres de [central de tareas](../articles/azure-functions/durable-functions-task-hubs.md) alternativos para aislar varias aplicaciones de Durable Functions unas de otras, incluso si usan el mismo back-end de almacenamiento.|
|controlQueueBatchSize|32|El número de mensajes que se van a extraer a la vez de la cola de control.|
|partitionCount |4|El recuento de particiones para la cola de control. Puede ser un entero positivo comprendido entre 1 y 16.|
|controlQueueVisibilityTimeout |5 minutos|El tiempo de espera de visibilidad de los mensajes de la cola de control quitados de la cola.|
|workItemQueueVisibilityTimeout |5 minutos|El tiempo de espera de visibilidad de los mensajes de la cola de elementos de trabajo quitados de la cola.|
|maxConcurrentActivityFunctions |10 veces el número de procesadores en la máquina actual.|El número máximo de funciones de actividad que se pueden procesar simultáneamente en una única instancia de host.|
|maxConcurrentOrchestratorFunctions |10 veces el número de procesadores en la máquina actual.|El número máximo de funciones de Orchestrator que se pueden procesar simultáneamente en una única instancia de host.|
|maxQueuePollingInterval|30 segundos|Intervalo de sondeo de cola de elementos de trabajo y control máximo en formato *hh:mm:ss:* . Los valores más altos pueden provocar mayores latencias de procesamiento de mensajes. Los valores más bajos pueden provocar mayores costos de almacenamiento debido a transacciones de almacenamiento mayor.|
|azureStorageConnectionStringName |AzureWebJobsStorage|El nombre de la configuración de aplicación que tiene la cadena de conexión de Azure Storage que se usa para administrar los recursos subyacentes de Azure Storage.|
|trackingStoreConnectionStringName||Nombre de una cadena de conexión que se usará para las tablas de historial e instancias. Si no se especifica, se usa la conexión `azureStorageConnectionStringName`.|
|trackingStoreNamePrefix||Prefijo que se usará para las tablas de historial e instancias cuando se especifica `trackingStoreConnectionStringName`. Si no se establece, el valor de prefijo predeterminado será `DurableTask`. Si no se especifica `trackingStoreConnectionStringName`, las tablas de historial e instancias usarán el valor `hubName` como prefijo y se pasarán por alto todos los valores de configuración de `trackingStoreNamePrefix`.|
|traceInputsAndOutputs |false|Un valor que indica si se realizará el seguimiento de las entradas y salidas de las llamadas de función. El comportamiento predeterminado al realizar el seguimiento de eventos de ejecución de funciones es incluir el número de bytes en las entradas y salidas serializadas de las llamadas de función. Este comportamiento proporciona información mínima sobre el aspecto de las entradas y salidas, sin sobredimensionar los registros o exponer por accidente información confidencial. Al establecer esta propiedad en true, el registro de funciones predeterminado registra todo el contenido de las entradas y salidas de función.|
|logReplayEvents|false|Un valor que indica si se debe escribir eventos de reproducción de orquestación en Application Insights.|
|eventGridTopicEndpoint ||La dirección URL de un punto de conexión de tema personalizado de Azure Event Grid. Cuando se establece esta propiedad, se publican eventos de notificación del ciclo de vida de orquestación en este punto de conexión. Esta propiedad admite la resolución de la configuración de la aplicación.|
|eventGridKeySettingName ||El nombre de la configuración de aplicación que contiene la clave usada para autenticarse con el tema personalizado de Azure Event Grid en `EventGridTopicEndpoint`.|
|eventGridPublishRetryCount|0|El número de reintentos, si, al publicar en el tema de Event Grid, se produce un error.|
|eventGridPublishRetryInterval|5 minutos|Event Grid publica el intervalo de reintento en formato *hh:mm:ss*.|
|eventGridPublishEventTypes||Lista de tipos de eventos que se van a publicar en Event Grid. Si no se especifica, se publicarán todos los tipos de evento. Los valores permitidos son `Started`, `Completed`, `Failed` y `Terminated`.|

Muchos de estos valores son para optimizar el rendimiento. Para más información, consulte [Rendimiento y escalado](../articles/azure-functions/durable-functions-perf-and-scale.md).
