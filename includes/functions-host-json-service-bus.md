---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0a3f739bae3ec758c8e25d581d3e2b9feb4af5b1
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133642"
---
```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Propiedad  |Valor predeterminado | DESCRIPCIÓN |
|---------|---------|---------| 
|maxConcurrentCalls|16|Número máximo de llamadas simultáneas a la devolución de llamada que el bombeo de mensajes debe iniciar. De forma predeterminada, el entorno de ejecución de Functions procesa simultáneamente varios mensajes. Para indicar al entorno de ejecución que procese solo los mensajes de una única cola o tema, establezca `maxConcurrentCalls` en 1. | 
|prefetchCount|N/D|Valor predeterminado de PrefetchCount que utilizará el receptor de mensajes subyacente.| 
|autoRenewTimeout|00:05:00|Duración máxima dentro de la cual el bloqueo de mensajes se renovará automáticamente.| 
