---
title: 'Inicio rápido: Reconocimiento de emociones en las caras de una imagen (Emotion API, Python)'
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Emotion API con Python.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 02/05/2018
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: bdc42233d027134ca42e52398eeefd83ae45669f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211260"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Inicio rápido: Creación de una aplicación para reconocer emociones en las caras de una imagen.

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/). 

En este artículo se proporciona información y ejemplos de código para ayudarle a empezar a usar rápidamente el [método de reconocimiento de Emotion API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) con Python para reconocer las emociones expresadas por una o varias personas en una imagen.

Para ejecutar este ejemplo como Jupyter Notebook en [MyBinder](https://mybinder.org), puede hacer clic en el distintivo launch Binder: [![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Requisito previo
Obtenga su clave de suscripción gratuita [aquí](https://azure.microsoft.com/try/cognitive-services/).

## <a name="running-the-walkthrough"></a>Ejecución del tutorial
Para continuar con este tutorial, reemplace `subscription_key` por la clave de API que obtuvo anteriormente.


```python
subscription_key = None
assert subscription_key
```

A continuación, compruebe que la dirección URL del servicio corresponde a la región que usó al configurar la clave de API. Si está usando una clave de prueba, no es necesario realizar ningún cambio.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

En este tutorial se usan las imágenes que están almacenadas en el disco. También puede usar imágenes que están disponibles mediante una dirección URL de acceso público. Para más información, consulte la [documentación de la API de REST](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

Puesto que los datos de imagen se pasan como parte del cuerpo de la solicitud, tenga en cuenta que debe establecer el encabezado `Content-Type` en `application/octet-stream`. Si va a pasar una imagen mediante una dirección URL, recuerde establecer el encabezado en:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
cree un diccionario que contenga la dirección URL:
```python
data = {'url': image_url}
```
y páselo a la biblioteca `requests` mediante:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

En primer lugar, descargue algunas imágenes de ejemplo del sitio de [Emotion API](https://azure.microsoft.com/services/cognitive-services/emotion/).


```bash
%%bash
mkdir -p images
curl -Ls https://aka.ms/csnb-emotion-1 -o images/emotion_1.jpg
curl -Ls https://aka.ms/csnb-emotion-2 -o images/emotion_2.jpg
```


```python
image_path = "images/emotion_1.jpg"
image_data = open(image_path, "rb").read()
```


```python
import requests
headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
response.raise_for_status()
analysis = response.json()
analysis
```




    [{'faceRectangle': {'height': 162, 'left': 130, 'top': 141, 'width': 162},
      'scores': {'anger': 9.29041e-06,
       'contempt': 0.000118981574,
       'disgust': 3.15619363e-05,
       'fear': 0.000589638,
       'happiness': 0.06630674,
       'neutral': 0.00555004273,
       'sadness': 7.44669524e-06,
       'surprise': 0.9273863}}]



El objeto JSON devuelto contiene los rectángulos de selección de las caras que se reconocieron junto con las emociones detectadas. Cada emoción se asocia con una puntuación de entre 0 y 1, donde una puntuación más alta es más indicativa de una emoción que una más baja.

Las siguientes líneas de código detectan las emociones en las caras de la imagen con la biblioteca `matplotlib`. Para reducir la acumulación de elementos, solo se muestran las tres emociones principales.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from io import BytesIO
from PIL import Image

plt.figure(figsize=(5,5))

image  = Image.open(BytesIO(image_data))
ax     = plt.imshow(image, alpha=0.6)

for face in analysis:
    fr = face["faceRectangle"]
    em = face["scores"]
    origin = (fr["left"], fr["top"])
    p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
    ax.axes.add_patch(p)
    ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
    plt.text(origin[0], origin[1], ct, fontsize=20)    
_ = plt.axis("off")
```

La función `annotate_image` que se muestra a continuación se puede usar para superponer las emociones encima de un archivo de imagen dada su ruta de acceso en el sistema de archivos. Se basa en el código para llamar a Emotion API mostrado anteriormente.


```python
def annotate_image(image_path):    
    image_data = open(image_path, "rb").read()
    headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
    response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
    response.raise_for_status()
    analysis = response.json()

    plt.figure()

    image  = Image.open(image_path)
    ax     = plt.imshow(image, alpha=0.6)

    for face in analysis:
        fr = face["faceRectangle"]
        em = face["scores"]
        origin = (fr["left"], fr["top"])
        p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
        ax.axes.add_patch(p)
        ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
        plt.text(origin[0], origin[1], ct, fontsize=20, va="bottom")    
    _ = plt.axis("off")
```

Por último, se puede llamar a la función `annotate_image` en un archivo de imagen, como se muestra en la siguiente línea:


```python
annotate_image("images/emotion_2.jpg")
```
