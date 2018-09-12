---
title: 'Guía de inicio rápido: Generación de una miniatura - REST, Node.js - Computer Vision'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, generará una miniatura de una imagen mediante Computer Vision con Node.js en Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: ebc9c89969641ace679b3d6677efd67138424712
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842615"
---
# <a name="quickstart-generate-a-thumbnail---rest-nodejs---computer-vision"></a>Guía de inicio rápido: Generación de una miniatura - REST, Node.js - Computer Vision

En esta guía de inicio rápido, generará una miniatura de una imagen con Computer Vision.

## <a name="prerequisites"></a>Requisitos previos

Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="get-thumbnail-request"></a>Solicitud Get Thumbnail

Con el método [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), puede generar una miniatura de una imagen. Debe especificar el alto y el ancho, que pueden ser diferentes a la relación de aspecto de la imagen de entrada. Computer Vision usa el recorte inteligente para identificar la región de interés de forma inteligente y generar coordenadas de recorte en función de esa región.

Para ejecutar el ejemplo, siga estos pasos:

1. Copie el código siguiente en un editor.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Si es necesario, cambie el valor de `uriBase` por la ubicación donde obtuvo las claves de suscripción.
1. También puede cambiar el valor `imageUrl` por la imagen que quiera analizar.
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
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail';

const imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

// Request parameters.
const params = {
    'width': '100',
    'height': '100',
    'smartCropping': 'true'
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
});
```

## <a name="get-thumbnail-response"></a>Respuesta de Get Thumbnail

Una respuesta correcta contiene el binario de la imagen en miniatura. Si se produce un error en la solicitud, la respuesta contiene un código de error y un mensaje para ayudar a determinar qué salió mal.

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API que se usan para analizar una imagen, detectar personajes y sitios emblemáticos, crear una miniatura y extraer texto impreso y escrito a mano. Para experimentar rápidamente con las versiones de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Exploración de las versiones de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
