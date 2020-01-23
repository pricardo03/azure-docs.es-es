---
title: archivo de inclusión
description: archivo de inclusión
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9c51ce726545d1c64d69c86c36fc69ea43c3b882
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279432"
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

|Propiedad |Valor predeterminado  | Descripción |
|---------|---------|---------| 
|batchSize|1000|Número máximo de solicitudes para agregar.| 
|flushTimeout|00:00:30|Período máximo de tiempo para agregar.| 

Las llamadas a funciones se agregan cuando se alcanza el primero de los dos límites.
