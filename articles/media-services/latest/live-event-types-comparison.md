---
title: Tipos de LiveEvent en Azure Media Services | Microsoft Docs
description: En este artículo aparece una tabla detallada que compara los tipos de LiveEvent.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/27/2018
ms.author: juliako
ms.openlocfilehash: 7ff48962d01a83e8c9fce380d92fbc196ff96533
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585488"
---
# <a name="liveevent-types-comparison"></a>Comparación de tipos de objetos LiveEvent

En Azure Media Services, un objeto [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) puede ser de uno de estos dos tipos: codificación en directo y paso a través. 

## <a name="types-comparison"></a>Comparación de tipos 

En la tabla siguiente se comparan las características de dos tipos de objetos LiveEvent.

| Característica | LiveEvent con paso a través | LiveEvent estándar (básico) |
| --- | --- | --- |
| La entrada de velocidad de bits única se codifica en varias velocidades de bits en la nube |Sin  |SÍ |
| Resolución de vídeo máxima para la fuente de contribución |4 K (4096 x 2160 en 60 fotogramas por segundo) |1080 p (1920 x 1088 en 30 fotogramas por segundo)|
| Niveles máximos recomendados en la fuente de contribución|Hasta 12|Un audio|
| Número máximo de niveles en la salida| Igual que la entrada|Hasta 7|
| Ancho de banda agregado máximo de la fuente de contribución|60 Mbps|N/D|
| Velocidad de bits máxima para una sola capa de la contribución |20 Mbps|20 Mbps|
| Compatibilidad con pistas de audio de varios idiomas|SÍ|Sin |
| Códecs compatibles de vídeo de entrada |H.264/AVC y H.265/HEVC|H.264/AVC|
| Códecs compatibles de vídeo de salida|Igual que la entrada|H.264/AVC|
| Profundidad de bits, entrada y salida de vídeo compatibles|Hasta 10 bits incluido HDR 10/HLG|8 bits|
| Códecs de audio de entrada compatibles|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Códecs de audio de salida compatibles|Igual que la entrada|AAC-LC|
| Protocolos de entrada|RTMP, MP4 fragmentado (Smooth Streaming)|RTMP, MP4 fragmentado (Smooth Streaming)|
| Precio|Consulte la [página de precios](https://azure.microsoft.com/pricing/details/media-services/) y haga clic en la pestaña "Vídeo en vivo"|Consulte la [página de precios](https://azure.microsoft.com/pricing/details/media-services/) y haga clic en la pestaña "Vídeo en vivo"|
| Tiempo de ejecución máximo|24 x 365 lineales en vivo|24x7|
| Posibilidad de pasar a través de los subtítulos CEA 608/708 insertados|SÍ|SÍ|
| Compatibilidad con inserción de tabletas táctiles|Sin |Sin |
| Compatibilidad con señalización de anuncios mediante API| Sin |Sin |
| Compatibilidad con los mensajes de señalización de anuncios mediante SCTE-35 en banda|SÍ|SÍ|
| Capacidad para la recuperación de breves pausas en la fuente de contribución|SÍ|No (LiveEvent comenzará a usar la tableta táctil transcurridos más de 6 segundos sin datos de entrada)|
| Compatibilidad con GOP de entrada no uniformes|SÍ|No: la entrada debe tener una duración fija del grupo de imágenes|
| Compatibilidad con la entrada de la velocidad de fotogramas variable|SÍ|No: la entrada debe ser una velocidad de fotogramas fija. Se tolerarán pequeñas variaciones, por ejemplo, durante las escenas con grandes movimientos. Pero la fuente de contribución no puede reducir la velocidad de fotogramas (por ejemplo, a 15 fotogramas/segundo).|
| Apagado automático de LiveEvent cuando se pierde la fuente de entrada|Sin |Después de 12 horas, si no hay ningún objeto LiveEvent en ejecución|

## <a name="next-steps"></a>Pasos siguientes

[Introducción al streaming en vivo](live-streaming-overview.md)
