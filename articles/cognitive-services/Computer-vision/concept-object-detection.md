---
title: 'Detección de objetos: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los conceptos relacionados con la característica de detección de objeto de Computer Vision API - uso y los límites.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 012ab849c926de332da55361c79c76c5a1311169
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60001854"
---
# <a name="detect-common-objects-in-images"></a>Detectar objetos comunes de imágenes

La detección de objetos es similar al [etiquetado](concept-tagging-images.md), pero la API devuelve las coordenadas del rectángulo (en píxeles) que delimita cada objeto encontrado. Por ejemplo, si una imagen contiene un perro, un gato y una persona, la operación de detección mostrará estos objetos junto con sus coordenadas en la imagen. Puede usar esta funcionalidad para procesar las relaciones entre los objetos de una imagen. También le permite determinar si hay varias instancias de la misma etiqueta en una imagen.

La API de detección aplica etiquetas en función de los objetos o seres vivos que se identifiquen en la imagen. Actualmente no hay ninguna relación formal entre la taxonomía de etiquetado y la taxonomía de detección de objeto. En un nivel conceptual, la API de detectar solo busca objetos y cosas de la vida, mientras que la API de la etiqueta a la que también puede incluir contextuales términos como "interiores", ya que no se pueden localizar con cuadros de límite.

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

## <a name="limitations"></a>Limitaciones

Es importante tener en cuenta las limitaciones de detección de objetos para que pueda evitar o mitigar los efectos de falsos negativos (objetos que faltan) y los detalles limitado.

* Por lo general no objetos no se detectan si son pequeños (menos del 5% de la imagen).
* Objetos generalmente no detecta si están ordenados en estrecha colaboración (una pila de platos, por ejemplo).
* Los objetos no se diferencian por la marca ni los nombres de productos (tipos diferentes de los refrescos en una estantería de un almacén, por ejemplo). Sin embargo, puede obtener información de la marca de una imagen mediante la característica [de detección de la marca](concept-brand-detection.md).

## <a name="use-the-api"></a>Uso de la API

La característica de detección de objetos forma parte de la API [de análisis de imágenes](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa). Puede llamar a esta API mediante una SDK nativa o con llamadas a REST. Incluir `Objects` en el **visualFeatures** parámetro de consulta. A continuación, cuando llegue la respuesta JSON completa, simplemente analizar la cadena para el contenido de la `"objects"` sección.

* [Inicio rápido: Análisis de imágenes (SDK .NET)](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Inicio rápido: Análisis de imágenes (API REST)](./quickstarts/csharp-analyze.md)