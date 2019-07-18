---
title: 'Inicio rápido: Detección y enmarcado de las caras de una imagen con el SDK de Python'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, va a crear un script de Python que usa la API Face para detectar caras en una imagen remota y enmarcarlas.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 07/15/2019
ms.author: sbowles
ms.openlocfilehash: 2f2245b4f6e4b38e0b071678ac0f3bddeb72f7ec
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68277520"
---
# <a name="quickstart-create-a-python-script-to-detect-and-frame-faces-in-an-image"></a>Inicio rápido: Creación de un script de Python para detectar y enmarcar caras en una imagen

En esta guía de inicio rápido, va a crear un script de Python que usa la API Face de Azure, mediante el SDK de Python, para detectar caras humanas en una imagen remota. La aplicación muestra una imagen seleccionada y dibuja un marco alrededor de cada cara detectada.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="prerequisites"></a>Requisitos previos

- Una clave de suscripción de Face API. Puede obtener una clave de la suscripción de evaluación gratuita en la página [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). O bien, siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse al servicio Face API y obtener la clave.
- [Python 2.7+ o 3.5+](https://www.python.org/downloads/)
- Herramienta [pip](https://pip.pypa.io/en/stable/installing/)

## <a name="get-the-face-sdk"></a>Obtención del SDK de Face

Para instalar el SDK de Python de Face, abra un símbolo del sistema y ejecute el comando siguiente:

```shell
pip install cognitive_face
```

## <a name="detect-faces-in-an-image"></a>Detección de caras en una imagen

Cree un nuevo script de Python denominado _FaceQuickstart.py_ y agregue el código siguiente. Este código administra la funcionalidad básica de detección de caras. Tendrá que reemplazar `<Subscription Key>` por el valor de la clave. Puede que también necesite cambiar el valor de `BASE_URL` para usar el identificador de región correcto para la clave (consulte la [documentación de Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para obtener una lista de los puntos de conexión de todas las regiones). Las claves de suscripción de la evaluación gratuita se generan en la región **westus**. Opcionalmente, puede establecer `img_url` en la dirección URL de cualquier imagen que desee usar.

El script detectará las caras mediante una llamada al método **cognitive_face.face.detect**, que incluye la API REST de [detección](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) y devuelve una lista de caras.

```python
import cognitive_face as CF

# Replace with a valid subscription key (keeping the quotes in place).
KEY = '<Subscription Key>'
CF.Key.set(KEY)

# Replace with your regional Base URL
BASE_URL = 'https://westus.api.cognitive.microsoft.com/face/v1.0/'
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)
```

### <a name="try-the-app"></a>Pruebe la aplicación

Ejecute la aplicación con el comando `python FaceQuickstart.py`. Debería obtener una respuesta de texto en la ventana de la consola, parecida a la siguiente:

```console
[{'faceId': '26d8face-9714-4f3e-bfa1-f19a7a7aa240', 'faceRectangle': {'top': 124, 'left': 459, 'width': 227, 'height': 227}}]
```

El resultado representa una lista de caras detectadas. Cada elemento de la lista es una instancia de **dict**, donde `faceId` es un identificador único de la cara detectada y `faceRectangle` describe la posición de la cara detectada. 

> [!NOTE]
> Los identificadores de caras expiran después de 24 horas. Tendrá que almacenar los datos de la cara si desea conservarla a largo plazo.

## <a name="draw-face-rectangles"></a>Dibujar rectángulos faciales

Con las coordenadas recibidas del comando anterior, puede dibujar rectángulos en las imágenes para representar visualmente cada cara. Tendrá que instalar Pillow (`pip install pillow`) para usar el módulo de imágenes de este. En la parte superior de *FaceQuickstart.py*, agregue el código siguiente:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

A continuación, en la parte inferior del script, agregue el código siguiente. Este código permite crear una función sencilla para analizar las coordenadas del rectángulo y usa Pillow para dibujar rectángulos en la imagen original. Después, muestra la imagen en el visor de imágenes predeterminado.

```python
# Convert width height to a point in a rectangle


def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))


# Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

# For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

# Display the image in the users default image browser.
img.show()
```

## <a name="run-the-app"></a>Ejecución de la aplicación

Se le pedirá que seleccione un visor de imágenes predeterminado primero. Después, debería ver una imagen parecida a la siguiente. También debería ver los datos del rectángulo impresos en la ventana de la consola.

![Mujer joven con un rectángulo rojo alrededor de la cara](../images/face-rectangle-result.png)

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido ha aprendido el proceso básico para usar el SDK de Python para Face API y ha creado un script para detectar y enmarcar caras en una imagen. A continuación, explore el uso del SDK de Python en un ejemplo más complejo. Vaya al ejemplo de Python para Face API de Cognitive en GitHub, clónelo en la carpeta del proyecto y siga las instrucciones del archivo _README.md_.

> [!div class="nextstepaction"]
> [Ejemplo de Python para Face API de Cognitive](https://github.com/Microsoft/Cognitive-Face-Python)
