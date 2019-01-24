---
title: Mensajes fallidos y directivas de reintento para suscripciones de Azure Event Grid
description: Describe la personalización de las opciones de entrega de eventos para Event Grid. Defina un destino de mensajes fallidos y especifique el tiempo para reintentar la entrega.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/06/2019
ms.author: spelluru
ms.openlocfilehash: 8660f09c41cf6226f2ffb173508d37c260522b80
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474258"
---
# <a name="dead-letter-and-retry-policies"></a>Mensajes fallidos y directivas de reintento

Cuando crea una suscripción a eventos, puede personalizar la configuración de entrega de estos. Este artículo muestra cómo configurar una ubicación de la cola de mensajes fallidos y personalizar la configuración de reintentos. Para obtener información acerca de estas características, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).

## <a name="install-preview-feature"></a>Instalación de la característica en vista previa

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Establecimiento de una ubicación para los eventos fallidos

Para establecer una ubicación de la cola de mensajes fallidos, se necesita una cuenta de almacenamiento para mantener los eventos que no se pueden entregar a un punto de conexión. Los ejemplos obtienen el identificador de recurso de una cuenta de almacenamiento existente. Crean una suscripción de eventos que usa un contenedor en esa cuenta de almacenamiento para el punto de conexión de mensajes con problemas de entrega.

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

containername=testcontainer

topicid=$(az eventgrid topic show --name demoTopic -g gridResourceGroup --query id --output tsv)
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Para desactivar las colas de mensajes fallidos, vuelva a ejecutar el comando para crear la suscripción de eventos, pero no proporcione un valor para `deadletter-endpoint`. No es necesario eliminar la suscripción de eventos.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$containername = "testcontainer"

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzureRmStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

Para desactivar las colas de mensajes fallidos, vuelva a ejecutar el comando para crear la suscripción de eventos, pero no proporcione un valor para `DeadLetterEndpoint`. No es necesario eliminar la suscripción de eventos.

## <a name="set-retry-policy"></a>Establecimiento de la directiva de reintentos

Al crear una suscripción a Event Grid, puede establecer durante cuanto tiempo debe intentar Event Grid entregar el evento. De manera predeterminada, Event Grid lo intenta durante 24 horas (1440 minutos), o 30 veces. Puede establecer cualquiera de estos valores para la suscripción a Event Grid. El valor del período de vida del evento debe ser un entero entre 1 y 1440. El valor de reintentos máximos debe ser un entero entre 1 y 30.

No se puede configurar la [programación de reintentos](delivery-and-retry.md#retry-schedule-and-duration).

### <a name="azure-cli"></a>Azure CLI

Para establecer el período de vida del evento en otro valor distinto de 1440 minutos, use:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

Para establecer el número máximo de reintentos en otro valor distinto a 30, use:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Si establece `event-ttl` y `max-deliver-attempts`, Event Grid usa la primera opción más próxima a la expiración para determinar cuándo se debe detener la entrega de eventos.

### <a name="powershell"></a>PowerShell

Para establecer el período de vida del evento en otro valor distinto de 1440 minutos, use:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

Para establecer el número máximo de reintentos en otro valor distinto a 30, use:

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -MaxDeliveryAttempt 18
```

Si establece `EventTtl` y `MaxDeliveryAttempt`, Event Grid usa la primera opción más próxima a la expiración para determinar cuándo se debe detener la entrega de eventos.

## <a name="next-steps"></a>Pasos siguientes

* Para una aplicación de ejemplo que usa una aplicación de función de Azure para procesar los eventos de mensajes fallidos, consulte [Ejemplos de mensajes fallidos de Azure Event Grid para .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/).
* Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).
* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).
