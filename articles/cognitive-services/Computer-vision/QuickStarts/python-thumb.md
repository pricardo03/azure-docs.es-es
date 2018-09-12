---
title: 'Guía de inicio rápido de Computer Vision para Python: Creación de miniaturas | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: En esta guía de inicio rápido, generará una miniatura de una imagen mediante Computer Vision con Python en Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: bc1d01cd4b8f15ba627d917825ee5205c34f5cdf
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772116"
---
# <a name="quickstart-generate-a-thumbnail---rest-python"></a>Guía de inicio rápido: Generación de una miniatura (REST, Python)

En esta guía de inicio rápido, generará una miniatura de una imagen con Computer Vision.

Puede ejecutar esta guía de inicio rápido paso a paso mediante un cuaderno de Jupyter en [MyBinder](https://mybinder.org). Para iniciar Binder, seleccione el botón siguiente:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Requisitos previos

Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="intelligently-generate-a-thumbnail"></a>Generación inteligente de una miniatura

Con el método [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), puede generar una miniatura de una imagen. Debe especificar el alto y el ancho, que pueden ser diferentes a la relación de aspecto de la imagen de entrada. Computer Vision usa el recorte inteligente para identificar la región de interés de forma inteligente y generar coordenadas de recorte en función de esa región.

Para ejecutar el ejemplo, siga estos pasos:

1. Copie el siguiente ejemplo de código en un nuevo archivo de script de Python.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Si es necesario, cambie el valor de `vision_base_url` por la ubicación donde obtuvo las claves de suscripción.
1. Tiene la opción de cambiar el valor `image_url` por otra imagen.
1. Ejecute el script.

El código siguiente usa la biblioteca `requests` de Python para llamar a Analyze Image API de Computer Vision. La clave de API se pasa a través del diccionario `headers`. El tamaño de la miniatura se pasa mediante el diccionario `params`. La miniatura se devuelve como una matriz de bytes en la respuesta.

## <a name="get-thumbnail-request"></a>Solicitud Get Thumbnail

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
# Free trial subscription keys are generated in the westcentralus region.
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

## <a name="next-steps"></a>Pasos siguientes

Explore una aplicación de Python que usa Computer Vision para realizar el reconocimiento óptico de caracteres (OCR), crear miniaturas con recorte inteligente, y detectar, clasificar, etiquetar y describir características visuales, como caras, en una imagen. Para experimentar rápidamente con las versiones de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Tutorial de Computer Vision API para Python](../Tutorials/PythonTutorial.md)
