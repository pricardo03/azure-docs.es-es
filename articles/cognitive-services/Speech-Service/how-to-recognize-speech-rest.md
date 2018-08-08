---
title: Reconocimiento de voz con la API REST
description: Obtenga información sobre cómo usar Conversión de voz en texto en el servicio Voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 15db2d9b872c76d70fd531af07fb55c701e86494
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331342"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Reconocimiento de voz con la API REST

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

La API REST se puede utilizar para reconocer expresiones cortas mediante una solicitud HTTP POST.

La API REST es la manera más sencilla de reconocer la voz si no se usa un idioma admitido por el SDK.
Se realiza una solicitud POST de HTTP al punto de conexión de servicio, y se pasa la expresión completa en el cuerpo de la solicitud.
Se recibe una respuesta que contiene el texto reconocido.

> [!NOTE]
> Cuando se usa la API REST, las expresiones se limitan a 15 segundos o menos.
> Consulte [Speech SDK](how-to-recognize-speech-csharp.md) para el reconocimiento de expresiones más largas.

Para obtener más información sobre la API REST de **Conversión de voz en texto**, vea [API REST](rest-apis.md#speech-to-text). Para verla en acción, descargue los [ejemplos de API REST](https://github.com/Azure-Samples/SpeechToText-REST) desde GitHub.

## <a name="next-steps"></a>Pasos siguientes

- [Consulte la introducción a la API REST](rest-apis.md)
