---
title: 'Inicio rápido: Creación de un proyecto de clasificación de imágenes con SDK de Custom Vision para Go'
titlesuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y realice una predicción con el SDK para Go.
services: cognitive-services
author: areddish
manager: daauld
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: quickstart
ms.date: 2/25/2018
ms.author: areddish
ms.openlocfilehash: 9a45cc3f8aaae3fb000858f8903ed4aff248513c
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56885231"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-go-sdk"></a>Inicio rápido: Creación de un proyecto de clasificación de imágenes con SDK de Custom Vision para Go

En este artículo se proporciona información y código de ejemplo para ayudarle a empezar a utilizar el SDK de Custom Vision con Go para crear un modelo de clasificación de imágenes. Después de crearlo, puede agregar etiquetas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Use este ejemplo como plantilla para crear su propia aplicación de Go. Si desea seguir el proceso de creación y utilizar un modelo de clasificación _sin_ código, consulte la [guía basada en explorador](getting-started-build-a-classifier.md) en su lugar.

## <a name="prerequisites"></a>Requisitos previos

- [Go 1.8+](https://golang.org/doc/install)

## <a name="install-the-custom-vision-sdk"></a>Instalar el SDK de Custom Vision

Para instalar el SDK de Custom Vision Service para Go, ejecute el comando siguiente en PowerShell:

```
go get -u github.com/Azure/azure-sdk-for-go/...
```

O, si usa dep, dentro de su repositorio ejecute:
```
dep ensure -add github.com/Azure/azure-sdk-for-go
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [python-get-images](includes/python-get-images.md)]

## <a name="add-the-code"></a>Adición del código

Cree un archivo llamado *sample.go* en el directorio de proyecto que prefiera.

### <a name="create-the-custom-vision-service-project"></a>Creación del proyecto de Custom Vision Service

Para crear un proyecto de Custom Vision Service, agregue el siguiente código al script. Inserte las claves de la suscripción en las definiciones pertinentes.

```go
import(
    "context"
    "bytes"
    "fmt"
    "io/ioutil"
    "path"
    "log"
    "time"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.2/customvision/training"
    "github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v1.1/customvision/prediction"
)

var (
    training_key string = "<your training key>"
    prediction_key string = "<your prediction key>"
    endpoint string = "https://southcentralus.api.cognitive.microsoft.com"
    project_name string = "Go Sample Project"
    sampleDataDirectory = "<path to sample images>"
)

func main() {
    fmt.Println("Creating project...")

    ctx = context.Background()

    trainer := training.New(training_key, endpoint)

    project, err := trainer.CreateProject(ctx, project_name, "sample project", nil, string(training.Multilabel))
    if (err != nil) {
        log.Fatal(err)
    }
```

### <a name="create-tags-in-the-project"></a>Creación de etiquetas en el proyecto

Para crear etiquetas de clasificación para el proyecto, agregue el código siguiente al final de *sample.go*:

```go
    # Make two tags in the new project
    hemlockTag, _ := trainer.CreateTag(ctx, *project.ID, "Hemlock", "Hemlock tree tag", string(training.Regular))
    cherryTag, _ := trainer.CreateTag(ctx, *project.ID, "Japanese Cherry", "Japanese cherry tree tag", string(training.Regular))
```

### <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

Para agregar las imágenes de ejemplo al proyecto, inserte el siguiente código después de crear la etiqueta. Este código carga cada imagen con su etiqueta correspondiente. Deberá especificar la ruta de acceso de la dirección URL de la imagen base, en función del lugar en que descargó el proyecto Cognitive Services Go SDK Samples.

> [!NOTE]
> Tendrá que cambiar la ruta de acceso a las imágenes en función de donde haya descargado anteriormente el proyecto Cognitive Services Go SDK Samples.

```go
    fmt.Println("Adding images...")
    japaneseCherryImages, err := ioutil.ReadDir(path.Join(sampleDataDirectory, "Japanese Cherry"))
    if err != nil {
        fmt.Println("Error finding Sample images")
    }

    hemLockImages, err := ioutil.ReadDir(path.Join(sampleDataDirectory, "Hemlock"))
    if err != nil {
        fmt.Println("Error finding Sample images")
    }

    for _, file := range hemLockImages {
        imageFile, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Hemlock", file.Name()))
        imageData := ioutil.NopCloser(bytes.NewReader(imageFile))

        trainer.CreateImagesFromData(ctx, *project.ID, imageData, []string{ hemlockTag.ID.String() })
    }

    for _, file := range japaneseCherryImages {
        imageFile, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Japanese Cherry", file.Name()))
        imageData := ioutil.NopCloser(bytes.NewReader(imageFile))
        trainer.CreateImagesFromData(ctx, *project.ID, imageData, []string{ cherryTag.ID.String() })
    }
```

### <a name="train-the-classifier"></a>Entrenamiento del clasificador

Este código crea la primera iteración del proyecto y la marca como la predeterminada. La iteración predeterminada refleja la versión del modelo que responderá a las solicitudes de predicción. La debe actualizar cada vez que vuelva a entrenar el modelo.

```go
    fmt.Println("Training...")
    iteration, _ := trainer.TrainProject(ctx, *project.ID)
    for {
        if *iteration.Status != "Training" {
            break
        }
        fmt.Println("Training status: " + *iteration.Status)
        time.Sleep(1 * time.Second)
        iteration, _ = trainer.GetIteration(ctx, *project.ID, *iteration.ID)
    }
    fmt.Println("Training status: " + *iteration.Status)

    *iteration.IsDefault = true
    trainer.UpdateIteration(ctx, *project.ID, *iteration.ID, iteration)
```

### <a name="get-and-use-the-default-prediction-endpoint"></a>Obtención y uso del punto de conexión de predicción predeterminado

Para enviar una imagen al punto de conexión de la predicción y recuperar la predicción, agregue el código siguiente al final del archivo:

```go
    fmt.Println("Predicting...")
    predictor := prediction.New(prediction_key, endpoint)

    testImageData, _ := ioutil.ReadFile(path.Join(sampleDataDirectory, "Test", "test_image.jpg"))
    results, _ := predictor.PredictImage(ctx, *project.ID, ioutil.NopCloser(bytes.NewReader(testImageData)), iteration.ID, "")

    for _, prediction := range *results.Predictions {
        fmt.Printf("\t%s: %.2f%%", *prediction.TagName, *prediction.Probability * 100)
        fmt.Println("")
    }
}
```

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute *sample.go*.

```PowerShell
go run sample.go
```

La salida de la aplicación debe ser similar al texto siguiente:

```
Creating project...
Adding images...
Training...
Training status: Training
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

Luego puede comprobar que la imagen de prueba (que se encuentra en **<base_image_url>/Images/Test/**) se ha etiquetado correctamente. También puede volver al [sitio web de Custom Vision](https://customvision.ai) y ver el estado actual del proyecto recién creado.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Pasos siguientes

Ya ha visto cómo todos los pasos del proceso de clasificación de imágenes se pueden realizar en código. En este ejemplo se ejecuta una sola iteración de entrenamiento, pero a menudo necesitará entrenar y probar su modelo varias veces para hacerlo más preciso.

> [!div class="nextstepaction"]
> [Prueba y reentrenamiento del modelo](test-your-model.md)