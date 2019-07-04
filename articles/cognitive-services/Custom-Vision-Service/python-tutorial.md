---
title: 'Inicio rápido: Creación de un proyecto de clasificación de imágenes con el SDK de Custom Vision para Python'
titlesuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y realice una predicción con el SDK de Python.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: 2fbb80b3b9a288442e83a0d46fa04c37bde1d358
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341151"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-python-sdk"></a>Inicio rápido: Creación de un proyecto de clasificación de imágenes con SDK de Custom Vision para Python

En este artículo se proporciona información y código de ejemplo para ayudarle a empezar a utilizar el SDK de Custom Vision con Python para crear un modelo de clasificación de imágenes. Después de crearlo, puede agregar etiquetas, cargar imágenes, entrenar el proyecto, obtener la dirección URL publicada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Utilice este ejemplo como plantilla para crear su propia aplicación de Python. Si desea seguir el proceso de creación y utilizar un modelo de clasificación _sin_ código, consulte la [guía basada en explorador](getting-started-build-a-classifier.md) en su lugar.

## <a name="prerequisites"></a>Requisitos previos

- [Python 2.7+ o 3.5+](https://www.python.org/downloads/)
- Herramienta [pip](https://pip.pypa.io/en/stable/installing/)

## <a name="install-the-custom-vision-sdk"></a>Instalar el SDK de Custom Vision

Para instalar el SDK de Custom Vision Service para Python, ejecute el comando siguiente en PowerShell:

```powershell
pip install azure-cognitiveservices-vision-customvision
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [python-get-images](includes/python-get-images.md)]


## <a name="add-the-code"></a>Adición del código

Cree un archivo llamado *sample.py* en el directorio del proyecto que prefiera.

### <a name="create-the-custom-vision-service-project"></a>Creación del proyecto de Custom Vision Service

Para crear un proyecto de Custom Vision Service, agregue el siguiente código al script. Inserte las claves de la suscripción en las definiciones pertinentes.

```Python
from azure.cognitiveservices.vision.customvision.training import CustomVisionTrainingClient
from azure.cognitiveservices.vision.customvision.training.models import ImageFileCreateEntry

ENDPOINT = "https://southcentralus.api.cognitive.microsoft.com"

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"
prediction_resource_id = "<your prediction resource id>"

publish_iteration_name = "classifyModel"

trainer = CustomVisionTrainingClient(training_key, endpoint=ENDPOINT)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My New Project")
```

### <a name="create-tags-in-the-project"></a>Creación de etiquetas en el proyecto

Para crear etiquetas de clasificación al proyecto, agregue el código siguiente al final de *sample.py*:

```Python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

### <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

Para agregar las imágenes de ejemplo al proyecto, inserte el siguiente código después de crear la etiqueta. Este código carga cada imagen con su etiqueta correspondiente. Deberá especificar la ruta de acceso de la dirección URL de la imagen base, en función del lugar en que descargó el proyecto Cognitive Services Python SDK Samples.

> [!NOTE]
> Tendrá que cambiar la ruta de acceso a las imágenes en función del lugar en que descargó el proyecto Cognitive Services Python SDK Samples antes.

```Python
base_image_url = "<path to project>"

print("Adding images...")

image_list = []

for image_num in range(1, 11):
    file_name = "hemlock_{}.jpg".format(image_num)
    with open(base_image_url + "images/Hemlock/" + file_name, "rb") as image_contents:
        image_list.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), tag_ids=[hemlock_tag.id]))

for image_num in range(1, 11):
    file_name = "japanese_cherry_{}.jpg".format(image_num)
    with open(base_image_url + "images/Japanese Cherry/" + file_name, "rb") as image_contents:
        image_list.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), tag_ids=[cherry_tag.id]))

upload_result = trainer.create_images_from_files(project.id, images=image_list)
if not upload_result.is_batch_successful:
    print("Image batch upload failed.")
    for image in upload_result.images:
        print("Image status: ", image.status)
    exit(-1)
```

### <a name="train-the-classifier-and-publish"></a>Entrenar el clasificador y publicarlo

Este código crea la primera iteración del proyecto y, después, publica dicha iteración en el punto de conexión de la predicción. El nombre que se da a la iteración publicada se puede utilizar para enviar solicitudes de predicción. Una iteración no está disponible en el punto de conexión de la predicción hasta que se publica.

```Python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Publish it to the project endpoint
trainer.publish_iteration(project.id, iteration.id, publish_iteration_name, prediction_resource_id)
print ("Done!")
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>Obtener y usar la iteración publicada en el punto de conexión de predicción

Para enviar una imagen al punto de conexión de la predicción y recuperar la predicción, agregue el código siguiente al final del archivo:

```python
from azure.cognitiveservices.vision.customvision.prediction import CustomVisionPredictionClient

# Now there is a trained endpoint that can be used to make a prediction
predictor = CustomVisionPredictionClient(prediction_key, endpoint=ENDPOINT)

with open(base_image_url + "images/Test/test_image.jpg", "rb") as image_contents:
    results = predictor.classify_image(
        project.id, publish_iteration_name, image_contents.read())

    # Display the results.
    for prediction in results.predictions:
        print("\t" + prediction.tag_name +
              ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute *sample.py*.

```powershell
python sample.py
```

La salida de la aplicación debe ser similar al texto siguiente:

```console
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

Luego puede comprobar que la imagen de prueba (que se encuentra en **<base_image_url>/Images/Test/** ) se ha etiquetado correctamente. También puede volver al [sitio web de Custom Vision](https://customvision.ai) y ver el estado actual del proyecto recién creado.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Pasos siguientes

Ya ha visto cómo todos los pasos del proceso de clasificación de imágenes se pueden realizar en código. En este ejemplo se ejecuta una sola iteración de entrenamiento, pero a menudo necesitará entrenar y probar su modelo varias veces para hacerlo más preciso.

> [!div class="nextstepaction"]
> [Prueba y reentrenamiento del modelo](test-your-model.md)
