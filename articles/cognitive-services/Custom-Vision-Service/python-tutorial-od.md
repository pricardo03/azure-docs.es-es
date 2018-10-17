---
title: 'Tutorial: Crear un proyecto de detección de objetos - Custom Vision API, Python'
titlesuffix: Azure Cognitive Services
description: Cree un proyecto, agregue etiquetas, cargue imágenes, entrene el proyecto y realice una predicción con el punto de conexión predeterminado.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: areddish
ms.openlocfilehash: f49f5ab32d834b32de54be2d96c3671ad46f79f3
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363707"
---
# <a name="tutorial-build-an-object-detection-project-with-python"></a>Tutorial: Crear un proyecto de detección de objetos con Python

Explore un script de Python básico que usa Computer Vision API para crear un proyecto de detección de objetos. Después de crearlo, puede agregar regiones etiquetadas, cargar imágenes, entrenar el proyecto, obtener la dirección URL predeterminada del punto de conexión de predicción del proyecto y utilizar el punto de conexión para probar una imagen mediante programación. Use este ejemplo de código abierto como plantilla para compilar su propia aplicación mediante Custom Vision API.

## <a name="prerequisites"></a>Requisitos previos

Para usar el tutorial, debe realizar los siguientes pasos:

- Instale Python 2.7 o posterior o Python 3.5 o posterior.
- Instalar PIP.

### <a name="platform-requirements"></a>Requisitos de plataforma
Este ejemplo se ha desarrollado para Python.

### <a name="get-the-custom-vision-sdk"></a>Obtener el SDK de Custom Vision

Para compilar este ejemplo, debe instalar el SDK de Python para Custom Vision API:

```
pip install azure-cognitiveservices-vision-customvision
```

Puede descargar las imágenes con los [ejemplos de Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples).

## <a name="step-1-get-the-training-and-prediction-keys"></a>Paso 1: obtener las claves de entrenamiento y de predicción

Para obtener las claves que se utilizan en este ejemplo, visite el [sitio de Custom Vision](https://customvision.ai) y seleccione el __icono de engranaje__ en la esquina superior derecha. En la sección __Cuentas__, copie los valores de los campos __Training Key__ (Clave de entrenamiento) y __Prediction Key__ (Clave de predicción).

![Imagen de la UI de las claves](./media/python-tutorial/training-prediction-keys.png)

Este ejemplo utiliza las imágenes de [esta ubicación](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/tree/master/samples/vision/images).

## <a name="step-2-create-a-custom-vision-service-project"></a>Paso 2: crear un proyecto de Custom Vision Service

Para crear un nuevo proyecto de Custom Vision Service, cree un archivo de script sample.py y agregue el siguiente contenido. Tenga en cuenta que la diferencia entre la creación de un proyecto de detección de objetos y de clasificación de imágenes es el dominio que se especifica en la llamada de create_project.

```Python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageFileCreateEntry, Region

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Find the object detection domain
obj_detection_domain = next(domain for domain in trainer.get_domains() if domain.type == "ObjectDetection")

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Detection Project", domain_id=obj_detection_domain.id)
```

## <a name="step-3-add-tags-to-your-project"></a>Paso 3: agregar etiquetas al proyecto

Para agregar etiquetas al proyecto, escriba el siguiente código para crear dos etiquetas:

```Python
# Make two tags in the new project
fork_tag = trainer.create_tag(project.id, "fork")
scissors_tag = trainer.create_tag(project.id, "scissors")
```

## <a name="step-4-upload-images-to-the-project"></a>Paso 4: cargar imágenes al proyecto

Para el proyecto de detección de objetos es necesario cargar la imagen, las regiones y las etiquetas. La región se encuentra en coordenadas normalizadas y especifica la ubicación del objeto etiquetado.

Para agregar las imágenes, la región y las etiquetas al proyecto, inserte el siguiente código después de crear la etiqueta. Tenga en cuenta que, para este tutorial, las regiones se codificaron de forma rígida alineadas con el código. Las regiones especifican el rectángulo de selección en coordenadas normalizadas.

```Python

fork_image_regions = {
    "fork_1": [ 0.145833328, 0.3509314, 0.5894608, 0.238562092 ],
    "fork_2": [ 0.294117659, 0.216944471, 0.534313738, 0.5980392 ],
    "fork_3": [ 0.09191177, 0.0682516545, 0.757352948, 0.6143791 ],
    "fork_4": [ 0.254901975, 0.185898721, 0.5232843, 0.594771266 ],
    "fork_5": [ 0.2365196, 0.128709182, 0.5845588, 0.71405226 ],
    "fork_6": [ 0.115196079, 0.133611143, 0.676470637, 0.6993464 ],
    "fork_7": [ 0.164215669, 0.31008172, 0.767156839, 0.410130739 ],
    "fork_8": [ 0.118872553, 0.318251669, 0.817401946, 0.225490168 ],
    "fork_9": [ 0.18259804, 0.2136765, 0.6335784, 0.643790841 ],
    "fork_10": [ 0.05269608, 0.282303959, 0.8088235, 0.452614367 ],
    "fork_11": [ 0.05759804, 0.0894935, 0.9007353, 0.3251634 ],
    "fork_12": [ 0.3345588, 0.07315363, 0.375, 0.9150327 ],
    "fork_13": [ 0.269607842, 0.194068655, 0.4093137, 0.6732026 ],
    "fork_14": [ 0.143382356, 0.218578458, 0.7977941, 0.295751631 ],
    "fork_15": [ 0.19240196, 0.0633497, 0.5710784, 0.8398692 ],
    "fork_16": [ 0.140931368, 0.480016381, 0.6838235, 0.240196079 ],
    "fork_17": [ 0.305147052, 0.2512582, 0.4791667, 0.5408496 ],
    "fork_18": [ 0.234068632, 0.445702642, 0.6127451, 0.344771236 ],
    "fork_19": [ 0.219362751, 0.141781077, 0.5919118, 0.6683006 ],
    "fork_20": [ 0.180147052, 0.239820287, 0.6887255, 0.235294119 ]
}

scissors_image_regions = {
    "scissors_1": [ 0.4007353, 0.194068655, 0.259803921, 0.6617647 ],
    "scissors_2": [ 0.426470578, 0.185898721, 0.172794119, 0.5539216 ],
    "scissors_3": [ 0.289215684, 0.259428144, 0.403186262, 0.421568632 ],
    "scissors_4": [ 0.343137264, 0.105833367, 0.332107842, 0.8055556 ],
    "scissors_5": [ 0.3125, 0.09766343, 0.435049027, 0.71405226 ],
    "scissors_6": [ 0.379901975, 0.24308826, 0.32107842, 0.5718954 ],
    "scissors_7": [ 0.341911763, 0.20714055, 0.3137255, 0.6356209 ],
    "scissors_8": [ 0.231617644, 0.08459154, 0.504901946, 0.8480392 ],
    "scissors_9": [ 0.170343131, 0.332957536, 0.767156839, 0.403594762 ],
    "scissors_10": [ 0.204656869, 0.120539248, 0.5245098, 0.743464053 ],
    "scissors_11": [ 0.05514706, 0.159754932, 0.799019635, 0.730392158 ],
    "scissors_12": [ 0.265931368, 0.169558853, 0.5061275, 0.606209159 ],
    "scissors_13": [ 0.241421565, 0.184264734, 0.448529422, 0.6830065 ],
    "scissors_14": [ 0.05759804, 0.05027781, 0.75, 0.882352948 ],
    "scissors_15": [ 0.191176474, 0.169558853, 0.6936275, 0.6748366 ],
    "scissors_16": [ 0.1004902, 0.279036, 0.6911765, 0.477124184 ],
    "scissors_17": [ 0.2720588, 0.131977156, 0.4987745, 0.6911765 ],
    "scissors_18": [ 0.180147052, 0.112369314, 0.6262255, 0.6666667 ],
    "scissors_19": [ 0.333333343, 0.0274019931, 0.443627447, 0.852941155 ],
    "scissors_20": [ 0.158088237, 0.04047389, 0.6691176, 0.843137264 ]
}

# Go through the data table above and create the images
print ("Adding images...")
tagged_images_with_regions = []

for file_name in fork_image_regions.keys():
    x,y,w,h = fork_image_regions[file_name]
    regions = [ Region(tag_id=fork_tag.id, left=x,top=y,width=w,height=h) ]

    with open("images/fork/" + file_name + ".jpg", mode="rb") as image_contents:
        tagged_images_with_regions.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), regions=regions))

for file_name in scissors_image_regions.keys():
    x,y,w,h = scissors_image_regions[file_name]
    regions = [ Region(tag_id=scissors_tag.id, left=x,top=y,width=w,height=h) ]

    with open("images/scissors/" + file_name + ".jpg", mode="rb") as image_contents:
        tagged_images_with_regions.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), regions=regions))


trainer.create_images_from_files(project.id, images=tagged_images_with_regions)
```

## <a name="step-5-train-the-project"></a>Paso 5: entrenar el proyecto

Ahora que ha agregado etiquetas e imágenes al proyecto, puede entrenarlo: 

1. Inserte el siguiente código. Esto crea la primera iteración del proyecto. 
2. Marque esta iteración como la iteración predeterminada.

```Python
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

## <a name="step-6-get-and-use-the-default-prediction-endpoint"></a>Paso 6: obtener y usar el punto de conexión de predicción predeterminado

Ahora está preparado para usar el modelo para la predicción: 

1. Obtenga el punto de conexión asociado a la iteración predeterminada. 
2. Utilice ese punto de conexión y envíe una imagen de prueba al proyecto.

```Python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint that can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

# Open the sample image and get back the prediction results.
with open("images/Test/test_od_image.jpg", mode="rb") as test_data:
    results = predictor.predict_image(project.id, test_data, iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100), prediction.bounding_box.left, prediction.bounding_box.top, prediction.bounding_box.width, prediction.bounding_box.height)
```

## <a name="step-7-run-the-example"></a>Paso 7: ejecutar el ejemplo

Ejecute la solución. Los resultados de predicción aparecen en la consola.

```
python sample.py
```
