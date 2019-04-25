---
title: Creación de un centro de eventos con PowerShell (Azure Event Hubs) | Microsoft Docs
description: En esta guía de inicio rápido se describe cómo crear un centro de eventos mediante Azure PowerShell y, después, enviar y recibir eventos mediante el SDK de .NET Standard.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: fba0d3ac5a20bd4f10b815450f6ce08e50f88ea0
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681308"
---
# <a name="quickstart-create-an-event-hub-using-azure-powershell"></a>Inicio rápido: Creación de un centro de eventos mediante Azure PowerShell

Azure Event Hubs es una plataforma de streaming de macrodatos y servicio de ingesta de eventos de gran escalabilidad capaz de recibir y procesar millones de eventos por segundo. Event Hubs puede procesar y almacenar eventos, datos o telemetría generados por dispositivos y software distribuido. Los datos enviados a un centro de eventos se pueden transformar y almacenar con cualquier proveedor de análisis en tiempo real o adaptadores de procesamiento por lotes y almacenamiento. Para más información sobre Event Hubs, consulte [Introducción a Event Hubs](event-hubs-about.md) y [Características de Event Hubs](event-hubs-features.md).

En esta guía de inicio rápido se crea un centro de eventos mediante Azure PowerShell.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para completar este tutorial, asegúrese de disponer de los siguientes elementos:

- Suscripción de Azure. Si no tiene una, [cree una cuenta gratuita][] antes de empezar.
- [Visual Studio 2017 Update 3 (versión 15.3, 26730.01)](https://www.visualstudio.com/vs) o posterior.
- [SDK de .NET Standard](https://www.microsoft.com/net/download/windows), versión 2.0 o posterior.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si está usando PowerShell localmente, tiene que ejecutar la versión más reciente de PowerShell para completar esta guía de inicio rápido. Si necesita instalarlo o actualizarlo, consulte [Instalación y configuración de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos es una recopilación lógica de recursos de Azure, para crear un centro de eventos necesita un grupo de recursos. 

En el siguiente ejemplo, se crea un grupo de recursos en la región Este de EE. UU. Sustituya `myResourceGroup` por el nombre del grupo de recursos que quiere utilizar:

```azurepowershell-interactive
New-AzResourceGroup –Name myResourceGroup –Location eastus
```

## <a name="create-an-event-hubs-namespace"></a>Creación de un espacio de nombres de Event Hubs

Una vez que se crea el grupo de recursos, cree un espacio de nombres de Event Hubs dentro de ese grupo de recursos. Un espacio de nombres de Event Hubs proporciona un nombre de dominio completo y exclusivo en el que puede crear el centro de eventos. Reemplace `namespace_name` por un nombre único para el espacio de nombres:

```azurepowershell-interactive
New-AzEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

## <a name="create-an-event-hub"></a>Creación de un centro de eventos

Ahora que ya tiene un espacio de nombres de Event Hubs, cree un centro de eventos dentro de ese espacio de nombres:  
El período permitido para `MessageRetentionInDays` está comprendido entre 1 y 7 días.

```azurepowershell-interactive
New-AzEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name -MessageRetentionInDays 3
```

Felicidades. Ha usado Azure PowerShell para crear un espacio de nombres de Event Hubs y un centro de eventos dentro de ese espacio de nombres. 

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado el espacio de nombres de Event Hubs y ha usado aplicaciones de ejemplo para enviar eventos al centro de eventos y recibirlos de este. Para encontrar instrucciones paso a paso sobre cómo enviar eventos a un centro de eventos o recibirlos de este, consulte los tutoriales sobre **envío y recepción de eventos**: 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (solo enviar)](event-hubs-c-getstarted-send.md)
- [Apache Storm (solo recibir)](event-hubs-storm-getstarted-receive.md)


[cree una cuenta gratuita]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-az-ps
[New-AzResourceGroup]: https://docs.microsoft.com/powershell/module/az.resources/new-Azresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
