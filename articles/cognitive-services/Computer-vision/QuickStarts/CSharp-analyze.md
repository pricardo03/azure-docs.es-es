---
title: 'Inicio rápido: Análisis de imágenes locales (REST, C#)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, analizará una imagen local mediante Computer Vision API con C#.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 4de419abfa72f9423111ebc0a8ba5a0d2e8a96e7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55171820"
---
# <a name="quickstart-analyze-a-local-image-using-the-rest-api-and-c35-in-computer-vision"></a>Inicio rápido: Análisis de imágenes locales mediante la API REST y C# en Computer Vision

En esta guía de inicio rápido, analizará una imagen almacenada localmente para extraer características visuales con la API de REST Computer Vision. Con el [método de análisis de imagen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), puede extraer características visuales basadas en el contenido de una imagen.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

- Debe tener [Visual Studio 2015](https://visualstudio.microsoft.com/downloads/) o posterior.
- Debe tener una clave de suscripción para Computer Vision. Para obtener una clave de suscripción, consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample-application"></a>Creación y ejecución de la aplicación de ejemplo

Para crear el ejemplo en Visual Studio, siga estos pasos:

1. Cree una solución de Visual Studio en Visual Studio, mediante la plantilla de aplicación de consola de Visual C#.
1. Instale el paquete NuGet Newtonsoft.Json.
    1. En el menú, haga clic en **Herramientas**, seleccione **Administrador de paquetes NuGet** y, luego, **Manage NuGet Packages for Solution** (Administrar paquetes NuGet para la solución).
    1. Haga clic en la pestaña **Examinar** y, en el cuadro **Buscar**, escriba "Newtonsoft.Json".
    1. Seleccione **Newtonsoft.Json** cuando se muestre, marque la casilla junto al nombre del proyecto y haga clic en **Instalar**.
1. Reemplace el código de `Program.cs` por el código siguiente y, a continuación, realice los cambios siguientes en el código cuando sea necesario:
    1. Reemplace el valor de `subscriptionKey` por la clave de suscripción.
    1. Reemplace el valor de `uriBase` por la dirección URL del punto de conexión para el método [Analizar imagen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) desde la región de Azure donde obtuvo las claves de suscripción, si es necesario.
1. Ejecute el programa.
1. En el símbolo del sistema, escriba la ruta de acceso a una imagen local.

```csharp
using Newtonsoft.Json.Linq;
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;

namespace CSHttpClientSample
{
    static class Program
    {
        // Replace <Subscription Key> with your valid subscription key.
        const string subscriptionKey = "<Subscription Key>";

        // You must use the same Azure region in your REST API method as you used to
        // get your subscription keys. For example, if you got your subscription keys
        // from the West US region, replace "westcentralus" in the URL
        // below with "westus".
        //
        // Free trial subscription keys are generated in the "westus" region.
        // If you use a free trial subscription key, you shouldn't need to change
        // this region.
        const string uriBase =
            "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze";

        static void Main()
        {
            // Get the path and filename to process from the user.
            Console.WriteLine("Analyze an image:");
            Console.Write(
                "Enter the path to the image you wish to analyze: ");
            string imageFilePath = Console.ReadLine();

            if (File.Exists(imageFilePath))
            {
                // Call the REST API method.
                Console.WriteLine("\nWait a moment for the results to appear.\n");
                MakeAnalysisRequest(imageFilePath).Wait();
            }
            else
            {
                Console.WriteLine("\nInvalid file path");
            }
            Console.WriteLine("\nPress Enter to exit...");
            Console.ReadLine();
        }

        /// <summary>
        /// Gets the analysis of the specified image file by using
        /// the Computer Vision REST API.
        /// </summary>
        /// <param name="imageFilePath">The image file to analyze.</param>
        static async Task MakeAnalysisRequest(string imageFilePath)
        {
            try
            {
                HttpClient client = new HttpClient();

                // Request headers.
                client.DefaultRequestHeaders.Add(
                    "Ocp-Apim-Subscription-Key", subscriptionKey);

                // Request parameters. A third optional parameter is "details".
                // The Analyze Image method returns information about the following
                // visual features:
                // Categories:  categorizes image content according to a
                //              taxonomy defined in documentation.
                // Description: describes the image content with a complete
                //              sentence in supported languages.
                // Color:       determines the accent color, dominant color, 
                //              and whether an image is black & white.
                string requestParameters =
                    "visualFeatures=Categories,Description,Color";

                // Assemble the URI for the REST API method.
                string uri = uriBase + "?" + requestParameters;

                HttpResponseMessage response;

                // Read the contents of the specified local image
                // into a byte array.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                // Add the byte array as an octet stream to the request body.
                using (ByteArrayContent content = new ByteArrayContent(byteData))
                {
                    // This example uses the "application/octet-stream" content type.
                    // The other content types you can use are "application/json"
                    // and "multipart/form-data".
                    content.Headers.ContentType =
                        new MediaTypeHeaderValue("application/octet-stream");

                    // Asynchronously call the REST API method.
                    response = await client.PostAsync(uri, content);
                }

                // Asynchronously get the JSON response.
                string contentString = await response.Content.ReadAsStringAsync();

                // Display the JSON response.
                Console.WriteLine("\nResponse:\n\n{0}\n",
                    JToken.Parse(contentString).ToString());
            }
            catch (Exception e)
            {
                Console.WriteLine("\n" + e.Message);
            }
        }

        /// <summary>
        /// Returns the contents of the specified file as a byte array.
        /// </summary>
        /// <param name="imageFilePath">The image file to read.</param>
        /// <returns>The byte array of the image data.</returns>
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            // Open a read-only file stream for the specified file.
            using (FileStream fileStream =
                new FileStream(imageFilePath, FileMode.Open, FileAccess.Read))
            {
                // Read the file's contents into a byte array.
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
}
```

## <a name="examine-the-response"></a>Examen de la respuesta

Se devuelve una respuesta correcta en JSON. La aplicación de ejemplo analiza y muestra una respuesta correcta en la ventana de la consola, parecida a la del ejemplo siguiente:

```json
{
    "categories": [
        {
            "name": "abstract_",
            "score": 0.00390625
        },
        {
            "name": "others_",
            "score": 0.0234375
        },
        {
            "name": "outdoor_",
            "score": 0.00390625
        }
    ],
    "description": {
        "tags": [
            "road",
            "building",
            "outdoor",
            "street",
            "night",
            "black",
            "city",
            "white",
            "light",
            "sitting",
            "riding",
            "man",
            "side",
            "empty",
            "rain",
            "corner",
            "traffic",
            "lit",
            "hydrant",
            "stop",
            "board",
            "parked",
            "bus",
            "tall"
        ],
        "captions": [
            {
                "text": "a close up of an empty city street at night",
                "confidence": 0.7965622853462756
            }
        ]
    },
    "requestId": "dddf1ac9-7e66-4c47-bdef-222f3fe5aa23",
    "metadata": {
        "width": 3733,
        "height": 1986,
        "format": "Jpeg"
    },
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": [
            "Black",
            "Grey"
        ],
        "accentColor": "666666",
        "isBWImg": true
    }
}
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no la necesite, elimine la solución de Visual Studio. Para ello, abra el Explorador de archivos, vaya a la carpeta en la que creó la solución de Visual Studio y elimine la carpeta.

## <a name="next-steps"></a>Pasos siguientes

Explore una aplicación básica de Windows que usa Computer Vision para realizar el reconocimiento óptico de caracteres (OCR), crear miniaturas con recorte inteligente, y detectar, clasificar, etiquetar y describir características visuales, como caras, en una imagen. Para experimentar rápidamente con las versiones de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Tutorial de Computer Vision API para C#](../Tutorials/CSharpTutorial.md)
