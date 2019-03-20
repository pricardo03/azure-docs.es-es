---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 31482cc7850c574b952c454021af729da324ba15
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "58115227"
---
<!-- N.B. no header, no intents here, language-agnostic -->

Cognitive Services [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) proporciona la manera más sencilla de usar **Conversión de voz en texto** en la aplicación con una funcionalidad completa.

1. Cree una configuración de voz y use una clave de suscripción al servicio Voz (o un token de autorización) y una [región](~/articles/cognitive-services/speech-service/regions.md) como parámetros. Cambie la configuración según sea necesario. Por ejemplo, indique un punto de conexión personalizado para especificar un extremo de servicio no estándar, o bien seleccione el idioma de entrada de voz o el formato de salida.

1. Cree un reconocedor de voz a partir de la configuración de la voz. Especifique una configuración de audio si quiere reconocer desde un origen que no sea el micrófono predeterminado (por ejemplo, una secuencia de audio o un archivo de audio).

1. Si quiere, asocie los eventos para la operación asincrónica. Después, el reconocedor llama a los controladores de eventos cuando tiene resultados provisionales y finales. En caso contrario, la aplicación recibirá solo un resultado de transcripción final.

1. Inicie el reconocimiento. Para realizar reconocimientos rápidos (por ejemplo, el reconocimiento de una orden o una pregunta), utilice el método `RecognizeOnceAsync()` (o el equivalente en su lenguaje). Este método devuelve la primera expresión reconocida. Para reconocimientos de larga ejecución, como transcripciones, utilice el método `StartContinuousRecognitionAsync()` (o el equivalente en su lenguaje). Asocie los eventos para obtener los resultados de reconocimiento asincrónico.

Consulte los siguientes fragmentos de código para escenarios de reconocimiento de voz que utilizan Speech SDK.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
