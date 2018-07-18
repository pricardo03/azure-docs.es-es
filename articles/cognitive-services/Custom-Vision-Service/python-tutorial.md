---
title: 'Creación de un tutorial de Python de Custom Vision Service: Azure Cognitive Services | Microsoft Docs'
description: Explore una aplicación básica de Python que usa Custom Vision API en Microsoft Cognitive Services. Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y realice una predicción con el punto de conexión predeterminado.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/07/2018
ms.author: areddish
ms.openlocfilehash: 0359935bf266d4f2a5cf845dd0d23183f4f77b72
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381282"
---
# <a name="custom-vision-api-python-tutorial"></a>Tutorial de Custom Vision API para Python

Obtenga información sobre cómo crear un proyecto de clasificación de imágenes con Custom Vision Service y un script básico de Python. Después de crearlo, puede agregar etiquetas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Use este ejemplo de código abierto como plantilla para compilar su propia aplicación mediante Custom Vision API.



## <a name="prerequisites"></a>requisitos previos

- Python 2.7+ o Python 3.5+.
- La herramienta de PIP.

## <a name="get-the-training-and-prediction-keys"></a>Obtener las claves de entrenamiento y predicción

Para obtener las claves que se utilizan en este ejemplo, visite la [página web de Custom Vision](https://customvision.ai) y seleccione el __icono de engranaje__ en la esquina superior derecha. En la sección __Cuentas__, copie los valores de los campos __Training Key__ (Clave de entrenamiento) y __Prediction Key__ (Clave de predicción).

![Imagen de la interfaz de usuario de claves](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Instalación del SDK de Custom Vision Service

Para instalar el SDK de Custom VIsion Service, use el comando siguiente:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Obtener imágenes de ejemplo

Este ejemplo usa las imágenes del directorio `Samples/Images` del proyecto [https://github.com/Microsoft/Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images). Clone o descargue y extraiga el proyecto en el entorno de desarrollo.

## <a name="create-a-custom-vision-service-project"></a>Creación de un proyecto de Custom Vision Service

Para crear un proyecto de Custom Vision Service, cree un archivo llamado `sample.py`. Use el código siguiente como el contenido del archivo:

> [!IMPORTANT]
> Defina `training_key` con el valor de la clave de aprendizaje que recuperó anteriormente.
>
> Defina `prediction_key` con el valor de la clave de predicción que recuperó anteriormente.

```python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageUrlCreateEntry

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Project")
```

## <a name="add-tags-to-your-project"></a>Adición de etiquetas al proyecto

Para agregar etiquetas al proyecto, agregue el código siguiente al final del archivo `sample.py`:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Carga de imágenes en el proyecto

Para agregar las imágenes de ejemplo al proyecto, inserte el siguiente código después de crear la etiqueta. Este código carga la imagen con la etiqueta correspondiente:

> [!IMPORTANT]
>
> Cambie la ruta de acceso a las imágenes en función de dónde descargó el proyecto Cognitive-CustomVision-Windows anteriormente.

```python
base_image_url = "https://raw.githubusercontent.com/Microsoft/Cognitive-CustomVision-Windows/master/Samples/"

print ("Adding images...")
for image_num in range(1,10):
    image_url = base_image_url + "Images/Hemlock/hemlock_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ hemlock_tag.id ] ) ])

for image_num in range(1,10):
    image_url = base_image_url + "Images/Japanese Cherry/japanese_cherry_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ cherry_tag.id ] ) ])


# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following:
#
#import os
#hemlock_dir = "Images\\Hemlock"
#for image in os.listdir(os.fsencode("Images\\Hemlock")):
#    with open(hemlock_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ hemlock_tag.id ])
#
#cherry_dir = "Images\\Japanese Cherry"
#for image in os.listdir(os.fsencode("Images\\Japanese Cherry")):
#    with open(cherry_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ cherry_tag.id ])
```

## <a name="train-the-project"></a>Entrenamiento del proyecto

Para entrenar el clasificador, agregue el código siguiente al final del archivo `sample.py`:

```python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Make it the default project endpoint
trainer.update_iteration(project.id, iteration.id, is_default=True)
print ("Done!")
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Obtención y uso del punto de conexión de predicción predeterminado

Para enviar una imagen al punto de conexión de predicción y recuperar la predicción, agregue el código siguiente al final del archivo `sample.py`:

```python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint that can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

test_img_url = base_image_url + "Images/Test/test_image.jpg"
results = predictor.predict_image_url(project.id, iteration.id, url=test_img_url)

# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following.
#
# Open the sample image and get back the prediction results.
# with open("Images\\test\\test_image.jpg", mode="rb") as test_data:
#     results = predictor.predict_image(project.id, test_data, iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-example"></a>Ejecutar el ejemplo

Ejecute la solución. Los resultados de predicción aparecen en la consola.

```
python sample.py
```

La salida de la aplicación es similar al texto siguiente:

```
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```