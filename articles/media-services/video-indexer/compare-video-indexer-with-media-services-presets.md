---
title: Comparación de valores predeterminados de Video Indexer y Azure Media Services v3 | Microsoft Docs
description: En este tema se comparan los valores predeterminados de Video Indexer y Azure Media Services v3.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 041e76ccecb4dd0fe9c060681609dfb92c03ec5a
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893153"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Comparación de los valores predeterminados de Video Indexer y Azure Media Services v3 

En este artículo se comparan las funcionalidades de las **API de Video Indexer** y las **API de Media Services v3**. 

Actualmente, hay una superposición entre las características que ofrece el [Video Indexer API](https://api-portal.videoindexer.ai/) y [las API de Media Services v3](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). En la tabla siguiente se ofrecen las instrucciones actuales para entender las similitudes y diferencias. 

## <a name="compare"></a>Comparación

|Característica|API de Video Indexer |Valores predeterminados del analizador de vídeo y el analizador de audio<br/>en Azure Media Services v3|
|---|---|---|
|Información de medios sociales|[Mejorada](video-indexer-output-json-v2.md) |[Aspectos básicos](../latest/intelligence-concept.md)|
|Experiencias|Consulte la lista completa de características admitidas: <br/> [Información general](video-indexer-overview.md)|Devuelve solo información de vídeo.|
|Facturación|[Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Cumplimiento normativo|[Cumplimiento de Azure](https://aka.ms/AzureCompliance)|Media Services es compatible con muchas certificaciones. Consulte el [documento PDF de ofertas de cumplimiento de Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) y busque "Media Services" para ver si cumple con un certificado de interés.|
|Versión de prueba gratuita|Este de EE. UU|No disponible|
|Disponibilidad |Oeste de Estados Unidos, Asia Oriental, Europa del Norte|Consulte el [estado de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Pasos siguientes

[Vídeo de introducción de indizador](video-indexer-overview.md)

[Información general de Media Services v3](../latest/media-services-overview.md)
