---
title: 'Guía de inicio rápido: Extracción de texto impreso (OCR) - REST, cURL - Computer Vision'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, extraerá texto impreso de una imagen mediante Computer Vision API con cURL.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: pafarley
ms.openlocfilehash: 6917e6f4ba0c2f8045d3f7da9a2e23acd949cf48
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341249"
---
# <a name="quickstart-extract-printed-text-ocr-using-the-rest-api-and-curl-in-computer-vision"></a>Guía de inicio rápido: Extracción de texto impreso (OCR) mediante la API de REST y cURL en Computer Vision

En esta guía de inicio rápido, extraerá texto impreso con el reconocimiento óptico de caracteres (OCR) de una imagen con la API de REST de Computer Vision. Con el método [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc), puede detectar texto impreso en cualquier imagen y extraer los caracteres reconocidos en una secuencia de caracteres que pueda usar una máquina.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

- Debe tener [cURL](https://curl.haxx.se/windows).
- Debe tener una clave de suscripción para Computer Vision. Para obtener una clave de suscripción, consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample-command"></a>Creación y ejecución del comando de ejemplo

Para crear y ejecutar el ejemplo, siga estos pasos:

1. Copie el comando siguiente en un editor de texto.
1. Realice los siguientes cambios en el comando donde sea necesario:
    1. Reemplace el valor de `<subscriptionKey>` por la clave de suscripción.
    1. Reemplace la dirección URL de la solicitud (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/ocr`) por la dirección URL del punto de conexión para el método [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) desde la región de Azure donde obtuvo las claves de suscripción, si es necesario.
    1. Si lo desea, cambie la dirección URL de la imagen del cuerpo de la solicitud (`https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png\`) por la dirección URL de una imagen diferente que desee analizar.
1. Abra una ventana de símbolo del sistema.
1. Pegue el comando del editor de texto en la ventana del símbolo del sistema y después ejecute el comando.

```console
curl -H "Ocp-Apim-Subscription-Key: <subscriptionKey>" -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/ocr?language=unk&detectOrientation=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png\"}"
```

## <a name="examine-the-response"></a>Examen de la respuesta

Se devuelve una respuesta correcta en JSON. La aplicación de ejemplo analiza y muestra una respuesta correcta en la ventana del símbolo del sistema, parecida a la del ejemplo siguiente:

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": 0,
  "regions": [
    {
      "boundingBox": "21,16,304,451",
      "lines": [
        {
          "boundingBox": "28,16,288,41",
          "words": [
            {
              "boundingBox": "28,16,288,41",
              "text": "NOTHING"
            }
          ]
        },
        {
          "boundingBox": "27,66,283,52",
          "words": [
            {
              "boundingBox": "27,66,283,52",
              "text": "EXISTS"
            }
          ]
        },
        {
          "boundingBox": "27,128,292,49",
          "words": [
            {
              "boundingBox": "27,128,292,49",
              "text": "EXCEPT"
            }
          ]
        },
        {
          "boundingBox": "24,188,292,54",
          "words": [
            {
              "boundingBox": "24,188,292,54",
              "text": "ATOMS"
            }
          ]
        },
        {
          "boundingBox": "22,253,297,32",
          "words": [
            {
              "boundingBox": "22,253,105,32",
              "text": "AND"
            },
            {
              "boundingBox": "144,253,175,32",
              "text": "EMPTY"
            }
          ]
        },
        {
          "boundingBox": "21,298,304,60",
          "words": [
            {
              "boundingBox": "21,298,304,60",
              "text": "SPACE."
            }
          ]
        },
        {
          "boundingBox": "26,387,294,37",
          "words": [
            {
              "boundingBox": "26,387,210,37",
              "text": "Everything"
            },
            {
              "boundingBox": "249,389,71,27",
              "text": "else"
            }
          ]
        },
        {
          "boundingBox": "127,431,198,36",
          "words": [
            {
              "boundingBox": "127,431,31,29",
              "text": "is"
            },
            {
              "boundingBox": "172,431,153,36",
              "text": "opinion."
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sean necesarios, cierre la ventana del símbolo del sistema y el editor de texto.

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API que se usan para analizar una imagen, detectar celebridades y sitios emblemáticos, crear una miniatura y extraer texto impreso y manuscrito. Para experimentar rápidamente con la versión de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Explore Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
