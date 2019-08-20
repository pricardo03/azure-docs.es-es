---
title: 'Inicio rápido: Creación de un proyecto de detección de objetos con el SDK de Custom Vision para Java'
titleSuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y detecte objetos mediante el SDK de Java.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: areddish
ms.openlocfilehash: 2513f6ad96a8c2c6fd42f81d1123cccdc59cdd5f
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946181"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-sdk-for-java"></a>Inicio rápido: Creación de un proyecto de detección de objetos con el SDK de Custom Vision para Java

En este artículo se proporciona información y código de ejemplo para ayudarle a empezar a utilizar el SDK de Custom Vision con Java para crear un modelo de detección de objetos. Después de crearlo, puede agregar regiones etiquetadas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Utilice este ejemplo como plantilla para crear su propia aplicación de Java.

## <a name="prerequisites"></a>Requisitos previos

- El IDE de Java que prefiera
- [JDK 7 u 8](https://aka.ms/azure-jdks) instalado.
- Maven instalado

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Obtención del SDK de Custom Vision y código de ejemplo

Para escribir una aplicación de Java que utiliza Custom Vision, necesitará los paquetes maven de Custom Vision, que están incluidos en el proyecto de ejemplo que descargará, pero puede acceder a ellos individualmente aquí.

Puede instalar el SDK de Custom Vision desde el repositorio central de Maven:
- [SDK de aprendizaje](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [SDK de predicción](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Clone o descargue el proyecto [Ejemplos del SDK de Cognitive Services para Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). Vaya a la carpeta **Vision/CustomVision/** .

Este proyecto Java crea un nuevo proyecto de detección de objetos de Custom Vision denominado __Sample Java OD Project__, al que se puede acceder en el [sitio web de Custom Vision](https://customvision.ai/). A continuación, carga las imágenes para entrenar y probar un clasificador. En este proyecto, el clasificador pretende determinar si un árbol es un __abeto__ o un __cerezo japonés__.

[!INCLUDE [get-keys](includes/get-keys.md)]

El programa está configurado para almacenar los datos clave como variables de entorno. Para establecer estas variables, vaya a la carpeta **Vision/CustomVision** en PowerShell. Después, escriba los comandos:

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Comprendiendo el código

Cargue el proyecto `Vision/CustomVision` en el IDE de Java y abra el archivo _CustomVisionSamples.java_. Busque el método **runSample** y marque como comentario la llamada al método **ImageClassification_Sample**. Así se ejecuta el escenario de detección de objetos, que no se describe en esta guía. El método **ObjectDetection_Sample** implementa la funcionalidad principal de esta guía de inicio rápido; vaya a su definición e inspeccione el código. 

### <a name="create-a-new-custom-vision-service-project"></a>Creación de un proyecto de Custom Vision Service

Vaya al bloque de código que crea un cliente de aprendizaje y un proyecto de detección de objetos. El proyecto creado se mostrará en el [sitio web de Custom Vision](https://customvision.ai/) que ha visitado anteriormente. Consulte las sobrecargas del método [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_) para especificar otras opciones al crear el proyecto (explicadas en la guía del portal web [Creación de un detector](get-started-build-detector.md)).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=181-206)]

### <a name="add-tags-to-your-project"></a>Adición de etiquetas al proyecto

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=208-218)]

### <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

Cuando se etiquetan imágenes en los proyectos de detección de objetos, es preciso especificar la región de cada objeto etiquetado mediante coordenadas normalizadas. Vaya a la definición del mapa `regionMap`. Este código asocia cada una de las imágenes del ejemplo a su región etiquetada.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=130-179)]

Luego, vaya al bloque de código que agrega las imágenes al proyecto. Las imágenes se leen de la carpeta **src/main/resources** carpeta del proyecto y se cargan en el servicio con sus etiquetas adecuadas y las coordenadas de la región.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=220-231)]

El fragmento de código anterior usa dos funciones auxiliares que recuperan las imágenes como flujos de recursos y las cargan en el servicio (puede cargar hasta 64 imágenes en un único lote).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=277-314)]

### <a name="train-the-project-and-publish"></a>Entrenar el proyecto y publicarlo

Este código crea la primera iteración del proyecto y, después, publica dicha iteración en el punto de conexión de la predicción. El nombre que se da a la iteración publicada se puede utilizar para enviar solicitudes de predicción. Una iteración no está disponible en el punto de conexión de la predicción hasta que se publica.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=233-242)]

### <a name="use-the-prediction-endpoint"></a>Uso del punto de conexión de la predicción

El punto de conexión de la predicción, representado aquí por el objeto `predictor`, es la referencia que utiliza para enviar una imagen al modelo actual y obtener una predicción de clasificación. En este ejemplo, `predictor` se define en otra parte mediante la variable de entorno de la clave de predicción.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?range=244-270)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Para compilar y ejecutar la solución con maven, ejecute el siguiente comando en el directorio del proyecto en PowerShell:

```powershell
mvn compile exec:java
```

En la salida de la consola podrá ver los resultados de la predicción y del. Luego puede comprobar que la imagen de prueba se ha etiquetado correctamente y que la región de detección es correcta.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Pasos siguientes

Ya ha visto cómo todos los pasos del proceso de detección de objetos se pueden realizar en código. En este ejemplo se ejecuta una sola iteración de entrenamiento, pero a menudo necesitará entrenar y probar su modelo varias veces para hacerlo más preciso. En la guía siguiente se trata la clasificación de imágenes, pero sus principios son similares a los de la detección de objetos.

> [!div class="nextstepaction"]
> [Prueba y reentrenamiento del modelo](test-your-model.md)
