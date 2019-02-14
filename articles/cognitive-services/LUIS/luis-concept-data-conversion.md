---
title: Conversión de datos
titleSuffix: Language Understanding - Azure Cognitive Services
description: Obtenga información sobre cómo se pueden modificar las expresiones antes de las predicciones en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: diberry
ms.openlocfilehash: bdf2d87e558726937fa4221a0d95179216c66051
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859207"
---
# <a name="convert-data-format-of-utterances"></a>Conversión del formato de datos de expresiones
LUIS usa el servicio Speech de Cognitive Services para convertir las expresiones de voz en texto antes de la predicción. 

## <a name="speech-to-intent-conversion-concepts"></a>Conceptos de la conversión de voz en intención
La conversión de voz en texto en LUIS permite enviar expresiones de voz a un punto de conexión y recibir una respuesta de predicción de LUIS. El proceso consiste en integrar el servicio [Speech](https://docs.microsoft.com/azure/cognitive-services/Speech) con LUIS. Más información sobre la conversión de voz en intención con un [tutorial](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Requisitos principales
No es necesario crear una clave de **Bing Speech API** para esta integración. Para esta integración, solo servirá una clave de **Language Understanding** creada en Azure Portal. No utilice la clave de inicio de LUIS.

### <a name="pricing-tier"></a>Nivel de precios
Esta integración usa otro modelo de [precios](luis-boundaries.md#key-limits) distinto de los planes de tarifa habituales de Language Understanding. 

### <a name="quota-usage"></a>Uso de cuota
Vea los [Límites clave](luis-boundaries.md#key-limits) para obtener información. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Use la conversión de voz a texto](luis-tutorial-speech-to-intent.md)

