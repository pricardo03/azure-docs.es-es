---
title: Conceptos de Video Indexer
titlesuffix: Azure Cognitive Services
description: En este tema se describen algunos conceptos del servicio Video Indexer.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: juliako
ms.openlocfilehash: 740f13e90397650ed9274937b16254e46c6deced
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984128"
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

## <a name="see-also"></a>Otras referencias

[Introducción a Video Indexer](video-indexer-overview.md)
