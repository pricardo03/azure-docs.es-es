---
title: 'Inicio rápido: Análisis de imágenes (SDK, C#)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, analizará una imagen mediante la biblioteca cliente de C# para Windows de Computer Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 3b6bb2ae0bcae869436eb54376b9792d8d4ccb0e
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878279"
---
# <a name="quickstart-analyze-an-image-using-the-computer-vision-sdk-and-c"></a>Inicio rápido: Análisis de imágenes con el SDK de Computer Vision y C#

En esta guía de inicio rápido, analizará una imagen local y remota para extraer características visuales mediante la biblioteca de cliente en Windows para Computer Vision.

## <a name="prerequisites"></a>Requisitos previos

* Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).
* Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).
* El paquete NuGet de la biblioteca cliente [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision). No es necesario descargar el paquete. A continuación, se proporcionan instrucciones de instalación.

## <a name="analyzeimageasync-method"></a>Método AnalyzeImageAsync

> [!TIP]
> Obtenga el código más reciente como solución de Visual Studio desde [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision).

Los métodos `AnalyzeImageAsync` y `AnalyzeImageInStreamAsync` encapsulan [Analyze Image API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) para las imágenes remotas y locales, respectivamente. Puede usar estos métodos para extraer características visuales basadas en el contenido de la imagen y seleccionar las características que se van a devolver, por ejemplo:

* Una lista detallada de las etiquetas relacionadas con el contenido de la imagen.
* Una descripción del contenido de la imagen en una oración completa.
* Las coordenadas, el sexo y la edad de las caras de la imagen.
* El tipo de imagen (imágenes prediseñadas o dibujo lineal).
* El color dominante, el color de énfasis o si una imagen está en blanco y negro.
* La categoría definida en esta [taxonomía](../Category-Taxonomy.md).
* ¿La imagen incluye contenido para adultos o insinuaciones de tipo sexual?

Para ejecutar el ejemplo, siga estos pasos:

1. Cree una aplicación de consola de Visual C# en Visual Studio.
1. Instale el paquete NuGet de la biblioteca cliente de Computer Vision.
    1. En el menú, haga clic en **Herramientas**, seleccione **Administrador de paquetes NuGet** y, luego, **Manage NuGet Packages for Solution** (Administrar paquetes NuGet para la solución).
    1. Haga clic en la pestaña **Examinar** y, en el cuadro **Buscar**, escriba "Microsoft.Azure.CognitiveServices.Vision.ComputerVision".
    1. Seleccione **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** cuando aparezca, marque la casilla junto al nombre del proyecto y haga clic en **Instalar**.
1. Reemplace `Program.cs` por el código siguiente.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Cambie `computerVision.Endpoint` por la región de Azure asociada con las claves de suscripción, si es necesario.
1. Reemplace `<LocalImage>` por la ruta de acceso y el nombre de archivo de una imagen local.
1. También tiene la opción de establecer `remoteImageUrl` en una imagen diferente.
1. Ejecute el programa.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace ImageAnalyze
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg";

        // Specify the features to return
        private static readonly List<VisualFeatureTypes> features =
            new List<VisualFeatureTypes>()
        {
            VisualFeatureTypes.Categories, VisualFeatureTypes.Description,
            VisualFeatureTypes.Faces, VisualFeatureTypes.ImageType,
            VisualFeatureTypes.Tags
        };

        static void Main(string[] args)
        {
            ComputerVisionClient computerVision = new ComputerVisionClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });

            // You must use the same region as you used to get your subscription
            // keys. For example, if you got your subscription keys from westus,
            // replace "westcentralus" with "westus".
            //
            // Free trial subscription keys are generated in the "westus"
            // region. If you use a free trial subscription key, you shouldn't
            // need to change the region.

            // Specify the Azure region
            computerVision.Endpoint = "https://westcentralus.api.cognitive.microsoft.com";

            Console.WriteLine("Images being analyzed ...");
            var t1 = AnalyzeRemoteAsync(computerVision, remoteImageUrl);
            var t2 = AnalyzeLocalAsync(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press ENTER to exit");
            Console.ReadLine();
        }

        // Analyze a remote image
        private static async Task AnalyzeRemoteAsync(
            ComputerVisionClient computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            ImageAnalysis analysis =
                await computerVision.AnalyzeImageAsync(imageUrl, features);
            DisplayResults(analysis, imageUrl);
        }

        // Analyze a local image
        private static async Task AnalyzeLocalAsync(
            ComputerVisionClient computerVision, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            using (Stream imageStream = File.OpenRead(imagePath))
            {
                ImageAnalysis analysis = await computerVision.AnalyzeImageInStreamAsync(
                    imageStream, features);
                DisplayResults(analysis, imagePath);
            }
        }

        // Display the most relevant caption for the image
        private static void DisplayResults(ImageAnalysis analysis, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(analysis.Description.Captions[0].Text + "\n");
        }
    }
}
```

## <a name="analyzeimageasync-response"></a>Respuesta de AnalyzeImageAsync

Una respuesta correcta muestra el título más pertinente para cada imagen.

Consulte [Inicios rápidos de API: Análisis de imágenes locales con C#](../QuickStarts/CSharp-analyze.md#examine-the-response) para ver un ejemplo de la salida JSON sin procesar.

```
http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg
a large waterfall over a rocky cliff
```

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API que se usan para analizar una imagen, detectar personajes y sitios emblemáticos, crear una miniatura y extraer texto impreso y escrito a mano.

> [!div class="nextstepaction"]
> [Exploración de las versiones de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
