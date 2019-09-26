---
title: 'Idiomas admitidos: Text Analytics API'
titleSuffix: Azure Cognitive Services
description: 'Una lista de los idiomas naturales admitidos por Text Analytics API. En este artículo se explica qué idiomas se admiten para cada operación: análisis de opiniones, extracción de frases clave, detección de idioma y reconocimiento de entidad.'
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: efac8755be8435ae7929fdcb2ecedb4959c363b9
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71097140"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Compatibilidad de idiomas y regiones para Text Analytics API

En este artículo se explica qué idiomas se admiten para cada operación: análisis de opiniones, extracción de frases clave, detección de idioma y reconocimiento de entidad con nombre.

## <a name="language-detection"></a>Detección de idiomas

La API Text Analytics puede detectar una amplia variedad de idiomas, variantes, dialectos y algunos idiomas regionales o culturales.  La detección de idioma devuelve el "script" de un idioma. Por ejemplo, en el caso de la frase "Tengo un perro", se devolverá `en` en lugar de `en-US`. El único caso especial es el chino, donde la funcionalidad de detección de idioma devolverá `zh_CHS` o `zh_CHT` si puede determinar el script dado el texto proporcionado. En situaciones donde no se puede identificar un script específico para un documento chino, devolverá simplemente `zh`.

No se ha publicado la lista exacta de idiomas de esta característica, pero puede detectar una amplia gama de idiomas, variantes, dialectos y algunos lenguajes cultural o configuración regional. 

Si tiene contenido que se expresa en un idioma que se usa con menos frecuencia, puede probar Detección de idioma para ver si devuelve un código. La respuesta para los idiomas que no se pueden detectar es `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Análisis de sentimiento, Extracción de frases clave y Reconocimiento entidad con nombre

Para el Análisis de sentimiento, la Extracción de frases clave y el Reconocimiento entidad, la lista de idiomas admitidos es más selectiva, ya que los analizadores se perfeccionan para dar cabida a las reglas lingüísticas de idiomas adicionales. La compatibilidad con el conjunto completo de [tipos de entidad](how-tos/text-analytics-how-to-entity-linking.md#supported-types-for-named-entity-recognition) actualmente está limitada a los idiomas siguientes: 
* English
* Chino simplificado
* Francés
* Alemán
* Español

Para los otros idiomas, solo se devuelven las entidades con nombre `Person`, `Location` y `Organization`.

## <a name="language-list-and-status"></a>Estado y lista de idiomas

La compatibilidad de idiomas se introduce inicialmente en la versión preliminar, pasando de forma gradual al estado de disponible de forma general (GA), independientemente de los demás y del servicio de Text Analytics en general. Los idiomas pueden mantenerse en versión preliminar, incluso mientras se realiza la transición de Text Analytics API a disponible de forma general.

| Idioma    | Código de idioma | Opinión | Frases clave | Reconocimiento de entidades con nombre |   Notas  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Árabe      | `ar`          |           |             | ✔ \*                     | |
| Checo       | `cs`          |           |             | ✔ \*                     | |
| Chino simplificado | `zh-hans`| ✔ \***     |             | ✔         |    |
| Chino (tradicional) | `zh-hant`| ✔ \***     |             | ✔         |    |
| Danés      | `da`          | ✔ \*     | ✔           | ✔ \*            |     |
| Neerlandés       | `nl`          | ✔ \*     | ✔          |  ✔ \*           |     |
| English     | `en`          | ✔ \***       | ✔           |  ✔ \*\*     |      |
| Finés     | `fi`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Francés      | `fr`          | ✔ \***       | ✔           |  ✔            |     |
| Alemán      | `de`          | ✔ \*     | ✔           |  ✔           |     |
| Griego       | `el`          | ✔ \*     |             |            |     |
| Húngaro   | `hu`          |           |             |  ✔ \*          |     | 
| Italiano     | `it`          | ✔ \***     | ✔           |  ✔ \*           |     |
| Japonés    | `ja`          | ✔ \***         | ✔           |  ✔ \*          |     |
| Coreano      | `ko`          |          | ✔           |  ✔ \*          |     |
| Noruego (Bokmål) | `no`  | ✔ \*     |  ✔          | ✔ \*            |     |
| Polaco      | `pl`          | ✔ \*     |  ✔          |  ✔ \*           |     |
| Portugués (Portugal) | `pt-PT`| ✔        |  ✔          | ✔ \*      |También se acepta `pt`|
| Portugués (Brasil)   | `pt-BR`|          |  ✔   |  ✔ \*       |     |
| Ruso     | `ru`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Español     | `es`          | ✔        | ✔           |   ✔ \*\*      |     | 
| Sueco     | `sv`          | ✔ \*     | ✔           |   ✔ \*          |     |
| Turco     | `tr`          | ✔ \*     |             |   ✔ \*          |  |

\* La compatibilidad con idiomas está en versión preliminar

\*\* [Reconocimiento de entidades con nombre](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-ner) y [Vinculación de entidad](how-tos/text-analytics-how-to-entity-linking.md#entity-linking) están disponibles para este idioma.  

\*** Disponible en la [versión preliminar pública de Análisis de sentimiento V3](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis#sentiment-analysis-v3-public-preview)

## <a name="see-also"></a>Otras referencias

[Página de documentación de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)   
[Página de producto de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
