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
ms.date: 09/26/2019
ms.author: juliako
ms.openlocfilehash: e4ff157f58a68c68f8610c6c473f5d69897650ad
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338411"
---
# <a name="azure-media-services-legacy-components"></a>Componentes heredados de Azure Media Services

Con el tiempo, se han realizado mejoras constantes en los componentes de Media Services. Dichas mejoras han dado lugar a la retirada de algunos componentes heredados. Puede encontrar instrucciones sobre cómo migrar la aplicación del componente heredado a un componente actual en los artículos siguientes.

## <a name="legacy-components-and-migration-guidance"></a>Guía de migración y componentes heredados

Anunciamos el desuso de los procesadores de multimedia *Windows Azure Media Encoder* (WAME) y *Azure Media Encoder* (AME). Estos procesadores se retirarán el 30 de noviembre de 2019.

* [Migración de Windows Azure Media Encoder a Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [Migración de Azure Media Encoder a Media Encoder Standard](migrate-azure-media-encoder.md)

También se anunció el desuso de *Azure Media Indexer v1* y *Azure Media Indexer v2 Preview*. El procesador multimedia [Azure Media Indexer v1](media-services-index-content.md) se retirará el 1 de octubre de 2020. Los procesadores multimedia [[Azure Media Indexer v2 Preview](media-services-process-content-with-indexer2.md) se retirarán el 1 de enero de 2019.  [Video Indexer de Azure Media Services ](https://docs.microsoft.com/azure/media-services/video-indexer/) reemplaza a estos procesadores multimedia heredados.

* [Migre de Azure Media Indexer v1 y Azure Media Indexer v2 a Video Indexer de Azure Media Services](migrate-indexer-v1-v2.md).

## <a name="next-steps"></a>Pasos siguientes

[Guía de migración para mover de Media Services v2 a v3](../latest/migrate-from-v2-to-v3.md)
