---
title: Reconocimiento de voz con la API REST
titleSuffix: Azure Cognitive Services
description: Aprenda a usar Speech to Text API del servicio Cognitive Services Speech.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: 7c7bbaa986a6efdb82a50048c7c218f96cd4014a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220338"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Reconocimiento de voz con la API REST

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

La API REST se puede usar para reconocer expresiones cortas mediante una solicitud HTTP POST.

La API REST es la manera más sencilla de reconocer la voz si no se usa un idioma admitido por el [SDK](speech-sdk.md). Se realiza una solicitud HTTP POST al punto de conexión de servicio y se pasa la expresión completa en el cuerpo de la solicitud. Se recibe una respuesta que contiene el texto reconocido.

> [!NOTE]
> Cuando se usa la API REST, las expresiones se limitan a 15 segundos o menos.
> Consulte [Speech SDK](how-to-recognize-speech-csharp.md) para el reconocimiento de expresiones más largas.

Para más información sobre la API REST de **conversión de voz en texto**, consulte el artículo [API REST](rest-apis.md#speech-to-text-api). Para ver la API en acción, descargue los [ejemplos de API REST](https://github.com/Azure-Samples/SpeechToText-REST) desde GitHub.

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [introducción a la API REST](rest-apis.md).
