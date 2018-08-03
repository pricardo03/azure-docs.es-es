---
title: Escenarios de Services Speech de Azure Cognitive Services | Azure Microsoft Docs
description: Escenarios
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 07/02/2018
ms.author: panosper
ms.openlocfilehash: 9e6be3608f5aa5ec5d68e6bbefff6da6c23c62fd
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247961"
---
# <a name="speech-scenarios"></a>Escenarios de voz

Hay muchos escenarios que pueden estar dotados con tecnologías de Voz. En la documentación, se analizan algunos de los más comunes y se señalan las características importantes. Para la mayor parte del contenido, el [SDK](speech-sdk.md) es fundamental en la habilitación de estos escenarios.

En la página se describe cómo hacer lo siguiente:
> [!div class="checklist"]
> * Crear aplicaciones que se activan por voz
> * Transcribir llamadas de audio de los centros de llamadas
> * Bots de voz

## <a name="voice-triggered-apps"></a>Aplicaciones que se activan por voz

Muchos usuarios desean habilitar la entrada de voz en sus aplicaciones. La entrada de voz es una excelente manera de hacer que la aplicación sea flexible, ya sea usando manos libres (por ejemplo, en un automóvil) o acelerando diversas tareas, como preguntar direcciones hasta noticias o información sobre el clima. 

### <a name="voice-triggered-apps-with-baseline-models"></a>Aplicaciones activadas por voz con modelos de línea base

Si la aplicación la va a usar el público general en entornos donde el ruido de fondo no es excesivo, la manera más fácil y rápida de hacerlo es simplemente descargar el [SDK de Voz](speech-sdk.md) y seguir los [ejemplos](quickstart-csharp-dotnet-windows.md) pertinentes. El SDK con el respaldo de su[clave de suscripción de Azure](https://azure.microsoft.com/try/cognitive-services/) permite que los desarrolladores carguen audio a los modelos de reconocimiento de voz de línea base que impulsan a Cortana y Skype. Los modelos son la vanguardia y los usan los productos ya mencionados. Puede estar listo para empezar en cuestión de minutos.

### <a name="voice-triggered-apps-with-custom-models"></a>Aplicaciones activadas por voz con modelos personalizados

Si la aplicación aborda un dominio específico, (como química, biología o necesidades alimentarias especiales), es posible que desee considerar la adaptación de un [modelo de lenguaje](how-to-customize-language-model.md). Adaptar un modelo de lenguaje enseñará al descodificador las frases y palabras más comunes que usa la aplicación. El descodificador podrá transcribir con más precisión una entrada de voz con un modelo de lenguaje personalizado para un dominio determinado en lugar del modelo de línea base. De manera similar, si el ruido de fondo donde se va a usar la aplicación es importante, es posible que quiera adaptar un modelo acústico. Explore la documentación para otros casos en los que la [adaptación de lenguaje](how-to-customize-language-model.md) y la [adaptación acústica](how-to-customize-acoustic-models.md) proporcionen valor y visite nuestro [portal de adaptación](https://customspeech.ai) para iniciar la experiencia de creación del modelo. De un modo similar a los modelos de línea base, los modelos personalizados se llaman a través del [SDK de Voz](speech-sdk.md) y mediante los [ejemplos](quickstart-csharp-dotnet-windows.md) pertinentes.

## <a name="transcribe-call-center-audio-calls"></a>Transcripción de llamadas de audio de centro de llamadas

Los centros de llamadas acumulan grandes volúmenes de audio. A través de la transcripción se puede obtener el valor que está oculto dentro de esos archivos de audio. La duración de la llamada, la opinión, la satisfacción del cliente y el valor general que la llamada proporciona al autor de la llamada se pueden detectar mediante la obtención de las transcripciones de las llamadas.

El mejor punto inicial es la [API de transcripciones de Azure Batch](batch-transcription.md) junto con los [ejemplos](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI) relacionados.

Primero deberá obtener una [clave de suscripción de Azure](https://azure.microsoft.com/try/cognitive-services/) y luego tendrá que consultar la [documentación]([Batch transcription API](batch-transcription.md)).

### <a name="transcribe-call-center-audio-calls-with-baseline-models"></a>Transcripción de llamadas de audio de los centros de llamadas con modelos de línea base

La decisión que se debe tomar es si va a usar los modelos de línea base internos para llevar a cabo la transcripción, adaptar un lenguaje o un modelo acústico o ambas opciones. Para usar modelos de línea base, la API solo requiere la clave de API. De manera interna, la API invocará el mejor modelo para los datos y lo adaptará.

### <a name="transcribe-call-center-audio-calls-with-custom-models"></a>Transcribir llamadas de audio de centros de llamadas con modelos personalizados

Si pretende usar un modelo personalizado, necesitará el identificador de dicho modelo junto con la clave de API. El identificador de modelo se obtiene en el [portal de adaptación](https://customspeech.ai). Este no es el identificador del punto de conexión que se encuentra en la vista "Detalles de punto de conexión", sino del identificador del modelo que puede recuperar si hace clic en "Detalles" de dicho modelo.

## <a name="voice-bots"></a>Bots de voz

El desarrollador puede potenciar su aplicación con la salida de voz. Speech Service puede sintetizar la voz para varios [idiomas](supported-languages.md) y proporciona los [puntos de conexión](rest-apis.md) para obtener acceso y agregar esa funcionalidad a la aplicación.

Además, para los usuarios que quieren agregar más personalidad y exclusividad a los bots, Speech Service permite que los desarrolladores personalicen una fuente de voz única. De manera similar a la personalización de las fuentes de voz, los modelos de reconocimiento de voz requieren datos del usuario. Los desarrolladores cargan esos datos en el [portal de adaptación de voz](https://customspeech.ai) y empiece a crear su marca de voz única para el bot. Los detalles se describen [aquí](how-to-text-to-speech.md) y en las páginas de [preguntas más frecuentes](faq-text-to-speech.md) 

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Comience a usar el SDK de Voz](speech-sdk.md)
