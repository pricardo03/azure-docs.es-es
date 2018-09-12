---
title: 'Guía de inicio rápido de Computer Vision API para cURL: análisis de imágenes locales | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: En esta guía de inicio rápido, analizará una imagen local mediante Computer Vision con cURL en Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 93ca3ea6eee3743dfd0c25c9514375ae63a531ee
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772410"
---
# <a name="quickstart-analyze-a-local-image---rest-curl"></a>Guía de inicio rápido: Análisis de imágenes locales (REST, cURL)

En esta guía de inicio rápido, analizará una imagen local para extraer características visuales mediante Computer Vision. Para analizar una imagen remota, consulte [Análisis de imágenes remotas con cURL](curl-analyze.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-a-local-image"></a>Analizar una imagen local

Este ejemplo es similar a [Analizar imágenes remotas con cURL](curl-analyze.md), salvo que la imagen para analizar se lee localmente desde el disco. Hay que realizar tres cambios:

- Cambiar el tipo de contenido a `"Content-Type: application/octet-stream"`.
- Cambiar el modificador `-d` a `--data-binary`.
- Especificar la imagen que se va a analizar mediante la siguiente sintaxis: `@C:/Pictures/ImageToAnalyze.jpg`.

Para ejecutar el ejemplo, siga estos pasos:

1. Copie el código siguiente en un editor.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Si es necesario, cambie la dirección URL de solicitud (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) para usar la ubicación donde obtuvo las claves de suscripción.
1. Reemplace `<Image To Analyze>` por la imagen local que desea analizar.
1. También tiene la opción de cambiar el idioma de la respuesta (`language=en`).
1. Abra una ventana de comandos en un equipo que tenga instalado cURL.
1. Pegue el código en la ventana y ejecute el comando.

>[!NOTE]
>Debe utilizar la misma ubicación en la llamada de REST que utilizó para obtener las claves de la suscripción. Por ejemplo, si obtuvo sus claves de suscripción de westus, reemplace "westcentralus" en la siguiente URL por "westus".

```json
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" -H "Content-Type: application/octet-stream" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" --data-binary <Image To Analyze>
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
  "requestId": "b6e33879-abb2-43a0-a96e-02cb5ae0b795",
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
