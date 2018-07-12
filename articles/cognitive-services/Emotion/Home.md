---
title: Información general sobre Emotion API | Microsoft Docs
description: Use el algoritmo de Microsoft de vanguardia, basado en la nube para el reconocimiento de emociones con el fin de crear aplicaciones más personalizadas, con Emotion API en Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 8383370cba3f78060e809f444f4ad3dab7380f4e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380799"
---
# <a name="what-is-emotion-api"></a>¿Qué es Emotion API?

> [!IMPORTANT]
> Emotion API ha quedado obsoleta el 30 de octubre de 2017. La funcionalidad ahora forma parte de [Face API](https://docs.microsoft.com/en-us/azure/cognitive-services/face/).

Le presentamos Microsoft Emotion API, que le permite crear aplicaciones más personalizadas con el algoritmo basado en la nube para el reconocimiento de emociones.

### <a name="emotion-recognition"></a>Reconocimiento de emociones

La versión beta de Emotion API toma una imagen como entrada y devuelve la confianza en una serie de emociones para cada cara de la imagen, además de disponer de un rectángulo de selección con Face API. Las emociones detectadas son felicidad, tristeza, sorpresa, ira, temor, desprecio, asco, neutralidad. Estas expresiones, que se comunican transcultural y universalmente a través de las mismas expresiones faciales básicas, se identifican mediante Emotion API. 

**Interpretación de los resultados:** 

En la interpretación de los resultados de Emotion API, la emoción detectada debe interpretarse como la emoción con la puntuación más alta, dado que las puntuaciones se normalizan para que sumen uno. Los usuarios pueden elegir establecer un umbral de confianza mayor dentro de su aplicación, dependiendo de sus necesidades. 

Para obtener más información sobre detección de emociones, consulte Referencia de API: 
  * Básico: Si un usuario ha llamado ya a Face API, puede enviar el rectángulo facial como entrada opcional y usar el nivel básico. [Referencia de API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Estándar: Si un usuario no envía un rectángulo facial, debe usar el modo estándar.  [Referencia de API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Para obtener un ejemplo sobre cómo interpretar secuencias de vídeo con Emotion API, consulte [Análisis de vídeos en tiempo real](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
