---
title: 'Recursos de documentación de búsqueda cognitiva: Azure Search'
description: Una lista anotada de artículos, tutoriales, ejemplos y entradas de blog relacionados con cargas de trabajo de búsqueda cognitiva en Azure Search.
services: search
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 225ac86efba8a32b085a5fb7fc41bed48c499eab
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65022177"
---
# <a name="documentation-resources-for-cognitive-search-workloads"></a>Recursos de documentación para las cargas de trabajo de búsqueda cognitiva

Búsqueda cognitiva, ahora disponible con carácter general, es una nueva capa de enriquecimiento en la indización de búsqueda de Azure que busca información latente de orígenes que no sean de texto y texto indiscriminada, transformarlos en contenido utilizable en búsquedas de texto completo en Azure Search.

Los artículos siguientes son la documentación completa para la búsqueda cognitiva.

## <a name="getting-started"></a>Introducción
+ [What is cognitive search?](cognitive-search-concept-intro.md) (¿Qué es la búsqueda cognitiva?)
+ [Inicio rápido: Probar Cognitive Search en el portal](cognitive-search-quickstart-blob.md)
+ [Tutorial: Aprendizaje de Cognitive Search API](cognitive-search-tutorial-blob.md)
+ [Ejemplo: creación de una aptitud personalizada](cognitive-search-create-custom-skill-example.md)

## <a name="how-to-guidance"></a>Guía de procedimientos
+ [Definición de un conjunto de habilidades](cognitive-search-defining-skillset.md)
+ [How to reference annotations in a skillset](cognitive-search-concept-annotations-syntax.md) (Referencia a las anotaciones de un conjunto de aptitudes)
+ [How to map fields to an index](cognitive-search-output-field-mapping.md) (Asignación de campos enriquecidos a un índice)
+ [How to process and extract information from images](cognitive-search-concept-image-scenarios.md) (Procesamiento y extracción de información de imágenes)
+ [How to rebuild an Azure Search index](search-howto-reindex.md) (Reconstrucción de un índice de Azure Search)
+ [How to define a custom skills interface](cognitive-search-custom-skill-interface.md) (Definición de una interfaz de aptitudes personalizada)
+ [Sugerencias para la solución de problemas](cognitive-search-concept-troubleshooting.md)

## <a name="reference"></a>Referencia

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
  + [Microsoft.Skills.Text.KeyPhraseSkill](cognitive-search-skill-keyphrases.md)
  + [Microsoft.Skills.Text.LanguageDetectionSkill](cognitive-search-skill-language-detection.md)
  + [Microsoft.Skills.Text.NamedEntityRecognitionSkill](cognitive-search-skill-named-entity-recognition.md)
  + [Microsoft.Skills.Text.MergeSkill](cognitive-search-skill-textmerger.md)
  + [Microsoft.Skills.Text.SplitSkill](cognitive-search-skill-textsplit.md)
  + [Microsoft.Skills.Text.SentimentSkill](cognitive-search-skill-sentiment.md)
  + [Microsoft.Skills.Vision.ImageAnalysisSkill](cognitive-search-skill-image-analysis.md)
  + [Microsoft.Skills.Vision.OcrSkill](cognitive-search-skill-ocr.md)
  + [Microsoft.Skills.Util.ShaperSkill](cognitive-search-skill-shaper.md)

+ [API DE REST](https://docs.microsoft.com/rest/api/searchservice/)
  + [Crear conjunto de habilidades (api-version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
  + [Creación de indizador (api-version = 2019-05-06)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)

## <a name="see-also"></a>Vea también

+ [API REST de Azure Search](https://docs.microsoft.com/rest/api/searchservice/)
+ [Indexadores de Azure Search](search-indexer-overview.md)
+ [¿Qué es Azure Search?](search-what-is-azure-search.md)
