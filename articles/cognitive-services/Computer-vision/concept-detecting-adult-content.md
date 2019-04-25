---
title: 'Descripción de contenido para adultos y subido de tono: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la detección de contenido para adultos y subido de tono en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 69a4c136e9c210dd40e004b8d5e1c1a2a8fceaa7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60368360"
---
# <a name="detect-adult-and-racy-content"></a>Detección de contenido para adultos y subido de tono

Computer Vision puede detectar material para adultos en imágenes para que los desarrolladores puedan restringir la presentación de dichas imágenes en su software. Las marcas de contenido se aplican con una puntuación entre cero y uno para que los desarrolladores puedan interpretar los resultados según sus preferencias. 

> [!NOTE]
> Esta característica también está disponible en el servicio [Azure Content Moderator](https://docs.microsoft.com/azure/cognitive-services/content-moderator/overview). Consulte esta alternativa para las soluciones en escenarios de moderación de contenido más rigurosas, como la moderación de texto y los flujos de trabajo de revisión humana.

## <a name="content-flag-definitions"></a>Definiciones de las marcas de contenido

Las imágenes **para adultos** se definen como aquellas de naturaleza pornográfica que suelen representar desnudos y actos sexuales. 

Las imágenes **subidas de tono** se definen como las imágenes de naturaleza sexualmente sugerente y que a menudo contienen material sexual menos explícito que las imágenes etiquetadas como **para adultos**. 

## <a name="identify-adult-and-racy-content"></a>Identificación de contenido para adultos y subido de tono

La API [de análisis](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa).

El método Analyze Image devuelve dos propiedades booleanas, `isAdultContent` y `isRacyContent`, en la respuesta JSON del método para indicar, respectivamente, que se trata de contenido para adultos o subido de tono. El método también devuelve dos propiedades `adultScore` y `racyScore`, que representan, respectivamente, las puntuaciones de confianza para la identificación de contenido para adultos o subido de tono.

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos relacionados con la [detección de contenido específico del dominio](concept-detecting-domain-content.md) y la [detección de caras](concept-detecting-faces.md).
