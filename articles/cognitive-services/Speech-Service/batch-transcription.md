---
title: 'Cómo usar la transcripción de lotes: servicios de Voz'
titlesuffix: Azure Cognitive Services
description: La transcripción de lotes es ideal si desea transcribir una gran cantidad de audio en el almacenamiento, como los blobs de Azure. Mediante la API REST dedicada, puede apuntar a archivos de audio con un identificador URI de firma de acceso compartido (SAS) y recibir las transcripciones de forma asincrónica.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: b4e7c11a6077104e874d67b75f5d00e8f481f739
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53086936"
---
# <a name="why-use-batch-transcription"></a>¿Por qué usar la transcripción de lotes?

La transcripción de lotes es ideal si desea transcribir una gran cantidad de audio en el almacenamiento, como los blobs de Azure. Mediante la API REST dedicada, puede apuntar a archivos de audio con un identificador URI de firma de acceso compartido (SAS) y recibir las transcripciones de forma asincrónica.

>[!NOTE]
> Se requiere una suscripción estándar (S0) para los servicios de Voz para usar la transcripción de lotes. Las claves de suscripción gratuita (F0) no funcionarán. Para más información, consulte [Precios y límites](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/).

## <a name="the-batch-transcription-api"></a>Transcription API de Azure Batch

Transcription API de Batch ofrece la transcripción asincrónica de voz a texto, además de otras características. Es una API REST que expone métodos para:

1. Crear solicitudes de procesamiento por lotes
1. Consultar el estado
1. Descargar transcripciones

> [!NOTE]
> Esta API resulta muy adecuada para centros de llamadas que suelen acumular miles de horas de audio. Una filosofía "dispara y olvida" guía la API, lo que permite la transcripción fácil de grandes volúmenes de registros de audio.

### <a name="supported-formats"></a>Formatos compatibles

Transcription API de Batch admite los siguientes formatos:

| Formato | Códec | Bitrate | Velocidad de muestreo |
|--------|-------|---------|-------------|
| WAV | PCM | 16 bits | 8 o 16 kHz, mono, estéreo |
| MP3 | PCM | 16 bits | 8 o 16 kHz, mono, estéreo |
| OGG | OPUS | 16 bits | 8 o 16 kHz, mono, estéreo |

> [!NOTE]
> Batch Transcription API requiere una clave de S0 (nivel de pago). No funciona con una clave gratuita (f0).

Si se trata de secuencias de audio estéreo, Batch Transcription API divide los canales izquierdo y derecho durante la transcripción. Los dos archivos JSON con el resultado se crean desde un único canal. Las marcas de tiempo por expresión permiten al desarrollador crear una transcripción final ordenada. En el siguiente ejemplo de JSON se muestra la salida de un canal, incluidas las propiedades para configurar el filtro de obscenidad y el modelo de puntuación.

```json
{
  "recordingsUrl": "https://contoso.com/mystoragelocation",
  "models": [],
  "locale": "en-US",
  "name": "Transcription using locale en-US",
  "description": "An optional description of the transcription.",
  "properties": {
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic"
  },
```

> [!NOTE]
> Transcription API de Batch usa un servicio REST para solicitar transcripciones, su estado y los resultados asociados. Puede usar la API con cualquier lenguaje. En la siguiente sección se describe cómo se usa la API.

### <a name="query-parameters"></a>Parámetros de consulta

Estos parámetros podrían incluirse en la cadena de consulta de la solicitud de REST.

| Parámetro | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| `ProfanityFilterMode` | Especifica cómo controlar las palabras soeces en los resultados del reconocimiento. Los valores aceptados son `none`, que deshabilita el filtrado de palabras soeces; `masked`, que reemplaza las palabras soeces con asteriscos; `removed`, que quita todas las palabras soeces del resultado; o `tags`, que agrega etiquetas de "palabras soeces". La configuración predeterminada es `masked`. | Opcional |
| `PunctuationMode` | Especifica cómo controlar la puntuación en los resultados del reconocimiento. Los valores aceptados son: `none`, que deshabilita la puntuación; `dictated`, que implica puntuación explícita; `automatic`, que permite que el descodificador se ocupe de la puntuación; o `dictatedandautomatic`, que implica signos de puntuación dictados o puntuación automática. | Opcional |


## <a name="authorization-token"></a>Token de autorización

Como sucede con todas las características del servicio Voz, puede crear una clave de suscripción en [Azure Portal](https://portal.azure.com) siguiendo nuestra [guía de inicio](get-started.md). Si va a obtener transcripciones de nuestros modelos de base de referencia, todo lo que necesita hacer es crear una clave.

Si va a personalizar y usar un modelo personalizado, agregue la clave de suscripción al portal de voz personalizado mediante los pasos siguientes:

1. Inicie sesión en [Custom Speech](https://customspeech.ai).

2. En la esquina superior derecha, seleccione **Suscripciones**.

3. Seleccione **Connect Existing Subscription** (Conectar la suscripción existente).

4. En la ventana desplegable, agregue la clave de suscripción y un alias.

    ![Ventana Agregar una suscripción](media/stt/Subscriptions.jpg)

5. Copie y pegue esa clave en el código de cliente del ejemplo siguiente.

> [!NOTE]
> Si pretende usar un modelo personalizado, también necesitará el identificador de dicho modelo. Este no es el identificador del punto de conexión que encontrará en la vista de detalles del punto de conexión. Es el identificador de modelo que se puede recuperar cuando se seleccionan los detalles de ese modelo.

## <a name="sample-code"></a>Código de ejemplo

Personalice el siguiente código de ejemplo con una clave de suscripción y una clave de API. Esta acción le permite obtener un token de portador.

```cs
     public static CrisClient CreateApiV2Client(string key, string hostName, int port)

        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

            return new CrisClient(client);
        }
```

Después de obtener el token, especifique el URI de SAS que apunte al archivo de audio que requiere la transcripción. El resto del código procesa una iteración en el estado y muestra los resultados. En primer lugar, configure la clave, la región, los modelos de uso y la asociación de seguridad, tal y como se muestra en el siguiente fragmento de código. A continuación, se crea una instancia del cliente y la solicitud POST.

```cs
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;

            // SAS URI
            private const string RecordingsBlobUri = "SAS URI pointing to the file in Azure Blob Storage";

            // adapted model Ids
            private static Guid AdaptedAcousticId = new Guid("guid of the acoustic adaptation model");
            private static Guid AdaptedLanguageId = new Guid("guid of the language model");

            // Creating a Batch Transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);

            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);
```

Ahora que ha realizado la solicitud, puede consultar y descargar los resultados de la transcripción, tal como se muestra en el siguiente fragmento de código:

```cs

            // get all transcriptions for the user
            transcriptions = await client.GetTranscriptionAsync().ConfigureAwait(false);

            // for each transcription in the list we check the status
            foreach (var transcription in transcriptions)
            {
                switch(transcription.Status)
                {
                    case "Failed":
                    case "Succeeded":

                            // we check to see if it was one of the transcriptions we created from this client.
                        if (!createdTranscriptions.Contains(transcription.Id))
                        {
                            // not created from here, continue
                            continue;
                        }

                        completed++;

                        // if the transcription was successful, check the results
                        if (transcription.Status == "Succeeded")
                        {
                            var resultsUri = transcription.ResultsUrls["channel_0"];
                            WebClient webClient = new WebClient();
                            var filename = Path.GetTempFileName();
                            webClient.DownloadFile(resultsUri, filename);
                            var results = File.ReadAllText(filename);
                            Console.WriteLine("Transcription succeeded. Results: ");
                            Console.WriteLine(results);
                        }

                    break;
                    case "Running":
                    running++;
                     break;
                    case "NotStarted":
                    notStarted++;
                    break;

                    }
                }
            }
        }
```

Para obtener detalles completos acerca de las llamadas anteriores, consulte nuestro [documento de swagger](https://westus.cris.ai/swagger/ui/index). Para obtener el ejemplo completo mostrado aquí, vaya a [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> En el código anterior, la clave de suscripción es del recurso Voz que crea en Azure Portal. Las claves obtenidas del recurso Custom Speech Service no sirven.

Tome nota de la configuración asincrónica para publicar audio y recibir el estado de la transcripción. El cliente que crea es un cliente HTTP de .NET. Hay un método `PostTranscriptions` para enviar los detalles del archivo de audio y un método `GetTranscriptions` para recibir los resultados. `PostTranscriptions` devuelve un identificador y `GetTranscriptions` lo usa para crear un identificador y obtener el estado de la transcripción.

El ejemplo de código actual no especifica un modelo personalizado. El servicio usa los modelos base de referencia para transcribir los archivos. Para especificar los modelos, puede pasar los mismos identificadores de modelo para el modelo acústico y de lenguaje.

Si no quiere usar los modelos base de referencia, pase los identificadores de modelo tanto para los modelos acústico como de lenguaje.

> [!NOTE]
> Para la transcripciones de base de referencia, no tiene que declarar los puntos de conexión de los modelos base de referencia. Si quiere usar modelos personalizados, proporcione sus identificadores de puntos de conexión como [ejemplo](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Si quiere usar una base de referencia acústica con un modelo de lenguaje base de referencia, tiene que declarar solo el identificador de punto de conexión del modelo personalizado. Microsoft detecta el modelo base de referencia asociado &mdash;ya sea acústico o de lenguaje&mdash;, y lo usa para satisfacer la solicitud de transcripción.

### <a name="supported-storage"></a>Almacenamiento compatible

Actualmente, el único almacenamiento admitido es Azure Blob Storage.

## <a name="download-the-sample"></a>Descarga del ejemplo

Puede encontrar el ejemplo de este artículo en [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Una transcripción de audio normalmente requiere un intervalo de tiempo igual a la duración del archivo de audio, más una sobrecarga de dos a tres minutos.

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
