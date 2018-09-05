---
title: Reconocimiento de voz con la API REST
description: Aprenda a usar Speech to Text API del servicio Cognitive Services Speech.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: eafec2dd262098bc4b7e485293818b79debe3d27
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126853"
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
