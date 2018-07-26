---
title: Administración de la configuración de entrega para suscripciones de Azure Event Grid
description: Describe la personalización de las opciones de entrega de eventos para Event Grid.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: tomfitz
ms.openlocfilehash: e91ee640d18e2cf804be33fd130bf48737c9efb1
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035676"
---
# <a name="manage-event-grid-delivery-settings"></a>Administración de la configuración de entrega de Event Grid

Cuando crea una suscripción a eventos, puede personalizar la configuración de entrega de estos. Puede establecer cuanto tiempo debe intentar Event Grid entregar el mensaje. Puede establecer una cuenta de almacenamiento para almacenar aquellos eventos que no se pueden entregar en un punto de conexión.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-retry-policy"></a>Establecimiento de la directiva de reintentos

Al crear una suscripción a Event Grid, puede establecer durante cuanto tiempo debe intentar Event Grid entregar el evento. De forma predeterminada, Event Grid lo intentará durante 24 horas (1440 minutos) y un máximo de 30 veces. Puede establecer cualquiera de estos valores para la suscripción a Event Grid.

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

## <a name="set-dead-letter-location"></a>Establecimiento de una ubicación para los eventos fallidos

Si Event Grid no puede entregar un evento, puede enviar el evento no entregado a una cuenta de almacenamiento. Este proceso se conoce como colas de eventos fallidos. De forma predeterminada, Event Grid no tiene activada esta opción. Para habilitarla, debe especificar una cuenta de almacenamiento para contener los eventos no entregados al crear la suscripción a eventos. Puede extraer eventos de esta cuenta de almacenamiento para resolver las entregas.

Event Grid enviará un evento a la ubicación de eventos fallidos si ha realizado el número máximo de reintentos o si recibe un mensaje de error que indica que la entrega nunca se realizará correctamente. Por ejemplo, si Event Grid recibe un error de formato incorrecto al entregar un evento, enviará inmediatamente ese evento a la ubicación de eventos fallidos.

Antes de establecer la ubicación de eventos fallidos, debe tener una cuenta de almacenamiento con un contenedor. Puede proporcionar el punto de conexión de este contenedor al crear la suscripción a eventos. El punto de conexión tiene este formato: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

El siguiente script obtiene el identificador de recurso de una cuenta de almacenamiento existente y crea una suscripción a eventos que utiliza un contenedor de esa cuenta de almacenamiento como punto de conexión para los eventos fallidos.

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

Para utilizar Event Grid para responder a eventos no entregados, [cree una suscripción a eventos](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) para el almacenamiento de blobs de los eventos fallidos. Cada vez que el almacenamiento de blobs de eventos fallidos recibe un evento no entregado, Event Grid lo notifica al controlador. El controlador responderá con las acciones que debe realizar para conciliar los eventos no entregados. 

Para desactivar las colas de mensajes fallidos, vuelva a ejecutar el comando para crear la suscripción de eventos, pero no proporcione un valor para `deadletter-endpoint`. No es necesario eliminar la suscripción de eventos.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información acerca de los reintentos y las entregas de eventos, consulte [Entrega y reintento de entrega de mensajes de Event Grid](delivery-and-retry.md).
* Para obtener una introducción a Event Grid, vea [Acerca de Event Grid](overview.md).
* Para comenzar a usar rápidamente Event Grid, vea [Creación y enrutamiento de eventos personalizados con Azure Event Grid](custom-event-quickstart.md).
