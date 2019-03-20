---
title: Uso de Azure Event Grid con eventos del esquema CloudEvents
description: Se describe cómo establecer el esquema CloudEvents para eventos de Azure Event Grid.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: babanisa
ms.openlocfilehash: 1eab0be59de90ad68cbf9601b7e2200e640875f4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58178074"
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Uso del esquema CloudEvents con Event Grid

Además de su [esquema de eventos predeterminado](event-schema.md), Azure Event Grid admite de forma nativa eventos del [esquema JSON CloudEvents](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](https://cloudevents.io/) es una [especificación abierta](https://github.com/cloudevents/spec/blob/master/spec.md) para la descripción de datos de eventos.

CloudEvents simplifica la interoperabilidad al proporcionar un esquema de eventos común para la publicación y el consumo de eventos basados en la nube. Este esquema permite herramientas uniformes, formas estándar de enrutar y administrar eventos y formas universales de deserializar el esquema de eventos externo. Con un esquema común, puede integrar más fácilmente el trabajo entre plataformas.

En la compilación de CloudEvents participan varios [colaboradores](https://github.com/cloudevents/spec/blob/master/community/contributors.md), entre ellos Microsoft, mediante [Cloud Native Compute Foundation](https://www.cncf.io/). Actualmente está disponible en versión 0.1.

En este artículo se describe cómo usar el esquema CloudEvents con Event Grid.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Instalación de la característica en vista previa

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>Esquema CloudEvent

Este es un ejemplo de un evento de Azure Blob Storage en formato de CloudEvents:

``` JSON
{
    "cloudEventsVersion" : "0.1",
    "eventType" : "Microsoft.Storage.BlobCreated",
    "eventTypeVersion" : "",
    "source" : "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}#blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "eventID" : "173d9985-401e-0075-2497-de268c06ff25",
    "eventTime" : "2018-04-28T02:18:47.1281675Z",
    "data" : {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
}
```

CloudEvents v0.1 tiene disponibles las siguientes propiedades:

| CloudEvents        | Type     | Valor JSON de ejemplo             | DESCRIPCIÓN                                                        | Asignación de Event Grid
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| eventType          | string   | "com.example.someevent"          | Tipo de repetición que sucedió                                   | eventType
| eventTypeVersion   | string   | "1.0"                            | La versión de eventType (opcional)                            | dataVersion
| cloudEventsVersion | string   | "0.1"                            | La versión de la especificación de CloudEvents que usa el evento        | *se pasa*
| de origen             | URI      | "/mycontext"                     | Describe el productor del evento                                       | topic#subject
| eventID            | string   | "1234-1234-1234"                 | Identificador del evento                                                    | id
| eventTime          | Timestamp| "2018-04-05T17:31:00Z"           | Marca de tiempo de cuando produjo el evento (opcional)                    | eventTime
| schemaURL          | URI      | "https://myschema.com"           | Un vínculo al esquema al que se adhiere el atributo de datos (opcional) | *no se usa*
| contentType        | string   | "application/json"               | Describe el formato de codificación de datos (opcional)                       | *no se usa*
| extensions         | Map      | { "extA": "vA", "extB", "vB" }  | Los metadatos adicionales (opcional)                                 | *no se usa*
| data               | Objeto   | { "objA": "vA", "objB", "vB" }  | La carga del evento (opcional)                                       | data

Para más información, consulte la [especificación de CloudEvents](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes).

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
  --input-schema cloudeventv01schema
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
  -InputSchema CloudEventV01Schema
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
  --event-delivery-schema cloudeventv01schema
```

Para PowerShell, use:
```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeliverySchema CloudEventV01Schema
```

La versión actual de CloudEvents no es compatible con el procesamiento por lote de eventos. Una suscripción de eventos que se configura para el esquema de CloudEvent recibe cada evento de forma individual. Actualmente, no puede usar un desencadenador de Event Grid para una aplicación de Azure Functions cuando el evento se entrega en el esquema de CloudEvents. Use un desencadenador HTTP. Para obtener ejemplos de la implementación de un desencadenador HTTP que recibe eventos en el esquema de CloudEvents, consulte [Uso de un desencadenador HTTP como un desencadenador de Event Grid](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

## <a name="next-steps"></a>Pasos siguientes

* Para información sobre la supervisión de las entregas de eventos, consulte [Supervisar la entrega de mensajes de Event Grid](monitor-event-delivery.md).
* Le animamos a que pruebe CloudEvents, nos deje su opinión y [contribuya](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) a dicho esquema.
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
