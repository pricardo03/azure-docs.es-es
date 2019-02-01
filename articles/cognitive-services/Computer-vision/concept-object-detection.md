---
title: 'Detección de objetos: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la detección de objetos en Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 3468f9341e0a8406733877a05798e427dd454fff
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167570"
---
# <a name="object-detection"></a>Detección de objetos

La detección de objetos es similar al [etiquetado](concept-tagging-images.md), pero la API devuelve las coordenadas del rectángulo (en píxeles) que delimita cada objeto encontrado. Por ejemplo, si una imagen contiene un perro, un gato y una persona, la operación de detección mostrará estos objetos junto con sus coordenadas en la imagen. Puede usar esta funcionalidad para procesar las relaciones entre los objetos de una imagen. También le permite saber cuándo hay varias instancias de la misma etiqueta en una imagen.

La API de detección aplica etiquetas en función de los objetos o seres vivos que se identifiquen en la imagen. Tenga en cuenta que, en este momento, no existe ninguna relación formal entre la taxonomía utilizada para el etiquetado y la taxonomía utilizada para la detección de objetos. Desde un punto de vista conceptual, la API de detección solo busca objetos y seres vivos, mientras que la API de etiquetado también puede incluir términos contextuales, como "interior", que no pueden localizarse con cuadros delimitadores.

## <a name="object-detection-example"></a>Ejemplo de detección de objetos

En la siguiente respuesta JSON, se ilustra qué devuelve Computer Vision cuando se detectan los objetos de la imagen de ejemplo.

![Una mujer con un dispositivo Microsoft Surface en una cocina](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos de [categorización de imágenes](concept-categorizing-images.md) y de [descripción de imágenes](concept-describing-images.md).
