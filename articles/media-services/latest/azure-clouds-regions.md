---
title: Nubes y regiones donde Azure Media Services v3 está disponible | Microsoft Docs
description: En este artículo se trata sobre las nubes y regiones de Azure donde Azure Media Services v3 está disponible.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/11/2019
ms.author: juliako
ms.openlocfilehash: 8eb49010d89c3039f46e5c84cd305b7d0b5ca025
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54306989"
---
# <a name="clouds-and-regions-in-which-azure-media-services-v3-exists"></a>Nubes y regiones donde existe Azure Media Services v3

Azure Media Services v3 está disponible a través del manifiesto de Azure Resource Manager en Azure global, Azure Government, Azure Alemania, Azure China 21Vianet. Sin embargo, no todas las características de Media Services están disponibles en todas las nubes de Azure. En este documento se describen las disponibilidades de los componentes principales de Media Services v3.

## <a name="feature-availability-in-azure-clouds"></a>Disponibilidad de características en las nubes de Azure

| Característica|Regiones de Azure global | Azure Government|Azure Alemania|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| [Azure EventGrid](reacting-to-media-services-events.md) | Disponible | No disponible | No disponible | No disponible |
| [VideoAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Disponible | No disponible | No disponible | No disponible |
| [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md) |  Disponible | No disponible | No disponible | No disponible |
| [StandardEncoderPreset](encoding-concept.md) | Disponible | Disponible | Disponible | Disponible |
| [LiveEvents](live-streaming-overview.md) | Disponible | Disponible | Disponible | Disponible |
| [StreamingEndpoints](streaming-endpoint-concept.md) | Disponible | Disponible | Disponible | Disponible |

## <a name="regions"></a>Regiones 

Cuando tenga que proporcionar el parámetro **location**, deberá proporcionar el nombre de código de región como el valor **location**. Para obtener el nombre en código de la región en la que está su cuenta y a la que se debe enrutar la llamada, puede ejecutar la siguiente línea en la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest):

```bash
az account list-locations
```

Una vez que ejecuta la línea mostrada anteriormente, obtiene una lista de todas las regiones de Azure. Vaya a la región de Azure que tenga el elemento *displayName* que busca y use su valor *name* para el parámetro **location**.

Por ejemplo, para la región Oeste de EE. UU. 2 de Azure (se muestra a continuación), usará "westus2" para el parámetro **location**.

```json
   {
      "displayName": "West US 2",
      "id": "/subscriptions/00000000-23da-4fce-b59c-f6fb9513eeeb/locations/westus2",
      "latitude": "47.233",
      "longitude": "-119.852",
      "name": "westus2",
      "subscriptionId": null
    }
```

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Media Services v3](media-services-overview.md)
