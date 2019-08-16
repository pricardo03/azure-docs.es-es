---
title: 'Escenas, cortes y fotogramas clave de Video Indexer: Azure'
titlesuffix: Azure Media Services
description: En este tema se proporciona información general sobre las escenas, los cortes y los fotogramas clave de Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 07/05/2019
ms.author: juliako
ms.openlocfilehash: cdabc1b6bfed519098f656710ef49a946e676cf2
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815654"
---
# <a name="scenes-shots-and-keyframes"></a>Escenas, capturas y fotogramas clave

Video Indexer admite la segmentación de vídeos en unidades temporales basadas en propiedades estructurales y semánticas. Esta funcionalidad permite a los clientes examinar, administrar y editar fácilmente su contenido de vídeo en función de distintas granularidades. Por ejemplo, según las escenas, los cortes y los fotogramas clave, como se describe en este tema.   

![Escenas, capturas y fotogramas clave](./media/scenes-shots-keyframes/scenes-shots-keyframes.png)
 
## <a name="scene-detection"></a>Detección de escenas  
 
Video Indexer determina cuándo cambia una escena en el vídeo según indicaciones visuales. Una escena representa un evento único y se compone de una serie de cortes consecutivos, que están semánticamente relacionadas. Una vista en miniatura de la escena es el primer fotograma clave de su captura subyacente. Video Indexer segmenta un vídeo en escenas según la coherencia de color entre cortes consecutivos y recupera la hora de comienzo y final de cada escena. La detección de escenas se considera una tarea complicada ya que supone cuantificar los aspectos semánticas de los vídeos.

> [!NOTE]
> Se aplica a los vídeos que contienen al menos tres escenas.

## <a name="shot-detection"></a>Detección de cortes

Video Indexer determina cuándo cambia un corte en el vídeo según indicaciones visuales, y realiza el seguimiento de las transacciones abruptas y graduales en el esquema de color de los fotogramas adyacentes. Los metadatos del corte incluyen una hora de inicio y de finalización, así como la lista de fotogramas clave incluidos en ese corte. Los cortes son fotogramas consecutivos que se toman con la misma cámara al mismo tiempo.

## <a name="keyframe-detection"></a>Detección de fotogramas clave

Selecciona los fotogramas que mejor representan el corte. Los fotogramas clave son los fotogramas representativos seleccionados de todo el vídeo según propiedades estéticas (por ejemplo, el contraste y la estabilidad). Video Indexer recupera una lista de identificadores de fotograma clave como parte de los metadatos del corte, en función de los cuales los clientes pueden extraer la miniatura del fotograma clave. 

Los fotogramas clave están asociados con cortes del archivo JSON de salida. 

## <a name="next-steps"></a>Pasos siguientes

[Examen de la salida de Video Indexer producida por la API](video-indexer-output-json-v2.md#scenes)
