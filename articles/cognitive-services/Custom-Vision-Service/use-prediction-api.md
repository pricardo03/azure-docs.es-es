---
title: 'Uso del punto de conexión de predicción de Custom Vision Service: Azure Cognitive Services | Microsoft Docs'
description: Obtenga información sobre cómo usar la API para probar las imágenes mediante programación con el clasificador de Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 54f9d9fec1f40c167341dec6a8699b6a558419da
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381294"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Uso del punto de conexión de predicción para probar imágenes mediante programación con un clasificación de Custom Vision Service

Después de entrenar el modelo, puede probar imágenes mediante programación enviándolas a Prediction API. 

> [!NOTE]
> En este documento se explica cómo usar C# para enviar una imagen a Prediction API. Para más información y ejemplos de uso de la API, consulte la [referencia de Prediction API](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>Obtención de una clave de predicción y dirección URL

En la [página web de Custom Vision](https://customvision.ai), seleccione el proyecto y luego seleccione la pestaña __Rendimiento__. Para mostrar información sobre el uso de Prediction API, seleccione __Prediction URL__. Copie la siguiente información para usarla en la aplicación:

* __Dirección URL__ para usar un __archivo de imagen__.
* Valor de la __Prediction-key__.

> [!TIP]
> Si tiene varias iteraciones, puede controlar cuál se usa al configurarla como predeterminada. Seleccione la iteración en la sección __Iteraciones__ y luego seleccione __Establecer como predeterminada__ en la parte superior de la página.

![Se muestra la pestaña de rendimiento con un rectángulo en rojo que rodea la dirección URL de Prediction.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>Creación de la aplicación

1. En Visual Studio, cree una aplicación de consola en C#.

2. Use el siguiente código como cuerpo del archivo __Program.cs__.

    > [!IMPORTANT]
    > Cambie la siguiente información:
    >
    > * Establezca el __espacio de nombres__ con el nombre del proyecto.
    > * Establezca el valor __Prediction-Key__ que recibió anteriormente en la línea que empieza con `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Establezca el valor __URL__ que recibió anteriormente en la línea que empieza con `string url =`.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CSPredictionSample
    {
        static class Program
        {
            static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }

            static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid subscription key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

                // Prediction URL - replace this example URL with your valid prediction URL.
                string url = "http://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/prediction/d16e136c-5b0b-4b84-9341-6a3fff8fa7fe/image?iterationId=f4e573f6-9843-46db-8018-b01d034fd0f2";

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
        }
    }
    ```

## <a name="use-the-application"></a>Uso de la aplicación

Cuando se ejecuta la aplicación, escriba la ruta de acceso a un archivo de imagen. La imagen se envía a la API y los resultados se devuelven como un documento JSON. El documento JSON siguiente es un ejemplo de la respuesta:

```json
{
    "Id":"3f76364c-b8ae-4818-a2b2-2794cfbe377a",
    "Project":"2277aca4-7aff-4742-8afb-3682e251c913",
    "Iteration":"84105bfe-73b5-4fcc-addb-756c0de17df2",
    "Created":"2018-05-03T14:15:22.5659829Z",
    "Predictions":[
        {"TagId":"35ac2ad0-e3ef-4e60-b81f-052a1057a1ca","Tag":"dog","Probability":0.102716163},
        {"TagId":"28e1a872-3776-434c-8cf0-b612dd1a953c","Tag":"cat","Probability":0.02037274}
    ]
}
```

## <a name="next-steps"></a>Pasos siguientes

[Exportación del modelo para su uso con dispositivos móviles](export-your-model.md)