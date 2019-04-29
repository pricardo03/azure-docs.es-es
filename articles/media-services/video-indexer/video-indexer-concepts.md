---
title: Conceptos de Video Indexer
titlesuffix: Azure Media Services
description: En este tema se describen algunos conceptos del servicio Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: c7bbe8c6b2ad51ed7272cd215552807c7cea3aee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60559831"
---
# <a name="video-indexer-concepts"></a>Conceptos de Video Indexer
 
En este artículo se describen algunos conceptos del servicio de Video Indexer.
    
## <a name="summarized-insights"></a>Información resumida

La información resumida contiene una vista agregada de los datos: caras, temas y emociones. Por ejemplo, en lugar de pasar por cada uno de los miles de intervalos de tiempo y comprobar qué caras hay en ellos, la información resumida contiene todas las caras y, para cada una de ellas, los intervalos de tiempo en los que aparece y el porcentaje del tiempo en el que se muestra.

## <a name="time-range-vs-adjusted-time-range"></a>intervalo de tiempo frente a intervalo de tiempo ajustado

TimeRange es el intervalo de tiempo del vídeo original. AdjustedTimeRange es el intervalo de tiempo respecto a la lista de reproducción actual. Como puede crear una lista de reproducción a partir de diferentes líneas de vídeos distintas, puede grabar un vídeo de una hora y utilizar solo una línea de él, por ejemplo, de 10:00 a 10:15. En ese caso, tendrá una lista de reproducción con 1 línea, donde el intervalo de tiempo es 10:00-10:15, pero adjustedTimeRange es 00:00-00:15.
 
## <a name="blocks"></a>Blocks

Los bloques están pensados para que resulte más fácil el recorrido por los datos. Por ejemplo, el bloque puede dividirse en función de cuándo cambian los oradores o de si hay una pausa larga.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo empezar, consulte [Suscripción y carga del primer vídeo](video-indexer-get-started.md).

## <a name="see-also"></a>Vea también

[Introducción a Video Indexer](video-indexer-overview.md)
