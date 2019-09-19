---
title: Escenas, tomas y fotogramas clave de Video Indexer
titleSuffix: Azure Media Services
description: En este tema se proporciona información general sobre las escenas, los cortes y los fotogramas clave de Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 07/05/2019
ms.author: juliako
ms.openlocfilehash: b24778434596f583be44572612c856fa4e0cecde
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860234"
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

## <a name="editorial-shot-type-detection"></a>Detección del tipo de toma editorial

El tipo de toma asociado a una toma individual en el JSON de las conclusiones representa su tipo editorial. Las características de estos tipos de toma son útiles al editar vídeos en clips, avances o al buscar un estilo concreto de fotograma clave para fines artísticos. Los diferentes tipos se determinan en función del análisis del primer fotograma clave de cada toma. Las capturas se identifican por la escala, el tamaño y la ubicación de las caras que aparecen en su primer fotograma clave. 

El tamaño y la escala de la toma se determinan en función de la distancia entre la cámara y las caras que aparecen en el fotograma. Con estas propiedades, Video Indexer detecta los siguientes tipos de toma:

* Plano general: muestra todo el cuerpo de una persona.
* Plano medio: muestra la cara y la parte superior del cuerpo de una persona.
* Primer plano: muestra principalmente la cara de una persona.
* Primerísimo primer plano: muestra la cara de una persona que rellena la pantalla. 

Los tipos de capturas también pueden determinarse por la ubicación de los personajes del sujeto con respecto al centro del fotograma. Esta propiedad define los siguientes tipos de toma de Video Indexer:

* Cara en la izquierda: una persona aparece en el lado izquierdo del fotograma.
* Cara en el centro: una persona aparece en el área central del fotograma.
* Cara en la derecha: una persona aparece en el lado derecho del fotograma.
* Exterior: una persona aparece en un entorno exterior.
* Interior: una persona aparece en un entorno interior.

Características adicionales:

* Dos tomas: muestra las caras de dos personas en tamaño medio.
* Varias caras: más de dos personas.

## <a name="next-steps"></a>Pasos siguientes

[Examen de la salida de Video Indexer producida por la API](video-indexer-output-json-v2.md#scenes)
