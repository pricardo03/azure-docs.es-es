---
title: 'Tutorial: Detección y enmarcado de caras de una imagen - Face API, Python'
titleSuffix: Azure Cognitive Services
description: Aprenda a usar Face API con el SDK de Python para detectar caras humanas en una imagen.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 6cc3ac25d2196c0275b445503b79b9ac06a791d3
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127744"
---
# <a name="tutorial-detect-and-frame-faces-with-the-face-api-and-python"></a>Tutorial: Detección y enmarcado de caras con Face API y Python 

En este tutorial, aprenderá a invocar Face API con el SDK de Python para detectar caras humanas en una imagen.

## <a name="prerequisites"></a>Requisitos previos

Para usar el tutorial, deberá realizar estos pasos:

- Instale Python 2.7+ o Python 3.5+.
- Instale pip.
- Instale el SDK de Python para Face API como sigue:

```bash
pip install cognitive_face
```

- Obtenga una [clave de suscripción](https://azure.microsoft.com/try/cognitive-services/) para Microsoft Cognitive Services. Puede utilizar la clave principal o la secundaria en este tutorial. (Tenga en cuenta que, para usar Face API, necesita disponer de una clave de suscripción válida).

## <a name="detect-a-face-in-an-image"></a>Detección de una cara en una imagen

```python
import cognitive_face as CF

KEY = '<Subscription Key>'  # Replace with a valid subscription key (keeping the quotes in place).
CF.Key.set(KEY)

BASE_URL = 'https://westus.api.cognitive.microsoft.com/face/v1.0/'  # Replace with your regional Base URL
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)
```

Aquí se indica un resultado de ejemplo. Se trata de un elemento `list` de caras detectadas. Cada elemento de la lista es una instancia de `dict`, donde `faceId` es un identificador único de la cara detectada y `faceRectangle` describe la posición de la cara detectada. Un identificador de cara expira en 24 horas.

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>Dibujo de rectángulos alrededor de las caras

Con las coordenadas JSON recibidas del comando anterior, puede dibujar rectángulos en las imágenes para representar visualmente cada cara. Necesitará `pip install Pillow` para usar el módulo de imágenes `PIL`.  Al principio del archivo, agregue lo siguiente:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Luego, después de `print(faces)`, incluya lo siguiente en el script:

```python
#Convert width height to a point in a rectangle
def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))

#Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

#For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

#Display the image in the users default image browser.
img.show()
```

## <a name="further-exploration"></a>Exploración adicional

Para ayudarlo a explorar más Face API, en este tutorial se ofrece un ejemplo de GUI. Para ejecutarlo, primero instale [wxPython](https://wxpython.org/pages/downloads/) y luego ejecute los comandos siguientes.

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a>Resumen

En este tutorial, se ha explicado el proceso básico para usar Face API mediante la invocación del SDK de Python. Para más información detallada sobre la API, vea los procedimientos y la [referencia de la API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related-topics"></a>Temas relacionados

- [Introducción a Face API en CSharp](FaceAPIinCSharpTutorial.md)
- [Introducción a Face API en Java para Android](FaceAPIinJavaForAndroidTutorial.md)
