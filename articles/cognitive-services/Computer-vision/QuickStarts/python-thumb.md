---
title: 'Inicio rápido: Generación de una miniatura (REST, Python)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, generará una miniatura de una imagen mediante Computer Vision API con Python.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 23ef0e83656e7f6d73bf89a787ab8864770a0f4b
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53581120"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-python-in-computer-vision"></a>Inicio rápido: Generación de una miniatura mediante la API de REST y Python en Computer Vision

En esta guía de inicio rápido, generará una miniatura de una imagen mediante la API de REST de Computer Vision. Con el método [Obtener miniatura](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), puede generar una miniatura de una imagen. Debe especificar el alto y el ancho, que pueden ser diferentes a la relación de aspecto de la imagen de entrada. Computer Vision usa el recorte inteligente para identificar el área de interés de forma inteligente y generar coordenadas de recorte en función de esa región.

Puede ejecutar esta guía de inicio rápido paso a paso mediante un cuaderno de Jupyter en [MyBinder](https://mybinder.org). Para iniciar Binder, seleccione el botón siguiente:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="prerequisites"></a>Requisitos previos

- Debe tener [Python](https://www.python.org/downloads/) instalado si desea ejecutar el ejemplo localmente.
- Debe tener una clave de suscripción para Computer Vision. Para obtener una clave de suscripción, consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample"></a>Creación y ejecución del ejemplo

Para crear y ejecutar el ejemplo, siga estos pasos:

1. Copie el código siguiente en un editor de texto.
1. Realice los siguientes cambios en el código donde sea necesario:
    1. Reemplace el valor de `subscription_key` por la clave de suscripción.
    1. Reemplace el valor de `vision_base_url` por la dirección URL del punto de conexión del recurso de Computer Vision en la región de Azure donde obtuvo las claves de suscripción, si es necesario.
    1. También puede reemplazar el valor de `image_url` por la dirección URL de una imagen diferente para la que desea generar una miniatura.
1. Guarde el código como un archivo con la extensión `.py`. Por ejemplo, `get-thumbnail.py`.
1. Abra una ventana de símbolo del sistema.
1. En el símbolo del sistema, utilice el comando `python` para ejecutar el ejemplo. Por ejemplo, `python get-thumbnail.py`.

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the "westus" region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

thumbnail_url = vision_base_url + "generateThumbnail"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'width': '50', 'height': '50', 'smartCropping': 'true'}
data    = {'url': image_url}
response = requests.post(thumbnail_url, headers=headers, params=params, json=data)
response.raise_for_status()

thumbnail = Image.open(BytesIO(response.content))

# Display the thumbnail.
plt.imshow(thumbnail)
plt.axis("off")

# Verify the thumbnail size.
print("Thumbnail is {0}-by-{1}".format(*thumbnail.size))
```

## <a name="examine-the-response"></a>Examen de la respuesta

Se devuelve una respuesta correcta como datos binarios, que representan los datos de imagen para la miniatura. Si la solicitud se realiza correctamente, la miniatura se genera a partir de los datos binarios de la respuesta y se muestra en el ejemplo. Si se produce un error en la solicitud, la respuesta se muestra en la ventana del símbolo del sistema. La respuesta de la solicitud con error contiene un código de error y un mensaje para ayudar a determinar qué salió mal.

## <a name="next-steps"></a>Pasos siguientes

Explore una aplicación de Python que usa Computer Vision para realizar el reconocimiento óptico de caracteres (OCR), crear miniaturas con recorte inteligente, y detectar, clasificar, etiquetar y describir características visuales, como caras, en una imagen. Para experimentar rápidamente con la versión de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Tutorial de Computer Vision API para Python](../Tutorials/PythonTutorial.md)
