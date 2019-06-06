---
title: 'Inicio rápido: Creación de un proyecto de detección de objetos con el SDK de Custom Vision para C#'
titlesuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y detecte objetos mediante el SDK para .NET con C#.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: 6f4b13e5fb2dc8ed595999bfc8a2abe4db15dcbb
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393901"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-net-sdk"></a>Inicio rápido: Creación de un proyecto de detección de objetos con el SDK de Custom Vision para .NET

En este artículo se proporciona información y código de ejemplo para ayudarle a empezar a utilizar el SDK de Custom Vision con C# para crear un modelo de detección de objetos. Después de crearlo, puede agregar regiones etiquetadas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Utilice este ejemplo como plantilla para crear su propia aplicación de .NET. 

## <a name="prerequisites"></a>Requisitos previos

- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Obtención del SDK de Custom Vision y código de ejemplo

Para escribir una aplicación de .NET que utiliza Custom Vision, necesitará los paquetes NuGet de Custom Vision, que están incluidos en el proyecto de ejemplo que descargará, pero puede acceder a ellos individualmente aquí.

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Clone o descargue el proyecto [Ejemplos de .NET de Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples). Vaya a la carpeta **CustomVision/ObjectDetection** y abra _ObjectDetection.csproj_ en Visual Studio.

Este proyecto de Visual Studio crea un nuevo proyecto de Custom Vision denominado __My New Project__, al que se puede acceder en el [sitio web de Custom Vision](https://customvision.ai/). A continuación, carga las imágenes para entrenar y probar un modelo de detección de objetos. En este proyecto, el modelo se entrena para detectar las tenedores y tijeras en las imágenes.

[!INCLUDE [get-keys](includes/get-keys.md)]

## <a name="understand-the-code"></a>Comprendiendo el código

Abra el archivo _Program.cs_ e inspeccione el código. Inserte las claves de la suscripción en las definiciones pertinentes del método **Main**.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=18-27)]

El parámetro de punto de conexión debe apuntar a la región donde se creó el grupo de recursos de Azure que contiene los recursos de Custom Vision. En este ejemplo se asume la región Centro-sur de EE. UU. y utilice:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=14-14)]

### <a name="create-a-new-custom-vision-service-project"></a>Creación de un proyecto de Custom Vision Service

El siguiente fragmento de código siguiente crea un proyecto de detección de objetos. El proyecto creado se mostrará en el [sitio web de Custom Vision](https://customvision.ai/) que ha visitado anteriormente. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=29-35)]

### <a name="add-tags-to-the-project"></a>Adición de etiquetas al proyecto

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=37-39)]

### <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

Cuando se etiquetan imágenes en los proyectos de detección de objetos, es preciso especificar la región de cada objeto etiquetado mediante coordenadas normalizadas. El código siguiente asocia cada una de las imágenes del ejemplo a su región etiquetada.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=41-84)]

Luego, esta asignación de asociaciones se usa para cargar cada imagen de ejemplo con sus coordenadas de región.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=86-104)]

En este momento, ya se han cargado todas las imágenes de ejemplo , y cada una de ellas tiene una etiqueta (**tenedor** o **tijeras**) y un rectángulo de píxeles asociado a dicha etiqueta.

### <a name="train-the-project"></a>Entrenamiento del proyecto

Este código crea la primera iteración de aprendizaje del proyecto.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=106-117)]

### <a name="publish-the-current-iteration"></a>Publicación de la iteración actual

El nombre que se da a la iteración publicada se puede utilizar para enviar solicitudes de predicción. Una iteración no está disponible en el punto de conexión de la predicción hasta que se publica.

```csharp
// The iteration is now trained. Publish it to the prediction end point.
var publishedModelName = "treeClassModel";
var predictionResourceId = "<target prediction resource ID>";
trainingApi.PublishIteration(project.Id, iteration.Id, publishedModelName, predictionResourceId);
Console.WriteLine("Done!\n");
```

### <a name="create-a-prediction-endpoint"></a>Creación de un punto de conexión de la predicción

```csharp
// Create a prediction endpoint, passing in the obtained prediction key
CustomVisionPredictionClient endpoint = new CustomVisionPredictionClient()
{
        ApiKey = predictionKey,
        Endpoint = SouthCentralUsEndpoint
};
```

### <a name="use-the-prediction-endpoint"></a>Uso del punto de conexión de la predicción

Esta parte del script carga la imagen de prueba, consulta el punto de conexión del modelo y envía los datos de la predicción a la consola.

```csharp
// Make a prediction against the new project
Console.WriteLine("Making a prediction:");
var imageFile = Path.Combine("Images", "test", "test_image.jpg");
using (var stream = File.OpenRead(imageFile))
{
        var result = endpoint.DetectImage(project.Id, publishedModelName, stream);

        // Loop over each prediction and write out the results
        foreach (var c in result.Predictions)
        {
                Console.WriteLine($"\t{c.TagName}: {c.Probability:P1} [ {c.BoundingBox.Left}, {c.BoundingBox.Top}, {c.BoundingBox.Width}, {c.BoundingBox.Height} ]");
        }
}
```

## <a name="run-the-application"></a>Ejecución de la aplicación

Cuando se ejecuta la aplicación, se debería abrir una ventana de consola y escribir la salida siguiente:

```
Creating new project:
        Training
Done!

Making a prediction:
        fork: 98.2% [ 0.111609578, 0.184719115, 0.6607002, 0.6637112 ]
        scissors: 1.2% [ 0.112389535, 0.119195729, 0.658031344, 0.7023591 ]
```

Luego puede comprobar que la imagen de prueba (que se encuentra en **Images/Test/** ) se ha etiquetado correctamente y que la región de detección es correcta. En este momento puede presionar cualquier tecla para salir de la aplicación.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Pasos siguientes

Ya ha visto cómo todos los pasos del proceso de detección de objetos se pueden realizar en código. En este ejemplo se ejecuta una sola iteración de entrenamiento, pero a menudo necesitará entrenar y probar su modelo varias veces para hacerlo más preciso. En la guía siguiente se trata la clasificación de imágenes, pero sus principios son similares a los de la detección de objetos.

> [!div class="nextstepaction"]
> [Prueba y reentrenamiento del modelo](test-your-model.md)
