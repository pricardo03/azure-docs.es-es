---
title: 'Detección de la marca: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la detección de marca/logotipo en Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 01/26/2019
ms.author: pafarley
ms.openlocfilehash: abeca204296bcb3933013f2b7434b8c558f62e50
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58496594"
---
# <a name="brand-detection"></a>Detección de la marca

La detección de la marca es un modo especializado de [detección de objetos](concept-object-detection.md) que usa una base de datos de miles de logotipos mundiales para identificar las marcas comerciales en imágenes o vídeos. Puede usar esta característica, por ejemplo, para detectar qué marcas son más populares en medios sociales o más frecuentes en la ubicación de los productos multimedia.

El servicio Computer Vision detecta si existen logotipos de marca en una imagen determinada; si es así, devuelve el nombre de la marca, una puntuación de confianza y las coordenadas del rectángulo delimitador del logotipo.

La base de datos de logotipos integrada cubre marcas populares de electrónica de consumo, vestimenta y mucho más. Si encuentra que el servicio Computer Vision no detecta la marca que está buscando, puede ser conveniente crear y entrenar su propio detector de logotipos con el servicio [Custom Vision](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/).

## <a name="brand-detection-example"></a>Ejemplo de detección de marcas

En la siguiente respuesta JSON, se ilustra qué devuelve Computer Vision cuando se detectan marcas de las imágenes de ejemplo.

![Un sudadera gris con una etiqueta y el logotipo de Microsoft](./Images/gray-shirt-logo.jpg)

```json
{
   "brands":[
      {
         "name":"Microsoft",
         "confidence":0.706,
         "rectangle":{
            "x":470,
            "y":862,
            "w":338,
            "h":327
         }
      }
   ],
   "requestId":"5fda6b40-3f60-4584-bf23-911a0042aa13",
   "metadata":{
      "width":2286,
      "height":1715,
      "format":"Jpeg"
   }
}
```
En algunos casos, el detector de marcas captará la imagen del logotipo y el nombre estilizado de la marca como dos logotipos independientes.

![Un camiseta roja con una etiqueta y el logotipo de Microsoft](./Images/red-shirt-logo.jpg)

```json
{
   "brands":[
      {
         "name":"Microsoft",
         "confidence":0.657,
         "rectangle":{
            "x":436,
            "y":473,
            "w":568,
            "h":267
         }
      },
      {
         "name":"Microsoft",
         "confidence":0.85,
         "rectangle":{
            "x":101,
            "y":561,
            "w":273,
            "h":263
         }
      }
   ],
   "requestId":"10dcd2d6-0cf6-4a5e-9733-dc2e4b08ac8d",
   "metadata":{
      "width":1286,
      "height":1715,
      "format":"Jpeg"
   }
}
```

## <a name="use-the-api"></a>Uso de la API

La característica de detección de marcas forma parte de la API [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa). Puede llamar a esta API mediante una SDK nativa o con llamadas a REST. Incluir `Brands` en el **visualFeatures** parámetro de consulta. A continuación, cuando llegue la respuesta JSON completa, simplemente analizar la cadena para el contenido de la `"brands"` sección.

* [Inicio rápido: Análisis de imágenes (SDK .NET)](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Inicio rápido: Análisis de imágenes (API REST)](./quickstarts/csharp-analyze.md)
