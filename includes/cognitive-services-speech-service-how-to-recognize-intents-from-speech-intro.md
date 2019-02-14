---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: 80bf9247bbb07fa61b7153e321b1991b82a9d616
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56247151"
---
<!-- N.B. no header, language-agnostic -->

Microsoft Cognitive Services [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) proporciona una manera de reconocer **intenciones a partir de contenido de voz** y es compatible con el servicio [Language Understanding Intelligent Service (LUIS)](https://www.luis.ai/home) de Cognitive Services.

1. Cree una configuración de voz con una clave de suscripción de LUIS y una [región](~/articles/cognitive-services/speech-service/regions.md#intent-recognition) como parámetros. La clave de suscripción de LUIS se denomina **clave de punto de conexión** en la documento de servicio. No se puede usar la clave de creación de LUIS. Consulte la nota que aparece más adelante en esta sección.

1. Cree un reconocedor de intenciones a partir de la configuración de voz. Especifique una configuración de audio si quiere reconocer desde un origen que no sea el micrófono predeterminado (por ejemplo, una secuencia de audio o un archivo de audio).

1. Obtenga el modelo de reconocimiento del lenguaje que se basa en su **AppId**. Agregue las intenciones que requiera.

1. Si quiere, asocie los eventos para la operación asincrónica. Después, el reconocedor llama a los controladores de eventos cuando tiene resultados provisionales y finales (incluye intenciones). Si no asocia los eventos, la aplicación recibe solo un resultado final de transcripción.

1. Inicie el reconocimiento de intenciones. Para el reconocimiento de una toma como, por ejemplo, el reconocimiento de comandos o consultas, use el método `RecognizeOnceAsync()`. Este método devuelve la primera expresión reconocida. Para el reconocimiento de larga ejecución, utilice el método `StartContinuousRecognitionAsync()`. Asocie los eventos para obtener los resultados de reconocimiento asincrónico.

Consulte los siguientes fragmentos de código para escenarios de reconocimiento de intenciones que utilizan Speech SDK. Reemplace los valores del ejemplo por su propia clave de suscripción a LUIS (clave de punto de conexión), la [región de la suscripción](~/articles/cognitive-services/speech-service/regions.md#intent-recognition) y el identificador **AppId** del modelo de intención.

> [!NOTE]
> A diferencia de otros servicios compatibles con Speech SDK, el reconocimiento de intenciones requiere una clave de suscripción específica (clave de punto de conexión de LUIS). Para más información acerca de la tecnología de reconocimiento de intenciones, consulte el [sitio web de LUIS](https://www.luis.ai). Para más información sobre cómo adquirir la **clave de punto de conexión**, consulte [Creación de una clave de punto de conexión de LUIS](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-how-to-azure-subscription).
