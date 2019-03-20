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
ms.openlocfilehash: 189683a9e98f161ce537284cc7b0349c94be2bf0
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57410917"
---
## <a name="timeout"></a>Duración de tiempo de espera de aplicación de función 

La duración de tiempo de espera de una aplicación de función se define mediante la propiedad functionTimeout en el [host.json](../articles/azure-functions/functions-host-json.md#functiontimeout) archivo de proyecto. En la tabla siguiente se muestra los valores predeterminado y máximo para ambos planes y en ambas versiones en tiempo de ejecución:

| Plan | Versión en tiempo de ejecución | Valor predeterminado | Máxima |
|------|---------|---------|---------|
| Consumo | 1.x | 5 | 10 |
| Consumo | 2.x | 5 | 10 |
| App Service | 1.x | Ilimitado | Ilimitado |
| App Service | 2.x | 30 | Ilimitado |

> [!NOTE] 
> Independientemente de la configuración de tiempo de espera de aplicación de función, 230 segundos es la cantidad máxima de tiempo que puede llevar a una función desencadenada por HTTP para responder a una solicitud. Esto es debido el [predeterminado de tiempo de espera de inactividad del equilibrador de carga de Azure](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds). Para los tiempos de procesamiento más largos, considere el uso de la [patrón asincrónico de Durable Functions](../articles/azure-functions/durable/durable-functions-concepts.md#async-http) o [aplazar el trabajo real y devolver una respuesta inmediata](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions).
