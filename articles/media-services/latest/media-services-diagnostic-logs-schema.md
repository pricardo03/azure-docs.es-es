---
title: 'Registros de diagnóstico de servicios de Azure Media esquemas: Azure'
description: Este artículo muestra los esquemas de los registros de diagnóstico de Azure Media Services.
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
ms.date: 01/20/2019
ms.author: juliako
ms.openlocfilehash: 225bc4368d2a87fa5fef3f74624fd10f42dc7c97
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556367"
---
# <a name="diagnostic-logs-schemas"></a>Esquemas de registros de diagnóstico

[Azure Monitor](../../azure-monitor/overview.md) habilita la supervisión de las métricas y registros de diagnóstico que le ayudarán a comprende cómo funcionan las aplicaciones. Puede supervisar los registros de diagnóstico de Media Services y crear alertas y notificaciones de los registros y métricas recopiladas. Puede enviar registros a [Azure Storage](https://azure.microsoft.com/services/storage/), transmitirlos a [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)y exportarlos a [Log Analytics](https://azure.microsoft.com/services/log-analytics/), o usar 3 servicios de terceros.

Para obtener información detallada, consulte [métricas de Azure Monitor](../../azure-monitor/platform/data-collection.md) y [registros de Azure Monitor Diagnostics](../../azure-monitor/platform/diagnostic-logs-overview.md).

En este artículo se describe los esquemas de los registros de diagnóstico de Media Services.

## <a name="top-level-diagnostic-logs-schema"></a>Esquema de registros de diagnósticos de nivel superior

Para una descripción detallada del esquema de los registros de diagnóstico de nivel superior, consulte [compatible de servicios, esquemas y categorías para los registros de diagnóstico de Azure](../../azure-monitor/platform/tutorial-dashboards.md).

## <a name="key-delivery-log-schema"></a>Esquema de entrega de claves del registro

### <a name="properties"></a>Properties (Propiedades)

Estas propiedades son específicas para el esquema de entrega de claves del registro.

|NOMBRE|DESCRIPCIÓN|
|---|---|
|keyId|El identificador de la clave solicitada.|
|keyType|Podría ser uno de los siguientes valores: "Borrar" (sin cifrado), "FairPlay", "PlayReady" o "Widevine".|
|policyName|Nombre de la directiva de Azure Resource Manager.|
|tokenType|El tipo de token.|
|statusMessage|El mensaje de estado.|

### <a name="examples"></a>Ejemplos

Propiedades del esquema de las solicitudes de entrega de claves.

```json
{
    "time": "2019-01-11T17:59:10.4908614Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Succeeded",
    "resultSignature": "OK",
    "durationMs": 315,
    "identity": {
        "authorization": {
            "issuer": "http://testacs",
            "audience": "urn:test"
        },
        "claims": {
            "urn:microsoft:azure:mediaservices:contentkeyidentifier": "3321e646-78d0-4896-84ec-c7b98eddfca5",
            "iss": "http://testacs",
            "aud": "urn:test",
            "exp": "1547233138"
        }
    },
    "level": "Informational",
    "location": "uswestcentral",
    "properties": {
        "requestId": "b0243468-d8e5-4edf-a48b-d408e1661050",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "JWT",
        "statusMessage": "OK"
    }
} 
```

```json
 {
    "time": "2019-01-11T17:59:33.4676382Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Failed",
    "resultSignature": "Unauthorized",
    "durationMs": 2,
    "level": "Error",
    "location": "uswestcentral",
    "properties": {
        "requestId": "875af030-b77c-416b-b7e1-58f23ebec182",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "None",
        "statusMessage": "No token present in authorization header or URL."
    }
} 
```

## <a name="next-steps"></a>Pasos siguientes

[Supervisar las métricas de Media Services y los registros de diagnóstico](media-services-metrics-diagnostic-logs.md)
