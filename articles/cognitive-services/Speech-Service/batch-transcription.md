---
title: 'Cómo usar la transcripción de lotes: servicio Voz'
titleSuffix: Azure Cognitive Services
description: La transcripción de lotes es ideal si desea transcribir una gran cantidad de audio en el almacenamiento, como los blobs de Azure. Mediante la API REST dedicada, puede apuntar a archivos de audio con un identificador URI de firma de acceso compartido (SAS) y recibir las transcripciones de forma asincrónica.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: panosper
ms.openlocfilehash: e98ac335f475e4a6db6b17d5954512f5749e6aec
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881154"
---
# <a name="why-use-batch-transcription"></a>¿Por qué usar la transcripción de lotes?

La transcripción de lotes es ideal si desea transcribir una gran cantidad de audio en el almacenamiento, como los blobs de Azure. Mediante la API REST dedicada, puede apuntar a archivos de audio con un identificador URI de firma de acceso compartido (SAS) y recibir las transcripciones de forma asincrónica.

## <a name="prerequisites"></a>Requisitos previos

### <a name="subscription-key"></a>Clave de suscripción

Como sucede con todas las características del servicio Voz, puede crear una clave de suscripción en [Azure Portal](https://portal.azure.com) siguiendo nuestra [guía de inicio](get-started.md). Si va a obtener transcripciones de nuestros modelos de base de referencia, todo lo que necesita hacer es crear una clave.

>[!NOTE]
> Se requiere una suscripción estándar (S0) para los servicios de Voz para usar la transcripción de lotes. Las claves de suscripción gratuita (F0) no funcionarán. Para más información, consulte [Precios y límites](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Modelos personalizados

Si tiene previsto personalizar modelos acústicos o de lenguaje, siga los pasos que se indican en [Creación de modelos acústicos](how-to-customize-acoustic-models.md) y [Personalización de modelos de lenguaje](how-to-customize-language-model.md). Para usar los modelos creados en la transcripción por lotes, necesita sus identificadores de modelo. Este identificador no es el mismo que el del punto de conexión que se encuentra en la vista de detalles del punto de conexión, sino el identificador del modelo que puede recuperar al seleccionar los detalles de los modelos.

## <a name="the-batch-transcription-api"></a>Transcription API de Azure Batch

Transcription API de Batch ofrece la transcripción asincrónica de voz a texto, además de otras características. Es una API REST que expone métodos para:

1. Crear solicitudes de procesamiento por lotes
1. Consultar el estado
1. Descargar transcripciones

> [!NOTE]
> Esta API resulta muy adecuada para centros de llamadas que suelen acumular miles de horas de audio. Facilita la transcripción de grandes volúmenes de grabaciones de audio.

### <a name="supported-formats"></a>Formatos compatibles

Transcription API de Batch admite los siguientes formatos:

| Formato | Códec | Bitrate | Velocidad de muestreo |
|--------|-------|---------|-------------|
| WAV | PCM | 16 bits | 8 o 16 kHz, mono, estéreo |
| MP3 | PCM | 16 bits | 8 o 16 kHz, mono, estéreo |
| OGG | OPUS | 16 bits | 8 o 16 kHz, mono, estéreo |

Si se trata de secuencias de audio estéreo, Batch Transcription API divide los canales izquierdo y derecho durante la transcripción. Los dos archivos JSON con el resultado se crean desde un único canal. Las marcas de tiempo por expresión permiten al desarrollador crear una transcripción final ordenada. Esta solicitud de ejemplo incluye las propiedades de filtrado de blasfemias, signos de puntuación y marcas de tiempo de nivel de palabra.

### <a name="configuration"></a>Configuración

Los parámetros de configuración se proporcionan como JSON:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True",
    "AddSentiment" : "True"
  }
}
```

> [!NOTE]
> Transcription API de Batch usa un servicio REST para solicitar transcripciones, su estado y los resultados asociados. Puede usar la API con cualquier lenguaje. En la siguiente sección se describe cómo se usa la API.

### <a name="configuration-properties"></a>Propiedades de configuración

Utilice estas propiedades opcionales para configurar la transcripción:

| Parámetro | DESCRIPCIÓN |
|-----------|-------------|
| `ProfanityFilterMode` | Especifica cómo controlar las palabras soeces en los resultados del reconocimiento. Los valores aceptados son `None`, que deshabilita el filtrado de palabras soeces; `masked`, que reemplaza las palabras soeces con asteriscos; `removed`, que quita todas las palabras soeces del resultado; o `tags`, que agrega etiquetas de "palabras soeces". La configuración predeterminada es `masked`. |
| `PunctuationMode` | Especifica cómo controlar la puntuación en los resultados del reconocimiento. Los valores aceptados son: `None`, que deshabilita la puntuación; `dictated`, que implica puntuación explícita; `automatic`, que permite que el descodificador se ocupe de la puntuación; o `dictatedandautomatic`, que implica signos de puntuación dictados o puntuación automática. |
 | `AddWordLevelTimestamps` | Especifica si las marcas de tiempo de nivel de palabra se deben agregar a la salida. Los valores aceptados son `true`, que permite las marcas de tiempo de nivel de palabra, y `false` (el valor predeterminado) que las deshabilita. |
 | `AddSentiment` | Especifica que la opinión se debe agregar a la expresión. Los valores aceptados son `true`, que permite la opinión por expresión y `false` (el valor predeterminado) que la deshabilita. |
 | `AddDiarization` | Especifica que el análisis de la diarización se debe llevar a cabo en la entrada que se espera sea un canal mono que contenga dos voces. Los valores aceptados son `true`, que permite la diarización, y `false` (el valor predeterminado) que la deshabilita. También requiere que `AddWordLevelTimestamps` se establezca en true.|

### <a name="storage"></a>Storage

La transcripción de lotes admite [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) para leer el audio y escribir las transcripciones en el almacenamiento.

## <a name="webhooks"></a>webhooks

El sondeo del estado de transcripción puede no ser el de mayor rendimiento o proporcionar la mejor experiencia de usuario. Para sondear el estado, puede registrar devoluciones de llamada, que notificarán al cliente cuando se hayan completado las tareas de transcripción de larga ejecución.

Para más información, consulte [Webhooks](webhooks.md).

## <a name="speaker-separation-diarization"></a>Separación de altavoces (diarización)

La diarización es el proceso de separación de los altavoces en una parte del audio. Nuestra canalización de Batch admite la diarización y es capaz de reconocer dos altavoces en las grabaciones de un canal mono.

Para pedir que la solicitud de transcripción de audio se procese para la diarización, simplemente tiene que agregar el parámetro pertinente en la solicitud HTTP como se muestra a continuación.

 ```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "AddWordLevelTimestamps" : "True",
    "AddDiarization" : "True"
  }
}
```

Las marcas de tiempo de nivel de palabra también tendría que estar "activadas" como indican los parámetros de la solicitud anterior.

El audio correspondiente contendrá los altavoces identificados por un número (actualmente se admiten solo dos voces, por lo que los altavoces se identificarán como "Altavoz 1" y "Altavoz 2") seguidos de la salida de la transcripción.

Tenga en cuenta también que la diarización no está disponible en las grabaciones estéreo. Además, toda la salida JSON contendrá la etiqueta Speaker. Si no se usa diarización, mostrará "Speaker: Null" en la salida JSON.

> [!NOTE]
> ¡La diarización está disponible en todas las regiones y para todas las configuraciones regionales!

## <a name="sentiment"></a>Opinión

Opinión es una nueva característica de Transcription API de Batch, que es importante en el dominio del centro de llamadas. Los clientes pueden usar los parámetros `AddSentiment` en sus solicitudes para

1.  Obtener información sobre la satisfacción del cliente
2.  Obtener información sobre el rendimiento de los agentes (equipo que atiende las llamadas)
3.  Identificar el punto exacto en el tiempo de cuándo una llamada giró hacia una dirección negativa
4.  Identificar qué salió bien al convertir las llamadas negativas en positivas
5.  Identificar lo que les gusta a los clientes y lo que no les gusta de un producto o servicio

Opinión se puntúa por segmento de audio, donde un segmento de audio se define como el lapso de tiempo entre el inicio de la expresión (desplazamiento) y el silencio de detección del fin del flujo de bytes. Todo el texto dentro de ese segmento se utiliza para calcular la opinión. NO se calcula ningún valor agregado de opinión para toda la llamada o la voz completa de cada canal. Estas agregaciones se dejan al propietario del dominio para seguir aplicándolas.

Opinión se aplica en la forma léxica.

Un ejemplo de salida JSON tiene el siguiente aspecto:

```json
{
  "AudioFileResults": [
    {
      "AudioFileName": "Channel.0.wav",
      "AudioFileUrl": null,
      "SegmentResults": [
        {
          "RecognitionStatus": "Success",
          "ChannelNumber": null,
          "Offset": 400000,
          "Duration": 13300000,
          "NBest": [
            {
              "Confidence": 0.976174,
              "Lexical": "what's the weather like",
              "ITN": "what's the weather like",
              "MaskedITN": "what's the weather like",
              "Display": "What's the weather like?",
              "Words": null,
              "Sentiment": {
                "Negative": 0.206194,
                "Neutral": 0.793785,
                "Positive": 0.0
              }
            }
          ]
        }
      ]
    }
  ]
}
```
La característica utiliza un modelo de Opinión, que se encuentra actualmente en versión Beta.

## <a name="sample-code"></a>Código de ejemplo

Hay ejemplos completos disponibles en el [repositorio de ejemplos de GitHub](https://aka.ms/csspeech/samples) dentro del subdirectorio `samples/batch`.

Si quiere usar un modelo acústico o de lenguaje personalizado, deberá personalizar el código de ejemplo con la información de suscripción, la región del servicio, el URI de SAS que apunta al archivo de audio que se va a transcribir y los identificadores del modelo.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

El código de ejemplo configurará el cliente y enviará la solicitud de transcripción. A continuación, sondeará la información de estado e imprimirá los detalles sobre el progreso de la transcripción.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Para más información sobre las llamadas anteriores, consulte nuestro [documento de Swagger](https://westus.cris.ai/swagger/ui/index). Para ver el ejemplo completo aquí mostrado, vaya a [GitHub](https://aka.ms/csspeech/samples) en el subdirectorio `samples/batch`.

Tome nota de la configuración asincrónica para publicar audio y recibir el estado de la transcripción. El cliente que crea es un cliente HTTP de .NET. Hay un método `PostTranscriptions` para enviar los detalles del archivo de audio y un método `GetTranscriptions` para recibir los resultados. `PostTranscriptions` devuelve un identificador y `GetTranscriptions` lo usa para crear un identificador y obtener el estado de la transcripción.

El ejemplo de código actual no especifica un modelo personalizado. El servicio usa los modelos base de referencia para transcribir los archivos. Para especificar los modelos, puede pasar los mismos identificadores de modelo para el modelo acústico y de lenguaje.

> [!NOTE]
> En el caso de transcripciones de base de referencia, no es necesario declarar los identificadores de los modelos de base de referencia. Si solo especifica un identificador de modelo de lenguaje (y ningún identificador de modelo acústico), se selecciona automáticamente un modelo acústico correspondiente. Si solo especifica un identificador de modelo acústico, se selecciona automáticamente un modelo de lenguaje correspondiente.

## <a name="download-the-sample"></a>Descarga del ejemplo

Puede encontrar el ejemplo en el directorio `samples/batch` en el [repositorio de ejemplos de GitHub](https://aka.ms/csspeech/samples).

> [!NOTE]
> Los trabajos de transcripción por lotes están programados de la mejor manera posible; no hay una estimación de tiempo de cuándo un trabajo cambiará al estado en ejecución. En este estado, la transcripción real se procesa más rápido que el audio en tiempo real.

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
