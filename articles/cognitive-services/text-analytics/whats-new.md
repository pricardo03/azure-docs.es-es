---
title: Novedades de Text Analytics API
titlesuffix: Text Analytics - Azure Cognitive Services
description: Obtenga información sobre los avances recientes del servicio de Text Analytics
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: aahi
ms.openlocfilehash: 44ef6fb118be4d1110a693faded6c57bc8b4e2fd
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499953"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Novedades de Text Analytics API

Text Analytics API se actualiza constantemente. Para mantenerse al día con los avances recientes, en este artículo se proporciona información sobre las nuevas versiones y características.

## <a name="named-entity-recognition-v3-public-preview---october-2019"></a>Versión preliminar pública de Reconocimiento de entidades con nombre v3: octubre de 2019

La siguiente versión de Reconocimiento de entidades con nombre (ER) ya está disponible para en versión preliminar pública y proporciona detección y categorización mejoradas de las entidades que se encuentran en el texto. Ofrece:

* Reconocimiento de los siguientes tipos de entidad nuevos:
    * Número de teléfono
    * Dirección IP

* Un nuevo punto de conexión para reconocer los tipos de entidad de información personal (solo en inglés)
* Puntos de conexión para independientes para el reconocimiento de entidades y la vinculación de entidades.

La vinculación de entidades admite el inglés y el español. La compatibilidad de idiomas de NER varía según el tipo de entidad. Para obtener más información, haga clic en el vínculo a continuación. 

> [!div class="nextstepaction"]
> [Más información sobre el Reconocimiento de entidades con nombre v3](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-v3-public-preview)

## <a name="sentiment-analysis-v3-public-preview---october-2019"></a>Versión preliminar pública de Análisis de sentimiento v3: octubre de 2019

La próxima versión de Análisis de sentimiento ya está disponible para la versión preliminar pública, que proporciona mejoras significativas en la precisión y el detalle de la categorización y puntuación del texto de la API. Además, proporciona:

* Etiquetado automático para los distintos sentimientos en el texto.
* Análisis de sentimiento y salida a nivel de documento y oración. 

Admite inglés (`en`), japonés (`ja`), chino simplificado (`zh-Hans`), chino tradicional (`zh-Hant`), francés (`fr`), italiano (`it`), español (`es`), neerlandés (`nl`), portugués (`pt`) y alemán (`de`) y está disponible en las siguientes regiones: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` y `West US 2`.

> [!div class="nextstepaction"]
> [Más información sobre Análisis de sentimiento v3](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-v3-public-preview)

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es Text Analytics API?](overview.md)  
* [Ejemplos de escenarios de usuario](text-analytics-user-scenarios.md)
* [Análisis de opiniones](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Detección de idioma](how-tos/text-analytics-how-to-language-detection.md)
* [Reconocimiento de entidades](how-tos/text-analytics-how-to-entity-linking.md)
* [Extracción de frases clave](how-tos/text-analytics-how-to-keyword-extraction.md)
