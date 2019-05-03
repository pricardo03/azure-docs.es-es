---
title: 'Aptitud de Cognitive Search de análisis de imágenes: Azure Search'
description: Extraiga texto semántico a través del análisis de imágenes mediante la aptitud cognitiva ImageAnalysis en una canalización de enriquecimiento de Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: bb18c858a17e290a8ce2cc88dc1e7d88d21afe0f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021909"
---
#   <a name="image-analysis-cognitive-skill"></a>Aptitud cognitiva de análisis de imágenes

La aptitud de **Análisis de imágenes** extrae un amplio conjunto de características visuales en función del contenido de la imagen. Por ejemplo, puede generar una leyenda a partir de una imagen, generar etiquetas o identificar celebridades y lugares de referencia. Esta aptitud utiliza los modelos de aprendizaje automático proporcionados por [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) en Cognitive Services. 

> [!NOTE]
> Expandir ámbito aumentando la frecuencia de procesamiento, agregando más documentos o agregar más algoritmos de inteligencia artificial, tendrá que [adjuntar un recurso de Cognitive Services facturable](cognitive-search-attach-cognitive-services.md). Los cargos se acumulan cuando se llama a las API de Cognitive Services y para la extracción de imágenes como parte de la fase de descifrado de documentos en Azure Search. No hay ningún cargo por la extracción de texto de documentos.
>
> Ejecución de habilidades integrados se cobra a los existentes [Cognitive Services paga a medida que vaya precio](https://azure.microsoft.com/pricing/details/cognitive-services/). Precios de extracción de imagen se describen en el [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Parámetros de las aptitudes

Los parámetros distinguen mayúsculas de minúsculas.

| Nombre de parámetro     | DESCRIPCIÓN |
|--------------------|-------------|
| defaultLanguageCode   |  Cadena que indica el idioma que se devolverá. Este servicio devuelve los resultados de reconocimiento en un idioma concreto. Si esta no se especifica este parámetro, se usa el valor predeterminado "en". <br/><br/>Estos son los idiomas admitidos: <br/>*en*: inglés (predeterminado) <br/> *zh*: chino simplificado|
|visualFeatures |   Matriz de cadenas que indica los tipos de características visual que se devolverán. Los tipos de características visuales válidos incluyen:  <ul><li> *Categorías*: clasifica el contenido de la imagen según una taxonomía definida en la [documentación](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy) de Cognitive Services.</li><li> *Etiquetas*: etiquetas de la imagen con una lista detallada de palabras relacionadas con el contenido de la imagen.</li><li>*Descripción* -describe la imagen de contenido con una frase completa de inglés.</li><li>*Caras* -detecta si están presentes las caras. Si hay caras, indica las coordenadas, el sexo y la edad.</li><li> *ImageType* -detecta si la imagen es imágenes prediseñadas o dibujo lineal.</li><li>  *Color* -determina el color de énfasis, el color dominante, y si una imagen es negra y blanco.</li><li>*Para adultos* -detecta si la imagen pornográfico por naturaleza (muestra desnudos o un acto de sexo). También se detecta contenido provocativo.</li></ul> Los nombres de las características visuales distinguen entre mayúsculas y minúsculas.|
| details   | Matriz de cadenas que indica qué detalles específicos del dominio que se devolverán. Los tipos de características visuales válidos incluyen: <ul><li>*Celebridades* -identifica celebridades si detecta en la imagen.</li><li>*Puntos de referencia* -identifica los puntos de referencia si detecta en la imagen.</li></ul>
 |

## <a name="skill-inputs"></a>Entradas de la aptitud

| Nombre de entrada      | DESCRIPCIÓN                                          |
|---------------|------------------------------------------------------|
| image         | Tipo complejo. Actualmente, solo funciona con el campo "/document/normalized_images", que crea el indexador de Azure Blob cuando ```imageAction``` está establecido en un valor diferente a ```none```. Para obtener más información, consulte este [ejemplo](#sample-output).|



##  <a name="sample-definition"></a>Definición de ejemplo
```json
{
    "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
    "context": "/document/normalized_images/*",
    "visualFeatures": [
        "Tags",
        "Faces",
        "Categories",
        "Adult",
        "Description",
        "ImageType",
        "Color"
    ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "image",
            "source": "/document/normalized_images/*"
        }
    ],
    "outputs": [
        {
            "name": "categories",
            "targetName": "myCategories"
        },
        {
            "name": "tags",
            "targetName": "myTags"
        },
        {
            "name": "description",
            "targetName": "myDescription"
        },
        {
            "name": "faces",
            "targetName": "myFaces"
        },
        {
            "name": "imageType",
            "targetName": "myImageType"
        },
        {
            "name": "color",
            "targetName": "myColor"
        },
        {
            "name": "adult",
            "targetName": "myAdultCategory"
        }
    ]
}
```

##  <a name="sample-input"></a>Entrada de ejemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "image": {
                    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
                    "width": 500,
                    "height": 300,
                    "originalWidth": 5000,
                    "originalHeight": 3000,
                    "rotationFromOriginal": 90,
                    "contentOffset": 500
                }
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Salida de ejemplo

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "categories": [
                    {
                        "name": "abstract_",
                        "score": 0.00390625
                    },
                    {
                        "name": "people_",
                        "score": 0.83984375,
                        "detail": {
                            "celebrities": [
                                {
                                    "name": "Satya Nadella",
                                    "faceBoundingBox": {
                                        "left": 597,
                                        "top": 162,
                                        "width": 248,
                                        "height": 248
                                    },
                                    "confidence": 0.999028444
                                }
                            ],
                            "landmarks": [
                                {
                                    "name": "Forbidden City",
                                    "confidence": 0.9978346
                                }
                            ]
                        }
                    }
                ],
                "adult": {
                    "isAdultContent": false,
                    "isRacyContent": false,
                    "adultScore": 0.0934349000453949,
                    "racyScore": 0.068613491952419281
                },
                "tags": [
                    {
                        "name": "person",
                        "confidence": 0.98979085683822632
                    },
                    {
                        "name": "man",
                        "confidence": 0.94493889808654785
                    },
                    {
                        "name": "outdoor",
                        "confidence": 0.938492476940155
                    },
                    {
                        "name": "window",
                        "confidence": 0.89513939619064331
                    }
                ],
                "description": {
                    "tags": [
                        "person",
                        "man",
                        "outdoor",
                        "window",
                        "glasses"
                    ],
                    "captions": [
                        {
                            "text": "Satya Nadella sitting on a bench",
                            "confidence": 0.48293603002174407
                        }
                    ]
                },
                "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
                "metadata": {
                    "width": 1500,
                    "height": 1000,
                    "format": "Jpeg"
                },
                "faces": [
                    {
                        "age": 44,
                        "gender": "Male",
                        "faceBoundingBox": {
                            "left": 593,
                            "top": 160,
                            "width": 250,
                            "height": 250
                        }
                    }
                ],
                "color": {
                    "dominantColorForeground": "Brown",
                    "dominantColorBackground": "Brown",
                    "dominantColors": [
                        "Brown",
                        "Black"
                    ],
                    "accentColor": "873B59",
                    "isBwImg": false
                    },
                "imageType": {
                    "clipArtType": 0,
                    "lineDrawingType": 0
                }
            }
        }
    ]
}
```


## <a name="error-cases"></a>Casos de error
En los siguientes casos de error, no se extrae ningún elemento.

| Código de error | DESCRIPCIÓN |
|------------|-------------|
| NotSupportedLanguage | El idioma proporcionado no se admite. |
| InvalidImageUrl | La dirección URL de la imagen tiene un formato que no se admite o no es accesible.|
| InvalidImageFormat | Los datos de entrada no son una imagen válida. |
| InvalidImageSize | La imagen de entrada es demasiado grande. |
| NotSupportedVisualFeature  | El tipo de característica especificado no es válido. |
| NotSupportedImage | La imagen no se admite como, por ejemplo, pornografía infantil. |
| InvalidDetails | El modelo específico del dominio no se admite. |

## <a name="see-also"></a>Vea también

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Create Indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
