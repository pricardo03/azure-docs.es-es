---
title: 'Inicio rápido: Generación de una miniatura (SDK, C#)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, generará una miniatura de una imagen mediante la biblioteca cliente C# para Windows de Computer Vision.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 30bc45e0c32fdb0cf06d8eb5b0cb7445e693d17d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55209509"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-sdk-and-c"></a>Inicio rápido: Generación de una miniatura mediante el SDK de Computer Vision y C#

En esta guía de inicio rápido, generará una miniatura de una imagen mediante la biblioteca cliente para Windows de Computer Vision.

## <a name="prerequisites"></a>Requisitos previos

* Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).
* Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).
* El paquete NuGet de la biblioteca cliente [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision). No es necesario descargar el paquete. A continuación, se proporcionan instrucciones de instalación.

## <a name="generatethumbnailasync-method"></a>Método GenerateThumbnailAsync

> [!TIP]
> Obtenga el código más reciente como solución de Visual Studio desde [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision).

Los métodos `GenerateThumbnailAsync` y `GenerateThumbnailInStreamAsync` encapsulan [Get Thumbnail API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) para las imágenes locales y remotas, respectivamente.  Puede usar estos métodos para generar una miniatura de una imagen. Debe especificar el alto y el ancho, que pueden ser diferentes a la relación de aspecto de la imagen de entrada. Computer Vision usa el recorte inteligente para identificar el área de interés de forma inteligente y generar coordenadas de recorte en función de esa región.

Para ejecutar el ejemplo, siga estos pasos:

1. Cree una aplicación de consola de Visual C# en Visual Studio.
1. Instale el paquete NuGet de la biblioteca cliente de Computer Vision.
    1. En el menú, haga clic en **Herramientas**, seleccione **Administrador de paquetes NuGet** y, luego, **Manage NuGet Packages for Solution** (Administrar paquetes NuGet para la solución).
    1. Haga clic en la pestaña **Examinar** y, en el cuadro **Buscar**, escriba "Microsoft.Azure.CognitiveServices.Vision.ComputerVision".
    1. Seleccione **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** cuando aparezca, marque la casilla junto al nombre del proyecto y haga clic en **Instalar**.
1. Reemplace `Program.cs` por el código siguiente.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Cambie `computerVision.Endpoint` por la región de Azure asociada con las claves de suscripción, si es necesario.
1. Tiene la opción de reemplazar `<LocalImage>` por la ruta de acceso y el nombre de archivo de una imagen local (si no se establece, se omite).
1. También tiene la opción de establecer `remoteImageUrl` en una imagen diferente.
1. Y puede establecer `writeThumbnailToDisk` en `true` de manera opcional para guardar la miniatura en el disco.
1. Ejecute el programa.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;

using System;
using System.IO;
using System.Threading.Tasks;

namespace ImageThumbnail
{
    class Program
    {
        private const bool writeThumbnailToDisk = false;

        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg";

        private const int thumbnailWidth = 100;
        private const int thumbnailHeight = 100;

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

            Console.WriteLine("Images being analyzed ...\n");
            var t1 = GetRemoteThumbnailAsync(computerVision, remoteImageUrl);
            var t2 = GetLocalThumbnailAsnc(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press ENTER to exit");
            Console.ReadLine();
        }

        // Create a thumbnail from a remote image
        private static async Task GetRemoteThumbnailAsync(
            ComputerVisionClient computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            Stream thumbnail = await computerVision.GenerateThumbnailAsync(
                thumbnailWidth, thumbnailHeight, imageUrl, true);

            string path = Environment.CurrentDirectory;
            string imageName = imageUrl.Substring(imageUrl.LastIndexOf('/') + 1);
            string thumbnailFilePath =
                path + "\\" + imageName.Insert(imageName.Length - 4, "_thumb");

            // Save the thumbnail to the current working directory,
            // using the original name with the suffix "_thumb".
            SaveThumbnail(thumbnail, thumbnailFilePath);
        }

        // Create a thumbnail from a local image
        private static async Task GetLocalThumbnailAsnc(
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
                Stream thumbnail = await computerVision.GenerateThumbnailInStreamAsync(
                    thumbnailWidth, thumbnailHeight, imageStream, true);

                string thumbnailFilePath =
                    localImagePath.Insert(localImagePath.Length - 4, "_thumb");

                // Save the thumbnail to the same folder as the original image,
                // using the original name with the suffix "_thumb".
                SaveThumbnail(thumbnail, thumbnailFilePath);
            }
        }

        // Save the thumbnail locally.
        // NOTE: This will overwrite an existing file of the same name.
        private static void SaveThumbnail(Stream thumbnail, string thumbnailFilePath)
        {
            if (writeThumbnailToDisk)
            {
                using (Stream file = File.Create(thumbnailFilePath))
                {
                    thumbnail.CopyTo(file);
                }
            }
            Console.WriteLine("Thumbnail {0} written to: {1}\n",
                writeThumbnailToDisk ? "" : "NOT", thumbnailFilePath);
        }
    }
}
```

## <a name="generatethumbnailasync-response"></a>Respuesta de GenerateThumbnailAsync

Una respuesta correcta guarda la miniatura de cada imagen localmente y muestra la ubicación de la miniatura, por ejemplo:

```cmd
Thumbnail written to: C:\Documents\LocalImage_thumb.jpg

Thumbnail written to: ...\bin\Debug\Bloodhound_Puppy_thumb.jpg
```

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API que se usan para analizar una imagen, detectar personajes y sitios emblemáticos, crear una miniatura y extraer texto impreso y escrito a mano.

> [!div class="nextstepaction"]
> [Exploración de las versiones de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
