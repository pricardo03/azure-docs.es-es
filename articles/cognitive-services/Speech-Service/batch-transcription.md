---
title: 'Uso de la transcripción por lotes: servicio de voz'
titleSuffix: Azure Cognitive Services
description: La transcripción de lotes es ideal si desea transcribir una gran cantidad de audio en el almacenamiento, como los blobs de Azure. Mediante la API REST dedicada, puede apuntar a archivos de audio con un identificador URI de firma de acceso compartido (SAS) y recibir las transcripciones de forma asincrónica.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: panosper
ms.openlocfilehash: dc473c814cdd69204cddd976bc77f19b5db567b1
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77200085"
---
# <a name="how-to-use-batch-transcription"></a>Uso de la transcripción por lotes

La transcripción por lotes es ideal para transcribir una gran cantidad de audio en el almacenamiento. Mediante la API REST dedicada, puede apuntar a archivos de audio con un identificador URI de firma de acceso compartido (SAS) y recibir los resultados de las transcripciones de forma asincrónica.

La API ofrece transcripción de voz a texto asincrónica y otras características. Puede usar la API REST para exponer métodos con los siguientes propósitos:

- Crear solicitudes de procesamiento por lotes
- Consultar el estado
- Descargar los resultados de las transcripciones
- Eliminar información de transcripciones del servicio

La API detallada está disponible como [documento de Swagger](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A), debajo del encabezado `Custom Speech transcriptions`.

Los trabajos de transcripción por lotes se programan de la mejor manera posible. Actualmente no hay ninguna estimación de cuándo un trabajo cambia al estado en ejecución. En la carga normal del sistema, se debe producir en cuestión de minutos. En este estado, la transcripción real se procesa más rápido que el audio en tiempo real.

Además de esta intuitiva API, no es necesario implementar puntos de conexión personalizados y no tiene que cumplir ningún requisito de simultaneidad.

## <a name="prerequisites"></a>Prerrequisitos

### <a name="subscription-key"></a>Clave de suscripción

Como sucede con todas las características del servicio Voz, puede crear una clave de suscripción en [Azure Portal](https://portal.azure.com) siguiendo nuestra [guía de inicio](get-started.md).

>[!NOTE]
> Se requiere una suscripción estándar (S0) para el servicio de voz para usar la transcripción de lotes. Las claves de suscripción gratuita (F0) no funcionarán. Para obtener más información, consulte los [precios y límites](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Modelos personalizados

Si tiene previsto personalizar modelos acústicos o de lenguaje, siga los pasos que se indican en los artículos de [creación de modelos acústicos](how-to-customize-acoustic-models.md) y [personalización de modelos de lenguaje](how-to-customize-language-model.md). Para usar los modelos creados en la transcripción por lotes, necesita sus identificadores de modelo. Puede recuperar el identificador del modelo al inspeccionar los detalles del modelo. No es necesario un punto de conexión personalizado implementado para el servicio de transcripción por lotes.

## <a name="the-batch-transcription-api"></a>Transcription API de Azure Batch

### <a name="supported-formats"></a>Formatos compatibles

Transcription API de Batch admite los siguientes formatos:

| Formato | Códec | Bitrate | Velocidad de muestreo |
|--------|-------|---------|-------------|
| WAV | PCM | 16 bits | 8 kHz o 16 kHz, mono o estéreo |
| MP3 | PCM | 16 bits | 8 kHz o 16 kHz, mono o estéreo |
| OGG | OPUS | 16 bits | 8 kHz o 16 kHz, mono o estéreo |

En el caso de las secuencias de audio estéreo, los canales izquierdo y derecho se dividen durante la transcripción. Para cada canal se crea un archivo de resultados JSON. Las marcas de tiempo por expresión generadas permiten al desarrollador crear una transcripción final ordenada.

### <a name="configuration"></a>Configuración

Los parámetros de configuración se proporcionan como JSON:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to use, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "None | Removed | Tags | Masked",
    "PunctuationMode": "None | Dictated | Automatic | DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True | False",
    "AddSentiment" : "True | False",
    "AddDiarization" : "True | False",
    "TranscriptionResultsContainerUrl" : "<service SAS URI to Azure container to store results into (write permission required)>"
  }
}
```

### <a name="configuration-properties"></a>Propiedades de configuración

Utilice estas propiedades opcionales para configurar la transcripción:

| Parámetro | Descripción |
|-----------|-------------|
| `ProfanityFilterMode` | Especifica cómo controlar las palabras soeces en los resultados del reconocimiento. Los valores aceptados son `None`, que deshabilita el filtrado de palabras soeces; `Masked`, que reemplaza las palabras soeces con asteriscos; `Removed`, que quita todas las palabras soeces del resultado; o `Tags`, que agrega etiquetas de "palabras soeces". El valor predeterminado es `Masked`. |
| `PunctuationMode` | Especifica cómo controlar la puntuación en los resultados del reconocimiento. Los valores aceptados son: `None`, que deshabilita la puntuación; `Dictated`, que implica puntuación explícita; `Automatic`, que permite que el descodificador se ocupe de la puntuación; o `DictatedAndAutomatic`, que implica signos de puntuación dictados o puntuación automática. |
| `AddWordLevelTimestamps` | Especifica si las marcas de tiempo de nivel de palabra se deben agregar a la salida. Los valores aceptados son `true`, que permite las marcas de tiempo de nivel de palabra, y `false` (el valor predeterminado) que las deshabilita. |
| `AddSentiment` | Especifica que la opinión se debe agregar a la expresión. Los valores aceptados son `true`, que permite la opinión por expresión y `false` (el valor predeterminado) que la deshabilita. |
| `AddDiarization` | Especifica que el análisis de la diarización se debe llevar a cabo en la entrada que se espera sea un canal mono que contenga dos voces. Los valores aceptados son `true`, que permite la diarización, y `false` (el valor predeterminado) que la deshabilita. También requiere que `AddWordLevelTimestamps` se establezca en true.|
|`TranscriptionResultsContainerUrl`|URL opcional con [SAS de servicio](../../storage/common/storage-sas-overview.md) en un contenedor grabable de Azure. El resultado se almacenará en este contenedor.

### <a name="storage"></a>Storage

La transcripción de lotes admite [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) para leer el audio y escribir las transcripciones en el almacenamiento.

## <a name="the-batch-transcription-result"></a>El resultado de la transcripción por lotes

Para el audio de entrada mono, se crea un archivo de resultado de transcripción. En el audio de entrada estéreo, se crean dos archivos de resultados de transcripción. Cada una tiene esta estructura:

```json
{
  "AudioFileResults":[ 
    {
      "AudioFileName": "Channel.0.wav | Channel.1.wav"      'maximum of 2 channels supported'
      "AudioFileUrl": null                                  'always null'
      "AudioLengthInSeconds": number                        'Real number. Two decimal places'
      "CombinedResults": [
        {
          "ChannelNumber": null                             'always null'
          "Lexical": string
          "ITN": string
          "MaskedITN": string
          "Display": string
        }
      ]
      SegmentResults:[                                      'for each individual segment'
        {
          "RecognitionStatus": "Success | Failure"
          "ChannelNumber": null
          "SpeakerId": null | "1 | 2"                       'null if no diarization
                                                             or stereo input file, the
                                                             speakerId as a string if
                                                             diarization requested for
                                                             mono audio file'
          "Offset": number                                  'time in ticks (1 tick is 100 nanosec)'
          "Duration": number                                'time in ticks (1 tick is 100 nanosec)'
          "OffsetInSeconds" : number                        'Real number. Two decimal places'
          "DurationInSeconds" : number                      'Real number. Two decimal places'
          "NBest": [
            {
              "Confidence": number                          'between 0 and 1'
              "Lexical": string
              "ITN": string
              "MaskedITN": string
              "Display": string
              "Sentiment":
                {                                           'this is omitted if sentiment is
                                                             not requested'
                  "Negative": number                        'between 0 and 1'
                  "Neutral": number                         'between 0 and 1'
                  "Positive": number                        'between 0 and 1'
                }
              "Words": [
                {
                  "Word": string
                  "Offset": number                          'time in ticks (1 tick is 100 nanosec)'
                  "Duration": number                        'time in ticks (1 tick is 100 nanosec)'
                  "OffsetInSeconds": number                 'Real number. Two decimal places'
                  "DurationInSeconds": number               'Real number. Two decimal places'
                  "Confidence": number                      'between 0 and 1'
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

El resultado contiene estas formas:

|Form|Contenido|
|-|-|
|`Lexical`|Las palabras reales reconocidas.
|`ITN`|El formato de normalización inversa de texto del texto reconocido. Se aplican las abreviaturas ("doctor Pérez" a "Dr Pérez"), los números de teléfono y otras transformaciones.
|`MaskedITN`|El formato ITN con enmascaramiento de palabras soeces aplicado.
|`Display`|El formato mostrado del texto reconocido. Esto incluye el uso de mayúsculas y minúsculas y la puntuación que se agrega.

## <a name="speaker-separation-diarization"></a>Separación de altavoces (diarización)

La diarización es el proceso de separación de los altavoces en una parte del audio. Nuestra canalización de Batch admite la diarización y es capaz de reconocer dos altavoces en las grabaciones de un canal mono. La característica no está disponible en las grabaciones estéreo.

Toda la salida de transcripción contiene un elemento `SpeakerId`. Si no se utiliza la diarización, se mostrará `"SpeakerId": null` en la salida JSON. Para la diarización, se admiten dos voces, por lo que los oradores se identificarán como `"1"` o `"2"`.

Para solicitar la diarización, basta con agregar el parámetro correspondiente en la solicitud HTTP, como se muestra a continuación.

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

Las marcas de tiempo de nivel de palabra también tendrían que estar "activadas" como indican los parámetros de la solicitud anterior.

## <a name="sentiment-analysis"></a>análisis de opiniones

La característica de opiniones calcula la opinión expresada en el audio. La opinión se expresa mediante un valor comprendido entre 0 y 1 para las opiniones `Negative`, `Neutral` y `Positive`. Por ejemplo, el análisis de sentimiento se puede usar en los escenarios de centros de llamadas:

- Obtener información sobre la satisfacción del cliente
- Obtener información sobre el rendimiento de los agentes (equipo que atiende las llamadas)
- Identificar el punto exacto en el tiempo de cuándo una llamada giró hacia una dirección negativa
- Identificar lo que salió bien al convertir una llamada negativa en positiva
- Identificar lo que les gusta a los clientes y lo que no les gusta de un producto o servicio

La opinión se puntúa por segmento de audio basado en la forma léxica. Todo el texto dentro de ese segmento de audio se utiliza para calcular la opinión. No se calcula ninguna opinión agregada en toda la transcripción.

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

## <a name="best-practices"></a>Procedimientos recomendados

El servicio transcripción puede controlar un gran número de transcripciones enviadas. Puede consultar el estado de las transcripciones a través de un elemento `GET` en el [método de transcripciones](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/GetTranscriptions). Consiga que la información devuelta se mantenga en un tamaño razonable especificando el parámetro `take` (unos cientos). [Elimine las transcripciones](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/DeleteTranscription) periódicamente del servicio una vez que recuperen los resultados. Esto garantizará respuestas rápidas en las llamadas de administración de transcripciones.

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

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
