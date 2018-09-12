---
title: 'Guía de inicio rápido de Computer Vision API para Node.js: Análisis de imágenes | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: En esta guía de inicio rápido, analizará una imagen mediante Computer Vision con Node.js en Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: dab6547e08b1b01a9090a817d728c86359c680f2
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772167"
---
# <a name="quickstart-analyze-a-remote-image---rest-nodejs"></a>Guía de inicio rápido: Análisis de imágenes remotas (REST, Node.js)

En esta guía de inicio rápido, analizará una imagen para extraer características visuales con Computer Vision.

## <a name="prerequisites"></a>Requisitos previos

Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-image-request"></a>Solicitud Analyze Image

Con el [método de análisis de imagen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), puede extraer características visuales basadas en el contenido de una imagen. Puede cargar una imagen o especificar una URL de imagen y elegir qué características se van a devolver, como:

* Una lista detallada de las etiquetas relacionadas con el contenido de la imagen.
* Una descripción del contenido de la imagen en una oración completa.
* Las coordenadas, el sexo y la edad de las caras de la imagen.
* El tipo de imagen (imágenes prediseñadas o dibujo lineal).
* El color dominante, el color de énfasis o si una imagen está en blanco y negro.
* La categoría definida en esta [taxonomía](../Category-Taxonomy.md).
* ¿La imagen incluye contenido para adultos o insinuaciones de tipo sexual?

Para ejecutar el ejemplo, siga estos pasos:

1. Copie el código siguiente en un editor.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Si es necesario, cambie el valor de `uriBase` por la ubicación donde obtuvo las claves de suscripción.
1. También puede cambiar el valor `imageUrl` por la imagen que quiera analizar.
1. También tiene la opción de cambiar el idioma de la respuesta (`'language': 'en'`).
1. Guarde el archivo con la extensión `.js`.
1. Abra el símbolo del sistema de Node.js y ejecute el archivo, por ejemplo: `node myfile.js`.

En este ejemplo se usa el paquete npm [request](https://www.npmjs.com/package/request).

```nodejs
'use strict';

const request = require('request');

// Replace <Subscription Key> with your valid subscription key.
const subscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to get your
// subscription keys. For example, if you got your subscription keys from
// westus, replace "westcentralus" in the URL below with "westus".
const uriBase =
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze';

const imageUrl =
    'http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

// Request parameters.
const params = {
    'visualFeatures': 'Categories,Description,Color',
    'details': '',
    'language': 'en'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
  let jsonResponse = JSON.stringify(JSON.parse(body), null, '  ');
  console.log('JSON Response\n');
  console.log(jsonResponse);
});
```

## <a name="analyze-image-response"></a>Respuesta de Analyze Image

Se devuelve una respuesta correcta en código JSON, por ejemplo:

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Grey",
    "dominantColorBackground": "Green",
    "dominantColors": [
      "Grey",
      "Green"
    ],
    "accentColor": "4D5E2F",
    "isBwImg": false
  },
  "requestId": "81b4e400-e3c1-41f1-9020-e6871ad9f0ed",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API que se usan para analizar una imagen, detectar personajes y sitios emblemáticos, crear una miniatura y extraer texto impreso y escrito a mano. Para experimentar rápidamente con las versiones de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Exploración de las versiones de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
