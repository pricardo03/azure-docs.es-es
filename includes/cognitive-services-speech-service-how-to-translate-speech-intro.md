---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: ab4795325f733d15403b53f027daa4fb8c824cc6
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "58113962"
---
<!-- N.B. no header, language-agnostic -->

Microsoft Cognitive Services [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) proporciona la manera más sencilla de usar **traducción de voz** en la aplicación.
El SDK proporciona toda la funcionalidad del servicio. El proceso básico para llevar a cabo la traducción de voz incluye los pasos siguientes:

1. Cree una configuración de traducción de voz y proporcione una clave de suscripción al servicio Voz (o un token de autorización) y una [región](~/articles/cognitive-services/speech-service/regions.md) como parámetros. Cambie la configuración según sea necesario. Por ejemplo, puede configurar los idiomas de traducción de origen y de destino, así como especificar si quiere salida de texto o de voz.

1. Cree un reconocedor de traducción a partir de la configuración de traducción de voz. Especifique una configuración de audio si quiere reconocer desde un origen que no sea el micrófono predeterminado (por ejemplo, una secuencia de audio o un archivo de audio).

1. Si quiere, asocie los eventos para la operación asincrónica. El reconocedor llama a los controladores de eventos cuando tiene resultados provisionales y finales, así como un evento de síntesis para la salida de audio opcional. En caso contrario, la aplicación recibirá solo un resultado de transcripción final.

1. Inicie el reconocimiento. Para la traducción de una toma, use el método `RecognizeOnceAsync()`, que devuelve la primera expresión reconocida. Para traducciones de larga ejecución, como la transcripción, utilice el método `StartContinuousRecognitionAsync()` y asocie los eventos de resultados de reconocimiento asincrónicos.

Consulte los siguientes fragmentos de código para escenarios de traducción de voz que utilizan Speech SDK.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
