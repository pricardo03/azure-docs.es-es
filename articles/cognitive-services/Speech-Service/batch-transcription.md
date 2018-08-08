---
title: Transcription API de Azure Batch
description: Ejemplos
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.technology: Speech to Text
ms.topic: article
ms.date: 04/26/2018
ms.author: panosper
ms.openlocfilehash: f21973855ceb3a257627c147490ac50465c54020
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281946"
---
# <a name="batch-transcription"></a>Transcripción de Azure Batch

La transcripción de Azure Batch es ideal para los casos de uso con grandes cantidades de audio. Permite al desarrollador apuntar a archivos de audio y obtener transcripciones de ellos en modo asincrónico.

## <a name="batch-transcription-api"></a>API de transcripciones de Azure Batch

La API de transcripciones de Azure Batch hace posible el escenario anterior. Ofrece la transcripción asíncrona de la conversión de voz en texto junto con otras características.

> [!NOTE]
> La API de transcripción de Azure Batch es ideal para centros de llamadas que suelen acumular miles de horas de audio. La filosofía "Dispara y olvida" de la API facilita la transcripción de grandes volúmenes de grabaciones de audio.

### <a name="supported-formats"></a>Formatos compatibles

La API de transcripción de Azure Batch pretende convertirse en el elemento de facto para todos los escenarios relacionados con centros de llamadas sin conexión y ofrecer compatibilidad con todos los formatos relacionados. Formatos compatibles actualmente:

NOMBRE| Canal  |
----|----------|
mp3 |   Mono   |   
mp3 |  Estéreo  | 
wav |   Mono   |
wav |  Estéreo  |

Si se trata de secuencias de audio estéreo, la transcripción de Azure Batch dividirá el canal izquierdo y derecho durante la transcripción. Los dos archivos JSON con el resultado se crean desde un único canal. Las marcas de tiempo por expresión permiten al desarrollador crear una transcripción final ordenada. En el siguiente ejemplo de JSON se muestra la salida de un canal.

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
> La API de transcripción de Azure Batch usa un servicio REST para solicitar transcripciones, su estado y los resultados asociados. La API puede usarse desde cualquier lenguaje. En la siguiente sección se describe su uso.

## <a name="authorization-token"></a>Token de autorización

Al igual que con todas las características de Unified Speech Service, el usuario necesita crear una clave de suscripción en [Azure Portal](https://portal.azure.com). Además, es necesario adquirir una clave de API desde el portal de voz. Estos son los pasos para generar una clave de API:

1. Inicie sesión en https://customspeech.ai.

2. Haga clic en Suscripciones.

3. Haga clic en la opción `Generate API Key`.

    ![Vista de carga](media/stt/Subscriptions.jpg)

4. Copie y pegue la clave en el código de cliente en el ejemplo siguiente.

> [!NOTE]
> Si pretende usar un modelo personalizado, también necesitará el identificador de dicho modelo. Tenga en cuenta que no se trata del identificador de implementación o del punto de conexión que se encuentra en la vista Detalles de punto de conexión, sino del identificador del modelo que puede recuperar si hace clic en los detalles de dicho modelo.

## <a name="sample-code"></a>Código de ejemplo

El uso de la API es bastante sencillo. El código de ejemplo a continuación se debe personalizar con una clave de suscripción y una clave de API que, a su vez, permite que el desarrollador obtenga un token de portador, tal como muestra el fragmento de código siguiente:

```cs
    public static async Task<CrisClient> CreateApiV1ClientAsync(string username, string key, string hostName, int port)
        {
            var client = new HttpClient();
            client.Timeout = TimeSpan.FromMinutes(25);
            client.BaseAddress = new UriBuilder(Uri.UriSchemeHttps, hostName, port).Uri;

            var tokenProviderPath = "/oauth/ctoken";
            var clientToken = await CreateClientTokenAsync(client, hostName, port, tokenProviderPath, username, key).ConfigureAwait(false);
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", clientToken.AccessToken);

            return new CrisClient(client);
        }
```

Una vez que se obtiene el token, el desarrollador debe especificar el URI de SAS que apunta al archivo de audio que requiere transcripción. El resto del código simplemente procesa una iteración en el estado y muestra los resultados.

```cs
   static async Task TranscribeAsync()
        { 
            // Creating a Batch transcription API Client
            var client = 
                await CrisClient.CreateApiV1ClientAsync(
                    "<your msa>", // MSA email
                    "<your api key>", // API key
                    "stt.speech.microsoft.com",
                    443).ConfigureAwait(false);
            
            var newLocation = 
                await client.PostTranscriptionAsync(
                    "<selected locale i.e. en-us>", // Locale 
                    "<your subscription key>", // Subscription Key
                    new Uri("<SAS URI to your file>")).ConfigureAwait(false);

            var transcription = await client.GetTranscriptionAsync(newLocation).ConfigureAwait(false);

            while (true)
            {
                transcription = await client.GetTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                if (transcription.Status == "Failed" || transcription.Status == "Succeeded")
                {
                    Console.WriteLine("Transcription complete!");

                    if (transcription.Status == "Succeeded")
                    {
                        var resultsUri = transcription.ResultsUrls["channel_0"];

                        WebClient webClient = new WebClient();

                        var filename = Path.GetTempFileName();
                        webClient.DownloadFile(resultsUri, filename);

                        var results = File.ReadAllText(filename);
                        Console.WriteLine(results);
                    }

                    await client.DeleteTranscriptionAsync(transcription.Id).ConfigureAwait(false);

                    break;
                }
                else
                {
                    Console.WriteLine("Transcription status: " + transcription.Status);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }

            Console.ReadLine();
        }
```

> [!NOTE]
> La clave de suscripción mencionada en el fragmento de código anterior es la clave del recurso Speech (versión preliminar) creado en Azure Portal. Las claves obtenidas del recurso Custom Speech Service no servirán.


Observe la configuración asincrónica para publicar audio y recibir el estado de la transcripción. El cliente creado es un cliente HTTP de .NET. Hay un método `PostTranscriptions` para enviar los detalles del archivo de audio y un método `GetTranscriptions` para recibir los resultados. `PostTranscriptions` devuelve un identificador, y el método `GetTranscriptions` usa este identificador para crear un identificador a fin de obtener el estado de la transcripción.

El código de ejemplo actual no especifica ningún modelo personalizado. El servicio usará los modelos de línea de base para transcribir los archivos. Si el usuario desea especificar los modelos, uno puede pasar el mismo método de modelIDs para el modelo acústico y de lenguaje. 

Si no se desea utilizar la línea de base, es necesario pasar los identificadores de los modelos para los modelos acústico y de lenguaje.

> [!NOTE]
> Para la transcripción de línea de base, el usuario no tiene que declarar los puntos de conexión de los modelos de línea de base. Si el usuario desea usar los modelos personalizados, debería proporcionar los identificadores de los puntos de conexión como en el [ejemplo](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI). Si el usuario desea utilizar una línea de base acústica con un modelo de lenguaje de línea de base, tendría que declarar el identificador del punto de conexión del modelo personalizado. Internamente, nuestro sistema determinará el modelo de línea de base del asociado (ya sea el acústico o el de lenguaje) y lo usará para satisfacer la solicitud de transcripción.

### <a name="supported-storage"></a>Almacenamiento compatible

Actualmente, el único almacenamiento compatible es Azure Blob.

## <a name="downloading-the-sample"></a>Descarga del ejemplo

El ejemplo mostrado aquí está en [GitHub](https://github.com/PanosPeriorellis/Speech_Service-BatchTranscriptionAPI).

> [!NOTE]
> Normalmente, una transcripción de audio requiere un intervalo de tiempo igual a la duración del archivo de audio más una sobrecarga de 2 a 3 minutos.

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
