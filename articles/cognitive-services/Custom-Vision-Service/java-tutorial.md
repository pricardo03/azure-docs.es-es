---
title: 'Tutorial: Creación de un proyecto de clasificación de imágenes con el SDK de Custom Vision para Java'
titlesuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y realice una predicción con el punto de conexión predeterminado.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: e302fc580d9c83d269f0deedd051a3ea23bd274e
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957228"
---
# <a name="tutorial-create-an-image-classification-project-with-the-custom-vision-sdk-for-java"></a>Tutorial: Creación de un proyecto de clasificación de imágenes con el SDK de Custom Vision para Java

Obtenga información sobre cómo crear un proyecto de clasificación de imágenes con Custom Vision Service mediante Java. Después de crearlo, puede agregar etiquetas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Use este ejemplo de código abierto como plantilla para compilar su propia aplicación mediante Custom Vision API.

## <a name="prerequisites"></a>Requisitos previos

- JDK 7 u 8 instalado.
- Maven instalado.

## <a name="get-the-training-and-prediction-keys"></a>Obtención de las claves de entrenamiento y predicción

Para obtener las claves que se utilizan en este ejemplo, visite la [página web de Custom Vision](https://customvision.ai) y seleccione el __icono de engranaje__ en la esquina superior derecha. En la sección __Cuentas__, copie los valores de los campos __Training Key__ (Clave de entrenamiento) y __Prediction Key__ (Clave de predicción).

![Imagen de la interfaz de usuario de claves](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Instalación del SDK de Custom Vision Service

Puede instalar el SDK de Custom Vision desde el repositorio central de Maven:
* [SDK de aprendizaje](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [SDK de predicción](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

## <a name="understand-the-code"></a>Comprensión del código

El proyecto completo, incluidas las imágenes, está disponible en el [repositorio de ejemplos de Custom Vision de Azure para Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). 

Use el IDE de Java favorito para abrir el proyecto `Vision/CustomVision`. 

Esta aplicación utiliza la clave de entrenamiento que recuperó anteriormente para crear un proyecto denominado __Sample Java Project__. A continuación, carga las imágenes para entrenar y probar un clasificador. El clasificador identifica si el árbol es un __abeto canadiense__ o un __cerezo japonés__.

Los siguientes fragmentos de código implementan la funcionalidad principal de este ejemplo:

## <a name="create-a-custom-vision-service-project"></a>Creación de un proyecto de Custom Vision Service

> [!IMPORTANT]
> Defina `trainingApiKey` con el valor de la clave de aprendizaje que recuperó anteriormente.

```java
final String trainingApiKey = "insert your training key here";
TrainingApi trainClient = CustomVisionTrainingManager.authenticate(trainingApiKey);

Trainings trainer = trainClient.trainings();

System.out.println("Creating project...");
Project project = trainer.createProject()
            .withName("Sample Java Project")
            .execute();
```

## <a name="add-tags-to-your-project"></a>Adición de etiquetas al proyecto

```java
// create hemlock tag
Tag hemlockTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Hemlock")
    .execute();

// create cherry tag
Tag cherryTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Japanese Cherry")
    .execute();
```

## <a name="upload-images-to-the-project"></a>Carga de imágenes en el proyecto

El ejemplo está configurado para incluir las imágenes en el paquete final. Las imágenes se leen desde la sección de recursos de jar y se cargan en el servicio.

```java
System.out.println("Adding images...");
for (int i = 1; i <= 10; i++) {
    String fileName = "hemlock_" + i + ".jpg";
    byte[] contents = GetImage("/Hemlock", fileName);
    AddImageToProject(trainer, project, fileName, contents, hemlockTag.id(), null);
}

for (int i = 1; i <= 10; i++) {
    String fileName = "japanese_cherry_" + i + ".jpg";
    byte[] contents = GetImage("/Japanese Cherry", fileName);
    AddImageToProject(trainer, project, fileName, contents, cherryTag.id(), null);
}
```

El fragmento de código anterior usa dos funciones auxiliares que recuperan las imágenes como flujos de recursos y las cargan en el servicio.

```java
private static void AddImageToProject(Trainings trainer, Project project, String fileName, byte[] contents, UUID tag)
{
    System.out.println("Adding image: " + fileName);

    ImageFileCreateEntry file = new ImageFileCreateEntry()
        .withName(fileName)
        .withContents(contents);

    ImageFileCreateBatch batch = new ImageFileCreateBatch()
        .withImages(Collections.singletonList(file));

    batch = batch.withTagIds(Collections.singletonList(tag));

    trainer.createImagesFromFiles(project.id(), batch);
}

private static byte[] GetImage(String folder, String fileName)
{
    try {
        return ByteStreams.toByteArray(CustomVisionSamples.class.getResourceAsStream(folder + "/" + fileName));
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    return null;
}
```

## <a name="train-the-project"></a>Entrenamiento del proyecto

Esto crea la primera iteración del proyecto y marca esta iteración como la predeterminada. 

```java
System.out.println("Training...");
Iteration iteration = trainer.trainProject(project.id());

while (iteration.status().equals("Training"))
{
    System.out.println("Training Status: "+ iteration.status());
    Thread.sleep(1000);
    iteration = trainer.getIteration(project.id(), iteration.id());
}

System.out.println("Training Status: "+ iteration.status());
trainer.updateIteration(project.id(), iteration.id(), iteration.withIsDefault(true));
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Obtención y uso del punto de conexión de predicción predeterminado

> [!IMPORTANT]
> Defina `predictionApiKey` con el valor de la clave de predicción que recuperó anteriormente.

```java
final String predictionApiKey = "insert your prediction key here";
PredictionEndpoint predictClient = CustomVisionPredictionManager.authenticate(predictionApiKey);

// Use below for predictions from a url
// String url = "some url";
// ImagePrediction results = predictor.predictions().predictImage()
//                         .withProjectId(project.id())
//                         .withUrl(url)
//                         .execute();

// load test image
byte[] testImage = GetImage("/Test", "test_image.jpg");

// predict
ImagePrediction results = predictor.predictions().predictImage()
    .withProjectId(project.id())
    .withImageData(testImage)
    .execute();

for (Prediction prediction: results.predictions())
{
    System.out.println(String.format("\t%s: %.2f%%", prediction.tagName(), prediction.probability() * 100.0f));
}
```

## <a name="run-the-example"></a>Ejecutar el ejemplo

Para compilar y ejecutar la solución con Maven:

```
mvn compile exec:java
```

La salida de la aplicación es similar al texto siguiente:

```
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