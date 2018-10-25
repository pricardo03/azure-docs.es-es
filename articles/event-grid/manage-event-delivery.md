---
title: Mensajes fallidos y directivas de reintento para suscripciones de Azure Event Grid
description: Describe la personalización de las opciones de entrega de eventos para Event Grid. Defina un destino de mensajes fallidos y especifique el tiempo para reintentar la entrega.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: fcf3ecaff6e8ba1421496a96d01428946cf8ab8e
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077789"
---
# <a name="dead-letter-and-retry-policies"></a>Mensajes fallidos y directivas de reintento

Cuando crea una suscripción a eventos, puede personalizar la configuración de entrega de estos. Este artículo muestra cómo configurar una ubicación de la cola de mensajes fallidos y personalizar la configuración de reintentos. Para obtener información acerca de estas características, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Establecimiento de una ubicación para los eventos fallidos

Para establecer una ubicación de la cola de mensajes fallidos, se necesita una cuenta de almacenamiento para mantener los eventos que no se pueden entregar a un punto de conexión. El siguiente script obtiene el identificador de recurso de una cuenta de almacenamiento existente y crea una suscripción a eventos que utiliza un contenedor de esa cuenta de almacenamiento como punto de conexión para los eventos fallidos.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

storagename=demostorage
containername=testcontainer

storageid=$(az storage account show --name $storagename --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Para desactivar las colas de mensajes fallidos, vuelva a ejecutar el comando para crear la suscripción de eventos, pero no proporcione un valor para `deadletter-endpoint`. No es necesario eliminar la suscripción de eventos.

## <a name="set-retry-policy"></a>Establecimiento de la directiva de reintentos

Al crear una suscripción a Event Grid, puede establecer durante cuanto tiempo debe intentar Event Grid entregar el evento. De forma predeterminada, Event Grid lo intentará durante 24 horas (1440 minutos) y un máximo de 30 veces. Puede establecer cualquiera de estos valores para la suscripción a Event Grid. El valor del período de vida del evento debe ser un entero entre 1 y 1440. El valor del número máximo de intentos de entrega debe ser un entero entre 1 y 30.

No se puede configurar la [programación de reintentos](delivery-and-retry.md#retry-schedule-and-duration).

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

Si configura `event-ttl` y `max-deliver-attempts`, Event Grid usará el primero para finalizar el número de reintentos.

## <a name="next-steps"></a>Pasos siguientes

* Para una aplicación de ejemplo que usa una aplicación de función de Azure para procesar los eventos de mensajes fallidos, consulte [Ejemplos de mensajes fallidos de Azure Event Grid para .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/).
* Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).
* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).
