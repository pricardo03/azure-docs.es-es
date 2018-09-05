---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: ee50a104a75d3cd5ff958bd49a1ff7010c5d5083
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43144402"
---
<!-- N.B. no header, language-agnostic -->

Microsoft Cognitive Services [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) proporciona la manera más sencilla de usar **traducción de voz** en la aplicación.
El SDK proporciona toda la funcionalidad del servicio. El proceso básico para llevar a cabo la traducción de voz incluye los pasos siguientes:

1. Cree una factoría de voz y proporcione una clave de suscripción a Speech Service o un token de autorización, y una [región](~/articles/cognitive-services/speech-service/regions.md) como parámetros.
   
1. Cree un reconocedor de traducción a partir de la factoría de voz. Puede configurar los idiomas de traducción de origen y de destino, así como especificar si quiere salida de texto o de voz. Hay distintas versiones de reconocedores de traducción en función del origen de audio que se use.

1. Si quiere, asocie los eventos para la operación asincrónica. El reconocedor llama a los controladores de eventos cuando tiene resultados provisionales y finales, así como un evento de síntesis para la salida de audio opcional. En caso contrario, la aplicación recibirá solo un resultado de transcripción final.

1. Inicie el reconocimiento. Para la traducción de una toma, use el método `RecognizeAsync()`, que devuelve la primera expresión reconocida. Para traducciones de larga ejecución, como la transcripción, utilice el método `StartContinuousRecognitionAsync()` y asocie los eventos de resultados de reconocimiento asincrónicos.

Consulte los siguientes fragmentos de código para escenarios de traducción de voz que utilizan Speech SDK.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
