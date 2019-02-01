---
title: 'Descripción de contenido para adultos y subido de tono: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la detección de contenido para adultos y subido de tono en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7f343f22eb881cf58a024f8464ffd0d615fe06e8
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158900"
---
# <a name="detecting-adult-and-racy-content"></a>Detección de contenido para adultos y subido de tono

Entre las diferentes categorías visuales, se encuentra el grupo de contenido para adultos y subido de tono, que permite detectar el material destinado a adultos y limita la presentación de imágenes de contenido sexual. El filtro para detección de contenido para adultos y subido de tono se puede establecer en una escala gradual, para ajustarlo a la preferencia del usuario.

## <a name="defining-adult-and-racy-content"></a>Detección de contenido para adultos y subido de tono

Entre las distintas características visuales cubiertas por el [método de análisis de imagen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), la característica visual para adultos habilita la detección de imágenes para adultos y subidas de tono. Las imágenes "para adultos" se definen como aquellas de naturaleza pornográfica que suelen representar desnudos y actos sexuales. Las imágenes "subidas de tono" se definen como las imágenes de naturaleza sexualmente sugerente y que a menudo contienen material sexual menos explícito que las imágenes etiquetadas como "para adultos" El tipo de característica visual para adultos suele usarse para restringir la visualización de imágenes con contenido sexualmente sugerente o explícito.

## <a name="identifying-adult-and-racy-content"></a>Identificación de contenido para adultos y subido de tono

El método de análisis de imagen devuelve dos propiedades, `isAdultContent` y `isRacyContent`, en la respuesta JSON del método para indicar, respectivamente, que se trata de contenido para adultos o subido de tono. Ambas propiedades devuelven un valor booleano true o false. El método también devuelve dos propiedades `adultScore` y `racyScore`, que representan, respectivamente, las puntuaciones de confianza para la identificación de contenido para adultos o subido de tono. El filtro para la detección de contenido para adultos y subido de tono se puede establecer en una escala gradual, con el fin de que pueda ajustarlo a sus preferencias según el escenario concreto.

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos relacionados con la [detección de contenido específico del dominio](concept-detecting-domain-content.md) y la [detección de caras](concept-detecting-faces.md).
