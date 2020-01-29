---
title: Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3
description: En este artículo se comparan las funcionalidades de Video Indexer y los valores preestablecidos de Azure Media Services v3.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 2e2abd4ffe5a6d0a6336d811599db687da146f1e
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76513191"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3 

En este artículo se comparan las funcionalidades de las **API de Video Indexer** y las **API de Media Services v3**. 

Actualmente hay una superposición entre las características que ofrecen las [API de Video Indexer](https://api-portal.videoindexer.ai/) y las [API de Media Services v3](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). En la tabla siguiente se ofrecen las instrucciones actuales para entender las similitudes y diferencias. 

## <a name="compare"></a>Comparación

|Característica|API de Video Indexer |Valores predeterminados del analizador de vídeo y el analizador de audio<br/>en Azure Media Services v3|
|---|---|---|
|Información de medios sociales|[Mejorado](video-indexer-output-json-v2.md). |[Aspectos básicos](../latest/intelligence-concept.md)|
|Experiencias|Consulte la lista completa de características admitidas: <br/> [Información general](video-indexer-overview.md)|Devuelve solo información de vídeo.|
|Facturación|[Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Cumplimiento normativo|Con certificación [ISO 27001](https://www.microsoft.com/TrustCenter/Compliance/ISO-IEC-27001), [ISO 27018](https://www.microsoft.com/trustcenter/Compliance/ISO-IEC-27018), [SOC 1,2,3](https://www.microsoft.com/TrustCenter/Compliance/SOC), [HIPAA](https://www.microsoft.com/trustcenter/compliance/hipaa), [FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/fedramp), [PCI](https://www.microsoft.com/trustcenter/compliance/pci) e [HITRUST](https://www.microsoft.com/TrustCenter/Compliance/hitrust). Para las actualizaciones más recientes, consulte el [estado actual de las certificaciones de Video Indexer](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942).|Media Services es compatible con muchas certificaciones. Consulte el [documento PDF de ofertas de cumplimiento de Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) y busque "Media Services" para ver si cumple con un certificado de interés.|
|Versión de prueba gratuita|East US|No disponible|
|Disponibilidad en regiones|Este de EE. UU. 2, Centro-sur de EE. UU., Oeste de EE. UU. 2, Norte de Europa, Oeste de Europa, Sudeste Asiático, Asia Oriental y Este de Australia.  Para las actualizaciones más recientes, visite la página de los [productos por región](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).|Consulte el [estado de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Video Indexer](video-indexer-overview.md)

[Introducción a Media Services v3](../latest/media-services-overview.md)
