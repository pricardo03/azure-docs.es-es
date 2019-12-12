---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2018
ms.author: nzthiago
ms.custom: include file
ms.openlocfilehash: fded43bb655cefda508b82eca94522730ab6da00
ms.sourcegitcommit: b5ff5abd7a82eaf3a1df883c4247e11cdfe38c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74941742"
---
## <a name="timeout"></a>Duración del tiempo de espera de una aplicación de función 

La duración del tiempo de espera de una aplicación de función se define mediante la propiedad functionTimeout en el archivo de proyecto [host.json](../articles/azure-functions/functions-host-json.md#functiontimeout). En la tabla siguiente se muestran los valores predeterminados y máximos en minutos para ambos planes y en ambas versiones en tiempo de ejecución:

| Plan | Versión en tiempo de ejecución | Valor predeterminado | Máxima |
|------|---------|---------|---------|
| Consumo | 1.x | 5 | 10 |
| Consumo | 2.x | 5 | 10 |
| Consumo | 3.x | 5 | 10 |
| App Service | 1.x | Ilimitado | Ilimitado |
| App Service | 2.x | 30 | Ilimitado |
| App Service | 3.x | 30 | Ilimitado |

> [!NOTE] 
> Independientemente de la configuración del tiempo de espera de la aplicación de función, 230 segundos es la cantidad de tiempo máxima que una función desencadenada por HTTP puede tardar en responder a una solicitud. Esto se debe al [tiempo de espera de inactividad predeterminado de Azure Load Balancer](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Para tiempos de procesamiento más largos, considere la posibilidad de usar el [patrón asincrónico de Durable Functions](../articles/azure-functions/durable/durable-functions-overview.md#async-http) o [aplazar el trabajo real y devolver una respuesta inmediata](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions).
