---
title: Uso de Transcription API de Azure Batch
titlesuffix: Azure Cognitive Services
description: Ejemplos de transcripción de grandes volúmenes de contenido de audio.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: cd57e9a90b07447392fbff48017bb29f002ad29e
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035958"
---
# <a name="use-batch-transcription"></a>Uso de la transcripción de lotes

La transcripción de Batch resulta muy conveniente si tiene grandes cantidades de audio en almacenamiento. Mediante la API REST, puede apuntar a archivos de audio por URI de firma de acceso compartido (SAS) y recibir las transcripciones de forma asincrónica.

## <a name="the-batch-transcription-api"></a>Transcription API de Azure Batch

Transcription API de Batch ofrece la transcripción asincrónica de voz a texto, además de otras características. Es una API REST que expone métodos para:

1. Crear solicitudes de procesamiento por lotes
1. Consultar el estado 
1. Descargar transcripciones

> [!NOTE]
> Esta API resulta muy adecuada para centros de llamadas que suelen acumular miles de horas de audio. Una filosofía "dispara y olvida" guía la API, lo que permite la transcripción fácil de grandes volúmenes de registros de audio.

### <a name="supported-formats"></a>Formatos compatibles

Transcription API de Batch admite los siguientes formatos:

NOMBRE| Canal  |
----|----------|
mp3 |   Mono   |   
mp3 |  Estéreo  | 
wav |   Mono   |
wav |  Estéreo  |
Opus|   Mono   |
Opus|  Estéreo  |

Si se trata de secuencias de audio estéreo, la transcripción de lotes divide el canal izquierdo y derecho durante la transcripción. Los dos archivos JSON con el resultado se crean desde un único canal. Las marcas de tiempo por expresión permiten al desarrollador crear una transcripción final ordenada. La salida de un canal, incluidas las propiedades para configurar el filtro de obscenidad y el modelo de puntuación, se muestra en el siguiente ejemplo de JSON:

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
