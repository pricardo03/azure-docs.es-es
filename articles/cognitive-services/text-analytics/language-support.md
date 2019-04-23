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
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 4f1ce8fd44a501f594f3093789d1ef03e664d018
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60008501"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Compatibilidad de idiomas y regiones para Text Analytics API

En este artículo se explica qué lenguajes se admiten para cada operación: análisis de sentimiento, extracción de frases clave, la detección de idioma y reconocimiento de entidades con nombre.

## <a name="language-detection"></a>Detección de idiomas

Text Analytics API puede detectar hasta 120 idiomas diferentes. La detección de idioma devuelve el "script" de un idioma. Por ejemplo, en el caso de la frase "Tengo un perro", se devolverá `en` en lugar de `en-US`. El único caso especial es el chino, donde la funcionalidad de detección de idioma devolverá `zh_CHS` o `zh_CHT` si puede determinar el script dado el texto proporcionado. En situaciones donde no se puede identificar un script específico para un documento chino, devolverá simplemente `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Análisis de sentimiento, extracción de frases clave y el reconocimiento de entidades con nombre

Para el Análisis de sentimiento, la Extracción de frases clave y el Reconocimiento entidad, la lista de idiomas admitidos es más selectiva, ya que los analizadores se perfeccionan para dar cabida a las reglas lingüísticas de idiomas adicionales.

## <a name="language-list-and-status"></a>Estado y lista de idiomas

La compatibilidad de idiomas se introduce inicialmente en la versión preliminar, pasando de forma gradual al estado de disponible de forma general (GA), independientemente de los demás y del servicio de Text Analytics en general. Los idiomas pueden mantenerse en versión preliminar, incluso mientras se realiza la transición de Text Analytics API a disponible de forma general.

| Idioma    | Código de idioma | Opinión | Frases clave | Reconocimiento de entidades con nombre |   Notas  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Árabe      | `ar`          |           |             | ✔ \*                     | |
| Checo       | `cs`          |           |             | ✔ \*                     | |
| Chino simplificado | `zh-CN`|           |             | ✔ \*        |    |
| Danés      | `da`          | ✔ \*     | ✔           | ✔ \*            |     |
| Neerlandés       | `nl`          | ✔ \*     | ✔          |  ✔ \*           |     |
| English     | `en`          | ✔        | ✔           |  ✔ \*\*     |      |
| Finés     | `fi`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Francés      | `fr`          | ✔        | ✔           |  ✔ \*           |     |
| Alemán      | `de`          | ✔ \*     | ✔           |  ✔ \*          |     |
| Griego       | `el`          | ✔ \*     |             |            |     |
| Húngaro   | `hu`          |           |             |  ✔ \*          |     | 
| Italiano     | `it`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Japonés    | `ja`          |          | ✔           |  ✔ \*          |     |
| Coreano      | `ko`          |          | ✔           |  ✔ \*          |     |
| Noruego (Bokmål) | `no`  | ✔ \*     |  ✔          | ✔ \*            |     |
| Polaco      | `pl`          | ✔ \*     |  ✔          |  ✔ \*           |     |
| Portugués (Portugal) | `pt-PT`| ✔        |  ✔          | ✔ \*      |También se acepta `pt`|
| Portugués (Brasil)   | `pt-BR`|          |  ✔   |  ✔ \*       |     |
| Ruso     | `ru`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Español     | `es`          | ✔        |            |   ✔ \*\*      |     | 
| Sueco     | `sv`          | ✔ \*     | ✔           |   ✔ \*          |     |
| Turco     | `tr`          | ✔ \*     |             |   ✔ \*          |  |

\* Compatibilidad con idiomas que se encuentra en versión preliminar

\*\* Denominado reconocimiento de entidades y [Entity linking](how-tos/text-analytics-how-to-entity-linking.md) están disponibles para este idioma.    

## <a name="see-also"></a>Vea también

[Página de documentación de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)   
[Página de producto de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
