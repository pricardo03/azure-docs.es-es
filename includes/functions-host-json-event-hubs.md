---
ms.openlocfilehash: b5d8f67a70961aab21312b6f241081dcb33f66fb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104456"
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

