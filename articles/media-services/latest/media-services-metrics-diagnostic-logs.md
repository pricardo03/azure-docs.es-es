---
title: Supervisar las métricas de Media Services y los registros de diagnóstico a través de Azure Monitor | Microsoft Docs
description: En este artículo se proporciona información general de cómo supervisar las métricas de Media Services y los registros de diagnóstico a través de Azure Monitor.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2019
ms.author: juliako
ms.openlocfilehash: bbf43ecb07947fad8cc1ee064d2038e4a21d4444
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65964773"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs"></a>Supervisar las métricas de Media Services y los registros de diagnóstico

[Azure Monitor](../../azure-monitor/overview.md) habilita la supervisión de las métricas y registros de diagnóstico que le ayudarán a comprende cómo funcionan las aplicaciones. Todos los datos recopilados por Azure Monitor se adapta a uno de dos tipos fundamentales, métricas y registros. Puede supervisar los registros de diagnóstico de Media Services y crear alertas y notificaciones de los registros y métricas recopiladas. Puede visualizar y analizar los datos de métricas mediante [Explorador de métricas](../../azure-monitor/platform/metrics-getting-started.md). Puede enviar registros a [Azure Storage](https://azure.microsoft.com/services/storage/), transmitirlos a [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)y exportarlos a [Log Analytics](https://azure.microsoft.com/services/log-analytics/), o usar 3 servicios de terceros.

Para información más detallada, consulte [métricas de Azure Monitor](../../azure-monitor/platform/data-platform.md) y [registros de Azure Monitor Diagnostics](../../azure-monitor/platform/diagnostic-logs-overview.md).

Este tema se tratan actualmente disponibles [las métricas de Media Services](#media-services-metrics) y [registros de diagnóstico de Media Services](#media-services-diagnostic-logs).

## <a name="media-services-metrics"></a>Métricas de Media Services

Las métricas se recopilan a intervalos regulares independientemente de si cambia el valor o no. Son útiles para las alertas, dado que se pueden muestrear con frecuencia y se puede activar una alerta con rapidez con una lógica relativamente sencilla.

Actualmente, los siguientes servicios de medios [extremos de Streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints) métricas se emiten por Azure:

|Métrica|`Display name`|DESCRIPCIÓN|
|---|---|---|
|Requests|Solicitudes|Proporciona información sobre el número total de solicitudes tramitadas por el punto de conexión de Streaming.|
|Salida|Salida|Número total de bytes de salida. Por ejemplo, los bytes transmitidos por secuencias el punto de conexión de Streaming.|
|SuccessE2ELatency|Latencia de extremo a extremo correcta| Proporciona información acerca de la latencia de extremo a extremo de solicitudes correctas.|

Por ejemplo, para obtener las métricas de "Salida" con la CLI, ejecutaría lo siguiente `az monitor metrics` comando de CLI:

```cli
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Para obtener información sobre cómo crear alertas de métricas, consulte [crear, ver y administrar alertas de métricas con Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

## <a name="media-services-diagnostic-logs"></a>Registros de diagnóstico de Media Services

Actualmente, puede obtener los registros de diagnóstico siguientes:

|NOMBRE|DESCRIPCIÓN|
|---|---|
|Solicitud de servicio de entrega de claves|Registros que muestran la información de solicitud de servicio de entrega de claves. Para obtener más información, consulte [esquemas](media-services-diagnostic-logs-schema.md).|

Para habilitar el almacenamiento de registros de diagnóstico en una cuenta de almacenamiento, ejecutaría lo siguiente `az monitor diagnostic-settings` comando de CLI: 

```cli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Por ejemplo:

```cli
az monitor diagnostic-settings create --name amsv3diagnostic \
    --storage-account storageaccountforamsv3  \
    --resource "/subscriptions/00000000-0000-0000-0000-0000000000/resourceGroups/amsv3ResourceGroup/providers/Microsoft.Media/mediaservices/amsv3account" \
    --resource-group "amsv3ResourceGroup" \
    --logs '[{"category": "KeyDeliveryRequests",  "enabled": true, "retentionPolicy": {"days": 3, "enabled": true }}]'
```

## <a name="next-steps"></a>Pasos siguientes 

[Cómo recopilar y usar los datos de registro de los recursos de Azure](../../azure-monitor/platform/diagnostic-logs-overview.md).
