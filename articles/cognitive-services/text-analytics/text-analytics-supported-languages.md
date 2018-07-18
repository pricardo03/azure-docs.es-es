---
title: 'Idiomas admitidos en Text Analytics API: Azure Cognitive Services | Microsoft Docs'
description: Lista de operaciones de previsualización de idiomas generalmente disponibles admitidos en Text Analytics API. Se aplica al análisis de opiniones, a la extracción de frases clave y a la detección de idioma.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: 2d341cfaf261bea6367bb55dd5d322f419e22d34
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382547"
---
# <a name="supported-languages-in-the-text-analytics-api"></a>Idiomas admitidos en Text Analytics API

En este artículo se explica qué idiomas se admiten para cada operación: análisis de opiniones, extracción de frases clave y detección de idioma.

## <a name="language-detection"></a>Detección de idiomas

Text Analytics API puede detectar hasta 120 idiomas diferentes. La detección de idioma devuelve el "script" de un idioma. Por ejemplo, en el caso de la frase "Tengo un perro", se devolverá `en` en lugar de `en-US`. El único caso especial es el chino, donde la funcionalidad de detección de idioma devolverá `zh_CHS` o `zh_CHT` si puede determinar el script dado el texto proporcionado. En situaciones donde no se puede identificar un script específico para un documento chino, devolverá simplemente `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-linking"></a>Análisis de sentimiento, extracción de frases clave y vinculación de entidad

Para el análisis de opiniones, la extracción de frases clave y la vinculación de entidad, la lista de idiomas admitidos es más selectiva, ya que los analizadores se perfeccionan para dar cabida a las reglas lingüísticas de idiomas adicionales.

## <a name="language-list-and-status"></a>Estado y lista de idiomas

La compatibilidad de idiomas se introduce inicialmente en la versión preliminar, pasando de forma gradual al estado de disponible de forma general (GA), independientemente de los demás y del servicio de Text Analytics en general. Los idiomas pueden mantenerse en versión preliminar, incluso mientras se realiza la transición de Text Analytics API a disponible de forma general.

| Idioma    | Código de idioma | Opinión | Frases clave | Entity Linking |   Notas  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Danés      | `da`          | ✔ \*     | ✔           |             |     |
| Neerlandés       | `nl`          | ✔ \*     | ✔          |             |     |
| English     | `en`          | ✔        | ✔           |  ✔ \*   |      |
| Finés     | `fi`          | ✔ \*     | ✔           |             |     |
| Francés      | `fr`          | ✔        | ✔           |             |     |
| Alemán      | `de`          | ✔ \*     | ✔           |            |     |
| Griego       | `el`          | ✔ \*     |             |            |     |
| Italiano     | `it`          | ✔ \*     | ✔           |             |     |
| Japonés    | `ja`          |          | ✔           |            |     |
| Coreano      | `ko`          |          | ✔           |            |     |
| Noruego (Bokmål) | `no`          | ✔ \*     |  ✔          |             |     |
| Polaco      | `pl`          | ✔ \*     |  ✔          |             |     |
| Portugués (Portugal) | `pt-PT`| ✔        |  ✔          |       |También se acepta `pt`|
| Portugués (Brasil)   | `pt-BR`|          |  ✔   |         |     |
| Ruso     | `ru`          | ✔ \*     | ✔           |             |     |
| Español     | `es`          | ✔        | ✔           |     |     |
| Sueco     | `sv`          | ✔ \*     | ✔           |             |     |
| Turco     | `tr`          | ✔ \*     |             |             |     |

\* indica la compatibilidad de idioma en la versión preliminar

## <a name="see-also"></a>Otras referencias

[Página de documentación de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)   
[Página de producto de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
