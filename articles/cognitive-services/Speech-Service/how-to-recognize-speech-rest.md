---
title: Reconocimiento de voz con la API REST
titleSuffix: Azure Cognitive Services
description: Aprenda a usar Speech to Text API del servicio Cognitive Services Speech.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: 1bcc290d987e8294f9a73cf8490f6cdd0251b440
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49467283"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Reconocimiento de voz con la API REST

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

La API REST se puede usar para reconocer expresiones cortas mediante una solicitud HTTP POST.

La API REST es la manera más sencilla de reconocer la voz si no se usa un idioma admitido por el [SDK](speech-sdk.md). Se realiza una solicitud HTTP POST al punto de conexión de servicio y se pasa la expresión completa en el cuerpo de la solicitud. Se recibe una respuesta que contiene el texto reconocido.

> [!NOTE]
> Cuando se usa la API REST, las expresiones se limitan a 15 segundos o menos.
> Consulte [Speech SDK](how-to-recognize-speech-csharp.md) para el reconocimiento de expresiones más largas.

Para más información sobre la API REST de **conversión de voz en texto**, consulte el artículo [API REST](rest-apis.md#speech-to-text). Para ver la API en acción, descargue los [ejemplos de API REST](https://github.com/Azure-Samples/SpeechToText-REST) desde GitHub.

## <a name="next-steps"></a>Pasos siguientes

- Consulte la [introducción a la API REST](rest-apis.md).
