---
title: Supervisión de los registros de diagnóstico y las métricas de Media Services mediante Azure Monitor | Microsoft Docs
description: En este artículo se proporciona información general sobre cómo supervisar los registro de diagnóstico y las métricas de Media Services mediante Azure Monitor.
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65964773"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs"></a>Supervisión de los registros de diagnóstico y las métricas de Media Services

[Azure Monitor](../../azure-monitor/overview.md) le permite supervisar métricas y registros de diagnóstico que le ayudan a conocer el rendimiento de sus aplicaciones. Todos los datos recopilados por Azure Monitor pueden clasificarse como uno de los dos tipos fundamentales: métricas y registros. Puede supervisar registros de diagnóstico de Media Services y crear alertas y notificaciones para las métricas y los registros recopilados. Puede usar el [Explorador de métricas](../../azure-monitor/platform/metrics-getting-started.md) para visualizar y analizar los datos de métrica. Puede enviar registros a [Azure Storage](https://azure.microsoft.com/services/storage/), difundirlos a [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) y exportarlos a [Log Analytics](https://azure.microsoft.com/services/log-analytics/), o bien usar servicios de terceros.

Para información general detallada, consulte [Métricas de Azure Monitor](../../azure-monitor/platform/data-platform.md) y [Registros de diagnóstico de Azure Monitor](../../azure-monitor/platform/diagnostic-logs-overview.md).

En este tema se analizan las [métricas de Media Services](#media-services-metrics) y los [registro de diagnóstico de Media Services](#media-services-diagnostic-logs) actualmente disponibles.

## <a name="media-services-metrics"></a>Métricas de Media Services

Las métricas se recopilan a intervalos regulares independientemente de si cambia el valor o no. Son útiles para las alertas, dado que se pueden muestrear con frecuencia y se puede activar una alerta con rapidez con una lógica relativamente sencilla.

Actualmente, Azure emite estas métricas de [Puntos de conexión de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints) de Media Services:

|Métrica|Nombre para mostrar|DESCRIPCIÓN|
|---|---|---|
|Requests|Requests|Brinda detalles con respecto al número total de solicitudes atendidas por el Punto de conexión de streaming.|
|Salida|Salida|Número total de bytes de salida. Por ejemplo, los bytes que el Punto de conexión de streaming transmite en secuencias.|
|SuccessE2ELatency|Latencia correcta de extremo a extremo| Brinda información sobre la latencia de extremo a extremo de solicitudes que se realizaron correctamente.|

Por ejemplo, para obtener las métricas de "Egress" con la CLI, ejecute el comando de la CLI `az monitor metrics`:

```cli
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Para más información sobre cómo crear alertas de métricas, consulte [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

## <a name="media-services-diagnostic-logs"></a>Registros de diagnóstico de Media Services

Actualmente puede obtener estos registros de diagnóstico:

|NOMBRE|DESCRIPCIÓN|
|---|---|
|Solicitud de servicio de entrega de claves|Registros que muestran la información de la solicitud de servicio de entrega de claves. Para más detalles, consulte los [esquemas](media-services-diagnostic-logs-schema.md).|

Para habilitar el almacenamiento de registros de diagnóstico en una cuenta de almacenamiento, debe usar el comando de la CLI `az monitor diagnostic-settings`: 

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

[Recopilación y consumo de datos de registro provenientes de los recursos de Azure](../../azure-monitor/platform/diagnostic-logs-overview.md).
