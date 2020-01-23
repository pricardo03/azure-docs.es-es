---
title: archivo de inclusión
description: archivo de inclusión
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a925076dfccd30c73febb2aadc8692667ea01525
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279592"
---
Controla la [característica de muestreo de Application Insights](../articles/azure-functions/functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Propiedad  |Valor predeterminado | Descripción |
|---------|---------|---------| 
|isEnabled|true|Habilita o deshabilita el muestreo.| 
|maxTelemetryItemsPerSecond|5|Umbral donde comienza el muestreo.| 
