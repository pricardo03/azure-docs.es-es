---
title: Uso de Azure Event Grid con eventos del esquema CloudEvents
description: Se describe cómo establecer el esquema CloudEvents para eventos de Azure Event Grid.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: babanisa
ms.openlocfilehash: 78498f06a1ec921813d886b426d62452f850f5e5
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2020
ms.locfileid: "75922746"
---
# <a name="use-cloudevents-v10-schema-with-event-grid"></a>Uso del esquema CloudEvents v1.0 con Event Grid

Además de su [esquema de eventos predeterminado](event-schema.md), Azure Event Grid admite de forma nativa eventos de la [implementación de JSON de CloudEvents v1.0](https://github.com/cloudevents/spec/blob/v1.0/json-format.md) y el [enlace del protocolo HTTP](https://github.com/cloudevents/spec/blob/v1.0/http-protocol-binding.md). [CloudEvents](https://cloudevents.io/) es una [especificación abierta](https://github.com/cloudevents/spec/blob/v1.0/spec.md) para la descripción de datos de eventos.

CloudEvents simplifica la interoperabilidad al proporcionar un esquema de eventos común para la publicación y el consumo de eventos basados en la nube. Este esquema permite herramientas uniformes, formas estándar de enrutar y administrar eventos y formas universales de deserializar el esquema de eventos externo. Con un esquema común, puede integrar más fácilmente el trabajo entre plataformas.

En la compilación de CloudEvents participan varios [colaboradores](https://github.com/cloudevents/spec/blob/master/community/contributors.md), entre ellos Microsoft, mediante [Cloud Native Compute Foundation](https://www.cncf.io/). Actualmente está disponible en versión 1.0.

En este artículo se describe cómo usar el esquema CloudEvents con Event Grid.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Instalación de la característica en vista previa

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>Esquema CloudEvent

Este es un ejemplo de un evento de Azure Blob Storage en formato de CloudEvents:

``` JSON
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",  
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "https://gridtesting.blob.core.windows.net/testcontainer/{new-file}",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```

Una descripción detallada de los campos disponibles, sus tipos y definiciones en CloudEvents v1.0 se encuentra [disponible aquí](https://github.com/cloudevents/spec/blob/v1.0/spec.md#required-attributes).

Los valores de encabezados de los eventos proporcionados en el esquema de CloudEvents y el esquema de Event Grid son los mismos excepto para `content-type`. En el esquema de CloudEvents, ese valor de encabezado es `"content-type":"application/cloudevents+json; charset=utf-8"`. En el esquema de Event Grid, ese valor de encabezado es `"content-type":"application/json; charset=utf-8"`.

## <a name="configure-event-grid-for-cloudevents"></a>Configuración de Event Grid para CloudEvents

Puede usar Event Grid con eventos de entrada y salida de esquema CloudEvents. CloudEvents se puede usar con eventos del sistema, como eventos de Blob Storage y eventos de IoT Hub, y con eventos personalizados. También pueden transformar esos eventos en la red de un formato a otro.


| Esquema de entrada       | Esquema de salida
|--------------------|---------------------
| Formato de CloudEvents | Formato de CloudEvents
| Formato de Event Grid  | Formato de CloudEvents
| Formato de CloudEvents | Formato de Event Grid
| Formato de Event Grid  | Formato de Event Grid

Con todos los esquemas de eventos, Event Grid requiere validación al publicar en un tema de Event Grid y al crear una suscripción de eventos. Para más información, vea [Event Grid security and authentication](security-authentication.md) (Seguridad y autenticación de Event Grid).

### <a name="input-schema"></a>Esquema de entrada

El esquema de entrada de un tema personalizado se establece al crear este.

Para la CLI de Azure, utilice:

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventschemav1_0
```

Para PowerShell, use:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName gridResourceGroup `
  -Location westcentralus `
  -Name <topic_name> `
  -InputSchema CloudEventSchemaV1_0
```

La versión actual de CloudEvents no admite el procesamiento por lote de eventos. Para publicar eventos con el esquema de CloudEvent en un tema, publique cada evento individualmente.

### <a name="output-schema"></a>Esquema de salida

El esquema de salida se establece al crear la suscripción de eventos.

Para la CLI de Azure, utilice:

```azurecli-interactive
topicID=$(az eventgrid topic show --name <topic-name> -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --source-resource-id $topicID \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventschemav1_0
```

Para PowerShell, use:
```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeliverySchema CloudEventSchemaV1_0
```

 Actualmente, no puede usar un desencadenador de Event Grid para una aplicación de Azure Functions cuando el evento se entrega en el esquema de CloudEvents. Use un desencadenador HTTP. Para obtener ejemplos de la implementación de un desencadenador HTTP que recibe eventos en el esquema de CloudEvents, consulte [Uso de un desencadenador HTTP como un desencadenador de Event Grid](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

 ## <a name="endpoint-validation-with-cloudevents-v10"></a>Validación de punto de conexión con CloudEvents v1.0

Si ya está familiarizado con Event Grid, es posible que conozca el protocolo de enlace de validación de punto de conexión de Event Grid para evitar el uso inapropiado. CloudEvents v1.0 implementa su propia [semántica de protección contra abusos](security-authentication.md#webhook-event-delivery) mediante el método HTTP OPTIONS. Puede leer más sobre este tema [aquí](https://github.com/cloudevents/spec/blob/v1.0/http-webhook.md#4-abuse-protection). Al usar el esquema de CloudEvents para la salida, Event Grid lo hace con la protección contra abusos de CloudEvents v1.0 en lugar del mecanismo de eventos de validación de Event Grid.

## <a name="next-steps"></a>Pasos siguientes

* Para información sobre la supervisión de las entregas de eventos, consulte [Supervisar la entrega de mensajes de Event Grid](monitor-event-delivery.md).
* Le animamos a que pruebe CloudEvents, nos deje su opinión y [contribuya](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) a dicho esquema.
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
