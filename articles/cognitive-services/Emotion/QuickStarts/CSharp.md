---
title: 'Inicio rápido: Reconocimiento de emociones en las caras de una imagen (Emotion API, C#)'
titlesuffix: Azure Cognitive Services
description: Obtenga información y código de ejemplo para ayudarle a empezar a usar rápidamente Emotion API con C#.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 45282c4ad534930fa72a8d8fb71105dca4af23de
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55218247"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Inicio rápido: Creación de una aplicación para reconocer emociones en las caras de una imagen.

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/).

Este artículo proporciona información y un ejemplo de código que le ayuda a empezar a usar rápidamente el [método de reconocimiento de Emotion API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) con C#. Puede usarlo para reconocer las emociones expresadas por una o varias personas en una imagen.

## <a name="prerequisites"></a>Requisitos previos
* Obtenga el [Windows SDK de Emotion API](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) de Cognitive Services,
* Obtenga la [clave de suscripción](https://azure.microsoft.com/try/cognitive-services/) gratuita.

## <a name="emotion-recognition-c-example-request"></a>Solicitud de ejemplo de reconocimiento de emociones en C#

Cree una nueva solución de consola en Visual Studio y, a continuación, reemplace Program.cs por el código siguiente. Cambie `string uri` para usar la región donde obtuvo las claves de suscripción. Reemplace el valor **Ocp-Apim-Subscription-Key** por su clave de suscripción válida. Para encontrar la clave de suscripción, vaya a Azure Portal. En el panel de navegación de la izquierda, bajo la sección **Claves**, vaya al recurso Emotion API. De forma similar, puede obtener el URI de conexión adecuado en el panel **Información general** para su recurso que figura en **Punto de conexión**.

![Las claves de recurso de API](../../media/emotion-api/keys.png)

Para procesar la respuesta a la solicitud, utilice una biblioteca como `Newtonsoft.Json`. De esta forma puede controlar una cadena JSON como una serie de objetos administrables denominados tokens. Para agregar esta biblioteca al paquete, haga clic con el botón derecho en el proyecto en el Explorador de soluciones y elija **Administrar paquetes de NuGet**. A continuación, busque **Newtonsoft**. El primer resultado deber ser **Newtonsoft.Json**. Seleccione **Instalar**. Ahora puede hacer referencia a esta biblioteca en la aplicación.

![Instalación de Newtonsoft.Json](../../media/emotion-api/newtonsoft-nuget.png)

```csharp
using System;
using System.IO;
using System.Net.Http.Headers;
using System.Net.Http;
using Newtonsoft.Json.Linq;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            Console.Write("Enter the path to a JPEG image file:");
            string imageFilePath = Console.ReadLine();

            MakeRequest(imageFilePath);

            Console.WriteLine("\n\n\nWait for the result below, then hit ENTER to exit...\n\n\n");
            Console.ReadLine(); // wait for ENTER to exit program
        }

        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        static async void MakeRequest(string imageFilePath)
        {
            var client = new HttpClient();

            // Request headers - replace this example key with your valid key.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<your-subscription-key>"); //

            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
            //   URI below with "westcentralus".
            string uri = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?";
            HttpResponseMessage response;
            string responseContent;

            // Request body. Try this sample with a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (var content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                response = await client.PostAsync(uri, content);
                responseContent = response.Content.ReadAsStringAsync().Result;
            }

            // A peek at the raw JSON response.
            Console.WriteLine(responseContent);

            // Processing the JSON into manageable objects.
            JToken rootToken = JArray.Parse(responseContent).First;

            // First token is always the faceRectangle identified by the API.
            JToken faceRectangleToken = rootToken.First;

            // Second token is all emotion scores.
            JToken scoresToken = rootToken.Last;

            // Show all face rectangle dimensions
            JEnumerable<JToken> faceRectangleSizeList = faceRectangleToken.First.Children();
            foreach (var size in faceRectangleSizeList) {
                Console.WriteLine(size);
            }

            // Show all scores
            JEnumerable<JToken> scoreList = scoresToken.First.Children();
            foreach (var score in scoreList) {
                Console.WriteLine(score);
            }
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Respuesta de ejemplo de reconocimiento de emociones
Una llamada correcta devuelve una matriz de entradas de cara y sus puntuaciones de emoción asociadas. Se clasifican según el tamaño del rectángulo de la cara en orden descendente. Una respuesta vacía indica que no se ha detectado ninguna cara. Una entrada de emoción contiene los siguientes campos:

* faceRectangle: ubicación del rectángulo de la cara en la imagen
* scores: puntuaciones de emociones de cada cara de la imagen

```json
application/json
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
