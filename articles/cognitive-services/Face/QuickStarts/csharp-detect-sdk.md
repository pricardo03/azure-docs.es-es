---
title: 'Guía de inicio rápido: Detección de caras en una imagen - SDK, C#'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se detectan las caras de una imagen mediante la biblioteca cliente Face de Windows con C# en Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: nolachar
ms.openlocfilehash: a9a7efd89f8e7462812064615d07acf12acbc3a3
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364130"
---
# <a name="quickstart-detect-faces-in-an-image-using-c35---face"></a>Guía de inicio rápido: Detección de caras en una imagen con C# - Face

En esta guía de inicio rápido se detectan las caras de personas en una imagen mediante la biblioteca cliente Face de Windows.

El código fuente del ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face).

## <a name="prerequisites"></a>Requisitos previos

* Necesita una clave de suscripción para ejecutar el ejemplo. Puede obtener las claves de la suscripción de evaluación gratuita en la página de [Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
* Cualquier edición de [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* El paquete NuGet de la biblioteca cliente [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview). No es necesario descargar el paquete. A continuación, se proporcionan instrucciones de instalación.

## <a name="detectwithurlasync-method"></a>Método DetectWithUrlAsync

Los métodos `DetectWithUrlAsync` y `DetectWithStreamAsync` encapsulan [Face - Detect API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para las imágenes remotas y locales, respectivamente. Puede usar estos métodos para detectar las caras en una imagen y devolver atributos faciales, como:

* Id. de rostro: identificador único que se usa en varios escenarios de Face API.
* Rectángulo facial: las indicaciones de ubicación (izquierda, parte superior, ancho y alto) del rostro en la imagen.
* Puntos de referencia: matriz de 27 puntos faciales de referencia de las posiciones importantes de los componentes del rostro.
* Atributos faciales como la edad, el sexo, la intensidad de la sonrisa, la postura de la cabeza y el vello facial.

Para ejecutar el ejemplo, siga estos pasos:

1. Cree una aplicación de consola de Visual C# en Visual Studio.
1. Instale el paquete NuGet de la biblioteca cliente de Face.
    1. En el menú superior, haga clic en **Herramientas**, seleccione **Administrador de paquetes NuGet** y, luego, **Manage NuGet Packages for Solution** (Administrar paquetes NuGet para la solución).
    1. Haga clic en la pestaña **Examinar** y luego seleccione **Incluir versión previa**.
    1. En el cuadro **Buscar**, escriba "Microsoft.Azure.CognitiveServices.Vision.Face".
    1. Seleccione **Microsoft.Azure.CognitiveServices.Vision.Face** cuando aparezca, marque la casilla junto al nombre del proyecto y haga clic en **Instalar**.
1. Sustituya *Program.cs* por el código siguiente.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Cambie `faceEndpoint` por la región de Azure asociada con las claves de suscripción, si es necesario.
1. Tiene la opción de reemplazar <`LocalImage>` por la ruta de acceso y el nombre de archivo de una imagen local (si no se establece, se omite).
1. También tiene la opción de establecer `remoteImageUrl` en una imagen diferente.
1. Ejecute el programa.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace DetectFace
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // You must use the same region as you used to get your subscription
        // keys. For example, if you got your subscription keys from westus,
        // replace "westcentralus" with "westus".
        //
        // Free trial subscription keys are generated in the westcentralus
        // region. If you use a free trial subscription key, you shouldn't
        // need to change the region.
        // Specify the Azure region
        private const string faceEndpoint =
            "https://westcentralus.api.cognitive.microsoft.com";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg";

        private static readonly FaceAttributeType[] faceAttributes =
            { FaceAttributeType.Age, FaceAttributeType.Gender };

        static void Main(string[] args)
        {
            FaceClient faceClient = new FaceClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });
            faceClient.Endpoint = faceEndpoint;

            Console.WriteLine("Faces being detected ...");
            var t1 = DetectRemoteAsync(faceClient, remoteImageUrl);
            var t2 = DetectLocalAsync(faceClient, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Detect faces in a remote image
        private static async Task DetectRemoteAsync(
            FaceClient faceClient, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine("\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            try
            {
                IList<DetectedFace> faceList =
                    await faceClient.Face.DetectWithUrlAsync(
                        imageUrl, true, false, faceAttributes);

                DisplayAttributes(GetFaceAttributes(faceList, imageUrl), imageUrl);
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imageUrl + ": " + e.Message);
            }
        }

        // Detect faces in a local image
        private static async Task DetectLocalAsync(FaceClient faceClient, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            try
            {
                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    IList<DetectedFace> faceList =
                            await faceClient.Face.DetectWithStreamAsync(
                                imageStream, true, false, faceAttributes);
                    DisplayAttributes(
                        GetFaceAttributes(faceList, imagePath), imagePath);
                }
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imagePath + ": " + e.Message);
            }
        }

        private static string GetFaceAttributes(
            IList<DetectedFace> faceList, string imagePath)
        {
            string attributes = string.Empty;

            foreach (DetectedFace face in faceList)
            {
                double? age = face.FaceAttributes.Age;
                string gender = face.FaceAttributes.Gender.ToString();
                attributes += gender + " " + age + "   ";
            }

            return attributes;
        }

        // Display the face attributes
        private static void DisplayAttributes(string attributes, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(attributes + "\n");
        }
    }
}
```

### <a name="detectwithurlasync-response"></a>Respuesta DetectWithUrlAsync

Una respuesta correcta muestra el sexo y la edad de cada cara de la imagen.

Consulte las [Guías de inicio rápido de API: Detectar caras en una imagen con C#](CSharp.md) para ver un ejemplo de la salida JSON sin procesar.

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo crear una aplicación de Windows WPF que usa el servicio Face para detectar caras en una imagen. La aplicación dibuja un marco alrededor de cada cara y muestra una descripción de la cara en la barra de estado.

> [!div class="nextstepaction"]
> [Tutorial: Creación de una aplicación WPF para detectar y enmarcar caras en una imagen](../Tutorials/FaceAPIinCSharpTutorial.md)
