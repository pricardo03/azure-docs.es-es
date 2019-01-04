---
title: 'Aptitud de búsqueda cognitiva Combinación de texto: Azure Search'
description: Combine el texto de una colección de campos en un solo campo consolidado. Use esta aptitud cognitiva en una canalización de enriquecimiento de Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: b29d32d39b4efb7e242a3ae3213512798622d1e9
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314523"
---
#    <a name="text-merge-cognitive-skill"></a>Aptitud cognitiva Combinación de texto

La aptitud **Combinación de texto** consolida el texto de una colección de campos en un solo campo. 

> [!NOTE]
> A partir del 21 de diciembre de 2018, podrá asociar un recurso de Cognitive Services con un conjunto de aptitudes de Azure Search. Esto nos permitirá empezar a cobrar por la ejecución del conjunto de aptitudes. En esta fecha, también empezaremos a cobrar por la extracción de imágenes como parte de la fase de descifrado de documentos. La extracción de texto de documentos continuará ofreciéndose sin costo adicional.
>
> La ejecución de aptitudes integradas se cobrará a los [precios de pago por uso existentes de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). La extracción de imágenes se cobrará al precio de la versión preliminar, tal y como se describe en la [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400). [Más información](cognitive-search-attach-cognitive-services.md).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.MergeSkill

## <a name="skill-parameters"></a>Parámetros de la aptitud

Los parámetros distinguen mayúsculas de minúsculas.

| Nombre de parámetro     | DESCRIPCIÓN |
|--------------------|-------------|
| insertPreTag  | Cadena que se incluirá antes de cada inserción. El valor predeterminado es `" "`. Para omitir el espacio, establezca el valor en `""`.  |
| insertPostTag | Cadena que se incluirá después de cada inserción. El valor predeterminado es `" "`. Para omitir el espacio, establezca el valor en `""`.  |


##  <a name="sample-input"></a>Entrada de ejemplo
Un documento JSON con una entrada útil para esta aptitud podría ser:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The brown fox jumps over the dog" ,
             "itemsToInsert": ["quick", "lazy"],
             "offsets": [3, 28],
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Salida de ejemplo
Este ejemplo muestra la salida de la entrada anterior, suponiendo que *insertPreTag* esté establecido en `" "` y *insertPostTag* esté establecido en `""`. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="extended-sample-skillset-definition"></a>Definición del conjunto de aptitudes de ejemplo extendido

Un escenario común a la hora de utilizar Combinación de texto es combinar la representación textual de imágenes (el texto de una aptitud de OCR o la leyenda de una imagen) en el campo de contenido de un documento. 

El siguiente conjunto de aptitudes de ejemplo utiliza la aptitud OCR para extraer el texto de las imágenes insertadas en el documento. A continuación, crea un campo *merged_text* para que contenga el texto original y el texto de OCR de cada imagen. 

```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " "
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```
En el ejemplo anterior se asume que existe un campo de imágenes normalizadas. Para obtener este campo de imágenes normalizadas, establezca la configuración *imageAction* en la definición del indexador en *generateNormalizedImages*, tal como se muestra a continuación:

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>Otras referencias

+ [Aptitudes predefinidas](cognitive-search-predefined-skills.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Create Indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
