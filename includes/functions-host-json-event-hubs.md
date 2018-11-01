---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: b5d8f67a70961aab21312b6f241081dcb33f66fb
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133802"
---
```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|maxBatchSize|64|Número máximo de eventos recibido por cada bucle de recepción.|
|prefetchCount|N/D|Valor predeterminado de PrefetchCount que utilizará el host de procesador de eventos subyacente.| 
|batchCheckpointFrequency|1|Número de lotes de eventos que se va a procesar antes de crear un punto de comprobación de cursor de EventHub.| 
