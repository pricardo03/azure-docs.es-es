---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 1103e5a217ca4972cc1c983a7ad0d07b0797e9e9
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43129365"
---
<!-- N.B. no header, no intents here, language-agnostic -->

Cognitive Services [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) proporciona la manera más sencilla de usar **Conversión de voz en texto** en la aplicación con una funcionalidad completa.

1. Cree una factoría de voz y proporcione una clave de suscripción a Speech Service (o un token de autorización) y una [región](~/articles/cognitive-services/speech-service/regions.md) como parámetros. También puede proporcionar un punto de conexión personalizado para especificar un punto de conexión de servicio no estándar.

1. Obtenga un reconocedor de voz a partir de la factoría de voz. Puede configurar el idioma de entrada y el formato de salida. Un reconocedor puede usar el micrófono predeterminado del dispositivo, una secuencia de audio o audio de un archivo.

1. Si quiere, asocie los eventos para la operación asincrónica. Después, el reconocedor llama a los controladores de eventos cuando tiene resultados provisionales y finales. En caso contrario, la aplicación recibirá solo un resultado de transcripción final.

1. Inicie el reconocimiento. Para el reconocimiento de una toma como, por ejemplo, el reconocimiento de comandos o consultas, use el método `RecognizeAsync()`. Este método devuelve la primera expresión reconocida. Para un reconocimiento de larga ejecución como el de la transcripción, utilice el método `StartContinuousRecognitionAsync()`. Asocie los eventos para obtener los resultados de reconocimiento asincrónico.

Consulte los siguientes fragmentos de código para escenarios de reconocimiento de voz que utilizan Speech SDK.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
