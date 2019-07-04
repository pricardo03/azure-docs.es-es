---
title: Enlaces de Azure Event Hubs para Azure Functions
description: Descubra cómo usar los enlaces de Azure Event Hubs de Azure Functions.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: azure functions, funciones, procesamiento de eventos, proceso dinámico, arquitectura sin servidor
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/08/2017
ms.author: cshoe
ms.openlocfilehash: aa808bf333b35ce46a40a2fc8da7a2c8e78ac1aa
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480623"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Enlaces de Azure Event Hubs para Azure Functions

En este artículo se explica cómo usar enlaces de [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) para Azure Functions. Azure Functions admite enlaces de desencadenador y salida para Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paquetes: Functions 1.x

En Azure Functions versión 1.x, los enlaces de Event Hubs se proporcionan en el paquete NuGet [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus), versión 2.x.
El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) de GitHub.


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paquetes: Functions 2.x

En Functions 2.x, use el paquete [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs), versión 3.x.
El código fuente del paquete se encuentra en el repositorio [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) de GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre desencadenadores y enlaces de Azure Functions](functions-triggers-bindings.md)
