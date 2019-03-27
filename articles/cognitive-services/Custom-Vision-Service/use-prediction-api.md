---
title: 'Ejemplo: Uso del punto de conexión de predicción para probar imágenes mediante programación con un clasificador (Custom Vision)'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar la API para probar las imágenes mediante programación con el clasificador de Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: article
ms.date: 03/26/2019
ms.author: anroth
ms.openlocfilehash: 715fa526c83608c9922315e3a0d89b67b31e0d16
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472734"
---
#  <a name="use-your-model-with-the-prediction-api"></a>Utilice el modelo con la API de predicción

Después de entrenar el modelo, puede probar imágenes mediante programación enviándolas a Prediction API.

> [!NOTE]
> En este documento se explica cómo usar C# para enviar una imagen a Prediction API. Para más información y ejemplos de uso de la API, consulte la [referencia de Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15).

## <a name="publish-your-trained-iteration"></a>Publicar la iteración entrenada

En la [página web de Custom Vision](https://customvision.ai), seleccione el proyecto y luego seleccione la pestaña __Rendimiento__.

Para enviar las imágenes a la API de predicción, primero debe publicar la iteración para la predicción, que se puede realizar seleccionando __publicar__ y especificar un nombre para la iteración publicada. Esto permitirá que se puede tener acceso a la API de predicción de los recursos de Azure de Custom Vision el modelo. 

![Se muestra la ficha rendimiento, con un rectángulo rojo que rodea el botón Publicar.](./media/use-prediction-api/unpublished-iteration.png)

Una vez que el modelo se ha publicado correctamente, aparecerá una etiqueta "Publicado" junto a la iteración en la barra lateral izquierda, así como el nombre de la iteración publicado en la descripción de la iteración.

![Se muestra la ficha rendimiento, con un rectángulo rojo que rodean a la etiqueta publicada y el nombre de la iteración publicada.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>Obtención de una clave de predicción y dirección URL

Una vez que se ha publicado el modelo, puede recuperar información sobre el uso de la API de predicción seleccionando __predicción URL__. Se abrirá un cuadro de diálogo como el que se muestra a continuación con información sobre el uso de la API de predicción, incluidos el __dirección URL de la predicción__ y __predicción clave__.

![La ficha rendimiento se muestra con un rectángulo rojo alrededor del botón de la dirección URL de la predicción.](./media/use-prediction-api/published-iteration-prediction-url.png)

![La ficha rendimiento se muestra con un rectángulo rojo que rodea el valor de dirección URL de predicción para el uso de un archivo de imagen y el valor de clave de la predicción.](./media/use-prediction-api/prediction-api-info.png)

> [!TIP]
> Su __predicción clave__ también puede encontrarse en el [Portal de Azure](https://portal.azure.com) para el recurso de Azure Custom Vision asociado a su proyecto, en la página __claves__. 

En el cuadro de diálogo, copie la siguiente información para su uso en la aplicación:

* __Dirección URL de la predicción__ para usar un __archivo de imagen__.
* __Clave de la predicción__ valor.

## <a name="create-the-application"></a>Creación de la aplicación

1. En Visual Studio, cree una aplicación de consola en C#.

1. Use el siguiente código como cuerpo del archivo __Program.cs__.

    > [!IMPORTANT]
    > Cambie la siguiente información:
    >
    > * Establezca el __espacio de nombres__ con el nombre del proyecto.
    > * Establecer el __predicción clave__ que recuperó anteriormente en la línea que comienza con el valor `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Establecer el __dirección URL de la predicción__ que recuperó anteriormente en la línea que comienza con el valor `string url =`.

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
                client.DefaultRequestHeaders.Add("Prediction-Key", "3b9dde6d1ae1453a86bfeb1d945300f2");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "https://southcentralus.api.cognitive.microsoft.com/customvision/v3.0/Prediction/8622c779-471c-4b6e-842c-67a11deffd7b/classify/iterations/Cats%20vs.%20Dogs%20-%20Published%20Iteration%203/image";

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

## <a name="use-the-application"></a>Uso de la aplicación

Al ejecutar la aplicación, debe proporcionar la ruta de acceso a un archivo de imagen en la consola. La imagen se envía a la API de predicción y se devuelven los resultados de predicción como un documento JSON. El siguiente JSON es un ejemplo de la respuesta.

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

[Exportación del modelo para su uso con dispositivos móviles](export-your-model.md)

[Empezar a trabajar con el SDK de .NET](csharp-tutorial.md)

[Empezar a trabajar con los SDK de Python](python-tutorial.md)

[Empezar a trabajar con el SDK de Java](java-tutorial.md)

[Empezar a trabajar con los SDK de nodo](node-tutorial.md)

[Empezar a trabajar con los SDK de Go](go-tutorial.md)
