---
title: 'Inicio rápido: Extracción de texto manuscrito (SDK, C#)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, extraerá texto de una imagen mediante la biblioteca cliente de C# para Windows de Computer Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/12/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: d71a566d5c6dc5505b4bd939e294f8428e9a5b93
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312916"
---
# <a name="quickstart-extract-text-using-the-computer-vision-sdk-and-c"></a>Inicio rápido: Extracción de texto con el SDK de Computer Vision y C#

En este inicio rápido, extraerá texto escrito a mano o impreso de una imagen mediante el SDK Computer Vision para C#. Si lo desea, puede descargar el código en esta guía como una aplicación de ejemplo completa desde el repositorio [Cognitive Services Csharp Vision](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision) en GitHub.

## <a name="prerequisites"></a>Requisitos previos

* Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).
* Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).
* El paquete NuGet de la biblioteca cliente [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision). No es necesario descargar el paquete. A continuación, se proporcionan instrucciones de instalación.

## <a name="create-and-run-the-sample-app"></a>Creación y ejecución de la aplicación de ejemplo

Para ejecutar el ejemplo, siga estos pasos:

1. Cree una aplicación de consola de Visual C# en Visual Studio.
1. Instale el paquete NuGet de la biblioteca cliente de Computer Vision.
    1. En el menú, haga clic en **Herramientas**, seleccione **Administrador de paquetes NuGet** y, luego, **Manage NuGet Packages for Solution** (Administrar paquetes NuGet para la solución).
    1. Haga clic en la pestaña **Examinar** y, en el cuadro **Buscar**, escriba "Microsoft.Azure.CognitiveServices.Vision.ComputerVision".
    1. Seleccione **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** cuando aparezca, marque la casilla junto al nombre del proyecto y haga clic en **Instalar**.
1. Reemplace `Program.cs` por el código siguiente. Los métodos `RecognizeTextAsync` y `RecognizeTextInStreamAsync` encapsulan [Recognize Text API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) para las imágenes remotas y locales, respectivamente. El método `GetTextOperationResultAsync` encapsula [Get Recognize Text Operation Results API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201).

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

    using System;
    using System.IO;
    using System.Threading.Tasks;

    namespace ExtractText
    {
        class Program
        {
            // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
            private const string subscriptionKey = "<SubscriptionKey>";

            // For printed text, change to TextRecognitionMode.Printed
            private const TextRecognitionMode textRecognitionMode =
                TextRecognitionMode.Handwritten;

            // localImagePath = @"C:\Documents\LocalImage.jpg"
            private const string localImagePath = @"<LocalImage>";

            private const string remoteImageUrl =
                "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
                "Cursive_Writing_on_Notebook_paper.jpg/" +
                "800px-Cursive_Writing_on_Notebook_paper.jpg";

            private const int numberOfCharsInOperationId = 36;

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
                var t1 = ExtractRemoteTextAsync(computerVision, remoteImageUrl);
                var t2 = ExtractLocalTextAsync(computerVision, localImagePath);

                Task.WhenAll(t1, t2).Wait(5000);
                Console.WriteLine("Press ENTER to exit");
                Console.ReadLine();
            }

            // Recognize text from a remote image
            private static async Task ExtractRemoteTextAsync(
                ComputerVisionClient computerVision, string imageUrl)
            {
                if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
                {
                    Console.WriteLine(
                        "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                    return;
                }

                // Start the async process to recognize the text
                RecognizeTextHeaders textHeaders =
                    await computerVision.RecognizeTextAsync(
                        imageUrl, textRecognitionMode);

                await GetTextAsync(computerVision, textHeaders.OperationLocation);
            }

            // Recognize text from a local image
            private static async Task ExtractLocalTextAsync(
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
                    // Start the async process to recognize the text
                    RecognizeTextInStreamHeaders textHeaders =
                        await computerVision.RecognizeTextInStreamAsync(
                            imageStream, textRecognitionMode);

                    await GetTextAsync(computerVision, textHeaders.OperationLocation);
                }
            }

            // Retrieve the recognized text
            private static async Task GetTextAsync(
                ComputerVisionClient computerVision, string operationLocation)
            {
                // Retrieve the URI where the recognized text will be
                // stored from the Operation-Location header
                string operationId = operationLocation.Substring(
                    operationLocation.Length - numberOfCharsInOperationId);

                Console.WriteLine("\nCalling GetHandwritingRecognitionOperationResultAsync()");
                TextOperationResult result =
                    await computerVision.GetTextOperationResultAsync(operationId);

                // Wait for the operation to complete
                int i = 0;
                int maxRetries = 10;
                while ((result.Status == TextOperationStatusCodes.Running ||
                        result.Status == TextOperationStatusCodes.NotStarted) && i++ < maxRetries)
                {
                    Console.WriteLine(
                        "Server status: {0}, waiting {1} seconds...", result.Status, i);
                    await Task.Delay(1000);

                    result = await computerVision.GetTextOperationResultAsync(operationId);
                }

                // Display the results
                Console.WriteLine();
                var lines = result.RecognitionResult.Lines;
                foreach (Line line in lines)
                {
                    Console.WriteLine(line.Text);
                }
                Console.WriteLine();
            }
        }
    }
    ```

1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Cambie `computerVision.Endpoint` por la región de Azure asociada con las claves de suscripción, si es necesario.
1. También puede establecer `textRecognitionMode` en `TextRecognitionMode.Printed`.
1. Reemplace `<LocalImage>` por la ruta de acceso y el nombre de archivo de una imagen local.
1. También tiene la opción de establecer `remoteImageUrl` en una imagen diferente.
1. Ejecute el programa.


## <a name="examine-the-response"></a>Examen de la respuesta

Una respuesta correcta imprime las líneas del texto reconocido en cada imagen.

```console
Calling GetHandwritingRecognitionOperationResultAsync()

Calling GetHandwritingRecognitionOperationResultAsync()
Server status: Running, waiting 1 seconds...
Server status: Running, waiting 1 seconds...

dog
The quick brown fox jumps over the lazy
Pack my box with five dozen liquor jugs
```

Consulte [Quickstart: Extracción de texto escrito a mano (REST, C#)](../QuickStarts/CSharp-hand-text.md#examine-the-response) para ver un ejemplo de la salida JSON sin procesar de la llamada API.

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API que se usan para analizar una imagen, detectar personajes y sitios emblemáticos, crear una miniatura y extraer texto impreso y escrito a mano.

> [!div class="nextstepaction"]
> [Exploración de las versiones de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
