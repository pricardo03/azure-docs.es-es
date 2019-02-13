---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a3af711503445000d9613feb2eec7967442fe538
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55736119"
---
Configuración de [Durable Functions](../articles/azure-functions/durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub",
    "ControlQueueBatchSize": 32,
    "PartitionCount": 4,
    "ControlQueueVisibilityTimeout": "00:05:00",
    "WorkItemQueueVisibilityTimeout": "00:05:00",
    "MaxConcurrentActivityFunctions": 10,
    "MaxConcurrentOrchestratorFunctions": 10,
    "AzureStorageConnectionStringName": "AzureWebJobsStorage",
    "TraceInputsAndOutputs": false,
    "LogReplayEvents": false,
    "EventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "EventGridKeySettingName":  "EventGridKey",
    "EventGridPublishRetryCount": 3,
    "EventGridPublishRetryInterval": "00:00:30"
  }
}
```

Los nombres de la central de tareas deben empezar por una letra y estar formados únicamente por letras y números. Si no se especifica, el nombre predeterminado de la central de tareas de la aplicación de función es **DurableFunctionsHub**. Para más información, consulte el artículo sobre las [centrales de tareas](../articles/azure-functions/durable-functions-task-hubs.md).

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Se pueden usar nombres de [central de tareas](../articles/azure-functions/durable-functions-task-hubs.md) alternativos para aislar varias aplicaciones de Durable Functions unas de otras, incluso si usan el mismo back-end de almacenamiento.|
|ControlQueueBatchSize|32|El número de mensajes que se van a extraer a la vez de la cola de control.|
|PartitionCount |4|El recuento de particiones para la cola de control. Puede ser un entero positivo comprendido entre 1 y 16.|
|ControlQueueVisibilityTimeout |5 minutos|El tiempo de espera de visibilidad de los mensajes de la cola de control quitados de la cola.|
|WorkItemQueueVisibilityTimeout |5 minutos|El tiempo de espera de visibilidad de los mensajes de la cola de elementos de trabajo quitados de la cola.|
|MaxConcurrentActivityFunctions |10 veces el número de procesadores en la máquina actual.|El número máximo de funciones de actividad que se pueden procesar simultáneamente en una única instancia de host.|
|MaxConcurrentOrchestratorFunctions |10 veces el número de procesadores en la máquina actual.|El número máximo de funciones de Orchestrator que se pueden procesar simultáneamente en una única instancia de host.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|El nombre de la configuración de aplicación que tiene la cadena de conexión de Azure Storage que se usa para administrar los recursos subyacentes de Azure Storage.|
|TraceInputsAndOutputs |false|Un valor que indica si se realizará el seguimiento de las entradas y salidas de las llamadas de función. El comportamiento predeterminado al realizar el seguimiento de eventos de ejecución de funciones es incluir el número de bytes en las entradas y salidas serializadas de las llamadas de función. Este comportamiento proporciona información mínima sobre el aspecto de las entradas y salidas, sin sobredimensionar los registros o exponer por accidente información confidencial a los registros. Al establecer esta propiedad en true, el registro de funciones predeterminado registra todo el contenido de las entradas y salidas de función.|
|LogReplayEvents|false|Un valor que indica si se debe escribir eventos de reproducción de orquestación en Application Insights.|
|EventGridTopicEndpoint ||La dirección URL de un punto de conexión de tema personalizado de Azure Event Grid. Cuando se establece esta propiedad, se publican eventos de notificación del ciclo de vida de orquestación en este punto de conexión. Esta propiedad admite la resolución de la configuración de la aplicación.|
|EventGridKeySettingName ||El nombre de la configuración de aplicación que contiene la clave usada para autenticarse con el tema personalizado de Azure Event Grid en `EventGridTopicEndpoint`.|
|EventGridPublishRetryCount|0|El número de reintentos, si, al publicar en el tema de Event Grid, se produce un error.|
|EventGridPublishRetryInterval|5 minutos|Event Grid publica el intervalo de reintento en formato *hh:mm:ss*.|

Muchas de estas propiedades son para optimizar el rendimiento. Para más información, consulte [Rendimiento y escalado](../articles/azure-functions/durable-functions-perf-and-scale.md).