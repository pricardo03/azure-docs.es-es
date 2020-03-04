---
title: Códigos de error de eventos en directo de Azure Media Services | Microsoft Docs
description: En este artículo se enumeran los códigos de error de eventos en directo.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2020
ms.author: juliako
ms.openlocfilehash: bfbef771d33ad4d63ec8eaef83331e497d476071
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599689"
---
# <a name="media-services-live-event-error-codes"></a>Códigos de error de eventos en directo de Media Services

En la tabla siguiente se enumeran los códigos de error de [Eventos en directo](live-events-outputs-concept.md):

|Error|Descripción|
|---|---| 
|MPE_INGEST_FRAMERATE_EXCEEDED|Este error se produce cuando el codificador de entrada envía secuencias que superan los 30 fps para codificar canales o eventos en directo.|
|MPE_INGEST_VIDEO_RESOLUTION_NOT_SUPPORTED|Este error se produce cuando el codificador de entrada envía secuencias que superan las resoluciones siguientes: 1920 x 1088 para la codificación de canales o eventos en directo y 4096 x 2160 para canales o eventos en directo de paso a través.|

## <a name="see-also"></a>Consulte también

[Códigos de error de puntos de conexión de streaming (origen)](streaming-endpoint-error-codes.md)

## <a name="next-steps"></a>Pasos siguientes

[Tutorial: Streaming en vivo con Media Services](stream-live-tutorial-with-api.md)
