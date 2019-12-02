---
title: Enlaces de Azure Event Hubs para Azure Functions
description: Descubra cómo usar los enlaces de Azure Event Hubs de Azure Functions.
author: craigshoemaker
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.topic: reference
ms.date: 11/08/2017
ms.author: cshoe
ms.openlocfilehash: 2e76853a7b1bf2e6dfda84ffa1454074c266d2c1
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227279"
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
