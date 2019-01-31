---
title: 'Ejemplo: Llamar a la API de imagen Analyze - Computer Vision'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo llamar a Computer Vision API mediante REST en Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 01/20/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 4cca3db778347e9bd8c8b881d85f0882f6b90f72
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216258"
---
# <a name="example-how-to-call-the-computer-vision-api"></a>Ejemplo: Cómo llamar a Computer Vision API

En esta guía se explica cómo llamar a Computer Vision API con REST. Los ejemplos están escritos en C# mediante la biblioteca cliente de Computer Vision API y como llamadas HTTP POST o GET. Nos centraremos en:

-   Cómo obtener "Etiquetas", "Descripciones" y "Categorías".
-   Cómo obtener la información "específica de dominio" (celebridades).

### <a name="Prerequisites">Requisitos previos</a> 
Dirección URL de la imagen o ruta de acceso a la imagen almacenada localmente.
  * Métodos de entrada admitidos: binarios de imagen RAW en forma de application/octet-stream o URL de imagen.
  * Formatos de imagen admitidos: JPEG, PNG, GIF, BMP
  * Tamaño del archivo de imagen: menos de 4 MB
  * Dimensión de la imagen: mayor que 50 x 50 píxeles.
  
En los ejemplos siguientes, se muestran las siguientes características:

1. Análisis de una imagen y obtención de una matriz de etiquetas y una descripción devuelta.
2. Análisis de una imagen con un modelo específico de domino (en concreto, el modelo "celebridades") y obtención del resultado correspondiente en el reajuste de JSON.

Las características se dividen en:

  * **Opción uno:** Análisis con ámbito. Se analiza solo un modelo determinado
  * **Opción dos:** Análisis mejorado. Se analiza para ofrecer detalles adicionales con una [taxonomía de 86 categorías](../Category-Taxonomy.md)
  
### <a name="Step1">Paso 1: Autorización de la llamada a la API</a> 
Cada llamada a Computer Vision API requiere una clave de suscripción. Esta clave se debe pasar a través de un parámetro de cadena de consulta o se debe especificar en el encabezado de la solicitud. 

Para obtener una clave de suscripción, vea [Cómo obtener las claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md
).

**1.** Pasar la clave de suscripción mediante una cadena de consulta; vea a continuación un ejemplo de Computer Vision API:

```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

**2.** El pase de la clave de suscripción también puede especificarse en el encabezado de la solicitud HTTP:

```ocp-apim-subscription-key: <Your subscription key>```

**3.** Al usar una biblioteca cliente, la clave de suscripción se pasa a través del constructor de VisionServiceClient:

```var visionClient = new VisionServiceClient(“Your subscriptionKey”);```

### <a name="Step2">Paso 2: Cargar una imagen en el servicio Computer Vision API y obtener las etiquetas, descripciones y celebridades</a>
La forma básica para realizar la llamada Computer Vision API es cargar directamente una imagen. Para esto, se envía una solicitud "POST" con el tipo de contenido application/octet-stream junto con los datos leídos de la imagen. Para "Etiquetas" y "Descripción", este método de carga será el mismo para todas las llamadas Computer Vision API. La única diferencia radicará en los parámetros de consulta que el usuario especifica. 

Aquí le mostramos cómo obtener "Etiquetas" y "Descripción" para una imagen determinada:

**Opción uno:** Obtener la lista de "Etiquetas" y una "Descripción"
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```
```
using Microsoft.ProjectOxford.Vision;
using Microsoft.ProjectOxford.Vision.Contract;
using System.IO;

AnalysisResult analysisResult;
var features = new VisualFeature[] { VisualFeature.Tags, VisualFeature.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.AnalyzeImageAsync(fs, features);
}
```
**Opción dos:** obtener la lista de "Etiquetas" solamente o la lista de "Descripción" solo:

###### <a name="tags-only"></a>Solo las etiquetas:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/tag&subscription-key=<Your subscription key>
var analysisResult = await visionClient.GetTagsAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>Solo la descripción:
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/describe&subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.DescribeAsync(fs);
}
```
### <a name="here-is-how-to-get-domain-specific-analysis-in-our-case-for-celebrities"></a>Aquí le mostramos cómo obtener un análisis específico de dominio (en nuestro caso, para las celebridades).

**Opción uno:** Análisis con ámbito. Se analiza solo un modelo determinado
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```
Para esta opción, todos los demás parámetros de consulta {visualFeatures, details} no son válidos. Si desea ver todos los modelos compatibles, use: 
```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```
**Opción dos:** Análisis mejorado. Se analiza para ofrecer detalles adicionales con una [taxonomía de 86 categorías](../Category-Taxonomy.md)

Para aplicaciones donde desea obtener un análisis genérico de imágenes adicional a los detalles de uno o más modelos específicos del dominio, ampliamos la API v1 con el parámetro de consulta de modelos.
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```
Cuando se invoca este método, se llamará primero al clasificador de 86 categorías. Si ninguna de las categorías concuerda con los modelos conocidos/coincidentes, se procede a una segunda pasada de invocaciones de clasificador. Por ejemplo, si "detalles=todo" o "detalles" incluye "celebridades", se llamará al modelo de celebridades después de llamar al clasificador de 86 categorías y el resultado incluirá la persona de la categoría. Esto aumentará la latencia para los usuarios interesados en las celebridades, en comparación con la opción uno.

Todos los parámetros de consulta v1 se comportarán igual en este caso.  Si no se especifica visualFeatures=categories, se habilitará de forma implícita.

### <a name="Step3">Paso 3: Recuperar y describir la salida JSON para analyze&visualFeatures=Tags, Description</a>

Este es un ejemplo:
```
  {
    “tags”: [
    {
    "name": "outdoor",
        "score": 0.976
    },
    {
    "name": "bird",
        "score": 0.95
    }
            ],
    “description”: 
    {
    "tags": [
    "outdoor",
    "bird"
            ],
    "captions": [
    {
    "text”: “partridge in a pear tree”,
            “confidence”: 0.96
    }
                ]
    }
  }
```
Campo   | Type  | Contenido
------|------|------|
Etiquetas    | objeto    | Objeto de nivel superior de la matriz de etiquetas
tags[].Name | string    | Palabra clave del clasificador de etiquetas
tags[].Score    | número    | Puntuación de confianza, entre 0 y 1
description  | objeto   | Objeto de nivel superior de la descripción
description.tags[] |    string  | Lista de etiquetas.  Si la confianza es insuficiente para poder producir un título, las etiquetas quizá sean la única información disponible para el llamador.
description.captions[].text | string    | Una frase que describe la imagen
description.captions[].confidence   | número    | Confianza para la frase

### <a name="Step4">Paso 4: Recuperar y describir la salida JSON de modelos específicos del dominio</a>

**Opción uno:** Análisis con ámbito. Se analiza solo un modelo determinado

El resultado será una matriz de etiquetas; un ejemplo será similar a este ejemplo:
```
  { 
    "result": [ 
      { 
    "name": "golden retriever", 
    "score": 0.98
    },
    { 
    "name": "Labrador retriever", 
    "score": 0.78
    }
              ]
  }
```

**Opción dos:** Análisis mejorado. Se analiza para ofrecer detalles adicionales con una taxonomía de 86 categorías

Para modelos específicos del dominio con la opción dos (análisis mejorado), el tipo de devolución de categorías se amplía. A continuación, encontrará un ejemplo:
```
  {
    "requestId": "87e44580-925a-49c8-b661-d1c54d1b83b5",
    "metadata":     {
      "width": 640,
      "height": 430,
      "format": "Jpeg"
                    },
    "result": {
      "celebrities": 
      [
        {
        "name": "Richard Nixon",
        "faceRectangle": {
          "left": 107,
          "top": 98,
          "width": 165,
          "height": 165
                         },
        "confidence": 0.9999827
        }
      ]
  }
```

El campo de categorías es una lista de una o varias de las [86 categorías](../Category-Taxonomy.md) de la taxonomía original. Tenga en cuenta también que las categorías que terminan con un guión bajo coincidirán con esa categoría y sus elementos secundarios (por ejemplo, people_ así como people_group, para el modelo de celebridades).

Campo   | Type  | Contenido
------|------|------|
Categorías | objeto | Objeto de nivel superior
categories[].name    | string   | Nombre de la taxonomía de 86 categorías
categories[].score  | número    | Puntuación de confianza, entre 0 y 1
categories[].detail  | ¿objeto?      | Objeto de detalle opcional

Tenga en cuenta que si varias categorías coinciden (por ejemplo, el clasificador de 86 categorías devuelve una puntuación para people_ and people_young si model=celebrities), los detalles están conectados a la coincidencia de nivel más general (people_ en dicho ejemplo).

### <a name="Errors">Respuestas con errores</a>
Son idénticas a vision.analyze, con el error adicional de NotSupportedModel (HTTP 400), que puede devolverse en escenarios de la opción uno y la opción dos. Para la segunda opción (análisis mejorado), si no se reconoce ninguno de los modelos especificados en los detalles, la API devolverá NotSupportedModel, incluso si uno o varios de ellos son válidos.  Los usuarios pueden llamar a listModels para averiguar qué modelos son compatibles.

### <a name="Summary">Resumen</a>

Estas son las funcionalidades básicas de Computer Vision API: cómo puede cargar imágenes y recuperar metadatos valiosos como valor devuelto.

Para usar la API de REST, vaya a la [referencia de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44).
 
