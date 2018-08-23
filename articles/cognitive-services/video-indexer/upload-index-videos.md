---
title: Carga e indexación de vídeos con Azure Video Indexer | Microsoft Docs
description: En este tema se muestra cómo usar las API para cargar e indexar los vídeos con Azure Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 08/17/2018
ms.author: juliako
ms.openlocfilehash: 8a9409c46cac8397bc449c586374729a4d864036
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "41929923"
---
# <a name="upload-and-index-your-videos"></a>Carga e indexación de los vídeos  

En este artículo se muestra cómo usar [Upload Video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) API para cargar e indexar los vídeos con Azure Video Indexer. También se explican algunos de los parámetros que puede establecer en la API para cambiar el proceso y la salida de la API.

> [!Note]
> Al crear una cuenta de Video Indexer, puede elegir una cuenta de evaluación gratuita (donde obtendrá un número determinado de minutos gratuitos de indexación) o una opción de pago (donde no está limitado por la cuota). <br/>Con la versión de evaluación gratuita, Video Indexer proporciona hasta 600 horas de indexación gratuita a los usuarios de sitios web y hasta 2400 horas de indexación gratuita a los usuarios de API. <br/>Con la opción de pago, se crea una cuenta de Video Indexer que está [conectada a su suscripción de Azure y a una cuenta de Azure Media Services](connect-to-azure.md). Se paga por minutos de indexación y por los cargos relacionados con la cuenta de Media Services. 

## <a name="configurations-and-params"></a>Configuración y parámetros

En esta sección se describen algunos de los parámetros opcionales y cuándo es conveniente establecerlos.

### <a name="externalid"></a>externalId 

Este parámetro permite especificar un identificador que se asociará con el vídeo. El identificador puede aplicarse a la integración del sistema de "Administración de contenido de vídeo" (VCM) externa. Los vídeos del portal de Video Indexer se pueden buscar con el identificador externo especificado.

### <a name="indexingpreset"></a>indexingPreset

Utilice este parámetro si hay grabaciones sin formato o externas que contengan ruido de fondo. Este parámetro se utiliza para configurar el proceso de indexación. Puede especificar los siguientes valores:

- `Default`: indexar y extraer información detallada con audio y vídeo
- `AudioOnly`: indexar y extraer información detallada solo con audio (e ignorar el vídeo)
- `DefaultWithNoiseReduction`: indexar y extraer información detallada de audio y vídeo, mientras se aplican algoritmos de reducción de ruido en la transmisión de audio

El precio depende de la opción de indexación seleccionada.  

### <a name="callbackurl"></a>callbackUrl

Una dirección URL de POST para notificar cuando se ha completado la indexación. Video Indexer agrega dos parámetros de cadena de consulta: id y state. Por ejemplo, si la dirección URL de devolución de llamada es "https://test.com/notifyme?projectName=MyProject", la notificación se enviará con parámetros adicionales a "https://test.com/notifyme?projectName=MyProject&id=1234abcd&state=Processed".

También puede agregar más parámetros a la dirección URL antes de enviar el POST a Video Indexer y estos parámetros se incluirán en la devolución de llamada. Después, en el código puede analizar la cadena de consulta y recuperar todos los parámetros especificados en la cadena de consulta (datos que había anexado originalmente a la dirección URL más la información suministrada por Video Indexer). 

### <a name="streamingpreset"></a>streamingPreset

Una vez cargado el vídeo, Video Indexer, codifica opcionalmente el vídeo. Después, continúa con la indexación y el análisis del vídeo. Cuando Video Indexer ha terminado de analizar, recibirá una notificación con el identificador de vídeo.  

Cuando se usa [Upload Video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?) o [Re-Index Video](https://api-portal.videoindexer.ai/docs/services/operations/operations/Re-index-video?) API, uno de los parámetros opcionales es `streamingPreset`. Si establece `streamingPreset` en `Default`, `SingleBitrate` o en `AdaptiveBitrate`, se desencadenará el proceso de codificación. Una vez realizados los trabajos de indexación y codificación, el vídeo se publica para que también pueda transmitirlo. El punto de conexión de streaming desde el que va a transmitir el vídeo debe estar en estado **Running** (En ejecución).

Para ejecutar los trabajos de indexación y codificación, la cuenta de [Azure Media Services conectada a la cuenta de Video Indexer](connect-to-azure.md) requiere unidades reservadas. Para más información, consulte [Escalado del procesamiento de elementos multimedia](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview). Como son trabajos de proceso intensivo, se recomienda encarecidamente el tipo de unidad S3. El número de unidades reservadas define el número máximo de trabajos que se pueden ejecutar en paralelo. La recomendación de referencia son 10 unidades reservadas S3. 

Si solo desea indexar el vídeo pero no codificarlo, establezca `streamingPreset` en `NoStreaming`.

## <a name="code-sample"></a>Código de ejemplo

El siguiente fragmento de código de C# muestra el uso de todas las API de Video Indexer juntas.

```csharp
public async Task Sample()
{
    var apiUrl = "https://api.videoindexer.ai";
    var location = "westus2";
    var apiKey = "...";

    System.Net.ServicePointManager.SecurityProtocol =
        System.Net.ServicePointManager.SecurityProtocol | System.Net.SecurityProtocolType.Tls12;

    // create the http client
    var handler = new HttpClientHandler();
    handler.AllowAutoRedirect = false;
    var client = new HttpClient(handler);
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);

    // obtain account information and access token
    string queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"generateAccessTokens", "true"},
            {"allowEdit", "true"},
        });
    HttpResponseMessage result = await client.GetAsync($"{apiUrl}/auth/trial/Accounts?{queryParams}");
    var json = await result.Content.ReadAsStringAsync();
    var accounts = JsonConvert.DeserializeObject<AccountContractSlim[]>(json);
    // take the relevant account, here we simply take the first
    var accountInfo = accounts.First();

    // we will use the access token from here on, no need for the apim key
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // upload a video
    var content = new MultipartFormDataContent();
    Debug.WriteLine("Uploading...");
    // get the video from URL
    var videoUrl = "VIDEO_URL"; // replace with the video URL

    // as an alternative to specifying video URL, you can upload a file.
    // remove the videoUrl parameter from the query params below and add the following lines:
    //FileStream video =File.OpenRead(Globals.VIDEOFILE_PATH);
    //byte[] buffer =newbyte[video.Length];
    //video.Read(buffer, 0, buffer.Length);
    //content.Add(newByteArrayContent(buffer));

    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"name", "video_name"},
            {"description", "video_description"},
            {"privacy", "private"},
            {"partition", "partition"},
            {"videoUrl", videoUrl},
        });
    var uploadRequestResult = await client.PostAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos?{queryParams}", content);
    var uploadResult = await uploadRequestResult.Content.ReadAsStringAsync();

    // get the video id from the upload result
    string videoId = JsonConvert.DeserializeObject<dynamic>(uploadResult)["id"];
    Debug.WriteLine("Uploaded");
    Debug.WriteLine("Video ID:");
    Debug.WriteLine(videoId);

    // wait for the video index to finish
    while (true)
    {
        await Task.Delay(10000);

        queryParams = CreateQueryString(
            new Dictionary<string, string>()
            {
                {"accessToken", accountInfo.AccessToken},
                {"language", "English"},
            });

        var videoGetIndexRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/Index?{queryParams}");
        var videoGetIndexResult = await videoGetIndexRequestResult.Content.ReadAsStringAsync();

        string processingState = JsonConvert.DeserializeObject<dynamic>(videoGetIndexResult)["state"];

        Debug.WriteLine("");
        Debug.WriteLine("State:");
        Debug.WriteLine(processingState);

        // job is finished
        if (processingState != "Uploaded" && processingState != "Processing")
        {
            Debug.WriteLine("");
            Debug.WriteLine("Full JSON:");
            Debug.WriteLine(videoGetIndexResult);
            break;
        }
    }

    // search for the video
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", accountInfo.AccessToken},
            {"id", videoId},
        });

    var searchRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/Search?{queryParams}");
    var searchResult = await searchRequestResult.Content.ReadAsStringAsync();
    Debug.WriteLine("");
    Debug.WriteLine("Search:");
    Debug.WriteLine(searchResult);

    // Generate video access token (used for get widget calls)
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var videoTokenRequestResult = await client.GetAsync($"{apiUrl}/auth/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/AccessToken?allowEdit=true");
    var videoAccessToken = (await videoTokenRequestResult.Content.ReadAsStringAsync()).Replace("\"", "");
    client.DefaultRequestHeaders.Remove("Ocp-Apim-Subscription-Key");

    // get insights widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
            {"widgetType", "Keywords"},
            {"allowEdit", "true"},
        });
    var insightsWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/InsightsWidget?{queryParams}");
    var insightsWidgetLink = insightsWidgetRequestResult.Headers.Location;
    Debug.WriteLine("Insights Widget url:");
    Debug.WriteLine(insightsWidgetLink);

    // get player widget url
    queryParams = CreateQueryString(
        new Dictionary<string, string>()
        {
            {"accessToken", videoAccessToken},
        });
    var playerWidgetRequestResult = await client.GetAsync($"{apiUrl}/{accountInfo.Location}/Accounts/{accountInfo.Id}/Videos/{videoId}/PlayerWidget?{queryParams}");
    var playerWidgetLink = playerWidgetRequestResult.Headers.Location;
    Debug.WriteLine("");
    Debug.WriteLine("Player Widget url:");
    Debug.WriteLine(playerWidgetLink);
}

private string CreateQueryString(IDictionary<string, string> parameters)
{
    var queryParameters = HttpUtility.ParseQueryString(string.Empty);
    foreach (var parameter in parameters)
    {
        queryParameters[parameter.Key] = parameter.Value;
    }

    return queryParameters.ToString();
}

public class AccountContractSlim
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public string Location { get; set; }
    public string AccountType { get; set; }
    public string Url { get; set; }
    public string AccessToken { get; set; }
}
```



## <a name="next-steps"></a>Pasos siguientes

[Examen de la salida de Azure Video Indexer producida por la API v2](video-indexer-output-json-v2.md)
