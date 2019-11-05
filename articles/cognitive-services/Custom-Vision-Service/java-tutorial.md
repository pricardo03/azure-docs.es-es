---
title: 'Inicio rápido: Creación de un proyecto de clasificación de imágenes con el SDK de Custom Vision para Java'
titleSuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y realice una predicción con el SDK para Java.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: areddish
ms.openlocfilehash: 2f80ba319db6d6d7ce512aba8191584ea5bba86b
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "73519460"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-sdk-for-java"></a>Inicio rápido: Creación de un proyecto de clasificación de imágenes con el SDK de Custom Vision para Java

En este artículo se proporciona información y código de ejemplo para ayudarle a empezar a utilizar el SDK de Custom Vision para Java para crear un modelo de clasificación de imágenes. Después de crearlo, puede agregar etiquetas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Utilice este ejemplo como plantilla para crear su propia aplicación de Java. Si desea seguir el proceso de creación y utilizar un modelo de clasificación _sin_ código, consulte la [guía basada en explorador](getting-started-build-a-classifier.md) en su lugar.

## <a name="prerequisites"></a>Requisitos previos

- El IDE de Java que prefiera
- [JDK 7 u 8](https://aka.ms/azure-jdks) instalado.
- Maven instalado
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Obtención del SDK de Custom Vision y código de ejemplo

Para escribir una aplicación de Java que utiliza Custom Vision, necesitará los paquetes maven de Custom Vision, que están incluidos en el proyecto de ejemplo que descargará, pero puede acceder a ellos individualmente aquí.

Puede instalar el SDK de Custom Vision desde el repositorio central de Maven:

- [SDK de aprendizaje](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [SDK de predicción](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Clone o descargue el proyecto [Ejemplos del SDK de Cognitive Services para Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). Vaya a la carpeta **Vision/CustomVision/** .

Este proyecto Java crea un nuevo proyecto de clasificación de imágenes de Custom Vision denominado __Sample Java Project__, al que se puede acceder en el [sitio web de Custom Vision](https://customvision.ai/). A continuación, carga las imágenes para entrenar y probar un clasificador. En este proyecto, el clasificador pretende determinar si un árbol es un __abeto__ o un __cerezo japonés__.

[!INCLUDE [get-keys](includes/get-keys.md)]

El programa está configurado para almacenar los datos clave como variables de entorno. Para establecer estas variables, vaya a la carpeta **Vision/CustomVision** en PowerShell. Después, escriba los comandos:

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Comprendiendo el código

Cargue el proyecto `Vision/CustomVision` en el IDE de Java y abra el archivo _CustomVisionSamples.java_. Busque el método **runSample** y convierta en comentario la llamada al método **ObjectDetection_Sample**. Así se ejecuta el escenario de detección de objetos, que no se trata en esta guía. El método **ImageClassification_Sample** implementa la funcionalidad principal de este ejemplo; vaya a su definición e inspeccione el código.

### <a name="create-a-custom-vision-service-project"></a>Creación de un proyecto de Custom Vision Service

Este primer fragmento de código crea un proyecto de clasificación de imágenes. El proyecto creado se mostrará en el [sitio web de Custom Vision](https://customvision.ai/) que ha visitado anteriormente. Consulte las sobrecargas del método [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_) para especificar otras opciones al crear el proyecto (explicadas en la guía del portal web [Compilación de un clasificador](getting-started-build-a-classifier.md)).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create)]

### <a name="create-tags-in-the-project"></a>Creación de etiquetas en el proyecto

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags)]

### <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

Las imágenes de ejemplo se incluyen en la carpeta **src/main/resources** del proyecto. Desde allí se leen y se cargan en el servicio con sus etiquetas correspondientes.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload)]

El fragmento de código anterior usa dos funciones auxiliares que recuperan las imágenes como flujos de recursos y las cargan en el servicio (puede cargar hasta 64 imágenes en un único lote).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

### <a name="train-the-classifier-and-publish"></a>Entrenar el clasificador y publicarlo

Este código crea la primera iteración del proyecto y, después, publica dicha iteración en el punto de conexión de la predicción. El nombre que se da a la iteración publicada se puede utilizar para enviar solicitudes de predicción. Una iteración no está disponible en el punto de conexión de la predicción hasta que se publica.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train)]

### <a name="use-the-prediction-endpoint"></a>Uso del punto de conexión de la predicción

El punto de conexión de la predicción, representado aquí por el objeto `predictor`, es la referencia que utiliza para enviar una imagen al modelo actual y obtener una predicción de clasificación. En este ejemplo, `predictor` se define en otra parte mediante la variable de entorno de la clave de predicción.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_predict)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Para compilar y ejecutar la solución con maven, ejecute el siguiente comando en el directorio del proyecto en PowerShell:

```powershell
mvn compile exec:java
```

La salida de la consola de la aplicación debería tener una apariencia similar al ejemplo siguiente:

```console
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

A continuación, puede comprobar que la predicción de la imagen de prueba (las últimas líneas de la salida) es correcta.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Pasos siguientes

Ya ha visto cómo todos los pasos del proceso de clasificación de imágenes se pueden realizar en código. En este ejemplo se ejecuta una sola iteración de entrenamiento, pero a menudo necesitará entrenar y probar su modelo varias veces para hacerlo más preciso.

> [!div class="nextstepaction"]
> [Prueba y reentrenamiento del modelo](test-your-model.md)
