---
title: Conceptos de Azure Video Indexer | Microsoft Docs
description: En este tema se describen algunos conceptos del servicio Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 224c8b05027f51fb99c8d58be34c3604032c0f77
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399502"
---
# <a name="video-indexer-concepts"></a>Conceptos de Video Indexer
 
En este tema se describen algunos conceptos del servicio Video Indexer.
    
## <a name="summarized-insights"></a>Información resumida

La información resumida contiene una vista agregada de los datos: caras, palabras clave, opiniones. Por ejemplo, en lugar de pasar por cada uno de los miles de intervalos de tiempo y comprobar qué caras hay en ellos, la información resumida contiene todas las caras y, para cada una de ellas, los intervalos de tiempo en los que aparece y el porcentaje del tiempo en el que se muestra.

## <a name="topicskeywords"></a>Temas o palabras clave

Los temas o palabras clave se encuentran en la lista de frases clave que extrae Video Indexer del texto. Por ejemplo, un vídeo de Scott Guthrie puede contener los siguientes temas o palabras clave: seguridad, Azure, Microsoft Cloud, ingresos.

## <a name="sentiments"></a>Opiniones

Cuando Video Indexer analiza las transcripciones, detecta también las opiniones. Por ejemplo, "esto es un evento muy emocionante" es una opinión positiva.

## <a name="time-range-vs-adjusted-time-range"></a>intervalo de tiempo frente a intervalo de tiempo ajustado

TimeRange es el intervalo de tiempo del vídeo original. AdjustedTimeRange es el intervalo de tiempo respecto a la lista de reproducción actual. Como puede crear una lista de reproducción a partir de diferentes líneas de vídeos distintas, puede grabar un vídeo de una hora y utilizar solo una línea de él, por ejemplo, de 10:00 a 10:15. En ese caso, tendrá una lista de reproducción con 1 línea, donde el intervalo de tiempo es 10:00-10:15, pero el intervalo de tiempo ajustado es 00:00-00:15.
 
## <a name="blocks"></a>Blocks

Los bloques están pensados para que resulte más fácil el recorrido por los datos. Por ejemplo, el bloque puede dividirse en función de cuándo cambian los oradores o de si hay una pausa larga.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo empezar, consulte [Suscripción y carga del primer vídeo](video-indexer-get-started.md).

## <a name="see-also"></a>Otras referencias

[Introducción a Video Indexer](video-indexer-overview.md)
