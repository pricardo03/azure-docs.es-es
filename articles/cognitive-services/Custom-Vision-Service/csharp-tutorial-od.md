---
title: 'Inicio rápido: Creación de un proyecto de detección de objetos con el SDK de Custom Vision para C#'
titleSuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y detecte objetos mediante el SDK para .NET con C#.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: areddish
ms.openlocfilehash: de8858b1b47e36a4d8b86f317f7471118fa4a772
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "73519488"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-net-sdk"></a>Inicio rápido: Creación de un proyecto de detección de objetos con el SDK de Custom Vision para .NET

En este artículo se proporciona información y código de ejemplo para ayudarle a empezar a utilizar el SDK de Custom Vision con C# para crear un modelo de detección de objetos. Después de crearlo, puede agregar regiones etiquetadas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Utilice este ejemplo como plantilla para crear su propia aplicación de .NET. 

## <a name="prerequisites"></a>Requisitos previos

- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Obtención del SDK de Custom Vision y código de ejemplo

Para escribir una aplicación de .NET que utiliza Custom Vision, necesitará los paquetes NuGet de Custom Vision, que están incluidos en el proyecto de ejemplo que descargará, pero puede acceder a ellos individualmente aquí.

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Clone o descargue el proyecto [Ejemplos de .NET de Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples). Vaya a la carpeta **CustomVision/ObjectDetection** y abra _ObjectDetection.csproj_ en Visual Studio.

Este proyecto de Visual Studio crea un nuevo proyecto de Custom Vision denominado __My New Project__, al que se puede acceder en el [sitio web de Custom Vision](https://customvision.ai/). A continuación, carga las imágenes para entrenar y probar un modelo de detección de objetos. En este proyecto, el modelo se entrena para detectar las tenedores y tijeras en las imágenes.

[!INCLUDE [get-keys](includes/get-keys.md)]

## <a name="understand-the-code"></a>Comprendiendo el código

Abra el archivo _Program.cs_ e inspeccione el código. [Cree variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para las claves de entrenamiento y predicción denominadas `CUSTOM_VISION_TRAINING_KEY` y `CUSTOM_VISION_PREDICTION_KEY`, respectivamente. El script las buscará.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_keys)]

Además, obtenga la dirección URL del punto de conexión de la página de configuración del sitio web de Custom Vision. Guárdela en una variable de entorno llamada `CUSTOM_VISION_ENDPOINT`. El script guarda una referencia a ella en la raíz de la clase.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_endpoint)]

### <a name="create-a-new-custom-vision-service-project"></a>Creación de un proyecto de Custom Vision Service

El siguiente fragmento de código siguiente crea un proyecto de detección de objetos. El proyecto creado se mostrará en el [sitio web de Custom Vision](https://customvision.ai/) que ha visitado anteriormente. Consulte el método [CreateProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__) para especificar otras opciones al crear el proyecto (explicadas en la guía del portal web [Creación de un detector](get-started-build-detector.md)).  

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_create)]


### <a name="add-tags-to-the-project"></a>Adición de etiquetas al proyecto

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_tags)]

### <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

Cuando se etiquetan imágenes en los proyectos de detección de objetos, es preciso especificar la región de cada objeto etiquetado mediante coordenadas normalizadas. El código siguiente asocia cada una de las imágenes del ejemplo a su región etiquetada.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload_regions)]

Luego, esta asignación de asociaciones se usa para cargar cada imagen de ejemplo con sus coordenadas de región. Puede cargar hasta 64 imágenes en un único lote.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload)]

En este momento, ya se han cargado todas las imágenes de ejemplo , y cada una de ellas tiene una etiqueta (**tenedor** o **tijeras**) y un rectángulo de píxeles asociado a dicha etiqueta.

### <a name="train-the-project"></a>Entrenamiento del proyecto

Este código crea la primera iteración de aprendizaje del proyecto.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_train)]

### <a name="publish-the-current-iteration"></a>Publicación de la iteración actual

El nombre que se da a la iteración publicada se puede utilizar para enviar solicitudes de predicción. Una iteración no está disponible en el punto de conexión de la predicción hasta que se publica.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_publish)]

### <a name="create-a-prediction-endpoint"></a>Creación de un punto de conexión de la predicción

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction_endpoint)]

### <a name="use-the-prediction-endpoint"></a>Uso del punto de conexión de la predicción

Esta parte del script carga la imagen de prueba, consulta el punto de conexión del modelo y envía los datos de la predicción a la consola.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Cuando se ejecuta la aplicación, se debería abrir una ventana de consola y escribir la salida siguiente:

```console
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
