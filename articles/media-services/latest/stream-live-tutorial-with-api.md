---
title: Streaming en vivo con Azure Media Services v3 mediante .NET Core | Microsoft Docs
description: Este tutorial le guía a través de los pasos del streaming en vivo con Media Services v3 mediante .NET Core.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/06/2018
ms.author: juliako
ms.openlocfilehash: f02820f8382268dfedeee8d07de2438660e5e33e
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236889"
---
# <a name="stream-live-with-azure-media-services-v3-using-net-core"></a>Streaming en vivo con Azure Media Services v3 mediante .NET Core

En Media Services, los objetos [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) son los responsables de procesar el contenido de streaming en vivo. Un objeto LiveEvent proporciona un punto de conexión de entrada (dirección URL de ingesta) que luego ofrece a un transcodificador en vivo. El objeto LiveEvent recibe flujos de entrada en vivo desde el codificador en directo y los deja a disposición del streaming a través de uno o más objetos [StreamingEndpoints](https://docs.microsoft.com/rest/api/media/streamingendpoints). Los objetos LiveEvents también proporcionan un punto de conexión de vista previa (dirección URL de vista previa) que se puede utilizar para obtener una vista previa y validar el flujo antes de un procesamiento y entrega ulteriores. En este tutorial se muestra cómo usar .NET Core para un tipo de **paso a través** de un evento en directo. 

> [!NOTE]
> Asegúrese de revisar [Streaming en vivo con Media Services v3](live-streaming-overview.md) antes de continuar. 

En este tutorial se muestra cómo realizar las siguientes acciones:    

> [!div class="checklist"]
> * Creación de una cuenta de Media Services
> * Acceso a la API de Media Services
> * Configuración de la aplicación de ejemplo
> * Examen del código que realiza el streaming en vivo
> * Vea el evento con [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) en http://ampdemo.azureedge.net
> * Limpieza de recursos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>requisitos previos

Los siguientes requisitos son necesarios para completar el tutorial.

* Instalación de Visual Studio Code o Visual Studio
* Una cámara o un dispositivo (como un equipo portátil) que se utiliza para difundir un evento.
* Un codificador en directo local que convierte las señales de la cámara en secuencias que se envían a un servicio de streaming en vivo de Media Services. La secuencia debe estar en formato **RTMP** o **Smooth Streaming**.

## <a name="download-the-sample"></a>Descarga del ejemplo

Clone un repositorio GitHub que contenga el ejemplo de .NET de streaming en la máquina con el siguiente comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

El ejemplo de streaming en vivo se encuentra en carpeta [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

> [!IMPORTANT]
> Este ejemplo utiliza un sufijo único para cada recurso. Si cancela la depuración o finaliza la aplicación sin ejecutarla, acabará con varios objetos LiveEvents en tu cuenta. <br/>
> Asegúrese de detener los objetos LiveEvents en ejecución. En caso contrario, se le **facturará** por ellos.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>Examen del código que realiza el streaming en vivo

En esta sección se examinan las funciones definidas en el archivo [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) del proyecto *MediaV3LiveApp*.

El ejemplo crea un sufijo único para cada recurso de modo que no tengamos conflictos de nombres si ejecuta el ejemplo varias veces sin limpiar.

> [!IMPORTANT]
> Este ejemplo utiliza un sufijo único para cada recurso. Si cancela la depuración o finaliza la aplicación sin ejecutarla, acabará con varios objetos LiveEvents en tu cuenta. <br/>
> Asegúrese de detener los objetos LiveEvents en ejecución. En caso contrario, se le **facturará** por ellos.
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>Uso de las API de Media Services con SDK de .NET

Para empezar a usar las API de Media Services con. NET, debe crear un objeto **AzureMediaServicesClient**. Para crear el objeto, debe proporcionar las credenciales necesarias para que el cliente se conecte a Azure mediante Azure AD. En el código que se clonó al principio del artículo, la función **GetCredentialsAsync** crea el objeto ServiceClientCredentials basándose en las credenciales proporcionadas en el archivo de configuración local.  

```csharp
private static async Task<IAzureMediaServicesClient> CreateMediaServicesClientAsync(ConfigWrapper config)
{
    var credentials = await GetCredentialsAsync(config);

    return new AzureMediaServicesClient(config.ArmEndpoint, credentials)
    {
        SubscriptionId = config.SubscriptionId,
    };
}
```

### <a name="create-a-live-event"></a>Creación de un evento en directo

En esta sección se muestra cómo crear un tipo de **paso a través** del objeto LiveEvent (LiveEventEncodingType establecido en None). Si desea crear un objeto LiveEvent habilitado para la codificación en directo, establezca LiveEventEncodingType en Basic. 

Algunos de los aspectos que podría especificar al crear el evento en directo son:

* Ubicación de Media Services 
* El protocolo de streaming para el evento en directo (actualmente, se admiten los protocolos RTMP y Smooth Streaming)
       
    No se puede cambiar la opción de protocolo mientras el objeto LiveEvent o sus objetos LiveOutputs asociados se están ejecutando. Si necesitan diferentes protocolos, debe crear un objeto LiveEvent independiente para cada protocolo de streaming.  
* Restricciones de IP en la ingesta y vista previa. Puede definir las direcciones IP permitidas para introducir un vídeo en este objeto LiveEvent. Las direcciones IP permitidas se pueden especificar como una dirección IP única (por ejemplo, 10.0.0.1), un intervalo IP que usa una dirección IP y una máscara de subred CIDR (por ejemplo, 10.0.0.1/22) o un intervalo de IP que usa una máscara de subred decimal con puntos; por ejemplo, 10.0.0.1(255.255.252.0).
    
    Si no se especifica ninguna dirección IP y no hay ninguna definición de regla, no se permitirá ninguna dirección IP. Para permitir las direcciones IP, cree una regla y establezca 0.0.0.0/0.

Al crear el evento, puede especificar que se inicie automáticamente. 

```csharp
Console.WriteLine($"Creating a live event named {liveEventName}");
Console.WriteLine();

LiveEventPreview liveEventPreview = new LiveEventPreview
{
    AccessControl = new LiveEventPreviewAccessControl(
        ip: new IPAccessControl(
            allow: new IPRange[]
            {
                new IPRange (
                    name: "AllowAll",
                    address: "0.0.0.0",
                    subnetPrefixLength: 0
                )
            }
        )
    )
};

// This can sometimes take awhile. Be patient.
LiveEvent liveEvent = new LiveEvent(
    location: mediaService.Location, 
    description:"Sample LiveEvent for testing",
    vanityUrl:false,
    encoding: new LiveEventEncoding(
                // Set this to Basic to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                encodingType:LiveEventEncodingType.None, 
                presetName:null
            ),
    input: new LiveEventInput(LiveEventInputProtocol.RTMP), 
    preview: liveEventPreview,
    streamOptions: new List<StreamOptionsFlag?>()
    {
        // Set this to Default or Low Latency.
        // Low latency reduces the amount of buffering Media Services does.
        // Low latency can also reduce the stability of the live stream. 
        StreamOptionsFlag.Default
    }
);

Console.WriteLine($"Creating the LiveEvent, be patient this can take time...");
liveEvent = client.LiveEvents.Create(config.ResourceGroup, config.AccountName, liveEventName, liveEvent, autoStart:true);
```

### <a name="get-ingest-urls"></a>Obtención de direcciones URL de introducción

Una vez creado el canal, obtendrá direcciones URL de introducción que se proporcionarán al codificador en directo. El codificador usa estas direcciones URL para introducir una secuencia en vivo.


```csharp
// Get the input endpoint to configure the on premise encoder with
string ingestUrl = liveEvent.Input.Endpoints.First().Url;
Console.WriteLine($"The ingest url to configure the on premise encoder with is:");
Console.WriteLine($"\t{ingestUrl}");
Console.WriteLine();
```

### <a name="get-the-preview-url"></a>Obtención de la URL de vista previa

Utilice el objeto previewEndpoint para obtener una vista previa y comprobar que la entrada del codificador se está recibiendo realmente.

> [!IMPORTANT]
> Asegúrese de que el vídeo fluye a la dirección URL de vista previa antes de continuar.

```sharp
string previewEndpoint = liveEvent.Preview.Endpoints.First().Url;
Console.WriteLine($"The preview url is:");
Console.WriteLine($"\t{previewEndpoint}");
Console.WriteLine();

Console.WriteLine($"Open the live preview in your browser and use the Azure Media Player to monitor the preview playback:");
Console.WriteLine($"\thttps://ampdemo.azureedge.net/?url={previewEndpoint}");
Console.WriteLine();
```

### <a name="create-and-manage-liveevents-and-liveoutputs"></a>Creación y administración de Creación y administración de objetos LiveEvent y objetos LiveOutput

Cuando la secuencia fluya al objeto LiveEvent, puede comenzar el evento de streaming mediante la creación de los objetos Asset, LiveOutput y StreamingLocator. Se archivará la secuencia y estará disponible a los usuarios a través del punto de conexión de streaming. 

#### <a name="create-an-asset"></a>Creación de un recurso

Cree un recurso para que lo utilice el objeto LiveOutput.

```csharp
string assetName = "archiveAsset" + uniqueness;
Console.WriteLine($"Creating an asset named {assetName}");
Console.WriteLine();
Asset asset = client.Assets.CreateOrUpdate(config.ResourceGroup, config.AccountName, assetName, new Asset());
```

#### <a name="create-a-liveoutput"></a>Creación de un objeto LiveOutput

```csharp
string manifestName = "output";
string liveOutputName = "liveOutput" + uniqueness;
Console.WriteLine($"Creating a live output named {liveOutputName}");
Console.WriteLine();

LiveOutput liveOutput = new LiveOutput(assetName: asset.Name, manifestName: manifestName, archiveWindowLength: TimeSpan.FromMinutes(10));
liveOutput = client.LiveOutputs.Create(config.ResourceGroup, config.AccountName, liveEventName, liveOutputName, liveOutput);
```

#### <a name="create-a-streaminglocator"></a>Creación de un objeto StreamingLocator

> [!NOTE]
> Cuando se crea la cuenta de Media Services, se agrega un punto de conexión de streaming **predeterminado** a la cuenta en estado **Detenido**. Para iniciar la transmisión del contenido y aprovechar el empaquetado dinámico y el cifrado dinámico, el punto de conexión de streaming desde el que va a transmitir el contenido debe estar en estado **Running** (En ejecución). 


```csharp
StreamingLocator locator = new StreamingLocator(assetName: assetName, streamingPolicyName: PredefinedStreamingPolicy.ClearStreamingOnly);
locator = client.StreamingLocators.Create(config.ResourceGroup, config.AccountName, streamingLocatorName, locator);

// Get the default Streaming Endpoint on the account
StreamingEndpoint streamingEndpoint = client.StreamingEndpoints.Get(config.ResourceGroup, config.AccountName, "default");

// If it's not running, Start it. 
if (streamingEndpoint.ResourceState != StreamingEndpointResourceState.Running)
{
    Console.WriteLine("Streaming Endpoint was Stopped, restarting now..");
    client.StreamingEndpoints.Start(config.ResourceGroup, config.AccountName, "default");
}

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Limpieza de los recursos en su cuenta de Media Services

Si se realizan eventos de streaming y desea limpiar los recursos aprovisionados anteriormente, siga el procedimiento siguiente.

* Detenga la inserción de la secuencia en el codificador.
* Detenga el objeto LiveEvent. Cuando se detiene el objeto LiveEvent, no se incurre en ningún cargo. Cuando necesite iniciarlo de nuevo, tendrá la misma URL de introducción, por lo que no necesitará volver a configurar su codificador.
* Puede detener el objeto StreamingEndpoint, a menos que desee seguir proporcionando el archivo de su evento en directo como una secuencia a petición. Cuando el objeto LiveEvent está en estado detenido, no se incurrirá en ningún cargo.

```csharp
private static void CleanupLiveEventAndOutput(IAzureMediaServicesClient client, string resourceGroup, string accountName, string liveEventName, string liveOutputName)
{
    // Delete the LiveOutput
    client.LiveOutputs.Delete(resourceGroup, accountName, liveEventName, liveOutputName);

    // Stop and delete the LiveEvent
    client.LiveEvents.Stop(resourceGroup, accountName, liveEventName);
    client.LiveEvents.Delete(resourceGroup, accountName, liveEventName);
}

private static void CleanupLocatorAssetAndStreamingEndpoint(IAzureMediaServicesClient client, string resourceGroup, string accountName, string streamingLocatorName, string assetName)
{
    // Delete the Streaming Locator
    client.StreamingLocators.Delete(resourceGroup, accountName, streamingLocatorName);

    // Delete the Archive Asset
    client.Assets.Delete(resourceGroup, accountName, assetName);
}

private static void CleanupAccount(IAzureMediaServicesClient client, string resourceGroup, string accountName)
{
    try{
        Console.WriteLine("Cleaning up the resources used, stopping the LiveEvent. This can take a few minutes to complete.");
        Console.WriteLine();

        var events = client.LiveEvents.List(resourceGroup, accountName);
        
        foreach (LiveEvent l in events)
        {
            if (l.Name == liveEventName){
                var outputs = client.LiveOutputs.List(resourceGroup, accountName, l.Name);

                foreach (LiveOutput o in outputs)
                {
                    client.LiveOutputs.Delete(resourceGroup, accountName, l.Name, o.Name);
                    Console.WriteLine($"LiveOutput: {o.Name} deleted from LiveEvent {l.Name}. The archived Asset and Streaming URLs are still retained for on-demand viewing.");
                }

                if (l.ResourceState == LiveEventResourceState.Running){
                    client.LiveEvents.Stop(resourceGroup, accountName, l.Name);
                    Console.WriteLine($"LiveEvent: {l.Name} Stopped.");
                    client.LiveEvents.Delete(resourceGroup, accountName, l.Name);
                    Console.WriteLine($"LiveEvent: {l.Name} Deleted.");
                    Console.WriteLine();
                }
            }
        }
    } 
    catch(ApiErrorException e)
    {
        Console.WriteLine("Hit ApiErrorException");
        Console.WriteLine($"\tCode: {e.Body.Error.Code}");
        Console.WriteLine($"\tCode: {e.Body.Error.Message}");
        Console.WriteLine();

    }
}
```        

## <a name="watch-the-event"></a>Visualización del evento

Para ver el evento, copie la dirección URL de streaming que obtuvo cuando ejecutó el código descrito en [Creación de un objeto StreamingLocator](#create-a-streaminglocator) y utilice el reproductor de su elección. Puede usar [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) para probar la secuencia en http://ampdemo.azureedge.net. 

El evento en directo convierte automáticamente los eventos en contenido a petición cuando se detiene. Incluso después de detener y eliminar el evento, los usuarios podrán transmitir el contenido archivado como un vídeo a petición siempre que no elimine el recurso. No se puede eliminar un recurso si lo está usando un evento; primero se debe eliminar el evento. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita ninguno de los recursos del grupo de recursos, incluida las cuentas de almacenamiento y de Media Services que ha creado en este tutorial, elimine el grupo de recursos que ha creado antes. Puede usar la herramienta **CloudShell**.

En **CloudShell**, ejecute el siguiente comando:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Si se deja el objeto LiveEvent en ejecución, se incurrirá en costos de facturación. Tenga en cuenta que si el proyecto o programa se bloquea o se cierra por cualquier motivo, es posible que el objeto LiveEvent se ejecute en un estado de facturación.

## <a name="next-steps"></a>Pasos siguientes

[Streaming de archivos](stream-files-tutorial-with-api.md)

