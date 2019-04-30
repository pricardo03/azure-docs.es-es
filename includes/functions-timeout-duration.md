---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
origin.date: 02/21/2018
ms.date: 03/25/2019
ms.author: v-junlch
ms.custom: include file
ms.openlocfilehash: ffb29fc76313e8870b52cb0a63936da7853ea6ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036312"
---
## <a name="timeout"></a>Duración de tiempo de espera de aplicación de función 

La duración de tiempo de espera de una aplicación de función se define mediante la propiedad functionTimeout en el [host.json](../articles/azure-functions/functions-host-json.md#functiontimeout) archivo de proyecto. En la siguiente tabla muestra los valores predeterminado y máximo en minutos para ambos planes y en ambas versiones en tiempo de ejecución:

| Plan | Versión en tiempo de ejecución | Valor predeterminado | Máxima |
|------|---------|---------|---------|
| Consumo | 1.x | 5 | 10 |
| Consumo | 2.x | 5 | 10 |
| App Service | 1.x | Ilimitado | Ilimitado |
| App Service | 2.x | 30 | Ilimitado |

> [!NOTE] 
> Independientemente de la configuración de tiempo de espera de aplicación de función, 230 segundos es la cantidad máxima de tiempo que puede llevar a una función desencadenada por HTTP para responder a una solicitud. Esto es debido el [predeterminado de tiempo de espera de inactividad del equilibrador de carga de Azure](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Para los tiempos de procesamiento más largos, considere el uso de la [patrón asincrónico de Durable Functions](../articles/azure-functions/durable/durable-functions-concepts.md#async-http) o [aplazar el trabajo real y devolver una respuesta inmediata](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions).

