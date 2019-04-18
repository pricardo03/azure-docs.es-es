---
title: Uso del punto de conexión de predicción para probar imágenes mediante programación con un clasificador (Custom Vision)
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar la API para probar las imágenes mediante programación con el clasificador de Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: article
ms.date: 04/02/2019
ms.author: anroth
ms.openlocfilehash: 1ee6edbf49bbcd2014afcf29ed3b737168a3b5bc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59046077"
---
# <a name="use-your-model-with-the-prediction-api"></a>Utilice el modelo con la API de predicción

Una vez haya entrenar el modelo, puede probar mediante programación las imágenes enviando el punto de conexión de API de predicción.

> [!NOTE]
> En este documento se explica cómo usar C# para enviar una imagen a Prediction API. Para obtener más información y ejemplos, vea el [referencia de API de predicción](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15).

## <a name="publish-your-trained-iteration"></a>Publicar la iteración entrenada

En la [página web de Custom Vision](https://customvision.ai), seleccione el proyecto y luego seleccione la pestaña __Rendimiento__.

Para enviar las imágenes a la API de predicción, primero debe publicar la iteración para la predicción, que se puede realizar seleccionando __publicar__ y especificar un nombre para la iteración publicada. Esto hará que el modelo puede tener acceso a la API de predicción de los recursos de Azure de visión personalizada.

![Se muestra la ficha rendimiento, con un rectángulo rojo que rodea el botón Publicar.](./media/use-prediction-api/unpublished-iteration.png)

Una vez que el modelo se ha publicado correctamente, verá una etiqueta "Publicado" aparecen junto a la iteración en la barra lateral izquierda, y su nombre aparecerá en la descripción de la iteración.

![Se muestra la ficha rendimiento, con un rectángulo rojo que rodean a la etiqueta publicada y el nombre de la iteración publicada.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>Obtención de una clave de predicción y dirección URL

Una vez que se ha publicado el modelo, puede recuperar la información necesaria mediante la selección __predicción URL__. Se abrirá un cuadro de diálogo con información sobre el uso de la API de predicción, incluidos el __dirección URL de la predicción__ y __predicción clave__.

![La ficha rendimiento se muestra con un rectángulo rojo alrededor del botón de la dirección URL de la predicción.](./media/use-prediction-api/published-iteration-prediction-url.png)

![La ficha rendimiento se muestra con un rectángulo rojo que rodea el valor de dirección URL de predicción para el uso de un archivo de imagen y el valor de clave de la predicción.](./media/use-prediction-api/prediction-api-info.png)

> [!TIP]
> Su __predicción clave__ también puede encontrarse en el [portal de Azure](https://portal.azure.com) página para el recurso de Azure Custom Vision asociado con el proyecto, en el __claves__ hoja.

En esta guía, se utilizará una imagen local, por lo que se copie la dirección URL en **si tiene un archivo de imagen** en una ubicación temporal. Copie el correspondiente __predicción clave__ también el valor.

## <a name="create-the-application"></a>Creación de la aplicación

1. En Visual Studio, cree un nuevo C# aplicación de consola.

1. Use el siguiente código como cuerpo del archivo __Program.cs__.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CVSPredictionSample
    {
        public static class Program
        {
            public static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            public static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid Prediction-Key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "<Your prediction key>");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "<Your prediction URL>";

                HttpResponseMessage response;

                // Request body. Try this sample with a locally stored image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (var content = new ByteArrayContent(byteData))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                    response = await client.PostAsync(url, content);
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }

            private static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
    ```

1. Cambie la siguiente información:
   * Establecer el `namespace` campo para el nombre del proyecto.
   * Reemplace el marcador de posición `<Your prediction key>` con el valor de clave que recuperó anteriormente.
   * Reemplace el marcador de posición `<Your prediction URL>` con la dirección URL que recuperó anteriormente.

## <a name="run-the-application"></a>Ejecución de la aplicación

Al ejecutar la aplicación, deberá especificar una ruta de acceso a un archivo de imagen en la consola. La imagen, a continuación, se envía a la API de predicción y se devuelven los resultados de predicción como una cadena con formato JSON. El siguiente es un ejemplo de respuesta.

```json
{
    "Id":"7796df8e-acbc-45fc-90b4-1b0c81b73639",
    "Project":"8622c779-471c-4b6e-842c-67a11deffd7b",
    "Iteration":"59ec199d-f3fb-443a-b708-4bca79e1b7f7",
    "Created":"2019-03-20T16:47:31.322Z",
    "Predictions":[
        {"TagId":"d9cb3fa5-1ff3-4e98-8d47-2ef42d7fb373","TagName":"cat", "Probability":1.0},
        {"TagId":"9a8d63fb-b6ed-4462-bcff-77ff72084d99","TagName":"dog", "Probability":0.1087869}
    ]
}
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido a enviar las imágenes en su custom/detector de clasificador de imágenes y recibirán una respuesta mediante programación con el C# SDK. A continuación, obtenga información sobre cómo completar escenarios de extremo a otro con C#, o empezar a usar un idioma diferente de SDK.

* [Inicio rápido: .NET SDK](csharp-tutorial.md)
* [Inicio rápido: SDK de Python](python-tutorial.md)
* [Inicio rápido: Java SDK](java-tutorial.md)
* [Inicio rápido: SDK de Node](node-tutorial.md)
* [Inicio rápido: SDK de go](go-tutorial.md)
