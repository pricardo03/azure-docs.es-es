---
title: ¿Qué es Emotion API?
titlesuffix: Azure Cognitive Services
description: Use el algoritmo de reconocimiento de emociones basado en la nube para crear aplicaciones más personalizadas.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: overview
ms.date: 02/06/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: ceeea8c143792e9a46b4b8a9892cad07770c5fbf
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236454"
---
# <a name="what-is-the-emotion-api"></a>¿Qué es Emotion API?

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/). 

Le presentamos Microsoft Emotion API, que le permite crear aplicaciones más personalizadas con el algoritmo basado en la nube para el reconocimiento de emociones.

### <a name="emotion-recognition"></a>Reconocimiento de emociones

La versión beta de Emotion API toma una imagen como entrada y devuelve la confianza en una serie de emociones para cada cara de la imagen, además de disponer de un rectángulo de selección con Face API. Las emociones detectadas son felicidad, tristeza, sorpresa, ira, temor, desprecio, asco, neutralidad. Estas expresiones, que se comunican transcultural y universalmente a través de las mismas expresiones faciales básicas, se identifican mediante Emotion API.

**Interpretación de los resultados:**

En la interpretación de los resultados de Emotion API, la emoción detectada debe interpretarse como la emoción con la puntuación más alta, dado que las puntuaciones se normalizan para que sumen uno. Los usuarios pueden elegir establecer un umbral de confianza mayor dentro de su aplicación, dependiendo de sus necesidades.

Para obtener más información sobre detección de emociones, consulte Referencia de API:
  * Básico: si un usuario ya ha llamado a Face API, puede enviar el rectángulo facial como entrada y usar el nivel básico. [Referencia de API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Estándar: si un usuario no envía un rectángulo facial, debe usar el modo estándar.  [Referencia de API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Para obtener un ejemplo sobre cómo interpretar secuencias de vídeo con Emotion API, consulte [Análisis de vídeos en tiempo real](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
