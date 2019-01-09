---
title: Streaming en directo con Azure Media Services v3 | Microsoft Docs
description: Este tutorial le guía a través de los pasos del streaming en vivo con Media Services v3 mediante .NET Core.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 12/28/2018
ms.author: juliako
ms.openlocfilehash: 858c062c2b3d61b38247e323bf70d2768d33b257
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969342"
---
# <a name="tutorial-stream-live-with-media-services-v3-using-apis"></a>Tutorial: Streaming en directo con Media Services v3 mediante las API

En Azure Media Services, los objetos [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) son los responsables de procesar el contenido de streaming en directo. Un objeto LiveEvent proporciona un punto de conexión de entrada (dirección URL de ingesta) que luego ofrece a un transcodificador en vivo. El objeto LiveEvent recibe flujos de entrada en vivo desde el codificador en directo y los deja a disposición del streaming a través de uno o más objetos [StreamingEndpoints](https://docs.microsoft.com/rest/api/media/streamingendpoints). Los objetos LiveEvents también proporcionan un punto de conexión de vista previa (dirección URL de vista previa) que se puede utilizar para obtener una vista previa y validar el flujo antes de un procesamiento y entrega ulteriores. En este tutorial se muestra cómo usar .NET Core para un tipo de **paso a través** de un evento en directo. 

> [!NOTE]
> Asegúrese de revisar [Streaming en vivo con Media Services v3](live-streaming-overview.md) antes de continuar. 

En este tutorial se muestra cómo realizar las siguientes acciones:    

> [!div class="checklist"]
> * Acceso a la API de Media Services
> * Configuración de la aplicación de ejemplo
> * Examen del código que realiza el streaming en vivo
> * Vea el evento con [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) en http://ampdemo.azureedge.net
> * Limpieza de recursos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos son necesarios para completar el tutorial.

- Instalación de Visual Studio Code o Visual Studio.
- Instale y use la CLI localmente, para este artículo es preciso usar la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

    Actualmente no todos los comandos de la [CLI de Media Services v3](https://aka.ms/ams-v3-cli-ref) funcionan en Azure Cloud Shell. Se recomienda usar la CLI localmente.

- [Cree una cuenta de Media Services](create-account-cli-how-to.md).

    Asegúrese de recordar los valores que usó para el nombre de la cuenta de Media Services y el nombre del grupo de recursos

- Una cámara o un dispositivo (como un equipo portátil) que se utiliza para difundir un evento.
- Un codificador en directo local que convierte las señales de la cámara en secuencias que se envían a un servicio de streaming en vivo de Media Services. La secuencia debe estar en formato **RTMP** o **Smooth Streaming**.

## <a name="download-the-sample"></a>Descarga del ejemplo

Clone un repositorio GitHub que contenga el ejemplo de .NET de streaming en la máquina con el siguiente comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

El ejemplo de streaming en vivo se encuentra en carpeta [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

> [!IMPORTANT]
> Este ejemplo utiliza un sufijo único para cada recurso. Si cancela la depuración o finaliza la aplicación sin ejecutarla, acabará con varios objetos LiveEvents en tu cuenta. <br/>
> Asegúrese de detener los objetos LiveEvents en ejecución. En caso contrario, se le **facturará** por ellos.

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>Examen del código que realiza el streaming en vivo

En esta sección se examinan las funciones definidas en el archivo [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) del proyecto *MediaV3LiveApp*.

El ejemplo crea un sufijo único para cada recurso de modo que no tengamos conflictos de nombres si ejecuta el ejemplo varias veces sin limpiar.

> [!IMPORTANT]
> Este ejemplo utiliza un sufijo único para cada recurso. Si cancela la depuración o finaliza la aplicación sin ejecutarla, acabará con varios objetos LiveEvents en tu cuenta. <br/>
> Asegúrese de detener los objetos LiveEvents en ejecución. En caso contrario, se le **facturará** por ellos.
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>Uso de las API de Media Services con SDK de .NET

Para empezar a usar las API de Media Services con. NET, debe crear un objeto **AzureMediaServicesClient**. Para crear el objeto, debe proporcionar las credenciales necesarias para que el cliente se conecte a Azure mediante Azure AD. En el código que se clonó al principio del artículo, la función **GetCredentialsAsync** crea el objeto ServiceClientCredentials basándose en las credenciales proporcionadas en el archivo de configuración local. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Creación de un evento en directo

En esta sección se muestra cómo crear un tipo de **paso a través** del objeto LiveEvent (LiveEventEncodingType establecido en None). Si desea crear un objeto LiveEvent habilitado para la codificación en directo, establezca LiveEventEncodingType en Estándar. 

Algunos de los aspectos que podría especificar al crear el evento en directo son:

* Ubicación de Media Services 
* El protocolo de streaming para el evento en directo (actualmente, se admiten los protocolos RTMP y Smooth Streaming)
       
    No se puede cambiar la opción de protocolo mientras el objeto LiveEvent o sus objetos LiveOutputs asociados se están ejecutando. Si necesitan diferentes protocolos, debe crear un objeto LiveEvent independiente para cada protocolo de streaming.  
* Restricciones de IP en la ingesta y vista previa. Puede definir las direcciones IP permitidas para introducir un vídeo en este objeto LiveEvent. Las direcciones IP permitidas se pueden especificar como una dirección IP única (por ejemplo, 10.0.0.1), un intervalo IP que usa una dirección IP y una máscara de subred CIDR (por ejemplo, 10.0.0.1/22) o un intervalo de IP que usa una máscara de subred decimal con puntos; por ejemplo, 10.0.0.1(255.255.252.0).
    
    Si no se especifica ninguna dirección IP y no hay ninguna definición de regla, no se permitirá ninguna dirección IP. Para permitir las direcciones IP, cree una regla y establezca 0.0.0.0/0.

Al crear el evento, puede especificar que se inicie automáticamente. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>Obtención de direcciones URL de introducción

Una vez creado LiveEvent, obtendrá direcciones URL de ingesta, que serán las que especificará en el codificador en directo. El codificador usa estas direcciones URL para introducir una secuencia en vivo.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>Obtención de la URL de vista previa

Utilice el objeto previewEndpoint para obtener una vista previa y comprobar que la entrada del codificador se está recibiendo realmente.

> [!IMPORTANT]
> Asegúrese de que el vídeo fluye a la dirección URL de vista previa antes de continuar.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-liveevents-and-liveoutputs"></a>Creación y administración de Creación y administración de objetos LiveEvent y objetos LiveOutput

Cuando la secuencia fluya al objeto LiveEvent, puede comenzar el evento de streaming mediante la creación de los objetos Asset, LiveOutput y StreamingLocator. Se archivará la secuencia y estará disponible a los usuarios a través del punto de conexión de streaming. 

#### <a name="create-an-asset"></a>Creación de un recurso

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

Cree un recurso para que lo utilice el objeto LiveOutput.

#### <a name="create-a-liveoutput"></a>Creación de un objeto LiveOutput

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaminglocator"></a>Creación de un objeto StreamingLocator

> [!NOTE]
> Cuando se crea la cuenta de Media Services, se agrega un punto de conexión de streaming **predeterminado** a la cuenta en estado **Detenido**. Para iniciar la transmisión del contenido y aprovechar el empaquetado dinámico y el cifrado dinámico, el punto de conexión de streaming desde el que va a transmitir el contenido debe estar en estado **Running** (En ejecución). 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

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

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupAccount)]   

## <a name="watch-the-event"></a>Visualización del evento

Para ver el evento, copie la dirección URL de streaming que obtuvo cuando ejecutó el código descrito en [Creación de un objeto StreamingLocator](#create-a-streaminglocator) y utilice el reproductor de su elección. Puede usar [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) para probar la secuencia en http://ampdemo.azureedge.net. 

El evento en directo convierte automáticamente los eventos en contenido a petición cuando se detiene. Incluso después de detener y eliminar el evento, los usuarios podrán transmitir el contenido archivado como un vídeo a petición siempre que no elimine el recurso. No se puede eliminar un recurso si lo está usando un evento; primero se debe eliminar el evento. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita ninguno de los recursos del grupo de recursos, incluida las cuentas de almacenamiento y de Media Services que ha creado en este tutorial, elimine el grupo de recursos que ha creado antes.

Ejecute el siguiente comando de la CLI:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Si se deja el objeto LiveEvent en ejecución, se incurrirá en costos de facturación. Tenga en cuenta que si el proyecto o programa se bloquea o se cierra por cualquier motivo, es posible que el objeto LiveEvent se ejecute en un estado de facturación.

## <a name="next-steps"></a>Pasos siguientes

[Streaming de archivos](stream-files-tutorial-with-api.md)

