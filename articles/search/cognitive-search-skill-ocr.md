---
title: Aptitud de Cognitive Search de OCR (Azure Search) | Microsoft Docs
description: Extraer el texto de los archivos de imagen en una canalización de enriquecimiento de Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 97d594a232c3576d0a0163b2d6847f06328bcd7b
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167518"
---
# <a name="ocr-cognitive-skill"></a>Aptitud cognitiva de OCR

La aptitud de **OCR** extrae texto de los archivos de imagen. Entre otros, estos son los formatos de archivos admitidos:

+ .JPEG
+ .JPG
+ .PNG
+ .BMP
+ .GIF

> [!NOTE]
> Cognitive Search está disponible en la versión preliminar pública. La ejecución del conjunto de habilidades y la extracción y normalización de imágenes se ofrecen actualmente de forma gratuita. Más adelante, se anunciarán los precios de estas funcionalidades. 

## <a name="skill-parameters"></a>Parámetros de la aptitud

Los parámetros distinguen mayúsculas de minúsculas.

| Nombre de parámetro     | DESCRIPCIÓN |
|--------------------|-------------|
| detectOrientation | Permite la detección automática de la orientación de la imagen. <br/> Valores válidos: true / false.|
|defaultLanguageCode | <p>  Código de idioma del texto de entrada. Los lenguajes compatibles incluyen: <br/> zh-Hans (chino simplificado) <br/> zh-Hant (chino tradicional) <br/>cs (checo) <br/>da (danés) <br/>nl (neerlandés) <br/>en (inglés) <br/>fi (finés)  <br/>fr (francés) <br/>  de (alemán) <br/>el (griego) <br/> hu (húngaro) <br/> it (italiano) <br/>  ja (japonés) <br/> ko (coreano) <br/> nb (noruego) <br/>   pl (polaco) <br/> pt (portugués) <br/>  ru (ruso) <br/>  es (español) <br/>  sv (sueco) <br/>  tr (turco) <br/> ar (árabe) <br/> ro (rumano) <br/> sr-Cyrl (serbio cirílico) <br/> sr-Latn (serbio latino) <br/>  sk (eslovaco) <br/>  unk (desconocido) <br/><br/> Si el código de idioma no está especificado o es nulo, el idioma se detecta de forma automática. </p> |
| textExtractionAlgorithm | "impreso" o "manuscrito". El algoritmo OCR de reconocimiento de texto "manuscrito" se encuentra actualmente en versión preliminar y solo se admite en inglés. |

## <a name="skill-inputs"></a>Entradas de aptitudes

| Nombre de entrada      | DESCRIPCIÓN                                          |
|---------------|------------------------------------------------------|
| imagen         | Tipo complejo. Actualmente, solo funciona con el campo "/document/normalized_images", que crea el indexador de blobs de Azure cuando ```imageAction``` está establecido en ```generateNormalizedImages```. Para obtener más información, consulte este [ejemplo](#sample-output).|


## <a name="skill-outputs"></a>Salidas de aptitudes
| Nombre de salida     | DESCRIPCIÓN                   |
|---------------|-------------------------------|
| text          | Texto sin formato extraído de la imagen.   |
| layoutText    | Tipo complejo que describe el texto extraído, así como la ubicación donde se encontró.|


## <a name="sample-definition"></a>Definición de ejemplo

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Texto y salida de layoutText de ejemplo

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Ejemplo: combinar el texto extraído de imágenes insertadas con el contenido del documento.

Un caso de uso común a la hora de combinar texto, es la capacidad de combinar la representación textual de imágenes (el texto de una aptitud de OCR o la leyenda de una imagen) en el campo de contenido de un documento. 

En el siguiente conjunto de aptitudes de ejemplo se crea un campo *merged_text* para guardar el contenido textual del documento, así como el texto de OCR de cada una de las imágenes insertadas en ese documento. 

#### <a name="request-body-syntax"></a>Sintaxis del cuerpo de la solicitud
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
      "insertPostTag": " ",
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
En el ejemplo del conjunto de aptitudes anterior, se asume que existe un campo de imágenes normalizadas. Para crear este campo, establezca la configuración *imageAction* en la definición del indexador como *generateNormalizedImages*, tal como se muestra a continuación:

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
+ [Aptitud TextMerger](cognitive-search-skill-textmerger.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Create Indexer (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
