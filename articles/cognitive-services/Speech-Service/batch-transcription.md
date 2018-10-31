---
title: API de transcripción de Azure Batch
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
ms.openlocfilehash: e7523bf97d6252422ebb853b818453c935640f50
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648809"
---
# <a name="batch-transcription"></a>Transcripción de Azure Batch

La transcripción de Batch resulta muy conveniente si tiene grandes cantidades de audio en almacenamiento. Con nuestra API de REST, puede apuntar a archivos de audio por URI de SAS y recibir transcripciones de forma asincrónica.

## <a name="batch-transcription-api"></a>API de transcripciones de Azure Batch

La API de transcripción de Batch ofrece la transcripción asincrónica de voz a texto junto con otras características. Es una API de REST que expone métodos para:

1. Crear solicitudes de procesamiento por lotes

2. Consultar el estado 

3. Descargar transcripciones

> [!NOTE]
> Esta API resulta muy adecuada para centros de llamadas que suelen acumular miles de horas de audio. Una filosofía "dispara y olvida" guía la API, lo que permite la transcripción fácil de grandes volúmenes de registros de audio.

### <a name="supported-formats"></a>Formatos compatibles

La API de transcripción de Batch admite los siguientes formatos:

NOMBRE| Canal  |
----|----------|
mp3 |   Mono   |   
mp3 |  Estéreo  | 
wav |   Mono   |
wav |  Estéreo  |

Si se trata de secuencias de audio estéreo, la transcripción de Batch divide el canal izquierdo y derecho durante la transcripción. Los dos archivos JSON con el resultado se crean desde un único canal. Las marcas de tiempo por expresión permiten al desarrollador crear una transcripción final ordenada. En el siguiente ejemplo de JSON se muestra la salida de un canal.

```json
       {
        "recordingsUrl": "https://mystorage.blob.core.windows.net/cris-e2e-datasets/TranscriptionsDataset/small_sentence.wav?st=2018-04-19T15:56:00Z&se=2040-04-21T15:56:00Z&sp=rl&sv=2017-04-17&sr=b&sig=DtvXbMYquDWQ2OkhAenGuyZI%2BYgaa3cyvdQoHKIBGdQ%3D",
        "resultsUrls": {
            "channel_0": "https://mystorage.blob.core.windows.net/bestor-87a0286f-304c-4636-b6bd-b3a96166df28/TranscriptionData/24265e4c-e459-4384-b572-5e3e7795221f?sv=2017-04-17&sr=b&sig=IY2qd%2Fkgtz2PwRe2C88BphH4Hv%2F1VCb1UVJ33xsw%2BEY%3D&se=2018-04-23T14:48:24Z&sp=r"
        },
        "statusMessage": "None.",
        "id": "0bb95356-ff06-469d-acc7-81f9144a269a",
        "createdDateTime": "2018-04-20T14:11:57.167",
        "lastActionDateTime": "2018-04-20T14:12:54.643",
        "status": "Succeeded",
        "locale": "en-US"
    },
```

> [!NOTE]
> La API de transcripción de Azure Batch usa un servicio REST para solicitar transcripciones, su estado y los resultados asociados. Puede usar la API con cualquier lenguaje. En la siguiente sección se describe su uso.

## <a name="authorization-token"></a>Token de autorización

Como sucede con todas las características del servicio Voz, puede crear una clave de suscripción en [Azure Portal](https://portal.azure.com) siguiendo nuestra [guía de inicio](get-started.md). Si va a obtener transcripciones de nuestros modelos de línea base, esto es todo lo que necesita hacer. 

Si va a personalizar y usar un modelo personalizado, deberá agregar entonces esta clave de suscripción al portal de voz personalizado de la manera siguiente:

1. Inicie sesión en [Custom Speech](https://customspeech.ai).

2. Seleccione **Suscripciones**.

3. Seleccione **Connect Existing Subscription** (Conectar la suscripción existente).

4. Agregue la clave de suscripción y un alias en la vista que aparece.

    ![Captura de pantalla de la página de suscripciones de Custom Speech](media/stt/Subscriptions.jpg)

5. Copie y pegue esa clave en el código de cliente del ejemplo siguiente.

> [!NOTE]
> Si pretende usar un modelo personalizado, también necesitará el identificador de dicho modelo. Tenga en cuenta que este no es el identificador del punto de conexión que encontrará en la vista de detalles del punto de conexión. Es el identificador de modelo que se puede recuperar cuando se seleccionan los detalles de ese modelo.

## <a name="sample-code"></a>Código de ejemplo

Personalice el siguiente código de ejemplo con una clave de suscripción y una clave de API. Esto le permite obtener un token de portador.

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

Después de obtener el token, debe especificar el URI de SAS que apunta al archivo de audio que requiere transcripción. El resto del código procesa una iteración en el estado y muestra los resultados. Inicialmente, uno podría configurar la clave, la región, los modelos para usar y la asociación de seguridad. como se muestra en el siguiente fragmento de código. Esto va seguido de la creación de instancias del cliente y la solicitud POST. 

```cs
            private const string SubscriptionKey = "<your Speech subscription key>";
            private const string HostName = "westus.cris.ai";
            private const int Port = 443;
    
            // SAS URI 
            private const string RecordingsBlobUri = "some SAS URI";

            // adapted model Ids
            private static Guid AdaptedAcousticId = new Guid("some guid");
            private static Guid AdaptedLanguageId = new Guid("some guid");

            // Creating a Batch transcription API Client
            var client = CrisClient.CreateApiV2Client(SubscriptionKey, HostName, Port);
            
            var transcriptionLocation = await client.PostTranscriptionAsync(Name, Description, Locale, new Uri(RecordingsBlobUri), new[] { AdaptedAcousticId, AdaptedLanguageId }).ConfigureAwait(false);
```

Ahora que se ha hecho la solicitud, el usuario puede consultar y descargar los resultados de transcripción a medida que aparece el fragmento de código.

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
                            // not creted form here, continue
                            continue;
                        }
                            
                        completed++;
                            
                        // if the transcription was successfull, check the results
                        if (transcription.Status == "Succeeded")
                        {
                            var resultsUri = transcription.ResultsUrls["channel_0"];
                            WebClient webClient = new WebClient();
                            var filename = Path.GetTempFileName();
                            webClient.DownloadFile(resultsUri, filename);
                            var results = File.ReadAllText(filename);
                            Console.WriteLine("Transcription succedded. Results: ");
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

Nuestro [documento de Swagger](https://westus.cris.ai/swagger/ui/index) proporciona detalles completos acerca de las llamadas anteriores. El ejemplo completo mostrado aquí está en [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> En el código anterior, la clave de suscripción es del recurso Speech que crea en Azure Portal. Las claves obtenidas del recurso Custom Speech Service no sirven.

Observe la configuración asincrónica para publicar audio y recibir el estado de la transcripción. El cliente creado es un cliente HTTP de .NET. Hay un método `PostTranscriptions` para enviar los detalles del archivo de audio y un método `GetTranscriptions` para recibir los resultados. `PostTranscriptions` devuelve un identificador, y `GetTranscriptions` usa este identificador para crear un identificador para obtener el estado de la transcripción.

El código de ejemplo actual no especifica ningún modelo personalizado. El servicio usa los modelos base de referencia para transcribir los archivos. Para especificar los modelos, puede pasar los mismos identificadores de modelo para el modelo acústico y de lenguaje. 

Si no quiere usar los modelos base de referencia, debe pasar los identificadores de modelo de los modelos acústico y de lenguaje.

> [!NOTE]
> Para la transcripción base de referencia, el usuario no tiene que declarar los puntos de conexión de los modelos base. Si quiere usar modelos personalizados, proporcione sus identificadores de puntos de conexión como [ejemplo](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Si quiere usar una base de referencia acústica con un modelo de lenguaje base de referencia, solo tiene que declarar el identificador de punto de conexión del modelo personalizado. Microsoft detecta el modelo base de referencia asociado (acústico o de lenguaje) y lo usa para satisfacer la solicitud de transcripción.

### <a name="supported-storage"></a>Almacenamiento compatible

Actualmente, el único almacenamiento admitido es Azure Blob Storage.

## <a name="downloading-the-sample"></a>Descarga del ejemplo

El ejemplo mostrado aquí está en [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Normalmente, una transcripción de audio requiere un intervalo de tiempo igual a la duración del archivo de audio más una sobrecarga de 2 a 3 minutos.

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
