---
title: Carga, codificación y transmisión con Azure Media Services | Microsoft Docs
description: Siga los pasos de este tutorial para cargar un archivo, codificar el vídeo y transmitir su contenido con Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 0216a95a5209f5545b34e446904b3215950c6fbc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638116"
---
# <a name="tutorial-upload-encode-and-stream-videos-using-apis"></a>Tutorial: Carga, codificación y transmisión de vídeos con las API

Media Services permite codificar los archivos multimedia en formatos que se pueden reproducir en una gran variedad de exploradores y dispositivos. Por ejemplo, puede que quiera transmitir su contenido en los formatos HLS o MPEG DASH de Apple. Antes de la transmisión, primero debe codificar su archivo de medios digitales de alta calidad. Para obtener instrucciones acerca de la codificación, consulte [El concepto de codificación](encoding-concept.md). Este tutorial carga un archivo de vídeo local y codifica el archivo cargado. También puede codificar contenido que se hace accesible a través de una dirección URL HTTPS. Para más información, consulte [Creación de una entrada de un trabajo desde una dirección URL HTTP(s)](job-input-from-http-how-to.md).

![Reproducción del vídeo](./media/stream-files-tutorial-with-api/final-video.png)

En este tutorial se muestra cómo realizar las siguientes acciones:    

> [!div class="checklist"]
> * Creación de una cuenta de Media Services
> * Acceso a la API de Media Services
> * Configuración de la aplicación de ejemplo
> * Examen del código que carga, codifica y transmite en secuencias
> * Ejecución de la aplicación
> * Prueba de la URL de streaming
> * Limpieza de recursos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>requisitos previos

Si no tiene Visual Studio instalado, puede obtener [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).

## <a name="download-the-sample"></a>Descarga del ejemplo

Clone un repositorio GitHub que contenga el ejemplo de .NET de streaming en la máquina con el siguiente comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

El ejemplo se encuentra en la carpeta [UploadEncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/UploadEncodeAndStreamFiles).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-uploads-encodes-and-streams"></a>Examen del código que carga, codifica y transmite en secuencias

En esta sección se examinan las funciones definidas en el archivo [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs) del proyecto *UploadEncodeAndStreamFiles*.

Este ejemplo realiza las acciones siguientes:

1. Crea una nueva transformación (en primer lugar, comprueba si existe la transformación especificada). 
2. Crea un recurso de salida que se utiliza como la salida del trabajo de codificación.
3. Crea un recurso de entrada y carga el archivo de vídeo local especificado en él. El recurso se usa como entrada del trabajo. 
4. Envía al trabajo de codificación con la entrada y salida que se han creado.
5. Comprueba el estado del trabajo.
6. Crea un objeto StreamingLocator.
7. Crea direcciones URL de streaming.

### <a name="start-using-media-services-apis-with-net-sdk"></a>Uso de las API de Media Services con SDK de .NET

Para empezar a usar las API de Media Services con. NET, debe crear un objeto **AzureMediaServicesClient**. Para crear el objeto, debe proporcionar las credenciales necesarias para que el cliente se conecte a Azure mediante Azure AD. En el código que se clonó al principio del artículo, la función **GetCredentialsAsync** crea el objeto ServiceClientCredentials basándose en las credenciales proporcionadas en el archivo de configuración local. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Creación de un recurso de entrada y carga de un archivo local en él 

La función **CreateInputAsset** crea un nuevo [recurso](https://docs.microsoft.com/rest/api/media/assets) de entrada y carga en él el archivo de vídeo local especificado. Este recurso se utiliza como entrada para el trabajo de codificación. En Media Services v3, la entrada a un trabajo puede ser un recurso, o puede ser contenido que se pone a disposición de la cuenta de Media Services a través de direcciones URL HTTPS. Para más información sobre cómo codificar desde una dirección URL HTTPS, consulte [este](job-input-from-http-how-to.md) artículo.  

En Media Services v3, se utilizan las API de Azure Storage para cargar archivos. En el siguiente fragmento de código de .NET se muestra cómo hacerlo.

La función siguiente realiza estas acciones:

* Crea un recurso. 
* Obtiene una [dirección URL de SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) que se puede escribir en el [contenedor de almacenamiento](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) del recurso.
* Carga el archivo en el contenedor de almacenamiento mediante la dirección URL de SAS.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-a-job"></a>Creación de un recurso de salida para almacenar el resultado de un trabajo 

El [recurso](https://docs.microsoft.com/rest/api/media/assets) de salida almacena el resultado del trabajo de codificación. El proyecto define la función **DownloadResults** que descarga los resultados de este recurso de salida en la carpeta "output", para que se pueda ver lo que tiene.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-encodes-the-uploaded-file"></a>Creación de una transformación y de un trabajo que codifica el archivo cargado
Cuando se codifica o procesa contenido en Media Services, es un patrón común configurar los ajustes de codificación como una receta. Después, podría enviar un **trabajo** para aplicar esa receta a un vídeo. Al enviar nuevos trabajos a cada nuevo vídeo, está aplicando esa receta a todos los vídeos de la biblioteca. Una receta en Media Services se llama **transformación**. Para más información, consulte [Transformaciones y trabajos](transform-concept.md). El ejemplo descrito en este tutorial define una receta que codifica el vídeo para transmitirlo a varios dispositivos iOS y Android. 

#### <a name="transform"></a>Transformación

Al crear una nueva instancia de la [transformación](https://docs.microsoft.com/rest/api/media/transforms), debe especificar qué desea originar como salida. El parámetro requerido es un objeto **TransformOutput**, como se muestra en el código siguiente. Cada objeto **TransformOutput** contiene un **valor preestablecido**. El **valor preestablecido** describe las instrucciones paso a paso de las operaciones de procesamiento de vídeo o audio que se utilizarán para generar el objeto **TransformOutput** deseado. El ejemplo descrito en este artículo utiliza un valor preestablecido integrado denominado **AdaptiveStreaming**. El valor preestablecido codifica el vídeo de entrada en una escala de velocidad de bits generada automáticamente (pares resolución-velocidad de bits) basada en la resolución y la velocidad de bits, y produce archivos ISO MP4 con vídeo H.264 y audio AAC correspondiente a cada par resolución-velocidad de bits. Para más información sobre este valor preestablecido, consulte el artículo sobre la [generación automática de la escala de velocidad de bits](autogen-bitrate-ladder.md).

Puede usar un valor de EncoderNamedPreset integrado o valores preestablecidos personalizados. Para más información, consulte [Personalización de los valores predefinidos del codificador](customize-encoder-presets-how-to.md).

Al crear una [transformación](https://docs.microsoft.com/rest/api/media/transforms), debe comprobar primero si ya existe una con el método **Get**, tal como se muestra en el código siguiente.  En Media Services v3, los métodos **Get** en las entidades devuelven **null** si la entidad no existe (una comprobación sin distinción entre mayúsculas y minúsculas en el nombre).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>Trabajo

Como se mencionó anteriormente, el objeto [Transform](https://docs.microsoft.com/rest/api/media/transforms) es la receta y un [trabajo](https://docs.microsoft.com/rest/api/media/jobs) es la solicitud real a Media Services para aplicar que dicho objeto **Transform** a un determinado contenido de audio o vídeo de entrada. El **trabajo** especifica información como la ubicación del vídeo de entrada y la ubicación de la salida.

En este ejemplo, el vídeo de entrada se ha cargado desde la máquina local. Para más información sobre cómo codificar desde una dirección URL HTTPS, consulte [este](job-input-from-http-how-to.md) artículo.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>Espere a que el trabajo se complete.

El trabajo tarda algún tiempo en completarse y cuando lo hace querrá recibir una notificación. En el ejemplo de código siguiente se muestra cómo sondear el servicio para conocer el estado del [trabajo](https://docs.microsoft.com/rest/api/media/jobs). El sondeo no es un procedimiento recomendado para aplicaciones de producción debido a la posible latencia. El sondeo se puede limitar si se sobreutiliza en una cuenta. Los desarrolladores deben utilizar en su lugar Event Grid.

Event Grid está diseñado para una alta disponibilidad, un rendimiento consistente y una escala dinámica. Con Event Grid, sus aplicaciones pueden escuchar y reaccionar a eventos de casi todos los servicios de Azure y de orígenes personalizados. El control de eventos sencillo y reactivo basado en HTTP le ayuda a crear soluciones eficaces con filtrado y enrutamiento de eventos inteligente.  Consulte [Enrutamiento de eventos a un punto de conexión web personalizado](job-state-events-cli-how-to.md).

El **trabajo** normalmente pasa por los siguientes estados: **Programado**, **En cola**, **Procesando**, **Finalizado** (el estado final). Si el trabajo ha encontrado un error, obtendrá el estado **Error**. Si el trabajo está en proceso de cancelación, obtendrá **Cancelando** y **Cancelado** cuando haya terminado.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#WaitForJobToFinish)]

### <a name="get-a-streaminglocator"></a>Obtención de un objeto StreamingLocator

Una vez finalizada la codificación, el siguiente paso es poner el vídeo del recurso de salida a disposición de los clientes para su reproducción. Puede hacerlo en dos pasos: primero, cree un objeto [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) y, en segundo lugar, cree las direcciones URL de streaming que pueden usar los clientes. 

El proceso de creación de un objeto **StreamingLocator** se denomina publicación. De forma predeterminada, el objeto **StreamingLocator** es válido inmediatamente después de realizar las llamadas a la API y dura hasta que se elimina, a menos que configure las horas de inicio y de finalización opcionales. 

Al crear un objeto [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), debe especificar el objeto **StreamingPolicyName** deseado. En este ejemplo, estará transmitiendo contenido no cifrado, de modo que se utiliza la directiva de streaming sin cifrar predefinida (**PredefinedStreamingPolicy.ClearStreamingOnly**).

> [!IMPORTANT]
> Al utilizar el objeto [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) personalizado, debe diseñar un conjunto limitado de dichas directivas para su cuenta de Media Service y reutilizarlas para sus objetos StreamingLocator siempre que se necesiten las mismas opciones y protocolos de cifrado. La cuenta de Media Service tiene una cuota para el número de entradas de StreamingPolicy. No debe crear un nuevo objeto StreamingPolicy para cada objeto StreamingLocator.

El código siguiente da por supuesto que está llamando a la función con un único objeto locatorName.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateStreamingLocator)]

Aunque el ejemplo de este tema trata del streaming, puede utilizar la misma llamada para crear un objeto StreamingLocator para la entrega de vídeo mediante descarga progresiva.

### <a name="get-streaming-urls"></a>Obtención de direcciones URL de streaming

Ahora que se ha creado un objeto [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), puede obtener las direcciones URL de streaming, tal y como se muestra en **GetStreamingURLs**. Para crear una dirección URL, debe concatenar el nombre de host de [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) y la ruta de acceso de **StreamingLocator**. En este ejemplo, se utiliza el objeto **StreamingEndpoint** *predeterminado*. Cuando cree una cuenta de Media Services por primera vez, este **StreamingEndpoint** *predeterminado* estará en un estado detenido, por lo que deberá llamar a **Start**.

> [!NOTE]
> En este método, se necesita el objeto locatorName que se utilizó al crear el objeto **StreamingLocator** para el recurso de salida.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#GetStreamingURLs)]

### <a name="clean-up-resources-in-your-media-services-account"></a>Limpieza de los recursos en su cuenta de Media Services

Por lo general, debe limpiar todo excepto los objetos que piensa reutilizar (típicamente, reutilizará transformaciones y conservará objetos StreamingLocators, etc.). Si desea que la cuenta esté limpia después de la experimentación, debe eliminar los recursos que no piensa volver a usar.  Por ejemplo, el código siguiente elimina los trabajos.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

1. Presione Control + F5 para ejecutar la aplicación *EncodeAndStreamFiles*.
2. Copie una de las direcciones URL de streaming desde la consola.

En este ejemplo se muestran las direcciones URL que pueden usarse para reproducir el vídeo con diferentes protocolos:

![Salida](./media/stream-files-tutorial-with-api/output.png)

## <a name="test-the-streaming-url"></a>Prueba de la URL de streaming

Para probar la transmisión, este artículo usa Azure Media Player. 

> [!NOTE]
> Si el reproductor está hospedado en un sitio https, asegúrese de actualizar la dirección URL a "https".

1. Abra un explorador web y vaya a [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/).
2. En el cuadro **Dirección URL:**, pegue uno de los valores de la dirección URL de streaming que se obtuvo al ejecutar la aplicación. 
3. Presione **Actualizar Player**.

Azure Media Player puede usarse para realizar pruebas, pero no debe usarse en un entorno de producción. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ya no necesita ninguno de los recursos del grupo de recursos, incluida las cuentas de almacenamiento y de Media Services que ha creado en este tutorial, elimine el grupo de recursos que ha creado antes. Puede usar la herramienta **CloudShell**.

En **CloudShell**, ejecute el siguiente comando:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Multithreading

Los SDK de Azure Media Services v3 no son seguros para subprocesos. Al desarrollar una aplicación multiproceso, debe generar y utilizar un nuevo objeto AzureMediaServicesClient por subproceso.

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo cargar, codificar y transmitir el vídeo, consulte el siguiente artículo: 

> [!div class="nextstepaction"]
> [Análisis de vídeos](analyze-videos-tutorial-with-api.md)
