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
ms.openlocfilehash: da0cbab59d9c801419ac4b3704fee3275f337fd9
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186469"
---
Especifica cuántas llamadas a funciones se agregan cuando se [calculan las métricas para Application Insights](../articles/azure-functions/functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Propiedad |Valor predeterminado  | DESCRIPCIÓN |
|---------|---------|---------| 
|batchSize|1000|Número máximo de solicitudes para agregar.| 
|flushTimeout|00:00:30|Período máximo de tiempo para agregar.| 

Las llamadas a funciones se agregan cuando se alcanza el primero de los dos límites.