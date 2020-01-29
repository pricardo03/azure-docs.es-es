---
title: Componentes heredados de Azure Media Services | Microsoft Docs
description: En este tema se describen los componentes heredados de Azure Media Services.
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
ms.date: 11/22/2019
ms.author: juliako
ms.openlocfilehash: 457f5b8eb0a740800f9cc0fa69bd554f0679ae9a
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76509179"
---
# <a name="azure-media-services-legacy-components"></a>Componentes heredados de Azure Media Services

Con el tiempo, se han realizado mejoras constantes en los componentes de Media Services. Dichas mejoras han dado lugar a la retirada de algunos componentes heredados. Puede encontrar instrucciones sobre cómo migrar la aplicación del componente heredado a un componente actual en los artículos siguientes.
 
## <a name="retirement-plans-of-legacy-components-and-migration-guidance"></a>Planes de retirada de componentes heredados y guía de migración

Anunciamos el desuso de los procesadores de multimedia *Windows Azure Media Encoder* (WAME) y *Azure Media Encoder* (AME). Estos procesadores se retirarán el 1 de marzo de 2020.

* [Migración de Windows Azure Media Encoder a Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [Migración de Azure Media Encoder a Media Encoder Standard](migrate-azure-media-encoder.md)

También anunciamos la retirada de los siguientes procesadores de multimedia de Media Analytics: 
 
|Nombre de procesador de multimedia|Fecha de retirada|Notas adicionales|
|---|---|
|[Azure Media Indexer 2](media-services-process-content-with-indexer2.md)| 1 de octubre de 2020|[Video Indexer de Azure Media Services](https://docs.microsoft.com/azure/media-services/video-indexer/) reemplazará a este procesador de multimedia. Para más información, consulte [Migración de Azure Media Indexer 2 a Video Indexer de Azure Media Services](migrate-indexer-v1-v2.md).|
|[Azure Media Indexer](media-services-index-content.md)|1 de octubre de 2020|[Video Indexer de Azure Media Services](https://docs.microsoft.com/azure/media-services/video-indexer/) reemplazará a este procesador de multimedia. Para más información, consulte [Migración de Azure Media Indexer a Video Indexer de Azure Media Services](migrate-indexer-v1-v2.md)

## <a name="next-steps"></a>Pasos siguientes

[Guía de migración para mover de Media Services v2 a v3](../latest/migrate-from-v2-to-v3.md)
